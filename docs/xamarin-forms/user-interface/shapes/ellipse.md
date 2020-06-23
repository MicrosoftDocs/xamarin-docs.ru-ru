---
title: 'Xamarin.FormsФигуры: эллипс'
description: Xamarin.FormsДля рисования эллипсов и кругов можно использовать класс Ellipse.
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 053805c14f195ef0dd3ae8f0cfcac2ee7425271d
ms.sourcegitcommit: dc49ba58510eeb52048a866e5d3daf5f1f68fbd2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2020
ms.locfileid: "85130919"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.FormsФигуры: эллипс

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Ellipse`Класс является производным от `Shape` класса и может использоваться для рисования эллипсов и кругов. Сведения о свойствах, которые `Ellipse` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Ellipse`Класс задает `Aspect` свойству, унаследованному от `Shape` класса, значение `Stretch.Fill` . Дополнительные сведения о `Aspect` свойстве см. в разделе [растяжение фигур](index.md#stretch-shapes).

## <a name="create-an-ellipse"></a>Создание эллипса

Чтобы нарисовать эллипс, создайте `Ellipse` объект и задайте его `WidthRequest` `HeightRequest` Свойства и. Используйте его `Fill` свойство, чтобы указать [`Color`](xref:Xamarin.Forms.Color) , используемое для заполнения внутренней части эллипса. Используйте его `Stroke` свойство, чтобы указать объект `Color` , используемый для рисования контура эллипса. `StrokeThickness`Свойство задает толщину контура эллипса.

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

![Круглая](ellipse-images/circle.png "Circle")

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.FormsМногоугольник](index.md)
