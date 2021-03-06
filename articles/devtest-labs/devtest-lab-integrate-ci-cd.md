---
title: Интеграция Azure DevTest Labs в Azure Pipelines
description: Сведения об интеграции Azure DevTest Labs с конвейером непрерывной интеграции и поставки Azure Pipelines.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 96f99d41d0a7ea07bf3854292f9c3bd6245414b3
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "87288931"
---
# <a name="integrate-azure-devtest-labs-into-your-azure-pipelines-cicd-pipeline"></a>Интеграция Azure DevTest Labs в конвейер Azure Pipelines CI/CD

Расширение " *задачи Azure DevTest Labs* " можно использовать для интеграции конвейеров сборки Azure pipelines непрерывной интеграции и непрерывной поставки (CI/CD) с Azure DevTest Labs. Расширение устанавливает несколько задач, включая: 

- Создание виртуальной машины.
- Создание пользовательского образа из виртуальной машины
- Удаление виртуальной машины 

Эти задачи упрощают, например, быстрое развертывание виртуальной машины *эталонного образа* для конкретной задачи тестирования, а затем удаляет виртуальную машину после завершения теста.

В этой статье показано, как использовать задачи Azure DevTest Labs для создания и развертывания виртуальной машины, создания пользовательского образа, а затем для удаления виртуальной машины — всего одного конвейера выпуска. Обычно задачи выполняются по отдельности в пользовательских конвейерах сборки, тестирования и развертывания.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="prerequisites"></a>Предварительные требования

