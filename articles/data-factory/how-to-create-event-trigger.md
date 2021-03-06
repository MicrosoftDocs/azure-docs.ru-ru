---
title: Создание триггеров на основе событий в Фабрике данных Azure
description: Узнайте, как создать в Фабрике данных Azure триггер, который запускает конвейер в ответ на событие.
ms.service: data-factory
author: chez-charlie
ms.author: chez
ms.reviewer: jburchel
ms.topic: conceptual
ms.date: 03/11/2021
ms.openlocfilehash: ae8b1eab81e3c898c25a613f552a49c8de64f49d
ms.sourcegitcommit: a67b972d655a5a2d5e909faa2ea0911912f6a828
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104889133"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-a-storage-event"></a>Создание триггера, запускающего конвейер в ответ на событие хранилища

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этой статье описываются триггеры событий хранилища, которые можно создать в конвейерах фабрики данных.

Управляемая событиями архитектура (EDA) — это общий шаблон интеграции данных, включающий в себя производство, обнаружение, потребление и реагирование на события. В сценариях интеграции данных часто требуется, чтобы клиенты фабрики данных инициировали конвейеры на основе событий, происходящих в учетной записи хранения, таких как получение или удаление файла в учетной записи хранилища BLOB-объектов Azure. Фабрика данных изначально интегрируется со службой " [Сетка событий Azure](https://azure.microsoft.com/services/event-grid/)", что позволяет запускать конвейеры на таких событиях.

Уделите 10 минут своего времени, чтобы просмотреть следующее видео с кратким обзором и демонстрацией этой функции:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]

