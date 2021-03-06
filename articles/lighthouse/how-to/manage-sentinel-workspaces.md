---
title: Управление рабочими областями Sentinel Azure в масштабе
description: Узнайте, как эффективно управлять Sentinel Azure на делегированных ресурсах клиентов.
ms.date: 03/02/2021
ms.topic: how-to
ms.openlocfilehash: 009edaefe021dedb5d9a40a8cc3bac2c2974ae10
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "101702527"
---
# <a name="manage-azure-sentinel-workspaces-at-scale"></a>Управление рабочими областями Sentinel Azure в масштабе

Как поставщик услуг, вы могли подключить несколько клиентов клиента к [Azure лигхсаусе](../overview.md). Azure Лигхсаусе позволяет поставщикам служб выполнять операции одновременно в нескольких клиентах Azure Active Directory (Azure AD), что делает задачи управления более эффективными.

Azure Sentinel предоставляет аналитику безопасности и аналитику угроз, предоставляя единое решение для обнаружения предупреждений, отображения угроз, упреждающего поиска и реагирования на угрозы. С помощью Azure Лигхсаусе вы можете управлять несколькими рабочими областями Sentinel Azure между клиентами в масштабе. Это позволяет выполнять такие сценарии, как выполнение запросов в нескольких рабочих областях, или создание книг для визуализации и мониторинга данных из подключенных источников данных для получения ценной информации. Такие IP-адреса, как запросы и модули PlayBook, остаются в вашем управляющем клиенте, но могут использоваться для управления безопасностью клиентов.

В этом разделе приводятся общие сведения об использовании [Sentinel Azure](../../sentinel/overview.md) в качестве масштабируемого способа для обеспечения видимости между клиентами и управляемыми службами безопасности.

> [!TIP]
> Хотя в этом разделе мы будем называть поставщиков услуг и клиентов, это руководство также применяется к [предприятиям, использующим Azure лигхсаусе для управления несколькими клиентами](../concepts/enterprise.md).

## <a name="architectural-considerations"></a>Рекомендации по архитектуре

Для поставщика управляемой службы безопасности (МССП), желающего создать предложение "безопасность как услуга" с помощью Sentinel Azure, может потребоваться единый центр операций безопасности (SOC) для централизованного мониторинга, управления и настройки нескольких рабочих областей Azure Sentinel, развернутых в отдельных клиентских клиентах. Аналогичным образом, предприятиям с несколькими клиентами Azure AD может потребоваться централизованное управление несколькими рабочими областями Sentinel Azure, развернутыми в своих клиентах.

Эта централизованная модель развертывания имеет следующие преимущества.

- Владение данными остается в каждом управляемом клиенте.
- Поддерживает требования для хранения данных в пределах географических границ.
- Обеспечивает изоляцию данных, так как данные для нескольких клиентов не хранятся в одной рабочей области.
- Предотвращает утечка данных от управляемых клиентов, помогая обеспечить соответствие данных.
- Связанные затраты взимается за каждый управляемый клиент, а не на управляющий клиент.
- Данные из всех источников данных и соединителей данных, интегрированных с Azure Sentinel (например, журналы действий Azure AD, журналы Office 365 или оповещения системы защиты от угроз Майкрософт), будут находиться в каждом клиенте.
- Сокращает задержку в сети.
- Легко добавлять или удалять новые дочерние компании или клиентов.

