---
title: Автоматическое обновление единиц обмена сообщениями (Служебная шина Azure)
description: В этой статье показано, как можно использовать автоматическое обновление единиц обмена сообщениями пространства имен служебной шины.
ms.topic: how-to
ms.date: 03/03/2021
ms.openlocfilehash: 7fc3aca82b8f01d70dec4fc2dac7842895417ec9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102177961"
---
# <a name="automatically-update-messaging-units-of-an-azure-service-bus-namespace"></a>Автоматическое обновление единиц обмена сообщениями в пространстве имен Служебной шины Azure 
Благодаря автомасштабированию вы получаете именно тот объем ресурсов, который нужен для обработки нагрузки в вашем приложении. Эта функция позволяет добавлять ресурсы для обработки дополнительной нагрузки и удалять неиспользуемые ресурсы для экономии средств. Дополнительные сведения о функции автомасштабирования Azure Monitor см. [в разделе Общие сведения об автомасштабировании в Microsoft Azure](../azure-monitor/autoscale/autoscale-overview.md) . 

Обмен сообщениями через служебную шину ценовой категории "Премиум" обеспечивает изоляцию ресурсов на уровне процессора и памяти, чтобы рабочая нагрузка каждого клиента выполнялась изолированно. Этот контейнер ресурсов называется **единицей обмена сообщениями**. Дополнительные сведения о единицах обмена сообщениями см. в разделе [Обмен сообщениями служебной шины Premium](service-bus-premium-messaging.md). 

С помощью функции автомасштабирования для пространств имен служебной шины уровня "Премиум" можно указать минимальное и максимальное число [единиц обмена сообщениями](service-bus-premium-messaging.md) , а также автоматически добавлять или удалять единицы обмена сообщениями на основе набора правил. 

Например, можно реализовать следующие сценарии масштабирования для пространств имен служебной шины с помощью функции автомасштабирования. 

- Увеличьте количество единиц обмена сообщениями для пространства имен служебной шины, когда загрузка ЦП пространства имен превышает 75%. 
- Уменьшите число единиц обмена сообщениями для пространства имен служебной шины, когда загрузка ЦП пространства имен ниже 25%. 
- Используйте больше единиц обмена сообщениями в рабочее время и меньше времени в часы работы. 

В этой статье показано, как можно автоматически масштабировать пространство имен служебной шины (обновления [единиц обмена сообщениями](service-bus-premium-messaging.md)) в портал Azure. 

> [!IMPORTANT]
> Эта статья относится только к уровню **Премиум** Служебной шины Azure. 

## <a name="autoscale-setting-page"></a>Страница параметров автомасштабирования
Сначала выполните следующие действия, чтобы перейти на страницу **Параметры автомасштабирования** для пространства имен служебной шины.

