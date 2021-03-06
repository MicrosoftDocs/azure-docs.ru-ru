---
title: Внедрение Azure Active Directory B2C пользовательского интерфейса в приложение с помощью настраиваемой политики
titleSuffix: Azure AD B2C
description: Сведения о внедрении пользовательского интерфейса Azure Active Directory B2C в приложение с помощью настраиваемой политики
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/21/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: ccad323c1834894367cca0ef0d3f98eb1b1b1ec3
ms.sourcegitcommit: c8b50a8aa8d9596ee3d4f3905bde94c984fc8aa2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2021
ms.locfileid: "105639910"
---
# <a name="embedded-sign-in-experience"></a>Встроенные возможности входа в систему

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-user-flow"

[!INCLUDE [active-directory-b2c-limited-to-custom-policy](../../includes/active-directory-b2c-limited-to-custom-policy.md)]

::: zone-end

::: zone pivot="b2c-custom-policy"

Для упрощения процесса входа можно избежать перенаправления пользователей на отдельную страницу входа или создание всплывающего окна. С помощью встроенного элемента Frame `<iframe>` можно внедрить пользовательский интерфейс входа Azure AD B2C непосредственно в веб-приложение.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="web-application-embedded-sign-in"></a>Встроенный вход в веб-приложение

Элемент встроенного фрейма `<iframe>` используется для внедрения документа на веб-странице HTML5. С помощью элемента IFRAME можно внедрить пользовательский интерфейс входа Azure AD B2C непосредственно в веб-приложение, как показано в следующем примере:

![Вход с наведением в DIV](media/embedded-login/login-hovering.png)

При использовании iframe учитывайте следующее.

- Встроенный вход поддерживает только локальные учетные записи. Большинство поставщиков удостоверений социальных сетей (например, Google и Facebook) блокируют страницы входа из подставляемых кадров.
- Так как файлы cookie сеанса Azure AD B2C в IFRAME считаются сторонними файлами cookie, некоторые браузеры (например, Safari или Chrome в режиме режиме инкогнито) блокируют или удаляют эти файлы cookie, что приводит к нежелательным последствиям для пользователя. Чтобы избежать этой проблемы, убедитесь, что имя домена приложения и домен Azure AD B2C имеют *один и тот же источник*. Чтобы использовать тот же источник, [включите личные домены](custom-domain.md) для Azure AD B2C клиента, а затем настройте веб-приложение с тем же источником. Например, приложение, размещенное на " https://app.contoso.com ", имеет тот же источник, что и Azure AD B2C, выполняющийся на " https://login.contoso.com ".

## <a name="prerequisites"></a>Предварительные условия

* Выполните шаги, описанные в статье [Начало работы с настраиваемыми политиками в Azure Active Directory B2C](custom-policy-get-started.md).
* [Включите личные домены](custom-domain.md) для политик.

## <a name="configure-your-policy"></a>Настройка политики

Чтобы разрешить внедрение пользовательского интерфейса Azure AD B2C в iframe, в `Content-Security-Policy` `X-Frame-Options` Azure AD B2C заголовки HTTP-ответа необходимо включить параметры политики безопасности содержимого и фрейма. Эти заголовки позволяют пользовательскому интерфейсу Azure AD B2C выполняться под именем домена приложения.

Добавьте элемент **жаурнэйфраминг** внутри элемента [релингпарти](relyingparty.md) .  Элемент **усержаурнэйбехавиорс** должен следовать за **дефаултусержаурнэй**. Элемент **усержаурнэйбехавиорс** должен выглядеть, как в следующем примере:

```xml
<!--
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" /> -->
  <UserJourneyBehaviors> 
    <JourneyFraming Enabled="true" Sources="https://somesite.com https://anothersite.com" /> 
  </UserJourneyBehaviors>
<!--
</RelyingParty> -->
```

Атрибут **Sources** содержит универсальный код ресурса (URI) веб-приложения. Добавьте пробел между URI. Каждый URI должен соответствовать следующим требованиям:

- КОД URI должен быть доверенным и принадлежащим вашему приложению.
- URI должен использовать схему HTTPS.  
- Необходимо указать полный URI веб-приложения. Подстановочные знаки не поддерживаются.

Кроме того, рекомендуется также блокировать внедрение собственного доменного имени в iframe, настроив заголовки содержимого-Security-Policy и X-Frame-Options соответственно на страницах приложения. Это позволит устранить проблемы безопасности, связанные со старыми браузерами, связанными с внедрением IFRAME.

## <a name="adjust-policy-user-interface"></a>Настройка пользовательского интерфейса политики

