---
title: Xamarin.FormsПрокрутка Карауселвиев
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
ms.openlocfilehash: 462948905f40679e2b931d4aa0039308c64a0a8f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136504"
---
# <a name="xamarinforms-carouselview-scrolling"></a>Xamarin.FormsПрокрутка Карауселвиев

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)определяет следующие свойства, связанные с прокруткой:

- `HorizontalScrollBarVisibility`Тип `ScrollBarVisibility` , который указывает, когда отображается горизонтальная полоса прокрутки.
- `IsDragging`Тип `bool` , который указывает, `CarouselView` прокручивается ли прокрутка. Это свойство доступно только для чтения, для которого значение по умолчанию — `false` .
- `IsScrollAnimated`Тип `bool` , который указывает, будет ли выполняться анимация при прокрутке `CarouselView` . Значение по умолчанию — `true`.
- `ItemsUpdatingScrollMode`Тип `ItemsUpdatingScrollMode` , который представляет поведение прокрутки `CarouselView` при добавлении новых элементов к нему.
- `VerticalScrollBarVisibility`Тип `ScrollBarVisibility` , который указывает, когда отображается вертикальная полоса прокрутки.

Все эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целями привязок данных.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)также определяет два [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) метода: прокрутка элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление. Обе перегрузки имеют дополнительные аргументы, которые можно указать, чтобы указать точную позицию элемента после завершения прокрутки и следует ли анимировать прокрутку.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Определяет [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) событие, которое возникает при вызове одного из [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) методов. [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs)Объект, сопровождающий `ScrollToRequested` событие, имеет множество свойств, включая `IsAnimated` , `Index` , `Item` и `ScrollToPosition` . Эти свойства задаются из аргументов, указанных в `ScrollTo` вызовах метода.

