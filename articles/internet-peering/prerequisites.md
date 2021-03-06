---
title: Предварительные требования для настройки пиринга с Майкрософт
titleSuffix: Azure
description: Предварительные требования для настройки пиринга с Майкрософт
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: prmitiki
ms.openlocfilehash: bc45bc8809eabe75902b602835ea595b56ff3cf8
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "97586846"
---
# <a name="prerequisites-to-set-up-peering-with-microsoft"></a>Предварительные требования для настройки пиринга с Майкрософт

Перед запросом нового пиринга или преобразования устаревшего пиринга в ресурс Azure убедитесь, что выполнены следующие условия.

## <a name="azure-related-prerequisites"></a>Предварительные требования, связанные с Azure
* **Учетная запись Microsoft Azure:** Если у вас нет учетной записи Microsoft Azure, создайте [учетную запись Microsoft Azure](https://azure.microsoft.com/free). Для настройки пиринга требуется действительная и активная подписка Microsoft Azure, так как пиринг моделируется как ресурсы в подписках Azure. Важно отметить следующее.
    * Типы ресурсов Azure, используемые для настройки пиринга, — это всегда бесплатные продукты Azure, т. е. Вы не платите за создание учетной записи Azure или создаете подписку или обращаетесь к ресурсам Azure **PeerAsn** и **пиринг** для настройки пиринга. Это не следует путать с соглашением об использовании пиринга для прямого пиринга между вами и корпорацией Майкрософт. условия, которые явно обсуждаются в нашей группе пиринга. Обратитесь в службу [пиринга Майкрософт](mailto:peering@microsoft.com) , если какие – либо вопросы в этом отношении.
    * Вы можете использовать одну и ту же подписку Azure для доступа к другим продуктам или облачным службам Azure, которые могут быть бесплатными или платными. При обращении к платному продукту взимается плата.
    * Если вы создаете новую учетную запись Azure и (или) подписку, вы можете получить бесплатный кредит Azure в течение пробного периода, который вы можете использовать для ознакомления с облачными службами Azure. Если интересно, посетите [Microsoft Azure учетную запись](https://azure.microsoft.com/free) для получения дополнительных сведений.

* **Связать одноранговый ASN:** Прежде чем запросить пиринг, сначала свяжите сведения о ASN и контактном лице с подпиской. Следуйте инструкциям в статье [связывание однорангового ASN с подпиской Azure](howto-subscription-association-powershell.md).

## <a name="other-prerequisites"></a>Другие необходимые компоненты
* **Профиль пирингдб:** Предполагается, что узлы имеют полный и актуальный профиль на [пирингдб](https://www.peeringdb.com). Мы используем эти сведения в нашей системе регистрации для проверки сведений о одноранговом узле, таких как сведения о центре Интернета, технические контактные данные и их присутствие на узлах и т. д.

## <a name="next-steps"></a>Дальнейшие действия

* [Создание или изменение прямого пиринга с помощью портала](howto-direct-portal.md).
* [Преобразование устаревшего прямого пиринга в ресурс Azure с помощью портала](howto-legacy-direct-portal.md)
* [Создание или изменение пиринга через точку обмена с помощью портала](howto-exchange-portal.md)
* [Преобразование устаревшего пиринга через точку обмена в ресурс Azure с помощью портала](howto-legacy-exchange-portal.md)