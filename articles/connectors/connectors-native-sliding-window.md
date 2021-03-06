---
title: Планирование задач по обработке непрерывных данных
description: Создание и выполнение повторяющихся задач, обрабатывающих непрерывные данные с помощью скользящего окна в Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: deli, logicappspm
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: 103805fbf395dc120acc96fbcee273abcf14939d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "96010424"
---
# <a name="schedule-and-run-tasks-for-contiguous-data-by-using-the-sliding-window-trigger-in-azure-logic-apps"></a>Планирование и запуск задач для непрерывных данных с помощью триггера скользящего окна в Azure Logic Apps

Чтобы регулярно запускать задачи, процессы или задания, которые должны обрабатывать данные в смежных фрагментах, можно запустить рабочий процесс приложения логики с помощью триггера **скользящего окна** . Можно задать дату и время, а также часовой пояс для запуска рабочего процесса и повторение для повторения этого рабочего процесса. Если по какой-либо причине повторение не проводилось, например в результате нарушений или отключенных рабочих процессов, этот триггер обрабатывает пропущенные повторения. Например, при синхронизации данных между базой данных и хранилищем резервных копий используйте триггер скользящего окна, чтобы данные синхронизировались без нерегулярных пробелов. Дополнительные сведения о встроенных триггерах и действиях расписания см. в разделе [планирование и запуск повторяющихся автоматизированных, задач и рабочих процессов с Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Ниже приведены некоторые закономерности, которые поддерживает этот триггер:

* Запустить немедленно и повторять каждые *n* часов, минут, ч, дней, недель или месяцев.

* Начните с определенной даты и времени, затем выполните и повторите каждые *n* секунд, минут, часов, дней, недель или месяцев. С помощью этого триггера можно указать время начала в прошлом, которое запускает все предыдущие повторения.

* Задержка каждого повторения в течение определенного времени перед выполнением.

Различия между этим триггером и триггером повторения или дополнительные сведения о расписании повторяющихся рабочих процессов см. [в разделе Планирование и выполнение повторяющихся автоматизированных задач, процессов и рабочих процессов с Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Если вы хотите активировать приложение логики и запустить его только один раз в будущем, см. статью [выполнение заданий только один раз](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас нет ее, вы можете [зарегистрироваться для получения бесплатной учетной записи Azure](https://azure.microsoft.com/free/).

* Базовые сведения о [приложениях логики](../logic-apps/logic-apps-overview.md). Если вы пока не знакомы с приложениями логики, изучите статью [о создании первого приложения логики](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-sliding-window-trigger"></a>Добавить триггер скользящего окна

1. Войдите на [портал Azure](https://portal.azure.com). Создание пустого приложения логики.

1. После появления конструктора приложений логики в поле поиска введите `sliding window` фильтр. В списке триггеры Выберите триггер **скользящего окна** в качестве первого шага в рабочем процессе приложения логики.

   ![Выбор триггера скользящего окна](./media/connectors-native-sliding-window/add-sliding-window-trigger.png)

1. Задайте интервал повторения и единицу времени. В этом примере задайте запуск рабочего процесса каждый день.

   ![Установка интервала и частоты](./media/connectors-native-sliding-window/sliding-window-trigger-details.png)

   | Свойство. | Имя JSON | Обязательно | Тип | Описание |
   |----------|----------|-----------|------|-------------|
   | **Интервал** | `interval` | Да | Целое число | Положительное целое число, которое описывает, как часто выполняется рабочий процесс с учетом заданной частоты. Ниже приведены минимальный и максимальный интервалы. <p>— Месяц: 1–16 месяцев; <br>— Неделя: 1-71 недель <br>— День: 1–500 дней; <br>— Час: 1–12 000 часов; <br>— Минута: 1–72 000 минут; <br>— Секунда: 1–9 999 999 секунд. <p>Например, если интервал равен 6, а значение частоты — "Месяц", то повтор будет происходить каждые 6 месяцев. |
   | **Частота** | `frequency` | Да | Строка | Единица времени для повторения: **Секунда**, **Минута**, **Час**, **День**, **Неделя** или **Месяц**. |
   ||||||

   ![Дополнительные параметры повторения](./media/connectors-native-sliding-window/sliding-window-trigger-more-options-details.png)

   Для получения дополнительных параметров повторения откройте список **Добавить новый параметр** . Все выбранные параметры отображаются в триггере после выбора.

   | Свойство | Обязательно | Имя JSON | Type | Описание |
   |----------|----------|-----------|------|-------------|
   | **Задержка** | Нет | delay | Строка | Продолжительность задержки каждого повторения с использованием [спецификации даты и времени ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Durations) |
   | **Часовой пояс** | Нет | timeZone | Строка | Применяется только при указании времени начала, так как этот триггер не принимает [смещение от UTC](https://en.wikipedia.org/wiki/UTC_offset). Выберите часовой пояс, который необходимо применить. |
   | **Время начала** | Нет | startTime | Строка | Укажите дату и время начала в следующем формате: <p>ГГГГ-ММ-ДДTчч:мм:сс, если выбран часовой пояс <p>-или- <p>ГГГГ-ММ-ДДTчч:мм:ссZ, если часовой пояс не выбран. <p>Например, если вы хотите 18 сентября 2017 в 2:00 PM, укажите "2017-09-18T14:00:00" и выберите часовой пояс, например тихоокеанское время (зима). Или укажите 2017-09-18T14:00:00Z без часового пояса. <p>**Примечание.** Это время начала должно соответствовать [спецификации даты и времени ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) в [формате UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), но без [смещения от UTC](https://en.wikipedia.org/wiki/UTC_offset). Если вы не выберите часовой пояс, то необходимо в конце добавить букву Z без пробелов. Эта буква Z ссылается на соответствующее [судовое время](https://en.wikipedia.org/wiki/Nautical_time). <p>Для простых расписаний время начала является первым, а для сложных повторений триггер не срабатывает раньше, чем время начала. [*Как можно использовать дату и время начала?*](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   |||||

1. Теперь создайте оставшийся рабочий процесс с другими действиями. Дополнительные действия, которые можно добавить, см. в разделе [соединители для Azure Logic Apps](../connectors/apis-list.md).

## <a name="workflow-definition---sliding-window"></a>Определение рабочего процесса — скользящее окно

В базовом определении рабочего процесса приложения логики, использующем JSON, можно просмотреть определение триггера скользящего окна с выбранными параметрами. Чтобы просмотреть это определение, на панели инструментов конструктора выберите **представление кода**. Чтобы вернуться к конструктору, выберите панель инструментов конструктора, **конструктор**.

В этом примере показано, как определение триггера скользящего окна может просмотреть базовое определение рабочего процесса, где задержка для каждого повторения составляет пять секунд для почасового повторения:

``` json
"triggers": {
   "Recurrence": {
      "type": "SlidingWindow",
      "Sliding_Window": {
         "inputs": {
            "delay": "PT5S"
         },
         "recurrence": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2019-05-13T14:00:00Z",
            "timeZone": "Pacific Standard Time"
         }
      }
   }
}
```

## <a name="next-steps"></a>Дальнейшие действия

* [Отложить следующее действие в рабочих процессах](../connectors/connectors-native-delay.md)
* [Соединители для Logic Apps](../connectors/apis-list.md)
