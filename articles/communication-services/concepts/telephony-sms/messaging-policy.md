---
title: Политика обмена сообщениями
titleSuffix: An Azure Communication Services concept document
description: Дополнительные сведения о политиках обмена сообщениями SMS.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 03/19/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: bb9765c2620f45d67bf888f8bfe8a4dee450cfd6
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105646549"
---
# <a name="azure-communication-services-messaging-policy"></a>Политика обмена сообщениями Служб коммуникации Azure

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Службы коммуникации Azure преобразуют способ общения наших заказчиков со своими клиентами благодаря созданию гибких и удобных настраиваемых условий взаимодействия за счет применения возможностей служб корпоративного уровня, которые применяются для Microsoft Teams и Skype. Интегрируйте функции обмена сообщениями SMS в свои коммуникационные решения и связывайтесь с клиентами в любое время и в любом месте, когда им нужна ваша поддержка. Просто помните о некоторых требованиях по обмену сообщениями, которые необходимо соблюдать.

Мы понимаем, что требования по обмену сообщениями могут показаться неудобными при изучении. Но на практике все оказывается очень просто. Не сложнее, чем запомнить понятие "COMS":

- C — Consent (Согласие)
- O — Opt-Out (Отказ от согласия)
- M — Message Content (Содержимое сообщения)
- S — Spoofing (Спуфинг)

Мы разработали эту политику обмена сообщениями, чтобы пользователи могли соблюдать нормативные требования и эффективно применять рекомендуемые методики. 

[!INCLUDE [Notice](../../includes/messaging-policy-include.md)]

## <a name="consent"></a>Согласие на предоставление разрешений 

### <a name="what-is-consent"></a>Что такое согласие?

Согласие — это соглашение между вами и получателем сообщений, которое дает вам право отправлять получателю автоматические сообщения. Согласие необходимо получить перед отправкой первого сообщения, и вы должны четко разъяснить получателю, что он согласен получать сообщения от вас. Такая процедура называется получением "предварительного четко выраженного согласия" от лица, которому вы планируете отправить сообщение.

Отправляемые сообщения должны относиться к тому типу сообщений, который получатель согласился получать, и отправляться только по номеру, указанному получателем. Если вы собираетесь отправлять информационные сообщения, такие как напоминания о встречах или оповещения, то согласие может быть письменным или устным. Если вы планируете отправлять рекламные сообщения, такие как коммерческие или маркетинговые сообщения, которые предназначены для продвижения продукта или услуги, согласие должно быть письменным.

### <a name="how-do-you-obtain-consent"></a>Как получить согласие?

Согласие можно получить разными способами, например:

- когда пользователь указывает свой номер телефона на веб-сайте; 
- когда пользователь инициирует обмен текстовыми сообщениями, или
- когда пользователь отправляет ключевое слово для регистрации по вашему номеру телефона. 
 
Независимо от способа получения согласия, вы и ваши клиенты должны убедиться, что согласие является однозначным. Область действия согласия должна быть понятна получателю.


### <a name="consent-requirements"></a>Требования к согласию:

- Перед получением согласия отправьте "Призыв к действию". Вы и ваши клиенты должны отправить потенциальным получателям сообщений "призыв к действию", которое будет содержать запрос на их согласие участвовать в вашей программе обмена сообщениями. Призыв к действию должен содержать, как минимум: (1) идентификационные данные отправителя сообщения, (2) четкие инструкции по предоставлению согласия, (3) инструкции по отказу от согласия и (4) сведения о любых сопутствующих сборах за передачу сообщений.
- Согласие нельзя передавать или назначать. Любое согласие, предоставляемое вам физическим лицом, не может быть передано или продано стороннему лицу, не являющемуся взаимозависимым. Если вы получаете согласие у физического лица для третьего лица, этому физическому лицу необходимо четко идентифицировать данное третье лицо. Кроме того, необходимо указать, что полученное согласие распространяется только на сообщения от третьего лица.
- Согласие ограничено рамками целей. Лицо, дающее свой номер для конкретной цели, предоставляет свое согласие на получение сообщений только для данной конкретной цели и только от данного отправителя сообщений. Перед получением согласия необходимо четко уведомить предполагаемого получателя сообщений о том, что вы будете отправлять повторяющиеся сообщения или сообщения от третьего лица.

