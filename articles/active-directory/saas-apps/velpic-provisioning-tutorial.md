---
title: Руководство. Настройка Velpic для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической подготовки и отмены подготовки учетных записей пользователей в Velpic.
services: active-directory
author: zhchia
writer: zhchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: zhchia
ms.openlocfilehash: cdd4fb96a42d154ccd8b508950283978ddf58ef4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "94354909"
---
# <a name="tutorial-configuring-velpic-for-automatic-user-provisioning"></a>Руководство. Настройка Velpic для автоматической подготовки пользователей

Цель этого руководства — показать, как в Velpic и Azure AD настроить автоматическую подготовку и отзыв учетных записей пользователей из Azure AD в Velpic.

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на базе службы подготовки пользователей Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Предварительные требования

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Клиент Azure Active Directory.
* клиент Velpic с [планом Enterprise](https://www.velpic.com/pricing.html) или выше;
* учетная запись пользователя в Velpic с разрешениями администратора.

## <a name="assigning-users-to-velpic"></a>Назначение пользователей приложению Velpic

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая "назначение". В контексте автоматической подготовки учетной записи синхронизированы будут только те пользователи и группы, которые были "назначены" приложению в Azure AD. 

Перед настройкой и включением службы подготовки необходимо решить, какие пользователи или группы в Azure AD представляют пользователей, которым необходим доступ к приложению Velpic. После этого можно назначить этих пользователей для приложения Velpic, выполнив следующие действия:

[Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-velpic"></a>Важные рекомендации по назначению пользователей в Velpic

* Рекомендуем назначить одного пользователя Azure AD в Velpic для тестирования конфигурации подготовки. Дополнительные пользователи и/или группы можно назначить позднее.

* При назначении пользователя в Velpic в диалоговом окне назначения необходимо выбрать роль **Пользователь** или другую действительную роль для конкретного приложения (если доступно). Учтите, что роль **Доступ по умолчанию** не подходит для подготовки, и такие пользователи будут пропущены.

## <a name="configuring-user-provisioning-to-velpic"></a>Настройка подготовки учетных записей пользователей в Velpic

В этом разделе описывается, как подключить Azure AD к API подготовки учетной записи пользователя в Velpic и настроить подготовку службы для создания, обновления и отмены назначения учетных записей пользователей в Velpic на основе назначения пользователей и групп в Azure AD.

> [!TIP]
> Для Velpic можно также включить единый вход на основе SAML. Для этого следуйте инструкциям на [портале Azure](https://portal.azure.com). Единый вход можно настроить независимо от автоматической подготовки, хотя эти две возможности дополняют друг друга.

### <a name="to-configure-automatic-user-account-provisioning-to-velpic-in-azure-ad"></a>Настройка автоматической подготовки учетных записей пользователей Azure AD в Velpic

1. На [портале Azure](https://portal.azure.com) перейдите в раздел **Azure Active Directory > Корпоративные приложения > Все приложения**.

2. Если для Velpic уже настроен единый вход, найдите свой экземпляр Velpic с помощью поля поиска. В противном случае щелкните **Добавить** и найдите **Velpic** в коллекции приложений. Выберите Velpic в результатах поиска и добавьте это приложение в список приложений.

3. Выберите экземпляр Velpic и откройте вкладку **Подготовка**.

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Подготовка Velpic](./media/velpic-provisioning-tutorial/Velpic1.png)

5. В разделе **Учетные данные администратора** введите **URL-адрес клиента и маркер секрета** Velpic. Эти значения можно найти в учетной записи Velpic: **Manage** > **Integration** > **Plugin** > **SCIM** (Управление > Интеграция > Подключаемый модуль > SCIM).

    ![Значения авторизации](./media/velpic-provisioning-tutorial/Velpic2.png)

6. На портале Azure щелкните **Проверить подключение**, чтобы убедиться, что Azure AD может подключиться к приложению Velpic. Если установить подключение не удалось, убедитесь, что у учетной записи Velpic есть разрешения администратора, и повторите шаг 5.

7. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок ниже.

8. Выберите команду **Сохранить**.

9. В разделе сопоставлений выберите **Синхронизировать пользователей Azure Active Directory с Velpic**.

10. В разделе **Сопоставления атрибутов** просмотрите атрибуты пользователей, которые будут синхронизированы из Azure AD в Velpic. Учтите, что атрибуты, которые выбраны в качестве свойств **Соответствие**, будут использоваться для сопоставления учетных записей пользователей в Velpic для операций обновления. Нажмите кнопку "Сохранить", чтобы подтвердить все изменения.

11. Чтобы включить службу подготовки Azure AD для Velpic, в разделе **Параметры** измените значение параметра **Состояние подготовки** на **Включено**.

12. Выберите команду **Сохранить**.

После этого начнется исходная синхронизация всех пользователей и (или) групп, назначенных в Velpic в разделе "Пользователи и группы". Обратите внимание, что начальная синхронизация будет занимать больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **Сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра отчетов о подготовке, в которых зафиксированы все действия, выполняемые службой подготовки.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../app-provisioning/check-status-user-account-provisioning.md)