Кроме того, [`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет `Scrolled` событие, которое срабатывает для указания на то, что произошла прокрутка. `ItemsViewScrolledEventArgs`Объект, сопровождающий `Scrolled` событие, имеет много свойств. Дополнительные сведения см. в разделе [Определение прокрутки](#detect-scrolling).

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Эта функция называется привязкой, так как элементы привязываются к позиции при остановке прокрутки. Дополнительные сведения см. в разделе [точки привязки](#snap-points).

[`CarouselView`](xref:Xamarin.Forms.CarouselView)также может загружать данные постепенно по мере прокрутки пользователем. Дополнительные сведения см. в статье [добавочная загрузка данных](populate-data.md#load-data-incrementally).

## <a name="detect-scrolling"></a>Обнаружение прокрутки

`IsDragging`Свойство можно проверить, чтобы определить, [`CarouselView`](xref:Xamarin.Forms.CarouselView) прокручивается ли в данный момент элементы.

Кроме того, [`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет событие, которое `Scrolled` срабатывает для указания на то, что произошла прокрутка. Это событие следует использовать, если требуется получить данные о прокрутке.

В следующем примере XAML показан объект `CarouselView` , который задает обработчик событий для `Scrolled` события:

```xaml
<CarouselView Scrolled="OnCollectionViewScrolled">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.Scrolled += OnCarouselViewScrolled;
```

В этом примере кода `OnCarouselViewScrolled` обработчик событий выполняется при `Scrolled` срабатывании события:

```csharp
void OnCarouselViewScrolled(object sender, ItemsViewScrolledEventArgs e)
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

В этом примере `OnCarouselViewScrolled` обработчик событий выводит значения `ItemsViewScrolledEventArgs` объекта, сопровождающего событие.

> [!IMPORTANT]
> `Scrolled`Событие срабатывает для прокрутки, инициированной пользователем, и для программной прокрутки.

## <a name="scroll-an-item-at-an-index-into-view"></a>Прокрутка элемента по индексу в представлении

Первая [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) перегрузка метода выполняет прокрутку элемента по указанному индексу в представлении. При наличии [`CarouselView`](xref:Xamarin.Forms.CarouselView) объекта с именем в `carouselView` следующем примере показано, как прокручивать элемент с индексом 6 в представление:

```csharp
carouselView.ScrollTo(6);
```

> [!NOTE]
> [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)Событие возникает при [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) вызове метода.

## <a name="scroll-an-item-into-view"></a>Прокрутить элемент на представление

Вторая [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) перегрузка метода выполняет прокрутку указанного элемента в представлении. При наличии [`CarouselView`](xref:Xamarin.Forms.CarouselView) объекта с именем в `carouselView` следующем примере показано, как прокручивать элемент пробосЦис обезьяны в представление:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
carouselView.ScrollTo(monkey);
```

> [!NOTE]
> [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested)Событие возникает при [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) вызове метода.

## <a name="disable-scroll-animation"></a>Отключить анимацию прокрутки

Анимация с прокруткой отображается при перемещении между элементами в [`CarouselView`](xref:Xamarin.Forms.CarouselView) . Эта анимация возникает как для прокрутки, инициированной пользователем, так и для программной прокрутки. Если задать `IsScrollAnimated` для свойства значение, `false` анимация будет отключена для обеих категорий прокрутки.

Кроме того, `animate` аргумент `ScrollTo` метода можно установить в значение, `false` чтобы отключить анимацию прокрутки при программной прокрутке:

```csharp
carouselView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>Управление положением прокрутки

При прокрутке элемента в представлении точное расположение элемента после прокрутки можно указать с помощью `position` аргумента [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) методов. Этот аргумент принимает [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) член перечисления.

### <a name="makevisible"></a>макевисибле

Элемент [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться, пока он не будет виден в представлении:

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

В этом примере кода создается минимальная прокрутка, необходимая для прокрутки элемента на представление.

> [!NOTE]
> [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition)Элемент используется по умолчанию, если `position` аргумент не указан при вызове `ScrollTo` метода.

### <a name="start"></a>Начать

Элемент [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться до начала представления:

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

В этом примере кода создается элемент, который прокручивается до начала представления.

### <a name="center"></a>Центр.

Элемент [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться по центру представления:

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

Этот пример кода приводит к переходу элемента в центр представления.

### <a name="end"></a>Конец

Элемент [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) указывает, что элемент должен быть прокручиваться до конца представления:

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Этот пример кода приводит к прокрутке элемента до конца представления.

## <a name="control-scroll-position-when-new-items-are-added"></a>Управление позицией прокрутки при добавлении новых элементов

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Определяет `ItemsUpdatingScrollMode` свойство, которое поддерживается связываемым свойством. Это свойство возвращает или задает `ItemsUpdatingScrollMode` значение перечисления, представляющее поведение прокрутки `CarouselView` при добавлении новых элементов к нему. Перечисление `ItemsUpdatingScrollMode` определяет следующие члены:

- `KeepItemsInView`корректирует смещение прокрутки, чтобы при добавлении новых элементов отображался первый видимый элемент.
- `KeepScrollOffset`поддерживает смещение прокрутки относительно начала списка при добавлении новых элементов.
- `KeepLastItemInView`корректирует смещение прокрутки для сохранения последнего элемента, отображаемого при добавлении новых элементов.

Значение свойства по умолчанию `ItemsUpdatingScrollMode` — `KeepItemsInView` . Поэтому при добавлении новых элементов к [`CarouselView`](xref:Xamarin.Forms.CarouselView) первому видимому элементу в списке будут отображаться. Чтобы убедиться, что новые добавленные элементы всегда видны в нижней части списка, `ItemsUpdatingScrollMode` свойство должно иметь значение `KeepLastItemInView` :

```xaml
<CarouselView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>Видимость полосы прокрутки

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Определяет `HorizontalScrollBarVisibility` `VerticalScrollBarVisibility` Свойства и, поддерживающие привязку свойств. Эти свойства получают или задают [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) значение перечисления, представляющее, когда отображается горизонтальная или вертикальная полоса прокрутки. Перечисление `ScrollBarVisibility` определяет следующие члены:

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

По умолчанию для [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) свойства задано значение `SnapPointsType.MandatorySingle` , которое обеспечивает прокрутку только по одному элементу за раз.

На следующих снимках экрана показано, что [`CarouselView`](xref:Xamarin.Forms.CarouselView) отключена привязка.

[![Снимок экрана Карауселвиев без точек привязки в iOS и Android](scrolling-images/snappoints-none.png "Карауселвиев без точек привязки")](scrolling-images/snappoints-none-large.png#lightbox "Карауселвиев без точек привязки")

### <a name="snap-points-alignment"></a>Выравнивание точек привязки

[`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)Перечисление `Start` определяет `Center` члены, и `End` .

> [!IMPORTANT]
> Значение [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) свойства учитывается только в том случае, если [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) свойство имеет значение `Mandatory` , или `MandatorySingle` .

#### <a name="start"></a>Начать

`SnapPointsAlignment.Start`Элемент указывает, что точки привязки выравниваться с ведущим ребром элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки при горизонтальной прокрутке [`CarouselView`](xref:Xamarin.Forms.CarouselView) , левый элемент будет выставляться слева от представления:

[![Снимок экрана Карауселвиев с начальной точкой привязки в iOS и Android](scrolling-images/snappoints-start.png "Карауселвиев с начальной точкой привязки")](scrolling-images/snappoints-start-large.png#lightbox "Карауселвиев с начальной точкой привязки")

#### <a name="center"></a>Центр.

`SnapPointsAlignment.Center`Элемент указывает, что точки привязки выровнены по центру элементов.

По умолчанию для [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) свойство имеет значение `Center` . Однако для полноты в следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки при горизонтальной прокрутке [`CarouselView`](xref:Xamarin.Forms.CarouselView) , центрированный элемент будет выровнен по центру представления:

[![Снимок экрана Карауселвиев с центральными точками привязки в iOS и Android](scrolling-images/snappoints-center.png "Карауселвиев с центральными точками привязки")](scrolling-images/snappoints-center-large.png#lightbox "Карауселвиев с центральными точками привязки")

#### <a name="end"></a>Конец

`SnapPointsAlignment.End`Элемент указывает, что точки привязки выравниваться с конечным ребром элементов. В следующем примере XAML показано, как задать этот элемент перечисления:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

Когда пользователь настраивается для инициации прокрутки при горизонтальной прокрутке [`CarouselView`](xref:Xamarin.Forms.CarouselView) , правый элемент будет выставляться по правому краю представления.

[![Снимок экрана Карауселвиев с конечными точками привязки в iOS и Android](scrolling-images/snappoints-end.png "Карауселвиев с конечными точками привязки")](scrolling-images/snappoints-end-large.png#lightbox "Карауселвиев с конечными точками привязки")

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
