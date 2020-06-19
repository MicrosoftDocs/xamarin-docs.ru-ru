---
title: Xamarin.FormsВведение в Карауселвиев
description: Карауселвиев — это представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2e67acd0188e1147481005502ad9ccdaada645d9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140274"
---
# <a name="xamarinforms-carouselview-introduction"></a>Xamarin.FormsВведение в Карауселвиев

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)представляет собой представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов. По умолчанию `CarouselView` элементы отображаются на горизонтальной ориентации. На экране будет отображаться один элемент с жестами прокрутки, приводящими к пересылке и обратной навигации по коллекции элементов. Кроме того, можно отображать индикаторы, представляющие каждый элемент в `CarouselView` :

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](populate-data-images/indicators.png "Индикаторвиев круги")](populate-data-images/indicators-large.png#lightbox "Индикаторвиев круги")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)доступна в Xamarin.Forms 4,3. Однако в настоящее время он экспериментальен и может использоваться только путем добавления следующей строки кода в `AppDelegate` класс в iOS или в ваш `MainActivity` класс в Android перед вызовом `Forms.Init` :

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)доступна в iOS и Android, но некоторые функции могут быть частично доступны только на универсальная платформа Windows.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)использует большую часть своей реализации с [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Однако эти два элемента управления имеют разные варианты использования. `CollectionView`обычно используется для представления списков данных любой длины, в то время `CarouselView` как обычно используется для выделения информации в списке ограниченной длины.