- Регистрация или вход в организацию [Azure DevOps](https://dev.azure.com) , а также [Создание проекта](/vsts/organizations/projects/create-project) в Организации. 
  
- Установите расширение Tasks Azure DevTest Labs из Visual Studio Marketplace.
  
  1. Перейдите на страницу расширения [Задачи Azure DevTest Labs](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).
  1. Выберите **получить бесплатно**.
  1. Выберите свою организацию Azure DevOps в раскрывающемся списке и нажмите кнопку **установить**. 
  
## <a name="create-the-template-to-build-an-azure-vm"></a>Создание шаблона для создания виртуальной машины Azure 

В этом разделе описывается создание шаблона Azure Resource Manager, который используется для создания виртуальной машины Azure по запросу.

1. Чтобы создать шаблон диспетчер ресурсов в подписке, выполните процедуру, описанную в разделе [Использование шаблона диспетчер ресурсов](devtest-lab-use-resource-manager-template.md).
   
1. Перед созданием шаблона Resource Manager добавьте [артефакт WinRM](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) как часть создания виртуальной машины. Для задач развертывания, таких как *копирование файлов Azure* и *PowerShell на целевых компьютерах* , необходим доступ к WinRM. Артефакту WinRM требуется имя узла в качестве параметра, которое должно быть полным доменным именем (FQDN) виртуальной машины. 
   
   > [!NOTE]
   > При использовании WinRM с общим IP-адресом необходимо добавить правило NAT для сопоставления внешнего порта с портом WinRM. Правило NAT не требуется при создании виртуальной машины с общедоступным IP-адресом.
   
   
1. Сохраните шаблон на компьютере в виде файла с именем *CreateVMTemplate.jsв*.
   
1. Верните шаблон в систему управления версиями.

## <a name="create-a-script-to-get-vm-properties"></a>Создание скрипта для получения свойств виртуальной машины

При выполнении шагов задач, таких как *копирование файлов Azure* или *PowerShell на целевых компьютерах* в конвейере выпуска, следующий скрипт собирает значения, необходимые для развертывания приложения на виртуальной машине. Эти задачи обычно используются для развертывания приложения на виртуальной машине Azure. Для задач требуются такие значения, как имя группы ресурсов виртуальной машины, IP-адрес и полное доменное имя.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Чтобы создать файл скрипта, выполните следующие действия.

1. Откройте текстовый редактор и вставьте в него следующий скрипт.
   
   ```powershell
   Param( [string] $labVmId)

   $labVmComputeId = (Get-AzResource -Id $labVmId).Properties.ComputeId

   # Get lab VM resource group name
   $labVmRgName = (Get-AzResource -Id $labVmComputeId).ResourceGroupName

   # Get the lab VM Name
   $labVmName = (Get-AzResource -Id $labVmId).Name

   # Get lab VM public IP address
   $labVMIpAddress = (Get-AzPublicIpAddress -ResourceGroupName $labVmRgName
                   -Name $labVmName).IpAddress

   # Get lab VM FQDN
   $labVMFqdn = (Get-AzPublicIpAddress -ResourceGroupName $labVmRgName
              -Name $labVmName).DnsSettings.Fqdn

   # Set a variable labVmRgName to store the lab VM resource group name
   Write-Host "##vso[task.setvariable variable=labVmRgName;]$labVmRgName"

   # Set a variable labVMIpAddress to store the lab VM Ip address
   Write-Host "##vso[task.setvariable variable=labVMIpAddress;]$labVMIpAddress"

   # Set a variable labVMFqdn to store the lab VM FQDN name
   Write-Host "##vso[task.setvariable variable=labVMFqdn;]$labVMFqdn"
   ```

1. Сохраните файл с именем, например *GetLabVMParams.ps1*, и верните его в систему управления версиями. 

## <a name="create-a-release-pipeline-in-azure-pipelines"></a>Создание конвейера выпуска в Azure Pipelines

Чтобы создать новый конвейер выпуска, выполните следующие действия.

1. На странице проекта Azure DevOps в области навигации слева выберите пункт **конвейеры**  >  **выпуски** .
1. Выберите **Новый конвейер**.
1. В разделе **Выбор шаблона** прокрутите вниз и выберите **пустое задание**, а затем нажмите кнопку **Применить**.

### <a name="add-and-set-variables"></a>Добавление и настройка переменных

Задачи конвейера используют значения, назначенные виртуальной машине при создании шаблона диспетчер ресурсов в портал Azure. 

Чтобы добавить переменные для значений, сделайте следующее: 

1. На странице конвейер выберите вкладку **переменные** .
   
1. Для каждой переменной выберите **Добавить** и введите имя и значение:
   
   |Имя|Значение|
   |---|---|
   |*vmName*|Имя виртуальной машины, назначенное в шаблоне диспетчер ресурсов|
   |*userName*|Имя пользователя для доступа к виртуальной машине|
   |*password*|Пароль для имени пользователя. Щелкните значок замка, чтобы скрыть и защитить пароль.

### <a name="create-a-devtest-labs-vm"></a>Создание виртуальной машины DevTest Labs

Следующим шагом является создание виртуальной машины эталонного образа для использования в будущих развертываниях. Виртуальную машину можно создать в экземпляре Azure DevTest Labs с помощью задачи *Azure DevTest Labs создание виртуальной машины* .

1. На вкладке **конвейер** конвейера выпуска выберите текст с гиперссылкой на **этапе 1** , чтобы **просмотреть задачи этапа**, а затем щелкните знак "плюс" **+** рядом с **заданием агента**. 
   
1. В разделе **Добавление задач** выберите **Azure DevTest Labs создать виртуальную машину** и нажмите кнопку **добавить**. 
   
1. Выберите **создать Azure DevTest Labs виртуальную машину** в левой области. 

1. В области справа заполните форму следующим образом:
   
   |Поле|Значение|
   |---|---|
   |**Подписка Azure RM**|Выберите подключение службы или подписку из **доступных подключений к службам Azure** или **доступных подписок Azure** в раскрывающемся списке и при необходимости выберите **авторизовать** .<br /><br />**Примечание.** Сведения о создании подключения к подписке Azure с ограниченными разрешениями см. в разделе [Azure Resource Manager Service Endpoint](/azure/devops/pipelines/library/service-endpoints#sep-azure-resource-manager).|
   |**Имя лаборатории**|Выберите имя существующей лаборатории, в которой будет создана лабораторная виртуальная машина.|
   |**Имя шаблона**|Введите полный путь и имя файла шаблона, который вы сохранили в репозитории исходного кода. Для упрощения пути можно использовать встроенные свойства, например:<br /><br />`$(System.DefaultWorkingDirectory)/Templates/CreateVMTemplate.json`|
   |**Параметры шаблона**|Введите параметры для переменных, которые вы определили ранее:<br /><br />`-newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)`|
   |**Выходные переменные**  >  **ИД виртуальной машины лаборатории**|Введите переменную для созданного идентификатора виртуальной машины лаборатории. При использовании **лабвмид** по умолчанию можно ссылаться на переменную в последующих задачах как *$ (лабвмид)*.<br /><br />Можно создать имя, отличное от значения по умолчанию, но не забывайте использовать правильное имя в последующих задачах. ИДЕНТИФИКАТОР виртуальной машины лаборатории можно записать в следующем формате:<br /><br />`/subscriptions/{subscription Id}/resourceGroups/{resource group Name}/providers/Microsoft.DevTestLab/labs/{lab name}/virtualMachines/{vmName}`|

### <a name="collect-the-details-of-the-devtest-labs-vm"></a>Получение сведений о виртуальной машине DevTest Labs

Выполните скрипт, созданный ранее, чтобы собрать подробные сведения о виртуальной машине DevTest Labs. 

1. На вкладке **конвейер** конвейера выпуска выберите текст с гиперссылкой на **этапе 1** , чтобы **просмотреть задачи этапа**, а затем щелкните знак "плюс" **+** рядом с **заданием агента**. 
   
1. В разделе **Добавление задач** выберите **Azure PowerShell** и нажмите кнопку **добавить**. 
   
1. Выберите **Azure PowerShell сценарий: FilePath** в левой области. 
   
1. В области справа заполните форму следующим образом:
   
   |Поле|Значение|
   |---|---|
   |**Тип подключения Azure**|Выберите **Azure Resource Manager**.|
   |**подписка Azure**;|Выберите подключение службы или подписку.| 
   |**Тип скрипта**|Выберите **путь к файлу скрипта**.|
   |**Путь к сценарию**|Введите полный путь и имя скрипта PowerShell, который вы сохранили в репозитории исходного кода. Для упрощения пути можно использовать встроенные свойства, например:<br /><br />`$(System.DefaultWorkingDirectory/Scripts/GetLabVMParams.ps1`|
   |**Аргументы скрипта**|Введите имя переменной *лабвмид* , которая была заполнена предыдущей задачей, например:<br /><br />`-labVmId '$(labVMId)'`|

Скрипт собирает необходимые значения и сохраняет их в переменных среды в конвейере выпуска, чтобы вы могли легко обращаться к ним в последующих шагах.

### <a name="create-a-vm-image-from-the-devtest-labs-vm"></a>Создание образа виртуальной машины на основе виртуальной машины DevTest Labs

Следующая задача — создать образ только что развернутой виртуальной машины в экземпляре Azure DevTest Labs. Впоследствии образ можно использовать для создания копий виртуальной машины по запросу, когда потребуется выполнить задачу разработки или выполнить некоторые тесты. 

1. На вкладке **конвейер** конвейера выпуска выберите текст с гиперссылкой на **этапе 1** , чтобы **просмотреть задачи этапа**, а затем щелкните знак "плюс" **+** рядом с **заданием агента**. 
   
1. В разделе **Добавление задач** выберите **Azure DevTest Labs создать пользовательский образ** и нажмите кнопку **добавить**. 
   
1. Настройте задачу следующим образом.
   
   |Поле|Значение|
   |---|---|
   |**Подписка Azure RM**|Выберите подключение службы или подписку.|
   |**Имя лаборатории**|Выберите имя существующей лаборатории, в которой будет создан образ.|
   |**Имя пользовательского образа**|Введите имя пользовательского образа.|
   |**Описание** (необязательно)|Введите описание, чтобы упростить выбор правильного образа позже.|
   |**Исходная виртуальная машина лаборатории**  >  **Идентификатор виртуальной машины исходной лаборатории**|Если вы изменили имя переменной Лабвмид по умолчанию, введите его здесь. По умолчанию установлено значение **$(labVMId)**.|
   |**Выходные переменные**  >  **Идентификатор пользовательского образа**|При необходимости можно изменить имя переменной по умолчанию.|
   
### <a name="deploy-your-app-to-the-devtest-labs-vm-optional"></a>Развертывание приложения на виртуальной машине DevTest Labs (необязательно)

Вы можете добавить задачи для развертывания приложения на новой виртуальной машине DevTest Labs. Задачи, которые обычно используются для развертывания приложения, — это *копирование файлов Azure* и *PowerShell на целевых компьютерах*.

Сведения о виртуальной машине, необходимые для параметров этих задач, хранятся в трех переменных конфигурации с именами **лабвмргнаме**, **лабвмипаддресс** и **лабвмфкдн** в конвейере выпуска. Если вы только хотите поэкспериментировать с созданием виртуальной машины DevTest Labs и пользовательского образа без развертывания приложения на ней, то этот шаг можно пропустить.

### <a name="delete-the-vm"></a>Удаление виртуальной машины

Последняя задача — удалить виртуальную машину, развернутую в экземпляре Azure DevTest Labs. Обычно после выполнения задач разработки или выполнения тестов на развернутой виртуальной машины ее удаляют. 

1. На вкладке **конвейер** конвейера выпуска выберите текст с гиперссылкой на **этапе 1** , чтобы **просмотреть задачи этапа**, а затем щелкните знак "плюс" **+** рядом с **заданием агента**. 
   
1. В разделе **Добавление задач** выберите **Azure DevTest Labs удалить виртуальную машину** и нажмите кнопку **добавить**. 
   
1. Настройте задачу следующим образом.
   
   - В разделе **Подписка Azure RM** выберите подключение службы или подписку. 
   - Если вы изменили имя переменной Лабвмид по умолчанию, введите его в поле **идентификатор виртуальной машины лаборатории**. По умолчанию установлено значение **$(labVMId)**.
   
### <a name="save-the-release-pipeline"></a>Сохранение конвейера выпуска

Чтобы сохранить новый конвейер выпуска, выполните следующие действия.

1. Выберите имя **Новый конвейер выпуска** на странице конвейер выпуска и введите новое имя для конвейера. 
   
1. Щелкните значок **сохранить** в правом верхнем углу. 

## <a name="create-and-run-a-release"></a>Создание и запуск выпуска

Чтобы создать и запустить выпуск с помощью нового конвейера, выполните следующие действия.

1. Выберите **создать выпуск** в верхнем правом углу на странице конвейер выпуска. 
   
1. В разделе **артефакты** выберите последнюю сборку и нажмите кнопку **создать**.
   
1. На каждом этапе выпуска обновите представление экземпляра DevTest Labs в портал Azure для просмотра создания виртуальной машины, создания образа и удаления виртуальной машины.

Вы можете использовать пользовательский образ для создания виртуальных машин всякий раз, когда они понадобятся.

## <a name="next-steps"></a>Дальнейшие действия
- Дополнительные сведения см. в статье [Создание сред со множеством виртуальных машин и ресурсов PaaS с помощью шаблонов Azure Resource Manager](devtest-lab-create-environment-from-arm.md).
- Ознакомьтесь с дополнительными руководствами диспетчер ресурсов шаблонов для автоматизации DevTest Labs из [общедоступного репозитория GitHub DevTest Labs](https://github.com/Azure/azure-quickstart-templates).
- При необходимости см. страницу [устранения неполадок DevOps в Azure](/azure/devops/pipelines/troubleshooting) .
 
