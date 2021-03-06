---
title: Использование защитника Azure для реестров контейнеров
description: Сведения об использовании защитника Azure для реестров контейнеров для сканирования образов Linux в реестрах, размещенных в Linux
author: memildin
ms.author: memildin
ms.date: 10/21/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: ee4992e41e792b570d8937edfe31efb4c651d742
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102100735"
---
# <a name="use-azure-defender-for-container-registries-to-scan-your-images-for-vulnerabilities"></a>Использование Azure Defender для реестров контейнеров с целью проверки образов на наличие уязвимостей

На этой странице объясняется, как использовать встроенный сканер уязвимостей для сканирования образов контейнеров, хранящихся в реестре контейнеров Azure на основе Azure Resource Manager.

При включении **Azure Defender для реестра контейнеров** все образы, которые вы отправляете в реестр, будут проверяться незамедлительно. Кроме того, проверяются все изображения, извлеченные за последние 30 дней. 

Когда средство проверки сообщает об уязвимостях в центре безопасности, центр безопасности отображает результаты и соответствующие сведения в качестве рекомендаций. Кроме того, результаты включают в себя связанную информацию, такую как действия по исправлению, соответствующие CVE, оценки CVSS и многое другое. Выявленные уязвимости можно просмотреть для одной или нескольких подписок или для определенного реестра.


## <a name="identify-vulnerabilities-in-images-in-azure-container-registries"></a>Выявление уязвимостей в образах Реестра контейнеров Azure 

Чтобы включить проверку уязвимостей образов, хранящихся в реестре контейнеров Azure на основе Azure Resource Manager, выполните следующие действия.

1. Включите **защитник Azure для реестров контейнеров** для вашей подписки. Теперь центр безопасности готов к сканированию изображений в реестре.

    >[!NOTE]
    > Плата за использование этой функции взимается за каждый образ.

1. Сканирование изображений активируется при каждой операции отправки или импорта, а также в том случае, если образ был извлечен в течение последних 30 дней. 

    По завершении проверки (обычно через приблизительно 2 минуты, но может составлять до 15 минут) результаты доступны в виде рекомендаций центра безопасности.

