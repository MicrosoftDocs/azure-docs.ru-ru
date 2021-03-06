---
title: Включить файл
description: Включить файл
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 4fdb891d668d99644d8a9ed9c15d158e65d53ba5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "72793125"
---
Защита инфраструктуры "Интернета вещей" (IoT) требует тщательно продуманной стратегии с точки зрения безопасности. Согласно этой стратегии необходимо защищать данные в облаке, обеспечивать целостность данных в процессе передачи через общедоступный Интернет и безопасно подготавливать устройства. Каждый следующий уровень обеспечивает большие гарантии безопасности во всей инфраструктуре.

## <a name="secure-an-iot-infrastructure"></a>Защита инфраструктуры IoT

Эту тщательно продуманную стратегию безопасности можно разработать и реализовать при условии активного участия разных сотрудников, занимающихся производством, разработкой и развертыванием устройств и инфраструктуры IoT. Эти сотрудники описаны ниже.

* **Изготовитель или интегратор оборудования Интернета вещей.** Как правило, это изготовители развертываемого оборудования Интернета вещей, интеграторы оборудования, которые собирают оборудование различных изготовителей, или поставщики оборудования, которые предоставляют оборудование для развертывания Интернета вещей, изготовленное или интегрированное другими поставщиками.

* **Разработчик решений Интернета вещей.** Разработка решения Интернета вещей обычно осуществляется разработчиком решений. Этот разработчик может входить в команду внутри компании или системный интегратор, специализирующийся на данной деятельности. Разработчик решений Интернета вещей может с нуля разработать различные компоненты решения Интернета вещей, интегрировать разные готовые компоненты или компоненты с открытым кодом и внедрить акселераторы решений, немного изменив их.

* **Специалист по развертыванию решений Интернета вещей.** Разработанное решение Интернета вещей следует развернуть в рабочей среде. При этом развертывается оборудование, налаживается взаимодействие устройств и развертываются решения на аппаратных устройствах или в облаке.

* **Оператор решений Интернета вещей.** После развертывания используемое решение Интернета вещей требует долгосрочного мониторинга, установки обновлений и обслуживания. Эти задачи может выполнять команда внутри компании, в которую входят специалисты по информационным технологиям, группы по эксплуатации и обслуживанию оборудования, а также специалисты в определенной области, которые следят за работой всей инфраструктуры IoT.

В разделах ниже представлены рекомендации по разработке, развертыванию и эксплуатации безопасной инфраструктуры IoT для каждого из этих сотрудников.

## <a name="iot-hardware-manufacturerintegrator"></a>Производитель или интегратор оборудования IoT

Ниже приведены рекомендации для изготовителей оборудования IoT и интеграторов оборудования.

* **Соблюдение минимальных требований к оборудованию.** Оборудование должно содержать минимум компонентов, необходимых для его работы, и ничего лишнего. Например, добавлять USB-порты следует, только если они нужны для работы устройства. Дополнительные компоненты открывают новые направления для атак на устройство, которых следует избегать.

* **Защита оборудования от незаконного изменения.** Создайте процедуры для выявления возможностей для физического незаконного изменения, например отошедшей крышки устройства или изъятия компонента устройства. Эти сигналы о незаконном изменении могут передаваться в потоке данных, отправляемых в облако, предупреждая операторов об этих событиях.

* **Создание решения на базе безопасного оборудования.** Если себестоимость проданных товаров позволяет, создайте функции безопасности, например безопасное зашифрованное хранилище, или функции загрузки на основе доверенного платформенного модуля (TPM). Эти функции повышают безопасность устройств и помогают защитить всю инфраструктуру IoT.

* **Обеспечение безопасности при обновлении.** Встроенное ПО будет обновляться на протяжении всего времени существования устройства, и это неизбежно. Разработка безопасных способов обновления и криптографическая надежность версий встроенного ПО позволит обеспечить безопасность устройств во время и после обновления.

## <a name="iot-solution-developer"></a>Разработчик решений IoT

Ниже приведены рекомендации для разработчиков решений IoT.

* **Выбор безопасного метода разработки программного обеспечения.** При разработке защищенного ПО нужно тщательно продумать обеспечение защиты на всех этапах — от создания проекта вплоть до его реализации, тестирования и развертывания. От этого метода зависит выбор платформ, языков и инструментов. Жизненный цикл разработки защищенных приложений (Майкрософт) предоставляет поэтапный способ создания защищенного программного обеспечения.

