---
title: Руководство по настройке размера кластера Interactive Query в Azure HDInsight
description: Руководство по настройке размера Interactive Query в Azure HDInsight
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: a7baa9340a1f0a99b94bfcbe535c73d0b502e2a0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "98933068"
---
# <a name="interactive-query-cluster-sizing-guide-in-azure-hdinsight"></a>Руководство по настройке размера кластера Interactive Query в Azure HDInsight

В этом документе описывается настройка размера кластера Interactive Query HDInsight (LLAP) для типичной рабочей нагрузки с целью достижения разумной производительности. В этом документе приведены общие рекомендации, и для конкретных рабочих нагрузок может потребоваться специфичная тонкая настройка.

## <a name="default-vm-types-for-interactive-query"></a>Типы виртуальных машин по умолчанию для Interactive Query

| Тип узла | Экземпляр | Размер |
|---|---|---|
| Head | D13 v2 | 8 виртуальных ЦП, 56 ГБ ОЗУ, диск SSD 400 ГБ |
| Рабочий узел | D14 v2 | 16 виртуальных ЦП, 112 ГБ ОЗУ, диск SSD 800 ГБ |
| ZooKeeper | A4 v2 | 4 виртуальных ЦП, 8 ГБ ОЗУ, диск SSD 40 ГБ |

## <a name="recommended-configurations"></a>Рекомендуемые конфигурации

Рекомендуемые значения конфигурации основаны на рабочем узле типа D14 v2.

| Клавиши | Значение | Описание |
|---|---|---|
| yarn.nodemanager.resource.memory-mb | 102400 (MБ) | Общий объем памяти в МБ, выделенный для всех контейнеров YARN на узле. |
| yarn.scheduler.maximum-allocation-mb | 102400 (MБ) | Максимальное выделение памяти в МБ для каждого запроса контейнера в Resource Manager. Запросы на память выше этого значения не будут выполнены. |
| yarn.scheduler.maximum-allocation-vcores | 12 |Максимальное количество ядер ЦП для каждого запроса контейнера в Resource Manager. Запросы на получение больших значений не будут выполнены. |
| yarn.scheduler.capacity.root.llap.capacity | 90 % | Распределение емкости YARN в очереди LLAP.  |
| hive.server2.tez.sessions.per.default.queue | количество_рабочих_узлов |Число сеансов для каждой очереди, заданной в hive.server2.tez.default.queues. Это число соответствует числу координаторов запросов (Tez AM). |
| tez.am.resource.memory.mb | 4096 (МБ) | Объем памяти в МБ, используемый Tez AppMaster. |
| hive.tez.container.size | 4096 (МБ) | Заданный размер контейнера Tez в МБ. |
| hive.llap.daemon.num.executors | 12 | Число исполнителей на одну управляющую программу LLAP. |
| hive.llap.io.threadpool.size | 12 | Размер пула потоков для исполнителей. |
| hive.llap.daemon.yarn.container.mb | 86016 (MБ) | Общий объем памяти в МБ, используемый отдельными управляющими программами LLAP (памяти на одну управляющую программу).|
| hive.llap.io.memory.size | 409600 (MБ) | Размер кэша в МБ на одну управляющую программу LLAP пр условии, что кэш SSD включен. |
| hive.auto.convert.join.noconditionaltask.size | 2048 (МБ) | Объем памяти в МБ для соединения с сопоставлением. |

## <a name="llap-daemon-size-estimations"></a>Оценка размеров управляющей программы LLAP  

### <a name="yarnnodemanagerresourcememory-mb"></a>yarn.nodemanager.resource.memory-mb

Это значение указывает максимальный общий памяти в МБ, используемой контейнерами YARN на каждом узле. Оно определяет объем памяти, который YARN может использовать на этом узле, поэтому это значение должно быть меньше, чем общий объем памяти на узле.

Установите значение = [общий объем физической памяти на узле] – [память для ОС и других служб].

Рекомендуется присвоить этому параметру значение около 90 % от всей доступной оперативной памяти. Для D14 v2 рекомендуется значение **102400 МБ**.

### <a name="yarnschedulermaximum-allocation-mb"></a>yarn.scheduler.maximum-allocation-mb

Это значение указывает максимальное выделение памяти в МБ для каждого запроса контейнера в Resource Manager. Запросы на память выше указанного значения не будут выполнены. Resource Manager может предоставлять память контейнерам только с приращением `yarn.scheduler.minimum-allocation-mb` и не может выйти за размер, указанный в параметре `yarn.scheduler.maximum-allocation-mb`. Это значение не должно превышать общую память узла, указанную в `yarn.nodemanager.resource.memory-mb`.

Для рабочего узла D14 v2 рекомендуется значение **102400 МБ**

### <a name="yarnschedulermaximum-allocation-vcores"></a>yarn.scheduler.maximum-allocation-vcores

