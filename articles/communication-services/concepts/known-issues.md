---
title: Службы связи Azure — известные проблемы
description: Дополнительные сведения о службах связи Azure
author: rinarish
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: troubleshooting
ms.service: azure-communication-services
ms.openlocfilehash: 7be40ac5f6cda7a81d68ca0b17f377891dd58480
ms.sourcegitcommit: 73d80a95e28618f5dfd719647ff37a8ab157a668
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/26/2021
ms.locfileid: "105606051"
---
# <a name="known-issues-azure-communication-services-sdks"></a>Известные проблемы: пакеты SDK для служб связи Azure
В этой статье содержатся сведения об ограничениях и известных проблемах, связанных с пакетами SDK для служб связи Azure.

> [!IMPORTANT]
> Существует несколько факторов, которые могут повлиять на качество вызывающего процесса. Дополнительные сведения о конфигурации сети служб связи и рекомендации по тестированию см. в документации по **[требованиям к сети](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/network-requirements)** .


## <a name="javascript-sdk"></a>Пакет SDK для JavaScript

В этом разделе содержатся сведения об известных проблемах, связанных с голосовыми и видеовызовами JavaScript в службах связи Azure.

### <a name="refreshing-a-page-doesnt-immediately-remove-the-user-from-their-call"></a>Обновление страницы не приводит к немедленному удалению пользователя из вызова

Если пользователь находится в вызове и принимает решение обновить страницу, служба мультимедиа служб связи не удаляет этого пользователя сразу же из вызова. Будет ожидать повторного присоединение пользователя. Пользователь будет удален из вызова после истечения времени ожидания службы мультимедиа.

Лучше создавать пользовательские интерфейсы, не требующие от конечных пользователей обновлять страницу приложения во время вызова. Если пользователь обновляет страницу, повторно используйте тот же идентификатор пользователя служб связи после возврата в приложение.

С точки зрения других участников вызова, пользователь останется в вызове в течение периода времени (1-2 минут). Если пользователь повторно присоединяется к тому же ИДЕНТИФИКАТОРу пользователя служб связи, он будет представлен как один и тот же существующий объект в `remoteParticipants` коллекции.

Если пользователь отправит видео перед обновлением, `videoStreams` коллекция сохранит данные предыдущего потока до истечения времени ожидания службы и удалит ее. В этом сценарии приложение может наблюдать за всеми новыми потоками, добавленными в коллекцию, и визуализировать их с наибольшим значением `id` . 


### <a name="its-not-possible-to-render-multiple-previews-from-multiple-devices-on-web"></a>Невозможно отобразить несколько предварительных версий с нескольких устройств в Интернете.
Это известное ограничение. Дополнительные сведения см. в [обзоре вызывающего пакета SDK](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/calling-sdk-features).

### <a name="enumerating-devices-isnt-possible-in-safari-when-the-application-runs-on-ios-or-ipados"></a>Перечисление устройств в Safari невозможно, если приложение выполняется в iOS или Ипадос

Приложения не могут перечислять или выбирать устройства MIC/динамика (например, Bluetooth) на Safari iOS/iPad. Это известное ограничение операционной системы.

Если вы используете Safari на macOS, приложение не сможет перечислять и выбирать динамики через службы связи диспетчер устройств. В этом сценарии устройства должны быть выбраны с помощью операционной системы. Если вы используете Chrome в macOS, приложение может перечислить или выбрать устройства с помощью служб связи диспетчер устройств.

### <a name="audio-connectivity-is-lost-when-receiving-sms-messages-or-calls-during-an-ongoing-voip-call"></a>При получении SMS-сообщений или вызовов во время текущего вызова VoIP теряется подключение к аудио.
Мобильные браузеры не поддерживают подключение в фоновом состоянии. Это может привести к снижению производительности вызова, если вызов VoIP был прерван событием, которое передает приложение в фоновом режиме.

<br/>Клиентская библиотека: вызов (JavaScript)
<br/>Браузеры: Safari, Chrome
<br/>Операционная система: iOS, Android

