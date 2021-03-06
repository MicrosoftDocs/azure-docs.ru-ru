---
title: Частичные термины, шаблоны и специальные символы
titleSuffix: Azure Cognitive Search
description: Используйте подстановочные знаки, регулярные выражения и префиксные запросы для сопоставления целых или частичных терминов в запросе Azure Когнитивный поиск запроса. Шаблоны с фиксированным соответствием, включающие специальные символы, можно разрешить с помощью полного синтаксиса запросов и пользовательских анализаторов.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/03/2020
ms.openlocfilehash: 2e2625fff802e71f797bf6970e763f2bf11c393e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104584182"
---
# <a name="partial-term-search-and-patterns-with-special-characters-hyphens-wildcard-regex-patterns"></a>Частичный Поиск терминов и шаблоны со специальными символами (дефисы, подстановочный знак, регулярное выражение, шаблоны)

*Частичный Поиск терминов* относится к запросам, состоящим из фрагментов терминов, где вместо целого термина может быть только начало, середина или окончание срока (иногда они называются запросами префикса, инфиксные или суффикса). Частичный Поиск терминов может включать комбинацию фрагментов, часто таких как дефисы, тире или косые черты, которые являются частью строки запроса. Типичные варианты использования включают части номера телефона, URL-адреса, кодов или составных слов с дефисами.

