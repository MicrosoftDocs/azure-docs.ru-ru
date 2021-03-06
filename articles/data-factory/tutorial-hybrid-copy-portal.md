---
title: Копирование данных из SQL Server в хранилище BLOB-объектов с помощью портала Azure
description: Узнайте, как копировать данные из локального хранилища данных в облако с помощью локальной среды выполнения интеграции в фабрике данных Azure.
ms.author: abnarain
author: nabhishek
ms.service: data-factory
ms.topic: tutorial
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 02/18/2021
ms.openlocfilehash: 4bfbd83f3f3910e1231bcce4043d9b59ccc512db
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104606656"
---
# <a name="copy-data-from-a-sql-server-database-to-azure-blob-storage"></a>Копирование из базы данных SQL Server в хранилище BLOB-объектов Azure

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этом руководстве вы создадите конвейер фабрики данных, в котором данные из базы данных SQL Server копируются в хранилище BLOB-объектов Azure, с помощью пользовательского интерфейса службы "Фабрика данных Azure". Вы создадите и будете использовать локальную среду выполнения интеграции, которая перемещает данные между локальным и облачным хранилищами данных.

> [!NOTE]
> В этой статье не содержится подробного введения в фабрику данных. Дополнительные сведения см. в статье [Введение в фабрику данных Azure](introduction.md).

Вот какие шаги выполняются в этом учебнике:

> [!div class="checklist"]
> * Создали фабрику данных.
> * Создайте локальную среду выполнения интеграции.
> * Создадите SQL Server и связанные службы Azure.
> * Создадите SQL Server и наборы данных больших двоичных объектов Azure.
> * Создадите конвейер с действием копирования для перемещения данных.
> * Запуск конвейера.
> * Осуществили мониторинг выполнения конвейера.

## <a name="prerequisites"></a>Предварительные требования
### <a name="azure-subscription"></a>Подписка Azure.
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

### <a name="azure-roles"></a>Роли Azure
Чтобы создать экземпляры фабрики данных, нужно назначить учетной записи пользователя, используемой для входа в Azure, роль *участника*, *владельца* либо *администратора* подписки Azure.

Чтобы просмотреть существующие разрешения в подписке, перейдите на портал Azure. Сначала в верхнем правом углу выберите имя пользователя, а затем — **Разрешения**. Если у вас есть доступ к нескольким подпискам, выберите соответствующую подписку. Примеры инструкций по назначению пользователю роли см. в статье [Назначение ролей Azure с помощью портала Azure](../role-based-access-control/role-assignments-portal.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 и 2017
В этом руководстве используйте базу данных SQL Server в качестве *исходного* хранилища данных. Конвейер фабрики данных, созданный в рамках этого руководства, копирует данные из этой базы данных SQL Server (источника) в хранилище BLOB-объектов (приемник). Затем создайте таблицу с именем **emp** в базе данных SQL Server и вставьте в нее несколько примеров записей.

1. Запустите среду SQL Server Management Studio. Если она еще не установлена на вашем компьютере, скачайте ее со страницы [Скачивание SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms).

1. Подключитесь к экземпляру SQL Server с помощью учетных данных.

1. Создайте пример базы данных. В представлении в виде дерева щелкните правой кнопкой мыши элемент **Базы данных** и выберите пункт **Новая база данных**.
1. В окне **Новая база данных** введите имя базы данных и нажмите кнопку **ОК**.

1. Чтобы создать таблицу **emp** и вставить в нее примеры данных, запустите приведенный ниже сценарий запроса к базе данных. В представлении в виде дерева щелкните правой кнопкой мыши созданную базу данных и выберите пункт **Новый запрос**.

   ```
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
   ```

### <a name="azure-storage-account"></a>Учетная запись хранения Azure.
В этом руководстве используйте учетную запись хранения Azure общего назначения (хранилища BLOB-объектов) в качестве целевого (принимающего) хранилища данных. Если у вас нет учетной записи хранения Azure общего назначения, см. инструкции по [созданию учетной записи хранения](../storage/common/storage-account-create.md). Конвейер фабрики данных, созданный в рамках этого руководства, копирует данные из базы данных SQL Server (источника) в хранилище BLOB-объектов (приемник). 

#### <a name="get-the-storage-account-name-and-account-key"></a>Получение имени и ключа учетной записи хранения и ключа учетной записи
В этом руководстве вы будете использовать имя и ключ своей учетной записи хранения. Чтобы получить имя и ключ учетной записи хранения, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com), используя имя пользователя и пароль Azure.

