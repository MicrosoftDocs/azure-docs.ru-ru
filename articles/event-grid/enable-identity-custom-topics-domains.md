---
title: Включение управляемого удостоверения в пользовательских разделах и доменах службы "Сетка событий Azure"
description: В этой статье описывается, как включить управляемое удостоверение службы для настраиваемого раздела или домена сетки событий Azure.
ms.topic: how-to
ms.date: 03/25/2021
ms.openlocfilehash: 06fd4d6e472b33496e773596b0f3afc8e70be948
ms.sourcegitcommit: a9ce1da049c019c86063acf442bb13f5a0dde213
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105630537"
---
# <a name="assign-a-system-managed-identity-to-an-event-grid-custom-topic-or-domain"></a>Назначение управляемого системой удостоверения для пользовательского раздела или домена сетки событий 
В этой статье показано, как включить управляемое системой удостоверение для пользовательского раздела или домена сетки событий. Дополнительные сведения об управляемых удостоверениях см. в статье [что такое управляемые удостоверения для ресурсов Azure](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="enable-identity-at-the-time-of-creation"></a>Включить удостоверение во время создания

### <a name="using-azure-portal"></a>Использование портала Azure
Вы можете включить назначенное системой удостоверение для пользовательского раздела или домена при его создании в портал Azure. На следующем рисунке показано, как включить управляемое системой удостоверение для пользовательского раздела. Фактически можно выбрать параметр **Включить назначенное системой удостоверение** на странице **Дополнительно** мастера создания разделов. Этот параметр также отображается на странице **Дополнительно** мастера создания домена. 

![Включить удостоверение при создании пользовательского раздела](./media/managed-service-identity/create-topic-identity.png)

### <a name="using-azure-cli"></a>Использование Azure CLI
Можно также использовать Azure CLI для создания пользовательского раздела или домена с удостоверением, назначенным системой. Используйте команду `az eventgrid topic create` с параметром `--identity`, для которого задано значение `systemassigned`. Если не указать значение для этого параметра, используется значение по умолчанию `noidentity`. 

```azurecli-interactive
# create a custom topic with a system-assigned identity
az eventgrid topic create -g <RESOURCE GROUP NAME> --name <TOPIC NAME> -l <LOCATION>  --identity systemassigned
```

Аналогичным образом можно использовать команду `az eventgrid domain create`, чтобы создать домен с удостоверением, управляемым системой.

## <a name="enable-identity-for-an-existing-custom-topic-or-domain"></a>Включение удостоверения для существующего пользовательского раздела или домена
В этом разделе вы узнаете, как включить управляемое системой удостоверение для существующего пользовательского раздела или домена. 

### <a name="using-azure-portal"></a>Использование портала Azure
В следующей процедуре показано, как включить управляемое системой удостоверение для пользовательского раздела. Действия по включению удостоверения для домена похожи. 

1. Перейдите на [портал Azure](https://portal.azure.com).
2. Поиск **разделов сетки событий** на панели поиска вверху.
3. Выберите **пользовательский раздел** , для которого необходимо включить управляемое удостоверение. 
4. Перейдите на вкладку **Удостоверение**. 
5. Включите **параметр, чтобы** включить удостоверение. 
1. Нажмите кнопку **сохранить** на панели инструментов, чтобы сохранить параметр. 

    :::image type="content" source="./media/managed-service-identity/identity-existing-topic.png" alt-text="Страница удостоверений для пользовательского раздела"::: 

Аналогичные действия можно использовать для включения удостоверения для домена сетки событий.

### <a name="use-the-azure-cli"></a>Использование Azure CLI
Используйте команду со значением, равным, `az eventgrid topic update` `--identity` `systemassigned` чтобы включить назначенное системой удостоверение для существующего пользовательского раздела. Чтобы отключить удостоверение, укажите значение `noidentity`. 

```azurecli-interactive
# Update the topic to assign a system-assigned identity. 
az eventgrid topic update -g $rg --name $topicname --identity systemassigned --sku basic 
```

Для обновления существующего домена используется аналогичная команда (`az eventgrid domain update`).


## <a name="next-steps"></a>Дальнейшие шаги
Добавьте удостоверение в соответствующую роль (например, отправитель данных служебной шины) в целевом расположении (например, в очередь служебной шины). Подробные инструкции см. [в статье Добавление удостоверения в роли Azure в местах назначения](add-identity-roles.md). 
