---
title: CollectionView Емптивиев Xamarin. Forms
description: В CollectionView можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: d5a22d110e52a397827fb451bc16872b72293755
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517480"
---
# <a name="xamarinforms-collectionview-emptyview"></a>CollectionView Емптивиев Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет следующие свойства, которые можно использовать для предоставления отзывов пользователей, когда нет данных для показа:

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)`object`тип, [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) строка, привязка или представление, которые будут отображаться, если свойство имеет `null`значение, или если коллекция, указанная `ItemsSource` свойством, имеет `null` значение или пустое значение. Значение по умолчанию — `null`.
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)— шаблон, используемый для форматирования указанного `EmptyView`объекта. Значение по умолчанию — `null`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

Основные сценарии использования для установки [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) свойства отображают Отзывы пользователей, когда операция фильтрации не [`CollectionView`](xref:Xamarin.Forms.CollectionView) возвращает никаких данных, и отображает Отзывы пользователей при получении данных из веб-службы.

> [!NOTE]
> Для [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) свойства можно задать представление, которое включает в себя интерактивное содержимое, если это необходимо.

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Отображать строку, если данные недоступны

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Свойству может [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) быть присвоена строка, которая будет отображаться, если свойство имеет `null`значение, или если коллекция, указанная `ItemsSource` свойством, `null` имеет значение или является пустым. В следующем коде XAML показан пример этого сценария:

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

В результате, поскольку коллекция, привязанная к данным, `null`имеет значение, то строка, [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) заданная в качестве значения свойства, отображается следующим образом:

