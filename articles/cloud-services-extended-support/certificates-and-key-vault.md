---
title: Хранение и использование сертификатов в Облачных службах Azure (расширенная поддержка)
description: Процессы хранения и использования сертификатов в облачных службах Azure (Расширенная поддержка)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 4d771e77fcca05b090e5d47d70ae93ece8f79e3e
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104865709"
---
# <a name="use-certificates-with-azure-cloud-services-extended-support"></a>Использование сертификатов с облачными службами Azure (Расширенная поддержка)

Хранилище ключей используется для хранения сертификатов, связанных с Облачными службами (расширенная поддержка). Хранилища ключей можно создавать с помощью [портал Azure](../key-vault/general/quick-create-portal.md) и [PowerShell](../key-vault/general/quick-create-powershell.md). Добавьте сертификаты в Key Vault, а затем сослаться на отпечатки сертификата в файле конфигурации службы. Также для хранилища ключей необходимо включить соответствующие разрешения, позволяющие ресурсу Облачных служб (расширенная поддержка) получить из хранилища ключей сертификат, хранимый в виде секрета.  

## <a name="upload-a-certificate-to-key-vault"></a>Отправка сертификата в Key Vault 

1.  Войдите в портал Azure и перейдите к Key Vault. Если вы не настроили Key Vault, вы можете создать его в этом же окне.

2. Выбор **политик доступа**

    :::image type="content" source="media/certs-and-key-vault-1.png" alt-text="На рисунке показано, как выбрать политики доступа в колонке хранилища ключей.":::

3. Убедитесь, что политики доступа включают следующее свойство:
    - **Разрешение доступа к виртуальным машинам Azure для развертывания**

    :::image type="content" source="media/certs-and-key-vault-2.png" alt-text="Изображение отображает окно политики доступа в портал Azure.":::
 
4.  Выбор **сертификатов** 

    :::image type="content" source="media/certs-and-key-vault-3.png" alt-text="На рисунке показано, как выбрать параметр сертификаты в окне &quot;политики хранилища ключей&quot; в портал Azure.":::

5. Выбор **создания/импорта**

    :::image type="content" source="media/certs-and-key-vault-4.png" alt-text="На рисунке показано, как выбрать параметр &quot;создать/импортировать&quot;":::

4.  Заполните необходимые сведения, чтобы завершить отправку сертификата. Сертификат должен быть в **. Формат PFX** .

    :::image type="content" source="media/certs-and-key-vault-5.png" alt-text="На рисунке показано окно импорта в портал Azure.":::

5.  Добавьте сведения о сертификате в роль в файле конфигурации службы (CSCFG-файл). Убедитесь, что отпечаток сертификата в портал Azure соответствует отпечатку в файле конфигурации службы (cscfg). 
    
    ```json
    <Certificate name="<your cert name>" thumbprint="<thumbprint in key vault" thumbprintAlgorithm="sha1" /> 
    ```
6.  Для развертывания с помощью шаблона ARM certificateUrl можно найти, перейдя к сертификату в хранилище ключей, помеченному как идентификатор секрета.

    :::image type="content" source="media/certs-and-key-vault-6.png" alt-text="На рисунке показано поле идентификатора секрета в хранилище ключей.":::

## <a name="next-steps"></a>Дальнейшие действия 
- Ознакомьтесь с [предварительными требованиями для развертывания](deploy-prerequisite.md) облачных служб (Расширенная поддержка).
- Ознакомьтесь с [часто задаваемыми вопросами об Облачных службах (расширенная поддержка)](faq.md).
- Разверните Облачную службу (расширенная поддержка) с помощью [портала Azure](deploy-portal.md), [PowerShell](deploy-powershell.md), [шаблона](deploy-template.md) или [Visual Studio](deploy-visual-studio.md).
