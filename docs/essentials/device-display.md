---
title: Xamarin.Essentials. Сведения о дисплее устройства
description: В этом документе описан класс DeviceDisplay в Xamarin.Essentials, который предоставляет метрики экрана для устройства, на котором работает приложение.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 867e6bd1828d4158f70226dbaad678f9d6037bb0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802397"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials. Сведения о дисплее устройства

Класс **DeviceDisplay** предоставляет сведения о метриках экрана устройства, на котором запущено приложение. Он также позволяет запросить, чтобы при работе приложения устройство не переводилось в спящий режим.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>Использование DeviceDisplay

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

## <a name="main-display-info"></a>Основные сведения об экране

В дополнение к базовой информации об устройстве класс **DeviceDisplay** содержит информацию об экране устройства и его ориентации.

```csharp
// Get Metrics
var mainDisplayInfo = DeviceDisplay.MainDisplayInfo;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = mainDisplayInfo.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = mainDisplayInfo.Rotation;

// Width (in pixels)
var width = mainDisplayInfo.Width;

// Height (in pixels)
var height = mainDisplayInfo.Height;

// Screen density
var density = mainDisplayInfo.Density;
```

Класс **DeviceDisplay** также предоставляет событие, которое запускается при изменении любой метрики экрана (на это событие можно подписаться):

```csharp
public class DisplayInfoTest
{
    public DisplayInfoTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.MainDisplayInfoChanged += OnMainDisplayInfoChanged;
    }

    void OnMainDisplayInfoChanged(object sender, DisplayInfoChangedEventArgs  e)
    {
        // Process changes
        var displayInfo = e.DisplayInfo;
    }
}
```

Класс **DeviceDisplay** предоставляет свойство `bool` с именем `KeepScreenOn`, задание которого предотвращает отключение или блокировку экрана устройства.

```csharp
public class KeepScreenOnTest
{
    public void ToggleScreenLock()
    {
        DeviceDisplay.KeepScreenOn = !DeviceDisplay.KeepScreenOn;
    }
}
```

## <a name="platform-differences"></a>Различия платформ

# <a name="android"></a>[Android](#tab/android)

Никаких различий.

# <a name="ios"></a>[iOS](#tab/ios)

- Доступ к `DeviceDisplay` должен выполняться в потоке пользовательского интерфейса, иначе возникнет исключение. Вы можете использовать метод [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) для запуска этого кода в потоке пользовательского интерфейса.

# <a name="uwp"></a>[UWP](#tab/uwp)

Никаких различий.

--------------

## <a name="api"></a>API

- [Исходный код DeviceDisplay](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/DeviceDisplay)
- [Документация по API DeviceDisplay](xref:Xamarin.Essentials.DeviceDisplay)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Device-Display-Information-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
