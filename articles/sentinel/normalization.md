---
title: Нормализация данных в Azure Sentinel | Документация Майкрософт
description: В этой статье объясняется, как Sentinel Azure нормализует данные из множества различных источников и подробно описывает схему нормализации.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: yelevin
ms.openlocfilehash: 5d847ac7ed805ad88bc24ed63896edc6f7596f9b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "101729782"
---
# <a name="normalization-in-azure-sentinel"></a>Нормализация в Azure Sentinel

В этой статье описывается нормализация схемы данных в Azure Sentinel, а именно схема сетевых подключений и сеансов, в которую включена ссылка.

## <a name="what-is-normalization"></a>Что такое нормализация

Совместная работа с различными типами данных и таблицами представляет проблемы. Необходимо ознакомиться с множеством различных типов данных и схем, написать и использовать уникальный набор правил аналитики, книг и запросов на поиск для каждого, даже для тех, кто совместно использует общие черты (например, относится к устройствам брандмауэра). Взаимосвязь между различными типами данных, необходимыми для расследования и поиском, также усложняется. Azure Sentinel обеспечивает удобный интерфейс для обработки данных из различных источников в единообразных нормализованных представлениях.

**Общая информационная модель** Azure Sentinel состоит из трех аспектов:

- **Нормализованные схемы** охватывают общие наборы прогнозируемых типов событий (таблиц), с которыми легко работать и создавать унифицированные возможности (например, сетевая таблица). Схема также содержит нормализованное соглашение о столбцах и определения стандартизации значений и форматов (стандартное представление данных, например IP-адреса).

- **Синтаксические анализаторы** сопоставляют существующие данные различных типов с нормализованной схемой. В соответствии с моделью данные могут быть проанализированы в нормализованную схему во время запроса (с помощью функций) или во время приема. В настоящее время поддерживается только синтаксический анализ времени выполнения запроса.

- **Содержимое для каждой нормализованной схемы** включает правила аналитики, интерактивные книги, поисковые запросы и дополнительное содержимое.

### <a name="current-release"></a>Текущий выпуск

В этом выпуске [нормализованная схема сетевых подключений и сеансов](./normalization-schema.md) (версия 1.0.0) доступна для общедоступной предварительной версии. Схема описывает сетевые подключения или сеансы, такие как зарегистрированные брандмауэрами, данные проводки, NSG, NetFlow, прокси-системы и шлюзы Интернета.  Выбранные парсеры времени выполнения запросов и правила аналитики доступны вместе со схемой и используют их.

В настоящее время схема доступна только через средства синтаксического анализа времени выполнения запросов (см. раздел средства синтаксического анализа).

