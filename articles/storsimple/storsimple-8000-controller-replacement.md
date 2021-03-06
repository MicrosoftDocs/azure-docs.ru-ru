---
title: Замена контроллера для устройства StorSimple серии 8000 | Документация Майкрософт
description: Здесь объясняется, как правильно удалить и заменить один или оба модуля контроллера на устройстве StorSimple серии 8000.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 9d8b75c48da2bb13d843258ead378d3e849da951
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "85514083"
---
# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Замена модуля контроллера на устройстве StorSimple
## <a name="overview"></a>Обзор
В этом учебнике объясняется, как снять и заменить один или оба модуля контроллера на устройстве StorSimple. В нем также обсуждается базовая логика сценариев замены одного или двух контроллеров.

> [!NOTE]
> Перед выполнением замены контроллера рекомендуется обновить встроенное ПО контроллера до последней версии.
> 
> Для предотвращения повреждения устройства StorSimple не извлекайте контроллер, пока показания индикаторов не будут соответствовать одной из следующих схем:
> 
> * Все индикаторы ПОГАСЛИ.
> * Индикатор 3, ![зеленый значок галочки](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png) и ![красный значок перекрестия](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) мигают, а индикаторы 0 и 7 **горят**.


В следующей таблице показаны поддерживаемые сценарии замены контроллера.

