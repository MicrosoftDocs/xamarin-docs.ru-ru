---
title: Xamarin.FormsМакет CollectionView
description: По умолчанию элементы CollectionView будут отображаться в вертикальном списке. Однако можно указать вертикальные и горизонтальные списки и сетки.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 425eb7abc14fb941dbfc28219907d98558cbfabb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137440"
---
# <a name="xamarinforms-collectionview-layout"></a>Xamarin.FormsМакет CollectionView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет следующие свойства, управляющие макетом:

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)Тип [`IItemsLayout`](xref:Xamarin.Forms.IItemsLayout) — указывает макет для использования.
- [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)Тип [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy) — определяет применяемую стратегию мер элементов.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) элементы отображаются в вертикальном списке. Однако можно использовать любой из следующих макетов:

- Вертикальный список — список из одного столбца, который увеличивается по вертикали при добавлении новых элементов.
- Горизонтальный список — один список строк, который увеличивается горизонтально по мере добавления новых элементов.
- Вертикальная сетка — сетка с несколькими столбцами, которая растет вертикально по мере добавления новых элементов.
- Горизонтальная сетка — многострочная сетка, которая растет горизонтально по мере добавления новых элементов.

Эти макеты можно указать, задав [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) для свойства класс, производный от [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) класса. Этот класс определяет следующие свойства:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)Тип [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) задает направление, в котором [`CollectionView`](xref:Xamarin.Forms.CollectionView) добавляются элементы расширения по мере добавления.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment), типа [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) , задает способ выравнивания точек привязки по элементам.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)Тип [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) — задает поведение точек привязки при прокрутке.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных. Дополнительные сведения о точках привязки см. в разделе [точки привязки](scrolling.md#snap-points) в [ Xamarin.Forms CollectionView прокрутки](scrolling.md) .

[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)Перечисление определяет следующие члены:

- `Vertical`Указывает, что [`CollectionView`](xref:Xamarin.Forms.CollectionView) будет расширяться по вертикали по мере добавления элементов.
- `Horizontal`Указывает, что [`CollectionView`](xref:Xamarin.Forms.CollectionView) будет разворачиваться горизонтально по мере добавления элементов.

`LinearItemsLayout`Класс наследует от [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) класса и определяет `ItemSpacing` свойство типа `double` , представляющее пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0. `LinearItemsLayout`Класс также определяет статические `Vertical` `Horizontal` члены и. Эти члены можно использовать для создания вертикальных или горизонтальных списков соответственно. Кроме того, `LinearItemsLayout` можно создать объект, указав в [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) качестве аргумента член перечисления.

[`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout)Класс наследует от [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) класса и определяет следующие свойства:

- `VerticalItemSpacing`Тип `double` , представляющий вертикальное пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0.
- `HorizontalItemSpacing`Тип `double` , представляющий горизонтальное пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0.
- `Span`Тип `int` , который представляет число столбцов или строк, отображаемых в сетке. Значение этого свойства по умолчанию равно 1, а его значение всегда должно быть больше или равно 1.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)использует собственные обработчики макетов для выполнения макета.

## <a name="vertical-list"></a>Вертикальный список

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображает свои элементы в вертикальном макете списка. Поэтому нет необходимости задавать [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) свойство для использования этого макета:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Однако для полноты [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно задать отображение элементов в вертикальном списке, задав [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) для его свойства значение `VerticalList` :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

Кроме того, это можно сделать, задав [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) для свойства `LinearItemsLayout` объект, указав в `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) качестве `Orientation` значения свойства элемент перечисления:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

В результате получается список из одного столбца, который по вертикали увеличивается по мере добавления новых элементов:

[![Снимок экрана с вертикальным макетом списка CollectionView на iOS и Android](layout-images/vertical-list.png "Вертикальный макет списка CollectionView")](layout-images/vertical-list-large.png#lightbox "Вертикальный макет списка CollectionView")

## <a name="horizontal-list"></a>Горизонтальный список

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может отображать свои элементы в горизонтальном списке, присвоив [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) свойству значение `HorizontalList` :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="HorizontalList">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Кроме того, этот макет можно также выполнить, задав [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) для свойства `LinearItemsLayout` объект, указав в `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) качестве `Orientation` значения свойства элемент перечисления:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

В результате получается один список строк, который увеличивается горизонтально по мере добавления новых элементов:

[![Снимок экрана с макетом горизонтального списка CollectionView в iOS и Android](layout-images/horizontal-list.png "CollectionView горизонтальный макет списка")](layout-images/horizontal-list-large.png#lightbox "CollectionView горизонтальный макет списка")

## <a name="vertical-grid"></a>Вертикальная сетка

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может отображать свои элементы в вертикальной сетке, присвоив его [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) свойству [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) объект, [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) свойство которого имеет значение `Vertical` :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="80" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

По умолчанию [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) элементы будут отображаться в одном столбце по вертикали. Однако в этом примере свойству присваивается значение `GridItemsLayout.Span` 2. В результате получается сетка из двух столбцов, которая увеличивается по вертикали при добавлении новых элементов:

[![Снимок экрана с вертикальным макетом сетки CollectionView в iOS и Android](layout-images/vertical-grid.png "Макет вертикальной сетки CollectionView")](layout-images/vertical-grid-large.png#lightbox "Макет вертикальной сетки CollectionView")

## <a name="horizontal-grid"></a>Горизонтальная сетка

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может отображать свои элементы в горизонтальной сетке, присвоив [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) свойству значение [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) объекта, [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) свойство которого имеет значение `Horizontal` :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Horizontal"
                        Span="4" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

По умолчанию элементы по горизонтали [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) будут отображаться в одной строке. Однако в этом примере свойству присваивается значение `GridItemsLayout.Span` 4. В результате получается сетка из четырех строк, которая увеличивается горизонтально по мере добавления новых элементов:

[![Снимок экрана с горизонтальным макетом сетки CollectionView в iOS и Android](layout-images/horizontal-grid.png "Горизонтальный макет сетки CollectionView")](layout-images/horizontal-grid-large.png#lightbox "Горизонтальный макет сетки CollectionView")

## <a name="headers-and-footers"></a>Верхние и нижние колонтитулы

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может представлять верхний и нижний колонтитулы, которые прокручивается с элементами списка. Верхний и нижний колонтитулы могут быть строками, представлениями или [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектами.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет следующие свойства для указания верхнего и нижнего колонтитулов:

- `Header`, типа `object` , указывает строку, привязку или представление, которое будет отображаться в начале списка.
- `HeaderTemplate`Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) указывает, `DataTemplate` используемый для форматирования `Header` .
- `Footer`, типа `object` , указывает строку, привязку или представление, которое будет отображаться в конце списка.
- `FooterTemplate`Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) указывает, `DataTemplate` используемый для форматирования `Footer` .

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

При добавлении заголовка к макету, который увеличивается горизонтально, слева направо заголовок отображается слева от списка. Аналогично, при добавлении нижнего колонтитула к макету, который увеличивается горизонтально, слева направо нижний колонтитул отображается справа от списка.

### <a name="display-strings-in-the-header-and-footer"></a>Отображение строк в верхнем и нижнем колонтитулах

`Header` `Footer` Для свойств и можно задать `string` значения, как показано в следующем примере:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="Monkeys"
                Footer="2019">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    Header = "Monkeys",
    Footer = "2019"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Этот код приводит к следующим снимкам экрана с заголовком на снимке экрана iOS и нижним колонтитулом, показанным на снимке экрана Android:

[![Снимок экрана: заголовок и нижний колонтитул CollectionView строки в iOS и Android](layout-images/header-footer-string.png "Верхний и нижний колонтитулы строки CollectionView")](layout-images/header-footer-string-large.png#lightbox "Верхний и нижний колонтитулы строки CollectionView")

### <a name="display-views-in-the-header-and-footer"></a>Отображение представлений в верхнем и нижнем колонтитулах

`Header` `Footer` Для свойств и можно задать представление. Это может быть одно представление или представление, содержащее несколько дочерних представлений. В следующем примере показаны `Header` Свойства и, `Footer` для каждого из которых задается объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) , содержащий [`Label`](xref:Xamarin.Forms.Label) объект:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.Header>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Monkeys"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Header>
    <CollectionView.Footer>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Friends of Xamarin Monkey"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Footer>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    Header = new StackLayout
    {
        Children =
        {
            new Label { Text = "Monkeys", ... }
        }
    },
    Footer = new StackLayout
    {
        Children =
        {
            new Label { Text = "Friends of Xamarin Monkey", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Этот код приводит к следующим снимкам экрана с заголовком на снимке экрана iOS и нижним колонтитулом, показанным на снимке экрана Android:

[![Снимок экрана верхнего и нижнего колонтитулов CollectionView с помощью представлений в iOS и Android](layout-images/header-footer-view.png "Верхний и нижний колонтитулы представления CollectionView")](layout-images/header-footer-view-large.png#lightbox "Верхний и нижний колонтитулы представления CollectionView")

### <a name="display-a-templated-header-and-footer"></a>Отображение шаблона верхнего и нижнего колонтитулов

`HeaderTemplate`Свойства и `FooterTemplate` можно задать для [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектов, которые используются для форматирования верхнего и нижнего колонтитулов. В этом сценарии `Header` `Footer` Свойства и должны быть привязаны к текущему источнику для применения шаблонов, как показано в следующем примере:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="{Binding .}"
                Footer="{Binding .}">
    <CollectionView.HeaderTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Monkeys"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.HeaderTemplate>
    <CollectionView.FooterTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Friends of Xamarin Monkey"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.FooterTemplate>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    HeaderTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    }),
    FooterTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    })
};
collectionView.SetBinding(ItemsView.HeaderProperty, ".");
collectionView.SetBinding(ItemsView.FooterProperty, ".");
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Этот код приводит к следующим снимкам экрана с заголовком на снимке экрана iOS и нижним колонтитулом, показанным на снимке экрана Android:

[![Снимок экрана верхнего и нижнего колонтитулов CollectionView с помощью шаблонов в iOS и Android](layout-images/header-footer-template.png "Верхний и нижний колонтитулы шаблона CollectionView")](layout-images/header-footer-template-large.png#lightbox "Верхний и нижний колонтитулы шаблона CollectionView")

## <a name="item-spacing"></a>Промежуток между элементами

По умолчанию между каждым элементом в не существует пробела [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Это поведение можно изменить, задав свойства в макете элементов, который используется `CollectionView` .

Когда [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) свойство присваивает свойству `LinearItemsLayout` объект, `LinearItemsLayout.ItemSpacing` свойству может быть присвоено `double` значение, представляющее пробел между элементами:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> `LinearItemsLayout.ItemSpacing`Свойство имеет набор обратного вызова проверки, который гарантирует, что значение свойства всегда больше или равно 0.

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

Этот код создает вертикальный список с одним столбцом с интервалом в 20 единиц между элементами:

[![Снимок экрана CollectionView с промежутком между элементами в iOS и Android](layout-images/vertical-list-spacing.png "CollectionView между элементами")](layout-images/vertical-list-spacing-large.png#lightbox "CollectionView между элементами")

Когда [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) свойство присваивает свойству [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) объект, `GridItemsLayout.VerticalItemSpacing` `GridItemsLayout.HorizontalItemSpacing` Свойства и могут быть установлены на `double` значения, представляющие пустое пространство по вертикали и горизонтали между элементами:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2"
                        VerticalItemSpacing="20"
                        HorizontalItemSpacing="30" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> `GridItemsLayout.VerticalItemSpacing`Свойства и `GridItemsLayout.HorizontalItemSpacing` имеют обратные вызовы проверки, которые гарантируют, что значения свойств всегда больше или равны 0.

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
    {
        VerticalItemSpacing = 20,
        HorizontalItemSpacing = 30
    }
};
```

Этот код приводит к вертикальной сетке с двумя столбцами с вертикальным интервалом, равным 20, между элементами и интервалом по горизонтали (30) между элементами:

[![Снимок экрана с CollectionView с промежутком между элементами на Android](layout-images/vertical-grid-spacing.png "CollectionView между элементами")](layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView между элементами")

## <a name="item-sizing"></a>Размер элемента

По умолчанию каждый элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView) принимает отдельное измерение и размер, при условии, что элементы пользовательского интерфейса в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) не задают фиксированные размеры. Это поведение, которое можно изменить, задается [`CollectionView.ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) значением свойства. Это значение свойства может быть задано одним из [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy) членов перечисления:

- `MeasureAllItems`— Каждый элемент по отдельности измеряется. Это значение по умолчанию.
- `MeasureFirstItem`— измеряется только первый элемент, и все последующие элементы получают тот же размер, что и первый элемент.

> [!IMPORTANT]
> `MeasureFirstItem`Стратегия изменения размера приводит к повышению производительности при использовании в ситуациях, когда размер элемента должен быть одинаковым для всех элементов.

В следующем примере кода показано, как задать [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) свойство:

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="dynamic-resizing-of-items"></a>Динамическое изменение размера элементов

В [`CollectionView`](xref:Xamarin.Forms.CollectionView) среде выполнения элементы в можно динамически изменять размер, изменяя связанные с макетом свойства элементов в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Например, в следующем примере кода изменяются [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) Свойства и [`Image`](xref:Xamarin.Forms.Image) объекта:

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

`OnImageTapped`Обработчик событий выполняется в ответ на [`Image`](xref:Xamarin.Forms.Image) касание объекта и изменяет размеры изображения, чтобы его было проще Просмотреть:

[![Снимок экрана CollectionView с динамическим изменением размера элемента в iOS и Android](layout-images/runtime-resizing.png "Изменение размера динамического элемента CollectionView")](layout-images/runtime-resizing-large.png#lightbox "Изменение размера динамического элемента CollectionView")

## <a name="right-to-left-layout"></a>Макет с письмом справа налево

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может Разметка содержимого в направлении справа налево путем присвоения [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) свойству значения [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) . Однако в `FlowDirection` идеале это свойство должно быть установлено на странице или в корневом макете, что приводит к тому, что все элементы на странице или в корневом макете реагируют на направление потока:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.VerticalListFlowDirectionPage"
             Title="Vertical list (RTL FlowDirection)"
             FlowDirection="RightToLeft">
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}">
            ...
        </CollectionView>
    </StackLayout>
</ContentPage>
```

По умолчанию [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) для элемента с родительским элементом имеет значение [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) . Таким образом, класс [`CollectionView`](xref:Xamarin.Forms.CollectionView) наследует `FlowDirection` значение свойства от [`StackLayout`](xref:Xamarin.Forms.StackLayout) , которое, в свою очередь, наследует `FlowDirection` значение свойства от [`ContentPage`](xref:Xamarin.Forms.ContentPage) . В результате отображается макет справа налево, показанный на следующих снимках экрана:

[![Снимок экрана с вертикальным макетом списка справа налево на CollectionView в iOS и Android](layout-images/vertical-list-rtl.png "CollectionView вертикальный макет списка справа налево")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView вертикальный макет списка справа налево")

Дополнительные сведения о направлении потока см. [в разделе локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.FormsПрокрутка CollectionView](scrolling.md)
