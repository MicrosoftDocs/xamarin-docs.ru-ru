---
title: Собственные представления в C#
description: На Xamarin.Forms страницы, созданные с помощью C#, можно ссылаться непосредственно на собственные представления из iOS, Android и UWP. В этой статье показано, как добавить собственные представления в Xamarin.Forms Макет, созданный с помощью C#, и как переопределить макет пользовательских представлений для исправления использования API измерения.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 71df780c648bcaa5a2ca4db388b52ac77a64d158
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91560550"
---
# <a name="native-views-in-c"></a>Собственные представления в C\#

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)

_На Xamarin.Forms страницы, созданные с помощью C#, можно ссылаться непосредственно на собственные представления из iOS, Android и UWP. В этой статье показано, как добавить собственные представления в Xamarin.Forms Макет, созданный с помощью C#, и как переопределить макет пользовательских представлений для исправления использования API измерения._

## <a name="overview"></a>Обзор

Любой Xamarin.Forms элемент управления, допускающий `Content` установку или имеющий `Children` коллекцию, может добавлять представления для конкретной платформы. Например, iOS `UILabel` можно напрямую добавить в [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) свойство или в [`StackLayout.Children`](xref:Xamarin.Forms.Layout`1.Children) коллекцию. Однако обратите внимание, что эта функция требует использования `#if` определений в Xamarin.Forms решениях общего проекта и недоступна из Xamarin.Forms решений .NET Standard Library.

На следующих снимках экрана показаны представления для конкретной платформы, добавленные в Xamarin.Forms [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

[![StackLayout, содержащий представления для конкретной платформы](code-images/screenshots-sml.png)](code-images/screenshots.png#lightbox "StackLayout, содержащий представления для конкретной платформы")

Возможность добавления в макет представлений для конкретной платформы Xamarin.Forms включена двумя методами расширения на каждой платформе:

- `Add` — Добавляет специфическое для платформы представление в [`Children`](xref:Xamarin.Forms.Layout`1.Children) коллекцию макета.
- `ToView` — принимает представление, зависящее от платформы, и заключает его в оболочку Xamarin.Forms [`View`](xref:Xamarin.Forms.View) , которое может быть задано как `Content` свойство элемента управления.

Использование этих методов в Xamarin.Forms общем проекте требует импорта соответствующего пространства имен для конкретной платформы Xamarin.Forms :

- **iOS** — Xamarin.Forms.Platform.iOS
- **Android** — Xamarin.Forms.Platform.Android
- **Универсальная платформа Windows (UWP)** — Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Добавление представлений для конкретной платформы на каждой платформе

В следующих разделах показано, как добавить представления для конкретной платформы в Xamarin.Forms Макет на каждой платформе.

### <a name="ios"></a>iOS

В следующем примере кода показано, как добавить в `UILabel` [`StackLayout`](xref:Xamarin.Forms.StackLayout) и [`ContentView`](xref:Xamarin.Forms.ContentView) .

```csharp
var uiLabel = new UILabel {
  MinimumFontSize = 14f,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = originalText,
};
stackLayout.Children.Add (uiLabel);
contentView.Content = uiLabel.ToView();
```

В примере предполагается, `stackLayout` что `contentView` экземпляры и ранее были СОЗДАНЫ в XAML или C#.

### <a name="android"></a>Android

В следующем примере кода показано, как добавить в `TextView` [`StackLayout`](xref:Xamarin.Forms.StackLayout) и [`ContentView`](xref:Xamarin.Forms.ContentView) .

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

В примере предполагается, `stackLayout` что `contentView` экземпляры и ранее были СОЗДАНЫ в XAML или C#.

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В следующем примере кода показано, как добавить в `TextBlock` [`StackLayout`](xref:Xamarin.Forms.StackLayout) и [`ContentView`](xref:Xamarin.Forms.ContentView) .

```csharp
var textBlock = new TextBlock
{
    Text = originalText,
    FontSize = 14,
    FontFamily = new FontFamily("HelveticaNeue"),
    TextWrapping = TextWrapping.Wrap
};
stackLayout.Children.Add(textBlock);
contentView.Content = textBlock.ToView();
```

В примере предполагается, `stackLayout` что `contentView` экземпляры и ранее были СОЗДАНЫ в XAML или C#.

## <a name="overriding-platform-measurements-for-custom-views"></a>Переопределение измерений платформы для пользовательских представлений

Пользовательские представления на каждой платформе часто реализуют только меру для сценария макета, для которого они были разработаны. Например, пользовательское представление могло бы занимать только половину доступной ширины устройства. Однако после совместного использования с другими пользователями может потребоваться пользовательское представление, чтобы занимать всю доступную ширину устройства. Таким образом, может потребоваться переопределить реализацию измерения пользовательских представлений при повторном использовании в Xamarin.Forms макете. По этой причине `Add` `ToView` методы расширения и предоставляют переопределения, позволяющие задавать делегаты измерений, которые могут переопределять макет пользовательского представления при добавлении в Xamarin.Forms Макет.

В следующих разделах показано, как переопределить макет пользовательских представлений, чтобы исправить использование API измерения.

### <a name="ios"></a>iOS

В следующем примере кода показан `CustomControl` класс, который наследует от `UILabel` :

```csharp
public class CustomControl : UILabel
{
  public override string Text {
    get { return base.Text; }
    set { base.Text = value.ToUpper (); }
  }

  public override CGSize SizeThatFits (CGSize size)
  {
    return new CGSize (size.Width, 150);
  }
}
```

Экземпляр этого представления добавляется в [`StackLayout`](xref:Xamarin.Forms.StackLayout) , как показано в следующем примере кода:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Однако поскольку `CustomControl.SizeThatFits` Переопределение всегда возвращает высоту 150, представление будет отображаться с пустым пространством выше и ниже, как показано на следующем снимке экрана:

![Ошибка customcontrol iOS с неправильной реализацией Сизесатфитс](code-images/ios-bad-measurement.png)

Решением этой проблемы является предоставление `GetDesiredSizeDelegate` реализации, как показано в следующем примере кода:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, double width, double height)
{
  var uiView = renderer.Control;

  if (uiView == null) {
    return null;
  }

  var constraint = new CGSize (width, height);

  // Let the CustomControl determine its size (which will be wrong)
  var badRect = uiView.SizeThatFits (constraint);

  // Use the width and substitute the height
  return new SizeRequest (new Size (badRect.Width, 70));
}
```

Этот метод использует ширину, предоставленную `CustomControl.SizeThatFits` методом, но замещает высоту 150 для высоты 70. При `CustomControl` добавлении экземпляра в [`StackLayout`](xref:Xamarin.Forms.StackLayout) `FixSize` метод может быть указан в качестве `GetDesiredSizeDelegate` для исправления неверного измерения, предоставленного `CustomControl` классом:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Это приводит к правильному отображению настраиваемого представления без пустого пространства выше и ниже, как показано на следующем снимке экрана:

![Ошибка customcontrol iOS с переопределением Жетдесиредсизе](code-images/ios-good-measurement.png)

### <a name="android"></a>Android

В следующем примере кода показан `CustomControl` класс, который наследует от `TextView` :

```csharp
public class CustomControl : TextView
{
  public CustomControl (Context context) : base (context)
  {
  }

  protected override void OnMeasure (int widthMeasureSpec, int heightMeasureSpec)
  {
    int width = MeasureSpec.GetSize (widthMeasureSpec);

    // Force the width to half of what's been requested.
    // This is deliberately wrong to demonstrate providing an override to fix it with.
    int widthSpec = MeasureSpec.MakeMeasureSpec (width / 2, MeasureSpec.GetMode (widthMeasureSpec));

    base.OnMeasure (widthSpec, heightMeasureSpec);
  }
}
```

Экземпляр этого представления добавляется в [`StackLayout`](xref:Xamarin.Forms.StackLayout) , как показано в следующем примере кода:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Однако поскольку `CustomControl.OnMeasure` Переопределение всегда возвращает половину запрошенной ширины, представление будет отображаться только в половину доступной ширины устройства, как показано на следующем снимке экрана:

![Android ошибка customcontrol с неправильной реализацией onmeasure](code-images/android-bad-measurement.png)

Решением этой проблемы является предоставление `GetDesiredSizeDelegate` реализации, как показано в следующем примере кода:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, int widthConstraint, int heightConstraint)
{
  var nativeView = renderer.Control;

  if ((widthConstraint == 0 && heightConstraint == 0) || nativeView == null) {
    return null;
  }

  int width = Android.Views.View.MeasureSpec.GetSize (widthConstraint);
  int widthSpec = Android.Views.View.MeasureSpec.MakeMeasureSpec (
    width * 2, Android.Views.View.MeasureSpec.GetMode (widthConstraint));
  nativeView.Measure (widthSpec, heightConstraint);
  return new SizeRequest (new Size (nativeView.MeasuredWidth, nativeView.MeasuredHeight));
}
```

Этот метод использует ширину, предоставленную `CustomControl.OnMeasure` методом, но умножает его на два. При `CustomControl` добавлении экземпляра в [`StackLayout`](xref:Xamarin.Forms.StackLayout) `FixSize` метод может быть указан в качестве `GetDesiredSizeDelegate` для исправления неверного измерения, предоставленного `CustomControl` классом:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Это приведет к правильному отображению пользовательского представления, занимая ширину устройства, как показано на следующем снимке экрана:

![Android ошибка customcontrol с настраиваемым делегатом Жетдесиредсизе](code-images/android-good-measurement.png)

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В следующем примере кода показан `CustomControl` класс, который наследует от `Panel` :

```csharp
public class CustomControl : Panel
{
  public static readonly DependencyProperty TextProperty =
    DependencyProperty.Register(
      "Text", typeof(string), typeof(CustomControl), new PropertyMetadata(default(string), OnTextPropertyChanged));

  public string Text
  {
    get { return (string)GetValue(TextProperty); }
    set { SetValue(TextProperty, value.ToUpper()); }
  }

  readonly TextBlock textBlock;

  public CustomControl()
  {
    textBlock = new TextBlock
    {
      MinHeight = 0,
      MaxHeight = double.PositiveInfinity,
      MinWidth = 0,
      MaxWidth = double.PositiveInfinity,
      FontSize = 14,
      TextWrapping = TextWrapping.Wrap,
      VerticalAlignment = VerticalAlignment.Center
    };

    Children.Add(textBlock);
  }

  static void OnTextPropertyChanged(DependencyObject dependencyObject, DependencyPropertyChangedEventArgs args)
  {
    ((CustomControl)dependencyObject).textBlock.Text = (string)args.NewValue;
  }

  protected override Size ArrangeOverride(Size finalSize)
  {
      // This is deliberately wrong to demonstrate providing an override to fix it with.
      textBlock.Arrange(new Rect(0, 0, finalSize.Width/2, finalSize.Height));
      return finalSize;
  }

  protected override Size MeasureOverride(Size availableSize)
  {
      textBlock.Measure(availableSize);
      return new Size(textBlock.DesiredSize.Width, textBlock.DesiredSize.Height);
  }
}
```

Экземпляр этого представления добавляется в [`StackLayout`](xref:Xamarin.Forms.StackLayout) , как показано в следующем примере кода:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Однако поскольку `CustomControl.ArrangeOverride` Переопределение всегда возвращает половину запрошенной ширины, представление будет обрезано до половины доступной ширины устройства, как показано на следующем снимке экрана:

![UWP ошибка customcontrol с неправильной реализацией ArrangeOverride](code-images/winrt-bad-measurement.png)

Решением этой проблемы является предоставление `ArrangeOverrideDelegate` реализации при добавлении представления в [`StackLayout`](xref:Xamarin.Forms.StackLayout) , как показано в следующем примере кода:

```csharp
stackLayout.Children.Add(fixedControl, arrangeOverrideDelegate: (renderer, finalSize) =>
{
    if (finalSize.Width <= 0 || double.IsInfinity(finalSize.Width))
    {
        return null;
    }
    var frameworkElement = renderer.Control;
    frameworkElement.Arrange(new Rect(0, 0, finalSize.Width * 2, finalSize.Height));
    return finalSize;
});
```

Этот метод использует ширину, предоставленную `CustomControl.ArrangeOverride` методом, но умножает его на два. Это приведет к правильному отображению пользовательского представления, занимая ширину устройства, как показано на следующем снимке экрана:

![UWP ошибка customcontrol с делегатом ArrangeOverride](code-images/winrt-good-measurement.png)

## <a name="summary"></a>Сводка

В этой статье объясняется, как добавить собственные представления в Xamarin.Forms Макет, созданный с помощью C#, и как переопределить макет пользовательских представлений для исправления использования API измерения.

## <a name="related-links"></a>Связанные ссылки

- [Нативимбеддинг (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)
- [Собственные формы](~/xamarin-forms/platform/native-forms.md)