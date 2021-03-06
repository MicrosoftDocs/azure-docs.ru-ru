---
title: Автоматизация установки локальной среды выполнения интеграции с помощью локальных сценариев PowerShell
description: Автоматизация установки локальной среды выполнения интеграции на локальные компьютеры.
ms.service: data-factory
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
ms.custom: seo-lt-2019
ms.date: 05/09/2020
ms.openlocfilehash: 8cbe54a23cb1c8b55afd86a18b51c0e392c3f78a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "100376213"
---
# <a name="automating-self-hosted-integration-runtime-installation-using-local-powershell-scripts"></a>Автоматизация установки локальной среды выполнения интеграции с помощью локальных сценариев PowerShell
Чтобы автоматизировать установку локальной среды выполнения интеграции на локальные компьютеры (не на виртуальных машинах Azure, где можно использовать шаблон Resource Manager), можно воспользоваться локальными сценариями PowerShell. В этой статье представлены два сценария, которые можно использовать.

## <a name="prerequisites"></a>Предварительные требования

* Запустите PowerShell на локальном компьютере. Для запуска сценариев потребуется выбрать команду **Запуск от имени администратора**.
* [Скачайте](https://www.microsoft.com/download/details.aspx?id=39717) программное обеспечение локальной среды выполнения интеграции. Скопируйте путь к скачанному файлу. 
* Также потребуется **ключ проверки подлинности**, чтобы зарегистрировать локальную среду выполнения интеграции.
* Для автоматизации обновлений, выполняемых вручную, необходима предварительно настроенная локальная среда выполнения интеграции.

## <a name="scripts-introduction"></a>Общие сведения о сценариях 

> [!NOTE]
> Эти сценарии создаются с помощью [ задокументированной служебной программы командной строки](./create-self-hosted-integration-runtime.md#set-up-an-existing-self-hosted-ir-via-local-powershell) в локальной среде выполнения интеграции. При необходимости можно настроить эти сценарии соответствующим образом в соответствии с потребностями в автоматизации.
> Сценарии необходимо применить на каждом узле, поэтому обязательно запустите его на всех узлах в случае установки высокого уровня доступности (2 узла и более).

* Для автоматизации установки: установите и зарегистрируйте новый узел локальной среды выполнения интеграции с помощью сценария **[InstallGatewayOnLocalMachine.ps1](https://github.com/nabhishek/SelfHosted-IntegrationRuntime_AutomationScripts/blob/master/InstallGatewayOnLocalMachine.ps1)** . Этот сценарий можно использовать для установки узла локальной среды выполнения интеграции и его регистрации с помощью ключа проверки подлинности. Сценарий принимает два аргумента — **первый** для указания расположения [локальной среды выполнения интеграции](https://www.microsoft.com/download/details.aspx?id=39717) на локальном диске, а **второй** для указания **ключа проверки подлинности** (для регистрации узла локальной среды IR).

* Для автоматизации обновлений, выполняемых вручную: обновите узел локальной среды IR до определенной версии или до актуальной версии с помощью сценария **[script-update-gateway.ps1](https://github.com/nabhishek/SelfHosted-IntegrationRuntime_AutomationScripts/blob/master/script-update-gateway.ps1)** . Он также поддерживается в случае, если вы отключили автоматическое обновление или хотите получить больший контроль над обновлениями. Сценарий можно использовать для обновления узла локальной среды выполнения интеграции до актуальной версии или до указанной более поздней версии (переход на использование более ранней версии не поддерживается). Он принимает аргумент для указания номера версии (пример: -version 3.13.6942.1). Если версия не указана, локальная среда IR всегда обновляется до последней версии, найденной в [загрузках](https://www.microsoft.com/download/details.aspx?id=39717).
    > [!NOTE]
    > Можно указать только последние 3 версии. В идеале сценарий используется для обновления существующего узла до актуальной версии. **ПРЕДПОЛАГАЕТСЯ, ЧТО У ВАС ЕСТЬ ЗАРЕГИСТРИРОВАННАЯ ЛОКАЛЬНАЯ СРЕДА IR**. 

## <a name="usage-examples"></a>Примеры использования

### <a name="for-automating-setup"></a>Для автоматизации установки
1. Скачайте локальную среду IR, которая находится [здесь](https://www.microsoft.com/download/details.aspx?id=39717). 
1. Укажите путь к расположению скачанного файла SHIR MSI (файл установки). Например, если путь *C:\Users\username\Downloads\IntegrationRuntime_4.7.7368.1.msi*, то для этой задачи можно использовать следующий пример для командной строки PowerShell:

   ```powershell
   PS C:\windows\system32> C:\Users\username\Desktop\InstallGatewayOnLocalMachine.ps1 -path "C:\Users\username\Downloads\IntegrationRuntime_4.7.7368.1.msi" -authKey "[key]"
   ```

    > [!NOTE]
    > Замените [key] ключом проверки подлинности, чтобы зарегистрировать IR.
    > Замените "username"на свое имя пользователя.
    > Укажите расположение файла "InstallGatewayOnLocalMachine.ps1" при выполнении сценария. В этом примере мы сохранили его на рабочем столе.

1. Если на компьютере имеется одна предварительно установленная локальная среда IR, сценарий автоматически удаляет ее, а затем настраивает новую. Появится следующее окно: ![настройка среды выполнения интеграции](media/self-hosted-integration-runtime-automation-scripts/integration-runtime-configure.png)

1. После завершения установки и регистрации ключа в локальной оболочке PowerShell отобразятся сообщения *Succeed to install gateway* и *Succeed to register gateway*, свидетельствующие об успешной установке и регистрации шлюза.
        [![результат выполнения сценария 1](media/self-hosted-integration-runtime-automation-scripts/script-1-run-result.png)](media/self-hosted-integration-runtime-automation-scripts/script-1-run-result.png#lightbox)

### <a name="for-automating-manual-updates"></a>Для автоматизации обновлений, выполняемых вручную
Этот скрипт используется для обновления/установки и регистрации последней версии локальной среды выполнения интеграции. Он выполняет следующие действия.
1. Проверяет текущую версию локальной среды IR
2. Получает последнюю или указанную версию из аргумента
3. При наличии более новой версии, чем текущая версия:
    * скачайте файл MSI локальной среды IR;
    * обновите среду.

Для использования этого сценария можно использовать следующий пример командной строки:
* Скачайте и установите последнюю версию шлюза:

   ```powershell
   PS C:\windows\system32> C:\Users\username\Desktop\script-update-gateway.ps1
   ```    
* Скачайте и установите указанную версию шлюза:
   ```powershell
   PS C:\windows\system32> C:\Users\username\Desktop\script-update-gateway.ps1 -version 3.13.6942.1
   ``` 
   Если текущая версия уже является последней, отобразится следующий результат, в котором предлагается не выполнять обновление.   
    [![результат выполнения сценария 2](media/self-hosted-integration-runtime-automation-scripts/script-2-run-result.png)](media/self-hosted-integration-runtime-automation-scripts/script-2-run-result.png#lightbox)