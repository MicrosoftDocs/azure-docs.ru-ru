---
title: Руководство. Установка физического устройства Azure Stack Edge Pro — распаковка, установка в стойку и подключение | Документация Майкрософт
description: Во втором руководстве по установке Azure Stack Edge Pro описано, как распаковать, установить в стойку и подключить физическое устройство.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 01/17/2020
ms.author: alkohli
ms.openlocfilehash: caf64de55c48d763b8600988e5ff2aba2c83e4f9
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106060199"
---
# <a name="tutorial-install-azure-stack-edge-pro"></a>Руководство по Установка Azure Stack Edge Pro

Это руководство содержит инструкции по установке физического устройства Azure Stack Edge Pro. Процедура установки включает распаковку устройства, установку в стойку и подключение к нему кабелей. 

Установка может занять около двух часов.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * распаковка устройства;
> * Установка устройства в стойку.
> * Подключение кабелей к устройству

## <a name="prerequisites"></a>Предварительные требования

Ниже приведены предварительные требования для установки физического устройства.

### <a name="for-the-azure-stack-edge-resource"></a>Для ресурса Azure Stack Edge

Перед тем как начать, убедитесь в следующем.

* Выполнены все инструкции из руководства по [подготовке к развертыванию Azure Stack Edge Pro](azure-stack-edge-deploy-prep.md).
    * Создан ресурс Azure Stack Edge для развертывания устройства.
    * Создан ключ активации, чтобы активировать устройство с помощью ресурса Azure Stack Edge.

 
### <a name="for-the-azure-stack-edge-pro-physical-device"></a>Для физического устройства Azure Stack Edge Pro

Перед развертыванием устройства:

- Убедитесь, что устройство надежно установлено на плоской, стабильной и ровной рабочей поверхности.
- Убедитесь, что в расположении, в котором планируется выполнить настройку, есть следующее:
    - стандартное питание переменного тока из независимого источника

        -или-
    - стоечный блок распределения питания (БРП) с источником бесперебойного питания (ИБП);
    - в стойке, куда вы планируете установить устройство, имеется один разъем высотой 1U.

### <a name="for-the-network-in-the-datacenter"></a>Сеть в центре обработки данных

Перед началом работы

