---
title: Настройка кластеров для интеграции Azure Active Directory
titleSuffix: Azure HDInsight
description: Узнайте, как настроить и настроить кластер HDInsight, интегрированный с Active Directory, с помощью Azure Active Directory доменных служб и Корпоративный пакет безопасности компонента.
ms.service: hdinsight
ms.topic: how-to
ms.custom: seodec18,seoapr2020, contperf-fy21q2
ms.date: 10/30/2020
ms.openlocfilehash: 6f478b97464cd47e9d0e04bfe83bd48a2b3bfe7c
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104867106"
---
# <a name="configure-hdinsight-clusters-for-azure-active-directory-integration-with-enterprise-security-package"></a>Настройка кластеров HDInsight для интеграции Azure Active Directory с Корпоративный пакет безопасности

Эта статья содержит сводку и обзор процесса создания и настройки кластера HDInsight, интегрированного с Azure Active Directory. Эта интеграция зависит от функции HDInsight, которая называется Корпоративный пакет безопасности (ESP), Azure Active Directory доменных служб (Azure AD-DS) и ранее существовавшей локальной Active Directory.

Подробное пошаговое руководство по настройке и настройке домена в Azure и созданию кластера с поддержкой ESP с последующей синхронизацией локальных пользователей см. [в статье Создание и настройка кластеров корпоративный пакет безопасности в Azure HDInsight](apache-domain-joined-create-configure-enterprise-security-cluster.md).

## <a name="background"></a>Фон

Корпоративный пакет безопасности (ESP) обеспечивает интеграцию Active Directory для Azure HDInsight. Такая интеграция позволяет пользователям домена использовать учетные данные домена для аутентификации в кластерах HDInsight и выполнения заданий обработки больших данных.

> [!NOTE]  
> Протокол ESP общедоступен в HDInsight 3,6 и 4,0 для следующих типов кластера: Apache Spark, Interactive, Hadoop и HBase. ESP для типа кластера Apache Kafka находится в режиме предварительной версии с поддержкой наилучшей поддержки. Кластеры ESP, созданные до даты протокола ESP (1 октября 2018), не поддерживаются.

## <a name="prerequisites"></a>Предварительные требования

Прежде чем можно будет создать кластер HDInsight с поддержкой ESP, необходимо выполнить несколько предварительных требований:

- Существующие локальные Active Directory и Azure Active Directory.
- Включите Azure AD-DS.
- Проверьте состояние работоспособности DS Azure AD, чтобы убедиться, что синхронизация завершена.
- Создание и авторизация управляемого удостоверения.
- Завершите настройку сети для DNS и связанных с ней проблем.

Каждый из этих элементов будет подробно рассмотрен ниже. Пошаговое руководство по выполнению всех этих действий см. [в статье Создание и настройка кластеров корпоративный пакет безопасности в Azure HDInsight](apache-domain-joined-create-configure-enterprise-security-cluster.md).

### <a name="enable-azure-ad-ds"></a>Включение Azure AD DS

Для создания кластера HDInsight с помощью ESP необходимо включить AD DS Azure. Дополнительные сведения см. [в статье включение Azure Active Directory доменных служб с помощью портал Azure](../../active-directory-domain-services/tutorial-create-instance.md).

Когда AD DS Azure включен, все пользователи и объекты начинают синхронизацию из Azure Active Directory (Azure AD) с AD DS Azure по умолчанию. Продолжительность операции синхронизации зависит от числа объектов в Azure AD. Синхронизация может занять несколько дней для сотен тысяч объектов.

Для работы с HDInsight имя домена, используемое в AD DS Azure, не должно содержать 39 символов или меньше.

Можно выбрать синхронизацию только тех групп, которым требуется доступ к кластерам HDInsight. Этот вариант синхронизации только определенных групп называется *синхронизацией определенных объектов*. Инструкции см. в статье [Настройка синхронизации с заданной областью из Azure AD в управляемый домен](../../active-directory-domain-services/scoped-synchronization.md).

При включении защищенного протокола LDAP введите имя домена в поле имя субъекта. И альтернативное имя субъекта в сертификате. Если доменное имя — *contoso100.onmicrosoft.com*, убедитесь, что в имени субъекта сертификата и альтернативном имени субъекта существует точное имя. Дополнительные сведения см. в разделе [Настройка защищенного протокола LDAP (LDAPS) для управляемого домена доменных служб Azure AD](../../active-directory-domain-services/tutorial-configure-ldaps.md).

