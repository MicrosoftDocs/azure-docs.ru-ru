---
title: Изменение ролей ресурсов для пакета Access в управлении назначениями Azure AD. Azure Active Directory
description: Узнайте, как изменить роли ресурсов для существующего пакета Access в Azure Active Directory управлении обслуживанием.
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 12/14/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 65f69cf492ec3e28d7f4aa86971dc6c91b34bdf5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "101644186"
---
# <a name="change-resource-roles-for-an-access-package-in-azure-ad-entitlement-management"></a>Изменение ролей ресурсов для пакета Access в управлении назначениями Azure AD

Диспетчер пакетов Access позволяет изменять ресурсы в пакете Access в любое время, не беспокоясь о подготовке доступа пользователя к новым ресурсам или удалении доступа из предыдущих ресурсов. В этой статье описывается, как изменить роли ресурса для существующего пакета Access.

В этом видео представлен обзор того, как изменить пакет Access.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3LD4Z]

## <a name="check-catalog-for-resources"></a>Проверка каталога на наличие ресурсов

Если необходимо добавить ресурсы в пакет Access, следует проверить, доступны ли ресурсы, необходимые в каталоге. Если вы являетесь диспетчером пакетов Access, вы не можете добавлять ресурсы в каталог, даже если вы его владеете. Вы ограничены использованием ресурсов, доступных в каталоге.

**Требуемая роль:** Глобальный администратор, Администратор пользователей, Владелец каталога или Диспетчер пакетов для доступа.

1. На портале Azure щелкните **Azure Active Directory** и выберите **Управление удостоверениями**.

1. В меню слева щелкните **Catalog** , а затем откройте каталог.

1. В меню слева щелкните **ресурсы** , чтобы просмотреть список ресурсов в этом каталоге.

    ![Список ресурсов в каталоге](./media/entitlement-management-access-package-resources/catalog-resources.png)

1. Если вы являетесь диспетчером пакетов Access и вам нужно добавить ресурсы в каталог, можно попросить владельца каталога добавить их.

## <a name="add-resource-roles"></a>Добавление ролей ресурсов

Роль ресурса — это коллекция разрешений, связанных с ресурсом. Способ предоставления пользователям ресурсов для запроса заключается в добавлении ролей ресурсов в пакет Access. Вы можете добавлять роли ресурсов для групп, команд, приложений и сайтов SharePoint.

**Требуемая роль:** Глобальный администратор, Администратор пользователей, Владелец каталога или Диспетчер пакетов для доступа.

1. На портале Azure щелкните **Azure Active Directory** и выберите **Управление удостоверениями**.

1. В меню слева щелкните **доступ к пакетам** и откройте пакет Access.

1. В меню слева щелкните **роли ресурсов**.

1. Щелкните **Добавить роли ресурсов** , чтобы открыть страницу Добавление ролей ресурсов для доступа к пакету.

    ![Доступ к пакету — Добавление ролей ресурсов](./media/entitlement-management-access-package-resources/resource-roles-add.png)

1. В зависимости от того, нужно ли добавить группу, рабочую группу, приложение или сайт SharePoint, выполните действия в одном из следующих разделов ролей ресурсов.

## <a name="add-a-group-or-team-resource-role"></a>Добавление группы или роли ресурса команды

Управление назначением можно автоматически добавлять пользователей в группу или группу в Microsoft Teams, если им назначен пакет Access. 

- Если группа или команда входит в пакет Access, а пользователь назначен этому пакету доступа, он добавляется в эту группу или группу, если они еще не установлены.
- По истечении срока назначения пакета доступа пользователь удаляется из группы или команды, если в настоящее время не назначен другой пакет Access, включающий эту же группу или команду.

Можно выбрать любую группу [безопасности Azure AD или группу Microsoft 365](../fundamentals/active-directory-groups-create-azure-portal.md). Администраторы могут добавить любую группу в каталог. Владельцы каталога могут добавить любую группу в каталог, если они являются владельцами группы. При выборе группы учитывайте следующие ограничения Azure AD:

