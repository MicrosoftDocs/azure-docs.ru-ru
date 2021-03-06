---
title: Моделирование сложных типов данных
titleSuffix: Azure Cognitive Search
description: Вложенные или иерархические структуры данных можно моделировать в индексе Azure Когнитивный поиск с помощью типов данных ComplexType и Collections.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
tags: complex data types; compound data types; aggregate data types
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/27/2020
ms.openlocfilehash: b0b2dd9904682121c83b22b9029097e7ee57fb11
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "96303756"
---
# <a name="how-to-model-complex-data-types-in-azure-cognitive-search"></a>Как моделировать сложные типы данных в Azure Когнитивный поиск

Внешние наборы данных, используемые для заполнения индекса Azure Когнитивный поиск, могут находиться во многих фигурах. Иногда они включают иерархические или вложенные структуры. Примеры могут включать несколько адресов для одного клиента, несколько цветов и размеров для одного номера SKU, несколько авторов одной книги и т. д. В терминах моделирования можно увидеть, что эти структуры называются *сложными*, *составными*, *составными* или *агрегатными* типами данных. Термин Azure Когнитивный поиск использует для этой концепции **сложный тип**. В Когнитивный поиск Azure сложные типы моделируются с помощью **сложных полей**. Сложное поле — это поле, содержащее дочерние поля (вложенные поля), которые могут иметь любой тип данных, включая другие сложные типы. Это работает так же, как структурированные типы данных в языке программирования.

Сложные поля представляют либо один объект в документе, либо массив объектов, в зависимости от типа данных. Поля типа `Edm.ComplexType` представляют отдельные объекты, а поля типа `Collection(Edm.ComplexType)` представляют массивы объектов.

Azure Когнитивный поиск изначально поддерживает сложные типы и коллекции. Эти типы позволяют моделировать почти любую структуру JSON в индексе Azure Когнитивный поиск. В предыдущих версиях Azure Когнитивный поиск API можно было импортировать только плоские наборы строк. В последней версии индекс теперь более точно соответствует исходным данным. Иными словами, если исходные данные имеют сложные типы, то индекс может также иметь сложные типы.

Чтобы приступить к работе, мы рекомендуем использовать [набор данных гостиниц](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md), который можно загрузить в мастере **импорта данных** в портал Azure. Мастер обнаруживает сложные типы в источнике и предлагает схему индекса на основе обнаруженных структур.

> [!Note]
> Поддержка сложных типов стала общедоступной, начиная с `api-version=2019-05-06` . 
>
> Если решение поиска основано на более ранних обходах плоских наборов данных в коллекции, необходимо изменить индекс, включив в него сложные типы, которые поддерживаются в последней версии API. Дополнительные сведения об обновлении версий API см. в статье [обновление до последней версии REST API](search-api-migration.md) или [обновление до последней версии пакета SDK для .NET](search-dotnet-sdk-migration-version-9.md).

## <a name="example-of-a-complex-structure"></a>Пример сложной структуры

Следующий документ JSON состоит из простых полей и сложных полей. Сложные поля, такие как `Address` и `Rooms` , имеют подполя. `Address` имеет один набор значений для этих вложенных полей, так как это один объект в документе. Напротив, `Rooms` имеет несколько наборов значений для своих вложенных полей, по одному для каждого объекта в коллекции.


```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Description": "Ideally located on the main commercial artery of the city in the heart of New York.",
  "Tags": ["Free wifi", "on-site parking", "indoor pool", "continental breakfast"]
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY"
  },
  "Rooms": [
    {
      "Description": "Budget Room, 1 Queen Bed (Cityside)",
      "RoomNumber": 1105,
      "BaseRate": 96.99,
    },
    {
      "Description": "Deluxe Room, 2 Double Beds (City View)",
      "Type": "Deluxe Room",
      "BaseRate": 150.99,
    }
    . . .
  ]
}
```

## <a name="indexing-complex-types"></a>Индексирование сложных типов

Во время индексирования можно использовать не более 3000 элементов во всех сложных коллекциях внутри одного документа. Элемент сложной коллекции является членом этой коллекции, поэтому в случае комнат (единственная сложная коллекция в примере отеля) Каждая комната является элементом. В приведенном выше примере, если "Секретная точка Motel" имела 500 помещений, в документе отеля будет 500 элементов комнаты. Для вложенных сложных коллекций каждый вложенный элемент также подсчитывается в дополнение к внешнему (родительскому) элементу.

