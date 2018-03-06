---
title: "Введение в iOS 9"
description: "В этой статье описаны все новые и измененные интерфейсы API и возможности, доступные в iOS 9 для разработчиков, Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 5b26989603695cfb309fba5a5318f7ef4d2460e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-9"></a>Введение в iOS 9

_В этой статье описаны все новые и измененные интерфейсы API и возможности, доступные в iOS 9 для разработчиков, Xamarin.iOS._

![](images/ios9-sml.png "Эмблема iOS 9")

Apple были добавлены несколько новых API-интерфейсов и служб в iOS 9 и усовершенствованиях существующих компонентов.

## <a name="3d-touch"></a>3D Touch

Новый iOS 9 и iPhone 6s и iPhone 6s Plus, 3D Touch добавляет давление конфиденциальные жестов в приложения iOS. С 3D сенсорный ввод, приложения iPhone теперь может не только сообщить, что пользователь касается экрана устройства, его можно также определения давление, exerting пользователя и отвечать на разные высоким уровнями.

3D Touch предоставляет следующие возможности для приложения:

- **Нехватка чувствительности** - приложений теперь можно оценить жестких или light пользователь касается экрана и имеют преимущество этой информации. Например приложение рисования можно сделать линии толще или тонкие основании жестких пользователь касается экрана.
- **Выбор и Pop** -приложения можно теперь позволяют пользователю взаимодействовать с данными, без необходимости перехода от их текущего контекста. Нажимая жестких на экране, они могут *Показать* на элемент интересующий (например, предварительный просмотр сообщения). Нажимая клавишу сложнее, они могут *Pop* в элемент.
- **Быстрые действия** -обработки для быстрого действий, таких как контекстные меню, которые может быть извлекается копии при щелчке элемента в приложении для настольных систем. С помощью быстрых действий, можно добавить общие быстрый и простой для быстрого доступа к функциям в приложении домашней значок Экран на устройстве iOS.

Чтобы узнать больше, см. в разделе нашей [введение в 3D Touch](~/ios/platform/3d-touch.md) руководства.

## <a name="app-transport-security"></a>Безопасность транспорта приложения