### <a name="consent-best-practices"></a>Рекомендации по предоставлению согласия:

Помимо требований по обмену сообщениями, рассмотренных выше, вам может быть будет нужно применять разные типовые приемы, к которым относятся: 

- Подробные сведения в рамках "призыва к действию". Чтобы убедиться, что вы получаете соответствующее согласие, укажите:
  - имя или описание своей программы обмена сообщениями или продукта;
  - номера телефона, с которых получатели будут получать сообщения, и 
  - любые применимые условия перед тем, как физическое лицо даст свое согласие на получение сообщений от вас.
- Точные записи согласия. Записи любого согласия, предоставленного вам физическим лицом, необходимо хранить не менее четырех лет. Записи согласия могут включать в себя следующие сведения:
  - отметки времени
  - среда передачи, по которой получено согласие;
  - конкретная кампания, для которой было получено согласие;
  - снимки экрана;
  - ИД сеанса или IP-адрес физического лица, предоставившего свое согласие.
- Политики конфиденциальности и безопасности. Разработчикам рекомендуется создавать простые политики конфиденциальности, с которыми могут знакомиться получатели сообщений до получения их согласия. Мы также рекомендуем использовать средства проактивного управления безопасностью для защиты конфиденциальной информации физических лиц.


## <a name="double-opt-in-consent"></a>Двойное предоставление согласия:

Службы коммуникации Azure рекомендуют использовать двойное предоставление согласия для всех кампаний по обмену сообщениями. Двойное предоставление согласия — это двухэтапный процесс, в рамках которого физическое лицо сначала предоставляет согласие на получение от вас сообщений определенных типов. Затем вы отправляете ему дополнительное сообщение с просьбой подтвердить свое согласие. Дополнительные сообщения необходимо отправлять только после того, как получатель сообщения подтвердит свое согласие.

Исходное сообщение с подтверждением, которое вы отправляете, должно содержать ваши идентификационные данные, возможность отказа от согласия для будущих сообщений (например, использование команды "Остановить"), бесплатный номер телефона или команду "Справка" для получения дополнительных сведений, уведомление о том, что пользователь зарегистрирован в программе повторяющихся сообщений, краткое описание программы, предполагаемую частоту отправки повторяющихся сообщений и все сопутствующие сборы. 

### <a name="does-azure-communication-services-ever-require-double-opt-in-consent"></a>Требуется ли для Служб коммуникации Azure двойное предоставление согласия?
Да. Несмотря на то, что двойное предоставление согласия рекомендуется использовать всегда, Службы коммуникации Azure требуют использовать такой метод для кампаний по передаче сообщений некоторых типов вследствие их частого применения в схемах фишинга или большого количества жалоб потребителей на них. К таким кампаниям относятся:
- автоматические сообщения о гарантийных обязательствах;
- планы краткосрочного страхования жизни;
- сообщения о рефинансировании задолженности или снижении процентной ставки, если они не отправляются финансовыми организациями;
- сообщения о привлечении потенциальных клиентов;
- лотереи, конкурсы и викторины с выдачей призов;
- предложения по работе на дому.

Список кампаний, для которых требуется двойное предоставление согласия, может изменяться по усмотрению Службы коммуникации Azure.

### <a name="exceptions-to-traditional-consent-rules"></a>Ниже указаны исключения для традиционных правил предоставления согласия:
Хотя перед отправкой сообщения обычно требуется предварительное четко выраженное согласие, существуют две ситуации, в которых подразумевается согласие на сообщение.

