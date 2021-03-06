---
title: Включение управляемого удостоверения для событий маршрутизации (Предварительная версия) — CLI
titleSuffix: Azure Digital Twins
description: Узнайте, как включить назначенное системой удостоверение для Azure Digital двойников и использовать его для перенаправления событий с помощью Azure CLI.
author: baanders
ms.author: baanders
ms.date: 02/09/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: c9ce87584373bd87a8f89ecb4ea692b44d3fab4d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102202966"
---
# <a name="enable-a-managed-identity-for-routing-azure-digital-twins-events-preview-azure-cli"></a>Включение управляемого удостоверения для маршрутизации событий цифровых двойников Azure (Предварительная версия): Azure CLI

[!INCLUDE [digital-twins-managed-service-identity-selector.md](../../includes/digital-twins-managed-service-identity-selector.md)]

В этой статье описывается, как включить [назначенное системой удостоверение для экземпляра Azure Digital двойников](concepts-security.md#managed-identity-for-accessing-other-resources-preview) (сейчас находится в предварительной версии) и использовать удостоверение при пересылке событий в Поддерживаемые назначения, такие как [концентратор событий](../event-hubs/event-hubs-about.md), назначения [служебной шины](../service-bus-messaging/service-bus-messaging-overview.md)   и [контейнер хранилища Azure](../storage/blobs/storage-blobs-introduction.md).

В этой статье описывается процесс, использующий [**Azure CLI**](/cli/azure/what-is-azure-cli).

Ниже приведены действия, описанные в этой статье. 

1. Создайте экземпляр Azure Digital двойников с назначенным системой удостоверением или включите назначенное системой удостоверение для существующего экземпляра Azure Digital двойников. 
1. Добавьте в удостоверение соответствующую роль или роли. Например, назначьте роль **отправителя данных концентратора событий Azure** удостоверению, если конечная точка является концентратором событий, или **роль отправителя данных служебной шины Azure** , если конечная точка является служебной.
1. Создайте конечную точку в Azure Digital двойников, которая сможет использовать назначенные системой удостоверения для проверки подлинности.

## <a name="enable-system-managed-identities-for-an-instance"></a>Включение управляемых системой удостоверений для экземпляра 

При включении в вашем экземпляре Azure Digital двойников удостоверения, назначенного системой, Azure автоматически создает для него удостоверение в [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md). Затем это удостоверение можно использовать для проверки подлинности конечных точек цифровых двойников Azure для пересылки событий.

Вы можете включить управляемые системой удостоверения для экземпляра Azure Digital двойников **в ходе начальной установки экземпляра** или **включить его позже в экземпляре, который уже существует**.

Любой из этих методов создания даст одинаковые параметры конфигурации и тот же конечный результат для своего экземпляра. В этом разделе описывается, как выполнять оба действия.

### <a name="add-a-system-managed-identity-during-instance-creation"></a>Добавить управляемое системой удостоверение во время создания экземпляра

В этом разделе вы узнаете, как включить управляемое системой удостоверение в экземпляре Azure Digital двойников, который в данный момент создается. 

Это можно сделать, добавив `--assign-identity` параметр в `az dt create` команду, которая используется для создания экземпляра. (Дополнительные сведения об этой команде см. в [справочной документации](/cli/azure/ext/azure-iot/dt#ext_azure_iot_az_dt_create) или [общих инструкциях по настройке экземпляра Azure Digital двойников](how-to-set-up-instance-cli.md#create-the-azure-digital-twins-instance)).

Чтобы создать экземпляр с управляемым системой удостоверением, добавьте  `--assign-identity` параметр следующим образом:

```azurecli-interactive
az dt create -n {new_instance_name} -g {resource_group} --assign-identity
```

### <a name="add-a-system-managed-identity-to-an-existing-instance"></a>Добавление управляемого системой удостоверения в существующий экземпляр

В этом разделе вы добавите управляемое системой удостоверение в уже существующий экземпляр цифрового двойников Azure.

Это также делается с помощью `az dt create` команды и `--assign-identity` параметра. Вместо указания нового имени создаваемого экземпляра можно указать имя уже существующего экземпляра, чтобы обновить значение `--assign-identity` для этого экземпляра.

Команда для **включения** управляемого удостоверения аналогична команде для создания экземпляра с управляемым системой удостоверением. Все эти изменения являются значением параметра имени экземпляра:

```azurecli-interactive
az dt create -n {name_of_existing_instance} -g {resource_group} --assign-identity
```

Чтобы **Отключить** управляемое удостоверение в экземпляре, где он включен, используйте следующую команду, чтобы установить `--assign-identity` для значение `false` .

```azurecli-interactive
az dt create -n {name_of_existing_instance} -g {resource_group} --assign-identity false
```

## <a name="assign-azure-roles-to-the-identity"></a>Назначение ролей Azure удостоверению 

После создания для своего экземпляра Azure Digital двойников удостоверения, назначенного системой, необходимо назначить ей соответствующие роли для проверки подлинности с разными типами [конечных точек](concepts-route-events.md) для перенаправления событий в поддерживаемые места назначения. В этом разделе описываются параметры ролей и способы их назначения назначенному системой удостоверению.

>[!NOTE]
> Это важный шаг, без которого удостоверение не сможет получить доступ к конечным точкам и событиям не будет доставлено.

### <a name="supported-destinations-and-azure-roles"></a>Поддерживаемые назначения и роли Azure 

Ниже приведены минимальные роли, которым удостоверение требуется для доступа к конечной точке в зависимости от типа назначения. Также будут работать роли с более высоким уровнем разрешений (например, роли владельца данных).

| Назначение | Роль Azure |
| --- | --- |
| Центры событий Azure | Отправитель данных Центров событий Azure |
| Azure Service Bus | Отправитель данных Служебной шины Azure |
| Контейнер хранилища Azure | Участник данных BLOB-объектов хранилища |

Дополнительные сведения о конечных точках, маршрутах и типах назначений, поддерживаемых для маршрутизации в Azure Digital двойников, см. в разделе [*Основные понятия: маршруты событий*](concepts-route-events.md).

### <a name="assign-the-role"></a>Назначение роли

[!INCLUDE [digital-twins-permissions-required.md](../../includes/digital-twins-permissions-required.md)]

Можно добавить `--scopes` параметр `az dt create` в команду, чтобы назначить удостоверение одной или нескольким областям с указанной ролью. Его можно использовать при первом создании экземпляра или позже, передав имя уже существующего экземпляра.

Ниже приведен пример, который создает экземпляр с управляемым системой удостоверением и присваивает этому удостоверению настраиваемую роль, вызываемую `MyCustomRole` в концентраторе событий.

```azurecli-interactive
az dt create -n {instance_name} -g {resource_group} --assign-identity --scopes "/subscriptions/<subscription ID>/resourceGroups/<resource_group>/providers/Microsoft.EventHub/namespaces/<Event_Hubs_namespace>/eventhubs/<event_hub_name>" --role MyCustomRole
```

Дополнительные примеры назначения ролей с помощью этой команды см. в [справочной документации по **AZ DT**](/cli/azure/ext/azure-iot/dt#ext_azure_iot_az_dt_create).

Кроме того, для создания ролей и управления ими можно также использовать группу команд с [**назначением ролей AZ**](/cli/azure/role/assignment) . Это можно использовать для поддержки дополнительных сценариев, когда не нужно группировать назначение ролей с помощью команды Create.

## <a name="create-an-endpoint-with-identity-based-authentication"></a>Создание конечной точки с проверкой подлинности на основе удостоверений

После настройки управляемого системой удостоверения для своего экземпляра Azure Digital двойников и назначения ему соответствующих ролей можно создать [конечные точки](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins) цифровых двойников Azure, которые могут использовать удостоверение для проверки подлинности. Этот параметр доступен только для конечных точек концентратора событий и служебной шины (он не поддерживается для службы "Сетка событий").

>[!NOTE]
> Вы не можете изменить конечную точку, которая уже была создана с помощью удостоверения на основе ключа, чтобы изменить проверку подлинности на основе удостоверений. При создании конечной точки необходимо выбрать тип проверки подлинности.

Это можно сделать, добавив `--auth-type` параметр в `az dt endpoint create` команду, которая используется для создания конечной точки. (Дополнительные сведения об этой команде см. в [справочной документации](/cli/azure/ext/azure-iot/dt/endpoint/create) или [общих инструкциях по настройке конечной точки Azure Digital двойников](how-to-manage-routes-apis-cli.md#create-the-endpoint)).

Чтобы создать конечную точку, использующую проверку подлинности на основе удостоверений, укажите `IdentityBased` тип проверки подлинности с помощью  `--auth-type` параметра. В приведенном ниже примере это показано в конечной точке концентраторов событий.

```azurecli-interactive
az dt endpoint create eventhub --endpoint-name {endpoint_name} --eventhub-resource-group {eventhub_resource_group} --eventhub-namespace {eventhub_namespace} --eventhub {eventhub_name} --auth-type IdentityBased -n {instance_name}
```

## <a name="considerations-for-disabling-system-managed-identities"></a>Рекомендации по отключению управляемых системой удостоверений

Так как удостоверение управляется отдельно от конечных точек, которые его используют, важно учитывать последствия, которые любые изменения в удостоверении или его ролях могут иметь на конечных точках в вашем экземпляре Azure Digital двойников. Если удостоверение отключено или для конечной точки удаляется необходимая роль, конечная точка может стать недоступной и поток событий будет нарушен.

Чтобы продолжить использовать конечную точку, настроенную с управляемым удостоверением, который теперь отключен, необходимо удалить конечную точку и [создать ее повторно](how-to-manage-routes-apis-cli.md#create-an-endpoint-for-azure-digital-twins) с другим типом проверки подлинности. Возобновление доставки в конечную точку после этого изменения может занять до часа.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения об управляемых удостоверениях в Azure AD: 
* [*Управляемые удостоверения для ресурсов Azure*](../active-directory/managed-identities-azure-resources/overview.md)