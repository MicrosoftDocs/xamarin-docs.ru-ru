---
title: Сводная информация о главе 22. Анимация
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводная информация о главе 22. Анимация
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a8a089c210a3fe2f48dbe32bf8cda6179af2a78
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136634"
---
# <a name="summary-of-chapter-22-animation"></a>Сводная информация о главе 22. Анимация

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)

Вы уже знаете, как создавать собственные анимации с помощью таймера Xamarin.Forms или `Task.Delay`, но в большинстве случаев проще использовать штатные средства анимации Xamarin.Forms. Анимация реализуется в следующих трех классах:

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) поддерживает обобщенный подход;
- [`Animation`](xref:Xamarin.Forms.Animation) дает больше гибкости, но усложняет процесс;
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions) наиболее универсален и работает на самом низком уровне.

Обычно анимация строится на основе свойств, подкрепленных привязываемыми свойствами. Это не обязательно, но только такие свойства динамически реагируют на изменения.

Для анимаций не существует стандартного интерфейса XAML, но вы можете интегрировать анимацию в XAML с помощью методик из главы 23 [ **Триггеры и реакции на событие**](chapter23.md).

## <a name="exploring-basic-animations"></a>Изучение основных анимаций

Основные функции анимации и методы расширения представлены в классе [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions). Эти методы применимы только к тем объектам, которые наследуют от `VisualElement`. Самая простая анимация использует свойства преобразования, описанные в [`Chapter 21. Transforms`](chapter21.md).

Пример [**AnimationTryout**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) демонстрирует применение обработчика событий `Clicked` из `Button` для вызова метода расширения [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), чтобы вращать кнопку по кругу.

Метод `RotateTo` изменяет значение свойства `Rotation` объекта `Button` в диапазоне от 0 до 360 градусов за четверть секунды (по умолчанию). При повторном нажатии на `Button` он ничего не делает, так как свойство `Rotation` уже имеет значение 360 градусов.

### <a name="setting-the-animation-duration"></a>Настройка продолжительности анимации

Второй аргумент `RotateTo` задает длительность в миллисекундах. Если вы укажете большое значение, нажатие `Button` в процессе анимации будет запускать новую анимацию с текущего угла.

### <a name="relative-animations"></a>Относительные анимации

Метод `RelRotateTo` выполняет относительное вращение, добавляя указанное значение к существующему значению. Этот метод позволяет несколько раз нажимать `Button`, каждый раз увеличивая значение `Rotation` на 360 градусов.

### <a name="awaiting-animations"></a>Анимации с ожиданием

Все методы анимации в `ViewExtensions` возвращают объекты `Task<bool>`. Это означает, что вы можете определить серию последовательных анимаций с помощью `ContinueWith` или `await`. При завершении `bool` возвращается значение `false`, если анимация не была прервана, или `true` в случае отмены вызовом метода [`CancelAnimation`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)), который отменяет запущенную другим методом из `ViewExtensions` анимацию для того же элемента.

### <a name="composite-animations"></a>Составные анимации

Вы можете сочетать анимации с ожиданием и без ожидания, чтобы создать составные анимации. В `ViewExtensions` есть анимации, которые работают со свойствами преобразования `TranslationX`, `TranslationY` и `Scale`.

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

Обратите внимание, что `TranslateTo` может влиять одновременно на свойства `TranslationX` и `TranslationY`.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll и Task.WhenAny

Вы также можете управлять несколькими анимациями одновременно, используя [`Task.WhenAll`](xref:System.Threading.Tasks.Task.WhenAll*) для получения сигнала о завершении нескольких задач и (или) [`Task.WhenAny`](xref:System.Threading.Tasks.Task.WhenAny*) для получения сигнала о завершении первой из нескольких задач.

### <a name="rotation-and-anchors"></a>Вращение и привязка

При вызове методов `ScaleTo`, `RelScaleTo`, `RotateTo` и `RelRotateTo` вы можете настроить свойства [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) и [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY), чтобы указать центральную точку для масштабирования и вращения.

Пример [**CircleButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) демонстрирует этот подход, выполняя вращение `Button` вокруг угла страницы.

### <a name="easing-functions"></a>Функции для реалистичной анимации

Обычно анимации выполняют строго линейное смещение от начального до конечного положений. Функции для реалистичной анимации позволяют ускорять или замедлять анимацию в процессе выполнения. Последний необязательный аргумент в методах анимации имеет тип [`Easing`](xref:Xamarin.Forms.Easing). Это класс, который определяет 11 статических полей только для чтения с типом `Easing`:

