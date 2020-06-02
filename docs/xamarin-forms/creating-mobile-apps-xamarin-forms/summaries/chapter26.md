---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 26. Custom layouts''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: deb46d1a70e7c707c998be8669b4af3b8e8d7ead
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136608"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Сводка по главе 26. Пользовательские макеты

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)

Xamarin.Forms включает несколько классов, производных от [`Layout<View>`](xref:Xamarin.Forms.Layout`1):

- `StackLayout`,
- `Grid`,
- `AbsoluteLayout` и
- `RelativeLayout`.

В этой главе описано, как создавать собственные классы, производные от `Layout<View>`.

## <a name="an-overview-of-layout"></a>Общие сведения о макете.

Не существует централизованной системы для обработки макета Xamarin.Forms. Каждый элемент самостоятельно определяет свой размер и способ отображения в определенной области.

### <a name="parents-and-children"></a>Родительские и дочерние объекты

Каждый элемент, имеющий дочерние элементы, отвечает за размещение этих дочерних элементов внутри себя. Именно родительский элемент в конечном итоге определяет размер дочернего элемента, исходя из доступного пространства и желаемого размера дочернего элемента.

### <a name="sizing-and-positioning"></a>Размер и размещение

Построение макета начинается с верхнего уровня визуального дерева, где располагается узел страницы, и распространяется на все его ветви. Самым важным открытым методом в макете является [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)), определяемый в `VisualElement`. Каждый элемент, являющийся родительским для других элементов, вызывает `Layout` для каждого дочернего элемента, чтобы предоставить размер и расположение относительно себя в формате значения [`Rectangle`](xref:Xamarin.Forms.Rectangle). Эти вызовы `Layout` распространяются по всему визуальному дереву.

Вызов `Layout` требуется для того, чтобы элемент отображался на экране. Он устанавливает следующие свойства только для чтения. Они должны соответствовать `Rectangle`, который был передан в этот метод:

- [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) типа `Rectangle`
- [`X`](xref:Xamarin.Forms.VisualElement.X) типа `double`
- [`Y`](xref:Xamarin.Forms.VisualElement.Y) типа `double`
- [`Width`](xref:Xamarin.Forms.VisualElement.Width) типа `double`
- [`Height`](xref:Xamarin.Forms.VisualElement.Height) типа `double`

Перед вызовом `Layout` параметры `Height` и `Width` имеют фиктивные значения &ndash;1.

Вызов `Layout` также вызывает следующие защищенные методы:

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double)), который вызывает
- [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double)), который может быть переопределен.

Наконец, срабатывает следующее событие:

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

Метод `OnSizeAllocated` переопределяется классами `Page` и `Layout`. Это единственные два класса Xamarin.Forms, которые могут иметь дочерние элементы. Переопределенный метод вызывает следующее:

- [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout) для производных от `Page` и [`UpdateChildrenLayout`](xref:Xamarin.Forms.Layout.UpdateChildrenLayout) для производных от `Layout`, которые вызывают
- [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) для производных от `Page` и [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) для производных от `Layout`.

Затем `LayoutChildren` вызывает `Layout` для каждого из дочерних элементов этого элемента. Если хотя бы один дочерний элемент имеет новое значение `Bounds`, возникает следующее событие:

- [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged) для производных от `Page` и [`LayoutChanged`](xref:Xamarin.Forms.Layout.LayoutChanged) для производных от `Layout`.

### <a name="constraints-and-size-requests"></a>Запросы ограничений и размера

Чтобы элемент `LayoutChildren` правильно вызывал `Layout` для всех дочерних элементов, он должен знать *предпочтительный* или *желаемый* размер этих дочерних элементов. Поэтому перед вызовами `Layout` для каждого из дочерних элементов обычно выполняются вызовы

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

После публикации книги метод `GetSizeRequest` был объявлен нерекомендуемым и заменен на

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

Метод `Measure` поддерживает свойство [`Margin`](xref:Xamarin.Forms.View.Margin) и содержит аргумент типа [`MeasureFlag`](xref:Xamarin.Forms.MeasureFlags), который имеет два элемента:

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) без учета полей

Для многих элементов `GetSizeRequest` или `Measure` получает собственный размер элемента от его отрисовщика. Оба метода имеют параметры для *ограничения* ширины и высоты. Например, `Label` на основе ограничения ширины определяет, как переносить несколько строк текста.

`GetSizeRequest` и `Measure` возвращают значение типа [`SizeRequest`](xref:Xamarin.Forms.SizeRequest), которое имеет два свойства:

- [`Request`](xref:Xamarin.Forms.SizeRequest.Request) типа `Size`
- [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum) типа `Size`

Очень часто эти два значения совпадают, а значение `Minimum` обычно можно игнорировать.

Также `VisualElement` определяет защищенный метод, аналогичный `GetSizeRequest`, который вызывается из `GetSizeRequest`:

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double)) возвращает значение `SizeRequest`.

Этот метод теперь считается устаревшим и заменен этим методом:

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

Каждый класс, производный от `Layout` или `Layout<T>`, должен переопределять `OnSizeRequest` или `OnMeasure`. Именно здесь класс макета определяет свой собственный размер, который обычно основывается на размерах его дочерних элементов, полученных путем вызова `GetSizeRequest` или `Measure` для дочерних элементов. До и после вызова `OnSizeRequest` или `OnMeasure` `GetSizeRequest` или `Measure` вносит корректировки с учетом следующих свойств:

- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) с типом `double` влияет на свойство `Request` объекта `SizeRequest`;
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) с типом `double` влияет на свойство `Request` объекта `SizeRequest`;
- [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest) с типом `double` влияет на свойство `Minimum` объекта `SizeRequest`;
- [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest) с типом `double` влияет на свойство `Minimum` объекта `SizeRequest`.

### <a name="infinite-constraints"></a>Бесконечные ограничения

Аргументы ограничения, передаваемые в `GetSizeRequest` (или `Measure`) и `OnSizeRequest` (или `OnMeasure`), могут быть бесконечными значениями (т. е. значения `Double.PositiveInfinity`). Но значения `SizeRequest`, возвращаемые этими методами, не могут содержать бесконечные измерения.

Бесконечные ограничения означают, что запрошенный размер должен отражать естественный размер элемента. Вертикальный `StackLayout` вызывает `GetSizeRequest` (или `Measure`) для своих дочерних элементов, указывая бесконечное ограничение высоты. Горизонтальный макет стека вызывает `GetSizeRequest` (или `Measure`) для своих дочерних элементов, указывая бесконечное ограничение ширины. Объект `AbsoluteLayout` вызывает `GetSizeRequest` (или `Measure`) для своих дочерних элементов, указывая бесконечные ограничения ширины и высоты.

### <a name="peeking-inside-the-process"></a>Внутренний механизм процесса

Пример [**ExploreChildSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) отображает сведения о запросе ограничений и размера для простого макета.

## <a name="deriving-from-layoutview"></a>Наследование от Layout\<View>

Пользовательский класс макета является производным от класса `Layout<View>`. Он выполняет две задачи:

- Переопределяет `OnMeasure` для вызова `Measure` для всех дочерних элементов макета. Возвращает запрошенный размера для самого макета.
- Переопределяет `LayoutChildren` для вызова `Layout` для всех дочерних элементов макета.

Цикл `for` или `foreach` в этих переопределениях должен пропустить все дочерние элементы, у которых свойство `IsVisible` имеет значение `false`.

Вызов `OnMeasure` не гарантируется. `OnMeasure` не будет вызываться, если родительский элемент макета самостоятельно управляет размером макета (например, если макет заполняет всю страницу). По этой причине `LayoutChildren` не может полагаться на размеры дочерних элементов, полученные во время вызова `OnMeasure`. Очень часто приходится прямо из `LayoutChildren` вызывать `Measure` для дочерних элементов макета или создавать некоторую логику кэширования размеров (которая будет обсуждаться далее).

### <a name="an-easy-example"></a>Простой пример

Пример [**VerticalStackDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) содержит упрощенный класс [`VerticalStack`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) и демонстрацию его использования.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Упрощение вертикального и горизонтального размещения

Одна из задач `VerticalStack` выполняется при переопределении `LayoutChildren`. Этот метод использует свойство `HorizontalOptions` дочернего элемента, чтобы определить положение этого дочернего элемента в слоте в пределах `VerticalStack`. Вместо него вы можете вызвать статический метод [`Layout.LayoutChildIntoBoundingRect`](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)). Этот метод вызывает `Measure` для дочернего элемента и применяет его свойства `HorizontalOptions` и `VerticalOptions` для размещения этого дочернего элемента в пределах указанного прямоугольника.

### <a name="invalidation"></a>Недействительность

Зачастую изменение свойства элемента влияет на отображение этого элемента в макете. Макет в этом случае нужно объявить недействительным, чтобы активировать новый расчет макета.

`VisualElement` определяет защищенный метод [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure), который обычно вызывается обработчиком изменения любого привязываемого свойства, изменение которого влияет на размер элемента. Метод `InvalidateMeasure` вызывает событие [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated).

Класс `Layout` определяет аналогичный защищенный метод с именем [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout), который должен вызываться производным от `Layout` при любых изменениях, которое влияют на положение и размеры его дочерних элементов.

### <a name="some-rules-for-coding-layouts"></a>Некоторые правила для программирования макетов

1. Свойства, определенные для производных от `Layout<T>`, должны поддерживаться привязываемыми свойствами, а обработчики изменений свойств должны вызывать `InvalidateLayout`.

2. Производный от `Layout<T>` элемент, который определяет присоединенные привязываемые свойства, должен переопределять [`OnAdded`](xref:Xamarin.Forms.Layout`1.OnAdded*), чтобы добавлять обработчик изменения свойств для своих дочерних элементов, и [`OnRemoved`](xref:Xamarin.Forms.Layout`1.OnRemoved*), чтобы удалять этот обработчик. Этот обработчик должен проверять наличие изменений в этих присоединенных привязываемых свойствах и реагировать на них вызовом `InvalidateLayout`.

3. Производный от `Layout<T>` элемент, который реализует кэш размеров дочерних элементов, должен переопределять `InvalidateLayout` и [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated), чтобы очищать кэш при вызове этих методов.

### <a name="a-layout-with-properties"></a>Макет с поддержкой свойств

Класс [`WrapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) в [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) предполагает, что все его дочерние элементы имеют одинаковый размер и переносит дочерние элементы из одной строки (или столбца) в следующую. Он определяет свойство `Orientation` как `StackLayout`, свойства `ColumnSpacing` и `RowSpacing` как `Grid`, а также кэширует размеры дочерних элементов.

