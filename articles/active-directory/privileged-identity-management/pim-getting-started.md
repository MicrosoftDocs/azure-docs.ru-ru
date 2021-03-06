---
title: Начало работы с PIM-Azure Active Directory | Документация Майкрософт
description: Узнайте, как включить управление привилегированными пользователями Azure AD и начать с ним работу на портале Azure.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: how-to
ms.workload: identity
ms.date: 09/15/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5bcfb21ab15355653780355f1b5e459bc806ec8c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "90600727"
---
# <a name="start-using-privileged-identity-management"></a>Начало работы с управлением привилегированными пользователями

В этой статье описывается, как включить управление привилегированными пользователями (PIM) и приступить к его использованию.

Используйте управление привилегированными пользователями (PIM) для управления доступом, контроля и мониторинга доступа в вашей организации Azure Active Directory (Azure AD). С помощью PIM вы можете предоставить необходимые и своевременный доступ к ресурсам Azure, ресурсам Azure AD и другим веб-службы Майкрософт, таким как Microsoft 365 или Microsoft Intune.

## <a name="prerequisites"></a>Предварительные требования

Чтобы использовать управление привилегированными пользователями, необходимо иметь одну из следующих лицензий:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5.

Дополнительные сведения см. [в статье требования к лицензиям для использования Управление привилегированными пользователями](subscription-requirements.md).

> [!Note]
> Когда пользователь, активный в привилегированной роли в Организации Azure AD с лицензией Premium P2, переходит к **ролям и администраторам** в Azure AD и выбирает роль (или даже только посещения управление привилегированными пользователями):
>
> - Мы автоматически включили PIM для Организации.
> - Теперь их опыт может назначить "обычное" назначение роли или назначение допустимой роли.
>
> Если управление PIM включено, оно не оказывает никакого влияния на вашу организацию, о которой нужно беспокоиться. Он предоставляет дополнительные возможности назначения, такие как активные и допустимые значения времени начала и окончания. PIM также позволяет определить область для назначений ролей с помощью административных единиц и пользовательских ролей. Если вы являетесь глобальным администратором или администратором привилегированных ролей, вы можете получить несколько дополнительных сообщений электронной почты, таких как Еженедельный дайджест PIM. Кроме того, в журнале аудита, связанном с назначением ролей, может отображаться субъект-служба MS PIM. Это ожидаемое изменение, которое не должно оказывать никакого влияния на рабочий процесс.

## <a name="prepare-pim-for-azure-ad-roles"></a>Подготовка PIM для ролей Azure AD

Ниже приведены задачи, которые мы рекомендуем подготовить управление привилегированными пользователями для управления ролями Azure AD.

1. [Настройте параметры роли Azure AD](pim-how-to-change-default-settings.md).
1. [Предоставление подходящих назначений](pim-how-to-add-role-to-user.md).
1. [Разрешить пользователям, которые имеют право активировать роль Azure AD,](pim-how-to-activate-role.md)своевременно.

## <a name="prepare-pim-for-azure-roles"></a>Подготовка PIM для ролей Azure

Ниже приведены задачи, которые мы рекомендуем подготовить для подготовки управление привилегированными пользователями управления ролями Azure для подписки.

1. [Обнаружение ресурсов Azure](pim-resource-roles-discover-resources.md)
1. [Настройте параметры роли Azure](pim-resource-roles-configure-role-settings.md).
1. [Предоставление подходящих назначений](pim-resource-roles-assign-roles.md).
1. [Разрешить пользователям, которые имеют право активировать свои роли Azure](pim-resource-roles-activate-your-roles.md), своевременно.

## <a name="navigate-to-your-tasks"></a>Переход к задачам

После того, как управление привилегированными пользователями настроена, вы сможете изучать свой путь.

![Окно навигации в управление привилегированными пользователями отображение задач и управление параметрами](./media/pim-getting-started/pim-quickstart-tasks.png)

| Задача и управление | Описание |
| --- | --- |
| **Мои роли**  | Отображается список назначенных вам действительных и активных ролей. Здесь можно активировать все действительные назначенные роли. |
| **Мои запросы** | Отображаются все запросы, ожидающие активации назначения действительных ролей. |
| **Утверждение запросов** | Отображается список запросов на активацию действительных ролей в вашем каталоге для пользователей, которых вы должны подтвердить. |
| **Проверка доступа** | Приведены активные проверки доступа, которые вы должны выполнить (для себя или для кого-то другого). |
| **Роли Azure AD** | Отображает панель мониторинга и параметры для администраторов привилегированных ролей для управления назначениями ролей Azure AD. Эта панель мониторинга отключена для всех, кто не является администратором привилегированных ролей. Эти пользователи имеют доступ к специальной панели мониторинга My view (Мое представление). Панель мониторинга "Мои представления" отображает только сведения о пользователе, обращающемся к панели мониторинга, а не всей Организации. |
| **Ресурсы Azure** | Отображает панель мониторинга и параметры для администраторов привилегированных ролей для управления назначениями ролей ресурсов Azure. Эта панель мониторинга отключена для всех, кто не является администратором привилегированных ролей. Эти пользователи имеют доступ к специальной панели мониторинга My view (Мое представление). Панель мониторинга "Мои представления" отображает только сведения о пользователе, обращающемся к панели мониторинга, а не всей Организации. |

## <a name="add-a-pim-tile-to-the-dashboard"></a>Добавление плитки PIM на панель мониторинга

Чтобы упростить открытие управление привилегированными пользователями, добавьте плитку PIM на панель мониторинга портал Azure.

1. Войдите на [портал Azure](https://portal.azure.com/).

1. Выберите **все службы** и найдите службу **Azure AD privileged Identity Management** .

    ![Azure AD Privileged Identity Management в разделе "Все службы"](./media/pim-getting-started/pim-all-services-find.png)

1. Выберите управление привилегированными пользователями **Быстрый запуск**.

1. Выберите **закрепить колонку на панели мониторинга** , чтобы закрепить страницу **быстрого запуска** управление привилегированными пользователями на панели мониторинга.

    ![Значок канцелярской кнопки для закрепления управление привилегированными пользователями страницы на панель мониторинга](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    На панели мониторинга Azure вы увидите такие плитки:

    ![Плитка "Быстрый запуск" управление привилегированными пользователями на панели мониторинга](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Назначение ролей Azure AD в управление привилегированными пользователями](pim-how-to-add-role-to-user.md)
- [Управление доступом к ресурсам Azure в управление привилегированными пользователями](pim-resource-roles-discover-resources.md)
