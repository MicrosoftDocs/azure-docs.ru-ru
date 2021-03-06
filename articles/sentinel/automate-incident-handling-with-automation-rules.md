---
title: Автоматизация обработки инцидентов в Azure Sentinel | Документация Майкрософт
description: В этой статье объясняется, как использовать правила автоматизации для автоматизации обработки инцидентов, чтобы повысить эффективность и эффективность SOC в ответ на угрозы безопасности.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2021
ms.author: yelevin
ms.openlocfilehash: 1ff9fbbb6cd4b8827555a6cb1b222ed4eb0a5299
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104609814"
---
# <a name="automate-incident-handling-in-azure-sentinel-with-automation-rules"></a>Автоматизация обработки инцидентов в Azure Sentinel с помощью правил автоматизации

В этой статье объясняется, что такое правила автоматизации маркеров Azure и как их использовать для реализации операций оркестрации, автоматизации и реагирования (ВЗЛЕТЕЛ), повышения эффективности SOC и экономии времени и ресурсов.

> [!IMPORTANT]
>
> - В настоящее время функция **правил автоматизации** доступна в **предварительной версии**. Ознакомьтесь с дополнительными [условиями использования Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) предварительных версий для дополнительных юридических условий, которые относятся к функциям Azure, которые доступны в бета-версии, предварительном просмотре или еще не выпущены в общедоступную версию.

## <a name="what-are-automation-rules"></a>Что такое правила автоматизации?

Правила автоматизации — это новая концепция в Azure Sentinel. Эта функция позволяет пользователям централизованно управлять автоматизацией обработки инцидентов. Помимо того, что вы можете назначать модули PlayBook инцидентам (а не только предупреждениям, как раньше), правила автоматизации позволяют автоматизировать ответы для нескольких правил аналитики одновременно, автоматически размечать, назначать или закрывать инциденты без необходимости модули playbook и управлять порядком выполнения выполняемых действий. Правила автоматизации упрощают использование службы автоматизации в Azure Sentinel и позволяют упростить сложные рабочие процессы для процессов оркестрации инцидентов.

## <a name="components"></a>Компоненты

Правила автоматизации состоят из нескольких компонентов:

### <a name="trigger"></a>Триггер

Правила автоматизации активируются при создании инцидента. 

Для просмотра — инциденты создаются на основе предупреждений по правилам аналитики, из которых существуют четыре типа, как описано в руководстве [обнаружение угроз с помощью встроенных правил аналитики в Sentinel](tutorial-detect-threats-built-in.md).

### <a name="conditions"></a>Условия

Сложные наборы условий можно определить, чтобы управлять выполнением действий (см. ниже). Эти условия обычно основываются на состояниях или значениях атрибутов инцидентов и их сущностях и могут включать `AND` / `OR` / `NOT` / `CONTAINS` операторы.

### <a name="actions"></a>Действия

Действия могут быть определены для выполнения при соблюдении условий (см. выше). В правиле можно определить множество действий, и можно выбрать порядок их выполнения (см. ниже). С помощью правил автоматизации можно определить следующие действия, не требуя [дополнительных функциональных возможностей сборник тренировочных заданий](automate-responses-with-playbooks.md):

