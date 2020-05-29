---
Title: « Xamarin.Forms скроллвиев» Description: « Xamarin.Forms скроллвиев — это макет, который способен прокручивать его содержимое».
MS. произв. Xamarin MS. AssetID: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 05/27/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-scrollview"></a>Xamarin.Formsскроллвиев

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-scrollviewdemos)

[![Xamarin.Formsскроллвиев](scrollview-images/layouts.png "[! Операцион. NO-LOC (Xamarin. Forms)] Скроллвиев")](scrollview-images/layouts-large.png#lightbox "[! Операцион. NO-LOC (Xamarin. Forms)] Скроллвиев")

[`ScrollView`](xref:Xamarin.Forms.ScrollView)— Это макет, который способен прокручивать его содержимое. `ScrollView`Класс является производным от [`Layout`](xref:Xamarin.Forms.Layout) класса, и по умолчанию прокручивает его содержимое по вертикали. `ScrollView`Может иметь только один дочерний элемент, хотя это может быть и другие макеты.

> [!WARNING]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)объекты не должны быть вложенными. Кроме того, `ScrollView` объекты не должны быть вложенными с другими элементами управления, обеспечивающими прокрутку, например [`CollectionView`](xref:Xamarin.Forms.CollectionView) , [`ListView`](xref:Xamarin.Forms.ListView) и [`WebView`](xref:Xamarin.Forms.WebView) .

[`ScrollView`](xref:Xamarin.Forms.ScrollView)определяет следующие свойства:

- [`Content`](xref:Xamarin.Forms.ScrollView.Content)Тип [`View`](xref:Xamarin.Forms.View) представляет содержимое, отображаемое в [`ScrollView`](xref:Xamarin.Forms.ScrollView) .
- [`ContentSize`](xref:Xamarin.Forms.ScrollView), типа [`Size`](xref:Xamarin.Forms.Size) , представляет размер содержимого. Это свойство доступно только для чтения.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView)Тип — [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) представляет, когда отображается горизонтальная полоса прокрутки.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation), типа [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) , представляет направление прокрутки [`ScrollView`](xref:Xamarin.Forms.ScrollView) . Значение по умолчанию этого свойства равно `Vertical`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollX), типа `double` , указывает текущую точку прокрутки по оси X. Значение этого свойства только для чтения по умолчанию равно 0.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollY)Тип `double` — указывает текущую точку прокрутки по оси Y. Значение этого свойства только для чтения по умолчанию равно 0.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView)Тип — [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) представляет, когда отображается вертикальная полоса прокрутки.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, за исключением [`Content`](xref:Xamarin.Forms.ScrollView.Content) свойства, что означает, что они могут быть целевыми объектами привязки данных и стилями.

[`Content`](xref:Xamarin.Forms.ScrollView.Content)Свойство является свойством [`ContentProperty`](xref:Xamarin.Forms.ContentPropertyAttribute) [`ScrollView`](xref:Xamarin.Forms.ScrollView) класса, поэтому его не нужно явно задавать из XAML.

> [!TIP]
> Чтобы получить максимально возможную производительность макета, следуйте рекомендациям по [оптимизации производительности макета](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance).

## <a name="scrollview-as-a-root-layout"></a>Скроллвиев в качестве корневого макета

[`ScrollView`](xref:Xamarin.Forms.ScrollView)Может иметь только один дочерний элемент, который может быть другими макетами. Поэтому, как правило, в качестве `ScrollView` корневого макета страницы. Чтобы прокрутить его дочернее содержимое, [`ScrollView`](xref:Xamarin.Forms.ScrollView) вычислит разницу между высотой ее содержимого и ее собственной высотой. Это различие — это величина, на которую `ScrollView` может прокручиваться содержимое.

[`StackLayout`](xref:Xamarin.Forms.StackLayout)Часто является дочерним элементом `ScrollView` . В этом случае объект выдает значение, равное `ScrollView` `StackLayout` сумме значений высоты дочерних элементов. Затем объект `ScrollView` может определить объем прокрутки его содержимого. Дополнительные сведения о `StackLayout` см. в разделе [ Xamarin.Forms StackLayout](stacklayout.md).

> [!CAUTION]
> В вертикальном [`ScrollView`](xref:Xamarin.Forms.ScrollView) случае не устанавливайте `VerticalOptions` для свойства значение `Start` , `Center` или `End` . Это означает, что значение должно `ScrollView` быть равно высоте по мере необходимости, что может равняться нулю. В то время как Xamarin.Forms защищается от этой ситуации, лучше избегать кода, который предполагает, что что-то не должно происходить.

