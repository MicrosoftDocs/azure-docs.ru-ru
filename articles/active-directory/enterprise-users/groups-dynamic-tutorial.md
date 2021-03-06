---
title: Добавление пользователей в динамическую группу — учебник по Azure AD | Документация Майкрософт
description: В этом руководстве вы используете группы с правилами членства пользователя, чтобы автоматически добавлять или удалять пользователей
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.topic: tutorial
ms.date: 12/02/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d498fb4efb3de316981963ee9606255e5b1450a
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106056969"
---
# <a name="tutorial-add-or-remove-group-members-automatically"></a>Руководство по Автоматическое добавление или удаление участников группы

В Azure Active Directory (Azure AD) можно автоматически добавлять или удалять пользователей в группах безопасности или Microsoft 365, поэтому не обязательно всегда делать это вручную. Всякий раз, когда какие-либо свойства пользователя или устройства меняются, Azure AD оценивает все динамические групповые правила в организации Azure AD, чтобы узнать, следует ли добавлять или удалять участников.

В этом руководстве описано следующее:
> [!div class="checklist"]
> * Создание автоматически заполняемых групп гостевых пользователей из партнерской компании.
> * Присвоение группе лицензий для партнерских функций, чтобы обеспечить к ним доступ гостевых пользователей.
> * (Дополнительно.) Защита группы **Все пользователи** путем удаления гостевых пользователей, чтобы, например, предоставить своим пользователям доступ только ко внутренним сайтам.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Глобальному администратору организации для этой функции требуется одна лицензия Microsoft Azure Active Directory Premium. Если у вас ее нет, в Azure AD выберите **Лицензии** > **Продукты** > **Try/Buy** (Попробовать или купить).

Вы не обязаны назначать лицензии пользователям, чтобы они были членами динамических групп. Чтобы охватить всех пользователей, требуется только минимальное количество доступных лицензий Azure AD Premium P1 в организации. 

## <a name="create-a-group-of-guest-users"></a>Создание группы гостевых пользователей

Сначала вы создадите группу для гостевых пользователей, которые принадлежат одной партнерской компании. Они нуждаются в специальном лицензировании, поэтому для этого лучше создать группу.

1. Войдите на портал Azure (https://portal.azure.com) с помощью учетной записи глобального администратора организации.
2. Выберите **Azure Active Directory** > **Группы** > **New group** (Новая группа).
   ![Выбор команды для создания группы](./media/groups-dynamic-tutorial/new-group.png)
3. В колонке **Группа**:
  
   * Выберите **Безопасность** в качестве типа группы.
   * Укажите `Guest users Contoso` в качестве имени и введите описание группы.
   * Для параметра **Тип членства** укажите **Динамический пользователь**.
   
4. Щелкните **Владельцы** и в колонке **Добавление владельцев** выберите нужных владельцев. Щелкните нужных владельцев, чтобы добавить выделение.
5. Щелкните **Выбрать**, чтобы закрыть колонку **Добавление владельцев**.  
6. Щелкните **Изменить динамический запрос** в поле **Динамические пользователи-члены**.
7. В колонке **Правила динамического членства** сделайте следующее:

   * В поле **Свойство** щелкните существующее значение и выберите **userType**. 
   * Убедитесь, что в поле **Оператор** выбрано значение **Равно**.  
   * Щелкните поле **Значение** и введите **Гость**. 
   * Щелкните гиперссылку **Добавить выражение**, чтобы добавить другую строку.
   * В поле **И/или** выберите **И**.
   * В поле **Свойство** выберите **companyName**.
   * Убедитесь, что в поле **Оператор** выбрано значение **Равно**.
   * В поле **Значение** введите **Contoso**.
   * Щелкните **Сохранить**, чтобы закрыть колонку **Правила динамического членства**.
   
8. Щелкните **Создать** в колонке **Группа**, чтобы создать группу.

## <a name="assign-licenses"></a>Назначение лицензий

Теперь, когда у вас есть новая группа, можно применить лицензии, которые требуются этим пользователям-партнерам.

1. В Azure AD выберите **Лицензии**, выберите одну или несколько лицензий, а затем щелкните **Присвоить**.
2. Выберите **Пользователи и группы**, а затем щелкните группу **Guest users Contoso** (Гостевые пользователи Contoso) и сохраните изменения.
3. **Параметры назначения** позволяют включать и выключать планы обслуживания, содержащие выбранные лицензии. При внесении изменений необходимо нажать кнопку **ОК**, чтобы сохранить их.
4. Чтобы завершить назначение, в нижней части области **Назначение лицензии** щелкните **Назначить**.

## <a name="remove-guests-from-all-users-group"></a>Удаление гостей из группы "Все пользователи"

Возможно, ваш окончательный административный план состоит в том, чтобы назначить всех гостевых пользователей в их группы по компании. Кроме того, теперь можно изменить группу **Все пользователи**, чтобы она была зарезервирована только для пользователей-участников в организации. Затем ее можно использовать для присвоения приложений и лицензий, которые относятся к домашней организации.

   ![Изменение группы "Все пользователи" на группу "Только для членов"](./media/groups-dynamic-tutorial/all-users-edit.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

**Удаление группы гостевых пользователей**

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетной записи глобального администратора организации.
2. Выберите **Azure Active Directory** > **Группы**. Выберите группу **Guest users Contoso** (Гостевые пользователи Contoso), нажмите кнопку с многоточием (...), а затем выберите **Удалить**. При удалении группы удаляются все присвоенные лицензии.

**Восстановление группы "Все пользователи"**
1. Выберите **Azure Active Directory** > **Группы**. Выберите имя группы **Все пользователи**, чтобы открыть ее.
1. Выберите **Правила динамического членства**, очистите весь текст в правиле и нажмите кнопку **Сохранить**.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как выполнять следующие задачи:
> [!div class="checklist"]
> * Создание группы гостевых пользователей
> * Присвоение лицензий новой группе
> * Изменение группы "Все пользователи" на группу "Только для членов"

Перейдите к следующей статье, чтобы получить основные сведения о групповом лицензировании.
> [!div class="nextstepaction"]
> [Основы группового лицензирования в Azure Active Directory](../fundamentals/active-directory-licensing-whatis-azure-portal.md)



