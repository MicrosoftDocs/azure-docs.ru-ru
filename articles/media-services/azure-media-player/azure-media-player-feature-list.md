---
title: Список компонентов Проигрыватель мультимедиа Azure
description: Справочник по функциям для Проигрыватель мультимедиа Azure.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: 88048c3328114f17b30859efb41bb9f059b71439
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "91296371"
---
# <a name="feature-list"></a>Список функций #
Ниже приведен список проверенных функций и неподдерживаемых функций.

| Компонент | НАЙДЕТЕ | ЧАСТИЧНО ПРОТЕСТИРОВАНО | НЕ ТЕСТИРОВАЛСЯ | НЕПОДДЕРЖИВАЕМЫЙ | ПРИМЕЧАНИЯ |
| ------- | ------ | ---------------- | -------- | ----------- | ----- |
| **Воспроизведение**                                |        |                  |          |             |                                                                                                                      |
| Простое воспроизведение по запросу                | X      |                  |          |             | Поддерживает потоки только из служб мультимедиа Azure                                                                      |
| Базовое воспроизведение в реальном времени                     | X      |                  |          |             | Поддерживает потоки только из служб мультимедиа Azure                                                                      |
| AES                                     | X      |                  |          |             | Поддержка службы доставки ключей служб мультимедиа Azure                                                                   |
| Несколько DRM                               |        | X                |          |             |                                                                                                                      |
| PlayReady                               | X      |                  |          |             | Поддержка службы доставки ключей служб мультимедиа Azure                                                                   |
| Widevine                                |        | X                |          |             | Поддерживает поля Widevine ПСШ, описанные в файле manifest                                                                    |
| FairPlay                                |        | X                |          |             | Поддержка службы доставки ключей служб мультимедиа Azure                                                                   |
| **Телекоммуникаций**                                   |        |                  |          |             |                                                                                                                      |
| MSE/EME (AzureHtml5JS)                  | X      |                  |          |             |                                                                                                                      |
| Откат флэш-памяти (Flash)                | X      |                  |          |             | На этой конференции доступны не все функции.                                                                         |
| Резервная Silverlight Silverlight      | X      |                  |          |             | На этой конференции доступны не все функции.                                                                         |
| Машинный HLS сквозной (Html5)         |        | X                |          |             | Не все функции доступны на этой конференции из-за ограничений платформы.                                            |
| **Функции**                                |        |                  |          |             |                                                                                                                      |
| Поддержка API                             | X      |                  |          |             | См. список известных проблем                                                                                                |
| Базовый пользовательский интерфейс                                | X      |                  |          |                                                                                                                                    |
| Инициализация с помощью JavaScript       | X      |                  |          |             |                                                                                                                      |
| Инициализация с помощью тега видео        |        | X                |          |             |                                                                                                                      |
| Адресация сегментов — на основе времени         | X      |                  |          |             |                                                                                                                      |
| Адресация сегментов — на основе индекса        |        |                  |          | X           |                                                                                                                      |
| Адресация сегмента — на основе байт         |        |                  |          | X           |                                                                                                                      |
| Служба переопределения URL-адресов служб мультимедиа Azure       |        | X                |          |             |                                                                                                                      |
| Специальные возможности — субтитры и субтитры  | X      |                 |          |             |  WebVTT (по запросу), CEA 708 (по запросу и Live) и IMSC1 (по запросу и в режиме реального времени).                                                       |
| Специальные возможности — сочетания клавиш                 | X      |                  |          |             |                                                                                                                      |
| Специальные возможности — высокая контрастность           |        | X                |          |             |                                                                                                                      |
| Специальные возможности — фокус на вкладке               |        | X                |          |             |                                                                                                                      |
| Сообщения об ошибках                         |        | X                |          |             | Сообщения об ошибках не согласованы между техническими специалистами                                                                         |
| Запуск события                        | X      |                  |          |             |                                                                                                                      |
| Диагностика                             |        | X                |          |             | Диагностические сведения доступны только на AzureHtml5JS Tech и частично доступны на Конференции Silverlight Tech. |
| Настраиваемый технический заказ                 |        | X                |          |             |                                                                                                                      |
| Эвристические методы — базовый                      | X      |                  |          |             |                                                                                                                      |
| Эвристика — Настройка              |        |                  | X        |             | Настройка доступна только на AzureHtml5JS Tech.                                                          |
| Нарушения                         | X      |                  |          |             |                                                                                                                      |
| Выбрать скорость                          | X      |                  |          |             | Этот API доступен только на конференциях AzureHtml5JS и Flash.                                                    |
| Поток с несколькими звуками                      |        | X                |          |             | Программный звуковой коммутатор поддерживается на конференциях AzureHtml5JS и Flash и доступен через выбор пользовательского интерфейса в AzureHtml5JS, Flash и Native Html5 (в Safari).  Большинству платформ требуются одни и те же частные данные кодека для переключения звуковых потоков (кодек, канал, частота выборки и т. д.). |
| Локализация пользовательского интерфейса                         |        | X                |          |             |                                                                                                                      |
| Воспроизведение нескольких экземпляров                 |        |                  |          | X           | Этот сценарий может работать для некоторых технических специалистов, но в настоящее время не поддерживается и не тестируется. Это также можно сделать для работы с помощью Iframes |
| Поддержка ADS                             |        | X                |          |             | AMP поддерживает вставку предварительных и средних рекламных объявлений из ОГРОМНых соответствующих серверов AD для VOD в AzureHtml5JS Tech |
| Analytics                               |        | X                |          |             | AMP предоставляет возможность прослушивания аналитических и диагностических событий для отправки на серверную часть аналитики по своему усмотрению.  Все события и свойства недоступны для всех специалистов из-за ограничений платформы.                                                                            |
| Пользовательские обложки                            |        |                  | X        |             | Этот сценарий можно сделать, установив для элементов управления значение false в AMP и используя собственный HTML и CSS.           |
| Очистка строки поиска                      |        |                  |          | X           |                                                                                                                      |
| Trick-Play                              |        |                  |          | X           |                                                                                                                      |
| Только звук                              | X      |                  |          |           | Поддерживается в AzureHtml5JS. Прогрессивное воспроизведение MP3 может работать с технической поддержкой HTML5, если платформа поддерживает ее.                                                                                                        |
| Только видео                              | X      |                  |          |           | Поддерживается в AzureHtml5JS.                                                                                                        |
| Презентация с несколькими периодами               |        |                  |          | X                                                                                                                                  |
| Несколько углов камеры                  |        |                  |          | X           |                                                                                                                      |
| Скорость воспроизведения                          |        | X                |          |             | Скорость воспроизведения поддерживается в большинстве сценариев, кроме мобильных, из-за частичной ошибки в Chrome                 |

## <a name="next-steps"></a>Дальнейшие действия ##
- [Краткое руководство. Проигрыватель мультимедиа Azure](azure-media-player-quickstart.md)