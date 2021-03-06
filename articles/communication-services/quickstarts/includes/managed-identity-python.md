---
ms.openlocfilehash: cefdf77052e559853cc85d129799e288032186b8
ms.sourcegitcommit: c8b50a8aa8d9596ee3d4f3905bde94c984fc8aa2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2021
ms.locfileid: "105645425"
---
## <a name="add-managed-identity-to-your-communication-services-solution"></a>Добавление управляемого удостоверения в решение служб связи

### <a name="install-the-sdk-packages"></a>Установка пакетов SDK

```console
pip install azure-identity
pip install azure-communication-identity
pip install azure-communication-sms
```

### <a name="use-the-sdk-packages"></a>Использование пакетов SDK

Добавьте следующее `import` в код, чтобы использовать удостоверение Azure.

```python
from azure.identity import DefaultAzureCredential
```

В приведенных ниже примерах используется [дефаултазурекредентиал](/python/api/azure-identity/azure.identity.defaultazurecredential). Эти учетные данные подходят для сред рабочей среды и разработки.

Сведения о регистрации приложения в среде разработки и настройке переменных среды см. в разделе [авторизация доступа с помощью управляемого удостоверения](../managed-identity-from-cli.md) .

### <a name="create-an-identity-and-issue-a-token"></a>Создание удостоверения и выдача маркера

В следующем примере кода показано, как создать объект клиента службы с управляемым удостоверением, а затем использовать клиент для выдаче маркера для нового пользователя:

```python
from azure.communication.identity import CommunicationIdentityClient 

def create_identity_and_get_token(resource_endpoint):
     credential = DefaultAzureCredential()
     client = CommunicationIdentityClient(resource_endpoint, credential)

     user = client.create_user()
     token_response = client.get_token(user, scopes=["voip"])
     
     return token_response
```

### <a name="send-an-sms-with-azure-managed-identity"></a>Отправка SMS с управляемым удостоверением Azure

В следующем примере кода показано, как создать объект клиента службы с управляемым удостоверением Azure, а затем использовать клиент для отправки SMS-сообщения:

```python
from azure.communication.sms import SmsClient

def send_sms(resource_endpoint, from_phone_number, to_phone_number, message_content):
     credential = DefaultAzureCredential()
     sms_client = SmsClient(resource_endpoint, credential)

     sms_client.send(
          from_=from_phone_number,
          to_=[to_phone_number],
          message=message_content,
          enable_delivery_report=True  # optional property
     )
```
