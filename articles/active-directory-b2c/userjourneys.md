---
title: Элемент UserJourneys | Документация Майкрософт
description: Сведения об указании элемента UserJourneys настраиваемой политики в Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/04/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 05307fe2ad9e0a59fa11c30f2dc7154ba5076603
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102174671"
---
# <a name="userjourneys"></a>UserJourneys

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

В путях взаимодействия пользователя указываются явные пути, через которые политика позволяет приложению, основанному на утверждениях, предоставлять пользователю требуемые утверждения. Пользователь проходит через эти пути, чтобы извлечь утверждения, которые необходимо предоставить проверяющей стороне. Другими словами, пути взаимодействия пользователя определяют бизнес-логику, в соответствии с которой пользователь выполняет действия, когда инфраструктура процедур идентификации Azure Active Directory B2C обрабатывает запрос.

Эти пути взаимодействия пользователя можно рассматривать как шаблоны, доступные для удовлетворения основных потребностей различных проверяющих сторон сообщества. Пути взаимодействия пользователя упрощают определение части политики, которая является проверяющей стороной. Политика может определять несколько путей взаимодействия пользователя. Каждый путь взаимодействия пользователя представляет собой последовательность шагов оркестрации.

Чтобы определить пути взаимодействия пользователя, поддерживаемые этой политикой, необходимо добавить элемент **UserJourneys** в элемент верхнего уровня файла политики.

Элемент **UserJourneys** содержит следующий элемент:

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| UserJourney | 1:n | Путь взаимодействия пользователя, определяющий все конструкции, необходимые для полного потока пользователя. |

Элемент **UserJourney** содержит следующий атрибут.

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| Идентификатор | Да | Идентификатор пути взаимодействия пользователя, который можно использовать для ссылки на этот путь из других элементов в политике. Элемент **DefaultUserJourney**[политики проверяющей стороны](relyingparty.md) указывает на этот атрибут. |

Элемент **UserJourney** содержит следующие элементы.

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| аусоризатионтечникалпрофилес | 0:1 | Список технических профилей авторизации. | 
| OrchestrationSteps | 1:n | Последовательность оркестрации, которая должна соблюдаться для успешного выполнения транзакции. Каждый путь взаимодействия пользователя состоит из упорядоченного списка шагов оркестрации, которые должны выполняться в соответствующей последовательности. Если какой-либо шаг завершится ошибкой, выполнение транзакции завершается сбоем. |

## <a name="authorizationtechnicalprofiles"></a>аусоризатионтечникалпрофилес

Предположим, что пользователь завершил UserJourney и получил доступ или маркер идентификации. Для управления дополнительными ресурсами, такими как [Конечная точка UserInfo](userinfo-endpoint.md), необходимо определить пользователя. Чтобы начать этот процесс, пользователь должен предоставить маркер доступа, выпущенный ранее, как доказательство того, что изначально прошло проверку подлинности с помощью допустимой политики Azure AD B2C. Во время этого процесса для пользователя всегда должен присутствовать действительный маркер, чтобы убедиться, что он может выполнить этот запрос. Технические профили авторизации проверяют входящий токен и извлекают утверждения из токена.

Элемент **аусоризатионтечникалпрофилес** содержит следующий элемент:

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| аусоризатионтечникалпрофиле | 0:1 | Список технических профилей авторизации. | 

Элемент **аусоризатионтечникалпрофиле** содержит следующий атрибут:

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| TechnicalProfileReferenceId | Да | Идентификатор технического профиля, который необходимо выполнить. |

В следующем примере показан элемент пути взаимодействия пользователя с техническими профилями авторизации:

```xml
<UserJourney Id="UserInfoJourney" DefaultCpimIssuerTechnicalProfileReferenceId="UserInfoIssuer">
  <Authorization>
    <AuthorizationTechnicalProfiles>
      <AuthorizationTechnicalProfile ReferenceId="UserInfoAuthorization" />
    </AuthorizationTechnicalProfiles>
  </Authorization>
  <OrchestrationSteps>
    <OrchestrationStep Order="1" Type="ClaimsExchange">
     ...
```

