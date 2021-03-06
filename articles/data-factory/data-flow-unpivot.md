---
title: Преобразование “Отменить свертывание” в потоке данных для сопоставления
description: Преобразование “Отменить свертывание” в потоке данных для сопоставления Фабрики данных Azure
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/14/2020
ms.openlocfilehash: ef861cdf394716a70d85e43ce9c60f46af2cc2e4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "93040203"
---
# <a name="unpivot-transformation-in-mapping-data-flow"></a>Преобразование “Отменить свертывание” в потоке данных для сопоставления

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Преобразование “Отменить свертывание” в потоке данных для сопоставления ADF позволяет превратить ненормализованный набор данных в более нормализованную версию. При этом одна запись со значениями из нескольких столбцов развертывается в несколько записей с одинаковыми значениями в одном столбце.

![Снимок экрана: выбранное в меню преобразование “Отменить свертывание”.](media/data-flow/unpivot1.png "Параметры отмены свертывания 1")

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4B1RR]

## <a name="ungroup-by"></a>Разгруппировать

![Снимок экрана: параметры отмены свертывания с выбранной вкладкой “Разгруппировать по”.](media/data-flow/unpivot5.png "Параметры отмены свертывания 2")

Сначала задайте столбцы, по которым будет производиться разгруппирование для развертывания агрегированного значения. Укажите один или несколько столбцов для разгруппировки со знаком "+" рядом со списком столбцов.

## <a name="unpivot-key"></a>Ключ отмены свертывания

![Снимок экрана: параметры отмены свертывания с выбранной вкладкой “Ключ отмены свертывания”.](media/data-flow/unpivot6.png "Параметры отмены свертывания 3")

Ключом отмены свертывания называется столбец, по которому ADF будет сводить данные в строку. По умолчанию каждое уникальное значение в наборе данных для этого поля будет сводиться в строку. При желании можно указать дополнительные значения из набора данных, которые нужно свести к значениям строки.

## <a name="unpivoted-columns"></a>Несвернутые столбцы

![Снимок экрана: параметры отмены свертывания с выбранной вкладкой “Просмотр данных”.](media/data-flow//unpivot7.png "Параметры отмены свертывания 4")

Наконец, выберите имя столбца для хранения значений несвернутых столбцов, преобразованных в строки.

(Дополнительно) Строки со значениями NULL можно удалить.

Например, SumCost — это имя столбца, выбранного в приведенном выше примере.

![На изображении показаны столбцы PO (Заказ на закупку), Vendor (Поставщик) и Fruit (Фрукт) до и после преобразования “Отмена свертывания”. В качестве ключа отмены свертывания используется столбец Fruit.](media/data-flow/unpivot3.png)

Если для параметра “Расположение столбцов” выбрано значение “Обычное”, все новые несвернутые столбцы с одинаковыми значениями будут сгруппированы вместе. Если для параметра “Расположение столбцов” выбрано значение “Боковое”, будут сгруппированы вместе новые несвернутые столбцы, сформированные на основе существующего столбца.

![Снимок экрана: результат преобразования.](media/data-flow//unpivot7.png "Параметры отмены свертывания 5")

В окончательном наборе результатов для несвернутых данных отображаются итоговые значения столбца, которые теперь не объединены в значения отдельных строк.

## <a name="next-steps"></a>Дальнейшие действия

Используйте [преобразование “Сведение”](data-flow-pivot.md) чтобы свести строки в столбцы.
