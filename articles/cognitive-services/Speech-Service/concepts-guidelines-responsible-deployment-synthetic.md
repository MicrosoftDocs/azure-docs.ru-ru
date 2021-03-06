---
title: Рекомендации для ответственного развертывания технологии искусственного голоса
titleSuffix: Azure Cognitive Services
description: Общие рекомендации корпорации Майкрософт по проектированию технологии искусственного голоса. Они были разработаны в ходе исследований, которые корпорация Майкрософт проработала с специалистами по голосовой связи, потребителями, а также сотрудниками, от речь, о разработке искусственного голоса.
services: cognitive-services
author: benoah
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: benoah
ms.openlocfilehash: 3a0b645acd7c21ff0416c748cdd2caf7041be508
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "101730819"
---
# <a name="guidelines-for-responsible-deployment-of-synthetic-voice-technology"></a>Рекомендации для ответственного развертывания технологии искусственного голоса

В этой статье вы узнаете о общих рекомендациях корпорации Майкрософт по проектированию технологии искусственного голоса. Эти рекомендации были разработаны в ходе исследований, которые корпорация Майкрософт проработала с специалистами по голосовой связи, потребителями и лицами с речью, от речь о ответственной разработке искусственных голосов.

Для развертывания технологии искусственного распознавания речи в большинстве случаев применяются следующие рекомендации.

### <a name="disclose-when-the-voice-is-synthetic"></a>Раскрывать при использовании искусственного голоса
Раскрытие голоса, созданного компьютером, не только снижает риск возникновения вредоносных результатов от обману, но и повышает доверие в Организации, которая доставляет голоса. Узнайте больше о [том, как раскрывать](concepts-disclosure-guidelines.md).

Корпорация Майкрософт требует, чтобы клиенты раскрыли искусственный характер пользовательского нейронного голоса для пользователей. 
* Обеспечьте адекватное раскрытие аудиторий, особенно при использовании голоса от хорошо известного человека. люди делают их ненужными в зависимости от того, кто его доставляет, независимо от того, есть ли у них сомнения или нет.  Например, раскрытие может быть совместно предоставлено в начале вещания. Дополнительные сведения см. в [шаблонах раскрытия](concepts-disclosure-patterns.md).   
* Рассмотрите возможность правильного раскрытия родителей или других участников с вариантами использования, предназначенными для дополнительных элементов и детей. Если ваш вариант использования предназначен для дополнительных или дочерних элементов, необходимо убедиться, что родители или юридические лица могут понять раскрытие с использованием искусственного носителя и принять решение о незначительных или дочерних возможностях использования этих возможностей. 

### <a name="select-appropriate-voice-types-for-your-scenario"></a>Выбор подходящих типов голоса для вашего сценария
Тщательно рассмотрите контекст использования и потенциальные угрозы, связанные с использованием искусственного голоса. Например, синтетические искусственные голоса могут оказаться непригодными для сценариев с высоким уровнем риска, например для личных сообщений, финансовых транзакций или сложных ситуаций, требующих ручной адаптации или сопереживание. 

Пользователи также могут иметь разные ожидания для типов голоса. Например, при прослушивании конфиденциальных новостей, прочитанных искусственным голосовым, некоторые пользователи предпочитают более привычным и человеческий тон, тогда как другие предпочитают несмещенную речь. Рассмотрите возможность тестирования приложения, чтобы лучше понять предпочтения пользователя.

### <a name="be-transparent-about-capabilities-and-limitations"></a>Быть прозрачным для возможностей и ограничений
При взаимодействии с высокоточными агентами искусственного голоса пользователи могут столкнуться с более высокими ожиданиями. Если системные возможности не соответствуют этим ожиданиям, доверие может снизиться, и это может привести к неприятные или даже потенциально опасному взаимодействии.

### <a name="provide-optional-human-support"></a>Предоставление дополнительной поддержки для людей
При неоднозначных сценариях транзакций (например, в центре поддержки вызовов) пользователи не всегда могут доверять агенту компьютера для надлежащего реагирования на их запросы. В таких ситуациях может потребоваться поддержка людей, независимо от реалистичного качества голоса или возможностей системы.

## <a name="considerations-for-voice-talent"></a>Рекомендации для специалистов по голосовым сообщениям
При работе с правами голоса, например с голосовыми субъектами, для создания искусственных голосов, приведенные ниже рекомендации применимы.

### <a name="obtain-meaningful-consent-from-voice-talent"></a>Получение осмысленного согласия от передачи голоса
Voice наработками должен контролировать свою голосовую модель (как и где они будут использоваться) и компенсировать ее использование. Корпорация Майкрософт требует от клиентов специального голоса получить явно письменное разрешение от своего голоса, чтобы создать искусственный голоса и его соглашение с помощью Voice наработками, а также все ограничения содержимого.  При создании искусственного голоса для хорошо известного человека следует предоставить пользователю, находящимся за ним, возможность изменить или утвердить содержимое.

Некоторые голоса наработками не знают о потенциально вредоносном использовании технологии и должны быть обучены владельцами системы о возможностях технологии. Корпорация Майкрософт требует, чтобы клиенты могли поделиться [раскрытием голоса](/legal/cognitive-services/speech-service/disclosure-voice-talent) с голосовыми специалистами в прямом сотрудничестве или с помощью полномочного представителя, который описывает, как искусственные голоса разрабатываются и работают вместе с текстом в речевых службах.

## <a name="considerations-for-those-with-speech-disorders"></a>Рекомендации для от речи
При работе с людьми с помощью от речи для создания или развертывания технологии искусственного голоса применяются следующие рекомендации.

### <a name="provide-guidelines-to-establish-contracts"></a>Предоставление рекомендаций по созданию контрактов
Предоставьте рекомендации по настройке контрактов для лиц, которые используют искусственный голос для помощи в работе. В контракте следует указать лиц, владеющих голосом, длительностью использования, критериями передачи владения, процедурами удаления шрифта голоса и предотвращения несанкционированного доступа. Кроме того, если было предоставлено разрешение, включите перенос шрифта в качестве владельца голоса после смерти к членам семьи.

### <a name="account-for-inconsistencies-in-speech-patterns"></a>Учет несоответствий в шаблонах речи
Для пользователей с голосовыми отами, которые записывают собственные голосовые шрифты, несоответствия в шаблоне речи (слурринг или невозможность произношения определенных слов) может усложнить процесс записи. В таких случаях необходимо учитывать искусственные технологии и сеансы записи (т. е. обеспечить перерывы и дополнительное число сеансов записи).

### <a name="allow-modification-over-time"></a>Разрешить изменение с течением времени
Пользователи с голосовыми отами хотят вносить изменения в свои искусственные голоса, чтобы отражать устаревание (например, дочерний объект, пуберти). Кроме того, пользователи могут изменять стилистические настройки, которые изменяются со временем, и могут захотеть изменить тон, диакритические знаки или другие характеристики голоса.


## <a name="see-also"></a>См. также раздел

* [Раскрытие голоса для речи](/legal/cognitive-services/speech-service/disclosure-voice-talent?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext)
* [Как раскрывать](concepts-disclosure-guidelines.md)
* [Шаблоны проектирования раскрытия](concepts-disclosure-patterns.md)