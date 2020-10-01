---
title: Вызов событий из эффекта
description: Эффект может определять вызов события, сигнализирующего об изменениях в базовом собственном представлении. В этой статье описываются реализация низкоуровневого отслеживания мультисенсорного ввода, а также создание событий, сигнализирующих о действиях прикосновения.
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3bbad7f96403cb1f386b80b2923b0cdd222f9596
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91555883"
---
# <a name="invoking-events-from-effects"></a>Вызов событий из эффекта

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)

_Эффект может определять вызов события, сигнализирующего об изменениях в базовом собственном представлении. В этой статье описываются реализация низкоуровневого отслеживания мультисенсорного ввода, а также создание событий, сигнализирующих о прикосновениях._

Описываемый в этой статье эффект предоставляет доступ к низкоуровневым событиям прикосновения. Эти низкоуровневые события недоступны через существующие классы `GestureRecognizer`, однако важны для некоторых типов приложений. Например, приложение для рисования пальцами должно отслеживать движение отдельных пальцев по экрану. Клавиатура музыкального приложения должна обнаруживать, когда пользователь касается отдельных клавиш и отпускает их, а также скольжение пальца между клавишами при исполнении глиссандо.

Этот эффект идеально подходит для отслеживания мультисенсорного ввода, поскольку он может присоединяться к любому элементу Xamarin.Forms.

## <a name="platform-touch-events"></a>События прикосновения платформы

В iOS, Android и на универсальной платформе Windows предусмотрен низкоуровневый API, с помощью которого приложения могут обнаруживать операции сенсорного ввода. Для всех этих платформ существует три базовых типа событий прикосновения:

- *Pressed* — палец касается экрана;
- *Moved* — палец, касающийся экрана, перемещается;
- *Released* — палец перестает касаться экрана.

В среде мультисенсорного ввода несколько пальцев могут одновременно касаться экрана. На различных платформах используются идентификаторы (ID), с помощью которых приложения различают пальцы.

Класс `UIView` в iOS определяет три переопределяемых метода (`TouchesBegan`, `TouchesMoved` и `TouchesEnded`), которые соответствуют этим трем базовым событиям. Использование этих методов описывается в статье [Отслеживание мультисенсорного ввода](~/ios/app-fundamentals/touch/touch-tracking.md). Тем не менее для работы с этими методами в программе iOS не требуется переопределять класс, производный от `UIView`. Объект `UIGestureRecognizer` в iOS также определяет эти три метода. Экземпляр класса, унаследованного от `UIGestureRecognizer`, можно присоединить к любому объекту `UIView`.

Класс `View` в Android определяет переопределяемый метод `OnTouchEvent`, который обрабатывает все операции сенсорного ввода. Тип действия прикосновения определяется с помощью членов перечисления `Down`, `PointerDown`, `Move`, `Up` и `PointerUp`, как описывается в статье [Отслеживание мультисенсорного ввода](~/android/app-fundamentals/touch/touch-tracking.md). Объект `View` в Android также определяет событие `Touch`, что позволяет присоединять обработчик событий к любому объекту `View`.

На универсальной платформе Windows (UWP) класс `UIElement` определяет события `PointerPressed`, `PointerMoved` и `PointerReleased`. Они описываются в статье [Обработка ввода указателя](/windows/uwp/input-and-devices/handle-pointer-input/) на сайте MSDN, а также в документации по API для класса [`UIElement`](/uwp/api/windows.ui.xaml.uielement/).

API `Pointer` на универсальной платформе Windows обеспечивает унифицированную обработку ввода с помощью мыши, прикосновений и пера. По этой причине событие `PointerMoved` вызывается при наведении указателя мыши на элемент, даже если кнопка мыши при этом не нажата. Объект `PointerRoutedEventArgs`, сопровождающий эти события, содержит свойство `Pointer` со свойством `IsInContact`, которое указывает, была ли нажата кнопка мыши или было ли прикосновения пальца к экрану.

Кроме того, на универсальной платформе Windows определяются события `PointerEntered` и `PointerExited`. Они указывают на перемещение указателя мыши или пальца с одного элемента на другой. Допустим, у вас есть два соседних элемента A и B, для каждого из которых установлены обработчики событий указателя. Когда палец касается элемента A, вызывается событие `PointerPressed`. При перемещении пальца элемент A вызывает событие `PointerMoved`. Если палец перемещается с элемента A на элемент B, элемент A вызывает событие `PointerExited`, а элемент B вызывает событие `PointerEntered`. Если после этого пользователь отпустит палец, элемент B вызовет событие `PointerReleased`.

