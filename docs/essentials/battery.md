---
title: Xamarin.Essentials. Батарея
description: В этом документе описан класс Battery в Xamarin.Essentials, который позволяет проверить информацию об аккумуляторе устройства и отслеживать изменения.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 01/22/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3138db09c2a3d343f36aa48f561930df47f48ae6
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802474"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials. Батарея

Класс **Battery** позволяет получать сведения о батарее устройства, отслеживать изменения и просматривать информацию о состоянии энергосбережения (находится ли устройство в режиме пониженного энергопотребления). Приложениям следует избегать фоновой обработки, если на устройстве включено состояние экономии электроэнергии.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Для доступа к функции **Battery** нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Требуется разрешение `Battery`, которое следует настроить в проекте Android. Для этого можно применить любой из следующих методов:

Откройте файл **AssemblyInfo.cs** в папке **Свойства** и добавьте в него:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.BatteryStats)]
```

ИЛИ обновите манифест Android:

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**.

```xml
<uses-permission android:name="android.permission.BATTERY_STATS" />
```

ИЛИ щелкните правой кнопкой мыши проект Android и откройте свойства проекта. В разделе **Манифест Android** найдите область **Требуемые разрешения:** и установите флажок для разрешения **Battery**. Это действие автоматически обновляет файл **AndroidManifest.xml**.

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="using-battery"></a>Использование Battery

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Проверьте текущую информацию об аккумуляторе:

```csharp
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or 1.0 when on AC or no battery.

var state = Battery.State;

switch (state)
{
    case BatteryState.Charging:
        // Currently charging
        break;
    case BatteryState.Full:
        // Battery is full
        break;
    case BatteryState.Discharging:
    case BatteryState.NotCharging:
        // Currently discharging battery or not being charged
        break;
    case BatteryState.NotPresent:
        // Battery doesn't exist in device (desktop computer)
    case BatteryState.Unknown:
        // Unable to detect battery state
        break;
}

var source = Battery.PowerSource;

switch (source)
{
    case BatteryPowerSource.Battery:
        // Being powered by the battery
        break;
    case BatteryPowerSource.AC:
        // Being powered by A/C unit
        break;
    case BatteryPowerSource.Usb:
        // Being powered by USB cable
        break;
    case BatteryPowerSource.Wireless:
        // Powered via wireless charging
        break;
    case BatteryPowerSource.Unknown:
        // Unable to detect power source
        break;
}
```

При каждом изменении каких-либо свойств аккумулятора активируется событие:

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryInfoChanged += Battery_BatteryInfoChanged;
    }

    void Battery_BatteryInfoChanged(object sender, BatteryInfoChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

Устройства, работающие на аккумуляторах, можно перевести в режим низкого энергопотребления. Иногда устройства переключаются в этот режим автоматически, например, если остаток заряда аккумулятора опускается ниже 20 %. Операционная система в режиме экономии электроэнергии сокращает количество действий, при которых быстро разряжается аккумулятор. В приложениях при включенном режиме экономии не выполняется фоновая обработка и другие энергоемких действий.

Вы также можете узнать текущее состояние энергосбережения на устройстве с помощью статического свойства `Battery.EnergySaverStatus`:

```csharp
// Get energy saver status
var status = Battery.EnergySaverStatus;
```

Это свойство возвращает элемент перечисления `EnergySaverStatus`, значением которого является `On`, `Off` или `Unknown`. Если свойство возвращает значение `On`, приложению следует избегать фоновой обработки и выполнения других действий, которые могут потреблять много электроэнергии.

Также приложению следует установить обработчик событий. Класс **Battery** предоставляет событие, которое активируется при изменении состояния энергосбережения:

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Battery.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

Если состояние экономии электроэнергии изменится на `On`, приложение должно прекратить выполнение фоновой обработки. Если состояние изменится на `Unknown` или `Off`, приложение может возобновить фоновую обработку.

## <a name="platform-differences"></a>Различия платформ

# <a name="android"></a>[Android](#tab/android)

Различия платформ отсутствуют.

# <a name="ios"></a>[iOS](#tab/ios)

- Устройство должно использоваться для тестирования интерфейсов API.
- Для `PowerSource` будут возвращаться только значения `AC` или `Battery`.

# <a name="uwp"></a>[UWP](#tab/uwp)

- Для `PowerSource` будут возвращаться только значения `AC` или `Battery`.

-----

## <a name="api"></a>API

- [Исходный код Battery](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Battery)
- [Документация по API аккумулятора](xref:Xamarin.Essentials.Battery)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Battery-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
