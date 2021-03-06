---
title: Поддержка оценки VMware в службе "Миграция Azure"
description: Узнайте о поддержке оценки для серверов, работающих в среде VMware, с помощью средства обнаружения и оценки службы "миграция Azure"
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: conceptual
ms.date: 03/17/2021
ms.openlocfilehash: 4d51fc13e3587c21a7340b35db10d3cf36ab74b5
ms.sourcegitcommit: f0a3ee8ff77ee89f83b69bc30cb87caa80f1e724
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2021
ms.locfileid: "105557553"
---
# <a name="support-matrix-for-vmware-assessment"></a>Таблица поддержки для оценки виртуальных машин VMware 

В этой статье приведены требования к предварительным требованиям и поддержке при обнаружении и оценке серверов, работающих в среде VMware, для миграции в Azure с помощью средства " [Миграция Azure: обнаружение и оценка](migrate-services-overview.md#azure-migrate-discovery-and-assessment-tool) ". Чтобы оценить серверы, создайте проект, который добавляет в проект средство "миграция и оценка Azure". После добавления средства разверните Устройство Миграции Azure. Устройство постоянно обнаруживает локальные серверы и отправляет метаданные конфигурации и производительности в Azure. После завершения обнаружения вы собираете обнаруженные серверы в группы и выполняете оценку группы.

Если вы хотите перенести серверы VMware в Azure, изучите [таблицу поддержки миграции](migrate-support-matrix-vmware-migration.md).



## <a name="limitations"></a>Ограничения

