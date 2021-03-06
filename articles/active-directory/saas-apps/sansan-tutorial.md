---
title: Руководство по Интеграция Azure Active Directory с Sansan | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Sansan.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/27/2020
ms.author: jeedes
ms.openlocfilehash: 6c35ce5e4b420b7f203b33b640a3175ba4c2739b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "96181517"
---
# <a name="tutorial-integrate-sansan-with-azure-active-directory"></a>Руководство по Интеграция Sansan с Azure Active Directory

В этом руководстве описано, как интегрировать Sansan с Azure Active Directory (Azure AD). Интеграция Sansan с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD вы можете контролировать доступ к Sansan.
* Вы можете включить автоматический вход пользователей в Sansan с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* подписка Sansan с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.
* Sansan поддерживает единый вход, инициируемый **поставщиком услуг**.
* После настройки Sansan вы можете применить функцию управления сеансом, которая в режиме реального времени защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-sansan-from-the-gallery"></a>Добавление Sansan из коллекции

Чтобы настроить интеграцию Sansan с Azure AD, необходимо добавить Sansan из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Sansan**.
1. Выберите **Sansan** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-sso"></a>Настройка и проверка единого входа Azure AD

Настройте и проверьте единый вход Azure AD в Sansan с использованием тестового пользователя **Britta Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Sansan.

Чтобы настроить и проверить единый вход Azure AD в Sansan, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
   * **[Создайте тестового пользователя Azure AD](#create-an-azure-ad-test-user)** для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
   * **[Назначьте тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** , чтобы позволить Britta Simon использовать единый вход в Azure AD.
1. **[Настройка Sansan](#configure-sansan)** необходима, чтобы настроить параметры единого входа на стороне приложения.
   * **[Создание тестового пользователя Sansan](#create-sansan-test-user)** требуется для того, чтобы в Sansan существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** необходима, чтобы убедиться в корректной работе конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Sansan** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения для следующих полей.

    1. В текстовом поле **URL-адрес входа** введите URL-адрес: `https://ap.sansan.com/`.

   1. В текстовом поле **Идентификатор (сущности)** введите URL-адрес: .  
   `https://ap.sansan.com/saml2/<company name>`

   1. В текстовом поле **URL-адрес ответа** введите URL-адрес в одном из таких форматов:

    
       | Среда | URL-адрес |
      |:--- |:--- |
      | PC |`https://ap.sansan.com/v/saml2/<company name>/acs` |
      | Приложение для смартфонов |`https://internal.api.sansan.com/<company name>/acs` |
      | Веб-версия для смартфонов |`https://ap.sansan.com/s/saml2/<company name>/acs` |

    > [!NOTE]
    > Эти значения приведены для примера. Проверьте фактические значения в **параметрах администрирования Sansan**.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

   ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка Sansan**.

   ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `Britta Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `BrittaSimon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Sansan.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **Sansan**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **Britta Simon** из списка пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-sansan"></a>Настройка Sansan

Чтобы настроить **параметры единого входа** на стороне **Sansan**, выполните указанные ниже действия в соответствии со своими требованиями.

   * Версия на [японском языке](https://jp-help.sansan.com/hc/ja/articles/900001551383 ).

   * Версия на [английском языке](https://jp-help.sansan.com/hc/en-us/articles/900001551383 ).


### <a name="create-sansan-test-user"></a>Создание тестового пользователя Sansan

В этом разделе объясняется, как создать пользователя Britta Simon в приложении Sansan. Дополнительные сведения о создании пользователя, см. в [этих инструкциях](https://jp-help.sansan.com/hc/en-us/articles/206508997-Adding-users).

## <a name="test-sso"></a>Проверка единого входа

Щелкнув элемент "Sansan" на Панели доступа, вы автоматически войдете в приложение Sansan, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)