На платформах iOS и Android такое поведение реализовано иначе, чем на UWP. Представление, которое первым получает вызов `TouchesBegan` или `OnTouchEvent` при касании пальца, продолжает принимать все действия прикосновения, даже если палец перемещается в другие представления. На платформе UWP можно реализовать аналогичное поведение, если приложение захватывает указатель. В этом случае в обработчике событий `PointerEntered` элемент вызывает `CapturePointer` и затем принимает все действия прикосновения от этого пальца.

Применяемый на платформе UWP подход эффективен для некоторых типов приложений, например в клавиатурах музыкальных приложений. Каждая клавиша может обрабатывать собственные события прикосновения, а также обнаруживать перемещение пальца между клавишами с помощью событий `PointerEntered` и `PointerExited`.

Поэтому в эффекте отслеживания сенсорного ввода, который описывается в этой статье, реализуется подход, принятый на универсальной платформе Windows (UWP).

## <a name="the-touch-tracking-effect-api"></a>API эффекта отслеживания сенсорного ввода

В примере, [**демонстрирующем эффект отслеживания сенсорного ввода**](/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/), содержится класс (и перечисление) для реализации низкоуровневого отслеживания сенсорного ввода. Эти типы принадлежат пространству имен `TouchTracking` и начинаются со слова `Touch`. Проект библиотеки .NET Standard **TouchTrackingEffectDemos** включает перечисление `TouchActionType` для типа событий прикосновения:

```csharp
public enum TouchActionType
{
    Entered,
    Pressed,
    Moved,
    Released,
    Exited,
    Cancelled
}
```

На всех платформах также реализовано событие, указывающее на отмену события прикосновения.

Класс `TouchEffect` в библиотеке .NET Standard является производным от `RoutingEffect` и определяет событие `TouchAction`, а также метод `OnTouchAction`, который вызывает событие `TouchAction`:

```csharp
public class TouchEffect : RoutingEffect
{
    public event TouchActionEventHandler TouchAction;

    public TouchEffect() : base("XamarinDocs.TouchEffect")
    {
    }

    public bool Capture { set; get; }

    public void OnTouchAction(Element element, TouchActionEventArgs args)
    {
        TouchAction?.Invoke(element, args);
    }
}
```

Также обратите внимание на свойство `Capture`. Для захвата событий прикосновения приложение должно присваивать этому свойству значение `true` до вызова события `Pressed`. В противном случае поведение событий прикосновения будет таким же, как на универсальной платформе Windows.

Класс `TouchActionEventArgs` в библиотеке .NET Standard содержит всю информацию, сопровождающую каждое событие:

```csharp
public class TouchActionEventArgs : EventArgs
{
    public TouchActionEventArgs(long id, TouchActionType type, Point location, bool isInContact)
    {
        Id = id;
        Type = type;
        Location = location;
        IsInContact = isInContact;
    }

    public long Id { private set; get; }

    public TouchActionType Type { private set; get; }

    public Point Location { private set; get; }

    public bool IsInContact { private set; get; }
}
```

Для отслеживания отдельных пальцев в приложении может использоваться свойство `Id`. Обратите внимание на свойство `IsInContact`. Оно всегда имеет значение `true` для событий `Pressed` и `false` для событий `Released`. Кроме того, оно всегда имеет значение `true` для событий `Moved` на платформах iOS и Android. На универсальной платформе Windows свойство `IsInContact` может иметь значение `false` для событий `Moved` в тех случаях, когда программа выполняется на настольном ПК и указатель мыши перемещается без нажатия кнопки.

Вы можете использовать класс `TouchEffect` в собственных приложениях. Для этого необходимо включить этот файл в проект библиотеки .NET Standard для решения и добавить экземпляр в коллекцию `Effects` любого элемента Xamarin.Forms. Чтобы получать события ввода, присоедините обработчик к событию `TouchAction`.

Если вы хотите использовать класс `TouchEffect` в собственном приложении, вам также потребуются реализации для платформ, включенные в решение **TouchTrackingEffectDemos**.

## <a name="the-touch-tracking-effect-implementations"></a>Реализации эффекта отслеживания сенсорного ввода

Реализации класса `TouchEffect` для платформ iOS, Android и UWP описываются далее, начиная с самой простой (UWP) и заканчивая структурно самой сложной реализацией для iOS.

### <a name="the-uwp-implementation"></a>Реализация для универсальной платформы Windows

Реализация класса `TouchEffect` для универсальной платформы Windows является самой простой. Как правило, этот класс является производным от класса `PlatformEffect` и включает два атрибута сборки:

```csharp
[assembly: ResolutionGroupName("XamarinDocs")]
[assembly: ExportEffect(typeof(TouchTracking.UWP.TouchEffect), "TouchEffect")]

namespace TouchTracking.UWP
{
    public class TouchEffect : PlatformEffect
    {
        ...
    }
}
```