Частичный Поиск терминов и строки запросов, содержащие специальные символы, могут быть проблематичными, если у индекса нет маркеров в ожидаемом формате. На [этапе лексического анализа](search-lucene-query-architecture.md#stage-2-lexical-analysis) индексирования (при условии использования стандартного анализатора по умолчанию) специальные символы отбрасываются, составные слова разбиваются и удаляются пробелы. Все это может привести к сбою запросов, если совпадений не найдено. Например, номер телефона, например `+1 (425) 703-6214` (с маркером `"1"` , `"425"` , `"703"` , `"6214"` ), не будет отображаться в запросе, `"3-62"` поскольку это содержимое фактически не существует в индексе. 

Решение заключается в вызове анализатора во время индексирования, сохраняющего полную строку, включая пробелы и специальные символы (при необходимости), чтобы можно было включать пробелы и символы в строку запроса. Аналогично, наличие полной строки, которая не разбита на части более мелких частей, позволяет сопоставлять шаблоны для запросов «начинается с» или «заканчивается на», где предоставленный шаблон можно оценивать по термину, не преобразованному лексическим анализом. Создание дополнительного поля для неизменной строки и использование анализатора с сохранением содержимого, который создает маркеры с целыми терминами, — это решение для сопоставления шаблонов и для сопоставления строк запросов, содержащих специальные символы.

> [!TIP]
> Если вы знакомы с API-интерфейсами POST и RESTFUL, [Скачайте коллекцию примеров запросов](https://github.com/Azure-Samples/azure-search-postman-samples/) , чтобы запросить частичные термины и специальные символы, описанные в этой статье.

## <a name="about-partial-term-search"></a>Сведения о частичном поиске терминов

Когнитивный поиск Azure сканирует все лексемы в индексе и не находит совпадение в частичном термине, если не включить подстановочные знаки-заместители ( `*` и `?` ) или отформатировать запрос как регулярное выражение. Частичные термины указываются с помощью этих методов:

+ [Запросы регулярных выражений](query-lucene-syntax.md#bkmk_regex) могут быть любым регулярным выражением, допустимым в Apache Lucene. 

+ [Подстановочные операторы с сопоставлением префиксов](query-simple-syntax.md#prefix-search) относятся к общему распознанному шаблону, включающему в себя начало термина, за которым следуют `*` `?` операторы или суффиксы, такие как `search=cap*` сопоставление для кап'на набережнаяного ИНН или ГАКК капитала. Префикс сопоставления поддерживается как в простом, так и в полном синтаксисе запроса Lucene.

+ [Подстановочный знак с инфиксные и сопоставлением суффикса](query-lucene-syntax.md#bkmk_wildcard) помещает `*` операторы и в `?` Начало Терма и требует синтаксиса регулярного выражения (где выражение заключено в символы косой черты). Например, строка запроса ( `search=/.*numeric*./` ) возвращает результаты по буквенным и буквенно-цифровым символам в виде суффикса и инфиксные.

Для частичного поиска терминов или шаблонов, а также нескольких других форм запросов, таких как нечеткий поиск, анализаторы не используются во время запроса. Для этих форм запросов, которые синтаксический анализатор обнаруживает при наличии операторов и разделителей, строка запроса передается в ядро без лексического анализа. Для этих форм запросов анализатор, указанный в поле, игнорируется.

> [!NOTE]
> Если частичная строка запроса содержит символы, такие как косые черты в фрагменте URL-адреса, может потребоваться добавить управляющие символы. В JSON обратная косая черта преобразуется в `/` обратную косую черту `\` . Таким образом, `search=/.*microsoft.com\/azure\/.*/` является синтаксисом фрагмента URL-адреса «Microsoft.com/Azure/».

## <a name="solving-partialpattern-search-problems"></a>Решение проблем с частичным поиском или недоступностью шаблонов

Если необходимо выполнить поиск по фрагментам или шаблонам или специальным символам, можно переопределить анализатор по умолчанию с помощью пользовательского анализатора, работающего в более простых правилах разметки, оставив всю строку в индексе. Выполнив шаг назад, подход выглядит следующим образом:

1. Определите поле для хранения неизменной версии строки (если требуется анализировать и неанализируемый текст во время запроса).
1. Оцените и выберите один из различных анализаторов, которые выдают маркеры на правильном уровне гранулярности.
1. Назначение анализатора полю
1. Сборка и тестирование индекса

> [!TIP]
> Оценка анализаторов — это итеративный процесс, требующий частых перестроение индекса. Этот шаг можно упростить с помощью POST, интерфейсов API для [создания индекса](/rest/api/searchservice/create-index), [удаления индекса](/rest/api/searchservice/delete-index),[загрузки документов](/rest/api/searchservice/addupdate-or-delete-documents)и [поиска документов](/rest/api/searchservice/search-documents). Для документов загрузки текст запроса должен содержать небольшой репрезентативный набор данных, который необходимо протестировать (например, поле с номерами телефонов или кодами продуктов). Используя эти API в той же коллекции POST, можно быстро пройти эти шаги.

## <a name="1---create-a-dedicated-field"></a>1. Создание выделенного поля

Анализаторы определяют, как термины размечены в индексе. Поскольку анализаторы назначаются для каждого поля, можно создавать поля в индексе для оптимизации различных сценариев. Например, можно определить "Феатурекоде" и "Феатурекодережекс" для поддержки обычного полнотекстового поиска в первом и расширенного сопоставления шаблонов во втором. Анализаторы, назначенные каждому полю, определяют, как содержимое каждого поля размечено в индексе.  

```json
{
  "name": "featureCode",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": null
},
{
  "name": "featureCodeRegex",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": "my_custom_analyzer"
},
```

<a name="set-an-analyzer"></a>

## <a name="2---set-an-analyzer"></a>2. Установка анализатора

При выборе анализатора, создающего маркеры полного термина, доступны следующие анализаторы:

| Анализатор | Расширения функциональности |
|----------|-----------|
| [Анализаторы языка](index-add-language-analyzers.md) | Сохраняет дефисы в составных словах, строках, гласных фрагментах и формах глаголов. Если шаблоны запросов содержат тире, может быть достаточно использования анализатора языка. |
| [This](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) | Содержимое всего поля размечено как один термин. |
| [Бель](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html) | Разделяется только на пробелы. Термины, содержащие дефисы или другие символы, рассматриваются как один маркер. |
| [Пользовательский анализатор](index-add-custom-analyzers.md) | такую Создание настраиваемого анализатора позволяет указать как лексему, так и фильтр маркеров. Предыдущие Анализаторы должны использоваться "как есть". Настраиваемый анализатор позволяет выбрать используемые маркеры и фильтры маркеров. <br><br>Рекомендуемым сочетанием является лексема [ключевых слов](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html) с [фильтром маркеров нижнего регистра](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html). Само по себе встроенное средство [анализа ключевых слов](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) не имеет строчных букв, что может привести к сбою запросов. Пользовательский анализатор предоставляет механизм для добавления фильтра маркеров нижнего регистра. |

Если вы используете средство тестирования веб-API, например POST, можно добавить [вызов RESTful для анализатора тестов](/rest/api/searchservice/test-analyzer) , чтобы проверить выходные данные с маркерами.

Для работы с необходимо иметь заполненный индекс. При наличии существующего индекса и поля, содержащего дефисы или частичные термины, можно испытать различные анализаторы для определенных терминов, чтобы узнать, какие токены будут выдаваться.  

1. Сначала проверьте стандартный анализатор, чтобы увидеть, как термины размечены по умолчанию.

   ```json
   {
   "text": "SVP10-NOR-00",
   "analyzer": "standard"
   }
    ```

1. Оцените ответ, чтобы увидеть, как текст размечен в индексе. Обратите внимание на то, как все термины имеют более низкие регистр, дефисы удалены, а подстроки разбиваются на отдельные маркеры. Только те запросы, которые соответствуют этим токенам, будут возвращать этот документ в результатах. Запрос, включающий "10-или", завершится ошибкой.

    ```json
    {
        "tokens": [
            {
                "token": "svp10",
                "startOffset": 0,
                "endOffset": 5,
                "position": 0
            },
            {
                "token": "nor",
                "startOffset": 6,
                "endOffset": 9,
                "position": 1
            },
            {
                "token": "00",
                "startOffset": 10,
                "endOffset": 12,
                "position": 2
            }
        ]
    }
    ```
1. Теперь измените запрос для использования `whitespace` `keyword` анализатора или:

    ```json
    {
    "text": "SVP10-NOR-00",
    "analyzer": "keyword"
    }
    ```

1. Теперь ответ состоит из одного маркера в верхнем регистре с тире, сохраненным как часть строки. Если необходимо выполнить поиск по шаблону или частичному термину, например "10-or", механизм запросов теперь имеет базу для поиска соответствия.


    ```json
    {

        "tokens": [
            {
                "token": "SVP10-NOR-00",
                "startOffset": 0,
                "endOffset": 12,
                "position": 0
            }
        ]
    }
    ```
> [!Important]
> Имейте в виду, что средства синтаксического анализа запросов часто имеют более низкие регистры в выражении поиска при построении дерева запросов. Если вы используете анализатор, не использующий текстовые входные данные в нижнем регистре во время индексирования и не получаете ожидаемые результаты, это может быть причиной. Решение заключается в добавлении фильтра маркеров нижнего регистра, как описано в разделе "использование пользовательских анализаторов" ниже.

## <a name="3---configure-an-analyzer"></a>3. Настройка анализатора
 
Независимо от того, оцениваете анализаторы или перемещаясь с определенной конфигурацией, необходимо указать анализатор для определения поля и, возможно, настроить сам анализатор, если вы не используете встроенный анализатор. При переключении анализаторов обычно требуется перестроить индекс (удалить, повторно создать и перезагрузить). 

### <a name="use-built-in-analyzers"></a>Использование встроенных анализаторов

Встроенные анализаторы могут быть заданы по имени в `analyzer` свойстве определения поля, при этом в индексе не требуется дополнительная настройка. В следующем примере показано, как задать `whitespace` анализатор для поля. 

Другие сценарии и дополнительные сведения о других встроенных анализаторах см. в разделе [встроенные анализаторы](./index-add-custom-analyzers.md#built-in-analyzers). 

```json
    {
      "name": "phoneNumber",
      "type": "Edm.String",
      "key": false,
      "retrievable": true,
      "searchable": true,
      "analyzer": "whitespace"
    }
```

### <a name="use-custom-analyzers"></a>Использование пользовательских анализаторов

Если вы используете [Пользовательский анализатор](index-add-custom-analyzers.md), определите его в индексе с помощью определяемого пользователем сочетания анализатора лексемы, фильтра маркеров с возможными параметрами конфигурации. Затем сослаться на определение поля так же, как у встроенного анализатора.

Если цель представляет собой разметку для всего термина, рекомендуется использовать пользовательский анализатор, состоящий из **ключевых слов** и **фильтров маркеров нижнего регистра** .

+ Лексема ключевых слов создает один маркер для всего содержимого поля.
+ Фильтр маркеров нижнего регистра преобразует прописные буквы в строчные. Синтаксические анализаторы запросов обычно имеют прописные буквы в верхнем регистре. В нижнем регистре хоможенизес входные данные с помощью лексемных терминов.

В следующем примере показан пользовательский анализатор, который предоставляет маркеры ключевых слов и фильтр маркеров нижнего регистра.

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
],

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": []
```

> [!NOTE]
> `keyword_v2`Лексема и `lowercase` Фильтр маркеров известны системой и используют их конфигурации по умолчанию, поэтому их можно ссылаться по имени, не определяя их первыми.

## <a name="4---build-and-test"></a>4. сборка и тестирование

Определив индекс с анализаторами и определениями полей, которые поддерживают ваш сценарий, загрузите документы с репрезентативными строками, чтобы можно было тестировать частичные строковые запросы. 

В предыдущих разделах была объяснена логика. В этом разделе описывается каждый API, который следует вызывать при тестировании решения. Как отмечалось ранее, при использовании интерактивного средства веб-тестирования, такого как POST, можно быстро выполнять эти задачи.

+ [Удалить индекс](/rest/api/searchservice/delete-index) удаляет существующий индекс с тем же именем, чтобы его можно было повторно создать.

+ [Создать индекс](/rest/api/searchservice/create-index) создает структуру индекса в службе поиска, включая определения и поля анализатора с указанием анализатора.

+ [Загрузка документов](/rest/api/searchservice/addupdate-or-delete-documents) импортирует документы, имеющие ту же структуру, что и индекс, а также содержимое, доступное для поиска. После выполнения этого шага индекс будет готов к выполнению запроса или тесту.

+ [Анализатор тестов](/rest/api/searchservice/test-analyzer) появился в мастере [установки анализатора](#set-an-analyzer). Протестируйте некоторые строки в индексе с помощью различных анализаторов, чтобы понять, как будут размечены условия.

+ В [документах поиска](/rest/api/searchservice/search-documents) объясняется, как создать запрос запроса, используя [простой синтаксис](query-simple-syntax.md) или [полный синтаксис Lucene](query-lucene-syntax.md) для подстановочных знаков и регулярных выражений.

  Для запросов с частичными терминами, таких как запрос "3-6214" для поиска совпадения в "+ 1 (425) 703-6214", можно использовать простой синтаксис: `search=3-6214&queryType=simple` .

  Для запросов инфиксные и суффиксов, таких как запрос "num" или "numeric" для поиска совпадений на "буквенно-цифровые", используйте полный синтаксис Lucene и регулярное выражение: `search=/.*num.*/&queryType=full`

## <a name="tune-query-performance"></a>Настройка производительности запросов

Если вы реализуете рекомендуемую конфигурацию, включающую в себя keyword_v2 лексему и фильтр маркеров более низкого регистра, вы можете заметить снижение производительности запросов из-за обработки дополнительных фильтров маркеров для существующих токенов в индексе. 

В следующем примере добавляется [едженграмтокенфилтер](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenizer.html) , чтобы префикс совпадал быстрее. Дополнительные маркеры создаются в 2-25 символах, содержащих символы: (не только MS, MSF, MSFT, MSFT/, MSFT/S, MSFT/SQ, MSFT/SQL). 

Как можно себе представить, Дополнительная разметка приводит к увеличению индекса. Если у вас есть достаточная емкость для размещения индекса большего размера, то этот подход с более быстрым временем ответа может быть лучшим решением.

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
],

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase", "my_edgeNGram"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": [
  {
  "@odata.type":"#Microsoft.Azure.Search.EdgeNGramTokenFilterV2",
  "name":"my_edgeNGram",
  "minGram": 2,
  "maxGram": 25,
  "side": "front"
  }
]
```

## <a name="next-steps"></a>Дальнейшие действия

В этой статье объясняется, как анализаторы вносят проблемы в запросы и решают проблемы запросов. В качестве следующего шага подробнее рассмотрим анализатор влияния на индексирование и обработку запросов. В частности, рассмотрите возможность использования API анализа текста для возврата выходных данных с маркерами, чтобы вы могли точно увидеть, что создает анализатор для вашего индекса.

+ [Языковые анализаторы](search-language-support.md)
+ [Анализаторы для обработки текста в Azure Когнитивный поиск](search-analyzers.md)
+ [API анализа текста (ОСТАВШАЯся)](/rest/api/searchservice/test-analyzer)
+ [Как работает полнотекстовый поиск (Архитектура запросов)](search-lucene-query-architecture.md)