---
title: Цвета в Xamarin. Forms
description: Xamarin. Forms предоставляет гибкий класс цветового кросс-платформенного цвета. В этой статье объясняются функциональные возможности, предоставляемые классом Color, и способы их использования.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
ms.openlocfilehash: 42b532b8565d2d8e0289b8fd446e1dd7762a09ac
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517031"
---
# <a name="colors-in-xamarinforms"></a>Цвета в Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin. Forms предоставляет гибкий класс цветового кросс-платформенного цвета._

В этой статье описываются различные способы использования [`Color`](xref:Xamarin.Forms.Color) класса в Xamarin. Forms.

[`Color`](xref:Xamarin.Forms.Color) Класс предоставляет ряд методов для создания `Color` экземпляра:

- **Именованные цвета** — коллекция общих именованных цветов, включая `Red`, `Green`и. `Blue`
- `FromHex`— Строковое значение, аналогичное синтаксису, используемому в HTML, например "00FF00". При необходимости в качестве первой пары символов ("CC00FF00") можно указать альфа-канал.
- `FromHsla`— `double` Значения тона, насыщенности и яркости с необязательным альфа-значением (0,0 – 1,0).
- `FromHsv`— Оттенок, насыщенность, значение `int` или `double` значения.
- `FromHsva`— Оттенок, насыщенность, значение `int` или `double` значения.
- `FromRgb`— Красный, зеленый и синий `int` значения (0-255).
- `FromRgba`— Красный, зеленый, синий и альфа `int` -значения (0-255).
- `FromUint`-задать одиночное `double` значение, представляющее **ARGB**.

Вот несколько примеров цветов, назначенных для `BackgroundColor` некоторых меток с использованием различных вариантов разрешенного синтаксиса:

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

Эти цвета показаны на каждой из перечисленных ниже платформ. Обратите внимание на окончательный `Accent` цвет — это синий длинных цвет для iOS и Android; Это значение определяется Xamarin. Forms.

 [![Демонстрационный цвет](colors-images/colors-sml.png "Демонстрационный цвет")](colors-images/colors.png#lightbox "Демонстрационный цвет")

## <a name="colordefault"></a>Цвет. по умолчанию

Используйте `Default` для установки (или повторной установки) значения цвета обратно на платформу по умолчанию (понимание того, что оно представляет отдельный основной цвет на каждой платформе для каждого свойства).

Разработчики могут использовать это значение для задания `Color` свойства, но **не** должны запрашивать этот экземпляр для его значений RGB (все они имеют значение-1).

## <a name="colortransparent"></a>Цвет. прозрачный

Задайте для цвета значение очистить.

## <a name="coloraccent"></a>Color. акцент

В iOS и Android для этого экземпляра задан контрастный цвет, видимый на фоне по умолчанию, но он отличается от цвета текста по умолчанию.

## <a name="additional-methods"></a>Дополнительные методы

[`Color`](xref:Xamarin.Forms.Color)к экземплярам относятся следующие дополнительные методы:

- `AddLuminosity`— Возвращает объект `Color` , изменяя яркость на заданную разницу.
- `MultiplyAlpha`— Возвращает объект `Color` , изменяя альфа-канал, умножая его на заданное альфа-значение.
- `ToHex`— Возвращает шестнадцатеричное `string` представление `Color`.
- `WithHue`— Возвращает `Color`, заменяя оттенок на заданное значение.
- `WithLuminosity`— Возвращает `Color`, заменяя яркость на предоставляемое значение.
- `WithSaturation`— Возвращает `Color`, заменяя насыщенность заданным значением.

## <a name="implicit-conversions"></a>Неявные преобразования

Неявное преобразование `Xamarin.Forms.Color` между `System.Drawing.Color` типами и может быть выполнено:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device. Рунтимеплатформ

В этом фрагменте кода `Device.RuntimePlatform` используется свойство для выборочного задания цвета `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="use-from-xaml"></a>Использовать из XAML

К цветам также можно обращаться в XAML, используя определенные имена цветов или шестнадцатеричные представления, показанные ниже:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> При использовании компиляции XAML имена цветов не чувствительны к регистру и поэтому могут быть записаны в нижнем регистре. Дополнительные сведения о компиляции XAML см. в статье [Компиляция XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="related-links"></a>Связанные ссылки

- [колорссампле](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [Средство выбора с возможностью привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
