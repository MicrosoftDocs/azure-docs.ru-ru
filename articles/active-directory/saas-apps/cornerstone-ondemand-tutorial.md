---
title: Руководство по интеграции единого входа Azure Active Directory с Cornerstone Single Sign-On | Документация Майкрософт
description: Сведения о настройке единого входа между Azure Active Directory и Cornerstone Single Sign-On.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/02/2021
ms.author: jeedes
ms.openlocfilehash: ba6eb0a1b607fc05c4d0c660dd3d7016f81ef4b3
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2021
ms.locfileid: "106449534"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cornerstone-single-sign-on"></a>Руководство по интеграции единого входа Azure Active Directory с Cornerstone Single Sign-On

В этом учебнике описано, как интегрировать Azure Active Directory (Azure AD) с Cornerstone Single Sign-On. Интеграция Cornerstone Single Sign-On с Azure AD обеспечивает следующие возможности:

* контроль доступа к Cornerstone Single Sign-On с помощью Azure AD;
* автоматический вход пользователей в Cornerstone Single Sign-On с помощью учетных записей Azure AD;
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* подписка Cornerstone Single Sign-On с поддержкой единого входа.

> [!NOTE]
> Эту интеграцию также можно использовать в облачной среде Azure AD для государственных организаций США. Это приложение можно найти в коллекции облачных приложений с поддержкой Azure AD для государственных организаций США и настроить таким же образом, как и в общедоступном облаке.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение Cornerstone Single Sign-On поддерживает единый вход, инициированный **поставщиком услуг**.
* Cornerstone Single Sign-On поддерживает [автоматическую подготовку пользователей](cornerstone-ondemand-provisioning-tutorial.md).


## <a name="adding-cornerstone-single-sign-on-from-the-gallery"></a>Добавление Cornerstone Single Sign-On из коллекции

Чтобы настроить интеграцию Azure AD с Cornerstone Single Sign-On, вам нужно добавить Cornerstone Single Sign-On из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Cornerstone Single Sign-On**.
1. Выберите **Cornerstone Single Sign-On** в области результатов и добавьте приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-sso-for-cornerstone-single-sign-on"></a>Настройка и проверка единого входа Azure AD для Cornerstone Single Sign-On

Настройте и проверьте единый вход Azure AD в Cornerstone Single Sign-On с помощью тестового пользователя **B.Simon**. Чтобы обеспечить работу единого входа, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Cornerstone Single Sign-On.

Чтобы настроить и проверить единый вход Azure AD в Cornerstone Single Sign-On, выполните следующие действия:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
2. **[Настройка единого входа в Cornerstone Single Sign-On](#configure-cornerstone-single-sign-on-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя в Cornerstone Single Sign-On](#create-cornerstone-single-sign-on-test-user)** требуется для того, чтобы в Cornerstone Single Sign-On существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
3. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Cornerstone Single Sign-On** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок карандаша, чтобы открыть диалоговое окно **Базовая конфигурация SAML** для изменения параметров.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<PORTAL_NAME>.csod.com`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<PORTAL_NAME>.csod.com/samldefault.aspx?ouid=<OUID>`.

    c. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<PORTAL_NAME>.csod.com/samldefault.aspx?ouid=<OUID>`.

    > [!NOTE]
    > Эти значения приведены для примера. Укажите вместо них фактические значения URL-адреса ответа, идентификатора и URL-адреса входа. Чтобы получить их, обратитесь в [группу поддержки Cornerstone Single Sign-On](mailto:moreinfo@csod.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

4. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемый URL-адрес можно скопировать из раздела **настройки Cornerstone Single Sign-On**.

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

В этом разделе описано, как включить единый вход Azure для пользователя B.Simon, предоставив этому пользователю доступ к Cornerstone Single Sign-On.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Cornerstone Single Sign-On**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-cornerstone-single-sign-on-sso"></a>Настройка единого входа в Cornerstone Single Sign-On

Чтобы настроить единый вход на стороне **Cornerstone Single Sign-On**, отправьте скачанный **сертификат (Base64)** и соответствующие URL-адреса, скопированные на портале Azure, в [службу поддержки Cornerstone Single Sign-On](mailto:moreinfo@csod.com) или свяжитесь с партнером. Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-cornerstone-single-sign-on-test-user"></a>Создание тестового пользователя Cornerstone Single Sign-On

Цель этого раздела — создать пользователя с именем B.Simon в Cornerstone Single Sign-On. Cornerstone Single Sign-On поддерживает автоматическую подготовку пользователей, которая по умолчанию включена. Дополнительные сведения о настройке автоматической подготовки пользователей можно найти [здесь](./cornerstone-ondemand-provisioning-tutorial.md).

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

* Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в Cornerstone Single Sign-On, где можно инициировать поток входа. 

* Перейдите по URL-адресу для входа в Cornerstone Single Sign-On и инициируйте поток входа.

* Вы можете использовать портал "Мои приложения" корпорации Майкрософт. Щелкнув плитку Cornerstone Single Sign-On на портале "Мои приложения", вы перейдете по URL-адресу для входа в Cornerstone Single Sign-On. Дополнительные сведения о портале "Мои приложения" см. в [этой статье](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Дальнейшие действия

После настройки Cornerstone Single Sign-On вы можете применить управление сеансами, которое защищает от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).