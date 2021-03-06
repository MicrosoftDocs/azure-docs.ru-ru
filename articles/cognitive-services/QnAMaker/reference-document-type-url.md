---
title: Типы URL-адресов, поддерживаемые для импорта — QnA Maker
description: Узнайте, как типы URL-адресов используются для импорта и создания пар QnA.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 01/02/2020
ms.openlocfilehash: 8bf50c1ea81cdf5246c47646d1a55926fe7d58d6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "91776703"
---
# <a name="urls-supported-for-importing-documents"></a>URL-адреса, поддерживаемые для импорта документов

Узнайте, как типы URL-адресов используются для импорта и создания пар QnA.

## <a name="faq-urls"></a>URL-адреса вопросов и ответов

QnA Maker может поддерживать веб-страницы вопросов и ответов в трех разных формах:

* Простые страницы вопросов и ответов
* Страницы вопросов и ответов со ссылками
* Страницы вопросов и ответов с домашней страницей разделов

### <a name="plain-faq-pages"></a>Простые страницы вопросов и ответов

Это наиболее распространенный тип страниц с вопросами и ответами, в котором ответы содержатся на той же странице, сразу после каждого вопроса.

Ниже показан пример простой страницы вопросов и ответов.

![Пример простой страницы с вопросами и ответами для базы знаний](./media/qnamaker-concepts-datasources/plain-faq.png)


### <a name="faq-pages-with-links"></a>Страницы вопросов и ответов со ссылками

В этом типе страниц с вопросами и ответами все вопросы собраны вместе и содержат ссылки на ответы, расположенные либо в разных разделах на той же странице, либо на разных страницах.

Ниже показан пример страницы вопросов и ответов со ссылками на разделы, которые расположенные на той же странице.

 ![Ссылка на раздел на странице с вопросами и ответами для базы знаний](./media/qnamaker-concepts-datasources/sectionlink-faq.png)


### <a name="parent-topics-page-links-to-child-answers-pages"></a>Страницы родительских разделов ссылки на дочерние страницы ответов

Этот тип часто задаваемых вопросов содержит страницу разделов, где каждый раздел связан с соответствующим набором вопросов и ответов на другой странице. QnA Maker обходит все связанные страницы, чтобы извлечь соответствующие вопросы & ответы.

Ниже приведен пример страницы разделов со ссылками на разделы часто задаваемых вопросов на разных страницах.

 ![Прямая ссылка на страницу с вопросами и ответами для базы знаний](./media/qnamaker-concepts-datasources/topics-faq.png)

## <a name="support-urls"></a>URL-адреса для поддержки

QnA Maker может обрабатывать полуструктурированные веб-страницы поддержки, такие как статьи в Интернете, описывающие способы выполнения определенной задачи, способы диагностики и устранения проблемы и рекомендации для процесса. Извлечение лучше всего работает с документами, которые имеют четкую структуру с иерархической структурой заголовков.

> [!NOTE]
> Извлечение для статьи о поддержке — это новая функция, которая находится на ранних стадиях. Она лучше всего подходит для простых страниц, которые хорошо структурированы и не содержат сложные колонтитулы.

![QnA Maker поддерживает извлечение из частично структурированных веб-страниц, где есть четкая структура и иерархические заголовки.](./media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)