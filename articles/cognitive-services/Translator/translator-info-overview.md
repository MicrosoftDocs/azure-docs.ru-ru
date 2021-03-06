---
title: Служба "Переводчик Майкрософт"
titlesuffix: Azure Cognitive Services
description: Интеграция службы Translator в приложения, веб-сайты, инструменты и другие решения обеспечивает многоязычное взаимодействие с пользователем.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.topic: overview
ms.subservice: translator-text
ms.date: 03/15/2021
ms.author: lajanuar
ms.custom: cog-serv-seo-aug-2020
keywords: Переводчик, перевод текста, машинный перевод, служба перевода
ms.openlocfilehash: ec76aa7554110b7440eb825f2d5e86ae2da6baa2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104657728"
---
# <a name="what-is-the-translator-service"></a>Что собой представляет служба "Переводчик"?

Переводчик — это облачная служба машинного перевода, которая входит в семейство когнитивных интерфейсов API [Azure Cognitive Services](../../index.yml?panel=ai&pivot=products) для создания интеллектуальных приложений. Служба Translator легко интегрируется в приложения, веб-сайты, инструменты и решения. Решение позволяет добавлять многоязычные пользовательские интерфейсы [более чем на 90 языках и диалектах](./language-support.md). Службу можно использовать для перевода текстов с любой операционной системой.

Эта документация включает статьи следующих типов:  

* [**Краткие руководства**](quickstart-translator.md) — инструкции по началу работы и отправке запросов в службу.  
* [**Руководства**](translator-how-to-signup.md) — содержат инструкции для более специфического или специализированного использования службы.  
* [**Статьи с основными понятиями**](character-counts.md) — здесь подробно описываются функциональность и возможности службы.  
* [**Руководства**](tutorial-wpf-translation-csharp.md) — объемные статьи, в которых описываются способы использования службы в качестве компонента расширенных бизнес-решений.  


## <a name="about-microsoft-translator"></a>О Microsoft Translator

Переводчик используется в многих продуктах и службах корпорации Майкрософт, а также в приложениях и рабочих процессах тысяч международных компаний. Благодаря этому целевое содержимое достигает своей аудитории во всем мире.

Кроме того, [служба Azure "Речь"](../speech-service/index.yml) позволяет переводить речь с использованием Переводчика. Служба "Речь" объединяет функциональные возможности API Перевода речи и Пользовательской службы "Речь" в единую и полностью настраиваемую службу. 

## <a name="language-support"></a>Поддержка языков

Переводчик предоставляет многоязычную поддержку для перевода текстов, транслитерации, распознавания языка и словарей. Чтобы увидеть полный список или получить доступ к нему программным образом с помощью [REST API](./reference/v3-0-languages.md), обратитесь к статье [Поддержка языков и регионов в API перевода текстов](language-support.md).  

## <a name="microsoft-translator-neural-machine-translation"></a>Нейронный машинный перевод в Microsoft Translator

Нейронный машинный перевод (NMT) — это новый стандарт высококачественных машинных переводов на базе ИИ. Он заменяет устаревшую технологию статистического машинного перевода (SMT), которая достигла вершины качества в середине 2010-х годов.

NMT обеспечивает лучшие переводы, чем SMT, не только с точки зрения оценки качества перевода, но и потому, что они будут звучать более свободно и естественно. Секрет такого улучшения кроется в том, что технология NMT использует для перевода слов контекст всего предложения. SMT принимал непосредственный контекст нескольких слов до и после каждого слова.

Модели NMT лежат в основе API и не видны пользователям. Единственным заметным отличием является улучшение качества перевода, особенно для таких языков, как китайский, японский и арабский.

См. дополнительные сведения о [принципах работы NMT](https://www.microsoft.com/en-us/translator/mt.aspx#nnt).

## <a name="improve-translations-with-custom-translator"></a>Улучшение переводов с помощью Пользовательского переводчика

 [Пользовательский переводчик](customization.md) — это расширение службы "Переводчик". Оно позволяет настроить систему нейронного перевода и улучшить перевод с учетом конкретной терминологии и стиля.

С помощью Пользовательского переводчика можно создавать системы перевода, которые распознают особую терминологию вашего бизнеса и отрасли. Вашу настраиваемую систему перевода можно легко интегрировать в существующие приложения, рабочие процессы и веб-сайты на нескольких типах устройств посредством обычного Переводчика, используя параметр категории.

## <a name="next-steps"></a>Дальнейшие действия

- [Создайте службу "Переводчик"](./translator-how-to-signup.md), чтобы получить ключи доступа и конечной точке.
- Воспользуйтесь сведениями из нашего [краткого руководства](quickstart-translator.md), чтобы быстро вызвать службу "Переводчик".
- Ознакомьтесь со [справочной документацией по API](./reference/v3-0-reference.md).
- [Сведения о тарифах](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