- Когда пользователь, включая гостя, добавляется как член группы или группы, он может видеть всех других участников этой группы или команды.
- Azure AD не может изменить членство в группе, которая была синхронизирована с Windows Server Active Directory с помощью Azure AD Connect или созданной в Exchange Online в качестве группы рассылки.  
- Членство динамических групп не может быть обновлено путем добавления или удаления члена, поэтому динамическое членство в группах не подходит для использования с управлением назначением.

Дополнительные сведения см. в разделе [Сравнение групп](/office365/admin/create-groups/compare-groups) и [Microsoft 365 групп и Microsoft Teams](/microsoftteams/office-365-groups).

1. На странице **Добавление ролей ресурсов для доступа к пакету** щелкните **группы и команды** , чтобы открыть панель Выбор групп.

1. Выберите группы и команды, которые необходимо включить в пакет Access.

    ![Доступ к пакету — Добавление ролей ресурсов — выбор групп](./media/entitlement-management-access-package-resources/group-select.png)

1. Нажмите кнопку **Выбрать**.

    После выбора группы или группы в столбце **подтипа** будет отображаться один из следующих подтипов:

    | Подтип | Описание |
    | --- | --- |
    | Безопасность | Используется для предоставления доступа к ресурсам. |
    | Distribution | Используется для отправки уведомлений группе людей. |
    | Microsoft 365 | Группа Microsoft 365, не поддерживающая команды. Используется для совместной работы пользователей как внутри организации, так и за ее пределами. |
    | Группа | Группа Microsoft 365, поддерживающая команды. Используется для совместной работы пользователей как внутри организации, так и за ее пределами. |

1. В списке **роль** выберите **владелец** или **член**.

    Обычно роль члена выбирается. Если выбрана роль владелец, пользователи могут добавлять или удалять других участников или владельцев.

    ![Доступ к пакету — Добавление роли ресурса для группы или команды](./media/entitlement-management-access-package-resources/group-role.png)

1. Нажмите кнопку **Добавить**.

    Все пользователи с существующими назначениями для пакета Access будут автоматически становиться членами этой группы или группы при добавлении.

## <a name="add-an-application-resource-role"></a>Добавление роли ресурса приложения

Azure AD может автоматически назначать пользователям доступ к корпоративному приложению Azure AD, включая приложения SaaS и приложения вашей организации, Федеративные в Azure AD, когда пользователю назначен пакет Access. Для приложений, которые интегрируются с Azure AD через федеративный единый вход, Azure AD выдаст токены Федерации для пользователей, назначенных приложению.

Приложения могут иметь несколько ролей. При добавлении приложения в пакет Access, если приложение имеет более одной роли, необходимо указать соответствующую роль для этих пользователей. При разработке приложений дополнительные сведения о добавлении этих ролей в приложения см. в статье [как настроить утверждение роли, выдаваемое в токене SAML для корпоративных приложений](../develop/active-directory-enterprise-app-role-management.md).

После того как роль приложения входит в пакет Access:

- Когда пользователю назначается этот пакет доступа, пользователь добавляется в эту роль приложения, если он отсутствует.
- Когда срок действия назначения пакета доступа пользователя истекает, его доступ будет удален из приложения, если он не назначен другому пакету Access, включающему эту роль приложения.

Ниже приведены некоторые рекомендации по выбору приложения.

- Приложения также могут иметь группы, назначенные их ролям.  Можно добавить группу вместо роли приложения в пакете Access, однако приложение не будет отображаться для пользователя как часть пакета Access на портале My Access.

1. На странице **Добавление ролей ресурсов для доступа к пакету** щелкните **приложения** , чтобы открыть панель выбор приложений.

1. Выберите приложения, которые необходимо включить в пакет Access.

    ![Доступ к пакету — Добавление ролей ресурсов — выбор приложений](./media/entitlement-management-access-package-resources/application-select.png)

1. Нажмите кнопку **Выбрать**.

1. В списке **роль** выберите роль приложения.

    ![Доступ к пакету — Добавление роли ресурса для приложения](./media/entitlement-management-access-package-resources/application-role.png)

