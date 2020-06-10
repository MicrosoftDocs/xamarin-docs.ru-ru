---
title: Элементы управления изображениями watchOS в Xamarin
description: В этом документе описывается, как использовать элементы управления "изображение" в приложении watchOS, созданном с помощью Xamarin. В нем обсуждается элемент управления Вкинтерфацеимаже, метод Сетимаже, добавление изображений в расширение Watch, анимация и многое другое.
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 9ab18e643038d4a61b3b201295d4298f2b5e1adc
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574188"
---
# <a name="watchos-image-controls-in-xamarin"></a>Элементы управления изображениями watchOS в Xamarin

watchOS предоставляет [`WKInterfaceImage`](xref:WatchKit.WKInterfaceImage) элемент управления для вывода изображений и простых анимаций. Некоторые элементы управления также могут иметь фоновое изображение (например, кнопки, группы и контроллеры интерфейса).

![](image-images/image-walkway.png "Apple Watch отображения рисунка") ![](image-images/image-animation.png "Apple Watch с простой анимацией")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

Используйте образы каталога активов, чтобы добавить образы для просмотра приложений комплекта.
**@2x**Требуются только версии, так как на всех устройствах наблюдения отображается Retina.

![](image-images/asset-universal-sml.png "Only 2x versions are required, since all watch devices have Retina displays")

Рекомендуется убедиться, что изображения имеют правильный размер для просмотра контрольных значений. *Избегайте* использования изображений с неправильным размером (особенно больших) и масштабирования для их показа в контрольном списке.

Вы можете использовать размеры набора для просмотра (38 и часы) в образе каталога активов, чтобы указать разные изображения для каждого отображаемого размера.

![](image-images/asset-watch-sml.png "You can use the Watch Kit sizes 38mm and 42mm in an asset catalog image to specify different images for each display size")

## <a name="images-on-the-watch"></a>Изображения на часах

Наиболее эффективный способ отобразить изображения — *включить их в проект приложения Watch* и отобразить их с помощью `SetImage(string imageName)` метода.

Например, пример [ватчкиткаталог](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog/) содержит несколько изображений, добавленных в каталог активов в проекте Watch App.

![](image-images/asset-whale-sml.png "The WatchKitCatalog sample has a number of images added to an asset catalog in the watch app project")

Они могут быть эффективно загружены и отображены в контрольном списке с помощью `SetImage` с параметром name строки:

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>Фоновые изображения

Та же логика применяется к `SetBackgroundImage (string imageName)` `Button` `Group` `InterfaceController` классам, и. Оптимальная производительность достигается за счет хранения образов в самом приложении.

## <a name="images-in-the-watch-extension"></a>Изображения в расширении контрольных значений

Помимо загрузки образов, которые хранятся в самом приложении, можно отправить изображения из пакета расширений в приложение Watch для просмотра (или же можно загрузить изображения из удаленного расположения и отобразить их).

Чтобы загрузить изображения из расширения Watch, создайте `UIImage` экземпляры, а затем вызовите `SetImage` их с помощью `UIImage` объекта.

Например, пример [ватчкиткаталог](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) содержит образ с именем **бумблеби** в проекте расширения Watch:

![](image-images/asset-bumblebee-sml.png "The WatchKitCatalog sample has an image named Bumblebee in the watch extension project")

Следующий код приведет к следующему:

- образ, загружаемый в память, и
- отображается в контрольном списке.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```

## <a name="animations"></a>Анимации

Чтобы анимировать набор изображений, все они должны начинаться с того же префикса и иметь числовой суффикс.

В примере [ватчкиткаталог](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) имеется серия нумерованных изображений в проекте Watch App с префиксом **шины** :

![](image-images/asset-bus-animation-sml.png "The WatchKitCatalog sample has a series of numbered images in the watch app project with the Bus prefix")

Чтобы отобразить эти изображения в виде анимации, сначала загрузите изображение с помощью `SetImage` с именем префикса, а затем вызовите `StartAnimating` :

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

Вызовите `StopAnimating` элемент управления Image, чтобы прерывать цикл анимации:

```csharp
animatedImage.StopAnimating ();
```

<a name="cache"></a>

## <a name="appendix-caching-images-watchos-1"></a>Приложение. кэширование образов (watchOS 1)

> [!IMPORTANT]
> приложения watchOS 3 выполняются на устройстве полностью. Следующая информация предназначена только для приложений watchOS 1.

Если приложение многократно использует образ, хранящийся в расширении (или загружен), можно кэшировать образ в хранилище контрольных значений, чтобы повысить производительность последующих экранов.

Используйте `WKInterfaceDevice` метод s `AddCachedImage` для передачи изображения в контрольное значение, а затем используйте `SetImage` параметр с именем Image в качестве строки для его вывода:

```csharp
var device = WKInterfaceDevice.CurrentDevice;
using (var image = UIImage.FromBundle ("Bumblebee")) {
    if (!device.AddCachedImage (image, "Bumblebee")) {
            Console.WriteLine ("Image cache full.");
        } else {
            cachedImage.SetImage ("Bumblebee");
        }
    }
}
```

Вы можете запросить содержимое кэша изображений в коде с помощью `WKInterfaceDevice.CurrentDevice.WeakCachedImages` .

### <a name="managing-the-cache"></a>Управление кэшем

Кэш размером около 20 МБ. Он хранится в перезапусках приложения, и когда оно заполняется, вы обязаны очищать файлы с помощью `RemoveCachedImage` `RemoveAllCachedImages` методов или для `WKInterfaceDevice.CurrentDevice` объекта.

## <a name="related-links"></a>Связанные ссылки

- [Ватчкиткаталог (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Документ с изображением Apple](https://developer.apple.com/documentation/watchkit/wkinterfaceimage)
