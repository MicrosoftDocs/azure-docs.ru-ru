---
title: Общие сведения об акселераторах решений Интернета вещей в Azure | Документация Майкрософт
description: Вы узнаете об акселераторах решений Интернета вещей Azure. Акселераторы решений Интернета вещей являются полноценными, комплексными, готовыми к развертыванию решениями для Интернета вещей.
author: dominicbetts
ms.author: dobett
ms.date: 03/09/2019
ms.topic: overview
ms.custom: mvc
ms.service: iot-accelerators
services: iot-accelerators
manager: timlt
ms.openlocfilehash: c966051ed5699d408fe83f1e9c862ca78b3282c4
ms.sourcegitcommit: 3ed0f0b1b66a741399dc59df2285546c66d1df38
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2021
ms.locfileid: "107714548"
---
# <a name="what-are-azure-iot-solution-accelerators"></a>Что такое акселераторы решений Интернета вещей Azure?

В облачных решениях Интернета вещей обычно используется пользовательский код и облачные службы для управления подключением устройств, обработки данных, аналитики и представления.

Акселераторы решений Интернета вещей — это готовые к развертыванию комплексные решения Интернета вещей для реализации распространенных сценариев Интернета вещей. Сценарии включают в себя подключенную фабрику и моделирование устройства. При развертывании акселератора решений вы получите все необходимые облачные службы вместе со всем необходимым кодом приложения.

Акселераторы решений являются отправной точкой для разработки собственных решений Интернета вещей. Исходный код всех акселераторов решений является открытым и доступен в GitHub. Рекомендуется загрузить и настроить акселераторы решений в соответствии с вашими требованиями.

Акселераторы решений также можно использовать для обучения перед созданием своего собственного решения для Интернета вещей с нуля. В акселераторах решений реализованы традиционные подходы для облачных решений Интернета вещей, которым нужно следовать.

Код приложения в каждом акселераторе решений включает веб-приложение, которое позволяет управлять им.

> [!NOTE]
> Решения для удаленного мониторинга и прогнозного обслуживания были удалены с сайта [акселераторов решений Azure IoT](https://www.azureiotsolutions.com/Accelerators). Дополнительные сведения см. в статье [Что такое акселераторы решений для Интернета вещей Azure?](/previous-versions/azure/iot-accelerators/about-iot-accelerators) (предыдущая версия).

## <a name="supported-iot-scenarios"></a>Поддерживаемые сценарии Интернета вещей

В настоящее время для развертывания доступны два акселератора решений:

### <a name="connected-factory"></a>Подключенная фабрика

Используйте [акселератор решений подключенной фабрики](iot-accelerators-connected-factory-features.md) для сбора данных телеметрии с промышленных объектов с помощью интерфейса [Унифицированная архитектура OPC](https://opcfoundation.org/about/opc-technologies/opc-ua/) и управления ими. Промышленные объекты могут включать сборочные и испытательные установки на производственной линии завода.

Панель мониторинга подключенного производства можно использовать для отслеживания и управления производственными устройствами.

:::image type="content" source="./media/about-iot-accelerators/cf-dashboard-inline.png" alt-text="Снимок экрана: панель мониторинга подключенного производства" lightbox="./media/about-iot-accelerators/cf-dashboard-expanded.png":::

### <a name="device-simulation"></a>Имитация устройства

Используйте [акселератор решений имитации устройств](iot-accelerators-device-simulation-overview.md) для запуска имитированных устройств, которые генерируют реалистичные данные телеметрии. Этот акселератор решений можно использовать для тестирования поведения других акселераторов решений или для тестирования собственных решений Интернета вещей.

С помощью веб-приложения имитации устройства можно настраивать и запускать моделирования.

:::image type="content" source="./media/about-iot-accelerators/ds-dashboard-inline.png" alt-text="Снимок экрана: панель мониторинга решения для имитации устройства." lightbox="./media/about-iot-accelerators/ds-dashboard-expanded.png":::

## <a name="design-principles"></a>Принципы проектирования

Все акселераторы решений следуют одним и тем же принципам проектирования и целям использования. Они спроектированы, чтобы быть:

* **Масштабируемыми**, позволяя подключать и управлять миллионами устройств.
* **Расширяемыми**, позволяя настраивать их в соответствии с вашими требованиями.
* **Понятными**, позволяя понять, как они работают и как реализованы.
* **Модульными**, позволяя заменять службы на альтернативные.
* **Защищенными**, соединяя в себе безопасность Azure со встроенными функциями безопасности для подключений и устройств.

## <a name="architectures-and-languages"></a>Архитектуры и языки

Изначально акселераторы решений были написаны на .NET с использованием архитектуры "модель-представление-контроллер" (MVC). Корпорация Майкрософт переводит акселераторы решений на новую архитектуру на основе микрослужб. В таблице ниже показано текущее состояние для акселераторов решений со ссылками на репозитории GitHub.

| Акселератор решений   | Architecture  | Языки     |
| ---------------------- | ------------- | ------------- |
| Подключенная фабрика      | MVC           | [.NET](https://github.com/Azure/azure-iot-connected-factory)          |
| Имитация устройства      | Микрослужбы | [.NET](https://github.com/Azure/azure-iot-pcs-device-simulation)          |

Дополнительные сведения об архитектуре микрослужб см. в статье [Introduction to the Azure IoT reference architecture](/azure/architecture/reference-architectures/iot/) (Введение в эталонную архитектуру Интернета вещей Azure).

## <a name="deployment-options"></a>Варианты развертывания

Акселераторы решений можно развертывать с сайта [Microsoft Azure IoT Solution Accelerators](https://www.azureiotsolutions.com/Accelerators#) (Акселераторы решений Интернета вещей Microsoft Azure) или с помощью командной строки.

Стоимость запуска акселератора решений — это [общая стоимость выполнения базовых служб Azure](https://azure.microsoft.com/pricing). При выборе параметров развертывания отображаются сведения об используемых службах Azure.

## <a name="next-steps"></a>Дальнейшие действия

Чтобы опробовать один из акселераторов решений Интернета вещей, см. краткое руководство по [подключенному решению фабрики](quickstart-connected-factory-deploy.md).
