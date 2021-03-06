---
title: Мониторинг инфраструктуры Azure
description: Узнайте о аспектах мониторинга инфраструктуры рабочей сети Azure, таких как сканирование уязвимостей.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 7b75c9dc874a41d4221c55a8b00dd12d943e80fc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "87542948"
---
# <a name="azure-infrastructure-monitoring"></a>Мониторинг инфраструктуры Azure   

## <a name="configuration-and-change-management"></a>Управление конфигурацией и изменениями
Azure ежегодно проверяет и обновляет параметры конфигурации, а также базовые конфигурации аппаратных средств, программного обеспечения и сетевых устройств. Изменения разрабатываются, тестируются и утверждаются до ввода в рабочую среду из среды разработки или тестирования.

Базовые конфигурации, требуемые для работы служб на основе Azure, проверяются командой по обеспечению безопасности и соответствия требованиям Azure, а также командами обслуживания. Составление обзоров командами обслуживания является частью процесса тестирования, выполняемого до развертывания рабочей службы.

## <a name="vulnerability-management"></a>управление уязвимостями;
Управление обновлениями безопасности помогает защитить системы от известных уязвимостей. Azure использует интегрированные развернутые системы для управления распространением и установкой обновлений безопасности программного обеспечения Майкрософт. Azure также позволяет использовать ресурсы Microsoft Security Response Center (MSRC). Специалисты MSRC круглосуточно и без выходных работают над обнаружением, мониторингом и решением проблем с безопасностью, а также над устранением облачных уязвимостей.

## <a name="vulnerability-scanning"></a>Сканирование уязвимостей
Сканирование уязвимостей выполняется операционными системами сервера, базами данных и сетевыми устройствами. Сканирования уязвимостей выполняются как минимум ежеквартально. Azure заключает контракты с независимыми оценщиками, чтобы проводить тест на проникновение для границ Azure. Кроме того, регулярно проводится проверка Red-Team, а результаты используются для улучшения безопасности.

## <a name="protective-monitoring"></a>Защитный мониторинг
Система безопасности Azure определила требования к активному мониторингу. Служебные команды настраивают активные средства мониторинга в соответствии с этими требованиями. Активные инструменты мониторинга включают в себя Microsoft Monitoring Agent (MMA) и System Center Operations Manager. Эти средства отправляют оповещения сотрудникам службы безопасности Azure в ситуациях, требующих немедленного вмешательства.

## <a name="incident-management"></a>Управление инцидентами
Компания Майкрософт реализует процесс управления инцидентами безопасности, чтобы облегчить скоординированное реагирование на возникающие инциденты.

Если корпорация Майкрософт узнает о любом несанкционированном доступе к каким-либо данным клиента, хранящимся на его оборудовании или в объектах компании, или о несанкционированном доступе к такому оборудованию или объектам, который приводит к потере, раскрытию или изменению данных клиента, корпорация Майкрософт предпримет такие действия:

- незамедлительное уведомление клиента об инциденте;
- незамедлительное расследование инцидента и предоставление клиенту подробной информации о нем;
- принятие обоснованных и быстрых мер для устранения последствий и сведение к минимуму любого ущерба, возникшего в результате инцидента.

Была создана платформа управления инцидентами, определяющая роли и распределяющая обязанности. Команда управления инцидентами безопасности Azure отвечает за управление инцидентами безопасности, включая эскалацию и обеспечение участия групп специалистов, когда это необходимо. Менеджеры по операциям Azure отвечают за контроль расследования и разрешения инцидентов безопасности и конфиденциальности.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о действиях корпорации Майкрософт в сфере защиты инфраструктуры Azure приведены в следующих статьях:

- [Объекты, локальная среда и физическая безопасность в Azure](physical-security.md)
- [Доступность инфраструктуры Azure](infrastructure-availability.md)
- [Компоненты и границы информационной системы Azure](infrastructure-components.md)
- [Сетевая архитектура Azure](infrastructure-network.md)
- [Рабочая сеть Azure](production-network.md)
- [Возможности безопасности Базы данных SQL Azure](infrastructure-sql.md)
- [Операции и управление в рабочей среде Azure](infrastructure-operations.md)
- [Целостность инфраструктуры Azure](infrastructure-integrity.md)
- [Защита данных клиентов в Azure](protection-customer-data.md)
