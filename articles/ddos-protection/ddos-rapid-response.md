---
title: Быстрое реагирование Защиты от атак DDoS Azure
description: Узнайте, как привлечь экспертов от атак DDoS во время активной атаки для получения специализированной поддержки.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: yitoh
ms.openlocfilehash: 8e860bf47420f2b58c44df695da7761bcc2aa0ce
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "100521787"
---
# <a name="azure-ddos-rapid-response"></a>Быстрое реагирование Защиты от атак DDoS Azure

При активном доступе клиенты стандарта защиты Azure от атак DDoS имеют доступ к группе от атак DDoS быстрого реагирования (ДРР), которая может помочь в исследовании атак во время атаки и анализа после атаки.

## <a name="prerequisites"></a>Предварительные условия

- Прежде чем выполнять действия, описанные в этом руководстве, необходимо создать [план защиты Azure от атак DDoS Standard](manage-ddos-protection.md).

## <a name="when-to-engage-drr"></a>Когда следует привлекать ДРР

ДРР следует привлекать только в том случае, если: 

- Во время атаки от атак DDoS, если обнаруживается, что производительность защищенного ресурса сильно снижена, или ресурс недоступен. 
- вы считаете, что ресурс подвергся атаке DDoS, но служба Защиты от атак DDoS неэффективно противодействует атаке;
- Вы планируете вирусное событие, которое значительно увеличит ваш сетевой трафик.
- Для атак, имеющих важное влияние на бизнес.

## <a name="engage-drr-during-an-active-attack"></a>Привлечение ДРР во время активной атаки

1. В портал Azure при создании нового запроса на поддержку выберите **тип проблемы** в качестве технической.
2. Выберите **Служба** как **Защита от атак DDoS**.
3. Выберите ресурс в раскрывающемся меню ресурсов. _Необходимо выбрать план от атак DDoS, связанный с виртуальной сетью, защищенной с помощью от атак DDoS Protection Standard, для привлечения ДРР._

    ![Выбор ресурса](./media/ddos-rapid-response/choose-resource.png)

4. На следующей странице **проблемы** выберите **серьезность** критического влияния и **тип проблемы** в разделе "атака".

    ![Псеверити и тип проблемы](./media/ddos-rapid-response/severity-and-problem-type.png)

5. Выполните дополнительные сведения и отправьте запрос в службу поддержки.

ДРР соответствует модели поддержки Azure Rapid Response. Дополнительные сведения о быстром отклике см. в разделе [Поддержка области действия и скорости реагирования](https://azure.microsoft.com/en-us/support/plans/response/) .

Дополнительные сведения см. в [стандартной документации по защите от атак DDoS](./ddos-protection-overview.md).

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как выполнять [тестирование с помощью симуляций](test-through-simulations.md).
- Узнайте, как [просматривать и настраивать данные телеметрии защиты от атак DDoS](telemetry.md).
- Узнайте, как [просматривать и настраивать ведение журнала диагностики от атак DDoS](diagnostic-logging.md).
