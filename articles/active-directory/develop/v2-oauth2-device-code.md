---
title: Поток кода устройства OAuth 2,0 | Службы
titleSuffix: Microsoft identity platform
description: Войдите в систему пользователей без браузера. Создавайте встроенные потоки проверки подлинности и без браузера, используя предоставление авторизации устройства.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 8c757f3e067aeac5d8145ca47b2eac145daba574
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "88272456"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-device-authorization-grant-flow"></a>Платформа удостоверений Майкрософт и поток предоставления авторизации устройства OAuth 2,0

Платформа удостоверений Майкрософт поддерживает [предоставление авторизации устройства](https://tools.ietf.org/html/rfc8628), что позволяет пользователям входить на устройства с ограниченным вводом, такие как интеллектуальный телевизор, устройство IOT или принтер.  Чтобы реализовать этот поток, на устройстве пользователю предлагается открыть определенную веб-страницу в браузере на другом устройстве и выполнить вход.  После входа пользователя устройство получает маркеры доступа и маркеры обновления, если они нужны.

В этой статье описывается, как программировать непосредственно в протокол в приложении.  По возможности рекомендуется использовать поддерживаемые библиотеки проверки подлинности Майкрософт (MSAL) вместо [получения маркеров и вызова защищенных веб-API](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows).  Также ознакомьтесь с [примерами приложений, которые используют MSAL](sample-v2-code.md).

## <a name="protocol-diagram"></a>Схема протокола

Весь поток кода устройства выглядит примерно так, как на следующей схеме. Каждый из этих шагов мы опишем далее в этой статье.

![Поток кода устройства](./media/v2-oauth2-device-code/v2-oauth-device-flow.svg)

## <a name="device-authorization-request"></a>Запрос авторизации устройства

Клиент должен сначала обратиться к серверу проверки подлинности и получить код для устройства и пользователя, который используется для запуска проверки подлинности. Клиент собирает этот запрос из конечной точки `/devicecode`. В этом запросе клиент должен также указать разрешения, которые нужно получить от пользователя. С момента отправки запроса у пользователя есть только 15 минут для входа (обычное значение для `expires_in`), поэтому выполняйте этот запрос, только когда пользователь указал, что готов выполнить вход.

> [!TIP]
> Попытайтесь выполнить этот запрос в Postman.
> [![Попытайтесь выполнить этот запрос в Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```HTTP
// Line breaks are for legibility only.

POST https://login.microsoftonline.com/{tenant}/oauth2/v2.0/devicecode
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
scope=user.read%20openid%20profile

```

| Параметр | Условие | Описание |
| --- | --- | --- |
| `tenant` | Обязательно | Может быть "/Common",/консумерс или/организатионс.  Он также может быть клиентом каталога, для которого требуется запросить разрешение, в формате GUID или понятном имени.  |
| `client_id` | Обязательно | **Идентификатор приложения (клиента)** , назначенный вашему приложению функцией [Регистрация приложений портала Azure](https://go.microsoft.com/fwlink/?linkid=2083908). |
| `scope` | Обязательно | Разделенный пробелами список [областей](v2-permissions-and-consent.md) , для которого необходимо согласие пользователя.  |

### <a name="device-authorization-response"></a>Ответ на запрос авторизации устройства

Успешный ответ содержит объект JSON с информацией, требуемой пользователю для выполнения входа.

| Параметр | Формат | Описание |
| ---              | --- | --- |
|`device_code`     | Строка | Длинная строка, которая используется для подтверждения сеанса между клиентом и сервером авторизации. Клиент использует этот параметр для запроса маркера доступа от сервера авторизации. |
|`user_code`       | Строка | Короткая строка, которая отображается пользователю, для идентификации того же сеанса на другом устройстве.|
|`verification_uri`| URI | URI, по которому пользователь должен перейти с помощью `user_code` для входа. |
|`expires_in`      | INT | Количество секунд до окончания срока действия `device_code` и `user_code`. |
|`interval`        | INT | Период между опросами (в секундах), которые выполняются клиентом. |
| `message`        | Строка | Понятная человеку строка с инструкциями для пользователя. Ее можно локализовать, включив **параметр запроса** в запросе формы `?mkt=xx-XX`, заменив соответствующий код языка и региональных параметров. |

> [!NOTE]
> `verification_uri_complete`Поле ответа в настоящее время не включено или не поддерживается.  Мы упомянули это, поскольку если вы прочитаете [стандартный стандарт](https://tools.ietf.org/html/rfc8628) , который `verification_uri_complete` указан как необязательная часть стандарта потока кода устройства.

## <a name="authenticating-the-user"></a>Проверка подлинности пользователя

После получения `user_code` и `verification_uri` Клиент отображает их пользователю, указывая, что вход выполняется с помощью мобильного телефона или браузера ПК.

Если пользователь проходит проверку подлинности с помощью личной учетной записи (в "/Common" или/консумерс), ему будет предложено войти еще раз, чтобы перевести состояние проверки подлинности на устройство.  Они также получат запрос на предоставление согласия, чтобы убедиться, что они знают о предоставляемых разрешениях.  Это не относится к рабочим или учебным учетным записям, используемым для проверки подлинности.

Хотя пользователь проходит проверку подлинности в `verification_uri`, клиент должен опросить конечную точку `/token` на наличие запрошенного токена с помощью `device_code`.

```HTTP
POST https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

grant_type: urn:ietf:params:oauth:grant-type:device_code
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8...
```

| Параметр | Обязательно | Описание|
| -------- | -------- | ---------- |
| `tenant`  | Обязательно | Один и тот же псевдоним клиента или клиента, используемый в первоначальном запросе. |
| `grant_type` | Обязательно | Должен содержать значение `urn:ietf:params:oauth:grant-type:device_code`.|
| `client_id`  | Обязательно | Должен соответствовать `client_id`, используемому в первоначальном запросе. |
| `device_code`| Обязательно | `device_code`, возвращаемый в запросе авторизации устройства.  |

### <a name="expected-errors"></a>Ожидаемые ошибки

Поток кода устройства — это протокол опроса, поэтому клиент должен ждать получения ошибок до того, как пользователь завершит проверку подлинности.

| Ошибка | Описание | Действие клиента |
| ------ | ----------- | -------------|
| `authorization_pending` | Пользователь не завершил проверку подлинности, но не отменил поток. | Повторите запрос не менее чем через `interval` с. |
| `authorization_declined` | Пользователь отклонил запрос авторизации.| Остановка опроса и возврат в состояние без проверки подлинности.  |
| `bad_verification_code`| Объект, `device_code` Отправленный в `/token` конечную точку, не распознан. | Убедитесь, что клиент отправляет правильный `device_code` в запросе. |
| `expired_token` | Прошло не менее `expires_in` секунд, и проверка подлинности больше невозможна с помощью этого `device_code`. | Прерывать опрос и возвращаться к состоянию без проверки подлинности. |

### <a name="successful-authentication-response"></a>Ответ об успешном прохождении проверки подлинности

Успешный ответ на запрос маркера будет выглядеть следующим образом:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Параметр | Формат | Описание |
| --------- | ------ | ----------- |
| `token_type` | Строка| Всегда имеет значение Bearer. |
| `scope` | Строки, разделенные пробелами | Если маркер успешно возвращается, здесь будет содержаться список областей, для которых этот маркер доступа является допустимым. |
| `expires_in`| INT | Срок действия прилагаемого маркера доступа (в секундах). |
| `access_token`| Непрозрачная строка | Выдается для запрошенных [областей](v2-permissions-and-consent.md).  |
| `id_token`   | JWT | Выдается, если исходный параметр `scope` содержит область `openid`.  |
| `refresh_token` | Непрозрачная строка | Выдается, если исходный параметр `scope` содержит `offline_access`.  |

Маркер обновления можно использовать для получения новых маркеров доступа и обновления маркеров с помощью того же потока, который описан в [документации по потоку кода OAuth](v2-oauth2-auth-code-flow.md#refresh-the-access-token).