Пример [**PhotoWrap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) размещает `WrapLayout` в `ScrollView` для отображения фотографий из коллекции.

### <a name="no-unconstrained-dimensions-allowed"></a>Неограниченные измерения не допускаются!

Класс [`UniformGridLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) предназначен для отображения всех своих дочерних элементов внутри себя. Поэтому он не поддерживает неограниченные измерения и в соответствующих случаях вызывает исключение.

Пример [**PhotoGrid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) демонстрирует применение `UniformGridLayout`.

[![Три снимка экрана с элементом PhotoGrid](images/ch26fg08-small.png "Макет однородной сетки")](images/ch26fg08-large.png#lightbox "Макет однородной сетки")

### <a name="overlapping-children"></a>Перекрывающиеся дочерние элементы

Производный от `Layout<T>` элемент допускает перекрытие для дочерних элементов. Но при этом дочерние элементы отображаются том порядке, в котором они включены в коллекцию `Children`, а не в порядке вызова методов `Layout`.

Класс `Layout` определяет два метода, которые позволяют перемещать дочерний объект в пределах коллекции:

- [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View)) для переноса дочернего элемента в начало коллекции;
- [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View)) для переноса дочернего элемента в конец коллекции.

Если применяется перекрытие дочерних элементов, элементы в конце коллекции визуально отображаются поверх элементов в начале коллекции.

Класс [`OverlapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) определяет присоединенное свойство для указания порядка отрисовки, позволяя отображать один из дочерних элементов поверх остальных. Пример [**StudentCardFile**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) демонстрирует такой сценарий.

[![Снимок экрана с тремя изображениями сетки карточек учащихся](images/ch26fg10-small.png "Перекрывающиеся дочерние элементы в макете")](images/ch26fg10-large.png#lightbox "Перекрывающиеся дочерние элементы в макете")

### <a name="more-attached-bindable-properties"></a>Дополнительные присоединенные привязываемые свойства

Класс [`CartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) из [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) определяет присоединенные привязываемые свойства для указания двух значений `Point` и значения толщины, а также размещает элементы `BoxView` в виде линий.

Пример [**UnitCube**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) использует эту возможность для отрисовки трехмерного куба.

### <a name="layout-and-layoutto"></a>Макет и LayoutTo

Производный от `Layout<T>` элемент может вызывать `LayoutTo` вместо `Layout` для анимации макета. Например, класс [`AnimatedCartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) может так делать, и это представлено в примере [**AnimatedUnitCube**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube).

## <a name="related-links"></a>Связанные ссылки

- [Глава 26 — полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Глава 26 — примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Создание пользовательских макетов](~/xamarin-forms/user-interface/layouts/custom.md)
