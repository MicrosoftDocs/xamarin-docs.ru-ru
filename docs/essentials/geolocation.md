---
title: Xamarin.Essentials. Геопозиционирование
description: В этом документе описан класс Geolocation в Xamarin.Essentials, который предоставляет интерфейсы API для получения текущих геолокационных координат устройства.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 03/13/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0d1f953537b0963c3d25fbfe486a1e7ae97ce716
ms.sourcegitcommit: 3aa9bdcaaedca74ab5175cb2338a1df122300243
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101749347"
---
# <a name="xamarinessentials-geolocation"></a>Xamarin.Essentials. Геопозиционирование

Класс **Geolocation** предоставляет API для получения текущих геолокационных координат устройства.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Чтобы проверить функциональность класса **Geolocation**, нужно создать описанную ниже конфигурацию для конкретной платформы:

# <a name="android"></a>[Android](#tab/android)

Требуются разрешения Coarse и Fine Location, которые следует настроить в проекте Android. Кроме того, если приложение предназначено для платформы Android 5.0 (уровень API 21) или более поздней версии, в файле манифеста необходимо объявить, что приложение использует аппаратные возможности. Для этого можно применить любой из следующих методов:

Откройте файл **AssemblyInfo.cs** в папке **Properties** и добавьте в него:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

Или обновите манифест Android:

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Или щелкните правой кнопкой мыши проект Android и откройте свойства проекта. В разделе **Манифест Android** найдите область **Требуемые разрешения:** и установите флажок для разрешений **ACCESS_COARSE_LOCATION** и **ACCESS_FINE_LOCATION**. Это действие автоматически обновляет файл **AndroidManifest.xml**.

Если приложение предназначено для Android 10 Q (API уровня 29 или более поздней версии) и запрашивает **LocationAlways**, необходимо также добавить следующее разрешение в **AssemblyInfo.cs**:

```csharp
[assembly: UsesPermission(Manifest.Permission.AccessBackgroundLocation)]
```

Или непосредственно в **AndroidManifest.xml**:

```xml
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
```

