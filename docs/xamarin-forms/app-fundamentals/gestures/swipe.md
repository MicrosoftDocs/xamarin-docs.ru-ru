---
title: Добавление распознавателя жестов прокрутки
description: Эта статья поясняет, как распознать жест прокрутки в представлении.
ms.prod: xamarin
ms.assetid: 164976C2-1429-49FB-9EB6-621E2681C19B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cbd1bdd097bc205f82f98574df5c356b03769e29
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562656"
---
# <a name="add-a-swipe-gesture-recognizer"></a>Добавление распознавателя жестов прокрутки

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)

_Жест прокрутки происходит, когда палец перемещается по экрану в горизонтальном или вертикальном направлении. Он часто используется для перемещения по содержимому. Примеры кода для этой статьи взяты из примера [жеста прокрутки](/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)._

Чтобы заставить [`View`](xref:Xamarin.Forms.View) распознавать жест прокрутки, создайте экземпляр [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer), задайте для свойства [`Direction`](xref:Xamarin.Forms.SwipeGestureRecognizer.Direction) значение перечисления [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) (`Left`, `Right`, `Up` или `Down`), при необходимости задайте свойство [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold), обработайте событие [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) и добавьте новый распознаватель жестов в коллекцию [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) представления. В следующем примере кода показан `SwipeGestureRecognizer`, присоединенный к [`BoxView`](xref:Xamarin.Forms.BoxView):

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

Вот эквивалент в коде C#:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
```

Класс [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) также включает в себя свойство [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold), для которого при необходимости можно задать значение `uint`, представляющее минимальное расстояние прокрутки, необходимое для распознавания жеста прокрутки, в аппаратно-независимых единицах. Значение по умолчанию для этого свойства равно 100, это означает, что любая прокрутка менее 100 аппаратно-независимых единиц будет игнорироваться.

## <a name="recognizing-the-swipe-direction"></a>Распознавание направления прокрутки

В приведенном выше примере свойство [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) задается равным одному значению из перечисления [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection). Однако также можно задать для этого свойства несколько значений из перечисления `SwipeDirection`, чтобы событие [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) активировалось в ответ на прокрутку в более чем одном направлении. Однако при этом действует то ограничение, что отдельный [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) может распознавать только жесты прокрутки по одной оси. Таким образом, жесты прокрутки по горизонтальной оси можно распознать, задав для свойства `Direction` значения `Left` и `Right`:

```xaml
<SwipeGestureRecognizer Direction="Left,Right" Swiped="OnSwiped"/>
```

Аналогичным образом, жесты прокрутки по вертикальной оси можно распознать, задав для свойства [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) значения `Up` и `Down`:

```csharp
var swipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up | SwipeDirection.Down };
```

Кроме того, для каждого направления жеста прокрутки можно создать [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer), чтобы распознавать такие жесты в каждом направлении:

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Right" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Up" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Down" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

Вот эквивалент в коде C#:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;
var rightSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Right };
rightSwipeGesture.Swiped += OnSwiped;
var upSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up };
upSwipeGesture.Swiped += OnSwiped;
var downSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Down };
downSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
boxView.GestureRecognizers.Add(rightSwipeGesture);
boxView.GestureRecognizers.Add(upSwipeGesture);
boxView.GestureRecognizers.Add(downSwipeGesture);
```

> [!NOTE]
> В приведенных выше примерах один обработчик событий реагирует на возникновение события [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped). Однако каждый экземпляр [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) при необходимости может использовать другой обработчик событий.

## <a name="responding-to-the-swipe"></a>Реагирование на жест прокрутки

Обработчик событий для события [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) показан в следующем примере:

```csharp
void OnSwiped(object sender, SwipedEventArgs e)
{
    switch (e.Direction)
    {
        case SwipeDirection.Left:
            // Handle the swipe
            break;
        case SwipeDirection.Right:
            // Handle the swipe
            break;
        case SwipeDirection.Up:
            // Handle the swipe
            break;
        case SwipeDirection.Down:
            // Handle the swipe
            break;
    }
}
```

