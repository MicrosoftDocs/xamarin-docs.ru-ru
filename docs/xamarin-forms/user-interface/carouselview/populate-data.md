---
title: Данные Карауселвиев в Xamarin. Forms
description: Карауселвиев заполняется данными путем присвоения свойству ItemsSource любой коллекции, реализующей IEnumerable.
ms.prod: xamarin
ms.assetid: 20DB2C57-CE3A-4D91-80DC-73AE361A3CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/29/2020
ms.openlocfilehash: cdd77d333ead9b4ff4d2cf29b1e36ee2f287dd22
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517401"
---
# <a name="xamarinforms-carouselview-data"></a>Данные Карауселвиев в Xamarin. Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)включает следующие свойства, которые определяют отображаемые данные и его внешний вид:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource), типа `IEnumerable`, задает коллекцию элементов для отображения и имеет значение по умолчанию `null`.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)— указывает шаблон, применяемый к каждому элементу в коллекции отображаемых элементов.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)Определяет `ItemsUpdatingScrollMode` свойство, представляющее поведение прокрутки `CarouselView` при добавлении новых элементов к нему. Дополнительные сведения об этом свойстве см. в разделе [управление позицией прокрутки при добавлении новых элементов](scrolling.md#control-scroll-position-when-new-items-are-added).

[`CarouselView`](xref:Xamarin.Forms.CarouselView)поддерживает добавочную виртуализацию данных при прокрутке пользователем. Дополнительные сведения см. в статье [добавочная загрузка данных](#load-data-incrementally).

## <a name="populate-a-carouselview-with-data"></a>Заполнение Карауселвиев данными

[`CarouselView`](xref:Xamarin.Forms.CarouselView) Заполняется данными путем установки его [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства в любую коллекцию, реализующую `IEnumerable`. Элементы можно добавлять в XAML путем инициализации `ItemsSource` свойства из массива строк:

```xaml
<CarouselView>
    <CarouselView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Baboon</x:String>
            <x:String>Capuchin Monkey</x:String>
            <x:String>Blue Monkey</x:String>
            <x:String>Squirrel Monkey</x:String>
            <x:String>Golden Lion Tamarin</x:String>
            <x:String>Howler Monkey</x:String>
            <x:String>Japanese Macaque</x:String>
        </x:Array>
    </CarouselView.ItemsSource>
</CarouselView>
```

> [!NOTE]
> Обратите внимание на то, что для элемента `x:Array` требуется атрибут `Type`, указывающий тип элементов в массиве.

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) Если требуется обновление при добавлении, удалении или изменении элементов в базовой коллекции, то базовая коллекция должна быть `IEnumerable` коллекцией, которая отправляет уведомления об изменении свойств, например `ObservableCollection`.

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) отображает элементы по горизонтали. На следующих снимках экрана `CarouselView` показано отображение различных строковых элементов в iOS и Android.