Рекомендуется ознакомиться с [документацией Android по обновлениям в фоновом расположении](https://developer.android.com/training/location/permissions), так как необходимо учитывать множество ограничений.

[!include[](~/essentials/includes/android-permissions.md)]

# <a name="ios"></a>[iOS](#tab/ios)

Приложение **Info.plist** должно содержать раздел `NSLocationWhenInUseUsageDescription`, чтобы получить доступ к расположению устройства.

Откройте редактор plist, добавьте свойство **Privacy — Location When In Use Usage Description** и введите значение для отображения пользователю.

Кроме того, вы можете изменить текстовый файл вручную, добавить в него следующий текст и обновить обоснование:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>Fill in a reason why your app needs access to location.</string>
```

# <a name="uwp"></a>[UWP](#tab/uwp)

Необходимо задать разрешение `Location` для приложения. Для этого откройте файл **Package.appxmanifest**, выберите вкладку **Возможности** и установите флажок **Расположение**.

-----

## <a name="using-geolocation"></a>Использование геолокации

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

В API геолокации пользователю также будет предложено предоставить разрешения при необходимости.

Вы можете получить последнее известное [расположение](xref:Xamarin.Essentials.Location) устройства, вызвав метод `GetLastKnownLocationAsync`. Зачастую эта операция быстрее, чем выполнение всего запроса, но не так точна и может вернуть `null`, если расположение для кэширования отсутствует.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
    }
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (FeatureNotEnabledException fneEx)
{
    // Handle not enabled on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to get location
}
```

Значения высоты не всегда доступно. Если она недоступна, свойство `Altitude` может быть со значением `null` или равным нулю. Если высота доступна, значение указывается в метрах над уровнем моря.

Чтобы запросить текущие координаты [расположения](xref:Xamarin.Essentials.Location) устройства, можно использовать `GetLocationAsync`. Лучше всего передать полный `GeolocationRequest` и `CancellationToken`, так как получение расположения устройства может занять некоторое время.

```csharp
CancellationTokenSource cts;

async Task GetCurrentLocation()
{
    try
    {
        var request = new GeolocationRequest(GeolocationAccuracy.Medium, TimeSpan.FromSeconds(10));
        cts = new CancellationTokenSource();
        var location = await Geolocation.GetLocationAsync(request, cts.Token);

        if (location != null)
        {
            Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}, Altitude: {location.Altitude}");
        }
    }
    catch (FeatureNotSupportedException fnsEx)
    {
        // Handle not supported on device exception
    }
    catch (FeatureNotEnabledException fneEx)
    {
        // Handle not enabled on device exception
    }
    catch (PermissionException pEx)
    {
        // Handle permission exception
    }
    catch (Exception ex)
    {
        // Unable to get location
    }
}

protected override void OnDisappearing()
{
    if (cts != null && !cts.IsCancellationRequested)
        cts.Cancel();
    base.OnDisappearing();
}
```

## <a name="geolocation-accuracy"></a>Точность геолокации

В следующей таблице приведены сведения о точности для каждой платформы:

### <a name="lowest"></a>Наименьшая

| Платформа | Расстояние (в метрах) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000–5000 |

### <a name="low"></a>Низкий

| Платформа | Расстояние (в метрах) |
| --- | --- |
| Android | 500 |
| iOS | 1000 |
| UWP | 300–3000 |

### <a name="medium-default"></a>Medium (по умолчанию)

| Платформа | Расстояние (в метрах) |
| --- | --- |
| Android | 100–500 |
| iOS | 100 |
| UWP | 30–500 |

### <a name="high"></a>Высокий

| Платформа | Расстояние (в метрах) |
| --- | --- |
| Android | 0–100 |
| iOS | 10 |
| UWP | <= 10 |

### <a name="best"></a>Лучшее

| Платформа | Расстояние (в метрах) |
| --- | --- |
| Android | 0–100 |
| iOS | ~0 |
| UWP | <= 10 |

<a name="calculate-distance"></a>

## <a name="detecting-mock-locations"></a>Определение расположений макетов
Некоторые устройства могут возвращать расположение макетов от поставщика или приложения, которое предоставляет расположения макетов. Его можно определить с помощью `IsFromMockProvider` на любом [`Location`](xref:Xamarin.Essentials.Location).

```csharp
var request = new GeolocationRequest(GeolocationAccuracy.Medium);
var location = await Geolocation.GetLocationAsync(request);

if (location != null)
{
    if(location.IsFromMockProvider)
    {
        // location is from a mock provider
    }
}
```

## <a name="distance-between-two-locations"></a>Расстояние между двумя расположениями

Классы [`Location`](xref:Xamarin.Essentials.Location) и [`LocationExtensions`](xref:Xamarin.Essentials.LocationExtensions) определяют методы `CalculateDistance`, которые дают возможность вычисления расстояния между двумя географическими расположениями. При расчете не учитываются дороги или другие пути. Это самое короткое расстояние между двумя точками на поверхности Земли, также известное как _расстояние по дуге большого круга_ или, говоря простым языком, расстояние напрямик.

Ниже приведен пример:

```csharp
Location boston = new Location(42.358056, -71.063611);
Location sanFrancisco = new Location(37.783333, -122.416667);
double miles = Location.CalculateDistance(boston, sanFrancisco, DistanceUnits.Miles);
```

В конструкторе `Location` аргументы широты и долготы указаны в следующем порядке. Положительные значения широты находятся к северу от экватора, а положительные значения долготы — к востоку от нулевого меридиана. Используйте последний аргумент `CalculateDistance`, чтобы указать единицы измерения: мили или километры. Класс `UnitConverters` также определяет методы `KilometersToMiles` и `MilesToKilometers` для преобразования между двумя единицами измерения.

## <a name="platform-differences"></a>Различия платформ

Высота на каждой платформе вычисляется по-разному.

# <a name="android"></a>[Android](#tab/android)

В Android [высота](https://developer.android.com/reference/android/location/Location#getAltitude()), если она доступна, возвращается в виде высоты над нормальным эллипсоидом всемирной геодезической системы координат в метрах. Если в этом расположении нет высоты, возвращается 0,0.

# <a name="ios"></a>[iOS](#tab/ios)

В iOS [высота](https://developer.apple.com/documentation/corelocation/cllocation/1423820-altitude) измеряется в метрах. Положительные значения указывают высоту выше уровня моря, а отрицательные значения указывают на высоту ниже уровня моря.

# <a name="uwp"></a>[UWP](#tab/uwp)

В UWP высота возвращается в метрах. Дополнительные сведения см. в документации по [AltitudeReferenceSystem](/uwp/api/windows.devices.geolocation.geopoint.altitudereferencesystem#Windows_Devices_Geolocation_Geopoint_AltitudeReferenceSystem).

-----

## <a name="api"></a>API

- [Geolocation source code](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Geolocation) (Исходный код для геолокации)
- [Документация по API геолокации](xref:Xamarin.Essentials.Geolocation)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Geolocation-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
