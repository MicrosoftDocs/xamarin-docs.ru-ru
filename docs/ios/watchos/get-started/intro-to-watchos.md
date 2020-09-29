---
title: Введение в watchOS
description: Этот документ содержит обзор watchOS, описывающих жизненный цикл приложения, типы пользовательского интерфейса, размеры экрана, ограничения и многое другое.
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 9cbf6e58586a223de51f1294aa1b4235b3b631f8
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436289"
---
# <a name="introduction-to-watchos"></a>Введение в watchOS

> [!NOTE]
> Ознакомьтесь с [введением в watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md) для получения общих сведений о новейших функциях.

## <a name="about-watchos"></a>О watchOS

Решение приложения watchOS имеет 3 проекта:

- **Просмотр расширения** — проект, содержащий код для приложения Watch.
- **Watch App** — содержит раскадровку и ресурсы пользовательского интерфейса.
- **родительское приложение iOS** — это приложение, которое является обычным приложением iPhone. Приложение для наблюдения и расширение поставляются в приложение iPhone для доставки в часы пользователя.

В приложениях watchOS 1 код в расширении выполняется на iPhone — Apple Watch фактически является внешним отображением. приложения watchOS 2 и 3 выполняются полностью на Apple Watch. Это различие показано на схеме ниже:

[![На этой схеме показана разница между watchOS 1 и watchOS 2 (и более поздними).](intro-to-watchos-images/arch-sml.png)](intro-to-watchos-images/arch.png#lightbox)

Независимо от того, какая версия watchOS предназначена, в Visual Studio для Mac Панель решения полное решение будет выглядеть примерно так:

[![Панель решения](intro-to-watchos-images/projectstructure-sml.png)](intro-to-watchos-images/projectstructure.png#lightbox)

*Родительское приложение* в решении watchOS — это регулярное приложение iOS. Это единственный проект в решении, который отображается **на телефоне**. Варианты использования для этого приложения включают руководства, экраны администрирования и фильтрацию среднего уровня, кэширование и т. д. Однако пользователь может установить и запустить контрольное приложение или расширение **, не открывая** родительское приложение, поэтому, если необходимо, чтобы родительское приложение выполнялось для однократной инициализации или администрирования, необходимо запрограммировать приложение или расширение Watch, чтобы сообщить пользователю о том, что.

Несмотря на то, что родительское приложение доставляет приложение Watch и расширение, они выполняются в разных изолированных средах.

На watchOS 1 они могут обмениваться данными через общую группу приложений или с помощью статической функции `WKInterfaceController.OpenParentApplication` , которая активирует `UIApplicationDelegate.HandleWatchKitExtensionRequest` метод в родительском приложении `AppDelegate` (см. раздел [Работа с родительским приложением](~/ios/watchos/app-fundamentals/parent-app.md)).

В watchOS 2 или более поздней версии инфраструктура подключения к данным используется для взаимодействия с родительским приложением с помощью `WCSession` класса.

## <a name="application-lifecycle"></a>Жизненный цикл приложения

В расширении Watch `WKInterfaceController` для каждой сцены раскадровки создается подкласс класса.

Эти `WKInterfaceController` классы аналогичны `UIViewController` объектам в программировании iOS, но не имеют такого же уровня доступа к представлению.
Например, нельзя динамически добавлять элементы управления в пользовательский интерфейс или переструктурировать его.
Однако вы можете скрывать и отображать элементы управления и, с некоторыми элементами управления, изменять размер, прозрачность и параметры внешнего вида.

Жизненный цикл `WKInterfaceController` объекта включает следующие вызовы:

- [Спящий](xref:WatchKit.WKInterfaceController.Awake*) режима. большую часть инициализации следует выполнять в этом методе.
- [Виллактивате](xref:WatchKit.WKInterfaceController.WillActivate) : вызывается вскоре перед отображением приложения Watch для пользователя. Используйте этот метод для выполнения последней инициализации, запуска анимации и т. д.
- На этом этапе отображается приложение Watch, и расширение начинает отвечать на введенные пользователем данные и обновляет отображение приложения в соответствии с логикой приложения.
- [Диддеактивате](xref:WatchKit.WKInterfaceController.DidDeactivate) После закрытия контрольного приложения пользователем вызывается этот метод. После возврата этого метода элементы управления пользовательского интерфейса не могут быть изменены до следующего `WillActivate` вызова. Этот метод также будет вызываться, если подключение к iPhone разорвано.
- После деактивации расширения оно становится недоступным для программы. Ожидающие асинхронные функции **не будут** вызываться. Расширения набора наблюдения не могут использовать режимы фоновой обработки. Если программа повторно активируется пользователем, но не была завершена операционной системой, первым вызываемым методом будет `WillActivate` .

![Общие сведения о жизненном цикле приложения](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png)

## <a name="types-of-user-interface"></a>Типы пользовательского интерфейса

Существует три типа взаимодействия, которые пользователь может использовать с вашим приложением для просмотра контрольных данных.
Все они запрограммированы с помощью пользовательских подклассов `WKInterfaceController` , поэтому ранее обсуждаемая последовательность жизненного цикла применяется глобально (уведомления запрограммированы с помощью `WKUserNotificationController` подклассов, которые сами по себе являются подклассами `WKInterfaceController` ):

### <a name="normal-interaction"></a>Нормальное взаимодействие

Большая часть отслеживания взаимодействия между приложениями и расширениями будет состоять из подклассов `WKInterfaceController` , которые вы пишете в соответствии с сценой в интерфейсе вашего приложения-наблюдателя. **раскадровка**. Это подробно описано в статьях об [установке](~/ios/watchos/get-started/installation.md) и [Начало работы](~/ios/watchos/get-started/index.md) .
На следующем рисунке показана часть раскадровки с примером в [каталоге контрольного комплекта](/samples/xamarin/ios-samples/watchos-watchkitcatalog) . Для каждого показанного здесь сцены имеется соответствующий пользовательский `WKInterfaceController` ( `LabelDetailController` , `ButtonDetailController` , `SwitchDetailController` и т. д.) в проекте расширения.

![Примеры обычных взаимодействий](intro-to-watchos-images/scenes.png)

### <a name="notifications"></a>Уведомления

[Уведомления](~/ios/watchos/platform/notifications.md) являются основным вариантом использования для Apple Watch. Поддерживаются как локальные, так и удаленные уведомления. Взаимодействие с уведомлениями происходит в два этапа: «короткий» и «длительный».

Короткий вид отображается вкратце и отображает значок приложения Watch, его имя и название (как указано в параметре `WKInterfaceController.SetTitle` ).

Длинный вид сочетает в себе предоставляемую системой **лентой** область и кнопку закрыть с пользовательским содержимым на основе раскадровки.

`WKUserNotificationInterfaceController` расширяется `WKInterfaceController` методами `DidReceiveLocalNotification` и `DidReceiveRemoteNotification` .
Переопределите эти методы, чтобы реагировать на события уведомления.

Дополнительные сведения о проектировании пользовательского интерфейса для уведомлений см. в [Apple Watch рекомендации по](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1) работе с человеком.

![Образцы уведомлений](intro-to-watchos-images/notifications.png)

## <a name="screen-sizes"></a>Размеры экрана

Apple Watch имеет два размера граней: 38 и часы, а также отношение числа 5:4 и Retina. Их пригодные для их работы размеры:

- 38:136 x 170 логических пикселов (272 x 340 пикселов)
- Часы: 156 x 195 логические Пиксели (312 x 390 пикселей).

Используйте `WKInterfaceDevice.ScreenBounds` , чтобы определить, на каком мониторе работает ваше приложение.

Как правило, проще разрабатывать макет текста и макета с более ограниченным отображением в формате 38, а затем увеличивать масштаб.
Если начать работу с более крупной средой, то масштабирование может привести к неверному перекрытию или усечению текста.

Узнайте больше о [работе с размерами экранов](~/ios/watchos/app-fundamentals/screen-sizes.md).

## <a name="limitations-of-watchos"></a>Ограничения watchOS

Существуют некоторые ограничения watchOS, которые следует учитывать при разработке приложений watchOS.

- Apple Watch устройства имеют ограниченный объем хранения, прежде чем загружать большие файлы (например, файлы аудио или фильмов).

- Многие [элементы управления](~/ios/watchos/user-interface/index.md) watchOS имеют аналог в UIKit, но являются разными классами ( `WKInterfaceButton` вместо `UIButton` , `WKInterfaceSwitch` для и `UISwitch` т. д.) и имеют ограниченный набор методов по сравнению с их эквивалентами UIKit. Кроме того, watchOS имеет некоторые элементы управления `WKInterfaceDate` , такие как (для отображения даты и времени), которых нет у UIKit.

  - Вы не можете маршрутизировать уведомления только для просмотра или только для iPhone (тип управления, который пользователь повлияет на маршрутизацию, не был объявлен компанией Apple).

Некоторые другие известные ограничения/часто задаваемые вопросы:

- Apple не будет разрешать сторонние пользовательские наблюдатели.

- Интерфейсы API, позволяющие отслеживать управление iTunes на подключенном телефоне, являются частными.

## <a name="further-reading"></a>Дополнительные материалы

Ознакомьтесь с документацией от Apple:

- [Разработка для контрольного набора](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

- [Руководство по программированию для контрольного комплекта](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

- [Apple Watch рекомендации по работе с человеком](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)

## <a name="related-links"></a>Связанные ссылки

- [Каталог watchOS 3 (пример)](/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Каталог watchOS 1 (пример)](/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Настройка и установка](~/ios/watchos/get-started/installation.md)
- [Первое видео о приложении](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Руководство по разработке Apple для контрольного набора](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Советы по WatchKit Apple](https://developer.apple.com/watchkit/tips/)
- [Введение в watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)