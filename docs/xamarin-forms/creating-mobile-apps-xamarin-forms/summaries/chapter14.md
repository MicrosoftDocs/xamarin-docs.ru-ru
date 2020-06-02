---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 14. Absolute layout''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 72ee9c4a481388e69aeeb52dbd5b8eeaabb164f6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136764"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Сводная информация о Главе 14. Абсолютный макет

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)

Как и `StackLayout`, [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) является производным от `Layout<View>` и наследует свойство `Children`. `AbsoluteLayout` реализует систему макета, в которой программисту нужно указывать позиции дочерних элементов и их размер (необязательно). Эти позиции определяются по координатам левого верхнего угла дочернего элемента относительно левого верхнего угла `AbsoluteLayout`, выраженным в не зависящих от устройства единицах измерения. `AbsoluteLayout` также реализует функции пропорционального размещения и изменения размера.

`AbsoluteLayout` следует рассматривать как специализированную систему макета, которая используется только в том случае, когда программисту важно строго определять размер дочерних элементов (например, элементов `BoxView`) или когда размер элемента не влияет на расположение других дочерних элементов. Свойства `HorizontalOptions` и `VerticalOptions` не влияют на дочерние элементы `AbsoluteLayout`.

В этой главе также представлена важная концепция *присоединенных привязываемых свойств*, которые позволяют присоединять свойства, определенные в одном классе (в нашем примере это `AbsoluteLayout`) к другому классу (дочернему элементу `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>Код для AbsoluteLayout

Вы можете добавить дочерний элемент в коллекцию `Children` элемента `AbsoluteLayout` с помощью стандартного метода [`Add`](xref:System.Collections.Generic.ICollection`1.Add*), но `AbsoluteLayout` предоставляет еще и расширенный метод [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*), который позволяет указать [`Rectangle`](xref:Xamarin.Forms.Rectangle). Еще один метод [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*) требует только [`Point`](xref:Xamarin.Forms.Point), то есть дочерний элемент неограничен и самостоятельно задает свои размеры.

Вы можете создать значение `Rectangle` с помощью [конструктора](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double)), который требует четыре значения &mdash;, из которых первые два указывают положение верхнего левого угла дочернего элемента относительно его родителя, а вторые два — размер этого дочернего элемента. Также можно использовать [конструктор](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size)), который принимает `Point` и значение [`Size`](xref:Xamarin.Forms.Size).

Эти методы `Add` демонстрируются в примере [**AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), который позиционирует элементы `BoxView` с помощью значений `Rectangle` и элемент `Label` по одному значению `Point`.

В примере [**ChessboardFixed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) для создания шаблона шахматной доски используются 32 элемента `BoxView`. Программа присваивает элементам `BoxView` жестко запрограммированный размер обеих сторон 35 единиц. Для `AbsoluteLayout` параметрам `HorizontalOptions` и `VerticalOptions` присвоено значение `LayoutOptions.Center`, в результате чего общий размер `AbsoluteLayout` соответствует квадрату со стороной 280 единиц.

## <a name="attached-bindable-properties"></a>Присоединенные привязываемые свойства

Вы можете также задать позицию и (необязательно) размер дочернего элемента `AbsoluteLayout` после добавления его в коллекцию `Children`, используя статический метод [`AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle)). Первым аргументом он принимает дочерний элемент, а вторым — объект `Rectangle`. Вы можете указать, что дочерний элемент самостоятельно задает свой размер по горизонтали и (или) вертикали, присвоив значение константы [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) параметрам ширины и высоты.

Пример [**ChessboardDynamic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) размещает `AbsoluteLayout` в `ContentView` с обработчиком `SizeChanged` для вызова `AbsoluteLayout.SetLayoutBounds` из всех дочерних элементов, чтобы максимально увеличить их размер.  

Присоединенное привязываемое свойство, определенное в `AbsoluteLayout`, является статическим полем только для чтения с типом `BindableProperty` и именем [`AbsoluteLayout.LayoutBoundsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty). Статический метод `AbsoluteLayout.SetLayoutBounds` реализуется путем вызова `SetValue` из дочернего элемента с помощью `AbsoluteLayout.LayoutBoundsProperty`. Этот дочерний элемент содержит словарь, в котором хранятся присоединенное привязываемой свойство и его значение. На этапе создания макета `AbsoluteLayout` может получить это значение, вызвав [`AbsoluteLayout.GetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject)), который реализуется вызовом `GetValue`.

## <a name="proportional-sizing-and-positioning"></a>Пропорциональный размер и размещение

`AbsoluteLayout` реализует функцию пропорционального размера и размещения. Этот класс определяет второе присоединенное привязываемой свойство [`LayoutFlagsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) с соответствующими статическими методами [`AbsoluteLayout.SetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags)) и [`AbsoluteLayout.GetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject)).

