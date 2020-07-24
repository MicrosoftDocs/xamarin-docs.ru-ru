---
title: 'Xamarin.FormsФигуры: ломаная линия'
description: Xamarin.FormsКласс ломаной линии может использоваться для рисования ряда Соединенных прямых линий.
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2f00daff6cada3f74b2dbcf6daa8245b80cf13eb
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937661"
---
# <a name="xamarinforms-shapes-polyline"></a>Xamarin.FormsФигуры: ломаная линия

![API предварительного выпуска](~/media/shared/preview.png "Этот API-интерфейс сейчас доступен в предварительной версии.")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polyline`Класс является производным от `Shape` класса и может использоваться для рисования ряда Соединенных прямых линий. Ломаная линия похожа на многоугольник, за исключением того, что последняя точка ломаной линии не соединена с первой точкой. Сведения о свойствах, которые `Polyline` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Polyline` определяет следующие свойства:

- `Points`Тип `PointCollection` , представляющий собой коллекцию `Point` структур, описывающих точки вершин ломаной линии.
- `FillRule`Тип `FillRule` , который определяет способ объединения пересекающихся областей в ломаной линии. Значение по умолчанию этого свойства равно `FillRule.EvenOdd`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

`PointsCollection`Тип является типом объекта `ObservableCollection` [`Point`](xref:Xamarin.Forms.Point) . `Point`Структура определяет `X` и `Y` свойства типа `double` , представляющие пару координат x и y в двухмерном пространстве. Таким образом, `Points` свойству следует присвоить список пар координат x и y, описывающих точки вершин ломаной линии, разделенных одной запятой и/или одним или несколькими пробелами. Например, допустимыми являются значения 40, 10 70, 80 и 40 10, 70 80.

Дополнительные сведения о `FillRule` перечислении см. в разделе [ Xamarin.Forms фигуры: правила заливки](fillrules.md).

## <a name="create-a-polyline"></a>Создание ломаной линии

Чтобы нарисовать ломаную линию, создайте `Polyline` объект и присвойте его `Points` свойству вершины фигуры. Чтобы присвоить контуру ломаную линию, задайте `Stroke` для его свойства значение [`Color`](xref:Xamarin.Forms.Color) . `StrokeThickness`Свойство определяет толщину контура ломаной линии.

> [!IMPORTANT]
> Если свойству присвоено значение `Fill` `Polyline` [`Color`](xref:Xamarin.Forms.Color) , внутреннее пространство ломаной линии закрашивается, даже если начальная и конечная точки не пересекаются.

В следующем примере XAML показано, как нарисовать ломаную линию.

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red" />
```

В этом примере рисуется Красная Ломаная линия:

![Ломаная линия](polyline-images/stroke.png "Ломаная линия")

В следующем примере XAML показано, как нарисовать пунктирную линию.

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          StrokeThickness="2"
          StrokeDashArray="1,1"
          StrokeDashOffset="6" />
```

В этом примере Ломаная линия имеет штриховую линию:

![Пунктирная Ломаная линия](polyline-images/dashed.png "Пунктирная Ломаная линия")

Дополнительные сведения о рисовании пунктирной линии см. в разделе [Рисование пунктирных фигур](index.md#draw-dashed-shapes).

В следующем примере XAML показана Ломаная линия, использующая правило заполнения по умолчанию:

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Blue"
          Stroke="Red"
          StrokeThickness="3" />
```

В этом примере поведение заливки ломаной линии определяется с помощью `EvenOdd` правила заполнения.

![Ломаная линия EvenOdd](polyline-images/evenodd.png "EvenOdd Полине")

В следующем примере XAML показана Ломаная линия, использующая `Nonzero` правило заполнения:

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Black"
          FillRule="Nonzero"
          Stroke="Yellow"
          StrokeThickness="3" />
```

![Ненулевая Ломаная линия](polyline-images/nonzero.png "Ненулевая Ломаная линия")

В этом примере поведение заливки ломаной линии определяется с помощью `Nonzero` правила заполнения.

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsМногоугольник](index.md)
- [Xamarin.FormsФигуры: правила заливки](fillrules.md)
