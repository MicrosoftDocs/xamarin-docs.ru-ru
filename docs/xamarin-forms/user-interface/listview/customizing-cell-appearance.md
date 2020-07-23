---
title: Настройка внешнего вида ячеек ListView
description: В этой статье рассматриваются варианты представления данных в Xamarin.Forms приложениях, а также удобство использования элемента управления ListView.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 05a001d3b49f38b2cb5306d8a19a08b4f8392425
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935568"
---
# <a name="customizing-listview-cell-appearance"></a>Настройка внешнего вида ячеек ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) Класс используется для представления прокручиваемых списков, которые можно настроить с помощью `ViewCell` элементов. `ViewCell`Элемент может отображать текст и изображения, указывать состояние true/false и принимать входные данные пользователя.

## <a name="built-in-cells"></a>Встроенные ячейки
Xamarin.Formsпоставляется со встроенными ячейками, которые работают во многих приложениях:

- [`TextCell`](#textcell)элементы управления используются для отображения текста с необязательной второй строкой для подробного текста.
- [`ImageCell`](#imagecell)элементы управления похожи на `TextCell` s, но включают изображение слева от текста.
- `SwitchCell`элементы управления используются для представления и записи состояния "вкл./выкл" или "истина/ложь".
- `EntryCell`элементы управления используются для представления текстовых данных, которые пользователь может изменять.

[`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell) [`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell) Элементы управления и чаще используются в контексте [`TableView`](~/xamarin-forms/user-interface/tableview.md) .

### <a name="textcell"></a>текстцелл

[`TextCell`](xref:Xamarin.Forms.TextCell)ячейка для отображения текста, при необходимости со второй строкой в виде подробного текста. На следующем снимке экрана показаны `TextCell` элементы в iOS и Android:

![Пример Текстцелл по умолчанию](customizing-cell-appearance-images/text-cell-default.png)

Текстцеллс отображаются как собственные элементы управления во время выполнения, поэтому производительность очень хороша по сравнению с настраиваемым `ViewCell` . Текстцеллс можно настраивать, позволяя задавать следующие свойства:

- `Text`&ndash;текст, отображаемый в первой строке крупным шрифтом.
- `Detail`&ndash;текст, отображаемый под первой строкой с меньшим шрифтом.
- `TextColor`&ndash;Цвет текста.
- `DetailColor`&ndash;Цвет текста сведений

На следующем снимке экрана показаны `TextCell` элементы с настроенными свойствами цвета:

![Пример настраиваемого Текстцелл](customizing-cell-appearance-images/text-cell-custom.png)

### <a name="imagecell"></a>имажецелл

[`ImageCell`](xref:Xamarin.Forms.ImageCell), например `TextCell` , может использоваться для отображения текста и дополнительного текста подробностей, а также обеспечивает высокую производительность, используя собственные элементы управления платформы. `ImageCell`отличается от `TextCell` в тем, что отображает изображение слева от текста.

На следующем снимке экрана показаны `ImageCell` элементы в iOS и Android: ![Пример Имажецелл по умолчанию](customizing-cell-appearance-images/image-cell-default.png "Пример Имажецелл по умолчанию")

`ImageCell`полезен, если необходимо отобразить список данных с визуальным аспектом, например со списком контактов или фильмов. `ImageCell`можно настраивать, что позволяет задать следующие значения:

- `Text`&ndash;текст, отображаемый в первой строке крупным шрифтом
- `Detail`&ndash;текст, отображаемый под первой строкой, с меньшим шрифтом
- `TextColor`&ndash;Цвет текста
- `DetailColor`&ndash;Цвет текста сведений
- `ImageSource`&ndash;изображение, отображаемое рядом с текстом

На следующем снимке экрана показаны `ImageCell` элементы с настроенными свойствами цвета: !["Пользовательский пример имажецелл"](customizing-cell-appearance-images/image-cell-custom.png "Пример настраиваемого Имажецелл") .

## <a name="custom-cells"></a>Пользовательские ячейки
Пользовательские ячейки позволяют создавать макеты ячеек, которые не поддерживаются встроенными ячейками. Например, может потребоваться представлять ячейку с двумя метками, имеющими одинаковый вес. Может `TextCell` быть недостаточно, поскольку `TextCell` имеет одну метку, которая меньше. Большинство настроек ячеек добавляют дополнительные данные только для чтения (например, дополнительные метки, изображения или другие отображаемые сведения).

Все пользовательские ячейки должны быть производными от того [`ViewCell`](xref:Xamarin.Forms.ViewCell) же базового класса, который используется всеми встроенными типами ячеек.

Xamarin.Formsпредлагает [поведение кэширования](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy) для `ListView` элемента управления, которое может улучшить производительность прокрутки для некоторых типов пользовательских ячеек.

На следующем снимке экрана показан пример пользовательской ячейки:

!["Пример настраиваемой ячейки"](customizing-cell-appearance-images/custom-cell.png "Пример пользовательской ячейки")

### <a name="xaml"></a>XAML
Пользовательская ячейка, показанная на предыдущем снимке экрана, может быть создана с помощью следующего кода XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

КОД XAML работает следующим образом:

- Пользовательская ячейка вложена в объект `DataTemplate` , который находится внутри `ListView.ItemTemplate` . Это тот же процесс, что и при использовании любой встроенной ячейки.
- `ViewCell`Тип пользовательской ячейки. Дочерний `DataTemplate` элемент элемента должен иметь класс или быть производным от `ViewCell` класса.
- Внутри `ViewCell` макет может управляться любым Xamarin.Forms макетом. В этом примере макет управляется `StackLayout` , что позволяет настроить цвет фона.

> [!NOTE]
> Любое свойство `StackLayout` , которое является связываемым, может быть привязано внутри пользовательской ячейки. Однако эта возможность не показана в примере XAML.

### <a name="code"></a>Код

Пользовательскую ячейку также можно создать в коде. Во-первых, должен быть создан пользовательский класс, производный от класса `ViewCell` .

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

В конструкторе страницы свойству ListView присваивается значение `ItemTemplate` `DataTemplate` `CustomCell` типа с указанным типом:

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

### <a name="binding-context-changes"></a>Изменения контекста привязки

При привязке к экземплярам пользовательских типов ячеек [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) элементы управления пользовательского интерфейса, отображающие `BindableProperty` значения, должны использовать [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) Переопределение, чтобы задать отображение данных в каждой ячейке, а не в конструкторе ячейки, как показано в следующем примере кода:

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name
    {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age
    {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location
    {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null)
        {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

[`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged)Переопределение будет вызываться при [`BindingContextChanged`](xref:Xamarin.Forms.BindableObject.BindingContextChanged) срабатывании события в ответ на значение [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) изменяемого свойства. Таким образом, при `BindingContext` изменении элементы управления пользовательского интерфейса, отображающие [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) значения, должны задать свои данные. Обратите внимание, что `BindingContext` необходимо проверить `null` значение, так как оно может быть задано Xamarin.Forms для сборки мусора, что, в свою очередь, приведет к `OnBindingContextChanged` вызову переопределения.

Кроме того, элементы управления пользовательского интерфейса могут привязываться к [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) экземплярам для вывода их значений, что устраняет необходимость переопределения `OnBindingContextChanged` метода.

> [!NOTE]
> При переопределении `OnBindingContextChanged` Убедитесь, что метод базового класса `OnBindingContextChanged` вызывается таким образом, чтобы зарегистрированные делегаты получили `BindingContextChanged` событие.

В XAML привязка настраиваемого типа ячейки к данным может быть достигнута, как показано в следующем примере кода:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Это привязывает `Name` свойства, `Age` и, которые можно привязать к `Location` `CustomCell` экземпляру, к `Name` `Age` свойствам, и `Location` каждого объекта в базовой коллекции.

Эквивалентная привязка в C# показана в следующем примере кода:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView
{
    ItemsSource = people,
    ItemTemplate = customCell
};
```

В iOS и Android, если элемент [`ListView`](xref:Xamarin.Forms.ListView) является вторичным и Пользовательская ячейка использует пользовательский модуль подготовки отчетов, пользовательский модуль подготовки отчетов должен правильно реализовать уведомление об изменении свойства. Когда ячейки повторно используются, их значения свойств изменятся при обновлении контекста привязки до значения доступной ячейки с `PropertyChanged` вызываемыми событиями. Дополнительные сведения см. [в разделе Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Дополнительные сведения о перезапуске ячеек см. в разделе [стратегия кэширования](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy).

## <a name="related-links"></a>Связанные ссылки

- [Встроенные ячейки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Пользовательские ячейки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Контекст привязки изменен (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
