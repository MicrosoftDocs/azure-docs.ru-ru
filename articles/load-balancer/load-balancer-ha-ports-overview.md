---
title: Общие сведения о портах с высоким уровнем доступности в Azure
titleSuffix: Azure Load Balancer
description: Сведения о распределении нагрузки на портах с высоким уровнем доступности во внутренней подсистеме балансировки нагрузки.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2019
ms.author: allensu
ms.openlocfilehash: 6f089af71e4d32023e9cebd6613872f7db0eed7a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "94694965"
---
# <a name="high-availability-ports-overview"></a>Общие сведения о портах с высоким уровнем доступности

Azure Load Balancer (цен. категория "Стандартный") помогает выполнять балансировку нагрузки **всех** потоков протоколов на **всех** портах одновременно, если используется внутренняя Load Balancer через порты ha.

Порты высокой доступности (HA) — это тип правила балансировки нагрузки, обеспечивающий простой способ балансировки нагрузки **всех** потоков, поступающих на **все** порты внутренней Load Balancer (цен. Категория "Стандартный"). Решение о распределении нагрузки принимается отдельно для каждого потока. Оно основано на соединении пяти кортежей: исходного IP-адреса, исходного порта, целевого IP-адреса, целевого порта и протокола.

Правила балансировки нагрузки портов с высоким уровнем доступности помогают в критически важных сценариях. Например, они обеспечивают высокий уровень доступности и масштабирование для виртуальных сетевых модулей в виртуальных сетях. Они также позволяют распределять нагрузку между большим количеством портов. 

Правила балансировки нагрузки портов с высоким уровнем доступности настраиваются, если для интерфейсных и внутренних портов задано значение **0** и протокол для **всех**. После этого внутренний ресурс подсистемы балансировки нагрузки будет равномерно распределять все потоки TCP и UDP независимо от номера порта.

## <a name="why-use-ha-ports"></a>Зачем использовать порты с высоким уровнем доступности

### <a name="network-virtual-appliances"></a><a name="nva"></a>Сетевые виртуальные устройства

Виртуальные сетевые модули можно использовать для защиты рабочей нагрузки Azure от различных угроз безопасности. Виртуальные сетевые модули, используемые в таких сценариях, должны быть надежными, высокодоступными и масштабируемыми по требованию.

Для этого добавьте экземпляры виртуальных сетевых модулей во внутренний пул внутренней подсистемы балансировки нагрузки и настройте правило распределения нагрузки на портах с высоким уровнем доступности.

Для виртуальных сетевых модулей с высоким уровнем доступности порты с высоким уровнем доступности предоставляют следующие преимущества:
- быстрая отработка отказа в работоспособные экземпляры с проверками работоспособности для каждого экземпляра;
- более высокая производительность с масштабированием до *N* активных экземпляров;
- сценарии "*N* активных" и "активный — пассивный";
- устранение необходимости в сложных решениях, например в использовании узлов Apache ZooKeeper для мониторинга модулей.

На следующей схеме представлено развертывание виртуальной сети со звездообразным подключением. "Лучи" принудительно туннелируют свой трафик в центральную виртуальную сеть и через виртуальный сетевой модуль перед выходом из доверенного пространства. Виртуальные сетевые модули находятся за внутренней подсистемой Load Balancer уровня "Стандартный" в конфигурации с портами высокого уровня доступности. Весь трафик может обрабатываться и перенаправляться соответствующим образом. При настройке, как показано на следующей схеме, правило балансировки нагрузки портов HA дополнительно обеспечивает симметрию потока для входящего и исходящего трафика.

