---
title: Устранение неполадок при проверке издателя | Службы
titleSuffix: Microsoft identity platform
description: Описание способов устранения неполадок с проверкой издателя для платформы Microsoft Identity путем вызова интерфейсов API Microsoft Graph.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: troubleshooting
ms.workload: identity
ms.date: 01/28/2021
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
ms.openlocfilehash: acce282dcaac971716a97d7722543d78e28b7a34
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102517673"
---
# <a name="troubleshoot-publisher-verification"></a>Устранение неполадок с проверкой издателя
Если не удается завершить процесс или возникло непредвиденное поведение при [проверке издателя](publisher-verification-overview.md), следует начать с следующих действий, если возникают ошибки или возникает непредвиденное поведение. 

1. Проверьте [требования](publisher-verification-overview.md#requirements) и убедитесь, что они выполнены.

1. Просмотрите инструкции, чтобы [отметить приложение как проверенное издателем](mark-app-as-publisher-verified.md), и убедитесь, что все шаги были выполнены успешно.

1. Ознакомьтесь со списком [типичных проблем](#common-issues).

1. Воспроизведите запрос с помощью [песочницы Graph](#making-microsoft-graph-api-calls), чтобы собрать дополнительную информацию и исключить любые проблемы в пользовательском интерфейсе.

## <a name="common-issues"></a>Распространенные проблемы
Ниже приведены некоторые распространенные проблемы, которые могут возникнуть в процессе. 

- **Я не могу узнать мой идентификатор Microsoft Partner Network (MPN ID) или я не знал, кто является основным контактом для учетной записи** 
    1. Перейдите на страницу [регистрации MPN](https://partner.microsoft.com/dashboard/account/v3/enrollment/joinnow/basicpartnernetwork/new).
    1. Войдите с помощью учетной записи пользователя в основном клиенте Azure AD организации. 
    1. Если учетная запись MPN уже существует, она будет распознана и добавлена в учетную запись. 
    1. Перейдите на страницу [профиля партнера](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile), где будет указан идентификатор MPN и основное контактное лицо учетной записи.

- **Я не могу узнать, кто является глобальным администратором Azure AD (также известным как администратор компании или администратор клиента), как найти их? Как насчет администратора приложения или администратора облачных приложений?**
    1. Войдите на [портал Azure AD](https://aad.portal.azure.com) используя учетную запись пользователя в основном клиенте организации.
    1. Перейдите в раздел [Управление ролями](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators).
    1. Щелкните нужную роль администратора.
    1. Появится список пользователей, которым назначена эта роль.

- **Я не знаю, кто является администратором моей учетной записи MPN**. Перейдите на страницу управления пользователями [MPN](https://partner.microsoft.com/pcv/users) и отфильтруйте список пользователей, чтобы увидеть, какие пользователи находятся в различных ролях администратора.

- **Я получаю сообщение об ошибке, что мой идентификатор MPN недействителен или у меня нет доступа к нему.**
    1. Перейдите к [профилю партнера](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile) и убедитесь в том, что: 
        - Идентификатор MPN указан правильно. 
        - Ошибки или "ожидающие действия" не отображаются, а состояние проверки в разделе юридического бизнес-профиля и сведений о партнере имеют значения "авторизирован" или "успешно".
    1. Перейдите на страницу [управления клиентами MPN](https://partner.microsoft.com/dashboard/account/v3/tenantmanagement) и убедитесь, что клиент, в котором зарегистрировано приложение и с помощью которого вы выполняете вход, используя учетную запись пользователя, находится в списке связанных клиентов. Чтобы добавить дополнительный клиент, следуйте приведенным [здесь](/partner-center/multi-tenant-account)инструкциям. Учтите, что всем глобальным администраторам любого добавляемого клиента будут предоставлены права глобального администратора в учетной записи центра партнеров.
    1. Перейдите на [страницу управления пользователями MPN](https://partner.microsoft.com/pcv/users) и подтвердите, что пользователь входит в систему как глобальный администратор, администратор MPN или администратор учетных записей. Чтобы добавить пользователя в роль в центре партнеров, следуйте приведенным [здесь](/partner-center/create-user-accounts-and-set-permissions)инструкциям.

- **При входе на портал Azure AD не отображаются зарегистрированные приложения. Почему?** 
    Регистрации приложений могли быть созданы с использованием другой учетной записи пользователя в этом клиенте, личной или клиентской учетной записи или в другом клиенте. Убедитесь, что вы вошли с использованием правильной учетной записи в клиент, где были созданы регистрации приложений.

- **Я получаю сообщение об ошибке, связанное с многофакторной проверкой подлинности. Что мне делать?** 
    Убедитесь, что [многофакторная проверка подлинности](../fundamentals/concept-fundamentals-mfa-get-started.md) включена и **требуется** для пользователя, с которым выполняется вход, и для этого сценария. Например, MFA может иметь следующее:
    - Всегда требуется для пользователя, с которым выполняется вход
    - [Требуется для управления Azure](../conditional-access/howto-conditional-access-policy-azure-management.md).
    - [Требуется для типа администратора](../conditional-access/howto-conditional-access-policy-admin-mfa.md) , с которым выполняется вход.

## <a name="making-microsoft-graph-api-calls"></a>Выполнение вызовов API Microsoft Graph 

Если вы столкнулись с проблемой, но не можете понять, что вы видите в пользовательском интерфейсе, может быть полезно выполнить устранение неполадок с помощью вызовов Microsoft Graph, чтобы выполнить те же операции, которые можно выполнить на портале регистрации приложений.

Самый простой способ выполнить эти запросы — использовать [песочницу Graph](https://developer.microsoft.com/graph/graph-explorer). Вы также можете рассмотреть другие варианты, например использовать [POST](https://www.postman.com/) или [вызвать веб-запросы](/powershell/module/microsoft.powershell.utility/invoke-webrequest) с помощью PowerShell.  

Вы можете использовать Microsoft Graph, чтобы установить и отменить проверенного издателя приложения и проверить результат после выполнения одной из этих операций. Результат можно увидеть в объекте [приложения](/graph/api/resources/application), соответствующем регистрации приложения, а также в любых [субъектах-службах](/graph/api/resources/serviceprincipal), которые были созданы из этого приложения. Дополнительные сведения о связи между этими объектами см. в следующих статьях: [Объекты приложения и субъекта-службы в Azure Active Directory](app-objects-and-service-principals.md).  

Ниже приведены примеры некоторых полезных запросов.  

### <a name="set-verified-publisher"></a>Указание проверенного издателя 

Запрос

```
POST /applications/0cd04273-0d11-4e62-9eb3-5c3971a7cbec/setVerifiedPublisher 

{ 

    "verifiedPublisherId": "12345678" 

} 
```
 
Ответ 
```
204 No Content 
```
> [!NOTE]
> *verifiedPublisherID* — это идентификатор MPN. 

### <a name="unset-verified-publisher"></a>Удаление проверенного издателя 

Запрос:  
```
POST /applications/0cd04273-0d11-4e62-9eb3-5c3971a7cbec/unsetVerifiedPublisher 
```
 
Ответ 
```
204 No Content 
```
### <a name="get-verified-publisher-info-from-application"></a>Получение сведений о проверенном издателе из приложения 
 
```
GET https://graph.microsoft.com/v1.0/applications/0cd04273-0d11-4e62-9eb3-5c3971a7cbec 

HTTP/1.1 200 OK 

{ 
    "id": "0cd04273-0d11-4e62-9eb3-5c3971a7cbec", 

    ... 

    "verifiedPublisher" : { 
        "displayName": "myexamplePublisher", 
        "verifiedPublisherId": "12345678", 
        "addedDateTime": "2019-12-10T00:00:00" 
    } 
} 
```

### <a name="get-verified-publisher-info-from-service-principal"></a>Получение сведений о проверенном издателе из субъекта-службы 
```
GET https://graph.microsoft.com/v1.0/servicePrincipals/010422a7-4d77-4f40-9335-b81ef5c23dd4 

HTTP/1.1 200 OK 

{ 
    "id": "010422a7-4d77-4f40-9335-b81ef5c22dd4", 

    ... 

    "verifiedPublisher" : { 
        "displayName": "myexamplePublisher", 
        "verifiedPublisherId": "12345678", 
        "addedDateTime": "2019-12-10T00:00:00" 
    } 
} 
```

## <a name="error-reference"></a>Справочник по ошибкам 

Ниже приведен список возможных кодов ошибок, которые могут быть получены при устранении неполадок в Microsoft Graph или в процессе регистрации приложений на портале.

### <a name="mpnaccountnotfoundornoaccess"></a>MPNAccountNotFoundOrNoAccess

Указанный идентификатор MPN (`MPNID`) не существует, или у вас нет доступа к нему. Укажите допустимый идентификатор MPN и повторите попытку.
    
Чаще всего это вызвано тем, что пользователь, выполнивший вход, не является членом правильной роли для учетной записи MPN в центре партнеров. см. раздел [требования](publisher-verification-overview.md#requirements) для получения списка соответствующих ролей и просмотрите [распространенные проблемы](#common-issues) , чтобы получить дополнительные сведения. Также может быть вызвано тем, что клиент регистрируется в не добавлении в учетную запись MPN или является недопустимым ИДЕНТИФИКАТОРом MPN.

### <a name="mpnglobalaccountnotfound"></a>MPNGlobalAccountNotFound

Указан недопустимый идентификатор MPN (`MPNID`). Укажите допустимый идентификатор MPN и повторите попытку.
    
Чаще всего возникает, когда предоставляется идентификатор MPN, соответствующий учетной записи расположения партнера (PLA). Поддерживаются только глобальные учетные записи партнеров. Дополнительные сведения см. в разделе [Структура учетной записи центра партнеров](/partner-center/account-structure) .

### <a name="mpnaccountinvalid"></a>MPNAccountInvalid

Указан недопустимый идентификатор MPN (`MPNID`). Укажите допустимый идентификатор MPN и повторите попытку.
    
Чаще всего это вызвано неправильным ИДЕНТИФИКАТОРом MPN.

### <a name="mpnaccountnotvetted"></a>MPNAccountNotVetted

Указанный идентификатор MPN (`MPNID`) не завершил процесс проверки. Завершите этот процесс в Центре партнеров и повторите попытку. 
    
Чаще всего происходит, когда учетная запись MPN не завершила процесс [проверки](/partner-center/verification-responses) .

### <a name="nopublisheridonassociatedmpnaccount"></a>NoPublisherIdOnAssociatedMPNAccount

Указан недопустимый идентификатор MPN (`MPNID`). Укажите допустимый идентификатор MPN и повторите попытку. 
   
Чаще всего это вызвано неправильным ИДЕНТИФИКАТОРом MPN.

### <a name="mpniddoesnotmatchassociatedmpnaccount"></a>MPNIdDoesNotMatchAssociatedMPNAccount

Указан недопустимый идентификатор MPN (`MPNID`). Укажите допустимый идентификатор MPN и повторите попытку.
    
Чаще всего это вызвано неправильным ИДЕНТИФИКАТОРом MPN.

### <a name="applicationnotfound"></a>ApplicationNotFound

Не удается найти целевое приложение (`AppId`). Укажите допустимый идентификатор приложения и повторите попытку.
    
Чаще всего возникает, если проверка выполняется с помощью API Graph, а идентификатор предоставленного приложения неверен. Примечание. необходимо указать идентификатор приложения, а не AppId/ClientId.

### <a name="b2ctenantnotallowed"></a>B2CTenantNotAllowed

Эта функция не поддерживается в клиенте Azure AD B2C.

### <a name="emailverifiedtenantnotallowed"></a>EmailVerifiedTenantNotAllowed

Эта функция не поддерживается в клиенте, проверенном по электронной почте.

### <a name="nopublisherdomainonapplication"></a>NoPublisherDomainOnApplication

Целевое приложение ( `AppId` ) должно иметь набор доменов издателя. Укажите домен издателя и повторите попытку.

Происходит, когда [домен издателя](howto-configure-publisher-domain.md) не настроен в приложении.

### <a name="publisherdomainmismatch"></a>PublisherDomainMismatch

Домен издателя целевого приложения (`publisherDomain`) не соответствует домену, используемому для проверки электронной почты в Центре партнеров (`pcDomain`). Убедитесь, что эти домены совпадают, и повторите попытку. 
    
Происходит, когда ни [домен издателя](howto-configure-publisher-domain.md) приложения, ни один из [пользовательских доменов](../fundamentals/add-custom-domain.md) , добавленных в клиент Azure AD, не совпадают с доменом, используемым для проверки электронной почты в центре партнеров.

### <a name="notauthorizedtoverifypublisher"></a>NotAuthorizedToVerifyPublisher

Вы не имеете права устанавливать проверенное свойство издателя в приложении (<`AppId` ) 
  
Чаще всего это вызвано тем, что пользователь, выполнивший вход, не является членом соответствующей роли для учетной записи MPN в Azure AD. Дополнительные сведения см. в статье [требования](publisher-verification-overview.md#requirements) к списку соответствующих ролей и [Общие вопросы](#common-issues) .

### <a name="mpnidwasnotprovided"></a>MPNIdWasNotProvided

Идентификатор MPN не указан в тексте запроса, или тип содержимого запроса не имел значения application/JSON.

### <a name="msanotsupported"></a>MSANotSupported 

Эта функция не поддерживается для учетных записей потребителей Майкрософт. Поддерживаются только приложения, зарегистрированные в Azure AD пользователем Azure AD.

### <a name="interactionrequired"></a>InteractionRequired

Происходит, когда многофакторная проверка подлинности не была выполнена перед попыткой добавить проверенного издателя в приложение. Дополнительные сведения см. в разделе [распространенные проблемы](#common-issues) . Примечание. MFA необходимо выполнять в том же сеансе при попытке добавить проверенного издателя. Если MFA включен, но не требуется для выполнения в сеансе, запрос завершится ошибкой. 

Отобразится следующее сообщение об ошибке: "из-за изменения конфигурации, внесенного администратором, или при перемещении в новое расположение для продолжения необходимо использовать многофакторную проверку подлинности".

### <a name="unabletoaddpublisher"></a>унаблетоаддпублишер

Отображается сообщение об ошибке: "проверенный издатель не может быть добавлен в это приложение. Обратитесь за помощью к администратору ".

Сначала убедитесь, что выполнены [требования к проверке издателя](publisher-verification-overview.md#requirements).

Когда выполняется запрос на добавление проверенного издателя, для оценки рисков безопасности используется ряд сигналов. Если запрос определен как рискованный, будет возвращена ошибка. По соображениям безопасности корпорация Майкрософт не раскрывает определенные критерии, используемые для определения того, является ли запрос рискованным или нет.

## <a name="next-steps"></a>Дальнейшие действия

Если вы проверили все предыдущие сведения и по-прежнему получаете сообщение об ошибке от Microsoft Graph, соберите как можно больше указанных ниже сведений, связанных с неудачным запросом, и [обратитесь в службу поддержки Майкрософт](developer-support-help-options.md#create-an-azure-support-request).

- Отметка времени 
- CorrelationId 
- ObjectID или UserPrincipalName пользователя, выполнившего вход; 
- ObjectId целевого приложения;
- AppId целевого приложения;
- TenantId, где зарегистрировано приложение;
- Идентификатор MPN
- выполненный запрос REST; 
- возвращаемый код ошибки и сообщение.