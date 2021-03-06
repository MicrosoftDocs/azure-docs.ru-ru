---
title: Архивация SQL Server в Azure с помощью рабочей нагрузки DPM
description: Общие сведения о резервном копировании баз данных SQL Server с помощью службы Azure Backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 592a51051a0d02a6c1d491db0fe559e2e62babb2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "96327055"
---
# <a name="back-up-sql-server-to-azure-as-a-dpm-workload"></a>Архивация SQL Server в Azure с помощью рабочей нагрузки DPM

В этой статье описываются действия по настройке резервного копирования SQL Server баз данных с помощью Azure Backup.

Чтобы создать резервные копии баз данных SQL Server в Azure, необходима учетная запись Azure. Если у вас ее нет, можно создать бесплатную учетную запись всего за несколько минут. Дополнительные сведения см. в статье [Создание бесплатной учетной записи Azure](https://azure.microsoft.com/pricing/free-trial/).

Чтобы создать резервную копию базы данных SQL Server в Azure и восстановить ее из Azure:

1. Создайте политику архивации для защиты баз данных SQL Server в Azure.
1. Создавайте резервные копии по запросу в Azure.
1. Восстановление базы данных из Azure.

>[!NOTE]
>DPM 2019 UR2 поддерживает SQL Server экземпляров отказоустойчивого кластера (FCI) с помощью общих томов кластера (CSV).<br><br>
>Эта функция поддерживает защиту [экземпляра отказоустойчивого кластера SQL Server с Локальные дисковые пространства в Azure](../azure-sql/virtual-machines/windows/failover-cluster-instance-storage-spaces-direct-manually-configure.md)  и [экземпляром отказоустойчивого кластера SQL Server с общими дисками Azure](../azure-sql/virtual-machines/windows/failover-cluster-instance-azure-shared-disks-manually-configure.md) . Сервер DPM должен быть развернут на виртуальной машине Azure для защиты экземпляра SQL FCI, развернутого на виртуальных машинах Azure. 

## <a name="prerequisites-and-limitations"></a>Предварительные условия и ограничения

* Если у вас есть база данных, файлы которой расположены на удаленном файловом ресурсе, то при включении защиты произойдет сбой с кодом ошибки 104. DPM не поддерживает защиту данных SQL Server на удаленном файловом ресурсе.
* DPM не может защищать базы данных, хранящиеся на удаленных общих ресурсах SMB.
* Убедитесь, что для [реплик группы обеспечения доступности установлен режим "только для чтения"](/sql/database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server).
* Необходимо явно добавить системную учетную запись **NTAuthority\System** в группу Sysadmin на SQL Server.
* При восстановлении частично автономной базы данных в альтернативное расположение убедитесь в том, что в целевом экземпляре SQL активирован параметр [Автономные базы данных](/sql/relational-databases/databases/migrate-to-a-partially-contained-database#enable).
* При восстановлении базы данных файлового потока в альтернативное расположение убедитесь в том, что в целевом экземпляре SQL активирован параметр [База данных файлового потока](/sql/relational-databases/blob/enable-and-configure-filestream).
* Защита для SQL Server AlwaysOn.
  * DPM обнаруживает группы обеспечения доступности при выполнении опроса на этапе создания группы защиты.
  * DPM обнаруживает отработку отказа и продолжает защищать базу данных.
  * DPM поддерживает геораспределенные кластеры экземпляра SQL Server.
* При защите баз данных, использующих функцию AlwaysOn, в работе DPM действуют следующие ограничения.
  * DPM будет соблюдать политику резервного копирования для групп доступности, которые задаются в SQL Server на основе настроек резервного копирования следующим образом.
    * Предпочитать вторичную: резервное копирование должно выполняться на вторичную реплику, если первичная реплика не является единственной репликой, подключенной к сети. Если доступно несколько вторичных реплик, для резервного копирования будет выбран узел с самым высоким приоритетом резервного копирования. Если доступна только первичная реплика, то резервная копия должна выполняться на первичной реплике.
    * Только вторичная: резервное копирование не должно выполняться на первичную реплику. Если доступна только первичная реплика, то архивирование не будет выполнено.
    * Первичная: резервное копирование всегда выполняется на первичную реплику.
    * Любая реплика: резервное копирование выполняется на любую доступную реплику в группе обеспечения доступности. Узел, с которого будет выполняться резервное копирование, будет определяться по приоритету резервного копирования всех узлов.
  * Следует отметить следующее.
    * Резервные копии могут происходить из любой доступной для чтения реплики, которая является первичной, синхронной вторичной базой данных-получателем.
    * Если какая-либо реплика исключена из резервной копии, например " **Исключить реплику** " включена или помечена как недоступная для чтения, то эта реплика не будет выбрана для резервного копирования при любом из параметров.
    * Если доступно несколько реплик и они доступны для чтения, для резервного копирования будет выбран узел с самым высоким приоритетом резервного копирования.
    * Если резервное копирование на выбранном узле завершается сбоем, операция резервного копирования завершается сбоем.
    * Восстановление в исходное расположение не поддерживается.
* Проблемы при резервном копировании SQL Server 2014 или выше
  * В SQL Server 2014 добавлена новая функция для создания [базы данных для локального экземпляра SQL Server в хранилище BLOB-объектов Azure](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure). DPM не может использоваться для защиты этой конфигурации.
  * Существуют некоторые известные проблемы с предпочтениями резервного копирования "предпочитать вторичные" для параметра SQL AlwaysOn. DPM всегда выполняет резервное копирование из базы данных-получателя. Если не удается найти базу данных-получатель, произойдет сбой резервного копирования.

## <a name="before-you-start"></a>Перед началом работы

Прежде чем начать, убедитесь, что выполнены [Предварительные условия](backup-azure-dpm-introduction.md#prerequisites-and-limitations) для использования Azure Backup для защиты рабочих нагрузок. Ниже приведены некоторые необходимые задачи.

* Создайте хранилище службы архивации.
* Скачайте учетные данные хранилища.
* Установите агент Azure Backup.
* Зарегистрируйте сервер в хранилище.

## <a name="create-a-backup-policy"></a>создание политики архивации;

Чтобы защитить базы данных SQL Server в Azure, сначала создайте политику архивации:

1. На сервере Data Protection Manager (DPM) выберите рабочую область **Защита** .
1. Выберите **создать** , чтобы создать группу защиты.

    ![Создайте группу защиты](./media/backup-azure-backup-sql/protection-group.png)
1. На начальной странице Ознакомьтесь с руководством по созданию группы защиты. Выберите **Далее**.
1. Выберите **серверы**.

    ![Выберите тип группы защиты серверов](./media/backup-azure-backup-sql/pg-servers.png)
1. Разверните SQL Server виртуальную машину, в которой находятся базы данных, для которых требуется выполнить резервное копирование. Вы увидите источники данных, которые можно архивировать с этого сервера. Разверните **все общие ресурсы SQL** , а затем выберите базы данных, для которых требуется создать резервную копию. В этом примере мы выбираем ReportServer $ MSDPM2012 и ReportServer $ MSDPM2012TempDB. Выберите **Далее**.

    ![Выберите базу данных SQL Server](./media/backup-azure-backup-sql/pg-databases.png)
1. Присвойте имя группе защиты, а затем выберите **я хочу оперативную защиту**.

    ![Выбор метода защиты данных — Краткосрочная защита дисков или оперативная защита Azure](./media/backup-azure-backup-sql/pg-name.png)
1. На странице **указание Short-Term целей** включите необходимые входные данные для создания точек резервного копирования на диск.

    В этом примере **диапазон хранения** устанавливается равным *5 дням*. **Периодичность синхронизации** резервных копий устанавливается каждые *15 минут*. Для **быстрой полной архивации** задано значение *8:00 PM*.

    ![Настройка краткосрочных целей для защиты резервного копирования](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > В этом примере точка резервного копирования создается в 8:00 PM ежедневно. Данные, измененные с момента передачи точки резервного копирования 8:00 PM, передаются. Этот процесс называется **быстрой полной архивацией**. Несмотря на то, что журналы транзакций синхронизируются каждые 15 минут, если необходимо восстановить базу данных в 9:00 PM, то точка создается путем воспроизведения журналов из последней точки быстрой полной архивации, которая в этом примере 8:00 PM.
   >
   >

1. Выберите **Далее**. DPM показывает общее доступное дисковое пространство. Он также показывает потенциальное использование дискового пространства.

    ![Настройка распределения дискового пространства](./media/backup-azure-backup-sql/pg-storage.png)

    По умолчанию DPM создает один том для каждого источника данных (SQL Server базы данных). Том используется для начальной резервной копии. В этой конфигурации Диспетчер логических дисков (LDM) ограничивает защиту DPM до 300 источников данных (SQL Server базы данных). Чтобы обойти это ограничение, выберите **Выравнивать данные в пуле носителей DPM**. При использовании этого параметра DPM использует один том для нескольких источников данных. Эта программа установки позволяет DPM защищать до 2 000 SQL Server баз данных.

    Если выбран параметр **автоматически увеличивать объем томов**, DPM может учитывать увеличенный объем резервного копирования по мере роста рабочих данных. Если не выбрать **Автоматическое увеличение количества томов**, DPM будет ограничивать хранилище резервных копий источниками данных в группе защиты.

1. Если вы являетесь администратором, вы можете автоматически переносить эту начальную резервную копию **по сети** и выбрать время передачи. Или выберите **ручной** перенос резервной копии. Выберите **Далее**.

    ![Выбор метода создания реплики](./media/backup-azure-backup-sql/pg-manual.png)

    Для начальной резервной копии требуется перенос всего источника данных (SQL Server базы данных). Данные резервной копии перемещаются с рабочего сервера (SQL Server компьютера) на сервер DPM. Если резервная копия имеет большой размер, передача данных по сети может привести к перегрузке пропускной способности. По этой причине администраторы могут использовать съемные носители для **ручной** передачи начальной резервной копии. Или они могут автоматически передавать данные **по сети** в указанное время.

    После завершения первоначального резервного копирования резервные копии будут последовательно выполняться в начальной резервной копии. Обычно добавочные резервные копии имеют небольшой размер и могут быть легко переданы по сети.

1. Выберите время выполнения проверки согласованности. Выберите **Далее**.

    ![Выберите время выполнения проверки согласованности](./media/backup-azure-backup-sql/pg-consistent.png)

    DPM может выполнить проверку согласованности для целостности точки резервного копирования. Она вычисляет контрольную сумму файла резервной копии на рабочем сервере (SQL Server компьютере в этом примере) и резервные данные для этого файла в DPM. Если проверка обнаруживает конфликт, то файл резервной копии в DPM считается поврежденным. DPM исправляет резервные данные, отправляя блоки, которые соответствуют несовпадению контрольной суммы. Поскольку проверка согласованности является трудоемкой операцией, администраторы могут выбрать расписание проверки согласованности или запустить ее автоматически.

1. Выберите источники данных для защиты в Azure. Выберите **Далее**.

    ![Выбор источников данных для защиты в Azure](./media/backup-azure-backup-sql/pg-sqldatabases.png)
1. Если вы являетесь администратором, вы можете выбрать расписания архивации и политики хранения, соответствующие политикам вашей организации.

    ![Выбор расписаний и политик хранения](./media/backup-azure-backup-sql/pg-schedule.png)

    В этом примере резервные копии выполняются ежедневно в 12:00 PM и 8:00 PM.

    > [!TIP]
    > Для быстрого восстановления сохраните на диске несколько краткосрочных точек восстановления. Эти точки восстановления используются для оперативного восстановления. Azure выступает в качестве хорошей автономности, обеспечивая более высокий уровень соглашения об уровне обслуживания и гарантированную доступность.
    >
    > Используйте DPM для планирования резервного копирования Azure после завершения резервного копирования локального диска. При выполнении этой методики в Azure копируется последняя резервная копия диска.
    >

1. Выберите расписание для политики хранения. Дополнительные сведения о том, как работает политика хранения, см. [в разделе использование Azure Backup для замены ленточной инфраструктуры](backup-azure-backup-cloud-as-tape.md).

    ![Выбор политики хранения](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    В этом примере:

    * Резервные копии создаются ежедневно в 12:00 PM и 8:00 PM. Они хранятся в течение 180 дней.
    * Резервная копия в субботу на 12:00 PM хранится в течение 104 недель.
    * Резервная копия за последнюю субботу месяца в 12:00 PM хранится в течение 60 месяцев.
    * Резервная копия за последнюю субботу марта в 12:00 составляет 10 лет.

    После выбора политики хранения нажмите кнопку **Далее**.

1. Выберите способ перемещения начальной резервной копии в Azure.

    * Параметр **автоматически по сети** следует за расписанием резервного копирования для передачи данных в Azure.
    * Дополнительные сведения о **автономной архивации** см. в разделе [Обзор автономной резервной копии](offline-backup-overview.md).

    После выбора механизма перемещения нажмите кнопку **Далее**.

1. На странице **Сводка** проверьте сведения о политике. Затем выберите **создать группу**. Вы можете выбрать **Закрыть** и наблюдать за ходом выполнения задания в рабочей области **мониторинг** .

    ![Ход создания группы защиты](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="create-on-demand-backup-copies-of-a-sql-server-database"></a>Создание резервных копий SQL Server базы данных по запросу

Точка восстановления создается при наступлении первой резервной копии. Вместо того чтобы ждать выполнения расписания, можно запустить создание точки восстановления вручную.

1. В группе защиты убедитесь, что база данных имеет состояние **ОК**.

    ![Группа защиты, показывающая состояние базы данных](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
1. Щелкните правой кнопкой мыши базу данных и выберите команду **создать точку восстановления**.

    ![Выберите, чтобы создать точку восстановления в сети](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
1. В раскрывающемся меню выберите **оперативная защита**. Затем нажмите кнопку **ОК** , чтобы начать создание точки восстановления в Azure.

    ![Начало создания точки восстановления в Azure](./media/backup-azure-backup-sql/sqlbackup-azure.png)
1. Ход выполнения задания можно просмотреть в рабочей области **мониторинг** .

    ![Просмотр хода выполнения задания в консоли мониторинга](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Восстановление базы данных SQL Server из Azure

Чтобы восстановить защищенную сущность, например базу данных SQL Server, из Azure:

1. Откройте консоль управления сервера DPM. Перейдите в рабочую область **Восстановление** , чтобы просмотреть серверы, на которых производится резервное копирование DPM. Выберите базу данных (в этом примере — ReportServer $ MSDPM2012). Выберите **время восстановления** , заканчивающееся на "в **сети**".

    ![выбора точки восстановления](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
1. Щелкните правой кнопкой мыши имя базы данных и выберите команду **восстановить**.

    ![Восстановление базы данных из Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
1. DPM отобразит сведения о точке восстановления. Выберите **Далее**. Чтобы перезаписать базу данных, выберите тип восстановления **Восстановить в исходном экземпляре SQL Server**. Выберите **Далее**.

    ![Восстановление базы данных в ее исходном расположении](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    В этом примере DPM позволяет восстановить базу данных на другой экземпляр SQL Server или в автономную сетевую папку.
1. На странице **Указание параметров восстановления** можно выбрать параметры восстановления. Например, можно выбрать **регулирование использования полосы пропускания сети** для регулирования пропускной способности, используемой при восстановлении. Выберите **Далее**.
1. На странице **Сводка** вы увидите текущую конфигурацию восстановления. Выберите **восстановить**.

    Состояние восстановления показывает восстанавливаемую базу данных. Можно нажать кнопку **Закрыть** , чтобы закрыть мастер и просмотреть ход выполнения в рабочей области **мониторинг** .

    ![Запуск процесса восстановления](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    После завершения восстановления восстановленная база данных будет соответствовать приложению.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в разделе [Azure Backup часто задаваемые вопросы](backup-azure-backup-faq.md).