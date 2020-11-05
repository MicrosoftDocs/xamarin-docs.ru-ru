---
title: 'Xamarin.Forms Фигуры: прямоугольник'
description: Xamarin.FormsКласс Rectangle можно использовать для рисования прямоугольников.
ms.prod: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7b5f477d4ec708071095ec666e34ef6d88ac0f0b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375048"
---
# <a name="no-locxamarinforms-shapes-rectangle"></a>Xamarin.Forms Фигуры: прямоугольник

![Предварительный выпуск API](~/media/shared/preview.png)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Rectangle`Класс является производным от `Shape` класса и может использоваться для рисования прямоугольников и квадратов. Сведения о свойствах, которые `Rectangle` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Rectangle` определяет следующие свойства:

- `RadiusX`Тип — `double` радиус оси x, используемый для округления углов прямоугольника. Значение этого свойства по умолчанию равно 0,0.
- `RadiusY`Тип — `double` радиус оси y, используемый для округления углов прямоугольника. Значение этого свойства по умолчанию равно 0,0.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

`Rectangle`Класс задает `Aspect` свойству, унаследованному от `Shape` класса, значение `Stretch.Fill` . Дополнительные сведения о `Aspect` свойстве см. в разделе [растяжение фигур](index.md#stretch-shapes).

## <a name="create-a-rectangle"></a>Создание прямоугольника

Чтобы нарисовать прямоугольник, создайте `Rectangle` объект и задают его `WidthRequest` `HeightRequest` Свойства и. Чтобы закрасить внутри прямоугольника, присвойте его `Fill` свойству значение [`Color`](xref:Xamarin.Forms.Color) . Чтобы присвоить прямоугольнику контур, установите `Stroke` для его свойства значение [`Color`](xref:Xamarin.Forms.Color) . `StrokeThickness`Свойство определяет толщину контура прямоугольника.

Чтобы передать скругленные углы прямоугольника, установите его `RadiusX` `RadiusY` Свойства и. Эти свойства устанавливают радиусы оси x и y, используемые для округления углов прямоугольника.

Чтобы нарисовать квадрат, сделайте `WidthRequest` Свойства и `HeightRequest` `Rectangle` объекта равными.

В следующем примере XAML показано, как нарисовать закрашенный прямоугольник:

```xaml
<Rectangle Fill="Red"
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

В этом примере отображается красный закрашенный прямоугольник с измерениями 150x50 (устройства, независимые от устройств):

![Закрашенный прямоугольник](rectangle-images/filled.png "Закрашенный прямоугольник")

В следующем примере XAML показано, как нарисовать закрашенный прямоугольник со скругленными углами:

```xaml
<Rectangle Fill="Blue"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10"
           WidthRequest="200"
           HeightRequest="100"
           HorizontalOptions="Start" />
```

В этом примере рисуется синий прямоугольник с закругленными углами:

![Прямоугольник со скругленными углами](rectangle-images/rounded.png "Прямоугольник со скругленными углами")

Дополнительные сведения о рисовании пунктирного прямоугольника см. в разделе [Рисование пунктирных фигур](index.md#draw-dashed-shapes).

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Многоугольник](index.md)