Переопределение `OnAttached` сохраняет некоторые данные в виде полей и присоединяет обработчики ко всем событиям указателя:

```csharp
public class TouchEffect : PlatformEffect
{
    FrameworkElement frameworkElement;
    TouchTracking.TouchEffect effect;
    Action<Element, TouchActionEventArgs> onTouchAction;

    protected override void OnAttached()
    {
        // Get the Windows FrameworkElement corresponding to the Element that the effect is attached to
        frameworkElement = Control == null ? Container : Control;

        // Get access to the TouchEffect class in the .NET Standard library
        effect = (TouchTracking.TouchEffect)Element.Effects.
                    FirstOrDefault(e => e is TouchTracking.TouchEffect);

        if (effect != null && frameworkElement != null)
        {
            // Save the method to call on touch events
            onTouchAction = effect.OnTouchAction;

            // Set event handlers on FrameworkElement
            frameworkElement.PointerEntered += OnPointerEntered;
            frameworkElement.PointerPressed += OnPointerPressed;
            frameworkElement.PointerMoved += OnPointerMoved;
            frameworkElement.PointerReleased += OnPointerReleased;
            frameworkElement.PointerExited += OnPointerExited;
            frameworkElement.PointerCanceled += OnPointerCancelled;
        }
    }
    ...
}    
```

Обработчик `OnPointerPressed` вызывает событие эффекта посредством вызова поля `onTouchAction` в методе `CommonHandler`:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerPressed(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Pressed, args);

        // Check setting of Capture property
        if (effect.Capture)
        {
            (sender as FrameworkElement).CapturePointer(args.Pointer);
        }
    }
    ...
    void CommonHandler(object sender, TouchActionType touchActionType, PointerRoutedEventArgs args)
    {
        PointerPoint pointerPoint = args.GetCurrentPoint(sender as UIElement);
        Windows.Foundation.Point windowsPoint = pointerPoint.Position;  

        onTouchAction(Element, new TouchActionEventArgs(args.Pointer.PointerId,
                                                        touchActionType,
                                                        new Point(windowsPoint.X, windowsPoint.Y),
                                                        args.Pointer.IsInContact));
    }
}
```

Обработчик `OnPointerPressed` также проверяет значение свойства `Capture` в классе эффекта в библиотеке .NET Standard и вызывает метод `CapturePointer`, если это свойство имеет значение `true`.

 Другой обработчик событий UWP реализован еще проще:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerEntered(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Entered, args);
    }
    ...
}
```

### <a name="the-android-implementation"></a>Реализация для Android

Реализации этого класса для Android и iOS вынужденно усложняются, поскольку в этих случаях необходимо реализовать события `Exited` и `Entered`, соответствующие перемещению пальца с одного элемента на другой. Обе реализации имеют схожую структуру.

Класс `TouchEffect` в Android устанавливает обработчик для события `Touch`:

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

Этот класс также определяет два статических словаря:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    static Dictionary<Android.Views.View, TouchEffect> viewDictionary =
        new Dictionary<Android.Views.View, TouchEffect>();

    static Dictionary<int, TouchEffect> idToEffectDictionary =
        new Dictionary<int, TouchEffect>();
    ...
