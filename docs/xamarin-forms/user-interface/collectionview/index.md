---
Title: « Xamarin.Forms CollectionView» Description: «CollectionView — это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета».
MS. произв. Xamarin MS. AssetID: 2BC9B223-2D5C-4B09-849C-B9D578954557 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 07/24/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

## <a name="introduction"></a>[Введение](introduction.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)— Это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета.

## <a name="data"></a>[Данные](populate-data.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)Заполняется данными путем установки его [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства в любую коллекцию, реализующую `IEnumerable` . Внешний вид каждого элемента в списке можно определить, задав [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) для свойства значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

## <a name="layout"></a>[Макет](layout.md)

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) элементы отображаются в вертикальном списке. Однако можно указать вертикальные и горизонтальные списки и сетки.

## <a name="selection"></a>[Выбор](selection.md)

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) выделение отключено. Однако можно включить один и несколько элементов.

## <a name="empty-views"></a>[Пустые представления](emptyview.md)

В [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.

## <a name="scrolling"></a>[Прокрутка](scrolling.md)

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, [`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет два [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) метода: программными средствами прокрутки элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление.

## <a name="grouping"></a>[Группирование](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)может отображать правильно сгруппированные данные, присвоив `IsGrouped` свойству значение `true` .
