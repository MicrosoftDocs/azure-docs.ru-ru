---
title: Создание проверки доступа для групп & приложений — Azure AD
description: Узнайте, как создать проверку доступа к членам группы или доступ к приложениям в Azure Active Directory проверках доступа.
services: active-directory
author: ajburnle
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 3/3/2021
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7143c3f9786d41c32ae954ab219197a9cfaa1050
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102176881"
---
# <a name="create-an-access-review-of-groups-and-applications-in-azure-ad-access-reviews"></a>Создание проверки доступа для групп и приложений в проверках доступа Azure AD

Доступ сотрудников и гостей к группам и приложениям изменяется со временем. Чтобы уменьшить риск, связанный с устаревшими назначениями доступа, администраторы могут создать в Azure Active Directory (AAD) проверки доступа для участников групп или доступа к приложениям. Если вы хотите проверять доступ регулярно, создайте повторяющиеся проверки доступа. Дополнительные сведения об этих сценариях см. в разделах [Управление пользовательским доступом с помощью проверок доступа Azure AD](manage-user-access-with-access-reviews.md) и [Управление гостевым доступом с помощью проверок доступа Azure AD](manage-guest-access-with-access-reviews.md).

Вы можете просмотреть краткую видеобеседу о включении проверок доступа:

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

В этой статье описывается, как создать одну или несколько проверок доступа для членов группы или доступа к приложениям.

## <a name="prerequisites"></a>Предварительные условия

- Azure AD Premium P2
- глобальный администратор или администратор пользователей.

