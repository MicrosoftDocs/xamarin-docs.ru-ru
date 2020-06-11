---
Title: « Xamarin.Forms CollectionView Data» Description: «CollectionView заполняется данными путем присвоения свойству ItemsSource любой коллекции, реализующей IEnumerable.»
MS. произв. Xamarin MS. AssetID: E1783E34-1C0F-401A-80D5-B2BE5508F5F8 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 04/29/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-collectionview-data"></a>Xamarin.FormsДанные CollectionView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)включает следующие свойства, которые определяют отображаемые данные и его внешний вид:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource), типа `IEnumerable` , задает коллекцию элементов для отображения и имеет значение по умолчанию `null` .
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) — указывает шаблон, применяемый к каждому элементу в коллекции отображаемых элементов.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)Определяет `ItemsUpdatingScrollMode` свойство, представляющее поведение прокрутки `CollectionView` при добавлении новых элементов к нему. Дополнительные сведения об этом свойстве см. в разделе [управление позицией прокрутки при добавлении новых элементов](scrolling.md#control-scroll-position-when-new-items-are-added).

[`CollectionView`](xref:Xamarin.Forms.CollectionView)поддерживает добавочную виртуализацию данных при прокрутке пользователем. Дополнительные сведения см. в статье [добавочная загрузка данных](#load-data-incrementally).

## <a name="populate-a-collectionview-with-data"></a>Заполнение CollectionView данными

[`CollectionView`](xref:Xamarin.Forms.CollectionView)Заполняется данными путем установки его [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства в любую коллекцию, реализующую `IEnumerable` . Элементы можно добавлять в XAML путем инициализации `ItemsSource` свойства из массива строк:

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> Обратите внимание на то, что для элемента `x:Array` требуется атрибут `Type`, указывающий тип элементов в массиве.

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.ItemsSource = new string[]
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

> [!WARNING]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)вызовет исключение, если его [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) обновление выполняется в потоке пользовательского интерфейса.

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображает элементы в вертикальном списке, как показано на следующих снимках экрана:

[![Снимок экрана CollectionView, содержащий текстовые элементы, в iOS и Android](populate-data-images/text.png "Текстовые элементы в CollectionView")](populate-data-images/text-large.png#lightbox "Текстовые элементы в CollectionView")

> [!IMPORTANT]
> Если [`CollectionView`](xref:Xamarin.Forms.CollectionView) требуется обновление при добавлении, удалении или изменении элементов в базовой коллекции, то базовая коллекция должна быть `IEnumerable` коллекцией, которая отправляет уведомления об изменении свойств, например `ObservableCollection` .

Сведения о том, как изменить [`CollectionView`](xref:Xamarin.Forms.CollectionView) Макет, см. в разделе [ Xamarin.Forms CollectionView Layout](layout.md). Сведения о том, как определить внешний вид каждого элемента в `CollectionView` , см. в [разделе Определение внешнего вида элемента](#define-item-appearance).

### <a name="data-binding"></a>привязка данных,

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может заполняться данными с помощью привязки данных для привязки своего [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства к `IEnumerable` коллекции. В XAML это достигается с помощью `Binding` расширения разметки:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

В этом примере [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) данные свойства привязываются к `Monkeys` свойству подключенного ViewModel.

> [!NOTE]
> Скомпилированные привязки можно включить для повышения производительности привязки данных в Xamarin.Forms приложениях. Дополнительные сведения см. в статье [Скомпилированные привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Дополнительные сведения о привязке данных см. в разделе [Привязка данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Определение внешнего вида элемента

Внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) может быть определен путем присвоения [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) свойству значения [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

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
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    Grid grid = new Grid { Padding = 10 };
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });

    Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 60, WidthRequest = 60 };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Label locationLabel = new Label { FontAttributes = FontAttributes.Italic, VerticalOptions = LayoutOptions.End };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Grid.SetRowSpan(image, 2);

    grid.Children.Add(image);
    grid.Children.Add(nameLabel, 1, 0);
    grid.Children.Add(locationLabel, 1, 1);

    return grid;
});
```

Элементы, указанные в параметре, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяют внешний вид каждого элемента в списке. В этом примере макет в `DataTemplate` управляется с помощью [`Grid`](xref:Xamarin.Forms.Grid) . `Grid`Содержит [`Image`](xref:Xamarin.Forms.Image) объект и два [`Label`](xref:Xamarin.Forms.Label) объекта, которые привязываются к свойствам `Monkey` класса:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

На следующих снимках экрана показан результат создания шаблонов каждого элемента в списке:

[![Снимок экрана CollectionView, где для каждого элемента используется шаблон, в iOS и Android](populate-data-images/datatemplate.png "Элементы шаблона в CollectionView")](populate-data-images/datatemplate-large.png#lightbox "Элементы шаблона в CollectionView")

Дополнительные сведения о шаблонах данных см. в разделе [Общие сведения о шаблонах данныхXamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно выбрать во время выполнения на основе значения элемента, задав [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) для свойства [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) объект.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
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

    <CollectionView ItemsSource="{Binding Monkeys}"
                    ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)Для свойства задается `MonkeyDataTemplateSelector` объект. В следующем примере показан `MonkeyDataTemplateSelector` класс:

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

`MonkeyDataTemplateSelector`Класс определяет `AmericanMonkey` Свойства и `OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых установлены разные шаблоны данных. `OnSelectTemplate`Переопределение возвращает `AmericanMonkey` шаблон, который отображает имя и расположение обезьяны в синем, если имя обезьяны содержит "America". Если имя обезьяны не содержит "America", то `OnSelectTemplate` Переопределение возвращает `OtherMonkey` шаблон, который отображает имя и расположение обезьяны в серебристом виде:

[![Снимок экрана выбора шаблона элемента среды выполнения CollectionView в iOS и Android](populate-data-images/datatemplateselector.png "Выбор шаблона элемента среды выполнения в CollectionView")](populate-data-images/datatemplateselector-large.png#lightbox "Выбор шаблона элемента среды выполнения в CollectionView")

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

> [!IMPORTANT]
> При использовании [`CollectionView`](xref:Xamarin.Forms.CollectionView) никогда не устанавливайте для корневого элемента [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектов значение `ViewCell` . Это приведет к возникновению исключения, так как `CollectionView` не содержит концепцию ячеек.

## <a name="context-menus"></a>Контекстные меню

[`CollectionView`](xref:Xamarin.Forms.CollectionView)поддерживает контекстные меню для элементов данных с помощью `SwipeView` , которое открывает контекстное меню с помощью жеста прокрутки. `SwipeView`— Это контейнерный элемент управления, который создает оболочку вокруг элемента содержимого и предоставляет элементы контекстного меню для этого элемента содержимого. Таким образом, контекстные меню реализуются для, `CollectionView` создавая объект `SwipeView` , который определяет содержимое, `SwipeView` вокруг которого выполняется оболочка, и элементы контекстного меню, которые выводятся с помощью жеста прокрутки. Это достигается путем установки в `SwipeView` качестве корневого представления в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , определяющего внешний вид каждого элемента данных в `CollectionView` :

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <SwipeView>
                <SwipeView.LeftItems>
                    <SwipeItems>
                        <SwipeItem Text="Favorite"
                                   IconImageSource="favorite.png"
                                   BackgroundColor="LightGreen"
                                   Command="{Binding Source={x:Reference collectionView}, Path=BindingContext.FavoriteCommand}"
                                   CommandParameter="{Binding}" />
                        <SwipeItem Text="Delete"
                                   IconImageSource="delete.png"
                                   BackgroundColor="LightPink"
                                   Command="{Binding Source={x:Reference collectionView}, Path=BindingContext.DeleteCommand}"
                                   CommandParameter="{Binding}" />
                    </SwipeItems>
                </SwipeView.LeftItems>
                <Grid BackgroundColor="White"
                      Padding="10">
                    <!-- Define item appearance -->
                </Grid>
            </SwipeView>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    // Define item appearance
    Grid grid = new Grid { Padding = 10, BackgroundColor = Color.White };
    // ...

    SwipeView swipeView = new SwipeView();
    SwipeItem favoriteSwipeItem = new SwipeItem
    {
        Text = "Favorite",
        IconImageSource = "favorite.png",
        BackgroundColor = Color.LightGreen
    };
    favoriteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.FavoriteCommand", source: collectionView));
    favoriteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    SwipeItem deleteSwipeItem = new SwipeItem
    {
        Text = "Delete",
        IconImageSource = "delete.png",
        BackgroundColor = Color.LightPink
    };
    deleteSwipeItem.SetBinding(MenuItem.CommandProperty, new Binding("BindingContext.DeleteCommand", source: collectionView));
    deleteSwipeItem.SetBinding(MenuItem.CommandParameterProperty, ".");

    swipeView.LeftItems = new SwipeItems { favoriteSwipeItem, deleteSwipeItem };
    swipeView.Content = grid;    
    return swipeView;
});
```

В этом примере `SwipeView` содержимое — это [`Grid`](xref:Xamarin.Forms.Grid) , определяющее внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Элементы считывания используются для выполнения действий с `SwipeView` содержимым и отображаются при считывании элемента управления с левой стороны:

[![Снимок экрана: пункты контекстного меню CollectionView в iOS и Android](populate-data-images/swipeview.png "CollectionView с элементами контекстного меню Свипевиев")](populate-data-images/swipeview-large.png#lightbox "CollectionView с элементами контекстного меню Свипевиев")

`SwipeView`поддерживает четыре разных направления прокрутки с направлением считывания, определяемым направленной `SwipeItems` коллекцией, `SwipeItems` в которую добавляются объекты. По умолчанию элемент прокрутки выполняется при касании пользователем. Кроме того, после выполнения элемента считывания элементы прокрутки скрываются и `SwipeView` содержимое отображается повторно. Однако эти поведения можно изменить.

Дополнительные сведения об `SwipeView` элементе управления см. в разделе [ Xamarin.Forms свипевиев](~/xamarin-forms/user-interface/swipeview.md).

## <a name="pull-to-refresh"></a>Обновление путем оттягивания

[`CollectionView`](xref:Xamarin.Forms.CollectionView)поддерживает функцию Pull для обновления с помощью `RefreshView` , которая позволяет обновлять отображаемые данные путем вывода списка элементов. `RefreshView`— Это контейнерный элемент управления, предоставляющий функции обновления для своего дочернего элемента, при условии, что дочерний объект поддерживает прокручиваемое содержимое. Таким образом, запрос на обновление реализуется для, `CollectionView` задавая его в качестве дочернего элемента для `RefreshView` :

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CollectionView ItemsSource="{Binding Animals}">
        ...
    </CollectionView>
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

CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = collectionView;
// ...
```

Когда пользователь инициирует обновление, `ICommand` `Command` выполняется свойство, определенное свойством, которое должно обновлять отображаемые элементы. Во время обновления отображается визуализация обновления, которая состоит из анимированного круга хода выполнения:

[![Снимок экрана CollectionView Pull-to-Refresh, в iOS и Android](populate-data-images/pull-to-refresh.png "CollectionView по запросу на обновление")](populate-data-images/pull-to-refresh-large.png#lightbox "CollectionView по запросу на обновление")

Значение `RefreshView.IsRefreshing` свойства указывает текущее состояние `RefreshView` . При активации обновления пользователем это свойство автоматически переходит в `true` . После завершения обновления следует сбросить свойство до `false` .

Дополнительные сведения о `RefreshView` см. в разделе [ Xamarin.Forms рефрешвиев](~/xamarin-forms/user-interface/refreshview.md).

## <a name="load-data-incrementally"></a>Добавочная загрузка данных

[`CollectionView`](xref:Xamarin.Forms.CollectionView)поддерживает добавочную виртуализацию данных при прокрутке пользователем. Это позволяет выполнять такие сценарии, как асинхронная загрузка страницы данных из веб-службы при прокрутке пользователем. Кроме того, точка, в которой загружаются дополнительные данные, настраивается таким образом, что пользователи не видят пустое пространство или останавливаются при прокрутке.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет следующие свойства для управления добавочной загрузкой данных:

- `RemainingItemsThreshold`, тип `int` , пороговое значение элементов, которые еще не отображаются в списке, в котором `RemainingItemsThresholdReached` будет вызвано событие.
- `RemainingItemsThresholdReachedCommand`Тип `ICommand` , который выполняется при `RemainingItemsThreshold` достижении.
- `RemainingItemsThresholdReachedCommandParameter` с типом `object`, который передается как параметр в `RemainingItemsThresholdReachedCommand`.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)также определяет `RemainingItemsThresholdReached` событие, которое возникает, когда `CollectionView` прокрутка достаточно велика, чтобы `RemainingItemsThreshold` элементы не отображались. Это событие может быть обработано для загрузки дополнительных элементов. Кроме того, при `RemainingItemsThresholdReached` срабатывании события `RemainingItemsThresholdReachedCommand` выполняется, что позволяет выполнять добавочную загрузку данных в ViewModel.

Значение свойства по умолчанию `RemainingItemsThreshold` равно-1, что означает, что `RemainingItemsThresholdReached` событие никогда не будет запущено. Если значение свойства равно 0, `RemainingItemsThresholdReached` событие будет инициировано при отображении последнего элемента в [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) . Для значений, превышающих 0, `RemainingItemsThresholdReached` событие будет запущено, когда `ItemsSource` содержит количество элементов, которые еще не прокручены.

> [!NOTE]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)проверяет `RemainingItemsThreshold` свойство таким образом, что его значение всегда больше или равно-1.

В следующем примере XAML показано, как выполнить [`CollectionView`](xref:Xamarin.Forms.CollectionView) добавочную загрузку данных:

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                RemainingItemsThreshold="5"
                RemainingItemsThresholdReached="OnCollectionViewRemainingItemsThresholdReached">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    RemainingItemsThreshold = 5
};
collectionView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

В этом примере кода `RemainingItemsThresholdReached` событие срабатывает при наличии 5 элементов, которые еще не прокручены, и в ответ выполняет `OnCollectionViewRemainingItemsThresholdReached` обработчик событий:

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> Данные также могут быть загружены постепенно путем привязки `RemainingItemsThresholdReachedCommand` к `ICommand` реализации в ViewModel.

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.Formsрефрешвиев](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin.Formsсвипевиев](~/xamarin-forms/user-interface/swipeview.md)
- [Привязка данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.FormsШаблоны данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Создание Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
