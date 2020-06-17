---
Title: " Xamarin.Forms CollectionView прокрутка" Description: "когда пользователь прокручивается для инициации прокрутки, конечную точку прокрутки можно контролировать, чтобы элементы были полностью отображены. Кроме того, CollectionView определяет два метода Скроллто, которые программным путем прокрутки элементов в представление. "
MS. произв. Xamarin MS. AssetID: 2ED719AF-33D2-434D-949A-B70B479C9BA5 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 09/17/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-collectionview-scrolling"></a>Xamarin.FormsПрокрутка CollectionView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)определяет два [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) метода: прокрутка элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление. Обе перегрузки имеют дополнительные аргументы, которые могут указывать на группу, к которой принадлежит элемент, точное расположение элемента после завершения прокрутки, а также следует ли анимировать прокрутку.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)Определяет [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) событие, которое возникает при вызове одного из [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) методов. [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs)Объект, сопровождающий `ScrollToRequested` событие, имеет множество свойств, включая `IsAnimated` , `Index` , `Item` и `ScrollToPosition` . Эти свойства задаются из аргументов, указанных в `ScrollTo` вызовах метода.

Кроме того, [`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет `Scrolled` событие, которое срабатывает для указания на то, что произошла прокрутка. `ItemsViewScrolledEventArgs`Объект, сопровождающий `Scrolled` событие, имеет много свойств. Дополнительные сведения см. в разделе [Определение прокрутки](#detect-scrolling).

[`CollectionView`](xref:Xamarin.Forms.CollectionView)также определяет `ItemsUpdatingScrollMode` свойство, представляющее поведение прокрутки `CollectionView` при добавлении новых элементов к нему. Дополнительные сведения об этом свойстве см. в разделе [управление позицией прокрутки при добавлении новых элементов](#control-scroll-position-when-new-items-are-added).

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Эта функция называется привязкой, так как элементы привязываются к позиции при остановке прокрутки. Дополнительные сведения см. в разделе [точки привязки](#snap-points).

[`CollectionView`](xref:Xamarin.Forms.CollectionView)также может загружать данные постепенно по мере прокрутки пользователем. Дополнительные сведения см. в статье [добавочная загрузка данных](populate-data.md#load-data-incrementally).

## <a name="detect-scrolling"></a>Обнаружение прокрутки

[`CollectionView`](xref:Xamarin.Forms.CollectionView)Определяет `Scrolled` событие, которое срабатывает для указания на то, что произошла прокрутка. В следующем примере XAML показан объект `CollectionView` , который задает обработчик событий для `Scrolled` события:

```xaml
<CollectionView Scrolled="OnCollectionViewScrolled">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.Scrolled += OnCollectionViewScrolled;
```

В этом примере кода `OnCollectionViewScrolled` обработчик событий выполняется при `Scrolled` срабатывании события:

```csharp
void OnCollectionViewScrolled(object sender, ItemsViewScrolledEventArgs e)
{
    Debug.WriteLine("HorizontalDelta: " + e.HorizontalDelta);
    Debug.WriteLine("VerticalDelta: " + e.VerticalDelta);
    Debug.WriteLine("HorizontalOffset: " + e.HorizontalOffset);
    Debug.WriteLine("VerticalOffset: " + e.VerticalOffset);
    Debug.WriteLine("FirstVisibleItemIndex: " + e.FirstVisibleItemIndex);
    Debug.WriteLine("CenterItemIndex: " + e.CenterItemIndex);
    Debug.WriteLine("LastVisibleItemIndex: " + e.LastVisibleItemIndex);
}
```

В этом примере `OnCollectionViewScrolled` обработчик событий выводит значения `ItemsViewScrolledEventArgs` объекта, сопровождающего событие.

> [!IMPORTANT]
> `Scrolled`Событие срабатывает для прокрутки, инициированной пользователем, и для программной прокрутки.

## <a name="scroll-an-item-at-an-index-into-view"></a>Прокрутка элемента по индексу в представлении

Первая [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) перегрузка метода выполняет прокрутку элемента по указанному индексу в представлении. При наличии [`CollectionView`](xref:Xamarin.Forms.CollectionView) объекта с именем в `collectionView` следующем примере показано, как прокручивать элемент с индексом 12 в представление:

```csharp
collectionView.ScrollTo(12);
```

Кроме того, элемент в сгруппированных данных можно прокручивать в представлении, указав индексы элементов и групп. В следующем примере показано, как прокрутить третий элемент во второй группе на представление:

```csharp
// Items and groups are indexed from zero.
collectionView.ScrollTo(2, 1);
```

> [!NOTE]
> [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)Событие возникает при [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) вызове метода.

## <a name="scroll-an-item-into-view"></a>Прокрутить элемент на представление

Вторая [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) перегрузка метода выполняет прокрутку указанного элемента в представлении. При наличии [`CollectionView`](xref:Xamarin.Forms.CollectionView) объекта с именем в `collectionView` следующем примере показано, как прокручивать элемент пробосЦис обезьяны в представление:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

Кроме того, элемент в сгруппированных данных можно прокручивать в представлении, указав элемент и группу. В следующем примере показано, как прокручивать элемент ПробосЦис обезьяны в группе Монкэйс в представление:

```csharp
GroupedAnimalsViewModel viewModel = BindingContext as GroupedAnimalsViewModel;
AnimalGroup group = viewModel.Animals.FirstOrDefault(a => a.Name == "Monkeys");
Animal monkey = group.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey, group);
```

> [!NOTE]
> [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)Событие возникает при [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) вызове метода.

## <a name="disable-scroll-animation"></a>Отключить анимацию прокрутки

Анимация с прокруткой отображается при прокрутке элемента в представлении. Однако эту анимацию можно отключить, задав `animate` для аргумента метода значение `ScrollTo` `false` :

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>Управление положением прокрутки

При прокрутке элемента в представлении точное расположение элемента после прокрутки можно указать с помощью `position` аргумента [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) методов. Этот аргумент принимает [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) член перечисления.

### <a name="makevisible"></a>макевисибле

Элемент [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться, пока он не будет виден в представлении:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

В этом примере кода создается минимальная прокрутка, необходимая для прокрутки элемента на представление:

[![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении в iOS и Android](scrolling-images/scrolltoposition-makevisible.png "Вертикальный список CollectionView с прокруткой элемента")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

> [!NOTE]
> [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)Элемент используется по умолчанию, если `position` аргумент не указан при вызове `ScrollTo` метода.

### <a name="start"></a>Начать

Элемент [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться до начала представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

Этот пример кода приводит к прокрутке элемента до начала представления:

[![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении в iOS и Android](scrolling-images/scrolltoposition-start.png "Вертикальный список CollectionView с прокруткой элемента")](scrolling-images/scrolltoposition-start-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

### <a name="center"></a>Центр.

Элемент [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться по центру представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

В этом примере кода показано, что элемент прокручивается в центр представления:

[![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении в iOS и Android](scrolling-images/scrolltoposition-center.png "Вертикальный список CollectionView с прокруткой элемента")](scrolling-images/scrolltoposition-center-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

### <a name="end"></a>Конец

Элемент [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться до конца представления:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Этот пример кода приводит к прокрутке элемента до конца представления:

[![Снимок экрана CollectionViewого вертикального списка с прокруткой элемента в представлении в iOS и Android](scrolling-images/scrolltoposition-end.png "Вертикальный список CollectionView с прокруткой элемента")](scrolling-images/scrolltoposition-end-large.png#lightbox "Вертикальный список CollectionView с прокруткой элемента")

## <a name="control-scroll-position-when-new-items-are-added"></a>Управление позицией прокрутки при добавлении новых элементов

[`CollectionView`](xref:Xamarin.Forms.CollectionView)Определяет `ItemsUpdatingScrollMode` свойство, которое поддерживается связываемым свойством. Это свойство возвращает или задает `ItemsUpdatingScrollMode` значение перечисления, представляющее поведение прокрутки `CollectionView` при добавлении новых элементов к нему. Перечисление `ItemsUpdatingScrollMode` определяет следующие члены:

- `KeepItemsInView`корректирует смещение прокрутки, чтобы при добавлении новых элементов отображался первый видимый элемент.
- `KeepScrollOffset`поддерживает смещение прокрутки относительно начала списка при добавлении новых элементов.
- `KeepLastItemInView`корректирует смещение прокрутки для сохранения последнего элемента, отображаемого при добавлении новых элементов.

Значение свойства по умолчанию `ItemsUpdatingScrollMode` — `KeepItemsInView` . Поэтому при добавлении новых элементов к [`CollectionView`](xref:Xamarin.Forms.CollectionView) первому видимому элементу в списке будут отображаться. Чтобы убедиться, что новые добавленные элементы всегда видны в нижней части списка, `ItemsUpdatingScrollMode` свойство должно иметь значение `KeepLastItemInView` :

```xaml
<CollectionView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>Видимость полосы прокрутки

