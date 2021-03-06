---
title: Общие сведения о ценах на общие ресурсы данных Azure
description: Узнайте, как работают цены на общий доступ к данным Azure
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 1c2c58e206a70a3801c712df3b5582409712d3c8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "88137042"
---
# <a name="understand-azure-data-share-pricing"></a>Общие сведения о ценах на общие ресурсы данных Azure

Для совместного использования на основе моментальных снимков стоимость общего ресурса Azure для моментального снимка набора данных и выполнения моментальных снимков. В этой статье объясняется, как оценить выполнение моментального снимка и выполнения моментальных снимков с помощью данных журнала моментальных снимков. В настоящее время поставщик данных оплачивается для моментального снимка набора данных и выполнения моментального снимка.

## <a name="dataset-snapshot"></a>Моментальный снимок набора данных

Моментальный снимок набора данных — это операция перемещения набора данных из источника в место назначения. При создании моментального снимка для общей папки моментальный снимок каждого набора данных в общей папке является операцией моментального снимка набора данных. Выполните следующие действия, чтобы просмотреть список моментальных снимков набора данных. 

1. В портал Azure перейдите к общему ресурсу данных.

1. Выберите общую папку из отправленного общего ресурса или полученного общего ресурса.

1. Перейдите на вкладку **Журнал** .

1. Щелкните время начала создания моментального снимка.
 
    ![Журнал моментальных снимков](./media/concepts/concepts-pricing/pricing-snapshot-history.png "Журнал моментальных снимков") 

1. Просмотр списка операций моментальных снимков набора данных. Каждый элемент строки соответствует операции моментального снимка набора данных. В этом примере есть два моментальных снимка набора данных.

    ![Операция моментального снимка набора данных](./media/concepts/concepts-pricing/pricing-dataset-snapshot.png "Операция моментального снимка набора данных")

## <a name="snapshot-execution"></a>Выполнение моментального снимка

Выполнение моментального снимка включает ресурсы, необходимые для перемещения набора данных из источника в место назначения. Для каждой операции создания моментального снимка набора данных выполнение моментального снимка вычисляется как число виртуальных ядер, умноженное на длительность моментального снимка. Плата рассчитывается пропорционально минуте и округляется. Число Виртуальное ядро выбирается динамически на основе пары "источник — целевой объект" и шаблона данных. Выполните следующие действия, чтобы просмотреть время начала моментального снимка, время окончания и виртуальных ядер, используемые для операции моментального снимка набора данных.

1. В портал Azure перейдите к общему ресурсу данных.

1. Выберите общую папку из отправленного общего ресурса или полученного общего ресурса.

1. Перейдите на вкладку **Журнал** .

1. Щелкните время начала создания моментального снимка.

    ![Журнал моментальных снимков](./media/concepts/concepts-pricing/pricing-snapshot-history.png "Журнал моментальных снимков") 

1. Щелкните состояние операции моментального снимка набора данных.

    ![Состояние моментального снимка набора данных](./media/concepts/concepts-pricing/pricing-snapshot-status.png "Состояние моментального снимка набора данных")

1. Просмотр времени начала, времени окончания и виртуальных ядер, используемых для этой операции моментального снимка набора данных. 

    ![Выполнение моментального снимка](./media/concepts/concepts-pricing/pricing-snapshot-execution.png "Выполнение моментального снимка")

## <a name="next-steps"></a>Дальнейшие действия

- Получение последних сведений о ценах — общие сведения о ценах на [данные Azure](https://azure.microsoft.com/pricing/details/data-share/)
- Оценка стоимости — [Калькулятор цен Azure](https://azure.microsoft.com/pricing/calculator/) с помощью калькулятора цен Azure