### <a name="repeatedly-switching-video-devices-may-cause-video-streaming-to-temporarily-stop"></a>Многократное переключение видеоустройств может привести к временной пересбою потоковой передачи видео

Переключение между видеоустройствами может привести к тому, что поток видео будет приостановлен, пока поток получен от выбранного устройства.

#### <a name="possible-causes"></a>Возможные причины
Переключение между устройствами часто может привести к снижению производительности. Разработчикам рекомендуется прерывать один поток устройства перед началом другого.

### <a name="bluetooth-headset-microphone-is-not-detected-therefore-is-not-audible-during-the-call-on-safari-on-ios"></a>Микрофон гарнитуры Bluetooth не обнаружен, поэтому во время вызова в Safari на iOS не выполняется звук
В Safari на iOS не поддерживаются гарнитуры Bluetooth. Устройство Bluetooth не будет отображаться в списке доступных параметров микрофона, и другие участники не смогут слышать вас при попытке использовать Bluetooth через Safari.

#### <a name="possible-causes"></a>Возможные причины
Это известное ограничение операционной системы macOS/iOS/Ипадос. 

С помощью Safari в **macOS** и **iOS/ипадос** невозможно перечислить или выбрать устройства динамика через службы связи Диспетчер устройств так как перечисление или выбор динамиков не поддерживается Safari. В этом сценарии выбор устройства должен быть обновлен с помощью операционной системы.

### <a name="rotation-of-a-device-can-create-poor-video-quality"></a>Поворот устройства может создать низкое качество видео
При вращении устройства пользователи могут столкнуться со снижением качества видео.

<br/>Затронутые устройства: Google пиксель 5, Google пиксель 3A, Apple iPad 8 и Apple iPad X
<br/>Клиентская библиотека: вызов (JavaScript)
<br/>Браузеры: Safari, Chrome
<br/>Операционная система: iOS, Android


### <a name="camera-switching-makes-the-screen-freeze"></a>Переключение камеры делает закрепление экрана 
Когда пользователь служб связи присоединяется к вызову с помощью пакета SDK для вызовов JavaScript, а затем нажимает кнопку переключателя камеры, Пользовательский интерфейс может перестать отвечать, пока приложение не будет обновлено или браузер не попадет на задний план пользователя.

<br/>Затронутые устройства: Google Pixel 4A
<br/>Клиентская библиотека: вызов (JavaScript)
<br/>Браузеры: Chrome
<br/>Операционная система: iOS, Android


#### <a name="possible-causes"></a>Возможные причины
Исследуется.

### <a name="if-the-video-signal-was-stopped-while-the-call-is-in-connecting-state-the-video-will-not-be-sent-after-the-call-started"></a>Если видеосигнал был остановлен, пока вызов находится в состоянии "подключение", видео не будет отправлено после начала вызова 
Если пользователи решили быстро включить или отключить видео во время вызова, `Connecting` это может привести к проблемам с потоком, полученным для вызова. Мы советуем разработчикам создавать приложения таким образом, чтобы не требовать включения и выключения видео в `Connecting` состоянии вызова. Эта проблема может привести к снижению производительности видео в следующих сценариях:

 - Если пользователь начинает с аудио, а затем запускает и останавливает видео, пока вызов находится в `Connecting` состоянии.
 - Если пользователь начинает с аудио, а затем запускает и останавливает видео, пока вызов находится в `Lobby` состоянии.


#### <a name="possible-causes"></a>Возможные причины
Исследуется.

###  <a name="sometimes-it-takes-a-long-time-to-render-remote-participant-videos"></a>Иногда Просмотр видео удаленных участников занимает много времени
Во время текущего вызова группы _пользователь A_ отправляет видео, а затем _пользователь б_ присоединяется к вызову. Иногда пользователь б не увидит видео с пользователя а, или видео A начинает подготовку к просмотру после длительной задержки. Эта проблема может быть вызвана сетевой средой, для которой требуется дополнительная настройка. Инструкции по настройке сети см. в документации по [сетевым требованиям](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/network-requirements) .
