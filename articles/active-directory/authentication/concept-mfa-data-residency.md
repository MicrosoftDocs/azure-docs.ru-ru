---
title: Местонахождение данных многофакторной проверки подлинности Azure AD
description: Узнайте, какие личные и корпоративные данные хранятся в многофакторной проверке подлинности Azure AD, и ваши пользователи и какие данные остаются в стране или регионе происхождения.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: inbarc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7381ab62eb39c555c6b4eb34f150fc71bea1f10f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103561470"
---
# <a name="data-residency-and-customer-data-for-azure-ad-multifactor-authentication"></a>Данные местонахождение и данные клиента для многофакторной проверки подлинности Azure AD

Azure Active Directory (Azure AD) хранит данные клиентов в географическом расположении на основе адреса, предоставляемого Организацией при подписке на службы Microsoft Online Service, такие как Microsoft 365 или Azure. Сведения о том, где хранятся данные клиентов, см. в разделе [где находятся ваши данные?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) в центре управления безопасностью Майкрософт.

Многофакторная проверка подлинности Azure AD и сервер многофакторной проверки подлинности с использованием Azure Active Directory и хранение персональных данных и данных Организации. В этой статье рассказано, где хранятся те или иные данные.

Служба многофакторной проверки подлинности Azure AD содержит центры обработки данных в США, Европе и Азиатско-Тихоокеанский регион. Следующие действия исходят от региональных центров обработки данных, за исключением указанных ниже случаев.

* Телефонные звонки многофакторной проверки подлинности берутся из США центров обработки данных и направляются глобальными поставщиками.
* Запросы проверки подлинности пользователей общего назначения в других регионах обрабатываются в зависимости от расположения пользователя.
* Push-уведомления, использующие приложение Microsoft Authenticator, в настоящее время обрабатываются в региональных центрах обработки данных на основе расположения пользователя. Зависящие от поставщика службы устройств, например Cлужба push-уведомлений Apple, могут находиться за пределами расположения пользователя.

## <a name="personal-data-stored-by-azure-ad-multifactor-authentication"></a>Персональные данные, сохраненные с помощью многофакторной проверки подлинности Azure AD

Личные данные — это сведения на уровне пользователя, связанные с конкретным пользователем. Персональные данные содержатся в следующих хранилищах.

* Заблокированные пользователи
* Обойденные пользователи
* Запросы на изменение маркеров устройств Microsoft Authenticator
* Отчеты о действиях многофакторной проверки подлинности
* Активации Microsoft Authenticator

Эти сведения хранятся в течение 90 дней.

Многофакторная проверка подлинности Azure AD не регистрирует личные данные, такие как имена пользователей, Номера телефонов или IP-адреса. Однако *значение userobjectid* определяет попытки проверки подлинности пользователей. Данные журнала хранятся в течение 30 дней.

### <a name="data-stored-by-azure-ad-multifactor-authentication"></a>Данные, хранящиеся в многофакторной проверке подлинности Azure AD

Для общедоступных облаков Azure, исключая проверку подлинности Azure AD B2C, расширение NPS, а также адаптер службы федерации Active Directory (AD FS) (AD FS) Windows Server 2016 или 2019, хранятся следующие персональные данные:

| Тип события                           | Тип хранилища данных |
|--------------------------------------|-----------------|
| OATH-токен                           | Журналы многофакторной проверки подлинности     |
| Односторонние SMS                          | Журналы многофакторной проверки подлинности     |
| Голосовой звонок                           | Журналы многофакторной проверки подлинности<br/>Хранилище данных отчетов о действиях многофакторной проверки подлинности<br/>Заблокированные пользователи (если было сообщено о мошенничестве) |
| Уведомление Microsoft Authenticator | Журналы многофакторной проверки подлинности<br/>Хранилище данных отчетов о действиях многофакторной проверки подлинности<br/>Заблокированные пользователи (если было сообщено о мошенничестве)<br/>Запросы на изменение при изменении токена устройства Microsoft Authenticator |

Для Microsoft Azure для государственных организаций, Microsoft Azure — Германия, Microsoft Azure с помощью 21Vianet, Azure AD B2C проверки подлинности, расширения NPS и адаптера AD FS Windows Server 2016 или 2019, сохраняются следующие персональные данные:

| Тип события                           | Тип хранилища данных |
|--------------------------------------|-----------------|
| OATH-токен                           | Журналы многофакторной проверки подлинности<br/>Хранилище данных отчетов о действиях многофакторной проверки подлинности |
| Односторонние SMS                          | Журналы многофакторной проверки подлинности<br/>Хранилище данных отчетов о действиях многофакторной проверки подлинности |
| Голосовой звонок                           | Журналы многофакторной проверки подлинности<br/>Хранилище данных отчетов о действиях многофакторной проверки подлинности<br/>Заблокированные пользователи (если было сообщено о мошенничестве) |
| Уведомление Microsoft Authenticator | Журналы многофакторной проверки подлинности<br/>Хранилище данных отчетов о действиях многофакторной проверки подлинности<br/>Заблокированные пользователи (если было сообщено о мошенничестве)<br/>Запросы на изменение при изменении токена устройства Microsoft Authenticator |

