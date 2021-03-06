---
title: Руководство по интеграции единого входа Azure Active Directory с приложением IntelligenceBank | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в IntelligenceBank.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/15/2020
ms.author: jeedes
ms.openlocfilehash: 9887c435c2aa8dc7ba8cab9481cf30195c4b334e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "92459940"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-intelligencebank"></a>Руководство по интеграции единого входа Azure Active Directory с приложением IntelligenceBank

В этом учебнике описано, как интегрировать IntelligenceBank с Azure Active Directory (Azure AD). Интеграция IntelligenceBank с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD вы можете контролировать доступ к IntelligenceBank.
* Вы можете включить автоматический вход пользователей в IntelligenceBank с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка приложения IntelligenceBank с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* IntelligenceBank поддерживает единый вход, инициированный **поставщиком услуг**.

* После настройки IntelligenceBank можно применить управление сеансами, которое защищает от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-intelligencebank-from-the-gallery"></a>Добавление IntelligenceBank из коллекции

Чтобы настроить интеграцию IntelligenceBank с Azure AD, необходимо добавить IntelligenceBank из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **IntelligenceBank**.
1. Выберите **IntelligenceBank** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-single-sign-on-for-intelligencebank"></a>Настройка и проверка единого входа Azure AD для IntelligenceBank

Настройте и проверьте единый вход Azure AD в IntelligenceBank с помощью тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем AAD и соответствующим пользователем в приложении IntelligenceBank.

Чтобы настроить и проверить единый вход Azure AD в IntelligenceBank, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в IntelligenceBank](#configure-intelligencebank-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя приложения IntelligenceBank](#create-intelligencebank-test-user)** требуется для того, чтобы в IntelligenceBank существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **IntelligenceBank** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<SUBDOMAIN>.intelligencebank.com`.

    b. В текстовом поле **Идентификатор (сущности)** введите любое из следующих значений:

    - `IB`
    - `IntelligenceBank`
    - `https://<SUBDOMAIN>.intelligencebank.com`

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<SUBDOMAIN>.intelligencebank.com/auth`.

    > [!NOTE]
    > Эти значения приведены для примера. Вместо них необходимо указать фактические значения URL-адреса входа, идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь к [группе поддержки IntelligenceBank](mailto:helpdesk@intelligencebank.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка IntelligenceBank**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход в Azure для пользователя B. Simon, предоставив этому пользователю доступ к IntelligenceBank.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **IntelligenceBank**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-intelligencebank-sso"></a>Настройка единого входа IntelligenceBank

1. В другом окне веб-браузера войдите на корпоративный сайт IntelligenceBank в качестве администратора.

1. Щелкните **Структура проверки подлинности** и выберите **Добавить**.

    ![Снимок экрана, на котором выбрана вкладка администрирования и значок добавления элемента.](./media/intelligencebank-tutorial/authenticator.PNG)

1. Выполните следующие действия:

    ![Снимок экрана с полями, где можно ввести описанные на этом шаге сведения.](./media/intelligencebank-tutorial/urls.PNG)

    а. В текстовом поле **Name** (Имя) введите имя, например `azureadsso`.

    b. В текстовом поле **Описание** введите допустимое описание.

    c. В качестве параметра **Тип** выберите **SAML** из раскрывающегося списка.

    d. В текстовое поле **Удаленный URL-адрес** вставьте значение **URL-адрес входа**, скопированное на портале Azure.

    д) В текстовое поле **Узел** вставьте значение **идентификатора сущности**, скопированное на портале Azure.

    е) Откройте в блокноте скачанный с портала Azure файл **сертификата (Base64)** и вставьте его содержимое в текстовое поле **CertData**.

    ж. В текстовое поле **SingleLogoutService** вставьте значение **Log out URL** (URL-адреса выхода), скопированное на портале Azure.

    h. Нажмите кнопку **Сохранить**.

### <a name="create-intelligencebank-test-user"></a>Создание тестового пользователя IntelligenceBank

1. В другом окне веб-браузера войдите на корпоративный сайт IntelligenceBank в качестве администратора.

1. Перейдите в раздел **Администратор** -> **Пользователи** и выберите **Add New User Icon** (Добавить значок нового пользователя), чтобы добавить **пользователя**.

    ![Снимок экрана для вкладки пользователей, где выбран значок "Пользователи".](./media/intelligencebank-tutorial/creating-user.PNG)

1. Заполните необходимые поля в соответствии с требованиями вашей организации и щелкните **Сохранить**.

    ![Снимок экрана: страница добавления пользователя, где можно ввести сведения о пользователе.](./media/intelligencebank-tutorial/creating-user-1.PNG)

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку IntelligenceBank на Панели доступа, вы автоматически войдете в приложение IntelligenceBank, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)

- [Попробуйте использовать IntelligenceBank с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Защита приложений с помощью функции управления настройками условного доступа для приложений в Microsoft Cloud App Security](/cloud-app-security/proxy-intro-aad)