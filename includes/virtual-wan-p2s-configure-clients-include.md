---
title: включить файл
description: включить файл
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9caf63fc90be7bae0461ddc24c94594a32199765
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "91812710"
---
#### <a name="microsoft-windows"></a>Microsoft Windows

##### <a name="openvpn"></a>OpenVPN

1. С официального веб-сайта загрузите и установите клиент OpenVPN.
1. Загрузите профиль VPN для шлюза. Это можно сделать с вкладки конфигурации VPN пользователя на портале Azure, или с помощью New-AzureRmVpnClientConfiguration в PowerShell.
1. Распакуйте профиль. Откройте файл конфигурации vpnconfig.ovpn из папки OpenVPN в Notepad.
1. Заполните раздел сертификата клиента подключения "точка — сеть" открытым ключом сертификата клиента P2S в формате base64. В сертификате с форматированием PEM вы можете открыть файл CER и скопировать ключ в формате base64, находящийся между заголовками сертификата. Инструкции см. в статье о том, [как экспортировать сертификат, чтобы получить закодированный открытый ключ](../articles/virtual-wan/certificates-point-to-site.md).
1. Заполните раздел секретного ключа закрытым ключом сертификата клиента P2S в base64. Инструкции см. в статье о том, [как извлечь закрытый ключ](../articles/virtual-wan/howto-openvpn-clients.md#windows).
1. Не изменяйте остальные поля. Для подключения к VPN используйте заполненную конфигурацию на входе клиента.
1. Скопируйте файл vpnconfig.ovpn в папку C:\Program Files\OpenVPN\config.
1. Щелкните правой кнопкой мыши значок OpenVPN в области уведомлений и выберите **Подключить**.

##### <a name="ikev2"></a>IKEv2

1. Выберите файлы конфигурации VPN-клиента, которые соответствуют архитектуре компьютера Windows. Для 64-разрядной архитектуры процессора выберите пакет установщика VpnClientSetupAmd64. Для 32-разрядной архитектуры процессора выберите пакет установщика VpnClientSetupX86.
1. Дважды щелкните пакет, чтобы установить его. При появлении всплывающего окна SmartScreen щелкните **Подробнее**, а затем **Выполнить в любом случае**.
1. На клиентском компьютере перейдите в раздел **Параметры сети** и щелкните **VPN**. Для VPN-подключения отображается имя виртуальной сети, к которой оно устанавливается.
1. Перед попыткой подключения убедитесь, что вы установили сертификат клиента на клиентском компьютере. Сертификат клиента требуется при использовании собственной аутентификации Azure на основе сертификата. Дополнительную информацию о создании сертификатов см. в разделе [Настройка подключения "точка — сеть" к виртуальной сети с использованием собственной аутентификации Azure на основе сертификата и портала Azure](../articles/virtual-wan/certificates-point-to-site.md). Инструкции по установке сертификата клиента см. в разделе [Установка сертификата клиента для аутентификации Azure на основе сертификата при подключениях типа "точка — сеть"](../articles/vpn-gateway/point-to-site-how-to-vpn-client-install-azure-cert.md).
