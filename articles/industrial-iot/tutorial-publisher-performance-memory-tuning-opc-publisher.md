---
title: Настройка производительности и памяти в издателе OPC Microsoft
description: Из этого учебника вы узнаете, как настроить производительность и память в издателе OPC.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: tutorial
ms.date: 3/22/2021
ms.openlocfilehash: 89e288d1186efd405019d6474dcbd332e7925d67
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104787380"
---
# <a name="tutorial-tune-the-opc-publisher-performance-and-memory"></a>Учебник. Настройка производительности и памяти в издателе OPC

В этом руководстве описано следующее.

> [!div class="checklist"]
> * Настройка производительности
> * Настройка потока сообщений для ресурсов памяти

При запуске издателя OPC в производственной среде необходимо учитывать требования для производительности сети (пропускная способность и задержка) и ресурсы памяти. Издатель OPC содержит указанные ниже параметры командной строки, которые помогут выполнить эти требования.

* Емкость очереди сообщений (`mq` для версии 2.5 и более ранних версий (недоступно в версии 2.6), а`om` — для версии 2.7)
* Интервал отправки для Центра Интернета вещей (`si`)
* Размер сообщений для Центра Интернета вещей (`ms`)

## <a name="adjusting-iot-hub-send-interval-and-iot-hub-message-size"></a>Настройка интервала отправки и размера сообщений для Центра Интернета вещей

Параметр `mq/om` управляет верхним пределом емкости внутренней очереди сообщений. Эта очередь помещает в буфер все сообщения перед их отправкой в Центр Интернета вещей. Допустимый размер очереди по умолчанию составляет 2 МБ для издателя OPC версии 2.5 и более ранних версий, 4000 сообщений Центра Интернета вещей — для версии 2.7 (то есть если параметр размера сообщений Центра Интернета вещей имеет значение 256 КБ, размер очереди будет доставлять до 1 ГБ). Если издатель OPC не может отправить сообщения в Центр Интернета вещей достаточно быстро, число элементов в этой очереди увеличится. Если это происходит во время тестовых запусков, можно выполнить одно или оба указанных действия для устранения проблемы:

* уменьшить интервал отправки данных для Центра Интернета вещей (`si`);

* увеличить размер сообщений для Центра Интернета вещей ( `ms`; максимально допустимое значение — 256 КБ).

Если очередь растет, несмотря на то что параметры `si` и `ms` настроены, достигается максимальная емкость очереди и сообщения теряются. Это связано с тем, что параметры `si` и `ms` имеют физический предел, а подключение к Интернету между издателем OPC и Центром Интернета вещей недостаточно быстрое для количества сообщений, которые должны быть отправлены в рамках данного сценария. В этом случае может помочь только соответствующая настройка нескольких параллельных издателей OPC. Параметр `mq/om` также имеет большое влияние на потребление памяти издателем OPC. 

Параметр `si` вынуждает издателя OPC отправлять сообщения в Центр Интернета вещей с указанным интервалом. Сообщение отправляется либо в том случае, если доступен максимальный размер сообщения (256 КБ данных) для Центра Интернета вещей (запускается сброс интервала отправки данных), либо по истечении заданного интервала времени.

С помощью параметра `ms` включается пакетная обработка сообщений, которые отправляются в Центр Интернета вещей. В большинстве сетевых настроек задержка отправки одного сообщения в Центр Интернета вещей имеет высокое значение по сравнению со временем, которое требуется для передачи полезных данных. Это в основном связано с требованиями для качества обслуживания (QoS), так как сообщения подтверждаются только после их обработки Центром Интернета вещей. Поэтому, если задержка для данных, поступающих в Центр Интернета вещей, приемлема, издатель OPC следует настроить для использования максимального размера сообщений (256 КБ), задав для параметра `ms` значение 0. Это также самый экономный способ использования издателя OPC.

Конфигурация по умолчанию отправляет данные в Центр Интернета вещей каждые 10 секунд (`si=10`) или при сборе 256 КБ данных, доступных для отправки в Центр Интернета вещей (`ms=0`). При этом добавляется максимальная задержка длительностью 10 секунд, но вероятность потери данных из-за большого размера сообщения — низкая. Метрика `monitored item notifications enqueue failure` в издателе OPC версии 2.5 и более ранних версий, а также `messages lost` в издателе OPC версии 2.7 показывает количество утерянных сообщений.

Если для параметров `si` и `ms` задано значение 0, издатель OPC отправляет сообщение в Центр Интернета вещей, как только данные стают доступными. В результате средний размер сообщений для Центра Интернета вещей составляет лишь больше 200 байт. Однако преимущество этой конфигурации заключается в том, что издатель OPC отправляет данные из подключенного ресурса без задержки. Количество потерянных сообщений будет высоким в случаях, когда необходимо опубликовать большой объем данных. Поэтому описанные действия не рекомендуемы для таких сценариев.

Для измерения производительности издателя OPC можно использовать параметр `di` для передачи метрик производительности в журнал трассировки с указанным интервалом (в секундах).

## <a name="next-steps"></a>Дальнейшие действия
Теперь, узнав, как настроить производительность и память в издателе OPC, вы можете просмотреть дополнительные ресурсы для издателя OPC в репозитории GitHub:

> [!div class="nextstepaction"]
> [Ресурсы издателя OPC в репозитории GitHub](https://github.com/Azure/Industrial-IoT)