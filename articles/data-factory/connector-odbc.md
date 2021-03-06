---
title: Копирование данных из хранилищ данных ODBC и обратно с помощью фабрики данных Azure
description: Узнайте, как копировать данные из хранилищ данных ODBC и в них с помощью действия копирования в конвейере фабрики данных Azure.
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: jingwang
ms.openlocfilehash: 9b73e10b0ed539879e9a32d3961b6375828cc153
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "100389626"
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Копирование данных из хранилищ данных ODBC и обратно с помощью фабрики данных Azure
> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](v1/data-factory-odbc-connector.md)
> * [Текущая версия](connector-odbc.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этой статье описывается, как с помощью действия копирования в фабрике данных Azure копировать данные в хранилище данных ODBC и из него. Это продолжение [статьи об обзоре действия копирования](copy-activity-overview.md), в которой представлены общие сведения о действии копирования.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Этот соединитель ODBC поддерживается для следующих действий:

- [Действие копирования](copy-activity-overview.md) с использованием [матрицы поддерживаемых источников и приемников](copy-activity-overview.md)
- [Действие поиска](control-flow-lookup-activity.md)

Данные можно копировать из источника ODBC в любой поддерживаемый приемник данных или из любого поддерживаемого приемника ODBC. Список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования, приведен в таблице [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

В частности, этот соединитель ODBC поддерживает копирование данных из **любого хранилища данных, совместимого с ODBC**, и в него с использованием **базовой** или **анонимной** проверки подлинности. Требуется **64-разрядный драйвер ODBC** . Для приемника ODBC интерфейс ADF поддерживает стандарт ODBC версии 2,0.

## <a name="prerequisites"></a>Предварительные условия

Чтобы использовать этот соединитель ODBC, сделайте следующее:

- Настроить локальную среду выполнения интеграции. Дополнительные сведения см. в статье [Создание и настройка локальной среды выполнения интеграции](create-self-hosted-integration-runtime.md).
- Установите 64-разрядный драйвер ODBC для хранилища данных на компьютере Integration Runtime.

## <a name="getting-started"></a>Начало работы

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах JSON, которые используются для определения сущностей фабрики данных, относящихся к соединителю ODBC.

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы ODBC поддерживаются следующие свойства:

| Свойство. | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Для свойства type необходимо задать значение **Odbc** | Да |
| connectionString | Строка подключения, исключающая учетные данные. Можно указать строку подключения вида `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"` или использовать системное имя источника данных (DSN), которое вы настроили на компьютере Integration Runtime с помощью `"DSN=<name of the DSN on IR machine>;"` (вы все равно должны указать соответствующие учетные данные в связанной службе).<br>Можно также добавить пароль в Azure Key Vault и извлечь `password` конфигурацию из строки подключения. Дополнительные сведения см. в разделе [хранение учетных данных в Azure Key Vault](store-credentials-in-key-vault.md) .| Да |
| authenticationType | Тип проверки подлинности, используемый для подключения к хранилищу данных ODBC.<br/>Допустимые значения: **базовый** и **Анонимный**. | Да |
| userName | При использовании обычной проверки подлинности укажите имя пользователя. | Нет |
| password | Введите пароль для учетной записи пользователя, указанной для выбранного имени пользователя. Пометьте это поле как SecureString, чтобы безопасно хранить его в фабрике данных, или [добавьте ссылку на секрет, хранящийся в Azure Key Vault](store-credentials-in-key-vault.md). | нет |
| учетные данные | Учетные данные в строке подключения, используемые для получения доступа и указанные в формате "драйвер-определенное свойство-значение". Например, `"RefreshToken=<secret refresh token>;"`. Пометьте это поле в качестве SecureString. | Нет |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Требуется локальная среда IR, как упоминалось в разделе [Предварительные требования](#prerequisites). |Да |

**Пример 1. Использование обычной проверки подлинности**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример 2. Использование анонимной проверки подлинности**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
            "authenticationType": "Anonymous",
            "credential": {
                "type": "SecureString",
                "value": "RefreshToken=<secret refresh token>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, доступных для определения наборов данных, см. в статье о [наборах данных](concepts-datasets-linked-services.md). В этом разделе содержится список свойств, поддерживаемых набором данных ODBC.

Чтобы скопировать данные из хранилища данных, совместимого с ODBC, или в него, поддерживаются следующие свойства:

| Свойство. | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Свойство Type набора данных должно иметь значение **одбктабле** . | Да |
| tableName | Имя таблицы в хранилище данных ODBC. | Нет для источника (если свойство query указано в источнике действия).<br/>Да для приемника. |

**Пример**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "OdbcTable",
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<ODBC linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

Если вы ранее использовали типизированный набор данных `RelationalTable`, он пока поддерживается и не требует изменений, но мы рекомендуем при любом удобном случае перейти на новую версию.

## <a name="copy-activity-properties"></a>Свойства действия копирования

Полный список разделов и свойств, используемых для определения действий, см. в статье [Конвейеры и действия в фабрике данных Azure](concepts-pipelines-activities.md). В этом разделе содержится список свойств, поддерживаемых источником ODBC.

### <a name="odbc-as-source"></a>ODBC в качестве источника

Чтобы скопировать данные из хранилища данных, совместимого с ODBC, в разделе **источник** действия копирования поддерживаются следующие свойства.

| Свойство. | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Свойство Type источника действия копирования должно иметь значение **одбксаурце** . | Да |
| query | Используйте пользовательский SQL-запрос для чтения данных. Например: `"SELECT * FROM MyTable"`. | Нет (если для набора данных задано свойство tableName) |

**Пример**.

```json
"activities":[
    {
        "name": "CopyFromODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ODBC input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OdbcSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Если вы ранее использовали типизированный источник `RelationalSource`, он пока поддерживается и не требует изменений, но мы рекомендуем в дальнейшем использовать более новую версию.

### <a name="odbc-as-sink"></a>ODBC в качестве приемника

Чтобы скопировать данные в хранилище данных, совместимое с ODBC, установите тип приемника в действии копирования **OdbcSink**. В разделе **sink** действия копирования поддерживаются следующие свойства:

| Свойство | Описание | Обязательно |
|:--- |:--- |:--- |
| type | Свойство type приемника действия копирования должно иметь значение **OdbcSink**. | Да |
| writeBatchTimeout |Время ожидания до выполнения операции пакетной вставки, пока не завершится срок ее действия.<br/>Допустимые значения: промежуток времени. Пример "00:30:00" (30 минут). |Нет |
| writeBatchSize |Вставляет данные в таблицу SQL, когда размер буфера достигает значения writeBatchSize.<br/>Допустимые значения: целое число (количество строк). |Нет (по умолчанию — 0 (автоматическое обнаружение)) |
| preCopyScript |Перед записью данных в хранилище данных при каждом запуске указывайте SQL-запрос для выполнения операции копирования. Это свойство можно использовать для очистки предварительно загруженных данных. |Нет |

> [!NOTE]
> Для "writeBatchSize", если он не задан (автоматически обнаружен), действие копирования сначала определяет, поддерживает ли драйвер пакетные операции, и установите для него значение 10000, если это не так. Если явно задано значение, отличное от 0, действие копирования учитывает это значение и завершается ошибкой во время выполнения, если драйвер не поддерживает пакетные операции.

**Пример**.

```json
"activities":[
    {
        "name": "CopyToODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ODBC output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OdbcSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Свойства действия поиска

Подробные сведения об этих свойствах см. в разделе [Действие поиска](control-flow-lookup-activity.md).

## <a name="troubleshoot-connectivity-issues"></a>Устранение проблем подключения

Для устранения проблем подключения используйте вкладку **Диагностика****диспетчера конфигурации для среды выполнения интеграции**.

1. Запустите **диспетчер конфигурации для среды выполнения интеграции**.
2. Перейдите на вкладку **Диагностика** .
3. В разделе "Проверить подключение" выберите **тип** хранилища данных (связанную службу).
4. Укажите **строку подключения** для подключения к хранилищу данных, выберите **проверку подлинности** и введите **имя пользователя**, **пароль** и/или **учетные данные**.
5. Щелкните **Проверить подключение** , чтобы проверить подключение к хранилищу данных.

## <a name="next-steps"></a>Дальнейшие действия
В таблице [Поддерживаемые хранилища данных](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в фабрике данных Azure.
