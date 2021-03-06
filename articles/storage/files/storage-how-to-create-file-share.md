---
title: Создание файлового ресурса Azure
titleSuffix: Azure Files
description: Как создать файловый ресурс Azure с помощью портал Azure, PowerShell или Azure CLI.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 1/20/2021
ms.author: rogarana
ms.subservice: files
ms.custom: devx-track-azurecli, references_regions
ms.openlocfilehash: 24bee926d84c7a5be3f19c39d39285c2cd486824
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102211028"
---
# <a name="create-an-azure-file-share"></a>Создание файлового ресурса Azure
Чтобы создать файловый ресурс Azure, необходимо ответить на три вопроса о том, как она будет использоваться:

- **Каковы требования к производительности для файлового ресурса Azure?**  
    Службы файлов Azure предлагают стандартные общие папки, размещенные на жестких дисках (на основе жесткого диска) и в общих файловых ресурсах уровня "Премиум", которые размещаются на дисках с твердотельным накопителем (на основе твердотельных накопителей).

- **Каковы требования к избыточности для файлового ресурса Azure?**  
    Стандартные общие папки предлагают локально избыточное (LRS), избыточное в зоне (ZRS), геоизбыточное (GRS) или геоизбыточное (ГЗРС) хранилище. Однако функция больших файловых ресурсов поддерживается только в локально избыточных файловых ресурсах и избыточных зонах. Файловые ресурсы уровня "Премиум" не поддерживают геоизбыточность.

    Общие файловые ресурсы уровня "Премиум" доступны с локально избыточностью и избыточностью зоны в подмножестве регионов. Чтобы узнать, доступны ли в вашем регионе общедоступные файловые ресурсы уровня "Премиум", см. страницу [доступность продуктов по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=storage) для Azure. Сведения о регионах, поддерживающих ZRS, см. в статье [избыточность службы хранилища Azure](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

- **Какой размер файлового ресурса требуется?**  
    В локальных и избыточных учетных записях хранения файловые ресурсы Azure могут охватывать до 100 Тиб, но в учетных записях хранения географических и геопоясов файловые ресурсы Azure могут включать не более 5 тиб. 

Дополнительные сведения об этих трех вариантах см. в статье [Планирование развертывания службы файлов Azure](storage-files-planning.md).

## <a name="prerequisites"></a>Предварительные требования
- В этой статье подразумевается, что вы уже создали подписку Azure. Если у вас еще нет подписки, вы можете [создать бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.
- Если вы хотите использовать Azure PowerShell, [установите последнюю версию](/powershell/azure/install-az-ps).
- Если вы хотите использовать Azure CLI, [установите последнюю версию](/cli/azure/install-azure-cli).

## <a name="create-a-storage-account"></a>Создание учетной записи хранения
Общие папки Azure развертываются в *учетных записях хранения*. Это объекты верхнего уровня, которые представляют общий пул хранилища. Этот пул хранилища можно использовать для развертывания нескольких файловых ресурсов. 

Azure поддерживает несколько типов учетных записей хранения для различных сценариев хранилища, которые могут иметь клиенты, но для службы файлов Azure существуют два основных типа учетных записей хранения. Тип учетной записи хранения, который необходимо создать, зависит от того, хотите ли вы создать стандартный файловый ресурс или общую папку Premium: 

- **Учетные записи хранения общего назначения версии 2.** Такие учетные записи хранения позволяют развертывать общие папки Azure на оборудовании на основе жестких дисков (HDD) уровня "Стандартный". Наряду с общими папками Azure такие учетные записи хранения поддерживают и другие ресурсы хранилища, включая контейнеры больших двоичных объектов, очереди и таблицы. Общие файловые ресурсы можно развернуть в оптимизированных для транзакции (по умолчанию), горячий или холодном уровнях.

- **Учетные записи хранения FileStorage.** Такие учетные записи хранения позволяют развертывать общие папки Azure на оборудовании на основе твердотельных накопителей (SSD) уровня "Премиум". Учетные записи FileStorage можно использовать только для хранения общих папок Azure. Другие ресурсы хранилища (контейнеры больших двоичных объектов, очереди, таблицы и т. д.) нельзя развертывать в учетной записи FileStorage.

# <a name="portal"></a>[Портал](#tab/azure-portal)
Чтобы создать учетную запись хранения с помощью портал Azure, выберите **+ создать ресурс** на панели мониторинга. В результирующем окне поиска Azure Marketplace найдите **учетную запись хранения** и выберите результирующий результат поиска. Это приводит к обзорной странице учетных записей хранения. Выберите **создать** , чтобы продолжить работу с мастером создания учетной записи хранения.

![Снимок экрана с параметром быстрого создания учетной записи хранения в браузере](media/storage-how-to-create-file-share/create-storage-account-0.png)

#### <a name="basics"></a>Основы
Первый раздел, который необходимо завершить для создания учетной записи хранения, помечен как **базовый**. В нем содержатся все необходимые поля для создания учетной записи хранения. Чтобы создать учетную запись хранения GPv2, убедитесь, что переключатель " **производительность** " установлен в значение " *стандартный* ", а в раскрывающемся списке " **тип учетной записи** " выбрано значение *StorageV2 (общего назначения версии 2)*.

![Снимок экрана с переключателем "производительность" со стандартным выбранным видом и типом учетной записи с выбранным StorageV2](media/storage-how-to-create-file-share/create-storage-account-1.png)

Чтобы создать учетную запись хранения Филестораже, убедитесь, что переключатель " **производительность** " имеет значение " *премиум* ", а в раскрывающемся списке " **тип учетной записи** " выбрано значение *филестораже*.

![Снимок экрана: переключатель "производительность" с выбранным Premium и типом учетной записи с выбранным Филестораже](media/storage-how-to-create-file-share/create-storage-account-2.png)

Другие основные поля не зависят от выбора учетной записи хранения.
- **Имя учетной записи хранения**: имя создаваемого ресурса учетной записи хранения. Это имя должно быть глобально уникальным, но в противном случае может потребоваться любое имя. Имя учетной записи хранения будет использоваться в качестве имени сервера при подключении общей папки Azure через SMB.
- **Location**— регион для учетной записи хранения, в которой будет выполнено развертывание. Это может быть регион, связанный с группой ресурсов, или любой другой доступный регион.
- **Репликация**. Хотя это и называется репликацией, это поле фактически означает **избыточность**; Это требуемый уровень избыточности: локальное избыточность (LRS), избыточность зоны (ZRS), геоизбыточность (GRS) и геоизбыточность геозоны (ГЗРС). В этом раскрывающемся списке также содержатся геоизбыточность с доступом для чтения (RA-GRS) и избыточность геозоны с доступом на чтение (RA-ГЗРС), которая не применяется к файловым ресурсам Azure. любая общая папка, созданная в учетной записи хранения с выбранными данными, будет иметь геоизбыточное значение или геоизбыточную зону соответственно. 

#### <a name="networking"></a>Сеть
Раздел "Сетевые подключения" позволяет настроить сетевые параметры. Эти параметры являются необязательными для создания учетной записи хранения и могут быть настроены позже при необходимости. Дополнительные сведения об этих параметрах см. в статье рекомендации по работе с [сетью в службе файлов Azure](storage-files-networking-overview.md).

#### <a name="data-protection"></a>Защита данных
Раздел Защита данных позволяет настроить политику обратимого удаления файловых ресурсов Azure в вашей учетной записи хранения. Другие параметры, связанные с обратимым удалением для больших двоичных объектов, контейнеров, восстановления на момент времени для контейнеров, управления версиями и веб-канала изменений, применяются только к хранилищу BLOB-объектов Azure.

#### <a name="advanced"></a>Продвинутый уровень
Раздел Advanced содержит несколько важных параметров для файловых ресурсов Azure.

- **Требуется безопасное перемещение**: это поле указывает, требуется ли для учетной записи хранения шифрование при передаче для обмена данными с учетной записью хранения. Если требуется поддержка SMB 2,1, необходимо отключить эту функцию.
- **Большие файловые ресурсы**. это поле включает учетную запись хранения для файловых ресурсов, охватывающих до 100 тиб. При включении этой функции учетная запись хранения будет ограничена только локально избыточными и избыточными параметрами хранения в пределах зоны. После включения учетной записи хранения GPv2 для больших файловых ресурсов невозможно отключить функцию большого общего файлового ресурса. Учетные записи хранения Филестораже (учетные записи хранения для файловых ресурсов уровня "Премиум") не имеют этого параметра, так как все файловые ресурсы уровня "Премиум" могут масштабироваться до 100 тиб. 

![Снимок экрана с важными дополнительными параметрами, которые применяются к службе файлов Azure](media/storage-how-to-create-file-share/create-storage-account-3.png)

Другие параметры, доступные на вкладке Дополнительно (иерархическое пространство имен для Azure Data Lake хранилище Gen 2, уровень больших двоичных объектов по умолчанию, NFSv3 для хранилища BLOB-объектов и т. д.), не применяются к службе файлов Azure.

> [!Important]  
> Выбор уровня доступа к большому двоичному объекту не влияет на уровень файлового ресурса.

#### <a name="tags"></a>Теги
Теги — это пары имя — значение, которые можно назначать различным ресурсам и группам ресурсов для их категоризации и консолидированного отображения счетов. Они необязательны и могут быть применены после создания учетной записи хранения.

#### <a name="review--create"></a>Просмотр и создание
Последним шагом для создания учетной записи хранения является нажатие кнопки **создать** на вкладке **Проверка и создание** . Эта кнопка будет недоступна, если не заполнены все обязательные поля для учетной записи хранения.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Чтобы создать учетную запись хранения с помощью PowerShell, мы будем использовать `New-AzStorageAccount` командлет. У этого командлета есть множество параметров. отображаются только необходимые параметры. Дополнительные сведения о дополнительных параметрах см. в [ `New-AzStorageAccount` документации по командлетам](/powershell/module/az.storage/new-azstorageaccount).

Чтобы упростить создание учетной записи хранения и последующего файлового ресурса, в переменных будут храниться несколько параметров. Вы можете заменить содержимое переменной любыми значениями, однако обратите внимание, что имя учетной записи хранения должно быть глобально уникальным.

```powershell
$resourceGroupName = "myResourceGroup"
$storageAccountName = "mystorageacct$(Get-Random)"
$region = "westus2"
```

Чтобы создать учетную запись хранения, способную хранить стандартные общие файловые ресурсы Azure, мы будем использовать следующую команду. `-SkuName`Параметр относится к требуемому типу избыточности. Если требуется геоизбыточная учетная запись хранения, избыточная или геозона, необходимо также удалить `-EnableLargeFileShare` параметр.

```powershell
$storAcct = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Standard_LRS `
    -Location $region `
    -Kind StorageV2 `
    -EnableLargeFileShare
```

Чтобы создать учетную запись хранения, способную хранить файловые ресурсы Azure уровня "Премиум", мы будем использовать следующую команду. Обратите внимание, что `-SkuName` параметр изменился так, чтобы `Premium` он включал и, и требуемый уровень избыточности локально избыточного ( `LRS` ). `-Kind`Параметр имеет значение, `FileStorage` `StorageV2` так как общие файловые ресурсы уровня "Премиум" должны создаваться в учетной записи хранения филестораже, а не в учетной записи хранения GPv2.

```powershell
$storAcct = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Premium_LRS `
    -Location $region `
    -Kind FileStorage 
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Чтобы создать учетную запись хранения с помощью Azure CLI, мы будем использовать команду AZ Storage Account Create. Эта команда имеет много параметров. отображаются только необходимые параметры. Дополнительные сведения о дополнительных параметрах см. в [ `az storage account create` документации по командам](/cli/azure/storage/account).

Чтобы упростить создание учетной записи хранения и последующего файлового ресурса, в переменных будут храниться несколько параметров. Вы можете заменить содержимое переменной любыми значениями, однако обратите внимание, что имя учетной записи хранения должно быть глобально уникальным.

```azurecli
resourceGroupName="myResourceGroup"
storageAccountName="mystorageacct$RANDOM"
region="westus2"
```

Чтобы создать учетную запись хранения, способную хранить стандартные общие файловые ресурсы Azure, мы будем использовать следующую команду. `--sku`Параметр относится к требуемому типу избыточности. Если требуется геоизбыточная учетная запись хранения, избыточная или геозона, необходимо также удалить `--enable-large-file-share` параметр.

```azurecli
az storage account create \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --kind StorageV2 \
    --sku Standard_ZRS \
    --enable-large-file-share \
    --output none
```

Чтобы создать учетную запись хранения, способную хранить файловые ресурсы Azure уровня "Премиум", мы будем использовать следующую команду. Обратите внимание, что `--sku` параметр изменился так, чтобы `Premium` он включал и, и требуемый уровень избыточности локально избыточного ( `LRS` ). `--kind`Параметр имеет значение, `FileStorage` `StorageV2` так как общие файловые ресурсы уровня "Премиум" должны создаваться в учетной записи хранения филестораже, а не в учетной записи хранения GPv2.

```azurecli
az storage account create \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --kind FileStorage \
    --sku Premium_LRS \
    --output none
```

---

## <a name="create-file-share"></a>Создание общей папки
После создания учетной записи хранения остается только создать общую папку. Этот процесс в основном не отличается от того, используется ли общий файловый ресурс уровня "Премиум" или стандартный файловый ресурс. Следует учитывать следующие отличия.

Стандартные общие файловые ресурсы можно развернуть на одном из уровней Standard: оптимизированные для транзакции (по умолчанию), горячий или круто. Это для каждого уровня файлового ресурса, на который не влияет **уровень доступа к большому двоичному объекту** учетной записи хранения (это свойство относится только к хранилищу BLOB-объектов Azure, оно не связано с файлами Azure). Вы можете изменить уровень общей папки в любое время после развертывания. Файловые ресурсы уровня "Премиум" нельзя преобразовать непосредственно в любой уровень Standard.

> [!Important]  
> Вы можете перемещать общие папки между уровнями в пределах типов учетной записи хранения GPv2 (оптимизированные для транзакций, а также горячего и холодного уровней). При перемещении общих папок между уровнями выполняются транзакции. При перемещении с более горячего уровня на более холодный на более холодном уровне начинает взиматься плата за транзакции записи для каждого файла в общей папке. А при перемещении с более холодного уровня на более горячий на более холодном уровне начинает взиматься плата за транзакции чтения для каждого файла в общей папке.

Свойство **Quota** означает, что некоторое различие между файловыми ресурсами уровня "Премиум" и "Стандартный":

- Для стандартных файловых ресурсов это верхний предел общего файлового ресурса Azure, за исключением того, что конечные пользователи не могут переходить. Если квота не указана, Стандартная общая папка может охватывать до 100 Тиб (или 5 Тиб, если для учетной записи хранения не задано свойство больших файловых ресурсов).

- Для файловых ресурсов уровня "Премиум" квота означает **подготовленный размер**. Подготовленный размер — это сумма, за которую будет взиматься плата, независимо от фактического использования. Дополнительные сведения о планировании файлового ресурса уровня "Премиум" см. в разделе [Подготовка файловых ресурсов](understanding-billing.md#provisioned-model)уровня "Премиум".

# <a name="portal"></a>[Портал](#tab/azure-portal)
Если вы только что создали учетную запись хранения, перейдите к ней на экране развертывания, выбрав пункт **Переход к ресурсу**. В учетной записи хранения выберите плитку с метками " **Общие папки** " (вы также можете переходить к **общим файловым ресурсам** через оглавление для учетной записи хранения).

![Снимок экрана с плиткой общих файловых ресурсов](media/storage-how-to-create-file-share/create-file-share-1.png)

В списке общих файловых ресурсов должны отобразиться все общие папки, созданные ранее в этой учетной записи хранения. пустая таблица, если файловые ресурсы еще не созданы. Выберите **+ Общая папка** , чтобы создать новый файловый ресурс.

На экране должна отобразиться Новая колонка с общей папкой. Заполните поля в колонке новый общий файловый ресурс, чтобы создать общую папку:

- **Имя**. имя создаваемой общей папки.
- **Квота**: квота общей папки для стандартных файловых ресурсов. подготовленный размер файлового ресурса для файловых ресурсов уровня "Премиум".
- **Уровни**: выбранный уровень для файлового ресурса. Это поле доступно только в **учетной записи хранения общего назначения (GPv2)**. Вы можете выбрать оптимизированные, горячий или интересные транзакции. В любое время можно изменить уровень общего ресурса. Мы рекомендуем выбрать активные уровень во время миграции, чтобы уменьшить расходы на транзакции, а затем при необходимости переключиться на более низкий уровень после завершения миграции.

Выберите **создать** , чтобы завершить создание новой общей папки.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Вы можете создать файловый ресурс Azure с помощью [`New-AzRmStorageShare`](/powershell/module/az.storage/New-AzRmStorageShare) командлета. Следующие команды PowerShell предполагают, что вы установили переменные `$resourceGroupName` и, `$storageAccountName` как указано выше, в разделе Создание учетной записи хранения с Azure PowerShell. 

В следующем примере показано создание общей папки с явным уровнем с помощью `-AccessTier` параметра. Для этого необходимо использовать модуль предварительной версии AZ. Storage, как показано в примере. Если уровень не указан, так как вы используете общедоступный модуль AZ. Storage или не включили эту команду, уровень по умолчанию для стандартных файловых ресурсов оптимизирован для транзакции.

> [!Important]  
> Для файловых ресурсов уровня "Премиум" `-QuotaGiB` параметр ссылается на подготовленный размер общей папки. Подготовленный размер общей папки — это сумма, за которую будет взиматься плата, независимо от использования. Счета за стандартные общие ресурсы выставляются на основе использования, а не подготовленного размера.

```powershell
# Assuming $resourceGroupName and $storageAccountName from earlier in this document have already
# been populated. The access tier parameter may be TransactionOptimized, Hot, or Cool for GPv2 
# storage accounts. Standard tiers are only available in standard storage accounts. 
$shareName = "myshare"

New-AzRmStorageShare `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $storageAccountName `
        -Name $shareName `
        -AccessTier TransactionOptimized `
        -QuotaGiB 1024 | `
    Out-Null
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Вы можете создать файловый ресурс Azure с помощью [`az storage share-rm create`](/cli/azure/storage/share-rm#az_storage_share_rm_create) команды. Следующие Azure CLI команды предполагают, что вы установили переменные `$resourceGroupName` и `$storageAccountName` , как определено выше, в разделе Создание учетной записи хранения с Azure CLI.

> [!Important]  
> Для файловых ресурсов уровня "Премиум" `--quota` параметр ссылается на подготовленный размер общей папки. Подготовленный размер общей папки — это сумма, за которую будет взиматься плата, независимо от использования. Счета за стандартные общие ресурсы выставляются на основе использования, а не подготовленного размера.

```azurecli
shareName="myshare"

az storage share-rm create \
    --resource-group $resourceGroupName \
    --storage-account $storageAccountName \
    --name $shareName \
    --access-tier "TransactionOptimized" \
    --quota 1024 \
    --output none
```

---

> [!Note]  
> Имя общей папки должно состоять из символов в нижнем регистре. Дополнительные сведения о присвоении имен общим папкам и файлам см. в статье [Naming and Referencing Shares, Directories, Files, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) (Именование общих ресурсов, каталогов, файлов и метаданных и ссылка на них).

### <a name="changing-the-tier-of-an-azure-file-share"></a>Изменение уровня файлового ресурса Azure
Общие файловые ресурсы, развернутые в **учетной записи хранения общего назначения версии 2 (GPv2)** , могут находиться в оптимизированных, горячий и холодном уровнях транзакций. Вы можете изменить уровень файлового ресурса Azure в любое время в соответствии с затратами на транзакции, как описано выше.

# <a name="portal"></a>[Портал](#tab/azure-portal)
На странице основная учетная запись хранения выберите **Общие папки**  . Выберите плитку с метками " **Общие папки** " (вы также можете переходить к **общим папкам** через оглавление для учетной записи хранения).

![Снимок экрана с плиткой общих файловых ресурсов](media/storage-how-to-create-file-share/create-file-share-1.png)

В списке таблиц общих файловых ресурсов выберите общую папку, для которой необходимо изменить уровень. На странице Обзор общей папки выберите в меню пункт **изменить уровень** .

![Снимок экрана страницы обзора общей папки с выделенной кнопкой "изменить уровень"](media/storage-how-to-create-file-share/change-tier-0.png)

В открывшемся диалоговом окне выберите нужный уровень: оптимизированная, горячий или замечательная.

![Снимок экрана диалогового окна "изменение уровня"](media/storage-how-to-create-file-share/change-tier-1.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
В следующем командлете PowerShell предполагается, что вы установили `$resourceGroupName` `$storageAccountName` переменные,, `$shareName` как описано в предыдущих разделах этого документа.

```PowerShell
Update-AzRmStorageShare `
    -ResourceGroupName $resourceGroupName `
    -StorageAccountName $storageAccountName `
    -Name $shareName `
    -AccessTier Cool
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Следующая команда Azure CLI предполагает, что вы установили `$resourceGroupName` переменные, `$storageAccountName` и, `$shareName` как описано в предыдущих разделах этого документа.

```azurecli
az storage share-rm update \
    --resource-group $resourceGroupName \
    --storage-account $storageAccountName \
    --name $shareName \
    --access-tier "Cool"
```

---

## <a name="next-steps"></a>Дальнейшие действия
- [Спланируйте развертывание файлов Azure](storage-files-planning.md) или [запланируйте развертывание синхронизация файлов Azure](storage-sync-files-planning.md). 
- [Обзор сетевых](storage-files-networking-overview.md)возможностей.
- Подключите и подключите общую папку в [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md)и [Linux](storage-how-to-use-files-linux.md).