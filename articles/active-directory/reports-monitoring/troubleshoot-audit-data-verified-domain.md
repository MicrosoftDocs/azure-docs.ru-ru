---
title: Устранение неполадок с данными аудита для проверенного изменения домена | Документация Майкрософт
description: Содержит сведения, которые будут отображаться в журналах действий Azure Active Directory при изменении домена, проверенного пользователями.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/22/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3c9ec3b1e96e47dbf46c6acb2c81147b614d069
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "87117438"
---
# <a name="troubleshoot-audit-data-on-verified-domain-change"></a>Устранение неполадок: аудит данных при изменении проверенного домена 


## <a name="i-have-a-lot-of-changes-to-my-users-and-i-am-not-sure-what-the-cause-of-it-is"></a>У меня много изменений в моих пользователях, и я не уверен, что его причина.

### <a name="symptoms"></a>Симптомы

Я проберу журналы аудита Azure AD и вижу несколько обновлений пользователей, происходящих в клиенте Azure AD. Эти события **обновления пользователя** не отображают сведения о **субъекте** , что приводит к неуверенности в том, что или кто запускает массовые изменения для пользователей. 

### <a name="cause"></a>Причина

 Распространенной причиной изменения массовых объектов является несинхронная Серверная операция с именем **ProxyCalc**.  **ProxyCalc** — это логика, которая определяет соответствующие **userPrincipalName** и **адреса прокси-сервера**, которые обновляются пользователями, группами или контактами Azure AD. Проектирование **ProxyCalc** заключается в том, чтобы все **userPrincipalName** и **адреса прокси** в Azure AD были одинаковыми в любой момент. **ProxyCalc** должен инициироваться явным изменением, таким как проверенное изменение домена, и не выполняется в фоновом режиме в качестве задачи. 

  

#### <a name="what-does-userprincipalname-consistency-mean"></a>Что означает согласованность UserPrincipalName? 

Для облачных пользователей согласованность означает, что для **userPrincipalName** задан проверенный суффикс домена. При обработке непротиворечивого **userPrincipalName** **ProxyCalc** преобразует его в суффикс onmicrosoft.com по умолчанию, например: username@Contoso.onmicrosoft.com 

Для синхронизированных пользователей согласованность означает, что **userPrincipalName** имеет значение проверенного суффикса домена и соответствует локальному значению **userPrincipalName** (шадовусерпринЦипалнаме). При обработке непротиворечивого **userPrincipalName** **ProxyCalc** вернется к тому же значению, что и **шадовусерпринЦипалнаме** , или, если суффикс домена был удален из клиента, преобразует его в суффикс домена по умолчанию *. onmicrosoft.com. 

  

#### <a name="what-does-proxy-address-consistency-mean"></a>Что означает согласованность адресов прокси-сервера? 

Для облачных пользователей согласованность означает, что адреса прокси-сервера совпадают с проверенным суффиксом домена. При обработке несоответствия адреса прокси-сервера **ProxyCalc** преобразует его в суффикс домена *. onmicrosoft.com по умолчанию, например: SMTP:username@Contoso.onmicrosoft.com 

Для синхронизированных пользователей согласованность означает, что адреса прокси-сервера совпадают со значениями локальных прокси-серверов (т. е. Шадовпроксяддрессес). **ProxyAddresses** должны быть синхронизированы с **шадовпроксяддрессес**. Если синхронизированному пользователю назначена лицензия Exchange, адреса прокси-сервера должны совпадать со значениями локальных прокси-серверов, а также должны совпадать с проверенным суффиксом домена. В этом случае **ProxyCalc** будет очистить непоследовательный адрес прокси-сервера с непроверенным суффиксом домена и будет удален из объекта в Azure AD. Если этот непроверенный домен будет проверен позже, **ProxyCalc** выполнит повторное вычисление и добавит адрес прокси из **шадовпроксяддрессес** обратно в объект в Azure AD.  

> [!NOTE]
> Для синхронизированных объектов, чтобы избежать **ProxyCalcной** логики при вычислении непредвиденных результатов, рекомендуется задать адреса прокси-сервера для проверенного домена Azure AD в локальном объекте.  

  
Одна из задач администрирования, которая может активировать **ProxyCalc** , — при наличии проверенного изменения домена. Эта задача выполняется каждый раз, когда проверенный домен добавляется в клиент Azure AD или удаляется из него, который внутренне активирует **ProxyCalc**.  

Например, если добавить проверенный Домен Fabrikam.com в клиент Contoso.onmicrosoft.com, это действие вызовет операцию ProxyCalc для всех объектов в клиенте. Это событие будет записываться в журналы аудита Azure AD как **Обновление пользовательских** событий с предшествующей событием **добавления проверенного домена** . С другой стороны, если Fabrikam.com был удален из клиента Contoso.onmicrosoft.com, то перед всеми событиями **обновления пользователя** будет стоять событие **удаления проверенного домена** .   

#### <a name="additional-notes"></a>Дополнительные замечания

ProxyCalc не приводит к изменениям некоторых объектов, которые: 

- нет активной лицензии Exchange 
- Задайте для **мсексчремотереЦипиенттипе** значение null 
- не считаются общими ресурсами. Общий ресурс имеет значение, когда **клаудмсексчреЦипиентдисплайтипе** содержит одно из следующих значений **: маилбоксусер (Shared)**, **публикфолдер**, **конференцеруммаилбокс**, **EquipmentMailbox**, **ArbitrationMailbox**, **RoomList**, **TeamMailboxUser**, **почтовый ящик**, **планирование почтовых ящиков**, **ACLableMailboxUser**, **ACLableTeamMailboxUser** 
  
 Чтобы создать больше корреляции между этими двумя разнородными событиями, корпорация Майкрософт работает над обновлением сведений о **субъекте** в журналах аудита для обнаружения этих изменений, вызванных проверенным изменением домена. Это действие поможет проверить, когда произошло событие проверенного изменения домена и было запущено для массового обновления объектов в своем клиенте. 

Кроме того, в большинстве случаев изменения пользователей в качестве **userPrincipalName** и **адреса прокси-серверов** не изменяются, поэтому мы работаем над отображением в журналах аудита только тех обновлений, которые привели к фактическому изменению объекта. Это действие предотвратит шум в журналах аудита и помогает администраторам сопоставить оставшиеся изменения пользователей с проверенным событием изменения домена, как описано выше. 

## <a name="next-steps"></a>Дальнейшие действия

[Теневые атрибуты службы синхронизации Azure AD Connect](../hybrid/how-to-connect-syncservice-shadow-attributes.md)