```

Объект `viewDictionary` получает новую запись каждый раз при вызове переопределения `OnAttached`:

```csharp
viewDictionary.Add(view, this);
```

Запись удаляется из словаря в `OnDetached`. Каждый экземпляр `TouchEffect` связан с конкретным представлением, к которому подключен эффект. Используя статический словарь, любой экземпляр `TouchEffect` может перечислять все остальные представления и соответствующие им экземпляры `TouchEffect`. Это необходимо для передачи событий от одного представления другому.

Android назначает событиям прикосновения идентификаторы, благодаря которым приложение может отслеживать отдельные пальцы. `idToEffectDictionary` связывает этот идентификатор с экземпляром `TouchEffect`. Элемент добавляется в этот словарь при вызове обработчика `Touch` для нажатия пальцем:

```csharp
void OnTouch(object sender, Android.Views.View.TouchEventArgs args)
{
    ...
    switch (args.Event.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:
            FireEvent(this, id, TouchActionType.Pressed, screenPointerCoords, true);

            idToEffectDictionary.Add(id, this);

            capture = libTouchEffect.Capture;
            break;

```

Элемент удаляется из словаря `idToEffectDictionary`, когда палец перестает касаться экрана. Метод `FireEvent` просто накапливает всю информацию, необходимую для вызова метода `OnTouchAction`:

```csharp
void FireEvent(TouchEffect touchEffect, int id, TouchActionType actionType, Point pointerLocation, bool isInContact)
{
    // Get the method to call for firing events
    Action<Element, TouchActionEventArgs> onTouchAction = touchEffect.libTouchEffect.OnTouchAction;

    // Get the location of the pointer within the view
    touchEffect.view.GetLocationOnScreen(twoIntArray);
    double x = pointerLocation.X - twoIntArray[0];
    double y = pointerLocation.Y - twoIntArray[1];
    Point point = new Point(fromPixels(x), fromPixels(y));

    // Call the method
    onTouchAction(touchEffect.formsElement,
        new TouchActionEventArgs(id, actionType, point, isInContact));
}
```

Все остальные типы прикосновений обрабатываются двумя разными способами: если свойство `Capture` имеет значение `true`, событие прикосновения просто получает информацию `TouchEffect`. Если свойство `Capture` имеет значение `false`, процесс усложняется, поскольку события прикосновения могут перемещаться из одного представления в другое. За обработку таких событий отвечает метод `CheckForBoundaryHop`, который вызывается во время событий перемещения. Этот метод использует оба статических словаря. Он выполняет перечисление словаря `viewDictionary`, чтобы определить представление, которого палец касается в текущий момент. Затем он сохраняет в словарь `idToEffectDictionary` текущий экземпляр `TouchEffect` (и соответственно текущее представление), связанный с конкретным идентификатором:

```csharp
void CheckForBoundaryHop(int id, Point pointerLocation)
{
    TouchEffect touchEffectHit = null;

    foreach (Android.Views.View view in viewDictionary.Keys)
    {
        // Get the view rectangle
        try
        {
            view.GetLocationOnScreen(twoIntArray);
        }
        catch // System.ObjectDisposedException: Cannot access a disposed object.
        {
            continue;
        }
        Rectangle viewRect = new Rectangle(twoIntArray[0], twoIntArray[1], view.Width, view.Height);

        if (viewRect.Contains(pointerLocation))
        {
            touchEffectHit = viewDictionary[view];
        }
    }

    if (touchEffectHit != idToEffectDictionary[id])
    {
        if (idToEffectDictionary[id] != null)
        {
            FireEvent(idToEffectDictionary[id], id, TouchActionType.Exited, pointerLocation, true);
        }
        if (touchEffectHit != null)
        {
            FireEvent(touchEffectHit, id, TouchActionType.Entered, pointerLocation, true);
        }
        idToEffectDictionary[id] = touchEffectHit;
    }
}
```

Если словарь `idToEffectDictionary` был изменен, этот метод может вызвать `FireEvent` для событий `Exited` и `Entered`, чтобы выполнить передачу от одного представления другому. Тем не менее палец может перемещаться в область представления без присоединенного объекта `TouchEffect`, а также из такой области в представление с присоединенным эффектом.

Обратите внимание на блок `try` и `catch` при доступе к представлению. Если был выполнен переход на страницу с последующим возвратом на домашнюю страницу, метод `OnDetached` не вызывается и элементы остаются в словаре `viewDictionary`, хотя Android рассматривает их как ликвидированные.

### <a name="the-ios-implementation"></a>Реализация для iOS

Реализация для iOS схожа с реализацией для Android, однако в ней класс `TouchEffect` должен создавать экземпляр, производный от `UIGestureRecognizer`. Это класс из проекта iOS `TouchRecognizer`. В нем содержатся два статических словаря, в которых хранятся экземпляры `TouchRecognizer`:

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

Структура класса `TouchRecognizer` во многом схожа с классом `TouchEffect` в Android.

> [!IMPORTANT]
> Во многих представлениях в `UIKit` поддержка сенсорного ввода не включена по умолчанию. Ее можно включить, добавив `view.UserInteractionEnabled = true;` в переопределение `OnAttached` в классе `TouchEffect` в проекте iOS. Это должно произойти после получения `UIView` (соответствует элементу, к которому присоединен эффект).

## <a name="putting-the-touch-effect-to-work"></a>Применение эффекта сенсорного ввода на практике

Программа [**TouchTrackingEffectDemos**](/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) содержит пять страниц, на которых можно протестировать применение эффекта отслеживания сенсорного ввода при выполнении стандартных задач.

На странице **перетаскивания элементов BoxView** вы можете добавить элементы `BoxView` в объект `AbsoluteLayout` и затем перетаскивать их по экрану. В [файле XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/BoxViewDraggingPage.xaml) создаются экземпляры двух представлений `Button` для добавления элементов `BoxView` в объект `AbsoluteLayout` и очистки объекта `AbsoluteLayout`.

Метод в [файле кода программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/BoxViewDraggingPage.xaml.cs), который добавляет новый элемент `BoxView` в объект `AbsoluteLayout`, также добавляет объект `TouchEffect` в элемент `BoxView` и присоединяет к эффекту обработчик событий:

```csharp
void AddBoxViewToLayout()
{
    BoxView boxView = new BoxView
    {
        WidthRequest = 100,
        HeightRequest = 100,
        Color = new Color(random.NextDouble(),
                          random.NextDouble(),
                          random.NextDouble())
    };

    TouchEffect touchEffect = new TouchEffect();
    touchEffect.TouchAction += OnTouchEffectAction;
    boxView.Effects.Add(touchEffect);
    absoluteLayout.Children.Add(boxView);
}
```

Обработчик события `TouchAction` обрабатывает все события прикосновения для всех элементов `BoxView`. Но учитывайте то, что он не поддерживает обработку прикосновения двух пальцев к одному элементу `BoxView`, так как эта программа поддерживает только перетаскивание, при которой пальцы будут мешать друг другу. По этой причине на странице определяется встроенный класс для каждого отслеживаемого на текущий момент пальца:

```csharp
class DragInfo
{
    public DragInfo(long id, Point pressPoint)
    {
        Id = id;
        PressPoint = pressPoint;
    }

    public long Id { private set; get; }

    public Point PressPoint { private set; get; }
}

Dictionary<BoxView, DragInfo> dragDictionary = new Dictionary<BoxView, DragInfo>();
```

Словарь `dragDictionary` содержит записи для каждого перетаскиваемого элемента `BoxView`.

При обнаружении действия прикосновения `Pressed` элемент добавляется в этот словарь, а в ответ на действие `Released` удаляется из него. В логике действия `Pressed` должна выполняться проверка на наличие в словаре элемента, соответствующего такому элементу `BoxView`. Если такой элемент присутствует в словаре, значит, этот элемент `BoxView` уже перетаскивается и новое событие относится к прикосновению второго пальца к этому же элементу `BoxView`. Для действий `Moved` и `Released` обработчик событий должен проверять наличие в словаре записи для такого элемента `BoxView`, а также соответствие свойства `Id` прикосновения к перетаскиваемому элементу `BoxView`, аналогичному свойству записи в словаре:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    BoxView boxView = sender as BoxView;

    switch (args.Type)
    {
        case TouchActionType.Pressed:
            // Don't allow a second touch on an already touched BoxView
            if (!dragDictionary.ContainsKey(boxView))
            {
                dragDictionary.Add(boxView, new DragInfo(args.Id, args.Location));

                // Set Capture property to true
                TouchEffect touchEffect = (TouchEffect)boxView.Effects.FirstOrDefault(e => e is TouchEffect);
                touchEffect.Capture = true;
            }
            break;

        case TouchActionType.Moved:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                Rectangle rect = AbsoluteLayout.GetLayoutBounds(boxView);
                Point initialLocation = dragDictionary[boxView].PressPoint;
                rect.X += args.Location.X - initialLocation.X;
                rect.Y += args.Location.Y - initialLocation.Y;
                AbsoluteLayout.SetLayoutBounds(boxView, rect);
            }
            break;

        case TouchActionType.Released:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                dragDictionary.Remove(boxView);
            }
            break;
    }
}
```

В логике действия `Pressed` свойству `Capture` объекта `TouchEffect` присваивается значение `true`. Это влияет на доставку всех последующих событий для этого пальца в тот же обработчик событий.

В логике действия `Moved` элемент `BoxView` перемещается посредством изменения присоединенного свойства `LayoutBounds`. Свойство `Location` аргументов события всегда задается относительно перетаскиваемого элемента `BoxView`. Если такой элемент `BoxView` перетаскивается с постоянной скоростью, свойства `Location` последующих событий будут иметь примерно одинаковые значения. Например, если коснуться пальцем в центре элемента `BoxView`, действие `Pressed` сохранит значение свойства `PressPoint` (50, 50), которое будет оставаться постоянным для последующих событий. Если элемент `BoxView` перетаскивается по диагонали с постоянной скоростью, последующие свойства `Location` во время выполнения действия `Moved` могут иметь значения (55, 55). Это означает, что в логике действия `Moved` добавляется 5 единиц к горизонтальной и вертикальной позиции элемента `BoxView`. При этом элемент `BoxView` перемещается таким образом, чтобы его центр снова оказался непосредственно в точке прикосновения пальца.

Вы можете одновременно перетаскивать несколько элементов `BoxView` разными пальцами.

[![Снимок экрана с тремя изображениями страницы перетаскивания элементов BoxView](touch-tracking-images/boxviewdragging-small.png)](touch-tracking-images/boxviewdragging-large.png#lightbox "Снимок экрана с тремя изображениями страницы перетаскивания элементов BoxView")

### <a name="subclassing-the-view"></a>Создание подкласса представления

Для элемента Xamarin.Forms зачастую проще обрабатывать собственные события прикосновения. Страница **перетаскивания перетаскиваемых элементов BoxView** функционирует аналогично странице **перетаскивания элементов BoxView**, однако на ней перетаскиваемые пользователем элементы являются экземплярами класса [`DraggableBoxView`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/DraggableBoxView.cs), производного от `BoxView`:

```csharp
class DraggableBoxView : BoxView
{
    bool isBeingDragged;
    long touchId;
    Point pressPoint;

    public DraggableBoxView()
    {
        TouchEffect touchEffect = new TouchEffect
        {
            Capture = true
        };
        touchEffect.TouchAction += OnTouchEffectAction;
        Effects.Add(touchEffect);
    }

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!isBeingDragged)
                {
                    isBeingDragged = true;
                    touchId = args.Id;
                    pressPoint = args.Location;
                }
                break;

            case TouchActionType.Moved:
                if (isBeingDragged && touchId == args.Id)
                {
                    TranslationX += args.Location.X - pressPoint.X;
                    TranslationY += args.Location.Y - pressPoint.Y;
                }
                break;

            case TouchActionType.Released:
                if (isBeingDragged && touchId == args.Id)
                {
                    isBeingDragged = false;
                }
                break;
        }
    }
}
```

Конструктор создает и присоединяет объект `TouchEffect`, после чего задает свойство `Capture` при создании первого экземпляра этого объекта. В этом случае словарь не требуется, поскольку в классе уже хранятся значения `isBeingDragged`, `pressPoint` и `touchId`, связанные с каждым пальцем. При обработке действия `Moved` изменяются свойства `TranslationX` и `TranslationY`, благодаря чему логика будет работать даже в том случае, если родительским для класса `DraggableBoxView` не является класс `AbsoluteLayout`.

### <a name="integrating-with-skiasharp"></a>Интеграция с SkiaSharp

В следующих двух демонстрациях используется графика, для работы с которой применяется SkiaSharp. Прежде чем изучать эти примеры, вы можете ознакомиться с [использованием SkiaSharp в Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md). Всю необходимую вам для этого информацию вы сможете найти в первых двух статьях, которые посвящены основам рисования, а также линиям и контурам в SkiaSharp.

На странице **рисования эллипса** вы можете нарисовать эллипс, проводя пальцами по экрану. В зависимости от характера движения пальцев, вы можете нарисовать эллипс из верхнего левого угла в нижний правый либо из любого другого угла в противоположный. Цвет и степень непрозрачности эллипса задаются случайным образом.

[![Снимок экрана с тремя изображениями страницы рисования эллипса](touch-tracking-images/ellipsedrawing-small.png)](touch-tracking-images/ellipsedrawing-large.png#lightbox "Снимок экрана с тремя изображениями страницы рисования эллипса")

При необходимости вы можете коснуться любого нарисованного эллипса и перетащить его в новое место. Для этого применяется метод проверки на попадание, в рамках которого осуществляется поиск графического объекта в конкретной точке. Эллипсы SkiaSharp не являются элементами Xamarin.Forms и не имеют собственной логики обработки объекта `TouchEffect`. Эффект `TouchEffect` должен применяться ко всему объекту `SKCanvasView`.

Файл [EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/EllipseDrawingPage.xaml) создает экземпляр `SKCanvasView` в объекте `Grid` из одной ячейки. Объект `TouchEffect` присоединяется к этому объекту `Grid`:

```xaml
<Grid x:Name="canvasViewGrid"
        Grid.Row="1"
        BackgroundColor="White">

    <skia:SKCanvasView x:Name="canvasView"
                        PaintSurface="OnCanvasViewPaintSurface" />
    <Grid.Effects>
        <tt:TouchEffect Capture="True"
                        TouchAction="OnTouchEffectAction" />
    </Grid.Effects>
</Grid>
```

В Android и на универсальной платформе Windows объект `TouchEffect` можно присоединять напрямую к `SKCanvasView`, однако в iOS такой подход не работает. Обратите внимание, что свойству `Capture` присвоено значение `true`.

Каждый эллипс, отрисовываемый SkiaSharp, представлен объектом типа `EllipseDrawingFigure`:

```csharp
class EllipseDrawingFigure
{
    SKPoint pt1, pt2;

    public EllipseDrawingFigure()
    {
    }

    public SKColor Color { set; get; }

    public SKPoint StartPoint
    {
        set
        {
            pt1 = value;
            MakeRectangle();
        }
    }

    public SKPoint EndPoint
    {
        set
        {
            pt2 = value;
            MakeRectangle();
        }
    }

    void MakeRectangle()
    {
        Rectangle = new SKRect(pt1.X, pt1.Y, pt2.X, pt2.Y).Standardized;
    }

    public SKRect Rectangle { set; get; }

    // For dragging operations
    public Point LastFingerLocation { set; get; }

    // For the dragging hit-test
    public bool IsInEllipse(SKPoint pt)
    {
        SKRect rect = Rectangle;

        return (Math.Pow(pt.X - rect.MidX, 2) / Math.Pow(rect.Width / 2, 2) +
                Math.Pow(pt.Y - rect.MidY, 2) / Math.Pow(rect.Height / 2, 2)) < 1;
    }
}
```

Свойства `StartPoint` и `EndPoint` используются, когда программа обрабатывает сенсорный ввод. Свойство `Rectangle` используется при рисовании эллипса. Свойство `LastFingerLocation` задействуется при перетаскивании эллипса, а метод `IsInEllipse` используется при проверке на попадание. Этот метод возвращает значение `true`, если точка находится внутри эллипса.

В [файле кода программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/EllipseDrawingPage.xaml.cs) содержатся три коллекции:

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

Словарь `draggingFigure` содержит подмножество коллекции `completedFigures`. Обработчик события `PaintSurface` SkiaSharp просто отрисовывает объекты в этих коллекциях `completedFigures` и `inProgressFigures`:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Fill
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (EllipseDrawingFigure figure in completedFigures)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
    foreach (EllipseDrawingFigure figure in inProgressFigures.Values)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
}
```

Самая сложная часть начинается при обработке действия `Pressed`. На этом этапе выполняется проверка на попадание, однако если код обнаруживает эллипс в точке касания пальца, его перетаскивание будет возможно только в том случае, если он на данный момент не перетаскивается с помощью другого пальца. Если в точке касания пальца не обнаруживается эллипс, код начинает обработку процесса рисования нового эллипса:

```csharp
case TouchActionType.Pressed:
    bool isDragOperation = false;

    // Loop through the completed figures
    foreach (EllipseDrawingFigure fig in completedFigures.Reverse<EllipseDrawingFigure>())
    {
        // Check if the finger is touching one of the ellipses
        if (fig.IsInEllipse(ConvertToPixel(args.Location)))
        {
            // Tentatively assume this is a dragging operation
            isDragOperation = true;

            // Loop through all the figures currently being dragged
            foreach (EllipseDrawingFigure draggedFigure in draggingFigures.Values)
            {
                // If there's a match, we'll need to dig deeper
                if (fig == draggedFigure)
                {
                    isDragOperation = false;
                    break;
                }
            }

            if (isDragOperation)
            {
                fig.LastFingerLocation = args.Location;
                draggingFigures.Add(args.Id, fig);
                break;
            }
        }
    }

    if (isDragOperation)
    {
        // Move the dragged ellipse to the end of completedFigures so it's drawn on top
        EllipseDrawingFigure fig = draggingFigures[args.Id];
        completedFigures.Remove(fig);
        completedFigures.Add(fig);
    }
    else // start making a new ellipse
    {
        // Random bytes for random color
        byte[] buffer = new byte[4];
        random.NextBytes(buffer);

        EllipseDrawingFigure figure = new EllipseDrawingFigure
        {
            Color = new SKColor(buffer[0], buffer[1], buffer[2], buffer[3]),
            StartPoint = ConvertToPixel(args.Location),
            EndPoint = ConvertToPixel(args.Location)
        };
        inProgressFigures.Add(args.Id, figure);
    }
    canvasView.InvalidateSurface();
    break;
```

Еще один пример использования SkiaSharp содержит страницу **рисования пальцами**. Вы можете выбрать цвет и ширину кисти в двух представлениях `Picker`, а затем начать рисование одним или несколькими пальцами:

[![Снимок экрана с тремя изображениями страницы рисования пальцами](touch-tracking-images/fingerpaint-small.png)](touch-tracking-images/fingerpaint-large.png#lightbox "Снимок экрана с тремя изображениями страницы рисования пальцами")

В этом примере также используется отдельный класс, который представляет каждую рисуемую на экране линию:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new SKPath();
    }

    public SKPath Path { set; get; }

    public Color StrokeColor { set; get; }

    public float StrokeWidth { set; get; }
}
```

Для отрисовывания каждой линии используется объект `SKPath`. В файле [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/FingerPaintPage.xaml.cs) хранятся две коллекции таких объектов, в одной из которой содержатся рисуемые в данный момент, а в другой завершенные ломаные линии:

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

При обработке действия `Pressed` создается новый объект `FingerPaintPolyline`, для объекта контура вызывается метод `MoveTo` для сохранения начальной точки, после чего этот объект добавляется в словарь `inProgressPolylines`. При обработке действия `Moved` для объекта контура вызывается метод `LineTo` с данными о новой позиции пальца, а при обработке действия `Released` завершенная ломаная линия переносится из словаря `inProgressPolylines` в `completedPolylines`. Сам код рисования SkiaSharp по-прежнему достаточно прост:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    StrokeCap = SKStrokeCap.Round,
    StrokeJoin = SKStrokeJoin.Round
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (FingerPaintPolyline polyline in completedPolylines)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }

    foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }
}
```

### <a name="tracking-view-to-view-touch"></a>Отслеживание сенсорного ввода между представлениями

Во всех предыдущих примерах свойству `Capture` объекта `TouchEffect` присваивалось значение `true` либо при создании объекта `TouchEffect`, либо при вызове события `Pressed`. В таком случае один элемент будет принимать все события, связанные с пальцем, который первым коснулся представления. В заключительном примере этой статьи свойству `Capture`*не присваивается* значение `true`. В этом случае поведение при перемещении пальца, касающегося экрана, от одного элемента к другому реализуется иначе. Элемент, из которого перемещается палец, получает событие, для которого свойству `Type` присваивается значение `TouchActionType.Exited`. Второй элемент получает событие, свойству `Type` которого присвоено значение `TouchActionType.Entered`.

Такой подход к обработке прикосновений эффективен в приложении, имитирующем клавиатуру музыкального приложения. Каждая клавиша должна обнаруживать собственное событие нажатия, а также событие перемещения пальца с одной клавиши на другую.

На странице **беззвучной клавиатуры** определяются небольшие классы [`WhiteKey`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/WhiteKey.cs) и [`BlackKey`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/BlackKey.cs), производные от класса [`Key`](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/Key.cs), который, в свою очередь, является производным от `BoxView`.

Класс `Key` уже готов для использования в реальной музыкальной программе. В нем определены открытые свойства `IsPressed` и `KeyNumber`, которому будут присваиваться коды клавиш, устанавливаемые стандартом MIDI. Класс `Key` также определяет событие `StatusChanged`, которое вызывается при изменении свойства `IsPressed`.

Каждая клавиша поддерживает прикосновения нескольких пальцев. Поэтому в классе `Key` используется объект `List`, содержащий идентификаторы прикосновений для всех пальцев, касающихся в данный момент соответствующей клавиши:

```csharp
List<long> ids = new List<long>();
```

Обработчик события `TouchAction` добавляет идентификатор в список `ids` для типов событий `Pressed` и `Entered`, но делает это только в том случае, если свойству `IsInContact` для события `Entered` присвоено значение `true`. Идентификатор удаляется из списка `List` для событий `Released` и `Exited`:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    switch (args.Type)
    {
      case TouchActionType.Pressed:
          AddToList(args.Id);
          break;

        case TouchActionType.Entered:
            if (args.IsInContact)
            {
                AddToList(args.Id);
            }
            break;

        case TouchActionType.Moved:
            break;

        case TouchActionType.Released:
        case TouchActionType.Exited:
            RemoveFromList(args.Id);
            break;
    }
}

```

