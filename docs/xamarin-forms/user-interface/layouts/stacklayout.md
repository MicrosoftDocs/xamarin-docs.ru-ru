---
title: Xamarin.Forms StackLayout
description: 'StackLayout организует дочерние представления в одномерном стеке: по горизонтали или по вертикали.'
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/11/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fcd5f3deb2c7645988cd70b3556b718151df99fd
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366676"
---
# <a name="no-locxamarinforms-stacklayout"></a>Xamarin.Forms StackLayout

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-stacklayoutdemos)

[![::: No-Loc (Xamarin. Forms)::: StackLayout](stacklayout-images/layouts.png "::: No-Loc (Xamarin. Forms)::: StackLayout")](stacklayout-images/layouts-large.png#lightbox "::: No-Loc (Xamarin. Forms)::: StackLayout")

Объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) упорядочивает дочерние представления в одномерном стеке: по горизонтали или по вертикали. По умолчанию объект `StackLayout` ориентирован по вертикали. Кроме того, `StackLayout` можно использовать в качестве родительского макета, содержащего другие дочерние макеты.

[`StackLayout`](xref:Xamarin.Forms.StackLayout)Класс определяет следующие свойства:

- [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation), типа [`StackOrientation`](xref:Xamarin.Forms.StackOrientation) , представляет направление, в котором располагаются дочерние представления. Значение по умолчанию этого свойства равно `Vertical`.
- [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)Тип `double` — указывает расстояние между каждым дочерним представлением. Значение этого свойства по умолчанию — шесть единиц, независимых от устройства.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами для привязок данных и стилей.

[`StackLayout`](xref:Xamarin.Forms.StackLayout)Класс является производным от `Layout<T>` класса, который определяет `Children` свойство типа `IList<T>` . `Children`Свойство является свойством `ContentProperty` `Layout<T>` класса, поэтому его не нужно явно задавать из XAML.

> [!TIP]
> Чтобы получить максимально возможную производительность макета, следуйте рекомендациям по [оптимизации производительности макета](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance).

## <a name="vertical-orientation"></a>Вертикальная ориентация

В следующем коде XAML показано, как создать вертикально-ориентированный [`StackLayout`](xref:Xamarin.Forms.StackLayout) , который содержит различные дочерние представления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.VerticalStackLayoutPage"
             Title="Vertical StackLayout demo">
    <StackLayout Margin="20">
        <Label Text="Primary colors" />
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <Label Text="Secondary colors" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