> [!NOTE]
> Интеграция, описанная в этой статье, зависит от службы [Сетка событий Azure](https://azure.microsoft.com/services/event-grid/). Убедитесь, что ваша подписка зарегистрирована у поставщика ресурсов "Сетка событий". См. дополнительные сведения о [поставщиках и типах ресурсов](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal). Необходимо иметь возможность выполнить действие *Microsoft. EventGrid/eventSubscriptions/**. Это действие является частью встроенной роли участника EventGrid подписки.

## <a name="data-factory-ui"></a>Пользовательский интерфейс Фабрики данных

В этом разделе показано, как создать триггер событий хранилища в пользовательском интерфейсе фабрики данных Azure.

1. Перейдите на вкладку **Правка** , показанную значком карандаша.

1. Выберите **триггер** в меню, а затем выберите **создать/изменить**.

1. На странице **Добавление триггеров** выберите **выбрать триггер...**, а затем выберите **+ создать**.

1. Выберите **событие хранилища** типов триггеров

    :::image type="content" source="media/how-to-create-event-trigger/event-based-trigger-image1.png" alt-text="Снимок экрана страницы &quot;Автор&quot; для создания нового триггера событий хранилища в пользовательском интерфейсе фабрики данных.":::

1. Выберите свою учетную запись хранения в раскрывающемся списке подписки Azure или укажите ее вручную с помощью идентификатора ресурса учетной записи хранения. Выберите контейнер, в котором должны выполняться события. Выбор контейнера является обязательным, но учитывать, что выбор всех контейнеров может привести к большому числу событий.

   > [!NOTE]
   > Сейчас триггер событий хранилища поддерживает только учетные записи хранения общего назначения (Azure Data Lake Storage 2-го поколения) и универсальной версии 2. В связи с ограничением сетки событий Azure фабрика данных Azure поддерживает не более 500 триггеров событий хранилища на учетную запись хранения. При достижении предельного значения обратитесь в службу поддержки за рекомендациями и увеличьте предельное значение для команды "Сетка событий". 

   > [!NOTE]
   > Чтобы создать новый или изменить существующий триггер события хранилища, учетная запись Azure, используемая для входа в фабрику данных и опубликуйте триггер событий хранилища, должна иметь соответствующее разрешение на управление доступом на основе ролей (Azure RBAC) в учетной записи хранения. Никаких дополнительных разрешений не требуется: субъекту-службе для фабрики данных Azure _не_ требуется специальное разрешение для учетной записи хранения или службы "Сетка событий". Дополнительные сведения об управлении доступом см. в разделе [Управление доступом на основе ролей](#role-based-access-control) .

1. С помощью свойств **Blob path begins with** (Путь большого двоичного объекта начинается с) и **Blob path ends with** (Путь большого двоичного объекта заканчивается на) можно задать названия контейнеров, папок и больших двоичных объектов, для которых необходимо получать события. Для триггера события хранилища необходимо определить хотя бы одно из этих свойств. Вы можете использовать различные шаблоны для свойств **Blob path begins with** (Путь большого двоичного объекта начинается с) и **Blob path ends with** (Путь большого двоичного объекта заканчивается на), как показано в примерах далее в этой статье.

    * **Путь большого двоичного объекта начинается с.** Путь к большому двоичному объекту должен начинаться с пути к папке. Допустимые значения включают `2018/` и `2018/april/shoes.csv`. Это поле нельзя выбрать, если контейнер не выбран.
    * **Путь большого двоичного объекта оканчивается на.** Путь к большому двоичному объекту должен заканчиваться именем файла или расширением. Допустимые значения включают `shoes.csv` и `.csv`. Имена контейнеров и папок, если они указаны, они должны быть разделены `/blobs/` сегментом. Например, контейнер с именем "orders" может иметь значение `/orders/blobs/2018/april/shoes.csv`. Чтобы указать папку в любом контейнере, опустите начальный символ "/". Например, `april/shoes.csv` запустит событие для любого файла с именем `shoes.csv` в папке с именем "april" в любом контейнере.
    * Обратите внимание, что путь к BLOB-объекту **начинается с** и **заканчивается** на — единственным сопоставлением шаблонов, разрешенным в триггере событий хранилища. Другие типы сопоставления с подстановочными знаками не поддерживаются для типа триггера.

1. Укажите, будет ли триггер отвечать на событие **создания BLOB-объекта**, **удаления BLOB-объекта** или на оба эти события. В указанном месте хранения каждое событие запустит конвейеры Фабрики данных, связанные с этим триггером.

    :::image type="content" source="media/how-to-create-event-trigger/event-based-trigger-image2.png" alt-text="Снимок экрана со страницей создания триггера событий хранилища.":::

1. Выберите, будет ли триггер игнорировать большие двоичные объекты с нулевым числом байтов.

1. После настройки триггера нажмите кнопку **Далее: предварительный просмотр данных**. На этом экране показаны существующие большие двоичные объекты, соответствующие конфигурации триггера событий хранилища. Убедитесь, что у вас есть нужные фильтры. Настройка слишком широких фильтров может привести к выбору большого количества созданных и удаленных файлов, что может значительно повлиять на стоимость. После проверки условий фильтра нажмите кнопку **Готово**.

    :::image type="content" source="media/how-to-create-event-trigger/event-based-trigger-image3.png" alt-text="Снимок экрана: страница предварительного просмотра триггера событий хранилища.":::

1. Чтобы подключить конвейер к этому триггеру, перейдите на холст конвейера и щелкните **триггер** и выберите **создать/изменить**. Когда появится боковая панель навигации, щелкните **Выберите триггер...** и выберите созданный триггер. Щелкните **Далее: просмотр данных**, чтобы подтвердить правильность конфигурации, а затем — **Далее**, чтобы проверить правильность этих данных.

1. Если у конвейера есть параметры, их можно указать в параметрах запуска триггера на боковой панели навигации. Триггер события хранилища захватывает путь к папке и имя файла большого двоичного объекта в свойствах `@triggerBody().folderPath` и `@triggerBody().fileName` . Чтобы использовать значения этих свойств в конвейере, необходимо сопоставить эти свойства с параметрами конвейера. После сопоставления свойств с параметрами можно получить доступ к значениям, записанным с помощью триггера, через выражение `@pipeline().parameters.parameterName` в конвейере. Подробное описание см. [в разделе Метаданные триггера Reference в конвейерах](how-to-use-trigger-parameterization.md) .

    :::image type="content" source="media/how-to-create-event-trigger/event-based-trigger-image4.png" alt-text="Снимок экрана: свойства сопоставления триггера события хранилища с параметрами конвейера.":::

    В предыдущем примере триггер настроен на срабатывание при создании пути BLOB-объекта, завершающегося в файле. csv, в папке Sample- _testing_ _— Data_. С помощью свойств **folderPath** и **fileName** записывается расположение нового большого двоичного объекта. Например, когда к пути sample-data/event-testing добавляется строка MoviesDB.csv, `@triggerBody().folderPath` получает значение `sample-data/event-testing`, а `@triggerBody().fileName` — значение `moviesDB.csv`. Эти значения сопоставлены в примере с параметрами конвейера `sourceFolder` и `sourceFile` , которые могут использоваться в конвейере как `@pipeline().parameters.sourceFolder` и `@pipeline().parameters.sourceFile` соответственно.

1. По завершении нажмите кнопку **Готово**.

## <a name="json-schema"></a>Схема JSON

В следующей таблице приведены общие сведения об элементах схемы, связанных с триггерами событий хранилища.

| **Элемент JSON** | **Описание** | **Тип** | **Допустимые значения** | **Обязательно** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **область** | Идентификатор ресурса Azure Resource Manager учетной записи хранения. | Строка | Идентификатор Azure Resource Manager | Да |
| **events** | Тип событий, вызывающих срабатывание триггера. | Array    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Да, любое сочетание этих значений. |
| **blobPathBeginsWith** | Путь большого двоичного объекта должен начинаться с шаблона, указанного для срабатывания триггера. Например, `/records/blobs/december/` активирует только триггер для больших двоичных объектов в папке `december` контейнера `records`. | Строка   | | Укажите значение хотя бы для одного из следующих свойств: `blobPathBeginsWith` или `blobPathEndsWith` . |
| **blobPathEndsWith** | Путь большого двоичного объекта должен оканчиваться шаблоном, указанным для срабатывания триггера. Например, `december/boxes.csv` активирует только триггер для больших двоичных объектов с именем `boxes` в папке `december`. | Строка   | | Вы должны предоставить значение хотя бы для одного из этих свойств: `blobPathBeginsWith` или `blobPathEndsWith`. |
| **ignoreEmptyBlobs** | Указывает, будут ли большие двоичные объекты с нулевыми байтами инициировать запуск конвейера. Значение по умолчанию для этого параметра — True. | Логическое | true или false | нет |

## <a name="examples-of-storage-event-triggers"></a>Примеры триггеров событий хранилища

В этом разделе приводятся примеры параметров триггера событий хранилища.

> [!IMPORTANT]
> Каждый раз, когда вы указываете контейнер и папку, контейнер и файл или контейнер, папку и файл, добавляйте сегмент пути `/blobs/`, как показано в следующих примерах. Для **blobPathBeginsWith** пользовательский интерфейс Фабрики данных в коде JSON триггера автоматически добавит `/blobs/` между папкой и именем контейнера.

| Свойство | Пример | Описание |
|---|---|---|
| **Путь большого двоичного объекта начинается с** | `/containername/` | Получает события для любого большого двоичного объекта в контейнере. |
| **Путь большого двоичного объекта начинается с** | `/containername/blobs/foldername/` | Получает события для любого большого двоичного объекта в контейнере `containername` и папке `foldername`. |
| **Путь большого двоичного объекта начинается с** | `/containername/blobs/foldername/subfoldername/` | Можно также указать ссылку на вложенную папку. |
| **Путь большого двоичного объекта начинается с** | `/containername/blobs/foldername/file.txt` | Получает события для большого двоичного объекта с именем `file.txt` в папке `foldername` в контейнере `containername`. |
| **Большой двоичный объект оканчивается на** | `file.txt` | Получает события для большого двоичного объекта с именем `file.txt` в любом пути. |
| **Большой двоичный объект оканчивается на** | `/containername/blobs/file.txt` | Получает события для большого двоичного объекта с именем `file.txt` в контейнере `containername`. |
| **Большой двоичный объект оканчивается на** | `foldername/file.txt` | Получает события для большого двоичного объекта с именем `file.txt` в папке `foldername` в любом контейнере. |

## <a name="role-based-access-control"></a>управление доступом на основе ролей;

Фабрика данных Azure использует управление доступом на основе ролей Azure (Azure RBAC), чтобы гарантировать, что несанкционированный доступ к прослушиванию, подписка на обновления из и активация конвейеров, связанных с событиями BLOB-объектов, строго запрещен.

* Для успешного создания нового или обновления существующего триггера событий хранилища учетная запись Azure, выполненная в фабрике данных, должна иметь соответствующий доступ к соответствующей учетной записи хранения. В противном случае операция с ошибкой с _доступом будет запрещена_.
* Фабрике данных не требуется специального разрешения для вашей службы "Сетка событий", поэтому _не_ нужно назначать особое разрешение RBAC субъекту-службе фабрики данных для операции.

Любой из следующих параметров RBAC работает для триггера событий хранилища:

* Роль владельца для учетной записи хранения
* Роль участника для учетной записи хранения
* _Microsoft. EventGrid/EventSubscriptions/_ разрешение на запись в учетную запись хранения _/Subscriptions/# # # #/resourceGroups/# # # #/провидерс/Микрософт.стораже/сторажеаккаунтс/сторажеаккаунтнаме_

Чтобы понять, как фабрика данных Azure доставляет два обещания, давайте вернемся к шагу и взглянем за сцену. Ниже приведены высокоуровневые рабочие потоки для интеграции фабрики данных, хранилища и службы "Сетка событий".

### <a name="create-a-new-storage-event-trigger"></a>Создание нового триггера событий хранилища

Этот высокоуровневый рабочий процесс описывает взаимодействие фабрики данных Azure с сеткой событий для создания триггера событий хранилища.

:::image type="content" source="media/how-to-create-event-trigger/storage-event-trigger-5-create-subscription.png" alt-text="Рабочий процесс создания триггера события хранилища.":::

Два заметных обращения из рабочих процессов:

* Фабрика данных Azure _не_ предоставляет прямого контакта с учетной записью хранения. Запрос на создание подписки выполняется, а затем ретранслируется и обрабатывается службой "Сетка событий". Таким образом, фабрике данных не требуются разрешения на учетную запись хранения для этого шага.

* Управление доступом и проверка разрешений происходят на стороне фабрики данных Azure. Перед тем как ADF отправляет запрос на событие подписки на хранилище, он проверяет разрешение для пользователя. В частности, он проверяет, имеет ли учетная запись Azure, в которой выполнен вход, и попытку создания триггера событий хранилища, имеет соответствующий доступ к соответствующей учетной записи хранения. В случае сбоя проверки разрешений также произойдет сбой создания триггера.

### <a name="storage-event-trigger-data-factory-pipeline-run"></a>Запуск конвейера фабрики данных событий хранилища

Эти высокоуровневые рабочие потоки описывают, как событие хранилища запускает конвейер через сетку событий

:::image type="content" source="media/how-to-create-event-trigger/storage-event-trigger-6-trigger-pipeline.png" alt-text="Рабочий процесс запуска конвейера событий хранилища.":::

Когда дело доходит до конвейера событий в фабрике данных, три заметных обращения в рабочем процессе:

* Сетка событий использует модель отправки, которая передает сообщение как можно быстрее, когда хранилище удаляет сообщение в систему. Это отличается от системы обмена сообщениями, например Kafka, где используется опрашивающая система.
* Триггер событий в фабрике данных Azure выступает в качестве активного прослушивателя для входящего сообщения и правильно активирует связанный конвейер.
* Сам триггер события хранилища не дает прямого контакта с учетной записью хранения.

  * С другой стороны, если у вас есть копия или другое действие в конвейере для обработки данных в учетной записи хранения, фабрика данных будет напрямую обращаться к хранилищу, используя учетные данные, хранящиеся в связанной службе. Убедитесь, что связанная служба настроена соответствующим образом.
  * Однако если в конвейере нет ссылки на учетную запись хранения, вам не нужно предоставлять разрешения фабрике данных для доступа к учетной записи хранения.

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения см. в руководстве по [выполнению конвейера и использованию триггеров](concepts-pipeline-execution-triggers.md#trigger-execution).
* Сведения о ссылках на метаданные триггера в конвейере см. в разделе [метаданные триггера справочника при выполнении конвейера](how-to-use-trigger-parameterization.md)
