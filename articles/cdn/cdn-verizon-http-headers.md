---
title: HTTP-заголовки (Verizon) для обработчика правил Azure CDN | Документация Майкрософт
description: В этой статье описывается использование HTTP-заголовков (Verizon) с обработчиком правил Azure CDN.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: allensu
ms.openlocfilehash: e20f6ce9540d357b61ae2cfdf0e8f96d127dc6c0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "84343223"
---
# <a name="verizon-specific-http-headers-for-azure-cdn-rules-engine"></a>HTTP-заголовки (Verizon) для обработчика правил Azure CDN

При использовании продуктов **Azure CDN уровня "Премиум" от Verizon**, когда HTTP-запрос отправляется на сервер-источник, сервер точки подключения (POP) может добавить один или несколько защищенных заголовков (или специальных заголовков прокси-сервера) в запросе клиента к точке подключения. Эти заголовки предоставляются в дополнение к полученным стандартным заголовкам пересылки. Сведения о стандартных заголовках запроса см. в разделе [о полях запросов](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields).

Если вы хотите, чтобы один из этих зарезервированных заголовков не был добавлен в POP-запрос Azure CDN (сеть доставки содержимого) к серверу-источнику, вам нужно создать в обработчике правил правило с [функцией специальных заголовков прокси-сервера](https://docs.vdms.com/cdn/Content/HRE/F/Proxy-Special-Headers.htm). В этом правиле исключите заголовок, который требуется удалить из списка заголовков по умолчанию, в поле заголовков. Если вы включили функцию [Заголовки ответа Debug Cache](https://docs.vdms.com/cdn/Content/HRE/F/Debug-Cache-Response-Headers.htm), не забудьте добавить необходимые заголовки `X-EC-Debug`. 

Например, чтобы удалить заголовок `Via`, поле заголовков правила должно включать в себя следующий список заголовков: *X-Forwarded-For, X-Forwarded-Proto, X-Host, X-Midgress, X-Gateway-List, X-EC-Name, Host*. 

![Правило "Специальные заголовки прокси-сервера"](./media/cdn-http-headers/cdn-proxy-special-header-rule.png)

В следующей таблице описаны заголовки, которые можно добавить в запрос с помощью POP CDN Verizon:

Заголовок запроса | Описание | Пример
---------------|-------------|--------
[Via](#via-request-header) | Идентифицирует сервер POP, который передал запрос на сервер-источник. | HTTP/1.1 ECS (dca/1A2B)
X-Forwarded-For | Указывает IP-адрес инициатора запроса.| 10.10.10.10
X-Forwarded-Proto | Указывает протокол запроса. | http
X-Host | Указывает имя узла запроса. | cdn.mydomain.com
X-Midgress | Указывает, передан ли запрос через дополнительный сервер CDN. Например, от POP-сервера к защитному серверу-источнику или от POP-сервера к серверу шлюза ADN. <br />Этот заголовок добавляется к запросу, только если присутствует трафик с промежуточного сервера. В этом случае заголовок имеет значение 1. Это означает, что запрос был передан через дополнительный CDN-сервер.| 1
[Узел](#host-request-header) | Идентифицирует узел и порт, на которых можно найти запрошенное содержимое. | marketing.mydomain.com:80
[X-Gateway-List](#x-gateway-list-request-header) | ADN. Определяет список отработок отказа серверов шлюза ADN, присвоенных источнику клиента. <br />Защитный сервер-источник. Указывает набор защитных серверов-источников, присвоенных источнику клиента. | `icn1,hhp1,hnd1`
X-EC- _&lt;имя&gt;_ | Заголовки запроса, которые начинаются с *X-EC* (например, X-EC-Tag, [X-EC-Debug](cdn-http-debug-headers.md)), зарезервированы для сети CDN.| waf-production

## <a name="via-request-header"></a>Заголовок запроса Via
Формат, через который заголовок запроса `Via` определяет POP-сервер, указан с помощью следующего синтаксиса:

`Via: Protocol from Platform (POP/ID)` 

Термины, используемые в синтаксисе, определяются следующим образом:
- Protocol. Указывает версию протокола (например, HTTP/1.1), используемую для передачи запроса. 

- Platform. Указывает платформу, на которой было запрошено содержимое. В этом поле допустимы следующие коды: 

    Код | Платформа
    -----|---------
    ECAcc | Большой HTTP-объект
    ECS   | Малый HTTP-объект
    ECD   | Сеть доставки приложений (ADN)

- POP. Указывает [POP](cdn-pop-abbreviations.md), который обработал запрос. 

- ID. Только для внутреннего использования.

### <a name="example-via-request-header"></a>Пример заголовка запроса Via

`Via: HTTP/1.1 ECD (dca/1A2B)`

## <a name="host-request-header"></a>Заголовок запроса Host
POP-серверы перезапишут заголовок `Host`, если выполняются оба следующих условия.
- Источник для запрошенного содержимого — это сервер-источник клиента.
- Параметр заголовка HTTP Host соответствующего источника клиента не пуст.

Заголовок запроса `Host` будет перезаписан для отражения значения, определенного в параметре HTTP-заголовка Host.
Если для заголовка HTTP-заголовка Host источника клиента не задано значение, заголовок `Host`, предоставленный инициатором запроса, будет отправлен на сервер-источник клиента.

## <a name="x-gateway-list-request-header"></a>Заголовок запроса X-Gateway-List
POP-сервер будет добавлять или перезаписывать заголовок запроса X-Gateway-List, если выполняется одно из следующих условий:
- Запрос указывает на платформу ADN.
- Запрос передается на сервер-источник клиента, который защищен с помощью функции защитного сервера-источника.

