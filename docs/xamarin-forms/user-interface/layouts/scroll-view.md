---
Title: " Xamarin.Forms скроллвиев" Description: ' в этой статье объясняется, как использовать Xamarin.Forms класс скроллвиев для представления макетов, которые не могут поместиться на одном экране, и которые имеют место для клавиатуры.
MS. произв. MS. AssetID: MS. Technology: Автор: MS. author: MS. Дата: нет-Loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinforms-scrollview"></a>Xamarin.Formsскроллвиев

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView)содержит макеты и позволяет им прокручивать экран. `ScrollView`также используется, чтобы разрешить представлениям автоматически перемещаться в видимую часть экрана при отображении клавиатуры.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>Назначение

[`ScrollView`](xref:Xamarin.Forms.ScrollView)можно использовать для того, чтобы более крупные представления отображались на небольших телефонах. Например, макет, который работает на iPhone 6S, может быть обрезан на iPhone 4S. Использование компонента `ScrollView` позволяет отображать обрезанные части макета на экране меньшего размера.

## <a name="usage"></a>Использование

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView)объекты не должны быть вложенными. Кроме того, `ScrollView` s не должен быть вложен в другие элементы управления, обеспечивающие прокрутку, например `ListView` и `WebView` .

[`ScrollView`](xref:Xamarin.Forms.ScrollView)предоставляет `Content` свойство, для которого можно задать одно представление или макет. Рассмотрим этот пример макета с очень большим Боксвиев, за которым следует `Entry` :

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

В C#:

```csharp
public class ScrollingDemoCode : ContentPage
{
    public ScrollingDemoCode()
    {
        StackLayout stackLayout = new StackLayout();
        stackLayout.Children.Add(new BoxView { BackgroundColor = Color.Red, HeightRequest = 600, WidthRequest = 150 });
        stackLayout.Children.Add(new Entry());
        ScrollView scrollView = new ScrollView();
        scrollView.Content = stackLayout;
        Content = scrollView;
    }
}
```

Перед прокруткой пользователя отображается только элемент `BoxView` :

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

Обратите внимание, что когда пользователь начинает вводить текст в `Entry` , представление прокручивается, чтобы оно не отображалось на экране:

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>Свойства

[`ScrollView`](xref:Xamarin.Forms.ScrollView)определяет следующие свойства:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty)Возвращает [`Size`](xref:Xamarin.Forms.Size) значение, представляющее размер содержимого.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty)Возвращает или задает [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) значение перечисления, представляющее направление прокрутки объекта `ScrollView` .
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty)Возвращает значение типа `double` , представляющее текущую точку прокрутки X.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty)Возвращает объект `double` , представляющий текущую точку прокрутки по оси Y.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty)Возвращает или задает [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) значение, которое представляет, когда отображается горизонтальная полоса прокрутки.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty)Возвращает или задает [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) значение, которое представляет, когда отображается вертикальная полоса прокрутки.

> [!NOTE]
> Прокрутку можно отключить, присвоив [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) свойству значение `Neither` .

## <a name="methods"></a>Методы

[`ScrollView`](xref:Xamarin.Forms.ScrollView)предоставляет `ScrollToAsync` метод, который можно использовать для прокрутки представления либо с помощью координат, либо путем указания конкретного представления, которое должно быть сделано видимым.

При использовании координат укажите `x` `y` координаты и, а также логическое значение, указывающее, следует ли анимировать прокрутку:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> `ScrollToAsync`Метод не будет приводить к прокрутке, если [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) свойство имеет значение `Neither` .

При прокрутке к определенному элементу `ScrollToPosition` перечисление указывает, где в представлении будет отображаться элемент:

- По **центру** &ndash; Прокручивает элемент до центра видимой части представления.
- **Конец** &ndash; Прокручивает элемент до конца видимой части представления.
- **Макевисибле** &ndash; Прокручивает элемент таким образом, чтобы он был виден в представлении.
- **Запуск** &ndash; Прокручивает элемент до начала видимой части представления.

`IsAnimated`Свойство указывает, как будет прокручиваться представление. Если задано значение `true` , будет использоваться плавная анимация, а не мгновенное перемещение содержимого в представление.

## <a name="events"></a>События

[`ScrollView`](xref:Xamarin.Forms.ScrollView)определяет только одно событие, `Scrolled` . `Scrolled`вызывается после завершения прокрутки представления. Обработчик событий `Scrolled` принимает `ScrolledEventArgs` , который имеет `ScrollX` `ScrollY` Свойства и. Ниже показано, как обновить метку с текущей позицией прокрутки `ScrollView` :

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Обратите внимание, что позиции прокрутки могут быть отрицательными из-за того, что при прокрутке в конце списка будет действовать стрелка.

## <a name="related-links"></a>Связанные ссылки

- [Макет (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Пример Бусинесстумбле (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
