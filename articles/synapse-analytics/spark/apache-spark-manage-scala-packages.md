---
title: Управление библиотеками Scala & Java для Apache Spark
description: Узнайте, как добавлять библиотеки Scala и Java в Azure синапсе Analytics и управлять ими.
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: midesa
ms.reviewer: jrasnick
ms.subservice: spark
ms.openlocfilehash: c70ecc4fc5469d728bc12d47024585ccf00ff98e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102098712"
---
# <a name="manage-scala-and-java-packages-for-apache-spark-in-azure-synapse-analytics"></a>Управление пакетами Scala и Java для Apache Spark в Azure синапсе Analytics

Библиотеки предоставляют многократно используемый код, который может потребоваться включить в программы или проекты. 

Может потребоваться обновить бессерверную среду Apache Spark пула по нескольким причинам. Например, может оказаться, что:
- Одна из основных зависимостей выпустила новую версию.
- необходим дополнительный пакет для обучения модели машинного обучения или подготовки данных.
- Вы нашли более эффективный пакет и больше не нуждаются в старом пакете.

Чтобы сделать код, разработанный сторонним приложением, доступным для приложений, можно установить библиотеку на одном из бессерверных Apache Sparkных пулов или сеансов записных книжек. В этой статье мы рассмотрим, как можно управлять пакетами Scala и Java.

## <a name="default-installation"></a>Установка по умолчанию
Apache Spark в Azure синапсе Analytics содержит полный набор библиотек для общих задач проектирования данных, подготовки данных, машинного обучения и визуализации данных. Список полных библиотек можно найти по адресу [Apache Spark поддержки версий](apache-spark-version-support.md). 

При запуске экземпляра Spark эти библиотеки будут автоматически добавлены. Дополнительные пакеты Scala/Java можно добавить на уровне пула Spark и сеанса.

## <a name="workspace-packages"></a>Пакеты рабочей области
Пакеты рабочей области могут быть пользовательскими или частными JAR-файлами. Вы можете передать эти пакеты в рабочую область и позже назначить их конкретному пулу Spark.

Чтобы добавить пакеты рабочей области, выполните следующие действия.
1. Перейдите на вкладку **Управление**  >  **пакетами рабочей области** .
2. Отправьте JAR-файлы с помощью средства выбора файлов.
3. После отправки файлов в рабочую область Azure синапсе можно добавить эти JAR-файлы в заданный пул Apache Spark.

![Снимок экрана, на котором выделены пакеты рабочей области.](./media/apache-spark-azure-portal-add-libraries/studio-add-workspace-package.png "Просмотр пакетов рабочей области")

## <a name="pool-libraries"></a>Библиотеки пулов
Определив пакеты Scala и Java, которые вы хотите использовать для приложения Spark, можно установить их в пул Spark. Библиотеки уровня пула доступны для всех записных книжек и заданий, выполняющихся в пуле.

Вы можете обновить библиотеки пула Spark, перейдя в Azure синапсе Studio или портал Azure. Здесь можно выбрать библиотеки рабочей области для установки. 

После сохранения изменений задание Spark запустит установку и кэширует полученную среду для последующего повторного использования. После завершения задания новые задания Spark или сеансы записных книжек будут использовать обновленные библиотеки пула. 

> [!IMPORTANT]
> - Если устанавливаемый пакет является большим или занимает много времени, это повлияет на время запуска экземпляра Spark.
> - Изменение версии PySpark, Python, Scala/Java, .NET или Spark не поддерживается.

#### <a name="manage-packages-from-azure-synapse-studio-or-azure-portal"></a>Управление пакетами из Azure синапсе Studio или портал Azure
Для управления библиотеками пула Spark можно использовать Azure синапсе Studio или портал Azure. 

Чтобы обновить или добавить библиотеки в пул Spark, выполните следующие действия.
1. Перейдите к рабочей области Azure синапсе Analytics из портал Azure.

    При обновлении с **портал Azure**:

    - В разделе **ресурсов синапсе** выберите вкладку **Пулы Apache Spark** и выберите пул Spark из списка.
     
    - Выберите **пакеты** из раздела **Параметры** пула Spark.
  
    ![Снимок экрана, посвященный кнопке "отправить файл конфигурации среды".](./media/apache-spark-azure-portal-add-libraries/apache-spark-add-library-azure.png "Добавление библиотек Python")
   
    При обновлении из **синапсе Studio**:
    - Выберите **Управление** на главной панели навигации, а затем выберите **Пулы Apache Spark**.

    - Выберите раздел **пакеты** для определенного пула Spark.
    ![Снимок экрана, посвященный параметру конфигурации "Отправить среду" из студии.](./media/apache-spark-azure-portal-add-libraries/studio-update-libraries.png "Добавление библиотек Python из студии")
   