Это ограничение применяется только к сложным коллекциям, а не к сложным типам (например, Address) или коллекциям строк (например, тегам).

## <a name="creating-complex-fields"></a>Создание сложных полей

Как и в случае с любым определением индекса, для создания схемы, включающей сложные типы, можно использовать портал, [REST API](/rest/api/searchservice/create-index)или [пакет SDK для .NET](/dotnet/api/azure.search.documents.indexes.models.searchindex) . 

В следующем примере показана схема индекса JSON с простыми полями, коллекциями и сложными типами. Обратите внимание, что внутри сложного типа каждое вложенное поле имеет тип и может иметь атрибуты, как и поля верхнего уровня. Схема соответствует приведенным выше примерам данных. `Address` — Это сложное поле, которое не является коллекцией (в отеле есть один адрес). `Rooms` — Это поле комплексной коллекции (в отеле содержится много комнат).

```json
{
  "name": "hotels",
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
    { "name": "Address", "type": "Edm.ComplexType",
      "fields": [
        { "name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
        { "name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
        { "name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true }
      ]
    },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)",
      "fields": [
        { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
        { "name": "Type", "type": "Edm.String", "searchable": true },
        { "name": "BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true }
      ]
    }
  ]
}
```

## <a name="updating-complex-fields"></a>Обновление сложных полей

Все [правила переиндексации](search-howto-reindex.md) , применяемые к полям в целом, по-прежнему применяются к сложным полям. Повторное описание некоторых основных правил, при добавлении поля в сложный тип, не требует перестроения индекса, но большинство изменений выполняется.

### <a name="structural-updates-to-the-definition"></a>Структурные обновления определения

Новые подполя можно добавить в сложное поле в любое время без перестроения индекса. Например, добавление "ZipCode" в `Address` или "удобствами" в `Rooms` разрешено, так же как и Добавление поля верхнего уровня в индекс. Существующие документы имеют значение NULL для новых полей, пока вы не заполните эти поля явным образом, обновив данные.

Обратите внимание, что внутри сложного типа каждое вложенное поле имеет тип и может иметь атрибуты, как и поля верхнего уровня.

### <a name="data-updates"></a>Обновления данных

Обновление существующих документов в индексе с `upload` действием выполняется одинаково для сложных и простых полей — все поля заменяются. Однако `merge` (или `mergeOrUpload` при применении к существующему документу) не работает одинаково для всех полей. В частности, `merge` не поддерживает слияние элементов в коллекции. Это ограничение существует для коллекций типов-примитивов и сложных коллекций. Чтобы обновить коллекцию, необходимо получить значение полной коллекции, внести изменения, а затем включить новую коллекцию в запрос API индекса.

## <a name="searching-complex-fields"></a>Поиск сложных полей

Выражения поиска в свободной форме работают, как ожидалось, со сложными типами. Если любое поле, поддерживающее Поиск, или вложенное поле в любом месте документа соответствует, то сам документ является совпадением.

Запросы более сложны, если имеется несколько терминов и операторов, а некоторые термины имеют указанные имена полей, как это возможно в [синтаксисе Lucene](query-lucene-syntax.md). Например, этот запрос пытается найти два подполя адреса в полях «Портленде» и «OR»:

> `search=Address/City:Portland AND Address/State:OR`

Такие запросы не *взаимосвязаны* для полнотекстового поиска, в отличие от фильтров. В фильтрах запросы к вложенным полям сложной коллекции сопоставляются с помощью переменных диапазона в [ `any` или `all` ](search-query-odata-collection-operators.md). Вышеприведенный запрос Lucene возвращает документы, содержащие "Портленде, Майн" и "Портленде, Орегон" вместе с другими городами в Орегон. Это происходит потому, что каждое предложение применяется ко всем значениям поля во всем документе, поэтому концепция «текущего вложенного документа» не существует. Дополнительные сведения об этом см. [в разделе Основные сведения о фильтрах коллекции OData в когнитивный Поиск Azure](search-query-understand-collection-filters.md).

## <a name="selecting-complex-fields"></a>Выбор сложных полей