[`CollectionView`](xref:Xamarin.Forms.CollectionView)Определяет `HorizontalScrollBarVisibility` `VerticalScrollBarVisibility` Свойства и, поддерживающие привязку свойств. Эти свойства получают или задают [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) значение перечисления, представляющее, когда отображается горизонтальная или вертикальная полоса прокрутки. Перечисление `ScrollBarVisibility` определяет следующие члены:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)Указывает поведение полосы прокрутки по умолчанию для платформы и является значением по умолчанию `HorizontalScrollBarVisibility` для `VerticalScrollBarVisibility` свойств и.
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)Указывает, что полосы прокрутки будут видимы, даже если содержимое умещается в представлении.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)Указывает, что полосы прокрутки не будут видны, даже если содержимое не умещается в представлении.

## <a name="snap-points"></a>Точки прикрепления

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Эта функция называется привязкой, так как элементы привязываются к позиции при остановке прокрутки и контролируются следующими свойствами [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) класса:

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)Тип [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) — задает поведение точек привязки при прокрутке.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment), типа [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) , задает способ выравнивания точек привязки по элементам.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> При возникновении привязки она будет выполняться в направлении, которое создает наименьший объем движения.

### <a name="snap-points-type"></a>Тип точек привязки

[`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)Перечисление определяет следующие члены:

- `None`Указывает, что прокрутка не привязывается к элементам.
- `Mandatory`Указывает, что содержимое всегда привязывается к ближайшей точке привязки, в которой будет естественно останавливаться прокрутка, а также направление инерции.
- `MandatorySingle`Указывает на то же поведение `Mandatory` , что и, но только прокручивает по одному элементу за раз.

По умолчанию [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) свойство имеет значение `SnapPointsType.None` , которое гарантирует, что прокрутка не привязывает элементы, как показано на следующих снимках экрана:

[![Снимок экрана с вертикальным списком CollectionView без точек привязки в iOS и Android](scrolling-images/snappoints-none.png "Вертикальный список CollectionView без точек привязки")](scrolling-images/snappoints-none-large.png#lightbox "Вертикальный список CollectionView без точек привязки")

### <a name="snap-points-alignment"></a>Выравнивание точек привязки

[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)Перечисление `Start` определяет `Center` члены, и `End` .

> [!IMPORTANT]
> Значение [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) свойства учитывается только в том случае, если [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) свойство имеет значение `Mandatory` , или `MandatorySingle` .

#### <a name="start"></a>Начать

`SnapPointsAlignment.Start`Элемент указывает, что точки привязки выравниваться с ведущим ребром элементов.

По умолчанию [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) свойство имеет значение `SnapPointsAlignment.Start` . Однако для полноты в следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки, верхний элемент будет соответствовать верхней части представления:

[![Снимок экрана с CollectionView вертикальным списком с точками привязки для запуска в iOS и Android](scrolling-images/snappoints-start.png "Вертикальный список CollectionView с начальной точкой привязки")](scrolling-images/snappoints-start-large.png#lightbox "Вертикальный список CollectionView с начальной точкой привязки")

#### <a name="center"></a>Центр.

`SnapPointsAlignment.Center`Элемент указывает, что точки привязки выровнены по центру элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки, верхний элемент выравнивается по центру в верхней части представления:

[![Снимок экрана с вертикальным CollectionView списком с центральными точками привязки в iOS и Android](scrolling-images/snappoints-center.png "Вертикальный список CollectionView с центральными точками привязки")](scrolling-images/snappoints-center-large.png#lightbox "Вертикальный список CollectionView с центральными точками привязки")

#### <a name="end"></a>Конец

`SnapPointsAlignment.End`Элемент указывает, что точки привязки выравниваться с конечным ребром элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки, нижний элемент будет соответствовать нижней части представления:

[![Снимок экрана с CollectionView вертикальным списком с конечными точками привязки в iOS и Android](scrolling-images/snappoints-end.png "Вертикальный список CollectionView с конечными точками привязки")](scrolling-images/snappoints-end-large.png#lightbox "Вертикальный список CollectionView с конечными точками привязки")

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