- Изучите сетевые требования к развертыванию Azure Stack Edge Pro и настройте сеть центра обработки данных в соответствии с требованиями. Дополнительные сведения см. в разделе [Требования к сети Azure Stack Edge Pro](azure-stack-edge-system-requirements.md#networking-port-requirements).

- Убедитесь, что минимальная пропускная способность интернет-канала, необходимая для оптимального функционирования устройства, составляет 20 Мбит/с.


## <a name="unpack-the-device"></a>распаковка устройства;

Данное устройство поставляется в одной упаковке. Выполните следующие действия для распаковки устройства. 

1. Расположите коробку на ровной плоской поверхности.
2. Проверьте коробку и упаковочный пеноматериал на предмет наличия пробоин, порезов, повреждений от воды или других видимых повреждений. Если коробка или упаковка серьезно повреждены, не открывайте коробку. Обратитесь в службу поддержки Майкрософт, специалисты которой помогут вам в оценке состояния устройства.
3. Распакуйте коробку. После распаковки коробки, убедитесь, что у вас имеется:
    - одно устройство Azure Stack Edge Pro в отдельном корпусе;
    - два кабеля питания;
    - один комплект направляющих;
    - буклет с информацией о технике безопасности и соответствии природоохранным и нормативным требованиям.

Если вы получили не все компоненты из перечисленных в этом руководстве, обратитесь в службу технической поддержки Azure Stack Edge Pro. Следующим шагом является установка устройства в стойку.


## <a name="rack-the-device"></a>установка устройства в стойку;

Устройства должны устанавливаться в стандартную 19-дюймовую стойку. Следуйте дальнейшим инструкциям, чтобы установить устройство в стандартную 19-дюймовую стойку.

> [!IMPORTANT]
> Для правильной работы устройства Azure Stack Edge Pro должны быть установлены в стойку.


### <a name="prerequisites"></a>Предварительные требования

- Перед началом работы ознакомьтесь с инструкциями по технике безопасности и соответствии природоохранным и нормативным требованиям, изложенными в информационном буклете. Этот буклет поставляется с устройством.
- Установку направляющих начинайте с нижней части стойки.
- Для установки инструментальных направляющих:
    -  Понадобится восемь винтов 10-32, 12-24, M5 или M6. Диаметр головки винтов не должен превышать 10 мм.
    -  Вам потребуется отвертка с плоским наконечником.

### <a name="identify-the-rail-kit-contents"></a>Содержимое комплекта направляющих

Проверьте содержимое комплекта направляющих:
1. две рельсовые направляющие A7 Dell ReadyRails II;
2. две стяжки с текстильной застежкой.

    ![Содержимое комплекта направляющих](./media/azure-stack-edge-deploy-install/identify-rail-kit-contents.png)

### <a name="install-and-remove-tool-less-rails-square-hole-or-round-hole-racks"></a>Монтаж и демонтаж безинструментальных направляющих (в стойку с квадратными или круглыми отверстиями)

> [!TIP]
> Этот вариант безинструментальный, так как он не требует использования инструментов для установки и снятия направляющих в квадратных или круглых отверстиях без резьбы в стойках.

1. Разместите правую и левую направляющие с надписью **FRONT** вовнутрь стойки и совместите штифты на ближних концах направляющих с требуемыми отверстиями на передних монтажных фланцах стойки.
2. Совместите штифты на дальних концах направляющих с требуемыми отверстиями на задних монтажных фланцах.
3. Зафиксируйте дальние концы направляющих так, чтобы их штифты полностью вошли в отверстия и защелки встали на место. Повторите эти действия с ближними концами направляющих, чтобы сориентировать и зафиксировать их на передних монтажных фланцах стойки.
4. Чтобы снять направляющую, отведите рычаг защелок, расположенный посредине конца направляющей, и высвободите направляющую.

    ![Монтаж и демонтаж безинструментальных направляющих](./media/azure-stack-edge-deploy-install/installing-removing-tool-less-rails.png)

### <a name="install-and-remove-tooled-rails-threaded-hole-racks"></a>Монтаж и демонтаж инструментальных направляющих (стойки с резьбовыми отверстиями)

> [!TIP]
> Этот вариант инструментальный, так как он требует использования инструментов (_отвертки с плоским наконечником_) для установки и снятия направляющих в круглых отверстиях с резьбой в стойках.

1. Удалите транспортировочные винты из передних и задних монтажных скоб с помощью отвертки с плоским наконечником.
2. Потяните и поверните направляющую, чтобы высвободить ее из монтажной скобы.
3. Закрепите левую и правую направляющую на передних монтажных фланцах стойки с помощью двух пар винтов.
4. Задвиньте правую и левую задние монтажные скобы вперед до их упора во фланцы задней вертикальной стойки и закрепите их с помощью двух пар винтов.

    ![Монтаж и демонтаж инструментальных направляющих](./media/azure-stack-edge-deploy-install/installing-removing-tooled-rails.png)

### <a name="install-the-system-in-a-rack"></a>Установка системы в стойку

1. Выдвиньте внутренние направляющие из стойки до их фиксации в конечном положении.
2. Установите устройство задней частью в направляющие, поместив его задние монтажные шпильки в задние монтажные пазы направляющих. Опустите устройство так, чтобы все его монтажные шпильки оказались в монтажных пазах.
3. Подайте устройство вперед, чтобы стопорные рычаги встали на место.
4. Нажмите кнопки разблокировки перемещения на обеих направляющих и задвиньте устройство в стойку.

    ![Установка устройства в стойку](./media/azure-stack-edge-deploy-install/installing-system-rack.png)

### <a name="remove-the-system-from-the-rack"></a>Извлечение устройства из стойки

1. Найдите стопорные рычаги, расположенные сбоку на выдвижных частях направляющих.
2. Разблокируйте каждый рычаг, поворачивая его вверх.
3. Крепко удерживая устройство, подайте его наружу из стойки, чтобы монтажные шпильки устройства сместились в монтажных пазах. Подайте устройство вверх, чтобы извлечь его из направляющих, после чего поставьте его на ровную поверхность.

    ![Извлечение устройства из направляющих](./media/azure-stack-edge-deploy-install/removing-system-rack.png)

### <a name="engage-and-release-the-slam-latch"></a>Фиксация и высвобождение защелок

> [!NOTE]
> Если устройство не оснащено защелками, зафиксируйте его в стойке винтами, как описано на шаге 3 этой процедуры.

1. Защелки расположены спереди по бокам устройства.
2. Если задвинуть устройство в стойку, защелки фиксируются автоматически. Чтобы высвободить защелки, их нужно потянуть вверх.
3. Чтобы защитить устройство при транспортировке в стойке или в нестабильном окружении, его можно зафиксировать в стойке винтами, которые расположены под каждой защелкой, с помощью отвертки Phillips №2.

    ![Фиксация и высвобождение защелок](./media/azure-stack-edge-deploy-install/engaging-releasing-slam-latch.png)


## <a name="cable-the-device"></a>Подключение кабелей к устройству

Проложите кабели, а затем подключите устройство. Ниже описано, как подсоединить кабель питания и сетевой кабель к устройству Azure Stack Edge Pro.

Перед подключением кабелей к устройству убедитесь, что у вас есть следующее:

- распакованное и установленное в стойку физическое устройство Azure Stack Edge Pro;
- два кабеля питания;
- как минимум один сетевой кабель 1 GbE RJ-45 для подключения к интерфейсу управления (на устройстве есть два сетевых интерфейса 1 GbE: для управления и для данных);
- один трансивер SFP 25 GbE + медный кабель для настройки каждого сетевого интерфейса данных (хотя бы один сетевой интерфейс данных — PORT 2, PORT 3, PORT 4, PORT 5 или PORT 6 — должен быть подключен к Интернету (через подключение к Azure));  
- доступ к двум блокам распределения питания (БРП) (рекомендуется).

> [!NOTE]
> - Если вы подключаете только один сетевой интерфейс данных, для отправки данных в Azure рекомендуем использовать сетевой интерфейс 25/10 GbE, например PORT 3, PORT 4, PORT 5 или PORT 6. 
> - Для достижения оптимальной производительности и обработки больших объемов данных рекомендуем подключить все порты данных.
> - Устройство Azure Stack Edge Pro необходимо подключить к сети центра обработки данных таким образом, чтобы оно могло принимать данные с исходных серверов данных.

На устройстве Azure Stack Edge Pro:

- На передней панели находятся дисковые накопители и кнопка питания.

    - На панели расположены 10 отсеков для накопителей.
    - В отсек 0 установлен диск SATA емкостью 240 ГБ с операционной системой. Отсек 1 свободен, а в отсеках 2–9 находятся твердотельные накопители NVMe для хранения данных.
- На задней панели находятся резервные блоки питания (PSU).
- Там же расположены 6 сетевых интерфейсов:

    - два интерфейса на 1 Гбит/с;
    - четыре интерфейса на 25 Гбит/с, которые также можно использовать как интерфейсы на 10 Гбит/с.
    - Контроллер управления основной платой (BMC)

- На задней панели установлены две сетевые карты с общим количеством портов, равным шести:

    - QLogic FastLinQ 41264;
    - QLogic FastLinQ 41262.

Полный список поддерживаемых кабелей, коммутаторов и приемопередатчиков для этих сетевых карт см. в [таблице взаимодействия Cavium FastlinQ серии 41000](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).
 
Выполните следующие действия, чтобы подключить к устройству питание и сеть.

1. Идентифицируйте порты на задней панели устройства.

    ![Задняя панель подключенного устройства](./media/azure-stack-edge-deploy-install/backplane-cabled.png)

2. Изучите расположение отсеков для накопителей и кнопки питания на передней панели устройства.

    ![Передняя панель устройства](./media/azure-stack-edge-deploy-install/device-front-plane-labeled-1.png)

3. Подключите кабели питания ко всем резервным блокам питания в корпусе. Чтобы обеспечить высокую доступность, установите оба резервных блока питания и подключите их к разным источникам питания.
4. Присоедините кабели питания ко всем распределительным блокам стойки. Убедитесь, что для двух резервных блоков питания используются разные источники питания.
5. Нажмите кнопку питания, чтобы включить устройство.
6. Подключите сетевой интерфейс 1 GbE PORT 1 к компьютеру, используемому для настройки физического устройства. PORT 1 — это выделенный интерфейс управления.
7. Подключите к сети ЦОД или Интернету порт PORT 2, PORT 3, PORT 4, PORT 5 или PORT 6 (или несколько из них).

    - Для порта PORT 2 используйте сетевой кабель RJ-45.
    - Для сетевых интерфейсов 10/25 GbE используйте медные кабели SFP+.

## <a name="next-steps"></a>Дальнейшие действия

В этом учебнике были рассмотрены следующие темы относительно Azure Stack Edge Pro:

> [!div class="checklist"]
> * распаковка устройства;
> * установка устройства в стойку;
> * Подключение кабелей к устройству

Перейдите к следующему руководству, чтобы научиться подключать, настраивать и активировать устройство.

> [!div class="nextstepaction"]
> [Подключение и настройка Azure Stack Edge Pro](./azure-stack-edge-deploy-connect-setup-activate.md)