## <a name="orchestrationsteps"></a>OrchestrationSteps

Путь взаимодействия пользователя представляется в виде последовательности оркестрации, которая должна быть соблюдена для успешного выполнения транзакции. Если какой-либо шаг завершится ошибкой, выполнение транзакции завершается сбоем. Эти шаги оркестрации касаются и стандартных блоков, и поставщиков утверждений, разрешенных в файле политики. В любом шаге оркестрации, отвечающем за отображение и преобразование для просмотра в пользовательском интерфейсе, также содержится ссылка на соответствующий идентификатор определения содержимого.

Шаги оркестрации можно условно выполнять на основе предусловий, определенных в элементе orchestration Step. Например, можно проверить выполнение шага оркестрации только в том случае, если существует определенное утверждение или если утверждение равно или не соответствует указанному значению.

Чтобы указать упорядоченный список шагов оркестрации, необходимо добавить элемент **OrchestrationSteps** как часть политики. Этот элемент является обязательным.

Элемент **OrchestrationSteps** содержит следующий элемент:

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| OrchestrationStep | 1:n | Упорядоченный шаг оркестрации. |

Элемент **OrchestrationStep** содержит следующие атрибуты.

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| `Order` | Да | Порядок шагов оркестрации. |
| `Type` | Да | Тип шага оркестрации. Возможные значения: <ul><li>**ClaimsProviderSelection** — указывает, что шаг оркестрации предоставляет пользователю различные варианты поставщиков утверждений, из которых нужно выбрать одного.</li><li>**CombinedSignInAndSignUp** — указывает, что на шаге оркестрации отображается общая страница для входа поставщика социальных сетей и для регистрации локальной учетной записи.</li><li>**ClaimsExchange** — указывает, что на шаге оркестрации выполняется обмен утверждениями с поставщиком утверждений.</li><li>Запросы **на выработку — указывает** , что этап оркестрации должен обрабатывать данные утверждений, отправленные Azure AD B2C от проверяющей стороны через его `InputClaims` конфигурацию.</li><li>**Инвокесубжаурнэй** — указывает, что этап оркестрации обменивается утверждениями с [подсистемой](subjourneys.md) (в общедоступной предварительной версии).</li><li>**SendClaims** — указывает, что на данном шаге оркестрации проверяющей стороне отправляется утверждение с маркером, выданным издателем утверждений.</li></ul> |
| ContentDefinitionReferenceId | Нет | Идентификатор [определения содержимого](contentdefinitions.md), связанного с этим шагом оркестрации. Обычно идентификатор ссылки на определение содержимого определяется в техническом профиле с самостоятельным подтверждением. Однако иногда в Azure AD B2C требуется показать некоторые данные, не используя технический профиль. Существует два примера. Если тип шага оркестрации является одним из следующих: `ClaimsProviderSelection` или  `CombinedSignInAndSignUp` , Azure AD B2C необходимо отобразить Выбор поставщика удостоверений без технического профиля. |
| CpimIssuerTechnicalProfileReferenceId | Нет | Тип шага этапа оркестрации — `SendClaims`. Это свойство определяет идентификатор технического профиля поставщика утверждений, выдающего маркер для проверяющей стороны.  Если это значение не указано, маркер проверяющей стороны не создается. |

Элемент **OrchestrationStep** может содержать следующие элементы.

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| Preconditions | 0:n | Список предварительных условий, которые необходимо соблюдать для выполнения шага оркестрации. |
| ClaimsProviderSelections | 0:n | Список возможных поставщиков утверждений для шага оркестрации. |
| ClaimsExchanges | 0:n | Список обмена утверждениями для шага оркестрации. |
| жаурнэйлист | 0:1 | Список кандидатов подкаталогов для шага оркестрации. |

