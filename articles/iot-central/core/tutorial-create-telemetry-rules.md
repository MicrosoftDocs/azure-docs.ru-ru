---
title: Учебник. Создание правил и управление ими в приложении Azure IoT Central
description: В этом руководстве показано, как правила Azure IoT Central позволяют вам отслеживать устройства практически в реальном времени и автоматически вызывать действия (например, отправлять сообщение электронной почты) при срабатывании правила.
author: dominicbetts
ms.author: dobett
ms.date: 01/08/2021
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: b0b5aafd85fe6d992afa9d879f73ef0ec43e00d3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "99834380"
---
# <a name="tutorial-create-a-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Руководство по Создание правила и настройка уведомлений в приложении Azure IoT Central

*Эта статья предназначена для операторов, разработчиков и администраторов.*

Вы можете использовать Azure IoT Central для удаленного мониторинга подключенных устройств. Правила Azure IoT Central позволяют отслеживать устройства практически в реальном времени и автоматически вызывать действия, например отправку сообщения электронной почты. В этой статье объясняется, как создать правила для отслеживания телеметрии, отправляемой устройством.

Устройства могут использовать телеметрию для отправки числовых данных с устройства. Правило срабатывает, когда данные выбранной телеметрии превышают заданное пороговое значение.

В рамках этого руководства вы создадите правило, с помощью которого отправляется сообщение электронной почты, когда температура на имитированном датчике превышает 70&deg; F.

В этом руководстве описано следующее:

> [!div class="checklist"]
>
> * Создание правила
> * Добавление действия электронной почты

## <a name="prerequisites"></a>Предварительные требования

Перед началом работы, завершите два предыдущих кратких руководства по [созданию приложения Azure IoT Central](./quick-deploy-iot-central.md) и [добавлению имитированного устройства в приложение IoT Central](./quick-create-simulated-device.md), чтобы создать шаблон устройства **Контроллер датчика** и работать с ним.

## <a name="create-a-rule"></a>Создание правила

Чтобы создать правило телеметрии, шаблон устройства должен иметь по крайней мере одно значение телеметрии. В этом учебнике используется имитация устройства **Контроллер датчика**, которое отправляет телеметрию по температуре и влажности. Вы добавили этот шаблон устройства и создали имитацию устройства в кратком руководстве по [добавлению имитированного устройства в приложение IoT Central](./quick-create-simulated-device.md). Правило отслеживает передаваемые устройством данные температуры и отправляет сообщение электронной почты, когда температура превышает 70 градусов.

> [!NOTE]
> Максимальное количество правил на приложение — 50.

1. В левой области щелкните **Правила**.

1. Если вы еще не создали правило, появится следующий экран:

    :::image type="content" source="media/tutorial-create-telemetry-rules/rules-landing-page.png" alt-text="Снимок экрана, на котором показан пустой список правил":::

1. Выберите команду **Создать**, чтобы добавить новое правило.

1. Введите имя _Монитор температуры_, чтобы указать правило, и нажмите клавишу ВВОД.

1. Выберите шаблон устройства **Контроллер датчика**. По умолчанию правило автоматически применяется ко всем устройствам, связанным с шаблоном устройства. Чтобы выполнить фильтрацию для подмножества устройств, выберите **+ Фильтр** и используйте свойства устройств для их определения. Чтобы отключить правило, переведите переключатель в положение **Включено/Отключено**:

    :::image type="content" source="media/tutorial-create-telemetry-rules/device-filters.png" alt-text="Снимок экрана, на котором показан выбор шаблона устройства в определении правила":::

### <a name="configure-the-rule-conditions"></a>Настройка условий правила

Условия определяют критерии, которые отслеживает правило. В этом учебнике настройте правило, которое будет срабатывать, когда температура превысит 70&deg; F.

1. Выберите **Температура** в раскрывающемся списке **Телеметрия**.

1. Затем выберите **Больше** в качестве **оператора** и введите _70_ в качестве **значения**.

    :::image type="content" source="media/tutorial-create-telemetry-rules/condition-filled-out.png" alt-text="Снимок экрана, на котором показано условие температуры для правила":::