- [`Linear`](xref:Xamarin.Forms.Easing.Linear), значение по умолчанию;
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn), [`SinOut`](xref:Xamarin.Forms.Easing.SinOut) и [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut);
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn), [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut) и [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut);
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) и [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) и [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

Суффикс `In` указывает, что эффект будет применен в начале анимации, `Out` применяет эффект в конце, а `InOut` — и в начале, и в конце анимации.

Пример [**BounceButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) демонстрирует применение функций для реалистичной анимации.

### <a name="your-own-easing-functions"></a>Собственные функции для реалистичной анимации

Вы даже можете определить собственные функции для реалистичной анимации, передавая [`Func<double, double>`](xref:System.Func`2) в [конструктор `Easing`](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double})). Также `Easing` определяет неявное преобразование из `Func<double, double>` в `Easing`. Аргумент функции для реалистичной анимации всегда находится в диапазоне от 0 до 1, так как анимация выполняется линейно от начального до конечного положения. Эта функция *обычно* возвращает значение в диапазоне от 0 до 1, но на короткий момент времени ее значение может выходить за пределы этого диапазона (например, при применении функций `SpringIn` и `SpringOut`) или применяться по другим правилам, если вы хорошо их понимаете.

Пример [**UneasyScale**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) демонстрирует пользовательскую функцию для реалистичной анимации, а в [**CustomCubicEase**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) вы найдете еще одну.

Пример [**SwingButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) также содержит пользовательскую функцию для реалистичной анимации наряду с методикой изменения свойств `AnchorX` и `AnchorY` в пределах последовательности анимаций с вращением.

В библиотеке [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) есть класс [`JiggleButton`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs), который использует пользовательскую функцию для реалистичной анимации для "дрожания" кнопки при нажатии. Эта техника демонстрируется в примере [**JiggleButtonDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo).

### <a name="entrance-animations"></a>Анимации входа

Анимации достаточно часто применяются при первом открытии страницы. Такие анимации удобно запускать из переопределения [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) для страницы. Для них лучше всего настроить в XAML желаемый вид страницы *после* завершения анимации, а начальный макет и ход анимации выполнить из кода.

Пример [**FadingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) использует метод расширения [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) для плавного появления содержимого страницы.

Пример [**SlidingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) использует метод расширения [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) для "вдвигания" содержимого страницы слева и справа.

Пример [**SwingingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) использует метод расширения [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) для анимации свойства `RotationY`. Также доступен метод [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)).

### <a name="forever-animations"></a>Бесконечные анимации

Противоположный крайний случай — выполнение анимации до тех пор, пока не завершится работа программы Обычно такой вариант применяется для демонстрационных целей.

Пример [**FadingTextAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) использует анимацию [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) для плавного появления и удаления двух элементов текста.

Пример [**PalindromeAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) демонстрирует палиндром, в котором последовательно переворачиваются на 180 отдельные буквы, пока все не окажутся вверх ногами. После этого вся строка отражается на 180 градусов, демонстрируя тот же текст, который был в исходной строке.

Пример [**CopterAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) вращает несложный вертолет `BoxView`, одновременно поворачивая его вокруг центра экрана.

Пример [**RotatingSpokes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) вращает лучи `BoxView` вокруг центра экрана, а затем вращает каждый луч по отдельности, что создает интересные эффекты:

[![Снимок экрана с тремя изображениями вращающихся лучей](images/ch22fg21-small.png "Вращение лучей")](images/ch22fg21-large.png#lightbox "Вращение лучей")

Но постоянное увеличение значения `Rotation` для элемента может привести к проблемам, как демонстрирует пример [**RotationBreakdown**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown).

Пример [**SpinningImage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) использует [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) и [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) для создания иллюзии вращения растрового изображения в трехмерном пространстве.

### <a name="animating-the-bounds-property"></a>Анимация свойства привязки

Остался лишь один метод `ViewExtensions`, который мы еще не продемонстрировали. Это метод [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), который, по сути, анимирует свойство только для чтения [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds), вызывая метод [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)). Обычно этот метод вызывается в производных от `Layout`, как будет описано далее в главе 26 [ **CustomLayouts**](chapter26.md).

Метод `LayoutTo` следует использовать только в исключительных случаях. Программа [**BouncingBox**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) применяет его для сжатия и растягивания элемента `BoxView`, когда тот "отскакивает" от краев страницы.

Пример [**XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) использует `LayoutTo` для перемещения плиток по принципу классической игры "пятнашки", в которой вместо чисел собирается и разбирается изображение.

[![Снимок экрана с тремя изображениями Xamarin Xuzzle](images/ch22fg26-small.png "Игра-головоломка Xuzzle")](images/ch22fg26-large.png#lightbox "Игра-головоломка Xuzzle")

### <a name="your-own-awaitable-animations"></a>Собственные анимации с ожиданием

Пример [**TryAwaitableAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) создает анимацию с поддержкой ожидания. Важнейшим здесь является класс `Task`[`TaskCompletionSource`, который умеет возвращать из метода объект ](xref:System.Threading.Tasks.TaskCompletionSource`1) и информировать о завершении анимации.

## <a name="deeper-into-animations"></a>Более подробно об анимациях

Система анимаций в Xamarin.Forms может запутать неподготовленного читателя. Помимо класса `Easing`, в эту систему входят классы `ViewExtensions`, `Animation` и `AnimationExtension`.

### <a name="viewextensions-class"></a>Класс ViewExtensions

Вы уже видели пример [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions). Он определяет девять методов, которые возвращают `Task<bool>` и [`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)). Семь из этих девяти методов работают со свойствами преобразования. Остальные два — это [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), который изменяет свойство [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) и [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), который вызывает метод [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)).

### <a name="animation-class"></a>Класс Animation

Класс [`Animation`](xref:Xamarin.Forms.AnimationExtensions) содержит [конструктор](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action)) с пятью аргументами, которые позволяют определить методы обратного вызова и информирования о завершении, а также параметры самой анимации.

Дочерние анимации можно добавлять с помощью [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)), [`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)), [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) и перегрузки [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)).

