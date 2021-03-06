---
title: Требования к лицензиям для использования управление привилегированными пользователями-Azure Active Directory | Документация Майкрософт
description: Сведения о требованиях к лицензиям для использования Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: pim
ms.date: 08/06/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74c9cd1c55f1b0dde173a7ffbeac92e5518db81e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "88005811"
---
# <a name="license-requirements-to-use-privileged-identity-management"></a>License requirements to use Privileged Identity Management (Требования к лицензии для использования PIM)

Чтобы использовать управление привилегированными пользователями (PIM) в Azure Active Directory (Azure AD), у каталога должна быть допустимая лицензия. Кроме того, лицензии должны быть назначены администраторам и соответствующим пользователям. В этой статье описываются требования к лицензиям для использования управление привилегированными пользователями.

## <a name="valid-licenses"></a>Действительные лицензии

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="licenses-you-must-have"></a>Лицензии, которые вам необходимы

В каталоге должно быть по меньшей мере столько же лицензий Azure AD Premium P2, сколько у вас есть сотрудников со следующими ролями и полномочиями:

- Пользователи, которым разрешен доступ к Azure AD или ролям Azure, управляемым с помощью PIM
- Пользователи, которым назначены права участников или владельцы привилегированных групп доступа
- Пользователи могут утверждать или отклонять запросы на активацию в PIM
- Пользователи, которые назначены для проверки доступа.
- Пользователи, которые выполняют проверки доступа.

Лицензии Azure AD Premium P2 **не** требуются для следующих задач.

- Лицензии не требуются для пользователей, которые настраивают PIM, настройки политик, получения оповещений и настройки проверок доступа.

Дополнительные сведения о лицензиях см. в статье [Назначение или удаление лицензий с помощью портала Azure Active Directory](../fundamentals/license-users-groups.md).

## <a name="example-license-scenarios"></a>Примеры сценариев лицензирования

Описанные ниже примеры сценариев лицензирования помогут вам определить необходимое количество лицензий.

| Сценарий | Вычисление | Количество лицензий |
| --- | --- | --- |
| В банке Woodgrove Bank имеется 10 администраторов для разных отделов и 2 глобальных администратора, которые настраивают PIM и управляют ими. Они делают пять администраторов доступными. | Пять лицензий для администраторов, имеющих право | 5 |
| ИТ-проект имеет 25 администраторов, на которых 14 управляется с помощью PIM. Для активации роли требуется утверждение, и в Организации есть три разных пользователя, которые могут утверждать активацию. | 14 лицензий для соответствующих ролей и трех утверждающих | 17 |
| Компания Contoso имеет 50 администраторов, которым Управление 42 осуществляется через PIM. Для активации роли требуется утверждение, и в Организации есть пять разных пользователей, которые могут утверждать активацию. Contoso также выполняет ежемесячные проверки пользователей, которым назначены роли администратора, а рецензенты — менеджеров пользователей, на которых шесть ролей не входят в роли администратора, управляемые PIM. | 42. лицензии на подходящие роли + пять утверждающих и шесть рецензентов | 53 |

## <a name="when-a-license-expires"></a>По истечении срока действия лицензии

Если срок действия лицензии Azure AD Premium P2, EMS или пробной версии истек, управление привилегированными пользователями функции больше не будут доступны в вашем каталоге:

- назначенные постоянные роли Azure AD сохраняются без изменений;
- Служба управление привилегированными пользователями в портал Azure, а также командлеты API Graph и интерфейсы PowerShell управление привилегированными пользователями, больше не будут доступны пользователям для активации привилегированных ролей, управления привилегированным доступом или проверки доступа привилегированных ролей.
- удаляются назначения для ролей Azure AD, так как пользователи уже не могут активировать привилегированные роли;
- Все текущие проверки доступа к ролям Azure AD будут завершаться, а управление привилегированными пользователями параметры конфигурации будут удалены.
- Управление привилегированными пользователями больше не будет отсылать сообщения электронной почты об изменениях назначения ролей.

## <a name="next-steps"></a>Дальнейшие действия

- [Deploy Azure AD Privileged Identity Management (PIM)](pim-deployment-plan.md) (Развертывание Azure AD Privileged Identity Management (PIM))
- [Начало работы с управлением привилегированными пользователями](pim-getting-started.md)
- [Роли, которыми нельзя управлять в управление привилегированными пользователями](pim-roles.md)
