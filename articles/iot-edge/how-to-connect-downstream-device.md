---
title: Подключение подчиненных устройств — Azure IoT Edge | Документация Майкрософт
description: Как настроить подчиненные или конечные устройства для подключения к Azure IoT Edge устройствам шлюза.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/15/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
- devx-track-js
ms.openlocfilehash: dc2d2d3e92435c7a028b43a095f456c2c383ecb4
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103199636"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Подключение подчиненного устройства к шлюзу Azure IoT Edge

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

В этой статье приводятся инструкции по установлению доверенного подключения между подчиненными устройствами и IoT Edge прозрачными шлюзами. В сценарии с прозрачным шлюзом одно или несколько устройств могут передавать свои сообщения через одно устройство шлюза, которое поддерживает подключение к центру Интернета вещей.

Для настройки успешного подключения к прозрачному шлюзу необходимо выполнить три шага. В этой статье рассматривается третий шаг:

1. Настройте устройство шлюза в качестве сервера, чтобы подчиненные устройства могли безопасно подключаться к нему. Настройте шлюз для приема сообщений от подчиненных устройств и направьте их в нужное место назначения. Инструкции см. в разделе [Настройка устройства IOT Edge для работы в качестве прозрачного шлюза](how-to-create-transparent-gateway.md).
2. Создайте удостоверение устройства для подчиненного устройства, чтобы оно может проходить проверку подлинности в центре Интернета вещей. Настройте подчиненное устройство для отправки сообщений через устройство шлюза. Дополнительные сведения см. в статье [Проверка подлинности подчиненного устройства в центре Интернета вещей Azure](how-to-authenticate-downstream-device.md).
3. **Подключите подчиненное устройство к устройству шлюза и начните отправлять сообщения.**

В этой статье обсуждаются основные понятия для подключений к подчиненным устройствам и приводятся инструкции по настройке нижестоящих устройств.

* принципы работы безопасности транспортного уровня (TLS) и сертификатов;
* сведения о работе библиотек TLS и использовании сертификатов в разных операционных системах;
* пошаговое руководство по примерам Azure IoT на нескольких языках, которые помогут вам быстро приступить к работе.

В этой статье под терминами *шлюз* и *шлюз IoT Edge* подразумевается устройство IoT Edge, которое настроенное в качестве прозрачного шлюза.

## <a name="prerequisites"></a>Предварительные требования

* Наличие файла сертификата корневого ЦС, который использовался для создания сертификата ЦС устройства, в [настройке устройства IOT EDGE в качестве прозрачного шлюза](how-to-create-transparent-gateway.md) , доступного на подчиненном устройстве. Подчиненное устройство использует этот сертификат для проверки подлинности устройства шлюза. Если вы использовали демонстрационные сертификаты, сертификат корневого ЦС называется **Азуре-ИОТ-тест-Онли. root. ca. CERT. pem**.
* Измените строку подключения, указывающую на устройство шлюза, как описано в разделах [Проверка подлинности подчиненного устройства в центре Интернета вещей Azure](how-to-authenticate-downstream-device.md).

## <a name="prepare-a-downstream-device"></a>Подготовка подчиненного устройства

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"
Роль подчиненного устройства может выполнять любое приложение или платформа с идентификатором, созданным с помощью облачной службы Центра Интернета вещей Azure. Часто для этих приложений применяется [пакет SDK для устройств Azure IoT](../iot-hub/iot-hub-devguide-sdks.md). Подчиненное устройство может даже быть приложением, которое работает на IoT Edge устройстве шлюза. Однако другое устройство IoT Edge не может быть нисходящем шлюзом IoT Edge.
:::moniker-end
<!-- end 1.1 -->

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"
Роль подчиненного устройства может выполнять любое приложение или платформа с идентификатором, созданным с помощью облачной службы Центра Интернета вещей Azure. Часто для этих приложений применяется [пакет SDK для устройств Azure IoT](../iot-hub/iot-hub-devguide-sdks.md). Подчиненное устройство может даже быть приложением, которое работает на IoT Edge устройстве шлюза.

В этой статье описаны действия по подключению устройства Интернета вещей в качестве подчиненного устройства. Если в качестве подчиненного устройства имеется IoT Edge устройство, см. статью [Подключение подчиненного IOT Edge устройства к шлюзу Azure IOT Edge](how-to-connect-downstream-iot-edge-device.md).
:::moniker-end
<!-- end 1.2 -->

