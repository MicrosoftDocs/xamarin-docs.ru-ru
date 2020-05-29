---
title: Xamarin.FormsКлассы стилей
description: Xamarin.Formsклассы стилей позволяют применять к элементу управления несколько стилей, не прибегая к наследованию стиля.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2afb06c2d97e6f15c2041b9c2e9cad092b13d90d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138769"
---
# <a name="xamarinforms-style-classes"></a>Xamarin.FormsКлассы стилей

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Классы стилей Xamarin. Forms позволяют применять к элементу управления несколько стилей, не прибегая к наследованию стиля._

## <a name="create-style-classes"></a>Создание классов стилей

Класс стиля можно создать, задав [`Class`](xref:Xamarin.Forms.Style.Class) свойству значение [`Style`](xref:Xamarin.Forms.Style) `string` , которое представляет имя класса. Преимущество этого предложения при определении явного стиля с помощью `x:Key` атрибута заключается в том, что к можно применить несколько классов стилей [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

> [!IMPORTANT]
> Несколько стилей могут совместно использовать одно и то же имя класса при условии, что они предназначены для разных типов. Это позволяет использовать несколько классов стилей, которые идентичны именам, для различных типов.

В следующем примере показаны три [`BoxView`](xref:Xamarin.Forms.BoxView) класса стилей и [`VisualElement`](xref:Xamarin.Forms.VisualElement) класс Style:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="BoxView"
               Class="Separator">
            <Setter Property="BackgroundColor"
                    Value="#CCCCCC" />
            <Setter Property="HeightRequest"
                    Value="1" />
        </Style>

        <Style TargetType="BoxView"
               Class="Rounded">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="10" />
        </Style>    

        <Style TargetType="BoxView"
               Class="Circle">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="WidthRequest"
                    Value="100" />
            <Setter Property="HeightRequest"
                    Value="100" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="50" />
        </Style>

        <Style TargetType="VisualElement"
               Class="Rotated"
               ApplyToDerivedTypes="true">
            <Setter Property="Rotation"
                    Value="45" />
        </Style>        
    </ContentPage.Resources>
</ContentPage>
```

`Separator` `Rounded` Классы стилей, и `Circle` каждый задают [`BoxView`](xref:Xamarin.Forms.BoxView) свойства для конкретных значений.

`Rotated`Класс Style имеет объект [`TargetType`](xref:Xamarin.Forms.Style.TargetType) [`VisualElement`](xref:Xamarin.Forms.VisualElement) , который означает, что его можно применять только к `VisualElement` экземплярам. Однако его [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) свойство имеет значение `true` , которое гарантирует, что его можно применить к любым элементам управления, производным от `VisualElement` , например [`BoxView`](xref:Xamarin.Forms.BoxView) . Дополнительные сведения о применении стиля к производному типу см. в разделе [применение стиля к производным типам](implicit.md#apply-a-style-to-derived-types).

Эквивалентный код на C# выглядит так:

```csharp
var separatorBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Separator",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#CCCCCC")
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 1
        }
    }
};

var roundedBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Rounded",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 10
        }
    }
};

var circleBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Circle",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = VisualElement.WidthRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 50
        }
    }
};

var rotatedVisualElementStyle = new Style(typeof(VisualElement))
{
    Class = "Rotated",
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.RotationProperty,
            Value = 45
        }
    }
};

Resources = new ResourceDictionary
{
    separatorBoxViewStyle,
    roundedBoxViewStyle,
    circleBoxViewStyle,
    rotatedVisualElementStyle
};
```

## <a name="consume-style-classes"></a>Использование классов стилей

Классы стилей можно использовать, установив [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) свойство элемента управления, имеющее тип `IList<string>` , в список имен классов стилей. Будут применены классы стилей при условии, что тип элемента управления совпадает с [`TargetType`](xref:Xamarin.Forms.Style.TargetType) классами стилей.

В следующем примере показаны три [`BoxView`](xref:Xamarin.Forms.BoxView) экземпляра, каждый из которых устанавливается в различные классы стилей:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <BoxView StyleClass="Separator" />       
        <BoxView WidthRequest="100"
                 HeightRequest="100"
                 HorizontalOptions="Center"
                 StyleClass="Rounded, Rotated" />
        <BoxView HorizontalOptions="Center"
                 StyleClass="Circle" />
    </StackLayout>
</ContentPage>    
```

В этом примере первый [`BoxView`](xref:Xamarin.Forms.BoxView) стиль является разделителем строк, а третий `BoxView` — циклическим. Ко второму `BoxView` применены два класса стилей, которые предоставляют скругленные углы и поворачивают его в 45 градусов:

![Боксвиевс с стилями с помощью классов стилей](style-class-images/boxviews.png)

> [!IMPORTANT]
> К элементу управления можно применить несколько классов стилей, поскольку [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) свойство имеет тип `IList<string>` . В этом случае классы стилей применяются в порядке возрастания списка. Поэтому, если несколько классов стилей устанавливают одинаковые свойства, приоритет будет иметь свойство класса Style, находящийся в самом высшем положении списка.

Эквивалентный код на C# выглядит так:

```csharp
...
Content = new StackLayout
{
    Children =
    {
        new BoxView { StyleClass = new [] { "Separator" } },
        new BoxView { WidthRequest = 100, HeightRequest = 100, HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Rounded", "Rotated" } },
        new BoxView { HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Circle" } }
    }
};
```

## <a name="related-links"></a>Связанные ссылки

- [Базовые стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
