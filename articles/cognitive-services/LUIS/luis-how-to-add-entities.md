---
title: Добавление сущностей — LUIS
description: Создание сущностей для извлечения данных ключей из фразы продолжительностью пользователей в приложениях Распознавание речи (LUIS). Извлеченные данные сущности используются клиентским приложением для фуллфил запросов клиентов.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 05/17/2020
ms.openlocfilehash: c5c6836c2d68036bf2b9c5abe191943537349b8d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "91540971"
---
# <a name="add-entities-to-extract-data"></a>Добавление сущностей для извлечения данных

Создание сущностей для извлечения данных ключей из фразы продолжительностью пользователей в приложениях Распознавание речи (LUIS). Извлеченные данные сущности используются клиентским приложением для фуллфил запросов клиентов.

Сущность представляет подлежащее извлечению слово или фразу в высказывании. Сущности описывают информацию, относящуюся к намерению, и иногда они необходимы приложению для выполнения поставленной задачи. Вы можете создавать сущности, когда вы добавляете пример utterance в намерение или помимо него (до или после), добавляя пример utterance к намерению.

## <a name="plan-entities-then-create-and-label"></a>Планирование сущностей, создание и добавление меток

сущности машинного обучения можно создать на основе примера фразы продолжительностью или создать на странице **сущности** .

Как правило, рекомендуется потратить время на планирование сущностей перед созданием сущности машинного обучения на портале. Затем создайте сущность машинного обучения на основе примера utterance с подробным описанием вложенных сущностей и функций, которые вы знакомы в момент времени. В [учебнике по сущностям делимыми](tutorial-machine-learned-entity.md) показано, как использовать этот метод.

В рамках планирования сущностей может быть известно, что вам потребуются сущности, совпадающие с текстом (например, предварительно созданные сущности, сущности регулярных выражений или список сущностей). Их можно создать на странице **сущности** , прежде чем они помечаются в примере фразы продолжительностью.

При создании меток можно либо пометить отдельные сущности, либо создать сборку до родительской сущности машинного обучения. Также можно начать с родительской сущности машинного обучения и разложить ее на дочерние сущности.

> [!TIP]
>Пометка всех слов, которые могут указывать на сущность, даже если эти слова не используются при извлечении в клиентском приложении.

## <a name="when-to-create-an-entity"></a>Когда следует создавать сущность

После планирования сущностей необходимо создать сущности машинного обучения и вложенные сущности. Для этого может потребоваться добавить предварительно созданные сущности или совпадающие с текстом сущности, чтобы предоставить функции для сущностей машинного обучения. Все они должны быть выполнены перед добавлением меток.

После того как вы начали добавлять метки к примеру фразы продолжительностью, можно создавать сущности, изученные компьютером или расширяющие сущности списка.

Используйте следующую таблицу, чтобы узнать, где создать или добавить каждый тип сущности в приложение.

|Тип сущности|Где создать сущность на портале LUIS|
|--|--|
|сущность машинного обучения|Сущности или сведения о намерениях|
|Сущность списка|Сущности или сведения о намерениях|
|Сущность регулярного выражения|Сущности|
|Сущность Pattern.any|Сущности|
|Предварительно созданная сущность|Сущности|
|Предварительно созданная сущность домена|Сущности|

Можно создать все сущности на странице **сущности** или создать пару сущностей в качестве метки для сущности в примере utterance на странице **сведений о намерениях** . Вы можете _пометить_ сущность в примере utterance на странице **сведений о намерениях** .



## <a name="how-to-create-a-new-custom-entity"></a>Создание новой настраиваемой сущности

Этот процесс работает для сущностей, предназначенных для машинного обучения, списка сущностей и сущностей регулярных выражений.