**Требования** | **Сведения**
--- | ---
**Ограничения проекта** | Для одной подписки Azure можно создать несколько проектов.<br/><br/> В одном [проекте](migrate-support-matrix.md#project)можно обнаружить и оценить до 50 000 серверов из среды VMware. Проект также может включать физические серверы и серверы из среды Hyper-V вплоть до ограничений оценки.
**Discovery** | Устройство для миграции Azure может обнаруживать до 10 000 серверов на vCenter Server.
**Оценка** | В одну группу можно добавить до 35 000 серверов.<br/><br/> Вы можете оценить до 35 000 серверов за одну оценку.

[Дополнительные сведения](concepts-assessment-calculation.md) об оценках.


## <a name="vmware-requirements"></a>Требования для VMware

**VMware** | **Сведения**
--- | ---
**vCenter Server** | Серверы, которые требуется обнаружить и оценить, должны управляться с помощью vCenter Server версии 5,5, 6,0, 6,5, 6,7 или 7,0.<br/><br/> Обнаружение серверов путем предоставления сведений об узле ESXi в устройстве в настоящее время не поддерживается.
**Разрешения** | Миграция Azure. для обнаружения и оценки требуется vCenter Server учетную запись только для чтения для обнаружения и оценки.<br/><br/> Если вы хотите выполнить обнаружение инвентаризации программного обеспечения и анализ зависимостей без агента, учетной записи требуются права для  >  **гостевых операций** виртуальных машин.

## <a name="server-requirements"></a>Требования к серверу
**VMware** | **Сведения**
--- | ---
**Поддерживаемые ОС** | Все операционные системы Windows и Linux можно оценить для миграции.
**Память** | Поддерживаются диски, подключенные к контроллерам SCSI, IDE и SATA.


## <a name="azure-migrate-appliance-requirements"></a>Требования к устройству Миграции Azure

Служба "Миграция Azure" использует [устройство Миграции Azure](migrate-appliance.md) для обнаружения и оценки. Вы можете развернуть устройство в качестве сервера в среде VMware с помощью шаблона OVA, импортированного в vCenter Server или с помощью [скрипта PowerShell](deploy-appliance-script.md).

- Узнайте больше о [требованиях к устройству](migrate-appliance.md#appliance---vmware) для VMware.
- В Azure для государственных организаций необходимо развернуть устройство [с помощью скрипта](deploy-appliance-script-government.md).
- Проверьте URL-адреса, необходимые устройству для доступа в [общедоступных](migrate-appliance.md#public-cloud-urls) и [правительственных](migrate-appliance.md#government-cloud-urls) облаках.


## <a name="port-access-requirements"></a>Требования к доступу к портам

**Устройство** | **Соединение**
--- | ---
**Устройство** | Входящие подключения через TCP-порт 3389 для подключения удаленного рабочего стола к устройству.<br/><br/> Входящие подключения через порт 44368 для удаленного доступа к приложению управления устройством с помощью URL-адреса: ```https://<appliance-ip-or-name>:44368``` <br/><br/>Исходящие подключения через порт 443 (HTTPS) для отправки метаданных обнаружения и производительности в службу "Миграция Azure".
**Сервер vCenter** | Входящие подключения через TCP-порт 443, чтобы разрешить устройству получать метаданные конфигурации и производительности для оценки. <br/><br/> По умолчанию устройство подключается к vCenter через порт 443. Если vCenter Server ожидает передачи данных через другой порт, можно изменить порт при настройке обнаружения.
**Узлы ESXi** | Если вы хотите выполнить [Обнаружение инвентаризации программного обеспечения](how-to-discover-applications.md)или [анализ зависимостей без агента](concepts-dependency-visualization.md#agentless-analysis), устройство подключается к узлам ESXi через TCP-порт 443, чтобы обнаружить инвентаризацию программного обеспечения и зависимости на серверах.

## <a name="application-discovery-requirements"></a>Требования к обнаружению приложений

Помимо обнаружения серверов, служба "миграция Azure" позволяет обнаруживать и оценивать инвентаризацию программного обеспечения, выполняемого на серверах. Обнаружение приложений позволяет выявление и планирование пути миграции, предназначенного для локальных рабочих нагрузок.

**Поддержка** | **Сведения**
--- | ---
**Поддерживаемые серверы** | В настоящее время поддерживается только для серверов в среде VMware. Обнаружение приложений можно выполнять на 10000 серверах, начиная с каждого устройства, выполняющего миграцию Azure.
**Операционные системы** | Поддерживаются серверы под управлением всех версий Windows и Linux.
**Требования к виртуальной машине** | Чтобы выполнить обнаружение инвентаризации программного обеспечения, на серверах должны быть установлены и запускаться средства VMware. <br/><br/> Потребуются средства VMware версии после 10.2.0.<br/><br/> Для серверов Windows требуется PowerShell версии 2.0 или более поздней.
**Discovery** | Обнаружение приложений на серверах выполняется из vCenter Server с помощью средств VMware, установленных на серверах. Устройство собирает сведения об инвентаризации программного обеспечения из vCenter Server с помощью API-интерфейсов vSphere. Обнаружение приложений — без агента. На сервере не установлен агент, и устройство не подключается напрямую к серверам. WMI и SSH должны быть включены и доступны на серверах Windows и Linux соответственно.
**Учетная запись vCenter Server пользователя** | VCenter Server учетной записи только для чтения, используемой для оценки, требуются права для  >  **гостевых операций** виртуальных машин, чтобы взаимодействовать с серверами для обнаружения приложений.
**Доступ к серверу** | Вы можете добавить несколько доменных и недоменных учетных данных (Windows/Linux) в диспетчере конфигурации устройства для обнаружения приложений.<br/><br/> Вам потребуется учетная запись гостевого пользователя для серверов Windows, а также обычная или обычная учетная запись пользователя (без доступа sudo) для всех серверов Linux.
**Доступ к портам** | Устройство миграции Azure должно иметь возможность подключения к TCP-порту 443 на узлах ESXi, на которых выполняются серверы, на которых требуется выполнять обнаружение приложений. VCenter Server возвращает подключение к узлу ESXi для загрузки файла, содержащего сведения об инвентаризации программного обеспечения.

## <a name="requirements-for-discovery-of-sql-server-instances-and-databases"></a>Требования к обнаружению экземпляров SQL Server и баз данных

[Обнаружение приложений](how-to-discover-applications.md) определяет экземпляры SQL Server. Используя эти сведения, устройство пытается подключиться к соответствующим экземплярам SQL Server через проверку подлинности Windows или SQL Server учетные данные для проверки подлинности, указанные на устройстве. После подключения устройство собирает данные о конфигурации и производительности SQL Server экземпляров и баз данных. Данные конфигурации SQL Server обновляются каждые 24 часа, а данные о производительности фиксируются каждые 30 секунд.

**Поддержка** | **Сведения**
--- | ---
**Поддерживаемые серверы** | В настоящее время поддерживается только для серверов SQL Server в среде VMware. Можно обнаружить до 300 SQL Server экземпляров или 6000 баз данных SQL, в зависимости от того, что меньше.
**Серверы Windows** | Поддерживаются Windows Server 2008 и более поздние версии.
**Серверы Linux** | В настоящее время не поддерживается.
**Механизм аутентификации** | Поддерживаются проверка подлинности Windows и SQL Server. Вы можете указать учетные данные обоих типов проверки подлинности в диспетчере конфигурации устройства.
**Доступ к SQL Server** | Для службы миграции Azure требуется учетная запись пользователя Windows, которая является членом роли сервера sysadmin.
**Версии SQL Server** | Поддерживаются SQL Server 2008 и более поздних версий.
**Выпуски SQL Server** | Поддерживаются выпуски Enterprise, Standard, Developer и Express.
**Поддерживаемая конфигурация SQL** | В настоящее время поддерживается обнаружение только автономных SQL Serverных экземпляров и соответствующих баз данных.<br/> Идентификация отказоустойчивого кластера и группы доступности Always On не поддерживается.
**Поддерживаемые службы SQL** | Поддерживается только SQL Server ядро СУБД. <br/> Обнаружение SQL Server Reporting Services (SSRS), SQL Server Integration Services (SSIS) и SQL Server Analysis Services (SSAS) не поддерживается.

> [!Note]
> Служба "Миграция Azure" шифрует обмен данными между экземплярами устройства службы "Миграция Azure" и исходными экземплярами SQL Server (для свойства "Шифровать соединение" установлено значение TRUE). Если соединения зашифрованы с помощью [**TrustServerCertificate**](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.trustservercertificate) (установлено значение TRUE), то транспортный уровень будет использовать протокол SSL для шифрования канала и не пойдет по цепочке сертификатов для проверки доверия. Для сервера устройства необходимо настроить [**доверие корневому центру сертификата**](/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine).<br/>
Если сертификат не был предоставлен на сервере при запуске, SQL Server создает самозаверяющий сертификат, который используется для шифрования пакетов входа. [**См. дополнительные сведения**](/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine).

## <a name="dependency-analysis-requirements-agentless"></a>Требования к анализу зависимостей (без агента)

[Анализ зависимостей](concepts-dependency-visualization.md) помогает определить зависимости между локальными серверами, которые вы хотите оценить и перенести в Azure. В таблице перечислены требования для настройки анализа зависимостей без агента.

**Поддержка** | **Сведения**
--- | --- 
**Поддерживаемые серверы** | В настоящее время поддерживается только для серверов в среде VMware.
**Серверы Windows** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64-разрядная версия).<br/>Microsoft Windows Server 2008 (32-bit).
**Серверы Linux** | Red Hat Enterprise Linux 7, 6, 5<br/> Ubuntu Linux 14.04, 16.04<br/> Debian 7, 8.<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7.<br/> SUSE Linux Enterprise Server 11 и более поздних версий.
**Требования к серверу** | Средства VMware (более поздние, чем 10.2.0) должны быть установлены и запускаться на серверах, которые вы хотите проанализировать.<br/><br/> На серверах должна быть установлена оболочка PowerShell версии 2,0 или более поздней.
**Метод обнаружения** |  Сведения о зависимостях между серверами собираются из vCenter Server с помощью средств VMware, установленных на сервере. Устройство собирает сведения из vCenter Server с помощью API-интерфейсов vSphere. На сервере не установлен агент, и устройство не подключается напрямую к серверам. WMI и SSH должны быть включены и доступны на серверах Windows и Linux соответственно.
**Учетная запись vCenter** | Учетная запись только для чтения, используемая службой "миграция Azure" для оценки, требует, чтобы для **виртуальных машин > гостевые операции** были включены права.
**Разрешения Windows Server** |  Учетная запись пользователя (локальная или Доменная) с разрешениями администратора на серверах.
**Учетная запись Linux** | Корневая учетная запись пользователя или учетная запись с такими разрешениями для файлов/бин/нетстат и/бин/ЛС: CAP_DAC_READ_SEARCH и CAP_SYS_PTRACE.<br/><br/> Задайте эти возможности с помощью следующих команд: <br/><br/> sudo сеткап CAP_DAC_READ_SEARCH, CAP_SYS_PTRACE = EP/бин/ЛС<br/><br/> sudo сеткап CAP_DAC_READ_SEARCH, CAP_SYS_PTRACE = EP/бин/нетстат
**Доступ к портам** | Устройство миграции Azure должно иметь возможность подключения к TCP-порту 443 на узлах ESXi с серверами, зависимости которых вы хотите обнаружить. VCenter Server возвращает подключение к узлу ESXi для загрузки файла, содержащего данные зависимостей.


## <a name="dependency-analysis-requirements-agent-based"></a>Требования к анализу зависимостей (на основе агентов)

[Анализ зависимостей](concepts-dependency-visualization.md) помогает определить зависимости между локальными серверами, которые вы хотите оценить и перенести в Azure. В таблице перечислены требования для настройки анализа зависимостей на основе агента.

**Требование** | **Сведения** 
--- | --- 
**Перед развертыванием** | У вас должен быть проект с помощью средства "миграция Azure: обнаружение и оценка", добавленного в проект.<br/><br/>  Визуализация зависимостей развертывается после настройки устройства миграции Azure для обнаружения локальных серверов.<br/><br/> [Узнайте](create-manage-projects.md), как создать проект в первый раз.<br/> [Узнайте, как](how-to-assess.md) добавить средство обнаружения и оценки в существующий проект.<br/> Узнайте, как настроить устройство Миграции Azure для оценки [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md) или физических серверов.
**Поддерживаемые серверы** | Поддерживается для всех серверов в локальной среде.
**Служба Log Analytics** | Служба "Миграция Azure" использует решение [Сопоставление служб](../azure-monitor/insights/service-map.md) в [журналах Azure Monitor](../azure-monitor/log-query/log-query-overview.md) для визуализации зависимостей.<br/><br/> Вы связываете новую или существующую рабочую область Log Analytics с проектом. Рабочую область проекта нельзя изменить после добавления. <br/><br/> Рабочая область должна находиться в той же подписке, что и проект.<br/><br/> Рабочая область должна находиться в следующих регионах: Восточная часть США, Юго-Восточная Азия или Западная Европа. Рабочие области в других регионах не могут быть связаны с проектом.<br/><br/> Рабочая область должна находиться в регионе, в котором [поддерживается Сопоставление служб](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions).<br/><br/> В Log Analytics рабочей области, связанной с миграцией Azure, помечается ключ проекта и имя проекта.
**Необходимые агенты** | На каждом сервере, который необходимо проанализировать, установите следующие агенты:<br/><br/> [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md).<br/> [Агент зависимостей](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Если локальные серверы не подключены к Интернету, необходимо скачать и установить шлюз Log Analytics.<br/><br/> Узнайте больше об установке [агента зависимостей](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) и [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Рабочая область Log Analytics** | Рабочая область должна находиться в той же подписке, что и проект.<br/><br/> Миграция Azure поддерживает рабочие области в следующих регионах: Восточная часть США, Юго-Восточная Азия и Западная Европа.<br/><br/>  Рабочая область должна находиться в регионе, в котором [поддерживается Сопоставление служб](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions).<br/><br/> Рабочую область проекта нельзя изменить после добавления.
**Стоимость** | На Сопоставление служб решение не взимается плата за первые 180 дней (из дня, когда вы связываете рабочую область Log Analytics с проектом)/<br/><br/> По истечении 180 дней применяются стандартные тарифы Log Analytics.<br/><br/> За использование в этой рабочей области Log Analytics любого другого решения, помимо "Сопоставление служб", взимается [стандартная плата](https://azure.microsoft.com/pricing/details/log-analytics/) Log Analytics.<br/><br/> При удалении проекта Рабочая область не удаляется вместе с ней. После удаления проекта за использование решения "Сопоставление служб" будет взиматься плата за каждый узел в соответствии с ценовым уровнем рабочей области Log Analytics.<br/><br/>Если у вас есть проекты, созданные до выхода общедоступной версии Миграция Azure (от 28 февраля 2018 г.), может взиматься дополнительная плата за Сопоставление служб. Чтобы не платить за службу 180 дней, рекомендуется создать новый проект, так как существующие рабочие области до общедоступной версии будут платными.
**Управление** | При регистрации агентов в рабочей области используется идентификатор и ключ, предоставленные проектом.<br/><br/> Рабочую область Log Analytics можно использовать за пределами службы "Миграция Azure".<br/><br/> При удалении связанного проекта Рабочая область не удаляется автоматически. [Удалите ее вручную](../azure-monitor/platform/manage-access.md).<br/><br/> Не удаляйте рабочую область, созданную службой "миграция Azure", если вы не удалите проект. В противном случае функция визуализации зависимостей не будет работать должным образом.
**Подключение к Интернету** | Если серверы не подключены к Интернету, необходимо установить на них шлюз Log Analytics.
**Azure для государственных организаций** | Анализ зависимостей на основе агента не поддерживается.


## <a name="next-steps"></a>Дальнейшие действия

- [Просмотрите](best-practices-assessment.md) рекомендации для создания оценок.
- [Подготовьтесь к оценке VMware](./tutorial-discover-vmware.md).