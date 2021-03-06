---
author: ericmitt
ms.author: ericmitt
ms.service: iot-pnp
ms.topic: include
ms.date: 07/13/2020
ms.openlocfilehash: 3ab9161c006a88a254b247a921512698ed46fa95
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "87351819"
---
1. Откройте обозреватель Azure IoT.

1. Если вы еще не установили подключение к центру Интернета вещей, на странице **Центров Интернета вещей** выберите **+ Add connection** (+ Добавить подключение). Введите строку подключения для центра Интернета вещей, созданного ранее, и нажмите кнопку **Save** (Сохранить).

1. На странице **IoT Plug and Play Settings** (Параметры IoT Plug and Play) щелкните **+ Add > Local folder** (+ Добавить > Локальная папка) и выберите локальную папку *Models*, в которой сохранены файлы модели.

1. На странице **Центров Интернета вещей** щелкните имя центра, с которым вы хотите работать. Откроется список устройств, зарегистрированных в Центре Интернета вещей

1. Щелкните **идентификатор** ранее созданного устройства.

1. В меню слева отображаются различные типы сведений, доступных для устройства.

1. Выберите **IoT Plug and Play components** (Компоненты Plug and Play IoT), чтобы просмотреть сведения о модели для устройства.

1. Вы можете просматривать различные компоненты устройства. (Компонент по умолчанию и все дополнительные компоненты.) Выберите компонент для работы.

1. Выберите страницу **Telemetry** (Телеметрия) и щелкните **Start** (Запустить), чтобы просмотреть данные телеметрии, которые отправляет устройство для компонента.

1. Выберите страницу **Properties (read-only)** (Свойства (только для чтения)), чтобы просмотреть свойства только для чтения, сообщаемые для этого компонента.

1. Выберите страницу **Properties(writable)** (Свойства (доступные для записи)), чтобы просмотреть доступные для записи свойства, которые можно обновить для этого компонента.

1. Выберите свойство по **имени**, введите новое значение и щелкните **Update desired value** (Обновить нужное значение).

1. Чтобы увидеть новое значение, нажмите кнопку **Refresh** (Обновить).

1. Выберите страницу **команд**, чтобы просмотреть все команды для этого компонента.

1. Выберите команду, которую нужно проверить, и при необходимости задайте параметр. Выберите **Send command** (Отправить команду), чтобы вызвать команду на устройстве. Ответ устройства на команду можно увидеть в окне командной строки, где выполняется пример кода.