В этом примере создается вертикальный [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Label`](xref:Xamarin.Forms.Label) объект, содержащий [`BoxView`](xref:Xamarin.Forms.BoxView) объекты и. По умолчанию между дочерними представлениями существует шесть независимых от устройства единиц пространства:

[![Снимок экрана с вертикально-ориентированным StackLayout](stacklayout-images/vertical.png "Вертикально ориентированный StackLayout")](stacklayout-images/vertical-large.png#lightbox "Вертикально ориентированный StackLayout")

Эквивалентный код на C# выглядит так:

```csharp
public class VerticalStackLayoutPageCS : ContentPage
{
    public VerticalStackLayoutPageCS()
    {
        Title = "Vertical StackLayout demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Primary colors" },
                new BoxView { Color = Color.Red },
                new BoxView { Color = Color.Yellow },
                new BoxView { Color = Color.Blue },
                new Label { Text = "Secondary colors" },
                new BoxView { Color = Color.Green },
                new BoxView { Color = Color.Orange },
                new BoxView { Color = Color.Purple }
            }
        };
    }
}
```

> [!NOTE]
> Значение [`Margin`](xref:Xamarin.Forms.View.Margin) свойства представляет расстояние между элементом и его соседними элементами. Дополнительные сведения см. в статье [Поля и заполнение](margin-and-padding.md).

## <a name="horizontal-orientation"></a>Ориентация по горизонтали

В следующем коде XAML показано, как создать горизонтально ориентированный [`StackLayout`](xref:Xamarin.Forms.StackLayout) , установив [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) для его свойства значение `Horizontal` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.HorizontalStackLayoutPage"
             Title="Horizontal StackLayout demo">
    <StackLayout Margin="20"
                 Orientation="Horizontal"
                 HorizontalOptions="Center">
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

В этом примере создается горизонтально [`StackLayout`](xref:Xamarin.Forms.StackLayout) содержащий [`BoxView`](xref:Xamarin.Forms.BoxView) объекты с шестью аппаратно-независимыми единицами пространства между дочерними представлениями:

[![Снимок экрана с горизонтально ориентированной StackLayout](stacklayout-images/horizontal.png "Горизонтально ориентированная StackLayout")](stacklayout-images/horizontal-large.png#lightbox "Горизонтально ориентированная StackLayout")

Эквивалентный код на C# выглядит так:

```csharp
public HorizontalStackLayoutPageCS()
{
    Title = "Horizontal StackLayout demo";
    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Orientation = StackOrientation.Horizontal,
        HorizontalOptions = LayoutOptions.Center,
        Children =
        {
            new BoxView { Color = Color.Red },
            new BoxView { Color = Color.Yellow },
            new BoxView { Color = Color.Blue },
            new BoxView { Color = Color.Green },
            new BoxView { Color = Color.Orange },
            new BoxView { Color = Color.Purple }
        }
    };
}
```

## <a name="space-between-child-views"></a>Пространство между дочерними представлениями

Интервал между дочерними представлениями в [`StackLayout`](xref:Xamarin.Forms.StackLayout) может быть изменен путем присвоения [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) свойству `double` значения.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.StackLayoutSpacingPage"
             Title="StackLayout Spacing demo">
    <StackLayout Margin="20"
                 Spacing="0">
        <Label Text="Primary colors" />
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <Label Text="Secondary colors" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

В этом примере создается вертикальный [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Label`](xref:Xamarin.Forms.Label) [`BoxView`](xref:Xamarin.Forms.BoxView) объект, содержащий объекты и, между которыми нет интервала:

[![Снимок экрана StackLayout без промежутков](stacklayout-images/spacing.png "StackLayout без промежутков")](stacklayout-images/spacing-large.png#lightbox "StackLayout без промежутков")

> [!TIP]
> [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)Свойству может быть присвоено отрицательное значение, чтобы сделать дочерние представления перекрывающимися.

Эквивалентный код на C# выглядит так:

```csharp
public class StackLayoutSpacingPageCS : ContentPage
{
    public StackLayoutSpacingPageCS()
    {
        Title = "StackLayout Spacing demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Spacing = 0,
            Children =
            {
                new Label { Text = "Primary colors" },
                new BoxView { Color = Color.Red },
                new BoxView { Color = Color.Yellow },
                new BoxView { Color = Color.Blue },
                new Label { Text = "Secondary colors" },
                new BoxView { Color = Color.Green },
                new BoxView { Color = Color.Orange },
                new BoxView { Color = Color.Purple }
            }
        };
    }
}
```

## <a name="position-and-size-of-child-views"></a>Расположение и размер дочерних представлений

Размер и расположение дочерних представлений в пределах [`StackLayout`](xref:Xamarin.Forms.StackLayout) зависит от значений дочерних представлений [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) и [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) свойств, а также от значений [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) свойств и. В вертикальных [`StackLayout`](xref:Xamarin.Forms.StackLayout) дочерних представлениях разворачиваются для заполнения доступной ширины, если их размер не задан явно. Аналогично, в горизонтальных `StackLayout` дочерних представлениях разворачиваются для заполнения доступной высоты, если их размер не задан явно.

[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)Свойства и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) [`StackLayout`](xref:Xamarin.Forms.StackLayout) его дочерние представления могут быть установлены в поля из [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) структуры, которая инкапсулирует два предпочтения макета:

- *Выравнивание* определяет положение и размер дочернего представления в его родительском макете.
- *Расширение* указывает, должно ли дочернее представление использовать дополнительное пространство, если оно доступно.

