---
title: 'Xamarin.FormsФигуры: прямоугольник'
description: Xamarin.FormsКласс Rectangle можно использовать для рисования прямоугольников.
ms.prod: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: da9649a4abb2cb65930d98576eda81739b711886
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101370"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.FormsФигуры: прямоугольник

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Rectangle`Класс является производным от `Shape` класса и может использоваться для рисования прямоугольников. Сведения о свойствах, которые `Rectangle` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Rectangle` определяет следующие свойства:

- `RadiusX`Тип — `double` радиус оси x, используемый для округления углов прямоугольника. Значение этого свойства по умолчанию равно 0,0.
- `RadiusY`Тип — `double` радиус оси y, используемый для округления углов прямоугольника. Значение этого свойства по умолчанию равно 0,0.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

`Rectangle`Класс задает `Aspect` свойству, унаследованному от `Shape` класса, значение `Stretch.Fill` .

## <a name="create-a-rectangle"></a>Создание прямоугольника

В следующем примере XAML показано, как нарисовать закрашенный прямоугольник со скругленными углами:

```xaml
<Rectangle Fill="DarkBlue"
           Stroke="Red"
           StrokeThickness="4"
           RadiusX="12"
           RadiusY="24"           
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.FormsМногоугольник](index.md)
