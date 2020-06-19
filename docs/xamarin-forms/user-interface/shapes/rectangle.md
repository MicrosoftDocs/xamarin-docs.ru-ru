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
ms.openlocfilehash: a0d528948ae8a57fc7c702d8a4ce3d3ec2eda15a
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990833"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.FormsФигуры: прямоугольник

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

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

- [Шапедемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsМногоугольник](index.md)