1. Войдите в [портал Azure](https://portal.azure.com). 
2. В строке поиска введите **служебная шина**, выберите служебная **шина** в раскрывающемся списке и нажмите клавишу **Ввод**. 
1. Выберите **пространство имен Premium** из списка пространств имен. 
1. Переключитесь на страницу **масштаб** . 

    :::image type="content" source="./media/automate-update-messaging-units/scale-page.png" alt-text="Пространство имен служебной шины — страница &quot;Масштаб&quot;":::

## <a name="manual-scale"></a>Ручное масштабирование 
Этот параметр позволяет задать фиксированное число единиц обмена сообщениями для пространства имен. 

1. На странице **Настройка автомасштабирования** выберите **масштаб вручную** , если он еще не выбран. 
1. Для параметра **единицы обмена сообщениями** выберите число единиц обмена сообщениями из раскрывающегося списка.
1. Нажмите кнопку **сохранить** на панели инструментов, чтобы сохранить параметр. 

    :::image type="content" source="./media/automate-update-messaging-units/manual-scale.png" alt-text="Масштабирование единиц обмена сообщениями вручную":::       


## <a name="custom-autoscale---default-condition"></a>Пользовательское условие автомасштабирования по умолчанию
Можно настроить автоматическое масштабирование единиц обмена сообщениями с помощью условий. Это условие масштабирования выполняется, если ни одно из других условий масштабирования не совпадает. Задать условие по умолчанию можно одним из следующих способов.

- Масштабирование на основе метрики (например, использование ЦП или памяти)
- Масштабировать до указанного числа единиц обмена сообщениями

Нельзя задать автоматическое масштабирование по заданному дню или диапазону дат для условия по умолчанию. Это условие масштабирования выполняется, если не совпадают никакие другие условия масштабирования с расписаниями. 

### <a name="scale-based-on-a-metric"></a>Масштабирование на основе метрики
В следующей процедуре показано, как добавить условие для автоматического увеличения числа единиц обмена сообщениями (горизонтальное масштабирование), когда загрузка ЦП превышает 75% и уменьшает количество единиц обмена сообщениями (в масштабе), если загрузка ЦП меньше 25%. Приращения выполняется от 1 до 2, от 2 до 4, от 4 до 8 и от 8 до 16. Аналогичным образом, уменьшение выполняется от 16 до 8, от 8 до 4, от 4 до 2 и от 2 до 1. 

1. На странице **Настройка автомасштабирования** выберите **Настраиваемый Автомасштабирование** для параметра **Выбор способа масштабирования ресурса** . 
1. В разделе **по умолчанию** на странице Укажите **имя** для условия по умолчанию. Щелкните значок с изображением **карандаша** , чтобы изменить текст. 
1. Выберите **Масштаб на основе метрики** для **режима масштабирования**. 
1. Выберите **+ Добавить правило**. 

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-metric-add-rule-link.png" alt-text="Масштабирование по умолчанию на основе метрики":::    
1. На странице **правило масштабирования** выполните следующие действия.
    1. Выберите метрику из раскрывающегося списка **имя метрики** . В этом примере это **ЦП**. 
    1. Выберите оператор и пороговые значения. В этом примере они **больше** и **75** для **порогового значения метрики, чтобы активировать действие масштабирования**. 
    1. Выберите **операцию** в разделе **действие** . В этом примере он имеет значение **увеличить**. 
    1. Затем выберите **Добавить** .
    
        :::image type="content" source="./media/automate-update-messaging-units/scale-rule-cpu-75.png" alt-text="По умолчанию — масштабировать, если загрузка ЦП превышает 75%":::       

        > [!NOTE]
        > Функция автомасштабирования увеличивает количество единиц обмена сообщениями для пространства имен, если общая загрузка ЦП превышает 75% в этом примере. Приращения выполняется от 1 до 2, от 2 до 4, от 4 до 8 и от 8 до 16. 
1. Выберите **+ Добавить правило** еще раз и выполните следующие действия на странице **правила масштабирования** :
    1. Выберите метрику из раскрывающегося списка **имя метрики** . В этом примере это **ЦП**. 
    1. Выберите оператор и пороговые значения. В этом примере они **меньше** и **25** для **порогового значения метрики, чтобы активировать действие масштабирования**. 
    1. Выберите **операцию** в разделе **действие** . В этом примере это значение **уменьшается**. 
    1. Затем выберите **Добавить** . 

        :::image type="content" source="./media/automate-update-messaging-units/scale-rule-cpu-25.png" alt-text="Масштаб по умолчанию в случае, если загрузка ЦП меньше 25%":::       

        > [!NOTE]
        > Функция автомасштабирования сокращает количество единиц обмена сообщениями для пространства имен, если в этом примере общая загрузка ЦП ниже 25%. Уменьшение выполняется от 16 до 8, от 8 до 4, с 4 до 2 и от 2 до 1. 
1. Задайте **минимальное** и **Максимальное** **число единиц** обмена сообщениями.

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-metric-based.png" alt-text="Правило по умолчанию, основанное на метрике":::
1. Нажмите кнопку **сохранить** на панели инструментов, чтобы сохранить параметр автомасштабирования. 
        
### <a name="scale-to-specific-number-of-messaging-units"></a>Масштабировать до указанного числа единиц обмена сообщениями
Выполните следующие действия, чтобы настроить правило для масштабирования пространства имен, чтобы использовать определенное количество единиц обмена сообщениями. Опять же, условие по умолчанию применяется, если ни одно из других условий масштабирования не совпадает. 

1. На странице **Настройка автомасштабирования** выберите **Настраиваемый Автомасштабирование** для параметра **Выбор способа масштабирования ресурса** . 
1. В разделе **по умолчанию** на странице Укажите **имя** для условия по умолчанию. 
1. Выберите **масштаб для конкретных единиц обмена сообщениями в** **режиме масштабирования**. 
1. Для **единиц обмена сообщениями** выберите число единиц обмена сообщениями по умолчанию. 

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-messaging-units.png" alt-text="По умолчанию — масштабирование по конкретным единицам обмена сообщениями":::       

## <a name="custom-autoscale---additional-conditions"></a>Настраиваемое Автомасштабирование — дополнительные условия
В предыдущем разделе показано, как добавить условие по умолчанию для параметра автомасштабирования. В этом разделе показано, как добавить дополнительные условия в параметр автомасштабирования. Для этих дополнительных условий, не относящихся к умолчанию, можно задать расписание на основе конкретных дней недели или диапазона дат. 

### <a name="scale-based-on-a-metric"></a>Масштабирование на основе метрики
1. На странице **Настройка автомасштабирования** выберите **Настраиваемый Автомасштабирование** для параметра **Выбор способа масштабирования ресурса** . 
1. Выберите **Добавить условие масштабирования** в блоке **по умолчанию** . 

    :::image type="content" source="./media/automate-update-messaging-units/add-scale-condition-link.png" alt-text="Пользовательский — добавить ссылку на условие масштабирования":::    
1. Укажите **имя** условия. 
1. Убедитесь, что выбран параметр **масштабировать на основе метрики** . 
1. Выберите **+ Добавить правило** , чтобы добавить правило для увеличения числа единиц обмена сообщениями, если общая загрузка цп превышает 75%. Выполните действия из раздела [условие по умолчанию](#custom-autoscale---default-condition) . 
5. Задайте **минимальное** и **Максимальное** **число единиц** обмена сообщениями.
6. Можно также задать **Расписание** для пользовательского условия (но не для условия по умолчанию). Можно указать даты начала и окончания для условия (или) выбрать конкретные дни (понедельник, вторник и т. д.) в неделе. 
    1. Если выбран параметр **указать даты начала и окончания**, выберите **Часовой пояс**, **дату и время начала** и **дату и время окончания** (как показано на следующем рисунке), чтобы условие действовало. 

       :::image type="content" source="./media/automate-update-messaging-units/custom-min-max-default.png" alt-text="Минимальное, максимальное и значение по умолчанию для количества единиц обмена сообщениями":::
    1. Если выбран вариант **повторять определенные дни**, выберите дни недели, часовой пояс, время начала и время окончания, когда должно применяться условие. 

        :::image type="content" source="./media/automate-update-messaging-units/repeat-specific-days.png" alt-text="Повторять определенные дни":::
  
### <a name="scale-to-specific-number-of-messaging-units"></a>Масштабировать до указанного числа единиц обмена сообщениями
1. На странице **Настройка автомасштабирования** выберите **Настраиваемый Автомасштабирование** для параметра **Выбор способа масштабирования ресурса** . 
1. Выберите **Добавить условие масштабирования** в блоке **по умолчанию** . 

    :::image type="content" source="./media/automate-update-messaging-units/add-scale-condition-link.png" alt-text="Пользовательский — добавить ссылку на условие масштабирования":::    
1. Укажите **имя** условия. 
2. Выберите параметр **масштабировать в определенные единицы обмена сообщениями** для **режима масштабирования**. 
1. Выберите число **единиц обмена сообщениями** из раскрывающегося списка. 
6. В поле **Расписание** укажите даты начала и окончания для условия (или) выберите конкретные дни (понедельник, вторник и т. д.) в неделю и раз. 
    1. Если выбран параметр **указать даты начала и окончания**, выберите **Часовой пояс**, **дату и время начала** и **дату и время окончания** действия условия. 
    
    :::image type="content" source="./media/automate-update-messaging-units/scale-specific-messaging-units-start-end-dates.png" alt-text="масштабировать до конкретных единиц обмена сообщениями — даты начала и окончания":::        
    1. Если выбран вариант **повторять определенные дни**, выберите дни недели, часовой пояс, время начала и время окончания, когда должно применяться условие.
    
    :::image type="content" source="./media/automate-update-messaging-units/repeat-specific-days-2.png" alt-text="масштабировать до конкретных единиц обмена сообщениями — повторять определенные дни":::

    
    Дополнительные сведения о работе параметров автомасштабирования, особенно о том, как он выбирает профиль или условие и оценивает несколько правил, см. в разделе Общие сведения о [параметрах автомасштабирования](../azure-monitor/autoscale/autoscale-understanding-settings.md).          

    > [!NOTE]
    > - Метрики, которые вы просматриваете для принятия решений об автоматическом масштабировании, могут быть 5-10 минут назад. При работе с рабочими нагрузками пиковых рекомендуется использовать меньшее время для увеличения и уменьшения длительности масштабирования (> 10 минут), чтобы убедиться в наличии достаточного количества единиц обмена сообщениями для обработки рабочих нагрузок пиковых. 
    > 
    > - Если вы видите ошибки из-за нехватки емкости (нет доступных единиц обмена сообщениями), отправьте запрос в службу поддержки.  


## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о единицах обмена сообщениями см. в [этой статье](service-bus-premium-messaging.md).

