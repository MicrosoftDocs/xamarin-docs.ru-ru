---
title: Xamarin.Essentials. Геокодирование
description: Класс Geocoding в Xamarin.Essentials предоставляет интерфейсы API для геокодирования метки в позиционные координаты и обратного геокодирования координат в метку.
ms.assetid: 3ADC440C-B000-4708-A2CC-296F5160AF90
author: jamesmontemagno
ms.author: jamont
ms.date: 05/28/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 66383e441435b5f4bdb48224c9ab602f28072b3d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802337"
---
# <a name="xamarinessentials-geocoding"></a>Xamarin.Essentials. Геокодирование

Класс **Geocoding** предоставляет API-интерфейсы для геокодирования метки в позиционные координаты и обратного геокодирования координат в метку.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Чтобы получить доступ к функции класса **Geocoding**, нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Дополнительная настройка не требуется.

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Для применения геокодирования нужно использовать ключ API Карт Bing. Зарегистрируйтесь для получения бесплатной учетной записи [Карт Bing](https://www.bingmapsportal.com/) В разделе **Моя учетная запись > My keys** (Мои ключи) создайте ключ и заполните сведения на основе типа приложения (которое должно быть **общедоступным приложением Windows (UWP, 8.x и более ранних версий)** для приложений UWP).

На раннем этапе работы приложения перед вызовом любого метода **Geocoding** задайте ключ API (он доступен только на UWP):

```csharp
Platform.MapServiceToken = "YOUR-KEY-HERE";
```

-----

## <a name="using-geocoding"></a>Использование Geocoding

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Получение координат [расположения](xref:Xamarin.Essentials.Location) для адреса:

```csharp
try
{
    var address =  "Microsoft Building 25 Redmond WA USA";
    var locations = await Geocoding.GetLocationsAsync(address);

    var location = locations?.FirstOrDefault();
    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

Значения высоты не всегда доступно. Если она недоступна, свойство `Altitude` может быть со значением `null` или равным нулю. Если высота доступна, значение указывается в метрах над уровнем моря.

## <a name="using-reverse-geocoding"></a>Использование обратного геокодирования

Обратное геокодирование — это процесс получения [меток](xref:Xamarin.Essentials.Placemark) для существующего набора координат:

```csharp
try
{
    var lat = 47.673988;
    var lon = -122.121513;

    var placemarks = await Geocoding.GetPlacemarksAsync(lat, lon);

    var placemark = placemarks?.FirstOrDefault();
    if (placemark != null)
    {
        var geocodeAddress =
            $"AdminArea:       {placemark.AdminArea}\n" +
            $"CountryCode:     {placemark.CountryCode}\n" +
            $"CountryName:     {placemark.CountryName}\n" +
            $"FeatureName:     {placemark.FeatureName}\n" +
            $"Locality:        {placemark.Locality}\n" +
            $"PostalCode:      {placemark.PostalCode}\n" +
            $"SubAdminArea:    {placemark.SubAdminArea}\n" +
            $"SubLocality:     {placemark.SubLocality}\n" +
            $"SubThoroughfare: {placemark.SubThoroughfare}\n" +
            $"Thoroughfare:    {placemark.Thoroughfare}\n";

        Console.WriteLine(geocodeAddress);
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Handle exception that may have occurred in geocoding
}
```

## <a name="distance-between-two-locations"></a>Расстояние между двумя расположениями

Классы [`Location`](xref:Xamarin.Essentials.Location) и [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) определяют методы вычисления расстояния между двумя расположениями. См. статью [ **Xamarin.Essentials: геопозиционирование**](geolocation.md#calculate-distance).

## <a name="api"></a>API

- [Исходный код Geocoding](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Geocoding)
- [Документация по API Geocoding](xref:Xamarin.Essentials.Geocoding)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Geocoding-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
