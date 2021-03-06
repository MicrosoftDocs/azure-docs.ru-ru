---
title: Что такое центр безопасности Azure | Документация Майкрософт
description: На этой странице описаны основные преимущества Центра безопасности — определение состояния безопасности и его улучшение с учетом облачных и локальных ресурсов.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/17/2021
ms.author: memildin
ms.openlocfilehash: 741cd68145b262c1f200ced9a7f28b25673b6925
ms.sourcegitcommit: 425420fe14cf5265d3e7ff31d596be62542837fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/20/2021
ms.locfileid: "107738924"
---
# <a name="what-is-azure-security-center"></a>Что такое Центр безопасности Azure?

Центр безопасности Azure — единая система управления безопасностью инфраструктуры, которая повышает уровень защищенности центров обработки данных и предоставляет расширенную защиту от угроз в гибридных рабочих нагрузках в облаке независимо от того, находятся ли они в Azure или нет, а также локально.

Безопасность ваших ресурсов — это совместные действия вашего поставщика облачных служб, Azure и вас, клиента. Необходимо убедиться, что рабочие нагрузки являются безопасными при переходе в облако. В то же время перемещение в IaaS (инфраструктура как услуга) в большей мере ответственность клиента, чем перемещение в PaaS (платформа как услуга) и SaaS (программное обеспечение как услуга). Центр безопасности Azure предоставляет средства, необходимые для усиления безопасности вашей сети, защиты служб и гарантии того, что ваши системы находятся на верхнем уровне безопасности.

Центр безопасности Azure справляется с тремя наиболее срочными угрозами безопасности:

-   **Быстро меняющиеся рабочие нагрузки**. Это преимущество и недостаток облака. С одной стороны пользователи получают расширенные возможности. С другой стороны — как можно гарантировать, что постоянно меняющиеся службы, применяемые и создаваемые пользователями, соответствуют вашим стандартам безопасности и рекомендациям?

-   **Все более изощренные атаки**. Где бы вы ни выполняли рабочие нагрузки, атаки становятся все более изощренными. Вам нужно защитить свои рабочие нагрузки в публичном облаке, которые являются, по сути, рабочими нагрузками с доступом в Интернет, которые могут увеличить число уязвимостей, если вы не следуете рекомендациям по безопасности.

-   **Специалистов в области безопасности недостаточно**. Количество оповещений безопасности и систем оповещения значительно превышает количество администраторов с необходимой подготовкой и опытом, которые помогут вам убедиться в том, что среды защищены. Оставаться в курсе последних атак — это постоянная задача, которая не позволяет оставаться на месте, так как мир систем безопасности постоянно меняется.

Чтобы помочь вам защититься от этих проблем, Центр безопасности предоставляет средства для таких задач:

-   **Повышение уровня безопасности**. Центр безопасности оценивает среду и предоставляет вам сведения о состоянии ресурсов, а также об их безопасности.

-   **Защита от угроз**. Центр безопасности оценивает рабочие нагрузки и выдает рекомендации по предотвращению угроз и оповещения системы безопасности.

-   **Быстрое получение системы безопасности**. В Центре безопасности все выполняется с облачной скоростью. Так как он изначально интегрирован, развернуть Центр безопасности несложно благодаря автоматической подготовке и защите с помощью служб Azure.

> [!NOTE]
> Эта служба поддерживает [Azure Lighthouse](../lighthouse/overview.md), что позволяет поставщикам услуг входить в собственный арендатор для управления подписками и группами ресурсов, которые делегируют клиенты. В сценариях использования Центра безопасности Azure следует делегировать подписку, а не отдельные группы ресурсов.

## <a name="architecture"></a>Architecture

Так как Центр безопасности является частью Azure, он отслеживает и защищает службы в Azure, включая Service Fabric, Базу данных SQL, Управляемый экземпляр SQL и учетные записи хранения, без необходимости выполнять развертывание.

Кроме того, Центр безопасности защищает сторонние серверы, не связанные с Azure, и виртуальные машины в облаке или локально, для серверов Windows и Linux, путем установки агента Log Analytics на них. Виртуальные машины Azure автоматически подготавливаются в Центре безопасности.

События, собранные с агентов и Azure, коррелируются в модуле аналитики безопасности для предоставления вам специализированных рекомендаций (задач по усилению), которым необходимо следовать, чтобы убедиться, что рабочие нагрузки являются безопасными, а также оповещений системы безопасности. Следует обратить внимание на такие оповещения как можно скорее, чтобы убедиться, что на рабочие нагрузки не происходит вредоносных атак.

Если включить Центр безопасности, встроенные в нем политики безопасности будут отражены как встроенная инициатива в категории "Центр безопасности" в службе "Политика Azure". Встроенная инициатива автоматически назначается всем подпискам, зарегистрированным в Центре безопасности (независимо от того, включено ли в них средство Azure Defender). Встроенная инициатива содержит только политики аудита. Дополнительные сведения об использовании политик Центра безопасности в службе "Политика Azure" см. в статье [Использование политик безопасности](tutorial-security-policy.md).