Следующий пример XAML имеет в [`ScrollView`](xref:Xamarin.Forms.ScrollView) качестве корневого макета страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ScrollViewDemos"
             x:Class="ScrollViewDemos.Views.ColorListPage"
             Title="ScrollView demo">
    <ScrollView>
        <StackLayout BindableLayout.ItemsSource="{x:Static local:NamedColor.All}">
            <BindableLayout.ItemTemplate>
                <DataTemplate>
                    <StackLayout Orientation="Horizontal">
                        <BoxView Color="{Binding Color}"
                                 HeightRequest="32"
                                 WidthRequest="32"
                                 VerticalOptions="Center" />
                        <Label Text="{Binding FriendlyName}"
                               FontSize="24"
                               VerticalOptions="Center" />
                    </StackLayout>
                </DataTemplate>
            </BindableLayout.ItemTemplate>
        </StackLayout>
    </ScrollView>
</ContentPage>
```

В этом примере свойство [`ScrollView`](xref:Xamarin.Forms.ScrollView) имеет свой набор содержимого [`StackLayout`](xref:Xamarin.Forms.StackLayout) , который использует связываемый макет для вывода [`Color`](xref:Xamarin.Forms.Color) полей, определенных Xamarin.Forms . По умолчанию `ScrollView` вертикальная прокрутка прокручивается, что открывает дополнительное содержимое:

[![Снимок экрана с корневым макетом Скроллвиев](scrollview-images/root-layout.png "Макет корневого Скроллвиев")](scrollview-images/root-layout-large.png#lightbox "Макет корневого Скроллвиев")

Эквивалентный код на C# выглядит так:

```csharp
public class ColorListPageCode : ContentPage
{
    public ColorListPageCode()
    {
        DataTemplate dataTemplate = new DataTemplate(() =>
        {
            BoxView boxView = new BoxView
            {
                HeightRequest = 32,
                WidthRequest = 32,
                VerticalOptions = LayoutOptions.Center
            };
            boxView.SetBinding(BoxView.ColorProperty, "Color");

            Label label = new Label
            {
                FontSize = 24,
                VerticalOptions = LayoutOptions.Center
            };
            label.SetBinding(Label.TextProperty, "FriendlyName");

            StackLayout horizontalStackLayout = new StackLayout
            {
                Orientation = StackOrientation.Horizontal,
                Children = { boxView, label }
            };
            return horizontalStackLayout;
        });

        StackLayout stackLayout = new StackLayout();
        BindableLayout.SetItemsSource(stackLayout, NamedColor.All);
        BindableLayout.SetItemTemplate(stackLayout, dataTemplate);

        ScrollView scrollView = new ScrollView { Content = stackLayout };

        Title = "ScrollView demo";
        Content = scrollView;
    }
}
```

Дополнительные сведения о макетах с возможностью привязки см. [в разделе макеты с возможностью привязки в Xamarin.Forms ](bindable-layouts.md).

## <a name="scrollview-as-a-child-layout"></a>Скроллвиев в качестве дочернего макета

[`ScrollView`](xref:Xamarin.Forms.ScrollView)Может быть дочерним макетом для другого родительского макета.

[`ScrollView`](xref:Xamarin.Forms.ScrollView)Часто является дочерним элементом [`StackLayout`](xref:Xamarin.Forms.StackLayout) . `ScrollView`Для вычисления разницы между высотой ее содержимого и ее собственной высотой требуется определенная высота, а разница — это величина, на которую `ScrollView` может прокручиваться содержимое. Если объект `ScrollView` является дочерним элементом объекта `StackLayout` , он не получает определенную высоту. Значение `StackLayout` `ScrollView` должно быть как можно более коротким, то есть либо высотой `ScrollView` содержимого, либо нулем. Для работы с этим сценарием `VerticalOptions` свойство объекта `ScrollView` должно иметь значение `FillAndExpand` . Это приведет к тому, что будет `StackLayout` предоставлять `ScrollView` все дополнительное пространство, не необходимое для других дочерних элементов, а `ScrollView` затем будет определена высота.

Следующий пример XAML имеет в [`ScrollView`](xref:Xamarin.Forms.ScrollView) качестве дочернего макета для [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ScrollViewDemos.Views.BlackCatPage"
             Title="ScrollView as a child layout demo">
    <StackLayout Margin="20">
        <Label Text="THE BLACK CAT by Edgar Allan Poe"
               FontSize="Medium"
               FontAttributes="Bold"
               HorizontalOptions="Center" />
        <ScrollView VerticalOptions="FillAndExpand">
            <StackLayout>
                <Label Text="FOR the most wild, yet most homely narrative which I am about to pen, I neither expect nor solicit belief. Mad indeed would I be to expect it, in a case where my very senses reject their own evidence. Yet, mad am I not -- and very surely do I not dream. But to-morrow I die, and to-day I would unburthen my soul. My immediate purpose is to place before the world, plainly, succinctly, and without comment, a series of mere household events. In their consequences, these events have terrified -- have tortured -- have destroyed me. Yet I will not attempt to expound them. To me, they have presented little but Horror -- to many they will seem less terrible than barroques. Hereafter, perhaps, some intellect may be found which will reduce my phantasm to the common-place -- some intellect more calm, more logical, and far less excitable than my own, which will perceive, in the circumstances I detail with awe, nothing more than an ordinary succession of very natural causes and effects." />
                <!-- More Label objects go here -->
            </StackLayout>
        </ScrollView>
    </StackLayout>
</ContentPage>
```