Можно анализировать данные в дополнительные представления и использовать [модель именования сущностей оссем](https://ossemproject.com/cdm/entities/intro.html#) для создания столбцов, которые будут соответствовать существующим и будущим нормализованным таблицам.

## <a name="normalized-schema-and-parsing"></a>Нормализованная схема и синтаксический анализ

### <a name="how-our-normalized-schemas-are-defined"></a>Как определяются наши нормализованные схемы

Azure Sentinel соответствует общей информационной модели [метаданных событий безопасности с открытым исходным кодом (оссем)](https://ossemproject.com/intro.html) , что позволяет сопоставлять прогнозируемые сущности между нормализованными таблицами. ОССЕМ — это проект с индикатором сообщества, который посвящен главным образом документации и стандартизации журналов событий безопасности из различных источников данных и операционных систем. Кроме того, проект предоставляет общую информационную модель (CIM), которую можно использовать для инженеров по обработке данных во время процедур нормализации данных, чтобы разрешить аналитикам безопасности запрашивать и анализировать данные в различных источниках данных.

[CIM оссем](https://ossemproject.com/cdm/intro.html) определяет набор сущностей (например, файл, узел, IP-адрес, процесс) и определяет набор атрибутов для каждой такой сущности. Кроме того, CIM определяет набор таблиц (например, таблицу [сетевых сеансов](https://ossemproject.com/cdm/tables/network_session.html) ), использующих соответствующие атрибуты из этих сущностей, что позволяет легко и предсказуемо корреляцию. Обратите внимание, что сущности могут быть вложенными (например, исходная сущность может содержать файловую сущность, у которой будет атрибут Name).

Дополнительные сведения о структуре сущностей ОССЕМ см. в [официальной справочной документации по оссем](https://ossemproject.com/cdm/guidelines/entity_structure.html).

### <a name="how-the-normalized-schemas-are-implemented-in-azure-sentinel"></a>Как нормализованные схемы реализуются в Azure Sentinel

В реализации ОССЕМ CIM в Azure Sentinel мы проецирован представление ОССЕМ на Log Analytics табличное представление, является ли это представление встроенным или созданным с помощью средств синтаксического анализа времени выполнения или функций, которые сопоставляют существующие данные с этим представлением. Для табличного представления мы объединяем имена сущностей ОССЕМ и имена атрибутов и сообщим их вместе с одним именем столбца. Например, исходная сущность, содержащая файловую сущность, содержащую хэш-сущность, содержащую атрибут MD5, будет реализована в следующем Log Analytics столбце: SrcFileHashMd5. (По умолчанию ОССЕМ использует *snake_case* , а Azure Sentinel и log Analytics использовать *PascalCase*. В ОССЕМ такой столбец будет src_file_hash_md5.)

В реализации Sentinel в Azure могут существовать дополнительные настраиваемые поля из-за Log Analytics требований к платформе и вариантов использования, характерных для клиентов Azure Sentinel.

### <a name="schema-reference"></a>Справочник по схеме

Дополнительные сведения см. в [справочном документе по схемам](./normalization-schema.md), а также официальной [документации по проекту оссем](https://ossemproject.com/cdm/intro.html).

Ссылка на схему также включает стандартизацию значений и формата. Исходные поля, исходные или проанализированные, могут не иметь стандартного формата или использовать стандартный список значений схемы, поэтому их необходимо преобразовать в стандартное представление схемы, чтобы быть полностью нормализованным.

## <a name="parsers"></a>Анализаторы

- [Что такое синтаксический анализ](#what-is-parsing)
- [Использование средств синтаксического анализа времени запросов](#using-query-time-parsers)

### <a name="what-is-parsing"></a>Что такое синтаксический анализ

При наличии базового набора определенных нормализованных таблиц необходимо преобразовать данные в эти таблицы (анализировать или сопоставлять). То есть вы извлечете определенные данные из своей необработанной формы в известные столбцы в нормализованной схеме. Синтаксический анализ в Azure Sentinel происходит во **время выполнения запросов** . средства синтаксического анализа создаются как log Analytics пользовательские функции (с использованием Kusto запросов языка ККЛ), которые преобразуют данные в существующих таблицах (например, CommonSecurityLog, таблицы пользовательских журналов, syslog) в нормализованную схему таблиц.

Другой тип синтаксического анализа, который пока не поддерживается в Azure Sentinel, — это **время приема** , позволяющее собирать данные непосредственно в нормализованные таблицы по мере их получения из источников данных. Анализ времени приема обеспечивает улучшенную производительность, так как модель данных запрашивается напрямую без необходимости использования функций.

### <a name="using-query-time-parsers"></a>Использование средств синтаксического анализа времени запросов

- [Установка средства синтаксического анализа](#installing-a-parser)
- [Использование средств синтаксического анализа](#using-the-parsers)
- [Настройка средств синтаксического анализа](#customizing-parsers)

#### <a name="installing-a-parser"></a>Установка средства синтаксического анализа

Доступные средства синтаксического анализа времени запросов доступны в [официальном репозитории GitHub](https://github.com/Azure/Azure-Sentinel/tree/master/Parsers/Normalized%20Schema%20-%20Networking%20(v1.0.0))Azure Sentinel. Каждое средство синтаксического анализа имеет версию, позволяющее пользователям легко использовать и отслеживать будущие обновления. Установка средства синтаксического анализа:

1. Скопируйте соответствующее содержимое средства синтаксического анализа из каждого соответствующего файла ККЛ из приведенной выше ссылки GitHub в буфер обмена.

1. На портале Sentinel Azure откройте страницу журналы и вставьте содержимое файла ККЛ на экран журналы и нажмите кнопку **сохранить**.

    :::image type="content" source="./media/normalization/install-new-parser.png" alt-text="Установка нового средства синтаксического анализа":::

1. В диалоговом окне Сохранить заполните поля следующим образом:
    1. **Имя**. рекомендуется использовать то же значение, которое использовалось в поле **псевдонима функции** (в приведенном выше примере *CheckPoint_Network_NormalizedParser*)
    
    1. **Сохранить как**: выбор **функции**

    1. **Псевдоним функции**: должен называться в следующем соглашении об именовании — *PRODUCT_Network_NormalizedParser* (в приведенном выше примере — *CheckPoint_Network_NormalizedParser*).

    1. **Категория**. можно выбрать существующую категорию или создать новую категорию (например, *нормализеднетворксессионспарсерс*).
    
        :::image type="content" source="./media/normalization/save-new-parser.png" alt-text="Сохранение средства синтаксического анализа":::

Для правильного использования средств синтаксического анализа необходимо также установить пустое средство синтаксического анализа сетевой схемы (которое создает пустое табличное представление всех полей схемы сетевых сеансов) и средство мета-синтаксического анализа сети (которое объединяет все включенные парсеры для создания единого представления данных из различных источников в схеме сети). Установка этих двух синтаксических анализаторов выполняется аналогично вышеупомянутым этапам.

При сохранении функции запроса может потребоваться закрыть обозреватель запросов и повторно открыть его, чтобы новая функция была отражена.

#### <a name="using-the-parsers"></a>Использование средств синтаксического анализа

После включения можно использовать мета-синтаксический анализатор для запроса унифицированного представления для всех включенных в настоящее время средств синтаксического анализа. Для этого перейдите на страницу "журналы Sentinel Azure" и запросите мета-анализатор:

:::image type="content" source="./media/normalization/query-parser.png" alt-text="Запрос средства синтаксического анализа":::
 
Вы также можете получить доступ к meta-анализатору или отдельным средствам синтаксического анализа с помощью обозревателя запросов на странице "журналы", щелкнув "Обозреватель запросов":

:::image type="content" source="./media/normalization/query-explorer.png" alt-text="Обозреватель запросов":::

На панели справа разверните раздел "сохраненные запросы" и найдите папку "Нормализеднетворкпарсерс" (или имя категории, выбранное при создании анализаторов):

:::image type="content" source="./media/normalization/find-parser.png" alt-text="Поиск средства синтаксического анализа":::

Можно щелкнуть каждое отдельное средство синтаксического анализа и просмотреть базовую функцию, которую он использует, и запустить ее (или обратиться к ней непосредственно по его псевдониму, как описано выше). Обратите внимание, что некоторые средства синтаксического анализа могут хранить исходные поля параллельно с нормализованными полями для удобства. Это можно легко изменить в запросе средства синтаксического анализа.

> [!TIP]
> Вы можете использовать сохраненные функции вместо таблиц Sentinel Azure в любом запросе, включая запросы поиска и обнаружения. Дополнительные сведения см. в разделе:
>
> - [Нормализация данных в Azure Sentinel](normalization.md#parsers)
> - [Анализ текста в журналах Azure Monitor](../azure-monitor/logs/parse-text.md)
>
#### <a name="customizing-parsers"></a>Настройка средств синтаксического анализа

Вы можете повторить описанные выше шаги (найти средство синтаксического анализа в обозревателе запросов), щелкнуть соответствующее средство синтаксического анализа и просмотреть его реализацию функции.
Например, можно изменить мета-синтаксический анализатор, чтобы добавить или удалить отдельные парсеры.

:::image type="content" source="./media/normalization/customize-parser.png" alt-text="Настройка средства синтаксического анализа":::
 
После изменения функции снова нажмите кнопку "Сохранить" и используйте то же имя, псевдоним и категорию. Откроется диалоговое окно переопределения — нажмите кнопку "ОК":

:::image type="content" source="./media/normalization/are-you-sure.png" alt-text="Уверен":::

#### <a name="additional-information"></a>Дополнительные сведения

JSON, XML и CSV особенно удобны для анализа во время выполнения запроса. В Azure Sentinel есть встроенные функции анализа для JSON, XML и CSV, а также средство синтаксического анализа JSON.  Дополнительные сведения см. [в разделе Использование полей JSON в Sentinel](https://techcommunity.microsoft.com/t5/azure-sentinel/tip-easily-use-json-fields-in-sentinel/ba-p/768747) (блог) Azure. 

Дополнительные сведения о [сохраненных запросах](../azure-monitor/logs/example-queries.md) (реализация анализаторов времени запросов) см. в log Analytics.


## <a name="next-steps"></a>Дальнейшие действия

В этом документе вы узнали о схеме нормализации Azure Sentinel. Сведения о самой ссылочной схеме см. в статье [Справочник по схеме нормализации данных Sentinel в Azure](./normalization-schema.md).

* [Блог Sentinel Azure](https://aka.ms/azuresentinelblog). Записи блога, посвященные безопасности и соответствию требованиям в Azure.