## <a name="strengthen-security-posture"></a>Повышение уровня безопасности

Центр безопасности Azure позволяет усилить систему безопасности. Это означает, что он помогает определить и выполнить задачи усиления защиты, рекомендуемые как лучшие методики по обеспечению безопасности, а также внедрить их на ваших компьютерах, в службах данных и приложениях. Это включает управление и принудительное применение политик безопасности и обеспечение соответствия виртуальных машин Azure, сторонних серверов, не связанных с Azure, и служб Azure PaaS. Центр безопасности предоставляет средства, необходимые для внимательного просмотра рабочих нагрузок и фокусирования на состоянии вашей сети безопасности. 

### <a name="manage-organization-security-policy-and-compliance"></a>Управление политикой безопасности организации и соответствия требованиям

Для обеспечения безопасности в первую очередь нужно убедиться, что ваши рабочие нагрузки в безопасности, и начать с внедрения настроенных политик безопасности. Так как все политики в Центре безопасности создаются на основе элементов управления службы "Политика Azure", в вашем распоряжении полнофункциональная и гибкая **политика мирового класса**. В Центре безопасности можно задать политики, применяемые в группах управления, между подписками и даже для всего клиента.

:::image type="content" source="./media/security-center-intro/sc-dashboard.png" alt-text="Страница управления политиками":::

Центр безопасности помогает **определять подписки для использования теневых ИТ**. Взглянув на подписки с меткой **не защищены** на панели мониторинга, вы можете немедленно узнать о недавно созданных подписках и убедиться, что они защищаются политиками и Центром безопасности Azure.

:::image type="content" source="./media/security-center-intro/sc-policy-dashboard.png" alt-text="Панель мониторинга политики Центра безопасности":::

### <a name="continuous-assessments"></a>Непрерывные оценки

