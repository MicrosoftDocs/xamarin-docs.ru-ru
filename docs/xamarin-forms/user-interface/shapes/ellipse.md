---
title: 'Xamarin.Forms Фигуры: эллипс'
description: Xamarin.FormsДля рисования эллипсов и кругов можно использовать класс Ellipse.
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/05/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b3130fd4cb054799ca9e0a61d13cacb2629320b7
ms.sourcegitcommit: 06701714021545eb5e932847829b876082194ffc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "99585836"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.Forms Фигуры: эллипс

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Ellipse`Класс является производным от `Shape` класса и может использоваться для рисования эллипсов и кругов. Сведения о свойствах, которые `Ellipse` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Ellipse`Класс задает `Aspect` свойству, унаследованному от `Shape` класса, значение `Stretch.Fill` . Дополнительные сведения о `Aspect` свойстве см. в разделе [растяжение фигур](index.md#stretch-shapes).

## <a name="create-an-ellipse"></a>Создание эллипса

Чтобы нарисовать эллипс, создайте `Ellipse` объект и задайте его `WidthRequest` `HeightRequest` Свойства и. Чтобы закрасить внутри эллипса, присвойте его `Fill` свойству [`Brush`](xref:Xamarin.Forms.Brush) производный объект. Чтобы присвоить контуру эллипс, присвойте его `Stroke` свойству [`Brush`](xref:Xamarin.Forms.Brush) производный объект. `StrokeThickness`Свойство задает толщину контура эллипса. Дополнительные сведения об `Brush` объектах см. в разделе [ Xamarin.Forms кисти](~/xamarin-forms/user-interface/brushes/index.md).

Чтобы нарисовать круг, сделайте `WidthRequest` Свойства и `HeightRequest` `Ellipse` объекта равными.

В следующем примере XAML показано, как нарисовать закрашенный эллипс:

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

В этом примере рисуется красный эллипс с 150x50 измерениями (аппаратно-независимые единицы):

![Закрашенный эллипс](ellipse-images/filled.png "Закрашенный эллипс")

В следующем примере XAML показано, как нарисовать круг:

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

В этом примере рисуется красный круг с 150x150 измерениями (единицы измерения, независимые от устройства):

![Незаполненный круг](ellipse-images/circle.png "Circle")

Сведения о рисовании штрихового эллипса см. в разделе [Рисование пунктирных фигур](index.md#draw-dashed-shapes).

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Многоугольник](index.md)
- [Кисти Xamarin.Forms](~/xamarin-forms/user-interface/brushes/index.md)
