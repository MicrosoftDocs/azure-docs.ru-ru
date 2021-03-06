---
title: Использование профилей Wi-Fi с мини-устройствами на Azure Stack граничных устройств R
description: В этой статье описывается создание профилей Wi-Fi для Azure Stackных мини-устройств R на основе высокозащищенных корпоративных сетей и персональных сетей.
services: databox
author: v-dalc@microsoft.com
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/24/2021
ms.author: alkohli
ms.openlocfilehash: 90c7c238cef104eae78618e51fa4b284adcc8f42
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2021
ms.locfileid: "105050549"
---
# <a name="use-wi-fi-profiles-with-azure-stack-edge-mini-r-devices"></a>Использование профилей Wi-Fi с мини-устройствами на Azure Stack граничных устройств R

В этой статье описывается, как использовать профили беспроводной сети (Wi-Fi) с устройствами на Azure Stack пограничных устройствах R.

Процедура подготовки профиля Wi-Fi зависит от типа беспроводной сети.

- В частной сети Wi-Fi с защитой WPA2, такой как домашняя сеть или Wi-Fi открытая хот-спот, вы можете скачать и использовать существующий профиль беспроводной связи с тем же паролем, который используется с другими устройствами.

- В корпоративной среде с высоким уровнем безопасности вы получаете доступ к устройству через сеть WPA2-Enterprise. В сети этого типа каждый клиентский компьютер будет иметь отдельный профиль Wi-Fi и будет проходить проверку подлинности с помощью сертификатов. Для определения требуемой конфигурации необходимо обратиться к администратору сети.

Требования к профилю для обоих типов сети будут обсуждаться позже.

В любом случае перед тестированием или использованием профиля с устройством очень важно убедиться, что профиль соответствует требованиям безопасности Организации.

## <a name="about-wi-fi-profiles"></a>О Wi-Fi профилях

Профиль Wi-Fi содержит SSID (идентификатор набора служб или **Сетевое имя**), ключ пароля и сведения о безопасности, необходимые для подключения Мини-устройства R Azure Stack ребра к беспроводной сети.

В следующем примере кода показаны основные параметры профиля для использования с обычной беспроводной сетью.

* `SSID` — Это сетевое имя.

* `name` Понятное имя для Wi-Fi соединения. Это имя, которое пользователи увидят при просмотре доступных подключений на устройстве.

* Профиль настроен для автоматического подключения компьютера к беспроводной сети, если он находится в диапазоне сети ( `connectionMode`  =  `auto` ).

```html
<?xml version="1.0"?>
<WLANProfile xmlns="http://www.contoso.com/networking/WLAN/profile/v1">
    <name>ContosoWIFICORP</name>
    <SSIDConfig>
        <SSID>
            <hex>1A234561234B5012</hex>
        </SSID>
        <nonBroadcast>false</nonBroadcast>
    </SSIDConfig>
    <connectionType>ESS</connectionType>
    <connectionMode>auto</connectionMode>
    <autoSwitch>false</autoSwitch>
```

