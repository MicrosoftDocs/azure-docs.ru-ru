---
title: Краткое руководство. Общие сведения о примере приложения — Azure Spring Cloud
description: Описание примера приложения, используемого в этом цикле кратких руководств по развертыванию в Azure Spring Cloud.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 09/08/2020
ms.custom: devx-track-java
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: dd36bb18e84ea299195b77286887a3b279f81469
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "104878860"
---
# <a name="introduction-to-the-sample-app"></a>Общие сведения о примере приложения

::: zone pivot="programming-language-csharp"
В этом цикле кратких руководств для демонстрации развертывания приложения Steeltoe для .NET Core в службе Azure Spring Cloud используется пример приложения, состоящего из двух микрослужб. Вы будете использовать такие возможности Azure Spring Cloud, как обнаружение служб, сервер конфигурации, журналы, метрики и распределенная трассировка.

## <a name="functional-services"></a>Функциональные службы

Пример приложения состоит из двух микрослужб:

* Служба `planet-weather-provider` возвращает текстовое описание погоды в ответ на HTTP-запрос, в котором указывается название планеты. Например, она может вернуть "Очень тепло" для Меркурия. Данные о погоде она получает с сервера конфигурации. Сервер конфигурации получает их из файла YAML в репозитории Git, например:

  ```yaml
  MercuryWeather: very warm
  VenusWeather: quite unpleasant
  MarsWeather: very cool
  SaturnWeather: a little bit sandy
  ```

* Служба `solar-system-weather` возвращает данные для четырех планет в ответ на HTTP-запрос. Она получает их, отправляя четыре HTTP-запроса в службу `planet-weather-provider`. Для вызова `planet-weather-provider` она использует службу обнаружения сервера Eureka. Данные она возвращает в формате JSON, например:

  ```json
  [{
      "Key": "Mercury",
      "Value": "very warm"
  }, {
      "Key": "Venus",
      "Value": "quite unpleasant"
  }, {
      "Key": "Mars",
      "Value": "very cool"
  }, {
      "Key": "Saturn",
      "Value": "a little bit sandy"
  }]
  ```

Архитектура примера приложения показана на следующей диаграмме.

:::image type="content" source="media/spring-cloud-quickstart-sample-app-introduction/sample-app-diagram.png" alt-text="Схема примера приложения":::

## <a name="code-repository"></a>Репозиторий кода

Пример приложения находится в папке [steeltoe-sample](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/steeltoe-sample) в репозитории Azure-Samples/Azure-Spring-Cloud-Samples на сайте GitHub.

В инструкциях, приведенных в следующих кратких руководствах, по мере необходимости упоминается исходный код.

::: zone-end

::: zone pivot="programming-language-java"
В этом кратком руководстве демонстрируется развертывание приложения в службе Azure Spring Cloud на примере приложения для учета личных финансов под названием PiggyMetrics. Приложение PiggyMetrics позволяет продемонстрировать закономерности архитектуры микрослужбы и разбивку служб. Вы увидите, как она развертывается в Azure с помощью эффективных возможностей Azure Spring Cloud, среди которых обнаружение служб, сервер конфигурации, журналы, метрики и распределенная трассировка.

Для изучения примеров развертывания Azure Spring Cloud требуется только знать расположение исходного кода, который предоставляется в случае необходимости.

## <a name="functional-services"></a>Функциональные службы

PiggyMetrics делится на три основные микрослужбы. Все они являются независимо развертываемыми приложениями, упорядоченными по бизнес-доменам.

* **Служба счетов (будет развернута)** . Содержит общую логику и проверку ввода пользователем: статьи доходов и затрат, сбережения и параметры счетов.
* **Служба статистики (не используется в этом кратком руководстве)** . Выполняет вычисления по основным статистическим параметрам и фиксирует временные ряды для каждой учетной записи. Точка данных содержит значения, нормированные на основе базовой валюты и периода времени. Эти данные используются для отслеживания динамики движения денежных средств на протяжении времени существования счета.
* **Служба уведомлений (не используется в этом кратком руководстве)** . Хранит контактные данные пользователей и параметры уведомлений, такие как напоминание и частота резервного копирования. Выполняемый по расписанию рабочий процесс собирает необходимые сведения из других служб и отправляет сообщения электронной почты подписавшимся клиентам.

## <a name="infrastructure-services"></a>Службы инфраструктуры

В распределенных системах применяется несколько распространенных схем обеспечения работы основных служб. Azure Spring Cloud предоставляет эффективные средства, улучшающие работу Spring Boot с целью реализации этих схем. 

* **Служба настройки (размещенная в Azure Spring Cloud)** . Конфигурация Azure Spring Cloud — это горизонтально масштабируемая централизованная служба настройки для распределенных систем. Она использует подключаемый репозиторий, который сейчас поддерживает локальное хранилище, Git и Subversion.
* **Обнаружение служб (размещенное в Azure Spring Cloud)** . Позволяет автоматически обнаруживать сетевые расположения для экземпляров служб, адреса которых могут назначаться динамически вследствие автоматического масштабирования, сбоев и обновлений.
* **Служба проверки подлинности (будет развернута)** . Ответственность за авторизацию полностью возложена на отдельный сервер, который предоставляет маркеры OAuth2 для внутренних служб ресурсов. Сервер проверки подлинности выполняет авторизацию пользователей и защищает обмен данными между компьютерами в пределах периметра.
* **Шлюз API (будет развернут)** . Три основные службы предоставляют клиенту внешний API. В реальных системах количество функций может очень быстро увеличиваться по мере роста сложности самих систем. В отрисовке одной сложной веб-страницы могут участвовать сотни служб. Шлюз API — это единая точка входа, используемая для обработки запросов и направления их соответствующей внутренней службе или для вызова нескольких внутренних служб, выполняющих статистическую обработку результатов. 

## <a name="sample-usage-of-piggymetrics"></a>Пример использования PiggyMetrics

Полные сведения о реализации см. на странице [PiggyMetrics](https://github.com/Azure-Samples/piggymetrics). При необходимости в образцах используются ссылки на исходный код.
::: zone-end

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Подготовка экземпляра Azure Spring Cloud](spring-cloud-quickstart-provision-service-instance.md)