> [!NOTE]
> Вы можете управлять делегированными ресурсами, расположенными в разных [регионах](../../availability-zones/az-overview.md#regions). Однако делегирование подписок в [национальной облаке](../../active-directory/develop/authentication-national-cloud.md) и общедоступном облаке Azure или в двух отдельных национальных облаках не поддерживается.

## <a name="granular-azure-role-based-access-control-azure-rbac"></a>Детальный контроль доступа на основе ролей Azure (Azure RBAC)

Каждая подписка клиента, которой будет управлять МССП, должна быть подключена [к Azure лигхсаусе](onboard-customer.md). Это позволяет назначенным пользователям в управляющем клиенте получать доступ и выполнять операции управления в рабочих областях Azure Sentinel, развернутых в клиентах клиентов.

При создании авторизаций можно назначить встроенные роли Azure Sentinel пользователям, группам или субъектам-службам в управляемом клиенте:

- [Читатель маркеров Azure](../../role-based-access-control/built-in-roles.md#azure-sentinel-reader)
- [Ответчик Sentinel Azure](../../role-based-access-control/built-in-roles.md#azure-sentinel-responder)
- [Участник Sentinel Azure](../../role-based-access-control/built-in-roles.md#azure-sentinel-contributor)

Также может потребоваться назначить дополнительные встроенные роли для выполнения дополнительных функций. Сведения о конкретных ролях, которые можно использовать с Azure Sentinel, см. [в разделе разрешения в Azure Sentinel](../../sentinel/roles.md).

После подключения клиентов пользователи смогут войти в управляемый клиент и [напрямую получить доступ к рабочей области Sentinel Azure для клиента](../../sentinel/multiple-tenants-service-providers.md) с назначенными ролями.

## <a name="view-and-manage-incidents-across-workspaces"></a>Просмотр инцидентов и управление ими в рабочих областях

Если вы управляете ресурсами Sentinel Azure для нескольких клиентов, вы можете просматривать и управлять инцидентами в нескольких рабочих областях одновременно для нескольких клиентов. Дополнительные сведения см. в статьях [Работа с инцидентами во многих рабочих областях](../../sentinel/multiple-workspace-view.md) и [расширение Sentinel в Azure для рабочих областей и клиентов](../../sentinel/extend-sentinel-across-workspaces-tenants.md).

> [!NOTE]
> Убедитесь, что пользователям в вашем управляющем клиенте назначены разрешения на чтение и запись для всех управляемых рабочих областей. Если у пользователя есть только разрешения на чтение в некоторых рабочих областях, то при выборе инцидентов в этих рабочих областях могут отображаться предупреждения, и пользователь не сможет изменить эти инциденты или другие, которые были выбраны вместе с ними (даже если у вас есть разрешения для других пользователей).

## <a name="configure-playbooks-for-mitigation"></a>Настройка модули PlayBook для устранения рисков

[Модули PlayBook](../../sentinel/tutorial-respond-threats-playbook.md) можно использовать для автоматического устранения рисков при активации оповещения. Эти модули PlayBook можно запускать вручную или автоматически при запуске конкретных оповещений. Модули PlayBook можно развернуть либо в управляющем клиенте, либо в клиенте клиента, с помощью процедур ответа, настроенных в зависимости от того, какие пользователи клиента должны предпринять действия в ответ на угрозу безопасности.

## <a name="create-cross-tenant-workbooks"></a>Создание книг между клиентами

[Azure Monitor книги в Azure Sentinel](../../sentinel/overview.md#workbooks) помогают визуализировать и отслеживать данные из подключенных источников данных для получения ценной информации. Вы можете использовать встроенные шаблоны книг в Azure Sentinel или создать настраиваемые книги для своих сценариев.

Вы можете развертывать книги в управляющем клиенте и создавать масштабируемые панели мониторинга для мониторинга и запроса данных в клиентах клиентов. Дополнительные сведения см. в разделе [мониторинг между рабочими областями](../../sentinel/extend-sentinel-across-workspaces-tenants.md#using-cross-workspace-workbooks). 

Можно также развертывать книги непосредственно в отдельном клиенте, который управляется для сценариев, относящихся к этому клиенту.

## <a name="run-log-analytics-and-hunting-queries-across-azure-sentinel-workspaces"></a>Выполнение Log Analytics и поиск запросов в рабочих областях Azure Sentinel

Создавайте и сохраняйте запросы Log Analytics для обнаружения угроз централизованно в управляющем клиенте, включая [запросы](../../sentinel/extend-sentinel-across-workspaces-tenants.md#cross-workspace-hunting)поиска. Затем эти запросы можно выполнять между всеми рабочими областями-Sentinel Azure ваших клиентов с помощью оператора UNION и выражения Workspace (). Дополнительные сведения см. в разделе [запросы между рабочими областями](../../sentinel/extend-sentinel-across-workspaces-tenants.md#cross-workspace-querying).

## <a name="use-automation-for-cross-workspace-management"></a>Использование автоматизации для управления между рабочими областями

Службу автоматизации можно использовать для управления несколькими рабочими областями Sentinel Azure и настройки [запросов](../../sentinel/hunting.md)на поиск, модули playbook и книг. Дополнительные сведения см. в разделе [Управление между рабочими областями с помощью службы автоматизации](../../sentinel/extend-sentinel-across-workspaces-tenants.md#cross-workspace-management-using-automation).

## <a name="monitor-security-of-office-365-environments"></a>Мониторинг безопасности сред Office 365

Используйте Azure Лигхсаусе в сочетании с Sentinel Azure, чтобы отслеживать безопасность сред Office 365 в клиентах. Во-первых, [в управляемом клиенте должны быть включены соединители данных Office 365](../../sentinel/connect-office-365.md) , чтобы сведения о действиях пользователя и администратора в Exchange и SharePoint (включая OneDrive) можно было принимать в рабочую область Sentinel Azure в управляемом клиенте. Сюда входят сведения о таких действиях, как скачивание файлов, запросы на доступ, изменения в событиях группы и операции с почтовыми ящиками, а также сведения о пользователях, которые выполнили эти действия. [Оповещения DLP для office 365](https://techcommunity.microsoft.com/t5/azure-sentinel/ingest-office-365-dlp-events-into-azure-sentinel/ba-p/1031820) также поддерживаются в составе встроенного соединителя Office 365.

Вы можете включить [соединитель Microsoft Cloud App Security (МКАС)](../../sentinel/connect-cloud-app-security.md) для потоковой передачи предупреждений и Cloud Discovery журналов в Azure Sentinel. Это позволяет получить представление о облачных приложениях, получить комплексную аналитику для определения и борьбы с киберугроз и контролировать, как передаются данные. Журналы действий для МКАС можно [использовать с помощью общего формата событий (CEF)](https://techcommunity.microsoft.com/t5/azure-sentinel/ingest-box-com-activity-events-via-microsoft-cloud-app-security/ba-p/1072849).

После настройки соединителей данных Office 365 можно использовать возможности Sentinel Azure для разных клиентов, такие как просмотр и анализ данных в книгах, использование запросов для создания пользовательских оповещений и настройка модули PlayBook для реагирования на угрозы.

## <a name="next-steps"></a>Дальнейшие действия

- Подробнее об [Azure Sentinel](../../sentinel/overview.md).
- Ознакомьтесь со [страницей с расценками Azure Sentinel](https://azure.microsoft.com/pricing/details/azure-sentinel/).
- Узнайте больше об [интерфейсах управления для различных клиентов](../concepts/cross-tenant-management-experience.md).

