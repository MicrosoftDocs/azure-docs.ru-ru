---
title: Создание предложения виртуальной машины Azure в Azure Marketplace.
description: Сведения о том, как создать предложение виртуальной машины и отправить ее образ на коммерческую платформу Майкрософт.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 03/10/2021
ms.openlocfilehash: 1ea8583b33fbe711fcffbf9626236923ecd0df8b
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2021
ms.locfileid: "107107233"
---
# <a name="how-to-create-a-virtual-machine-offer-on-azure-marketplace"></a>Создание предложения виртуальной машины Azure в Azure Marketplace

В этой статье описывается, как создать и опубликовать предложение виртуальной машины Azure в [Azure Marketplace](https://azuremarketplace.microsoft.com/). Она подходит для виртуальных машин на базе Windows или Linux, оснащенных операционной системой, виртуальным жестким диском (VHD) и дисками данных в количестве до 16 штук.

Прежде чем начать, изучите статью [Управление учетной записью коммерческого магазина в центре партнеров](create-account.md). Убедитесь, что ваша учетная запись зарегистрирована в программе коммерческой платформы.

## <a name="before-you-begin"></a>Подготовка к работе

Если вы еще не сделали этого, ознакомьтесь с разделом [Планирование предложения виртуальной машины](marketplace-virtual-machines.md). В нем объясняются технические требования для виртуальной машины и перечисляются сведения и активы, которые понадобятся при создании предложения.

## <a name="create-a-new-offer"></a>Создание нового предложения

1. Войдите в [Центр партнеров](https://partner.microsoft.com/dashboard/home).
2. В меню слева выберите **Коммерческая платформа** > **Обзор**.
3. На странице **Обзор** выберите **Новое предложение** > **Виртуальная машина Azure**.

    ![Снимок экрана, показывающий параметры меню в левой части экрана и кнопку "Новое предложение".](./media/create-vm/new-offer-azure-virtual-machine.png)

> [!NOTE]
> После того как предложение будет опубликовано, все изменения, которые вы внесете в него в Центре партнеров, появятся в Azure Marketplace только после повторной публикации этого предложения. Если вы вносили в предложение изменения, обязательно опубликуйте его повторно.

Заполните поле **ИД предложения**. У каждого предложения в вашей учетной записи должен быть уникальный идентификатор.

- Идентификатор отображается для клиентов в веб-адресе предложения на Azure Marketplace, а также, если применимо, в Azure PowerShell и Azure CLI.
- Вы можете использовать только строчные буквы и цифры. Идентификатор может содержать дефисы и символы подчеркивания, но не пробелы и может включать не больше 50 символов. Например, если вы введете **test-offer-1**, веб-адрес предложения будет иметь следующий вид: `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Идентификатор предложения нельзя изменить после выбора команды **Создать**.

Укажите **псевдоним** для вашего предложения. Псевдоним предложения — это имя, которое используется для предложения в Центре партнеров.

- Это имя не используется в Azure Marketplace. Оно отличается от имени предложения и других значений, видимых клиентам.

Выберите **Создать**, чтобы создать предложение и продолжить. В Центре партнеров откроется страница **Настройка предложения**.

## <a name="enable-a-test-drive-optional"></a>Включение тестового выпуска (необязательно)

Тестовый выпуск — это отличный способ продемонстрировать свое предложение потенциальным клиентам, предоставив им доступ к предварительно настроенной среде на определенное количество часов. Предложив тестовый выпуск, вы увеличите скорость преобразования и создадите интересы с высоким уровнем квалификации. Дополнительные сведения о тестовых выпусках см. в статье [Что такое тестовый выпуск?](./what-is-test-drive.md).

> [!TIP]
> Тестовый выпуск отличается от бесплатной пробной версии. Вы можете предложить либо тестовый выпуск, либо бесплатный пробный период, либо и то, и другое. Все варианты предоставляют клиентам ваше решение на фиксированный промежуток времени. Во время тестового выпуска клиенты самостоятельно протестируют пробную версию продукта, ознакомятся с его ключевыми функциями и преимуществами на практике.

Чтобы активировать тестовый выпуск на определенный период времени, установите флажок **Включить тестовый выпуск** на вкладке "Настройка предложения". Тестовый выпуск будет настроен позже. При использовании тестового диска требуется настроить CRM (см. следующий раздел).

## <a name="configure-customer-leads-management"></a>Управление настройкой получения сведений о потенциальных клиентах

[!INCLUDE [Customer leads](includes/customer-leads.md)] 

Выберите **Сохранить черновик**, прежде чем перейти к следующей вкладке в меню слева, щелкните **Свойства**.

## <a name="next-steps"></a>Следующие шаги

- [Настройка свойств предложения для виртуальной машины](azure-vm-create-properties.md)
- [Рекомендации по размещению предложений](gtm-offer-listing-best-practices.md)