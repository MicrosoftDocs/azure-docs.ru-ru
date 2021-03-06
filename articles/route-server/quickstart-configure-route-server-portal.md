---
title: Краткое руководство. Создание и настройка сервера маршрутизации с помощью портала Azure
description: В этом кратком руководстве показано, как создать и настроить сервер маршрутизации с помощью портала Azure.
services: route-server
author: duongau
ms.service: route-server
ms.topic: quickstart
ms.date: 03/03/2021
ms.author: duau
ms.openlocfilehash: f76c48af4f5ebc8013daad457f9973cf7792c7c6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "102548004"
---
# <a name="quickstart-create-and-configure-route-server-using-the-azure-portal"></a>Краткое руководство. Создание и настройка сервера маршрутизации с помощью портала Azure

Выполнив инструкции, приведенные в этой статье, вы сможете настроить сервер маршрутизации Azure для пиринга с виртуальными сетевыми модулями (NVA) в виртуальной сети, используя портал Azure. Сервер маршрутизации Azure будет анализировать маршруты от NVA и программировать их для виртуальных машин в виртуальной сети. Он также будет объявлять маршруты виртуальной сети к NVA. См. дополнительные сведения о [сервере маршрутизации Azure](overview.md).

> [!IMPORTANT]
> Сервер маршрутизации Azure (предварительная версия) сейчас предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены.
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Предварительные требования

* Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) бесплатно.
* Ознакомьтесь с [ограничениями службы для сервера маршрутизации Azure](route-server-faq.md#limitations).

## <a name="create-a-route-server"></a>Создание сервера маршрутизации

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Войдите в учетную запись Azure и выберите подписку.

В браузере откройте [портал Azure](https://portal.azure.com) и выполните вход с помощью учетной записи Azure.

### <a name="create-a-route-server"></a>Создание сервера маршрутизации

1. Перейдите к https://aka.ms/routeserver.

1. Выберите **+ Create new route server** (+ Создать сервер маршрутизации).

    :::image type="content" source="./media/quickstart-configure-route-server-portal/route-server-landing-page.png" alt-text="Экран &quot;Снимок экрана: целевая страница сервера маршрутизации&quot;."::: 

1. На странице **Create a Route Server** (Создать сервер маршрутизации) введите или выберите необходимую информацию.

    :::image type="content" source="./media/quickstart-configure-route-server-portal/create-route-server-page.png" alt-text="Экран &quot;Снимок экрана: страница создания сервера маршрутизации&quot;.":::     

    | Настройки | Значение |
    |----------|-------|
    | Подписка | Выберите подписку Azure, которую нужно использовать для развертывания сервера маршрутизации. |
    | Группа ресурсов | Выберите группу ресурсов, в которой будет создан сервер маршрутизации. Создайте группу ресурсов, если у вас нет существующей группы ресурсов. |
    | Имя | Введите имя сервера маршрутизации. |
    | Регион | Выберите регион, в котором будет создан сервер маршрутизации. Выберите тот же регион, что и для виртуальной сети, созданной ранее, чтобы увидеть виртуальную сеть в раскрывающемся списке. |
    | Виртуальная сеть | Выберите виртуальную сеть, в которой будет создан сервер маршрутизации. Можно создать новую виртуальную сеть или использовать имеющуюся. Если вы используете имеющуюся виртуальную сеть, убедитесь, что адресное пространство подсети не меньше /27, чтобы обеспечить соблюдение требований к подсети сервера маршрутизации. Если виртуальная сеть не отображается в раскрывающемся списке, убедитесь, что выбраны правильные группа ресурсов или регион. |
    | Подсеть | После создания или выбора виртуальной сети появится поле подсети. Эта подсеть предназначена только для сервера маршрутизации. Выберите **Управление конфигурацией подсети** и создайте подсеть сервера маршрутизации Azure. Выберите **+ Subnet** (+ Подсеть) и создайте подсеть, следуя приведенным ниже рекомендациям.</br><br>– Используйте для подсети имя *RouteServerSubnet*.</br><br>– Адресное пространство подсети должно быть не меньше /27.</br> |

1. Выберите **Просмотр и создание**, проверьте раздел "Сводка" и щелкните **Создать**. 

    > [!NOTE]
    > Развертывание сервера маршрутизации займет около 20 минут.

## <a name="set-up-peering-with-nva"></a>Настройка пиринга с помощью виртуальных сетевых модулей

Инструкции из этого раздела помогут вам в настройке пиринга BGP с помощью виртуальных сетевых модулей.

1. Перейдите к пункту [Сервер маршрутизации](https://aka.ms/routeserver) на портале Azure и выберите сервер маршрутизации, который нужно настроить.

    :::image type="content" source="./media/quickstart-configure-route-server-portal/select-route-server.png" alt-text="Экран &quot;Снимок экрана: список серверов маршрутизации&quot;."::: 

1. Выберите **Одноранговые узлы** в разделе *Параметры* на левой панели навигации. Нажмите кнопку **+ Добавить**, чтобы добавить новый одноранговый узел.

    :::image type="content" source="./media/quickstart-configure-route-server-portal/peers-landing-page.png" alt-text="Экран &quot;Снимок экрана: целевая страница одноранговых узлов&quot;."::: 

1. Введите следующие сведения об одноранговом узле виртуального сетевого модуля.

    :::image type="content" source="./media/quickstart-configure-route-server-portal/add-peer-page.png" alt-text="Экран &quot;Снимок экрана: страница добавления однорангового узла&quot;.":::

    | Настройки | Значение |
    |----------|-------|
    | Имя | Укажите имя для пиринга между сервером маршрутизации и виртуальным сетевым модулем. |
    | ASN |  Введите номер автономной системы (ASN) своего виртуального сетевого модуля. |
    | Адрес IPv4 | Введите IP-адрес виртуального сетевого модуля, с которым будет взаимодействовать сервер маршрутизации, чтобы установить связь по BGP. |

1. Нажмите кнопку **Добавить**, чтобы добавить этот одноранговый узел.

## <a name="complete-the-configuration-on-the-nva"></a>Завершение настройки NVA

Вам понадобятся IP-адреса однорангового узла сервера маршрутизации Azure и ASN для завершения настройки NVA и настройки сеанса BGP. Эти сведения можно получить на странице обзора сервера маршрутизации.

:::image type="content" source="./media/quickstart-configure-route-server-portal/route-server-overview.png" alt-text="Экран &quot;Снимок экрана: страница обзора сервера маршрутизации&quot;.":::

## <a name="configure-route-exchange"></a>Настройка обмена маршрутами

Если у вас есть шлюз ExpressRoute и VPN-шлюз и вы хотите, чтобы они могли обмениваться маршрутами с сервером маршрутизации, можно включить обмен маршрутами.

1. Перейдите к пункту [Сервер маршрутизации](https://aka.ms/routeserver) на портале Azure и выберите сервер маршрутизации, который нужно настроить.

1. Выберите **Конфигурация** в разделе *Параметры* на левой панели навигации.

1. Выберите **Включить** для параметра **Подключение между ветвями** и нажмите кнопку **Сохранить**.

    :::image type="content" source="./media/quickstart-configure-route-server-portal/enable-route-exchange.png" alt-text="Экран &quot;Снимок экрана: сведения о том, как включить обмен маршрутами&quot;.":::

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вам больше не нужен сервер маршрутизации Azure, выберите **Удалить** на странице обзора, чтобы отключить сервер маршрутизации.

:::image type="content" source="./media/quickstart-configure-route-server-portal/delete-route-server.png" alt-text="Экран &quot;Снимок экрана: сведения о том, как удалить сервер маршрутизации&quot;.":::

## <a name="next-steps"></a>Дальнейшие действия

Создав сервер маршрутизации Azure, вы можете продолжить изучение способов взаимодействия между сервером маршрутизации Azure с ExpressRoute и VPN-шлюзами: 

> [!div class="nextstepaction"]
> [Поддержка Azure ExpressRoute и VPN-шлюза Azure](expressroute-vpn-support.md)
