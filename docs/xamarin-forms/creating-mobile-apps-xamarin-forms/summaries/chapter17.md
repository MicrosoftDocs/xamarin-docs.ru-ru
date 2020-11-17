---
title: Сводка по главе 17. Достигаем мастерства в использовании класса Grid
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводка по главе 17. Достигаем мастерства в использовании класса Grid
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d2904270b601b6602457873e8b0180c13a0d6d95
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373462"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Сводка по главе 17. Достигаем мастерства в использовании класса Grid

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)

> [!NOTE]
> Эта книга была опубликована весной 2016 года и с тех пор не обновлялась. Многое в этой книге остается ценным, но некоторые материалы устарели, а некоторые разделы перестали быть полностью верными или полными.

[`Grid`](xref:Xamarin.Forms.Grid) — мощный механизм макета, который позволяет упорядочить дочерние элементы по строкам и столбцам ячеек. В отличие от аналогичного HTML-элемента `table`, `Grid` используется исключительно для макета, а не для представления.

## <a name="the-basic-grid"></a>Базовое использование Grid

Класс `Grid` является производным от класса [`Layout<View>`](xref:Xamarin.Forms.Layout`1), определяющего свойство [`Children`](xref:Xamarin.Forms.Layout`1.Children), которое наследует `Grid`. Эту коллекцию можно заполнить в XAML или в коде.

### <a name="the-grid-in-xaml"></a>Grid в XAML

Определение `Grid` в XAML обычно начинается с заполнения коллекций [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) и [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) класса `Grid` объектами [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) и [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition). Таким способом вы устанавливаете количество строк и столбцов `Grid`, а также их свойства.

`RowDefinition` имеет свойство [`Height`](xref:Xamarin.Forms.RowDefinition.Height), а `ColumnDefinition` имеет свойство [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width). Тип обоих — [`GridLength`](xref:Xamarin.Forms.GridLength), структура.

В XAML [`GridLengthTypeConverter`](xref:Xamarin.Forms.GridLengthTypeConverter) преобразует простые текстовые строки в значения `GridLength`. В фоновом режиме [`GridLength` конструктор](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType)) создает значение `GridLength` на основе числа и значения типа [`GridUnitType`](xref:Xamarin.Forms.GridUnitType), перечисления с тремя членами:

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; — ширина или высота задаются в единицах, не зависящих от устройства (число в XAML).
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; — высота или ширина автоматически определяется на основе содержимого ячейки ("Auto" в XAML).
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; — остаточная высота или ширина выделяется пропорционально (в XAML число с "\*", которое называется *star*).

Каждому дочернему элементу `Grid` также необходимо назначить строку и столбец (явно или неявно). Диапазоны строк и столбцов являются необязательными. Все они задаются с помощью присоединенных свойств с возможностью привязки &mdash; свойств, определяемых `Grid` но устанавливаемых для дочерних элементов `Grid`. `Grid` определяет четыре статических присоединенных свойства с возможностью привязки:

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; — строка, начинающаяся с нуля (значение по умолчанию — 0).
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; — столбец, начинающийся с нуля (значение по умолчанию — 0).
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; — количество строк, охватываемых дочерним элементом (значение по умолчанию — 1).
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; — количество столбцов, охватываемых дочерним элементом (значение по умолчанию — 1).

В коде программа может использовать восемь статических методов для установки и получения этих значений:

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) и [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) и [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) и [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) и [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

В XAML для установки этих значений используются следующие атрибуты:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

В примере [**SimpleGridDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) демонстрируется создание и инициализация `Grid` в XAML.

`Grid` наследует свойство [`Padding`](xref:Xamarin.Forms.Layout.Padding) от `Layout` и определяет два дополнительных свойства, которые обеспечивают интервал между строками и столбцами:

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) — по умолчанию имеет значение 6.
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) — по умолчанию имеет значение 6.

Коллекции `RowDefinitions` и `ColumnDefinitions` не являются строго обязательными. Если их нет, `Grid` создает строки и столбцы для дочерних элементов `Grid` и предоставляет им все значения `GridLength` по умолчанию для "\*" (star).

### <a name="the-grid-in-code"></a>Grid в коде

В примере [**GridCodeDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) показано, как создать и заполнить `Grid` в коде. Присоединенные свойства для каждого дочернего элемента можно задать напрямую или опосредованно, вызвав дополнительные методы `Add`, такие как [`Add`](xref:Xamarin.Forms.Grid.IGridList`1.Add*), определенные интерфейсом [Grid.IGridList<T>](xref:Xamarin.Forms.Grid.IGridList`1).

### <a name="the-grid-bar-chart"></a>Линейчатая диаграмма Grid

В примере [**GridBarChart**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) показано, как добавить несколько элементов `BoxView` в `Grid` с помощью метода массовой операции [`AddHorizontal`](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*). По умолчанию эти элементы `BoxView` имеют одинаковую ширину. После этого можно управлять высотой каждого элемента `BoxView`, чтобы воспроизвести линейчатую диаграмму.

`Grid` в примере **GridBarChart** совместно использует родительский элемент `AbsoluteLayout` с изначально невидимым `Frame`. Кроме того, программа задает `TapGestureRecognizer` в каждом элементе `BoxView`, чтобы с помощью `Frame` выводились сведения о том, что панели коснулись.

### <a name="alignment-in-the-grid"></a>Выравнивание в Grid

В примере [**GridAlignment**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) показано, как с помощью свойств `VerticalOptions` и `HorizontalOptions` выровнять дочерние элементы в ячейке `Grid`.

В примере [**SpacingButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) между элементами `Button`, центрированными в ячейках `Grid`, устанавливаются одинаковые пробелы.

### <a name="cell-dividers-and-borders"></a>Разделители и границы ячеек

`Grid` не включает функцию, которая отвечает за отображение разделителей или границ ячеек. Но вы можете сделать свою собственную.

[**GridCellDividers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) демонстрирует, как определить дополнительные строки и столбцы специально для тонких элементов `BoxView`, чтобы имитировать разделительные линии.

Программа [**GridCellBorders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) не создает никаких дополнительных ячеек. Вместо этого она позволяет выровнять элементы `BoxView` в каждой ячейке, чтобы имитировать ее границу.

## <a name="almost-real-life-grid-examples"></a>Примеры Grid практически в реальном времени

В примере [**KeypadGrid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) с помощью `Grid` отображается клавиатура:

[![Три снимка экрана с элементом KeypadGrid](images/ch17fg12-small.png "Grid для клавиатуры")](images/ch17fg12-large.png#lightbox "Grid для клавиатуры")

### <a name="responding-to-orientation-changes"></a>Реагирование на изменения ориентации

`Grid` может помочь структурировать программу для реагирования на изменения ориентации. Пример [**GridRgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) демонстрирует прием, который позволяет переместить элемент между второй строкой телефона в портретной ориентации и вторым столбцом телефона в альбомной ориентации.

Программа инициализирует элементы `Slider` в диапазоне от 0 до 255 и с помощью привязок данных выводит значения ползунков в шестнадцатеричном виде. Значения `Slider` являются числами с плавающей запятой, а строка форматирования .NET для шестнадцатеричного формата применяется только с целыми числами. В этой ситуации поможет класс [`DoubleToIntConvert`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) из библиотеке [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="related-links"></a>Связанные ссылки

- [Полный текст главы 17 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Примеры для главы 17](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Сетка](~/xamarin-forms/user-interface/layouts/grid.md)
