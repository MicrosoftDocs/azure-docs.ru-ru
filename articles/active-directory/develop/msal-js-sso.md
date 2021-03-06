---
title: Единый вход (MSAL.js) | Службы
titleSuffix: Microsoft identity platform
description: Узнайте о создании единого входа с помощью библиотеки проверки подлинности Майкрософт для JavaScript (MSAL.js).
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 8080d4cf4c3f0091f7837b3fccead5474c42db55
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "84690784"
---
# <a name="single-sign-on-with-msaljs"></a>Единый вход с использованием MSAL.js

Единый Sign-On (SSO) позволяет пользователям вводить свои учетные данные один раз для входа и устанавливать сеанс, который можно повторно использовать в нескольких приложениях без необходимости повторной проверки подлинности. Это обеспечивает удобство работы пользователя и сокращает повторяющиеся запросы учетных данных.

Azure AD предоставляет приложениям возможность единого входа, устанавливая файл cookie сеанса при первой проверке подлинности пользователя. Библиотека MSAL.js позволяет приложениям использовать эти возможности несколькими способами.

## <a name="sso-between-browser-tabs"></a>Единый вход между вкладками браузера

Если приложение открыто на нескольких вкладках и сначала вы входите на одну вкладку, пользователь также вошел на другие вкладки без запроса. MSAL.js кэширует маркер идентификатора для пользователя в браузере `localStorage` и будет выполнять вход пользователя в приложение на других открытых вкладках.

По умолчанию MSAL.js использует, `sessionStorage` что не позволяет совместно использовать сеансы вкладок. Чтобы обеспечить единый вход между вкладками, установите `cacheLocation` в MSAL.js значение, `localStorage` как показано ниже.

```javascript
const config = {
    auth: {
        clientId: "abcd-ef12-gh34-ikkl-ashdjhlhsdg"
    },
    cache: {
        cacheLocation: 'localStorage'
    }
}

const myMSALObj = new UserAgentApplication(config);
```

## <a name="sso-between-apps"></a>Единый вход между приложениями

Когда пользователь проходит проверку подлинности, в домене Azure AD в браузере задается файл cookie сеанса. MSAL.js полагается на этот файл cookie сеанса для предоставления единого входа для пользователя между различными приложениями. MSAL.js также кэширует маркеры ИДЕНТИФИКАТОРов и маркеры доступа пользователя в хранилище браузера на домен приложения. В результате поведение единого входа различается в различных случаях:  

### <a name="applications-on-the-same-domain"></a>Приложения в одном домене

Когда приложения размещаются в одном домене, пользователь может войти в приложение один раз, а затем проверить подлинность других приложений без запроса. MSAL.js использует маркеры, кэшированные для пользователя в домене, для предоставления единого входа.

### <a name="applications-on-different-domain"></a>Приложения в другом домене

Если приложения размещаются в разных доменах, доступ к маркерам, кэшированным в домене а, невозможен MSAL.js в домене B.

Это означает, что когда пользователи, выполнившие вход в домен а, переходят к приложению в домене B, они будут перенаправлены или появится запрос на страницу Azure AD. Так как Azure AD по-прежнему имеет файл cookie сеанса пользователя, он будет входить в систему пользователя, и ему не потребуется повторно вводить учетные данные. Если в сеансе с Azure AD у пользователя несколько учетных записей пользователей, пользователю будет предложено выбрать соответствующую учетную запись для входа.

### <a name="automatically-select-account-on-azure-ad"></a>Автоматически выбирать учетную запись в Azure AD

В некоторых случаях приложение имеет доступ к контексту проверки подлинности пользователя и хочет избежать запроса на выбор учетной записи Azure AD при входе в несколько учетных записей.  Это можно сделать несколькими способами:

**Использование идентификатора сеанса (SID)**

Идентификатор сеанса — это [необязательное утверждение](active-directory-optional-claims.md) , которое можно настроить в маркерах идентификации. Это утверждение позволяет приложению обнаруживать сеанс пользователя Azure AD независимо от имени учетной записи пользователя или пользователя. Идентификатор SID можно передать в параметрах запроса в `acquireTokenSilent` вызов. Это позволит Azure AD обходить выбор учетной записи. Идентификатор безопасности привязан к файлу cookie сеанса и не будет пересекаться с контекстами браузера.

```javascript
var request = {
    scopes: ["user.read"],
    sid: sid
}

userAgentApplication.acquireTokenSilent(request).then(function(response) {
        const token = response.accessToken
    }
).catch(function (error) {  
        //handle error
});
```

