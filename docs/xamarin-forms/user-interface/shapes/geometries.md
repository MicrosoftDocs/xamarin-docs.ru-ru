---
title: 'Xamarin.FormsФигуры: геометрические объекты'
description: Xamarin.Formsклассы Geometry позволяют описать геометрию двухмерной фигуры.
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bac0846700d6bcc7b23681dcb6ebf8ddafa155bc
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947384"
---
# <a name="xamarinforms-shapes-geometries"></a>Xamarin.FormsФигуры: геометрические объекты

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Geometry`Класс и классы, производные от него, позволяют описать геометрию двухмерной фигуры. `Geometry`объекты могут быть простыми, например прямоугольниками и круговыми, или составными, созданными из двух или более геометрических объектов. Более сложные геометрические объекты можно создавать с помощью `PathGeometry` класса, который позволяет описывать дуги и кривые.

`Geometry`Классы и `Shape` кажутся похожими, в том, что они описывают двумерные фигуры, но имеют важное отличие. `Geometry`Класс является производным от `BindableObject` класса, тогда как `Shape` класс является производным от `View` класса. Таким образом, `Shape` объекты могут визуализировать себя и участвовать в системе макета, а `Geometry` объекты — нет. Хотя `Shape` объекты удобнее использовать `Geometry` , чем объекты, `Geometry` объекты являются более гибкими. Хотя `Shape` объект используется для отрисовки двухмерной графики, `Geometry` объект можно использовать для определения геометрической области для двухмерной графики и определения области для обрезки.

Базовый класс для всех геометрических объектов является абстрактным классом `Geometry` . Классы, производные от этого класса, можно сгруппировать в три категории: простые геометрические объекты, геометрические объекты траектории и составные геометрические объекты.

## <a name="simple-geometries"></a>Простые геометрические объекты

Простые геометрические классы: `EllipseGeometry` , `LineGeometry` и `RectangleGeometry` . Они используются для создания базовых геометрических фигур, таких как круги, линии и прямоугольники. Такие же фигуры, как и более сложные фигуры, можно создать с помощью или, `PathGeometry` объединив геометрические объекты, но эти классы предоставляют более простые средства для создания этих базовых геометрических фигур.

### <a name="ellipsegeometry"></a>EllipseGeometry

Геометрия эллипса представляет геометрию или эллипс или окружность и определяется центральной точкой, x-радиусом и y-радиусом.

`EllipseGeometry`Класс определяет следующие свойства:

- `Center`Тип `Point` , представляющий центральную точку геометрии.
- `RadiusX`Тип `double` , представляющий значение x-радиуса геометрии. Значение этого свойства по умолчанию равно 0,0.
- `RadiusY`Тип `double` , представляющий значение y-радиуса геометрии. Значение этого свойства по умолчанию равно 0,0.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

В следующем примере показано, как создать и отобразить `EllipseGeometry` :

```xaml
<Path Fill="Blue"
      Stroke="Red"
      StrokeThickness="1">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

В этом примере центр `EllipseGeometry` имеет значение (50, 50), а для x-RADIUS и y-RADIUS устанавливается значение 50. При этом создается круг с диаметром 100, внутренняя часть которого окрашивается синим цветом.

### <a name="linegeometry"></a>LineGeometry

Геометрия линии представляет геометрию линии и определяется путем указания начальной точки линии и конечной точки.

`LimeGeometry`Класс определяет следующие свойства:

- `StartPoint`Тип `Point` , представляющий начальную точку линии.
- `EndPoint`Тип `Point` , представляющий конечную точку линии.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

В следующем примере показано, как создать и отобразить `LineGeometry` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

В этом примере объект `LineGeometry` рисуется от (10, 20) до (100 130).

### <a name="rectanglegeometry"></a>RectangleGeometry

Прямоугольная геометрия представляет прямоугольник и определяется `Rect` структурой, указывающей ее относительное расположение и высоту и ширину.

