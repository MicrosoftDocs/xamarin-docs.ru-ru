---
title: Неявные стили вXamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3fb6ea40ced93103ec9cc92fa707f68c674d7826
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139013"
---
# <a name="implicit-styles-in-xamarinforms"></a>Неявные стили вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Неявный стиль — это тот, который используется всеми элементами управления одного и того же TargetType, не требуя, чтобы каждый элемент управления ссылался на стиль._

## <a name="create-an-implicit-style-in-xaml"></a>Создание неявного стиля в XAML

Чтобы объявить объект на [`Style`](xref:Xamarin.Forms.Style) уровне страницы, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) необходимо добавить на страницу, а затем `Style` в объект включить одно или несколько объявлений `ResourceDictionary` . `Style`Делается *неявным* , если не указан `x:Key` атрибут. Затем стиль будет применяться к визуальным элементам, соответствующим `TargetType` точно, но не к элементам, которые являются производными от этого `TargetType` значения.

В следующем примере кода показан *неявный* стиль, объявленный в XAML на странице `ResourceDictionary` и примененный к [`Entry`](xref:Xamarin.Forms.Entry) экземплярам страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)Определяет один *неявный* стиль, применяемый к [`Entry`](xref:Xamarin.Forms.Entry) экземплярам страницы. `Style`Используется для отображения синего текста на желтом фоне, а также для настройки других параметров внешнего вида. `Style`Добавляется в страницу [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) без указания `x:Key` атрибута. Таким образом, `Style` атрибут применяется ко всем `Entry` экземплярам неявным образом, так как они [`TargetType`](xref:Xamarin.Forms.Style.TargetType) точно соответствуют свойству `Style` . Однако `Style` не применяется к `CustomEntry` экземпляру, который является подклассом `Entry` . Результат показан на следующих снимках экрана.

[![Пример неявных стилей](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

Кроме того, четвертый [`Entry`](xref:Xamarin.Forms.Entry) метод переопределяет [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) Свойства и неявного стиля на различные `Color` значения.

### <a name="create-an-implicit-style-at-the-control-level"></a>Создание неявного стиля на уровне элемента управления

Помимо создания *неявных* стилей на уровне страницы их также можно создать на уровне элемента управления, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В этом примере *неявное* значение [`Style`](xref:Xamarin.Forms.Style) присваивается [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) коллекции [`StackLayout`](xref:Xamarin.Forms.StackLayout) элемента управления. После этого *неявный* стиль можно применить к элементу управления и его дочерним элементам.

Сведения о создании стилей в приложении [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) см. в разделе [глобальные стили](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-implicit-style-in-c35"></a>Создание неявного стиля в C&#35;

[`Style`](xref:Xamarin.Forms.Style)экземпляры можно добавить в [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) коллекцию страницы в C#, создав новый объект [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , а затем добавив `Style` экземпляры в `ResourceDictionary` , как показано в следующем примере кода:

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

Конструктор определяет один *неявный* стиль, который применяется к [`Entry`](xref:Xamarin.Forms.Entry) экземплярам страницы. `Style`Используется для отображения синего текста на желтом фоне, а также для настройки других параметров внешнего вида. `Style`Добавляется в страницу [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) без указания `key` строки. Таким образом, `Style` атрибут применяется ко всем `Entry` экземплярам неявным образом, так как они [`TargetType`](xref:Xamarin.Forms.Style.TargetType) точно соответствуют свойству `Style` . Однако `Style` не применяется к `CustomEntry` экземпляру, который является подклассом `Entry` .

## <a name="apply-a-style-to-derived-types"></a>Применить стиль к производным типам

[`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes)Свойство позволяет применять стиль к элементам управления, производным от базового типа, на который ссылается [`TargetType`](xref:Xamarin.Forms.Style.TargetType) свойство. Таким образом, задание этого свойства `true` позволяет использовать один стиль для нескольких типов при условии, что типы являются производными от базового типа, указанного в `TargetType` свойстве.

В следующем примере показан неявный стиль, который устанавливает красный цвет фона [`Button`](xref:Xamarin.Forms.Button) для экземпляров:

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

Размещение этого стиля на уровне страницы приведет к [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) его применению ко всем [`Button`](xref:Xamarin.Forms.Button) экземплярам на странице, а также ко всем элементам управления, производным от `Button` . Однако если [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) свойство осталось неопределенным, стиль будет применен только к `Button` экземплярам.

Эквивалентный код на C# выглядит так:

```csharp
var buttonStyle = new Style(typeof(Button))
{
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.Red
        }
    }
};

Resources = new ResourceDictionary { buttonStyle };
```

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Базовые стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Работа со стилями (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Стиль](xref:Xamarin.Forms.Style)
- [Setter](xref:Xamarin.Forms.Setter)
