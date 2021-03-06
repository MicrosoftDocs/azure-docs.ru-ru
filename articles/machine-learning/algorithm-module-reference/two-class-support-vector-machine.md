---
title: 'Two-Class поддержки векторной машины: ссылка на модуль'
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль Two-Class поддержки векторного компьютера в Машинное обучение Azure для создания двоичного классификатора.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/22/2020
ms.openlocfilehash: 46cfdd319fc89e569d165dc2e11303e67c6dd54e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "93420569"
---
# <a name="two-class-support-vector-machine-module"></a>Модуль Two-Class поддержки векторного компьютера

В этой статье описывается модуль в конструкторе Машинное обучение Azure.

Этот модуль используется для создания модели, основанной на алгоритме поддержки машинного вектора. 

Виртуальные машины поддержки (SVM) — это хорошо переоцениваемый класс методов защищенного обучения. Эта конкретная реализация подходит для прогнозирования двух возможных результатов на основе непрерывных или категорий переменных.

После определения параметров модели обучить модель с помощью обучающих модулей и предоставляя *набор данных с тегами* , включающий столбец меток или результатов.

## <a name="about-support-vector-machines"></a>О поддержке виртуальных машин

Методы опорных векторов принадлежат к самым первым алгоритмам машинного обучения, а модели по этому методу использовались во многих сценариях — от получения информации до классификации текста и изображений. SVM можно использовать для задач классификации и регрессии.

Эта модель SVM является защищенной моделью обучения, которая требует наличия помеченных данных. В процессе обучения алгоритм анализирует входные данные и распознает закономерности в многомерном пространстве, именуемом «назначением *».*  Все входные примеры представлены в этом пространстве в виде точек и сопоставлены с выходными категориями таким образом, чтобы категории были разделены по ширине и по возможности очищать зазор.

Для прогнозирования алгоритм SVM назначает новые примеры в одной или другой категории, сопоставляя их в одну и ту же область. 

## <a name="how-to-configure"></a>Порядок настройки 

Для этого типа модели рекомендуется нормализовать набор данных перед его использованием для обучения классификатора.
  
1.  Добавьте в конвейер модуль **поддержки для векторного компьютера с двумя классами** .  
  
2.  Укажите, как должна быть обучена модель, установив параметр " **создать режим инструктора** ".  
  
    -   **Один параметр**. Если вы умеете настраивать модель, вы можете указать конкретный набор значений в качестве аргументов.  

    -   **Диапазон параметров**. Если вы не знаете наилучших параметров, оптимальные параметры можно найти с помощью модуля [Настройка модели параметры](tune-model-hyperparameters.md) . Вы предоставляете некоторый диапазон значений, и преподаватель выполняет итерацию по нескольким сочетаниям параметров, чтобы определить сочетание значений, которое дает наилучший результат.

3.  Для параметра **число итераций** введите число, которое обозначает количество итераций, используемых при построении модели.  
  
     Этот параметр можно использовать для достижения компромисса между скоростью обучения и его точностью.  
  
4.  В поле **лямбда-выражение** введите значение, которое будет использоваться в качестве веса для применения в качестве весов.  
  
     Это коэффициент регуляризации, который можно использовать для настройки модели. Большие значения ведут к ухудшению более сложных моделей.  
  
5.  Выберите параметр **нормализация функций**, если требуется нормализовать функции перед обучением.
  
     При применении нормализации до начала обучения точки данных центрируются по среднему и масштабируются в одну единицу стандартного отклонения.
  
6.  Чтобы нормализовать коэффициенты, выберите пункт **проект для модульной сферы**.
  
     Проецирование значений в Модульное пространство означает, что перед обработкой точки данных центрируются в 0 и масштабируются в одну единицу стандартного отклонения.
  
7.  В окне **Начальное число случайных чисел** введите целое значение, которое будет использоваться в качестве начального значения, если требуется обеспечить воспроизводимость между запусками.  В противном случае в качестве начального значения используется системное значение системных часов, что может привести к слегка разным результатам во время выполнения.
  
9. Подключите набор данных с меткой и обучите модель:

    + Если присвоить **параметру** **создать режим инструктора** значение Single, подключить набор данных с тегами и модуль [обучение модели](train-model.md) .  
  
    + Если задать **режим создания инструктора** в **диапазоне параметров**, подключите набор данных с тегами и обучите модель с помощью [параметров настройки модели](tune-model-hyperparameters.md).  
  
    > [!NOTE]
    > 
    > При передаче диапазона параметров для [обучения модели](train-model.md)используется только значение по умолчанию в списке с одним параметром.  
    > 
    > Если передать один набор значений параметров в модуль [Настройка модели настройки](tune-model-hyperparameters.md) , когда он ожидает диапазон параметров для каждого параметра, он пропускает значения и использует значения по умолчанию для учений.  
    > 
    > Если выбрать параметр **диапазон параметров** и ввести одно значение для любого параметра, это единственное заданное значение будет использоваться во время очистки, даже если другие параметры меняются в диапазоне значений.
  
10. Отправьте конвейер.

## <a name="results"></a>Результаты

После завершения обучения:

+ Чтобы сохранить моментальный снимок обученной модели, выберите вкладку **выходные данные** в правой панели модуля **обучение модели** . Щелкните значок **зарегистрировать набор данных** , чтобы сохранить модель как модуль для повторного использования.

+ Чтобы использовать модель для оценки, добавьте модуль **оценки модели** в конвейер.


## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [набором доступных модулей](module-reference.md) в службе Машинного обучения Azure. 