Аргумент `AbsoluteLayout.SetLayoutFlags` и возвращаемое значение `AbsoluteLayout.GetLayoutFlags` имеют тип [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags), то есть перечисление со следующими элементами:

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None) (равно значению 0);
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1);
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2);
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3);
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4);
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8);
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12);
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF).

Их можно объединять с помощью оператора "побитового ИЛИ" из C#.

При установке этих флагов некоторые свойства структуры связывания макета `Rectangle`, которые применяются для выбора положения и размера дочернего элемента, рассматриваются как пропорциональные.

Если задан флаг `WidthProportional`, значение 1 для параметра `Width` означает, что ширина дочернего элемента совпадает с шириной `AbsoluteLayout`. Такой же подход используется для высоты.

При пропорциональном размещении учитывается размер элементов. Если задан флаг `XProportional`, свойство `X` структуры макета `Rectangle` вычисляется как пропорциональное. Значение 0 означает, что левый край дочернего элемента размещается по левому краю `AbsoluteLayout`, но положение 1 означает, что правый край дочернего элемента размещается по правому краю `AbsoluteLayout`, а не за правым краем `AbsoluteLayout`, как можно было бы ожидать. Значение 0,5 для свойства `X` размещает дочерний элемент в центре `AbsoluteLayout` по горизонтальной оси.

Пример [**ChessboardProportional**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) демонстрирует применение функции пропорционального размера и размещения.

## <a name="working-with-proportional-coordinates"></a>Использование пропорциональных координат

Иногда бывает удобно использовать пропорциональное размещение не так, как оно реализовано в `AbsoluteLayout`. Вы можете предпочесть пропорциональные координаты, при использовании которых значение 1 для свойства `X` размещает левый (а не правый) край дочернего элемента по правому краю `AbsoluteLayout`.

Эту альтернативную схему размещения можно назвать "дробные координаты дочернего элемента". Пересчет между дробными координатами дочернего элемента и структурой макета, которую использует `AbsoluteLayout`, можно выполнить по следующим формулам:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

Этот подход демонстрируется в примере [**ProportionalCoordinateCalc**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc).

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout и XAML

Вы можете применить `AbsoluteLayout` в XAML и настроить присоединенные привязываемые свойства дочерних элементов `AbsoluteLayout` с помощью значений атрибутов `AbsoluteLayout.LayoutBounds` и `AbsoluteLayout.LayoutFlags`. Этот подход применяется в примерах [**AbsoluteXamlDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) и [**ChessboardXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml). Вторая из этих программ содержит 32 элемента `BoxView`, но с помощью неявного `Style` со свойством `AbsoluteLayout.LayoutFlags` обходится минимальным количеством разметки.

В XAML атрибут, состоящий из имени класса, символа точки и имени свойства *всегда* является присоединенным привязываемым свойством.

## <a name="overlays"></a>Наложения

Вы можете с помощью `AbsoluteLayout` создать *наложение*, которое перекрывает страницу другими элементами управления, например для защиты от вмешательства пользователя в работу основных элементов управления на этой страницы.

Пример [**SimpleOverlay**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) демонстрирует эту методику и [`ProgressBar`](xref:Xamarin.Forms.ProgressBar), с помощью которой обозначает ход выполнения задачи в программе.

## <a name="some-fun"></a>Немного развлечений

Пример [**DotMatrixClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) отображает текущее время на модели матричного дисплея размером 5x7 точек. Каждая точка представляет собой `BoxView` (в общей сложности их 228), которому присваивается размер и положение на `AbsoluteLayout`.

[![Снимок экрана с тремя изображениями цветных матричных часов](images/ch14fg08-small.png "Часы с матрицей из точек")](images/ch14fg08-large.png#lightbox "Часы с матрицей из точек")

Программа [**BouncingText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) перемещает по экрану два объекта `Label`, моделируя отражение от горизонтальных и вертикальных границ экрана.

## <a name="related-links"></a>Связанные ссылки

- [Глава 14, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Примеры для Главы 14](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
- [Вложенные свойства](~/xamarin-forms/xaml/attached-properties.md)
