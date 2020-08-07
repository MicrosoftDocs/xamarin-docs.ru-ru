---
title: Xamarin.FormsНаборы
description: Xamarin.FormsКласс Brush является абстрактным классом, который закрашивает область с выходными данными.
ms.prod: xamarin
ms.assetid: 44420FC2-304C-4175-8654-76769F79A813
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f1f56103b20eac84ce6106c0955acebf974cfe3
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919147"
---
# <a name="no-locxamarinforms-brushes"></a>Xamarin.FormsНаборы

![Предварительный просмотр API](~/media/shared/preview.png "Этот API-интерфейс сейчас доступен в предварительной версии.")

Кисть позволяет закрасить область, например фон элемента управления, используя различные подходы. Поддержка кисти в доступна Xamarin.Forms в `Xamarin.Forms` пространстве имен в iOS, Android, macOS, универсальная платформа Windows (UWP) и Windows Presentation Foundation (WPF).

> [!IMPORTANT]
> Поддержка кисти в в Xamarin.Forms настоящее время экспериментальна и может использоваться только путем установки `Brush_Experimental` флага. Дополнительные сведения см. в разделе [экспериментальные флаги](~/xamarin-forms/internals/experimental-flags.md).

`Brush`Класс является абстрактным классом, который закрашивает область с выходными данными. Классы, производные от, `Brush` описывают различные способы рисования области. В следующем списке описываются различные типы кистей, доступные в Xamarin.Forms .

- `SolidColorBrush`, который закрашивает область сплошным цветом. Дополнительные сведения см. в разделе [ Xamarin.Forms кисти: сплошные цвета](solidcolor.md).
- `LinearGradientBrush`, который закрашивает область линейным градиентом. Дополнительные сведения см. в разделе [ Xamarin.Forms кисти: линейные градиенты](lineargradient.md).
- `RadialGradientBrush`, который закрашивает область с радиальным градиентом. Дополнительные сведения см. в разделе [ Xamarin.Forms кисти: радиальные градиенты](radialgradient.md).

Экземпляры этих типов кистей могут быть назначены `Stroke` `Fill` свойствам и свойствам `Shape` , а также `Background` свойству объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

> [!NOTE]
> `VisualElement.Background`Свойство позволяет использовать кисти в качестве фона для любого элемента управления.

`Brush`Класс также содержит `IsNullOrEmpty` метод, который возвращает объект `bool` , который представляет, определена ли кисть.
