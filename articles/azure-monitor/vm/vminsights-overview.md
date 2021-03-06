---
title: Что такое VM Insights?
description: Обзор VM Insights, который отслеживает работоспособность и производительность виртуальных машин Azure и автоматически обнаруживает и сопоставляет компоненты приложений и их зависимости.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/22/2020
ms.openlocfilehash: 18e1fdcdee347a057c452f6170f36ec7f1f43244
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102046420"
---
# <a name="overview-of-vm-insights"></a>Общие сведения о VM Insights

Служба "аналитика VM" отслеживает производительность и работоспособность виртуальных машин и масштабируемых наборов виртуальных машин, включая выполняющиеся процессы и зависимости от других ресурсов. Это помогает обеспечить предсказуемую производительность и доступность важнейших приложений за счет выявления узких мест производительности и сетевых проблем, а также помогает понять, связана ли проблема с другими зависимостями.

VM Insights поддерживает операционные системы Windows и Linux на следующих компьютерах:

- Виртуальные машины Azure
- Масштабируемые наборы виртуальных машин Azure
- Гибридные виртуальные машины, подключенные к службе "Дуга Azure"
- Локальные виртуальные машины
- Виртуальные машины, размещенные в другой облачной среде
  

Функция "аналитика VM" хранит свои данные в журналах Azure Monitor, что позволяет ему обеспечивать эффективную статистическую обработку и фильтрацию и анализировать тенденции данных с течением времени. Вы можете просмотреть эти данные на одной ВИРТУАЛЬНОЙ машине непосредственно из виртуальной машины или использовать Azure Monitor для предоставления агрегированного представления нескольких виртуальных машин.

![Перспектива полезных сведений о виртуальной машине на портале Azure](media/vminsights-overview/vminsights-azmon-directvm.png)


## <a name="pricing"></a>Цены
Нет прямой платы за использование VM Insights, но плата взимается за ее активность в Log Analytics рабочей области. На основе цен, опубликованных на [странице цен на Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/), счета за использование VM Insights выставляются за:

- Данные, полученные от агентов и хранимые в рабочей области.
- Данные о состоянии работоспособности, собранные из работоспособности гостя (Предварительная версия)
- Правила генерации оповещений на основе данных журнала и работоспособности.
- Уведомления, отправленные из правил генерации оповещений.

Размер журнала зависит от длины строк счетчиков производительности и может увеличиваться с учетом количества логических дисков и сетевых адаптеров, выделенных для виртуальной машины. Если вы уже используете Сопоставление служб, единственное изменение, которое вы увидите, — это дополнительные данные о производительности, которые отправляются в Azure Monitorный `InsightsMetrics` тип данных.


## <a name="configuring-vm-insights"></a>Настройка аналитики виртуальной машины
Ниже приведены действия по настройке аналитики виртуальной машины. Чтобы получить подробные инструкции по каждому шагу, выполните следующие действия.

- [Создайте рабочую область Log Analytics.](./vminsights-configure-workspace.md#create-log-analytics-workspace)
- [Добавьте решение Вминсигхтс в рабочую область.](./vminsights-configure-workspace.md#add-vminsights-solution-to-workspace)
- [Установите агенты на виртуальной машине и масштабируемом наборе виртуальных машин для мониторинга.](./vminsights-enable-overview.md)



## <a name="next-steps"></a>Следующие шаги

- Требования и методы, позволяющие включить мониторинг для виртуальных машин, см. в статье [развертывание VM Insights](./vminsights-enable-overview.md) .
