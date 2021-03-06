---
title: Высокий уровень доступности в Azure Cosmos DB
description: В этой статье описывается, как Azure Cosmos DB обеспечивает высокий уровень доступности
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/05/2021
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: fd704d45aa7dc10835a205f12ce26fc01a7ea44f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104584505"
---
# <a name="how-does-azure-cosmos-db-provide-high-availability"></a>Как Azure Cosmos DB обеспечивают высокую доступность
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB обеспечивает высокий уровень доступности двумя основными способами. Во-первых, Azure Cosmos DB реплицирует данные между регионами, настроенными в учетной записи Cosmos. Во-вторых, Azure Cosmos DB поддерживает 4 реплики данных в пределах региона.

Azure Cosmos DB является глобально распределенной службой базы данных и является базовой службой, доступной во [всех регионах, где доступна служба Azure](https://azure.microsoft.com/global-infrastructure/services/?products=cosmos-db&regions=all). Вы можете связать любое количество регионов Azure с учетной записью Azure Cosmos, и ваши данные будут автоматически и прозрачно реплицированы. Добавить или удалить регион в учетной записи Azure Cosmos можно в любое время. Cosmos DB доступна для клиентов в пяти различных облачных средах Azure:

* **Общедоступный пиринг Azure** — облако, которое доступно по всему миру.

* Версия **Azure для Китая (21vianet** ) доступна с помощью уникальной связи между корпорацией Майкрософт и 21vianet, одним из крупнейших поставщиков услуг Интернета в Китае.

* **Azure для Германии** предоставляет службы в рамках модели доверенного лица для работы с данными, что гарантирует, что данные клиента остаются в Германии под контролем международных GmbH на основе T-Systems, подразделении подразделение Telekom, выступающей в качестве доверенного лица для работы с данными в Германии.

* **Azure для государственных организаций** — доступна в четырех регионах в Соединенных Штатах для государственных организаций США и их партнеров.

* Службы **Azure для государственных организаций в области защиты (DoD)** доступны в двух регионах США в Отдел обороны США.

В пределах региона Azure Cosmos DB поддерживает четыре копии данных в качестве реплик в физических секциях, как показано на следующем рисунке:

:::image type="content" source="./media/high-availability/cosmosdb-data-redundancy.png" alt-text="Физическое секционирование" border="false":::

* Данные в контейнерах Azure Cosmos [горизонтально секционированы](partitioning-overview.md).

* Набор секций — это коллекция из нескольких наборов реплик. Каждая секция в каждом регионе защищена с помощью набора реплик. При этом все записи реплицируются и надежно фиксируются большей частью реплик. Реплики распределены между 10–20 доменами сбоя.

* Все секции реплицируются во всех регионах. Каждый регион содержит все секции данных контейнера Azure Cosmos и может обслуживать операции чтения, а также обслуживать операции записи при включенной записи в нескольких регионах.  

Если ваша учетная запись Azure Cosmos распределена между *n* регионами Azure, будет существовать по крайней мере *n* копий всех данных. Наличие учетной записи Azure Cosmos в более чем 2 регионах повышает доступность приложения и обеспечивает низкую задержку в связанных регионах.

## <a name="slas-for-availability"></a>Соглашения об уровне обслуживания для доступности

Azure Cosmos DB предоставляет комплексные соглашения об уровне обслуживания, которые охватывают пропускную способность, задержку на 99-м процентиль, согласованность и высокий уровень доступности. В таблице ниже приведены гарантии высокого уровня доступности, предоставляемые Azure Cosmos DB для учетных записей одного и нескольких регионов. Для повышения доступности записи настройте учетную запись Azure Cosmos для работы с несколькими регионами записи.

|Operation type (Тип операции)  | Одна область |Несколько регионов (операций записи в одну область)|Несколько регионов (несколько регионов записи) |
|---------|---------|---------|-------|
|Запись    | 99,99    |99,99   |99,999|
|Операции чтения     | 99,99    |99,999  |99,999|

> [!NOTE]
> На практике фактическая доступность для записи ограниченного устаревания, сеанса, согласованного префикса и модели согласованности в конечном итоге значительно выше, чем опубликованные соглашения об уровне обслуживания. Фактическая доступность чтения для всех уровней согласованности значительно выше, чем в опубликованных Соглашениях об уровне обслуживания.

## <a name="high-availability-with-azure-cosmos-db-in-the-event-of-regional-outages"></a>Высокий уровень доступности с Azure Cosmos DB в случае региональных простоев

В редких случаях нестандартного сбоя Azure Cosmos DB гарантирует высокую доступность базы данных. В зависимости от конфигурации учетной записи Azure Cosmos в процессе сбоя захватывается следующее поведение Azure Cosmos DB.

* Если Azure Cosmos DB, то перед тем как операция записи будет подтверждена клиенту, данные надежно фиксируются кворумом реплик в пределах региона, принимающего операции записи. Дополнительные сведения см. в разделе [уровни согласованности и пропускная способность](./consistency-levels.md#consistency-levels-and-throughput) .

* Учетные записи, связанные с несколькими регионами, настроенные на использование нескольких регионов записи, будут сохранять высокую доступность как для операций записи, так и для операций чтения. Региональные отработки отказа обнаруживаются и обрабатываются в клиенте Azure Cosmos DB. Они также мгновенно и не нуждаются в изменениях в приложении.

* Учетные записи с одним регионом могут потерять доступность после регионального сбоя. Всегда рекомендуется настроить по крайней **мере два региона** (желательно, по крайней мере, два региона записи) с учетной записью Azure Cosmos, чтобы обеспечить высокий уровень доступности в любое время.

### <a name="multi-region-accounts-with-a-single-write-region-write-region-outage"></a>Учетные записи в нескольких регионах с одним регионом для записи (сбой области записи)

* Во время сбоя в регионе записи учетная запись Azure Cosmos автоматически сделает дополнительный регион новым основным регионом записи при **включении автоматической отработки отказа** в учетной записи Azure Cosmos. Если этот параметр включен, отработка отказа выполняется в другом регионе в порядке указанного приоритета региона.

* Обратите внимание, что отработка отказа вручную не должна быть активирована и не будет выполнена в случае сбоя исходного или целевого региона. Это обусловлено проверкой согласованности, необходимой для процедуры отработки отказа, которая требует подключения между регионами.

* Когда ранее затронутый регион возвращается в режим «в сети», любые данные записи, которые не были реплицированы при сбое региона, становятся доступными через [канал конфликтов](how-to-manage-conflicts.md#read-from-conflict-feed). Приложения могут читать канал конфликтов, разрешать конфликты на основе логики конкретного приложения и записывать обновленные данные обратно в контейнер Azure Cosmos, если это необходимо.

* После восстановления ранее затронутый регион записи становится автоматически доступным как регион чтения. Вы можете вернуться к восстановленному региону в качестве региона записи. Регионы можно переключать с помощью [PowerShell, Azure CLI или портал Azure](how-to-manage-database-account.md#manual-failover). **Нет данных или потерь доступности** до, во время или после переключения региона записи, и ваше приложение по-своему постоянно доступно.

> [!IMPORTANT]
> Настоятельно рекомендуется настроить учетные записи Cosmos для Azure, используемые для рабочих нагрузок, чтобы **включить автоматический переход на другой ресурс**. Это позволяет Cosmos DB автоматически отработки отказа баз данных учетных записей в критический регионах. В отсутствие этой конфигурации учетная запись будет испытывать потерю доступности записи во время непростоя области записи, так как переход на другой ресурс вручную не будет выполнен из-за отсутствия подключения к региону.

### <a name="multi-region-accounts-with-a-single-write-region-read-region-outage"></a>Учетные записи в нескольких регионах с одним регионом для записи (сбой области чтения)

* Во время сбоя в регионе чтения учетные записи Azure Cosmos, использующие любой уровень согласованности или строгую согласованность с тремя или более регионами чтения, будут оставаться в высокой доступности для операций чтения и записи.

* Учетные записи Cosmos Azure, использующие строгую согласованность с тремя регионами (одна запись, два чтения), будут поддерживать доступность записи во время сбоя в регионе чтения. Для учетных записей с двумя регионами и автоматической отработкой отказа учетная запись прекращает принимать операции записи до тех пор, пока регион не будет помечен как сбой и происходит автоматический переход на другой ресурс.

* Затронутый регион автоматически отключается и будет помечен в автономном режиме. [Azure Cosmos DB пакеты SDK](sql-api-sdk-dotnet.md) будут перенаправлять вызовы чтения в следующий доступный регион в списке предпочтительных регионов.

* Если ни один из регионов в списке предпочтительных регионов не доступен, вызовы автоматически будут перенаправлены к текущему региону записи.

* Для обработки сбоя региона чтения изменения в коде приложения не требуются. Когда затронутый регион чтения возвращается в оперативный режим, он автоматически синхронизируется с текущим регионом записи и снова станет доступен для обслуживания запросов на чтение.

* Последующие операции чтения перенаправляются в восстановленный регион без каких-либо изменений в коде приложения. Во время отработки отказа и повторного присоединение к ранее неисправному региону проверки согласованности чтения продолжают обрабатываться Azure Cosmos DB.

* Даже в редких и тех случаях, когда регион Azure постоянно Неустранимая, нет потерь данных, если ваша учетная запись Azure Cosmos в нескольких регионах настроена со *строгой* согласованностью. В случае постоянного Неустранимая региона записи в многорегионовую учетную запись Azure Cosmos, настроенную на согласованность с ограниченной устойчивостью, окно потенциальной потери данных ограничено окном устаревания (*k* или *t*), где K = 100000 обновлений или t = 5 минут, которые когда-либо происходят первыми. Для уровней согласованности в сеансах с согласованным префиксом и в конечном итоге размер окна потенциальной потери данных ограничен 15 минутами. Дополнительные сведения о целевых объектах RTO и RPO для Azure Cosmos DB см. в статье [уровни согласованности и устойчивость данных](./consistency-levels.md#rto) .

## <a name="availability-zone-support"></a>Поддержка зоны доступности

Помимо устойчивости между регионами, Azure Cosmos DB также поддерживает **избыточность зоны** в поддерживаемых регионах при выборе региона, связываемого с учетной записью Azure Cosmos.

Благодаря поддержке зоны доступности (AZ) Azure Cosmos DB обеспечит помещение реплик в несколько зон в пределах заданного региона, чтобы обеспечить высокую доступность и устойчивость к зональные сбоям. Зоны доступности предоставить соглашение об уровне обслуживания 99,995%, не изменяя задержки. В случае сбоя одной зоны избыточность зоны обеспечивает полную устойчивость данных с RPO = 0 и доступ с RTO = 0. Избыточность зоны — это дополнительная возможность для региональной репликации. Для обеспечения устойчивости в региональной зоне нельзя использовать только избыточность зоны.

Избыточность зоны может быть настроена только при добавлении нового региона в учетную запись Azure Cosmos. Для существующих регионов можно включить избыточность зоны, удалив регион, а затем добавив ее обратно с включенной избыточностью зоны. Для учетной записи одного региона требуется добавить один дополнительный регион для временной отработки отказа в, а затем удалить и добавить нужный регион с включенной избыточностью зоны.

При настройке операций записи в несколько регионов для учетной записи Azure Cosmos вы можете использовать избыточность зоны без дополнительных затрат. В противном случае ознакомьтесь со следующей таблицей, в которой приведены цены на поддержку избыточности зоны. Список регионов, в которых доступны зоны доступности, см. в разделе [зоны доступности](../availability-zones/az-region.md).

В следующей таблице перечислены возможности высокой доступности различных конфигураций учетных записей.

|КПЭ|Одна область без AZs|Одна область с AZs|Операции записи в несколько регионов, одна область с AZs|Несколько регионов, запись в несколько регионов с помощью AZs|
|---------|---------|---------|---------|---------|
|Соглашение об уровне обслуживания для доступности записи | 99,99 % | 99,995% | 99,995% | 99,999 % |
|Соглашение об уровне обслуживания для чтения  | 99,99 % | 99,995% | 99,995% | 99,999 % |
|Ошибки зоны — потери данных | Потеря данных | Без потери данных | Без потери данных | Без потери данных |
|Сбои зоны — доступность | Потери доступности | Без потерь доступности | Без потерь доступности | Без потерь доступности |
|Региональный сбой — потери данных | Потеря данных |  Потеря данных | Зависит от уровня согласованности. Дополнительные сведения см. в статье о [согласованности, доступности и производительности](./consistency-levels.md) . | Зависит от уровня согласованности. Дополнительные сведения см. в статье о [согласованности, доступности и производительности](./consistency-levels.md) .
|Региональный сбой — доступность | Потери доступности | Потери доступности | Нет потерь доступности для сбоя региона чтения, временная ошибка для области записи | Без потерь доступности |
|Цена (***1** _) | Н/Д | Скорость подготовки единиц запросов в секунду (x 1,25) | Скорость подготовки единиц запросов в секунду x 1,25 (_ *_2_* *) | Скорость записи в нескольких регионах |

***1*** для единиц запросов к бессерверным учетным ЗАПИСЯМ (ru) умножается на 1,25.

***2*** 1,25 Rate применяется только к тем регионам, в которых включено AZ.

Зоны доступности можно включить через:

* [Портал Azure](how-to-manage-database-account.md#addremove-regions-from-your-database-account)

* [Azure PowerShell](manage-with-powershell.md#create-account)

* [Azure CLI](manage-with-cli.md#add-or-remove-regions)

* [Шаблоны диспетчера ресурсов Azure](./manage-with-templates.md)

## <a name="building-highly-available-applications"></a>Создание высокодоступных приложений

* Изучите ожидаемое [поведение пакетов SDK Azure Cosmos](troubleshoot-sdk-availability.md) во время этих событий и какие конфигурации влияют на нее.

* Чтобы обеспечить высокую доступность для записи и чтения, настройте учетную запись Azure Cosmos, чтобы она занимала как минимум два региона с несколькими регионами записи. Эта конфигурация обеспечит наивысшую доступность, наименьшую задержку и лучшую масштабируемость для операций чтения и записи по соглашениям об уровне обслуживания. Дополнительные сведения см. в статье [Настройка учетной записи Azure Cosmos с несколькими регионами записи](tutorial-global-distribution-sql-api.md).

* Для учетных записей Azure Cosmos с несколькими регионами, настроенных с помощью региона с одной записью, [включите автоматическую отработку отказа с помощью Azure CLI или портал Azure](how-to-manage-database-account.md#automatic-failover). При включенной автоматической отработке отказа в случае регионального сбоя Cosmos DB будет автоматически выполнять отработку отказа вашей учетной записи.  

* Даже если ваша учетная запись Azure Cosmos имеет высокую доступность, приложение может быть неправильно спроектировано для поддержания высокой доступности. Чтобы протестировать сквозную высокую доступность приложения, в ходе тестирования приложения или аварийного восстановления (DR) временно отключите автоматическую отработку отказа для учетной записи, запустите [отработку отказа вручную с помощью PowerShell, Azure CLI или портал Azure](how-to-manage-database-account.md#manual-failover), а затем Отслеживайте отработку отказа приложения. После завершения можно выполнить отработку отказа в основной регион и восстановить автоматическую отработку отказа для учетной записи.

> [!IMPORTANT]
> Не вызывайте отработку отказа вручную во время Cosmos DB сбоя в исходном или целевом регионе, так как для поддержания согласованности данных требуется регион, так как он требует регионов с подключением и не будет выполняться.

* В глобально распределенной среде базы данных существует прямая связь между уровнем согласованности и устойчивостью данных в случае сбоя уровня региона. При разработке плана обеспечения непрерывности бизнес-процессов нужно понимать, какое максимальное время до полного восстановления приложения после аварийного события допустимо. Время, необходимое приложению для полного восстановления, называется целевым временем восстановления (RTO). Необходимо также понимать, потерю какого максимального периода последних обновлений данных приложение может допустить при восстановлении после аварийного события. Этот период обновлений является целевой точкой восстановления (RPO). Значения RPO и RTO для Azure Cosmos DB см. в разделе [Уровни согласованности и устойчивость данных](./consistency-levels.md#rto).

## <a name="what-to-expect-during-a-region-outage"></a>Что следует предполагать во время сбоя региона

Для учетных записей с одним регионом клиенты будут испытывать утрату доступности чтения и записи.

В зависимости от следующей таблицы учетные записи с несколькими регионами будут работать по-разному.

| Регионы записи | автоматический переход на другой ресурс | Чего следует ожидать | Предпринимаемые действия |
| -- | -- | -- | -- |
| Регион с одной записью | Не включено | В случае сбоя в области чтения все клиенты будут перенаправлены в другие регионы. Без потери доступности чтения или записи. Потери данных нет. <p/> В случае сбоя в регионе записи клиенты будут испытывать потерю доступности при записи. Потери данных будут зависеть от выбранного уровня констистенци. <p/> Cosmos DB восстановит доступность записи автоматически по окончании сбоя. | Во время сбоя убедитесь, что в оставшихся регионах для поддержки трафика для чтения достаточно ресурсов. <p/> Не *запускайте* отработку отказа вручную во время сбоя, так как она не будет выполнена. <p/> В случае сбоя перенастройте подготовленную емкость соответствующим образом. |
| Регион с одной записью | Активировано | В случае сбоя в области чтения все клиенты будут перенаправлены в другие регионы. Без потери доступности чтения или записи. Потери данных нет. <p/> В случае сбоя в регионе записи клиенты будут испытывать потерю доступности, пока не Cosmos DB автоматически выбирает новый регион в качестве нового региона записи в соответствии с вашими предпочтениями. Потери данных будут зависеть от выбранного уровня констистенци. | Во время сбоя убедитесь, что в оставшихся регионах для поддержки трафика для чтения достаточно ресурсов. <p/> Не *запускайте* отработку отказа вручную во время сбоя, так как она не будет выполнена. <p/> Когда происходит сбой, вы можете восстановить нереплицированные данные в неисправном регионе из [веб-канала конфликтов](how-to-manage-conflicts.md#read-from-conflict-feed), переместить регион записи обратно в исходный регион и повторно настроить подготовленную емкость соответствующим образом. |
| Несколько регионов записи | Неприменимо | Без потери доступности чтения или записи. <p/> Данные теряются по мере выбора уровня согласованности. | Во время сбоя убедитесь, что в остальных регионах для поддержки дополнительного трафика подготовлена достаточная емкость. <p/> Когда происходит сбой, вы можете восстановить нереплицированные данные в неисправном регионе из [веб-канала конфликтов](how-to-manage-conflicts.md#read-from-conflict-feed) и повторно настроить подготовленную емкость соответствующим образом. |

## <a name="next-steps"></a>Дальнейшие действия

См. подробнее:

* [Достижение компромисса между доступностью и быстродействием для разных уровней согласованности](./consistency-levels.md)

* [Глобальное масштабирование подготовленной пропускной способности](./request-units.md)

* [Глобальное распределение (взгляд изнутри)](global-dist-under-the-hood.md)

* [Уровни согласованности для Azure Cosmos DB](consistency-levels.md)

* [Настройка учетной записи Cosmos с несколькими регионами записи](how-to-multi-master.md)

* [Поведение пакета SDK в многоязычных средах](troubleshoot-sdk-availability.md)