Благодаря Azure AD B2C [настройке пользовательского интерфейса](customize-ui.md)вы почти полностью контролируете содержимое HTML и CSS, представляемое пользователям. Выполните действия по настройке HTML-страницы с помощью определений содержимого. Чтобы поместить Azure AD B2C пользовательский интерфейс в размер IFRAME, предоставьте пустую HTML-страницу без фоновых и дополнительных пробелов.  

Следующий код CSS скрывает Azure AD B2C HTML-элементы и корректирует размер панели для заполнения IFRAME.

```css
div.social, div.divider {
    display: none;
}

div.api_container{
    padding-top:0;
}

.panel {
    width: 100%!important
}
```

В некоторых случаях может потребоваться уведомить приложение, в котором в настоящее время отображается Azure AD B2C страница. Например, когда пользователь выбирает вариант регистрации, приложение может ответить, скрыв ссылки для входа с помощью учетной записи социальных сетей или изменив размер IFRAME.

Чтобы уведомить приложение о текущей Azure AD B2C странице, [Включите политику для JavaScript](./javascript-and-page-layout.md), а затем используйте сообщения в HTML5 POST. Следующий код JavaScript отправляет сообщение POST в приложение с помощью `signUp` :

```javascript
window.parent.postMessage("signUp", '*');
```

## <a name="configure-a-web-application"></a>Настройка веб-приложения

Когда пользователь нажимает кнопку входа, [веб-приложение](code-samples.md#web-apps-and-apis) создает запрос на авторизацию, который выполняет Azure AD B2C входа в систему. После завершения входа Azure AD B2C возвращает маркер идентификатора или код авторизации в настроенный URI перенаправления в приложении.

Для поддержки встроенного имени входа свойство IFRAME **src** указывает на контроллер входа, например `/account/SignUpSignIn` , который создает запрос авторизации и перенаправляет пользователя к Azure AD B2C политике.

```html
<iframe id="loginframe" frameborder="0" src="/account/SignUpSignIn"></iframe>
``` 

После получения и проверки маркера идентификации приложением поток авторизации завершается, и приложение распознает пользователя и доверяет ему. Так как поток авторизации происходит внутри iframe, необходимо перезагрузить главную страницу. После перезагрузки страницы кнопка входа изменится на "выйти", а имя пользователя появится в пользовательском интерфейсе.  

Ниже приведен пример, демонстрирующий, как URI перенаправления входа может обновлять главную страницу:

```javascript
window.top.location.reload();
```

### <a name="add-sign-in-with-social-accounts-to-a-web-app"></a>Добавление единого входа с учетными записями социальных сетей в веб-приложение

Поставщики удостоверений в социальных сетях блокируют свои страницы входа из подготовки к просмотру во встроенных кадрах. Можно использовать отдельную политику для учетных записей социальных сетей или единую политику для входа и регистрации с использованием локальных и социальных учетных записей. Затем можно использовать `domain_hint` параметр строки запроса. Параметр указания домена переводит пользователя непосредственно на страницу входа поставщика удостоверений социальных сетей.

В приложении добавьте кнопки Вход с помощью социальных учетных записей. Когда пользователь нажимает одну из кнопок учетной записи социальных сетей, элемент управления должен изменить имя политики или задать параметр указания домена.

<!-- TBD: add a diagram -->

URI перенаправления может быть тем же URI перенаправления, который используется в IFRAME. Можно пропустить перезагрузку страницы.

## <a name="configure-a-single-page-application"></a>Настройка одностраничного приложения

Для одностраничного приложения также потребуется вторая HTML-страница входа, которая загружается в IFRAME. На этой странице входа размещается код библиотеки проверки подлинности, который создает код авторизации и возвращает маркер.

Когда одностраничному приложению требуется маркер доступа, используйте код JavaScript для получения маркера доступа из IFRAME и объекта, который его содержит.

> [!NOTE]
> В настоящее время запуск MSAL 2,0 в iframe не поддерживается.

Следующий код является примером, который выполняется на главной странице и вызывает код JavaScript IFRAME:

```javascript
function getToken()
{
  var token = document.getElementById("loginframe").contentWindow.getToken("adB2CSignInSignUp");

  if (token === "LoginIsRequired")
    document.getElementById("tokenTextarea").value = "Please login!!!"
  else
    document.getElementById("tokenTextarea").value = token.access_token;
}

function logOut()
{
  document.getElementById("loginframe").contentWindow.policyLogout("adB2CSignInSignUp", "B2C_1A_SignUpOrSignIn");
}
```

## <a name="next-steps"></a>Дальнейшие шаги

См. следующие статьи:

- [Настройка пользовательского интерфейса](customize-ui.md)
- Справочник по элементам [релингпарти](relyingparty.md)
- [Включение политики для JavaScript](./javascript-and-page-layout.md)
- [Примеры кода](code-samples.md)

::: zone-end
