---
title: Оценка характеристик — Персонализация
titleSuffix: Azure Cognitive Services
description: При выполнении оценки в ресурсе персонализации из портал Azure Персонализация предоставляет сведения о том, какие функции контекста и действия проводятся на модель.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: c0e47a2943cf8c934d201f76aefc41868adf0b25
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "87127729"
---
# <a name="feature-evaluation"></a>Оценка возможностей

При выполнении оценки в ресурсе персонализации из [портал Azure](https://portal.azure.com)Персонализация предоставляет сведения о том, какие функции контекста и действия проводятся на модель. 

Это полезно в следующих целях:

* Представьте себе дополнительные функции, которые можно использовать, чтобы получить сведения о том, какие функции более важны в модели.
* Узнайте, какие функции не важны, и, возможно, удалите их или проанализируйте, что может повлиять на использование.
* Предоставьте рекомендации по редакционным или контрольм командам о новом содержимом или о продуктах, которые стоит перевести в каталог.
* Устранение распространенных проблем и ошибок, возникающих при отправке компонентов в персонализацию.

Более важные функции имеют более высокий вес в модели. Так как эти функции имеют более высокий вес, они, как правило, находятся в том случае, когда персонализация получает более высокие выгоды.

## <a name="getting-feature-importance-evaluation"></a>Получение оценки важности компонентов

Чтобы увидеть результаты важности компонентов, необходимо выполнить оценку. В ходе оценки создаются удобные для восприятия метки функций на основе имен компонентов, наблюдаемых в течение периода оценки.

Итоговая информация о важности признаков представляет текущую модель в сети персонализации. Ознакомительная версия анализирует важность функции модели, сохраненной на дату окончания периода оценки, после того, как все обучение будет выполнено во время оценки, с текущей политикой обучения в Интернете. 

Результаты важности функций не представляют другие политики и модели, протестированные или созданные во время оценки.  Ознакомительная версия не будет включать функции, отправленные персонализации по истечении периода оценки.

## <a name="how-to-interpret-the-feature-importance-evaluation"></a>Как интерпретировать оценку важности функции

Персонализация оценивает функции, создавая "группы" функций, имеющих одинаковую важность. Можно сказать, что одна группа имеет большую степень важности, чем другие, но в рамках группы упорядочивание функций осуществляется в алфавитном порядке.

Сведения о каждом компоненте включают:

* Поступает ли компонент из контекста или действий.
* Ключ и значение функции.

Например, приложение заказа на покупку с немаловажным магазином может увидеть «context. Погода: горячий» в качестве очень важной функции.

Персонализация отображает корреляции функций, которые, когда учитывается вместе, обеспечивают более высокие выгоды.

Например, вы можете увидеть "Context. Погода: горячий *с* Action. MenuItem: ицекреам", а также "Context. Погода: холодный *с* Action. MenuItem: вармтеа:

## <a name="actions-you-can-take-based-on-feature-evaluation"></a>Действия, которые можно выполнить на основе оценки компонентов

### <a name="imagine-additional-features-you-could-use"></a>Представьте себе дополнительные функции, которые можно использовать

Познакомьтесь с наиболее важными функциями модели. Например, если вы видите «context. Мобилебаттери: Low» в мобильном приложении, можно подумать, что тип соединения может также сделать так, чтобы клиенты могли увидеть один видеоклип на другом, а затем добавить в приложение функции, связанные с типом подключения и пропускной способностью.

### <a name="see-what-features-are-not-important"></a>Узнайте, какие функции не важны

Возможно удаление неважных функций или дальнейшее анализ того, что может повлиять на использование. Возможности могут быть неограниченными по многим причинам. Может быть, что подлинная функция не влияет на поведение пользователя. Но это также может означать, что эта функция не очевидна для пользователя. 

Например, видеосайт может увидеть, что "Action. Видеоресолутион = 4000" является функцией низкого уровня, которая противоречит исследованию пользователей. Причиной может быть то, что приложение даже не упоминает или не отображает разрешение видео, поэтому пользователи не изменили их поведение на их основе.

### <a name="provide-guidance-to-editorial-or-curation-teams"></a>Предоставление руководств редакционным или контроль командам

Предоставьте рекомендации по новому содержимому или продуктам, которые следует перевести в каталог. Персонализация разработана как средство, которое дополняет персонал и команды. Одним из способов это является предоставление информации для редакционных групп о продуктах, статьях или содержимом, посвященном поведению. Например, в сценарии приложения для видео может быть видно, что имеется важный компонент с именем Action. Видеоентитиес. cat: true, предлагающий редакционной группе разместить видео в категории CAT.

### <a name="troubleshoot-common-problems-and-mistakes"></a>Устранение распространенных проблем и ошибок

Распространенные проблемы и ошибки можно устранить, изменив код приложения, чтобы они не отправляли недопустимые или неправильно отформатированные функции для персонализации. 

Распространенные ошибки при отправке компонентов включают следующее.

* Отправка личных сведений (PII). Личные сведения, характерные для одного лица (например, имя, номер телефона, номера кредитных карт, IP-адреса), не следует использовать с персонализацией. Если приложению требуется отслеживание пользователей, используйте неидентифицирующий UUID или другой идентификатор UserID. В большинстве сценариев это также проблематично.
* При большом количестве пользователей маловероятно, что взаимодействие с каждым пользователем будет взвесить больше, чем все взаимодействие с Генеральной совокупностью, так что отправка идентификаторов пользователей (даже если они не являются PII), вероятно, повышают уровень шума, чем значение для модели.
* Отправка полей даты и времени в виде точных меток времени вместо значений времени признаками. Использование таких функций, как context. TimeStamp. Day = понедельник или "Context. TimeStamp. Hour" = "13", является более полезным. Для каждого из них будет не более 7 или 24 значений компонентов. Но "Context. TimeStamp": "1985-04-12T23:20:50.52 Z" настолько точно, что из него не будет изучено, так как это никогда не будет происходить повторно.

## <a name="next-steps"></a>Дальнейшие действия

Общие сведения о [масштабируемости и производительности](concepts-scalability-performance.md) с помощью персонализации.

