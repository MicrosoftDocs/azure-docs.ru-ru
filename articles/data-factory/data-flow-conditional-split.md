---
title: Преобразование "Условное разбиение" в потоке данных для сопоставления
description: Разбиение данных на разные потоки с помощью преобразования "Условное разбиение" в потоке данных для сопоставления в Фабрике данных Azure
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/21/2020
ms.openlocfilehash: eece6f97e82f3800d4f59ac1849b34c2a1e4635b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "83800090"
---
# <a name="conditional-split-transformation-in-mapping-data-flow"></a>Преобразование "Условное разбиение" в потоке данных для сопоставления

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Преобразование "Условное разбиение" направляет строки данных в различные потоки на основе условий соответствия. Преобразование "Условное разбиение" аналогично применению структуры решения CASE в языке программирования. Преобразование вычисляет выражения, и на основе результатов направляет строку данных в указанный поток.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4wKCX]

## <a name="configuration"></a>Конфигурация

Параметр **Разбить по** определяет, передается ли строка данных в первый соответствующий поток или в каждый поток, которому она соответствует.

С помощью построителя выражений потока данных введите выражение для условия разбиения. Чтобы добавить новое условие, щелкните значок "плюс" в существующей строке. Для строк, которые не соответствуют никаким условиям, можно также добавить поток по умолчанию.

![условное разбиение](media/data-flow/conditionalsplit1.png "параметры условного разбиения")

## <a name="data-flow-script"></a>Скрипт потока данных

### <a name="syntax"></a>Синтаксис

```
<incomingStream>
    split(
        <conditionalExpression1>
        <conditionalExpression2>
        ...
        disjoint: {true | false}
    ) ~> <splitTx>@(stream1, stream2, ..., <defaultStream>)
```

### <a name="example"></a>Пример

Ниже приведен пример преобразования "Условное разбиение" с именем `SplitByYear`, которое принимает входной поток `CleanData`. Это преобразование имеет два условия разделения — `year < 1960` и `year > 1980`. `disjoint` имеет значение false, так как данные переходят в первое условие соответствия. Каждая строка, соответствующая первому условию, переходит в выходной поток `moviesBefore1960`. Все оставшиеся строки, соответствующие второму условию, переходят в выходной поток `moviesAFter1980`. Все остальные строки проходят через поток по умолчанию `AllOtherMovies`.

В интерфейсе Фабрики данных это преобразование выглядит следующим образом:

![условное разбиение](media/data-flow/conditionalsplit1.png "параметры условного разбиения")

Скрипт потока данных для этого преобразования представлен в следующем фрагменте кода:

```
CleanData
    split(
        year < 1960,
        year > 1980,
        disjoint: false
    ) ~> SplitByYear@(moviesBefore1960, moviesAfter1980, AllOtherMovies)
```

## <a name="next-steps"></a>Дальнейшие действия

В число распространенных преобразований потока данных, используемых с условным разбиением, входят [преобразование "Соединение"](data-flow-join.md), [преобразование "Уточняющий запрос"](data-flow-lookup.md) и [преобразование "Выбор"](data-flow-select.md).
