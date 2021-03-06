---
title: Учебник по интеграции Azure Active Directory с Genea Access Control | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Genea Access Control.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/17/2021
ms.author: jeedes
ms.openlocfilehash: 82c05f77781abdaea3b2c84aa1071656c206439a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104669869"
---
# <a name="tutorial-azure-active-directory-integration-with-genea-access-control"></a>Учебник по интеграции Azure Active Directory с Genea Access Control

В этой инструкции описано, как интегрировать Genea Access Control с Azure Active Directory (Azure AD). Интеграция Genea Access Control с Azure AD обеспечивает следующие возможности:

* Контроль доступа к Genea Access Control с помощью Azure AD.
* Автоматический вход пользователей в Genea Access Control с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Genea Access Control, вам потребуется:

* Подписка Azure AD. (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* Подписка Genea Access Control с поддержкой единого входа

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение Genea Access Control поддерживает единый вход, инициированный **поставщиком услуг и поставщиком удостоверений**.
> [!NOTE]
> Идентификатор этого приложения — фиксированное строковое значение, поэтому в одном клиенте можно настроить только один экземпляр.


## <a name="adding-genea-access-control-from-the-gallery"></a>Добавление Genea Access Control из коллекции

Чтобы настроить интеграцию Genea Access Control с Azure AD, необходимо добавить Genea Access Control из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Genea Access Control**.
1. Выберите **Genea Access Control** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-sso-for-genea-access-control"></a>Настройка и проверка единого входа Azure AD для Genea Access Control

Настройте и проверьте единый вход Azure AD в Genea Access Control с помощью тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Genea Access Control.

Чтобы настроить и проверить единый вход Azure AD в Genea Access Control, выполните следующие действия:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Genea Access Control](#configure-genea-access-control-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Genea Access Control](#create-genea-access-control-test-user)** требуется для того, чтобы в Genea Access Control существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Genea Access Control** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок карандаша, чтобы открыть диалоговое окно **Базовая конфигурация SAML** для изменения параметров.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующее действие.

    В текстовом поле **Идентификатор** введите URL-адрес: `https://login.sequr.io`.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    а. В текстовом поле **URL-адрес входа** введите URL-адрес: `https://login.sequr.io`.

    b. В текстовом поле **Состояние ретранслятора** вы получите это значение, которое описано далее в руководстве.

6. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

7. Требуемые URL-адреса можно скопировать из раздела **Настройка Genea Access Control**.

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

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к Genea Access Control.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Genea Access Control**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
## <a name="configure-genea-access-control-sso"></a>Настройка единого входа в Genea Access Control

1. В другом окне веб-браузера войдите на корпоративный веб-сайт Genea Access Control с правами администратора.

1. Щелкните **Integrations** (Интеграции) на панели навигации слева.

    ![Снимок экрана: пункт Integration (Интеграция) на панели навигации слева.](./media/sequr-tutorial/configure-1.png)

1. Прокрутите вниз до раздела **Single Sign-On** (Единый вход) и нажмите кнопку **Manage** (Управление).

    ![Снимок экрана для раздела единого входа, где выделена кнопка управления.](./media/sequr-tutorial/configure-2.png)

1. В разделе **Manage Single Sign-On** (Управление единым входом) сделайте следующее:

    ![Снимок экрана: раздел управления единым входом для ввода описанных здесь значений.](./media/sequr-tutorial/configure-3.png)

    а. В текстовое поле **URL-адрес единого входа для поставщика удостоверений** вставьте значение **URL-адреса входа**, скопированное на портале Azure.

    b. Перетащите файл **сертификата**, который вы скачали с портала Azure, или вручную введите содержимое сертификата.

    c. После сохранения конфигурации будет создано значение состояния ретранслятора. Скопируйте значение **состояния ретранслятора** и вставьте его в текстовое поле **Состояние ретранслятора** в разделе **Базовая конфигурация SAML** на портале Azure.

    d. Выберите команду **Сохранить**.

### <a name="create-genea-access-control-test-user"></a>Создание тестового пользователя Genea Access Control

В этом разделе описано, как создать пользователя Britta Simon в приложении Genea Access Control. Чтобы добавить пользователей на платформу Genea Access Control, обратитесь в [группу поддержки Genea Access Control](mailto:support@sequr.io). Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

#### <a name="sp-initiated"></a>Инициация поставщиком услуг:

* Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в Genea Access Control, где можно инициировать поток входа.  

* Перейдите по URL-адресу для входа в Genea Access Control и инициируйте оттуда поток входа.

#### <a name="idp-initiated"></a>Вход, инициированный поставщиком удостоверений

* На портале Azure выберите **Тестировать приложение**, после чего вы автоматически войдете в приложение Genea Access Control, для которого настроен единый вход 

Вы можете также использовать портал "Мои приложения" корпорации Майкрософт для тестирования приложения в любом режиме. Щелкнув плитку Genea Access Control на портале "Мои приложения", вы будете перенаправлены на страницу входа приложения для инициации потока входа (при настройке в режиме поставщика службы) или автоматически войдете в приложение Genea Access Control, для которого настроен единый вход (при настройке в режиме поставщика удостоверений). Дополнительные сведения о портале "Мои приложения" см. в [этой статье](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Дальнейшие действия

После настройки Genea Access Control вы можете применить функцию управления сеансом, которая защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).