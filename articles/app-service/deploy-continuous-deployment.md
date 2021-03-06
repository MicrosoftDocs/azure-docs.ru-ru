---
title: настройка непрерывного развертывания;
description: Узнайте, как включить CI/CD в службу приложений Azure из GitHub, BitBucket, Azure Repos или других репозиториев. Выберите конвейер сборки, соответствующий вашим потребностям.
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.topic: article
ms.date: 03/12/2021
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 52f0db739cff9614dc4e9f5ef71d582e926fc65a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103470274"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Непрерывное развертывание в службе приложений Azure

[Служба приложений Azure](overview.md) обеспечивает непрерывное развертывание из репозиториев [GitHub](https://help.github.com/articles/create-a-repo), [BitBucket](https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html)и [Azure Repos](/azure/devops/repos/git/creatingrepo) , потянув за последние обновления.

> [!NOTE]
> Страница « **центр разработки» (классическая)** в портал Azure, которая является старым процессом развертывания, будет признана устаревшей в марте 2021. Это изменение не повлияет на существующие параметры развертывания в приложении, и вы можете продолжить управление развертыванием приложения на странице **центра развертывания** .

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="configure-deployment-source"></a>Настройка источника развертывания

1. В [портал Azure](https://portal.azure.com)перейдите на страницу управления для приложения службы приложений.

1. В меню слева щелкните Параметры **центра развертывания**  >  . 

1. В поле **источник** выберите один из параметров CI/CD.

    ![В этой статьи показано, как выбрать источник развертывания в центре развертывания для службы приложений Azure.](media/app-service-continuous-deployment/choose-source.png)

Выберите вкладку, соответствующую выбранным действиям.

# <a name="github"></a>[GitHub](#tab/github)

4. [Действия GitHub](#how-the-github-actions-build-provider-works) являются поставщиком сборки по умолчанию. Чтобы изменить его, щелкните **изменить поставщик** службы  >  **сборок службы приложений** (KUDU) > **ОК**.

    > [!NOTE]
    > Чтобы использовать Azure Pipelines в качестве поставщика сборки для приложения службы приложений, не настраивайте его в службе приложений. Вместо этого настройте CI/CD непосредственно из Azure Pipelines. Параметр **Azure pipelines** просто указывает на нужное направление.

1. При первом развертывании из GitHub щелкните **авторизовать** и следуйте запросам авторизации. Если требуется выполнить развертывание из репозитория другого пользователя, щелкните **изменить учетную запись**.

1. После авторизации учетной записи Azure с помощью GitHub выберите **организацию**, **репозиторий** и **ветвь** , чтобы настроить CI/CD для.

1. Если выбран поставщик сборки "действия GitHub", можно выбрать нужный файл рабочего процесса с раскрывающимся списком " **стек среды выполнения** " и " **версия** ". Azure фиксирует этот файл рабочего процесса в выбранном репозитории GitHub для обработки задач сборки и развертывания. Чтобы просмотреть файл перед сохранением изменений, щелкните **Предварительный просмотр файла**.

    > [!NOTE]
    > Служба приложений обнаруживает [параметр языкового стека](configure-common.md#configure-language-stack-settings) приложения и выбирает наиболее подходящий шаблон рабочего процесса. Если выбрать другой шаблон, он может развернуть приложение, которое не работает должным образом. Дополнительные сведения см. [в разделе как работает поставщик сборки действий GitHub](#how-the-github-actions-build-provider-works).

1. Выберите команду **Сохранить**.
   
    Новые фиксации в выбранном репозитории в приложении Службы приложений будут непрерывно развертываться. Фиксации и развертывания можно отвести на вкладке **журналы** .

# <a name="bitbucket"></a>[BitBucket](#tab/bitbucket)

Интеграция BitBucket использует службы сборок службы приложений (KUDU) для автоматизации сборки.

4. При первом развертывании из BitBucket щелкните **авторизовать** и следуйте инструкциям на экране авторизации. Если требуется выполнить развертывание из репозитория другого пользователя, щелкните **изменить учетную запись**.

1. Для BitBucket выберите **группу** BitBucket, **репозиторий** и **ветвь** , которые нужно развернуть непрерывно.

1. Выберите команду **Сохранить**.
   
    Новые фиксации в выбранном репозитории в приложении Службы приложений будут непрерывно развертываться. Фиксации и развертывания можно отвести на вкладке **журналы** .
   
# <a name="local-git"></a>[Локальный репозиторий Git](#tab/local)

См. раздел [Локальное развертывание Git в службе приложений Azure](deploy-local-git.md).

# <a name="azure-repos"></a>[Azure Repos](#tab/repos)

> [!NOTE]
> Azure Repos в качестве источника развертывания — это поддержка приложений Windows.
>

4. Служба сборок службы приложений (KUDU) является поставщиком сборки по умолчанию.

    > [!NOTE]
    > Чтобы использовать Azure Pipelines в качестве поставщика сборки для приложения службы приложений, не настраивайте его в службе приложений. Вместо этого настройте CI/CD непосредственно из Azure Pipelines. Параметр **Azure pipelines** просто указывает на нужное направление.

1. Выберите **организацию Azure DevOps**, **проект**, **репозиторий** и **ветвь** , которые нужно развернуть непрерывно. 

    Если ваша организация DevOps отсутствует в списке, она еще не связана с вашей подпиской Azure. Дополнительные сведения см. в статье [Создание подключения к службе Azure](/azure/devops/pipelines/library/connect-to-azure).

-----

## <a name="disable-continuous-deployment"></a>Отключение непрерывного развертывания

1. В [портал Azure](https://portal.azure.com)перейдите на страницу управления для приложения службы приложений.

1. В меню слева щелкните Параметры **центра развертывания**  >    >  **Отключить**. 

    ![Показано, как отключить синхронизацию облачных папок с приложением службы приложений в портал Azure.](media/app-service-continuous-deployment/disable.png)

1. По умолчанию файл рабочего процесса действия GitHub сохраняется в репозитории, но он по-прежнему будет запускать развертывание в приложении. Чтобы удалить его из репозитория, выберите **удалить файл рабочего процесса**.

1. Нажмите кнопку **OK**.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="how-the-github-actions-build-provider-works"></a>Как работает поставщик сборки действий GitHub

Поставщик сборки действий GitHub является параметром для [CI/CD из GitHub](#configure-deployment-source)и выполняет следующие действия для настройки CI/CD.

- Придепозитайте файл рабочего процесса действий GitHub в репозиторий GitHub для обработки задач сборки и развертывания в службе приложений.
- Добавляет профиль публикации приложения в качестве секрета GitHub. Файл рабочего процесса использует этот секрет для проверки подлинности в службе приложений.
- Захватывает сведения из [журналов выполнения рабочего процесса](https://docs.github.com/actions/managing-workflow-runs/using-workflow-run-logs) и отображает их на вкладке **журналы** в **центре развертывания** приложения.

Настроить поставщик сборки действий GitHub можно следующими способами.

- Настройте файл рабочего процесса после его создания в репозитории GitHub. Дополнительные сведения см. в разделе [синтаксис рабочего процесса для действий GitHub](https://docs.github.com/actions/reference/workflow-syntax-for-github-actions). Просто убедитесь, что рабочий процесс развертывается в службе приложений с помощью действия " [Azure/веб-приложения — развертывание](https://github.com/Azure/webapps-deploy) ".
- Если выбранная ветвь защищена, вы по-прежнему можете просмотреть файл рабочего процесса без сохранения конфигурации, а затем вручную добавить его в репозиторий. Этот метод не обеспечивает интеграцию журналов с портал Azure.
- Вместо профиля публикации разверните его с помощью [субъекта-службы](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) в Azure Active Directory.

#### <a name="authenticate-with-a-service-principal"></a>Проверка подлинности с помощью субъекта-службы

Эта необязательная конфигурация заменяет проверку подлинности по умолчанию профилями публикации в созданном файле рабочего процесса.

1. Создание субъекта-службы с помощью команды [AZ AD SP Create-for-RBAC](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) в [Azure CLI](/cli/azure/). В следующем примере замените *\<subscription-id>* , *\<group-name>* и *\<app-name>* собственными значениями:

    ```azurecli-interactive
    az ad sp create-for-rbac --name "myAppDeployAuth" --role contributor \
                                --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> \
                                --sdk-auth
    ```
    
    > [!IMPORTANT]
    > Для обеспечения безопасности предоставьте минимально необходимый доступ к субъекту-службе. Область в предыдущем примере ограничена конкретным приложением службы приложений, а не всей группой ресурсов.
    
1. Сохраните все выходные данные JSON для следующего шага, включая верхний уровень `{}` .

1. В [GitHub](https://github.com/)найдите репозиторий, выберите **параметры > секреты > добавить новый секрет**.

1. Вставьте все выходные данные JSON, полученные из команды Azure CLI, в поле значения секрета. Присвойте секрету имя, например `AZURE_CREDENTIALS` .

1. В файле рабочего процесса, созданном в **центре развертывания**, измените `azure/webapps-deploy` Шаг с помощью кода, как в следующем примере (который изменяется из файла рабочего процесса Node.js):

    ```yaml
    - name: Sign in to Azure 
    # Use the GitHub secret you added
    - uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Deploy to Azure Web App
    # Remove publish-profile
    - uses: azure/webapps-deploy@v2
      with:
        app-name: '<app-name>'
        slot-name: 'production'
        package: .
    - name: Sign out of Azure
      run: |
        az logout
    ```
    
## <a name="deploy-from-other-repositories"></a>Развертывание из других репозиториев

Для приложений Windows можно вручную настроить непрерывное развертывание из облачного репозитория Git или Mercurial, который не поддерживает напрямую портал, например [GitLab](https://gitlab.com/). Для этого выберите внешний Git в раскрывающемся списке **источник** . Дополнительные сведения см. в [статье Настройка непрерывного развертывания с помощью ручных действий](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="more-resources"></a>Дополнительные ресурсы

* [Развертывание из Azure Pipelines в службы приложений Azure](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps)
* [Исследование распространенных проблем с непрерывным развертыванием](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Использование Azure PowerShell](/powershell/azure/)
* [Проект Kudu](https://github.com/projectkudu/kudu/wiki)
