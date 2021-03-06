---
title: Часто задаваемые вопросы об размерах виртуальных машин Azure без локального временного диска
description: В этой статье приведены ответы на часто задаваемые вопросы о Microsoft Azure размерах виртуальных машин без локального временного диска.
author: brbell
ms.service: virtual-machines
ms.topic: conceptual
ms.subservice: sizes
ms.author: brbell
ms.reviewer: mimckitt
ms.date: 06/15/2020
ms.openlocfilehash: bd4dcbdc7ab13d18ef7f2d7102c56d1bd8d8758d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104582108"
---
# <a name="azure-vm-sizes-with-no-local-temporary-disk"></a>Размеры виртуальных машин Azure без локального временного диска 
В этой статье приведены ответы на часто задаваемые вопросы о размерах виртуальных машин Azure, у которых нет локального временного диска (т. е. локальный временный диск отсутствует). Дополнительные сведения об этих размерах виртуальных машин см. в статьях [спецификации для dv4 и Dsv4 (общего назначения рабочих нагрузок)](dv4-dsv4-series.md) или [спецификации для Ev4 и Esv4 (рабочих нагрузок, оптимизированных для памяти)](ev4-esv4-series.md).

## <a name="what-does-no-local-temp-disk-mean"></a>Что означает отсутствие локального временного диска? 
Обычно у нас есть размеры виртуальных машин (например, Standard_D2s_v3, Standard_E48_v3), которые включают небольшой локальный диск (т. е. диск D:). Теперь с новыми размерами виртуальных машин этот маленький локальный диск больше не существует; Тем не менее можно присоединить HDD (цен. категория "Стандартный"), SSD (цен. категория "Премиум") или SSD (цен. категория "Ультра").

## <a name="what-if-i-still-want-a-local-temp-disk"></a>Что делать, если требуется локальный временный диск?
Если для рабочей нагрузки требуется локальный временный диск, у нас также есть новые размеры виртуальных машин [Ddv4 и Ddsv4](ddv4-ddsv4-series.md) или [Edv4 и Edsv4](edv4-edsv4-series.md) . Эти размеры предлагают 50% больших временных дисков по сравнению с предыдущими размерами v3.

> [!NOTE]
> Локальный временный диск не является постоянным; чтобы обеспечить постоянное сохранение данных, используйте параметры HDD (цен. категория "Стандартный"), SSD (цен. категория "Премиум") или SSD (цен. категория "Ультра"). 

## <a name="what-are-the-differences-between-these-new-vm-sizes-and-the-general-purpose-dv3dsv3-or-the-memory-optimized-ev3esv3-vm-sizes-that-i-am-used-to"></a>Каковы различия между этими новыми размерами виртуальных машин и общего назначения Dv3/Dsv3 или размерами виртуальных машин, оптимизированных для памяти Ev3/Esv3? 
| v3 семейства виртуальных машин с локальным временным диском   | Новые семейства v4 с локальным временным диском | Новые семейства v4 без локального временного диска |
|---|---|---|
| Dv3   | Ddv4 | Dv4 |
| Dsv3 | Ddsv4  | Dsv4 |
| Ev3   | Edv4  | Ev4 |
| Esv3 | Edsv4 |    Esv4 | 

## <a name="can-i-resize-a-vm-size-that-has-a-local-temp-disk-to-a-vm-size-with-no-local-temp-disk"></a>Можно ли изменить размер виртуальной машины с локальным временным диском на размер виртуальной машины без локального временного диска?  
Нет. Для изменения размера разрешены только следующие сочетания: 

1. Виртуальная машина (с локальным временным диском) — > виртуальная машина (с локальным временным диском); перетаскивани 
2. Виртуальная машина (без локального временного диска) — > виртуальной машине (без локального временного диска). 

Если вам интересно решить эту проблемы, см. следующий вопрос.

> [!NOTE]
> Если образ зависит от диска ресурсов, а файл подкачки или файл подкачки существует на локальном временном диске, образы без диска не будут работать — вместо этого используйте альтернативу "with Disk". 

## <a name="how-do-i-migrate-from-a-vm-size-with-local-temp-disk-to-a-vm-size-with-no-local-temp-disk"></a>Разделы справки выполнить миграцию с размера виртуальной машины с локальным временным диском на размер виртуальной машины без локального временного диска?  
Чтобы выполнить миграцию, выполните следующие действия. 

1. Подключитесь к виртуальной машине с локальным временным диском (например, диск D:) в качестве локального администратора.
2. Следуйте указаниям в разделе "временное перемещение pagefile.sys на диск C" раздела [Использование диска D: в качестве диска данных на виртуальной машине Windows](./windows/change-drive-letter.md) для перемещения файла подкачки с локального временного диска (D: диск) на диск C:.

   > [!NOTE]
   > Следуйте указаниям в разделе "временное перемещение pagefile.sys на диск C" раздела Использование диска D: в качестве диска данных на виртуальной машине Windows для перемещения файла подкачки с локального временного диска (D: диск) на диск C:. **Отклонение от описанных действий приведет к появлению сообщения об ошибке "не удалось изменить размер виртуальной машины, так как переход с диска ресурса на диск виртуальной машины, не относящийся к ресурсу, и наоборот запрещен.**

3. Создайте моментальный снимок виртуальной машины, выполнив действия, описанные в разделе [Создание моментального снимка с помощью портала или Azure CLI](./linux/snapshot-copy-managed-disk.md). 
4. С помощью моментального снимка создайте новую виртуальную машину без диска (например, dv4, Dsv4, Ev4, Esv4), выполнив действия, описанные в разделе [Создание виртуальной машины из моментального снимка с помощью интерфейса командной строки](/previous-versions/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot). 

## <a name="do-these-vm-sizes-support-both-linux-and-windows-operating-systems-os"></a>Поддерживают ли эти размеры виртуальных машин операционные системы Linux и Windows (ОС)?
Да.

## <a name="will-this-break-my-custom-scripts-custom-images-or-os-images-that-have-scratch-files-or-page-files-on-a-local-temp-disk"></a>Будет ли это разбивать пользовательские сценарии, пользовательские образы или образы ОС, имеющие файлы временных файлов или файлы страниц на локальном временном диске?
Если настраиваемый образ ОС указывает на локальный временный диск, образ может работать неправильно с этим размером без диска.

## <a name="have-questions-or-feedback"></a>Есть вопросы или предложения?
Заполните [форму обратной связи]( https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRUMzdCQkw0OVVRTldFUUtXSTlLQVBPUkVHSy4u). 

## <a name="next-steps"></a>Дальнейшие действия 
В этом документе вы узнали больше о наиболее часто встречающихся вопросах, связанных с виртуальными машинами Azure без локального временного диска. Дополнительные сведения об этих размерах виртуальных машин см. в следующих статьях:

- [Спецификации для серии dv4 и Dsv4 (общего назначенияная Рабочая нагрузка)](dv4-dsv4-series.md)
- [Спецификации для серии Ev4 и Esv4 (Рабочая нагрузка, оптимизированная для памяти)](ev4-esv4-series.md)