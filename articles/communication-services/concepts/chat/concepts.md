---
title: Понятия, связанные с чатами в Службах коммуникации Azure
titleSuffix: An Azure Communication Services concept document
description: Узнайте о понятиях, связанных с чатами в Службах коммуникации.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 292f430a1b08d59efdf05405437b3d1aa49ea2b7
ms.sourcegitcommit: d23602c57d797fb89a470288fcf94c63546b1314
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2021
ms.locfileid: "106168605"
---
# <a name="chat-concepts"></a>Понятия, связанные с чатами 

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include-chat.md)]

Пакеты SDK для чата Служб коммуникации Azure позволяют добавить в приложения текстовый чат реального времени. На этой странице перечислены основные понятия и возможности,с вязанные с чатами.    

Дополнительные сведения о языках и возможностях конкретного пакета SDK см. в [описании пакета SDK для чатов Служб коммуникации](./sdk-features.md).  

## <a name="chat-overview"></a>Общие сведения о чатах    

Беседы в чате организованы в **потоки чатов**. Потоки чатов характеризуются следующим образом:

- Поток чата однозначно идентифицируется по `ChatThreadId`. 
- Поток чата может включать одного или нескольких пользователей в качестве участников, которые могут отправлять сообщения в этот поток. 
- Пользователь может быть участником одного или нескольких потоков чатов. 
- Только участники потока имеют доступ к определенному потоку чата и только они могут выполнять с ним определенные операции. К таким операциям относятся отправка и получение сообщений, а также добавление и удаление участников. 
- Пользователь автоматически добавляется в качестве участника в каждый созданный им поток чата.

### <a name="user-access"></a>Доступ пользователей
Обычно создатель и участники потоков имеют одинаковый уровень доступа к потоку и могут выполнять с ним все операции, доступные в пакете SDK, включая его удаление. У участников нет доступа на запись к сообщениям, отправленным другими участниками, т. е. обновлять или удалять отправленные сообщения может только их отправитель. Если другой участник попытается сделать это, он получит сообщение об ошибке. 

Если вы хотите ограничить доступ к функциям чата для некоторых пользователей, можно настроить доступ в доверенной службе. Доверенной называется служба, которая управляет проверкой подлинности и авторизацией участников чата. Далее мы рассмотрим это подробнее.  

### <a name="chat-data"></a>Данные чата 
Службы коммуникации хранят журнал чата неограниченно долго, пока он не будет удален явным образом. Участники потока чата могут просматривать журнал сообщений определенного потока с помощью `ListMessages`. Пользователи, удаленные из потока чата, смогут получать сообщения из журнала этого потока вплоть до момента удаления, но не смогут отправлять или получать новые сообщения. Если неактивный поток чата не включает участников, он автоматически удаляется через 30 дней. Дополнительные сведения о хранении данных в Службах коммуникации см. в документации по [обеспечению конфиденциальности](../privacy.md).  

### <a name="service-limits"></a>Ограничения службы  
- Максимальное число участников, разрешенное в потоке разговора, — 250.   
- Максимально допустимый размер сообщения составляет примерно 28 КБ.  
- В потоках более чем с 20 участниками уведомления о прочтении и индикатор ввода обычно не поддерживаются.    

## <a name="chat-architecture"></a>Архитектура чата    

Архитектура чата включает две основные части: 1) доверенная служба; 2) клиентское приложение.    

:::image type="content" source="../../media/chat-architecture.png" alt-text="Схема архитектуры чата в Службах коммуникации."::: 

 - **Доверенная служба.** Для правильного управления сеансом чата требуется служба, которая помогает подключаться к Службам коммуникации с использованием строки подключения к ресурсу. Эта служба отвечает за создание потоков чатов, а также добавление и удаление участников и выдачу пользователям маркеров доступа. Дополнительные сведения о маркерах доступа можно найти в [этом](../../quickstarts/access-tokens.md) кратком руководстве.  
 - **Клиентское приложение** подключается к доверенной службе и получает маркеры доступа, которые затем используются для подключения к Службам коммуникации. Когда доверенная служба создаст поток чата и добавит пользователей в качестве участников, эти участники смогут подключаться к потоку чата и отправлять сообщения через клиентское приложение. Чтобы подписаться на сообщения и обновления потоков, отправляемые другими участниками, можно использовать в клиентском приложении функцию уведомлений в реальном времени, которую мы обсудим ниже.
    
        
## <a name="message-types"></a>Типы сообщений    

В журнал сообщений служба чата помещает сообщения, созданные пользователями и системой. Системные сообщения создаются при обновлении потока разговора, что позволяет отслеживать добавление и удаление участников или обновление темы для потока чата. Вызов `List Messages` или `Get Messages` для потока чата возвращает сообщения обоих типов в хронологическом порядке.

