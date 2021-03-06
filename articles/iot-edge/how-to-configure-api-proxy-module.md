---
title: Настройка модуля прокси-сервера API — Azure IoT Edge | Документация Майкрософт
description: Узнайте, как настроить модуль прокси API для IoT Edge иерархий шлюза.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/10/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
monikerRange: '>=iotedge-2020-11'
ms.openlocfilehash: 1070a4c8daecfedae513f2fd8738c27abfb33078
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103200584"
---
# <a name="configure-the-api-proxy-module-for-your-gateway-hierarchy-scenario-preview"></a>Настройка модуля прокси API для сценария иерархии шлюза (Предварительная версия)

[!INCLUDE [iot-edge-version-202011](../../includes/iot-edge-version-202011.md)]

Прокси-модуль API позволяет IoT Edge устройствам передавать HTTP-запросы через шлюзы вместо прямого подключения к облачным службам. В этой статье рассматриваются параметры конфигурации, позволяющие настроить модуль для поддержки требований иерархии шлюза.

>[!NOTE]
>Для этого компонента требуется служба IoT Edge 1.2, которая предоставляется в общедоступной предварительной версии, в которой выполняются контейнеры Linux.

В некоторых сетевых архитектурах IoT Edge устройства за шлюзами не имеют прямого доступа к облаку. Все модули, пытающиеся подключиться к облачным службам, завершатся ошибкой. Прокси-модуль API поддерживает подчиненные IoT Edge устройства в этой конфигурации путем повторной маршрутизации подключений модулей для прохода по слоям иерархии шлюза. Он обеспечивает безопасный способ взаимодействия клиентов с несколькими службами по протоколу HTTPS без туннелирования, но при этом соединения на каждом уровне прерывается. Прокси-модуль API выступает в качестве прокси-модуля между устройствами IoT Edge в иерархии шлюзов до тех пор, пока они не находят на устройство IoT Edge на верхнем уровне. На этом этапе службы, работающие на верхнем уровне IoT Edge устройстве, обрабатывали эти запросы, а прокси-модуль API передает весь трафик HTTP из локальных служб в облако через один порт.

Прокси-модуль API может включить множество сценариев для иерархий шлюзов, в том числе разрешить устройствам более низкого уровня получать образы контейнеров или отправлять большие двоичные объекты в хранилище. Настройка правил прокси определяет способ маршрутизации данных. В этой статье обсуждаются некоторые распространенные параметры конфигурации.

## <a name="deploy-the-proxy-module"></a>Развертывание модуля прокси-сервера

Модуль прокси API можно найти в реестре контейнеров Microsoft (мкр): `mcr.microsoft.com/azureiotedge-api-proxy:latest` .

