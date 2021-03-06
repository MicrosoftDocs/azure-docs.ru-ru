---
title: Использование частных конечных точек для безопасного доступа к зрения
description: В этой статье описывается, как настроить закрытую конечную точку для учетной записи зрения.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 03/02/2021
ms.openlocfilehash: 09fa10e7f7751321601c5c4871b2cf36ccf6f01f
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104720937"
---
# <a name="use-private-endpoints-for-your-purview-account"></a>Использование частных конечных точек для учетной записи зрения

Вы можете использовать частные конечные точки для учетных записей зрения, чтобы клиенты и пользователи в виртуальной сети могли безопасно обращаться к каталогу по частному каналу. Частная конечная точка использует IP-адрес из адресного пространства виртуальной сети для вашей учетной записи зрения. Сетевой трафик между клиентами в виртуальной сети и учетной записью зрения проходит через виртуальную сеть и частный канал в магистральной сети Майкрософт, что устраняет уязвимость в общедоступном Интернете.

## <a name="create-purview-account-with-a-private-endpoint"></a>Создание учетной записи зрения с закрытой конечной точкой

1. Перейдите к [портал Azure](https://portal.azure.com) , а затем к своей учетной записи зрения.

1. Заполните основные сведения и задайте для параметра способ подключения значение частная конечная точка на вкладке **Сетевые подключения** . Настройте частные конечные точки приема, предоставив сведения о **подписке, виртуальной сети и подсетях** , которые необходимо связать с частной конечной точкой.

    > [!NOTE]
    > Создайте частную конечную точку приема, только если планируется включить сетевую изоляцию для сквозных сценариев сканирования как для Azure, так и для локальных источников. В настоящее время не поддерживаются частные конечные точки приема, работающие с источниками AWS.

    :::image type="content" source="media/catalog-private-link/create-pe-azure-portal.png" alt-text="Создание частной конечной точки в портал Azure":::

1. При необходимости можно также настроить **Частная зона DNS зону** для каждой частной конечной точки приема.

1. Нажмите кнопку Добавить, чтобы добавить закрытую конечную точку для учетной записи зрения.

1. На странице Создание частной конечной точки задайте для параметра зрения подресурса значение **учетная запись**, выберите виртуальную сеть и подсеть и выберите зону частная зона DNS, в которой будет ЗАРЕГИСТРИРОВАНА служба DNS (можно также использовать собственные DNS-серверы или создать записи DNS с помощью файлов узлов на виртуальных машинах).

    :::image type="content" source="media/catalog-private-link/create-pe-account.png" alt-text="Параметры создания частной конечной точки":::

1. Щелкните **ОК**.

1. Выберите **Review + create** (Просмотреть и создать). Вы будете перенаправлены на страницу Просмотр и создание, где Azure проверит вашу конфигурацию.

1. При появлении сообщения о том, что проверка пройдена, выберите **Создать**.

    :::image type="content" source="media/catalog-private-link/validation-passed.png" alt-text="Проверка, пройденная для создания учетной записи":::

## <a name="create-a-private-endpoint-for-the-purview-web-portal"></a>Создание частной конечной точки для веб-портала зрения

1. Перейдите к только что созданной учетной записи зрения, выберите подключения частной конечной точки в разделе Параметры.

1. Щелкните + частная конечная точка, чтобы создать новую закрытую конечную точку.

    :::image type="content" source="media/catalog-private-link/pe-portal.png" alt-text="Создание закрытой конечной точки портала":::

1. Введите Основные сведения.

1. На вкладке ресурс выберите тип ресурса **Microsoft. зрения/Accounts**.

1. Выберите ресурс, который будет только что созданной учетной записью зрения, и выберите целевой подресурс для **портала**.
    :::image type="content" source="media/catalog-private-link/pe-portal-details.png" alt-text="Сведения для частной конечной точки портала":::

1. Выберите зону "Виртуальная сеть и Частная зона DNS" на вкладке "Конфигурация". Перейдите на страницу Сводка и нажмите кнопку **создать** , чтобы создать частную конечную точку портала.

## <a name="enabling-access-to-azure-active-directory"></a>Включение доступа к Azure Active Directory

> [!NOTE]
> Если ваша виртуальная машина, VPN-шлюз или шлюз пиринга виртуальной сети имеет общий доступ к Интернету, он может получить доступ к зрения порталу и учетной записи зрения, включенной с частными конечными точками, и не нужно следовать остальным приведенным ниже инструкциям. Если в частной сети правила группы безопасности сети настроены на запрет всего общего трафика Интернета, необходимо добавить правила для включения доступа AAD. Чтобы сделать это, следуйте приведенным ниже инструкциям.

Приведенные ниже инструкции предназначены для безопасного доступа к зрения из виртуальной машины Azure. Аналогичные действия необходимо выполнить при использовании VPN или других шлюзов пиринга виртуальной сети.

1. Перейдите к виртуальной машине в портал Azure, выберите вкладку "сеть" в разделе "Параметры". Затем выберите правила исходящего порта и щелкните Добавить правило исходящего порта.

   :::image type="content" source="media/catalog-private-link/outbound-rule-add.png" alt-text="Добавление исходящего правила":::

2. В колонке Добавление правила исходящего порта выберите *назначение* в качестве тега службы, тег целевой службы — **AzureActiveDirectory**, диапазоны портов назначения — *, действие — разрешить, **приоритет должен быть выше, чем правило, которое запрещает весь Интернет-трафик**. Создайте правило.

   :::image type="content" source="media/catalog-private-link/outbound-rule-details.png" alt-text="Добавление сведений о исходящем правиле":::

3. Выполните те же действия, чтобы создать другое правило, разрешающее тег службы "**AzureResourceManager**". Если необходимо получить доступ к портал Azure, можно также добавить правило для тега службы "*AzurePortal*".

4. Подключитесь к виртуальной машине, откройте браузер, перейдите в консоль браузера (Ctrl + Shift + J) и перейдите на вкладку "сеть", чтобы отслеживать сетевые запросы. Введите web.purview.azure.com в поле URL-адрес и попробуйте войти с использованием учетных данных AAD. Возможно, вход в систему не будет выполнен, и на вкладке "сеть" в консоли вы увидите, что AAD пытается получить доступ к aadcdn.msauth.net, но не получает блокировки.

   :::image type="content" source="media/catalog-private-link/login-fail.png" alt-text="Сведения о сбое входа":::

5. В этом случае откройте командную строку на виртуальной машине и проверьте связь с этим URL-адресом (aadcdn.msauth.net), получите его IP, а затем добавьте правило исходящего порта для IP-адреса в правилах сетевой безопасности виртуальной машины. Задайте для параметра Назначение значение IP-адрес и задайте для IP-адресов назначения значение аадкдн. Из-за подсистемы балансировки нагрузки и диспетчера трафика IP-адрес AAD CDN может быть динамическим. После получения его IP-адреса лучше добавить его в файл узла виртуальной машины, чтобы принудительно посетить этот IP-адрес в браузере, чтобы получить CDN AAD.

   :::image type="content" source="media/catalog-private-link/ping.png" alt-text="Проверка связи":::

   :::image type="content" source="media/catalog-private-link/aadcdn-rule.png" alt-text="Правило AAD CDN":::

6. Когда новое правило будет создано, вернитесь к виртуальной машине и повторите попытку входа с помощью учетных данных AAD. Если вход выполнен, зрения Portal готов к использованию. Но в некоторых случаях AAD перейдет в другие домены для входа на основе типа учетной записи клиента. Например, для учетной записи live.com AAD перейдет в live.com для входа в систему, после чего эти запросы будут заблокированы снова. Для учетных записей сотрудников Microsoft AAD будет получать доступ к msft.sts.microsoft.com для получения сведений для входа. Проверьте сетевые запросы на вкладке "сети браузера", чтобы узнать, какие запросы домена блокируются, повторить предыдущий шаг, чтобы получить его IP-адрес и добавить правила исходящего порта в группу безопасности сети, чтобы разрешить запросы для этого IP-адреса (если это возможно, добавьте URL-адрес и файл узла виртуальной машины, чтобы исправить разрешение DNS). Если вы знакомы с диапазонами IP-адресов домена с точным именем, вы также можете напрямую добавить их в правила сети.

7. Теперь вход в AAD должен быть успешным. Портал зрения будет успешно загружен, но список всех учетных записей зрения работать не будет, так как он сможет получить доступ только к определенной учетной записи зрения. Введите *Web. зрения. Azure. com/Resource/{пурвиеваккаунтнаме}* , чтобы напрямую посетить учетную запись зрения, для которой успешно настроена частная конечная точка.
 
## <a name="ingestion-private-endpoints-and-scanning-sources-in-private-networks-vnets-and-behind-private-endpoints"></a>Частные конечные точки приема и сканирование источников в частных сетях, виртуальных сетей и за частные конечные точки

Если вы хотите обеспечить сетевую изоляцию для метаданных, передаваемых из источника, который сканируется в зрения Датамап, то необходимо выполнить следующие действия.
1. Включите **закрытую конечную точку приема** , выполнив действия, описанные в [этом](#creating-an-ingestion-private-endpoint) разделе.
1. Сканирование источника с помощью **автономного IR**.
 
    1. Все локальные типы источников, такие как SQL Server, Oracle, SAP и другие, в настоящее время поддерживаются только с помощью локальных сканирований на основе IR. Локальная IR должна выполняться в частной сети и затем быть соединена с виртуальной сетью в Azure. Затем необходимо включить виртуальную сеть Azure в закрытой конечной точке приема, [выполнив следующие действия.](#creating-an-ingestion-private-endpoint) 
    1. Для всех типов источников **Azure** , таких как хранилище BLOB-объектов Azure, база данных SQL Azure и другие, необходимо явно выбрать Запуск проверки с помощью локальной среды IR, чтобы обеспечить сетевую изоляцию. Выполните действия, описанные [здесь](manage-integration-runtimes.md) , чтобы настроить локальное IR-соединение. Затем настройте проверку в источнике Azure, выбрав ее в раскрывающемся списке **подключить через среду выполнения интеграции** , чтобы обеспечить сетевую изоляцию. 
    
    :::image type="content" source="media/catalog-private-link/shir-for-azure.png" alt-text="Выполнение проверки Azure с помощью локальной среды IR":::

> [!NOTE]
> Сейчас мы не поддерживаем метод учетных данных MSI при сканировании источников Azure с помощью локальной среды IR. Для этого источника Azure необходимо использовать один из других поддерживаемых методов учетных данных.

## <a name="enable-private-endpoint-on-existing-purview-accounts"></a>Включить закрытую конечную точку в существующих учетных записях зрения

После создания учетной записи зрения можно добавить частные конечные точки зрения двумя способами:

- Использование портал Azure (учетная запись зрения)
- Использование частного центра ссылок

### <a name="using-the-azure-portal-purview-account"></a>Использование портал Azure (учетная запись зрения)

1. Перейдите к учетной записи зрения из портал Azure, выберите подключения к частной конечной точке в разделе " **сети** " **параметров**.

    :::image type="content" source="media/catalog-private-link/pe-portal.png" alt-text="Создать закрытую конечную точку учетной записи":::

1. Щелкните + частная конечная точка, чтобы создать новую закрытую конечную точку.

1. Введите Основные сведения.

1. На вкладке ресурс выберите тип ресурса **Microsoft. зрения/Accounts**.

1. Выберите ресурс в качестве учетной записи зрения и выберите целевой подресурс для **учетной записи**.

1. Выберите зону " **Виртуальная сеть** и **Частная зона DNS** " на вкладке "Конфигурация". Перейдите на страницу Сводка и нажмите кнопку **создать** , чтобы создать частную конечную точку портала.

> [!NOTE]
> Необходимо выполнить те же действия, что и для целевого подресурса, выбранного в качестве **портала** .

#### <a name="creating-an-ingestion-private-endpoint"></a>Создание закрытой конечной точки приема

1. Перейдите к учетной записи зрения из портал Azure, выберите подключения к частной конечной точке в разделе " **сети** " **параметров**.
1. Перейдите на вкладку **подключения к частной конечной точке приема** и щелкните **+ создать** , чтобы создать новую закрытую конечную точку приема.

1. Укажите основные сведения и сведения о виртуальной сети.
 
    :::image type="content" source="media/catalog-private-link/ingestion-pe-fill-details.png" alt-text="Сведения о заполнении закрытой конечной точки":::

1. Нажмите кнопку **создать** , чтобы завершить настройку.

> [!NOTE]
> Частные конечные точки приема можно создать только с помощью зрения портал Azure описанной выше возможности. Его нельзя создать из частного центра ссылок.

### <a name="using-the-private-link-center"></a>Использование частного центра ссылок

1. Перейдите на [портал Azure](https://portal.azure.com).

2. На панели поиска в верхней части страницы найдите "Частная ссылка" и перейдите к колонке "Частная ссылка", щелкнув первый вариант.

3. Щелкните "+ Добавить" и заполните основные сведения.

   :::image type="content" source="media/catalog-private-link/private-link-center.png" alt-text="Создание PE из частного центра ссылок":::

4. Выберите ресурс, для которого уже создана учетная запись зрения, и выберите целевой подресурс для **учетной записи**.

5. Выберите зону "Виртуальная сеть и Частная зона DNS" на вкладке "Конфигурация". Перейдите на страницу Сводка и нажмите кнопку **создать** , чтобы создать закрытую конечную точку учетной записи.

> [!NOTE]
> Необходимо выполнить те же действия, что и для целевого подресурса, выбранного в качестве **портала** .

## <a name="firewalls-to-restrict-public-access"></a>Брандмауэры для ограничения общего доступа

Чтобы окончательно отключить доступ к учетной записи зрения из общедоступного Интернета, выполните следующие действия. Этот параметр будет применяться как к частным конечным точкам, так и к частным подключениям к частной конечной точке приема.

1. Перейдите к учетной записи зрения из портал Azure, выберите подключения к частной конечной точке в разделе " **сети** " **параметров**.
1. Перейдите на вкладку Брандмауэр и убедитесь, что переключатель установлен в положение **запретить**.

    :::image type="content" source="media/catalog-private-link/private-endpoint-firewall.png" alt-text="Параметры брандмауэра частной конечной точки":::

## <a name="next-steps"></a>Следующие шаги

- [Обзор каталога данных Azure зрения](how-to-browse-catalog.md)

- [Поиск по каталогу данных Azure Purview](how-to-search-catalog.md)
