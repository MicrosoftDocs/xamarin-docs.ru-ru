---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 4. Scrolling the stack''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9890ecdf286ee33d7af28fd20847788b063df8f5
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136894"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Сводная информация о Главе 4. Прокрутка стека

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

Эта глава посвящена в первую очередь такому понятию, как *макет*, которое обобщенно включает все классы и методики, используемые в Xamarin.Forms для организации визуального отображения нескольких представлений на странице.

Для создания макета нужно определить классы, производные от [`Layout`](xref:Xamarin.Forms.Layout) и [`Layout<T>`](xref:Xamarin.Forms.Layout`1). В этой главе рассматривается [`StackLayout`](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> [`FlexLayout`](~/xamarin-forms/user-interface/layouts/flex-layout.md) появился в Xamarin.Forms 3.0 и используется примерно так же, как `StackLayout`, но дает намного больше гибкости.

Также в этой главе вы познакомитесь с классами [`ScrollView`](xref:Xamarin.Forms.ScrollView), [`Frame`](xref:Xamarin.Forms.Frame) и [`BoxView`](xref:Xamarin.Forms.BoxView).

## <a name="stacks-of-views"></a>Стек представлений

[`StackLayout`](xref:Xamarin.Forms.StackLayout) является производным от `Layout<View>` и наследует свойство [`Children`](xref:Xamarin.Forms.Layout`1) от типа `IList<View>`. В эту коллекцию вы можете добавить несколько представлений, и тогда `StackLayout` отображает их в виде горизонтального или вертикального стека.

Присвойте свойству [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) объекта `StackLayout` значение одного из элементов перечисления [`StackOrientation`](xref:Xamarin.Forms.StackOrientation), то есть [`Vertical`](xref:Xamarin.Forms.StackOrientation.Vertical) или [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). Значение по умолчанию — `Vertical`.

Присвойте свойству [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) объекта `StackLayout` значение `double`, чтобы задать расстояние между дочерними элементами. По умолчанию используется значение 6.

Вы можете в коде добавлять элементы в коллекцию `Children` с `StackLayout`, используя цикл `for` или `foreach`, как показано в примере [**ColorLoop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop), или инициализировать коллекцию `Children` списком отдельных представлений, как показано в примере [**ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Дочерние элементы должны быть производными от `View`, но могут содержать другие объекты `StackLayout`.

## <a name="scrolling-content"></a>Прокрутка содержимого

Если `StackLayout` содержит слишком много дочерних элементов и они не помещаются на странице, поместите `StackLayout` в [`ScrollView`](xref:Xamarin.Forms.ScrollView), чтобы разрешить прокрутку.

Присвойте свойству [`Content`](xref:Xamarin.Forms.ScrollView.Content) объекта `ScrollView` значение представления, для которого вы хотите применить прокрутку. Обычно это `StackLayout`, но может быть и любое другое представление.

Установите свойству [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) объекта `ScrollView` значение одного из элементов свойства [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation), то есть [`Vertical`](xref:Xamarin.Forms.ScrollOrientation.Vertical), [`Horizontal`](xref:Xamarin.Forms.ScrollOrientation.Horizontal) или [`Both`](xref:Xamarin.Forms.ScrollOrientation.Both). Значение по умолчанию — `Vertical`. Если содержимое `ScrollView` является `StackLayout`, обе ориентации должны быть единообразными.

В примере [**ReflectedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) демонстрируется использование `ScrollView` и `StackLayout` для вывода доступных цветов. В этом примере также показано, как использовать отражение .NET для получения всех открытых статических свойств и полей структуры `Color`, не перечисляя их явным образом.

## <a name="the-expands-option"></a>Параметр Expand

Когда `StackLayout` размещает дочерние элементы в стеке, каждый из них занимает в пределах общей высоты `StackLayout` определенную область, которая зависит от размера дочернего элемента и значений его свойств `HorizontalOptions` и `VerticalOptions`. Этим свойствам присваиваются значения типа [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions).

Структура `LayoutOptions` определяет два свойства.

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) с типом перечисления [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) и четырьмя элементами: [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End) и [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) типа `bool`

Для удобства структура `LayoutOptions` также определяет восемь статических полей только для чтения с типом `LayoutOptions`, которые описывают все возможные сочетания двух свойств экземпляра:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

В дальнейшем обсуждении всегда предполагается `StackLayout` с вертикальной ориентацией по умолчанию. Для горизонтального `StackLayout` все работает так же.

Для вертикального `StackLayout` параметр `HorizontalOptions` определяет горизонтальное положение дочернего элемента в пределах ширины `StackLayout`. Если параметр `Alignment` имеет значение `Start`, `Center` или `End`, дочерний элемент не ограничивается в горизонтальном направлении. Дочерний элемент самостоятельно определяет свою ширину и располагается слева, по центру или справа от `StackLayout`. Значение `Fill` налагает ограничение на горизонтальный размер дочернего элемента и заполняет ширину, указанную в `StackLayout`.

Для вертикального `StackLayout` каждый дочерний элемент не имеет ограничений по вертикали, и размер выделяемой ему области определяется в зависимости от высоты дочернего элемента, а значит параметр `VerticalOptions` не применяется.

