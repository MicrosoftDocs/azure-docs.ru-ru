---
title: Azure Active Directory отчеты о действиях входа — Предварительная версия | Документация Майкрософт
description: Общие сведения о отчетах о действиях входа на портале Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/16/2021
ms.author: markvi
ms.reviewer: besiler
ms.collection: M365-identity-device-management
ms.openlocfilehash: 185638d683699403c304603d968cfe84e32a55b5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103574566"
---
# <a name="azure-active-directory-sign-in-activity-reports---preview"></a>Отчеты об активности входа Azure Active Directory — Предварительная версия

Архитектура создания отчетов в Azure Active Directory (Azure AD) состоит из следующих компонентов:

- **Действие** 
    - **Входы** — сведения о том, когда пользователи, приложения и управляемые ресурсы входят в Azure AD и получают доступ к ресурсам.
    - **Журналы аудита**  -  [Журналы аудита](concept-audit-logs.md) предоставляют сведения о действиях пользователей и управления группами, управляемых приложениях и действиях с каталогами.
- **Безопасность** 
    - **Рискованные входы** . [рискованный вход](../identity-protection/overview-identity-protection.md) — это индикатор попытки входа, которая не является законным владельцем учетной записи пользователя.
    - **Пользователи, помеченные для риска** — [рискованный пользователь](../identity-protection/overview-identity-protection.md) является индикатором для учетной записи пользователя, которая могла быть скомпрометирована.

Классический отчет о событиях входа в Azure Active Directory предоставляет обзор интерактивных входов пользователей. Кроме того, теперь у вас есть доступ к трем дополнительным отчетам для входа, которые теперь доступны в предварительной версии:

- Неинтерактивные входы пользователей

- Входы субъекта-службы

- Управляемые удостоверения для входа в ресурсы Azure

В этой статье приводится обзор отчета о действиях входа с предварительной версией неинтерактивных, приложений и управляемых удостоверений для входа в ресурсы Azure. Сведения о отчете о входе без предварительных версий функций см. в разделе  [отчеты о действиях входа на портале Azure Active Directory](concept-sign-ins.md).



## <a name="prerequisites"></a>Предварительные требования

Прежде чем приступить к работе с этой функцией, необходимо ознакомиться с ответами на следующие вопросы:

- Кто имеет доступ к данным?

- Какие лицензии Azure AD требуются для доступа к действию входа?

### <a name="who-can-access-the-data"></a>Кто имеет доступ к данным?

- Пользователи в роли "администратор безопасности", "читатель безопасности" и "читатель отчетов"

- Глобальные администраторы.

- Любой пользователь (не администратор) может получить доступ к своим данным о действиях входа. 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Какие лицензии Azure AD требуются для доступа к действию входа?

Для просмотра действий входа клиент должен иметь связанную с ним лицензию Azure AD Premium. Чтобы обновить выпуск Azure Active Directory, ознакомьтесь со статьей [Регистрация для работы с выпусками Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md). Для отображения данных в отчетах после обновления до лицензии Premium без действий с данными до обновления может потребоваться несколько дней.



## <a name="sign-ins-report"></a>Отчет о входе

Отчет о событиях входа содержит ответы на следующие вопросы.

- Что такое шаблон входа пользователя, приложения или службы?
- Сколько пользователей, приложений или служб выполнили вход за неделю?
- Каков статус их входа?


В колонке отчет о событиях входа можно переключаться между:

- **Интерактивные входы** в систему — входы в систему, где пользователь предоставляет фактор проверки подлинности, например пароль, ответ через приложение MFA, биометрический фактор или QR-код.

- **Неинтерактивные входы** в систему — входы, выполняемые клиентом от имени пользователя. Для этих входов не требуется взаимодействие или фактор проверки подлинности от пользователя. Например, проверка подлинности и авторизация с помощью маркеров обновления и доступа, не требующих ввода учетных данных пользователем.

- **Входы в систему субъекта-службы** — это приложения и субъекты служб, которые не используют какого-либо пользователя. В этих операциях входа приложение или служба предоставляют учетные данные от своего имени для проверки подлинности или доступа к ресурсам.

- **Управляемые удостоверения для ресурсов Azure входы** — входы в систему с помощью ресурсов Azure с секретами, которыми управляет Azure. Дополнительные сведения см. в статье [что такое управляемые удостоверения для ресурсов Azure?](../managed-identities-azure-resources/overview.md) 


