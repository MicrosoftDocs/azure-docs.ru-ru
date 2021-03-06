---
title: Мониторинг уровня приложения Service Fabric Azure
description: Сведения о журналах и событиях на уровне приложений и служб, используемых для мониторинга и диагностики кластеров Azure Service Fabric.
ms.topic: conceptual
ms.date: 11/21/2018
ms.openlocfilehash: a60eef008afae4185acc266c74c4fb0ce694d560
ms.sourcegitcommit: a9ce1da049c019c86063acf442bb13f5a0dde213
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105627495"
---
# <a name="application-logging"></a>Ведение журнала приложения

Инструментирование кода — не только метод получения аналитических сведений о пользователях, но и единственный способ проверить наличие проблем в приложении, а также выяснить, где требуются исправления. Технически есть возможность подключить отладчик к рабочей службе, но обычно так никто не делает. Поэтому вам нужны подробные данные инструментирования.

Некоторые решения выполняют инструментирование кода автоматически. Они могут вполне успешно работать, но почти всегда требуется инструментирование вручную, которое должно соответствовать используемой бизнес-логике. В итоге у вас должно быть достаточно информации для экспертной отладки приложения. Приложения Service Fabric можно инструментировать на любой платформе ведения журналов. В этом документе описывается несколько различных подходов к инструментированию кода и преимущества этих подходов для разных ситуаций. 

Примеры использования этих подходов см. в статье [Добавление ведения журнала в приложение Service Fabric](service-fabric-how-to-diagnostics-log.md).

## <a name="application-insights-sdk"></a>Пакет SDK для Application Insights

Azure Application Insights поставляется с широкими возможностями интеграции с Azure Service Fabric. Пользователи могут добавлять пакеты NuGet ИИ Service Fabric и получать данные и журналы, созданные и собранные готовыми к просмотру на портале Azure. Кроме того пользователям рекомендуется добавлять свои собственные данные телеметрии для диагностики и отладки своих приложений, а также получения сведения о самых используемых службах и частей приложений. Класс [TelemetryClient](/dotnet/api/microsoft.applicationinsights.telemetryclient) в пакете SDK предоставляет много способов отслеживать данные телеметрии в приложении. Дополнительные сведения об инструментировании и добавлении Application Insights в приложение см. руководстве по [наблюдению и диагностике приложений .NET](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="eventsource"></a>EventSource

Если вы создаете решение Service Fabric из шаблона в Visual Studio, в нем создается класс **ServiceEventSource** или **ActorEventSource**, производный от **EventSource**. При этом создается шаблон, в который вы можете добавлять события для конкретного приложения или службы. Имя **EventSource** **должно** быть уникальным и должно быть переименовано из строки шаблона по умолчанию — &lt; проекта MyCompany-Solution &gt; - &lt; &gt; . Наличие нескольких определений **EventSource** с одинаковым именем приведет к возникновению проблемы во время выполнения. У каждого определенного события должен быть уникальный идентификатор. Если идентификатор не является уникальным, происходит сбой во время выполнения. Некоторые организации предварительно назначают для идентификаторов диапазоны значений, чтобы избежать конфликтов между командами разработчиков. Дополнительные сведения см. в [блоге Вэнса (Vance)](/archive/blogs/vancem/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource) или в [документации MSDN](/previous-versions/msp-n-p/dn774985(v=pandp.20)).

## <a name="aspnet-core-logging"></a>Ведение журнала ASP.NET Core

Очень важно тщательно спланировать методы инструментирования кода. Правильный план инструментирования позволит избежать потенциальной дестабилизации базы кода, которая повлечет за собой повторное инструментрирование кода. Чтобы снизить риск, вы можете применить библиотеку инструментирования, например [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), которая входит в Microsoft ASP.NET Core. ASP.NET Core предоставляет интерфейс [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger), который можно подключить к любому поставщику, с минимальными изменениями существующего кода. Код ASP.NET Core можно использовать на платформах Windows, Linux и в полной версии .NET Framework, то есть код инструментирования будет полностью стандартизирован.

## <a name="next-steps"></a>Дальнейшие шаги

После выбора поставщика ведения журнала для инструментирования приложений и служб вам потребуется агрегировать журналы и события перед отправкой в любую платформу аналитики. См. дополнительные сведения об [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) и [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md), чтобы ознакомиться с некоторыми рекомендуемыми параметрами Azure Monitor.