2. Чтобы добавить JAR-файлы, перейдите к разделу " **пакеты рабочей области** ", чтобы добавить его в пул. 
3. После сохранения изменений будет запущено системное задание для установки и кэширования указанных библиотек. Этот процесс позволяет сократить общее время запуска сеанса. 
4. После успешного завершения задания все новые сеансы будут получать обновленные библиотеки пула.

> [!IMPORTANT]
> Выбрав параметр для **принудительного создания новых параметров**, вы завершите все текущие сеансы для выбранного пула Spark. После завершения сеансов необходимо будет дождаться перезапуска пула. 
>
> Если этот параметр не установлен, необходимо дождаться завершения текущего сеанса Spark или завершить его вручную. После завершения сеанса необходимо разрешить перезапуск пула.

#### <a name="track-installation-progress-preview"></a>Отслеживать ход установки (Предварительная версия)
Зарезервированное системное задание Spark инициируется каждый раз при обновлении пула с помощью нового набора библиотек. Это задание Spark помогает отслеживать состояние установки библиотеки. Если установка завершается сбоем из-за конфликтов библиотек или других проблем, пул Spark вернется к предыдущему состоянию или по умолчанию. 

Кроме того, пользователи также могут проверить журналы установки, чтобы выявить конфликты зависимостей, или узнать, какие библиотеки были установлены во время обновления пула.

Для просмотра этих журналов выполните следующие действия.
1. Перейдите в список приложений Spark на вкладке " **монитор** ". 
2. Выберите Задание системного приложения Spark, соответствующее обновлению пула. Эти системные задания выполняются под названием *системресерведжоб-либрариманажемент* .
   ![Снимок экрана, на котором выделяется зарезервированное системное задание библиотеки.](./media/apache-spark-azure-portal-add-libraries/system-reserved-library-job.png "Просмотр задания системной библиотеки")
3. Переключитесь на просмотр журналов **драйверов** и **stdout** . 
4. В результатах вы увидите журналы, связанные с установкой пакетов.
    ![Снимок экрана, на котором показаны результаты задания зарезервированной системной библиотеки.](./media/apache-spark-azure-portal-add-libraries/system-reserved-library-job-results.png "Просмотр хода выполнения задания системной библиотеки")

## <a name="session-scoped-libraries"></a>Библиотеки с областью действия сеанса 
Помимо библиотек уровня пула, в начале сеанса записной книжки можно также указать библиотеки с областью действия сеанса.  Библиотеки с областью действия сеанса позволяют указывать и использовать JAR-пакеты исключительно в рамках сеанса записной книжки. 

При использовании библиотек с областью действия сеанса важно учитывать следующие моменты.
   - При установке библиотек с областью действия сеанса доступ к указанным библиотекам будет иметь только Текущая записная книжка. 
   - Эти библиотеки не влияют на другие сеансы или задания, использующие один и тот же пул Spark. 
   - Эти библиотеки устанавливаются поверх базовой среды выполнения и библиотек уровня пула. 
   - Библиотеки записных книжек будут иметь наивысший приоритет.

Чтобы указать пакеты Java или Scala с областью действия сеанса, можно использовать ```%%configure``` параметр:

```scala
%%configure -f
{
    "conf": {
        "spark.jars": "abfss://<<file system>>@<<storage account>.dfs.core.windows.net/<<path to JAR file>>",
    }
}
```

Мы рекомендуем запустить%% configure в начале записной книжки. Полный список допустимых параметров можно найти в этом [документе](https://github.com/cloudera/livy#request-body) .

## <a name="next-steps"></a>Дальнейшие действия
- Просмотр библиотек по умолчанию: [Поддержка версий Apache Spark](apache-spark-version-support.md)
- Устранение ошибок при установке библиотеки: [Устранение неполадок библиотеки](apache-spark-troubleshoot-library-errors.md)
