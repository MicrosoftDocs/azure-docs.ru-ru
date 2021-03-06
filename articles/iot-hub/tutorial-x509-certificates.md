---
title: Руководство. Общие сведения о сертификатах X.509 с открытым ключом для Центра Интернета вещей Azure | Документация Майкрософт
description: Руководство. Общие сведения о сертификатах X.509 с открытым ключом для Центра Интернета вещей Azure
author: v-gpettibone
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 02/26/2021
ms.author: robinsh
ms.custom:
- mvc
- 'Role: Cloud Development'
- 'Role: Data Analytics'
ms.openlocfilehash: 5503f9ad57180146c25a01c133a27b34e643496c
ms.sourcegitcommit: 2654d8d7490720a05e5304bc9a7c2b41eb4ae007
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "107378352"
---
# <a name="tutorial-understanding-x509-public-key-certificates"></a>Руководство. Общие сведения о сертификатах X.509 с открытым ключом

Сертификаты X.509 — это цифровые документы, которые представляют пользователя, компьютер, службу или устройство. Они могут выдаваться корневым или подчиненным центром сертификации (ЦС) либо центром регистрации и содержат открытый ключ субъекта сертификата. Они не содержат закрытый ключ субъекта, который должен храниться безопасно. Сертификаты с открытым ключом определяются в документе [RFC 5280](https://tools.ietf.org/html/rfc5280). Они имеют цифровую подпись и обычно содержат следующую информацию:

* сведения о субъекте сертификата;
* открытый ключ, который соответствует открытому ключу субъекта сертификата;
* сведения о ЦС, выдавшем сертификат;
* поддерживаемые алгоритмы шифрования и (или) цифровой подписи;
* сведения для определения состояния отзыва или допустимости сертификата.

## <a name="certificate-fields"></a>Поля сертификата

Со временем появились три версии сертификата. В каждой из версий добавлялись новые поля. Версия 3, которая действует в настоящий момент, содержит все поля версий 1 и 2, дополненные полями версии 3. Версия 1 определяет следующие поля:

* **Version** (Версия) — значение 1, 2 или 3, которое указывает используемую версию сертификата.
* **Serial Number** (Серийный номер) — уникальный идентификатор, который присваивается каждому сертификату ЦС.
* **CA Signature Algorithm** (Алгоритм подписывания ЦС) — имя алгоритма, который ЦС использует для подписывания содержимого сертификата.
* **Issuer Name** (Имя издателя) — различающееся имя ЦС, который выдал этот сертификат.
* **Validity Period** (Период действия) — период времени, в течение которого сертификат считается действительным.
* **Subject Name** (Имя субъекта) — имя сущности, которую представляет сертификат.
* **Subject Public Key Info** (Данные открытого ключа субъекта) — открытый ключ, принадлежащий субъекту сертификата.

В версии 2 добавлены следующие поля с информацией об издателе сертификата, хотя эти поля редко используются:

* **Issuer Unique ID** (Уникальный идентификатор издателя): уникальный идентификатор центра сертификации, который выдал сертификат. Он определяется самим центром сертификации.
* **Subject Unique ID** (Уникальный идентификатор субъекта) — уникальный идентификатор субъекта сертификата, который определяется ЦС, выдавшим сертификат.

В сертификатах версии 3 добавлены следующие расширения:

* **Authority Key Identifier** (Идентификатор ключа центра) — может содержать одно из двух значений:
  * субъект ЦС и серийный номер сертификата ЦС, который выдал сертификат;
  * хэш-код открытого ключа ЦС, который выдал этот сертификат.
* **Subject Key Identifier** (Идентификатор ключа субъекта) — хэш-код открытого ключа сертификата.
* **Key Usage** (Использование ключа) — определяет службу, для которой можно использовать сертификат. Здесь могут содержаться одно или несколько значений из следующего списка:
  * **Цифровая подпись**
  * **Неотрекаемость**
  * **Шифрование ключа**
  * **Data Encipherment** (Шифрование данных);
  * **Key Agreement** (Согласование ключей);
  * **Key Cert Sign** (Подпись сертификата с ключом);
  * **CRL Sign** (Подпись списка отзыва сертификатов);
  * **Encipher Only** (Только шифрование);
  * **Decipher Only** (Только расшифровка).
* **Private Key Usage Period** (Период использования закрытого ключа) — период действия закрытого ключа из пары ключей.
* **Certificate Policies** (Политики сертификатов) — политики, используемые для проверки субъекта сертификата.
* **Policy Mappings** (Сопоставления политик) — сопоставляет политику в одной организации с политикой в другой.
* **Subject Alternative Name** (Альтернативное имя субъекта) — список альтернативных имен для субъекта.
* **Issuer Alternative Name** (Альтернативное имя издателя) — список альтернативных имен для ЦС, выдавшего сертификат.
* **Subject Dir Attribute** (Атрибут DIR субъекта) — атрибуты из каталога X.500 или LDAP.
* **Basic Constraints** (Базовые ограничения) — позволяет определить в сертификате, кому он выдан: ЦС, пользователю, компьютеру, устройству или службе. Это расширение также включает ограничение длины пути, которое ограничивает допустимое количество подчиненных ЦС в цепочке.
* **Name Constraints** (Ограничения имен) — определяет, какие пространства имен разрешены в сертификате, выданном ЦС.
* **Policy Constraints** (Ограничения политики) — позволяет запретить сопоставления политик между ЦС.
* **Extended Key Usage** (Расширенное использование ключа) — указывает, как можно использовать открытый ключ сертификата, кроме целей, определяемых расширением **Key Usage** (Использование ключа).
* **CRL Distribution Points** (Точки распространения списков отзыва сертификатов) — содержит один или несколько URL-адресов, в которых публикуется базовый список отзыва сертификатов.
* **Inhibit anyPolicy** (Запрет любой политики) — запрещает использование значения **All Issuance Policies** (Все политики выдачи) с идентификатором OID (2.5.29.32.0) в сертификатах подчиненного ЦС.
* **Freshest CRL** (Актуальный список отзыва сертификатов) — содержит один или несколько URL-адресов, в которых публикуется разностный список отзыва сертификатов ЦС, выдавшего этот сертификат.
* **Authority Information Access** (Доступ к информации центра) — содержит один или несколько URL-адресов, в которых публикуется сертификат ЦС, выдавшего этот сертификат.
* **Subject Information Access** (Доступ к сведениям о субъекте) — содержит сведения о том, как можно получить дополнительные сведения о субъекте сертификата.

## <a name="certificate-formats"></a>Форматы сертификатов

Сертификаты можно сохранить в нескольких форматах. Для проверки подлинности в Центре Интернета вещей Azure обычно используются форматы PEM и PFX.

### <a name="binary-certificate"></a>Двоичный сертификат

Содержит двоичный сертификат в необработанном формате в кодировке DER ASN.1.

### <a name="ascii-pem-format"></a>Формат ASCII PEM

Сертификат PEM (файл с расширением .pem) содержит сертификат в кодировке Base64 с префиксом -----BEGIN CERTIFICATE----- и постфиксом -----END CERTIFICATE-----. Формат PEM очень распространен и является обязательным к использованию при отправке некоторых сертификатов в Центр Интернета вещей.

### <a name="ascii-pem-key"></a>Ключ ASCII (PEM)

Содержит ключ DER в кодировке Base64 с добавлением необязательных метаданных со сведениями об алгоритме, используемом для защиты пароля.

### <a name="pkcs7-certificate"></a>Сертификат PKCS#7

Этот формат предназначен для передачи подписанных или зашифрованных данных. Он определяется стандартом [RFC 2315](https://tools.ietf.org/html/rfc2315). В нем может содержаться полная цепочка сертификатов.

### <a name="pkcs8-key"></a>Ключ PKCS#8

Этот формат предназначен для хранения закрытого ключа и определен в стандарте [RFC 5208](https://tools.ietf.org/html/rfc5208).

### <a name="pkcs12-key-and-certificate"></a>Ключ и сертификат PKCS#12

Сложный формат, который используется для хранения и защиты ключа и полной цепочки сертификатов. Он обычно используется с расширением .pfx. PKCS#12 — это синоним формата PFX.

## <a name="for-more-information"></a>Дополнительные сведения

Дополнительные сведения см. в следующих разделах:

* [Руководство для новичков по терминологии, связанной с сертификатами X.509](https://techcommunity.microsoft.com/t5/internet-of-things/the-layman-s-guide-to-x-509-certificate-jargon/ba-p/2203540)
* [Концептуальное представление о сертификатах ЦС X.509 в отрасли Интернета вещей](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-concept)

## <a name="next-steps"></a>Дальнейшие действия

Если вы хотите создать тестовые сертификаты, которые можно использовать для проверки подлинности устройств в Центре Интернета вещей, изучите следующие разделы:

* [Использование скриптов, предоставляемых корпорацией Майкрософт для создания тестовых сертификатов](tutorial-x509-scripts.md)
* [Использование OpenSSL для создания тестовых сертификатов](tutorial-x509-openssl.md)
* [Использование OpenSSL для создания самозаверяющих тестовых сертификатов](tutorial-x509-self-sign.md)

Если у вас есть сертификат корневого или подчиненного центра сертификации (ЦС), который вы хотите отправить в центр Интернета вещей и подтвердить права владения, см. руководство [Подтверждение принадлежности сертификата ЦС](tutorial-x509-prove-possession.md).
