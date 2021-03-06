---
title: Что такое маршрут "Стандартный" или "Премиум" для передней дверцы Azure?
description: Эта статья поможет вам понять, как служба "Предварительная дверца Azure" Standard/Premium соответствует правилу маршрутизации, используемому для входящего запроса.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: duau
ms.openlocfilehash: db026c4903aa30a0a4c8154af8ad6eeb4b72b706
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "101100053"
---
# <a name="what-is-azure-front-door-standardpremium-preview-route"></a>Что такое маршрут передней дверцы Azure (Предварительная версия) "Стандартный" или "Премиум"?

> [!Note]
> Эта документация предназначена для Azure Front Door категории "Стандартный" или "Премиум" (предварительная версия). Сведения об Azure Front Door см. [здесь](../front-door-overview.md).

Маршрут "Стандартный/Премиум" передней дверцы Azure определяет, как будет обрабатываться трафик при поступлении входящего запроса в среду передней дверцы Azure. С помощью параметров маршрута определяется связь между доменом и серверной группой происхождения. Включив дополнительные функции, такие как шаблон для машинного набора правил, можно получить более детализированный контроль над трафиком.

Конфигурация маршрутизации "Стандартный" или "Премиум" для передней дверцы состоит из двух основных частей: "левая часть" и "правая часть". Мы сопоставлены входящий запрос с левой стороны маршрута, и правая часть определяет, как обрабатываются запросы.

> [!IMPORTANT]
> Передняя дверца Azure Standard/Premium (Предварительная версия) в настоящее время доступна в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены.
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="incoming-match-left-hand-side"></a>Входящее соответствие (левая сторона)

Следующие свойства определяют, соответствует ли входящий запрос правилу маршрутизации (левая сторона)

* **Протоколы HTTP** (HTTP/HTTPS)
* **Узлы** (например, www \. foo.com, \* . Bar.com);
* **Пути** (например, /\*, /users/\*, /file.gif)

Эти свойства развертываются внутри так, чтобы любое сочетание протоколов, узлов и путей становилось набором потенциальных соответствий.

### <a name="route-data-right-hand-side"></a>Данные маршрута (справа)

Решение о том, как обработать запрос, зависит от того, включено ли кэширование для маршрута. Если кэшированный ответ недоступен, запрос перенаправляется в соответствующую серверную часть.

## <a name="route-matching"></a>Сопоставление маршрутов

В этом разделе основное внимание уделяется сопоставлению с заданным правилом маршрутизации Front Door. Суть в том, что сначала всегда выполняется сопоставление **наиболее конкретному соответствию**, мы смотрим только на "левую сторону".  Сначала мы устанавливаем соответствие по HTTP-протоколу, затем по узлу внешнего интерфейса, затем по пути.

### <a name="frontend-host-matching"></a>Сопоставление узла внешнего интерфейса

При сопоставлении интерфейсных узлов используется логика, определенная ниже:

1. Следует искать любой маршрут с точным соответствием на узле.
2. Если точное соответствие среди узлов внешнего интерфейса не найдено, следует отклонить запрос и отправить ошибку 400: неверный запрос.

Чтобы рассмотреть эту процедуру еще подробнее, обратимся к примеру конфигурации маршрутов Front Door (только левая сторона):

| Правило маршрутизации | Узлы внешнего интерфейса | Путь |
|-------|--------------------|-------|
| A | foo.contoso.com | /\* |
| B | foo.contoso.com | /users/\* |
| C | www \. Fabrikam.com, foo.Adventure-Works.com  | /\*, /images/\* |

Если следующие входящие запросы были отправлены во Front Door, они будут соответствовать следующим правилам маршрутизации (см. выше):

| Входящий узел внешнего интерфейса | Сопоставленные правила маршрутизации |
|---------------------|---------------|
| foo.contoso.com | A, B |
| www \. Fabrikam.com | C |
| images.fabrikam.com | Ошибка 400: неверный запрос |
| foo.adventure-works.com | C |
| contoso.com | Ошибка 400: неверный запрос |
| www \. Adventure-Works.com | Ошибка 400: неверный запрос |
| www \. northwindtraders.com | Ошибка 400: неверный запрос |