[![Снимок экрана: вертикальный список CollectionView с пустым представлением текста, в iOS и Android](emptyview-images/null-itemssource.png "Вертикальный список CollectionView с пустым представлением текста")](emptyview-images/null-itemssource-large.png#lightbox "Вертикальный список CollectionView с пустым представлением текста")

## <a name="display-views-when-data-is-unavailable"></a>Отображать представления, если данные недоступны

Для [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства можно задать представление, которое будет отображаться, если свойство имеет `null`значение, или если коллекция, указанная `ItemsSource` свойством, имеет значение или `null` является пустым. Это может быть одно представление или представление, содержащее несколько дочерних представлений. В следующем примере XAML показано `EmptyView` свойство, для которого задано представление, содержащее несколько дочерних представлений:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </CollectionView.EmptyView>
    </CollectionView>
</StackLayout>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new StackLayout
    {
        Children =
        {
            new Label { Text = "No results matched your filter.", ... },
            new Label { Text = "Try a broader filter?", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Когда [`SearchBar`](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, коллекция, отображаемая [`CollectionView`](xref:Xamarin.Forms.CollectionView) , фильтруется для условия поиска, хранящегося в [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) свойстве. Если операция фильтрации не возвращает данные, то в [`StackLayout`](xref:Xamarin.Forms.StackLayout) качестве значения [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) свойства будет указан набор.

[![Снимок экрана CollectionView списка с настраиваемым пустым представлением в iOS и Android](emptyview-images/filter-multiple-views.png "Вертикальный список CollectionView с настраиваемым пустым представлением")](emptyview-images/filter-multiple-views-large.png#lightbox "Вертикальный список CollectionView с настраиваемым пустым представлением")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>Отображать шаблонный пользовательский тип, если данные недоступны

Для [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства можно задать пользовательский тип, шаблон которого отображается, если свойство имеет `null`значение, или если коллекция, указанная `ItemsSource` свойством, имеет значение или `null` является пустым. [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) Свойству может быть присвоено [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) значение, определяющее внешний вид `EmptyView`. В следующем коде XAML показан пример этого сценария:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <views:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CollectionView.EmptyView>
        <CollectionView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CollectionView.EmptyViewTemplate>
    </CollectionView>
</StackLayout>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

`FilterData` Тип определяет `Filter` свойство и соответствующий [`BindableProperty`](xref:Xamarin.Forms.BindableProperty):

```csharp
public class FilterData : BindableObject
{
    public static readonly BindableProperty FilterProperty = BindableProperty.Create(nameof(Filter), typeof(string), typeof(FilterData), null);

    public string Filter
    {
        get { return (string)GetValue(FilterProperty); }
        set { SetValue(FilterProperty, value); }
    }
}
```

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Свойству задается `FilterData` объект, а данные `Filter` свойства привязываются к [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) свойству. Когда [`SearchBar`](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, коллекция, отображаемая [`CollectionView`](xref:Xamarin.Forms.CollectionView) , фильтруется для условия поиска, хранящегося в `Filter` свойстве. Если операция фильтрации не возвращает никаких данных, то [`Label`](xref:Xamarin.Forms.Label) отображается значение, заданное в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) свойстве, которое задается в качестве значения свойства.

[![Снимок экрана CollectionView списка с пустым шаблоном представления в iOS и Android](emptyview-images/emptyviewtemplate.png "Вертикальный список CollectionView с пустым шаблоном представления")](emptyview-images/emptyviewtemplate-large.png#lightbox "Вертикальный список CollectionView с пустым шаблоном представления")

> [!NOTE]
> При отображении пользовательского типа с шаблоном, когда данные недоступны, [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) свойству может быть присвоено представление, содержащее несколько дочерних представлений.

## <a name="choose-an-emptyview-at-runtime"></a>Выбор Емптивиев во время выполнения

Представления, которые будут отображаться [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) при недоступности данных, могут быть определены как [`ContentView`](xref:Xamarin.Forms.ContentView) объекты в. [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Во `EmptyView` время выполнения для свойства можно задать конкретное `ContentView`значение, основанное на определенной бизнес-логике. В следующем коде XAML показан пример этого сценария:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.Resources>
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No items to display."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CollectionView x:Name="collectionView"
                        ItemsSource="{Binding Monkeys}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    ...
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

Этот XAML определяет два [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)объекта на уровне страницы [`Switch`](xref:Xamarin.Forms.Switch) с объектом, управляющим объектом, который `ContentView` будет задан как значение [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) свойства. [`Switch`](xref:Xamarin.Forms.Switch) При переключении обработчик `OnEmptyViewSwitchToggled` событий выполняет `ToggleEmptyView` метод:

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

`ToggleEmptyView` Метод задает для [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) свойства `collectionView` объекта один из двух [`ContentView`](xref:Xamarin.Forms.ContentView) объектов [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), хранящихся в, в зависимости от значения [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) свойства. Когда [`SearchBar`](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, коллекция, отображаемая [`CollectionView`](xref:Xamarin.Forms.CollectionView) , фильтруется для условия поиска, хранящегося в [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) свойстве. Если операция фильтрации не возвращает никаких данных, то `ContentView` объект, заданный как `EmptyView` свойство, отображается следующим образом:

[![Снимок экрана CollectionViewого вертикального списка с замененными пустыми представлениями в iOS и Android](emptyview-images/swap.png "Вертикальный список CollectionView с замененными пустыми представлениями")](emptyview-images/swap-large.png#lightbox "Вертикальный список CollectionView с замененными пустыми представлениями")

Дополнительные сведения о словарях ресурсов см. в статье [словари ресурсов Xamarin. Forms](~/xamarin-forms/xaml/resource-dictionaries.md).

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>Выбор Емптивиевтемплате во время выполнения

Внешний вид [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) можно выбрать во время выполнения на основе его значения, задав для [`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) свойства [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) объект.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AdvancedTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="BasicTemplate">
            ...
        </DataTemplate>

        <controls:SearchTermDataTemplateSelector x:Key="SearchSelector"
                                                 DefaultTemplate="{StaticResource AdvancedTemplate}"
                                                 OtherTemplate="{StaticResource BasicTemplate}" />
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <CollectionView ItemsSource="{Binding Monkeys}"
                        EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                        EmptyViewTemplate="{StaticResource SearchSelector}" />
    </StackLayout>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) Свойству [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) задается свойство, а [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) свойству присваивается `SearchTermDataTemplateSelector` объект.

Когда [`SearchBar`](xref:Xamarin.Forms.SearchBar) выполняет `FilterCommand`, коллекция, отображаемая [`CollectionView`](xref:Xamarin.Forms.CollectionView) , фильтруется для условия поиска, хранящегося в [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) свойстве. Если операция фильтрации не возвращает никаких данных, то [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) выбранный `SearchTermDataTemplateSelector` объект задается как [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) свойство и отображается.

В следующем примере показан `SearchTermDataTemplateSelector` класс:

```csharp
public class SearchTermDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate OtherTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        string query = (string)item;
        return query.ToLower().Equals("xamarin") ? OtherTemplate : DefaultTemplate;
    }
}
```

`SearchTermTemplateSelector` Класс определяет `DefaultTemplate` свойства и `OtherTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых установлены разные шаблоны данных. `OnSelectTemplate` Переопределение возвращает `DefaultTemplate`, при котором пользователю выводится сообщение, если поисковый запрос не равен Xamarin. Если поисковый запрос равен Xamarin, то `OnSelectTemplate` переопределение возвращает `OtherTemplate`, которое отображает базовое сообщение для пользователя:

[![Снимок экрана: Выбор шаблона пустого представления среды выполнения CollectionView в iOS и Android](emptyview-images/datatemplateselector.png "Выбор шаблона пустого представления среды выполнения в CollectionView")](emptyview-images/datatemplateselector-large.png#lightbox "Выбор шаблона пустого представления среды выполнения в CollectionView")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Словари ресурсов Xamarin.Forms](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
