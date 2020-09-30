---
title: Xamarin.Forms Класс устройства
description: В этой статье объясняется, как использовать Xamarin.Forms класс Device для детального контроля над функциональными возможностями и макетами на уровне отдельных платформ.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8eff115e894f77aeacff0f6c072bfd338fa19844
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91560511"
---
# <a name="no-locxamarinforms-device-class"></a>Xamarin.Forms Класс устройства

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)

[`Device`](xref:Xamarin.Forms.Device)Класс содержит ряд свойств и методов, помогающих разработчикам настраивать макет и функциональность для отдельных платформ.

Помимо методов и свойств, предназначенных для целевого кода на конкретных типах и размерах оборудования, `Device` класс включает методы, которые можно использовать для взаимодействия с элементами управления пользовательского интерфейса из фоновых потоков. Дополнительные сведения см. в разделе [взаимодействие с интерфейсом пользователя из фоновых потоков](#interact-with-the-ui-from-background-threads).

## <a name="provide-platform-specific-values"></a>Укажите значения, зависящие от платформы

До 2.3.4 платформа, в которой Xamarin.Forms было запущено приложение, может быть получена путем проверки [`Device.OS`](xref:Xamarin.Forms.Device.OS) Свойства и его сравнения со [`TargetPlatform.iOS`](xref:Xamarin.Forms.TargetPlatform.iOS) [`TargetPlatform.Android`](xref:Xamarin.Forms.TargetPlatform.Android) [`TargetPlatform.WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone) значениями перечисления,, и [`TargetPlatform.Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) . Аналогично одной из [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) перегрузок можно использовать для предоставления для элемента управления значений, зависящих от платформы.

Однако, поскольку Xamarin.Forms 2.3.4 эти API-интерфейсы являются устаревшими и замененными. [`Device`](xref:Xamarin.Forms.Device)Класс теперь содержит константы открытых строк, которые обозначают платформы: [`Device.iOS`](xref:Xamarin.Forms.Device.iOS) , [`Device.Android`](xref:Xamarin.Forms.Device.Android) , `Device.WinPhone` (не рекомендуется), `Device.WinRT` [`Device.UWP`](xref:Xamarin.Forms.Device.UWP) и [`Device.macOS`](xref:Xamarin.Forms.Device.macOS) . Аналогичным образом  [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) перегрузки заменяются API- [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) [`On`](xref:Xamarin.Forms.On) интерфейсами и.

В C# значения, зависящие от платформы, можно предоставить, создав `switch` инструкцию для [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) свойства, а затем предоставив `case` инструкции для требуемых платформ:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)Классы и [`On`](xref:Xamarin.Forms.On) предоставляют те же функции в XAML:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)Класс является универсальным классом, который должен быть создан с помощью `x:TypeArguments` атрибута, соответствующего целевому типу. В [`On`](xref:Xamarin.Forms.On) классе [`Platform`](xref:Xamarin.Forms.On.Platform) атрибут может принимать одно `string` значение или несколько значений, разделенных запятыми `string` .

> [!IMPORTANT]
> Указание неверного `Platform` значения атрибута в `On` классе не приведет к ошибке. Вместо этого код будет выполняться без применения значения, зависящего от платформы.

Кроме того, `OnPlatform` расширение разметки можно использовать в XAML для настройки внешнего вида пользовательского интерфейса на уровне платформы. Дополнительные сведения см. в разделе [расширение разметки для платформы](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="deviceidiom"></a>Device. идиом

`Device.Idiom`Свойство можно использовать для изменения макетов или функций в зависимости от устройства, на котором работает приложение. [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom)Перечисление содержит следующие значения:

- **Телефон** — устройства iPhone, iPod Touch и Android с узким пределом, чем 600 DIP. ^
- **Планшет** — iPad, устройства Windows и устройства Android, ширина которых превышает 600 DIP ^
- **Настольный** компьютер — возвращается только в [приложениях UWP](~/xamarin-forms/platform/windows/installation/index.md) на настольных компьютерах с Windows 10 (возвращается `Phone` на мобильных устройствах Windows, в том числе в сценариях с Continuum).
- **ТВ** – Tizen телевизионные устройства
- **Watch** — Tizen Watch Devices
- **Не поддерживается** — не используется

*значение ^ DIP не обязательно является физическим числом пикселей*

`Idiom`Свойство особенно полезно для создания макетов, которые используют преимущества более крупных экранов, например:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

[`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1)Класс предоставляет те же функции в XAML:

```xaml
<StackLayout>
    <StackLayout.Margin>
        <OnIdiom x:TypeArguments="Thickness">
            <OnIdiom.Phone>0,20,0,0</OnIdiom.Phone>
            <OnIdiom.Tablet>0,40,0,0</OnIdiom.Tablet>
            <OnIdiom.Desktop>0,60,0,0</OnIdiom.Desktop>
        </OnIdiom>
    </StackLayout.Margin>
    ...
</StackLayout>
```

[`OnIdiom`](xref:Xamarin.Forms.OnPlatform`1)Класс является универсальным классом, который должен быть создан с помощью `x:TypeArguments` атрибута, соответствующего целевому типу.

Кроме того, `OnIdiom` расширение разметки можно использовать в XAML для настройки внешнего вида пользовательского интерфейса на основе идиомы устройства, на котором выполняется приложение. Дополнительные сведения см. в разделе [расширение разметки onidiom](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom-markup-extension).

## <a name="deviceflowdirection"></a>Device. FlowDirection

[`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)Значение извлекает [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) значение перечисления, представляющее текущее направление потока, используемое устройством. Направление потока — это направление, в котором глаз человека перемещается по элементам пользовательского интерфейса на странице. Перечисление имеет следующие значения.

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

В XAML [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) значение можно получить с помощью `x:Static` расширения разметки:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Эквивалентный код в C#:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Дополнительные сведения о направлении потока см. [в разделе локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="devicestyles"></a>Device. Styles

[ `Styles` Свойство](~/xamarin-forms/user-interface/styles/index.md) содержит встроенные определения стилей, которые можно применить к некоторым свойствам элемента управления (например, `Label` ) `Style` . Доступны следующие стили:

- BodyStyle
- каптионстиле
- листитемдетаилтекстстиле
- листитемтекстстиле
- субтитлестиле
- титлестиле

## <a name="devicegetnamedsize"></a>Device. Жетнамедсизе

`GetNamedSize` может использоваться при задании [`FontSize`](~/xamarin-forms/user-interface/text/fonts.md) в коде C#:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

## <a name="devicegetnamedcolor"></a>Device.GetNamedColor

Xamarin.Forms в 4,6 введена поддержка именованных цветов. Именованный цвет — это цвет, имеющий разное значение в зависимости от того, какой системный режим (например, светло-или темный) активен на устройстве. В Android доступ к именованным цветам осуществляется через класс [R. Color](https://developer.android.com/reference/android/R.color#constants_2) . В iOS именованные цвета называются [системными цветами](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#system-colors). На универсальная платформа Windows именованные цвета называются [ресурсами темы XAML](/windows/uwp/design/controls-and-patterns/xaml-theme-resources).

`GetNamedColor`Метод можно использовать для получения именованных цветов в Android, iOS и UWP. Метод принимает `string` аргумент и возвращает [`Color`](xref:Xamarin.Forms.Color) :

```csharp
// Retrieve an Android named color
Color color = Device.GetNamedColor(NamedPlatformColor.HoloBlueBright);
```

`Color.Default` будет возвращен, если не удается найти имя цвета или если `GetNamedColor` вызывается на неподдерживаемой платформе.

> [!NOTE]
> Поскольку `GetNamedColor` метод возвращает объект `Color` , относящийся к платформе, он обычно следует использовать вместе со [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) свойством.

`NamedPlatformColor`Класс содержит константы, определяющие именованные цвета для Android, iOS и UWP:

| Android | iOS | macOS | UWP |
| --- | --- | --- | --- |
| `BackgroundDark` | `Label` | `AlternateSelectedControlTextColor` | `SystemAltHighColor` |
| `BackgroundLight` | `Link` | `ControlAccent` | `SystemAltLowColor` |
| `Black` | `OpaqueSeparator` | `ControlBackgroundColor` | `SystemAltMediumColor` |
| `DarkerGray` | `PlaceholderText` | `ControlColor` | `SystemAltMediumHighColor` |
| `HoloBlueBright` | `QuaternaryLabel` | `DisabledControlTextColor` | `SystemAltMediumLowColor` |
| `HoloBlueDark` | `SecondaryLabel` | `FindHighlightColor` | `SystemBaseHighColor` |
| `HoloBlueLight` | `Separator` | `GridColor` | `SystemBaseLowColor` |
| `HoloGreenDark` | `SystemBlue` | `HeaderTextColor` | `SystemBaseMediumColor` |
| `HoloGreenLight` | `SystemGray` | `HighlightColor` | `SystemBaseMediumHighColor` |
| `HoloOrangeDark` | `SystemGray2` | `KeyboardFocusIndicatorColor` | `SystemBaseMediumLowColor` |
| `HoloOrangeLight` | `SystemGray3` | `Label` | `SystemChromeAltLowColor` |
| `HoloPurple` | `SystemGray4` | `LabelColor` | `SystemChromeBlackHighColor` |
| `HoloRedDark` | `SystemGray5` | `Link` | `SystemChromeBlackLowColor` |
| `HoloRedLight` | `SystemGray6` | `LinkColor` | `SystemChromeBlackMediumColor` |
| `TabIndicatorText` | `SystemGreen` | `PlaceholderText` | `SystemChromeBlackMediumLowColor` |
| `Transparent` | `SystemIndigo` | `PlaceholderTextColor` | `SystemChromeDisabledHighColor` |
| `White` | `SystemOrange` | `QuaternaryLabel`| `SystemChromeDisabledLowColor` |
| `WidgetEditTextDark` | `SystemPink` | `QuaternaryLabelColor` | `SystemChromeHighColor` |
| | `SystemPurple` | `SecondaryLabel` | `SystemChromeLowColor` |
| | `SystemRed` | `SecondaryLabelColor` | `SystemChromeMediumColor` |
| | `SystemTeal` | `SelectedContentBackgroundColor` | `SystemChromeMediumLowColor` |
| | `SystemYellow` | `SelectedControlColor` | `SystemChromeWhiteColor` |
| | `TertiaryLabel` | `SelectedControlTextColor` | `SystemListLowColor` |
| | | `SelectedMenuItemTextColor` | `SystemListMediumColor`|
| | | `SelectedTextBackgroundColor` | |
| | | `SelectedTextColor` | |
| | | `Separator` | |
| | | `SeparatorColor` | |
| | | `ShadowColor` | |
| | | `SystemBlue` | |
| | | `SystemGray` | |
| | | `SystemGreen` | |
| | | `SystemIndigo` | |
| | | `SystemOrange` | |
| | | `SystemPink` | |
| | | `SystemPurple` | |
| | | `SystemRed` | |
| | | `SystemTeal` | |
| | | `SystemYellow` | |
| | | `TertiaryLabel` | |
| | | `TertiaryLabelColor` | |
| | | `TextBackgroundColor` | |
| | | `TextColor` | |
| | | `UnderPageBackgroundColor` | |
| | | `UnemphasizedSelectedContentBackgroundColor` | |
| | | `UnemphasizedSelectedTextBackgroundColor` | |
| | | `UnemphasizedSelectedTextColor` | |
| | | `WindowBackgroundColor` | |
| | | `WindowFrameTextColor` | |

## <a name="devicestarttimer"></a>Device. Старттимер

`Device`Класс также содержит метод, `StartTimer` который предоставляет простой способ запуска зависимых от времени задач, которые работают в Xamarin.Forms общем коде, включая библиотеку .NET Standard. Передайте значение, чтобы `TimeSpan` установить интервал и вернуться, `true` чтобы таймер работал или `false` был приостановлен после текущего вызова.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () =>
{
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Если код внутри таймера взаимодействует с пользовательским интерфейсом (например, при настройке текста `Label` или отображении предупреждения), он должен быть выполнен внутри `BeginInvokeOnMainThread` выражения (см. ниже).

> [!NOTE]
> `System.Timers.Timer`Классы и `System.Threading.Timer` являются .NET Standard альтернативами использования `Device.StartTimer` метода.

## <a name="interact-with-the-ui-from-background-threads"></a>Взаимодействие с пользовательским интерфейсом из фоновых потоков

Большинство операционных систем, в том числе iOS, Android и универсальная платформа Windows, используют однопотоковую модель для кода, включающего пользовательский интерфейс. Этот поток часто называют *основным потоком* или *потоком пользовательского интерфейса*. Следствием этой модели является то, что весь код, обращающийся к элементам пользовательского интерфейса, должен выполняться в основном потоке приложения.

Приложения иногда используют фоновые потоки для выполнения потенциально длительных операций, таких как извлечение данных из веб-службы. Если код, выполняемый в фоновом потоке, должен иметь доступ к элементам пользовательского интерфейса, он должен запустить этот код в основном потоке.

`Device`Класс включает следующие `static` методы, которые можно использовать для взаимодействия с элементами пользовательского интерфейса из фоновых потоков:

| Метод | Аргументы | Возвращает | Цель |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | Вызывает объект `Action` в основном потоке и не ожидает его завершения. |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | Вызывает объект `Func<T>` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | Вызывает объект `Action` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | Вызывает объект `Func<Task<T>>` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | Вызывает объект `Func<Task>` в основном потоке и ожидает его завершения. |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | Возвращает `SynchronizationContext` для основного потока. |

В следующем коде показан пример использования `BeginInvokeOnMainThread` метода:

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## <a name="related-links"></a>Связанные ссылки

- [Пример устройства](/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [Пример стилей](/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [API устройств](xref:Xamarin.Forms.Device)