---
title: Руководство по интеграции единого входа Azure Active Directory c BullseyeTDP | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и BullseyeTDP.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/16/2020
ms.author: jeedes
ms.openlocfilehash: 0003804ac45debf46a732aa5d46ab643eb1f0335
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "92456625"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-bullseyetdp"></a>Руководство по интеграции единого входа Azure Active Directory c BullseyeTDP

В этом учебнике описано, как интегрировать BullseyeTDP с Azure Active Directory (Azure AD). Интеграция BullseyeTDP с Azure AD обеспечивает следующие возможности.

* Контроль доступа к BullseyeTDP с помощью Azure AD.
* Включение автоматического входа пользователей в BullseyeTDP с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка BullseyeTDP с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* BullseyeTDP поддерживает единый вход, инициированный **поставщиком удостоверений**.

* После настройки BullseyeTDP вы можете применить функцию управления сеансом, которая в реальном времени защищает от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-bullseyetdp-from-the-gallery"></a>Добавление BullseyeTDP из коллекции

Чтобы настроить интеграцию BullseyeTDP с Azure AD, необходимо добавить BullseyeTDP из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **BullseyeTDP**.
1. Выберите **BullseyeTDP** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-single-sign-on-for-bullseyetdp"></a>Настройка и проверка единого входа Azure AD для BullseyeTDP

Настройте и проверьте единый вход Azure AD в BullseyeTDP с использованием данных тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем AAD и соответствующим пользователем в BullseyeTDP.

Чтобы настроить и проверить единый вход Azure AD в BullseyeTDP, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в BullseyeTDP](#configure-bullseyetdp-sso)** необходима, чтобы задать параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя BullseyeTDP](#create-bullseyetdp-test-user)** требуется для того, чтобы в BullseyeTDP существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **BullseyeTDP** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. В разделе **Базовая конфигурация SAML** приложение предварительно настроено и ему заданы требуемые URL-адреса. Пользователь должен сохранить конфигурацию, нажав кнопку **Сохранить**.


1. Приложение BullseyeTDP ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

1. В дополнение к описанному выше приложение BullseyeTDP ожидает несколько дополнительных атрибутов в ответе SAML, как показано ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.
    
    | Имя | Исходный атрибут|
    | ---- | --------------- |
    | AccessToken | < AccessTokenValue > |
    | ApplicationKey | < ApplicationKeyValue >|
    | EmployeeID | user.employeeid |

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка BullseyeTDP**.

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

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к BullseyeTDP.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **BullseyeTDP**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-bullseyetdp-sso"></a>Настройка единого входа в BullseyeTDP

Чтобы настроить единый вход на стороне **BullseyeTDP**, нужно отправить скачанный **сертификат (Base64)** и соответствующие URL-адреса, скопированные на портале Azure, в [группу поддержки BullseyeTDP](mailto:hello@bullseyetdp.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-bullseyetdp-test-user"></a>Создание тестового пользователя в BullseyeTDP

В этом разделе описано, как создать пользователя Britta Simon в приложении BullseyeTDP. Чтобы добавить пользователей на платформу BullseyeTDP, обратитесь в [службу поддержки BullseyeTDP](mailto:hello@bullseyetdp.com). Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку BullseyeTDP на Панели доступа, вы автоматически войдете в приложение BullseyeTDP, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)

- [Попробуйте использовать BullseyeTDP с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Защита приложений с помощью функции управления настройками условного доступа для приложений в Microsoft Cloud App Security](/cloud-app-security/proxy-intro-aad)