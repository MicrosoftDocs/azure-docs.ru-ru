---
title: Руководство по Интеграция Azure Active Directory с Mitel Connect | Документация Майкрософт
description: Из этой статьи вы узнаете, как настроить единый вход с помощью Azure Active Directory в Mitel Connect.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/31/2020
ms.author: jeedes
ms.openlocfilehash: b89ae74a1f4ead568a5454c9494d4a82b0eeea3e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "96015185"
---
# <a name="tutorial-azure-active-directory-integration-with-mitel-micloud-connect-or-cloudlink-platform"></a>Руководство по интеграции Azure Active Directory с Mitel MiCloud Connect или платформой CloudLink

В этом учебнике описано, как использовать приложение Mitel Connect, чтобы интегрировать Azure Active Directory (Azure AD) с Mitel MiCloud Connect или платформой CloudLink. Приложение Mitel Connect доступно в коллекции Azure. Интеграция Azure AD с MiCloud Connect или платформой CloudLink обеспечивает следующие преимущества:

* Вы можете контролировать доступ пользователей к приложениям MiCloud Connect и CloudLink в Azure AD, используя корпоративные учетные данные этих пользователей.
* Из своей учетной записи вы можете включить автоматический вход пользователей в MiCloud Connect или CloudLink (единый вход) с использованием учетных записей Azure AD.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см в статье [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

Если у вас нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/) перед интеграцией Azure AD с Mitel MiCloud Connect или платформой CloudLink.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с MiCloud Connect, вам потребуется:

