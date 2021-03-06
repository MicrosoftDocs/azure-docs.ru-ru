---
title: Поддержка единого входа и политик защиты приложений в мобильных приложениях, которые вы разрабатываете | Службы
titleSuffix: Microsoft identity platform
description: Описание и общие сведения о создании мобильных приложений, поддерживающих единый вход и политики защиты приложений с помощью платформы удостоверений Майкрософт и интеграции с Azure Active Directory.
services: active-directory
author: knicholasa
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/14/2020
ms.author: nichola
ms.openlocfilehash: 4f0588667df6acb11a43e8c3469c67f65ed3cdd9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98165184"
---
# <a name="support-single-sign-on-and-app-protection-policies-in-mobile-apps-you-develop"></a>Поддержка единого входа и политик защиты приложений в мобильных приложениях, которые вы разрабатываете

Единый вход (SSO) — это ключевое предложение платформы Microsoft Identity и Azure Active Directory, обеспечивающее простые и безопасные имена входа для пользователей приложения. Кроме того, политики защиты приложений (APP) обеспечивают поддержку политик безопасности ключей, обеспечивающих безопасность данных пользователя. Вместе эти функции обеспечивают безопасный вход пользователей в систему и управление данными приложения.

> [!VIDEO https://www.youtube.com/embed/JpeMeTjQJ04]

В этой статье объясняется, зачем важны единый вход и приложение, а также общее руководство по созданию мобильных приложений, поддерживающих эти функции. Это применимо как для телефонов, так и для планшетных приложений. Если вы ИТ-администратор, желающий развернуть единый вход в клиенте Azure Active Directory организации, ознакомьтесь [с нашим руководством по планированию развертывания единого входа](../manage-apps/plan-sso-deployment.md) .

## <a name="about-single-sign-on-and-app-protection-policies"></a>Об использовании единого входа и политик защиты приложений

[Единый вход (SSO)](../manage-apps/plan-sso-deployment.md) позволяет пользователю войти в систему и получить доступ к другим приложениям без повторного ввода учетных данных. Это упрощает доступ к приложениям и устраняет необходимость в запоминании длинных списков имен пользователей и паролей. Его реализация в приложении упрощает доступ к приложению и его использование.

Кроме того, включение единого входа в приложении разблокирует новые механизмы проверки подлинности, которые входят в современную проверку подлинности, например [имена входа без пароля](../authentication/concept-authentication-passwordless.md). Имена пользователей и пароли являются одним из самых популярных направлений атак для приложений, и включение единого входа позволяет снизить риск, применяя условный доступ или учетные записи, не имеющие пароля, которые обеспечивают дополнительную безопасность или используют более безопасные механизмы проверки подлинности. Наконец, включение единого входа также обеспечивает [единый](v2-protocols-oidc.md#single-sign-out)выход. Это полезно в таких ситуациях, как рабочие приложения, которые будут использоваться на общих устройствах.

[Политики защиты приложений (App)](/mem/intune/apps/app-protection-policy) гарантируют, что данные организации останутся надежными и автономными. Они позволяют компаниям управлять своими данными и защищать их в приложении, а также управлять доступом пользователей к приложению и его данным. Реализация политик защиты приложений позволяет приложению подключать пользователей к ресурсам, защищенным политиками условного доступа, и безопасно передавать данные в другие защищенные приложения и обратно. В сценариях, разблокируемых политиками защиты приложений, необходимо указать ПИН-код для открытия приложения, управления обменом данными между приложениями и предотвращения сохранения данных приложений компании в личных хранилищах.

## <a name="implementing-single-sign-on"></a>Реализация единого входа

Мы рекомендуем использовать следующие преимущества единого входа в приложение.

### <a name="use-the-microsoft-authentication-library-msal"></a>Использование библиотеки проверки подлинности Майкрософт (MSAL)

Лучшим выбором для реализации единого входа в приложении является использование [библиотеки проверки подлинности Майкрософт (MSAL)](msal-overview.md). С помощью MSAL вы можете добавить в приложение проверку подлинности с помощью минимального кода и вызовов API, получить все функции [платформы Microsoft Identity](./index.yml), а также разрешить корпорации Майкрософт выполнять обслуживание решения безопасной проверки подлинности. По умолчанию MSAL добавляет поддержку единого входа для приложения. Кроме того, использование MSAL является обязательным, если вы планируете реализовать политики защиты приложений.

> [!NOTE]
> Можно настроить MSAL для использования встроенного веб-представления. Это предотвратит единый вход. Используйте поведение по умолчанию (то есть системный веб-браузер), чтобы убедиться, что единый вход будет работать.

Если вы используете [библиотеку ADAL](../azuread-dev/active-directory-authentication-libraries.md) в своем приложении, настоятельно рекомендуется [выполнить миграцию в MSAL](msal-migration.md), так как [ADAL является устаревшим](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/update-your-applications-to-use-microsoft-authentication-library/ba-p/1257363).

Для приложений iOS имеется [Краткое](quickstart-v2-ios.md) руководство, в котором показано, как настроить входы с помощью MSAL, а также [инструкции по настройке MSAL для различных сценариев единого входа](single-sign-on-macos-ios.md).

Для приложений Android у нас есть [Краткое](quickstart-v2-android.md) руководство, в котором показано, как настроить входы с помощью MSAL, а также инструкции по [включению единого входа между приложениями в Android с помощью MSAL](msal-android-single-sign-on.md).

### <a name="use-the-system-web-browser"></a>Использование системного веб-браузера

Для интерактивной проверки подлинности требуется веб-браузер. Для мобильных приложений, использующих современные библиотеки проверки подлинности, отличные от MSAL (то есть другие библиотеки OpenID Connect Connect или SAML), или если вы реализуете собственный код проверки подлинности, для включения единого входа следует использовать системный браузер в качестве поверхности проверки подлинности.

В Google есть рекомендации по выполнению этой задачи в приложениях Android: [пользовательские вкладки Chrome — Google Chrome](https://developer.chrome.com/multidevice/android/customtabs).

Компания Apple представила рекомендации по выполнению этой задачи в приложениях iOS: [Проверка подлинности пользователя с помощью веб-службы | Документация для разработчиков Apple](https://developer.apple.com/documentation/authenticationservices/authenticating_a_user_through_a_web_service).

> [!TIP]
> [Подключаемый модуль единого входа для устройств Apple](apple-sso-plugin.md) позволяет использовать единый вход для приложений iOS, использующих внедренные веб-представления на управляемых устройствах с помощью Intune. Мы рекомендуем MSAL и системный браузер как лучший вариант для разработки приложений, позволяющих использовать единый вход для всех пользователей, но в некоторых сценариях это позволит использовать единый вход в некоторых ситуациях, в противном случае это невозможно.

## <a name="enable-app-protection-policies"></a>Включить политики защиты приложений

Чтобы включить политики защиты приложений, используйте [библиотеку проверки подлинности Майкрософт (MSAL)](msal-overview.md). MSAL — это библиотека проверки подлинности и авторизации платформы Microsoft Identity, а пакет Intune SDK разрабатывается для совместной работы с ним.

Кроме того, для проверки подлинности необходимо использовать приложение брокера. Брокер требует, чтобы приложение предпредоставил сведения о приложении и устройстве, чтобы обеспечить соответствие приложений. Пользователи iOS будут использовать приложение [Microsoft Authenticator](../user-help/user-help-auth-app-sign-in.md) и Android будут использовать приложение Microsoft Authenticator или [приложение корпоративный портал](https://play.google.com/store/apps/details?id=com.microsoft.windowsintune.companyportal) для [проверки подлинности через посредника](./msal-android-single-sign-on.md). По умолчанию MSAL использует брокер в качестве первого выбора для выполнения запроса проверки подлинности, поэтому использование брокера для проверки подлинности будет включено для приложения автоматически при использовании MSAL.

Наконец, [Добавьте пакет SDK Intune](/mem/intune/developer/app-sdk-get-started) в приложение, чтобы включить политики защиты приложений. Пакет SDK для большей части следует модели перехвата и автоматически применит политики защиты приложений, чтобы определить, разрешены ли действия, выполняемые приложением. Кроме того, есть интерфейсы API, которые можно вызывать вручную, чтобы сообщить приложению об ограничениях на определенные действия.

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Планирование развертывания Azure Active Directory единого входа](../manage-apps/plan-sso-deployment.md)
- [Как настроить единый вход для macOS и iOS](single-sign-on-macos-ios.md)
- [Подключаемый модуль единого входа Microsoft Enterprise для устройств Apple (Предварительная версия)](apple-sso-plugin.md)
- [Проверка подлинности через посредника в Android](./msal-android-single-sign-on.md)
- [Агенты авторизации и их включение](./msal-android-single-sign-on.md)
- [Начало работы с SDK для приложений Microsoft Intune](/mem/intune/developer/app-sdk-get-started)
- [Настройка параметров пакета SDK для приложений Intune](/mem/intune/developer/app-sdk-ios#configure-settings-for-the-intune-app-sdk)
- [Защищенные приложения Microsoft Intune](/mem/intune/apps/apps-supported-intune-apps)
