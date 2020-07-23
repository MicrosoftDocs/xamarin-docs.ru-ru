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
ms.openlocfilehash: 49aa0ffcf7e7fa4d22024ee08f8bdf509cafd73a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937804"
---
# <a name="xamarinforms-carouselview-introduction"></a>Xamarin.FormsВведение в Карауселвиев

![API предварительного выпуска](~/media/shared/preview.png "Этот API-интерфейс сейчас доступен в предварительной версии.")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)представляет собой представление для представления данных в прокручиваемом макете, где пользователи могут перемещаться для перемещения по коллекции элементов. По умолчанию `CarouselView` элементы отображаются на горизонтальной ориентации. На экране будет отображаться один элемент с жестами прокрутки, приводящими к пересылке и обратной навигации по коллекции элементов. Кроме того, можно отображать индикаторы, представляющие каждый элемент в `CarouselView` :

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](populate-data-images/indicators.png "Индикаторвиев круги")](populate-data-images/indicators-large.png#lightbox "Индикаторвиев круги")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)доступна в Xamarin.Forms 4,3. Однако в настоящее время он экспериментальен и может использоваться только путем добавления следующей строки кода в `AppDelegate` класс в iOS или в ваш `MainActivity` класс в Android перед вызовом `Forms.Init` :

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)доступна в iOS и Android, но некоторые функции могут быть частично доступны только на универсальная платформа Windows.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)использует большую часть своей реализации с [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Однако эти два элемента управления имеют разные варианты использования. `CollectionView`обычно используется для представления списков данных любой длины, в то время `CarouselView` как обычно используется для выделения информации в списке ограниченной длины.
