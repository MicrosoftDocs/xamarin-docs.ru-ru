---
title: Xamarin.Forms Введение в Карауселвиев
description: Карауселвиев — это представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2aa33b0bd2e11d854f4f4dcfe03258a621301395
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97939030"
---
# <a name="no-locxamarinforms-carouselview-introduction"></a>Xamarin.Forms Введение в Карауселвиев

[`CarouselView`](xref:Xamarin.Forms.CarouselView) представляет собой представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов. По умолчанию `CarouselView` элементы отображаются на горизонтальной ориентации. На экране будет отображаться один элемент с жестами прокрутки, приводящими к пересылке и обратной навигации по коллекции элементов. Кроме того, можно отображать индикаторы, представляющие каждый элемент в `CarouselView` :

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](populate-data-images/indicators.png "Индикаторвиев круги")](populate-data-images/indicators-large.png#lightbox "Индикаторвиев круги")

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) обеспечивает циклический доступ к коллекции элементов. Таким образом, прокрутка назад от первого элемента в коллекции приведет к отображению последнего элемента в коллекции. Аналогичным образом, прокрутка вперед от последнего элемента в коллекции вернется к первому элементу в коллекции.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) использует большую часть своей реализации с [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Однако эти два элемента управления имеют разные варианты использования. `CollectionView` обычно используется для представления списков данных любой длины, в то время `CarouselView` как обычно используется для выделения информации в списке ограниченной длины.