Методы `AddToList` и `RemoveFromList` проверяют, был ли объект `List` изменен с пустого на заполненный, и в соответствующем случае вызывают событие `StatusChanged`.

Различные элементы `WhiteKey` и `BlackKey` упорядочиваются в [файле XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffect/TouchTrackingEffect/TouchTrackingEffect/SilentKeyboardPage.xaml) страницы, который удобнее просматривать в альбомном режиме:

[![Снимок экрана с тремя изображениями страницы беззвучной клавиатуры](touch-tracking-images/silentkeyboard-small.png)](touch-tracking-images/silentkeyboard-large.png#lightbox "Снимок экрана с тремя изображениями страницы беззвучной клавиатуры")

Если вы проведете пальцами по клавишам, вы увидите небольшое изменение цвета, свидетельствующее о передаче событий прикосновения от одной клавиши другой.

## <a name="summary"></a>Сводка

В этой статье демонстрируется вызов событий в эффекте, а также описываются принципы написания и использования эффекта для низкоуровневой обработки мультисенсорного ввода.

## <a name="related-links"></a>Связанные ссылки

- [Отслеживание мультисенсорного ввода в iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Отслеживание мультисенсорного ввода в Android](~/android/app-fundamentals/touch/touch-tracking.md)
- [Эффект отслеживания сенсорного ввода (пример)](/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)