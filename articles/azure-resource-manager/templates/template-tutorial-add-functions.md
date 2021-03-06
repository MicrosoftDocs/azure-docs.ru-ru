---
title: Учебник по добавлению функций шаблона
description: Сведения о том, как добавить функции шаблона в шаблон Azure Resource Manager для создания значений.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: 52b5bd0650b3a069adc3ef7f101c48a4674deaab
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "97107113"
---
# <a name="tutorial-add-template-functions-to-your-arm-template"></a>Руководство по добавлению функций шаблона в шаблон ARM

Из этого учебника вы узнаете, как добавить [функции шаблона](template-functions.md) в шаблон Azure Resource Manager (ARM). Функции используются для динамического создания значений. Помимо этих системных функций шаблонов можно также создать [функции, определяемые пользователем](./template-user-defined-functions.md). Для выполнения инструкций из этого учебника требуется **7 минут**.

## <a name="prerequisites"></a>Предварительные требования

Советуем выполнить инструкции из [руководства о параметрах](template-tutorial-add-parameters.md), но это необязательно.

Вам понадобится Visual Studio Code с расширением инструментов Resource Manager и либо Azure PowerShell, либо Azure CLI. Дополнительные сведения см. в разделе об [инструментах шаблона](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Проверка шаблона

В конце предыдущего учебника шаблон содержал следующий код JSON:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-sku/azuredeploy.json":::

Расположение учетной записи хранения жестко задано к значению **Восточная часть США**. Однако может потребоваться развернуть учетную запись хранения в других регионах. У вас вновь возникают проблемы с отсутствием гибкости в шаблоне. Вы можете добавить параметр для расположения, но было бы замечательно, если бы его значение по умолчанию имело больше смысла, чем просто жестко заданное значение.

## <a name="use-function"></a>Использование функции

Если вы выполнили инструкции из предыдущего учебника в этой серии, вы уже использовали функцию. При добавлении `"[parameters('storageName')]"` использовалась функция [parameters](template-functions-deployment.md#parameters). Квадратные скобки указывают на то, что синтаксис внутри них — это [выражение шаблона](template-expressions.md). Resource Manager разрешает синтаксис и не рассматривает его как литеральное значение.

Функции позволяют адаптировать шаблон за счет динамического получения значения во время развертывания. В рамках этого учебника вы используете функцию для получения расположения группы ресурсов, используемой для развертывания.

В следующем примере показаны изменения для добавления параметра `location`. Значение параметра по умолчанию вызывает функцию [resourceGroup](template-functions-resource.md#resourcegroup). Эта функция возвращает объект со сведениями о группе ресурсов, используемой для развертывания. Одним из свойств объекта является свойство расположения. При использовании значения по умолчанию расположение учетной записи хранения совпадает с расположением группы ресурсов. Ресурсы внутри группы ресурсов не должны совместно использовать одно и то же расположение. При необходимости можно также указать другое расположение.

Скопируйте весь файл и замените шаблон на его содержимое.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-location/azuredeploy.json" range="1-44" highlight="24-27,34":::

## <a name="deploy-template"></a>Развертывание шаблона

В рамках предыдущих учебников вы создали учетную запись хранения в восточной части США, но ваша группа ресурсов была создана в центральной части США. В этом учебнике учетная запись хранения создается в том же регионе, что и группа ресурсов. Используйте значение по умолчанию для расположения, чтобы не указывать значение этого параметра. Необходимо указать новое имя для учетной записи хранения, так как вы создаете ее в другом расположении. Например, используйте **store2** в качестве префикса вместо **store1**.

Если вы еще не создали группу ресурсов, см. [этот раздел](template-tutorial-create-first-template.md#create-resource-group). В этом примере предполагается, что для переменной `templateFile` указан путь к файлу шаблона, как показано в [первом учебнике](template-tutorial-create-first-template.md#deploy-template).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addlocationparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storageName "{new-unique-name}"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Чтобы выполнить эту команду развертывания, необходимо иметь [последнюю версию](/cli/azure/install-azure-cli) Azure CLI.

```azurecli
az deployment group create \
  --name addlocationparameter \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storageName={new-unique-name}
```

---

> [!NOTE]
> Если развертывание завершилось сбоем, используйте параметр `verbose`, чтобы получить сведения о создаваемых ресурсах. Используйте параметр `debug`, чтобы получить дополнительные сведения для отладки.

## <a name="verify-deployment"></a>Проверка развертывания

Чтобы проверить развертывание, просмотрите группу ресурсов на портале Azure.

1. Войдите на [портал Azure](https://portal.azure.com).
1. В меню слева выберите **Группы ресурсов**.
1. Выберите группу ресурсов, в которую выполнено развертывание.
1. Вы увидите, что ресурс учетной записи хранения развернут и имеет то же расположение, что и группа ресурсов.

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы переходите к следующему учебнику, вам не нужно удалять группу ресурсов.

Если вы останавливаете работу сейчас, вам может потребоваться очистить развернутые ресурсы, удалив группу ресурсов.

1. На портале Azure в меню слева выберите **Группа ресурсов**.
2. В поле **Фильтровать по имени** введите имя группы ресурсов.
3. Выберите имя группы ресурсов.
4. В главном меню выберите **Удалить группу ресурсов**.

## <a name="next-steps"></a>Дальнейшие действия

В этом учебнике вы использовали функцию при определении значения по умолчанию для параметра. В этой серии учебников вы продолжите использовать функции. К концу серии вы добавите функции в каждый раздел шаблона.

> [!div class="nextstepaction"]
> [Учебник по добавлению переменных в шаблон Azure Resource Manager](template-tutorial-add-variables.md)