[![Снимок экрана Карауселвиев, содержащий текстовые элементы, в iOS и Android](populate-data-images/text.png "Текстовые элементы в Карауселвиев")](populate-data-images/text-large.png#lightbox "Текстовые элементы в Карауселвиев")

Сведения о том, как изменить [`CarouselView`](xref:Xamarin.Forms.CarouselView) ориентацию, см. в разделе о [макете Xamarin. Forms карауселвиев](layout.md). Сведения о том `CarouselView`, как определить внешний вид каждого элемента в, см. в разделе [Определение внешнего вида элемента](#define-item-appearance).

### <a name="data-binding"></a>привязка данных,

[`CarouselView`](xref:Xamarin.Forms.CarouselView)может заполняться данными с помощью привязки данных для привязки своего [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства к `IEnumerable` коллекции. В XAML это достигается с помощью расширения `Binding` разметки:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}" />
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

В этом примере данные [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства привязываются к `Monkeys` свойству подключенного ViewModel.

> [!NOTE]
> Скомпилированные привязки можно включить для повышения производительности привязки данных в приложениях Xamarin. Forms. Дополнительные сведения см. в статье [Скомпилированные привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Определение внешнего вида элемента

Внешний вид каждого элемента в [`CarouselView`](xref:Xamarin.Forms.CarouselView) может быть определен путем присвоения [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) свойству значения. [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    Label nameLabel = new Label { ... };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Image image = new Image { ... };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label locationLabel = new Label { ... };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Label detailsLabel = new Label { ... };
    detailsLabel.SetBinding(Label.TextProperty, "Details");

    StackLayout stackLayout = new StackLayout
    {
        Children = { nameLabel, image, locationLabel, detailsLabel }
    };

    Frame frame = new Frame { ... };
    StackLayout rootStackLayout = new StackLayout
    {
        Children = { frame }
    };

    return rootStackLayout;
});
```

Элементы, указанные в параметре, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяют внешний вид каждого элемента в `CarouselView`. `DataTemplate` В этом примере макет в управляется с помощью [`StackLayout`](xref:Xamarin.Forms.StackLayout), а данные отображаются с [`Image`](xref:Xamarin.Forms.Image) объектом и тремя [`Label`](xref:Xamarin.Forms.Label) объектами, которые привязываются к свойствам `Monkey` класса:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

На следующих снимках экрана показан результат создания шаблонов каждого элемента:

[![Снимок экрана Карауселвиев, где для каждого элемента используется шаблон, в iOS и Android](populate-data-images/datatemplate.png "Элементы шаблона в Карауселвиев")](populate-data-images/datatemplate-large.png#lightbox "Элементы шаблона в Карауселвиев")

Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в [`CarouselView`](xref:Xamarin.Forms.CarouselView) можно выбрать во время выполнения на основе значения элемента, задав для [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) свойства [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) объект.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls"
             x:Class="CarouselViewDemos.Views.HorizontalLayoutDataTemplateSelectorPage">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CarouselView ItemsSource="{Binding Monkeys}"
                  ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Для [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) свойства задается `MonkeyDataTemplateSelector` объект. В следующем примере показан `MonkeyDataTemplateSelector` класс:

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

`MonkeyDataTemplateSelector` Класс определяет `AmericanMonkey` свойства и `OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых установлены разные шаблоны данных. `OnSelectTemplate` Переопределение возвращает `AmericanMonkey` шаблон, если имя обезьяны содержит "America". Если имя обезьяны не содержит "America", `OnSelectTemplate` переопределение возвращает `OtherMonkey` шаблон, который отображает данные, выделенные серым цветом:

[![Снимок экрана выбора шаблона элемента среды выполнения Карауселвиев в iOS и Android](populate-data-images/datatemplateselector.png "Выбор шаблона элемента среды выполнения в Карауселвиев")](populate-data-images/datatemplateselector-large.png#lightbox "Выбор шаблона элемента среды выполнения в Карауселвиев")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

> [!IMPORTANT]
> При использовании [`CarouselView`](xref:Xamarin.Forms.CarouselView)никогда не устанавливайте для корневого элемента [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектов значение `ViewCell`. Это приведет к возникновению исключения, так как `CarouselView` не содержит концепцию ячеек.

## <a name="display-indicators"></a>индикаторы отображения

Индикаторы, представляющие количество элементов и текущую позиции в `CarouselView`, могут отображаться рядом с. `CarouselView` Это можно сделать с помощью `IndicatorView` элемента управления:

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

В этом примере объект `IndicatorView` отображается под элементом `CarouselView`и с индикатором для каждого элемента в. `CarouselView` `IndicatorView` Заполняется данными путем присвоения `CarouselView.IndicatorView` свойству `IndicatorView` объекта значения. Каждый индикатор представляет собой светло-серый круг, а индикатор, представляющий текущий элемент в, `CarouselView` темно-серый:

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](populate-data-images/indicators.png "Индикаторвиев круги")](populate-data-images/indicators-large.png#lightbox "Индикаторвиев круги")

> [!IMPORTANT]
> Установка `CarouselView.IndicatorView` свойства приводит к привязке `IndicatorView.Position` свойства к `CarouselView.Position` свойству и привязке `IndicatorView.ItemsSource` свойства к `CarouselView.ItemsSource` свойству.

Дополнительные сведения о индикаторах см. в разделе [Xamarin. Forms индикаторвиев](~/xamarin-forms/user-interface/indicatorview.md).

## <a name="context-menus"></a>Контекстные меню

[`CarouselView`](xref:Xamarin.Forms.CarouselView)поддерживает контекстные меню для элементов данных с помощью `SwipeView`, которое открывает контекстное меню с помощью жеста прокрутки. `SwipeView` — Это контейнерный элемент управления, который создает оболочку вокруг элемента содержимого и предоставляет элементы контекстного меню для этого элемента содержимого. Таким образом, контекстные меню реализуются для `CarouselView` , создавая объект `SwipeView` , который определяет содержимое, вокруг `SwipeView` которого выполняется оболочка, и элементы контекстного меню, которые выводятся с помощью жеста прокрутки. Это достигается путем добавления `SwipeView` в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , который определяет внешний вид каждого элемента данных в: `CarouselView`

```xaml
<CarouselView x:Name="carouselView"
              ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                    <Frame HasShadow="True"
                           BorderColor="DarkGray"
                           CornerRadius="5"
                           Margin="20"
                           HeightRequest="300"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand">
                        <SwipeView>
                            <SwipeView.TopItems>
                                <SwipeItems>
                                    <SwipeItem Text="Favorite"
                                               IconImageSource="favorite.png"
                                               BackgroundColor="LightGreen"
                                               Command="{Binding Source={x:Reference carouselView}, Path=BindingContext.FavoriteCommand}"
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.TopItems>
                            <SwipeView.BottomItems>
                                <SwipeItems>
                                    <SwipeItem Text="Delete"
                                               IconImageSource="delete.png"
                                               BackgroundColor="LightPink"
                                               Command="{Binding Source={x:Reference carouselView}, Path=BindingContext.DeleteCommand}"
                                               CommandParameter="{Binding}" />
                                </SwipeItems>
                            </SwipeView.BottomItems>
                            <StackLayout>
                                <!-- Define item appearance -->
                            </StackLayout>
                        </SwipeView>
                    </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    StackLayout stackLayout = new StackLayout();
    Frame frame = new Frame { ... };

    SwipeView swipeView = new SwipeView();
    SwipeItem favoriteSwipeItem = new SwipeItem
    {
        Text = "Favorite",
        IconImageSource = "favorite.png",
        BackgroundColor = Color.LightGreen
    };
    favoriteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.FavoriteCommand", source: carouselView));
    favoriteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    SwipeItem deleteSwipeItem = new SwipeItem
    {
        Text = "Delete",
        IconImageSource = "delete.png",
        BackgroundColor = Color.LightPink
    };
    deleteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.DeleteCommand", source: carouselView));
    deleteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    swipeView.TopItems = new SwipeItems { favoriteSwipeItem };
    swipeView.BottomItems = new SwipeItems { deleteSwipeItem };

    StackLayout swipeViewStackLayout = new StackLayout { ... };
    swipeView.Content = swipeViewStackLayout;
    frame.Content = swipeView;
    stackLayout.Children.Add(frame);

    return stackLayout;
});
```

В этом `SwipeView` примере содержимое — [`StackLayout`](xref:Xamarin.Forms.StackLayout) это, определяющее внешний вид каждого элемента, окруженного [`Frame`](xref:Xamarin.Forms.Frame) в. [`CarouselView`](xref:Xamarin.Forms.CarouselView) Элементы прокрутки используются для выполнения действий с `SwipeView` содержимым и отображаются при прокрутке элемента управления сверху и снизу:

[![Снимок экрана: элемент контекстного меню карауселвиев внизу на снимке экрана iOS и Android](populate-data-images/swipeview-bottom.png "Карауселвиев с наименьшим пунктом контекстного меню Свипевиев")](populate-data-images/swipeview-bottom-large.png#lightbox "Карауселвиев с наименьшим пунктом контекстного меню Свипевиев")
в[![элементе меню верхнего уровня карауселвиев в iOS и Android](populate-data-images/swipeview-top.png "Карауселвиев с элементом контекстного меню Top Свипевиев")](populate-data-images/swipeview-top-large.png#lightbox "Карауселвиев с элементом контекстного меню Top Свипевиев")

`SwipeView`поддерживает четыре разных направления прокрутки с направлением считывания, определяемым направленной `SwipeItems` коллекцией, `SwipeItems` в которую добавляются объекты. По умолчанию элемент прокрутки выполняется при касании пользователем. Кроме того, после выполнения элемента считывания элементы прокрутки скрываются и `SwipeView` содержимое отображается повторно. Однако эти поведения можно изменить.

Дополнительные сведения об элементе управления `SwipeView` см. в разделе [Xamarin. Forms свипевиев](~/xamarin-forms/user-interface/swipeview.md).

## <a name="pull-to-refresh"></a>Обновление путем оттягивания

[`CarouselView`](xref:Xamarin.Forms.CarouselView)поддерживает функцию Pull для обновления с помощью `RefreshView`, которая позволяет обновлять отображаемые данные путем извлечения элементов. `RefreshView` — Это контейнерный элемент управления, предоставляющий функции обновления для своего дочернего элемента, при условии, что дочерний объект поддерживает прокручиваемое содержимое. Таким образом, запрос на обновление реализуется для `CarouselView` , задавая его в качестве дочернего элемента для `RefreshView`:

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CarouselView ItemsSource="{Binding Animals}">
        ...
    </CarouselView>
</RefreshView>
```

Эквивалентный код на C# выглядит так:

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = carouselView;
// ...
```

Когда пользователь инициирует обновление, выполняется `ICommand` `Command` свойство, определенное свойством, которое должно обновлять отображаемые элементы. Во время обновления отображается визуализация обновления, которая состоит из анимированного круга хода выполнения:

[![Снимок экрана Карауселвиев Pull-to-Refresh, в iOS и Android](populate-data-images/pull-to-refresh.png "Карауселвиев по запросу на обновление")](populate-data-images/pull-to-refresh-large.png#lightbox "Карауселвиев по запросу на обновление")

Значение `RefreshView.IsRefreshing` свойства указывает текущее состояние `RefreshView`. При активации обновления пользователем это свойство автоматически переходит в `true`. После завершения обновления следует сбросить свойство до `false`.

Дополнительные сведения о `RefreshView`см. в разделе [Xamarin. Forms рефрешвиев](~/xamarin-forms/user-interface/refreshview.md).

## <a name="load-data-incrementally"></a>Добавочная загрузка данных

[`CarouselView`](xref:Xamarin.Forms.CarouselView)поддерживает добавочную виртуализацию данных при прокрутке пользователем. Это позволяет выполнять такие сценарии, как асинхронная загрузка страницы данных из веб-службы при прокрутке пользователем. Кроме того, точка, в которой загружаются дополнительные данные, настраивается таким образом, что пользователи не видят пустое пространство или останавливаются при прокрутке.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)определяет следующие свойства для управления добавочной загрузкой данных:

- `RemainingItemsThreshold`, тип `int`, пороговое значение элементов, которые еще не отображаются в списке, в `RemainingItemsThresholdReached` котором будет вызвано событие.
- `RemainingItemsThresholdReachedCommand`Тип `ICommand`, который выполняется при `RemainingItemsThreshold` достижении.
- `RemainingItemsThresholdReachedCommandParameter` с типом `object`, который передается как параметр в `RemainingItemsThresholdReachedCommand`.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)также определяет `RemainingItemsThresholdReached` событие, которое возникает, когда `CarouselView` прокрутка достаточно велика, `RemainingItemsThreshold` чтобы элементы не отображались. Это событие может быть обработано для загрузки дополнительных элементов. Кроме того, при срабатывании `RemainingItemsThresholdReached` `RemainingItemsThresholdReachedCommand` события выполняется, что позволяет выполнять добавочную загрузку данных в ViewModel.

Значение `RemainingItemsThreshold` свойства по умолчанию равно-1, что означает, что `RemainingItemsThresholdReached` событие никогда не будет запущено. Если значение свойства равно 0, `RemainingItemsThresholdReached` событие будет инициировано при отображении последнего элемента в. [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) Для значений, превышающих 0, `RemainingItemsThresholdReached` событие будет запущено, когда `ItemsSource` содержит количество элементов, которые еще не прокручены.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)проверяет `RemainingItemsThreshold` свойство таким образом, что его значение всегда больше или равно-1.

В следующем примере XAML показано, [`CarouselView`](xref:Xamarin.Forms.CarouselView) как выполнить добавочную загрузку данных:

```xaml
<CarouselView ItemsSource="{Binding Animals}"
              RemainingItemsThreshold="2"
              RemainingItemsThresholdReached="OnCarouselViewRemainingItemsThresholdReached"
              RemainingItemsThresholdReachedCommand="{Binding LoadMoreDataCommand}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    RemainingItemsThreshold = 2
};
carouselView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

В этом примере кода `RemainingItemsThresholdReached` событие срабатывает, когда еще не прокручивается 2 элемента, а в ответе выполняется обработчик `OnCollectionViewRemainingItemsThresholdReached` событий:

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> Данные также могут быть загружены постепенно путем привязки `RemainingItemsThresholdReachedCommand` к `ICommand` реализации в ViewModel.

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Индикаторвиев Xamarin. Forms](~/xamarin-forms/user-interface/indicatorview.md)
- [Рефрешвиев Xamarin. Forms](~/xamarin-forms/user-interface/refreshview.md)
- [Привязка данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание DataTemplateSelector Xamarin. Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
