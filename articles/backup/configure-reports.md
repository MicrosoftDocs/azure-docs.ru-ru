---
title: Настройка отчетов службы Azure Backup
description: Настройка и просмотр отчетов для Azure Backup с помощью анализа журналов и книг Azure
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 0720af0848aa8263587dfd9573d205abf73303d4
ms.sourcegitcommit: f0a3ee8ff77ee89f83b69bc30cb87caa80f1e724
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2021
ms.locfileid: "105562330"
---
# <a name="configure-azure-backup-reports"></a>Настройка отчетов службы Azure Backup

Чаще всего администраторам резервного копирования необходимо получать полезную информацию о резервных копиях из данных, которые охватывают длительный период времени. Ниже перечислены различные сценарии использования такого решения.

- Выделение и прогнозирование потребления ресурсов облачного хранилища.
- Аудит операций резервного копирования и восстановления.
- Определение ключевых тенденций при различных уровнях детализации.

Сегодня Azure Backup предлагает решение для создания отчетов, которое использует [журналы Azure Monitor](../azure-monitor/logs/log-analytics-tutorial.md) и [книги Azure](../azure-monitor/visualize/workbooks-overview.md). Эти ресурсы предоставляют подробные аналитические сведения об операциях резервного копирования для всего пространства резервного копирования. В этой статье объясняется, как настраивать и просматривать отчеты Azure Backup.

## <a name="supported-scenarios"></a>Поддерживаемые сценарии

- Отчеты о резервном копировании поддерживаются для виртуальных машин Azure, SQL на виртуальных машинах Azure, SAP HANA на виртуальных машинах Azure, агенте служб восстановления Microsoft Azure (MARS), Microsoft Azure Backup Server (MABS) и System Center Data Protection Manager (DPM). Для резервного копирования файлового ресурса Azure отображаются данные для записей, созданных после 1 июня 2020.
- Для резервного копирования файловых ресурсов Azure данные на защищенных экземплярах отображаются для записей, созданных после 1 февраля 2021 (значение по умолчанию равно нулю для старых записей).
- Для рабочих нагрузок DPM отчеты о резервном копировании поддерживаются в DPM версии 5.1.363.0 и выше и в версии агента 2.0.9127.0 и выше.
- Для рабочих нагрузок MABS отчеты о резервном копировании поддерживаются в MABS версии 13.0.415.0 и выше и в версии агента 2.0.9170.0 и выше.
- Отчеты о резервном копировании можно просматривать во всех элементах резервного копирования, хранилищах, подписках и регионах, если их данные отправляются в рабочую область службы анализа журналов, к которой у пользователя есть доступ. Для просмотра отчетов по набору хранилищ необходимо иметь доступ с правами только для чтения к рабочей области службы анализа журналов, в которую хранилища отправляют свои данные. Доступ к отдельным хранилищам не требуется.
- Если вы являетесь пользователем [Azure Lighthouse](../lighthouse/index.yml) с делегированным доступом к подпискам ваших заказчиков, вы можете использовать эти отчеты в Azure Lighthouse для просмотра отчетов по всем арендаторам.
- В настоящее время данные можно просматривать в отчетах о резервном копировании максимум по 100 рабочим областям службы анализа журналов (в арендаторах).
- Данные по заданиям резервного копирования журналов в настоящее время не отображаются в отчетах.

[!INCLUDE [backup-center.md](../../includes/backup-center.md)]

## <a name="get-started"></a>Начало работы

Чтобы начать использовать отчеты, выполните следующие действия.

### <a name="1-create-a-log-analytics-workspace-or-use-an-existing-one"></a>1. Выберите существующую или создайте новую рабочую область службы анализа журналов

Настройте одну или несколько рабочих областей службы анализа журналов для хранения данных отчетов о резервном копировании. Расположение и подписка, в которых можно создать рабочую область службы анализа журналов, не зависят от расположения и подписки, где существуют хранилища.

Инструкции по настройке рабочей области службы анализа журналов см. в разделе [Создание рабочей области службы анализа журналов на портале Azure](../azure-monitor/logs/quick-create-workspace.md).

