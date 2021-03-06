---
title: Новые возможности Сетка событий Azure — заметки о выпуске
description: Узнайте о новых возможностях Сетки событий Azure, ознакомившись с последними заметками о выпуске, известными проблемами, исправлениями ошибок, нерекомендуемыми функциями и предстоящими изменениями.
ms.topic: overview
ms.date: 07/23/2020
ms.openlocfilehash: da0b26e4f163f428e6955a37636ceb19bb34abc5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105107539"
---
# <a name="whats-new-in-azure-event-grid"></a>Новые возможности в Сетке событий Azure

>Получайте уведомления о том, когда следует повторно посетить эту страницу для обновлений, скопировав и вставив URL-адрес `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Event+Grid%22&locale=en-us` в средство чтения RSS-канала ![значок средства чтения RSS](./media/whats-new/feed-icon-16x16.png).

Сетка событий Azure постоянно совершенствуется. Чтобы вы оставались в курсе последних разработок, в этой статье предоставлены такие сведения:

- Последние выпуски.
- Известные проблемы
- Исправления ошибок
- Нерекомендуемые функции.
- Планы по изменениям.

## <a name="600-2020-06"></a>6.0.0 (2020-06)
- Включена поддержка нового общедоступного API службы версии 2020-06-01.
- Новые возможности, которые стали общедоступными:
    - [Сопоставления входных данных](input-mappings.md)
    - [Пользовательская входная схема](input-mappings.md)
    - [Схема событий облака версии 10](cloud-event-schema.md)
    - [Раздел служебной шины как назначение](handler-service-bus.md)
    - [Функция Azure как назначение](handler-functions.md)
    - [Пакетная обработка данных веб-перехватчика](./edge/delivery-output-batching.md)
    - [Защищенный веб-перехватчик (поддержка Azure Active Directory)](secure-webhook-delivery.md)
    - [Фильтрация по IP-адресам](configure-firewall.md)
    - [Поддержка Приватного канала](configure-private-endpoints.md)
    - [Схема доставки событий](event-schema.md)

## <a name="532-preview-2020-05"></a>5.3.2-preview (2020-05)
- Этот выпуск содержит дополнительные исправления ошибок для повышения качества.
- Как и версия 5.3.1-preview, этот выпуск соответствует API версии 2020-04-01-preview, которая включает следующие новые возможности: 
    - [Поддержка фильтрации по IP-адресам при публикации событий в доменах и темах](configure-firewall.md)
    - [Партнерские разделы](./partner-events-overview.md)
    - [Системные разделы как отслеживаемые ресурсы на портале Azure](system-topics.md)
    - [Доставка событий с использованием управляемого удостоверения службы](managed-service-identity.md) 
    - [Поддержка Приватного канала](configure-private-endpoints.md)

## <a name="531-preview-2020-04"></a>5.3.1-preview (2020-04)
- Этот выпуск содержит разные исправления ошибок для повышения качества.
- Как и версия 5.3.0-preview, этот выпуск соответствует API версии 2020-04-01-preview, которая включает следующие новые возможности: 
    - [Поддержка фильтрации по IP-адресам при публикации событий в доменах и темах](configure-firewall.md)
    - [Партнерские разделы](./partner-events-overview.md)
    - [Системные разделы как отслеживаемые ресурсы на портале Azure](system-topics.md)
    - [Доставка событий с использованием управляемого удостоверения службы](managed-service-identity.md) 
    - [Поддержка Приватного канала](configure-private-endpoints.md)

## <a name="530-preview-2020-03"></a>5.3.0-preview (2020-03)
- Мы представляем новые возможности в дополнение к тем, которые уже добавлены в версии 5.2.0-preview. 
- Как и версия 5.2.0-preview, этот выпуск соответствует версии API 2020-04-01-preview.
- Включена поддержка следующих новых возможностей: 
    - [Поддержка фильтрации по IP-адресам при публикации событий в доменах и темах](configure-firewall.md)
    - [Партнерские разделы](./partner-events-overview.md)
    - [Системные разделы как отслеживаемые ресурсы на портале Azure](system-topics.md)
    - [Доставка событий с использованием управляемого удостоверения службы](managed-service-identity.md) 
    - [Поддержка Приватного канала](configure-private-endpoints.md)

## <a name="520-preview-2020-01"></a>5.2.0-preview (2020-01)
- Этот выпуск соответствует версии API 2020-04-01-preview.
- Включена поддержка следующих новых возможностей:
    - [Поддержка фильтрации по IP-адресам при публикации событий в доменах и темах](configure-firewall.md)

## <a name="500-2019-05"></a>5.0.0 (2019-05)
- Этот выпуск соответствует версии API `2019-06-01`.
- Включена поддержка следующих новых возможностей:
    * [Домены](event-domains.md)
    * Разбиение на страницы и использование фильтра поиска для операций со списком ресурсов. Пример см. в статье [Разделы — список по подписке](/rest/api/eventgrid/version2020-10-15-preview/partnernamespaces/listbysubscription).
    * [Очередь служебной шины как назначение](handler-service-bus.md)
    * [Расширенная фильтрация](event-filtering.md#advanced-filtering)

## <a name="410-preview-2019-03"></a>4.1.0-preview (2019-03)
- Этот выпуск соответствует версии API 2019-02-01-preview.
- Включена поддержка следующих новых возможностей:
    * Разбиение на страницы и использование фильтра поиска для операций со списком ресурсов. Пример см. в статье [Разделы — список по подписке](/rest/api/eventgrid/version2020-10-15-preview/partnernamespaces/listbysubscription).
    * [Создание и удаление разделов домена вручную](how-to-event-domains.md)
    * [Очередь Служебной шины как назначение](handler-service-bus.md)

## <a name="400-2018-12"></a>4.0.0 (2018-12)
- Этот выпуск соответствует версии API `2019-01-01`.
- Он поддерживает общедоступные версии следующих возможностей, связанных с подписками на события:
    * [Назначение для недоставленных сообщений](manage-event-delivery.md)
    * [Очередь службы хранилища Azure как назначение](handler-storage-queues.md)
    * [Azure Relay — гибридное подключение как назначение](handler-relay-hybrid-connections.md)
    * [Проверка подтверждения вручную](webhook-event-delivery.md)
    * [Поддержка политик повтора](delivery-and-retry.md)
- Функции, которые по-прежнему предоставляются в предварительной версии (например, [домены Сетки событий](event-domains.md) или [поддержка расширенных фильтров](event-filtering.md#advanced-filtering)), доступны в составе пакета SDK версии 3.0.1-preview.

## <a name="301-preview-2018-10"></a>3.0.1-preview (2018-10)
- Зависимость от [версии 10.0.3 пакета NuGet Newtonsoft](https://www.nuget.org/packages/Newtonsoft.Json/10.0.3).

## <a name="300-preview-2018-10"></a>3.0.0-preview (2018-10)
- Этот выпуск SDK предоставляется в предварительной версии с новыми функциями, появившимися в версии API 2018-09-15-preview. В этом выпуске включена поддержка следующих возможностей:
    - [Домены и разделы доменов](event-domains.md)
    - [Дата истечения срока действия для подписки на события](concepts.md#event-subscription-expiration)
    - [Расширенная фильтрация](event-filtering.md#advanced-filtering) для подписок на события
    - Стабильная версия пакета SDK, предназначенная для версии API `2018-01-01`, сохраняется в качестве версии 1.3.0.

## <a name="next-steps"></a>Дальнейшие действия