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
ms.openlocfilehash: a78536463b4966c38025d5c46a33aa07cb81bfdf
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918386"
---
# <a name="no-locxamarinforms-carouselview-introduction"></a>Xamarin.FormsВведение в Карауселвиев

![Предварительный выпуск API](~/media/shared/preview.png)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)представляет собой представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов. По умолчанию `CarouselView` элементы отображаются на горизонтальной ориентации. На экране будет отображаться один элемент с жестами прокрутки, приводящими к пересылке и обратной навигации по коллекции элементов. Кроме того, можно отображать индикаторы, представляющие каждый элемент в `CarouselView` :

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](populate-data-images/indicators.png "Индикаторвиев круги")](populate-data-images/indicators-large.png#lightbox "Индикаторвиев круги")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)доступна в Xamarin.Forms 4,3. Однако в настоящее время он экспериментальен и может использоваться только путем добавления следующей строки кода в `AppDelegate` класс в iOS или в ваш `MainActivity` класс в Android перед вызовом `Forms.Init` :

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)доступна в iOS и Android, но некоторые функции могут быть частично доступны только на универсальная платформа Windows.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)использует большую часть своей реализации с [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Однако эти два элемента управления имеют разные варианты использования. `CollectionView`обычно используется для представления списков данных любой длины, в то время `CarouselView` как обычно используется для выделения информации в списке ограниченной длины.