По умолчанию данные в рабочей области службы анализа журналов хранятся в течение 30 дней. Чтобы просмотреть данные за более длительный период времени, измените срок хранения рабочей области службы анализа журналов. Сведения об изменении срока хранения см. в разделе [Управление использованием и затратами с помощью журналов Azure Monitor](../azure-monitor/logs/manage-cost-storage.md).

### <a name="2-configure-diagnostics-settings-for-your-vaults"></a>2. Настройка параметров диагностики для хранилищ

Ресурсы Диспетчера ресурсов Azure, такие как хранилища служб восстановления, записывают сведения о запланированных операциях и операциях, активируемых пользователем, в виде диагностических данных.

В разделе "Мониторинг" хранилища служб восстановления выберите **Параметры диагностики** и укажите целевой объект для диагностических данных хранилища служб восстановления. Дополнительные сведения об использовании диагностических событий см. в разделе [Использование параметров диагностики для хранилищ служб восстановления](./backup-azure-diagnostic-events.md).

![Панель параметров диагностики](./media/backup-azure-configure-backup-reports/resource-specific-blade.png)

Служба Azure Backup также предоставляет встроенное определение политики Azure, которое автоматизирует настройку параметров диагностики для всех хранилищ в заданной области. Сведения об использовании этой политики см. в разделе [Настройка параметров диагностики хранилища в масштабе](./azure-policy-configure-diagnostics.md).

