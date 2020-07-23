---
title: watchOS элементов управления пользовательского интерфейса в Xamarin
description: В этом документе описаны различные элементы управления, доступные для использования в пользовательских интерфейсах watchOS. Он содержит описание меток, кнопок, переключателей, ползунков, изображений, разделителей, карт и т. д.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/19/2016
ms.openlocfilehash: b8f33be80851815f00b0460a8e31f01733c425e4
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939726"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>watchOS элементов управления пользовательского интерфейса в Xamarin

В образце [**ватчкиткаталог**](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) демонстрируются различные элементы управления watchOS. Раскадровка приложения показана здесь (щелкните, чтобы увеличить):

[![Образец макета watchOS](images/storyboard-sml.png)](images/storyboard.png#lightbox)

Программные имена всех элементов управления начинаются с префикса `WKInterface` (например, `WKInterfaceLabel`, `WKInterfaceButton`).

|Control|Описание|Снимок экрана|
|---|---|---|
|Метка|Используйте `SetText` и другие свойства для управления внешним видом текста в элементе управления Label. `NSAttributedString`также поддерживается.<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Кнопка|Создание и задание свойств в раскадровке. Нажмите CTRL + перетаскивание, чтобы добавить, `Action` чтобы реализовать обработчик для того, чтобы он был нажат.<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Параметр|Используется `SetOn` для управления состоянием переключателя.<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Ползунок|Возможны различные стили.<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Образ —|Используйте `myImage.SetImage("MyWatchImage")` для загрузки изображений в контрольных значениях или `WKInterfaceDevice.CurrentDevice.AddCachedImage` для их кэширования для повторного использования в контрольных значениях.<br />[Документация по управлению изображением](~/ios/watchos/user-interface/image.md)<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Separator|Используйте разделители для создания привлекательных пользовательских интерфейсов наблюдения.<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Карта|Изображение на карте отображается статически, но вы можете управлять многими аспектами его внешнего вида, включая добавление ПИН-кодов.<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Фильм & Инлинемове|Фильмы можно либо открывать самостоятельно, либо встроено<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Группа|Используйте группы для создания привлекательных пользовательских интерфейсов наблюдения.<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Таблица|Упрощенная версия таблиц в iOS. Реализуйте `DidSelectRow` для реагирования на выбор пользователя (или используйте перехода).<br />[Документация по элементу управления Table](~/ios/watchos/user-interface/table.md)<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Устройство|`WKInterfaceDevice.CurrentDevice`включает такие свойства `ScreenBounds` , как, `ScreenScale` и `PreferredContentSizeCategory` .<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Меню](~/ios/watchos/user-interface/menu.md)|Определите меню принудительного нажатия в раскадровке и реализуйте действия для каждой кнопки в коде.<br />[Документация по элементу управления Menu (Force Touch)](~/ios/watchos/user-interface/menu.md)<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Текстовый ввод|Используйте `PresentTextInputController` и `WKTextInputMode` перечисление.<br />[Документация по вводу текста](~/ios/watchos/user-interface/text-input.md)<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digital Crown|Digital Crown можно использовать для управления средством выбора, или его поворот может быть записан в коде.<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Жесты|Существует четыре типа распознавания жестов, которые можно добавить в сцену: касание, прокрутка, панорамирование и Лонгпресс.<br />[Код каталога](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|

## <a name="related-links"></a>Связанные ссылки

- [Ватчкиткаталог (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Справочник по API для контрольного набора](xref:WatchKit)