- Получатель инициирует сеанс связи. Если физическое лицо инициирует сеанс связи, отправляя вам сообщение, вы можете предоставить запрашиваемые сведения в ответ на конкретный запрос, содержащийся в сообщении. Однако подразумеваемое согласие, предоставляемое этим человеком, ограничено сеансом связи, который он инициировал, если только вы не получите согласие на дальнейшую связь.
- Исключения для определенных услуг. Существует несколько специальных услуг, в рамках которых для инициирования сообщения может подразумеваться наличие согласия. Прежде всего, к ним относятся следующие услуги: 
- сообщения о доставке пакетов;
- сообщения из финансового учреждения, связанные со срочными вопросами (например, потенциально мошеннические транзакции или нарушение безопасности данных);
- сообщения от поставщика медицинских услуг, содержащие важную информацию, для которой критично время, и сведения, относящиеся к лечению (например, напоминание о визите к врачу или сдаче анализов, результаты лабораторных анализов и уведомления о рецептах). 
 
Такие сообщения не должны содержать рекламные предложения или объявления.


## <a name="opt-out"></a>Отказ от согласия

Получатели сообщений могут отозвать свое согласие и отказаться от получения сообщений в будущем, используя любые оправданные средства. Для отзыва согласия получателям сообщений нельзя указывать какое-либо исключительное средство. 

### <a name="opt-out-requirements"></a>Требования по отказу от согласия:

Убедитесь, что получатели сообщений могут отказаться от своего согласия на получение будущих сообщений в любое время. Необходимо также предоставить несколько вариантов отказа от согласия. После того как получатель сообщений отзовет свое согласие, ему не следует отправлять дополнительные сообщения, если только данный человек снова не предоставит свое согласие.

Одним из наиболее распространенных методов отказа от согласия является добавление ключевого слова "Остановить" в начальное сообщение для каждого нового сеанса связи. Будьте готовы к удалению клиентов, которые отвечают с использованием ключевого слова "остановить" (все буквы в нижнем регистре) или других распространенных ключевых слов, например "отменить подписку" или "отменить". После того как пользователь отзовет свое согласие, его следует удалить из всех кампаний по передаче повторяющихся сообщений, если только они не выразят явным образом согласие на продолжение получения сообщений в рамках определенной программы.

### <a name="opt-out-best-practices"></a>Рекомендации по отказу от согласия:

Кроме ключевых слов, к другим распространенным способам отказа от согласия являются сообщение клиентам специального адреса электронной почты для отказа от согласия, номера телефона сотрудников службы поддержки или ссылки на веб-страницу для отмены подписки. 


### <a name="how-we-handle-opt-out-requests"></a>Как мы обрабатываем запросы на отказ от согласия:

Если пользователь отправляет запрос на отказ от согласия на будущие сообщения в Службу коммуникации Azure по бесплатному номеру телефона, то весь последующий трафик с этого номера будет автоматически остановлен. Тем не менее необходимо убедиться, что вы не отправляете дополнительные сообщения в рамках этой кампании отправки сообщений с новых или других номеров телефона. Если вы отдельно получили четко выраженное согласие для другой кампании отправки сообщений, вы можете продолжать отправлять сообщения с другого номера телефона в рамках этой кампании. Дополнительные сведения об [обработке отказов от согласия](https://github.com/Prakulka/azure-docs-pr/blob/master/articles/communication-services/concepts/telephony-sms/sms-faq.md#how-can-i-receive-messages-using-azure-communication-services) см. в разделе часто задаваемых вопросов

## <a name="message-content"></a>Содержимое сообщения

### <a name="adult-content"></a>Содержимое для взрослых.

Содержимое сообщения, относящееся к таким темам, как секс, ненависть, алкоголь, огнестрельное оружие, курение, азартные игры, тотализаторы и конкурсы, может активировать дополнительные требования. Такое содержимое в некоторых юрисдикциях категорически запрещено. В случае отправки сообщения, где имеется такое содержимое, вы обязаны соблюдать все применимые законы юрисдикций, в которых реализуется взаимодействие. По запросу правоохранительных органов или Службы коммуникации Azure вы должны быть готовы предоставить подтверждения согласия с местными законодательными актами, регулирующими содержимое для взрослых.

Даже если такое содержимое не является противозаконным, необходимо предусмотреть способ проверки возраста при предоставлении согласия, чтобы отсечь по возрасту получателей от сообщений, предназначенных для взрослых. В США к маркетинговым материалам, предоставляемым детям младше 13 лет, применяются дополнительные юридические требования. 

### <a name="prohibited-content"></a>Запрещенное содержимое.

Службы коммуникации Azure запрещают определенное содержимое в сообщениях, независимо от наличия согласия. К запрещенному содержимому относятся:
- содержимое, которое призывает к противозаконным действиям (например, уклонение от уплаты налогов или жестокое обращение с животными в США);
- разжигание ненависти, клевета, притеснение или иные высказывания, которые признаны явно оскорбительными;
- порнографические материалы;
- непристойное или вульгарное содержимое;
- запугивание и угрозы;
- содержимое, нацеленное на мошенничество, обман, причинение вреда или противоправное получение материальных ценностей; 
- содержимое, подстрекающее к нанесению ущерба, к дискриминации или насилию;
- содержимое, связанное с распространением вредоносных программ;
- содержимое, нацеленное на несоблюдение требований по минимальному возрасту.

Мы оставляем за собой право изменять список запрещенного содержимого в сообщениях в любое время.

## <a name="spoofing"></a>спуфинг;

Спуфинг — это действие, приводящее к отображению неверного или неточного исходящего номера на устройстве получателя сообщений. Настоятельно рекомендуем вам и любому поставщику услуг, услугами которого вы пользуетесь, не отправлять поддельные сообщения. Спуфинг подменяет идентификационные данные отправителя сообщений и не дает возможности получателям сообщений отказаться от нежелательного взаимодействия. Мы также требуем, чтобы вы соблюдали все применимые законы в отношении спуфинга.

## <a name="final-thoughts"></a>В заключение

### <a name="legal-responsibility"></a>Юридическая ответственность:

Настоящая политика обмена сообщениями не является юридической консультацией. Мы оставляем за собой право изменять данную политику в любое время. Службы коммуникации Azure не несут ответственности за обеспечение соответствия содержимого, временных параметров или получателей сообщений наших клиентов всем применимым юридическим требованиям. 

Наши клиенты отвечают за соблюдение всех требований к обмену сообщениями. Если вы являетесь поставщиком платформы или программного обеспечения, использующим Службы коммуникации Azure для обмена сообщениями, вы обязаны требовать, чтобы ваши клиенты также соблюдали все требования, изложенные в данной политике обмена сообщениями. Дополнительные инструкции см. в документе [Принципы и рекомендации по обмену сообщениями](https://api.ctia.org/wp-content/uploads/2019/07/190719-CTIA-Messaging-Principles-and-Best-Practices-FINAL.pdf), подготовленном в CTIA.

### <a name="penalties"></a>Штрафные санкции

Мы призываем наших клиентов разрабатывать и внедрять политики и процедуры, предназначенные для обеспечения соответствия всем требованиям по обмену сообщениями. Нарушение требований по обмену сообщениями может привести к существенным штрафным санкциям, размер которых может быстро возрастать. В ваших интересах изучать и соблюдать все применимые требования по обмену сообщениями, разрабатывать эффективные меры защиты для ограничения и устранения нарушений до их распространения. Если вы нарушаете нашу политику обмена сообщениями или иные юридические требования, мы будем работать с вами с целью обеспечения соответствия требованиям в будущем. Однако мы оставляем за собой право удалить любого клиента с платформы Служб коммуникации Azure, который проявляет признаки несоответствия политике обмена сообщениями или юридическим требованиям.
