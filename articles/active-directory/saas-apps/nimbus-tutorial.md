---
title: Руководство по интеграции единого входа Azure Active Directory с Nimbus | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Nimbus.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/17/2020
ms.author: jeedes
ms.openlocfilehash: 0cc005ee22bff897a87679a0bde95ffec6e98e51
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "92522416"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-nimbus"></a>Руководство по интеграции единого входа Azure Active Directory с Nimbus

В этом руководстве описано, как интегрировать Nimbus с Azure Active Directory (Azure AD). Интеграция Nimbus с Azure AD обеспечивает следующие возможности.

* Управление доступом к Nimbus с помощью Azure AD.
* Автоматический вход пользователей в Nimbus с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Nimbus с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Nimbus поддерживает единый вход, инициируемый **поставщиком услуг и поставщиком удостоверений**.
* Nimbus поддерживает **JIT-подготовку** пользователей

## <a name="adding-nimbus-from-the-gallery"></a>Добавление Nimbus из коллекции

Чтобы настроить интеграцию Nimbus с Azure AD, необходимо добавить Nimbus из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Nimbus**.
1. Выберите **Nimbus** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-nimbus"></a>Настройка и проверка единого входа Azure AD для Nimbus

Настройте и проверьте единый вход Azure AD в Nimbus, используя данные тестового пользователя **B. Simon**. Чтобы обеспечить работу единого входа, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Nimbus.

Чтобы настроить и проверить единый вход Azure AD в Nimbus, выполните действия из следующих стандартных блоков:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Nimbus](#configure-nimbus-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Nimbus](#create-nimbus-test-user)** требуется для того, чтобы в Nimbus существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Nimbus** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<CUSTOMER_NAME>.time2work.com/Security/ADFS.aspx`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<CUSTOMER_NAME>.time2work.com/Security/ADFS.aspx?authmode=15&saml=2.0`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<CUSTOMER_NAME>.time2work.com/`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, обратитесь в [группу поддержки Nimbus](mailto:support@nimbus.cloud). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)
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

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к Nimbus.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **Nimbus**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-nimbus-sso"></a>Настройка единого входа для Nimbus

Для настройки единого входа на стороне **Nimbus** необходимо отправить **URL-адрес метаданных федерации приложений** в [группу поддержки Nimbus](mailto:support@nimbus.cloud). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-nimbus-test-user"></a>Создание тестового пользователя Nimbus

В этом разделе вы создадите в Nimbus пользователя с именем Britta Simon. Приложение Nimbus поддерживает JIT-подготовку пользователей, которая включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Nimbus, он создается после проверки подлинности.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

#### <a name="sp-initiated"></a>Инициация поставщиком услуг:

* Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в Nimbus, где можно инициировать поток входа.  

* Перейдите по URL-адресу для входа в Nimbus и инициируйте поток входа.

#### <a name="idp-initiated"></a>Вход, инициированный поставщиком удостоверений

* На портале Azure выберите **Тестировать приложение**, и вы автоматически войдете в приложение Nimbus, для которого настроен единый вход. 

Вы можете также использовать Панель доступа корпорации Майкрософт для тестирования приложения в любом режиме. Щелкнув плитку Nimbus на Панели доступа, вы будете перенаправлены на страницу входа приложения, если выполнены настройки для использования в режиме поставщика услуг, или автоматически войдете в приложение Nimbus, для которого настроен единый вход, если выполнены настройки для использования в режиме поставщика удостоверений. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="next-steps"></a>Дальнейшие действия

После настройки Nimbus вы можете применить функцию управления сеансом, которая защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).