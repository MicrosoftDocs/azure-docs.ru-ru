---
title: Что такое онтология?
titleSuffix: Azure Digital Twins
description: Узнайте о ДТДЛ отрасли онтологиес для моделирования в определенном домене
author: baanders
ms.author: baanders
ms.date: 2/12/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 3393856b25040cff603ea2ef51e8adbcba78dc26
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102034699"
---
# <a name="what-is-an-ontology"></a>Что такое онтология? 

Словарь решения Azure Digital двойников определяется с помощью [моделей](concepts-models.md), описывающих типы сущностей, существующих в вашей среде.

Иногда, если решение привязано к определенной отрасли, оно может быть простым и эффективным для начала работы с набором моделей, которые уже существуют, вместо создания собственного набора моделей с нуля. Эти существовавшие ранее наборы моделей называются **онтологиес**. 

Как правило, онтологи — это набор моделей для данного домена, таких как структура здания, система IoT, смарт-город, сетка энергии, веб-содержимое и т. д. Онтологиес часто используются в качестве схем для диаграмм знаний, так как они могут включать:
* Упорядочивание программных компонентов, документация, библиотеки запросов и т. д.
* Сокращенные инвестиции в концептуальное моделирование и разработка систем
* Упрощение взаимодействия с данными на семантическом уровне
* Рекомендуемое повторное использование, а не начинать с нуля или "переносить колесико"

В этой статье объясняется, почему и как использовать онтологиес для моделей цифровых двойников Azure, а также о том, какие онтологиес и средства для них доступны сегодня.

## <a name="using-ontologies-for-azure-digital-twins"></a>Использование онтологиес для Azure Digital двойников

Онтологиес предоставляет отличную отправную точку для цифровых решений двойника. Они охватывают набор моделей для конкретных доменов и связи между сущностями для проектирования, создания и анализа цифрового двойника графа. Онтологиес позволяют разработчикам решений начать цифровое решение двойников с проверенной отправной точки и сосредоточиться на решении бизнес-задач. Онтологиес, предоставляемые корпорацией Майкрософт, также спроектированы так, чтобы быть легко расширяемыми, чтобы их можно было настроить для решения. 

Кроме того, использование этих онтологиес в решениях позволяет настроить их для более тесной интеграции между разными партнерами и поставщиками, так как онтологиес может предоставить общий словарь для решений.

Поскольку модели в Azure Digital двойников представлены в формате [Digital двойников Definition Language (дтдл)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md), онтологиес для использования с Azure Digital двойников также пишутся в дтдл. 

## <a name="strategies-for-integrating-ontologies"></a>Стратегии интеграции онтологиес

Существует три возможные стратегии интеграции стандартных отраслевых онтологиес с ДТДЛ. Вы можете выбрать наиболее подходящий вариант в зависимости от ваших потребностей:

| Стратегия | Описание | Ресурсы |
| --- | --- | --- |
| **Внедрение** | Вы можете запустить решение с помощью ДТДЛ онтологи с открытым кодом, созданного на основе широко принятых отраслевых стандартов. Можно либо использовать эти наборы моделей, либо расширить их с помощью собственных дополнений для настроенного решения. | [*Основные понятия: &nbsp; внедрение &nbsp; отраслевого &nbsp; стандарта онтологиес*](concepts-ontologies-adopt.md)<br><br>[*Основные понятия: &nbsp; расширение &nbsp; онтологиес*](concepts-ontologies-extend.md) |
| **Преобразовать** | Если у вас уже есть модели, представленные в другом стандартном формате, их можно преобразовать в ДТДЛ, чтобы использовать их с Azure Digital двойников. | [*Основные понятия: &nbsp; Преобразование &nbsp; онтологиес*](concepts-ontologies-convert.md)<br><br>[*Основные понятия: &nbsp; расширение &nbsp; онтологиес*](concepts-ontologies-extend.md) |
| **Автор** | Вы всегда можете разрабатывать собственные пользовательские модели ДТДЛ с нуля, используя любые применимые отраслевые стандарты в качестве идей. | [*Основные понятия: модели ДТДЛ*](concepts-models.md) |

### <a name="using-ontology-strategies-in-a-model-development-path"></a>Использование стратегий онтологи в пути разработки модели

Независимо от стратегии, выбранной для интеграции онтологи в Azure Digital двойников, вы можете выполнить инструкции по созданию и передаче онтологи в качестве моделей ДТДЛ.

1. Начните с изучения и понимания [дтдл моделирования в Azure Digital двойников](concepts-models.md).
1. Продолжайте работу с выбранной стратегией интеграции онтологи из приведенных выше: [**внедрение**](concepts-ontologies-adopt.md), [**Преобразование**](concepts-ontologies-convert.md)или [**Создание**](concepts-models.md) моделей на основе онтологи.
    1. При необходимости [Расширьте](concepts-ontologies-extend.md) свой онтологи, чтобы настроить его в своих целях.
1. [Проверьте модели](how-to-parse-models.md) , чтобы убедиться, что они работают с дтдл документами.
1. Отправьте готовые модели в Azure Digital двойников с помощью [API](how-to-manage-model.md#upload-models) или примера, подобного [отправке Azure Digital двойников Model](https://github.com/Azure/opendigitaltwins-building-tools/tree/master/ModelUploader).

После этого вы сможете использовать модели в своем экземпляре Azure Digital двойников. 

Вы можете визуализировать их с помощью примеров, например [обозревателя цифровых двойников Azure](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/) или [визуализатора цифровых двойников моделей Azure](https://github.com/Azure/opendigitaltwins-building-tools/tree/master/AdtModelVisualizer), или перейти к их использованию для создания [цифрового двойников](concepts-twins-graph.md).

## <a name="next-steps"></a>Следующие шаги

Узнайте больше о стратегиях внедрения, преобразования и разработки онтологиес:
* [*Основные понятия: внедрение отраслевого стандарта онтологиес*](concepts-ontologies-adopt.md)
* [*Основные понятия: преобразование онтологиес*](concepts-ontologies-convert.md)
* [*Руководство. Управление моделями ДТДЛ*](how-to-manage-model.md)

Или Узнайте, как модели используются для создания цифровых двойников: [*Основные понятия: Digital двойников и Graph двойника*](concepts-twins-graph.md).