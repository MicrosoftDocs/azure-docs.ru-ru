---
title: Обновление владельца группы с истекшим сроком действия в управление привилегированными пользователями в Azure AD | Документация Майкрософт
description: Узнайте, как расширить или продлить назначения ролей, назначаемые ролями, в Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: pim
ms.date: 06/30/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0cfbb0be22dee4550050d6af10314f3a3bb1f583
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "87505995"
---
# <a name="extend-or-renew-privileged-access-group-assignments-preview-in-privileged-identity-management"></a>Расширение или обновление назначений привилегированных прав доступа (Предварительная версия) в управление привилегированными пользователями

Azure Active Directory (Azure AD) управление привилегированными пользователями (PIM) предоставляет элементы управления для управления жизненным циклом доступа и назначения для привилегированных групп доступа. Администраторы могут назначать роли с помощью свойств даты и времени начала и окончания. По окончании назначения управление привилегированными пользователями отправляет уведомления по электронной почте затронутым пользователям или группам. Также уведомления по электронной почте отправляются администраторам ресурса, чтобы они приняли меры для сохранения требуемого доступа. Назначения нужно возобновить. Для удобства они остаются видимыми в течение 30 дней после истечения срока действия, даже если доступ не продлевается.

## <a name="who-can-extend-and-renew"></a>Кто может продлить и продлить

Только администраторы ресурса могут расширять или обновлять назначения группы привилегированного доступа. Затронутый пользователь или группа могут запросить продление назначений, срок действия которых скоро истечет, и запросить продление уже истекших назначений.

## <a name="when-notifications-are-sent"></a>При отправке уведомлений

Управление привилегированными пользователями отправляет администраторам уведомления по электронной почте и затронутые пользователи привилегированных прав доступа, срок действия которых истекает:

- В течение 14 дней до истечения срока действия
- Один день до истечения срока действия
- По истечении срока действия назначения

Администраторы получают уведомления, когда пользователь или группа запрашивают, чтобы продлить или продлить назначение с истекшим сроком действия или с истекшим сроком действия. Когда администратор разрешает запрос, все администраторы и запрашивающий пользователь получают уведомления об утверждении или отказе.

## <a name="extend-group-assignments"></a>Расширить назначения групп

Следующие шаги описывают процесс запроса, разрешения или администрирования расширения или обновления назначения группы.

### <a name="self-extend-expiring-assignments"></a>Самостоятельные назначения истечения срока действия

Пользователи, назначенные привилегированной группе доступа, могут расширять назначения групп с истекающим сроком действия непосредственно на вкладке " **подходящие** " или " **Активные** " на странице **назначения** для группы. Пользователи или группы могут запросить продление допустимых и активных назначений, срок действия которых истечет в течение следующих 14 дней.

![Страница "Мои роли" со списком подходящих ассгнментс с помощью столбца Action](media/groups-renew-extend/self-extend-group-assignment.png)

Если дата и время окончания назначения находится в пределах 14 дней, команда **расширения** становится доступной. Чтобы запросить расширение назначения группы, выберите **расширить** , чтобы открыть форму запроса.

![Расширение области назначения групп с указанием поля причины и сведений](media/groups-renew-extend/extend-request-details-group-assignment.png)

>[!NOTE]
>Мы рекомендуем подробно описать, для чего требуется продление и на какой срок (если у вас есть такая информация).

Через некоторое время администраторы получат уведомление по электронной почте с запросом на проверку запроса расширения. Если запрос на расширение уже отправлен, на портале появится уведомление Azure.

Чтобы просмотреть состояние или отменить запрос, откройте страницу « **ожидающие запросы** » для назначения группы.

![Назначения привилегированного доступа — страница "ожидающие запросы" с ссылкой для отмены](media/groups-renew-extend/group-assignment-extend-cancel-request.png)

### <a name="admin-approved-extension"></a>Расширение утверждено администратором

Когда пользователь или группа отправляет запрос на расширение назначения группы, администраторы получают уведомление по электронной почте, содержащее сведения о первоначальном назначении и причины запроса. Уведомление содержит прямую ссылку на утверждение или отклонение запроса для администратора.