Дополнительные сведения о параметрах профиля Wi-Fi см. в статье **профиль предприятия** в разделе [Добавление параметров Wi-Fi для устройств Windows 10 и более новых версий](/mem/intune/configuration/wi-fi-settings-windows#enterprise-profile). см. раздел [Настройка профиля Cisco Wi-Fi](azure-stack-edge-mini-r-manage-wifi.md#configure-cisco-wi-fi-profile).

Чтобы включить беспроводные подключения на Azure Stack пограничной устройстве R, настройте порт Wi-Fi на устройстве, а затем добавьте Wi-Fi профили на устройство. В корпоративной сети вы также отправляете сертификаты на устройство. Затем можно подключиться к беспроводной сети из локального веб-интерфейса устройства. Дополнительные сведения см. [в статье Управление беспроводными подключениями на Azure Stack пограничном мини-приложении R](./azure-stack-edge-mini-r-manage-wifi.md).

## <a name="profile-for-wpa2---personal-network"></a>Профиль для сети WPA2-Personal

В частной сети Wi-Fi с защитой WPA2, например в домашней сети или Wi-Fi открытой хот-споте, несколько устройств могут использовать один и тот же профиль и один и тот же пароль. В домашней сети мобильный телефон и ноутбук используют один и тот же профиль и пароль беспроводной сети для подключения к сети.

Например, клиент Windows 10 может создать профиль среды выполнения. При входе в беспроводную сеть вам будет предложено ввести пароль Wi-Fi и, после того, как пароль будет введен, вы будете подключены. В этой среде сертификаты не требуются.

В сети этого типа вы можете экспортировать профиль Wi-Fi с переносного компьютера, а затем добавить его на устройство Mini R на Azure Stack. Инструкции см. [в разделе Экспорт профиля Wi-Fi](#export-a-wi-fi-profile)ниже.

> [!IMPORTANT]
> Прежде чем создавать профиль Wi-Fi для мини-устройства с Azure Stackным пограничным устройством R, обратитесь к администратору сети, чтобы узнать о требованиях безопасности Организации к беспроводным сетям. Не следует тестировать или использовать какой-либо профиль Wi-Fi на устройстве, пока не будет известно, что беспроводная сеть соответствует требованиям.

## <a name="profiles-for-wpa2---enterprise-network"></a>Профили для сети WPA2-Enterprise

В корпоративной сети с защитой WPA2 необходимо обратиться к администратору сети, чтобы получить необходимый профиль Wi-Fi и сертификат для подключения к сети Azure Stack Мини-устройства R.

Для высокозащищенных сетей устройство Azure может использовать защищенный протокол PEAP с расширенной проверкой подлинности Protocol-Transport уровня безопасности (EAP-TLS). PEAP с EAP-TLS использует проверку подлинности компьютера. Клиент и сервер используют сертификаты для проверки их удостоверений.

> [!NOTE]
> * Проверка подлинности пользователей с помощью протокола PEAP (протокол проверки подлинности Майкрософт) версии 2 (PEAP MSCHAPv2) не поддерживается на Azure Stack пограничных устройствах R.
> * Проверка подлинности EAP-TLS необходима для доступа к Azure Stack пограничных функциональных возможностей R. Беспроводное подключение, настроенное с помощью Active Directory, не будет работать.

Сетевой администратор создаст уникальный профиль Wi-Fi и сертификат клиента для каждого компьютера. Сетевой администратор решает, следует ли использовать отдельный сертификат для каждого устройства или общего сертификата.

Если на рабочем месте работает несколько физических расположений, администратору сети может потребоваться предоставить более одного профиля Wi-Fi для конкретного сайта и сертификат для беспроводных подключений.

В корпоративной сети не рекомендуется изменять параметры профилей Wi-Fi, предоставляемых администратором сети. Единственная корректировка, которую вы можете захотеть сделать, — это параметры автоматического подключения. Дополнительные сведения см. в разделе [базовый профиль](/mem/intune/configuration/wi-fi-settings-windows#basic-profile) в параметрах Wi-Fi для устройств Windows 10 и более новых версий.

В корпоративной среде с высоким уровнем безопасности вы можете использовать существующий профиль беспроводной сети в качестве шаблона:

* Вы можете скачать профиль беспроводной сети Организации с рабочего компьютера. Инструкции см. [в разделе Экспорт профиля Wi-Fi](#export-a-wi-fi-profile)ниже.

* Если другие пользователи в вашей организации уже подключаются к своим Azure Stackным мини-устройствам с пограничными устройствами R по беспроводной сети, они могут загрузить профиль Wi-Fi с устройства. Инструкции см. в статье [скачивание Wi-Fi профиля](azure-stack-edge-mini-r-manage-wifi.md#download-wi-fi-profile).

## <a name="export-a-wi-fi-profile"></a>Экспорт профиля Wi-Fi

Чтобы экспортировать профиль для интерфейса Wi-Fi на компьютере, выполните следующие действия.

1. Чтобы просмотреть профили беспроводной связи на компьютере, в меню " **Пуск** " Откройте **командную строку** (cmd.exe) и введите следующую команду:

   `netsh wlan show profiles`

   Результат должен выглядеть следующим образом.

   ```dos
   Profiles on interface Wi-Fi:

   Group policy profiles (read only)
   ---------------------------------
       <None>

   User profiles
   -------------
       All User Profile     : ContosoCORP
       All User Profile     : ContosoFTINET
       All User Profile     : GusIS2809
       All User Profile     : GusGuests
       All User Profile     : SeaTacGUEST
       All User Profile     : Boat
   ```

2. Чтобы экспортировать профиль, введите следующую команду:

   `netsh wlan export profile name=”<profileName>” folder=”<path>\<profileName>"`

   Например, следующая команда сохраняет профиль Контософтинет в формате XML в папку downloads для пользователя с именем `gusp` .

   ```dos
   C:\Users\gusp>netsh wlan export profile name="ContosoFTINET" folder=c:Downloads

   Interface profile "ContosoFTINET" is saved in file "c:Downloads\ContosoFTINET.xml" successfully.
   ```

## <a name="add-certificate-wi-fi-profile-to-device"></a>Добавление сертификата, Wi-Fi профиля на устройство

Если у вас есть необходимые профили Wi-Fi и сертификаты, выполните следующие действия, чтобы настроить для беспроводных подключений устройство Mini R Azure Stack.

1. Для сети WPA2-Enterprise передайте необходимые сертификаты на устройство, следуя указаниям в статье [Отправка сертификатов](./azure-stack-edge-gpu-manage-certificates.md#upload-certificates).

1. Передайте Wi-Fi профили на мини-устройство R, а затем подключитесь к нему, следуя указаниям в статье [Добавление, подключение к профилю Wi-Fi](./azure-stack-edge-mini-r-manage-wifi.md#add-connect-to-wi-fi-profile).

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как [настроить сеть для Azure Stack пограничных Мини-R](azure-stack-edge-mini-r-deploy-configure-network-compute-web-proxy.md).
- Узнайте, как [управлять Wi-Fi на Azure Stack пограничных Мини-R](azure-stack-edge-mini-r-manage-wifi.md).
