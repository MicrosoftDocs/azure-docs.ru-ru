---
title: Поддерживаемые хранилища данных в Azure Data Share
description: Сведения о хранилищах данных, которые поддерживаются для использования в общем ресурсе данных Azure.
ms.service: data-share
author: jifems
ms.author: jife
ms.topic: conceptual
ms.date: 12/16/2020
ms.openlocfilehash: 852c44f5edc5c0b0f5f655f63ab040927bd9bc7b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "97963685"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Поддерживаемые хранилища данных в Azure Data Share

Общая папка данных Azure предоставляет доступ к открытым и гибким данным, включая возможность совместного использования с различными хранилищами данных. Поставщики данных могут обмениваться данными из одного типа хранилища данных, и потребители данных могут выбрать хранилище данных для получения данных. 

В этой статье вы узнаете о обширном наборе хранилищ данных Azure, поддерживаемых общим ресурсом данных Azure. Вы также узнаете, как поставщики данных и потребители данных могут комбинировать различные хранилища данных. 

## <a name="supported-data-stores"></a>Поддерживаемые хранилища данных 

В следующей таблице описаны хранилища данных, поддерживаемые общим ресурсом данных Azure. 

| Хранилище данных | Общий доступ на основе полных моментальных снимков | Совместное использование на основе добавочных моментальных снимков | Общий доступ на месте 
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| хранилище BLOB-объектов Azure |✓ |✓ | |
| Хранилище Azure Data Lake Storage 1-го поколения |✓ |✓ | |
| Azure Data Lake Storage 2-го поколения |✓ |✓ ||
| База данных SQL Azure |✓ | | |
| Azure Synapse Analytics (прежнее название: Хранилище данных SQL Azure) |✓ | | |
| Выделенный пул SQL Azure синапсе Analytics (Рабочая область) |✓ | | |
| Обозреватель данных Azure | | |✓ |

## <a name="data-store-support-matrix"></a>Матрица поддержки хранилища данных

Azure Data Share позволяет потребителям данных выбирать хранилище данных для приема данных. Например, данные, общие для базы данных SQL Azure, можно получить в Azure Data Lake Storage 2-го поколения, базе данных SQL Azure или Azure синапсе Analytics. Когда клиенты настроили полученный общий ресурс данных, они могут выбрать формат для получения данных. 

В следующей таблице описаны сочетания и параметры, которые потребители данных могут выбирать при принятии и настройке общей папки данных. Дополнительные сведения см. [в разделе Настройка сопоставления набора данных](how-to-configure-mapping.md).

| Хранилище данных | Хранилище BLOB-объектов | Azure Data Lake Storage 1-го поколения | Data Lake Storage 2-го поколения | База данных SQL | Аналитика синапсе (ранее — хранилище данных SQL) | Выделенный пул SQL синапсе Analytics (Рабочая область) | Data Explorer
|:--- |:--- |:--- |:--- |:--- |:--- |:--- | :--- |
| Хранилище BLOB-объектов | ✓ || ✓ |||
| Azure Data Lake Storage 1-го поколения | ✓ | | ✓ |||
| Data Lake Storage 2-го поколения | ✓ | | ✓ |||
| База данных SQL | ✓ | | ✓ | ✓ | ✓ | ✓ ||
| Аналитика синапсе (ранее — хранилище данных SQL) | ✓ | | ✓ | ✓ | ✓ | ✓ ||
| Выделенный пул SQL синапсе Analytics (Рабочая область) | ✓ | | ✓ | ✓ | ✓ | ✓ ||
| Data Explorer ||||||| ✓ |

## <a name="share-from-a-storage-account"></a>Предоставление общего доступа к данным из учетной записи хранения
Общая папка данных Azure поддерживает общий доступ к файлам, папкам и файловым системам Azure Data Lake Storage 1-го поколения и Azure Data Lake Storage 2-го поколения. Он также поддерживает общий доступ к BLOB-объектам, папкам и контейнерам из хранилища BLOB-объектов Azure. В настоящее время поддерживаются только блочные BLOB-объекты. 

Если файловые системы, контейнеры или папки используются совместно с общим доступом на основе моментальных снимков, потребители данных могут создать полную копию общих данных. Или же они могут использовать функцию добавочного моментального снимка для копирования только новых файлов или обновленных файлов. 

Добавочный моментальный снимок основан на времени последнего изменения файлов. Существующие файлы, имена которых совпадают с именами файлов в полученных данных, перезаписываются в моментальном снимке. Файлы, удаленные из источника, не удаляются на целевом объекте. 

Дополнительные сведения см. [в статье общий доступ и получение данных из хранилища BLOB-объектов Azure и Azure Data Lake Storage](how-to-share-from-storage.md).

## <a name="share-from-a-sql-based-source"></a>Предоставление общего доступа к данным из источника на основе SQL
Общая папка данных Azure поддерживает общий доступ к таблицам и представлениям из базы данных SQL Azure и Azure синапсе Analytics (ранее — хранилище данных SQL Azure). Он поддерживает общий доступ к таблицам из выделенного пула SQL Azure синапсе Analytics (Рабочая область). Совместное использование несинапсеного пула SQL в Azure (Рабочая область) в настоящее время не поддерживается. 

Потребители данных могут принять данные в Azure Data Lake Storage 2-го поколения или хранилище больших двоичных объектов Azure в виде CSV-файла или файла Parquet. Они также могут принимать данные в виде таблиц в базу данных SQL Azure и Azure синапсе Analytics.

Когда потребители принимают данные в Azure Data Lake Storage 2-го поколения или хранилище BLOB-объектов Azure, полные моментальные снимки перезаписывают содержимое целевого файла, если файл уже существует. Если данные получены в таблицу, а Целевая таблица еще не существует, то общая папка данных Azure создает таблицу SQL с помощью исходной схемы. Если целевая таблица уже существует и имеет то же имя, она удаляется и перезаписывается последним полным моментальным снимком. Добавочные моментальные снимки в настоящее время не поддерживаются.

Дополнительные сведения см. [в статье общий доступ и получение данных из базы данных SQL Azure и Azure синапсе Analytics](how-to-share-from-sql.md).

## <a name="share-from-data-explorer"></a>Общий доступ из обозреватель данных
Общий ресурс данных Azure поддерживает возможность совместного использования баз данных на месте из кластеров Azure обозреватель данных. Поставщик данных может совместно использоваться на уровне базы данных или кластера. 

Если данные являются общими на уровне базы данных, потребители данных могут обращаться только к тем базам данных, которые являются общими для поставщика данных. Когда поставщик использует данные совместно на уровне кластера, потребители данных могут получить доступ ко всем базам данных из кластера поставщика, включая любые будущие базы данных, создаваемые поставщиком данных.

Для доступа к общим базам данных потребителям данных требуется собственный кластер Azure обозреватель данных. Их кластер должен находиться в том же центре обработки данных Azure, что и кластер Azure обозреватель данных. 

Если установлено отношение общего доступа, Общая папка данных Azure создает символьную ссылку между кластером поставщика и кластером потребителя. Данные, получаемые в исходном кластере с помощью пакетного режима, отображаются в целевом кластере в течение нескольких минут.

Дополнительные сведения см. [в статье общий доступ и получение данных из Azure обозреватель данных](/azure/data-explorer/data-share). 

## <a name="next-steps"></a>Дальнейшие действия

Чтобы узнать, как начать совместное использование данных, перейдите к руководству по [совместному использованию данных](share-your-data.md) .