Дополнительные сведения см. в статье [Лицензионные требования](access-reviews-overview.md#license-requirements).

## <a name="create-one-or-more-access-reviews"></a>Создание одной или нескольких проверок доступа

1. Войдите в портал Azure и откройте [страницу Управление удостоверениями](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

2. В меню слева щелкните **Проверка доступа**.

3. Щелкните **Создать проверку доступа**, чтобы создать проверку доступа.

    ![Панель "проверки доступа" в управлении удостоверениями](./media/create-access-review/access-reviews.png)

4. На **шаге 1: выберите, что следует проверить** выберите ресурс для проверки.

    ![Создание проверки доступа — имя и описание проверки](./media/create-access-review/select-what-review.png)

5. Если вы выбрали **команды и группы** на шаге 1, у вас есть два варианта на шаге 2.
   - **Все группы Microsoft 365 с гостевыми пользователями.** Выберите этот параметр, если вы хотите создать повторяющиеся проверки для всех гостевых пользователей во всех группах Майкрософт и группах M365 в вашей организации. Вы можете исключить определенные группы, щелкнув "выбрать группы для исключения".
   - **Выберите команды и группы.** Выберите этот параметр, если вы хотите указать конечный набор команд или групп для проверки. После выбора этого параметра вы увидите список групп справа, которые нужно выбрать.

     ![Команды и группы](./media/create-access-review/teams-groups.png)

     ![Команды и группы, выбранные в пользовательском интерфейсе](./media/create-access-review/teams-groups-detailed.png)

6. Если вы выбрали **приложения** на шаге 1, то можете выбрать одно или несколько приложений на шаге 2.

    >[!NOTE]
    > Выбор нескольких групп и (или) приложений приведет к созданию нескольких проверок доступа. Например, если выбрать 5 групп для проверки, это приведет к 5 отдельным проверкам доступа.

   ![Интерфейс, отображаемый, если выбраны приложения, а не группы.](./media/create-access-review/select-application-detailed.png)

7. Затем на шаге 3 можно выбрать область для проверки. Параметры
   - **Только гостевые пользователи.** При выборе этого параметра Проверка доступа будет ограничена только гостевыми пользователями Azure AD B2B в вашем каталоге.
   - **Сотрудников.** Выбор этого параметра ограничивает проверку доступа всеми объектами пользователей, связанными с ресурсом.

    >[!NOTE]
    > Если вы выбрали все группы Microsoft 365 с гостевыми пользователями на шаге 2, то единственный вариант — проверить гостевых пользователей на шаге 3.

8. Щелкните Далее: рецензии
9. В разделе **Выбор рецензентов** выберите одного или нескольких пользователей для выполнения проверок доступа. Можно выбрать одно из следующих значений.
    - **Владельцы групп** (доступны только при выполнении проверки в группе или группе)
    - **Выбрано пользователей или групп:**
    - **Пользователи просматривают собственный доступ**
    - **Руководители пользователей.**
    Если выбрать одного из **руководителей пользователей** или **владельцев групп**  , вы также можете указать резервного рецензента. Резервным рецензентам предлагается выполнить проверку, если у пользователя нет диспетчера, указанного в каталоге, или группа не имеет владельца.

    ![Новая проверка доступа](./media/create-access-review/new-access-review.png)

10. В разделе " **указать повторение проверки** " можно указать периодичность, например **еженедельно, ежемесячно, ежеквартально, в два** раза в год. Затем указывается **Длительность**, определяющая время, в течение которого проверка будет открываться для входных данных рецензентов. Например, для ежемесячной проверки вы можете настроить продолжительность не более 27 дней, что позволяет избежать перекрытия интервалов проверки. Может потребоваться сократить длительность, чтобы убедиться, что входные данные рецензентов применены ранее. Затем можно выбрать **дату начала** и **дату окончания**.

    ![Выберите, как часто должна выполняться проверка](./media/create-access-review/frequency.png)

11. Нажмите кнопку **Далее: параметры** в нижней части страницы.
12. В **параметрах завершения** можно указать, что происходит после завершения проверки.

    ![Создание параметров завершения проверки доступа](./media/create-access-review/upon-completion-settings-new.png)

Если вы хотите автоматически удалить доступ для запрещенных пользователей, установите для параметра Автоматическое применение результатов значение ресурс, чтобы включить. Если вы хотите вручную применять результаты по завершении проверки, выберите здесь значение Отключить.
Используйте параметр если рецензенты не отвечают на список, чтобы указать, что происходит для пользователей, не просмотренных рецензентом в течение периода проверки. Этот параметр не влияет на пользователей, для которых рецензенты выполняют проверку вручную. Если в качестве решения рецензент выберет запрет, доступ пользователя будет заблокирован.

- **Без изменений** — доступ пользователя сохраняется без изменений.
- **Удалить доступ** — доступ пользователя блокируется.
- **Утвердить доступ** — доступ пользователя утверждается.
- **Получить рекомендации** — применить рекомендации системы в отношении запрета или утверждения доступа пользователя.

    ![Параметры выполнения после завершения](./media/create-access-review/upon-completion-settings-new.png)

Используйте действие для применения запрещенных **гостевых** пользователей, чтобы указать, что произойдет с гостевыми пользователями, если они запрещены.
- Удаление членства пользователя из ресурса приведет к удалению доступного пользователя к проверяемой группе или приложению, но они смогут входить в клиент.
- Запретить пользователю входить в систему в течение 30 дней, а затем удалить пользователя из клиента заблокировать доступ запрещенных пользователей от входа в клиент, независимо от того, есть ли у них доступ к другим ресурсам. Если произошла ошибка или администратор решает повторно включить доступ, он может сделать это в течение 30 дней после отключения пользователя. Если для отключенных пользователей не выполняются никакие действия, они будут удалены из клиента.

Дополнительные сведения о рекомендациях по удалению гостевых пользователей, которые больше не имеют доступа к ресурсам в Организации, см. в статье [использование Azure AD Identity Governance для просмотра и удаления внешних пользователей, у которых больше нет доступа к ресурсам.](access-reviews-external-users.md)

   >[!NOTE]
   >Действие, применяемое к запрещенным гостевым пользователям, не настраивается при рецензировании, ограниченном несколькими гостевыми пользователями. Кроме того, ее нельзя настроить для проверок **всех M365 групп с гостевыми пользователями.** Если вы не настраиваете, параметр по умолчанию для удаления членства пользователя из ресурса используется для запрещенных пользователей.

13. В **помощниках по включению решений проверки** выберите, должен ли проверяющий получать рекомендации во время процесса проверки.

    ![Включить параметры вспомогательных функций принятия решений](./media/create-access-review/helpers.png)

14. В разделе **Дополнительные параметры** можно выбрать следующее.
    - Чтобы **позволить** рецензенту указать причину утверждения, необходимо задать **обоснование** .
    - Настройте **уведомления по электронной почте** , чтобы **Разрешить** службе Azure AD отправлять уведомления по электронной почте проверяющим при запуске проверки доступа и для администраторов при завершении проверки.
    - Задайте для параметра **Напоминания** значение **Включить**, чтобы AAD отправляла напоминания о текущей проверке доступа рецензентам, которые еще не завершили проверку. Эти напоминания будут самостоятельными в течение всего времени проверки.
    - Содержимое сообщения электронной почты, отправленного рецензентам, формируется автоматически на основе сведений о проверке, таких как имя рецензии, имя ресурса, Дата выполнения и т. д. Если требуется способ передачи дополнительных сведений, таких как дополнительные инструкции или контактные данные, эти сведения можно указать в разделе **дополнительное содержимое для электронной почты рецензента** . Введенные сведения включаются в приглашение и напоминание по электронной почте, отправленные назначенным рецензентам. В разделе, выделенном на изображении ниже, показано, где отображаются эти сведения.


      ![дополнительное содержимое для рецензента](./media/create-access-review/additional-content-reviewer.png)

15. Чтобы перейти к следующей странице, нажмите кнопку **"Далее": "Проверка и создание** "
16. Назовите проверку доступа. При необходимости укажите описание проверки. Имя и описание отображаются для рецензентов.
17. Проверьте сведения и нажмите кнопку **создать** .

       ![создать экран проверки](./media/create-access-review/create-review.png)

## <a name="start-the-access-review"></a>Запуск проверки доступа

Завершив настройку параметров для проверки доступа, щелкните **Запустить**. Проверка доступа будет отображаться в списке с индикатором состояния.

![Список проверок доступа и их состояние](./media/create-access-review/access-reviews-list.png)

По умолчанию Azure AD отправляет электронные сообщения рецензентам вскоре после запуска проверки. Если вы выбрали не отправлять сообщения через службу Azure AD, обязательно сообщите рецензентам, что им необходимо выполнить проверку доступа. Вы можете просмотреть инструкции по [просмотру доступа к группам или приложениям](perform-access-review.md). Если ваш отзыв предназначен для того, чтобы проверить собственный доступ для гостей, покажите им инструкции по [просмотру доступа к группам или приложениям](review-your-access.md).

Если вы назначили гостей в качестве рецензентов и не приняли приглашение, они не получат сообщение электронной почты от проверки доступа, так как они должны принять приглашение перед просмотром.

## <a name="access-review-status-table"></a>Таблица состояния проверки доступа

| Состояние | Определение |
|--------|------------|
|NotStarted | Была создана проверка, обнаружение пользователя ожидает запуска. |
|Инициализация   | Выполняется обнаружение пользователей для обнаружения всех пользователей, которые являются частью проверки. |
|Запуск | Проверка начинается. Если уведомления по электронной почте включены, сообщения электронной почты отправляются рецензентам. |
|InProgress | Проверка начата. Если уведомления по электронной почте включены, они были отправлены рецензентам. Рецензенты могут отправлять решения до наступления срока. |
|Окончания | Проверка завершена, и сообщения электронной почты отправляются владельцу проверки. |
|Автоматическая проверка | Проверка находится на этапе проверки системы. Система записывает решения для пользователей, которые не были проверены на основе рекомендаций или предварительно настроенных решений. |
|Автоматическое рецензирование | Решения были зарегистрированы системой для всех пользователей, которые не были проверены. Проверка готова к **применению** , если включено автоматическое применение. |
|Развертывание | Пользователи, которые были утверждены, не будут изменять доступ. |
|Применено | Запрещенные пользователи, если они есть, были удалены из ресурса или каталога. |
|Сбой | Не удалось выполнить проверку. Эта ошибка может быть связана с удалением клиента, изменением лицензий или другими внутренними изменениями клиента. |

## <a name="create-reviews-via-apis"></a>Создание проверок через API

Проверки доступа можно также создать с помощью API. Все действия по управлению проверками доступа для групп и пользователей приложений, которые выполняются на портале Azure, доступны и через API Microsoft Graph. Дополнительные сведения см. в [справочнике по API проверок доступа Azure AD](/graph/api/resources/accessreviews-root?view=graph-rest-beta). Пример кода см. в разделе [Пример получения проверок доступа Azure AD с помощью Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Дальнейшие действия

- [Проверка доступа к группам или приложениям](perform-access-review.md)
- [Обзор доступа к группам или приложениям](review-your-access.md)
- [Выполнение проверки доступа для групп или приложений](complete-access-review.md)