Вы также можете развернуть модуль прокси API непосредственно из Azure Marketplace: [IOT Edge API-прокси](https://azuremarketplace.microsoft.com/marketplace/apps/azure-iot.azureiotedge-api-proxy?tab=Overview).

## <a name="understand-the-proxy-module"></a>Общие сведения о модуле прокси-сервера

Прокси-модуль API использует обратный прокси-сервер Nginx для маршрутизации данных через уровни сети. Прокси-сервер внедряется в модуль, что означает, что образ модуля должен поддерживать конфигурацию прокси-сервера. Например, если прокси-сервер прослушивает определенный порт, модуль должен открыть этот порт.

Прокси-сервер начинается с файла конфигурации по умолчанию, внедренного в модуль. Вы можете передать новую конфигурацию в модуль из облака, используя его [модуль двойника](../iot-hub/iot-hub-devguide-module-twins.md). Кроме того, можно использовать переменные среды для включения или отключения параметров конфигурации во время развертывания.

Эта статья сначала посвящена файлу конфигурации по умолчанию и использованию переменных среды для включения его параметров. Затем мы обсудим настройку файла конфигурации в конце.

### <a name="default-configuration"></a>Конфигурация по умолчанию

Модуль прокси API поставляется с конфигурацией по умолчанию, которая поддерживает распространенные сценарии и позволяет настраивать. Можно управлять конфигурацией по умолчанию с помощью переменных среды модуля.

В настоящее время переменные среды по умолчанию включают в себя:

| Переменная среды | Описание |
| -------------------- | ----------- |
| `PROXY_CONFIG_ENV_VAR_LIST` | Перечислите все переменные, которые необходимо обновить, в списке с разделителями-запятыми. Этот шаг предотвращает случайное изменение неправильных параметров конфигурации.
| `NGINX_DEFAULT_PORT` | Изменяет порт, который прослушивает прокси-сервер nginx. Если вы обновляете эту переменную среды, убедитесь, что выбранный порт также доступен в модуле dockerfile и объявлен как привязка порта в манифесте развертывания.<br><br>Значение по умолчанию — 443.<br><br>При развертывании из Azure Marketplace порт по умолчанию обновляется до 8000, чтобы избежать конфликтов с модулем edgeHub. Дополнительные сведения см. в разделе [Minimize Open Ports](#minimize-open-ports). |
| `DOCKER_REQUEST_ROUTE_ADDRESS` | Адрес для маршрутизации запросов DOCKER. Измените эту переменную на устройстве верхнего уровня, чтобы она указывала на модуль реестра.<br><br>По умолчанию используется родительское имя узла. |
| `BLOB_UPLOAD_ROUTE_ADDRESS` | Адрес для маршрутизации запросов реестра больших двоичных объектов. Измените эту переменную на устройстве верхнего уровня, чтобы она указывала на модуль хранилища BLOB-объектов.<br><br>По умолчанию используется родительское имя узла. |

## <a name="minimize-open-ports"></a>Уменьшение числа открытых портов

Чтобы максимально ограничить количество открытых портов, модуль прокси API должен ретранслировать весь трафик HTTPS (порт 443), включая трафик, предназначенный для модуля edgeHub. Прокси-модуль API настроен по умолчанию для повторной маршрутизации всего трафика edgeHub через порт 443.

Чтобы настроить развертывание для сворачивания открытых портов, выполните следующие действия.

1. Обновите параметры модуля edgeHub, чтобы не выполнять привязку к порту 443, в противном случае произойдет конфликт привязки портов. По умолчанию модуль edgeHub привязывается к портам 443, 5671 и 8883. Удалите привязку порта 443 и оставьте два других на месте:

   ```json
   {
     "HostConfig": {
       "PortBindings": {
         "5671/tcp": [
           {
             "HostPort": "5671"
           }
         ],
         "8883/tcp": [
           {
             "HostPort": "8883"
           }
         ]
       }
     }
   }
   ```

1. Настройте модуль прокси API для привязки к порту 443.

   1. Задайте для переменной среды **NGINX_DEFAULT_PORT** значение `443` .
   1. Обновите параметры создания контейнера для привязки к порту 443.

      ```json
      {
        "HostConfig": {
          "PortBindings": {
            "443/tcp": [
              {
                "HostPort": "443"
              }
            ]
          }
        }
      }
      ```

Если не нужно сокращать открытые порты, можно разрешить модулю edgeHub использовать порт 443 и настроить модуль прокси API для прослушивания другого порта. Например, модуль прокси API может прослушивать порт 8000, присвоив переменной среды **NGINX_DEFAULT_PORT** значение `8000` и создав привязку порта для порта 8000.

## <a name="enable-container-image-download"></a>Включить скачивание образа контейнера

Распространенным вариантом использования прокси-модуля API является включение IoT Edge устройств на более низких уровнях для извлечения образов контейнеров. В этом сценарии используется [модуль реестра DOCKER](https://hub.docker.com/_/registry) для получения образов контейнеров из облака и их кэширования на верхнем уровне. Прокси API передает все HTTPS-запросы для скачивания образа контейнера из нижних слоев, которые должны обслуживаться модулем реестра на верхнем уровне.

В этом сценарии требуется, чтобы нижестоящие IoT Edge устройства указывали на имя домена, `$upstream` за которым следует номер порта прокси-сервера API вместо реестра контейнеров образа. Например, `$upstream:8000/azureiotedge-api-proxy:1.0`.

Этот вариант использования демонстрируется в руководстве [Создание иерархии IOT Edge устройств с помощью шлюзов](tutorial-nested-iot-edge.md).

Настройте следующие модули на **верхнем уровне**:

* Модуль реестра DOCKER
  * Настройте модуль с запоминающим именем, например *реестром* , и предоставьте порт в модуле для получения запросов.
  * Настройте модуль для соответствия реестру контейнеров.
* Модуль прокси API
  * Настройте следующие переменные среды:

    | Имя | Значение |
    | ---- | ----- |
    | `DOCKER_REQUEST_ROUTE_ADDRESS` | Имя модуля реестра и открытый порт. Например, `registry:5000`. |
    | `NGINX_DEFAULT_PORT` | Порт, прослушиваемый прокси-сервером nginx для запросов от подчиненных устройств. Например, `8000`. |

  * Настройте следующие Креатеоптионс:

    ```json
    {
       "HostConfig": {
          "PortBindings": {
             "8000/tcp": [
                {
                   "HostPort": "8000"
                }
             ]
          }
       }
    }
    ```

Настройте следующий модуль на любом **более низком уровне** для этого сценария:

* Модуль прокси API
  * Настройте следующие переменные среды:

    | Имя | Значение |
    | ---- | ----- |
    | `NGINX_DEFAULT_PORT` | Порт, прослушиваемый прокси-сервером nginx для запросов от подчиненных устройств. Например, `8000`. |

  * Настройте следующие Креатеоптионс:

    ```json
    {
       "HostConfig": {
          "PortBindings": {
             "8000/tcp": [
                {
                   "HostPort": "8000"
                }
             ]
          }
       }
    }
    ```  

## <a name="enable-blob-upload"></a>Включить передачу BLOB-объектов

Другим вариантом использования прокси-модуля API является включение IoT Edge устройств на более низких уровнях для передачи больших двоичных объектов. Этот вариант использования позволяет устранять неполадки на устройствах более низкого уровня, например при передаче журналов модулей или при отправке пакета поддержки.

В этом сценарии используется [хранилище BLOB-объектов Azure в модуле IOT Edge](https://azuremarketplace.microsoft.com/marketplace/apps/azure-blob-storage.edge-azure-blob-storage) на верхнем уровне для управления созданием и передачей больших двоичных объектов.

Настройте следующие модули на **верхнем уровне**:

* Хранилище BLOB-объектов Azure в модуле IoT Edge.
* Модуль прокси API
  * Настройте следующие переменные среды:

    | Имя | Значение |
    | ---- | ----- |
    | `BLOB_UPLOAD_ROUTE_ADDRESS` | Имя модуля хранилища BLOB-объектов и открытый порт. Например, `azureblobstorageoniotedge:1102`. |
    | `NGINX_DEFAULT_PORT` | Порт, прослушиваемый прокси-сервером nginx для запросов от подчиненных устройств. Например, `8000`. |

  * Настройте следующие Креатеоптионс:

    ```json
    {
       "HostConfig": {
          "PortBindings": {
             "8000/tcp": [
                {
                   "HostPort": "8000"
                }
             ]
          }
       }
    }
    ```

Настройте следующий модуль на любом **более низком уровне** для этого сценария:

* Модуль прокси API
  * Настройте следующие переменные среды:

    | Имя | Значение |
    | ---- | ----- |
    | `NGINX_DEFAULT_PORT` | Порт, прослушиваемый прокси-сервером nginx для запросов от подчиненных устройств. Например, `8000`. |

  * Настройте следующие Креатеоптионс:

    ```json
    {
       "HostConfig": {
          "PortBindings": {
             "8000/tcp": [
                {
                   "HostPort": "8000"
                }
             ]
          }
       }
    }
    ```

Выполните следующие действия, чтобы отправить пакет поддержки или файл журнала в модуль хранилища BLOB-объектов, расположенный на верхнем уровне.

1. Создайте контейнер больших двоичных объектов, используя либо Обозреватель службы хранилища Azure, либо интерфейсы API-интерфейсов. Дополнительные сведения см. [в статье хранение данных на границе с помощью хранилища BLOB-объектов Azure на IOT Edge](how-to-store-data-blob.md).
1. Отправьте запрос в журнал или пакет поддержки в соответствии с инструкциями в статье [Извлечение журналов из IOT Edge развертываний](how-to-retrieve-iot-edge-logs.md), но используйте имя домена `$upstream` и порт открытого прокси вместо адреса модуля хранилища BLOB-объектов. Пример:

   ```json
   {
      "schemaVersion": "1.0",
      "sasUrl": "https://$upstream:8000/myBlobStorageName/myContainerName?SAS_key",
      "since": "2d",
      "until": "1d",
      "edgeRuntimeOnly": false
   }
   ```

## <a name="edit-the-proxy-configuration"></a>Изменение конфигурации прокси-сервера

Файл конфигурации по умолчанию внедряется в модуль прокси API, но вы можете передать новую конфигурацию в модуль через облако с помощью модуля двойника.

При написании собственной конфигурации можно по-прежнему использовать среду для настройки параметров для каждого развертывания. Используйте следующий синтаксис:

* Используйте `${MY_ENVIRONMENT_VARIABLE}` для получения значения переменной среды.
* Используйте условные операторы, чтобы включить или отключить параметры в зависимости от значения переменной среды:

   ```conf
   #if_tag ${MY_ENVIRONMENT_VARIABLE}
       statement to execute if environment variable evaluates to 1
   #endif_tag ${MY_ENVIRONMENT_VARIABLE}

   #if_tag !${MY_ENVIRONMENT_VARIABLE}
       statement to execute if environment variable evaluates to 0
   #endif_tag !${MY_ENVIRONMENT_VARIABLE}
   ```

Когда модуль прокси API анализирует конфигурацию прокси-сервера, он сначала заменяет все переменные среды, перечисленные в, на `PROXY_CONFIG_ENV_VAR_LIST` их значения с помощью подстановки. Затем `#if_tag` `#endif_tag` заменяется все между парой и. Затем модуль предоставляет проанализированную конфигурацию для обратных прокси-серверов nginx.

Чтобы динамически обновить конфигурацию прокси-сервера, выполните следующие действия.

1. Запишите файл конфигурации. Этот шаблон по умолчанию можно использовать в качестве ссылки: [nginx_default_config. conf](https://github.com/Azure/iotedge/blob/master/edge-modules/api-proxy-module/templates/nginx_default_config.conf)
1. Скопируйте текст файла конфигурации и преобразуйте его в Base64.
1. Вставьте закодированный файл конфигурации в качестве значения `proxy_config` требуемого свойства в модуль двойника.

   ![Вставить закодированный файл конфигурации как значение свойства proxy_config](./media/how-to-configure-api-proxy-module/change-config.png)

## <a name="next-steps"></a>Следующие шаги

Используйте прокси-модуль API для [подключения подчиненного IOT Edge устройства к шлюзу Azure IOT Edge](how-to-connect-downstream-iot-edge-device.md).