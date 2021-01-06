---
title: Xamarin.Forms карауселвиев
description: Карауселвиев — это представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов.
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 958a7c292ba636368a016894e98fe8aaff0d0f60
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940568"
---
# <a name="no-locxamarinforms-carouselview"></a>Xamarin.Forms карауселвиев

## <a name="introduction"></a>[Введение](introduction.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Представляет собой представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов.

## <a name="data"></a>[Data](populate-data.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Заполняется данными путем установки его [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства в любую коллекцию, реализующую `IEnumerable` . Внешний вид каждого элемента можно определить, задав [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) для свойства значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

## <a name="layout"></a>[Макет](layout.md)

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) элементы отображаются в горизонтальном списке. Однако он также имеет доступ к тем же макетам, что и CollectionView, включая вертикальную ориентацию.

## <a name="interaction"></a>[Взаимодействие](interaction.md)

Доступ к отображаемому в данный момент элементу в [`CarouselView`](xref:Xamarin.Forms.CarouselView) можно получить с помощью `CurrentItem` `Position` свойств и.

## <a name="empty-views"></a>[Пустые представления](emptyview.md)

В [`CarouselView`](xref:Xamarin.Forms.CarouselView) можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.

## <a name="scrolling"></a>[Прокрутка](scrolling.md)

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, [`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет два [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) метода: программными средствами прокрутки элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление.