> [!Note]
> Идентификатор безопасности может использоваться только с необъявляемыми запросами проверки подлинности, выполняемыми `acquireTokenSilent` вызовом в MSAL.js.
Инструкции по настройке необязательных утверждений в манифесте приложения можно найти [здесь](active-directory-optional-claims.md).

**Использование подсказки для входа**

Если вы не настроили заявку SID или не хотите обойти запрос выбора учетной записи в интерактивных вызовах проверки подлинности, это можно сделать, предоставив `login_hint` в параметрах запроса, а при необходимости — `domain_hint` как `extraQueryParameters` в MSAL.js интерактивных методах ( `loginPopup` , `loginRedirect` `acquireTokenPopup` и `acquireTokenRedirect` ). Пример:

```javascript
var request = {
    scopes: ["user.read"],
    loginHint: preferred_username,
    extraQueryParameters: {domain_hint: 'organizations'}
}

userAgentApplication.loginRedirect(request);
```

Значения для login_hint и domain_hint можно получить, прочитав утверждения, возвращенные в маркере идентификации для пользователя.

* для **логинхинт** должно быть задано `preferred_username` утверждение в маркере идентификации.

* **domain_hint** требуется передавать только при использовании центра "/Common". Указание домена определяется по ИДЕНТИФИКАТОРу клиента (TID).  Если `tid` утверждение в маркере идентификации является `9188040d-6c67-4c5b-b112-36a304b66dad` потребителей. В противном случае это Организации.

Дополнительные сведения о значениях для указания имени входа и указании домена см. [здесь](v2-oauth2-implicit-grant-flow.md) .

> [!Note]
> Нельзя передавать идентификаторы безопасности и login_hint в одно и то же время. Это приведет к ошибочному ответу.

## <a name="sso-without-msaljs-login"></a>ЕДИНЫЙ вход без MSAL.js входа

MSAL.js требует вызова метода Login для определения контекста пользователя перед получением маркеров для API-интерфейсов. Так как методы входа являются интерактивными, пользователь видит запрос.

В некоторых случаях приложения имеют доступ к контексту пользователя или маркеру идентификации, прошедшему проверку подлинности, который был инициирован в другом приложении, и вы хотите использовать единый вход для получения маркеров без предварительного входа с помощью MSAL.js.

Пример: пользователь вошел в родительское веб-приложение, в котором размещается другое приложение JavaScript, выполняющееся как надстройка или подключаемый модуль.

Возможности единого входа в этом сценарии можно получить следующим образом.

Передайте объект, `sid` если он доступен (или `login_hint` , при необходимости), в `domain_hint` качестве параметров запроса к MSAL.js `acquireTokenSilent` вызову следующим образом:

```javascript
var request = {
    scopes: ["user.read"],
    loginHint: preferred_username,
    extraQueryParameters: {domain_hint: 'organizations'}
}

userAgentApplication.acquireTokenSilent(request).then(function(response) {
        const token = response.accessToken
    }
).catch(function (error) {  
        //handle error
});
```

## <a name="sso-in-adaljs-to-msaljs-update"></a>Единый вход в ADAL.js для MSAL.js обновления

MSAL.js применяют функции контроля использования ADAL.js для сценариев проверки подлинности Azure AD. Чтобы сделать миграцию с ADAL.js на MSAL.js простоту и избежать повторного входа пользователей в систему, Библиотека считывает маркер идентификации, представляющий сеанс пользователя в кэше ADAL.js, и без проблем подписывает пользователя в MSAL.js.  

Чтобы воспользоваться преимуществами единого входа (SSO) при обновлении с ADAL.js, необходимо убедиться, что библиотеки используются `localStorage` для кэширования маркеров. Присвойте параметру значение `cacheLocation` `localStorage` в конфигурации MSAL.js и ADAL.js при инициализации следующим образом:


```javascript
// In ADAL.js
window.config = {
   clientId: 'g075edef-0efa-453b-997b-de1337c29185',
   cacheLocation: 'localStorage'
};

var authContext = new AuthenticationContext(config);


// In latest MSAL.js version
const config = {
    auth: {
        clientId: "abcd-ef12-gh34-ikkl-ashdjhlhsdg"
    },
    cache: {
        cacheLocation: 'localStorage'
    }
}

const myMSALObj = new UserAgentApplication(config);
```

После настройки MSAL.js сможет считывать кэшированное состояние пользователя, прошедшего проверку подлинности, в ADAL.js и использовать его для предоставления единого входа в MSAL.js.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [сеансе единого входа и значениях времени существования маркеров](active-directory-configurable-token-lifetimes.md) в Azure AD.
