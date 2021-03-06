---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 06/02/2020
ms.author: mimart
ms.openlocfilehash: 2ad6b90616077a6d18550e86692b109bda622af7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "96025847"
---
Чтобы зарегистрировать приложение в клиенте Azure AD B2C, можно использовать новый унифицированный интерфейс **регистрации приложений** или устаревший интерфейс **приложений (прежняя версия)** . [См. дополнительные сведения о новом интерфейсе](../articles/active-directory-b2c/app-registrations-training-guide.md).

#### <a name="app-registrations"></a>[Регистрация приложений](#tab/app-reg-ga/)

1. Войдите на [портал Azure](https://portal.azure.com).
1. Выберите фильтр **Каталог и подписка** в верхнем меню, а затем выберите каталог, содержащий клиент Azure AD B2C.
1. В меню слева выберите **Azure AD B2C**. Либо щелкните **Все службы**, а затем найдите и выберите **Azure AD B2C**
1. Щелкните **Регистрация приложений** и выберите **Новая регистрация**.
1. Введите **имя** приложения. Например, *nativeapp1*.
1. В разделе **Поддерживаемые типы учетных записей** выберите **Учетные записи в любом каталоге организации или поставщике удостоверений**.
1. В разделе **URI перенаправления** в раскрывающемся списке выберите **Общедоступный/собственный клиент (мобильный и классический)** .
1. Введите URI перенаправления с уникальной схемой. Например, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. При выборе URI перенаправления нужно учитывать следующие важные моменты:
    * **Разработка** Для задач разработки можно задать URI перенаправления как `http://localhost`, и Azure AD B2C будет учитывать любой порт в запросе. Если зарегистрированный URI содержит порт, Azure AD B2C будет использовать только этот порт. Например, если зарегистрированный URI перенаправления задан как `http://localhost`, URI перенаправления в запросе может быть `http://localhost:<randomport>`. Если зарегистрированный URI перенаправления задан как `http://localhost:8080`, URI перенаправления в запросе должен быть `http://localhost:8080`.
    * **Уникальные**. Схема URI перенаправления должна быть уникальной для каждого приложения. В примере `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect``com.onmicrosoft.contosob2c.exampleapp` представляет собой схему. Рекомендуется придерживаться этого шаблона. Если два приложения используют ту же схему, пользователю предоставляется возможность выбрать приложение. Если пользователь сделает неправильный выбор, произойдет сбой входа.
    * **Полнота.** В URI перенаправления должна быть схема и путь. Путь должен содержать по крайней мере одну косую черту после имени домена. Например, `//oauth/` — правильно, а `//oauth` — неправильно. Не добавляйте в URI специальные символы, например символы подчеркивания.
1. В разделе **Разрешения** установите флажок *Предоставьте согласие администратора для разрешений openid и offline_access*.
2. Выберите **Зарегистрировать**.

#### <a name="applications-legacy"></a>[Приложения (прежние версии)](#tab/applications-legacy/)

1. Войдите на [портал Azure](https://portal.azure.com).
1. Выберите фильтр **Каталог и подписка** в верхнем меню, а затем выберите каталог, содержащий клиент Azure AD B2C.
1. В меню слева выберите **Azure AD B2C**. Либо щелкните **Все службы**, а затем найдите и выберите **Azure AD B2C**
1. Щелкните **Applications (Legacy)** (Приложения (Прежние версии)), а затем выберите **Добавить**.
1. Введите имя приложения. Например, *nativeapp1*.
1. Для поля **Собственный клиент** выберите **Да**.
1. Введите **пользовательский URI перенаправления** с уникальной схемой. Например, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. При выборе URI перенаправления нужно учитывать два важных момента:
    * **Уникальные**. Схема URI перенаправления должна быть уникальной для каждого приложения. В примере `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect``com.onmicrosoft.contosob2c.exampleapp` представляет собой схему. Рекомендуется придерживаться этого шаблона. Если два приложения используют ту же схему, пользователю предоставляется возможность выбрать приложение. Если пользователь сделает неправильный выбор, произойдет сбой входа.
    * **Полнота.** В URI перенаправления должна быть схема и путь. Путь должен содержать по крайней мере одну косую черту после имени домена. Например, `//oauth/` — правильно, а `//oauth` — неправильно. Не добавляйте в URI специальные символы, например символы подчеркивания.
1. Нажмите кнопку **создания**.