Центр безопасности постоянно обнаруживает новые ресурсы, развертываемые в рабочих нагрузках, и оценивает, настроены ли они в соответствии с рекомендациями по безопасности. Если нет, они помечаются и вы получаете список приоритетных рекомендаций относительно того, что необходимо исправить для защиты своих компьютеров. Список рекомендаций реализуется и поддерживается [Azure Security Benchmark](https://docs.microsoft.com/security/benchmark/azure/introduction), руководствами с рекомендациями по обеспечению безопасности и соответствия требованиям для Azure, подготовленные корпорацией Майкрософт на основе распространенных платформ соответствия. Это широко принятое тестирование основано на стандартах [Центра Интернет-безопасности (CIS)](https://www.cisecurity.org/benchmark/azure/) и [Национального института стандартов и технологий (NIST)](https://www.nist.gov/) с ориентацией на облачную безопасность.

Чтобы помочь вам понять, насколько важна каждая рекомендация для вашего общего состояния безопасности, в Центре безопасности рекомендации сгруппированы по средствам управления безопасностью, для которых указывается значение **оценки безопасности**. Очень важно **приоритизировать работу системы безопасности**.

:::image type="content" source="./media/security-center-intro/sc-secure-score.png" alt-text="Оценка безопасности в Центре безопасности":::

### <a name="network-map"></a>Карта сети

Одно из самых мощных средств Центра безопасности для постоянного мониторинга состояния защиты сети — это **карта сети**. Карта позволяет вам просмотреть топологию рабочих нагрузок, чтобы вы могли убедиться, что каждый узел настроен правильно. Вы можете увидеть, как соединены ваши узлы, что помогает блокировать нежелательные подключения, которые потенциально упрощают злоумышленнику проникновение в вашу сеть.

:::image type="content" source="./media/security-center-intro/sc-net-map.png" alt-text="Карта сети Центра безопасности":::


### <a name="optimize-and-improve-security-by-configuring-recommended-controls"></a>Оптимизация и повышение уровня безопасности путем настройки рекомендуемых элементов управления

Основная ценность Центра безопасности Azure состоит в его рекомендациях. Рекомендации предназначены для разрешения определенных проблем безопасности, найденных в ваших рабочих нагрузках, Центр безопасности выполняет за вас работу администратора безопасности: он не только находит уязвимости, но и предоставляет вам определенные инструкции по их устранению.

Таким образом, Центр безопасности позволяет не только задавать политики безопасности, но и применять стандарты конфигурации безопасности в ваших ресурсах.

Рекомендации помогают снизить вероятность атак каждого из ваших ресурсов. Это включает виртуальные машины Azure, серверы за пределами Azure и службы Azure PaaS, такие как SQL и учетные записи хранения, и многое другое, где каждый тип ресурса оценивается иначе и имеет собственные стандарты.

:::image type="content" source="./media/security-center-intro/sc-recommendation-example.png" alt-text="Пример рекомендации Центра безопасности":::

## <a name="protect-against-threats"></a>Защита от угроз

Защита от угроз Центра безопасности позволяет обнаруживать и предотвращать угрозы на уровне инфраструктуры как услуги (IaaS), серверов за пределами Azure, а также на уровне платформы как услуги (PaaS) в Azure.

Защита от угроз Центра безопасности включает объединение анализа цепочки отказов, который автоматически сопоставляет оповещения в среде на основе анализа цепочки отказов в киберсреде, чтобы помочь лучше понять полную историю кампании атаки, откуда она была запущена и какое влияние имела на ваши ресурсы.

:::image type="content" source="media/security-center-managing-and-responding-alerts/alerts-page.png" alt-text="Список оповещений системы безопасности в Центре безопасности Azure":::

### <a name="integration-with-microsoft-defender-for-endpoint"></a>Интеграция с Microsoft Defender для конечной точки

Azure Defender для серверов предусматривает автоматическую нативную интеграцию с Microsoft Defender для конечной точки. Дополнительные сведения см. в статье [Защита конечных точек с помощью интегрированного решения EDR Центра безопасности: Microsoft Defender для конечной точки](security-center-wdatp.md).


### <a name="protect-paas"></a>Защита PaaS

Центр безопасности помогает выявлять угрозы в службах Azure PaaS. Вы можете обнаружить угрозы для служб Azure, включая службы приложений Azure, Azure SQL, учетную запись хранения Azure и другие службы данных. Вы также можете воспользоваться интеграцией платформенной функциональности в аналитике поведения пользователей и сущностей (UEBA) Microsoft Cloud App Security для обнаружения аномалий в журналах действий Azure.

### <a name="block-brute-force-attacks"></a>Блокирование атак методом подбора

Центр безопасности позволяет снизить уязвимость к атакам методом подбора. За счет сокращения доступа к портам виртуальной машины можно усилить свою сеть, предотвращая ненужный доступ с применением JIT-доступа к виртуальной машине. Вы можете установить политики безопасного доступа на выбранных портах только для авторизованных пользователей, допустимых диапазонов IP-адресов источника или IP-адресов и на ограниченный период времени.

### <a name="protect-data-services"></a>Защита службы данных

Центр безопасности включает возможности, которые помогут вам автоматически выполнять классификацию данных в Azure SQL. Вы также можете получить оценки потенциальных уязвимостей в Azure SQL и службе хранилища, а также рекомендации по их устранению.

## <a name="get-secure-faster"></a>Быстрое получение системы безопасности

Интеграция платформенной функциональности Azure (включая Политику Azure и журналы Azure Monitor) в сочетании с беспроблемной интеграцией с другими решениями безопасности корпорации Майкрософт, такими как Microsoft Cloud App Security и Microsoft Defender для конечной точки, помогает сделать ваше решение безопасности комплексным, а также простым в подключении и развертывании.

Кроме того, вы можете расширить полное решение за пределы Azure в рабочие нагрузки, выполняющиеся на других облаках и в локальных центрах обработки данных.

### <a name="automatically-discover-and-onboard-azure-resources"></a>Автоматическое обнаружение и подключение ресурсов Azure

Центр безопасности предоставляет простую встроенную интеграцию с платформой Azure и ее ресурсами. Это означает, что вы можете объединить систему безопасности, включающую службу "Политика Azure" и встроенные политики Центра безопасности во всех ресурсах Azure и убедиться, что они автоматически применяются к обнаруженным ресурсам по мере их создания в Azure.

Расширенный сбор журналов — журналы Windows и Linux используются в модуле анализа безопасности для создания рекомендаций и предупреждений.

## <a name="next-steps"></a>Дальнейшие действия

- Для начала работы с Центром безопасности необходима подписка Microsoft Azure. Если у вас нет подписки, вы можете зарегистрироваться для получения [бесплатной пробной версии](https://azure.microsoft.com/free/).

- Если вы впервые переходите на панель мониторинга Центра безопасности Azure на портале Azure или включаете Центр безопасности программным способом через API, ценовая категория "Бесплатный" Центра безопасности будет включена во всех текущих подписках Azure. Чтобы воспользоваться преимуществами расширенного управления безопасностью и функциями обнаружения угроз, нужно включить Azure Defender. Средство Azure Defender доступно в бесплатной пробной версии в течение 30 дней. Дополнительные сведения см. на странице с [ценами на центр безопасности](https://azure.microsoft.com/pricing/details/security-center/).

- Если вы готовы включить Azure Defender сейчас, ознакомьтесь со статьей [Краткое руководство. Настройка Центра безопасности Azure](security-center-get-started.md), где приведены пошаговые инструкции.