> [!TIP]
> Не устанавливайте [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) Свойства и для объекта [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) , [`StackLayout`](xref:Xamarin.Forms.StackLayout) если не требуется. Значения по умолчанию для свойств `LayoutOptions.Fill` и `LayoutOptions.FillAndExpand` обеспечивают наилучшую оптимизацию макета. Изменение этих свойств требует затрат и потребляет память даже при восстановлении значений по умолчанию.

### <a name="alignment"></a>Соответствие

В следующем примере XAML устанавливаются параметры выравнивания для каждого дочернего представления в [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.AlignmentPage"
             Title="Alignment demo">
    <StackLayout Margin="20">
        <Label Text="Start"
               BackgroundColor="Gray"
               HorizontalOptions="Start" />
        <Label Text="Center"
               BackgroundColor="Gray"
               HorizontalOptions="Center" />
        <Label Text="End"
               BackgroundColor="Gray"
               HorizontalOptions="End" />
        <Label Text="Fill"
               BackgroundColor="Gray"
               HorizontalOptions="Fill" />
    </StackLayout>
</ContentPage>
```

В этом примере параметры выравнивания устанавливаются для [`Label`](xref:Xamarin.Forms.Label) объектов, чтобы контролировать их положение в [`StackLayout`](xref:Xamarin.Forms.StackLayout) . [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)Поля, [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) , [`End`](xref:Xamarin.Forms.LayoutOptions.End) и [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) используются для определения выравнивания [`Label`](xref:Xamarin.Forms.Label) объектов внутри родительского элемента `StackLayout` :

[![Снимок экрана с набором параметров выравнивания StackLayout](stacklayout-images/alignment.png "StackLayout с параметрами выравнивания")](stacklayout-images/alignment-large.png#lightbox "StackLayout с параметрами выравнивания")

[`StackLayout`](xref:Xamarin.Forms.StackLayout) учитывает только параметры выравнивания дочерних представлениях, которые находятся в направлении, противоположном ориентации `StackLayout`. Поэтому дочерние представления [`Label`](xref:Xamarin.Forms.Label) в `StackLayout` с вертикальной ориентацией получают свойства [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) в соответствии с одним из следующих полей выравнивания:

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), который размещает элемент [`Label`](xref:Xamarin.Forms.Label) в левой части [`StackLayout`](xref:Xamarin.Forms.StackLayout) .
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) — располагает [`Label`](xref:Xamarin.Forms.Label) в центре [`StackLayout`](xref:Xamarin.Forms.StackLayout).
- [`End`](xref:Xamarin.Forms.LayoutOptions.End), который размещает элемент [`Label`](xref:Xamarin.Forms.Label) в правой части [`StackLayout`](xref:Xamarin.Forms.StackLayout) .
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) — гарантирует, что [`Label`](xref:Xamarin.Forms.Label) заполняет ширину [`StackLayout`](xref:Xamarin.Forms.StackLayout).

Эквивалентный код на C# выглядит так:

```csharp
public class AlignmentPageCS : ContentPage
{
    public AlignmentPageCS()
    {
        Title = "Alignment demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
                new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
                new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
                new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
            }
        };
    }
}
```

### <a name="expansion"></a>Расширение

В следующем примере XAML устанавливаются параметры расширения для каждого [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.ExpansionPage"
             Title="Expansion demo">
    <StackLayout Margin="20">
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Start"
               BackgroundColor="Gray"
               VerticalOptions="StartAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Center"
               BackgroundColor="Gray"
               VerticalOptions="CenterAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="End"
               BackgroundColor="Gray"
               VerticalOptions="EndAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Fill"
               BackgroundColor="Gray"
               VerticalOptions="FillAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
    </StackLayout>
</ContentPage>
```

В этом примере параметры расширения устанавливаются для объектов, [`Label`](xref:Xamarin.Forms.Label) чтобы управлять их размером в [`StackLayout`](xref:Xamarin.Forms.StackLayout) . [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)Поля, [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand) , [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) и [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) используются для определения параметров выравнивания, а также для того, `Label` будет ли заниматься больше места, если он доступен в пределах родительского элемента `StackLayout` :