![Типы отчетов о входе в систему](./media/concept-all-sign-ins/sign-ins-report-types.png)












## <a name="user-sign-ins"></a>Вход пользователей

На каждой вкладке в колонке входов отображаются столбцы по умолчанию, приведенные ниже. Некоторые вкладки имеют дополнительные столбцы.

- дата входа;

- Идентификатор запроса

- Имя пользователя или идентификатор пользователя

- Имя приложения или идентификатор приложения

- Состояние входа

- IP-адрес устройства, используемого для входа



### <a name="interactive-user-sign-ins"></a>Интерактивные входы пользователей


Интерактивные входы пользователей — это входы, в которых пользователь обеспечивает проверку подлинности в Azure AD или взаимодействует напрямую с Azure AD или с вспомогательным приложением, например Microsoft Authenticator приложением. К факторам пользователей относятся пароли, ответы на проблемы MFA, биометрические факторы или QR-коды, предоставляемые пользователем в Azure AD или в вспомогательное приложение.

> [!NOTE]
> Этот отчет также включает Федеративные входы из поставщиков удостоверений, которые являются федеративными в Azure AD.  



Примечание. интерактивный отчет о входах пользователей, используемый для хранения некоторых неинтерактивных операций входа от клиентов Microsoft Exchange. Хотя эти операции входа были неинтерактивными, они были добавлены в отчет интерактивных входов пользователей для дополнительной видимости. Когда Неинтерактивный отчет о входе в систему не заполнится общедоступной предварительной версией в ноябре 2020, эти журналы событий входа в систему не были перемещены в Неинтерактивный отчет о входе пользователя для повышения точности. 


**Размер отчета:** мелкий <br> 
**Примеров**

- Пользователь предоставляет имя пользователя и пароль на экране входа в Azure AD.

- Пользователь передает запрос MFA по SMS.

- Пользователь предоставляет биометрический жест для разблокировки компьютера Windows с помощью Windows Hello для бизнеса.

- Пользователь является федеративным в Azure AD с помощью утверждения AD FS SAML.


В дополнение к полям по умолчанию в отчет о интерактивных входах также отображается следующее: 

- Расположение для входа

- Применен ли условный доступ



Чтобы настроить представление списка, нажмите **Столбцы** на панели инструментов.

![Интерактивные столбцы входа пользователей](./media/concept-all-sign-ins/columns-interactive.png "Интерактивные столбцы входа пользователей")





Настройка представления позволяет отображать дополнительные поля или удалять уже отображаемые поля.

![Все интерактивные столбцы](./media/concept-all-sign-ins/all-interactive-columns.png)


Выберите элемент в представлении списка, чтобы получить более подробные сведения о соответствующем входе.

![Действие входа](./media/concept-all-sign-ins/interactive-user-sign-in-details.png "Интерактивные входы пользователей")



### <a name="non-interactive-user-sign-ins"></a>Неинтерактивные входы пользователей

Неинтерактивные входы пользователей — это входы, выполненные клиентским приложением или компонентами ОС от имени пользователя. Как и интерактивные входы пользователей, эти операции входа выполняются от имени пользователя. В отличие от интерактивных входов пользователей, эти операции входа не повлияют на то, что пользователь должен указать фактор проверки подлинности. Вместо этого устройство или клиентское приложение использует маркер или код для проверки подлинности или доступа к ресурсу от имени пользователя. Как правило, пользователь будет воспринимать эти входы как происходящие в фоновом режиме действия пользователя.


**Размер отчета:** Достаточ <br>
**Примеры:** 

- Клиентское приложение использует маркер обновления OAuth 2,0 для получения маркера доступа.

- Клиент использует код авторизации OAuth 2,0 для получения маркера доступа и маркера обновления.

- Пользователь выполняет единый вход (SSO) в веб-приложение или на Windows на компьютере, присоединенном к Azure AD.

- Пользователь входит во второе приложение Microsoft Office, когда у него есть сеанс на мобильном устройстве с помощью ФОЦИ (семейства идентификаторов клиентов).




Помимо полей по умолчанию, отчет о неинтерактивных входах также показывает: 

- Идентификатор ресурса

- Количество сгруппированных входов в систему




Вы не можете настраивать поля, показанные в этом отчете.