* Подписка Azure AD. (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* Учетная запись Mitel MiCloud Connect или Mitel CloudLink в зависимости от приложения, которое требуется настроить.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD.

* Mitel Connect поддерживает единый вход, инициированный **поставщиком служб**.
* После настройки Mitel Connect вы можете применить управление сеансами, которое защищает от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="add-mitel-connect-from-the-gallery"></a>Добавление Mitel Connect из коллекции

Чтобы настроить интеграцию Mitel Connect с Azure AD, вам необходимо добавить Mitel Connect из коллекции в список управляемых приложений SaaS на портале Azure.

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева выберите **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Выберите **Новое приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Mitel Connect**, выберите **Mitel Connect** на панели результатов, а затем — **Добавить**.

     ![Mitel Connect в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в MiCloud Connect или на платформе CloudLink с использованием тестового пользователя **_Britta Simon_**. Для работы единого входа необходимо установить связь между пользователем на портале Azure AD и соответствующим пользователем на платформе Mitel. Сведения о настройке и тестировании единого входа Azure AD с MiCloud Connect или платформой CloudLink см. в следующих разделах.
* Настройка и проверка единого входа Azure AD в MiCloud Connect.
* Настройка и проверка единого входа Azure AD на платформе CloudLink.

## <a name="configure-and-test-azure-ad-sso-with-micloud-connect"></a>Настройка и проверка единого входа Azure AD в MiCloud Connect

Чтобы настроить и проверить единый вход Azure AD в MiCloud Connect, вам потребуется выполнить следующие действия:

1. **[Настройка единого входа для MiCloud Connect с помощью Azure AD](#configure-micloud-connect-for-sso-with-azure-ad)** требуется для того, чтобы разрешить пользователям применять эту функцию и настроить параметры единого входа на стороне приложения.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
4. **[Создание тестового пользователя Mitel MiCloud Connect](#create-a-mitel-micloud-connect-test-user)** требуется для того, чтобы в MiCloud Connect существовала учетная запись пользователя Britta Simon, связанного с одноименным пользователем в Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

## <a name="configure-micloud-connect-for-sso-with-azure-ad"></a>Настройка единого входа для MiCloud Connect с помощью Azure AD

В этом разделе описано, как включить единый вход Azure AD для MiCloud Connect на портале Azure и настроить учетную запись в MiCloud Connect для использования единого входа с помощью Azure AD.

Настроить MiCloud Connect с поддержкой единого входа для Azure AD проще всего, параллельно открыв портал Azure и портал Mitel Account. Вам потребуется скопировать некоторые сведения с портала Azure на портал Mitel Account и наоборот.


1. Чтобы открыть страницу конфигураций на [портале Azure](https://portal.azure.com/), сделайте следующее:

    1. На странице интеграции с приложением **Mitel Connect** выберите **Единый вход**.

       ![Ссылка "Настройка единого входа"](common/select-sso.png)

    1. В диалоговом окне **Выбрать метод единого входа** выберите **SAML**.
    
       ![Режим выбора единого входа](common/select-saml-option.png)
    
       Появится страница настройки единого входа на базе SAML.

2. Чтобы открыть диалоговое окно конфигурации на портале Mitel Account, сделайте следующее:

    1. В меню **Phone System** (Телефонная система) выберите **Add-On Features** (Функции надстройки).

    1. Справа от элемента **Single Sign-On** (Единый вход) выберите **Activate** (Активировать) или **Settings** (Параметры).
    
    Откроется диалоговое окно Connect Single Sign-On Settings (Параметры подключения единого входа).
    
3. Установите флажок **Enable Single Sign-On** (Включить единый вход).
    
    ![Снимок экрана, на котором показана страница параметров единого входа Mitel Connect с установленным флажком Enable Single Sign-On (Включить единый вход).](./media/mitel-connect-tutorial/mitel-connect-enable.png)

4. На портале Azure выберите значок **Изменить** в разделе **Базовая конфигурация SAML**.
   
    ![Снимок экрана: страница настройки единого входа с помощью SAML с выделенным значком редактирования.](common/edit-urls.png)

    Появится диалоговое окно "Базовая конфигурация SAML".

5.  На портале Mitel Account скопируйте URL-адрес в поле **Mitel Identifier (Entity ID)** (Идентификатор Mitel (сущности)) и вставьте его в поле **Идентификатор (сущности)** на портале Azure.

6. На портале Mitel Account скопируйте URL-адрес в поле **Reply URL (Assertion Consumer Service URL)** (URL-адрес ответа (URL-адрес службы обработчика утверждений)) и вставьте его в поле **URL-адрес ответа (URL-адрес службы обработчика утверждений)** на портале Azure.

   ![Снимок экрана: раздел базовой конфигурации SAML на портале Azure, а также раздел настройки поставщика удостоверений на портале Mitel Account со строками, указывающими на связь между ними.](./media/mitel-connect-tutorial/mitel-azure-basic-configuration.png)

7. В текстовом поле **URL-адрес входа** введите любой из следующих URL-адресов:

    1. [https://portal.shoretelsky.com]( **https://portal.shoretelsky.com** ) — для использования портала Mitel Account как приложения Mitel по умолчанию;
    1. [https://teamwork.shoretel.com]( **https://teamwork.shoretel.com** ) — для использования Teamwork как приложения Mitel по умолчанию.

    > [!NOTE]
    > Приложение Mitel по умолчанию — это приложение, на которое пользователь переходит, когда щелкает плитку Mitel Connect на Панели доступа. Также это приложение, к которому предоставляется доступ при выполнении тестовой настройки из Azure AD.

8. Нажмите кнопку **Сохранить** в диалоговом окне **Базовая конфигурация SAML** на портале Azure.

9. На портале Azure на странице **Вход на основе SAML** в разделе **Сертификат подписи SAML** выберите **Скачать** рядом с полем **Сертификат (Base64)** , чтобы скачать **сертификат для подписи** и сохранить его на компьютер.

    ![Снимок экрана: панель "Сертификат подписи SAML" для скачивания сертификата.](./media/mitel-connect-tutorial/azure-signing-certificate.png)

10. Откройте файл сертификата для подписи в текстовом редакторе, скопируйте все данные в файле и вставьте их в поле **Signing Certificate** (Сертификат для подписи) на портале Mitel Account. 

      ![Снимок экрана: поле Signing Certificate (Сертификат для подписи).](./media/mitel-connect-tutorial/mitel-connect-signing-certificate.png)

11. На портале Azure на странице **Вход на основе SAML** в разделе **Настройка Mitel Connect** сделайте следующее:

     1. Скопируйте URL-адрес в поле **URL-адрес входа** и вставьте его в поле **Sign-in URL** (URL-адрес входа) на портале Mitel Account.

     1. Скопируйте URL-адрес в поле **Идентификатор Azure AD** и вставьте его в поле **Entity ID** (Идентификатор сущности) на портале Mitel Account.
         
         ![Снимок экрана: связь между страницами настройки единого входа на базе SAML на порталах Azure и Mitel Account.](./media/mitel-connect-tutorial/mitel-azure-set-up-connect.png)

12. Нажмите кнопку **Save** (Сохранить) в диалоговом окне **Connect Single Sign-On Settings** (Параметры подключения единого входа).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

В этом разделе описано, как создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В диалоговом окне "Свойства пользователя" сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    1. В поле **Имя** введите **BrittaSimon**.
  
    1. В поле **Имя пользователя** введите brittasimon@\<yourcompanydomain\>.\<extension\>.  Например, BrittaSimon@contoso.com.

    1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    1. Нажмите кнопку **создания**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Mitel Connect.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Mitel Connect**.

    ![Ссылка на Mitel Connect в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Выберите **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** выберите **Britta Simon** в списке **Пользователи**, затем в нижней части экрана — **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка, а затем в нижней части экрана — **Выбрать**.

7. В диалоговом окне **Добавление назначения** выберите **Назначить**.

### <a name="create-a-mitel-micloud-connect-test-user"></a>Создание тестового пользователя Mitel MiCloud Connect

В этом разделе описано, как создать пользователя Britta Simon из учетной записи MiCloud Connect. Перед использованием единого входа необходимо создать и активировать пользователей.

Дополнительные сведения о добавлении пользователей на портале Mitel Account см. в статье [Adding a User](https://shoretelcommunity.force.com/s/article/Adding-Users-092815) (Добавление пользователя) базы знаний Mitel.

Создайте пользователя из своей учетной записи MiCloud Connect, указав следующие значения:

* **Имя.** Britta Simon
* **Business Email Address:** `brittasimon@<yourcompanydomain>.<extension>` (Рабочий адрес электронной почты)  .  
  (Пример: [brittasimon@contoso.com](mailto:brittasimon@contoso.com).)
* **Username:** (Имя пользователя) `brittasimon@<yourcompanydomain>.<extension>`.  
  (Пример: [brittasimon@contoso.com](mailto:brittasimon@contoso.com). Как правило, имя пользователя совпадает с рабочим адресом электронной почты пользователя.)

> [!NOTE]
> Имя пользователя MiCloud Connect должно совпадать с адресом электронной почты пользователя в Azure.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью Панели доступа.

Выбрав плитку Mitel Connect на Панели доступа, вы автоматически перейдете на страницу входа в приложение MiCloud Connect, которое вы настроили как приложение по умолчанию в поле **URL-адрес входа**. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="configure-and-test-azure-ad-sso-with-cloudlink-platform"></a>Настройка и проверка единого входа Azure AD на платформе CloudLink

В этом разделе описано, как включить единый вход Azure AD для платформы CloudLink на портале Azure и как настроить учетную запись платформы CloudLink, чтобы разрешить единый вход с помощью Azure AD.

Чтобы настроить единый вход платформы CloudLink в Azure AD, рекомендуется параллельно открыть портал Azure и портал CloudLink Accounts, так как потребуется скопировать определенную информацию из портала Azure на портал учетных записей CloudLink и наоборот.

1. Чтобы открыть страницу конфигураций на [портале Azure](https://portal.azure.com/), сделайте следующее:

    1. На странице интеграции с приложением **Mitel Connect** выберите **Единый вход**.

       ![Ссылка "Настройка единого входа"](common/select-sso.png)

    1. В диалоговом окне **Выбрать метод единого входа** выберите **SAML**.

       ![Режим выбора единого входа](common/select-saml-option.png)
    
       Откроется страница **Вход на основе SAML**, в которой отобразится раздел **Базовая конфигурация SAML**.

       ![Снимок экрана: страница "Вход на основе SAML" с разделом "Базовая конфигурация SAML".](./media/mitel-connect-tutorial/mitel-azure-saml-settings.png)

2. Чтобы получить доступ к панели конфигурации **единого входа Azure AD** на портале CloudLink Accounts, сделайте следующее:

    1. Перейдите на страницу **сведений об учетной записи** для учетной записи клиента, с которой необходимо включить интеграцию.

    1. В разделе **Integrations** (Интеграции) выберите **+ Add new** (+ Добавить). Во всплывающем окне отобразится панель **Integrations** (Интеграции).

    1. Выберите вкладку **3rd party** (Сторонние). Отобразится список поддерживаемых приложений сторонних производителей. Нажмите кнопку **Add** (Добавить), связанную с **единым входом Azure AD**, затем выберите **Done** (Готово).

       ![Снимок экрана: страница "Интеграции" с возможностью добавления единого входа Azure AD](./media/mitel-connect-tutorial/mitel-cloudlink-integrations.png)

       **Единый вход Azure AD** включен для учетной записи клиента и добавляется в раздел **интеграции** на странице **сведений об учетной записи**.   

   1. Выберите **Complete Setup** (Завершить установку).
    
      ![Снимок экрана: параметр Complete Setup (Завершить установку) для единого входа Azure AD.](./media/mitel-connect-tutorial/mitel-cloudlink-complete-setup.png)
      
      Откроется панель конфигурации **единого входа Azure AD**.
      
       ![Снимок экрана: конфигурация единого входа Azure AD.](./media/mitel-connect-tutorial/mitel-cloudlink-sso-setup.png)
       
       Mitel рекомендует не устанавливать флажок **Enable Mitel Credentials (Optional)** (Включить учетные данные Mitel (необязательно)) в разделе **Optional Mitel credentials** (Дополнительные учетные данные Mitel). Установите этот флажок в том случае, если вы хотите, чтобы пользователь входил в приложение CloudLink, используя учетные данные Mitel помимо параметра единого входа.

3. На портале Azure на странице **Вход на основе SAML** щелкните значок **Изменить** в разделе **Базовая конфигурация SAML**. Откроется панель **Базовая конфигурация SAML**.

    ![Снимок экрана: панель "Базовая конфигурация SAML" с выбранным значком редактирования.](./media/mitel-connect-tutorial/mitel-azure-saml-basic.png)
 
 4. На портале CloudLink Accounts скопируйте URL-адрес в поле **Mitel Identifier (Entity ID)** (Идентификатор Mitel (сущности)) и вставьте его в поле **Идентификатор (сущности)** на портале Azure.

 5. На портале CloudLink Accounts скопируйте URL-адрес в поле **Reply URL (Assertion Consumer Service URL)** (URL-адрес ответа (URL-адрес службы обработчика утверждений)) и вставьте его в поле **URL-адрес ответа (URL-адрес службы обработчика утверждений)** на портале Azure.  
    
    ![Снимок экрана: связь между страницами на порталах CloudLink Accounts и Azure.](./media/mitel-connect-tutorial/mitel-cloudlink-saml-mapping.png) 

 6. В текстовом поле **URL-адрес входа** введите URL-адрес `https://accounts.mitel.io`, чтобы использовать портал CloudLink Accounts в качестве приложения Mitel по умолчанию.
     
     ![Снимок экрана: текстовое поле "URL-адрес входа".](./media/mitel-connect-tutorial/mitel-cloudlink-sign-on-url.png)
  
     > [!NOTE]
     > Приложение Mitel по умолчанию — это приложение, на которое пользователь переходит, когда выбирает плитку Mitel Connect на Панели доступа. Также это приложение, к которому предоставляется доступ при выполнении пользователем тестовой настройки из Azure AD.

7. Нажмите кнопку **Сохранить** в диалоговом окне **Базовая конфигурация SAML**.

8. На портале Azure на странице **Вход на основе SAML** в разделе **Сертификат подписи SAML** выберите **Скачать** рядом с полем **Сертификат (Base64)** , чтобы скачать **сертификат для подписи**. Сохраните сертификат на своем компьютере.
  
    ![Снимок экрана: раздел "Сертификат подписи SAML" для скачивания сертификата в формате Base64.](./media/mitel-connect-tutorial/mitel-cloudlink-save-certificate.png)

9. Откройте файл сертификата для подписи в текстовом редакторе, скопируйте все данные в файле и вставьте их в поле **Signing Certificate** (Сертификат для подписи) на портале CloudLink Accounts.  

    > [!NOTE]
    > Если у вас несколько сертификатов, рекомендуется их вставлять один за другим. 
       
    ![Снимок экрана: второй шаг процедуры, в рамках которого необходимо ввести значения из интеграции Azure AD.](./media/mitel-connect-tutorial/mitel-cloudlink-enter-certificate.png)

10. На портале Azure на странице **Вход на основе SAML** в разделе **Настройка Mitel Connect** сделайте следующее:

     1. Скопируйте URL-адрес в поле **URL-адрес входа** и вставьте его в поле **Sign-in URL** (URL-адрес входа) на портале CloudLink Accounts.

     1. Скопируйте URL-адрес в поле **Идентификатор Azure AD** и вставьте его в поле **IDP Identifier (Entity ID)** (Идентификатор поставщика удостоверений (сущности)) на портале CloudLink Accounts.
     
        ![Снимок экрана: источник описанных здесь значений в Mintel Connect.](./media/mitel-connect-tutorial/mitel-cloudlink-copy-settings.png)

11. Нажмите кнопку **Сохранить** на панели **единого входа Azure AD** на портале CloudLink Accounts.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

В этом разделе описано, как создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В диалоговом окне "Свойства пользователя" сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    1. В поле **Имя** введите **BrittaSimon**.
  
    1. В поле **Имя пользователя** введите brittasimon@\<yourcompanydomain\>.\<extension\>.  Например, BrittaSimon@contoso.com.

    1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    1. Нажмите кнопку **создания**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Mitel Connect.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Mitel Connect**.

    ![Ссылка на Mitel Connect в списке приложений](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Выберите **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** выберите **Britta Simon** в списке **Пользователи**, затем в нижней части экрана — **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка, а затем в нижней части экрана — **Выбрать**.

7. В диалоговом окне **Добавление назначения** выберите **Назначить**.

### <a name="create-a-cloudlink-test-user"></a>Создание тестового пользователя CloudLink

В этом разделе описывается создание тестового пользователя с именем **_Britta Simon_** на платформе CloudLink. Перед использованием единого входа необходимо создать и активировать пользователя.

Дополнительные сведения о добавлении пользователей на портал учетных записей CloudLink см. в разделе **_Управление пользователями_** в [документации об учетных записях CloudLink](https://www.mitel.com/document-center/technology/cloudlink/all-releases/en/cloudlink-accounts-html).

Создайте пользователя на портале учетных записей CloudLink, указав следующие значения:

* Имя: Britta Simon
* Имя: Britta
* Фамилия: Simon
* Адрес электронной почты: BrittaSimon@contoso.com

> [!NOTE]
> Адрес электронной почты пользователя CloudLink должен совпадать с **именем субъекта-пользователя** на портале Azure.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Выбрав плитку Mitel Connect на Панели доступа, вы автоматически перейдете на страницу входа в приложение CloudLink, которое вы настроили как приложение по умолчанию в поле **URL-адрес входа**. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)