- Изменение состояния инцидента с обновлением рабочего процесса.

  - При переходе на "Closed", указывая [причину закрытия](tutorial-investigate-cases.md#closing-an-incident) и добавляя комментарий. Это позволяет отследить производительность и эффективность, а также настроить для сокращения ложных срабатываний.

- Изменение серьезности инцидента — можно переоценивать и изменять приоритеты в зависимости от наличия, отсутствия, значений или атрибутов сущностей, участвующих в инциденте.

- Назначение инцидента владельцу — это помогает вам направить типы инцидентов сотрудникам, которые лучше всего подходят для работы с ними, или к большинству доступных сотрудников.

- Добавление тега к инциденту — это полезно для классификации инцидентов по темам, злоумышленникам или другим общим знаменателям.

Кроме того, можно определить действие для запуска [сборник тренировочных заданий](tutorial-respond-threats-playbook.md), чтобы выполнить более сложные действия ответа, включая все, что затрагивает внешние системы. В правилах автоматизации можно использовать только модули PlayBook, активируемые [триггером инцидента](automate-responses-with-playbooks.md#azure-logic-apps-basic-concepts) . Можно определить действие для включения нескольких модули PlayBook или сочетаний модули playbook и других действий, а также порядок их запуска.

### <a name="expiration-date"></a>Срок действия

Вы можете определить дату истечения срока действия правила автоматизации. После этой даты правило будет отключено. Это полезно для обработки (т. е. закрытия) инцидентов, вызванных запланированными действиями с ограниченным временем, такими как тестирование на проникновение.

### <a name="order"></a>Номер

Можно определить порядок, в котором будут выполняться правила автоматизации. Последующие правила автоматизации будут оценивать условия инцидента в соответствии с его состоянием после выполнения действий с помощью предыдущих правил автоматизации.

Например, если "первое правило автоматизации" изменило серьезность инцидента с среднего на низкий, а "второе правило автоматизации" будет выполняться только для инцидентов со средним или более высоким уровнем серьезности, оно не будет выполняться в этом инциденте.

## <a name="common-use-cases-and-scenarios"></a>Распространенные варианты использования и сценарии

### <a name="incident-triggered-automation"></a>Автоматизация, активируемая по инциденту

До настоящего момента только предупреждения могут активировать автоматический ответ с помощью модули PlayBook. С помощью правил автоматизации инциденты теперь могут запускать автоматические цепочки ответов, которые могут включать новые модули PlayBook, активируемые инцидентом ([требуются специальные разрешения](#permissions-for-automation-rules-to-run-playbooks)) при создании инцидента. 

### <a name="trigger-playbooks-for-microsoft-providers"></a>Активация модули PlayBook для поставщиков Майкрософт

Правила автоматизации позволяют автоматизировать обработку оповещений системы безопасности Майкрософт, применяя эти правила к инцидентам, созданным на основе оповещений. Правила автоматизации могут вызывать модули PlayBook ([требуются специальные разрешения](#permissions-for-automation-rules-to-run-playbooks)) и передавать инциденты в них со всеми подробностями, включая оповещения и сущности. Как правило, передовые методики Azure Sentinel используют очередь инцидентов в качестве фокальной точки операций безопасности.

В число оповещений системы безопасности Microsoft входят следующие.

- Microsoft Cloud App Security (MCAS)
- Защита идентификации Azure AD
- Защитник Azure (ASC)
- Defender для Интернета вещей (прежнее название — ASC для Интернета вещей)
- Microsoft Defender для Office 365 (прежнее название — Office 365 ATP)
- Microsoft Defender для конечной точки (ранее — MDATP)
- Защитник Майкрософт для идентификации (прежнее название — Azure ATP)

### <a name="multiple-sequenced-playbooksactions-in-a-single-rule"></a>Несколько последовательных модули PlayBook/действий в одном правиле

Теперь вы можете получить практически полный контроль над порядком выполнения действий и модули Playbook в одном правиле автоматизации. Вы также управляете порядком выполнения самих правил автоматизации. Это позволяет значительно упростить модули PlayBook, сокращая их до одной задачи или небольшой, простой последовательности задач и объединив эти небольшие модули Playbook в разных комбинациях в различных правилах автоматизации.

### <a name="assign-one-playbook-to-multiple-analytics-rules-at-once"></a>Назначение одного сборник тренировочных заданий нескольким правилам аналитики одновременно

Если у вас есть задача, которую необходимо автоматизировать для всех правил аналитики, скажем, создание запроса в службу поддержки во внешней системе обработки билетов, вы можете применить один сборник тренировочных заданий к любому или всем правилам аналитики, включая все будущие правила, в одном снимке. Это делает простыми, но повторяющиеся задачи обслуживания, которые гораздо меньше.

### <a name="automatic-assignment-of-incidents"></a>Автоматическое назначение инцидентов

Вы можете автоматически назначать инциденты для правильного владельца. Если у SOC есть аналитик, специализирующийся на определенной платформе, все инциденты, связанные с этой платформой, могут быть автоматически назначены этому аналитику.

### <a name="incident-suppression"></a>Подавление инцидентов

С помощью правил можно автоматически разрешать инциденты с известными ложными или небезопасными положительными результатами без использования модули PlayBook. Например, при выполнении тестов на проникновение, выполнении запланированного обслуживания или обновления или тестировании процедур автоматизации может быть создано множество ложных срабатываний инцидентов, которые необходимо проигнорировать в SOC. Правило автоматизации с ограничением по времени может автоматически закрывать эти инциденты по мере их создания, в то же время размечая их с помощью дескриптора причины их создания.

### <a name="time-limited-automation"></a>Автоматизация с ограниченным временем

Для правил автоматизации можно добавить даты окончания срока действия. Возможны случаи, отличные от подавления инцидентов, которые гарантируют ограниченную автоматизацию времени. Может потребоваться назначить определенный тип инцидента конкретному пользователю (скажем, к InterNIC или консультанту) за определенный период времени. Если период времени известен заранее, это правило может быть отключено в конце релевантности, без необходимости запоминать это.

### <a name="automatically-tag-incidents"></a>Автоматическое обозначение инцидентов

Можно автоматически добавлять в инциденты текстовые теги для группирования или классификации в соответствии с любыми критериями.

## <a name="automation-rules-execution"></a>Выполнение правил автоматизации

Правила автоматизации выполняются последовательно в соответствии с определенным порядком. Каждое правило автоматизации выполняется после того, как предыдущий элемент завершил свою работу. В рамках правила автоматизации все действия выполняются последовательно в том порядке, в котором они определены.

Для действий сборник тренировочных заданий существует две минуты между началом действия сборник тренировочных заданий и следующим действием в списке.

### <a name="permissions-for-automation-rules-to-run-playbooks"></a>Разрешения для выполнения модули PlayBook правил автоматизации

Когда правило службы автоматизации маркеров Azure выполняет сборник тренировочных заданий, оно использует специальную учетную запись службы Azure Sentinel, специально санкционированную для этого действия. Использование этой учетной записи (в отличие от учетной записи пользователя) увеличивает уровень безопасности службы.

Чтобы правило автоматизации выполняло сборник тренировочных заданий, этой учетной записи необходимо предоставить явные разрешения для группы ресурсов, в которой находится сборник тренировочных заданий. На этом этапе любое правило автоматизации сможет выполнять любые сборник тренировочных заданий в этой группе ресурсов.

Когда вы настраиваете правило автоматизации и добавляете действие **Run сборник тренировочных заданий** , появится раскрывающийся список модули PlayBook. Модули PlayBook, к которому метка Azure не имеет разрешений, будет отображаться как недоступная (выделена серым цветом). Вы можете предоставить разрешение на метку Azure для групп ресурсов модули Playbook в месте, выбрав ссылку **Управление разрешениями сборник тренировочных заданий** .

> [!NOTE]
> **Разрешения в архитектуре с несколькими клиентами**
>
> Правила автоматизации полностью поддерживают развертывания между рабочими областями и несколькими клиентами (в случае с несколькими клиентами с помощью [Azure лигхсаусе](extend-sentinel-across-workspaces-tenants.md#managing-workspaces-across-tenants-using-azure-lighthouse)).
>
> Таким образом, если в развертывании Sentinel-меток Azure используется архитектура с несколькими клиентами (например, МССП), вы можете использовать правило автоматизации в одном клиенте для запуска сборник тренировочных заданий, который находится в другом клиенте, но разрешения для Sentinel на выполнение модули PlayBook должны быть определены в клиенте, где размещается модули PlayBook, а не в клиенте, где определены правила автоматизации.

## <a name="creating-and-managing-automation-rules"></a>Создание правил автоматизации и управление ими

Вы можете создавать и администрировать правила автоматизации из различных точек в интерфейсе Sentinel Azure в зависимости от конкретных потребностей и вариантов использования.

- **Колонка автоматизации**

    Правила автоматизации можно централизованно управлять в новой колонке " **Автоматизация** " (которая заменяет колонку **модули PlayBook** ) на вкладке " **правила автоматизации** ". (теперь вы можете управлять модули Playbook в этой колонке на вкладке **модули PlayBook** .) После этого можно создать новые правила автоматизации и изменить существующие. Кроме того, можно перетаскивать правила автоматизации, чтобы изменить порядок выполнения, а также включить или отключить их.

    В колонке **Автоматизация** отображаются все правила, определенные в рабочей области, а также их состояние (включено/отключено) и правила аналитики, к которым они применяются.

    Если требуется правило автоматизации, которое будет применяться ко многим правилам аналитики, создайте его непосредственно в колонке **Автоматизация** . В верхнем меню выберите **создать** и **Добавить новое правило**, после чего откроется панель **Создание правила автоматизации** . Здесь у вас есть полная гибкость в настройке правила: вы можете применить его к любым правилам аналитики (включая будущие) и определить самый широкий диапазон условий и действий.

- **Мастер правил аналитики**

    На вкладке **автоматизированный ответ** мастера правил аналитики можно просматривать, управлять и создавать правила автоматизации, которые применяются к конкретному правилу аналитики, создаваемому или изменяемому в мастере.

    Если щелкнуть **создать** и один из типов правил (**правило запроса** или **правило создания инцидента Майкрософт**) в верхнем меню в колонке **аналитика** или если выбрать существующее правило аналитики и нажать кнопку **изменить**, откроется мастер правил. При выборе вкладки **автоматизированный ответ** вы увидите раздел **Автоматизация инцидентов**, в котором будут отображаться правила автоматизации, применяемые в настоящее время к этому правилу. Можно выбрать существующее правило автоматизации для изменения или нажать кнопку **Добавить новое** , чтобы создать новый.

    Вы заметите, что при создании правила автоматизации отсюда на панели **Создание правила автоматизации** отображается условие **правила аналитики** как недоступное, так как это правило уже настроено для применения только к правилу аналитики, которое вы редактируете в мастере. Все остальные параметры конфигурации по-прежнему доступны вам.

- **Колонка инцидентов**

    Вы также можете создать правило автоматизации в колонке **инциденты** , чтобы ответить на один повторяющийся инцидент. Это полезно при создании [правила подавления](#incident-suppression) для автоматического закрытия "бесшумных" инцидентов. Выберите инцидент из очереди и щелкните **создать правило автоматизации** в верхнем меню.

    Вы заметите, что на панели **Создание правила автоматизации** заполнены все поля со значениями из инцидента. Оно присваивает правилу имя, совпадающее с именем инцидента, применяет его к правилу аналитики, создавшему инцидент, и использует все доступные сущности в инциденте в качестве условий правила. Он также предлагает действие подавления (закрытие) по умолчанию и предлагает дату истечения срока действия правила. Вы можете добавлять или удалять условия и действия, а также изменять дату истечения срока действия по своему усмотрению.

## <a name="auditing-automation-rule-activity"></a>Аудит действия правила автоматизации

Возможно, вам интересно знать, что произошло с конкретным инцидентом и какое правило автоматизации может быть недоступно или не выполнено. Вы можете получить полную запись Хроники инцидентов, доступных в таблице *секуритинЦидент* в колонке **журналы** . Чтобы просмотреть все действия правил автоматизации, используйте следующий запрос:

```kusto
SecurityIncident
| where ModifiedBy contains "Automation"
```

## <a name="next-steps"></a>Дальнейшие действия

В этом документе вы узнали, как использовать правила автоматизации для управления очередью инцидентов Azure Sentinel и реализации базовой автоматизации обработки инцидентов.

- Дополнительные сведения о расширенных возможностях автоматизации см. [в статье Автоматизация реагирования на угрозы с помощью модули Playbook в Azure Sentinel](automate-responses-with-playbooks.md).
- Справку по реализации правил автоматизации и модули PlayBook см. в разделе [учебник. Использование модули PlayBook для автоматизации реагирования угроз в Azure Sentinel](tutorial-respond-threats-playbook.md).