Этот параметр конфигурации указывает максимальное количество ядер виртуального ЦП для каждого запроса контейнера в Resource Manager. Запрос большего количества ядер не будет выполнен. Эта конфигурация является глобальным свойством планировщика YARN. Для контейнера управляющей программы LLAP это значение можно задать равным 75 % от общего числа доступных виртуальных ядер. Оставшиеся 25 % должны быть зарезервированы для NodeManager, DataNode и других служб, работающих на рабочих узлах.  

В рабочих узлах D14 v2 16 виртуальных ядер, и можно выделить 75 % из них. Поэтому рекомендуемое значение для контейнера управляющей программы LLAP равно **12**.

### <a name="hiveserver2tezsessionsperdefaultqueue"></a>hive.server2.tez.sessions.per.default.queue

Это значение конфигурации определяет количество сеансов Tez, которые должны быть запущены параллельно для каждой очереди, заданной в `hive.server2.tez.default.queues`. Значение соответствует числу Tez AM (координаторов запросов). Рекомендуется, чтобы это значение совпадало с количеством рабочих узлов, чтобы иметь один Tez AM на узел. Число Tez AM может быть выше, чем число узлов управляющей программы LLAP. Их основная обязанность заключается в координации выполнения запроса и назначении фрагментов плана запроса соответствующим управляющим программам LLAP для выполнения. Для достижения более высокой пропускной способности рекомендуется, чтобы этот параметр представлял собой произведение числа узлов управляющей программы LLAP.  

Кластер HDInsight по умолчанию имеет четыре управляющие программы LLAP, выполняющиеся на четырех рабочих узлах, поэтому рекомендуемое значение равно **4**.  

### <a name="tezamresourcememorymb-hivetezcontainersize"></a>tez.am.resource.memory.mb, hive.tez.container.size

`tez.am.resource.memory.mb` определяет размер мастер-приложения Tez.  
Рекомендуется использовать значение **4096 МБ**.

`hive.tez.container.size` определяет объем памяти, выделяемый контейнеру Tez. Это значение должно быть задано между минимальным размером контейнера YARN (`yarn.scheduler.minimum-allocation-mb`) и максимальным размером контейнера YARN (`yarn.scheduler.maximum-allocation-mb`).  
Рекомендуется установить значение **4096 МБ**.  

Общее правило заключается в том, чтобы значение не превышало объема памяти на каждый процессор, при условии наличия одного процессора на контейнер. `Rreserve` память для всех Tez AM на узле перед предоставлением памяти для управляющей программы LLAP. Например, если вы используете два Tez AM (4 ГБ каждый) на узел, выделите 82 из 90 ГБ для управляющей программы LLAP, зарезервировав 8 ГБ для двух Tez AM.

### <a name="yarnschedulercapacityrootllapcapacity"></a>yarn.scheduler.capacity.root.llap.capacity

Это значение указывает процент емкости, выделенной для очереди LLAP. Кластер Interactive Query HDInsight выделяет 90 % от общей емкости для очереди LLAP, а оставшиеся 10 % — очереди по умолчанию для других операций выделения контейнеров.  
В рабочем узле D14 v2 для очереди LLAP рекомендуется значение **90**.

### <a name="hivellapdaemonyarncontainermb"></a>hive.llap.daemon.yarn.container.mb

Общий объем памяти для управляющей программы LLAP зависит от следующих компонентов.

* Настройка размеров контейнера YARN (`yarn.scheduler.maximum-allocation-mb`, `yarn.scheduler.maximum-allocation-mb`, `yarn.nodemanager.resource.memory-mb`)

* Память кучи, используемая исполнителями (Xmx)

    Доступный объем ОЗУ после резервирования запаса:  
    Для D14 v2 HDI 4.0 это значение (86–6) = 80 ГБ  
    Для D14 v2 HDI 3.6 это значение (84–6) = 78 ГБ

* Кэш вне памяти для каждой управляющей программы (hive.llap.io.memory.size)

* Запас

    Это часть памяти вне кучи, используемая для нагрузки на виртуальной машине Java (метапространство, стек потоков, структуры данных gc и т. д.). Эта часть составляет около 6 % размера кучи (Xmx). Для большей уверенности ее можно рассчитать как 6 % от общего объема памяти управляющей программы LLAP. Поскольку это возможно, когда включен кэш SSD, управляющая программа LLAP сможет использовать все доступное пространство в памяти только для кучи.  
    Для D14 v2 рекомендуемым значением является ceil(86 ГБ x 0,06) ~= **6 ГБ**.  

Памяти на управляющую программу = [размер кэша в памяти] + [размер кучи] + [запас].

Это можно вычислить следующим образом.

Памяти Tez AM на узел = [(Количество Tez AM / Количество узлов управляющей программы LLAP) * Размер Tez AM].
Размер контейнера управляющей программы LLAP = [90 % от макс. памяти контейнера YARN] – [Память Tez AM на узел].

