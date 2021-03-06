---
title: Руководство по интеграции Azure Active Directory с Meta Networks Connector | Документы Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Meta Networks Connector.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: b14a75dba2860c9dee58e40673d3299fdde277e7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "92516874"
---
# <a name="tutorial-azure-active-directory-integration-with-meta-networks-connector"></a>Руководство по интеграции Azure Active Directory с Meta Networks Connector

В этом руководстве описано, как интегрировать Meta Networks Connector с Azure Active Directory (Azure AD).
Интеграция Meta Networks Connector с Azure AD обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к Meta Networks Connector.
* Вы можете включить автоматический вход пользователей в Meta Networks Connector (единый вход) с учетными записями Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>предварительные требования

Чтобы настроить интеграцию Azure AD с Meta Networks Connector, вам потребуется:

* подписка Azure AD; (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Meta Networks Connector с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Meta Networks Connector поддерживает инициированный единый вход **пакета обновлений** и **выдающей точки распространения**.
 
* Meta Networks Connector поддерживает **JIT**-подготовку пользователей.

## <a name="adding-meta-networks-connector-from-the-gallery"></a>Добавление Meta Networks Connector из коллекции

Чтобы настроить интеграцию Meta Networks Connector с Azure AD, нужно добавить Meta Networks Connector из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Meta Networks Connector из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Meta Networks Connector**, выберите **Meta Networks Connector** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Meta Networks Connector в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в приложение Meta Networks Connector с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Meta Networks Connector.

Чтобы настроить и проверить единый вход Azure AD в Meta Networks Connector, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Meta Networks Connector](#configure-meta-networks-connector-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Meta Networks Connector](#create-meta-networks-connector-test-user)** требуется для того, чтобы в Meta Networks Connector существовал пользователь Britta Simon, связанный с представлением этого же пользователя в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Meta Networks Connector, выполните следующие действия:

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Meta Networks Connector** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Изменение базовой конфигурации SAML](common/edit-urls.png)

4. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующие действия.

    ![Снимок экрана, на котором показан раздел "Базовая конфигурация SAML", где можно ввести идентификатор, URL-адрес ответа и нажать кнопку "Сохранить"](common/idp-intiated.png)

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/saml/metadata`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/sso/saml`.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Снимок экрана, на котором показан параметр "Задать дополнительные URL-адреса" и поле для ввода URL-адреса входа](common/both-advanced-urls.png)

    а. В текстовое поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/login`.

    b. В текстовом поле **Состояние ретранслятора** введите URL-адрес в формате `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/#/`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа, которые описываются далее в этом руководстве.

6. Приложение Meta Networks Connector ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию. Нажмите кнопку **Изменить**, чтобы открыть диалоговое окно **Атрибуты пользователя**.

    ![Снимок экрана, на котором показан раздел "Атрибуты пользователя" с выбранным значком "Изменить"](common/edit-attribute.png)
    
7. В дополнение к описанному выше приложение Meta Networks Connector ожидает несколько дополнительных атрибутов в ответе SAML. В разделе **Утверждения пользователя** диалогового окна **Атрибуты пользователя** выполните следующие действия, чтобы добавить атрибут токена SAML, как показано в приведенной ниже таблице.
    
    | Имя | Атрибут источника | Пространство имен|
    | ---------------| --------------- | -------- |
    | firstname | user.givenname | |
    | lastname | user.surname | |
    | emailaddress| user.mail| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | name | user.userprincipalname| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | phone | user.telephonenumber | |

    а. Щелкните **Добавить новое утверждение**, чтобы открыть диалоговое окно **Управление утверждениями пользователя**.

    ![Снимок экрана, на котором показан раздел "Утверждения пользователя" с параметром "Добавить новое утверждение".](common/new-save-attribute.png)

    ![Снимок экрана, на котором показано диалоговое окно "Управление утверждениями пользователя", где можно ввести описанные значения](common/new-attribute-details.png)

    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. Оставьте пустым поле **Пространство имен**.

    d. В качестве источника выберите **Атрибут**.

    д) В списке **Атрибут источника** введите значение атрибута, отображаемое для этой строки.

    е) Нажмите кнопку **ОК**.

    ж. Выберите команду **Сохранить**.

8. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

9. Скопируйте требуемый URL-адрес из раздела **Настройка Meta Networks Connector**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а. URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-meta-networks-connector-single-sign-on"></a>Настройка единого входа в Meta Networks Connector

1. Откройте новую вкладку в браузере и войдите в учетную запись администратора Meta Networks Connector.
    
    > [!NOTE]
    > Meta Networks Connector является безопасной системой. Поэтому перед переходом на портал этого приложения необходимо, чтобы ваш общедоступный IP-адрес был добавлен в список разрешений на его стороне. Чтобы получить свой общедоступный IP-адрес, перейдите по ссылке, указанной [здесь](https://whatismyipaddress.com/). Отправьте свой IP-адрес [группе поддержки клиентов Meta Networks Connector](mailto:support@metanetworks.com), чтобы он был добавлен в список разрешений.
    
2. Выберите **Administrator** (Администратор) и щелкните **Settings** (Параметры).
    
    ![Снимок экрана, на котором выбран элемент "Параметры" из меню администрирования.](./media/metanetworksconnector-tutorial/configure3.png)
    
3. Убедитесь, что параметры **Log Internet Traffic** (Ведение журнала трафика Интернета) и **Force VPN MFA** (Принудительная многофакторная проверка подлинности через VPN) отключены.
    
    ![Снимок экрана демонстрирует отключение этих параметров.](./media/metanetworksconnector-tutorial/configure1.png)
    
4. Выберите **Administrator** (Администратор) и щелкните **SAML**.
    
    ![Снимок экрана демонстрирует элемент SAML, выбранный в меню администрирования.](./media/metanetworksconnector-tutorial/configure4.png)
    
5. Выполните следующие действия на странице **DETAILS** (Сведения).
    
    ![Снимок экрана демонстрирует страниц подробных сведений, где вы можете ввести описанные здесь значения.](./media/metanetworksconnector-tutorial/configure2.png)
    
    а. Скопируйте значение **SSO URL** (URL-адрес единого входа) и вставьте его в текстовое поле **URL-адрес входа** в разделе **Домены и URL-адреса приложения Meta Networks Connector**.
    
    b. Скопируйте значение **Recipient URL** (URL-адрес получателя) и вставьте его в текстовое поле **URL-адрес ответа** в разделе **Домены и URL-адреса приложения Meta Networks Connector**.
    
    c. Скопируйте значение **Audience URI (SP Entity ID)** (URI аудитории (Идентификатор сущности поставщика услуг)) и вставьте его в текстовое поле **Идентификатор (сущности)** в разделе **Домены и URL-адреса приложения Meta Networks Connector**.
    
    d. Включение SAML
    
6. На вкладке **GENERAL** (Общие) выполните следующее.

    ![Снимок экрана демонстрирует страниц общих сведений, где вы можете ввести описанные здесь значения.](./media/metanetworksconnector-tutorial/configure5.png)

    а. В поле **URL-адрес единого входа для поставщика удостоверений** вставьте значение **URL-адреса входа**, скопированное на портале Azure.

    b. В поле **Издатель поставщика удостоверений** вставьте значение **идентификатора Azure AD**, скопированное на портале Azure.

    c. Откройте в Блокноте сертификат, скачанный с портала Azure, и вставьте его в текстовое поле **Сертификат X.509**.

    d. Включите **JIT-подготовку**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon\@домен_вашей_компании.доменная_зона**.  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Meta Networks Connector.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Meta Networks Connector**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **Meta Networks Connector**.

    ![Ссылка на Meta Networks Connector в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-meta-networks-connector-test-user"></a>Создание тестового пользователя Meta Networks Connector

В этом разделе вы создадите в Meta Networks Connector пользователя Britta Simon. Приложение Meta Networks Connector поддерживает JIT-подготовку. Эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в Meta Networks Connector, он создается при попытке доступа к приложению Meta Networks Connector.

>[!Note]
>Чтобы создать пользователя вручную, обратитесь к [группе поддержки клиентов Meta Networks Connector](mailto:support@metanetworks.com).

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку "Meta Networks Connector" на панели доступа, вы автоматически войдете в приложение Meta Networks Connector, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)