---
title: Интеграция службы управления API Azure с Azure Application Insights
titleSuffix: Azure API Management
description: Сведения о просмотре и записи в журнал для событий управления API Azure в Azure Application Insights.
services: api-management
author: mikebudzynski
ms.service: api-management
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/25/2021
ms.author: apimpm
ms.openlocfilehash: 97f4eb34b88b3454d65b65d236833e1256c98671
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103564264"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Интеграция управления API Azure в Azure Application Insights

Управление API Azure может быть легко интегрировано в Azure Application Insights. Azure Application Insights — это расширяемая служба управления производительностью приложений (APM), предназначенная для веб-разработчиков, создающих приложения и управляющих ими на нескольких платформах. В этом руководстве описываются все этапы этой интеграции и стратегии для снижения влияния интеграции на производительность вашего экземпляра службы управления API.

## <a name="prerequisites"></a>Предварительные требования

Для выполнения действий, описанных в этом руководстве, вам потребуется экземпляр службы управления API. Если у вас его нет, выполните действия, указанные в [этом руководстве](get-started-create-service-instance.md).

## <a name="create-an-application-insights-instance"></a>Создание экземпляра Application Insights

Прежде чем можно будет использовать Application Insights, необходимо сначала создать экземпляр службы. Действия по созданию экземпляра с помощью портал Azure см. в разделе [ресурсы Application Insights на основе рабочей области](../azure-monitor/app/create-workspace-resource.md).
## <a name="create-a-connection-between-application-insights-and-api-management"></a>Создание подключения между Application Insights и управлением API

1. Перейдите к **экземпляру службы управления API Azure** на **портал Azure**.
1. Выберите **Application Insights** в меню слева.
1. Выберите **+ Добавить**.  
    :::image type="content" source="media/api-management-howto-app-insights/apim-app-insights-logger-1.png" alt-text="Снимок экрана, на котором показано, куда добавить новое подключение":::
1. Выберите ранее созданный экземпляр **Application Insights** и укажите краткое описание.
1. Нажмите кнопку **Создать**.
1. Вы только что создали средство ведения журнала Application Insights с ключом инструментирования. Оно должно появиться в списке.  
    :::image type="content" source="media/api-management-howto-app-insights/apim-app-insights-logger-2.png" alt-text="Снимок экрана, на котором показано, где просмотреть созданное средство ведения журнала Application Insights с помощью ключа инструментирования":::

> [!NOTE]
> Фактически сущность [средства ведения журнала](/rest/api/apimanagement/2019-12-01/logger/createorupdate) создается в экземпляре службы управления API с ключом инструментирования экземпляра Application Insights.

## <a name="enable-application-insights-logging-for-your-api"></a>Включение ведения Application Insights для вашего API

1. Перейдите к **экземпляру службы управления API Azure** на **портал Azure**.
1. Выберите **API** в меню слева.
1. Выберите свой API, в данном случае **Demo Conference API**. Если настроено, выберите версию.
1. Перейдите на вкладку **Параметры** на верхней панели.
1. Прокрутите содержимое вкладки вниз до раздела **Журналы диагностики**.  
    :::image type="content" source="media/api-management-howto-app-insights/apim-app-insights-api-1.png" alt-text="Средство ведения журнала App Insights":::
1. Установите флажок **Включить**.
1. Выберите подключенное средство ведения журнала в раскрывающемся списке **Назначение**.
1. Введите **100** в качестве **выборки (%)** и установите флажок " **всегда записывать ошибки в журнал** ".
1. Щелкните **Сохранить**.

> [!WARNING]
> Переопределение значения по умолчанию **0** в параметре **число байт полезных данных в журнал** может значительно снизить производительность API.

> [!NOTE]
> Фактически сущность [Диагностика](/rest/api/apimanagement/2019-12-01/diagnostic/createorupdate) с именем applicationinsights создается на уровне API.