1. На панели слева выберите **Все службы**. Отфильтруйте содержимое по ключевому слову **хранение**, а затем выберите **Учетные записи хранения**.

    ![Поиск учетной записи хранения](media/doc-common-process/search-storage-account.png)

1. В списке учетных записей хранения найдите с помощью фильтра свою учетную запись хранения (при необходимости). Затем выберите свою учетную запись хранения.

1. В окне **Учетная запись хранения** выберите параметр **Ключи доступа**.

1. Скопируйте значения полей **Имя учетной записи хранения** и **key1**. Затем вставьте их в Блокнот или другой редактор для дальнейшего использования в руководстве.

#### <a name="create-the-adftutorial-container"></a>Создание контейнера adftutorial
В этом разделе вы создадите контейнер больших двоичных объектов с именем **adftutorial** в хранилище BLOB-объектов.

1. В окне **Учетная запись хранения** перейдите на вкладку **Обзор** и выберите **Контейнеры**.

    ![Выбор параметра "BLOB-объекты"](media/tutorial-hybrid-copy-powershell/select-blobs.png)

1. В окне **Контейнеры** выберите **+ Container** (Создать контейнер).

1. В окне **Создание контейнера** в поле **Имя** введите **adftutorial**. Щелкните **Создать**.

1. В списке контейнеров выберите **adftutorial**, который вы только что создали.

1. Не закрывайте окно **контейнера** **adftutorial**. Оно понадобится для проверки выходных данных в конце учебника. Фабрика данных автоматически создает выходную папку в этом контейнере, поэтому ее не нужно создавать.

## <a name="create-a-data-factory"></a>Создание фабрики данных
На этом этапе вы создадите фабрику данных и запустите пользовательский интерфейс службы "Фабрика данных" для создания конвейера в фабрике данных.

1. Откройте веб-браузер **Microsoft Edge** или **Google Chrome**. Сейчас только эти браузеры поддерживают пользовательский интерфейс фабрики данных.
1. В меню слева выберите **Создать ресурс** > **Интеграция** > **Фабрика данных**:

   ![Выбор фабрики данных в области "Создать"](./media/doc-common-process/new-azure-data-factory-menu.png)

1. На странице **Новая фабрика данных** в поле **Имя** введите **ADFTutorialDataFactory**.

   Имя фабрики данных должно быть *глобально уникальным*. Если вы увидите следующее сообщение об ошибке для поля имени, введите другое имя фабрики данных (например, ваше_имя_ADFTutorialBulkCopyDF). Дополнительные сведения о правилах именования артефактов фабрики данных см. в статье [Фабрика данных Azure — правила именования](naming-rules.md).

    :::image type="content" source="./media/doc-common-process/name-not-available-error.png" alt-text="Новое сообщение об ошибке фабрики данных со сведениями о том, что имя дублируется.":::

1. Выберите **подписку** Azure, в рамках которой вы хотите создать фабрику данных.
1. Для **группы ресурсов** выполните одно из следующих действий:

   - Выберите **Использовать существующую** и укажите существующую группу ресурсов в раскрывающемся списке.

   - Выберите **Создать новую** и укажите имя группы ресурсов.
        
     Сведения о группах ресурсов см. в статье [Общие сведения об Azure Resource Manager](../azure-resource-manager/management/overview.md).
1. В качестве **версии** выберите **V2**.
1. В качестве **расположения** выберите расположение фабрики данных. В раскрывающемся списке отображаются только поддерживаемые расположения. Хранилища данных (например, служба хранилища и база данных SQL) и вычислительные ресурсы (например, Azure HDInsight), используемые фабрикой данных, могут располагаться в других регионах.
1. Нажмите кнопку **создания**.

1. Когда завершится создание, откроется страница **Фабрика данных**, как показано на рисунке ниже.

    :::image type="content" source="./media/doc-common-process/data-factory-home-page.png" alt-text="Домашняя страница Фабрики данных Azure с плиткой &quot;Создание и мониторинг&quot;.":::
1. Чтобы открыть пользовательский интерфейс службы "Фабрика данных" на отдельной вкладке, выберите элемент **Создание и мониторинг**.


## <a name="create-a-pipeline"></a>Создание конвейера

