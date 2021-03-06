---
title: Общие сведения об управляемом экземпляре Azure для Apache Cassandra
description: Подробнее об управляемом экземпляре Azure для Apache Cassandra. Эта служба управляет развертыванием и масштабированием собственных экземпляров Apache Cassandra с открытым исходным кодом в Azure.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: overview
ms.date: 03/02/2021
ms.openlocfilehash: d99c62e7d88510c351f87d85b4f8c5c271767cb3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "101747837"
---
# <a name="what-is-azure-managed-instance-for-apache-cassandra-preview"></a>Что такое управляемый экземпляр Azure для Apache Cassandra? (предварительная версия)

> [!IMPORTANT]
> Управляемый экземпляр базы данных SQL Azure для Apache Cassandra в настоящее время предоставляется в режиме общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены.
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Служба "Управляемый экземпляр базы данных SQL Azure для Apache Cassandra" позволяет автоматизировать операции развертывания и масштабирования для управляемых решений Apache Cassandra с открытым кодом для центров обработки данных. Служба позволяет ускорить реализацию гибридных сценариев и сократить объем текущего обслуживания. В обще выпуске будут доступны возможности глубокой интеграции и перемещения данных с помощью программного интерфейса [AP Apache Cassandra для базы данных Azure Cosmos](../cosmos-db/cassandra-introduction.md).

:::image type="content" source="./media/introduction/icon.gif" alt-text="Управляемый экземпляр базы данных SQL Azure для Apache Cassandra — это управляемая служба для Apache Cassandra." border="false":::

## <a name="key-benefits"></a>Основные преимущества

### <a name="hybrid-deployments"></a>Гибридные развертывания

Эта служба упрощает размещение управляемых экземпляров центров обработки данных Apache Cassandra, которые развертываются автоматически как масштабируемые наборы виртуальных машин в новой или существующей виртуальной сети Azure. Эти центры обработки данных можно добавить в существующий канал Apache Cassandra, работающий локально через [Azure ExpressRoute](/azure/architecture/reference-architectures/hybrid-networking/expressroute) в Azure, или в другую облачную среду.

- **Упрощенное развертывание.** После установки гибридного подключения можно с легкостью выполнить развертывание с помощью протокола сплетен.
- **Размещенные метрики.** Метрики размещаются в [Prometheus](https://prometheus.io/docs/introduction/overview/) для отслеживания активности в кластере.

### <a name="simplified-scaling"></a>Упрощенное масштабирование

В управляемом экземпляре можно управлять всеми операциями вертикального масштабирования узлов центра обработки данных. Введите необходимое количество узлов, и они будут работать под управлением оркестратора масштабирования на канале Cassandra.

### <a name="managed-and-cost-effective"></a>Полная управляемость и рентабельность

Эта служба обеспечивает управление основными задачами Apache Cassandra:

- Подготовка кластера
- Подготовка центра обработки данных
- Масштабирование центра обработки данных
- Удаление центра обработки данных
- Запуск восстановления в пространстве ключей
- Изменение конфигурации центра обработки данных
- Настройка резервных копий

Гибкая модель ценообразования основана на экземплярах и не требует вознаграждений за лицензирование. Эта модель ценообразования позволяет настраивать решение в соответствии с требованиями к рабочим нагрузкам. Вы выбираете количество ядер, номер SKU виртуальной машины, объем памяти и необходимый размер дискового пространства.

## <a name="next-steps"></a>Дальнейшие действия

Начните работу с одним из кратких руководств:

* [Создание кластера управляемых экземпляров на портале Azure](create-cluster-portal.md)
* [Развертывание управляемого кластера Apache Spark с Azure Databricks](deploy-cluster-databricks.md)
* [Управление ресурсами службы "Управляемый экземпляр Azure для ресурсов Apache Cassandra" с помощью Azure CLI](manage-resources-cli.md)
