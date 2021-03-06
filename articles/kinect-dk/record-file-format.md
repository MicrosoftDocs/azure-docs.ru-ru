---
title: Использование пакета SDK для датчика Kinect Azure для записи формата файла
description: Узнайте, как использовать формат записанного файла SDK для датчика Kinect для Azure.
author: xthexder
ms.author: jawirth
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: reference
keywords: Kinect, Azure, датчик, пакет SDK, глубина, RGB, запись, воспроизведение, матроска, MKV
ms.openlocfilehash: f4fa14b0841cb76b2ba191310ecbca312d29f805
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "97654597"
---
# <a name="use-azure-kinect-sensor-sdk-to-record-file-format"></a>Использование пакета SDK для датчика Kinect Azure для записи формата файла

Для записи данных датчика используется формат контейнера матроска (. MKV), который позволяет хранить несколько дорожек с помощью широкого спектра кодеков. Файл записи содержит записи для хранения цветов, глубины, IR-изображений и иму.

Низкоуровневые сведения о формате контейнера MKV можно найти на [веб-сайте матроска](https://www.matroska.org/index.html).

| Имя записи | Формат кодека                          |
|------------|---------------------------------------|
| Цвет      | Mode-Dependent (МЖПЕГ, NV12 или YUY2) |
| DEPTH      | b16g (16-разрядные оттенки серого, с обратным порядком байтов)   |
| IR         | b16g (16-разрядные оттенки серого, с обратным порядком байтов)   |
| иму        | Пользовательской структуре см. [Пример структуры иму](record-file-format.md#imu-sample-structure) ниже. |

## <a name="using-third-party-tools"></a>Использование средств сторонних разработчиков

Средства, такие как `ffmpeg` или `mkvinfo` команда из набора средств [мквтулникс](https://mkvtoolnix.download/) , можно использовать для просмотра и извлечения информации из файлов записи.

Например, следующая команда извлечет глубину глубины в виде последовательности из 16-разрядной Пнгс в ту же папку:

```
ffmpeg -i output.mkv -map 0:1 -vsync 0 depth%04d.png
```

`-map 0:1`Параметр извлечет индекс 1, который для большинства записей будет иметь глубину. Если запись не содержит цветовую трек, `-map 0:0` будет использоваться.

`-vsync 0`Параметр заставляет FFmpeg извлекать кадры "как есть" вместо того, чтобы пытаться соответствовать частоте 30 кадров/с, 15 кадров/с и 5 кадров/с.

## <a name="imu-sample-structure"></a>Пример структуры иму

Если данные иму извлекаются из файла без использования API воспроизведения, данные будут находиться в двоичном формате.
Структура данных иму ниже. Все поля имеют прямой порядок байтов.

| Поле                        | Тип     |
|------------------------------|----------|
| Отметка времени акселерометр (μс) | uint64   |
| Акселерометр данные (x, y, z) | float [3] |
| Отметка времени гироскопом (μс)     | uint64   |
| Гироскопом данные (x, y, z)     | float [3] |

## <a name="identifying-tracks"></a>Определение дорожек

Может потребоваться определить, какая запись содержит цвет, глубину, IR и т. д. Определение дорожек необходимо при работе со сторонними инструментами для чтения файла матроска.
Номера дорожек зависят от режима камеры и набора включенных дорожек. Теги используются для обозначения значения каждой записи.

Приведенный ниже список тегов прикрепляется к определенному элементу матроска и может использоваться для поиска соответствующей записи или вложения.

Эти теги доступны для просмотра с помощью таких средств, как `ffmpeg` и `mkvinfo` .
Полный список тегов приведен на странице [запись и воспроизведение](record-playback-api.md) .

| Имя тега             | Целевой объект тега             | Значение тега             |
|----------------------|------------------------|-----------------------|
| K4A_COLOR_TRACK      | Цветовая дорожка            | UID матроска Track    |
| K4A_DEPTH_TRACK      | Глубина глубины            | UID матроска Track    |
| K4A_IR_TRACK         | IR-трек               | UID матроска Track    |
| K4A_IMU_TRACK        | ИМУ              | UID матроска Track    |
| K4A_CALIBRATION_FILE | Прикрепление к калибровке | Имя файла вложения   |

## <a name="next-steps"></a>Дальнейшие действия

[Запись и воспроизведение](record-playback-api.md)