[`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs) можно проверить, чтобы определить направление прокрутки, с помощью пользовательской логики, реагирующей на прокрутку по мере необходимости. Направление жеста прокрутки можно получить из свойства [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) аргументов событий, которому будет присвоено одно из значений из перечисления [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection). Кроме того, аргументы события также имеют свойство [`Parameter`](xref:Xamarin.Forms.SwipedEventArgs.Parameter), которому будет присвоено значение свойства [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter), если оно определено.

## <a name="using-commands"></a>Использование команд

Класс [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) также включает в себя свойства [`Command`](xref:Xamarin.Forms.SwipeGestureRecognizer.Command) и [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter). Эти свойства обычно используются в приложениях на основе шаблона "модель — представление — модель представления" (MVVM). Свойство `Command` определяет `ICommand`, вызываемый при распознавании жеста прокрутки, при этом свойство `CommandParameter` определяет объект, передаваемый в `ICommand.`. Следующий пример кода показывает, как привязать свойство `Command` к `ICommand`, определенному в модели представления, чей экземпляр задан в качестве [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left, CommandParameter = "Left" };
leftSwipeGesture.SetBinding(SwipeGestureRecognizer.CommandProperty, "SwipeCommand");
boxView.GestureRecognizers.Add(leftSwipeGesture);
```

Эквивалентный код XAML:

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Command="{Binding SwipeCommand}" CommandParameter="Left" />
    </BoxView.GestureRecognizers>
</BoxView>
```

`SwipeCommand` является свойством типа `ICommand`, определенным в экземпляре модели представления, который задан в качестве [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы. Когда жест прокрутки распознается, выполняется метод `Execute` объекта `SwipeCommand`. Аргумент для метода `Execute` — это значение свойства [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter). Дополнительные сведения о командах см. в разделе [Командный интерфейс](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="creating-a-swipe-container"></a>Создание контейнера прокрутки

Класс `SwipeContainer`, показанный в следующем примере кода, является универсальным классом распознавания прокрутки, который используется в качестве оболочки для [`View`](xref:Xamarin.Forms.View), чтобы осуществить распознавание жеста прокрутки:

```csharp
public class SwipeContainer : ContentView
{
    public event EventHandler<SwipedEventArgs> Swipe;

    public SwipeContainer()
    {
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Left));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Right));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Up));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Down));
    }

    SwipeGestureRecognizer GetSwipeGestureRecognizer(SwipeDirection direction)
    {
        var swipe = new SwipeGestureRecognizer { Direction = direction };
        swipe.Swiped += (sender, e) => Swipe?.Invoke(this, e);
        return swipe;
    }
}
```

Класс `SwipeContainer` создает объекты [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) для всех четырех направлений прокрутки и присоединяет обработчики событий `Swipe`. Эти обработчики событий вызывают событие `Swipe`, определенное `SwipeContainer`.

В следующем примере кода XAML показан класс `SwipeContainer`, используемый в качестве оболочки для [`BoxView`](xref:Xamarin.Forms.BoxView):

```xaml
<ContentPage ...>
    <StackLayout>
        <local:SwipeContainer Swipe="OnSwiped" ...>
            <BoxView Color="Teal" ... />
        </local:SwipeContainer>
    </StackLayout>
</ContentPage>
```

В следующем примере кода показано, как `SwipeContainer` используется качестве оболочки для [`BoxView`](xref:Xamarin.Forms.BoxView) на странице C#:

```csharp
public class SwipeContainerPageCS : ContentPage
{
    public SwipeContainerPageCS()
    {
        var boxView = new BoxView { Color = Color.Teal, ... };
        var swipeContainer = new SwipeContainer { Content = boxView, ... };
        swipeContainer.Swipe += (sender, e) =>
        {
          // Handle the swipe
        };

        Content = new StackLayout
        {
            Children = { swipeContainer }
        };
    }
}
```

Когда [`BoxView`](xref:Xamarin.Forms.BoxView) получает жест прокрутки, активируется событие [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) в [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer). Это обрабатывается классом `SwipeContainer`, активирующим собственное событие `Swipe`. Это событие `Swipe` обрабатывается на странице. [`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs) можно проверить, чтобы определить направление прокрутки, с помощью пользовательской логики, реагирующей на прокрутку по мере необходимости.

## <a name="related-links"></a>Связанные ссылки

- [Жест прокрутки (пример)](/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [SwipeGestureRecognizer](xref:Xamarin.Forms.SwipeGestureRecognizer)