---
title: Xamarin.Forms Наборы
description: Xamarin.FormsКласс Brush является абстрактным классом, который закрашивает область с выходными данными.
ms.prod: xamarin
ms.assetid: 44420FC2-304C-4175-8654-76769F79A813
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e0bb8b5e1668e683fa8b15f752b77d865cb317b5
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940399"
---
# <a name="no-locxamarinforms-brushes"></a>Xamarin.Forms Наборы

Кисть позволяет закрасить область, например фон элемента управления, используя различные подходы. Поддержка кисти в доступна Xamarin.Forms в `Xamarin.Forms` пространстве имен в iOS, Android, macOS, универсальная платформа Windows (UWP) и Windows Presentation Foundation (WPF).

`Brush`Класс является абстрактным классом, который закрашивает область с выходными данными. Классы, производные от, `Brush` описывают различные способы рисования области. В следующем списке описываются различные типы кистей, доступные в Xamarin.Forms .

- `SolidColorBrush`, который закрашивает область сплошным цветом. Дополнительные сведения см. в разделе [ Xamarin.Forms кисти: сплошные цвета](solidcolor.md).
- `LinearGradientBrush`, который закрашивает область линейным градиентом. Дополнительные сведения см. в разделе [ Xamarin.Forms кисти: линейные градиенты](lineargradient.md).
- `RadialGradientBrush`, который закрашивает область с радиальным градиентом. Дополнительные сведения см. в разделе [ Xamarin.Forms кисти: радиальные градиенты](radialgradient.md).

Экземпляры этих типов кистей могут быть назначены `Stroke` `Fill` свойствам и свойствам `Shape` , а также `Background` свойству объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

> [!NOTE]
> `VisualElement.Background`Свойство позволяет использовать кисти в качестве фона для любого элемента управления.

`Brush`Класс также содержит `IsNullOrEmpty` метод, который возвращает объект `bool` , который представляет, определена ли кисть.
