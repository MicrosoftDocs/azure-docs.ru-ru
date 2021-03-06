---
title: Настройка DNS-имен с помощью диспетчера трафика
description: Узнайте, как настроить личный домен для приложения службы приложений Azure, которое интегрируется с диспетчером трафика для балансировки нагрузки.
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.topic: article
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 2910ea3f896ba3920126737965ca9c9dbabcfeb3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "101709110"
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-with-traffic-manager-integration"></a>Настройка пользовательского доменного имени в службе приложений Azure с интеграцией диспетчера трафика

[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

> [!NOTE]
> Сведения об облачных службах см. в статье [Настройка пользовательского доменного имени для облачной службы Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).

При использовании [диспетчера трафика Azure](../traffic-manager/index.yml) для балансировки нагрузки трафика в [службу приложений Azure](overview.md)доступ к приложению службы приложений можно получить с помощью **\<traffic-manager-endpoint> . trafficmanager.NET**. Вы можете назначить собственное доменное имя, например www \. contoso.com, в приложении службы приложений, чтобы предоставить пользователям более узнаваемое доменное имя.

В этой статье показано, как настроить пользовательское доменное имя с помощью приложения службы приложений, интегрированного с [диспетчером трафика](../traffic-manager/traffic-manager-overview.md).

> [!NOTE]
> При настройке доменного имени с помощью конечной точки диспетчера трафика поддерживаются только записи [CNAME](https://en.wikipedia.org/wiki/CNAME_record) . Поскольку записи не поддерживаются, сопоставление корневого домена, например contoso.com, также не поддерживается.
> 

## <a name="prepare-the-app"></a>Подготовка приложения

Чтобы связать пользовательское DNS-имя с приложением, интегрированным с диспетчером трафика Azure, [план службы приложений](https://azure.microsoft.com/pricing/details/app-service/) веб-приложения должен быть на уровне **Standard** или выше. На этом шаге следует убедиться, что приложение службы приложений находится в поддерживаемой ценовой категории.

### <a name="check-the-pricing-tier"></a>Проверка ценовой категории

В [портал Azure](https://portal.azure.com)найдите и выберите **службы приложений**.

На странице **Службы приложений** выберите имя приложения Azure.

![Переход к приложению Azure на портале](./media/app-service-web-tutorial-custom-domain/select-app.png)

В левой области навигации страницы приложения выберите **увеличить масштаб (план службы приложений)**.

![Меню увеличения масштаба](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Текущий уровень приложения выделен синей рамкой. Убедитесь, что приложение находится на уровне **Standard** или выше (любой уровень в **рабочей** или **изолированной** категории). Если да, закройте страницу " **масштаб вверх** " и перейдите к статье [Создание сопоставления CNAME](#create-the-cname-mapping).

![Проверка ценовой категории](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

### <a name="scale-up-the-app-service-plan"></a>Изменение уровня плана службы приложений

Если необходимо увеличить масштаб приложения, выберите любую из ценовых категорий в категории **Рабочая** . Чтобы просмотреть дополнительные параметры, щелкните **См. дополнительные параметры**.

Нажмите кнопку **Применить**.

## <a name="create-traffic-manager-endpoint"></a>Создать конечную точку диспетчера трафика

Выполните действия, описанные в разделе [Добавление или удаление конечных точек](../traffic-manager/traffic-manager-manage-endpoints.md), добавьте приложение службы приложений в качестве конечной точки в профиле диспетчера трафика.

Когда приложение службы приложений находится в поддерживаемой ценовой категории, оно отображается в списке доступных целевых объектов службы приложений при добавлении конечной точки. Если приложение отсутствует в списке, [Проверьте ценовую категорию приложения](#prepare-the-app).

## <a name="create-the-cname-mapping"></a>Создание сопоставления CNAME
> [!NOTE]
> Чтобы настроить [приобретенный домен службы приложений](manage-custom-dns-buy-domain.md), пропустите этот раздел и перейдите к разделу [Включение пользовательского домена](#enable-custom-domain).
> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Хотя особенности каждого поставщика домена различаются, *вы сопоставляете* [некорневое имя пользовательского домена](#what-about-root-domains) (например, **www.contoso.com**) с доменным именем диспетчера трафика (**contoso.trafficmanager.NET** *),* интегрированным с приложением. 

> [!NOTE]
> Если запись уже используется и вам нужно заблаговременно привязать к ней свое приложение, создайте дополнительную запись CNAME. Например, чтобы выполнить предварительную привязку **www \. contoso.com** к приложению, создайте запись CNAME из **awverify. www** в **contoso.trafficmanager.NET**. Затем можно добавить "www \. contoso.com" в приложение без необходимости изменять запись CNAME "www". Дополнительные сведения см. [в статье перенос активного DNS-имени в службу приложений Azure](manage-custom-dns-migrate-domain.md).

По завершении добавления или изменения записей DNS сохраните эти изменения в своем поставщике домена.

### <a name="what-about-root-domains"></a>Что насчет корневых доменов?

Так как диспетчер трафика поддерживает только сопоставление пользовательских доменов с записями CNAME, а стандарты DNS не поддерживают записи CNAME для сопоставления корневых доменов (например, **contoso.com**), диспетчер трафика не поддерживает сопоставление с корневыми доменами. Чтобы обойти эту ошибку, используйте перенаправление URL-адреса с уровня приложения. Например, в ASP.NET Core можно использовать [переписывание URL-адресов](/aspnet/core/fundamentals/url-rewriting). Затем используйте диспетчер трафика для балансировки нагрузки поддомена (**www.contoso.com**). Другой подход — [создать запись псевдонима для доменного имени вершине для ссылки на профиль диспетчера трафика Azure](../dns/tutorial-alias-tm.md). Например, contoso.com. Вместо использования службы перенаправления можно настроить Azure DNS для ссылки на профиль диспетчера трафика непосредственно из зоны. 

Для сценариев с высоким уровнем доступности можно реализовать балансировку нагрузки при настройке DNS без диспетчера трафика, создав несколько *записей* , которые указывают из корневого домена на IP-адрес каждой копии приложения. Затем [сопоставьте один и тот же корневой домен со всеми копиями приложения](app-service-web-tutorial-custom-domain.md#map-an-a-record). Так как одно и то же доменное имя не может быть сопоставлено с двумя разными приложениями в одном регионе, эта настройка работает только в том случае, если копии приложения находятся в разных регионах.

## <a name="enable-custom-domain"></a>Включить личный домен
После распространения записей для доменного имени используйте браузер, чтобы убедиться, что имя пользовательского домена разрешается в приложение службы приложений.

> [!NOTE]
> Распространение CNAME через систему DNS может занять некоторое время. Вы можете использовать службу, например <a href="https://www.digwebinterface.com/">https://www.digwebinterface.com/</a> , чтобы убедиться, что запись CNAME доступна.
> 
> 

1. После того как разрешение домена будет выполнено, вернитесь на страницу приложения на [портале Azure](https://portal.azure.com) .
2. В левой области навигации выберите **Личные домены**  >  **Добавить имя узла**.
4. Введите имя пользовательского домена, которое было сопоставлено ранее, и выберите **проверить**.
5. Убедитесь, что в поле **Тип записи имени узла** выбрана запись **CNAME (wwwexample\.com или любой поддомен)** .

6. Так как приложение службы приложений теперь интегрировано с конечной точкой диспетчера трафика, в разделе **конфигурация CNAME** должно отобразиться имя домена диспетчера трафика. Выберите его и щелкните **Добавить личный домен**.

    ![Добавление DNS-имени в приложение](./media/configure-domain-traffic-manager/enable-traffic-manager-domain.png)

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Защита настраиваемого DNS-имени с помощью привязки SSL в Службе приложений Azure](configure-ssl-bindings.md)