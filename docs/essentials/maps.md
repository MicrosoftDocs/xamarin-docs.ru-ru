---
title: 'Xamarin.Essentials: класс Map'
description: Класс Map в Xamarin.Essentials позволяет приложению открывать установленное приложение карт на определенном местоположении или метке.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/26/2020
ms.custom: video
ms.openlocfilehash: 16f4a69e2d9216bb2e03fbcb663403d198b42c98
ms.sourcegitcommit: 6a6cbb62bcf149f9515ee8868679a8a07ce17956
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/26/2020
ms.locfileid: "83862720"
---
# <a name="xamarinessentials-map"></a>Xamarin.Essentials: Карта

Класс **Map** позволяет приложению открывать установленное приложение карт на определенном местоположении или метке.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-map"></a>Использование класса Map

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Чтобы использовать функции класса Map, вызовите метод `OpenAsync` с параметром `Location` или `Placemark` для открытия с дополнительными параметрами `MapLaunchOptions`.

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        try
        {
            await Map.OpenAsync(location, options);
        }
        catch (Exception ex)
        {
            // No map application available to open
        }
    }
}
```

При открытии с параметром `Placemark` требуется следующая информация:

- `CountryName`
- `AdminArea`
- `Thoroughfare`
- `Locality`

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var placemark = new Placemark
            {
                CountryName = "United States",
                AdminArea = "WA",
                Thoroughfare = "Microsoft Building 25",
                Locality = "Redmond"
            };
        var options =  new MapLaunchOptions { Name = "Microsoft Building 25" };

        try
        {
            await Map.OpenAsync(placemark, options);
        }
        catch (Exception ex)
        {
            // No map application available to open or placemark can not be located
        }
    }
}
```

## <a name="extension-methods"></a>Методы расширения

Если у вас уже есть ссылка на `Location` или `Placemark`, вы можете использовать встроенный метод расширения `OpenMapAsync` с дополнительными параметрами `MapLaunchOptions`:

```csharp
public class MapTest
{
    public async Task OpenPlacemarkOnMap(Placemark placemark)
    {
        try
        {
            await placemark.OpenMapAsync();
        }
        catch (Exception ex)
        {
            // No map application available to open
        }
    }
}
```

## <a name="directions-mode"></a>Режим направлений

Если вызвать `OpenMapAsync` без параметров `MapLaunchOptions`, карта запустится для указанного расположения. При желании можно рассчитать навигационный маршрут с текущего положения устройства. Для этого нужно установить `NavigationMode` в `MapLaunchOptions`:

```csharp
public class MapTest
{
    public async Task NavigateToBuilding25()
    {
        var location = new Location(47.645160, -122.1306032);
        var options =  new MapLaunchOptions { NavigationMode = NavigationMode.Driving };

        await Map.OpenAsync(location, options);
    }
}
```

## <a name="platform-differences"></a>Различия платформ

# <a name="android"></a>[Android](#tab/android)

- `NavigationMode` поддерживает езду на велосипеде, вождение и ходьбу.

# <a name="ios"></a>[iOS](#tab/ios)

- `NavigationMode` поддерживает вождение, перемещение в общественном транспорте и ходьбу.

# <a name="uwp"></a>[UWP](#tab/uwp)

- `NavigationMode` поддерживает вождение, перемещение в общественном транспорте и ходьбу.

--------------

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

Android использует схему `geo:` Uri для запуска приложения карт на устройстве. Для пользователя может отобразиться запрос на выбор из существующих приложений того, которое поддерживает эту схему Uri.  Xamarin.Essentials тестируется с помощью службы Google Maps, которая поддерживает эту схему.

# <a name="ios"></a>[iOS](#tab/ios)

Нет особенностей реализации для платформы.

# <a name="uwp"></a>[UWP](#tab/uwp)

Нет особенностей реализации для платформы.

--------------

## <a name="api"></a>API

- [Исходный код Map](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Map)
- [Документация по API для Map](xref:Xamarin.Essentials.Map)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Maps-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
