---
title: Управление виртуальными сетями. Azure CLI — гибкий сервер в базе данных Azure для MySQL
description: Создание виртуальных сетей и управление ими для базы данных Azure для MySQL — гибкого сервера с помощью Azure CLI
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: e60f9010b8fbfb3766aea6b5cffa1e7c001c6eca
ms.sourcegitcommit: bed20f85722deec33050e0d8881e465f94c79ac2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105106740"
---
# <a name="create-and-manage-virtual-networks-for-azure-database-for-mysql---flexible-server-using-the-azure-cli"></a>Создание виртуальных сетей и управление ими для базы данных Azure для MySQL — гибкого сервера с помощью Azure CLI

> [!IMPORTANT]
> Сейчас предоставляется общедоступная предварительная версия Гибкого сервера Базы данных Azure для MySQL.

Гибкий сервер Базы данных Azure для MySQL поддерживает два типа взаимоисключающих методов сетевого подключения к гибкому серверу. Эти два варианта указаны ниже:

- Открытый доступ (разрешенные IP-адреса)
- Закрытый доступ (интеграция виртуальной сети)

В этой статье мы рассмотрим создание сервера MySQL с **частным доступом (интеграция с виртуальной сетью)** с помощью Azure CLI. С помощью *частного доступа (интеграция виртуальной сети)* можно развернуть гибкий сервер в своей [виртуальной сети Azure](../../virtual-network/virtual-networks-overview.md). Виртуальные сети Azure обеспечивают частную и безопасную сетевую связь. В частном доступе подключения к серверу MySQL ограничены только в пределах виртуальной сети. Дополнительные сведения см. в разделе [частный доступ (интеграция виртуальной сети)](./concepts-networking.md#private-access-vnet-integration).

В базе данных Azure для гибкого сервера MySQL можно развернуть сервер только в виртуальной сети и подсети во время создания сервера. После развертывания гибкого сервера в виртуальной сети и подсети его нельзя переместить в другую виртуальную сеть, подсеть или *общий доступ (разрешенные IP-адреса)*.

## <a name="launch-azure-cloud-shell"></a>Запуск Azure Cloud Shell

[Azure Cloud Shell](../../cloud-shell/overview.md) — это бесплатная интерактивная оболочка, с помощью которой можно выполнять действия, описанные в этой статье. Она включает предварительно установленные общие инструменты Azure и настроена для использования с вашей учетной записью.

Чтобы открыть Cloud Shell, просто выберите **Попробовать** в правом верхнем углу блока кода. Кроме того, Cloud Shell можно открыть в отдельной вкладке браузера. Для этого перейдите на страницу [https://shell.azure.com/bash](https://shell.azure.com/bash). Нажмите кнопку **Копировать**, чтобы скопировать блоки кода. Вставьте код в Cloud Shell и нажмите клавишу **ВВОД**, чтобы выполнить его.

Если вы решили установить и использовать CLI локально, для выполнения инструкций, приведенных в этом кратком руководстве, вам потребуется Azure CLI 2.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Предварительные требования

Вам потребуется войти в учетную запись с помощью команды [az login](/cli/azure/reference-index#az-login). Обратите внимание на свойство **ID** , которое ОТНОСИТСЯ к **идентификатору подписки** для учетной записи Azure.

```azurecli-interactive
az login
```

Выберите конкретную подписку вашей учетной записи, выполнив команду [az account set](/cli/azure/account#az-account-set). Запишите значение **идентификатора** из выходных данных **имени входа AZ** , чтобы использовать его в качестве значения аргумента **Subscription** в команде. Если вы используете несколько подписок, выберите соответствующую, в которой за ресурс будет взиматься плата. Чтобы отобразить все ваши подписки, выполните команду [az account list](/cli/azure/account#az-account-list).

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-azure-database-for-mysql-flexible-server-using-cli"></a>Создание базы данных Azure для гибкого сервера MySQL с помощью интерфейса командной строки
С помощью команды можно `az mysql flexible-server` создать гибкий сервер с *закрытым доступом (интеграция с виртуальной сетью)*. Эта команда использует частный доступ (интеграция с виртуальной сетью) в качестве метода подключения по умолчанию. Виртуальная сеть и подсеть будут созданы, если они не указаны. Вы также можете предоставить уже существующую виртуальную сеть и подсеть с помощью идентификатора подсети. <!-- You can provide the **vnet**,**subnet**,**vnet-address-prefix** or**subnet-address-prefix** to customize the virtual network and subnet.--> Существует несколько вариантов создания гибкого сервера с помощью интерфейса командной строки, как показано в примерах ниже.

>[!Important]
> С помощью этой команды подсеть делегируется **Microsoft. дбформискл/флексиблесерверс**. Это делегирование означает, что только гибкие серверы Базы данных Azure для MySQL могут использовать эту подсеть. Другие типы ресурсов Azure не могут быть делегированы подсети.
>

Полный список настраиваемых параметров интерфейса командной строки см. в [справочной документации](/cli/azure/mysql/flexible-server) по Azure CLI. Например, в приведенных ниже командах можно дополнительно указать группу ресурсов.

- Создание гибкого сервера с использованием виртуальной сети по умолчанию, подсети с префиксом адреса по умолчанию
    ```azurecli-interactive
    az mysql flexible-server create
    ```
- Создайте гибкий сервер, используя уже существующую виртуальную сеть и подсеть. Если указанная виртуальная сеть и подсеть не существует, будет создана виртуальная сеть и подсеть с префиксом адреса по умолчанию.
    ```azurecli-interactive
    az mysql flexible-server create --vnet myVnet --subnet mySubnet
    ```

- Создайте гибкий сервер, используя уже существующую виртуальную сеть, подсеть и используя идентификатор подсети. В указанной подсети не должно быть развернутого другого ресурса, и эта подсеть будет делегирована в **Microsoft. дбформискл/флексиблесерверс**, если она еще не делегирована.
    ```azurecli-interactive
    az mysql flexible-server create --subnet /subscriptions/{SubID}/resourceGroups/{ResourceGroup}/providers/Microsoft.Network/virtualNetworks/{VNetName}/subnets/{SubnetName}
    ```
    > [!Note]
    > Виртуальная сеть и подсеть должны находиться в том же регионе и подписке, что и гибкий сервер.
<
- Создайте гибкий сервер, используя новую виртуальную сеть, подсеть с префиксом адреса не по умолчанию.
    ```azurecli-interactive
    az mysql flexible-server create --vnet myVnet --address-prefixes 10.0.0.0/24 --subnet mySubnet --subnet-prefixes 10.0.0.0/24
    ```
Полный список настраиваемых параметров интерфейса командной строки см. в [справочной документации](/cli/azure/mysql/flexible-server) по Azure CLI.


## <a name="next-steps"></a>Следующие шаги
- Дополнительные сведения о работе [в сети в базе данных Azure для гибкого сервера MySQL](./concepts-networking.md).
- [Создание виртуальной сети для Гибкого сервера Базы данных Azure для MySQL и управление ею с помощью портала Azure](./how-to-manage-virtual-network-portal.md).
- Узнайте больше о [базе данных Azure для гибкого сервера виртуальной сети MySQL](./concepts-networking.md#private-access-vnet-integration).