В этом примере есть два [`StackLayout`](xref:Xamarin.Forms.StackLayout) объекта. Первый `StackLayout` — это корневой объект макета, который содержит [`Label`](xref:Xamarin.Forms.Label) объект и [`ScrollView`](xref:Xamarin.Forms.ScrollView) как его дочерние элементы. Объект `ScrollView` имеет в `StackLayout` качестве своего содержимого и `StackLayout` содержит несколько `Label` объектов. Такое размещение гарантирует, что первый `Label` всегда будет отображаться на экране, а текст, отображаемый другими `Label` объектами, можно прокручивать:

[![Снимок экрана дочернего макета Скроллвиев](scrollview-images/child-layout.png "Дочерний макет Скроллвиев")](scrollview-images/child-layout-large.png#lightbox "Дочерний макет Скроллвиев")

Эквивалентный код на C# выглядит так:

```csharp
public class BlackCatPageCS : ContentPage
{
    public BlackCatPageCS()
    {
        Label titleLabel = new Label
        {
            Text = "THE BLACK CAT by Edgar Allan Poe",
            // More properties set here to define the Label appearance
        };

        ScrollView scrollView = new ScrollView
        {
            VerticalOptions = LayoutOptions.FillAndExpand,
            Content = new StackLayout
            {
                Children =
                {
                    new Label
                    {
                        Text = "FOR the most wild, yet most homely narrative which I am about to pen, I neither expect nor solicit belief. Mad indeed would I be to expect it, in a case where my very senses reject their own evidence. Yet, mad am I not -- and very surely do I not dream. But to-morrow I die, and to-day I would unburthen my soul. My immediate purpose is to place before the world, plainly, succinctly, and without comment, a series of mere household events. In their consequences, these events have terrified -- have tortured -- have destroyed me. Yet I will not attempt to expound them. To me, they have presented little but Horror -- to many they will seem less terrible than barroques. Hereafter, perhaps, some intellect may be found which will reduce my phantasm to the common-place -- some intellect more calm, more logical, and far less excitable than my own, which will perceive, in the circumstances I detail with awe, nothing more than an ordinary succession of very natural causes and effects.",
                    },
                    // More Label objects go here
                }
            }
        };

        Title = "ScrollView as a child layout demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = { titleLabel, scrollView }
        };
    }
}
```

## <a name="orientation"></a>Ориентация

[`ScrollView`](xref:Xamarin.Forms.ScrollView)имеет [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) свойство, представляющее направление прокрутки объекта `ScrollView` . Это свойство имеет тип [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) , который определяет следующие члены:

- `Vertical`Указывает, что `ScrollView` будет прокручиваться вертикально. Этот элемент является значением по умолчанию для [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) Свойства.
- `Horizontal`Указывает, что `ScrollView` будет прокручиваться по горизонтали.
- `Both`Указывает, что `ScrollView` будет прокручиваться по горизонтали и вертикали.
- `Neither`Указывает, что элемент `ScrollView` не будет прокручиваться.

> [!TIP]
> Прокрутку можно отключить, присвоив [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) свойству значение `Neither` .

## <a name="detect-scrolling"></a>Обнаружение прокрутки

[`ScrollView`](xref:Xamarin.Forms.ScrollView)Определяет [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) событие, которое срабатывает для указания на то, что произошла прокрутка. [`ScrolledEventArgs`](xref:Xamarin.Forms.ScrolledEventArgs)Объект, сопровождающий `Scrolled` событие, имеет `ScrollX` Свойства и и `ScrollY` оба типа `double` .

> [!IMPORTANT]
> `ScrolledEventArgs.ScrollX`Свойства и `ScrolledEventArgs.ScrollY` могут иметь отрицательные значения из-за эффектов отскока, возникающих при прокрутке до начала [`ScrollView`](xref:Xamarin.Forms.ScrollView) .

В следующем примере XAML показан объект [`ScrollView`](xref:Xamarin.Forms.ScrollView) , который задает обработчик событий для [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) события:

```xaml
<ScrollView Scrolled="OnScrollViewScrolled">
        ...
</ScrollView>
```

Эквивалентный код на C# выглядит так:

```csharp
ScrollView scrollView = new ScrollView();
scrollView.Scrolled += OnScrollViewScrolled;
```

В этом примере `OnScrollViewScrolled` обработчик событий выполняется при [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled) срабатывании события:

```csharp
void OnScrollViewScrolled(object sender, ScrolledEventArgs e)
{
    Console.WriteLine($"ScrollX: {e.ScrollX}, ScrollY: {e.ScrollY}");
}
```

В этом примере `OnScrollViewScrolled` обработчик событий выводит значения [`ScrolledEventArgs`](xref:Xamarin.Forms.ScrolledEventArgs) объекта, сопровождающего событие.

> [!NOTE]
> [`Scrolled`](xref:Xamarin.Forms.ScrollView.Scrolled)Событие срабатывает для прокрутки, инициированной пользователем, и для программной прокрутки.

## <a name="scroll-programmatically"></a>Прокрутка программным способом

[`ScrollView`](xref:Xamarin.Forms.ScrollView)определяет два [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) метода: асинхронная прокрутка `ScrollView` . Одна из перегрузок прокручивается до указанной позицией в `ScrollView` , а другая Прокручивает указанный элемент в представление. Обе перегрузки имеют дополнительный аргумент, который можно использовать для указания, следует ли анимировать прокрутку.

> [!IMPORTANT]
> Эти [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) методы не будут приводить к прокрутке, если [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) свойство имеет значение `Neither` .

### <a name="scroll-a-position-into-view"></a>Прокрутить точку в область просмотра

Позицией в [`ScrollView`](xref:Xamarin.Forms.ScrollView) можно прокручивать до [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) метода, принимающего `double` `x` `y` аргументы и. При наличии вертикального `ScrollView` объекта с именем в `scrollView` следующем примере показано, как выполнить прокрутку до 150 единиц, независимых от устройства, из верхней части `ScrollView` .

```csharp
await scrollView.ScrollToAsync(0, 150, true);
```

Третьим аргументом для [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) является `animated` аргумент, который определяет, отображается ли анимация с прокруткой при программной прокрутке [`ScrollView`](xref:Xamarin.Forms.ScrollView) .

### <a name="scroll-an-element-into-view"></a>Прокрутка элемента на представление

Элемент в [`ScrollView`](xref:Xamarin.Forms.ScrollView) можно прокрутить на представление с помощью [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) метода, принимающего [`Element`](xref:Xamarin.Forms.Element) аргументы и [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) . В `ScrollView` `scrollView` [`Label`](xref:Xamarin.Forms.Label) `label` следующем примере показано, как прокручивать элемент в представлении по вертикали с именем и именем.

```csharp
await scrollView.ScrollToAsync(label, ScrollToPosition.End, true);
```

Третьим аргументом для [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) является `animated` аргумент, который определяет, отображается ли анимация с прокруткой при программной прокрутке [`ScrollView`](xref:Xamarin.Forms.ScrollView) .

При прокрутке элемента в представлении точное расположение элемента после завершения прокрутки можно задать с помощью второго аргумента `position` [`ScrollToAsync`](xref:Xamarin.Forms.ScrollView.ScrollToAsync*) метода. Этот аргумент принимает [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) член перечисления:

- `MakeVisible`Указывает, что элемент должен быть прокручиваться до тех пор, пока он не будет виден в `ScrollView` .
- `Start`Указывает, что элемент должен быть прокручиваться до начала `ScrollView` .
- `Center`Указывает, что элемент должен быть прокручиваться по центру объекта `ScrollView` .
- `End`Указывает, что элемент должен быть прокручиваться до конца `ScrollView` .

## <a name="scroll-bar-visibility"></a>Видимость полосы прокрутки

[`ScrollView`](xref:Xamarin.Forms.ScrollView)Определяет [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView) [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView) Свойства и, поддерживающие привязку свойств. Эти свойства получают или задают [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibility) значение перечисления, которое показывает, отображается ли горизонтальная или вертикальная полоса прокрутки. Перечисление `ScrollBarVisibility` определяет следующие члены:

- `Default`Указывает поведение полосы прокрутки по умолчанию для платформы и является значением по умолчанию `HorizontalScrollBarVisibility` `VerticalScrollBarVisibility` свойств и.
- `Always`Указывает, что полосы прокрутки будут видимы, даже если содержимое умещается в представлении.
- `Never`Указывает, что полосы прокрутки не будут видны, даже если содержимое не умещается в представлении.

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации Скроллвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-scrollviewdemos)
- [Xamarin.FormsStackLayout](stacklayout.md)
- [Связываемые макеты вXamarin.Forms](bindable-layouts.md)