В следующем примере создается самозаверяющий сертификат. Доменное имя *contoso100.onmicrosoft.com* находится в обоих полях `Subject` (имя субъекта) и `DnsName` (альтернативное имя субъекта).

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.onmicrosoft.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.onmicrosoft.com, contoso100.onmicrosoft.com
```

> [!NOTE]  
> Только администраторы клиента имеют права на включение AD DS Azure. Если хранилище кластера имеет Azure Data Lake Storage 1-го поколения или Gen2, необходимо отключить многофакторную идентификацию Azure AD только для тех пользователей, которым требуется доступ к кластеру с использованием обычной проверки подлинности Kerberos.
>
> Вы можете использовать [Надежные IP-адреса](../../active-directory/authentication/howto-mfa-mfasettings.md#trusted-ips) или [Условный доступ](../../active-directory/conditional-access/overview.md) , чтобы отключить многофакторную проверку подлинности для конкретных пользователей, *только* если они обращаются к диапазону IP для виртуальной сети кластера HDInsight. Если вы используете условный доступ, убедитесь, что конечная точка службы Active Directory включена в виртуальной сети HDInsight.
>
> Если хранилище кластера является хранилищем BLOB-объектов Azure, не отключайте многофакторную проверку подлинности.

### <a name="check-azure-ad-ds-health-status"></a>Проверка состояния работоспособности AD DS Azure

Просмотрите состояние работоспособности Azure Active Directory доменных служб, выбрав **работоспособность** в категории **Управление** . Убедитесь, что AD DS Azure имеет значение зеленый (работает) и синхронизация завершена.

:::image type="content" source="./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png" alt-text="Работоспособность AD DS Azure" border="true":::

### <a name="create-and-authorize-a-managed-identity"></a>Создание и авторизация управляемого удостоверения

Используйте *управляемое пользователем удостоверение* для упрощения операций с защищенными доменными службами. При назначении управляемому удостоверению роли **участника доменных служб HDInsight** она может читать, создавать, изменять и удалять операции доменных служб.

Некоторые операции доменных служб, такие как создание подразделений и субъектов-служб, необходимы для Корпоративный пакет безопасности HDInsight. Вы можете создавать управляемые удостоверения в любой подписке. Дополнительные сведения об управляемых удостоверениях см. в статье [управляемые удостоверения для ресурсов Azure](../../active-directory/managed-identities-azure-resources/overview.md). Дополнительные сведения о работе управляемых удостоверений в Azure HDInsight см. [в статье управляемые удостоверения в Azure hdinsight](../hdinsight-managed-identities.md).

Чтобы настроить кластеры ESP, создайте управляемое пользователем удостоверение, если оно еще не установлено. См. раздел [`Create, list, delete, or assign a role to a user-assigned managed identity by using the Azure portal`](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

Затем назначьте роль **участника доменных служб HDInsight** управляемому удостоверению в службе **контроля доступа** для Azure AD DS. Чтобы назначить эту роль, необходимы права администратора AD DS Azure.

:::image type="content" source="./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png" alt-text="Служба контроля доступа доменных служб Azure Active Directory" border="true":::

Назначение роли **участника доменных служб HDInsight** гарантирует, что это удостоверение имеет надлежащий ( `on behalf of` ) доступ для выполнения операций доменных служб в домене Azure AD DS. Эти операции включают создание и удаление подразделений.

После того как управляемому удостоверению назначается роль, администратор AD DS Azure управляет тем, кто его использует. Сначала администратор выбирает управляемое удостоверение на портале. Затем в разделе **Обзор** выберите **Управление доступом (IAM)** . Администратор назначает роль " **управляемый оператор идентификации** " пользователям или группам, которым требуется создавать кластеры ESP.

Например, администратор AD DS Azure может назначить эту роль группе **маркетингтеам** для управляемого удостоверения **сжмси** . Пример показан на следующем рисунке. Это назначение гарантирует, что сотрудники Организации смогут использовать управляемое удостоверение для создания кластеров ESP.

:::image type="content" source="./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png" alt-text="Назначение роли оператора управляемого удостоверения HDInsight" border="true":::

### <a name="network-configuration"></a>Сетевая конфигурация

> [!NOTE]  
> AD DS Azure необходимо развернуть в виртуальной сети на основе Azure Resource Manager. Классические виртуальные сети не поддерживаются для Azure AD DS. Дополнительные сведения см. [в статье включение Azure Active Directory доменных служб с помощью портал Azure](../../active-directory-domain-services/tutorial-create-instance-advanced.md#create-and-configure-the-virtual-network).

Включите доменные службы Azure AD. Затем локальный сервер службы доменных имен (DNS) выполняется на виртуальных машинах Active Directory. Настройте виртуальную сеть Azure AD DS для использования этих пользовательских DNS-серверов. Чтобы найти нужные IP-адреса, выберите **Свойства** в категории **Управление** и просмотрите раздел **IP-адрес в виртуальной сети**.

:::image type="content" source="./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns1.png" alt-text="Поиск IP-адресов для локальных DNS-серверов" border="true":::

Измените конфигурацию DNS-серверов в виртуальной сети Azure AD DS. Чтобы использовать эти настраиваемые IP-адреса, в категории " **Параметры** " выберите **серверы DNS** . Затем выберите параметр **Custom (пользовательский** ), введите первый IP-адрес в текстовом поле и нажмите кнопку **сохранить**. Добавьте дополнительные IP-адреса, выполнив те же действия.

:::image type="content" source="./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png" alt-text="Обновление конфигурации DNS виртуальной сети" border="true":::

Проще разместить экземпляр Azure AD DS и кластер HDInsight в одной и той же виртуальной сети Azure. Если вы планируете использовать разные виртуальные сети, необходимо выполнить одноранговую связь между виртуальными сетями, чтобы контроллер домена был виден виртуальным машинам HDInsight. Дополнительные сведения см. в статье [Пиринг между виртуальными сетями](../../virtual-network/virtual-network-peering-overview.md).

После пиринга виртуальных сетей настройте виртуальную сеть HDInsight для использования настраиваемого DNS-сервера. И введите частные IP-адреса AD DS Azure в качестве адресов серверов DNS. Если обе виртуальные сети используют одни и те же DNS-серверы, имя личного домена будет разрешаться в правильный IP-адрес и будет доступен из HDInsight. Например, если доменное имя — `contoso.com` , то после выполнения этого шага `ping contoso.com` следует разрешить доступ к правильному IP-адресу Azure AD DS.

:::image type="content" source="./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png" alt-text="Настройка пользовательских DNS-серверов для одноранговой виртуальной сети" border="true":::

Если вы используете правила группы безопасности сети (NSG) в подсети HDInsight, необходимо разрешить [необходимые IP-адреса](../hdinsight-management-ip-addresses.md) для входящего и исходящего трафика.

Чтобы протестировать сетевую настройку, присоедините виртуальную машину Windows к виртуальной сети или подсети HDInsight и проверьте связь с доменным именем. (Он должен разрешаться в IP-адрес.) Запустите **ldp.exe** , чтобы получить доступ к домену AD DS Azure. Затем присоедините эту виртуальную машину Windows к домену, чтобы убедиться в успешности всех необходимых вызовов RPC между клиентом и сервером.

Используйте **nslookup** , чтобы подтвердить сетевой доступ к вашей учетной записи хранения. Или любую внешнюю базу данных, которую можно использовать (например, External хранилище метаданных Hive или Ranger DB). Убедитесь, что [необходимые порты](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers) разрешены в ПРАВИЛАх NSG подсети Azure AD DS, если NSG защищает AD DS Azure. Если домен присоединяется к этой виртуальной машине Windows успешно, можно перейти к следующему шагу и создать кластеры ESP.

## <a name="create-an-hdinsight-cluster-with-esp"></a>Создание кластера HDInsight с помощью ESP

После правильной настройки предыдущих шагов необходимо создать кластер HDInsight с включенным протоколом ESP. При создании кластера HDInsight можно включить Корпоративный пакет безопасности на вкладке **безопасность и сеть** . Для Azure Resource Manager шаблона развертывания воспользуйтесь порталом один раз. Затем скачайте шаблон с заполнением на странице " **Проверка и создание** " для повторного использования в будущем.

Вы также можете включить компонент [брокера идентификаторов HDInsight](identity-broker.md) во время создания кластера. Компонент Service Broker позволяет войти в Ambari с помощью многофакторной проверки подлинности и получить необходимые билеты Kerberos, не требуя хэширования паролей в Azure AD DS.

> [!NOTE]  
> Первые шесть символов имени кластера ESP должно быть уникальными в вашей среде. Например, если в разных виртуальных сетях имеется несколько кластеров ESP, выберите соглашение об именовании, которое гарантирует уникальность первых шести символов в именах кластеров.

:::image type="content" source="./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp.png" alt-text="Проверка домена для Корпоративный пакет безопасности Azure HDInsight" border="true":::

После включения ESP стандартные неправильности конфигурации, связанные с Azure AD DS, автоматически обнаруживаются и проверяются. После устранения этих ошибок можно перейти к следующему шагу.

:::image type="content" source="./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp-error.png" alt-text="Сбой проверки домена Корпоративный пакет безопасности Azure HDInsight" border="true":::

При создании кластера HDInsight с помощью ESP необходимо указать следующие параметры:

* **Администратор кластера**: выберите администратора кластера из синхронизированного экземпляра AD DS Azure. Эта учетная запись домена должна быть уже синхронизирована и доступна в Azure AD DS.

* **Группы доступа к кластеру**: группы безопасности, чьи пользователи должны синхронизироваться и иметь доступ к кластеру, должны быть доступны в AD DS Azure. Примером является группа HiveUsers. Дополнительные сведения см. в статье [Создание группы и добавление в нее пользователей в Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

* **URL-адрес LDAPS**. Пример: `ldaps://contoso.com:636` .

Созданное управляемое удостоверение можно выбрать из раскрывающегося списка **управляемое удостоверение, назначаемое пользователем** при создании нового кластера.

:::image type="content" source="./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-identity.png" alt-text="Управляемое удостоверение Azure HDINSIGHT ESP домен Active Directory Services" border="true":::.

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о настройке политик Hive и выполнении запросов Hive см. в статье [Настройка политик Apache Hive в HDInsight с Корпоративным пакетом безопасности](apache-domain-joined-run-hive.md).
* Сведения о подключении к кластерам HDInsight с корпоративным пакетом безопасности с использованием SSH см. в разделе [Проверка подлинности при использовании присоединенного к домену кластера HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight).