| Имя параметра                        | Тип значения                        | Описание                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Включить                              | Логическое                           | Указывает, включено ли ведение журнала для данного API.                                                                                                                                                                                                                                                                                                |
| Назначение                         | Средство ведения журнала Azure Application Insights | Указывает используемое средство ведения журнала Azure Application Insights                                                                                                                                                                                                                                                                                           |
| Выборка (%)                        | Decimal                           | Значения от 0 до 100 (в процентах). <br/> Указывает процент запросов, которые будут регистрироваться в Application Insights. Выборка в 0 % означает, что в журнал не будет записан ни один запрос, а выборка в 100 % — что в журнал будут записаны все запросы. <br/> Используйте этот параметр, чтобы снизить производительность при регистрации запросов к Application Insights. См. раздел [влияние производительности и выборка журналов](#performance-implications-and-log-sampling). |
| Всегда записывать ошибки в журнал                   | Логическое                           | Если этот параметр выбран, все сбои будут регистрироваться в Application Insights, независимо от параметра **выборки** .   
| IP-адрес клиента журнала | |  Если этот параметр выбран, IP-адрес клиента для запросов API будет записываться в Application Insights.                                         |
| Уровень детализации         |                                   | Задает уровень детализации. В журнал будут занесены только пользовательские трассировки с более высоким уровнем серьезности. По умолчанию: Information.      | 
| Протокол корреляции |  |  Выберите протокол, используемый для корреляции данных телеметрии, отправляемых несколькими компонентами. По умолчанию: устаревший <br/>Дополнительные сведения см. [в разделе корреляция телеметрии в Application Insights](../azure-monitor/app/correlation.md).  |
| Основные параметры: заголовки для ведения журнала              | list                              | Указывает заголовки, которые будут регистрироваться в Application Insights для запросов и ответов.  По умолчанию: заголовки не записываются в журналы.                                                                                                                                                                                                             |
| Основные параметры: количество байтов полезных данных для записи в журнал | Целое число                           | Указывает, сколько первых байтов тела записывается в Application Insights для запросов и ответов.  По умолчанию: 0.                                                                                                                                                                                                    |
| Дополнительные параметры: запрос клиента  |                                   | Указывает, будет ли и как *запросы к интерфейсу* будут регистрироваться в Application Insights. *Запросы клиента* — это запросы, поступающие к службе управления API Azure.                                                                                                                                                                        |
| Дополнительные параметры: ответ клиента |                                   | Указывает, должны ли и как будут регистрироваться *отклики* интерфейса в Application Insights. *Ответы клиента* — это ответы, возвращаемые службой управления API Azure.                                                                                                                                                                   |
| Дополнительные параметры: запрос сервера   |                                   | Указывает, будет ли и как *серверные запросы* регистрироваться в Application Insights. *Запросы сервера* — это запросы, поступающие от службы управления API Azure.                                                                                                                                                                        |
| Дополнительные параметры: ответ сервера  |                                   | Указывает, будут ли регистрироваться *ответы внутреннего сервера* в Application Insights. *Ответы сервера* — это ответы, поступающие к службе управления API Azure.                                                                                                                                                                       |

> [!NOTE]
> Вы можете указать средства ведения журнала на различных уровнях — одно средство ведения журнала для одного API или одно средство ведения журнала для всех API.
>  
> Если указываются оба средства ведения журнала одновременно, используется следующий механизм:
> + если это различные средства ведения журнала, будут использованы оба средства ведения журнала (мультиплексирование журналов);
> + если это же одни и те же средства ведения журнала, но с различными параметрами, то средство ведения журнала для одного API (с более высоким уровнем детализации) переопределит средство ведения журнала для всех API.

## <a name="what-data-is-added-to-application-insights"></a>Какие данные добавляются в Application Insights

Application Insights получает:

+ Элемент данных телеметрии *Запрос* для каждого входящего запроса (*запрос клиента*, *ответ клиента*);
+ Элемент данных телеметрии *Зависимость* для каждого запроса, направляемого в службу сервера (*запрос сервера*, *ответ сервера*);
+ Элемент телеметрии *исключений* для каждого невыполненного запроса:
    + сбой из-за закрытого подключения клиента
    + активировал раздел " *On-Error* " политик API
    + возвратили код ответа HTTP 4xx или 5xx.
+ Элемент телеметрии *трассировки* , если настроена политика [трассировки](api-management-advanced-policies.md#Trace) . `severity`Параметр в `trace` политике должен быть больше или равен `verbosity` значению в Application Insights ведения журнала.

> [!NOTE]
> Сведения о максимальном размере и количестве метрик и событий для каждого экземпляра Application Insights см. в разделе [ограничения Application Insights](../azure-monitor/service-limits.md#application-insights) .

## <a name="performance-implications-and-log-sampling"></a>Влияние выборок журналов на производительность

> [!WARNING]
> Запись всех событий может привести к серьезному снижению производительности в зависимости от частоты входящих запросов.

По результатам внутренних нагрузочных тестов включение этой функции приводит к снижению пропускной способности на 40–50 %, если частота запросов превышает 1000 запросов в секунду. Application Insights предназначен для использования статистического анализа для оценки производительность приложений. Она не может использоваться в качестве системы аудита и не предназначена для записи в журналы всех запросов для интенсивно используемых API.

Вы можете управлять количеством регистрируемых в журнале запросов, изменив параметр **выборки** (см. предыдущие шаги). Значение 100% означает, что все запросы записываются в журнал, а 0% — без ведения журнала. **Выборка** позволяет сократить объем данных телеметрии, что позволяет избежать значительного снижения производительности, сохраняя при этом преимущества ведения журнала.

Пропуск заголовков и текста запросов также позволяет уменьшить отрицательное влияние на производительность.

## <a name="video"></a>Видеоролик

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2pkXv]
>
>

## <a name="next-steps"></a>Дальнейшие действия

+ Дополнительные сведения об [Azure Application Insights](/azure/application-insights/).
+ См. раздел [ведение журналов с помощью Центров событий Azure](api-management-howto-log-event-hubs.md).
