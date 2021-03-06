---
title: Возврат семантического ответа
titleSuffix: Azure Cognitive Search
description: Описывает композицию семантического ответа и получение ответов из результирующего набора.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/12/2021
ms.openlocfilehash: 9bb62544887e0bc0269b98cd98fbf97fc477352f
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104722435"
---
# <a name="return-a-semantic-answer-in-azure-cognitive-search"></a>Получение семантического ответа в Azure Когнитивный поиск

> [!IMPORTANT]
> Семантический поиск находится в общедоступной предварительной версии, доступном только в предварительной версии REST API. Функции предварительной версии предлагаются "как есть", в [дополнение к дополнительным условиям использования](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)и не обязательно должны иметь одинаковую реализацию при общедоступной доступности. Эти функции являются оплачиваемыми. Дополнительные сведения см. в разделе [доступность и цены](semantic-search-overview.md#availability-and-pricing).

При составления [семантического запроса](semantic-how-to-query-request.md)можно дополнительно извлечь содержимое из документов с наибольшим соответствием, которые "отвечает" на запрос напрямую. В ответ можно включить один или несколько ответов, которые затем можно отобразить на странице поиска, чтобы улучшить взаимодействие с пользователем в приложении.

В этой статье вы узнаете, как запросить семантический ответ, распаковать ответ и узнать, какие характеристики содержимого наиболее пригодного для создания высококачественных ответов.

## <a name="prerequisites"></a>Предварительные требования

Все необходимые условия, применяемые к [семантическим запросам](semantic-how-to-query-request.md) , также применяются к ответам, включая уровень служб и регион.

+ Логика запроса должна включать параметры семантического запроса, а также параметр "Answers". Необходимые параметры описаны в этой статье.

+ Строки запросов, введенные пользователем, должны быть обработаны на языке с характеристиками вопроса (что, где, когда, как).

+ Документы поиска должны содержать текст с характеристиками ответа, и этот текст должен находиться в одном из полей, перечисленных в "searchFields". Например, при наличии запроса "что такое хэш-таблица", если ни один из searchFields не содержит прохождение, включающее "хэш-таблицу —...", ответ вряд ли будет возвращен.

## <a name="what-is-a-semantic-answer"></a>Что такое семантический ответ?

Семантический ответ — это вложенная структура [ответа семантического запроса](semantic-how-to-query-request.md). Он состоит из одного или более точных постановок из документа поиска, который обстоится как ответ на запрос, который выглядит как вопрос. Для возврата ответа фразы или предложения должны существовать в документе поиска с характеристиками языка ответа, а сам запрос должен быть в качестве вопроса.

Для выбора наилучшего ответа в Когнитивный поиск используется модель считывания машинного чтения. Модель создает набор потенциальных ответов из доступного содержимого, и при достижении высокого уровня достоверности будет предложен ответ.

Ответы возвращаются в виде независимого объекта верхнего уровня в полезных данных ответа на запрос, который можно выбрать для отображения на страницах поиска, а также в результатах поиска. Структурно это элемент массива в ответе, состоящий из текста, ключа документа и оценки достоверности.

<a name="query-params"></a>

## <a name="how-to-request-semantic-answers-in-a-query"></a>Запрос семантических ответов в запросе

Чтобы получить семантический ответ, запрос должен иметь семантику "queryType", "Куерилангуаже", "searchFields" и параметр "Answers". Указание параметра "Answers" не гарантирует, что вы получите ответ, но запрос должен включать этот параметр, если требуется вызывать метод обработки ответа.

Параметр "searchFields" имеет решающее значение для возврата высококачественного ответа как в терминах содержимого, так и в порядке (см. ниже). 

```json
{
    "search": "how do clouds form",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "searchFields": "title,locations,content",
    "answers": "extractive|count-3",
    "count": "true"
}
```

+ Строка запроса не должна иметь значение null, и ее следует сформулировать как вопрос. В этой предварительной версии значения "queryType" и "Куерилангуаже" должны быть заданы в точности так, как показано в примере.

+ Параметр "searchFields" определяет, какие строковые поля предоставляют маркеры модели извлечения. Те же поля, которые создают субтитры, также дают ответы. Точные инструкции по заданию этого поля для работы с заголовками и ответами см. в разделе [Set searchFields](semantic-how-to-query-request.md#searchfields). 

+ Для "Answers" создается параметр `"answers": "extractive"` , где число возвращаемых ответов по умолчанию равно одному. Вы можете увеличить количество ответов, добавив число, как показано в приведенном выше примере, максимум до пяти.  Требуется ли больше одного ответа, зависит от взаимодействия с пользователем приложения и от того, как нужно отображать результаты.

## <a name="deconstruct-an-answer-from-the-response"></a>Деконструировать ответ из ответа

Ответы предоставляются в @search.answers массиве, который сначала появляется в ответе. Если ответ определен неопределенным образом, ответ будет отображаться как `"@search.answers": []` . При проектировании страницы результатов поиска, содержащей ответы, не забудьте обрабатывать случаи, в которых ответы не найдены.

В разделе @search.answers key — это ключ документа или идентификатор совпадения. Используя ключ документа, можно использовать API [документов подстановки](/rest/api/searchservice/lookup-document) для получения всех или всех частей документа поиска, включаемых на страницу поиска или на страницу сведений.

Как "текст", так и "выделение" предоставляют одинаковое содержимое как в виде обычного текста, так и с помощью светлых фрагментов. По умолчанию основные элементы имеют стиль `<em>` , который можно переопределить с помощью существующих параметров хигхлигхтпретаг и хигхлигхтпосттаг. Как отмечалось в других местах, веществом ответа является буквальное содержимое из документа поиска. Модель извлечения ищет характеристики ответа, чтобы найти соответствующее содержимое, но не создает новый язык в ответе.

Оценка — это показатель достоверности, отражающий надежность ответа. Если в ответе есть несколько ответов, этот показатель используется для определения порядка. Верхние и верхние заголовки могут быть производными от разных документов поиска, где первый ответ исходит от одного документа, а верхний заголовок — от другого, но в общем случае вы увидите те же документы в верхней позиции в каждом массиве.

За ответами следует массив "value", который всегда включает оценки, заголовки и все поля, которые могут быть извлечены по умолчанию. Если указан параметр SELECT, массив "value" ограничивается указанными полями. Дополнительные сведения об элементах в ответе см. в разделе [Создание семантического запроса](semantic-how-to-query-request.md).

При наличии запроса с формой "принцип работы облаков" в ответе возвращается следующий ответ:

```json
{
    "@search.answers": [
        {
            "key": "4123",
            "text": "Sunlight heats the land all day, warming that moist air and causing it to rise high into the   atmosphere until it cools and condenses into water droplets. Clouds generally form where air is ascending (over land in this case),   but not where it is descending (over the river).",
            "highlights": "Sunlight heats the land all day, warming that moist air and causing it to rise high into the   atmosphere until it cools and condenses into water droplets. Clouds generally form<em> where air is ascending</em> (over land in this case),   but not where it is<em> descending</em> (over the river).",
            "score": 0.94639826
        }
    ],
    "value": [
        {
            "@search.score": 0.5479723,
            "@search.rerankerScore": 1.0321671911515296,
            "@search.captions": [
                {
                    "text": "Like all clouds, it forms when the air reaches its dew point—the temperature at which an air mass is cool enough for its water vapor to condense into liquid droplets. This false-color image shows valley fog, which is common in the Pacific Northwest of North America.",
                    "highlights": "Like all<em> clouds</em>, it<em> forms</em> when the air reaches its dew point—the temperature at    which an air mass is cool enough for its water vapor to condense into liquid droplets. This false-color image shows valley<em> fog</em>, which is common in the Pacific Northwest of North America."
                }
            ],
            "title": "Earth Atmosphere",
            "content": "Fog is essentially a cloud lying on the ground. Like all clouds, it forms when the air reaches its dew point—the temperature at  \n\nwhich an air mass is cool enough for its water vapor to condense into liquid droplets.\n\nThis false-color image shows valley fog, which is common in the Pacific Northwest of North America. On clear winter nights, the \n\nground and overlying air cool off rapidly, especially at high elevations. Cold air is denser than warm air, and it sinks down into the \n\nvalleys. The moist air in the valleys gets chilled to its dew point, and fog forms. If undisturbed by winds, such fog may persist for \n\ndays. The Terra satellite captured this image of foggy valleys northeast of Vancouver in February 2010.\n\n\n",
            "locations": [
                "Pacific Northwest",
                "North America",
                "Vancouver"
            ]
        }
```

## <a name="tips-for-producing-high-quality-answers"></a>Советы по созданию высококачественных ответов

Для получения лучших результатов следует возвращать семантические ответы на документ, совокупности следующие характеристики:

+ "searchFields" должен предоставлять поля, которые предлагают достаточно текста, в котором может быть найден ответ. В качестве ответа может использоваться только буквальный текст из документа.

+ строки запроса не должны иметь значение null (Search = `*` ), а строка должна иметь характеристики вопроса, в отличие от поиска по ключевым словам (последовательный список произвольных терминов или фраз). Если строка запроса не является ответом, обработка ответов пропускается, даже если в запросе в качестве параметра запроса указано "Answers".

+ Семантическое извлечение и формирование сводных данных имеют ограничения на то, сколько маркеров на документ можно проанализировать своевременно. На практике, при наличии больших документов, которые выполняются на сотнях страниц, следует попытаться разбить содержимое на более мелкие документы первыми.

## <a name="next-steps"></a>Следующие шаги

+ [Обзор семантического поиска](semantic-search-overview.md)
+ [Алгоритм семантического ранжирования](semantic-ranking.md)
+ [Алгоритм ранжирования подобия](index-ranking-similarity.md)
+ [Создание семантического запроса](semantic-how-to-query-request.md)