1. При необходимости можно задать **агрегацию по времени**. При выборе агрегации по времени необходимо также выбрать тип агрегата, например "среднее значение" или "сумма", в раскрывающемся списке.

    * Без агрегирования правило запускается для каждой точки данных телеметрии, которая соответствует условию. Например, если правило настроено на активацию, когда температура превышает 70, правило будет активироваться почти мгновенно, когда температура устройства превысит 70.
    * При использовании агрегирования правило срабатывает, если агрегированное значение точек данных телеметрии во временном окне соответствует условию. Например, если правило настроено для активации, когда температура превышает 70, а для агрегата времени задано значение 10 минут, правило будет активироваться, когда устройство сообщит о средней температуре выше 70, вычисленной за 10-минутный интервал.

    :::image type="content" source="media/tutorial-create-telemetry-rules/aggregate-condition-filled-out.png" alt-text="Снимок экрана, на котором показано заполненное условие агрегирования":::

Вы можете добавить несколько условий к правилу, выбрав **+ Условие**. Если задано несколько условий, для срабатывания правила необходимо, чтобы были соблюдены все условия. Каждое условие присоединяется с помощью неявного предложения `AND`. Если вы используете агрегацию по времени с несколькими условиями, все значения телеметрии должны быть агрегированы.

### <a name="configure-actions"></a>Настройка действий

После определения условия необходимо настроить действия, выполняемые при срабатывании правила. Действия вызываются в том случае, если соблюдаются все условия, указанные в правиле.

1. Щелкните **+ Электронная почта** в разделе **Действия**.

1. Введите _Предупреждение о температуре_ в качестве отображаемого имени действия, ваш адрес электронной почты в поле **Кому** и текст _Проверьте устройство!_ как примечание, которое отображается в тексте сообщения.

    > [!NOTE]
    > Письма отправляются только пользователям, которые были добавлены в приложение и входили в систему минимум один раз. Узнайте больше об [управлении пользователями](howto-administer.md) в Azure IoT Central.

    :::image type="content" source="media/tutorial-create-telemetry-rules/configure-action.png" alt-text="Снимок экрана, на котором показано действие электронной почты для правила":::

1. Чтобы сохранить действие, нажмите кнопку **Готово**. К правилу можно добавить несколько действий.

1. Чтобы сохранить правило, нажмите кнопку **Сохранить**. Правило станет активным в течение нескольких минут и начнет выполнять мониторинг данных телеметрии, отправляемых в приложение. Если указанное в правиле условие соблюдается, правило активирует настроенное действие отправки электронной почты.

Через некоторое время в случае срабатывания правила вы получите сообщение электронной почты:

:::image type="content" source="media/tutorial-create-telemetry-rules/email.png" alt-text="Снимок экрана: уведомление по электронной почте":::

## <a name="delete-a-rule"></a>Удаление правила

Если вам больше не нужно правило, удалите его. Для этого откройте правило и щелкните **Удалить**.

## <a name="enable-or-disable-a-rule"></a>Включение и отключение правила

Выберите правило, которое требуется включить или отключить. Установите переключатель **Включить или Отключить** в правиле, чтобы включить или отключить правило для всех устройств, охватываемых этим правилом.

## <a name="enable-or-disable-a-rule-for-specific-devices"></a>Включение или отключение правила для определенных устройств

Выберите правило, которое требуется включить или отключить. Используйте один или несколько фильтров в разделе **Целевые устройства**, чтобы ограничить область действия правила для устройств, которые вы хотите отслеживать.

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [iot-central-clean-up-resources](../../../includes/iot-central-clean-up-resources.md)]

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как выполнять следующие задачи:

* Создание правила на основе телеметрии
* Добавление действия

Теперь, когда вы определили правило на основе порогового значения, рекомендуется выполнить такой следующий шаг:

> [!div class="nextstepaction"]
> [Создание действий веб-перехватчиков на основе правил](./howto-create-webhooks.md)