<a node="diagram"></a>
![Схема виртуальной сети "звезда" с NVA, развернутой в режиме высокой доступности](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Если вы используете виртуальные сетевые модули, узнайте у поставщика этих модулей, как лучше всего использовать порты высокого уровня доступности и какие сценарии поддерживаются.

### <a name="load-balancing-large-numbers-of-ports"></a>Балансировка нагрузки для большого количества портов

Можно также использовать порты с высоким уровнем доступности для приложений, требующих распределения нагрузки между большим количеством портов. Эти сценарии можно упростить, применив [Load Balancer уровня "Стандартный"](./load-balancer-overview.md) с портами высокого уровня доступности. Единое правило распределения нагрузки заменяет несколько отдельных правил для каждого порта балансировки нагрузки.

## <a name="region-availability"></a>Доступность по регионам

Функция портов высокого уровня доступности предлагается во всех глобальных регионах Azure.

## <a name="supported-configurations"></a>Поддерживаемые конфигурации

### <a name="a-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Конфигурация с портами высокого уровня доступности на базе единого неплавающего IP-адреса (без прямого ответа от сервера) для внутренней подсистемы балансировки нагрузки уровня "Стандартный"

Это самая простая конфигурация для портов высокого уровня доступности. Чтобы настроить правило балансировки нагрузки для портов высокого уровня доступности на базе единого внешнего IP-адреса, выполните следующие действия:
1. При настройке подсистемы балансировки нагрузки уровня "Стандартный" установите флажок **Порты высокой доступности** в конфигурации правила балансировки нагрузки.
2. Для параметра **Плавающий IP-адрес** установите значение **Отключено**.

Эта конфигурация не позволяет использовать любые другие конфигурации правил балансировки нагрузки для текущего ресурса подсистемы балансировки нагрузки. Она также не позволяет использовать другие конфигурации ресурсов внутренней подсистемы балансировки нагрузки для заданного набора внутренних экземпляров.

Но вы можете настроить общедоступный ресурс подсистемы балансировки нагрузки уровня "Стандартный" для внутренних экземпляров в дополнение к этому правилу портов высокого уровня доступности.

### <a name="a-single-floating-ip-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Конфигурация с портами высокого уровня доступности на базе единого плавающего IP-адреса (с прямым ответом от сервера) для внутренней подсистемы балансировки нагрузки уровня "Стандартный"

Вы можете точно так же настроить для подсистемы балансировки нагрузки правило балансировки нагрузки с **портом высокого уровня доступности** и единой внешней частью, **установив** параметр **Плавающий IP-адрес**. 

Такая конфигурация позволяет использовать дополнительные правила балансировки нагрузки с плавающим IP-адресом и (или) общедоступные ресурсы подсистемы балансировки нагрузки. Однако на основе этой конфигурации нельзя использовать балансировку нагрузки с портами высокого уровня доступности без плавающего IP-адреса.

### <a name="multiple-ha-ports-configurations-on-an-internal-standard-load-balancer"></a>Несколько конфигураций с портами высокого уровня доступности во внутренней подсистеме балансировки нагрузки уровня "Стандартный"

Если для вашего сценария нужны несколько интерфейсов с портами высокого уровня доступности, взаимодействующих с одним внешним пулом, вы можете сделать следующее: 
- Настроить несколько частных внешних IP-адресов для одного внутреннего ресурса подсистемы балансировки нагрузки уровня "Стандартный".
- Настроить несколько правил балансировки нагрузки, для каждого из которых выбран один уникальный внешний IP-адрес.
- Выбрать вариант **Порты высокой доступности**, а для параметра **Плавающий IP-адрес** выбрать значение **Включено** для всех правил балансировки нагрузки.

### <a name="an-internal-load-balancer-with-ha-ports-and-a-public-load-balancer-on-the-same-back-end-instance"></a>Внутренняя подсистема балансировки нагрузки с портами высокого уровня доступности и общедоступная подсистема балансировки нагрузки для одного и того же внутреннего экземпляра

Можно настроить *один* общедоступный ресурс Load Balancer (цен. Категория "Стандартный") для серверных ресурсов, а также один внутренний Load Balancer (цен. Категория "Стандартный") с ПОРТАМИ высокой доступности.

## <a name="limitations"></a>Ограничения

- Правила балансировки нагрузки портов с высоким уровнем доступности доступны только для внутренних Load Balancer (цен. категория "Стандартный").
- **Не** поддерживается объединение правил балансировки нагрузки портов с высоким уровнем доступности и правила балансировки нагрузки портов с высоким уровнем доступности, указывающих на те же серверные IP-конфигурации, если не включен параметр с плавающим IP-адресом.
- Существующие IP-фрагменты будут перенаправлены правилами балансировки нагрузки портов с высоким уровнем доступности в то же место назначения, что и первый пакет.  Фрагментация IP-пакетов UDP или TCP не поддерживается.
- Симметрия потока (в основном для сценариев NVA) поддерживается с экземпляром сервера и одним сетевым интерфейсом (и одной IP-конфигурацией) только при использовании, как показано на приведенной выше схеме, и использовании правил балансировки нагрузки портов с высоким уровнем доступности. В других сценариях симметрия потока не поддерживается. Это означает, что два или более ресурсов службы Load Balancer и их соответствующие правила принимают независимые решения и никогда не координируются. См. описание и схему [виртуальных сетевых модулей](#nva). Если вы используете несколько сетевых интерфейсов или добавляете NVA между общедоступным и внутренним Load Balancer, то симметрия потока недоступна.  Вы можете обойти эту проблему с помощью источника, преобразующего сетевые адреса (NAT) потока входящего трафика на IP-адреса устройства, что позволит ответам поступать на тот же виртуальный сетевой модуль.  Тем не менее мы настоятельно рекомендуем использовать одну сетевую карту и эталонную архитектуру, показанную на схеме выше.


## <a name="next-steps"></a>Дальнейшие действия

- [Дополнительные сведения о Load Balancer (цен. категория "Стандартный")](load-balancer-overview.md)
