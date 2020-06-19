---
title: Xamarin.Forms CollectionView
description: CollectionView — это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a2c9fd9e6e48192bc2237d6b451b533fcee6e6ed
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136452"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

## <a name="introduction"></a>[Введение](introduction.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)— Это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета.

## <a name="data"></a>[Data](populate-data.md)

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
