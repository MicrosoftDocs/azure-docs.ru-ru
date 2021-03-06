---
title: Настройка расписания обновления ОС для кластеров Azure HDInsight
description: Узнайте, как настроить расписание обновления путем частичной замены ОС для кластеров HDInsight.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 01/21/2020
ms.openlocfilehash: 636caf592baa4df771f7cc50095911d0337456d0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98939392"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Настройка расписания исправлений ОС для кластеров HDInsight под управлением Linux

> [!IMPORTANT]
> Образы Ubuntu становятся доступными для нового создания кластера Azure HDInsight в течение трех месяцев публикации. Работающие кластеры не устанавливаются с автоматической установкой исправлений. Клиенты должны использовать действия сценария или другие механизмы для исправления работающего кластера. Рекомендуется выполнить эти действия сценария и применить обновления для системы безопасности сразу после создания кластера.

HDInsight предоставляет поддержку для выполнения общих задач в кластере, например для установки исправлений ОС, обновлений безопасности и перезагрузки узлов. Эти задачи выполняются с помощью следующих двух скриптов, которые могут выполняться как [действия сценария](hdinsight-hadoop-customize-cluster-linux.md)и настроены с параметрами.

- `schedule-reboots.sh` — Выполните немедленное перезагрузку или запланируйте перезапуск на узлах кластера.
- `install-updates-schedule-reboots.sh` — Установите все обновления, только обновления ядра, системы безопасности или только обновления ядра.

> [!NOTE]  
> Действия скрипта не будут автоматически применять обновления для всех будущих циклов обновления. Запускайте скрипты каждый раз, когда необходимо применить новые обновления для установки обновлений, а затем перезапустите виртуальную машину.

## <a name="preparation"></a>Подготовка

Исправление на репрезентативной нерабочей среде перед развертыванием в рабочей версии. Разработайте план для адекватного тестирования системы до фактической установки исправлений.

От времени до времени из сеанса SSH с кластером может появиться сообщение о наличии обновлений для системы безопасности. Сообщение может выглядеть примерно так:

```
89 packages can be updated.
82 updates are security updates.

*** System restart required ***

Welcome to Spark on HDInsight.

```

Установка исправлений является необязательной и по своему усмотрению.

## <a name="restart-nodes"></a>Перезапустить узлы
  
[Расписание — перезагрузка](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh)— задает тип перезагрузки, который будет выполняться на компьютерах в кластере. При отправке действия скрипта настройте его для применения ко всем трем типам узлов: головной узел, Рабочий узел и Zookeeper. Если скрипт не применяется к типу узла, виртуальные машины для этого типа узла не будут обновлены или перезапущены.

`schedule-reboots script`Принимает один числовой параметр:

| Параметр | Допустимые значения | Определение |
| --- | --- | --- |
| Тип выполняемой перезагрузки | 1 или 2 | Значение 1 включает перезапуск по расписанию (запланированный через 12-24 часов). Значение 2 включает немедленное перезагрузку (за 5 минут). Если параметр не задан, по умолчанию используется значение 1. |  

## <a name="install-updates-and-restart-nodes"></a>Установка обновлений и перезапуск узлов

Сценарий [install-updates-Schedule-reboots.sh](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh) предоставляет параметры для установки различных типов обновлений и ПЕРЕЗАПУСКА виртуальной машины.

`install-updates-schedule-reboots`Сценарий принимает два числовых параметра, как описано в следующей таблице.

| Параметр | Допустимые значения | Определение |
| --- | --- | --- |
| Тип устанавливаемых обновлений | 0, 1 или 2 | Значение 0 устанавливает только обновления ядра. Значение 1 устанавливает обновления ядра, системы безопасности и 2 устанавливает все обновления. Если параметр не указан, по умолчанию используется значение 0. |
| Тип выполняемой перезагрузки | 0, 1 или 2 | Значение 0 отключает перезапуск. Значение 1 включает перезапуск по расписанию, а 2 — немедленное перезагрузку. Если параметр не указан, по умолчанию используется значение 0. Пользователь должен изменить входной параметр 1 на входной параметр 2. |

> [!NOTE]
> Сценарий необходимо пометить как сохраненный после применения к существующему кластеру. В противном случае все новые узлы, созданные операциями масштабирования, будут использовать расписание обновления путем частичной замены ОС по умолчанию. Если применить сценарий как часть процесса создания кластера, он будет сохранен автоматически.

> [!NOTE]
> Параметр запланированной перезагрузки выполняет автоматическое пошаговое перезагрузку исправленных узлов кластера в течение 12 – 24 часов и учитывает высокий уровень доступности, домен обновления и домен сбоя. Запланированная перезагрузка не прерывает выполняющиеся рабочие нагрузки, но может отказаться от емкости кластера в промежуточном режиме, если узлы недоступны, что увеличивает время обработки. 

## <a name="next-steps"></a>Дальнейшие действия

Конкретные действия по использованию действий скрипта см. в следующих разделах: [Настройка кластеров HDInsight под управлением Linux с помощью действия сценария](hdinsight-hadoop-customize-cluster-linux.md).

- [Использование действия сценария во время создания кластера](hdinsight-hadoop-customize-cluster-linux.md#script-action-during-cluster-creation)
- [Применение действия скрипта в работающем кластере](hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster)