[![Снимок экрана с набором параметров расширения StackLayout](stacklayout-images/expansion.png "StackLayout с параметрами расширения")](stacklayout-images/expansion-large.png#lightbox "StackLayout с параметрами расширения")

[`StackLayout`](xref:Xamarin.Forms.StackLayout) может развернуть дочерние представления только в направлении своей ориентации. Поэтому `StackLayout` в вертикальной ориентации может развернуть дочерние представления [`Label`](xref:Xamarin.Forms.Label), которые задают свои свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) одному из полей выравнивания. Это означает, что для вертикального выравнивания каждый `Label` занимает одинаковый объем пространства в `StackLayout`. Но только последний `Label` со значением свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions), равным [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand), имеет другой размер.

> [!TIP]
> При использовании метода [`StackLayout`](xref:Xamarin.Forms.StackLayout) Убедитесь, что только одно дочернее представление имеет значение [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) . В этом случае указанный дочерний элемент будет занимать максимальное пространство, предоставляемое ему макетом `StackLayout`. Выполнять эти вычисления несколько раз слишком затратно.

Эквивалентный код на C# выглядит так:

```csharp
public ExpansionPageCS()
{
    Title = "Expansion demo";
    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children =
        {
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
        }
    };
}
```

> [!IMPORTANT]
> Когда все пространство в [`StackLayout`](xref:Xamarin.Forms.StackLayout) занято, параметр расширения ни на что не влияет.

Дополнительные сведения о выравнивании и расширении см. [в разделе Параметры Xamarin.Forms макета в ](layout-options.md).

## <a name="nested-stacklayout-objects"></a>Вложенные объекты StackLayout

[`StackLayout`](xref:Xamarin.Forms.StackLayout)Можно использовать в качестве родительского макета, содержащего вложенные дочерние `StackLayout` объекты или другие дочерние макеты.

В следующем коде XAML показан пример вложенных [`StackLayout`](xref:Xamarin.Forms.StackLayout) объектов:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.CombinedStackLayoutPage"
             Title="Combined StackLayouts demo">
    <StackLayout Margin="20">
        ...
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Red" />
                <Label Text="Red"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Yellow" />
                <Label Text="Yellow"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Blue" />
                <Label Text="Blue"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        ...
    </StackLayout>
</ContentPage>
```

В этом примере родительский объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) содержит вложенные `StackLayout` объекты внутри [`Frame`](xref:Xamarin.Forms.Frame) объектов. Родительский объект `StackLayout` ориентирован вертикально, а дочерние `StackLayout` объекты ориентированы горизонтально:

[![Снимок экрана вложенных объектов StackLayout](stacklayout-images/combined.png "Вложенные Стакклайаутс")](stacklayout-images/combined-large.png#lightbox "Вложенные Стакклайаутс")

> [!IMPORTANT]
> Чем глубже вы вкладывает [`StackLayout`](xref:Xamarin.Forms.StackLayout) объекты и другие макеты, тем больше вложенных макетов влияет на производительность. Дополнительные сведения см. [в разделе Выбор правильного макета](~/xamarin-forms/deploy-test/performance.md#choose-the-correct-layout).

Эквивалентный код на C# выглядит так:

```csharp
public class CombinedStackLayoutPageCS : ContentPage
{
    public CombinedStackLayoutPageCS()
    {
        Title = "Combined StackLayouts demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Primary colors" },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Red },
                            new Label { Text = "Red", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Yellow },
                            new Label { Text = "Yellow", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Blue },
                            new Label { Text = "Blue", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                // ...
            }
        };
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации StackLayout (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-stacklayoutdemos)
- [Параметры макета в Xamarin.Forms](layout-options.md)
- [Выбор Xamarin.Forms макета](choose-layout.md)
- [Улучшение Xamarin.Forms производительности приложения](~/xamarin-forms/deploy-test/performance.md)