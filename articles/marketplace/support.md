---
title: Получение поддержки для программы коммерческого рынка в центре партнеров
description: Узнайте о вариантах поддержки программы коммерческого рынка в центре партнеров, в том числе о том, как отправить запрос в службу поддержки.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: navits09
ms.author: navits
ms.date: 01/19/2020
ms.openlocfilehash: a1726b29c153bf680d29fe821ac34aa958064335
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98879247"
---
# <a name="support-for-the-commercial-marketplace-program-in-partner-center"></a>Поддержка программы коммерческого рынка в центре партнеров

Корпорация Майкрософт предоставляет поддержку самых разнообразных продуктов и услуг. Чтобы обеспечить надлежащий и своевременный отклик, важно найти нужную группу поддержки. Чтобы отправить ваш запрос правильной группе поддержки, учитывайте следующие сценарии.

- Если вы являетесь издателем и у вас есть вопрос у клиента, попросите его обратиться в службу поддержки, используя ссылки на поддержку в [портал Azure](https://portal.azure.com/).
- Если вы являетесь издателем и обнаружили проблемы с безопасностью приложения, работающего в Azure, см. статью [как зарегистрировать запрос в службу поддержки для событий безопасности](../security/fundamentals/event-support-ticket.md). Издатели должны сообщать о подозрительных событиях безопасности, включая инциденты безопасности и уязвимости своих предложений программного обеспечения и служб Azure Marketplace при первой же возможности.
- Если вы являетесь издателем и у вас есть вопрос, относящийся к вашему приложению или службе, ознакомьтесь со следующими вариантами поддержки.

## <a name="get-help-or-open-a-support-ticket"></a>Получение справки или обращение в службу поддержки

1. Войдите в свою рабочую учетную запись. Если вы еще не сделали этого, вам нужно будет [создать учетную запись центра партнеров](partner-center-portal/create-account.md).

1. В меню в правом верхнем углу страницы щелкните значок **поддержки** . В правой части страницы появится панель **Справка и поддержка** .

1. Чтобы получить справку по коммерческому магазину, выберите **коммерческое Marketplace**.

   ![Раскрывающееся меню поддержки](./media/support/commercial-marketplace-support-pane.png)

1. В поле **Сводка проблемы** введите краткое описание проблемы.

1. В поле **тип проблемы** выполните одно из следующих действий.

    - **Вариант 1**. Введите ключевые слова, такие как Marketplace, приложение Azure, предложение SaaS, Управление учетными записями, управление интересами, проблемы развертывания, выплата или перенос предложения для совместной продажи. Затем выберите тип проблемы из отображаемого списка рекомендуемых.

    - **Вариант 2**. Выберите пункт **Обзор разделов** в списке **Категория** , а затем выберите **коммерческий** магазин. Затем выберите соответствующий **раздел** и **подраздел**.

1. Выбрав нужный раздел, щелкните **проверить решения**.

    ![Следующий шаг](./media/support/next-step.png)

Показаны следующие параметры.

- Чтобы выбрать другой раздел, щелкните **выбрать другую ошибку**.
- Чтобы устранить эту проблему, ознакомьтесь с рекомендуемыми действиями и документами, если они доступны.

    ![Рекомендуемые решения](./media/support/recommended-solutions.png)

Если вы не можете найти свой ответ в справке самостоятельно, выберите **указать сведения о вопросе**. Заполните все обязательные поля, чтобы ускорить процесс разрешения, а затем выберите **Отправить**.

>[!Note]
>Если вы не вошли в центр партнеров, вам может потребоваться выполнить вход, прежде чем можно будет создать билет.

## <a name="track-your-existing-support-requests"></a>Следите за существующими запросами в службу поддержки

Чтобы проверить открытые и закрытые билеты, в меню навигации слева выберите пункт поддержка **коммерческого рынка**  >  .

## <a name="record-issue-details-with-a-har-file"></a>Запись сведений о проблемах с помощью файла HAR

Чтобы помочь агентам службы поддержки в устранении проблемы, можно присоединить файл формата HTTP Archive (HAR) к запросу в службу поддержки. Файлы HAR — это журналы сетевых запросов в веб-браузере.

> [!WARNING]
> Файлы HAR могут записывать конфиденциальные данные о вашей учетной записи центра партнеров.

### <a name="microsoft-edge-and-google-chrome"></a>Microsoft ребро и Google Chrome

Чтобы создать файл HAR с помощью **Microsoft ребр** или **Google Chrome**, сделайте следующее:

1. Перейдите на веб-страницу, где возникла проблема.
2. В правом верхнем углу окна щелкните значок многоточия, а затем выберите **инструменты** для  >  **разработчиков**. Можно нажать клавишу F12 в качестве ярлыка.
3. В области средства разработчика перейдите на вкладку **сеть** .
4. Выберите пункт **отключить запись журнала сети** и **снимите флажок** , чтобы удалить существующие журналы. Значок записи превратится в серый цвет.

    ![Удаление существующих журналов в Microsoft ребр или Google Chrome](media/support/chromium-stop-clear-session.png)

5. Выберите **запись журнала сети** , чтобы начать запись. Когда вы начнете запись, значок записи станет красным.

    ![Как начать запись в Microsoft ребр или Google Chrome](media/support/chromium-start-session.png)

6. Воспроизведите проблему, которую вы хотите устранить.
7. Выполнив повторное устранение проблемы, выберите команду " **отключить запись журнала сети**".
8. Выберите **Экспорт Har**, помеченный значком со стрелкой вниз и сохраните файл.

    ![Экспорт файла HAR в Microsoft ребр или Google Chrome](media/support/chromium-network-export-har.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox;

Создание файла HAR с помощью **Mozilla Firefox**:

1. Перейдите на веб-страницу, где возникла проблема.
1. В правом верхнем углу окна щелкните значок с многоточием, а затем — средства **веб-разработчика**  >  . Можно нажать клавишу F12 в качестве ярлыка.
1. Перейдите на вкладку **сеть** и нажмите кнопку **очистить** , чтобы удалить существующие журналы.

    ![Удаление существующих журналов в Mozilla Firefox](media/support/firefox-clear-session.png)

1. Воспроизведите проблему, которую вы хотите устранить.
1. После воссоздания проблемы выберите **Har Export/Import**  >  **сохранить все как HAR**.

    ![Экспорт файла HAR в Mozilla Firefox](media/support/firefox-network-export-har.png)

### <a name="apple-safari"></a>Apple Safari.

Создание файла HAR с помощью **Safari**:

1. Включение средств разработчика в Safari: выберите параметры **Safari**  >  . Перейдите на вкладку **Дополнительно** , а затем **в строке меню выберите команду Отобразить разработку**.
1. Перейдите на веб-страницу, где возникла проблема.
1. Щелкните **Develop** (Разработка), а затем **Show Web Inspector** (Показать веб-инспектор).
1. Перейдите на вкладку **сеть** и выберите **очистить сетевые элементы** , чтобы удалить существующие журналы.

    ![Удаление существующих журналов в Safari](media/support/safari-clear-session.png)

1. Воспроизведите проблему, которую вы хотите устранить.
1. После воспроизведения проблемы выберите **Экспорт** и сохраните файл.

    ![Экспорт файла HAR в Safari](media/support/safari-network-export-har.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Update an existing offer in the Commercial Marketplace](partner-center-portal/update-existing-offer.md) (Обновление имеющегося предложения на коммерческой платформе Marketplace)