Если вертикальный `StackLayout` сам по себе неограничен, то есть его параметр `VerticalOptions` имеет значение `Start`, `Center` или `End`, то высота этого `StackLayout` определяется как сумма высот дочерних элементов.

Однако, если вертикальный `StackLayout` вертикально ограничен, то есть его параметр `VerticalOptions` имеет значение `Fill`, то высота `StackLayout` определяется высотой контейнера и может быть больше, чем общая высота его дочерних элементов. Если это так, и если по крайней мере один дочерний элемент имеет параметр `VerticalOptions` и флаг `Expands` со значением `true`, дополнительное пространство в `StackLayout` равномерно распределяется между всеми дочерними элементами, у которых флаг `Expands` имеет значение `true`. После этого общая высота дочернего элемента будет равна высоте `StackLayout`, а часть `Alignment` параметра `VerticalOptions` определяет, какую вертикальную позицию занимает дочерний элемент в своей области.

Это показано в примере [**VerticalOptionsDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo).

## <a name="frame-and-boxview"></a>Frame и BoxView

Эти два прямоугольных представления часто используются для наглядной презентации.

Представление [`Frame`](xref:Xamarin.Forms.Frame) отображает прямоугольную рамку вокруг другого представления, которая может быть макетом, например `StackLayout`. `Frame` наследует свойство [`Content`](xref:Xamarin.Forms.ContentView.Content) из [`ContentView`](xref:Xamarin.Forms.ContentView), который вы указываете для отображения представления в пределах `Frame`. По умолчанию `Frame` прозрачна. Задайте следующие три свойства, чтобы изменить внешний вид рамки.

- Свойство [`OutlineColor`](xref:Xamarin.Forms.Frame.OutlineColor) сделает ее видимой. Обычно для `OutlineColor` задают значение `Color.Accent`, если базовая цветовая схема не известна.
- Для свойства [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) можно задать значение `true`, чтобы на устройствах iOS отображалась черная тень.
- Для свойства [`Padding`](xref:Xamarin.Forms.Layout.Padding) задайте значение `Thickness`, чтобы оставить пробел между рамкой и содержимым. По умолчанию используется значение 20 единиц со всех сторон.

`Frame` имеет значения по умолчанию `HorizontalOptions` и `VerticalOptions` для `LayoutOptions.Fill`, то есть `Frame` полностью заполнит свой контейнер. При других значениях параметров размер `Frame` зависит от размера содержимого.

Применение `Frame` демонстрируется в примере [**FramedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText).

[`BoxView`](xref:Xamarin.Forms.BoxView) отображает прямоугольную область, цвет которой задается свойством [`Color`](xref:Xamarin.Forms.BoxView.Color).

Если объект `BoxView` ограничен (свойства `HorizontalOptions` и `VerticalOptions` имеют значения по умолчанию `LayoutOptions.Fill`), `BoxView` заполняет все доступное пространство. Если объект `BoxView` не ограничен (то есть `HorizontalOptions` и `LayoutOptions` имеют значения `Start`, `Center` или `End`), его размер по умолчанию устанавливается в 40 единиц. `BoxView` может быть ограниченным по одному измерению и неограниченным по другому.

Часто задают свойства [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) для `BoxView`, чтобы присвоить ему конкретный размер. Это продемонстрировано в примере [**SizedBoxView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView).

Вы можете использовать несколько экземпляров `StackLayout`, чтобы объединить `BoxView` с несколькими экземплярами `Label` в `Frame` и отобразить определенный цвет, а затем поместить каждое из этих представлений в `StackLayout` в `ScrollView`, чтобы создать симпатичный список цветов, как показано в примере [**ColorBlocks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks).

[![Снимок экрана с тремя изображениями цветных блоков](images/ch04fg11-small.png "Список цветов")](images/ch04fg11-large.png#lightbox "Список цветов")

## <a name="a-scrollview-in-a-stacklayout"></a>Размещение ScrollView в StackLayout

Обычно `StackLayout` размещают в `ScrollView`, но иногда бывает удобно разместить `ScrollView` в `StackLayout`. Теоретически это не должно быть возможным, так как дочерние элементы вертикального `StackLayout` не ограничены по вертикали. Но `ScrollView` должны быть ограничены по вертикали. Необходимо присвоить элементу определенную высоту, чтобы появилась возможность определить размер дочернего элемента для прокрутки.

Хитрость заключается в том, чтобы присвоить значение `FillAndExpand` параметру `VerticalOptions` в `ScrollView`, который является дочерним элементом `StackLayout`. Это показано в примере [**BlackCat**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat).

Пример **BlackCat** также демонстрирует, как определить и использовать ресурсы программы, внедренные в общую библиотеку. Такого же результата можно добиться с помощью проектов общих ресурсов (SAP), но этот процесс немного сложнее, как показано в примере [**BlackCatSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap).

## <a name="related-links"></a>Связанные ссылки

- [Глава 4, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Примеры для Главы 4](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Примеры F# для Главы 4](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