### <a name="preconditions"></a>Preconditions

Элемент **Preconditions** содержит следующий элемент:

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| Precondition | 1:n | В зависимости от используемого технического профиля, этот элемент перенаправляет клиента к выбранному поставщику утверждений либо выполняет вызов сервера для обмена утверждениями. |


#### <a name="precondition"></a>Precondition

Шаги оркестрации можно условно выполнять на основе предусловий, определенных на этапе оркестрации. Существует два типа предусловий:
 
- **Утверждения существуют** — указывает, что действия должны выполняться, если указанные утверждения существуют в текущем контейнере утверждений пользователя.
- **Утверждение равно** — указывает, что действия должны выполняться, если существует указанное утверждение, и его значение равно указанному значению. При проверке выполняется порядковое сравнение с учетом регистра. При проверке типа логического утверждения используйте `True` или `False` .

Элемент **предусловия** содержит следующие атрибуты:

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| `Type` | Да | Тип выполняемой проверки или запроса для данного предварительного условия. Для этого элемента можно установить значение **ClaimsExist**, которое указывает, что действия должны выполняться, если указанные утверждения существуют в текущем наборе утверждений пользователя, или значение **ClaimEquals**, указывающее на то, что действия следует выполнить, если указанное утверждение существует и его значение соответствует указанному. |
| `ExecuteActionsIf` | Да | Используйте `true` или `false` тест, чтобы решить, должны ли быть выполнены действия в предусловии. |

Элементы **Precondition** содержат следующие элементы:

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| Значение | 1:2 | Идентификатор типа утверждения. Утверждение уже определено в разделе схемы утверждений в файле политики или в родительском файле политики. Если предусловие имеет тип `ClaimEquals` , второй `Value` элемент содержит проверяемое значение. |
| Действие | 1:1 | Действие, которое требуется выполнить, если в результате проверки предварительного условия в рамках шага оркестрации получено значение true. Если для `Action` присвоено значение `SkipThisOrchestrationStep`, не нужно выполнять сопоставленный элемент `OrchestrationStep`. |

#### <a name="preconditions-examples"></a>Примеры предварительных условий

Следующие предварительные условия позволяют проверить, существует ли элемент objectId пользователя. В пути взаимодействия пользователь выбрал способ входа с использованием локальной учетной записи. При наличии элемента objectId можно пропустить этот шаг оркестрации.

```xml
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Следующие предварительные условия позволяют проверить, выполнил ли пользователь вход с помощью учетной записи социальной сети. Предпринята попытка найти учетную запись пользователя в каталоге. Если пользователь входит в систему или регистрируется с локальной учетной записью, пропустите этот этап оркестрации.

```xml
<OrchestrationStep Order="3" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>authenticationSource</Value>
      <Value>localAccountAuthentication</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="AADUserReadUsingAlternativeSecurityId" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId-NoError" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Элемент Preconditions позволяет проверить несколько предварительных условий. В следующем примере проверяется, существуют ли элементы objectId или email. Если первое условие истинно, путешествие переходит к следующему этапу оркестрации.

