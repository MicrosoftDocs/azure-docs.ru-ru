---
title: Рекомендации по обеспечению безопасности для Центра Интернета вещей
description: Узнайте о концепции рекомендаций по безопасности и их использовании в центре защитника для центра Интернета вещей.
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: a9e33248354aab659694e39df605cc070fdaaf73
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104779347"
---
# <a name="security-recommendations-for-iot-hub"></a>Рекомендации по обеспечению безопасности для Центра Интернета вещей

Защитник для Интернета вещей сканирует ресурсы Azure и устройства IoT и предоставляет рекомендации по безопасности для сокращения зоны атак.
Рекомендации по безопасности являются практичными и предназначены для помощи клиентам в обеспечении рекомендаций по обеспечению безопасности.

В этой статье вы найдете список рекомендаций, которые можно активировать в центре Интернета вещей.

## <a name="built-in-recommendations-in-iot-hub"></a>Встроенные рекомендации в центре Интернета вещей

Оповещения о рекомендациях предоставляют подробные сведения и рекомендации по улучшению безопасности среды.

| Severity | Имя | Источник данных | Описание |
|--|--|--|--|
| Высокий | Идентичные учетные данные для проверки подлинности, используемые несколькими устройствами | Центр Интернета вещей | Учетные данные для проверки подлинности центра Интернета вещей используются несколькими устройствами. Этот процесс может указывать на устройство столкновении, выполняющее олицетворение допустимого устройства. Использование дублирующихся учетных данных увеличивает риск олицетворения устройства вредоносным субъектом. |
| Средний | Политика фильтрации IP-адресов по умолчанию должна быть запрещена | Центр Интернета вещей | Конфигурация IP-фильтра должна иметь правила, определенные для разрешенного трафика, и по умолчанию запретить весь остальной трафик по умолчанию. |
| Средний | Правило фильтрации IP-адресов включает диапазон больших IP-адресов | Центр Интернета вещей | Диапазон IP-адресов источника правила "разрешить IP-фильтры" слишком велик. Более строгие правила могут предоставлять центр Интернета вещей вредоносным субъектам. |
| Низкий | Включение журналов диагностики в центре Интернета вещей | Центр Интернета вещей | Включите журналы и сохраняйте до года. Хранение журналов позволяет воссоздать журнал действий для изучения при возникновении инцидента безопасности или компрометации сети. |

## <a name="next-steps"></a>Следующие шаги

- [Обзор](overview.md) службы "защитник для Интернета вещей"
- Узнайте, как [получить доступ к данным безопасности](how-to-security-data-access.md)
- Дополнительные сведения об [исследовании устройства](how-to-investigate-device.md)
