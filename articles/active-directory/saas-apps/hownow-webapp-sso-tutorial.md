---
title: Руководство по Интеграция единого входа Azure Active Directory с HowNow WebApp SSO | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и HowNow WebApp SSO.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/06/2020
ms.author: jeedes
ms.openlocfilehash: 8d5881d838c4fe952206afb827fd60ed98dbba86
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "96178353"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hownow-webapp-sso"></a>Руководство по интеграции единого входа Azure Active Directory с HowNow WebApp SSO

В этом руководстве описано, как интегрировать HowNow WebApp SSO с Azure Active Directory (AAD). Интеграция HowNow WebApp SSO с Azure AD обеспечивает следующие возможности:

* С помощью Azure AD вы можете контролировать доступ к HowNow WebApp SSO.
* Вы можете включить для пользователей автоматический вход в HowNow WebApp SSO с учетными записями Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка HowNow WebApp SSO с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение HowNow WebApp SSO поддерживает единый вход, инициируемый **поставщиком услуг**.

* HowNow WebApp SSO поддерживает **JIT**-подготовку пользователей

## <a name="adding-hownow-webapp-sso-from-the-gallery"></a>Добавление HowNow WebApp SSO из коллекции

Чтобы настроить интеграцию HowNow WebApp SSO с Azure AD, необходимо добавить HowNow WebApp SSO из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **HowNow WebApp SSO**.
1. Выберите **HowNow WebApp SSO** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-hownow-webapp-sso"></a>Настройка и проверка единого входа Azure Active Directory для HowNow WebApp SSO

Настройте и проверьте единый вход Azure AD в HowNow WebApp SSO с помощью тестового пользователя **B.Simon**. Чтобы обеспечить единый вход, необходимо установить связь между пользователем Azure Active Directory и соответствующим пользователем в HowNow WebApp SSO.

Чтобы настроить и проверить единый вход Azure AD в HowNow WebApp SSO, выполните действия из следующих стандартных блоков:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в приложении HowNow WebApp SSO](#configure-hownow-webapp-sso-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя HowNow WebApp SSO](#create-hownow-webapp-sso-test-user)** требуется для того, чтобы в HowNow WebApp SSO существовал пользователь B.Simon, связанный с представлением этого же пользователя в AAD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **HowNow WebApp SSO** найдите раздел **Управление** и выберите элемент **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<CUSTOMER_NAME>.hownow.app/users/saml/sign_in`.

    b. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате: `https://<CUSTOMER_NAME>.hownow.app/users/saml/metadata`.

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<CUSTOMER_NAME>.hownow.app/users/saml/auth`.

    > [!NOTE]
    > Эти значения приведены для примера. Вместо них необходимо указать фактические значения URL-адреса входа, идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь в [службу технической поддержки клиентов HowNow WebApp SSO](mailto:support@gethownow.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. В разделе **Сертификат подписи SAML** щелкните кнопку **Правка**, чтобы открыть диалоговое окно **Сертификат подписи SAML**.

    ![Изменение сертификата подписи SAML](common/edit-certificate.png)

1. В разделе **Сертификат подписи SAML** скопируйте **значение отпечатка** и сохраните его на компьютере.

    ![Копирование значения "Отпечаток"](common/copy-thumbprint.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка приложения HowNow WebApp SSO**.

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

В этом разделе описано, как включить единый вход в Azure для пользователя B.Simon, предоставив этому пользователю доступ к HowNow WebApp SSO.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **HowNow WebApp SSO**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-hownow-webapp-sso-sso"></a>Настройка единого входа в HowNow WebApp SSO

Чтобы настроить единый вход на стороне приложения **HowNow WebApp SSO**, нужно отправить **значение отпечатка** и соответствующие URL-адреса, скопированные на портале Azure, в [службу технической поддержки HowNow WebApp SSO](mailto:support@gethownow.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-hownow-webapp-sso-test-user"></a>Создание тестового пользователя HowNow WebApp SSO

В рамках этого раздела вы создадите в HowNow WebApp SSO пользователя B.Simon. Приложение HowNow WebApp SSO поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в HowNow WebApp SSO, он создается автоматически после проверки подлинности.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

* Выберите **Тестировать приложение** на портале Azure. После этого действия открывается URL-адрес для входа в HowNow WebApp SSO, где можно инициировать поток входа. 

* Вручную введите URL-адрес для входа в HowNow WebApp SSO и инициируйте поток входа.

* Вы можете использовать Панель доступа (Майкрософт). Щелкните плитку HowNow WebApp SSO на панели доступа. Откроется URL-адрес для входа в HowNow WebApp SSO. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="next-steps"></a>Дальнейшие действия

После настройки HowNow WebApp SSO вы можете применить функцию управления сеансом, которая в реальном времени защищает конфиденциальные данные вашей организации от кражи и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).