```xml
<OrchestrationStep Order="4" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>email</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="SelfAsserted-SocialEmail" TechnicalProfileReferenceId="SelfAsserted-SocialEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claims-provider-selection"></a>Выбор поставщика утверждений

Выбор поставщика удостоверений позволяет пользователям выбрать действие из списка параметров. Выбор поставщика удостоверений состоит из пары двух этапов оркестрации: 

1. **Buttons** — он начинается с типа `ClaimsProviderSelection` или `CombinedSignInAndSignUp` содержит список параметров, которые пользователь может выбрать. Порядок параметров внутри `ClaimsProviderSelections` элемента определяет порядок кнопок, представленных пользователю.
2. **Действия** , за которыми следует тип `ClaimsExchange` . ClaimsExchange содержит список действий. Действие — это ссылка на технический профиль, например [OAuth2](oauth2-technical-profile.md), [OpenID Connect Connect](openid-connect-technical-profile.md), [claims](claims-transformation-technical-profile.md)или [Self-Assert](self-asserted-technical-profile.md). Когда пользователь нажимает одну из кнопок, выполняется соответствующее действие.

Элемент **клаимспровидерселектионс** содержит следующий элемент:

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| ClaimsProviderSelection | 1:n | Предоставляет список доступных для выбора поставщиков утверждений.|

Элемент **клаимспровидерселектионс** содержит следующие атрибуты:

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| дисплайоптион| Нет | Управляет поведением случая, когда доступен один из вариантов выбора поставщика утверждений. Возможные значения: `DoNotShowSingleProvider` (по умолчанию) пользователь сразу же перенаправляется в федеративный поставщик удостоверений. Или `ShowSingleProvider` Azure AD B2C представляет страницу входа с выбором поставщика удостоверений. Чтобы использовать этот атрибут, [версия определения содержимого](page-layout.md) должна быть `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` и выше.|

Элемент **ClaimsProviderSelection** содержит следующие атрибуты.

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| TargetClaimsExchangeId | Нет | Идентификатор обмена утверждениями, выполняемого на следующем шаге оркестрации выбора поставщика утверждений. Можно задать только данный атрибут или атрибут ValidationClaimsExchangeId, но не оба. |
| ValidationClaimsExchangeId | Нет | Идентификатор обмена утверждениями, выполняемого на текущем шаге оркестрации для проверки выбора поставщика утверждений. Можно задать только данный атрибут или атрибут TargetClaimsExchangeId, но не оба. |

### <a name="claims-provider-selection-example"></a>Пример выбора поставщика утверждений

На следующем этапе оркестрации пользователь может выбрать вход с помощью Facebook, LinkedIn, Twitter, Google или локальной учетной записи. Если пользователь выбирает один из поставщиков удостоверений социальных сетей, второй шаг оркестрации выполняется с выбранным обменом утверждениями, указанном в атрибуте `TargetClaimsExchangeId`. Второй шаг оркестрации перенаправляет пользователя к поставщику удостоверений социальных сетей для завершения процесса входа в систему. Если пользователь решит выполнить вход с использованием локальной учетной записи, Azure AD B2C остается на том же шаге оркестрации (той же странице регистрации или входа в систему) и пропускает второй этап оркестрации.

```xml
<OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsignin">
  <ClaimsProviderSelections>
    <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
    <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
  </ClaimsProviderSelections>
  <ClaimsExchanges>
  <ClaimsExchange Id="LocalAccountSigninEmailExchange"
        TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
  </ClaimsExchanges>
</OrchestrationStep>


<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claimsexchanges"></a>ClaimsExchanges

Элемент **ClaimsExchanges** содержит следующий элемент:

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| ClaimsExchange | 1:n | В зависимости от используемого технического профиля, этот элемент перенаправляет клиента в соответствии с выбранным элементом ClaimsProviderSelection либо выполняет вызов сервера для обмена утверждениями. |

Элемент **ClaimsExchange** содержит следующие атрибуты.

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| Идентификатор | Да | Идентификатор шага обмена утверждениями. Этот идентификатор используется для ссылки на обмен утверждениями из шага выбора поставщика утверждений в политике. |
| TechnicalProfileReferenceId | Да | Идентификатор технического профиля, который необходимо выполнить. |

## <a name="journeylist"></a>жаурнэйлист

Элемент **жаурнэйлист** содержит следующий элемент:

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| Кандидат | 1:1 | Ссылка на подпрограмму, которую необходимо вызвать. |

### <a name="candidate"></a>Кандидат

Элемент **Candidate** содержит следующие атрибуты:

| Атрибут | Обязательно | Описание |
| --------- | -------- | ----------- |
| субжаурнэйреференцеид | Да | Идентификатор [вспомогательного пути](subjourneys.md) , который должен быть выполнен. |