### <a name="data-stored-by-azure-multifactor-authentication-server"></a>Данные, хранящиеся на сервере многофакторной идентификации Azure

Если используется сервер многофакторной проверки подлинности Azure, хранятся следующие персональные данные.

> [!IMPORTANT]
> По состоянию на 2019 1 июля корпорация Майкрософт больше не предлагает сервер многофакторной проверки подлинности для новых развертываний. Новые клиенты, желающие требовать от пользователей многофакторную проверку подлинности, должны использовать облачную многофакторную проверку подлинности Azure AD. Существующие клиенты, которые активировали сервер многофакторной проверки подлинности до 1 июля 2019, могут скачать последнюю версию и обновления и создать учетные данные активации обычным образом.

| Тип события                           | Тип хранилища данных |
|--------------------------------------|-----------------|
| OATH-токен                           | Журналы многофакторной проверки подлинности<br />Хранилище данных отчетов о действиях многофакторной проверки подлинности |
| Односторонние SMS                          | Журналы многофакторной проверки подлинности<br />Хранилище данных отчетов о действиях многофакторной проверки подлинности |
| Голосовой звонок                           | Журналы многофакторной проверки подлинности<br />Хранилище данных отчетов о действиях многофакторной проверки подлинности<br />Заблокированные пользователи (если было сообщено о мошенничестве) |
| Уведомление Microsoft Authenticator | Журналы многофакторной проверки подлинности<br />Хранилище данных отчетов о действиях многофакторной проверки подлинности<br />Заблокированные пользователи (если было сообщено о мошенничестве)<br />Запросы на изменение при изменении маркеров устройств Microsoft Authenticator |

## <a name="organizational-data-stored-by-azure-ad-multifactor-authentication"></a>Данные организации, хранящиеся в многофакторной проверке подлинности Azure AD

Организационные данные — это сведения на уровне клиента, которые могут предоставлять конфигурацию или настройку среды. Параметры клиента из следующих портал Azure страниц многофакторной проверки подлинности могут хранить данные организации, такие как пороговые значения блокировки или сведения об ИДЕНТИФИКАТОРе вызывающего объекта для входящих запросов проверки подлинности телефона:

* Блокировка учетной записи
* Предупреждение о мошенничестве
* Уведомления
* Параметры телефонного звонка

Для сервера многофакторной идентификации Azure следующие страницы портал Azure могут содержать данные организации:

* Параметры сервера
* Разовый обход
* Правила кэширования
* Состояние сервера многофакторной проверки подлинности

## <a name="multifactor-authentication-logs-location"></a>Расположение журналов многофакторной проверки подлинности

В следующей таблице показано расположение журналов службы для общедоступных облаков.

| Общедоступное облако| Журналы входа | Отчет о действиях многофакторной проверки подлинности        | Журналы службы многофакторной проверки подлинности       |
|-------------|--------------|----------------------------------------|------------------------|
| США          | США           | США                                     | США                     |
| Европа      | Европа       | США                                     | Европа <sup>2</sup>    |
| Австралия   | Австралия    | США<sup>1</sup>                         | Австралия <sup>2</sup> |

<sup>1</sup> Журналы OATH-кода хранятся в Австралии.

<sup>2</sup> Голосовой вызов журналов службы многофакторной проверки подлинности хранится в США.

В следующей таблице показано расположение журналов служб для облаков независимых.

| Национальное облако                      | Журналы входа                         | Отчет о действиях многофакторной проверки подлинности (включая персональные данные)| Журналы службы многофакторной проверки подлинности |
|--------------------------------------|--------------------------------------|-------------------------------|------------------|
| Microsoft Azure — Германия              | Германия                              | США                            | США               |
| Azure China 21Vianet                 | Китай                                | США                            | США               |
| Облако Microsoft для государственных организаций           | США                                   | США                            | США               |

Отчеты о действиях многофакторной проверки подлинности содержат персональные данные, такие как имя участника-пользователя (UPN) и полный номер телефона.

Журналы службы многофакторной проверки подлинности используются для работы службы.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о том, какие сведения о пользователях собираются с помощью облачной многофакторной проверки подлинности Azure AD и сервера многофакторной идентификации Azure, см. в статье сбор данных пользователей многофакторной проверки [подлинности Azure AD](howto-mfa-reporting-datacollection.md).