`$select`Параметр используется для выбора полей, возвращаемых в результатах поиска. Чтобы использовать этот параметр для выбора конкретных вспомогательных полей сложного поля, включите родительское поле и подполе, разделенные косой чертой ( `/` ).

> `$select=HotelName, Address/City, Rooms/BaseRate`

Если вы хотите, чтобы они были доступны в результатах поиска, поля должны быть помечены в индексе как доступные для получения. В инструкции можно использовать только те поля, которые помечены как доступные для получения `$select` .

## <a name="filter-facet-and-sort-complex-fields"></a>Фильтрация, аспект и сортировка сложных полей

Тот же [синтаксис пути OData](query-odata-filter-orderby-syntax.md) , который используется для фильтрации и полей поиска, можно также использовать для аспектов, сортировки и выбора полей в запросе поиска. Для сложных типов применяются правила, определяющие, какие подполя могут быть помечены как доступные для сортировки или для использования в качестве аспектов. Дополнительные сведения об этих правилах см. в [справочнике по API создания индекса](/rest/api/searchservice/create-index).

### <a name="faceting-sub-fields"></a>Аспекты дочерних полей

Любое подполе может быть помечено как Facet, если оно не относится к типу `Edm.GeographyPoint` или `Collection(Edm.GeographyPoint)` .

Количество документов, возвращенное в результатах аспекта, вычисляется для родительского документа (отеля), а не для вложенных документов в сложной коллекции (комнатах). Например, предположим, что в отеле есть 20 комнат типа "набор". Учитывая этот параметр аспекта `facet=Rooms/Type` , число аспектов будет равно одному для отеля, а не 20 для комнат.

### <a name="sorting-complex-fields"></a>Сортировка сложных полей

Операции сортировки применяются к документам (гостиницам), а не к поддокументам (комнатам). При наличии коллекции сложных типов, например комнат, важно понимать, что вы не можете отсортировать все комнаты. На самом деле вы не можете выполнить сортировку по какой бы то ни было коллекции.

Операции сортировки работают, когда поля содержат одно значение для каждого документа, является ли поле простым полем или вложенным полем сложного типа. Например, `Address/City` можно выполнить сортировку, так как имеется только один адрес в отеле, поэтому `$orderby=Address/City` сортирует Гостиницы по городу.

### <a name="filtering-on-complex-fields"></a>Фильтрация по сложным полям

В критерии фильтра можно ссылаться на подполя сложного поля. Просто используйте тот же [синтаксис пути OData](query-odata-filter-orderby-syntax.md) , который используется для аспектов, сортировки и выбора полей. Например, следующий фильтр возвратит все гостиницы в Канаде:

> `$filter=Address/Country eq 'Canada'`

Для фильтрации по полю сложной коллекции можно использовать **лямбда-выражение** с [ `any` `all` операторами и](search-query-odata-collection-operators.md). В этом случае **переменная диапазона** лямбда-выражения является объектом с вспомогательными полями. Вы можете ссылаться на эти подполя с помощью стандартного синтаксиса пути OData. Например, следующий фильтр возвратит все гостиницы, имеющие по крайней мере одну комнату Deluxe и все Курение комнаты:

> `$filter=Rooms/any(room: room/Type eq 'Deluxe Room') and Rooms/all(room: not room/SmokingAllowed)`

Как и в случае с простыми полями верхнего уровня, простые вложенные поля сложных полей можно включать в фильтры только в том случае, если для атрибута **FILTERED** задано значение `true` в определении индекса. Дополнительные сведения см. в [справочнике по API создания индекса](/rest/api/searchservice/create-index).

## <a name="next-steps"></a>Дальнейшие действия

Попробуйте [набор данных гостиниц](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md) в мастере **импорта данных** . Для доступа к данным потребуется Cosmos DB сведения о подключении, указанные в файле сведений.

С этой информацией, первым шагом мастера является создание нового Azure Cosmos DB источника данных. Далее в мастере при получении страницы целевой индекс вы увидите индекс со сложными типами. Создайте и загрузите этот индекс, а затем выполните запросы, чтобы понять новую структуру.

> [!div class="nextstepaction"]
> [Краткое руководство. Мастер создания портала для импорта, индексирования и запросов](search-get-started-portal.md)