![Отключенные столбцы](./media/concept-all-sign-ins/disabled-columns.png "Отключенные столбцы")

Для упрощения дайджеста данных события входа в систему группируются. Клиенты часто создают множество неинтерактивных входов от имени одного пользователя за короткий период времени, который совместно используют все те же характеристики, за исключением времени попытки входа. Например, клиент может получить маркер доступа один раз в час от имени пользователя. Если пользователь или клиент не меняют состояние, IP-адрес, ресурс и все остальные сведения одинаковы для каждого запроса маркера доступа. Когда Azure AD регистрирует несколько входов, которые идентичны, кроме даты и времени, эти операции входа из одной и той же сущности объединяются в одну строку. Строка с несколькими идентичными операциями входа (за исключением даты и времени) будет иметь значение больше 1 в столбце #-INS. Можно развернуть строку, чтобы просмотреть все различные операции входа и их различные метки времени. Операции входа объединяются в неинтерактивных пользователях при совпадении следующих данных.


- Приложение

- Пользователь

- IP-адрес

- Состояние

- Идентификатор ресурса


Вы можете:

- Разверните узел, чтобы просмотреть отдельные элементы группы.  

- Щелкните отдельный элемент, чтобы просмотреть все сведения 


![Сведения о неинтерактивном входе пользователя](./media/concept-all-sign-ins/non-interactive-sign-ins-details.png)




## <a name="service-principal-sign-ins"></a>Входы субъекта-службы

В отличие от интерактивных и неинтерактивных входов пользователей, входы субъекта-службы не затрагивают пользователя. Вместо этого они входят в систему по любым учетным записям, не являющимся пользователями, таким как приложения или субъекты-службы (за исключением входа управляемого удостоверения, которые включены только в отчет о входе в управляемые удостоверения). В этих операциях входа приложение или служба предоставляет собственные учетные данные, например сертификат или секрет приложения для проверки подлинности или доступа к ресурсам.


**Размер отчета:** Достаточ <br>
**Примеры:**

- Субъект-служба использует сертификат для проверки подлинности и доступа к Microsoft Graph. 

- Приложение использует секрет клиента для проверки подлинности в потоке учетных данных клиента OAuth. 


Этот отчет имеет представление списка по умолчанию, которое показывает:

- дата входа;

- Идентификатор запроса

- Имя или идентификатор субъекта-службы

- Состояние

- IP-адрес

- Имя ресурса

- Идентификатор ресурса

- Число входов в систему

Вы не можете настраивать поля, показанные в этом отчете.

![Отключенные столбцы](./media/concept-all-sign-ins/disabled-columns.png "Отключенные столбцы")

Для упрощения дайджеста данных в журналах входа субъекта-службы группируются события входа субъекта-службы. Операции входа из одной и той же сущности в одних и тех же условиях объединяются в одну строку. Можно развернуть строку, чтобы просмотреть все различные операции входа и их различные метки времени. Операции входа обрабатываются в отчете субъекта-службы при совпадении следующих данных.

- Имя или идентификатор субъекта-службы

- Состояние

- IP-адрес

- Имя или идентификатор ресурса

Вы можете:

- Разверните узел, чтобы просмотреть отдельные элементы группы.  

- Щелкните отдельный элемент, чтобы просмотреть все сведения 


![Сведения о столбце](./media/concept-all-sign-ins/service-principals-sign-ins-view.png "Сведения о столбце")




## <a name="managed-identity-for-azure-resources-sign-ins"></a>Управляемое удостоверение для входа в ресурсы Azure 

Управляемое удостоверение для входа в ресурсы Azure — это входы, которые были выполнены ресурсами с секретами, управляемыми Azure, для упрощения управления учетными данными.

**Размер отчета:** Значительные <br> 
**Примеров**

Виртуальная машина с управляемыми учетными данными использует Azure AD для получения маркера доступа.   


Этот отчет имеет представление списка по умолчанию, которое показывает:


- Идентификатор управляемого удостоверения

- Имя управляемого удостоверения

- Ресурс

- Идентификатор ресурса

- Количество сгруппированных входов в систему

Вы не можете настраивать поля, показанные в этом отчете.