1. Войдите на портал [LUIS](https://www.luis.ai) и выберите **Подписка** и **Ресурс для разработки**, чтобы просмотреть приложения, назначенные этому ресурсу для разработки.
1. Откройте приложение, выбрав его имя на странице " **Мои приложения** ".
1. Выберите страницу **сущности** .
1. Выберите **+ создать**, а затем выберите тип сущности.
1. Продолжайте настройку сущности, а затем выберите **создать** по завершении.

## <a name="create-a-machine-learned-entity"></a>Создание сущности, полученной от компьютера

1. Войдите на портал [LUIS](https://www.luis.ai) и выберите **Подписка** и **Ресурс для разработки**, чтобы просмотреть приложения, назначенные этому ресурсу для разработки.
1. Откройте приложение, выбрав его имя на странице " **Мои приложения** ".
1. В разделе **Сборка** выберите **сущности** на левой панели, а затем щелкните **+ создать**.
1. В диалоговом окне **Создание типа сущности** введите имя сущности и выберите **полученные от компьютера**, а затем выберите. Чтобы добавить подсущности, выберите **Добавить структуру**. Нажмите кнопку **создания**.

    > [!div class="mx-imgBorder"]
    > ![Снимок экрана создания сущности, полученной от компьютера.](media/add-entities/machine-learned-entity-with-structure.png)

1. В меню **Добавление подсущностей** добавьте подсущность, выбрав в **+** строке родительской сущности.

    > [!div class="mx-imgBorder"]
    > ![Снимок экрана добавления вложенных сущностей.](media/add-entities/machine-learned-entity-with-subentities.png)

1. Нажмите кнопку **создать** , чтобы завершить процесс создания.

## <a name="add-a-feature-to-a-machine-learned-entity"></a>Добавление компонента в сущность, полученная от компьютера

1. Войдите на портал [LUIS](https://www.luis.ai) и выберите **Подписка** и **Ресурс для разработки**, чтобы просмотреть приложения, назначенные этому ресурсу для разработки.
1. Откройте приложение, выбрав его имя на странице " **Мои приложения** ".
1. В разделе **Build (сборка** ) выберите **сущности** на левой панели, а затем выберите сущность, полученные от компьютера.
1. Добавьте компонент, выбрав **+ Добавить функцию** в строке сущность или подсущность.
1. Выберите существующие сущности и списки фраз.
1. Если сущность должна быть извлечена только в том случае, если она найдена, то для этой функции следует выбрать звездочку `*` .

    > [!div class="mx-imgBorder"]
    > ![Снимок экрана добавления функции в сущность.](media/add-entities/machine-learned-entity-schema-with-features.png)

## <a name="create-a-regular-expression-entity"></a>Создание сущности регулярного выражения

1. Войдите на портал [LUIS](https://www.luis.ai) и выберите **Подписка** и **Ресурс для разработки**, чтобы просмотреть приложения, назначенные этому ресурсу для разработки.
1. Откройте приложение, выбрав его имя на странице " **Мои приложения** ".
1. В разделе **Сборка** выберите **сущности** на левой панели, а затем щелкните **+ создать**.

1. В диалоговом окне **Создание типа сущности** введите имя сущности и выберите **Regex**, введите регулярное выражение в поле **Regex** и нажмите кнопку **создать**.

    > [!div class="mx-imgBorder"]
    > ![Снимок экрана создания сущности регулярного выражения.](media/add-entities/add-regular-expression-entity.png)


<a name="add-list-entities"></a>

## <a name="create-a-list-entity"></a>Создавать сущности списка

Сущности списка — это фиксированный закрытый набор связанных машинных слов. В то время как автор может изменить список, LUIS не будет увеличивать или уменьшать список. Также можно импортировать в существующую сущность List, используя [Формат List Entity. JSON](reference-entity-list.md#example-json-to-import-into-list-entity).

В следующем списке показано каноническое имя и синонимы.

|Имя элемента списка цветов|Цвета-синонимы|
|--|--|
|Красный|Crimson, кровь, Apple, пожар-Engine|
|Синий|Небесно, кобалт|
|Зеленый|Келли, травяной|

Используйте процедуру, чтобы создать сущность списка. После создания сущности List не нужно помечать пример фразы продолжительностью с намерением. Элементы списка и синонимы сопоставляются с использованием точного текста.
1. Войдите на портал [LUIS](https://www.luis.ai) и выберите **Подписка** и **Ресурс для разработки**, чтобы просмотреть приложения, назначенные этому ресурсу для разработки.
1. Откройте приложение, выбрав его имя на странице " **Мои приложения** ".
1. В разделе **Сборка** выберите **сущности** на левой панели, а затем щелкните **+ создать**.

1. В диалоговом окне **Создание типа сущности** введите имя сущности, например `Colors` и **список** выбора.
1. В диалоговом окне **Создание сущности списка** в поле **Добавить новый подсписок...**. Введите имя элемента списка, например `Green` , и добавьте синонимы.

    > [!div class="mx-imgBorder"]
    > ![Создайте список цветов как сущность списка на странице сведений о сущности.](media/how-to-add-entities/create-list-entity-of-colors.png)

1. По завершении добавления элементов списка и синонимов выберите **создать**.

    Завершив работу с группой изменений в приложении, не забудьте **обучить** приложение. Не обучить приложение после одного изменения.

    > [!NOTE]
    > Эта процедура демонстрирует создание и маркировку сущности List из примера utterance на странице **сведений о намерениях** . Одну и ту же сущность можно также создать на странице **сущности** .

## <a name="add-a-role-for-an-entity"></a>Добавление роли для сущности

Роль — это именованный подтип сущности на основе контекста.

### <a name="add-a-role-to-distinguish-different-contexts"></a>Добавление роли для различения разных контекстов

В следующем utterance есть два расположения, и каждое из них определяется семантически по словам, таким как `to` и `from` :

`Pick up the package from Seattle and deliver to New York City.`

В этой процедуре добавьте `origin` роли и `destination` в предварительно созданную сущность geographyV2.
1. Войдите на портал [LUIS](https://www.luis.ai) и выберите **Подписка** и **Ресурс для разработки**, чтобы просмотреть приложения, назначенные этому ресурсу для разработки.
1. Откройте приложение, выбрав его имя на странице " **Мои приложения** ".
1. В разделе **Сборка** на левой панели выберите **Сущности**.

1. Выберите **+ добавить предварительно созданную сущность**. Выберите **geographyV2** , а затем нажмите кнопку **Готово**. При этом в приложение добавляется предварительно созданная сущность.

    Если ваш шаблон, который включает сущность Pattern.any, извлекает сущности неправильно, используйте [явный список](reference-pattern-syntax.md#explicit-lists), чтобы решить эту проблему.

1. Выберите только что добавленную предварительно созданную сущность geographyV2 из списка **сущностей** сущностей.
1. Чтобы добавить новую роль, выберите пункт **+** Далее, чтобы **не добавлять роли**.
1. В текстовом поле **Type Role...** введите имя роли, `Origin` а затем введите. Добавьте второе имя роли, `Destination` а затем введите.

    > [!div class="mx-imgBorder"]
    > ![Снимок экрана добавления роли отправления сущности Location](media/how-to-add-entities//add-role-to-prebuilt-geographyv2-entity.png)

    Роль добавляется в предварительно созданную сущность, но не добавляется в фразы продолжительностью с помощью этой сущности.

### <a name="label-text-with-a-role-in-an-example-utterance"></a>Добавление метки к тексту с ролью в примере utterance

> [!TIP]
> Роли можно заменять путем добавления меток к подсущностям сущностей машинного обучения.

1. Войдите на портал [LUIS](https://www.luis.ai) и выберите **Подписка** и **Ресурс для разработки**, чтобы просмотреть приложения, назначенные этому ресурсу для разработки.
1. Откройте приложение, выбрав его имя на странице " **Мои приложения** ".
1. Перейдите на страницу сведений о намерениях, в которой приведен пример фразы продолжительностью, который использует эту роль.
1. Чтобы добавить метку к роли, выберите метку сущности (сплошная линия под текстом) в примере utterance, а затем в раскрывающемся списке выберите **Просмотр в области сущностей** .

    > [!div class="mx-imgBorder"]
    > ![Снимок экрана: Отображение выбранного элемента меню области сущностей.](media/add-entities/view-in-entity-pane.png)

    Справа откроется палитра сущностей.

1. Выберите сущность, затем перейдите в нижнюю часть палитры и выберите роль.

    > [!div class="mx-imgBorder"]
    > ![На снимке экрана показано, где выбрать роль.](media/add-entities/select-role-in-entity-palette.png)

<a name="add-pattern-any-entities"></a>
<a name="add-a-patternany-entity"></a>
<a name="create-a-pattern-from-an-utterance"></a>

## <a name="create-a-patternany-entity"></a>Создание шаблона. любая сущность

**Шаблон. Любая** сущность доступна только с [шаблонами](luis-how-to-model-intent-pattern.md).


## <a name="do-not-change-entity-type"></a>Не изменяйте тип сущности

Интеллектуальная служба распознавания речи не позволяет изменить тип сущности, поскольку неизвестно, что добавлять или удалять для создания этой сущности. Чтобы изменить тип, лучше создать новую сущность правильного типа с незначительно отличающимся именем. После создания сущности в каждом высказывании удалите старое отмеченное имя сущности и добавьте новое. После того как все высказывания будут повторно отмечены, удалите старую сущность.


## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Использование предварительно созданных моделей](howto-add-prebuilt-models.md)

Дополнительные сведения
* [Обучение](luis-how-to-train.md)
* [Тестирование](luis-interactive-test.md)
* Как [опубликовать](luis-how-to-publish-app.md)
* Закономерно
    * [Основные понятия](luis-concept-patterns.md)
    * [Синтаксис](reference-pattern-syntax.md)
* [Репозиторий GitHub с предварительно созданными сущностями](https://github.com/Microsoft/Recognizers-Text)
* [Основные понятия извлечения данных](luis-concept-data-extraction.md)



