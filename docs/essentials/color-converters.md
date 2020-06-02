---
title: Преобразователи цвета в Xamarin.Essentials
description: Класс ColorConverters в Xamarin.Essentials предоставляет несколько вспомогательных методов и методов расширения для работы с System.Drawing.Color.
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
ms.custom: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 57b85a909855594c70bfd529d7a794b4bd7bca7e
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130225"
---
# <a name="xamarinessentials-color-converters"></a>Xamarin.Essentials. Преобразователи цвета

Класс **ColorConverters** в Xamarin.Essentials предоставляет несколько вспомогательных методов для System.Drawing.Color.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-color-converters"></a>Использование преобразователей цвета

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

При работе с `System.Drawing.Color` можно использовать встроенные преобразователи Xamarin.Forms для создания цвета из Hsl, Hex или UInt.

```csharp
var blueHex = ColorConverters.FromHex("#3498db");
var blueHsl = ColorConverters.FromHsl(204, 70, 53);
var blueUInt = ColorConverters.FromUInt(3447003);
```

## <a name="using-color-extensions"></a>Использование расширений цвета

Методы расширения на `System.Drawing.Color` позволяют применять разные свойства:

```csharp
var blue = ColorConverters.FromHex("#3498db");

// Multiplies the current alpha by 50%
var blueWithAlpha = blue.MultiplyAlpha(.5f);
```

Существует несколько других методов расширения, в том числе:

- GetComplementary
- MultiplyAlpha
- ToUInt
- WithAlpha
- WithHue
- WithLuminosity
- WithSaturation

## <a name="using-platform-extensions"></a>Использование расширений платформы

Кроме того, можно преобразовать System.Drawing.Color в структуре цвета для определенной платформы. Эти методы могут вызываться только из проектов iOS, Android и UWP.

```csharp
var system = System.Drawing.Color.FromArgb(255, 52, 152, 219);

// Extension to convert to Android.Graphics.Color, UIKit.UIColor, or Windows.UI.Color
var platform = system.ToPlatformColor();
```

```csharp
var platform = new Android.Graphics.Color(52, 152, 219, 255);

// Back to System.Drawing.Color
var system = platform.ToSystemColor();
```

Метод `ToSystemColor` применим к Android.Graphics.Color UIKit.UIColor и Windows.UI.Color.

## <a name="api"></a>API

- [Исходный код преобразователей цвета](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [Документация по API преобразователей цвета](xref:Xamarin.Essentials.ColorConverters)
- [Исходный код расширений цвета](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/ColorConverters.shared.cs)
- [Документация по API расширений цвета](xref:Xamarin.Essentials.ColorExtensions)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Color-Converters-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