### <a name="path-matching"></a>Согласование путей

После того как служба "Передняя дверь" (Standard) Azure определяет конкретный интерфейсный узел и отфильтровывает возможные правила маршрутизации только для маршрутов с этим интерфейсным узлом. Затем передняя дверца фильтрует правила маршрутизации на основе пути запроса. Мы используем ту же логику, что и узлы внешнего интерфейса:

1. Следует искать любое правило маршрутизации с точным соответствием по пути
2. Если путей с точным соответствием не найдено, ищите правила маршрутизации с соответствующим путем с символами подстановки
3. Если правила маршрутизации с соответствующим путем не найдены, отклоняйте запрос и возвращайте HTTP-ответ "Ошибка 400: неверный запрос".

>[!NOTE]
> Любые пути без подстановочного знака считаются путями точного соответствия. Даже если путь завершается косой чертой, он считается путем точного соответствия.

Чтобы пояснить это, давайте взглянем еще на несколько примеров:

| Правило маршрутизации | Узел внешнего интерфейса    | Путь     |
|-------|---------|----------|
| A     | www\.contoso.com | /        |
| B     | www\.contoso.com | /\*      |
| C     | www\.contoso.com | /ab      |
| D     | www\.contoso.com | /abc     |
| E     | www\.contoso.com | /abc/    |
| F     | www\.contoso.com | /abc/\*  |
| G     | www\.contoso.com | /abc/def |
| H     | www\.contoso.com | /path/   |

С учетом конфигурации в следующем примере таблицы сопоставлений будут представлены следующие данные:

| Входящий запрос    | Соответствующий маршрут |
|---------------------|---------------|
| www \. contoso.com/            | A             |
| www \. contoso.com/a           | B             |
| www \. contoso.com/AB          | C             |
| www \. contoso.com/ABC         | D             |
| www \. contoso.com/abzzz       | B             |
| www \. contoso.com/ABC/        | E             |
| www \. contoso.com/ABC/d       | F             |
| www \. contoso.com/ABC/DEF     | G             |
| www \. contoso.com/ABC/defzzz  | F             |
| www \. contoso.com/ABC/DEF/GHI | F             |
| www \. contoso.com/Path        | B             |
| www \. contoso.com/Path/       | H             |
| www \. contoso.com/Path/ZZZ    | B             |

>[!WARNING]
> </br> Если нет правил маршрутизации для узла внешнего интерфейса с точным соответствием и универсальным путем маршрута (`/*`), то не будет соответствия и для любого правила маршрутизации.
>
> Пример конфигурации:
>
> | Маршрут | Узел             | Путь    |
> |-------|------------------|---------|
> | A     | profile.contoso.com | /api/\* |
>
> Таблица соответствия:
>
> | Входящий запрос       | Соответствующий маршрут |
> |------------------------|---------------|
> | profile.domain.com/other | Отсутствует. Ошибка 400: неверный запрос |

### <a name="routing-decision"></a>Решение о маршрутизации

После того как передняя дверца Azure уровня "Стандартный" или "Премиум" сопоставляется с одним правилом маршрутизации, необходимо выбрать способ обработки запроса. Если передняя дверца Azure уровня "Стандартный" или "Премиум" имеет кэшированный ответ, доступный для соответствующего правила маршрутизации, запрос возвращается клиенту. Далее следует определить, есть ли у вас правила для соответствующего правила маршрутизации, как в передней дверце Azure Standard/Premium. Если набор правил не определен, запрос перенаправляется во внутренний пул, как есть. В противном случае набор правил выполняется в том порядке, в котором они настроены.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как [создать переднюю дверь уровня "Стандартный" или "Премиум](create-front-door-portal.md)".