1. Нажмите кнопку **Добавить**.

    Всем пользователям с существующими назначениями для пакета Access автоматически будет предоставлен доступ к этому приложению при его добавлении.

## <a name="add-a-sharepoint-site-resource-role"></a>Добавление роли ресурса сайта SharePoint

Azure AD может автоматически назначать пользователям доступ к сайту SharePoint Online или семейству веб-сайтов SharePoint Online, если им назначен пакет Access.

1. На странице **Добавление ролей ресурсов для доступа к пакету** щелкните **сайты SharePoint** , чтобы открыть панель "Выбор сайтов SharePoint Online".

    :::image type="content" source="media/entitlement-management-access-package-resources/resource-sharepoint-add.png" alt-text="Доступ к пакету — Добавление ролей ресурсов — выбор сайтов SharePoint — представление портала":::

1. Выберите сайты SharePoint Online, которые необходимо включить в пакет Access.

    ![Доступ к пакету — Добавление ролей ресурсов. Выбор сайтов SharePoint Online](./media/entitlement-management-access-package-resources/sharepoint-site-select.png)

1. Нажмите кнопку **Выбрать**.

1. В списке **роль** выберите роль сайта SharePoint Online.

    ![Доступ к пакету — Добавление роли ресурса для сайта SharePoint Online](./media/entitlement-management-access-package-resources/sharepoint-site-role.png)

1. Нажмите кнопку **Добавить**.

    Всем пользователям с существующими назначениями для пакета доступа будет автоматически предоставлен доступ к сайту SharePoint Online при его добавлении.

## <a name="remove-resource-roles"></a>Удаление ролей ресурсов

**Требуемая роль:** Глобальный администратор, Администратор пользователей, Владелец каталога или Диспетчер пакетов для доступа.

1. На портале Azure щелкните **Azure Active Directory** и выберите **Управление удостоверениями**.

1. В меню слева щелкните **доступ к пакетам** и откройте пакет Access.

1. В меню слева щелкните **роли ресурсов**.

1. В списке ролей ресурсов найдите роль ресурса, которую необходимо удалить.

1. Нажмите кнопку с многоточием (**...**) и выберите **удалить роль ресурса**.

    Все пользователи с существующими назначениями для пакета Access будут автоматически отменять доступ к этой роли ресурса при их удалении.

## <a name="when-changes-are-applied"></a>При применении изменений

В управлении назначением Azure AD будет обрабатывать групповые изменения для назначения и ресурсов в пакетах доступа несколько раз в день. Таким образом, при назначении или изменении ролей ресурсов пакета доступа может потребоваться до 24 часов для внесения этого изменения в Azure AD, а также время, необходимое для распространения этих изменений на другие службы Microsoft Online Services или подключенные приложения SaaS. Если изменение влияет только на несколько объектов, это изменение, скорее всего, займет всего несколько минут в Azure AD, после чего другие компоненты Azure AD будут обнаруживать это изменение и обновлять приложения SaaS. Если изменение влияет на тысячи объектов, изменение займет больше времени. Например, если у вас есть пакет Access с 2 приложениями и 100 пользователей, и вы решили добавить роль сайта SharePoint в пакет Access, может возникнуть задержка, пока все пользователи не будут входить в эту роль сайта SharePoint. Ход выполнения можно отслеживать с помощью журнала аудита Azure AD, журнала подготовки Azure AD и журналов аудита сайта SharePoint.

При удалении члена команды они также удаляются из группы Microsoft 365. С удалением из чата команды может возникнуть задержка. Дополнительные сведения см. в разделе [членство в группе](/microsoftteams/office-365-groups#group-membership).

## <a name="next-steps"></a>Дальнейшие действия

- [Создание простой группы и добавление в нее участников с помощью Azure Active Directory](../fundamentals/active-directory-groups-create-azure-portal.md)
- [Узнайте как настроить утверждения роли, выдаваемые в токене SAML для корпоративных приложений](../develop/active-directory-enterprise-app-role-management.md)
- [Введение в SharePoint Online](/sharepoint/introduction)