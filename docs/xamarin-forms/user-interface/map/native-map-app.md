---
title: Запустите собственное приложение Map из Xamarin.Forms
description: Собственное приложение Maps на каждой платформе может быть запущено из Xamarin.Forms приложения с помощью Xamarin.Essentials класса Launcher.
ms.prod: xamarin
ms.assetid: 5CF7CD67-3F20-4D80-B99E-D35A5FD1019A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/30/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ede34258650c378a45ee694f90de1e8b249acf71
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91559887"
---
# <a name="launch-the-native-map-app-from-no-locxamarinforms"></a>Запустите собственное приложение Map из Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Собственное приложение Map на каждой платформе можно запустить из Xamarin.Forms приложения с помощью Xamarin.Essentials `Launcher` класса. Этот класс позволяет приложению открывать другое приложение с помощью собственной схемы URI. Функцию запуска можно вызвать с помощью `OpenAsync` метода, передав `string` `Uri` аргумент или, представляющий настраиваемую схему URL-адресов для открытия. Дополнительные сведения о Xamarin.Essentials см. в разделе [Xamarin.Essentials](~/essentials/index.md?context=xamarin/xamarin-forms) .

> [!NOTE]
> Альтернативой использованию Xamarin.Essentials `Launcher` класса является использование его `Map` класса. Дополнительные сведения см. в разделе [ Xamarin.Essentials Map](~/essentials/maps.md?context=xamarin/xamarin-forms).

Приложение Maps на каждой платформе использует уникальную настраиваемую схему URI. Сведения о схеме URI Maps в iOS см. в разделе [Map Links](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html) on Developer.Apple.com. Дополнительные сведения о схеме универсального кода ресурса (URI) Maps в Android см. в разделе [карты для разработчиков Maps](https://developer.android.com/guide/components/intents-common.html#Maps) и на устройствах [Google Maps для Android](https://developers.google.com/maps/documentation/urls/android-intents) в Developers.Android.com. Сведения о схеме URI Maps на универсальная платформа Windows (UWP) см. в разделе [Запуск приложения Windows Maps](/windows/uwp/launch-resume/launch-maps-app).

## <a name="launch-the-map-app-at-a-specific-location"></a>Запуск приложения Map в определенном расположении

Расположение в приложении сопоставления машинного кода можно открыть, добавив соответствующие параметры запроса в настраиваемую схему URI для каждого приложения-карты:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    // https://developer.apple.com/library/ios/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html
    await Launcher.OpenAsync("http://maps.apple.com/?q=394+Pacific+Ave+San+Francisco+CA");
}
else if (Device.RuntimePlatform == Device.Android)
{
    // open the maps app directly
    await Launcher.OpenAsync("geo:0,0?q=394+Pacific+Ave+San+Francisco+CA");
}
else if (Device.RuntimePlatform == Device.UWP)
{
    await Launcher.OpenAsync("bingmaps:?where=394 Pacific Ave San Francisco CA");
}
```

Этот пример кода приводит к запуску собственного приложения Map на каждой платформе с картой в центре по ПИН-коду, представляющему указанное расположение:

[![Снимок экрана собственного приложения Map в iOS и Android](native-map-app-images/location.png "Собственное приложение для карт")](native-map-app-images/location-large.png#lightbox "Собственное приложение для карт")

## <a name="launch-the-map-app-with-directions"></a>Запуск приложения Map с инструкциями

Собственное приложение Maps может запустить отображение направлений, добавив соответствующие параметры запроса в настраиваемую схему URI для каждого приложения карты:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    // https://developer.apple.com/library/ios/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html
    await Launcher.OpenAsync("http://maps.apple.com/?daddr=San+Francisco,+CA&saddr=cupertino");
}
else if (Device.RuntimePlatform == Device.Android)
{
    // opens the 'task chooser' so the user can pick Maps, Chrome or other mapping app
    await Launcher.OpenAsync("http://maps.google.com/?daddr=San+Francisco,+CA&saddr=Mountain+View");
}
else if (Device.RuntimePlatform == Device.UWP)
{
    await Launcher.OpenAsync("bingmaps:?rtp=adr.394 Pacific Ave San Francisco CA~adr.One Microsoft Way Redmond WA 98052");
}
```

Этот пример кода приводит к запуску собственного приложения Map на каждой платформе с картой, расположенной в центре маршрута между указанными расположениями:

[![Снимок экрана собственного маршрута приложения для карт на iOS и Android](native-map-app-images/directions.png "Направления собственного приложения Map")](native-map-app-images/directions-large.png#lightbox "Направления собственного приложения Map")

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.Essentials](~/essentials/index.md?context=xamarin/xamarin-forms)
- [Ссылки на карту](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html)
- [Руководством для разработчиков карт](https://developer.android.com/guide/components/intents-common.html#Maps)
- [Устройства Google Maps для Android](https://developers.google.com/maps/documentation/)
- [Запуск приложения "Карты Windows"](/windows/uwp/launch-resume/launch-maps-app)