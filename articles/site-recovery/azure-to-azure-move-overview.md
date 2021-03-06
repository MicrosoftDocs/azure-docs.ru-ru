---
title: Перемещение виртуальных машин Azure в другой регион с помощью Azure Site Recovery
description: Перенос виртуальных машин Azure из одного региона Azure в другой с помощью службы Azure Site Recovery.
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 61d596c4b3a65c54e1a70682adad5b7328c384f8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "90007372"
---
# <a name="moving-azure-vms-to-another-azure-region"></a>Перемещение виртуальных машин Azure в другой регион Azure

В этой статье приводится обзор причин и действий по переносу виртуальных машин Azure в другой регион Azure с помощью [Azure Site Recovery](site-recovery-overview.md). 


## <a name="reasons-to-move-azure-vms"></a>Причины перемещения виртуальных машин Azure

Перемещение виртуальных машин может потребоваться по следующим причинам:

- У вас уже есть развертывание в одном регионе и была добавлена поддержка нового региона, который ближе к пользователям вашего приложения или службы. В этом случае вы можете переместить виртуальные машины "как есть" в новый регион, чтобы уменьшить задержку. Такой же подход применяется, если необходимо консолидировать подписки или существуют правила управления или организации, требующие перемещения.
- Ваша виртуальная машина была развернута как одноэкземплярная или как часть группы доступности. Если вы хотите расширить Соглашения об уровне обслуживания относительно доступности, вы можете переместить виртуальные машины в зону доступности.

## <a name="move-vms-with-resource-mover"></a>Перемещение виртуальных машин с помощью Resource Mover

Теперь вы можете перемещать виртуальные машины в другой регион с помощью [Azure Resource Mover](../resource-mover/tutorial-move-region-virtual-machines.md). Resource Mover поддерживается в общедоступной предварительной версии и обеспечивает следующее.
- Один центр для перемещения ресурсов между регионами.
- Сокращение времени и упрощение при перемещении. Все, что вам нужно, находится в одном расположении.
- Простой и согласованный процесс перемещения различных типов ресурсов Azure.
- Простой способ обнаружения зависимостей между ресурсами, которые необходимо переместить. Это помогает перемещать связанные ресурсы вместе, чтобы после перемещения в целевом регионе все правильно работало.
- Автоматическая очистка ресурсов в исходном регионе, если их нужно удалить после перемещения.
- Тестирование. Вы можете попробовать выполнить перемещение, а затем отменить его, если не хотите выполнять полный переход.



## <a name="move-vms-with-site-recovery"></a>Перемещение виртуальных машин с помощью Site Recovery

Для этого необходимо выполнить приведенные ниже шаги.

1. Проверьте предварительные требования.
2. Выполните подготовку исходных виртуальных машин.
3. Выполните подготовку целевого региона.
4. Скопируйте данные в целевой регион. Используйте технологию репликации Azure Site Recovery для копирования данных исходной виртуальной машины в целевой регион.
5. Выполните проверку настройки. После завершения репликации проверьте конфигурацию, выполнив тестовую отработку отказа в непроизводственной сети.
6. Выполните перемещение.
7. Выполните отмену ресурсов в исходном регионе.

> [!NOTE]
> Сведения об этих шагах приведены в следующих разделах.
> [!IMPORTANT]
> Сейчас Azure Site Recovery поддерживает перемещение виртуальных машин из одного региона в другой, но не поддерживает перемещение в пределах одного региона.

## <a name="typical-architectures-for-a-multi-tier-deployment"></a>Стандартные архитектуры для многоуровневого развертывания

В этом разделе описаны самые распространенные архитектуры развертывания для многоуровневого приложения в Azure. Примером является трехуровневое приложение с общедоступным IP-адресом. Каждый из уровней (сеть, приложение и база данных) имеет по две виртуальные машины, подключенные с помощью подсистемы балансировки нагрузки Azure к другим уровням. Уровень базы данных использует репликацию с функцией SQL Server Always On между виртуальными машинами для обеспечения высокой доступности.

* **Виртуальные машины с одним экземпляром, развернутые на различных уровнях**. Каждая виртуальная машина на уровне настроена как виртуальная машина с одним экземпляром и подключена с помощью подсистем балансировки нагрузки к другим уровням. Это самая простая конфигурация.

     ![Выбор для перемещения развертывания виртуальной машины с одним экземпляром на уровнях](media/move-vm-overview/regular-deployment.png)

* **Виртуальные машины на каждом уровне, развернутые в группах доступности**. Каждая виртуальная машина на уровне настраивается в группе доступности. [Группы доступности](../virtual-machines/windows/tutorial-availability-sets.md) распределяют развернутые в Azure виртуальные машины между несколькими изолированными аппаратными узлами в кластере. Это гарантирует, что в случае сбоя оборудования или программного обеспечения в Azure затрагивается только подмножество виртуальных машин, а общее решение остается доступным для использования.

     ![Развертывание виртуальной машины в группах доступности](media/move-vm-overview/avset.png)

* **Виртуальные машины на каждом уровне, развернутые в Зонах доступности**. Каждая виртуальная машина на уровне настраивается в [Зонах доступности](../availability-zones/az-overview.md). Зона доступности в регионе Azure — это сочетание домена сбоя и домена обновления. Например, если вы создаете три или боле виртуальных машин в трех зонах в регионе Azure, виртуальные машины эффективно распределяются между тремя доменами сбоя и тремя доменами обновления. Платформа Azure поддерживает такое распределение между доменами обновления, чтобы виртуальные машины в различных зонах не обновлялись одновременно.

     ![Развертывание зоны доступности](media/move-vm-overview/zone.png)

## <a name="move-vms-as-is-to-a-target-region"></a>Перемещение виртуальных машин "как есть" в целевое расположение

Здесь показано, как будут выглядеть развертывания после перемещения "как есть" в целевой регион с учетом вышеупомянутых [архитектур](#typical-architectures-for-a-multi-tier-deployment).

* **Виртуальные машины с одним экземпляром, развернутые на различных уровнях**
* **Виртуальные машины на каждом уровне, развернутые в группах доступности**
* **Виртуальные машины на каждом уровне, развернутые в Зонах доступности**

## <a name="move-vms-to-increase-availability"></a>Перемещение виртуальных машин для повышения доступности

* **Виртуальные машины с одним экземпляром, развернутые на различных уровнях**

     ![Развертывание виртуальной машины с одним экземпляром на уровнях](media/move-vm-overview/single-zone.png)

* **Виртуальные машины на каждом уровне, развернутые в группах доступности**. Вы можете настроить размещение виртуальных машин в группе доступности в отдельных Зонах доступности при включении репликации для виртуальной машины с помощью Azure Site Recovery. После перемещения в соответствии с Соглашением об уровне обслуживания будет обеспечиваться доступность на уровне 99,99 %.

     ![Развертывание виртуальной машины в группы доступности и Зоны доступности](media/move-vm-overview/aset-azone.png)

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> 
> * [Перенос виртуальных машин Azure в другой регион](azure-to-azure-tutorial-migrate.md)
> 
> * [Перенос виртуальных машин Azure в Зоны доступности](move-azure-vms-avset-azone.md)