1. [Просмотрите и исправьте результаты, как описано ниже](#view-and-remediate-findings).

## <a name="identify-vulnerabilities-in-images-in-other-container-registries"></a>Выявление уязвимостей в образах в других реестрах контейнеров 

1. Используйте средства записи контроля доступа для переноса образов в реестр из центра DOCKER или реестра контейнеров Microsoft.  После завершения импорта импортированные изображения просматриваются защитником Azure. 

    Дополнительные сведения см. в статье [Импорт образов контейнеров в реестр контейнеров](../container-registry/container-registry-import-images.md) .

    По завершении проверки (обычно через приблизительно 2 минуты, но может составлять до 15 минут) результаты доступны в виде рекомендаций центра безопасности.

1. [Просмотрите и исправьте результаты, как описано ниже](#view-and-remediate-findings).


## <a name="view-and-remediate-findings"></a>Просмотреть и исправить результаты

1. Чтобы просмотреть результаты, перейдите на страницу **рекомендации** . Если обнаружены проблемы, вы увидите **уязвимости в образе реестра контейнеров Azure, которые должны быть исправлены**

    ![Рекомендации по исправлению проблем ](media/monitor-container-security/acr-finding.png)

1. Выберите рекомендацию. 

    Откроется страница сведений об рекомендации с дополнительными сведениями. Эти сведения включают список реестров с уязвимыми образами ("затронутые ресурсы") и действия по исправлению. 

1. Выберите конкретный реестр, чтобы просмотреть в нем репозитории с уязвимыми репозиториями.

    ![Выберите реестр](media/monitor-container-security/acr-finding-select-registry.png)

    Откроется страница сведения о реестре со списком затронутых репозиториев.

1. Выберите конкретный репозиторий, чтобы просмотреть в нем репозитории с уязвимыми образами.

    ![Выбор репозитория](media/monitor-container-security/acr-finding-select-repository.png)

    Откроется страница сведения о репозитории. В нем перечислены уязвимые образы, а также оценка серьезности полученных результатов.

1. Выберите конкретное изображение, чтобы увидеть уязвимости.

    ![Выбор изображений](media/monitor-container-security/acr-finding-select-image.png)

    Откроется список результатов для выбранного изображения.

    ![Список полученных результатов](media/monitor-container-security/acr-findings.png)

1. Чтобы узнать больше о поиске, выберите Поиск. 

    Откроется область сведений о выводах.

    [![Область сведений о полученных данных](media/monitor-container-security/acr-finding-details-pane.png)](media/monitor-container-security/acr-finding-details-pane.png#lightbox)

    Эта панель содержит подробное описание проблемы и ссылки на внешние ресурсы для устранения угроз.

1. Выполните действия, описанные в разделе "исправление" этой панели.

1. После выполнения действий, необходимых для устранения проблемы безопасности, замените образ в реестре:

    1. Отправка обновленного образа. Это приведет к запуску проверки. 
    
    1. На странице рекомендаций см. рекомендации по устранению уязвимости в образе реестра контейнеров Azure, которые следует исправить. 
    
        Если рекомендация по-прежнему отображается, а обрабатываемый образ по-прежнему отображается в списке уязвимых образов, проверьте действия по исправлению еще раз.

    1. Если вы уверены, что обновленное изображение Отправлено, проверено и больше не отображается в рекомендации, удалите "старый" уязвимый образ из реестра.


## <a name="disable-specific-findings-preview"></a>Отключить определенные выводы (Предварительная версия)

> [!NOTE]
> [!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)]

Если правила вашей организации требуют игнорировать обнаруженную проблему, а не исправлять ее, вы можете исключить ее из результатов поиска. Отключенные результаты не учитываются в оценке безопасности и не создают нежелательный шум.

Если результат поиска соответствует критерию, заданному в правилах отключения, он не будет отображаться в списке результатов. Типичные сценарии включают:

- Отключить результаты с уровнем серьезности ниже среднего
- Отключить неисправленные результаты
- Отключить результаты с оценками CVSS ниже 6,5
- Отключить выводы с конкретным текстом в проверке или категории безопасности (например, "RedHat", "CentOS Security Update для sudo")

> [!IMPORTANT]
> Чтобы создать правило, необходимо иметь разрешения на изменение политики в политике Azure.
>
> Дополнительные сведения см. в статье [разрешения RBAC в политике Azure](../governance/policy/overview.md#azure-rbac-permissions-in-azure-policy).

Можно использовать любой из следующих критериев: 

- Поиск идентификатора 
- Category
- Проверка безопасности 
- Оценки CVSS v3
- Уровень серьезности 
- Состояние исправления 

Чтобы создать правило, выполните следующие действия.

1. На странице сведений о рекомендациях для **уязвимостей в образах реестра контейнеров Azure необходимо устранить проблему**, выбрав **Отключить правило**.
1. Выберите соответствующую область.
1. Определите критерии.
1. Выберите **Применить правило**.

    :::image type="content" source="./media/defender-for-container-registries-usage/new-disable-rule-for-registry-finding.png" alt-text="Создание правила отключения для полученных результатов в реестре":::

1. Для просмотра, переопределения или удаления правила выполните следующие действия. 
    1. Выберите **Отключить правило**.
    1. В списке область подписки с активными правилами отображаются как **примененное правило**.
        :::image type="content" source="./media/remediate-vulnerability-findings-vm/modify-rule.png" alt-text="Изменение или удаление существующего правила":::
    1. Чтобы просмотреть или удалить правило, выберите меню с многоточием ("...").


## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> См. сведения об [Azure Defender](azure-defender.md).