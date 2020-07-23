---
title: Создание Xamarin.Forms DataTemplate
description: Шаблоны данных могут создаваться как встроенные в ResourceDictionary или из пользовательского типа или соответствующего типа ячейки Xamarin.Forms. В этой статье рассматривается каждый из способов.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9ed7e485adbc13862ff6fb6af9b02c43cc6a1f10
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935646"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Создание Xamarin.Forms DataTemplate

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_Шаблоны данных могут создаваться как встроенные в ResourceDictionary или из пользовательского типа или соответствующего типа ячейки Xamarin.Forms. В этой статье рассматривается каждый из способов._

Стандартный сценарий использования [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) — отображение данных из коллекции объектов в [`ListView`](xref:Xamarin.Forms.ListView). Внешним видом данных в каждой ячейке [`ListView`](xref:Xamarin.Forms.ListView) можно управлять, присваивая свойству [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Это можно делать несколькими способами:

- [Создание встроенного шаблона DataTemplate](#creating-an-inline-datatemplate)
- [Создание шаблона DataTemplate с типом](#creating-a-datatemplate-with-a-type)
- [Создание шаблона DataTemplate как ресурса](#creating-a-datatemplate-as-a-resource)

Какой бы способ ни использовался, в результате внешний вид каждой ячейки [`ListView`](xref:Xamarin.Forms.ListView) определяется шаблоном [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), как показано на следующих снимках экрана.

![ListView с шаблоном DataTemplate](creating-images/data-template-appearance.png)

## <a name="creating-an-inline-datatemplate"></a>Создание встроенного шаблона DataTemplate

Свойству [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) может быть присвоен встроенный шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Встроенный шаблон, который является прямым потомком соответствующего свойства элемента управления, следует использовать, если нет необходимости повторно использовать шаблон данных в другом месте. Элементы шаблона `DataTemplate` определяют внешний вид каждой ячейки, как показано в следующем примере кода XAML.

```xaml
<ListView Margin="0,20,0,0">
    <ListView.ItemsSource>
        <x:Array Type="{x:Type local:Person}">
            <local:Person Name="Steve" Age="21" Location="USA" />
            <local:Person Name="John" Age="37" Location="USA" />
            <local:Person Name="Tom" Age="42" Location="UK" />
            <local:Person Name="Lucas" Age="29" Location="Germany" />
            <local:Person Name="Tariq" Age="39" Location="UK" />
            <local:Person Name="Jane" Age="30" Location="USA" />
        </x:Array>
    </ListView.ItemsSource>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Label Text="{Binding Name}" FontAttributes="Bold" />
                    <Label Grid.Column="1" Text="{Binding Age}" />
                    <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Дочерний объект встроенного шаблона [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) должен иметь тип [`Cell`](xref:Xamarin.Forms.Cell) или производный от него тип. В этом примере используется класс [`ViewCell`](xref:Xamarin.Forms.ViewCell), производный от `Cell`. Управление макетом внутри `ViewCell` осуществляется здесь с помощью [`Grid`](xref:Xamarin.Forms.Grid). Макет `Grid` содержит три экземпляра [`Label`](xref:Xamarin.Forms.Label), свойства [`Text`](xref:Xamarin.Forms.Label.Text) которых привязаны к соответствующим свойствам каждого объекта `Person` в коллекции.

Эквивалентный код на языке C# показан в следующем примере:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
    public WithDataTemplatePageCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        var personDataTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            ...
            var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
            var ageLabel = new Label();
            var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

            nameLabel.SetBinding(Label.TextProperty, "Name");
            ageLabel.SetBinding(Label.TextProperty, "Age");
            locationLabel.SetBinding(Label.TextProperty, "Location");

            grid.Children.Add(nameLabel);
            grid.Children.Add(ageLabel, 1, 0);
            grid.Children.Add(locationLabel, 2, 0);

            return new ViewCell { View = grid };
        });

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemsSource = people, ItemTemplate = personDataTemplate, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

На C# встроенный шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) создается с помощью перегрузки конструктора, задающей аргумент `Func`.

## <a name="creating-a-datatemplate-with-a-type"></a>Создание шаблона DataTemplate с типом

Свойству [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) также может присваиваться шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), созданный из типа ячейки. Преимуществом этого подхода является возможность повторного использования внешнего вида, определяемого типом ячейки, в нескольких шаблонах данных в приложении. Пример такого подхода показан в следующем коде XAML.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <local:PersonCell />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

В этом случае свойству [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) присвоен шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), созданный на основе пользовательского типа, который определяет внешний вид ячейки. Пользовательский тип должен быть производным от типа [`ViewCell`](xref:Xamarin.Forms.ViewCell), как показано в следующем примере кода.

```xaml
<ViewCell xmlns="http://xamarin.com/schemas/2014/forms"
          xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
          x:Class="DataTemplates.PersonCell">
     <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.2*" />
            <ColumnDefinition Width="0.3*" />
        </Grid.ColumnDefinitions>
        <Label Text="{Binding Name}" FontAttributes="Bold" />
        <Label Grid.Column="1" Text="{Binding Age}" />
        <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
    </Grid>
</ViewCell>
```

Управление макетом внутри [`ViewCell`](xref:Xamarin.Forms.ViewCell) осуществляется здесь с помощью [`Grid`](xref:Xamarin.Forms.Grid). Макет `Grid` содержит три экземпляра [`Label`](xref:Xamarin.Forms.Label), свойства [`Text`](xref:Xamarin.Forms.Label.Text) которых привязаны к соответствующим свойствам каждого объекта `Person` в коллекции.

Эквивалентный код на языке C# показан в следующем примере.

```csharp
public class WithDataTemplatePageFromTypeCS : ContentPage
{
    public WithDataTemplatePageFromTypeCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemTemplate = new DataTemplate(typeof(PersonCellCS)), ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

На C# шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) создается с помощью перегрузки конструктора, задающей тип ячейки в качестве аргумента. Тип ячейки должен быть производным от типа [`ViewCell`](xref:Xamarin.Forms.ViewCell), как показано в следующем примере кода.

```csharp
public class PersonCellCS : ViewCell
{
    public PersonCellCS()
    {
        var grid = new Grid();
        ...
        var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        var ageLabel = new Label();
        var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

        nameLabel.SetBinding(Label.TextProperty, "Name");
        ageLabel.SetBinding(Label.TextProperty, "Age");
        locationLabel.SetBinding(Label.TextProperty, "Location");

        grid.Children.Add(nameLabel);
        grid.Children.Add(ageLabel, 1, 0);
        grid.Children.Add(locationLabel, 2, 0);

        View = grid;
    }
}
```

> [!NOTE]
> Обратите внимание на то, что в Xamarin.Forms также есть типы ячеек, которые позволяют отображать простые данные в ячейках [`ListView`](xref:Xamarin.Forms.ListView). Дополнительные сведения см. в статье [Настройка внешнего вида ячейки](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

## <a name="creating-a-datatemplate-as-a-resource"></a>Создание шаблона DataTemplate как ресурса

Шаблоны данных также можно создавать как повторно используемые объекты в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Для этого каждому объявлению присваивается уникальный атрибут `x:Key`, который предоставляет описательный ключ в `ResourceDictionary`, как показано в следующем примере кода XAML.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="personTemplate">
                 <ViewCell>
                    <Grid>
                        ...
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        ...
        <ListView ItemTemplate="{StaticResource personTemplate}" Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) присваивается свойству [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) с помощью расширения разметки `StaticResource`. Обратите внимание на то, что шаблон `DataTemplate` может определяться не только в объекте [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) страницы, но и на уровне элемента управления или приложения.

В следующем примере кода показана эквивалентная страница на языке C#:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
  public WithDataTemplatePageCS ()
  {
    ...
    var personDataTemplate = new DataTemplate (() => {
      var grid = new Grid ();
      ...
      return new ViewCell { View = grid };
    });

    Resources = new ResourceDictionary ();
    Resources.Add ("personTemplate", personDataTemplate);

    Content = new StackLayout {
      Margin = new Thickness(20),
      Children = {
        ...
        new ListView { ItemTemplate = (DataTemplate)Resources ["personTemplate"], ItemsSource = people };
      }
    };
  }
}
```

Шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) добавляется в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) с помощью метода [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)), задающего строку `Key`. По ней можно ссылаться на шаблон `DataTemplate`, который нужно извлечь.

## <a name="summary"></a>Сводка

В этой статье были представлены сведения о создании шаблонов данных как встроенных, на основе пользовательского типа или в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Встроенный шаблон следует использовать, если нет необходимости повторно использовать шаблон данных в другом месте. Шаблон данных можно использовать повторно, определив его как пользовательский тип или как ресурс на уровне элемента управления, страницы или приложения.

## <a name="related-links"></a>Связанные ссылки

- [Вид ячейки](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Шаблоны данных (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