>[!NOTE]
>Устройства IoT, зарегистрированные в центре Интернета вещей, могут использовать [модуль двойников](../iot-hub/iot-hub-devguide-module-twins.md) для изоляции различных процессов, оборудования или функций на одном устройстве. Шлюзы IoT Edge поддерживают подключения подчиненных модулей с помощью проверки подлинности с симметричным ключом, а не для сертификата X. 509.

Чтобы подключить подчиненное устройство к шлюзу Azure IoT Edge, вам потребуются следующие два компонента:

* Устройство или приложение, в настройках которого указана строка подключения устройств Центра Интернета вещей с данными для подключения к шлюзу.

    Этот шаг был выполнен в предыдущей статье. [Проверка подлинности подчиненного устройства в центре Интернета вещей Azure](how-to-authenticate-downstream-device.md#retrieve-and-modify-connection-string).

* Устройство или приложение должно доверять **сертификату корневого ЦС** шлюза, чтобы проверить подключения TLS к устройству шлюза.

    Этот шаг подробно описан в оставшейся части этой статьи. Этот шаг можно выполнить одним из двух способов: установив сертификат ЦС в хранилище сертификатов операционной системы или (для определенных языков), обратившись к сертификату в приложениях с помощью пакетов SDK для Интернета вещей Azure.

## <a name="tls-and-certificate-fundamentals"></a>Основные сведения о TLS и сертификатах

При создании безопасного подключения между подчиненными устройствами и IoT Edge возникают такие же трудности, как и при любом обмене данными через Интернет в режиме "клиент — сервер". Клиент и сервер безопасно обмениваются данными через Интернет по [протоколу TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security). Протокол TLS основан на конструкциях стандарта [инфраструктуры открытых ключей (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure), называемых сертификатами. Протокол TLS является довольно вовлеченным спецификацией и предназначен для широкого спектра разделов, относящихся к защите двух конечных точек. В этом разделе перечислены основные понятия, относящиеся к безопасному подключению устройств к IoT Edge шлюзу.

Когда клиент подключается к серверу, сервер предоставляет ему *цепочку сертификатов сервера*. Такая цепочка обычно состоит из сертификата корневого ЦС, одного или нескольких сертификатов промежуточных ЦС, и сертификата самого сервера. Клиент устанавливает отношения доверия с сервером, криптографически проверяя всю цепочку сертификатов сервера. Эта проверка клиента цепочки сертификатов сервера называется *проверкой цепочки серверов*. Клиент не позволяет серверу доказать владение закрытым ключом, связанным с сертификатом сервера, в процессе, который называется « *доказательство принадлежности*». Сочетание проверки и проверки принадлежности серверной цепочки называется *проверкой подлинности сервера*. Чтобы проверить цепочку сертификатов сервера, клиенту нужна копия сертификата корневого ЦС, который использовался для создания (выдачи) этого сертификата сервера. Обычно браузеры при подключении к веб-сайтам используют предварительно настроенный набор часто используемых сертификатов ЦС, оптимизируя работу клиента.

Когда устройство подключается к Центру Интернета вещей, оно выполняет роль клиента облачной службы, а служба Центра Интернета вещей представляет собой сервер. Облачная служба Центра Интернета вещей поддерживается сертификатом корневого ЦС с именем **Baltimore CyberTrust Root**, который является общедоступным и широко распространенным. Так как сертификат ЦС для Центра Интернета вещей уже установлен на большинстве устройств, во многих реализациях TLS (OpenSSL, Schannel, LibreSSL) он автоматически используется для проверки сертификата сервера. Однако устройство, которое успешно подключается к центру Интернета вещей, может столкнуться с проблемами при подключении к шлюзу IoT Edge.

Когда устройство подключается к шлюзу IoT Edge, подчиненное устройство выполняет роль клиента, а устройство шлюза представляет собой сервер. Azure IoT Edge позволяет создавать цепочки сертификатов шлюза, тем не менее, они будут видны. Вы можете использовать общедоступный сертификат ЦС, например Baltimore, или использовать самозаверяющий (или внутренний) сертификат корневого ЦС. Сертификаты общедоступных ЦС часто предоставляются за плату, что делает их использование более оправданным для рабочих сценариев. Самозаверяющие сертификаты удобны для разработки и тестирования. Если вы используете демонстрационные сертификаты, они представляют собой самозаверяющие сертификаты корневого ЦС.

Если вы используете для шлюза IoT Edge самозаверяющий корневой сертификат, он должен быть установлен или предоставлен для всех подчиненных устройств, которые будут подключаться к этому шлюзу.

![Установка сертификата шлюза](./media/how-to-create-transparent-gateway/gateway-setup.png)

Дополнительные сведения о сертификатах IoT Edge и некоторых ограничениях для рабочей среды см. в статье [Сведения об использовании сертификатов Azure IoT Edge](iot-edge-certs.md).

## <a name="provide-the-root-ca-certificate"></a>Укажите сертификат корневого ЦС

Чтобы проверить сертификаты устройства шлюза, подчиненному устройству требуется собственная копия сертификата корневого ЦС. Если вы использовали сценарии, предоставленные в репозитории IoT Edge Git для создания тестовых сертификатов, сертификат корневого ЦС называется **Азуре-ИОТ-тест-Онли. root. ca. CERT. pem**. Если вы еще не сделали это в рамках других этапов подготовки подчиненных устройств, переместите этот файл сертификата в любой каталог на подчиненном устройстве. Для перемещения файла сертификата можно использовать службу, такую как [Azure Key Vault](../key-vault/index.yml) , или функцию, например [протокол безопасного копирования](https://www.ssh.com/ssh/scp/) .

## <a name="install-certificates-in-the-os"></a>Установка сертификатов в ОС

Когда сертификат корневого ЦС появится на подчиненном устройстве, необходимо убедиться, что приложения, подключающиеся к шлюзу, имеют доступ к сертификату.

Установка сертификата корневого ЦС в хранилище сертификатов операционной системы обычно позволяет большинству приложений использовать сертификат корневого ЦС. Существуют некоторые исключения, например приложения NodeJS, которые не используют хранилище сертификатов ОС, а используют внутреннее хранилище сертификатов среды выполнения узла. Если вы не можете установить сертификат на уровне операционной системы, переходите к [использованию сертификатов с пакетами SDK для Интернета вещей Azure](#use-certificates-with-azure-iot-sdks).

### <a name="ubuntu"></a>Ubuntu

Команды из следующего примера устанавливают сертификат ЦС на узле под управлением ОС Ubuntu. В этом примере предполагается, что вы используете сертификат **Азуре-ИОТ-тест-Онли. root. ca. CERT. pem** из статей о предварительных требованиях и скопировали сертификат в расположение на подчиненном устройстве.

```bash
sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
sudo update-ca-certificates
```

Должно отобразиться сообщение "обновление сертификатов в/ЕТК/ССЛ/цертс... 1 добавлено, 0 удалено; Готово».

### <a name="windows"></a>Windows

В следующих шагах описано, как установить сертификат ЦС на узле под управлением ОС Windows. В этом примере предполагается, что вы используете сертификат **Азуре-ИОТ-тест-Онли. root. ca. CERT. pem** из статей о предварительных требованиях и скопировали сертификат в расположение на подчиненном устройстве.

Сертификаты можно установить с помощью [импорта сертификата](/powershell/module/pkiclient/import-certificate) PowerShell с правами администратора:

```powershell
import-certificate  <file path>\azure-iot-test-only.root.ca.cert.pem -certstorelocation cert:\LocalMachine\root
```

Сертификаты также можно установить с помощью служебной программы **certlm** :

1. В меню "Пуск" выполните поиск по элементу **Управление сертификатами компьютеров**. Откроется служебная программа **certlm**.
2. Перейдите к разделу **Сертификаты — локальные**  >  **Доверенные корневые центры сертификации** компьютера.
3. Щелкните правой кнопкой мыши элемент **Сертификаты** и выберите **Все задачи** > **Импорт**. Откроется мастер импорта сертификатов.
4. Выполните действия по его подсказкам и импортируйте файл сертификата `<path>/azure-iot-test-only.root.ca.cert.pem`. По завершении вы увидите сообщение "Импорт выполнен".

Можно также установить сертификаты программным способом с помощью API .NET, как показано в примере кода для .NET далее в этой статье.

Обычно приложения используют стек TLS [Schannel](/windows/desktop/com/schannel), предоставляемый ОС Windows, для безопасного подключения через TLS. При использовании Schannel *требуется*, чтобы все сертификаты были установлены в хранилище сертификатов Windows перед попыткой установить TLS-подключение.

## <a name="use-certificates-with-azure-iot-sdks"></a>Использование сертификатов с пакетами SDK для Azure IoT

В этом разделе на примере простых приложений описывается, как пакеты SDK для Azure IoT подключаются к устройству IoT Edge. Все эти примеры пытаются подключить клиент устройства и отправить сообщения телеметрии в шлюз, а затем закрывают подключение и завершают работу.

Прежде чем использовать примеры уровня приложения, следует подготовить следующие два компонента:

* Строка подключения к центру Интернета вещей подчиненного устройства, измененная для указания на устройство шлюза, и все сертификаты, необходимые для проверки подлинности подчиненного устройства в центре Интернета вещей. Дополнительные сведения см. в статье [Проверка подлинности подчиненного устройства в центре Интернета вещей Azure](how-to-authenticate-downstream-device.md).

* Полный путь к сертификату корневого ЦС, который вы скопировали (сохранили) на подчиненное устройство.

    Например, `<path>/azure-iot-test-only.root.ca.cert.pem`.

### <a name="nodejs"></a>Node.js

Этот раздел содержит пример приложения для подключения клиентского устройства Azure IoT NodeJS к шлюзу IoT Edge. Для приложений NodeJS необходимо установить сертификат корневого ЦС на уровне приложения, как показано ниже. Приложения NodeJS не используют хранилище сертификатов системы.

1. Получите пример файла **edge_downstream_device.js** из [репозитория примеров использования пакета SDK для устройств Azure IoT для Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples).
2. Убедитесь, что у вас есть все необходимые компоненты для запуска примера (см. файл **readme.md**).
3. В файле edge_downstream_device.js измените переменные **connectionString** и **edge_ca_cert_path**.
4. В документации по пакету SDK вы найдете инструкции по запуску примера на конкретном устройстве.

Чтобы помочь вам разобраться в выполняемом примере, мы приводим ниже фрагмент кода, в котором клиентский пакет SDK считывает файл сертификата и использует его для создания безопасного TLS-подключения:

```javascript
// Provide the Azure IoT device client via setOptions with the X509
// Edge root CA certificate that was used to setup the Edge runtime
var options = {
    ca : fs.readFileSync(edge_ca_cert_path, 'utf-8'),
};
```

### <a name="net"></a>.NET

Этот раздел знакомит вас с примером приложения для подключения клиентского устройства Azure IoT .NET к шлюзу IoT Edge. Приложения .NET могут автоматически применять любые сертификаты, установленные в хранилище сертификатов операционной системы на узлах Windows и Linux.

1. Получите пример для **EdgeDownstreamDevice** из [папки примеров .NET для IoT Edge](https://github.com/Azure/iotedge/tree/master/samples/dotnet/EdgeDownstreamDevice).
2. Убедитесь, что у вас есть все необходимые компоненты для запуска примера (см. файл **readme.md**).
3. В файле **Properties/launchSettings.json** измените переменные **DEVICE_CONNECTION_STRING** и **CA_CERTIFICATE_PATH**. Если вы хотите использовать сертификат, установленный в хост-системе в хранилище доверенных сертификатов, оставьте значение этой переменной пустым.
4. В документации по пакету SDK вы найдете инструкции по запуску примера на конкретном устройстве.

Чтобы установить доверенный сертификат в хранилище сертификатов программным способом с помощью приложения .NET, воспользуйтесь функцией **InstallCACert()**, которая определена в файле **EdgeDownstreamDevice/Program.cs**. Эта операция является идемпотентной, то есть ее можно безопасно выполнять несколько раз и получать одинаковые значения без побочных эффектов.

### <a name="c"></a>C

Этот раздел знакомит вас с примером приложения для подключения клиентского устройства Azure IoT C к шлюзу IoT Edge. Пакет SDK для C поддерживает множество библиотек TLS, включая OpenSSL, WolfSSL и Schannel. См. дополнительные сведения о [пакете SDK для Azure IoT C](https://github.com/Azure/azure-iot-sdk-c).

1. Получите приложение **iotedge_downstream_device_sample** из [набора примеров использования пакета SDK для устройств Azure IoT для С](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples).
2. Убедитесь, что у вас есть все необходимые компоненты для запуска примера (см. файл **readme.md**).
3. В файле iotedge_downstream_device_sample.c измените переменные **connectionString** и **edge_ca_cert_path**.
4. В документации по пакету SDK вы найдете инструкции по запуску примера на конкретном устройстве.


Пакет SDK для устройств Azure IoT для C предоставляет возможность зарегистрировать сертификат ЦС при настройке клиента. Эта операция не устанавливает сертификат, а просто использует сертификат в текстовом формате прямо из памяти. Сохраненный сертификат предоставляется базовому стеку TLS при установке подключения.

```C
(void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
```

>[!NOTE]
> Метод регистрации сертификата ЦС при настройке клиента может измениться при использовании [управляемого](https://github.com/Azure/azure-iot-sdk-c#packages-and-libraries) пакета или библиотеки. Например, [Библиотека IDE на основе Arduino](https://github.com/azure/azure-iot-arduino) потребует добавления сертификата ЦС в массив сертификатов, определенный в файле Global [certs. c](https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c) , вместо использования `IoTHubDeviceClient_LL_SetOption` операции.  

Если вы не используете OpenSSL или другую библиотеку TLS, на узлах Windows пакет SDK по умолчанию применяет Schannel. Чтобы обеспечить правильную работу Schannel, сертификат корневого ЦС для IoT Edge должен быть установлен в хранилище сертификатов Windows, а не с помощью операции `IoTHubDeviceClient_SetOption`.

### <a name="java"></a>Java

Этот раздел знакомит вас с примером приложения для подключения клиентского устройства Azure IoT Java к шлюзу IoT Edge.

1. Получите пример **Send-event** из [набора примеров использования пакета SDK для устройств Интернета Azure IoT для Java](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples).
2. Убедитесь, что у вас есть все необходимые компоненты для запуска примера (см. файл **readme.md**).
3. В документации по пакету SDK вы найдете инструкции по запуску примера на конкретном устройстве.

### <a name="python"></a>Python

Этот раздел знакомит вас с примером приложения для подключения клиентского устройства Azure IoT Python к шлюзу IoT Edge.

1. Получите пример для **send_message_downstream** из [пакета SDK для устройств Azure IOT для примеров Python](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-edge-scenarios).
2. Задайте `IOTHUB_DEVICE_CONNECTION_STRING` `IOTEDGE_ROOT_CA_CERT_PATH` переменные среды и, как указано в комментариях к скрипту Python.
3. Дополнительные инструкции по запуску примера на устройстве см. в документации по пакету SDK.

## <a name="test-the-gateway-connection"></a>Тестирование подключения к шлюзу

Используйте этот пример команды на подчиненном устройстве, чтобы проверить возможность подключения к устройству шлюза:

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

Эта команда проверяет соединения через МКТТС (порт 8883). Если вы используете другой протокол, настройте команду по мере необходимости для AMQPS (5671) или HTTPS (433).

Выходные данные этой команды могут быть длинными, включая сведения обо всех сертификатах в цепочке. Если подключение установлено успешно, вы увидите строку, например `Verification: OK` или `Verify return code: 0 (ok)` .

![Проверка подключения шлюза](./media/how-to-connect-downstream-device/verification-ok.png)

## <a name="troubleshoot-the-gateway-connection"></a>Устранение неполадок подключения шлюза

Если конечное устройство постоянно подключено к устройству шлюза, попробуйте выполнить следующие действия для решения проблемы.

1. Совпадает ли имя узла шлюза в строке подключения со значением имени узла в файле конфигурации IoT Edge на устройстве шлюза?
2. Разрешается ли имя узла шлюза в IP-адрес? Вы можете разрешить периодические подключения с помощью DNS или путем добавления записи файла узла на конечном устройстве.
3. Открыты ли порты связи в брандмауэре? Связь, основанная на используемом протоколе (МКТТС: 8883/AMQPS: 5671/HTTPS: 433), должна быть возможна между подчиненным устройством и прозрачным IoT Edge.

## <a name="next-steps"></a>Следующие шаги

Узнайте, как IoT Edge расширяет [возможности автономной работы](offline-capabilities.md) для подчиненных устройств.