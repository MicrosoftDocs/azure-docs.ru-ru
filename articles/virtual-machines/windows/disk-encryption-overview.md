---
title: Включение шифрования дисков Azure для виртуальных машин Windows
description: В этой статье приводятся инструкции по включению Microsoft Azure шифрования дисков для виртуальных машин Windows.
author: msmbaldwin
ms.service: virtual-machines
ms.subservice: disks
ms.collection: windows
ms.topic: conceptual
ms.author: mbaldwin
ms.date: 10/05/2019
ms.custom: seodec18
ms.openlocfilehash: 8e95f770a3335d66eae0a690e148c4d6ddc22d5c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102555335"
---
# <a name="azure-disk-encryption-for-windows-vms"></a>Шифрование дисков Azure для виртуальных машин Windows

Шифрование дисков Azure помогает защитить данные согласно корпоративным обязательствам по обеспечению безопасности и соответствия. Он использует функцию [BitLocker](https://en.wikipedia.org/wiki/BitLocker) в Windows, чтобы обеспечить шифрование томов для дисков ОС и данных виртуальных машин Azure, а также интегрировано с [Azure Key Vault](../../key-vault/index.yml) для управления ключами и секретами шифрования диска и их администрирования.

Шифрование дисков Azure является отказоустойчивой зоной, так же как и виртуальными машинами. Дополнительные сведения см. в статье [службы Azure, поддерживающие зоны доступности](../../availability-zones/az-region.md).

Если вы используете [центр безопасности Azure](../../security-center/index.yml), он оповестит вас при наличии незашифрованных виртуальных машин. Вы получите оповещение высокого уровня серьезности вместе c рекомендацией о шифровании таких виртуальных машин.

![Оповещение о шифровании диска в центре безопасности Azure](../media/disk-encryption/security-center-disk-encryption-fig1.png)

> [!WARNING]
> - Если вы уже использовали шифрование дисков Azure c Azure AD для шифрования виртуальной машины, сохраняйте и далее этот способ шифрования виртуальной машины. Подробнее см. в разделе [Шифрование дисков Azure с использованием приложения Azure AD (предыдущий выпуск)](disk-encryption-overview-aad.md). 
> - Выполнение некоторых приведенных рекомендаций может привести к более интенсивному использованию данных, сети или вычислительных ресурсов, а следовательно к дополнительным затратам на лицензии или подписки. Необходима действующая подписка Azure, которая позволяет создавать ресурсы Azure в поддерживаемых регионах.

Вы можете изучить основы шифрования дисков Azure для Windows всего за несколько минут с помощью [краткого руководства создание и шифрование виртуальной машины Windows с Azure CLI](disk-encryption-cli-quickstart.md) или [Создание и шифрование виртуальной машины windows с помощью Azure PowerShell](disk-encryption-powershell-quickstart.md)быстрого запуска.

## <a name="supported-vms-and-operating-systems"></a>Поддерживаемые виртуальные машины и операционные системы

### <a name="supported-vms"></a>Поддерживаемые виртуальные машины

Виртуальные машины Windows доступны в [различных размерах](../sizes-general.md). Шифрование дисков Azure поддерживается на виртуальных машинах поколения 1 и 2. Шифрование дисков Azure также доступно для виртуальных машин с хранилищем класса Рremium.

Шифрование дисков Azure недоступно на виртуальных машинах [уровня "базовый", "серия](https://azure.microsoft.com/pricing/details/virtual-machines/series/)" или на виртуальных машинах с объемом памяти менее 2 ГБ.  Шифрование дисков Azure также недоступно в образах виртуальных машин без временных дисков (dv4, Dsv4, Ev4 и Esv4).  См. раздел [размеры виртуальных машин Azure без локального временного диска](../azure-vms-no-temp-disk.md).  Дополнительные сведения об исключениях см. в статье [Шифрование дисков Azure. Неподдерживаемые сценарии](disk-encryption-windows.md#unsupported-scenarios).

### <a name="supported-operating-systems"></a>Поддерживаемые операционные системы

- Клиент Windows: Windows 8 и более поздней версии.
- Windows Server: Windows Server 2008 R2 и более поздние версии.  
 
> [!NOTE]
> Windows Server 2008 R2 требует установки платформа .NET Framework 4,5 для шифрования; Установите его из Центр обновления Windows с помощью необязательного обновления Microsoft .NET Framework 4.5.2 для 64-разрядных систем Windows Server 2008 R2 x64 ([KB2901983](https://www.catalog.update.microsoft.com/Search.aspx?q=KB2901983)).  
>  
> Windows Server 2012 R2 Core и Windows Server 2016 Core требуют, чтобы компонент BdeHdCfg был установлен на виртуальной машине для шифрования.


## <a name="networking-requirements"></a>Требования к сети
Чтобы включить шифрование дисков Azure, виртуальные машины должны соответствовать следующим требованиям к конфигурации конечной точки сети.
  - Чтобы получить маркер для подключения к хранилищу ключей, виртуальная машина Windows должна иметь возможность подключения к конечной точке Azure Active Directory \[ Login.microsoftonline.com \] .
  - Чтобы записать ключи шифрования в хранилище ключей, виртуальная машина Windows должна иметь возможность подключения к конечной точке хранилища ключей.
  - Виртуальная машина Windows должна иметь возможность подключения к конечной точке службы хранилища Azure, в которой размещен репозиторий расширения Azure, и учетной записи хранения Azure, в которой размещены VHD-файлы.
  -  Если ваша политика безопасности ограничивает доступ к Интернету с виртуальных машин Azure, можно разрешить указанный выше универсальный код ресурса (URI) и настроить определенное правило, чтобы разрешить исходящие подключения к данным IP-адресам. Дополнительные сведения см. в статье [Доступ к Azure Key Vault из-за брандмауэра](../../key-vault/general/access-behind-firewall.md).    


## <a name="group-policy-requirements"></a>Требования к групповая политика

Шифрование дисков Azure использует предохранитель внешнего ключа BitLocker для виртуальных машин Windows. Если виртуальные машины присоединены к домену, не применяйте групповые политики, требующие использования предохранителей TPM. Сведения о групповой политике для параметра "разрешить BitLocker без совместимого доверенного платформенного модуля" см. в разделе [Справочник по BitLocker групповая политика](/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1).

Политика BitLocker на виртуальных машинах, присоединенных к домену, с пользовательской групповой политикой должна включать следующий параметр: [Настройка хранилища пользователя сведений о восстановлении BitLocker — > разрешить 256-разрядный ключ восстановления](/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Шифрование дисков Azure завершится ошибкой, если параметры настраиваемой групповой политики для BitLocker несовместимы. На компьютерах без соответствующего параметра политики может потребоваться применить новую политику, принудительно обновить ее (gpupdate.exe /force) и перезагрузить компьютер.

Шифрование дисков Azure завершится ошибкой, если групповая политика уровня домена блокирует алгоритм AES-CBC, который используется BitLocker.

## <a name="encryption-key-storage-requirements"></a>Требования к хранилищу ключей шифрования  

Службе шифрования дисков Azure необходимо Azure Key Vault, чтобы управлять секретами и ключами шифрования дисков и контролировать их. Хранилище ключей и виртуальные машины должны находиться в одном регионе и подписке Azure.

Дополнительные сведения см. в статье [Создание и настройка хранилища ключей для шифрования дисков Azure](disk-encryption-key-vault.md).

## <a name="terminology"></a>Терминология
В следующей таблице приводятся распространенные термины, используемые в документации по шифрованию дисков Azure.

| Терминология | Определение |
| --- | --- |
| Azure Key Vault | Key Vault представляет собой службу управления криптографическими ключами. Она основана на аппаратных модулях безопасности, соответствующих Федеральному стандарту обработки информации (FIPS). Эти стандарты позволяют надежно хранить криптографические ключи и конфиденциальные данные. Дополнительные сведения см. в документации по [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) и в статье [Создание и настройка хранилища ключей для шифрования дисков Azure](disk-encryption-key-vault.md). |
| Azure CLI | [Azure CLI](/cli/azure/install-azure-cli) оптимизирован для администрирования ресурсов Azure и управления ими из командной строки.|
| BitLocker |[BitLocker](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831713(v=ws.11)) — это известная промышленным стандартам технология шифрования томов Windows, которая используется для включения шифрования дисков на виртуальных машинах Windows. |
| Ключ шифрования ключей (KEK) | Асимметричный ключ (RSA 2048), который применяется для защиты секрета или помещения его в оболочку. Вы можете использовать защищенный HSM ключ или ключ с программной защитой. Дополнительные сведения см. в документации по [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) и в статье [Создание и настройка хранилища ключей для шифрования дисков Azure](disk-encryption-key-vault.md). |
| Командлеты PowerShell | Дополнительные сведения см. в статье [Общие сведения об Azure PowerShell](/powershell/azure/). |

## <a name="next-steps"></a>Дальнейшие действия

- [Краткое руководство. Создание и шифрование виртуальной машины Windows с помощью Azure CLI ](disk-encryption-cli-quickstart.md)
- [Краткое руководство. Создание и шифрование виртуальной машины Windows с помощью Azure PowerShell](disk-encryption-powershell-quickstart.md)
- [Сценарии шифрования дисков Azure для виртуальных машин Windows](disk-encryption-windows.md)
- [Скрипт CLI для подготовки необходимых компонентов для службы "Шифрование дисков Azure"](https://github.com/ejarvi/ade-cli-getting-started) 
- [Скрипт PowerShell для подготовки предварительных требований для шифрования дисков Azure](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- [Создание и настройка хранилища ключей для шифрования дисков Azure](disk-encryption-key-vault.md)