1. На странице **Начало работы** выберите **Create pipeline** (Создать конвейер). Будет автоматически создан конвейер. Конвейер отобразится в представлении в виде дерева и откроется в редакторе.

   ![Страница Let's get started (Начало работы)](./media/doc-common-process/get-started-page.png)

1. На общей панели в разделе **Свойства** укажите значение **SQLServerToBlobPipeline** для параметра **Имя**. Затем сверните панель, щелкнув значок "Свойства" в правом верхнем углу.

1. На панели инструментов **Действия** разверните **Move & Transform** (Переместить и преобразовать). Перетащите действие **копирования** на рабочую область конструирования конвейера. Задайте этому действию имя **CopySqlServerToAzureBlobActivity**.

1. В окне **свойств** перейдите на вкладку **Источник** и выберите **+ Создать**.

1. В диалоговом окне **Новый набор данных** найдите **SQL Server**. Выберите **SQL Server**, а затем нажмите кнопку **Продолжить**.
    ![Новый набор данных SqlServer](./media/tutorial-hybrid-copy-portal/create-sqlserver-dataset.png)

1. В диалоговом окне **Set Properties** (Установка свойств) в поле **Имя** введите **SqlServerDataset**. В разделе **Связанная служба** выберите **+ Создать**. На этом этапе вы создадите подключение к исходному хранилищу данных (база данных SQL Server).

1. В диалоговом окне **New Linked Service** (Новая связанная служба) добавьте **имя** **SqlServerLinkedService**. В разделе **Connect via integration runtime** (Подключение через среду выполнения интеграции) выберите **+Создать**.  В этом разделе вы создадите локальную среду выполнения интеграции и свяжете ее с локальным компьютером, на котором находится база данных SQL Server. Локальная среда выполнения интеграции — это компонент, который копирует данные из базы данных SQL Server на компьютере в хранилище BLOB-объектов.

1. В диалоговом окне **Integration Runtime Setup** (Настройка среды выполнения интеграции) выберите **Независимый**, а затем щелкните **Продолжить**.

1. В поле "Имя" введите **TutorialIntegrationRuntime**. Щелкните **Создать**.

1. Для параметров, выберите элемент **Click here to launch the express setup for this computer** (Щелкните здесь, чтобы запустить экспресс-установку для этого компьютера). Это действие устанавливает среду выполнения интеграции на локальном компьютере и регистрирует ее с использованием фабрики данных. Кроме того, вы можете использовать режим установки вручную: скачайте файл установки, запустите его и примените ключ для регистрации среды выполнения интеграции.
    ![Настройка среды выполнения интеграции](./media/tutorial-hybrid-copy-portal/intergration-runtime-setup.png)

1. В окне **Экспресс-установка Integration Runtime (Self-hosted)** выберите **Закрыть** по завершении процесса.

    ![Экспресс-установка Integration Runtime (Self-hosted)](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)

1. Убедитесь, что в диалоговом окне **New Linked Service (SQL Server)** (Новая связанная служба) для поля **Connect via integration runtime** (Подключение через среду выполнения интеграции) выбран вариант **TutorialIntegrationRuntime**. Затем выполните следующие действия:

    а. В поле **Имя** введите **SqlServerLinkedService**.

    b. В поле **Имя сервера** введите имя экземпляра SQL Server.

    c. В поле **Имя базы данных** введите имя базы данных, которая содержит таблицу **emp**.

    d. В поле **Тип проверки подлинности**, выберите нужный тип аутентификации, который будет использоваться в фабрике данных для подключения к базе данных SQL Server.

    д) В полях **имени пользователя** и **пароля** введите имя пользователя и пароль. При необходимости в качестве имени пользователя используйте *mydomain\\myuser*.

    е) Выберите **Test connection** (Проверить подключение). Этот шаг позволит проверить подключение Фабрики данных к базе данных SQL Server при помощи созданной в этом процессе локальной среды выполнения интеграции.

    ж. Выберите **Сохранить**, чтобы сохранить связанную службу.
 
    ![Создание связанной службы (SQL Server)](./media/tutorial-hybrid-copy-portal/new-sqlserver-linked-service.png)

1. После создания связанной службы откроется страница **Задание свойств** для SqlServerDataset. Сделайте следующее:

    а. Убедитесь, что в поле **Связанная служба** выбрано значение **SqlServerLinkedService**.

    b. В разделе **Имя таблицы** выберите **[dbo].[emp]** .
    
    c. Щелкните **ОК**.

1. Перейдите на вкладку **SQLServerToBlobPipeline** или выберите элемент **SQLServerToBlobPipeline** в представлении в виде дерева.

1. Перейдите на вкладку **Приемник** в нижней части окна **свойств** и выберите **+ Создать**.

1. В диалоговом окне **Новый набор данных** выберите **Хранилище BLOB-объектов Azure**. Затем выберите **Continue** (Продолжить).

