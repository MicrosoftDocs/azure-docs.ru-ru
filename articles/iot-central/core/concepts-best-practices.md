---
title: Рекомендации по разработке устройств в Azure IoT Central | Документация Майкрософт
description: В этой статье описываются рекомендации по подключению устройств в Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 03/03/2021
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom:
- device-developer
ms.openlocfilehash: e8ae8b0173e53c0a46ded1a2690175e367997c9f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102054953"
---
# <a name="best-practices-for-device-development"></a>Рекомендации по разработке устройств

*Эта статья предназначена для разработчиков устройств*

В этих рекомендациях показано, как реализовать устройства, чтобы воспользоваться преимуществами встроенного аварийного восстановления и автоматического масштабирования в IoT Central.

В следующем списке показан поток высокого уровня, когда устройство подключается к IoT Central:

1. Используйте службу DPS для подготовки устройства и получения строки подключения устройства.

1. Используйте строку подключения для подключения внутренней конечной точки центра Интернета вещей IoT Central. Отправка данных в приложение IoT Central и получение данных из него.

1. Если устройство получает ошибки подключения, то в зависимости от типа ошибки повторите попытку подключения или повторно выполните инициализацию устройства.

## <a name="use-dps-to-provision-the-device"></a>Использование DPS для подготовки устройства

Для подготовки устройства с помощью DPS используйте идентификатор области, учетные данные и идентификатор устройства из приложения IoT Central. Дополнительные сведения о типах учетных данных см. в разделе Регистрация [группы X. 509](concepts-get-connected.md#x509-group-enrollment) и [регистрация группы SAS](concepts-get-connected.md#sas-group-enrollment). Дополнительные сведения об идентификаторах устройств см. в разделе [Регистрация устройств](concepts-get-connected.md#device-registration).

При успешном выполнении служба DPS возвращает строку подключения, которую устройство может использовать для подключения к приложению IoT Central. Сведения об устранении ошибок подготовки см. в разделе [Проверка состояния подготовки устройства](troubleshoot-connection.md#check-the-provisioning-status-of-your-device).

Устройство может кэшировать строку подключения, которая будет использоваться для последующих соединений. Однако устройство должно быть подготовлено к [обработке ошибок подключения](#handle-connection-failures).

## <a name="connect-to-iot-central"></a>Подключение к IoT Central

Используйте строку подключения для подключения внутренней конечной точки центра Интернета вещей IoT Central. Подключение позволяет отправлять данные телеметрии в приложение IoT Central, синхронизировать значения свойств с приложением IoT Central и отвечать на команды, отправленные приложением IoT Central.

## <a name="handle-connection-failures"></a>Обработку ошибок соединения

В целях масштабирования или аварийного восстановления IoT Central может обновить свой базовый центр Интернета вещей. Чтобы поддерживать подключение, код устройства должен обрабатывать определенные ошибки подключения, устанавливая подключение к новой конечной точке центра Интернета вещей.

Если устройство получает одну из следующих ошибок при подключении, для получения новой строки подключения необходимо повторить шаг подготовки с помощью DPS. Эти ошибки означают, что строка подключения, используемая устройством, больше не действительна:

- Недоступная конечная точка центра Интернета вещей.
- Токен безопасности с истекшим сроком действия.
- Устройство отключено в центре Интернета вещей.

Если устройство получает следующие ошибки при подключении, для повторного подключения следует использовать стратегию отката. Эти ошибки означают, что строка подключения, используемая устройством, по-прежнему действительна, но временные условия останавливают подключение устройства:

- Устройство с заблокированным оператором.
- Из службы произошла внутренняя ошибка 500.

Дополнительные сведения о кодах ошибок устройств см. в разделе [Устранение неполадок подключения устройств](troubleshoot-connection.md).

## <a name="next-steps"></a>Дальнейшие действия

Если вы являетесь разработчиком устройства, вот некоторые из предлагаемых дальнейших действий:

- Ознакомьтесь с примером кода, в котором показано, как использовать маркеры SAS в [руководстве по созданию и подключению клиентского приложения к приложению IOT Central Azure.](tutorial-connect-device.md)
- Узнайте, как [подключать устройства с помощью сертификатов X. 509, используя пакет SDK для устройств Node.js для IOT Central приложения](how-to-connect-devices-x509.md) .
- Узнайте, как [отслеживать подключение устройств с помощью Azure CLI](./howto-monitor-devices-azure-cli.md)
- Узнайте, как [определить новый тип устройств IOT в приложении IOT Central Azure](./howto-set-up-template.md) .
- Узнайте об [устройствах Azure IOT EDGE и Azure IOT Central](./concepts-iot-edge.md)