Для упрощения дайджеста данных управляемые удостоверения для ресурсов Azure входят в журналы входа в систему, а не Интерактивные события входа группируются. Операции входа из одной и той же сущности объединяются в одну строку. Можно развернуть строку, чтобы просмотреть все различные операции входа и их различные метки времени. Операции входа обрабатываются в отчете управляемые удостоверения, если все следующие данные совпадают.

- Имя или идентификатор управляемого удостоверения

- Состояние

- IP-адрес

- Имя или идентификатор ресурса

Выберите элемент в представлении списка, чтобы отобразить все операции входа, сгруппированные под узлом.

Выберите сгруппированный элемент, чтобы просмотреть все сведения о входе в систему. 


## <a name="filter-sign-in-activities"></a>Фильтрация действий входа

Настроив фильтр, можно ограничить область возвращаемых данных входа. Azure AD предоставляет широкий спектр дополнительных фильтров, которые можно задать. При настройке фильтра следует всегда уделять особое внимание заданному фильтру диапазона **дат** . Правильный фильтр диапазона дат гарантирует, что Azure AD возвратит только те данные, которые вас интересуют.     

Фильтр диапазона **дат** позволяет определить временные рамки для возвращаемых данных.
Возможны следующие значения:

- Один месяц

- Семь дней

- Двадцать четыре часа

- Особые настройки

![Фильтр диапазона дат](./media/concept-all-sign-ins/date-range-filter.png)





### <a name="filter-user-sign-ins"></a>Фильтрация входов пользователей

Фильтр для интерактивных и неинтерактивных входов совпадает. По этой причине фильтр, настроенный для интерактивных входов, сохраняется для неинтерактивных входов и наоборот. 






## <a name="access-the-new-sign-in-activity-reports"></a>Доступ к новым отчетам о действиях входа 

Отчет о действиях входа в портал Azure предоставляет простой метод переключения и отключения предварительного просмотра отчета. Если предварительные отчеты включены, вы получаете новое меню, которое предоставляет доступ ко всем типам отчетов о действиях входа.     


Для доступа к новым отчетам входа с помощью неинтерактивных и входов в приложения: 

1. На [портале Azure](https://portal.azure.com)выберите **Azure Active Directory**.

    ![Выбор Azure AD](./media/concept-all-sign-ins/azure-services.png)

2. В разделе **мониторинг** щелкните **входы**.

    ![Выбор входа](./media/concept-all-sign-ins/sign-ins.png)

3. Щелкните панель **предварительного просмотра** .

    ![Включить новое представление](./media/concept-all-sign-ins/enable-new-preview.png)

4. Чтобы вернуться к представлению по умолчанию, снова щелкните панель **предварительного просмотра** . 

    ![Восстановить классический вид](./media/concept-all-sign-ins/switch-back.png)







## <a name="download-sign-in-activity-reports"></a>Скачать отчеты о действиях входа

При скачивании отчета о действиях входа выполняется следующее:

- Отчет о входе можно скачать в формате CSV или JSON.

- Вы можете загрузить до 100-K записей. Если вы хотите загрузить дополнительные данные, используйте API отчетов.

- Загрузка основана на сделанном выборе фильтра.

- На число записей, которые можно скачать, влияют особенности [политики хранения отчетов Azure Active Directory](reference-reports-data-retention.md). 


![Скачивание отчетов](./media/concept-all-sign-ins/download-reports.png "Скачивание отчетов")


Каждая загрузка CSV состоит из шести разных файлов:

- Интерактивные входы в систему

- Сведения о проверке подлинности интерактивных входов

- Неинтерактивные входы в систему

- Сведения о проверке подлинности неинтерактивных входов

- Входы субъекта-службы

- Управляемое удостоверение для входа в ресурсы Azure

Каждая загрузка JSON состоит из четырех разных файлов:

- Интерактивные входы в систему (включая сведения о проверке подлинности)

- Неинтерактивные входы (включая сведения о проверке подлинности)

- Входы субъекта-службы

- Управляемое удостоверение для входа в ресурсы Azure

![Загрузка файлов](./media/concept-all-sign-ins/download-files.png "Загрузка файлов")




## <a name="next-steps"></a>Следующие шаги

* [Коды ошибок в отчете о действиях входа](reference-sign-ins-error-codes.md)
* [Политики хранения отчетов Azure Active Directory](reference-reports-data-retention.md)
* [Задержки в отчетах Azure Active Directory](reference-reports-latencies.md)