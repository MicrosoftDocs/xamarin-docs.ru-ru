---
title: Расширения платформы Xamarin.Essentials
description: Xamarin.Essentials предоставляет несколько методов расширения платформы при работе с такими типами платформы, как Rect, Size и Point.
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3266d3870dc0d1ddef747168b2ac6b4d397a7c33
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135360"
---
# <a name="xamarinessentials-platform-extensions"></a>Xamarin.Essentials. расширения платформы

Xamarin.Essentials предоставляет несколько методов расширения платформы при работе с такими типами платформы, как Rect, Size и Point. Это означает, что можно осуществлять преобразование между версией `System` этих типов для их конкретных типов в iOS, Android и UWP.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-platform-extensions"></a>Использование расширений платформы

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Все расширения платформы могут вызываться только из проектов iOS, Android или UWP.

## <a name="android-extensions"></a>Расширения Android

Доступ к этим расширениям можно получить только из проекта Android.

### <a name="application-context--activity"></a>Действие и контекст приложения

Используя расширения платформы в классе `Platform`, можно получить доступ к текущим `Context` или `Activity` для работающего приложения.

```csharp

var context = Platform.AppContext;

// Current Activity or null if not initialized or not started.
var activity = Platform.CurrentActivity;
```

Если возникает ситуация, когда требуется `Activity`, но приложение запущено не полностью, следует использовать метод `WaitForActivityAsync`.

```csharp
var activity = await Platform.WaitForActivityAsync();
```

### <a name="activity-lifecycle"></a>Жизненный цикл действия

Кроме получения текущего действия, вы также можете зарегистрироваться для получения событий жизненного цикла.

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);

    Xamarin.Essentials.Platform.Init(this, bundle);

    Xamarin.Essentials.Platform.ActivityStateChanged += Platform_ActivityStateChanged;
}

protected override void OnDestroy()
{
    base.OnDestroy();
    Xamarin.Essentials.Platform.ActivityStateChanged -= Platform_ActivityStateChanged;
}

void Platform_ActivityStateChanged(object sender, Xamarin.Essentials.ActivityStateChangedEventArgs e) =>
    Toast.MakeText(this, e.State.ToString(), ToastLength.Short).Show();
```

Ниже перечислены состояния действий:

* Создано
* Возобновление
* Приостановлено
* Уничтожаются
* SaveInstanceState
* Запуск
* Остановлено

См. документацию по [жизненному циклу действия](https://docs.microsoft.com/xamarin/android/app-fundamentals/activity-lifecycle/).

## <a name="ios-extensions"></a>Расширения iOS

Доступ к этим расширениям можно получить только из проекта iOS.

### <a name="current-uiviewcontroller"></a>Current UIViewController

Получите доступ к текущему представлению `UIViewController`:

```csharp
var vc = Platform.GetCurrentUIViewController();
```

Этот метод возвращает `null`, если не удается обнаружить `UIViewController`.

## <a name="cross-platform-extensions"></a>Кроссплатформенные расширения

Эти расширения существуют на каждой платформе.

### <a name="point"></a>Точка

```csharp
var system = new System.Drawing.Point(x, y);

// Convert to CoreGraphics.CGPoint, Android.Graphics.Point, and Windows.Foundation.Point
var platform = system.ToPlatformPoint();

// Back to System.Drawing.Point
var system2 = platform.ToSystemPoint();
```

### <a name="size"></a>Размер

```csharp
var system = new System.Drawing.Size(width, height);

// Convert to CoreGraphics.CGSize, Android.Util.Size, and Windows.Foundation.Size
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### <a name="rectangle"></a>Прямоугольник

```csharp
var system = new System.Drawing.Rectangle(x, y, width, height);

// Convert to CoreGraphics.CGRect, Android.Graphics.Rect, and Windows.Foundation.Rect
var platform = system.ToPlatformRectangle();

// Back to System.Drawing.Rectangle
var system2 = platform.ToSystemRectangle();
```

## <a name="api"></a>API

- [Исходный код преобразователей](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/PlatformExtensions)
- [Документация по API преобразователей точек](xref:Xamarin.Essentials.PointExtensions)
- [Документация по API преобразователей прямоугольника](xref:Xamarin.Essentials.RectangleExtensions)
- [Документация по API преобразователей размера](xref:Xamarin.Essentials.SizeExtensions)