Новые для iOS 9 безопасности транспорта приложения (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения. ATS гарантирует, соответствуют всем каналам связи безопасное подключение, советы и рекомендации, тем самым предотвращая случайного раскрытия конфиденциальной информации, непосредственно через приложение или библиотеки, которая его использует.

Поскольку ATS включена по умолчанию в приложениях, собранных для iOS 9 и OS X 10.11 (El Capitan), все подключения, использующие [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) или [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) будут защищены ATS требования безопасности. Если эти требования не подходят для подключений, произойдет сбой с исключением.

Чтобы больше узнать о ATS, см. в разделе нашей [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md) руководства.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>Многозадачность для iPad

С iOS 9 Apple добавлена поддержка многозадачной работы два приложения в то же время на iPad определенного оборудования. В результате приложения Xamarin.iOS больше не могут считать, что они являются только приложение, работающее в любой момент времени или которым они имеют доступ на весь экран или ресурсы устройства.

Многозадачность для iPad поддерживает следующие функции:

- **Слайд по** -пользователь может временно запуска второго приложения iOS в слайд out панели (либо на правой или левой части экрана, в зависимости от направления языка), который охватывает примерно 25% в настоящее время выполнения основного приложения. Слайд по доступен только на iPad Pro, iPad воздуха, iPad 2 воздуха, iPad Mini 2, iPad Mini 3 или iPad Mini 4.
- **Разделенное представление** -на оборудовании, поддерживаемых iPad (iPad iPad Mini 4 и iPad Pro только 2 воздуху), пользователь может выбрать второй приложения и запустите side-by-side с текущего выполняемого приложения в режиме экрана разбиения. Пользователь может контролировать процентную долю главный экран, занимающую каждого приложения.
- **Изображение в изображении** — для приложений, содержимое воспроизведения видео, видео, которые теперь могут воспроизводиться в окне перемещаемый и изменять в размере, расположенном поверх других приложений, выполняющихся на устройстве iOS. Пользователь имеет полный контроль над размер и положение этого окна. Изображение в изображении доступна только на iPad Pro, iPad воздуха, iPad 2 воздуха, iPad Mini 2, iPad Mini 3 или iPad Mini 4.

Чтобы больше узнать о новых возможностей многозадачной iOS 9, см. в разделе нашей [многозадачность для iPad](~/ios/platform/multitasking.md) руководства.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>Новые контакты и контакты инфраструктур пользовательского интерфейса

С появлением iOS 9, Apple выпустила две новые платформы, [контактов](https://developer.xamarin.com/api/namespace/Contacts/) и [ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/), замените существующий адресной книги и используемые инфраструктур пользовательского интерфейса адресной книги iOS 8 или более ранних.

Эти новые, объектно ориентированного платформы содержатся следующие сведения.

- **Контакты** — Xamarin.iOS предоставляет доступ к контактные данные пользователя. Так как большинство приложений требуется только доступ только для чтения, эта платформа была оптимизирована для доступ к потокам безопасный, только для чтения.
- **ContactsUI** — предоставляет Xamarin.iOS элементы пользовательского интерфейса для отображения, редактирования, выбрать и создать контакты на устройствах iOS.

Дополнительные сведения см. в разделе нашей [контакты и пользовательского интерфейса контактов](~/ios/platform/contacts.md) документации.


## <a name="new-search-apis"></a>Новые API поиска

Поиск была расширена в iOS 9, чтобы предоставить новые способы доступа к данным внутри приложения Xamarin.iOS. С помощью новых API поиска, можно сделать содержимое своего приложения для поиска Spotlight и Safari результатов поиска, передачи и Siri напоминаний и предложения. Это позволяет пользователям быстрый доступ к действия и сведения глубокой в приложении.

Кроме того новые API поиска облегчают интеграцию поиска в приложении без возможности реализации предыдущего поиска. По этой причине Apple заявляет, что обычно занимает несколько часов, чтобы создать глобально для поиска с помощью приложения поиска содержимого с приложением iOS 9.

Дополнительные сведения см. в разделе нашей [поиска усовершенствования](~/ios/platform/search/index.md) документации.

## <a name="new-stack-view"></a>Новое представление стека

Элемент управления View стека ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/)) использует возможности автоматический макет и размер классов для управления стек представлений (по горизонтали или по вертикали), которое динамически реагировать устройства iOS ориентации и размера экрана.

С помощью элемента управления представление стека, объем работ, необходимых для макета пользовательского интерфейса, значительно снижается. Макет всех представлений прикреплено к представлению стека осуществляется автоматически на основании разработчика определенные свойства, такие как оси, распространение, выравнивание и интервалы.

Дополнительные сведения см. в разделе нашей [введение в представление стека](~/ios/user-interface/controls/uistackview.md) документации.


## <a name="collection-view-changes"></a>Просмотр изменений коллекции

В iOS 9, представление коллекции ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) теперь поддерживает перетащите переупорядочения элементов без дополнительной настройки, добавив новый распознаватель жестов по умолчанию и новые вспомогательные методы.

Используя эти новые методы, можно легко реализовать перетащите для изменения порядка в представлении коллекции и существует возможность настройки внешнего вида элементов любом этапе процесса изменения порядка.

Чтобы больше узнать об изменениях представление коллекции для iOS 9, см. в разделе нашей [изменения представления коллекции](~/ios/user-interface/controls/uicollectionview.md) руководства.

## <a name="game-enhancements"></a>Усовершенствования игры

С iOS 9 Apple улучшилась несколько технологий игры API-интерфейсам для упрощения реализации игровой графики и звук в своем приложении Xamarin.iOS. К ним относятся одновременно простота разработки до высокого уровня платформы и пользуясь возможностями устройства iOS GPU для повышения скорости и графические возможности благодаря улучшениям нижнего уровня.

Сюда входят вместе с новые расширенные функции исходного состояния системы, SceneKit и SpriteKit GameplayKit, ReplayKit, модель ввода-вывода, MetalKit и шейдеров производительности системы.

Дополнительные сведения см. в разделе нашей [игры усовершенствования](~/ios/platform/gaming/index.md) документации.

## <a name="homekit-framework-changes"></a>Изменения HomeKit Framework

[HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) framework, представленных в iOS 8, предоставляет возможность настройки и управления различные стандартные HomeKit включены (например, автоматические индикаторы блокировки дверцы и привилегией дверца гараж) из приложения Xamarin.iOS. Помимо простой установки и настройки, стандартные HomeKit можно регулировать с помощью голосовые команды Siri.

В iOS 9 Apple упрощается установки, развернут типы стандартные поддерживается и предоставляемые дополнительные аксессуаров взаимодействия (например, управление стандартную программу удаленно через iCloud).

Дополнительные сведения см. в разделе нашей [введение в HomeKit](~/ios/platform/homekit.md), [образца приложения iOS HomeKitIntro](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) и Apple [HomeKit](https://developer.apple.com/homekit/) документации.

## <a name="handoff-framework-changes"></a>Изменения Framework перемещение вручную

Перемещение вручную (также известный как непрерывности) был создан Apple в iOS 8 и OS X Yosemite (10.10) как способ для пользователя начать действие на одном из своих устройств (iOS или Mac) и продолжить это же действие на другом свои устройства (как определено в iClou пользователя d учетной записи).

Перемещение вручную был развернут в iOS 9, чтобы новые, также поддерживают улучшенные возможности поиска. Дополнительные сведения см. в разделе нашей [поиска усовершенствования](~/ios/platform/search/index.md) документации. Дополнительные сведения об использовании перемещение вручную см. в разделе нашей [Общие сведения о переадресации](~/ios/platform/handoff.md) документации.

## <a name="new-extension-points"></a>Новые точки расширения

В iOS 8 Apple реализовала расширения — библиотек, предоставляемых операционной системой в стандартных контекстах, например в центре уведомлений при запросе клавиатуру или, редактирующего фотографию.

С iOS 9 Apple расширяет расширения поддержки, предоставив несколько новых _точек расширения_ , определение политик использования и предоставляют интерфейсы API для работы в пределах данной области, следующим образом:

- **Новая точка расширения модульного аудио** — использовать эту точку расширения для предоставления звуковые эффекты, музыкальные инструменты, звуковой генераторы, т. д. для использования в других приложениях аудио единицы узла (например, GarageBand). Это точка расширения также позволяет продавать _аудио единицы_ (аудио подключаемых модулей) в магазине приложений.
- **Новая точка расширения обслуживания индекса** — использовать эту точку расширения для поддержки повторное индексирование данных приложения без необходимости следующем запуске приложения.
- **Новые точки расширения сети** (требуется специальное разрешение у Apple):
    - **Расширение поставщика прокси приложения** — использовать эту точку расширения для реализации пользовательских прозрачной сети клиентский прокси-сервер.
    - **Фильтрация данных поставщика / расширение поставщика управления фильтра** -эти точки расширения используются для реализации фильтрации на устройстве содержимого динамических сетевых.
    - **Расширение поставщика туннельный пакет** — использовать эту точку расширения для реализации пользовательского VPN туннельного протокола на стороне клиента.
- **Новые точки расширения Safari**:
    - **Содержимого блокировки расширения** — использовать эту точку расширения для определения списка заблокированных содержимого, которое будет отображаться при просмотре пользователем веб-сайте.
    - **Общие ссылки расширения** — использовать эту точку расширения для просмотра содержимого приложения в Safari общие ссылки.

Дополнительные сведения см. в разделе нашей [введение расширения](~/ios/platform/extensions.md) и Apple [руководство по программированию расширения приложения](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) документации.

## <a name="keychain-enhancements"></a>Усовершенствования в цепочке ключей

В iOS 9 Apple усовершенствованы цепочки ключей для обеспечения новый тип ключа шифрования Enclave защиты и дополнительные параметры защиты для элемента следующим образом:

- Новое ограничение Touch ID, которое делает недействительными цепочки ключей элементов, при изменении базы данных отпечатков пальцев.
- Новые ограничения, позволяющие только создание записей списка управления доступом с помощью Touch ID и секретный код.
- Новый контекст проверки подлинности, позволяет выполнять проверку подлинности, отдельно от `SecItem` вызовов.
- Доступ к энтропии список управления (с помощью параметра пароль приложения) для шифрования элемента цепочки ключей приложением.
- Поддержка создании и использовании ключей внутри безопасного enclave (через `kSecAttrTokenIDSecureEnclave` атрибута).

Дополнительные сведения см. в разделе нашей [введение в Touch ID](~/ios/platform/touchid.md) документации.


## <a name="right-to-left-language-support"></a>Поддержка языка справа налево

В iOS 9 Apple внес представления отраженных пользовательского интерфейса проще, чем когда-либо, предоставляя полную поддержку языков справа налево. Это поведение характеризуется следующим образом.

- Стандартная [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) элементы управления автоматически переместятся справа налево на основе параметров языка устройств iOS.
- [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) класс предоставляет атрибуты, которые позволяют определить, каким образом данного представления должно появляться при Перевернутая справа налево.
- Возможность программно зеркальное отражение изображения с помощью [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/) свойство [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/) класса.

Дополнительные сведения см. в разделе Apple [языков поддерживающие справа-налево](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) документации.



## <a name="additional-framework-changes"></a>Изменения дополнительных Framework

Помимо основных изменений, рассмотренных выше Apple внесенные изменения и усовершенствования несколько существующих инфраструктур IOS 9, включая следующие:

- Инфраструктура AV Foundation
- AVKit Framework
- CloudKit Framework
- Инфраструктура Foundation
- Перемещение вручную Framework
- HealthKit Framework
- HomeKit Framework
- Проверка подлинности локальных Framework
- MapKit Framework
- PassKit Framework
- Платформа служб Safari
- UIKit Framework

Дополнительные сведения см. в разделе нашей [изменения Framework дополнительных iOS 9](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) документации.

## <a name="deprecated-apis-and-functions"></a>Устаревшие интерфейсы API и функции

Следующие API-интерфейсы и функции в iOS 9, устаревшим Apple:

- **Адрес книги & пользовательского интерфейса адресной книги** -платформ контактных данных и обратитесь в службу пользовательского интерфейса были заменены эти API-интерфейсы. Дополнительные сведения см. в разделе нашей [контакты и пользовательского интерфейса контактов](~/ios/platform/contacts.md) документации.
- **CBCentralManager** - `RetrievePeripherals` и `RetrieveConnectedPeripherals` методы `CBCentralManager` класса будут удалены в iOS 9. Вызов этих методов приведет к сбою при связывание стандартную программу или при запуске приложения, приложения.
- **FetchAllChanges** - `FetchAllChanges` из `CKFetchRecordChangesOperation` класс был амортизации и будет удален в iOS 9.
- **Проигрыватель** -платформа проигрывателя мультимедиа был объявлен устаревшим в iOS 9. Вместо этого используйте AVKit или API-интерфейсы AV Foundation.

Полный список конкретных возражения API см. в разделе Apple [iOS 9.0 наборы API](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) документации.

## <a name="ios-9-sample-apps"></a>Образец приложения iOS 9

У нас есть некоторые [iOS 9 примерам](https://developer.xamarin.com/samples/ios/iOS9/) Чтобы приступить к работе:

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

Также ознакомьтесь с iOS части этих образцов (дополнительное Mac OS X версии скоро!):

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>Связанные ссылки

- [Образцы iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Общие сведения о 3D Touch](~/ios/platform/3d-touch.md)
- [Безопасность транспорта приложения](~/ios/app-fundamentals/ats.md)
- [Многозадачность для iPad](~/ios/platform/multitasking.md)
- [Контакты и контакты пользовательского интерфейса](~/ios/platform/contacts.md)
- [Новые API поиска](~/ios/platform/search/index.md)
- [Введение в представление стека](~/ios/user-interface/controls/uistackview.md)
- [Просмотр изменений коллекции](~/ios/user-interface/controls/uicollectionview.md)
- [Усовершенствования игр](~/ios/platform/gaming/index.md)
- [Общие сведения о HomeKit](~/ios/platform/homekit.md)
- [Общие сведения о переадресации](~/ios/platform/handoff.md)
- [Дополнительные изменения платформы iOS 9](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [Устранение неполадок](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [Новые возможности iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Обновление приложений Xamarin.iOS для iOS9 (видео)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)