> [!NOTE]
> После настройки диагностики для завершения передачи исходных данных может потребоваться до 24 часов. После того как данные начнут передаваться в рабочую область службы анализа журналов, они могут не сразу отображаться в отчетах, поскольку данные за текущий день не отображаются в отчетах. Дополнительные сведения см. в разделе [Соглашения, используемые в отчетах по резервному копированию](#conventions-used-in-backup-reports). Рекомендуется начинать просматривать отчеты через два дня после настройки хранилищ для отправки данных в службу анализа журналов.

#### <a name="3-view-reports-in-the-azure-portal"></a>3. Просмотр отчетов на портале Azure

После настройки хранилищ для отправки данных в службу анализа журналов просмотрите отчеты о резервном копировании, перейдя на панель любого хранилища и выбрав **Отчеты о резервном копировании**.

![Панель мониторинга хранилища](./media/backup-azure-configure-backup-reports/vault-dashboard.png)

Выберите эту ссылку, чтобы открыть книгу отчета о резервном копировании.

> [!NOTE]
>
> - В настоящее время первоначальная загрузка отчета может занимать до 1 минуты.
> - Хранилище служб восстановления — это просто точка входа для отчетов о резервном копировании. После открытия книги отчета о резервном копировании из области хранилища выберите соответствующий набор рабочих областей службы анализа журналов, чтобы просмотреть данные, агрегированные по всем хранилищам.

Отчет содержит различные вкладки.

##### <a name="summary"></a>Итоги

Используйте эту вкладку, чтобы получить общий обзор пространства резервного копирования. Краткий обзор общего количества элементов резервного копирования, общего объема потребления ресурсов облачного хранилища, количества защищенных экземпляров и процента успешно выполненных заданий для каждого типа рабочей нагрузки. Дополнительные сведения о конкретных типах артефактов резервного копирования см. на соответствующих вкладках.

   ![Вкладка "Сводка"](./media/backup-azure-configure-backup-reports/summary.png)

##### <a name="backup-items"></a>Резервные элементы

Эта вкладка используется для просмотра сведений и тенденций в облачном хранилище, используемом на уровне элементов резервного копирования. Например, если вы используете SQL в резервной копии виртуальной машины Azure, вы можете просмотреть сведения об использовании облачного хранилища для каждой базы данных SQL, для которой выполняется резервное копирование. Кроме того, можно просматривать данные для элементов резервного копирования с определенным состоянием защиты. Например, при щелчке на плитке **Защита остановлена** в верхней части вкладки выполняется фильтрация всех мини-приложений под ней, чтобы отображались только данные для элементов резервного копирования в состоянии "Защита остановлена".

   ![Элемент "Элементы резервного копирования"](./media/backup-azure-configure-backup-reports/backup-items.png)

##### <a name="usage"></a>Использование

Используйте эту вкладку, чтобы просмотреть ключевые параметры выставления счетов для резервных копий. Сведения, отображаемые на этой вкладке, находятся на уровне сущности выставления счетов (в защищенном контейнере). Например, если выполняется резервное копирование сервера DPM в Azure, можно просмотреть тенденцию защищенных экземпляров и облачного хранилища, используемого для сервера DPM. Аналогичным образом, если вы используете SQL в Azure Backup или SAP HANA в Azure Backup, на этой вкладке отображаются сведения об использовании на уровне виртуальной машины, на которой содержатся эти базы данных.

   ![Вкладка "Использование"](./media/backup-azure-configure-backup-reports/usage.png)

> [!NOTE]
> Для рабочих нагрузок DPM пользователи могут увидеть небольшое различие (в порядке 20 МБ на сервер DPM) между значениями использования, показанными в отчетах, по сравнению со статистическим значением использования, как показано на вкладке **Обзор** хранилища служб восстановления. Это различие учитывается тем, что каждый сервер DPM, регистрируемый для резервного копирования, имеет связанный источник данных метаданных, который не является артефактом для создания отчетов.

##### <a name="jobs"></a>Задания

Эта вкладка используется для просмотра длительных тенденций в заданиях, таких как количество невыполненных заданий в день и основных причин сбоя задания. Эти сведения можно просмотреть на агрегированном уровне и на уровне элементов резервного копирования. Выберите определенный элемент резервного копирования в сетке, чтобы просмотреть подробные сведения о каждом задании, которое было запущено для этого элемента резервного копирования в выбранном диапазоне времени.

   ![Вкладка "Задания"](./media/backup-azure-configure-backup-reports/jobs.png)

##### <a name="policies"></a>Политики

Эта вкладка используется для просмотра сведений обо всех активных политиках, таких как число связанных элементов и Общее облачное хранилище, использованное элементами, резервные копии которых были заданной политикой. Выберите конкретную политику, чтобы просмотреть сведения о каждом из связанных с ней элементов резервного копирования.

   ![Вкладка "Политики"](./media/backup-azure-configure-backup-reports/policies.png)

##### <a name="optimize"></a>Оптимизация

Используйте эту вкладку, чтобы получить сведения о потенциальных возможностях оптимизации затрат для резервных копий. Ниже приведены сценарии, в которых на данный момент вкладка оптимизировать предоставляет аналитические сведения:

###### <a name="inactive-resources"></a>Неактивные ресурсы

С помощью этого представления можно вычислить элементы архивации, для которых в течение длительного времени не создавалась успешная архивация. Это может означать, что базовый компьютер, для которого создается резервная копия, больше не существует (и, следовательно, в результате неудачных резервных копий), или возникла проблема с компьютером, который предотвращает надежное резервное копирование.

Чтобы просмотреть неактивные ресурсы, перейдите на вкладку **Оптимизация** и выберите плитку **Неактивные ресурсы** . На этой плитке отображается сетка, содержащая подробные сведения о всех неактивных ресурсах, существующих в выбранной области. По умолчанию в сетке отображаются элементы без точки восстановления за последние семь дней. Чтобы найти неактивные ресурсы для другого диапазона времени, можно настроить фильтр **диапазона времени** в верхней части вкладки.

Определив неактивный ресурс, вы можете изучить проблему еще раз, перейдя на панель мониторинга элементов архивации или на панель ресурсов Azure для этого ресурса (когда это возможно). В зависимости от вашего сценария можно либо прерывать резервное копирование компьютера (если он больше не существует), либо удалять ненужные резервные копии, что экономит затраты, или устранять проблемы на компьютере, чтобы гарантировать надежность резервного копирования.

![Вкладка "оптимизация" — неактивные ресурсы](./media/backup-azure-configure-backup-reports/optimize-inactive-resources.png)

###### <a name="backup-items-with-a-large-retention-duration"></a>Элементы резервного копирования с большим сроком хранения

С помощью этого представления можно указать, какие элементы имеют резервные копии, которые хранятся в течение длительного времени, чем требуется для Организации.

Если щелкнуть плитку **оптимизация политики** , а затем плитку **Оптимизация удержания** , отобразится сетка, содержащая все элементы резервного копирования, для которых период хранения либо ежедневной, еженедельной, месячной, либо ежегодной точки сохранения (RP) больше указанного значения. По умолчанию в сетке отображаются все элементы архивации в выбранной области. Вы можете использовать фильтры для ежедневного, еженедельного, ежемесячного и ежегодного хранения RP, чтобы отфильтровать сетку и указать, для каких элементов можно уменьшить период удержания, чтобы сэкономить на затратах на хранение резервных копий.

Для рабочих нагрузок базы данных, таких как SQL и SAP HANA, периоды хранения, показанные в сетке, соответствуют периодам хранения полных точек резервного копирования, а не разностным точкам резервного копирования. То же относится и к фильтрам хранения.  

![Оптимизация хранения по табуляции](./media/backup-azure-configure-backup-reports/optimize-retention.png)

###### <a name="databases-configured-for-daily-full-backup"></a>Базы данных, настроенные для ежедневного полного резервного копирования 

С помощью этого представления можно выявление рабочих нагрузок базы данных, настроенных для ежедневного полного резервного копирования. Часто использование ежедневной разностной резервной копии вместе с еженедельным полным резервным копированием является более экономичным.

Если щелкнуть плитку **оптимизация политики** , а затем элемент **Оптимизация расписания архивации** , отобразится сетка, содержащая все базы данных с политикой ежедневной полной архивации. Вы можете выбрать определенный элемент архивации и изменить политику, чтобы использовать ежедневную разностную резервную копию с еженедельной полной архивацией.

Для фильтра **типа "Управление резервным копированием** " в верхней части вкладки должны быть выбраны элементы **SQL на виртуальной машине Azure** и **SAP HANA на виртуальной машине Azure** , чтобы сетка могла отображать рабочие нагрузки баз данных должным образом.

![Вкладка "оптимизация" — Оптимизация расписания резервного копирования](./media/backup-azure-configure-backup-reports/optimize-backup-schedule.png)

###### <a name="policy-adherence"></a>Соответствие политике

С помощью этой вкладки можно определить, имеют ли все экземпляры резервных копий по крайней мере одну успешную архивацию каждый день. Для элементов с политикой еженедельного резервного копирования эту вкладку можно использовать для определения наличия хотя бы одной успешной резервной копии всех экземпляров резервных копий в неделю.

Доступно два типа представлений соответствия политики.

* **Соблюдение политик по периоду времени**. с помощью этого представления можно узнать, сколько элементов имели по крайней мере одну успешную архивацию в определенный день и сколько не прошло резервное копирование в этот день. Можно щелкнуть строку, чтобы просмотреть сведения обо всех заданиях резервного копирования, которые были запущены в выбранный день. Обратите внимание, что при увеличении диапазона времени до большего значения, например за последние 60 дней, сетка подготавливается к просмотру в еженедельном представлении и отображает количество всех элементов, в которых каждый день в данной неделе по крайней мере одна успешная архивация. Аналогично, существует месячное представление для больших диапазонов времени.

В случае создания элементов, резервное копирование которых выполнялось еженедельно, эта сетка помогает найти все элементы, для которых в данной неделе есть хотя бы одна успешная резервная копия. Для большего диапазона времени, например за последние 120 дней, сетка подготавливается к просмотру в месячном представлении и отображает количество всех элементов, для которых в заданном месяце имеется хотя бы одна успешная архивация в течение недели. Дополнительные сведения о ежедневных, еженедельных и месячных представлениях см. [в разделе соглашения о резервном копировании](#conventions-used-in-backup-reports) .

![Соблюдение политик по периоду времени](./media/backup-azure-configure-backup-reports/policy-adherence-by-time-period.png)

* Политика, использующая **экземпляр резервной копии**. с помощью этого представления можно политикам присвоить сведения о соответствии на уровне экземпляра резервной копии. Зеленая ячейка означает, что экземпляру резервной копии была по крайней мере одна успешная архивация в указанный день. Красная Ячейка означает, что экземпляр резервной копии не имел даже одной успешной резервной копии в указанный день. Для ежедневных, еженедельных и ежемесячных агрегатов выполняются те же действия, что и в представлении «политика за период времени». Можно щелкнуть любую строку, чтобы просмотреть все задания резервного копирования в заданном экземпляре резервной копии за выбранный период времени.

![Соответствие политике экземпляром резервной копии](./media/backup-azure-configure-backup-reports/policy-adherence-by-backup-instance.png)

###### <a name="email-azure-backup-reports"></a>Отправка отчетов Azure Backup по электронной почте

С помощью функции **отчета по электронной почте** , доступной в отчетах по резервному копированию, можно создавать автоматизированные задачи для получения периодических отчетов по электронной почте. Эта функция работает путем развертывания приложения логики в среде Azure, которое запрашивает данные из выбранных рабочих областей Log Analytics (LA) на основе предоставленных входных данных.

После создания приложения логики необходимо авторизовать подключения к журналам Azure Monitor и Office 365. Для этого перейдите к **Logic Apps** в портал Azure и найдите имя созданной задачи. При выборе пункта меню **подключения API** открывается список подключений API, которые необходимо авторизовать. [Узнайте больше о том, как настраивать сообщения электронной почты и устранять неполадки](backup-reports-email.md).

###### <a name="customize-azure-backup-reports"></a>Настройка отчетов Azure Backup

Отчеты о резервном копировании используют [системные функции в журналах Azure Monitor](backup-reports-system-functions.md). Эти функции работают с данными в необработанных Azure Backup таблицах в LA и возвращают форматированные данные, которые позволяют легко получать сведения обо всех сущностях, связанных с резервным копированием, с помощью простых запросов. 

Для создания собственных книг отчетов с использованием отчетов по резервному копированию в качестве основы можно перейти к отчетам по резервному копированию, щелкнуть **изменить** в верхней части отчета, а также просмотреть/изменить запросы, используемые в отчетах. Дополнительные сведения о создании пользовательских отчетов см. в [документации по книгам Azure](../azure-monitor/visualize/workbooks-overview.md) . 

## <a name="export-to-excel"></a>Экспорт в Excel

Нажмите кнопку со стрелкой вниз в правом верхнем углу любого мини-приложения, например таблицы или диаграммы, чтобы экспортировать содержимое этого мини-приложения "как есть" в виде таблицы Excel с применением существующих фильтров. Чтобы экспортировать больше строк таблицы в Excel, можно увеличить число строк, отображаемых на странице, с помощью раскрывающегося списка **Количество строк на странице** в верхней части каждой сетки.

## <a name="pin-to-dashboard"></a>Закрепить на панели мониторинга

Чтобы закрепить мини-приложение на панели мониторинга портала Azure, нажмите кнопку "Закрепить" в верхней части мини-приложения. С помощью этой функции можно создавать настраиваемые панели мониторинга для просмотра наиболее важных сведений.

## <a name="cross-tenant-reports"></a>Отчеты по всем арендаторам

Если вы используете [Azure Lighthouse](../lighthouse/index.yml) с делегированным доступом к подпискам в нескольких средах арендатора, можно использовать фильтр подписок по умолчанию. Нажмите кнопку "Фильтр" в правом верхнем углу портала Azure, чтобы выбрать все подписки, для которых требуется отобразить данные. Это позволит выбрать рабочие области службы анализа журналов на арендаторах для просмотра отчетов по нескольким арендаторам.

## <a name="conventions-used-in-backup-reports"></a>Соглашения, используемые в отчетах по резервному копированию

- Фильтры применяются слева направо и сверху вниз на каждой вкладке. Таким образом, любой фильтр применяется ко всем мини-приложениям, расположенным либо справа от этого фильтра, либо ниже него.
- При выборе цветной плитки выполняется фильтрация мини-приложений под плиткой, обозначающей записи, относящиеся к значению этой плитки. Например, при щелчке на плитке **Защита остановлена** на вкладе **Элементы резервного копирования** выполняется фильтрация всех мини-приложений под ней, чтобы отображались только данные для элементов резервного копирования в состоянии "Защита остановлена".
- Плитки, которые не являются цветными, не выбираются.
- Данные по части текущего неполного дня не отображаются в отчетах. Таким образом, если выбранное значение **временного диапазона** равно **Последние 7 дней**, отчет отображает записи за последние семь полных дней. Текущий день не включается в отчет.
- В отчете отображаются сведения о заданиях (помимо заданий журнала), которые были *активированы* в выбранном диапазоне времени.
- Значения, отображаемые для **облачного хранилища** и **защищенных экземпляров**, находятся в *конце* выбранного диапазона времени.
- Элементы резервного копирования, отображаемые в отчетах, — это элементы, которые находятся в *конце* выбранного диапазона времени. Элементы резервного копирования, удаленные в середине выбранного диапазона времени, не отображаются. Для политик резервного копирования применяется то же самое соглашение.
- Если выбранный диапазон времени занимает не более 30 дней, диаграммы подготавливаются к просмотру ежедневно, где по одной точке данных каждый день. Если диапазон времени занимает больше 30 дней и меньше (или равен) 90 дней, диаграммы подготавливаются к просмотру в неделю. Для больших диапазонов времени диаграммы подготавливаются к просмотру в месячном представлении. Статистическая обработка данных еженедельно или ежемесячно помогает повысить производительность запросов и упростить чтение данных в диаграммах.
- Сетки с соблюдением политик также следуют подобной логике агрегирования, как описано выше. Однако существует несколько незначительных отличий. Первое отличие заключается в том, что для элементов с политикой еженедельного резервного копирования нет ежедневного представления (доступны только еженедельные и месячные представления). Кроме того, в сетках для элементов с политикой еженедельного резервного копирования месяц считается 4-недельным периодом (28 дней), а не 30 днями, чтобы избежать рассмотрения частичных недель.

## <a name="query-load-times"></a>Время загрузки страницы

Мини-приложения в отчете резервного копирования созданы на основе запросов Kusto, которые выполняются в заданных пользователем рабочих областях службы анализа журналов. Эти запросы обычно предполагают обработку больших объемов данных с несколькими объединениями для предоставления расширенных аналитических данных. В результате мини-приложения могут не загружаться мгновенно, если пользователь просматривает отчеты в большом пространстве резервного копирования. Эта таблица предоставляет приблизительную оценку времени, которое может занимать загрузка различных мини-приложений, в зависимости от количества элементов резервного копирования и временного диапазона, за который просматриваются отчеты.

| **# Источники данных**                         | **Временной горизонт** | **Приблизительное время загрузки**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ~5 К                       | 1 месяц          | Плитки: 5–10 с <br> Сетки: 5–10 с <br> Диаграммы: 5–10 с <br> Фильтры уровня отчета: 5–10 с|
| ~5 К                       | 3 месяца          | Плитки: 5–10 с <br> Сетки: 5–10 с <br> Диаграммы: 5–10 с <br> Фильтры уровня отчета: 5–10 с|
| ~10 K                       | 3 месяца          | Плитки: 15–20 с <br> Сетки: 15–20 с <br> Диаграммы: 1–2 мин <br> Фильтры уровня отчета: 25–30 с|
| ~15 K                       | 1 месяц          | Плитки: 15–20 с <br> Сетки: 15–20 с <br> Диаграммы: 50–60 с <br> Фильтры уровня отчета: 20–25 с|
| ~15 K                       | 3 месяца          | Плитки: 20–30 с <br> Сетки: 20–30 с <br> Диаграммы: 2–3 мин <br> Фильтры уровня отчета: 50–60 с |

## <a name="what-happened-to-the-power-bi-reports"></a>Что случилось с отчетами Power BI?

- В настоящее время прекращается поддержка более ранней версии приложения шаблона Power BI для создания отчетов, которое получало данные из учетной записи хранения Azure. Рекомендуется начать отправку диагностических данных хранилища в службу анализа журналов для просмотра отчетов.

- Кроме того, также прекращается поддержка [схемы версии 1](./backup-azure-diagnostics-mode-data-model.md#v1-schema-vs-v2-schema) для отправки диагностических данных в учетную запись хранения или рабочую область службы анализа журналов. Это означает, что если вы написали пользовательские запросы или службы автоматизации на основе схемы версии 1, рекомендуется обновить эти запросы, чтобы использовать текущую поддерживаемую схему v2.

## <a name="next-steps"></a>Дальнейшие действия

[Дополнительные сведения о мониторинге и создании отчетов с помощью Azure Backup](./backup-azure-monitor-alert-faq.md)