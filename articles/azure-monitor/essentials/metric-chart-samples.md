---
title: Примеры диаграмм метрик Azure Monitor
description: Сведения о визуализации данных Azure Monitor.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.openlocfilehash: b7e848e4565e2b1badd9bc418d28ea984a0306b2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102051112"
---
# <a name="metric-chart-examples"></a>Примеры диаграмм метрик 

Платформа Azure предоставляет [более тысячи метрик](./metrics-supported.md), многие из которых обладают измерениями. Используя [фильтры измерения](./metrics-charts.md), применяя тип диаграммы управления [разделение](./metrics-charts.md) и изменяя параметры диаграммы, вы можете создавать эффективные представления и панели мониторинга диагностики, которые содержат подробные сведения о работоспособности инфраструктуры и приложения. В данной статье приведены некоторые примеры диаграмм, которые можно создать с помощью [Обозревателя метрик](./metrics-charts.md), и инструкции с обязательными шагами, которые необходимо выполнить для настройки каждой диаграммы.

Хотите поделиться примерами хороших диаграмм со всем миром? Присоединитесь к этой странице на GitHub и делитесь собственными примерами диаграмм!

## <a name="website-cpu-utilization-by-server-instances"></a>Использование ЦП экземплярами сервера на веб-сайте

На этой диаграмме показано, находился ли ЦП Службы приложений в допустимом диапазоне, и она разбивает его на экземпляры, чтобы определить, правильно ли была распределена нагрузка. То что приложение было запущено на едином экземпляре сервера до 06:00, а затем было масштабировано путем добавления дополнительного экземпляра, можно увидеть на диаграмме.

![График со средним процентом использования процессора экземпляром сервера](./media/metrics-charts/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Как настроить эту диаграмму:

Выберите ресурс службы приложений и найдите метрику **Процент ЦП**. Затем щелкните **Apply splitting** (Применить разделение) и выберите измерение **Экземпляр**.

## <a name="application-availability-by-region"></a>Доступность приложения по регионам

Чтобы узнать, в каком географическом регионе существуют проблемы, просмотрите доступность приложения по регионам. На этой диаграмме показано метрику доступности Application Insights. Можно увидеть, что в отслеживаемом приложении нет проблем с доступностью из центра обработки данных в восточной части США, но оно испытывает проблему частичной доступности из западной части США и Восточной Азии.

![Диаграмма средней доступности по расположениям](./media/metrics-charts/availability-by-location.png)

### <a name="how-to-configure-this-chart"></a>Как настроить эту диаграмму:

Сначала необходимо включить мониторинг [доступности Application Insights](../app/monitor-web-app-availability.md) для веб-сайта. Затем выберите ресурс Application Insights и укажите метрику "Доступность". Примените разделение на измерении **Местоположение запуска**.

## <a name="volume-of-failed-storage-account-transactions-by-api-name"></a>Объем невыполненных транзакций учетной записи хранения по имени API

Ресурс вашей учетной записи хранения испытывает избыточный объем неудачных транзакций. Вы можете использовать метрику транзакции, чтобы указать, какой API отвечает за избыточный сбой. Обратите внимание, что следующая диаграмма настроена с тем же измерением (именем API) при разделении и фильтрации по типу ответа с ошибкой:

![Линейчатая диаграмма транзакций API](./media/metrics-charts/split-and-filter-example.png)

### <a name="how-to-configure-this-chart"></a>Как настроить эту диаграмму:

Во время выбора метрики необходимо выбрать учетную запись хранения и метрику **Транзакции**. Измените тип диаграммы на **Линейчатая диаграмма**. Щелкните **Apply splitting** (Применить разделение) и выберите **API-имя** измерения. После этого щелкните **Добавить фильтр** и еще раз выберите измерение **API-имя**. В диалоговом окне фильтра выберите API, которые необходимо отобразить на диаграмме.

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о Azure Monitor см. в статье [Создание интерактивных отчетов с книгами Azure Monitor](../visualize/workbooks-overview.md).
* Ознакомьтесь со статьей [Обозреватель метрик Azure Monitor](metrics-charts.md).