`RectangleGeometry`Класс определяет следующие свойства:

- `Rect`Тип `FormsRect` , который представляет размеры прямоугольника.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

В следующем примере показано, как создать и отобразить `RectangleGeometry` :

```xaml
<Path Fill="Blue"
      Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <RectangleGeometry Rect="50,50,25,25" />
  </Path.Data>
</Path>
```

Расположение и размеры прямоугольника определяются `Rect` структурой. В этом примере позицией является (50, 50), а высота и ширина — как 25, что создает квадрат.

## <a name="path-geometries"></a>Геометрические контуры

Геометрия контура описывает сложную фигуру, которая может состоять из дуг, кривых, эллипсов, линий и прямоугольников.

`PathGeometry`Класс определяет следующие свойства:

- `Figures`Тип `PathFigureCollection` , который представляет коллекцию `PathFigure` объектов, описывающих содержимое пути.
- `FillRule`Тип `FillRule` , который определяет, как объединяются пересекающиеся области, содержащиеся в геометрии. Значение по умолчанию этого свойства равно `FillRule.EvenOdd`.

> [!NOTE]
> `Figures`Свойство является свойством `ContentProperty` `PathGeometry` класса, поэтому его не нужно явно задавать из XAML.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

Объект состоит `PathGeometry` из коллекции `PathFigure` объектов, каждый из которых `PathFigure` описывает фигуру в геометрии. Каждый `PathFigure` из них состоит из одного или нескольких `PathSegment` объектов, каждый из которых описывает сегмент фигуры. Существует много типов сегментов:

- `ArcSegment`, который создает эллиптическую дугу между двумя точками.
- `BezierSegment`, который создает кривую Безье третьего порядка между двумя точками.
- `LineSegment`, который создает линию между двумя точками.
- `PolyBezierSegment`, который создает ряд кривых Безье третьего порядка.
- `PolyLineSegment`, который создает ряд строк.
- `PolyQuadraticBezierSegment`, который создает ряд квадратичных кривых Безье.
- `QuadraticBezierSegment`, который создает кривую Безье второго типа.

Сегменты в `PathFigure` объединяются в одну геометрическую форму, а конечная точка каждого сегмента является начальной точкой следующего сегмента. `StartPoint`Свойство объекта `PathFigure` определяет точку, из которой рисуется первый сегмент. Каждый последующий сегмент начинается в конечной точке предыдущего сегмента. Например, вертикальная линия из `10,50` в `10,150` может быть определена путем присвоения `StartPoint` свойству значения `10,50` и создания `LineSegment` свойства со `Point` свойством `10,150` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,50">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="10,150" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

Более сложные геометрические объекты можно создавать с помощью сочетания `PathSegment` объектов, а также с помощью нескольких `PathFigure` объектов в `PathGeometry` .

## <a name="composite-geometries"></a>Составные геометрические объекты

Составные геометрические объекты можно создавать с помощью `GeometryGroup` . `GeometryGroup`Класс создает составную геометрию из одного или нескольких `Geometry` объектов. `Geometry`В можно добавить любое количество объектов `GeometryGroup` .

В следующем примере показано, как объединить геометрические объекты в `GeometryGroup` :

```xaml
<Path Stroke="Green"
      StrokeThickness="2"
      Fill="Orange">
    <Path.Data>
        <GeometryGroup>
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,250" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,250" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

В этом примере `EllipseGeometry` объединяются четыре объекта с одинаковыми координатами x-RADIUS и y-радиуса, но с разными координатами центра. При этом создаются четыре перекрывающихся кружков, внутренние области которых заполняются с помощью `EvenOdd` правила заливки.

## <a name="other-features"></a>Другие возможности

`GeometryHelper`Класс предоставляет следующие вспомогательные методы:

- `FlattenGeometry`
- `FlattenCubicBezier`
- `FlattenQuadraticBezier`
- `FlattenArc`

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.FormsМногоугольник](index.md)
