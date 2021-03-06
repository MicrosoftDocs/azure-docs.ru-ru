---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2020
ms.author: erhopf
ms.openlocfilehash: 6d20df031633df4642ce9fb5cbbc469fd7f0a5da
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96188503"
---
Чтобы создать проект Visual Studio для кросс-платформенной разработки .NET для мобильных приложений с помощью Xamarin, вам потребуется:
- Настройка параметров разработки Visual Studio.
- Создание проекта и выбор целевой архитектуры. 
- Установка пакета SDK службы "Речь".

### <a name="set-up-visual-studio-development-options"></a>Настройка параметров разработки Visual Studio

Для начала убедитесь, что вы правильно настроили Visual Studio для кросс-платформенной разработки мобильных приложений с помощью .NET:

1. Откройте Visual Studio 2019.

1. В строке меню Visual Studio выберите **Инструменты** > **Get Tools and Features** (Получить инструменты и компоненты), чтобы открыть Visual Studio Installer и просмотреть диалоговое окно **Идет изменение**.

   ![Снимок экрана: вкладка Workloads (Рабочие нагрузки)с диалоговым окном изменения и Visual Studio Installer](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-workload.png)

1. На вкладке **Загрузки** в **Windows** найдите **разработку мобильных приложений с помощью рабочей нагрузки .NET**. Если флажок рядом с этой рабочей нагрузкой уже выбран, закройте диалоговое окно **Идет изменение** и перейдите к шагу 5.

1. Установите флажок **Разработка мобильных приложений на .NET** и выберите **Изменить**. В диалоговом окне **Перед началом работы** выберите пункт **Продолжить**, чтобы установить рабочую нагрузку разработки мобильных приложений с помощью .NET. Установка нового компонента может занять некоторое время.

1. Закройте Visual Studio Installer.

### <a name="create-the-project"></a>Создание проекта

1. В строке меню Visual Studio выберите **Файл** > **Создать** > **Проект**, чтобы открыть окно **Создание проекта**.

   ![Снимок экрана: создание проекта в Visual Studio.](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-create-new-project.png)

1. Найдите и выберите **Мобильное приложение (Xamarin.Forms)** .

1. Выберите **Далее**, чтобы отобразить экран **Настроить новый проект**.

   ![Снимок экрана: настройка нового проекта в Visual Studio.](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-configure-your-new-project.png)

1. В поле **Имя проекта** введите *helloworld*.

1. В поле **Расположение** перейдите и выберите или создайте папку для сохранения проекта.

1. Выберите **Создать**, чтобы открыть окно **New Mobile App Xamarin Forms Project** (Новый проект мобильного приложения форм Xamarin).

   ![Снимок экрана: диалоговое окно нового проекта для мобильного приложения Xamarin.Forms в Visual Studio.](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-new-xamarin-project.png)

1. Выберите **пустой** шаблон.

1. В разделе **Платформа** установите флажки **Android**, **iOS** и **Windows (UWP)** .

1. Щелкните **ОК**. Вы возвращаетесь в интегрированную среду разработки Visual Studio. Проект создан и отображается в области **Обозреватель решений**.

   ![Проект helloworld Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-helloworld.png)

Теперь выберите архитектуру целевой платформы и запустите проект. На панели инструментов Visual Studio найдите раскрывающийся список **Платформы решения**. (Если вы не видите его, выберите **Просмотр** > **Панели инструментов** > **Стандартная**, чтобы отобразить панель инструментов с элементом **Платформы решения**.) Если вы используете 64-битную версию Windows, выберите **x64** в раскрывающемся списке. При необходимости можно выбрать **x86**, так как 64-битная версия Windows также может запускать 32-битные приложения. В раскрывающемся списке **Start-up Projects** (Проекты запуска) выберите **helloworld.UWP (универсальные приложения для Windows)** .

### <a name="install-the-speech-sdk"></a>Установка пакета SDK службы "Речь"

Установите [пакет NuGet SDK службы "Речь"](https://aka.ms/csspeech/nuget) и сделайте на него ссылку в своем проекте.

1. Щелкните правой кнопкой мыши файл решения в **обозревателе решений**. Выберите **Manage NuGet Packages for Solution** (Управление пакетами NuGet для решения), чтобы открыть окно **NuGet — Solution** (NuGet — решение).

1. Нажмите кнопку **Обзор**.

   ![Снимок экрана: диалоговое окно "Управление пакетами для решения" при установке пакета SDK службы "Речь".](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-nuget-solution-browse.png)

1. В списке **Источник пакета** выберите nuget.org.

1. В поле **поиска** введите *Microsoft.CognitiveServices.Speech*. Затем выберите этот пакет после появления в результатах поиска.

   ![Снимок экрана: выделенный пакет Microsoft.CognitiveServices.Speech.](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-nuget-install.png)

   > [!NOTE] 
   > В библиотеке iOS в NuGet `Microsoft.CognitiveServices.Speech` не включен bitcode. Если для приложения требуется включить библиотеку bitcode, используйте NuGet `Microsoft.CognitiveServices.Speech.Xamarin.iOS` для проекта iOS.

1. В области состояния пакета рядом с результатами поиска выберите все проекты: **helloworld**, **helloworld.Android**, **helloworld.iOS** и **helloworld.UWP**.

1. Выберите пункт **Установить**.

1. В диалоговом окне **Просмотр изменений** нажмите **ОК**.

1. В диалоговом окне **Принятие условий лицензионного соглашения** просмотрите лицензию, а затем выберите **Я принимаю**. Установите ссылку на пакет SDK службы "Речь" для всех проектов. После успешного завершения установки может отобразиться следующее предупреждение для helloworld.iOS. Это известная проблема, которая не должна повлиять на функциональность приложения.

   > Не удалось разрешить ссылку "C:\Users\Default\.nuget\packages\microsoft.cognitiveservices.speech\1.7.0\build\Xamarin.iOS\libMicrosoft.CognitiveServices.Speech.core.a". Если эта ссылка требуется для кода, могут возникнуть ошибки при компиляции.