| Случай | Сценарий замены | Используемая процедура |
|:--- |:--- |:--- |
| 1 |Один контроллер находится в состоянии сбоя, другой контроллер исправен и работает. |[Замена одного контроллера](#replace-a-single-controller), которая описывает [логику замены одного контроллера](#single-controller-replacement-logic), а также [этапы такой замены](#single-controller-replacement-steps). |
| 2 |Оба контроллера неисправны и требуют замены. Корпус, диски и корпуса дисков исправны. |[Замена двух контроллеров](#replace-both-controllers), которая описывает [логику замены двух контроллеров](#dual-controller-replacement-logic), а также [этапы такой замены](#dual-controller-replacement-steps). |
| 3 |Контроллеры из одного устройства или из различных устройств меняются местами. Корпус, диски и корпуса дисков исправны. |Появится предупреждение о несоответствии слота. |
| 4 |Один контроллер отсутствует, а во втором произошел сбой. |[Замена двух контроллеров](#replace-both-controllers), которая описывает [логику замены двух контроллеров](#dual-controller-replacement-logic), а также [этапы такой замены](#dual-controller-replacement-steps). |
| 5 |Один или оба контроллера вышли из строя. Невозможно получить доступ к устройству через последовательную консоль или удаленное взаимодействие Windows PowerShell. |[Обратитесь в службу поддержки Microsoft](storsimple-8000-contact-microsoft-support.md) для замены контроллеров вручную. |
| 6 |Используются контроллеры с разными версиями сборки:<ul><li>Используются контроллеры с разными версиями ПО.</li><li>Используются контроллеры с разными версиями встроенного ПО.</li></ul> |Если используются контроллеры с разными версиями ПО, логика замены обнаруживает это и обновляет версию ПО на новом контроллере.<br><br>Если используются контроллеры с разными версиями встроенного ПО и более старую версию **невозможно** обновить автоматически, на портале Azure появится предупреждение. Следует проверить наличие обновлений и установить обновления встроенного ПО.</br></br>Если используются контроллеры с разными версиями встроенного ПО и его старую версию невозможно обновить автоматически, логика замены контроллеров обнаружит это, и после запуска контроллера встроенное ПО обновится автоматически. |

Если в модуле контроллера произошел сбой, его необходимо удалить. Сбой может произойти в одном или обоих модулях контроллеров, что приведет к замене одного контроллера или замене двух контроллеров. Чтобы узнать о процедурах замены и их логике, см. такие разделы:

* [Замена одного контроллера](#replace-a-single-controller)
* [Замена двух контроллеров](#replace-both-controllers)
* [Снятие контроллера](#remove-a-controller)
* [Установка контроллера](#insert-a-controller)
* [Определение активного контроллера устройства](#identify-the-active-controller-on-your-device)

> [!IMPORTANT]
> Перед снятием и заменой контроллера ознакомьтесь со сведениями о безопасности в статье [Замена компонентов оборудования StorSimple](storsimple-8000-hardware-component-replacement.md).
> 
> 

## <a name="replace-a-single-controller"></a>Замена одного контроллера
Если один из двух контроллеров на устройстве Microsoft Azure StorSimple вышел из строя, неисправен или отсутствует, необходимо заменить один контроллер.

### <a name="single-controller-replacement-logic"></a>Логика замены одного контроллера
При замене одного контроллера сначала необходимо снять неисправный контроллер. (Оставшийся контроллер в устройстве является активным контроллером.) При вставке контроллера замены выполняются следующие действия.

1. новый контроллер немедленно начинает взаимодействовать с устройством StorSimple;
2. на новый контроллер копируется моментальный снимок виртуального жесткого диска активного контроллера;
3. моментальный снимок изменяется так, чтобы при запуске нового контроллера с этим виртуальным жестким диском этот контроллер находился в режиме ожидания.
4. После внесения изменений новый контроллер будет запущен в режиме ожидания.
5. При запуске обоих контроллеров кластер подключается к сети.

### <a name="single-controller-replacement-steps"></a>Действия для замены одного контроллера
При сбое одного из контроллеров устройства Microsoft Azure StorSimple выполните следующие действия. (Другой контроллер должен быть включен и запущен. Если оба контроллера находятся в состоянии сбоя или не работают, перейдите к разделу [Действия для замены двух контроллеров](#dual-controller-replacement-steps).)

> [!NOTE]
> Для перезапуска контроллера и полного восстановления после замены одного контроллера может потребоваться 30–45 минут. Общее время выполнения всей процедуры, включая подключение кабелей, составляет приблизительно 2 часа.


#### <a name="to-remove-a-single-failed-controller-module"></a>Для снятия одного неисправного модуля контроллера
1. На портале Azure в службе диспетчера устройств StorSimple щелкните **Устройства**, затем щелкните имя устройства, которое требуется отслеживать.
2. Последовательно выберите элементы **Мониторинг > Работоспособность оборудования**. Состояние контроллера 0 или 1 должно отображаться красным цветом, который указывает на сбой.
   
   > [!NOTE]
   > Неисправный контроллер при замене одного контроллера всегда находится в режиме ожидания.
   
3. Для поиска неисправного модуля контроллера воспользуйтесь рис. 1 и следующей таблицей.
   
    ![Задняя панель модулей основного корпуса устройства](./media/storsimple-controller-replacement/IC740994.png)
   
    **Рис. 1.** Задняя поверхность устройства StorSimple
   
   | Метка | Описание |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Контроллер 0 |
   | 4 |Контроллер 1 |
4. Отключите все подключенные сетевые кабели из портов данных неисправного контроллера. При использовании модели 8600 также отключите кабели SAS, которые подключают контроллер к контроллеру EBOD.
5. Чтобы демонтировать неисправный контроллер, следуйте указаниям из раздела [Снятие контроллера](#remove-a-controller) .
6. Установите новый контроллер в тот же слот, из которого был удален неисправный контроллер. При этом запускается логика замены одного контроллера. Дополнительные сведения см. в разделе [Логика замены одного контроллера](#single-controller-replacement-logic).
7. Пока логика замены одного контроллера работает в фоновом режиме, подключите кабели. Внимательно подключите все кабели точно так же, как они были подключены до замены.
8. После перезапуска контроллера проверьте **состояние контроллера** и **состояние кластера** на портале Azure и убедитесь, что контроллер вернулся в работоспособное состояние и находится в режиме ожидания.

> [!NOTE]
> При отслеживании устройства через последовательную консоль можно заметить несколько перезапусков при восстановлении контроллера после процедуры замены. При появлении меню последовательной консоли процедура замены считается завершенной. Если меню не отображается в течение двух часов после начала замены контроллера, [обратитесь в службу поддержки Майкрософт](storsimple-8000-contact-microsoft-support.md).
>
> Начиная с обновления 4, в интерфейсе Windows PowerShell устройства можно также использовать командлет `Get-HCSControllerReplacementStatus`, чтобы наблюдать за состоянием процесса замены контроллера.
> 

## <a name="replace-both-controllers"></a>Замена двух контроллеров
Если оба контроллера на устройстве Microsoft Azure StorSimple вышли из строя, неисправны или отсутствуют, необходимо заменить оба контроллера. 

### <a name="dual-controller-replacement-logic"></a>Логика замены двух контроллеров
При замене двух контроллеров сначала снимите оба контроллера и затем установите новые. При установке двух новых контроллеров выполняются следующие действия:

1. Новый контроллер в слоте 0 проверяет следующее:
   
   1. Использует ли он текущие версии ПО и встроенного ПО?
   2. Является ли он частью кластера?
   3. Работает ли одноранговый контроллер и является ли он частью кластера?
      
      Если ни одно из этих условий не выполнено, контроллер ищет последнюю ежедневную резервную копию (находится в **nonDOMstorage** на диске S). Контроллер копирует последний моментальный снимок виртуального жесткого диска из резервной копии.
2. Контроллер в слоте 0 формирует собственный образ из моментального снимка.
3. Тем временем контроллер в слоте 1 ожидает, пока контроллер 0 не завершит формирование образа и не запустится.
4. После запуска контроллера 0 контроллер 1 обнаруживает кластер, созданный контроллером 0, при этом срабатывает логика замены одного контроллера. Дополнительные сведения см. в разделе [Логика замены одного контроллера](#single-controller-replacement-logic).
5. После этого запускаются оба контроллера и кластер подключается к сети.

> [!IMPORTANT]
> После замены двух контроллеров и настройки устройства StorSimple обязательно выполните резервное копирование устройства вручную. Ежедневное резервное копирование конфигурации устройства будет запущено только по истечении 24 часов. Обратитесь [в службу поддержки Майкрософт](storsimple-8000-contact-microsoft-support.md) , чтобы создать резервную копию устройства вручную.


### <a name="dual-controller-replacement-steps"></a>Действия для замены двух контроллеров
При выходе из строя обоих контроллеров устройства Microsoft Azure StorSimple необходимо выполнить описанные ниже действия. Выход из строя обоих контроллеров может произойти в центре обработки данных, когда система охлаждения прекращает работу, в результате чего на короткий период времени выходят из строя оба контроллера. В зависимости от того, включено ли устройство StorSimple, и от того, используется ли модель 8600 или 8100, необходимо выполнить различный набор действий.

> [!IMPORTANT]
> Для перезапуска контроллера и полного восстановления после замены двух контроллеров может потребоваться от 45 минут до 1 часа. Общее время выполнения всей процедуры, включая подключение кабелей, составляет приблизительно 2,5 часа.

#### <a name="to-replace-both-controller-modules"></a>Для замены двух модулей контроллеров
1. Если устройство выключено, пропустите этот шаг и переходите к следующему шагу. Если устройство включено, выключите его.
   
   1. При использовании модели 8600 сначала отключите основной корпус, затем корпус EBOD.
   2. Дождитесь полного выключения устройства. Все индикаторы на задней панели устройства погаснут.
2. Отключите все сетевые кабели, подключенные к портам данных. При использовании модели 8600 также отключите кабели SAS, которые подключают основной корпус к корпусу EBOD.
3. Извлеките оба контроллера из устройства StorSimple. Дополнительные сведения см. в разделе [Снятие контроллера](#remove-a-controller).
4. Сначала вставьте новый контроллер 0, а затем новый контроллер 1. Дополнительные сведения см. в разделе [Установка контроллера](#insert-a-controller). При этом запускается логика замены двух контроллеров. Дополнительные сведения см. в разделе [Логика замены двух контроллеров](#dual-controller-replacement-logic).
5. Пока логика замены контроллеров работает в фоновом режиме, подключите кабели. Внимательно подключите все кабели точно так же, как они были подключены до замены. Подробные инструкции для своей модели см. в разделе "Подключение кабельного хозяйства к устройству" статьи [Установка устройства StorSimple 8100](storsimple-8100-hardware-installation.md) или [Установка устройства StorSimple 8600](storsimple-8600-hardware-installation.md).
6. Включите устройство StorSimple. При использовании модели 8600:
   
   1. сначала включите корпус EBOD.
   2. Подождите, пока корпус EBOD включится.
   3. Включите основной корпус.
   4. После перезапуска первого контроллера и его перехода в рабочее состояние будет запущена система.
      
      > [!NOTE]
      > При отслеживании устройства через последовательную консоль можно заметить несколько перезапусков при восстановлении контроллера после процедуры замены. При появлении меню последовательной консоли процедура замены считается завершенной. Если меню не появляется в течение 2,5 часов после начала замены контроллера, [обратитесь в службу поддержки Майкрософт](storsimple-8000-contact-microsoft-support.md).
     
## <a name="remove-a-controller"></a>Снятие контроллера
Используйте следующую процедуру для снятия неисправного модуля контроллера с устройства StorSimple.

> [!NOTE]
> Следующие рисунки представлены для контроллера 0. Для контроллера 1 их нужно изменить на обратные.


#### <a name="to-remove-a-controller-module"></a>Для снятия модуля контроллера
1. Возьмитесь за защелку контроллера большим и указательным пальцем.
2. Плавно сожмите большой и указательный палец для освобождения защелки контроллера.
   
    ![ Освобождение защелки контроллера](./media/storsimple-controller-replacement/IC741047.png)
   
    **Рис. 2.** Освобождение защелки контроллера
3. Используйте защелку в качестве рукоятки для извлечения контроллера из корпуса.
   
    ![Извлечение контроллера из корпуса](./media/storsimple-controller-replacement/IC741048.png)
   
    **Рис. 3.** Извлечение контроллера из корпуса

## <a name="insert-a-controller"></a>Установка контроллера
Используйте следующую процедуру для установки нового контроллера в устройство StorSimple после снятия неисправного.

#### <a name="to-install-a-controller-module"></a>Для установки модуля контроллера
1. Проверьте отсутствие повреждений в соединительных разъемах. Не устанавливайте модуль, если контакты на разъеме повреждены или изогнуты.
2. Установите модуль контроллера в корпус с открытой защелкой.
   
    ![Установка контроллера в корпус](./media/storsimple-controller-replacement/IC741053.png)
   
    **Рис. 4.** Установка контроллера в корпус
3. Установив модуль контроллера, начните закрывать защелку, продолжая нажимать на модуль контроллера. Защелка начнет закрываться, фиксируя контроллер на месте установки.
   
    ![Закрытие защелки контроллера](./media/storsimple-controller-replacement/IC741054.png)
   
    **Рис. 5.** Закрытие защелки контроллера
4. После срабатывания защелки установка завершена. Индикатор **ОК** должен загореться.
   
   > [!NOTE]
   > Для активации контроллера и индикатора может потребоваться до 5 минут.
  
5. Чтобы убедиться, что замена выполнена успешно, в портал Azure перейдите на устройство, а затем перейдите к разделу **мониторинг**  >  **работоспособности оборудования** и убедитесь, что контроллер 0 и контроллер 1 работоспособны (состояние — зеленый).

## <a name="identify-the-active-controller-on-your-device"></a>Определение активного контроллера устройства
Существует множество ситуаций, в которых необходимо определить активный контроллер на устройстве StorSimple: например, первая регистрация устройства или замена контроллера. Активный контроллер выполняет все операции со встроенным ПО дисков и сетевые операции. Для определения активного контроллера можно использовать любой из следующих методов:

* [Определение активного контроллера с помощью портала Azure](#use-the-azure-portal-to-identify-the-active-controller)
* [Определение активного контроллера с помощью Windows PowerShell для StorSimple](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)
* [Определение активного контроллера с помощью проверки физического устройства](#check-the-physical-device-to-identify-the-active-controller)

Каждая из этих процедур описана ниже.

### <a name="use-the-azure-portal-to-identify-the-active-controller"></a>Определение активного контроллера с помощью портала Azure
В портал Azure перейдите на устройство, а затем для **наблюдения за**  >  **работоспособностью оборудования** и прокрутите до раздела **контроллеры** . Здесь можно проверить, какой контроллер активен.

![Определение активного контроллера с помощью портала Azure](./media/storsimple-controller-replacement/IC752072.png)

**Рис. 6.** Отображение активного контроллера на портале Azure

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Определение активного контроллера с помощью Windows PowerShell для StorSimple
При доступе к устройству через последовательную консоль выводится заглавное сообщение. Заглавное сообщение содержит основные сведения об устройстве StorSimple, такие как модель, имя, версия установленного программного обеспечения и состояние контроллера, к которому производится доступ. Пример заглавного сообщения приведен на следующем рисунке:

![Заглавное сообщение в последовательной консоли](./media/storsimple-controller-replacement/IC741098.png)

**Рис. 7.** Заглавное сообщение с активным контроллером 0

С помощью заглавного сообщения можно определить, активен или пассивен контроллер, к которому вы подключаетесь.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Определение активного контроллера с помощью проверки физического устройства
Для определения активного контроллера на устройстве найдите синий индикатор над портом ДАННЫЕ 5 на задней поверхности основного корпуса.

Если индикатор мигает, контроллер активен и другой контроллер находится в режиме ожидания. В качестве вспомогательного средства воспользуйтесь следующей схемой и таблицей.

![Задняя панель основного корпуса устройства с портами данных](./media/storsimple-controller-replacement/IC741055.png)

**Рис. 8.** Задняя поверхность основного корпуса с портами данных и индикаторами

| Метка | Описание |
|:--- |:--- |
| 1–6 |сетевые порты ДАННЫЕ 0–5  |
| 7 |Синий индикатор |

## <a name="next-steps"></a>Дальнейшие действия
Узнайте подробнее о [замене компонентов оборудования StorSimple](storsimple-8000-hardware-component-replacement.md).

