---
title: 'Xamarin.FormsФигуры: эллипс'
description: Xamarin.FormsДля рисования эллипсов и кругов можно использовать класс Ellipse.
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 142deccbcd29548e2d72b7144a01322f9909d98f
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101274"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.FormsФигуры: эллипс

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Ellipse`Класс является производным от `Shape` класса и может использоваться для рисования эллипсов и кругов. Сведения о свойствах, которые `Ellipse` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Ellipse`Класс задает `Aspect` свойству, унаследованному от `Shape` класса, значение `Stretch.Fill` .

## <a name="create-an-ellipse"></a>Создание эллипса

В следующем примере XAML показано, как нарисовать закрашенный эллипс:

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

В следующем примере XAML показано, как нарисовать круг:

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.FormsМногоугольник](index.md)