* **Осторожность при выборе программного обеспечения с открытым кодом.** Программное обеспечение с открытым кодом позволяет ускорить разработку решений. При его выборе следует учитывать уровень активности сообщества в отношении каждого такого компонента. Если сообщество активно, то это гарантирует поддержку программного обеспечения, а также обнаружение и устранение проблем. И наоборот, если используется проект малоизвестного неактивного программного обеспечения с открытым кодом, то поддержка предоставляться не будет, а проблемы, скорее всего, не будут выявляться.

* **Осторожность при интеграции.** Множество брешей в системе защиты ПО обнаруживаются на границе интерфейсов API и библиотек. Функциональная возможность, которая не нужна для текущего развертывания, может быть по-прежнему доступна на уровне API. Для обеспечения общей безопасности обязательно проверьте все интерфейсы интегрируемых компонентов на наличие уязвимостей.

## <a name="iot-solution-deployer"></a>Специалист по развертыванию решений IoT

Ниже приведены рекомендации для специалистов по развертыванию решений IoT.

* **Безопасное развертывание оборудования**. Оборудование Интернета вещей может потребоваться развернуть в незащищенных местах, например в общественных помещениях или неконтролируемых расположениях. В этом случае следует обеспечить максимальную защиту развертываемого оборудования от незаконного изменения. Если в оборудовании установлены USB-порты или другие порты, убедитесь, что они надежно прикрыты. Их можно использовать в качестве точек входя для атаки.

* **Безопасное хранение ключей аутентификации.** При развертывании каждому устройству требуются коды и связанные с ними ключи аутентификации, создаваемые облачной службой. Эти ключи следует хранить в физически безопасном месте даже после развертывания. Вредоносное устройство может использовать любой скомпрометированный ключ для маскировки под существующее устройство.

## <a name="iot-solution-operator"></a>Оператор решений IoT

Ниже приведены рекомендации для операторов решений IoT.

* **Обновление системы.** Операционная система и все драйверы устройства должны быть обновлены до последней версии. Корпорация Майкрософт автоматически обновляет Windows 10 (Интернет вещей или другие SKU), если включен соответствующий параметр. Таким образом вы получаете безопасную операционную систему для устройств Интернета вещей. Постоянное обновление других операционных систем, например Linux, также позволяет защитить их от атак злоумышленников.

* **Защита от вредоносных действий.** По возможности добавьте новейшие антивирусные и антивредоносные программы в операционную систему каждого устройства. Это поможет устранить большинство внешних угроз. Вы можете защитить большинство современных операционных систем от угроз, приняв соответствующие меры.

* **Частое проведение аудита.** Для реагирования на проблемы с безопасностью в первую очередь следует выполнять аудит инфраструктуры Интернета вещей на наличие инцидентов безопасности. Большинство операционных систем обладает встроенными средствами ведения журнала событий. Их следует часто просматривать, чтобы убедиться в отсутствии брешей в системе безопасности. Данные аудита можно отправить как отдельный поток данных телеметрии в облачную службу для анализа.

* **Физическая защита инфраструктуры Интернета вещей.** Самые серьезные атаки на систему безопасности инфраструктуры Интернета вещей осуществляются посредством физического доступа к устройствам. Очень важно обеспечить защиту USB-портов и других физически доступных компонентов от атак злоумышленников. Одним из ключей к обнаружению брешей является ведение журналов физического доступа, например использования USB-портов. Опять же, в Windows 10 (IoT и другие SKU) доступна возможность подробного ведения журнала таких событий.

* **Защита учетных данных облака.** С помощью учетных данных для аутентификации в облаке, используемых для настройки развертывания инфраструктуры Интернета вещей и работы с ней, возможно, проще всего получить доступ к системе Интернета вещей и скомпрометировать ее. Чтобы защитить учетные данные, следует часто изменять пароль и не использовать их на общедоступных компьютерах.

Возможности разных устройств IoT различаются. Некоторые устройства могут быть компьютерами со стандартной операционной системой настольного компьютера. А на некоторых устройствах может быть установлена очень упрощенная операционная система. Описанные выше рекомендации по безопасности могут применяться к этим устройствам в разной степени. Следует использовать дополнительные рекомендации по развертыванию и безопасности, предоставляемые изготовителем устройств (если таковые имеются).

Некоторые устаревшие и ограниченные устройства могут быть не предназначены для развертывания IoT. В них может отсутствовать возможность шифрования данных, подключения к Интернету или предоставления расширенных средств аудита. Чтобы обеспечить безопасность, необходимую при подключении таких устройств к Интернету, при объединении данных устаревших устройств нужно использовать полевые шлюзы. Полевой шлюз обеспечивает безопасную аутентификацию, согласование зашифрованных сеансов, получение команд из облака и другие функции безопасности.