Помимо использования ссылки из электронной почты, администраторы могут утверждать или отклонять запросы, перейдя на портал администрирования управление привилегированными пользователями и выбрав пункт **утвердить запросы** в левой области.

![Назначения привилегированного доступа группы — страница "утверждение запросов" со списком запросов и ссылок для утверждения или запрета](media/groups-renew-extend/group-assignment-extend-admin-approve.png)

Когда администратор выбирает **утверждение** или **отклонение**, отображаются подробные сведения о запросе, а также поле для предоставления делового обоснования для журналов аудита.

![Утверждение запроса на назначение группы с указанием причины запроса, типа назначения, времени начала, времени окончания и причины](media/groups-renew-extend/group-assignment-extend-admin-approve-reason.png)

При утверждении запроса на расширение назначения группы Администраторы ресурсов могут выбрать новую дату начала, дату окончания и тип назначения. Изменение типа назначения применяется в тех случаях, когда администратор предоставляет ограниченный доступ для выполнения конкретной задачи (например, на один день). В нашем примере администратор может изменить статус назначения с **Доступно** на **Активно**. Это означает, что запрашивающая сторона сразу получит указанный доступ, без необходимости активации.

### <a name="admin-initiated-extension"></a>Расширение, инициированное администратором

Если пользователь, назначенный группе, не запрашивает расширение для назначения группы, администратор может расширить назначение от имени пользователя. Административные расширения назначения группы не нуждаются в утверждении, но уведомления отправляются всем другим администраторам после того, как назначение будет расширено.

Чтобы расширить назначение группы, перейдите к представлению назначения в управление привилегированными пользователями. Найдите назначение, для которого требуется расширение. Выберите **Продлить** в столбце действий.

![Страница "назначения" со списком допустимых назначений групп со ссылками для расширения](media/groups-renew-extend/group-assignment-extend-admin-approve.png)

## <a name="renew-group-assignments"></a>Продлить назначения групп

Хотя концептуально аналогичен процессу запроса расширения, процесс обновления назначения группы с истекшим сроком действия отличается. С помощью следующих шагов назначения и администраторы могут обновлять доступ к назначениям с истекшим сроком действия при необходимости.

### <a name="self-renew"></a>Самостоятельное продление

Пользователи, которым не удается получить доступ к ресурсам, могут получить доступ к журналу назначений с истекшим сроком действия до 30 дней. Для этого они пересматривают **Мои роли** в левой области, а затем выбирают вкладку **просроченные назначения** .

![Страница "Мои роли" — Вкладка "назначения с истекшим сроком действия"](media/groups-renew-extend/groups-renew-from-my-roles.png)

В списке назначений по умолчанию отображаются допустимые **назначения**. С помощью раскрывающегося меню можно переключаться между доступными и активными назначениями.

Чтобы запросить продление для любого из назначений групп в списке, выберите действие **продление** . Укажите также причину запроса. Полезно указать длительность в дополнение к любому дополнительному контексту или деловому обоснование, которое может помочь администратору ресурсов принять решение о необходимости утверждения или отклонения.

![Панель «обновление назначения группы» с полем «причина»](media/groups-renew-extend/groups-renew-request-form.png)

После отправки запроса Администраторы ресурсов получают уведомления о запросе, ожидающем обновления назначения группы.

### <a name="admin-approves"></a>Утверждение администратором

Администраторы ресурсов могут получить доступ к запросу продления по ссылке в уведомлении по электронной почте или путем доступа к управление привилегированными пользователями из портал Azure и выбора параметра **утвердить запросы** с левой панели.

Когда администратор выбирает **утверждение** или **отклонить**, сведения о запросе отображаются вместе с полем для предоставления делового обоснования для журналов аудита.

При утверждении запроса на продление назначения группы Администраторы ресурсов должны ввести новую дату начала, дату окончания и тип назначения.

## <a name="next-steps"></a>Дальнейшие действия

- [Утверждение или отклонение запросов на назначения привилегированных групп доступа в управление привилегированными пользователями](groups-approval-workflow.md)
- [Настройка параметров группы привилегированного доступа в управление привилегированными пользователями](groups-role-settings.md)