После этого объект анимации запускается вызовом метода [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})).

### <a name="animationextensions-class"></a>Класс AnimationExtensions

Класс [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) содержит в основном методы расширения. Есть несколько версий метода `Animate`, а общий метод [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) настолько многообразен, что можно обойтись одной этой функцией для абсолютно любой анимации.

## <a name="working-with-the-animation-class"></a>Работа с классом Animation

Пример [**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) демонстрирует выполнение нескольких разных анимаций через класс [`Animation`](xref:Xamarin.Forms.Animation).

### <a name="child-animations"></a>Дочерние анимации

Пример [**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) также демонстрирует несколько дочерних анимаций, в которых используются родственные методы [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) и [`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)).

### <a name="beyond-the-high-level-animation-methods"></a>Выходим за пределы высокоуровневых методов анимации

Пример [**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) также демонстрирует выполнение анимации без использования свойств, с которыми работают методы `ViewExtensions`. В одном из этих примеров увеличивается ряд периодов, а в другом анимируется свойство `BackgroundColor`.

### <a name="more-of-your-own-awaitable-methods"></a>Еще немного о собственных методах с поддержкой ожидания

Метод [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) из `ViewExtensions` не работает с функцией [`Easing.SpringOut`](xref:Xamarin.Forms.Easing.SpringOut). Он останавливается, когда функция для реалистичной анимации возвращает результат больше 1.

Библиотека [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) содержит класс [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) с методами расширения [`TranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) и [`TranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49), при вызове которых не возникает эта проблема, а также с методами [`CancelTranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) и [`CancelTranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) для отмены выполняемых анимаций.

В примере [**SpringSlidingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) демонстрируется метод `TranslateXTo`.

Класс `MoreExtensions` также содержит метод расширения [`TranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76), который сочетает преобразования по X и по Y, а также метод [`CancelTranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113).

### <a name="implementing-a-bezier-animation"></a>Реализация анимации Безье

Есть возможность разработать анимацию, которая перемещает элемент по траектории сплайна Безье. Библиотека [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) содержит структуру [`BezierSpline`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs), которая инкапсулирует сплайн Безье, и перечисление [`BezierTangent`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) для управления ориентацией.

Класс [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) содержит метод расширения [`BezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) и метод [`CancelBezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161).

Пример [**BezierLoop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) демонстрирует анимацию перемещения элемента по траектории кривой Безье.

## <a name="working-with-animationextensions"></a>Работа с классом AnimationExtensions

В стандартной коллекции отсутствует анимация цвета. Это связано с тем, что не существует разумного способа интерполировать два значения `Color`. Можно выполнять такую интерполяцию в координатах RGB-значения, но подход через HSL-значения ничем не хуже.

Поэтому класс [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) в библиотеке [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) содержит два метода анимации `Color`: [`RgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166) и [`HslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Они дополняются двумя методами отмены: [`CancelRgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) и [`CancelHslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206).)

Оба этих метода используют [`ColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), который выполняет анимацию путем вызова расширенного универсального метода [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) из [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions).

Пример [**ColorAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) демонстрирует применение двух типов анимации цвета.

## <a name="structuring-your-animations"></a>Структурирование анимаций

Иногда бывает удобно выражать анимации в XAML и использовать их совместно с MVVM. Эта технология рассматривается далее в главе 23 [ **Триггеры и реакции на событие**](chapter23.md).

## <a name="related-links"></a>Связанные ссылки

- [Глава 22, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Примеры для главы 22](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Анимация](~/xamarin-forms/user-interface/animation/index.md)