Для сообщений, созданных пользователями, можно задать тип сообщения в `SendMessageOptions` при отправке сообщения в поток чата. Если это значение не указано, Службы коммуникации по умолчанию используют тип `text`. Это значение важно правильно указать, если отправляется код HTML. Если указать `html`, Службы коммуникации очистят содержимое так, чтобы оно безопасно отображалось на клиентских устройствах.
 - `text` — сообщение в формате обычного текста, составленное и отправленное пользователем в поток чата. 
 - `html` — форматированное сообщение с синтаксисом HTML, составленное и отправленное пользователем в поток чата. 

Типы системных сообщений 
 - `participantAdded`. Системное сообщение о том, что один или несколько участников были добавлены в поток чата.
 - `participantRemoved`. Системное сообщение о том, что участник был удален из потока чата.
 - `topicUpdated`. Системное сообщение, указывающее на изменение темы беседы.

## <a name="real-time-notifications"></a>Уведомления в реальном времени  

Некоторые пакеты SDK (например, пакет SDK для чата для JavaScript) поддерживают уведомления в реальном времени. Эта возможность позволяет клиентам в реальном времени получать от Служб коммуникации обновления потока чата и входящие сообщения, не опрашивая API. Клиентское приложение может подписываться на следующие события:
 - `chatMessageReceived` — отправка сообщения участником в поток чата.
 - `chatMessageEdited` — изменение сообщения в потоке чата. 
 - `chatMessageDeleted` — удаление сообщения в потоке чата.   
 - `typingIndicatorReceived` — отправка состояния набора текста другим участником в поток чата.    
 - `readReceiptReceived` — отправка подтверждения о прочтении сообщения другим участником.  
 - `chatThreadCreated` — создание потока чата пользователем Служб коммуникации.    
 - `chatThreadDeleted` — удаление потока чата пользователем Служб коммуникации.    
 - `chatThreadPropertiesUpdated` — обновление свойств потока чата (сейчас для потока поддерживается только обновление темы). 
 - `participantsAdded` — добавление пользователя а качестве участника потока чата.     
 - `participantsRemoved` — удаление существующего участника из потока чата.

Уведомления в реальном времени обеспечивают для пользователей возможность общения в реальном времени. Чтобы отправлять push-уведомления о сообщениях, которые пользователи пропустили в свое отсутствие, Службы коммуникации в интеграции с Сеткой событий Azure публикуют события чата (создаваемые после выполнения операций), которые вы можете подключить к собственной службе уведомлений приложений. Дополнительные сведения см. в статье [Обработка событий в Службах коммуникации Azure](https://docs.microsoft.com/azure/event-grid/event-schema-communication-services?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fcommunication-services%2Ftoc.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json).


## <a name="build-intelligent-ai-powered-chat-experiences"></a>Создание интеллектуальных интерфейсов для общения на основе ИИ   

С помощью [API Azure Cognitive Services](../../../cognitive-services/index.yml) и пакета SDK для чатов вы можете реализовать следующие варианты использования:

- разрешить пользователям общаться на разных языках;  
- помочь службе поддержки приоритизировать обращения, определяя негативную тональность во входящем сообщении клиента; 
- анализировать входящие сообщения для обнаружения ключей и распознавания сущностей и передавать пользователю приложения важную информацию с учетом содержимого сообщения.

Один из способов реализации этих возможностей — создать доверенную службу, которая выступает в качестве участника потока чата. Предположим, вы хотите включить перевод на несколько языков. Эта служба будет прослушивать сообщения, которыми обмениваются другие участники [1], вызывать API Cognitive Services для перевода содержимого на нужный язык [2, 3] и отправлять переведенный текст в виде сообщения в поток чата [4].

Это означает, что в журнале сообщений будут содержаться как исходные, так и переведенные сообщения. В клиентском приложении можно добавить логику для отображения исходных или переведенных сообщений. В [этом кратком руководстве](../../../cognitive-services/translator/quickstart-translator.md) показано, как использовать API Cognitive Services для перевода текста на разные языки. 
    
:::image type="content" source="../media/chat/cognitive-services.png" alt-text="Схема взаимодействия Cognitive Services со Службами коммуникации."::: 

## <a name="next-steps"></a>Дальнейшие действия   

> [!div class="nextstepaction"] 
> [Начало работы с чатом](../../quickstarts/chat/get-started.md)    

Вас могут заинтересовать следующие документы:  
- Ознакомьтесь с [пакетом SDK для чатов](sdk-features.md)