Для рабочего узла D14 v2 HDI 4.0 рекомендуется значение (90 – (1/1 * 4 ГБ)) = **86 ГБ**.
(Для HDI 3.6 рекомендуется значение **84 ГБ**, так как необходимо зарезервировать ~2 ГБ для Slider AM.)  

### <a name="hivellapiomemorysize"></a>hive.llap.io.memory.size

Этот параметр — это объем памяти, доступный для управляющей программы LLAP в качестве кэша. Управляющие программы LLAP могут использовать диск SSD в качестве кэша. Параметр `hive.llap.io.allocator.mmap` = true включает кэширование на SSD. D14 v2 поставляется с диском SSD ~800 ГБ, а кэширование SSD включено по умолчанию для кластера Interactive Query (LLAP). Оно настроено на использование 50 % пространства SSD для кэша вне кучи.

Для D14 v2 рекомендуется значение **409600 МБ**.  

Для других виртуальных машин, для которых не включено кэширование SSD, полезно предоставить часть доступной оперативной памяти для кэширования LLAP, чтобы добиться лучшей производительности. Измените общий объем памяти для управляющей программы LLAP следующим образом.  

Общая память управляющей программы LLAP = [размер кэша LLAP] + [размер кучи] + [запас].

Рекомендуется установить размер кэша и размер кучи, которые лучше всего подходят для вашей рабочей нагрузки.  

### <a name="hivellapdaemonnumexecutors"></a>hive.llap.daemon.num.executors

Этот параметр управляет количеством исполнителей, которые могут выполнять задачи параллельно, для каждой управляющей программы LLAP. Это значение представляет собой баланс количества доступных виртуальных ядер, объема памяти, выделенного для каждого исполнителя, и общего объема памяти, доступного для управляющей программы LLAP. Как правило, это значение должно быть максимально близко к количеству ядер.

В D14 v2 доступно 16 виртуальных ядер, но не все из них могут быть предоставлены. На рабочих узлах также выполняются другие службы, такие как NodeManager, DataNode и Metrics Monitor, которым требуется определенная часть доступных виртуальных ядер. Это значение можно настроить до 75 % от общего числа виртуальных ядер, доступных на этом узле.

Для D14 v2 рекомендуется значение (0,75 x 16) = **12**

Рекомендуется резервировать ~6 ГБ пространства кучи на каждый исполнитель. Настраивайте количество исполнителей на основе доступного размера управляющей программы LLAP и количества доступных виртуальных ядер на каждом узле.  

### <a name="hivellapiothreadpoolsize"></a>hive.llap.io.threadpool.size

Это значение задает размер пула потоков для исполнителей. Так как количество исполнителей зафиксировано в соответствии с заданным параметром, значение будет совпадать с количеством исполнителей на управляющую программу LLAP.

Для D14 v2 рекомендуется присвоить этому параметру значение **12**.

Значение этого параметра не может превышать значения `yarn.nodemanager.resource.cpu-vcores`.

### <a name="hiveautoconvertjoinnoconditionaltasksize"></a>hive.auto.convert.join.noconditionaltask.size

Убедитесь, что у вас включен `hive.auto.convert.join.noconditionaltask`, чтобы этот параметр вступил в силу. Этот параметр позволяет пользователю указать размер таблиц, которые могут уместиться в памяти, для соединения с сопоставлением. Если сумма размеров n-1 `tables/partitions` для n-уровневого соединения меньше заданного значения, будет выбрано соединение с сопоставлением. Для вычисления порогового значения для автопреобразования в соединение с сопоставлением следует использовать размер памяти исполнителя LLAP.

Для D14 v2 рекомендуется присвоить этому параметру значение **2048 МБ**.

Рекомендуется настроить это значение в соответствии с вашей рабочей нагрузкой, так как слишком низкое значение не позволит использовать функцию автопреобразования. Слишком большое значение может привести к паузам для сборки мусора, что негативно скажется на производительности запросов.

## <a name="next-steps"></a>Дальнейшие действия

* [Руководство по шлюзам](gateway-best-practices.md)
* [Секреты настройки памяти для Apache Tez — пошаговое руководство](https://community.cloudera.com/t5/Community-Articles/Demystify-Apache-Tez-Memory-Tuning-Step-by-Step/ta-p/245279)
* [Определение размера памяти для соединения с сопоставлением для LLAP](https://community.cloudera.com/t5/Community-Articles/Map-Join-Memory-Sizing-For-LLAP/ta-p/247462)
* [LLAP: обзор архитектуры на одной странице](https://community.cloudera.com/t5/Community-Articles/LLAP-a-one-page-architecture-overview/ta-p/247439)
* [Погружение в Hive LLAP](https://community.cloudera.com/t5/Community-Articles/Hive-LLAP-deep-dive/ta-p/248893)
