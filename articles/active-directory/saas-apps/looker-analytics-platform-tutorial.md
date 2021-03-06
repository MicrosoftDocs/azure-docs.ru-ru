---
title: Руководство. Интеграция единого входа Azure Active Directory с Looker Analytics Platform | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Looker Analytics Platform.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/10/2020
ms.author: jeedes
ms.openlocfilehash: dbb6f6d278256730e77677e78f452615fe4b611e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "96180750"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-looker-analytics-platform"></a>Руководство. Интеграция единого входа Azure Active Directory с Looker Analytics Platform

В этом руководстве описано, как интегрировать Looker Analytics Platform с Azure Active Directory (Azure AD). Интеграция Looker Analytics Platform с Azure AD обеспечивает возможности, перечисленные ниже.

* С помощью Azure AD можно контролировать доступ к Looker Analytics Platform.
* Вы можете включить для пользователей автоматический вход в Looker Analytics Platform с учетными записями Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Looker Analytics Platform с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение Looker Analytics Platform поддерживает единый вход, инициированный **поставщиком службы и поставщиком удостоверений**.
* Looker Analytics Platform поддерживает **JIT**-подготовку пользователей.


## <a name="adding-looker-analytics-platform-from-the-gallery"></a>Добавление Looker Analytics Platform из коллекции

Чтобы настроить интеграцию Looker Analytics Platform с Azure AD, необходимо добавить Looker Analytics Platform из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Looker Analytics Platform**.
1. Выберите **Looker Analytics Platform** на панели результатов, а затем добавьте приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-looker-analytics-platform"></a>Настройка и проверка единого входа Azure AD для Looker Analytics Platform

Настройте и проверьте единый вход Azure AD в Looker Analytics Platform с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Looker Analytics Platform.

Чтобы настроить и проверить единый вход Azure AD в Looker Analytics Platform, выполните действия из следующих стандартных блоков.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Looker Analytics Platform](#configure-looker-analytics-platform-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Looker Analytics Platform](#create-looker-analytics-platform-test-user)** требуется для того, чтобы в Looker Analytics Platform существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Looker Analytics Platform** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `<SPN>_looker`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<SUBDOMAIN>.looker.com/samlcallback`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<SUBDOMAIN>.looker.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, обратитесь в [службу технической поддержки Looker Analytics Platform](mailto:support@looker.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка Looker Analytics Platform**.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к Looker Analytics Platform.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Looker Analytics Platform**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-looker-analytics-platform-sso"></a>Настройка единого входа в Looker Analytics Platform

1. В другом окне веб-браузера войдите на веб-сайт Looker Analytics Platform с правами администратора.

1. Поочередно откройте **Admin** -> **Authentication** -> **SAML** (Администрирование > Проверка подлинности > SAML).

    ![Снимок экрана: выбор SAML](./media/looker-analytics-platform-tutorial/admin.png)

1. Вставьте значение **метаданных федерации**, которые вы скопировали на портале Azure, в текстовое поле **IDP Metadata** (Метаданные поставщика удостоверений), и щелкните **Load** (Загрузить).

    ![Снимок экрана: отправка метаданных](./media/looker-analytics-platform-tutorial/metadata.png)

1. Выполните действия из следующих стандартных блоков раздела **User Attribute Settings** (Параметры атрибутов пользователей).

    ![Снимок экрана: раздел User Attribute Settings (Параметры атрибутов пользователей)](./media/looker-analytics-platform-tutorial/user-attribute-settings.png)

    А. Добавьте следующее значение в поле Email Attr (Атрибут электронной почты): `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    Б. Добавьте следующее значение в поле Fname Attr (Атрибут личного имени): `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    c. Добавьте следующее значение в поле Lname Attr (Атрибут фамилии): `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`

    d. В разделе **Test User Authentication** (Тест проверки подлинности пользователя) щелкните действие **Test SAML Authentication** (Протестировать проверку подлинности SAML). Если в ответ загрузится страница с сообщением Server Response Successfully Validated (Ответ сервера успешно проверен), значит интеграция SAML настроена правильно.
    
    д) В разделе **Save and Apply Settings** (Сохранить и применить настройки) установите флажок **I have confirmed the configuration above and want to enable applying it globally** (Представленная выше конфигурация проверена и будет применена глобально).

    е) Нажмите кнопку **Update Settings** (Изменить параметры).

### <a name="create-looker-analytics-platform-test-user"></a>Создание тестового пользователя Looker Analytics Platform

В этом разделе вы создадите в Looker Analytics Platform пользователя B.Simon. Looker Analytics Platform поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Looker Analytics Platform, он создается автоматически после проверки подлинности.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

#### <a name="sp-initiated"></a>Инициация поставщиком услуг:

* Выберите **Тестировать приложение** на портале Azure. Это действие открывает URL-адрес для входа в Looker Analytics Platform, где можно инициировать поток входа.  

* Вручную введите URL-адрес для входа в Looker Analytics Platform и инициируйте поток входа.

#### <a name="idp-initiated"></a>Вход, инициированный поставщиком удостоверений

* Выберите **Тестировать приложение** на портале Azure, и вы автоматически войдете в Looker Analytics Platform, для которого только что настроили единый вход. 

Вы можете также использовать Панель доступа корпорации Майкрософт для тестирования приложения в любом режиме. Щелкнув плитку Looker Analytics Platform на Панели доступа, вы будете перенаправлены на страницу входа приложения, если выполнены настройки для использования в режиме поставщика услуг, или автоматически войдете в приложение ooker Analytics Platform, для которого настроен единый вход, если выполнены настройки для использования в режиме поставщика удостоверений. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="next-steps"></a>Дальнейшие действия

После настройки Looker Analytics Platform вы можете применить функцию управления сеансом, которая в реальном времени защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).