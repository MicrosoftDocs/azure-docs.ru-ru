---
title: Руководство. Интеграция Azure Active Directory с Syncplicity | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Syncplicity.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.openlocfilehash: 3c665795325ed3863583eb0f21f3e0d3f534154a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "103201464"
---
# <a name="tutorial-integrate-syncplicity-with-azure-active-directory"></a>Руководство по интеграции Syncplicity с Azure Active Directory

В этом руководстве описано, как интегрировать Syncplicity с Azure Active Directory (Azure AD). Интеграция Syncplicity с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD вы можете контролировать доступ к Syncplicity.
* Вы можете включить автоматический вход пользователей в Syncplicity с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, [здесь](https://azure.microsoft.com/free/) вы можете получить бесплатную пробную версию сроком на 12 месяцев.
* подписка Syncplicity с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде. Syncplicity поддерживает единый вход, инициируемый **поставщиком услуг**.

## <a name="adding-syncplicity-from-the-gallery"></a>Добавление Syncplicity из коллекции

Чтобы настроить интеграцию приложения Syncplicity с Azure AD, вам нужно добавить Syncplicity из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. В разделе **Создать** щелкните **Корпоративное приложение**.
1. В разделе **Обзор коллекции Azure AD** введите в поле поиска строку **Syncplicity**.
1. Выберите **Syncplicity** в области результатов и щелкните **Создать**, чтобы добавить это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-sso"></a>Настройка и проверка единого входа Azure AD

Настройте и проверьте единый вход Azure AD в Syncplicity с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Syncplicity.

Чтобы настроить и проверить единый вход Azure AD в Syncplicity, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Syncplicity](#configure-syncplicity-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Syncplicity](#create-syncplicity-test-user)** требуется для того, чтобы в Syncplicity существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.
7. **[Обновление единого входа](#update-sso)** позволяет внести необходимые изменения на стороне Syncplicity, если вы изменили настройки единого входа в Azure AD.

### <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Syncplicity** найдите раздел **Начало работы** и выберите **Настройка единого входа**.
2. На странице **Выбрать метод единого входа** выберите **SAML**.
3. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

4. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<companyname>.syncplicity.com/sp`.

    b. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.syncplicity.com`.
    
    c. В текстовое поле **URL-адреса ответа или URL-адрес службы обработчика утверждений** введите URL-адрес в следующем формате: `https://<companyname>.syncplicity.com/Auth/AssertionConsumerService.aspx`.

    > [!NOTE]
    > Эти значения приведены для примера. Необходимо обновить эти значения действующим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов Syncplicity](https://www.syncplicity.com/contact-us). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат для подписи токена SAML** щелкните **Изменить**. Затем в диалоговом окне нажмите кнопку с многоточием рядом с активным сертификатом и выберите **Скачать сертификат PEM**.

   ![Ссылка для скачивания сертификата](common/certificatebase64.png)

    > [!NOTE]
    > Здесь нужен именно PEM-сертификат, так как Syncplicity не принимает сертификаты в формате CER.

6. Требуемые URL-адреса вы можете скопировать из раздела **Настройка Syncplicity**.

   ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="configure-syncplicity-sso&quot;></a>Настройка единого входа в Syncplicity

1. Войдите в клиент **Syncplicity**.

1. В меню в верхней части страницы щелкните **Администрирование**, выберите **Параметры**, а затем — **Личный домен и единый вход**.

    ![Syncplicity](./media/syncplicity-tutorial/ic769545.png &quot;Syncplicity")

1. На странице **изменения настроек единого входа** выполните следующие действия.

    ![Единый вход\(Единый вход\)](./media/syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")

    а. В текстовом поле **Custom Domain** (Пользовательский домен) введите свой домен.
  
    b. Выберите **Включить** в **Состояние единого входа**.

    c. В текстовом поле **Entity Id** (Идентификатор сущности) введите значение **Identifier (Entity ID)** (Идентификатор (ИД сущности)), которое вы использовали в разделе **Базовая конфигурация SAML** на портале Azure.

    d. В текстовом поле **Sign-in page URL** (URL-адрес страницы входа) вставьте **URL-адрес входа**, скопированный на портале Azure.

    д) В текстовое поле **Logout page URL** (URL-адрес страницы выхода) вставьте **URL-адрес выхода**, скопированный на портале Azure.

    е) Напротив пункта **Identity Provider Certificate** (Сертификат поставщика удостоверений) нажмите кнопку **Выбрать файл**, а затем отправьте сертификат, скачанный с портала Azure.

    ж. Нажмите кнопку **Сохранить изменения**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
2. В верхней части экрана выберите **Новый пользователь**.
3. В разделе **Свойства пользователя** выполните следующие действия.

   а. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.

   b. В поле **Имя** введите `B.Simon`.  
   
   c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   
   d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к Syncplicity.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Syncplicity**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя или группу**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)
1. На странице **Добавление назначения** выберите **Пользователи**. 
1. В диалоговом окне **Пользователи** выберите в списке пользователей **B.Simon**, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. На странице **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-syncplicity-test-user"></a>Создание тестового пользователя Syncplicity

Чтобы пользователи Azure AD могли входить систему, их необходимо подготовить для Syncplicity. В этом разделе описывается порядок создания учетных записей пользователей Azure AD в Syncplicity.

**Чтобы подготовить учетные записи пользователей для Syncplicity, выполните следующие действия:**

1. Войдите в клиент **Syncplicity** (например: `https://company.Syncplicity.com`).

1. Щелкните **Admin** (Администрирование), затем **User Accounts** (Учетные записи пользователей) и **Add a User** (Добавить пользователя).

    ![Управление пользователями](./media/syncplicity-tutorial/ic769764.png "Управление пользователями")

1. Введите в поле **Email addresses** (Адреса электронной почты) данные учетной записи Azure AD, которую вам нужно подготовить, затем выберите значение **User** (Пользователь) в поле **Role** (Роль) и щелкните **Next** (Далее).

    ![Сведения об учетной записи](./media/syncplicity-tutorial/ic769765.png "Сведения об учетной записи")

    > [!NOTE]
    > Владелец учетной записи Azure AD получит электронное сообщение со ссылкой для подтверждения и активации учетной записи.

1. Выберите группу в организации, в которую будет входить новый пользователь, и нажмите **Далее**.

    ![Членство в группе](./media/syncplicity-tutorial/ic769772.png "Членство в группе")

    > [!NOTE]
    > Если список групп пуст, просто щелкните **Далее**.

1. Выберите папки, которые будут находиться под управлением Syncplicity на пользовательском компьютере, и нажмите **Далее**.

    ![Папки Syncplicity](./media/syncplicity-tutorial/ic769773.png "Папки Syncplicity")

> [!NOTE]
> Вы можете использовать любые другие средства создания учетной записи пользователя Syncplicity или API-интерфейсы, предоставляемые Syncplicity для подготовки учетных записей пользователей Azure AD.

### <a name="test-sso"></a>Проверка единого входа

Щелкнув плитку "Syncplicity" на панели доступа, вы автоматически войдете в приложение Syncplicity, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

### <a name="update-sso"></a>Обновление единого входа

Каждый раз, когда вы изменяете настройки единого входа, необходимо проверить используемый **сертификат для подписи токена SAML**. Если сертификат был изменен, обязательно загрузите в Syncplicity новый, как описано в разделе **[Настройка единого входа в Syncplicity](#configure-syncplicity-sso)** .

Если вы используете мобильное приложение Syncplicity, обратитесь за помощью в группу поддержки клиентов Syncplicity по адресу support@syncplicity.com.

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)
