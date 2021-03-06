---
title: Настройка области обнаружения серверов в VMware с помощью службы "миграция Azure"
description: Описывает, как задать область обнаружения для серверов, размещенных в службе "Оценка и миграция VMware", с помощью службы "миграция Azure".
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: how-to
ms.date: 03/13/2021
ms.openlocfilehash: 29ac42da6560a717f12cd256fdd71282e0bd313f
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104775363"
---
# <a name="set-discovery-scope-for-servers-in-vmware-environment"></a>Задать область обнаружения для серверов в среде VMware

В этой статье описывается, как ограничить область обнаружения для серверов в среде VMware в следующих случаях:

- Обнаружение серверов с помощью устройства " [Миграция Azure](migrate-appliance-architecture.md) " при использовании средства "миграция Azure": обнаружение и оценка.
- Обнаружение серверов с помощью устройства "миграция [Azure](migrate-appliance-architecture.md) ", когда вы используете средство миграции для Azure Migration: Server, для переноса серверов из среды VMware в Azure без агента.

При настройке устройства оно подключается к vCenter Server и запускает обнаружение. Перед подключением устройства к vCenter Server можно ограничить обнаружение vCenter Server центрами обработки данных, кластерами, папкой кластеров, узлами, папкой узлов или отдельными серверами. Чтобы задать область, необходимо назначить разрешения учетной записи, которую устройство использует для доступа к vCenter Server.

## <a name="before-you-start"></a>Прежде чем начать

Если вы не настроили учетную запись пользователя vCenter, используемую службой "миграция Azure" для обнаружения, сделайте это сейчас для [оценки](./tutorial-discover-vmware.md#prepare-vmware) или [безагентного переноса](./migrate-support-matrix-vmware-migration.md#agentless-migration).


## <a name="assign-permissions-and-roles"></a>Назначение разрешений и ролей

Вы можете назначать разрешения для объектов инвентаризации VMware с помощью одного из двух методов:

- В учетной записи, используемой устройством, назначьте роль с необходимыми разрешениями для объектов, для которых необходимо задать область.
- Кроме того, можно назначить роль учетной записи на уровне центра обработки данных и распространить ее на дочерние объекты. Затем предоставьте учетной записи роль " **нет доступа** " для каждого объекта, который вы не хотите использовать в области. Этот подход не рекомендуется, так как он громоздкий и может предоставлять доступ к элементам управления доступом, так как каждому новому дочернему объекту автоматически предоставляется доступ, наследуемый от родительского объекта.

На уровне папки vCenter Server нельзя обнаружить область обнаружения запасов. Если необходимо определить область для серверов в папке, создайте пользователя и предоставьте доступ к каждому требуемому серверу по отдельности. Поддерживаются папки узлов и кластеров.


### <a name="assign-a-role-for-assessment"></a>Назначение роли для оценки

1. В учетной записи vCenter, используемой для обнаружения, примените роль **только для чтения** для всех родительских объектов, на которых размещены серверы, на которых нужно обнаружить и оценить (узел, кластер, папка hosts, папка Clusters, вплоть до центра обработки данных).
2. Распространить эти разрешения на дочерние объекты в иерархии.

    ![Назначение разрешений](./media/tutorial-assess-vmware/assign-perms.png)

### <a name="assign-a-role-for-agentless-migration"></a>Назначение роли для миграции без агента

1. В учетной записи vCenter устройства, которую вы используете для миграции, примените определяемую пользователем роль с [необходимыми разрешениями](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless)ко всем родительским объектам, на которых размещены серверы, которые требуется обнаружить и перенести.
2. Можно присвоить роли имя, которое проще найти. Например, <em>Azure_Migrate</em>.

## <a name="work-around-for-server-folder-restriction"></a>Обходное решение для ограничения папки сервера

В настоящее время средство "миграция Azure: обнаружение и оценка" не может обнаружить серверы, если доступ предоставляется на уровне папки vCenter Server. Если вы хотите ограничить поиск и оценку по папкам сервера, используйте это решение.

1. Назначьте разрешения только на чтение для всех серверов, расположенных в папках, для которых необходимо выполнить обнаружение и оценку.
2. Предоставьте доступ только для чтения всем родительским объектам, на которых размещены узлы серверов, кластер, папка hosts, папка Clusters, вплоть до центра обработки данных. Вам не нужно распространять разрешения на все дочерние объекты.
3. Чтобы использовать учетные данные для обнаружения, выберите центр обработки данных в качестве **области коллекции**.


Настройка управления доступом на основе ролей гарантирует, что соответствующая учетная запись пользователя vCenter имеет доступ только к серверам, зависящим от клиента.


## <a name="next-steps"></a>Дальнейшие действия

[Настройка устройства](how-to-set-up-appliance-vmware.md)