---
title: Требования к инфраструктуре интерфейса SIP — Службы коммуникации Azure
description: Сведения о требованиях к инфраструктуре для настройки интерфейса SIP в Службах коммуникации Azure.
author: boris-bazilevskiy
manager: nmurav
services: azure-communication-services
ms.author: bobazile
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: ede650ae072ef53ed40a9372a292ab69fe8cc1af
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "103492735"
---
# <a name="sip-interface-infrastructure-requirements"></a>Требования к инфраструктуре интерфейса SIP 

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

 
В этой статье приведены сведения об инфраструктуре, лицензировании и возможностях подключения пограничного контроллера сеансов, которые упростят вашу работу при планировании развертывания интерфейса SIP.


## <a name="infrastructure-requirements"></a>Требования к инфраструктуре
Требования к инфраструктуре для поддерживаемых пограничных контроллеров сеансов, доменов и другие требования к сетевым подключениям для развертывания интерфейса SIP приведены в следующей таблице:  

|Требование к инфраструктуре|Вам потребуется|
|:--- |:--- |
|Пограничный контроллер сеансов (SBC)|Поддерживаемый SBC. Дополнительные сведения см. в разделе [Поддерживаемые SBC](#supported-session-border-controllers-sbcs).|
|Магистрали телефонной связи, подключенные к SBC|Одна или несколько магистралей телефонной связи, подключенные к SBC. На одном конце SBC подключается к Службам коммуникации Azure (ACS) через интерфейс SIP. SBC также может подключаться к сторонним телефонным системам, таким как УАТС, аналоговые телефонные адаптеры и т. д. Подходит любой вариант ТСОП, подключенный к SBC (для настройки магистралей ТСОП к SBC обратитесь к поставщикам SBC или поставщикам магистралей).|
|Подписка Azure.|Подписка Azure, которая используется для создания ресурса ACS, настройки и подключения к SBC.|
|Маркер доступа Служб коммуникации|Для совершения вызовов вам необходим допустимый маркер доступа с областью `voip`. См. раздел [Маркеры доступа](../identity-model.md#access-tokens).|
|Общедоступный IP-адрес для SBC|Общедоступный IP-адрес, который можно использовать для подключения к SBC. В зависимости от типа SBC может использовать преобразование сетевых адресов (NAT).|
|Полное доменное имя (FQDN) для SBC|Полное доменное для SBC, где часть домена в нем не совпадает с зарегистрированными доменами для вашей организации в Microsoft 365 или Office 365. Дополнительные сведения см. в разделе [Доменные имена для SBC](#sbc-domain-names).|
|Общедоступная запись DNS для SBC |Общедоступная запись DNS, которая сопоставляет полное доменное имя SBC с общедоступным IP-адресом. |
|Открытый доверенный сертификат для SBC |Сертификат для SBC, используемый для обмена данными с интерфейсом SIP. Дополнительные сведения см. в разделе [Открытый доверенный сертификат для SBC](#public-trusted-certificate-for-the-sbc).|
|IP-адреса и порты брандмауэра для сигналов и мультимедиа SIP |SBC обменивается данными со следующими службами в облаке:<br/><br/>прокси-сервер SIP, который обрабатывает сигналы;<br/>обработчик мультимедиа, который обрабатывает данные мультимедиа.<br/><br/>Эти две службы имеют отдельные IP-адреса в Microsoft Cloud, как описано далее в этом документе.


## <a name="sbc-domain-names"></a>Доменные имена SBC

Клиенты без Office 365 могут использовать любое доменное имя, для которого они могут получить открытый сертификат.

В таблице ниже вы найдете примеры DNS-имен, зарегистрированных для арендатора, сведения о том, можно ли использовать такие имена в качестве полных доменных имен для SBC, а также примеры допустимых полных доменных имен.

|DNS-имя|Можно использовать как полное доменное имя для SBC|Примеры полных доменных имен|
|:--- |:--- |:--- |
contoso.com|Да|**Допустимые имена:**<br/>sbc1.contoso.com<br/>ssbcs15.contoso.com<br/>europe.contoso.com|
|contoso.onmicrosoft.com|Нет|Использование доменов *.onmicrosoft.com не поддерживается для имен SBC.

Если вы являетесь клиентом Office 365, доменное имя SBC не должно совпадать с именем, зарегистрированным в доменах арендатора Office 365. Ниже приведен пример совместной работы Office 365 и Служб коммуникации Azure:

|Домен, зарегистрированный в Office 365|Примеры полного доменного имени SBC в Teams|Примеры полного доменного имени SBC в ACS|
|:--- |:--- |:--- |
**contoso.com** (домен второго уровня)|**sbc.contoso.com** (имя в домене второго уровня)|**sbc.acs.contoso.com** (имя в домене третьего уровня)<br/>**sbc.fabrikam.com** (любое имя в другом домене)|
|**o365.contoso.com** (домен третьего уровня)|**sbc.o365.contoso.com** (имя в домене третьего уровня)|**sbc.contoso.com** (имя в домене второго уровня)<br/>**sbc.acs.o365.contoso.com** (имя в домене четвертого уровня)<br/>**sbc.fabrikam.com** (любое имя в другом домене)

Связывание SBC работает на уровне ресурсов Служб коммуникации, то есть вы можете связывать несколько SBC с одним ресурсом Служб коммуникации, но не можете связать один SBC с несколькими ресурсами Служб коммуникации. Для связывания с различными ресурсами требуются уникальные полные доменные имена SBC.

## <a name="public-trusted-certificate-for-the-sbc"></a>Открытый доверенный сертификат для SBC

Корпорация Майкрософт рекомендует запросить сертификат для SBC. Для этого создайте запрос подписи сертификата (CSR). Конкретные инструкции по созданию CSR для SBC см. в инструкциях по взаимодействию или в документации, предоставляемой поставщиками SBC. 

  > [!NOTE]
  > Большинство центров сертификации требуют, чтобы длина закрытого ключа составляла не менее 2048 бит. Учитывайте это при создании CSR.

В сертификате должно быть указано полное доменное имя SBC в качестве общего имени или поле с альтернативным именем субъекта (SAN). Сертификат должен быть выдан непосредственно центром сертификации, а не промежуточным поставщиком.

Кроме того, интерфейс SIP Служб коммуникации поддерживает подстановочные знаки в общем имени и (или) SAN, при этом подстановочный знак должен соответствовать стандарту [RFC HTTP Over TLS](https://tools.ietf.org/html/rfc2818#section-3.1). 

Например, можно использовать `\*.contoso.com`, что соответствует полному доменному имени SBC `sbc.contoso.com`, но не `sbc.test.contoso.com`.

Сертификат должен быть создан одним из следующих корневых центров сертификации:

- AffirmTrust
- AddTrust External CA Root
- Baltimore CyberTrust Root*
- Buypass
- Cybertrust
- Class 3 Public Primary Certification Authority
- Comodo Secure Root CA
- Deutsche Telekom 
- DigiCert Global Root CA
- DigiCert High Assurance EV Root CA
- Entrust
- GlobalSign;
- Go Daddy;
- GeoTrust;
- Verisign, Inc. 
- SSL.com
- Starfield
- Symantec Enterprise Mobile Root for Microsoft 
- SwissSign
- Thawte Timestamping CA
- Trustwave
- TeliaSonera 
- T-Systems International GmbH (Deutsche Telekom)
- QuoVadis

Корпорация Майкрософт регулярно добавляет дополнительные центры сертификации с учетом пожеланий клиентов. 

## <a name="sip-signaling-fqdns"></a>Передача сигналов SIP: полные доменные имена 

Точки подключения для интерфейса SIP Служб коммуникации представлены следующими тремя полными доменными именами:

- **sip.pstnhub.microsoft.com** (глобальное полное доменное имя) — должно использоваться первым. Когда SBC отправляет запрос для разрешения этого имени, DNS-серверы Microsoft Azure возвращают IP-адрес, который указывает на основной центр обработки данных Azure, назначенный SBC. Назначение основано на метриках производительности центров обработки данных и географической близости к SBC. Возвращенный IP-адрес соответствует основному полному доменному имени.
- **sip2.pstnhub.microsoft.com** (второе полное доменное имя) — географически сопоставляется с регионом второго приоритета.
- **sip3.pstnhub.microsoft.com** (третье полное доменное имя) — географически сопоставляется с регионом третьего приоритета.

Размещение этих трех полных доменных имен в определенном порядке необходимо для следующего:

- Обеспечение оптимального взаимодействия (менее загруженный и ближайший к SBC центр обработки данных назначается путем отправки запроса к первому полному доменному имени).
- Обеспечение отработки отказа, если SBC подключается к центру обработки данных с временными проблемами. Дополнительные сведения см. в разделе [Механизм отработки отказа](#failover-mechanism-for-sip-signaling) ниже.  

Полные доменные имена — sip.pstnhub.microsoft.com, sip2.pstnhub.microsoft.com и sip3.pstnhub.microsoft.com — будут разрешены на один из следующих IP-адресов:

- `52.114.148.0`
- `52.114.132.46`
- `52.114.75.24` 
- `52.114.76.76` 
- `52.114.7.24` 
- `52.114.14.70`
- `52.114.16.74`
- `52.114.20.29`

Откройте порты брандмауэра для этих IP-адресов, чтобы разрешить входящий и исходящий трафик для таких адресов для передачи сигналов. Если ваш брандмауэр поддерживает DNS-имена, полное доменное имя `sip-all.pstnhub.microsoft.com` разрешается на все такие IP-адреса. 

## <a name="sip-signaling-ports"></a>Передача сигналов SIP: порты;

Используйте следующие порты для интерфейса SIP Служб коммуникации:

|Трафик|Исходный тип|Кому|Исходный порт|Конечный порт|
|:--- |:--- |:--- |:--- |:--- |
|SIP/TLS|Прокси-сервер SIP|SBC|1024–65535|Определен на SBC (для Office 365 GCC High/DoD нужно использовать только порт 5061)|
SIP/TLS|SBC|Прокси-сервер SIP|Определен на SBC|5061|

### <a name="failover-mechanism-for-sip-signaling"></a>Механизм отработки отказа для передачи сигналов SIP

SBC отправляет запрос DNS для разрешения sip.pstnhub.microsoft.com. В зависимости от расположения SBC и метрик производительности центра обработки данных выбирается основной центр обработки данных. Если с основным центром обработки данных возникает проблема, SBC попытается связаться с sip2.pstnhub.microsoft.com, который разрешается на второй назначенный центр обработки данных. В том редком случае, если центры обработки данных в двух регионах будут недоступны, SBC повторит попытку связи с последним полным доменным именем (sip3.pstnhub.microsoft.com), которое предоставляет IP-адрес третьего центра данных.

## <a name="media-traffic-ip-and-port-ranges"></a>Мультимедийный трафик: диапазоны IP-адресов и портов

Мультимедийный трафик передается в отдельную службу, которая называется обработчиком мультимедиа, и из нее. На момент публикации обработчик мультимедиа для ACS может использовать любой IP-адрес Azure. Скачайте [полный список адресов](https://www.microsoft.com/download/details.aspx?id=56519).

### <a name="port-range"></a>Диапазон портов
Диапазон портов для обработчиков мультимедиа приведен в следующей таблице: 

|Трафик|Исходный тип|Кому|Исходный порт|Конечный порт|
|:--- |:--- |:--- |:--- |:--- |
|UDP/SRTP|Обработчик мультимедиа|SBC|3478–3481 и 49152–53247|Определен на SBC|
|UDP/SRTP|SBC|Обработчик мультимедиа|Определен на SBC|3478–3481 и 49152–53247|

  > [!NOTE]
  > Корпорация Майкрософт рекомендует включить по меньшей мере два порта на каждый параллельный вызов на SBC.


## <a name="media-traffic-media-processors-geography"></a>Мультимедийный трафик: география обработчиков мультимедиа

Мультимедийный трафик проходит через компоненты, называемые обработчиками мультимедиа. Обработчики мультимедиа размещаются в тех же центрах обработки данных, что и прокси-серверы SIP. Кроме того, существуют дополнительные обработчики мультимедиа для оптимизации потока мультимедиа. Например, в Австралии мы сейчас не предоставляем компонент прокси-сервера SIP (данные SIP передаются через Сингапур или Гонконг (САР)), но в Австралии у нас есть локальный обработчик мультимедиа. Потребность в локальных обработчиках мультимедиа обуславливается задержкой, которая возникает при отправке трафика на дальние расстояния, например из Австралии в Сингапур или Гонконг (САР). Хотя задержка в приведенном выше примере допустима для поддержания хорошего качества вызовов при использовании трафика SIP, для трафика мультимедиа в реальном времени она слишком высокая.

Расположения, в которых развернуты прокси-сервер SIP и компоненты обработчиков мультимедиа:
- США (два в центрах обработки данных в Западной части США и Восточной части США);
- Европа (центры обработки данных в Амстердаме и Дублине);
- Азия (центры обработки данных в Сингапуре и Гонконге (САР));
- Австралия (центры обработки данных в Восточной Австралии и Юго-Восточной Австралии).

Расположения, в которых развернуты только обработчики мультимедиа (данные SIP передаются через указанный выше ближайший центр обработки данных):
- Япония (центры обработки данных в Восточной Японии и Западной Японии).


## <a name="media-traffic-codecs"></a>Мультимедийный трафик: кодеки

### <a name="leg-between-sbc-and-cloud-media-processor-or-microsoft-teams-client"></a>Участок между SBC и облачным обработчиком мультимедиа или клиентом Microsoft Teams.
Применимо к вариантам с обходом мультимедиа или без него.

Интерфейс прямой маршрутизации на участке между пограничным контроллером сеансов и облачным обработчиком мультимедиа может использовать следующие кодеки:

- SILK, G.711, G.722, G.729.

Вы можете принудительно использовать конкретный кодек на пограничном контроллере сеансов, исключив из предложения нежелательные кодеки.

### <a name="leg-between-acs-sdk-app-and-cloud-media-processor"></a>Участок между приложением пакета SDK ACS и облачным обработчиком мультимедиа

На участке между облачным обработчиком мультимедиа и приложением пакета SDK ACS используется SILK или G.722. Выбор кодека на этом участке основывается на алгоритмах Майкрософт, которые учитывают множество параметров. 

## <a name="supported-session-border-controllers-sbcs"></a>Поддерживаемые пограничные контроллеры сеансов (SBC)

Сертификация в настоящее время в процессе. В то же время клиенты могут использовать [пограничные контроллеры сеансов, сертифицированные для Teams](/MicrosoftTeams/direct-routing-border-controllers). 

## <a name="next-steps"></a>Дальнейшие шаги

### <a name="conceptual-documentation"></a>Основная документация

- [Понятие телефонии](./telephony-concept.md)
- [Типы телефонных номеров в Службах коммуникации Azure](./plan-solution.md)
- [Цены](../pricing.md)

### <a name="quickstarts"></a>Краткие руководства

- [Звонок на телефон](../../quickstarts/voice-video-calling/pstn-call.md)