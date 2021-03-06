---
title: Устранение неполадок в Live Video Analytics на IoT Edge в Azure
description: В этой статье рассматриваются действия по устранению неполадок в службе Live Video Analytics на IoT Edge.
author: IngridAtMicrosoft
ms.topic: how-to
ms.author: inhenkel
ms.date: 12/04/2020
ms.openlocfilehash: d766843f58bc2cdd0dcdddfad337b23fefb28768
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "101698745"
---
# <a name="troubleshoot-live-video-analytics-on-iot-edge"></a>Устранение неполадок в Live Video Analytics на IoT Edge

В этой статье рассматриваются действия по устранению неполадок в реальном видео-аналитике (лва) на Azure IoT Edge.

## <a name="troubleshoot-deployment-issues"></a>Устранение неполадок развертывания

### <a name="diagnostics"></a>Диагностика

В рамках развертывания Live Video Analytics вы настраиваете ресурсы Azure, такие как центр Интернета вещей и устройства IoT Edge. В качестве первого шага для диагностики проблем всегда убедитесь, что Пограничное устройство правильно настроено, выполнив следующие инструкции:

1. [Выполните `check` команду](../../iot-edge/troubleshoot.md#run-the-check-command).
1. [Проверьте версию IOT Edge](../../iot-edge/troubleshoot.md#check-your-iot-edge-version).
1. [Проверьте состояние диспетчера безопасности IOT EDGE и его журналов](../../iot-edge/troubleshoot.md#check-the-status-of-the-iot-edge-security-manager-and-its-logs).
1. [Просмотр сообщений, передаваемых через центр IOT Edge](../../iot-edge/troubleshoot.md#view-the-messages-going-through-the-iot-edge-hub).
1. [Перезапустите контейнеры](../../iot-edge/troubleshoot.md#restart-containers).
1. [Проверьте правила конфигурации брандмауэра и порта](../../iot-edge/troubleshoot.md#check-your-firewall-and-port-configuration-rules).

### <a name="pre-deployment-issues"></a>Проблемы перед развертыванием

Если инфраструктура пограничной инфраструктуры подойдет, можно найти проблемы с файлом манифеста развертывания. Чтобы развернуть модуль Live Video Analytics в модуле IoT Edge на IoT Edge устройстве вместе с любыми другими модулями Интернета вещей, используйте манифест развертывания, который содержит центр IoT Edge, IoT Edge агент и другие модули и их свойства. Для развертывания файла манифеста можно использовать следующую команду:

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content <path-to-deployment_manifest.json>
```
Если код JSON сформирован неправильно, может появиться следующее сообщение об ошибке:   
&nbsp;&nbsp;&nbsp;**Не удалось выполнить синтаксический анализ JSON из файла: " <deployment manifest.json> " для аргумента "Content" с исключением: "дополнительные данные: строка 101, столбец 1 (char 5325)"**

При возникновении этой ошибки рекомендуется проверить JSON на отсутствие квадратных скобок или другие проблемы со структурой файла. Чтобы проверить структуру файла, можно использовать клиент, например ["Блокнот" + + с подключаемым модулем средства просмотра JSON](https://riptutorial.com/notepadplusplus/example/18201/json-viewer) или онлайн-инструмент, например [модуль форматирования JSON & проверяющий элемент управления](https://jsonformatter.curiousconcept.com/).

### <a name="during-deployment-diagnose-with-media-graph-direct-methods"></a>Во время развертывания: диагностика с помощью прямых методов Media Graph 

После того как модуль Live Video Analytics в модуле IoT Edge правильно развернут на устройстве IoT Edge, можно создать и запустить граф мультимедиа, вызвав [прямые методы](direct-methods.md).  
>[!NOTE]
>  Вызовы прямых методов следует вносить **`lvaEdge`** только в модуль.

Вы можете использовать портал Azure для выполнения диагностики графа мультимедиа с помощью прямых методов:

1. В портал Azure перейдите в центр Интернета вещей, подключенный к устройству IoT Edge.

1. Найдите **Автоматическое управление устройствами**, а затем выберите **IOT Edge**.  

1. В списке пограничных устройств выберите устройство, которое требуется диагностировать.  
         
    ![Снимок экрана портал Azure отображения списка пограничных устройств](./media/troubleshoot-how-to/lva-sample-device.png)


1. Проверьте, имеет ли код ответа значение *200-ОК*. Ниже приведены другие коды ответов для [среды выполнения IOT Edge](../../iot-edge/iot-edge-runtime.md) .
    * 400 — конфигурация развертывания имеет неправильный формат или недопустима.
    * 417-у устройства нет набора конфигураций развертывания.
    * 412 — версия схемы в конфигурации развертывания недопустима.
    * 406 — устройство IoT Edge работает в автономном режиме или не отправляет отчеты о состоянии.
    * 500 — в среде выполнения IoT Edge произошла ошибка.

    > [!TIP]
    > При возникновении проблем с Azure IoT Edgeными модулями в среде используйте **[Azure IOT Edge Стандартные диагностические действия](../../iot-edge/troubleshoot.md?preserve-view=true&view=iotedge-2018-06)** в качестве руководств по устранению неполадок и диагностике.
### <a name="post-deployment-direct-method-error-code"></a>После развертывания: код ошибки прямого метода
1. Если вы получаете состояние `501 code` , убедитесь, что имя прямого метода является точным. Если имя метода и полезные данные запроса являются точными, необходимо получить результаты вместе с кодом успешного выполнения = 200. 
1. Если полезные данные запроса неточны, вы получите состояние `400 code` и полезные данные ответа, которые указывают код ошибки и сообщение, которое должно помочь при диагностике проблемы с помощью вызова прямого метода.
    * Проверка сообщаемых и требуемых свойств поможет понять, синхронизированы ли свойства модуля с развертыванием. Если это не так, можно перезапустить устройство IoT Edge. 
    * Воспользуйтесь руководством по [прямым методам](direct-methods.md) , чтобы вызвать несколько методов, особенно простых, например графтопологилист. Кроме того, в этом руководством указываются ожидаемые полезные данные запроса и ответа, а также коды ошибок. После успешного выполнения простых прямых методов можно быть уверенным в том, что модуль Live Video Analytics IoT Edge работает нормально.
        
       ![Снимок экрана: панель "прямой метод" для модуля IoT Edge.](./media/troubleshoot-how-to/direct-method.png) 

1. Если для столбцов, **указанных в** параметре развертывание и **сообщаемые по** столбцам устройства, указано *значение Да*, можно вызвать прямые методы в реальном видео Analytics для IOT Edge модуля. Выберите модуль, чтобы перейти на страницу, где можно проверить требуемые и сообщаемые свойства и вызвать прямые методы. Необходимо учитывать следующее: 

### <a name="post-deployment-diagnose-logs-for-issues-during-the-run"></a>После развертывания: Диагностика журналов на наличие проблем во время выполнения 

Журналы контейнера для модуля IoT Edge должны содержать диагностические сведения, помогающие отладить проблемы во время выполнения модуля. Вы можете [проверить журналы контейнера на наличие проблем](../../iot-edge/troubleshoot.md#check-container-logs-for-issues) и самостоятельно диагностировать проблему. 

Если вы выполнили все предыдущие проверки и все еще столкнулись с проблемами, соберите журналы с IoT Edge устройства [с помощью `support bundle` команды](../../iot-edge/troubleshoot.md#gather-debug-information-with-support-bundle-command) для дальнейшего анализа командой Azure. Вы можете [связаться с нами](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) для получения поддержки и отправки собранных журналов.

## <a name="common-error-resolutions"></a>Распространенные способы устранения ошибок

Функция Live Video Analytics развертывается как модуль IoT Edge на устройстве IoT Edge и работает совместно с IoT Edge агентом и модулями концентратора. Некоторые распространенные ошибки, возникающие при развертывании в реальном времени, вызываются проблемами с базовой инфраструктурой IoT. К ошибкам относятся:

* [Агент IOT Edge останавливается через минуту](../../iot-edge/troubleshoot-common-errors.md#iot-edge-agent-stops-after-about-a-minute).
* [Агент IOT EDGE не может получить доступ к образу модуля (403)](../../iot-edge/troubleshoot-common-errors.md#iot-edge-agent-cant-access-a-modules-image-403).
* [Модуль агента IOT Edge сообщает об пустом файле конфигурации и не запускает модули на устройстве](../../iot-edge/troubleshoot-common-errors.md#edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device).
* [Не удается запустить центр IOT Edge](../../iot-edge/troubleshoot-common-errors.md#iot-edge-hub-fails-to-start).
* [Сбой управляющей программы IOT Edge безопасности с недопустимым именем узла](../../iot-edge/troubleshoot-common-errors.md#iot-edge-security-daemon-fails-with-an-invalid-hostname).
* [Интерактивная аналитика видео или любой другой настраиваемый модуль IOT EDGE не может отправить сообщение в концентратор ребра с ошибкой 404](../../iot-edge/troubleshoot-common-errors.md#iot-edge-module-fails-to-send-a-message-to-edgehub-with-404-error).
* [Модуль IOT Edge развертывается успешно, а затем исчезает с устройства](../../iot-edge/troubleshoot-common-errors.md#iot-edge-module-deploys-successfully-then-disappears-from-device).

    > [!TIP]
    > При возникновении проблем с Azure IoT Edgeными модулями в среде используйте **[Azure IOT Edge Стандартные диагностические действия](../../iot-edge/troubleshoot.md?preserve-view=true&view=iotedge-2018-06)** в качестве руководств по устранению неполадок и диагностике.

При запуске **[скрипта настройки ресурсов Live Video Analytics](https://github.com/Azure/live-video-analytics/tree/master/edge/setup)** также могут возникать проблемы. Ниже перечислены некоторые распространенные проблемы.

* Использование подписки, в которой отсутствуют права владельца. Это приведет к сбою сценария с ошибкой **форбидденеррор** или **AuthorizationFailed** .
    * Чтобы устранить эту ошибку, убедитесь, что у вас есть права **владельца** для подписки, которую планируется использовать. Если вы не можете сделать это самостоятельно, обратитесь к администратору подписки, чтобы предоставить правильные права доступа.
* **Не удалось выполнить развертывание шаблона из-за нарушения политики.**
    * Чтобы устранить эту проблему, обратитесь к ИТ-администратору, чтобы убедиться, что в вызовах для создания виртуальной машины обойти блокирование проверки подлинности SSH. Это не потребуется, так как мы используем защищенную сеть бастиона, для взаимодействия с ресурсами Azure требуется имя пользователя и пароль. Эти учетные данные будут храниться в файле **~/клауддриве/лва-сампле/vm-edge-device-credentials.txt** в Cloud Shell, после того как виртуальная машина будет успешно создана, развернута и подключена к центру Интернета вещей.
* Скрипт установки не может создать субъект-службу или ресурсы Azure.
    * Чтобы устранить эту ошибку, убедитесь, что ваша подписка и клиент Azure не достигли максимального предела службы. Дополнительные сведения об ограничениях и ограничениях [службы Azure AD](../../active-directory/enterprise-users/directory-service-limits-restrictions.md) , а так: [ограничениях, квотах и ограничениях для подписок Azure и служб.](../../azure-resource-manager/management/azure-subscription-service-limits.md)

> [!TIP]
> Если имеются дополнительные проблемы, с которыми может потребоваться помощь, **[собирайте журналы и отправьте запрос в службу поддержки](#collect-logs-for-submitting-a-support-ticket)**. Вы также можете обратиться к нам, отправив нам электронное письмо по адресу **[amshelp@microsoft.com](mailto:amshelp@microsoft.com)** .
### <a name="live-video-analytics-working-with-external-modules"></a>Интерактивная аналитика видео работает с внешними модулями

Интерактивная аналитика видео с помощью обработчиков расширения Media Graph может расширить граф мультимедиа для отправки и получения данных из других модулей IoT Edge с помощью протоколов HTTP или gRPC. В качестве [конкретного примера](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/httpExtension)этот граф мультимедиа может отправлять видеоматериалы в виде изображений во внешний модуль вывода, например Йоло v3, и получать результаты анализа на основе JSON с помощью протокола HTTP. В такой топологии назначением для событий является преимущественно центр Интернета вещей. В ситуациях, когда вы не видите события вывода в концентраторе, проверьте следующее:

* Убедитесь, что в концентраторе, на котором выполняется публикация медиа-графика, и что проверяемый центр является одним и тем же. При создании нескольких развертываний вы можете получить несколько центров и по ошибке проверить неправильный центр для событий.
* В портал Azure проверьте, развернут и выполняется ли внешний модуль. В примере изображения ртспсим, yolov3, tinyyolov3 и Логаналитиксажент являются IoT Edge модулями, работающими извне модуля Лваедже.

    [![Снимок экрана, на котором отображается состояние выполнения модулей в центре Интернета вещей Azure. ](./media/troubleshoot-how-to/iot-hub-azure.png)](./media/troubleshoot-how-to/iot-hub-azure.png#lightbox)

* Проверьте, отправляются ли события в правильную конечную точку URL-адреса. Внешний контейнер AI предоставляет URL-адрес и порт, через который он получает и возвращает данные из запросов POST. Этот URL-адрес указан как `endpoint: url` свойство для обработчика РАСШИРЕНИЙ HTTP. Как видно в [URL-адресе топологии](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/httpExtension/2.0/topology.json), конечной точке присваивается параметр URL-адреса. Убедитесь, что значение по умолчанию для параметра или переданное значение является точным. Можно проверить, работает ли он с помощью URL-адреса клиента (в виде фигур).  

    Например, ниже приведен контейнер Йоло v3, который работает на локальном компьютере с IP-адресом 172.17.0.3.  
    
    ```
    curl -X POST http://172.17.0.3/score -H "Content-Type: image/jpeg" --data-binary @<fullpath to jpg>
    ```

    Возвращенный результат:

    ```
    {"inferences": [{"type": "entity", "entity": {"tag": {"value": "car", "confidence": 0.8668569922447205}, "box": {"l": 0.3853073438008626, "t": 0.6063712999658677, "w": 0.04174524943033854, "h": 0.02989496027381675}}}]}
    ```
    > [!TIP]
    > Используйте **[команду DOCKER проверки](https://docs.docker.com/engine/reference/commandline/inspect/)** , чтобы найти IP-адрес компьютера.
    
* Если вы используете один или несколько экземпляров графа, который использует обработчик расширения мультимедиа Graph, следует использовать это `samplingOptions` поле для управления скоростью кадров в секунду (кадров/с) в канале видео. 

   * В некоторых ситуациях, где ЦП или память пограничных компьютеров сильно загружены, можно потерять определенные события вывода. Чтобы устранить эту ошибку, установите низкое значение для `maximumSamplesPerSecond` свойства в `samplingOptions` поле. Вы можете задать для него значение 0,5 ("Максимумсамплесперсеконд": "0,5") на каждом экземпляре графа, а затем повторно запустить экземпляр, чтобы проверить наличие событий вывода в концентраторе.
    
### <a name="multiple-direct-methods-in-parallel--timeout-failure"></a>Несколько прямых методов в Parallel — сбой времени ожидания 

Интерактивная аналитика видео на IoT Edge предоставляет прямую модель программирования на основе методов, которая позволяет настроить несколько топологий и несколько экземпляров графов. В рамках настройки топологии и графа вы вызываете несколько прямых вызовов методов для модуля IoT Edge. При одновременном вызове этих нескольких вызовов методов, особенно тех, которые запускают и останавливают графы, может произойти сбой времени ожидания, как показано ниже: 

Метод инициализации сборки Microsoft.Media.LiveVideoAnalytics.Test.Feature.Edge.AssemblyInitializer.IniТиализеассемблясинк вызвал исключение. Microsoft. Azure. Devices. Common. Exceptions. Иосубексцептион: Microsoft. Azure. Devices. Common. Exceptions. Иосубексцептион:<br/> `{"Message":"{\"errorCode\":504101,\"trackingId\":\"55b1d7845498428593c2738d94442607-G:32-TimeStamp:05/15/2020 20:43:10-G:10-TimeStamp:05/15/2020 20:43:10\",\"message\":\"Timed out waiting for the response from device.\",\"info\":{},\"timestampUtc\":\"2020-05-15T20:43:10.3899553Z\"}","ExceptionMessage":""}. Aborting test execution. `

*Не* рекомендуется вызывать прямые методы параллельно. Вызывайте их последовательно (т. е. Сделайте один прямой вызов метода только после завершения предыдущего).

### <a name="collect-logs-for-submitting-a-support-ticket"></a>Собирайте журналы для отправки запроса в службу поддержки

Если шаги самостоятельного устранения неполадок не помогли устранить проблему, перейдите на портал Azure и [откройте запрос в службу поддержки](../../azure-portal/supportability/how-to-create-azure-support-request.md).

> [!WARNING]
> Журналы могут содержать личные сведения (PII), такие как IP-адрес. Все локальные копии журналов будут удалены сразу после завершения анализа и закрытия запроса в службу поддержки.  

Чтобы собрать соответствующие журналы, которые необходимо добавить к билету, выполните приведенные ниже инструкции по порядку и отправьте файлы журнала в области **сведений** запроса на поддержку.  
1. [Настройка модуля Live Video Analytics для накопления подробных журналов](#configure-live-video-analytics-module-to-collect-verbose-logs)
1. [Включить журналы отладки](#live-video-analytics-debug-logs)
1. Воспроизведите проблему.
1. Подключение к виртуальной машине с помощью страницы **центра Интернета вещей** на портале
    1. Заархивировать все файлы в папке *дебуглогс*

       > [!NOTE]
       > Эти файлы журналов не предназначены для самостоятельной диагностики. Они предназначены для анализа ваших проблем командой инженеров Azure.

       * В следующей команде обязательно замените **$DEBUG _LOG_LOCATION_ON_EDGE_DEVICE** расположением журналов отладки на пограничном устройстве, настроенном ранее на **шаге 2**.  

           ```
           sudo apt install zip unzip  
           zip -r debugLogs.zip $DEBUG_LOG_LOCATION_ON_EDGE_DEVICE 
           ```

    1. Прикрепите файл *debugLogs.zip* к запросу в службу поддержки.
1. Выполните [команду поддержки пакета](#use-the-support-bundle-command), собирайте журналы и вложите их в службу поддержки.

### <a name="configure-live-video-analytics-module-to-collect-verbose-logs"></a>Настройка модуля Live Video Analytics для накопления подробных журналов
Настройте модуль Live Video Analytics для получения подробных журналов, задав `logLevel` и следующим образом `logCategories` :
```
"logLevel": "Verbose",
"logCategories": "Application,Events,MediaPipeline",
```

Это можно сделать одним из следующих:
* В **портал Azure**, обновив свойства двойника удостоверения модуля модуля Live Video Analytics, настройте   [ ![ идентификатор ](media/troubleshoot-how-to/module-twin.png) двойника.](media/troubleshoot-how-to/module-twin.png#lightbox)    
* В файле **манифеста развертывания** можно добавить эти записи в узел Свойства модуля Live Video Analytics.

### <a name="use-the-support-bundle-command"></a>Использование команды support-пучок

Если необходимо собрать журналы с IoT Edge устройства, проще всего использовать `support-bundle` команду. Эта команда собирает:

- Журналы модулей
- IoT Edge журналов диспетчера безопасности и модуля контейнера
- IoT Edge проверить выходные данные JSON
- Полезные отладочные сведения

1. Выполните `support-bundle` команду с флагом *--с* , чтобы указать, сколько времени следует охватывать журналы. Например, 2H получит журналы за последние два часа. Можно изменить значение этого флага, чтобы включить журналы для различных периодов.

    ```
    sudo iotedge support-bundle --since 2h
    ```

   Эта команда создает файл с именем *support_bundle.zip* в каталоге, в котором была выполнена команда. 
   
1. Прикрепите файл *support_bundle.zip* к запросу в службу поддержки.

### <a name="live-video-analytics-debug-logs"></a>Журналы отладки для анализа видео в реальном времени

Чтобы настроить интерактивную аналитику видео в модуле IoT Edge для создания журналов отладки, выполните следующие действия.

1. Войдите в [портал Azure](https://portal.azure.com)и перейдите в центр Интернета вещей.
1. На левой панели выберите **IOT Edge**.
1. В списке устройств выберите идентификатор целевого устройства.
1. В верхней части панели выберите **задать модули**.

   ![Снимок экрана кнопки "задать модули" в портал Azure.](media/troubleshoot-how-to/set-modules.png)

1. В разделе **IOT Edge modules** найдите и выберите **лваедже**.
1. Выберите **Параметры создания контейнера**.
1. В разделе **привязки** добавьте следующую команду:

    `/var/local/mediaservices/logs:/var/lib/azuremediaservices/logs`

    > [!NOTE] 
    > Эта команда привязывает папки журналов между граничным устройством и контейнером. Чтобы сохранить журналы в другом расположении, используйте следующую команду, заменив **$LOG _LOCATION_ON_EDGE_DEVICE** на расположение, которое вы хотите использовать. `/var/$LOG_LOCATION_ON_EDGE_DEVICE:/var/lib/azuremediaservices/logs`

1. Нажмите кнопку **Обновить**.
1. Выберите **Review + Create** (Просмотреть и создать). Сообщение об успешной проверке публикуется под зеленым баннером.
1. Нажмите кнопку **создания**.
1. Обновите **двойника удостоверений модуля** , чтобы он указывал на параметр дебуглогсдиректори, указывающий на каталог, в котором собираются журналы.

    а. В таблице **модули** выберите **лваедже**.  
    b. В верхней части панели выберите **модуль удостоверение двойника**. Откроется Редактируемая панель.  
    c. В разделе **требуемый ключ** добавьте следующую пару "ключ-значение":  
    `"DebugLogsDirectory": "/var/lib/azuremediaservices/logs"`

    > [!NOTE] 
    > Эта команда привязывает папки журналов между граничным устройством и контейнером. Если вы хотите получить журналы в другом расположении на устройстве, сделайте следующее:
    > 1. Создайте привязку для расположения журнала отладки в разделе " **привязки** ", заменив **_LOG_LOCATION_ON_EDGE_DEVICE $DEBUG** и **$Debug _LOG_LOCATION** на нужное расположение: `/var/$DEBUG_LOG_LOCATION_ON_EDGE_DEVICE:/var/$DEBUG_LOG_LOCATION`
    > 2. Используйте следующую команду, заменив **$DEBUG _LOG_LOCATION** на расположение, используемое на предыдущем шаге:  
    > `"DebugLogsDirectory": "/var/$DEBUG_LOG_LOCATION"`  
    
    d. Щелкните **Сохранить**.


1. Вы можете отключить сбор журналов, установив значение в поле **идентификатор модуля двойника** в значение *null*. Вернитесь на страницу **удостоверения модуля двойника** и измените следующий параметр на:

    `"DebugLogsDirectory": ""`

### <a name="best-practices-around-logging"></a>Рекомендации по ведению журнала

[Мониторинг и ведение журнала](monitoring-logging.md) должны помочь в понимании таксономии и создании журналов, которые помогут в отладке проблем с лва. 

По мере того как реализация сервера gRPC различается на разных языках, не существует стандартного способа добавления ведения журнала внутри сервера.  

Например, при создании сервера gRPC с помощью .NET Core служба gRPC добавляет журналы в категорию **gRPC** . Чтобы включить подробные журналы из gRPC, настройте префиксы GRPC на уровне отладки в appsettings.jsдля файла, добавив следующие элементы в подраздел LogLevel в разделе Ведение журнала: 

```
{ 
  "Logging": { 
    "LogLevel": { 
      "Default": "Debug", 
      "System": "Information", 
      "Microsoft": "Information", 
      "Grpc": "Debug" 
       } 
  } 
} 
``` 

Это также можно настроить в файле startup. cs с помощью Конфигурелоггинг: 

```
public static IHostBuilder CreateHostBuilder(string[] args) => 
    Host.CreateDefaultBuilder(args) 
        .ConfigureLogging(logging => 
        { 

           logging.AddFilter("Grpc", LogLevel.Debug); 
        }) 
        .ConfigureWebHostDefaults(webBuilder => 
        { 
            webBuilder.UseStartup<Startup>(); 
        }); 

``` 

[Ведение журналов и диагностики в gRPC на платформе .NET](/aspnet/core/grpc/diagnostics?preserve-view=true&view=aspnetcore-3.1) предоставляет некоторые рекомендации по сбору журналов диагностики с сервера gRPC. 

### <a name="a-failed-grpc-connection"></a>Неудачное подключение gRPC 

Если граф активен и потоковая передача с камеры, подключение будет поддерживаться функцией Live Video Analytics. 

### <a name="monitoring-and-balancing-the-load-of-cpu-and-gpu-resources-when-these-resources-become-bottlenecks"></a>Мониторинг и балансировка нагрузки ресурсов ЦП и GPU, когда эти ресурсы становятся узкими местами

Интерактивная аналитика видео не отслеживает и не обеспечивает мониторинг аппаратных ресурсов. Разработчики должны будут использовать решения для мониторинга изготовителей оборудования. Однако если вы используете контейнеры Kubernetes, вы можете отслеживать устройство с помощью [панели мониторинга Kubernetes](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). 

gRPC в документах .NET Core также имеют полезную информацию о [рекомендациях по повышению производительности](/aspnet/core/grpc/performance?preserve-view=true&view=aspnetcore-3.1) и [балансировке нагрузки](/aspnet/core/grpc/performance?preserve-view=true&view=aspnetcore-3.1#load-balancing).  

### <a name="troubleshooting-an-inference-server-when-it-does-not-receive-any-frames-and-you-are-receiving-an-unknown-protocol-error"></a>Устранение неполадок сервера вывода, когда он не получает никаких кадров и вы получаете сообщение об ошибке "Неизвестный" протокол 

Существует несколько вещей, которые можно выполнить для получения дополнительных сведений о проблеме.  

* Включите категорию журнала **едиапипелине** в нужные свойства модуля Live Video Analytics и убедитесь, что для уровня ведения журнала задано значение `Information` .  
* Чтобы проверить сетевое подключение, можно выполнить следующую команду на пограничном устройстве. 

   ```
   sudo docker exec lvaEdge /bin/bash -c “apt update; apt install -y telnet; telnet <inference-host> <inference-port>” 
   ```

   Если команда выводит короткую строку текста жумблед, программа Telnet успешно смогла открыть подключение к серверу вывода и открыть двоичный канал gRPC. Если вы не видите это, Telnet сообщит о сетевой ошибке. 
* На сервере вывода можно включить дополнительное ведение журнала в библиотеке gRPC. Это может дать дополнительные сведения о самом канале gRPC. Это зависит от языка и инструкций для [C#](/aspnet/core/grpc/diagnostics?preserve-view=true&view=aspnetcore-3.1). 

### <a name="picking-more-images-from-buffer-of-grpc-without-sending-back-result-for-first-buffer"></a>Выбор дополнительных изображений из буфера gRPC без отправки результата для первого буфера

В рамках контракта на передачу данных gRPC все сообщения, отправляемые в ходе работы службы Video Analytics на сервер, на который производится запись в gRPC, должны быть подтверждены. Отсутствие подтверждения получения кадра изображения приводит к прерыванию контракта данных и может привести к нежелательным ситуациям.  

Чтобы использовать сервер gRPC с помощью функции Live Video Analytics, можно использовать общую память для лучшей производительности. Для этого необходимо использовать возможности общей памяти Linux, предоставляемые языком программирования или средой. 

1. Откройте маркер общей памяти Linux.
1. При получении кадра получите доступ к смещению адреса в общей памяти.
1. Подтвердите завершение обработки кадра, чтобы его память можно было освободить в службе Live Video Analytics.

   > [!NOTE]
   > Если вы задерживаете подтверждение поступления кадра в Live Video Analytics в течение длительного времени, это может привести к переполнению общей памяти, что приведет к падению данных.
1. Сохраняйте каждый кадр в выбранной структуре данных (список, массив и т. д.) на сервере.
1. После этого можно запустить логику обработки, когда у вас будет необходимое количество кадров изображения.
1. Возвращайте возвращаемый результат обратно в Live Video Analytics при готовности.

## <a name="next-steps"></a>Дальнейшие действия

[Руководство по Запись видео в облако и его воспроизведение оттуда на основе событий](event-based-video-recording-tutorial.md)