1. В диалоговом окне **Выбрать формат** выберите тип формата данных. Затем выберите **Continue** (Продолжить).

    ![Выбор формата данных](./media/doc-common-process/select-data-format.png)

1. В диалоговом окне **Set Properties** (Установка свойств) введите **AzureBlobDataset** в качестве имени. Рядом с текстовым полем **Связанная служба** нажмите кнопку **+ Создать**.

1. В окне **New Linked Service (Azure Blob Storage)** (Новая связанная служба (хранилище BLOB-объектов Azure)) в качестве имени введите **AzureStorageLinkedService** и выберите учетную запись хранения в списке **Имя учетной записи хранения**. Проверьте подключение, а затем нажмите кнопку **Создать**, чтобы развернуть связанную службу.

1. После создания связанной службы откроется страница **Set properties** (Установка свойств). Щелкните **ОК**.

1. Откройте набор данных приемника. На вкладке **Подключение** сделайте следующее:

    а. Убедитесь, что в списке **Связанная служба** выбрано значение **AzureStorageLinkedService**.

    b. В поле **Путь к файлу** введите значение **adftutorial/fromonprem** для частей **Container/ Directory** (Контейнер/Каталог). Если указанной папки выходных данных не существует в контейнере adftutorial, она будет автоматически создана в фабрике данных.

    c. Для части **Файл** выберите **Добавить динамическое содержимое**.
    ![динамическое выражение для разрешения имени файла](./media/tutorial-hybrid-copy-portal/file-name.png)

    d. Добавьте `@CONCAT(pipeline().RunId, '.txt')` и нажмите кнопку **Готово**. Файл будет переименован в PipelineRunID.txt.

1. Перейдите на вкладку с открытым конвейером или выберите конвейер в представлении в виде дерева. Убедитесь, что в списке **Sink Dataset** (Целевой набор данных) выбрано значение **AzureBlobDataset**.

1. Чтобы проверить параметры конвейера, выберите **Проверка** на панели инструментов для этого конвейера. Чтобы закрыть **Pipe validation output** (Результаты проверки канала), выберите значок **>>** .
    ![Проверка конвейера](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)
    

1. Чтобы опубликовать сущности, созданные в Фабрике данных, выберите **Опубликовать все**.

1. Дождитесь всплывающего окна с сообщением **Публикация выполнена**. Чтобы проверить состояние публикации, выберите ссылку **Показать уведомления** в верхней части окна. Чтобы закрыть окно уведомлений, выберите **Закрыть**.


## <a name="trigger-a-pipeline-run"></a>Активация выполнения конвейера
Выберите **Добавить триггер** на панели инструментов контейнера, а затем **Trigger Now** (Запустить сейчас).

## <a name="monitor-the-pipeline-run"></a>Мониторинг конвейера

1. Перейдите на вкладку **Мониторинг**. Отобразится конвейер, который вы запускали вручную на предыдущем этапе.

1. Чтобы просмотреть выполнение действий, связанных с выполнением конвейера, выберите ссылку **SQLServerToBlobPipeline** в разделе *ИМЯ КОНВЕЙЕРА*. 
    ![Мониторинг выполнений конвейера](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)

1. На странице **выполнения действий** выберите ссылку сведений (образ очков), чтобы просмотреть сведения об операции копирования. Выберите **Все запуски конвейеров** в верхней части окна, чтобы вернуться к представлению "Выполнения конвейеров".

## <a name="verify-the-output"></a>Проверка выходных данных
Конвейер автоматически создает выходную папку с именем *fromonprem* в контейнере больших двоичных объектов `adftutorial`. Убедитесь, что в выходной папке отображается файл *[pipeline().RunId].txt*.


## <a name="next-steps"></a>Дальнейшие действия
В этом примере конвейер копирует данные из одного расположения в другое в хранилище BLOB-объектов. Вы ознакомились с выполнением следующих задач:

> [!div class="checklist"]
> * Создали фабрику данных.
> * Создайте локальную среду выполнения интеграции.
> * Создавать связанные службы SQL Server и хранилища.
> * Создавать наборы данных SQL Server и хранилища BLOB-объектов.
> * Создадите конвейер с действием копирования для перемещения данных.
> * Запуск конвейера.
> * Осуществили мониторинг выполнения конвейера.

Список хранилищ данных, поддерживаемых фабрикой данных, см. в разделе [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

Чтобы узнать о копировании данных в пакетном режиме из источника в место назначения, перейдите к следующему руководству:

> [!div class="nextstepaction"]
>[Копирование нескольких таблиц в пакетном режиме с помощью фабрики данных Azure](tutorial-bulk-copy-portal.md)