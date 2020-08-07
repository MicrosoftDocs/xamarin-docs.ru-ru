---
title: Xamarin.Formsкарауселвиев
description: Карауселвиев — это представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов.
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2ff88431869c4a14919fb13e5059cd11fcdbdc3c
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918645"
---
# <a name="no-locxamarinforms-carouselview"></a>Xamarin.Formsкарауселвиев

![Предварительный выпуск API](~/media/shared/preview.png)

## <a name="introduction"></a>[Введение](introduction.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Представляет собой представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов.

## <a name="data"></a>[Данные](populate-data.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Заполняется данными путем установки его [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) свойства в любую коллекцию, реализующую `IEnumerable` . Внешний вид каждого элемента можно определить, задав [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) для свойства значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

## <a name="layout"></a>[Макет](layout.md)

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) элементы отображаются в горизонтальном списке. Однако он также имеет доступ к тем же макетам, что и CollectionView, включая вертикальную ориентацию.

## <a name="interaction"></a>[Взаимодействие](interaction.md)

Доступ к отображаемому в данный момент элементу в [`CarouselView`](xref:Xamarin.Forms.CarouselView) можно получить с помощью `CurrentItem` `Position` свойств и.

## <a name="empty-views"></a>[Пустые представления](emptyview.md)

В [`CarouselView`](xref:Xamarin.Forms.CarouselView) можно указать пустое представление, которое предоставляет пользователю отзыв о том, что данные недоступны для отображения. Пустое представление может быть строкой, представлением или несколькими представлениями.

## <a name="scrolling"></a>[Прокрутка](scrolling.md)

Когда пользователь начинает прокручивать, можно управлять конечной позицией прокрутки, чтобы элементы отображались полностью. Кроме того, [`CarouselView`](xref:Xamarin.Forms.CarouselView) определяет два [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) метода: программными средствами прокрутки элементов в представление. Одна из перегрузок прокручивается элемент по указанному индексу в представление, а другой Прокручивает указанный элемент в представление.
