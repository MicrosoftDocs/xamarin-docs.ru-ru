---
title: 'Xamarin.Forms Фигуры: геометрические объекты'
description: Xamarin.Forms классы Geometry позволяют описать геометрию двухмерной фигуры.
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f3a89e0c5c49ec790cf35443030d50d3ddef9ed4
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97939814"
---
# <a name="no-locxamarinforms-shapes-geometries"></a>Xamarin.Forms Фигуры: геометрические объекты

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Geometry`Класс и классы, производные от него, позволяют описать геометрию двухмерной фигуры. `Geometry` объекты могут быть простыми, например прямоугольниками и круговыми, или составными, созданными из двух или более геометрических объектов. Кроме того, можно создать более сложные геометрические объекты, включающие дуги и кривые.

`Geometry`Класс является родительским классом для нескольких классов, определяющих различные категории геометрических объектов:

- `EllipseGeometry`, представляющий геометрию эллипса или круга.
- `GeometryGroup`, представляющий контейнер, который может объединять несколько объектов Geometry в один объект.
- `LineGeometry`, представляющий геометрию линии.
- `PathGeometry`, представляющий геометрию сложной фигуры, которая может состоять из дуг, кривых, эллипсов, линий и прямоугольников.
- `RectangleGeometry`, представляющий геометрию прямоугольника или квадрата.

> [!NOTE]
> Существует также `RoundedRectangleGeometry` класс, производный от `GeometryGroup` класса. Дополнительные сведения см. в разделе [раундректанглежеометри](#roundrectanglegeometry).

`Geometry`Классы и `Shape` кажутся похожими, в том, что они описывают двумерные фигуры, но имеют важное отличие. `Geometry`Класс является производным от [`BindableObject`](xref:Xamarin.Forms.BindableObject) класса, тогда как `Shape` класс является производным от [`View`](xref:Xamarin.Forms.View) класса. Таким образом, `Shape` объекты могут визуализировать себя и участвовать в системе макета, а `Geometry` объекты — нет. Хотя `Shape` объекты удобнее использовать `Geometry` , чем объекты, `Geometry` объекты являются более гибкими. Хотя `Shape` объект используется для отрисовки двухмерной графики, `Geometry` объект можно использовать для определения геометрической области для двухмерной графики и определения области для обрезки.

Следующие классы имеют свойства, которые могут быть установлены в `Geometry` объекты:

- `Path`Класс использует `Geometry` для описания его содержимого. Можно подготовить к просмотру `Geometry` , присвоив `Path.Data` свойству `Geometry` объект и задав `Path` `Fill` свойства объекта и `Stroke` .
- [`VisualElement`](xref:Xamarin.Forms.VisualElement)Класс имеет `Clip` свойство типа `Geometry` , которое определяет контур содержимого элемента. Если `Clip` для свойства задан `Geometry` объект, видима будет только область, которая находится в пределах области `Geometry` . Дополнительные сведения см. в разделе [Обрезка с помощью класса Geometry](#clip-with-a-geometry).

Классы, производные от класса, `Geometry` можно сгруппировать в три категории: простые геометрические объекты, геометрические объекты траекторий и составные геометрические объекты.

## <a name="simple-geometries"></a>Простые геометрические объекты

Простые геометрические классы: `EllipseGeometry` , `LineGeometry` и `RectangleGeometry` . Они используются для создания базовых геометрических фигур, таких как круги, линии и прямоугольники. Такие же фигуры, как и более сложные фигуры, можно создать с помощью или, `PathGeometry` объединив геометрические объекты, но эти классы предоставляют более простой подход к созданию этих базовых геометрических фигур.

### <a name="ellipsegeometry"></a>EllipseGeometry

Геометрия эллипса представляет геометрию или эллипс или окружность и определяется центральной точкой, x-радиусом и y-радиусом.

Класс `EllipseGeometry` определяет следующие свойства:

- `Center`Тип [`Point`](xref:Xamarin.Forms.Point) , представляющий центральную точку геометрии.
- `RadiusX`Тип `double` , представляющий значение x-радиуса геометрии. Значение этого свойства по умолчанию равно 0,0.
- `RadiusY`Тип `double` , представляющий значение y-радиуса геометрии. Значение этого свойства по умолчанию равно 0,0.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

В следующем примере показано, как создать и отобразить `EllipseGeometry` `Path` объект в объекте.

```xaml
<Path Fill="Blue"
      Stroke="Red">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

В этом примере центр `EllipseGeometry` имеет значение (50, 50), а для x-RADIUS и y-RADIUS устанавливается значение 50. При этом создается красный круг с диаметром 100 единиц, не зависящих от устройства, внутренняя заливка которой окрашена в синий цвет:

![EllipseGeometry](geometry-images/ellipse.png "EllipseGeometry")

### <a name="linegeometry"></a>LineGeometry

Геометрия линии представляет геометрию линии и определяется путем указания начальной точки линии и конечной точки.

Класс `LineGeometry` определяет следующие свойства:

- `StartPoint`Тип [`Point`](xref:Xamarin.Forms.Point) , представляющий начальную точку линии.
- `EndPoint`Тип [`Point`](xref:Xamarin.Forms.Point) , представляющий конечную точку линии.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

В следующем примере показано, как создать и визуализировать `LineGeometry` `Path` объект в объекте.

```xaml
<Path Stroke="Black">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

В этом примере a `LineGeometry` (10, 20) отображается в (100 130):

![LineGeometry](geometry-images/line.png "LineGeometry")

> [!NOTE]
> Задание `Fill` свойства объекта `Path` , которое визуализирует, не `LineGeometry` будет действовать, так как линия не имеет внутренней части.

### <a name="rectanglegeometry"></a>RectangleGeometry

Прямоугольная геометрия представляет геометрию прямоугольника или квадрата и определяется `Rect` структурой, указывающей ее относительное расположение и высоту и ширину.

`RectangleGeometry`Класс определяет `Rect` свойство типа `Rect` , которое представляет размеры прямоугольника. Это свойство поддерживается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом, что означает, что он может быть целевым объектом привязок данных и имеет стиль.

В следующем примере показано, как создать и визуализировать `RectangleGeometry` `Path` объект в объекте.

```xaml
<Path Fill="Blue"
      Stroke="Red">
  <Path.Data>
    <RectangleGeometry Rect="10,10,150,100" />
  </Path.Data>
</Path>
```

Расположение и размеры прямоугольника определяются `Rect` структурой. В этом примере используется значение (10, 10), ширина — 150, а высота — 100 единиц, независимых от устройства:

![RectangleGeometry](geometry-images/rectangle.png "RectangleGeometry")

## <a name="path-geometries"></a>Геометрические контуры

Геометрия контура описывает сложную фигуру, которая может состоять из дуг, кривых, эллипсов, линий и прямоугольников.

Класс `PathGeometry` определяет следующие свойства:

- `Figures`Тип `PathFigureCollection` , который представляет коллекцию `PathFigure` объектов, описывающих содержимое пути.
- `FillRule`Тип `FillRule` , который определяет, как объединяются пересекающиеся области, содержащиеся в геометрии. Значение по умолчанию этого свойства равно `FillRule.EvenOdd`.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

Дополнительные сведения о `FillRule` перечислении см. в разделе [ Xamarin.Forms фигуры: правила заливки](fillrules.md).

> [!NOTE]
> `Figures`Свойство является свойством `ContentProperty` `PathGeometry` класса, поэтому его не нужно явно задавать из XAML.

Объект состоит `PathGeometry` из коллекции `PathFigure` объектов, каждый из которых `PathFigure` описывает фигуру в геометрии. Каждый `PathFigure` из них состоит из одного или нескольких `PathSegment` объектов, каждый из которых описывает сегмент фигуры. Существует много типов сегментов:

- `ArcSegment`, который создает эллиптическую дугу между двумя точками.
- `BezierSegment`, который создает кривую Безье третьего порядка между двумя точками.
- `LineSegment`, который создает линию между двумя точками.
- `PolyBezierSegment`, который создает ряд кривых Безье третьего порядка.
- `PolyLineSegment`, который создает ряд строк.
- `PolyQuadraticBezierSegment`, который создает ряд квадратичных кривых Безье.
- `QuadraticBezierSegment`, который создает кривую Безье второго типа.

Все приведенные выше классы являются производными от абстрактного `PathSegment` класса.

Сегменты в `PathFigure` объединяются в одну геометрическую форму, а конечная точка каждого сегмента является начальной точкой следующего сегмента. `StartPoint`Свойство объекта `PathFigure` определяет точку, из которой рисуется первый сегмент. Каждый последующий сегмент начинается в конечной точке предыдущего сегмента. Например, вертикальная линия из `10,50` в `10,150` может быть определена путем присвоения `StartPoint` свойству значения `10,50` и создания `LineSegment` свойства со `Point` свойством `10,150` :

```xaml
<Path Stroke="Black">
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

### <a name="create-an-arcsegment"></a>Создание ArcSegment

`ArcSegment`Создает эллиптическую дугу между двумя точками. Эллиптическая дуга определяется ее начальной и конечной точками, x-and y-радиусом, коэффициентом поворота по оси x, значением, указывающим, должна ли дуга быть больше 180 градусов, и значение, описывающее направление рисования дуги.

Класс `ArcSegment` определяет следующие свойства:

- `Point`Тип [`Point`](xref:Xamarin.Forms.Point) , представляющий конечную точку эллиптической дуги. Значение этого свойства по умолчанию равно (0, 0).
- `Size`Тип [`Size`](xref:Xamarin.Forms.Size) , который представляет координаты x и y радиуса дуги. Значение этого свойства по умолчанию равно (0, 0).
- `RotationAngle`Тип `double` , который представляет величину в градусах, на которую вращается эллипс вокруг оси x. Значение по умолчанию для этого свойства равно 0.
- `SweepDirection`Тип `SweepDirection` , который указывает направление рисования дуги. Значение по умолчанию этого свойства равно `SweepDirection.CounterClockwise`.
- `IsLargeArc`Тип `bool` , который указывает, должна ли дуга быть больше 180 градусов. Значение по умолчанию этого свойства равно `false`.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

> [!NOTE]
> `ArcSegment`Класс не содержит свойство для начальной точки дуги. Он определяет только конечную точку дуги, которую он представляет. Начальная точка дуги — это текущая точка элемента, `PathFigure` к которому `ArcSegment` добавляется объект.

Перечисление `SweepDirection` определяет следующие члены:

- `CounterClockwise`, который указывает, что Дуги рисуются в направлении по часовой стрелке.
- `Clockwise`, который указывает, что Дуги рисуются по часовой стрелке.

В следующем примере показано, как создать и отобразить `ArcSegment` `Path` объект в объекте.

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <ArcSegment Size="100,50"
                                            RotationAngle="45"
                                            IsLargeArc="True"
                                            SweepDirection="CounterClockwise"
                                            Point="200,100" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

В этом примере эллиптическая дуга рисуется от (10, 10) до (200 100).

### <a name="create-a-beziersegment"></a>Создание BezierSegment

`BezierSegment`Создает кривую Безье третьего порядка между двумя точками. Кривая Безье третьего порядка определяется четырьмя точками: начальной точкой, конечной точкой и двумя контрольными точками.

Класс `BezierSegment` определяет следующие свойства:

- `Point1`Тип [`Point`](xref:Xamarin.Forms.Point) , представляющий первую контрольную точку кривой. Значение этого свойства по умолчанию равно (0, 0).
- `Point2`Тип [`Point`](xref:Xamarin.Forms.Point) , представляющий вторую контрольную точку кривой. Значение этого свойства по умолчанию равно (0, 0).
- `Point3`Тип [`Point`](xref:Xamarin.Forms.Point) , представляющий конечную точку кривой. Значение этого свойства по умолчанию равно (0, 0).

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

> [!NOTE]
> `BezierSegment`Класс не содержит свойство для начальной точки кривой. Начальная точка кривой — это текущая точка элемента, `PathFigure` к которому `BezierSegment` добавляется объект.

Две контрольные точки кривой Безье третьего порядка ведут себя как магнитные, привлечения части того, что в противном случае будет прямой линией к себе и создания кривой. Первая контрольная точка влияет на начальную часть кривой. Вторая контрольная точка влияет на конечную часть кривой. Кривая не обязательно проходит через любую контрольную точку. Вместо этого каждая точка управления перемещает свою часть линии в сторону самого себя, но не саму себя.

В следующем примере показано, как создать и визуализировать `BezierSegment` `Path` объект в объекте.

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <BezierSegment Point1="100,0"
                                               Point2="200,200"
                                               Point3="300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

В этом примере кривая Безье третьего порядка рисуется от (10, 10) до (300, 10). Кривая имеет две контрольные точки в (100, 0) и (200 200):

![BezierSegment](geometry-images/beziersegment.png "BezierSegment")

### <a name="create-a-linesegment"></a>Создание LineSegment

`LineSegment`Создает линию между двумя точками.

`LineSegment`Класс определяет `Point` свойство типа [`Point`](xref:Xamarin.Forms.Point) , представляющего конечную точку сегмента линии. Значение этого свойства по умолчанию равно (0, 0) и поддерживается объектом. это [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) означает, что он может быть целевым объектом привязок данных и имеет стиль.

> [!NOTE]
> `LineSegment`Класс не содержит свойство для начальной точки линии. Он определяет только конечную точку. Начальная точка линии — это текущая точка элемента, `PathFigure` к которому `LineSegment` добавляется объект.

В следующем примере показано, как создавать и визуализировать `LineSegment` объекты в `Path` объекте.

```xaml
<Path Stroke="Black"      
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure IsClosed="True"
                                StartPoint="10,100">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="100,100" />
                                <LineSegment Point="100,50" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

В этом примере сегмент линии рисуется от (10 100) до (100 100) и от (100 100) до (100, 50). Кроме того, `PathFigure` закрывается, так как `IsClosed` свойство имеет значение `true` . Это приводит к прорисовке треугольника:

![линесегментс](geometry-images/linesegments.png "линесегментс")

### <a name="create-a-polybeziersegment"></a>Создание PolyBezierSegment

`PolyBezierSegment`Создает одну или несколько кривых Безье третьего порядка.

`PolyBezierSegment`Класс определяет `Points` свойство типа `PointCollection` , которое представляет точки, определяющие `PolyBezierSegment` . Объект `PointCollection` является `ObservableCollection` набором [`Point`](xref:Xamarin.Forms.Point) объектов. Это свойство поддерживается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом, что означает, что он может быть целевым объектом привязок данных и имеет стиль.

> [!NOTE]
> `PolyBezierSegment`Класс не содержит свойство для начальной точки кривой. Начальная точка кривой — это текущая точка элемента, `PathFigure` к которому `PolyBezierSegment` добавляется объект.

В следующем примере показано, как создать и визуализировать `PolyBezierSegment` `Path` объект в объекте.

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <PolyBezierSegment Points="0,0 100,0 150,100 150,0 200,0 300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

В этом примере параметр `PolyBezierSegment` задает две кривые Безье третьего порядка. Первая кривая — от (10, 10) до (150 100) с контрольной точкой (0, 0) и другой контрольной точкой (100, 0). Вторая кривая — от (150 100) до (300, 10) с контрольной точкой (150, 0) и другой контрольной точкой (200, 0):

![PolyBezierSegment](geometry-images/polybeziersegment.png "PolyBezierSegment")

### <a name="create-a-polylinesegment"></a>Создание PolyLineSegment

`PolyLineSegment`Создает один или несколько сегментов линии.

`PolyLineSegment`Класс определяет `Points` свойство типа `PointCollection` , которое представляет точки, определяющие `PolyLineSegment` . Объект `PointCollection` является `ObservableCollection` набором [`Point`](xref:Xamarin.Forms.Point) объектов. Это свойство поддерживается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом, что означает, что он может быть целевым объектом привязок данных и имеет стиль.

> [!NOTE]
> `PolyLineSegment`Класс не содержит свойство для начальной точки линии. Начальная точка линии — это текущая точка элемента, `PathFigure` к которому `PolyLineSegment` добавляется объект.

В следующем примере показано, как создать и визуализировать `PolyLineSegment` `Path` объект в объекте.

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigure StartPoint="10,10">
                    <PathFigure.Segments>
                        <PolyLineSegment Points="50,10 50,50" />
                    </PathFigure.Segments>
                </PathFigure>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

В этом примере параметр `PolyLineSegment` определяет две строки. Первая строка — от (10, 10) до (50, 10), а вторая — от (50, 10) до (50, 50):

![PolyLineSegment](geometry-images/polylinesegment.png "PolyLineSegment")

### <a name="create-a-polyquadraticbeziersegment"></a>Создание PolyQuadraticBezierSegment

`PolyQuadraticBezierSegment`Создает одну или несколько кривых Безье второго типа.

`PolyQuadraticBezierSegment`Класс определяет `Points` свойство типа `PointCollection` , которое представляет точки, определяющие `PolyQuadraticBezierSegment` . Объект `PointCollection` является `ObservableCollection` набором [`Point`](xref:Xamarin.Forms.Point) объектов. Это свойство поддерживается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом, что означает, что он может быть целевым объектом привязок данных и имеет стиль.

> [!NOTE]
> `PolyQuadraticBezierSegment`Класс не содержит свойство для начальной точки кривой. Начальная точка кривой — это текущая точка элемента, `PathFigure` к которому `PolyQuadraticBezierSegment` добавляется объект.

В следующем примере показано, как создать и визуализировать `PolyQuadraticBezierSegment` объект в `Path` объекте:

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <PolyQuadraticBezierSegment Points="100,100 150,50 0,100 15,200" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

В этом примере параметр `PolyQuadraticBezierSegment` задает две кривые Безье. Первая кривая — от (10, 10) до (150, 50) с контрольной точкой в (100 100). Вторая кривая — от (100 100) до (15 200) с контрольной точкой в (0100):

![PolyQuadraticBezierSegment](geometry-images/polyquadraticbeziersegment.png "PolyQuadraticBezierSegment")

### <a name="create-a-quadraticbeziersegment"></a>Создание QuadraticBezierSegment

`QuadraticBezierSegment`Создает кривую Безье второго типа между двумя точками.

Класс `QuadraticBezierSegment` определяет следующие свойства:

- `Point1`Тип [`Point`](xref:Xamarin.Forms.Point) , который представляет контрольную точку кривой. Значение этого свойства по умолчанию равно (0, 0).
- `Point2`Тип [`Point`](xref:Xamarin.Forms.Point) , представляющий конечную точку кривой. Значение этого свойства по умолчанию равно (0, 0).

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

> [!NOTE]
> `QuadraticBezierSegment`Класс не содержит свойство для начальной точки кривой. Начальная точка кривой — это текущая точка элемента, `PathFigure` к которому `QuadraticBezierSegment` добавляется объект.

В следующем примере показано, как создать и визуализировать `QuadraticBezierSegment` `Path` объект в объекте.

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <QuadraticBezierSegment Point1="200,200"
                                                        Point2="300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

В этом примере кривая Безье второго типа рисуется от (10, 10) до (300, 10). Кривая имеет контрольную точку в (200 200):

![QuadraticBezierSegment](geometry-images/quadraticbeziersegment.png "QuadraticBezierSegment")

### <a name="create-complex-geometries"></a>Создание сложных геометрических объектов

Более сложные геометрические объекты можно создавать с помощью сочетания `PathSegment` объектов. В следующем примере создается фигура с помощью `BezierSegment` , `LineSegment` и `ArcSegment` .

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigure StartPoint="10,50">
                    <PathFigure.Segments>
                        <BezierSegment Point1="100,0"
                                       Point2="200,200"
                                       Point3="300,100"/>
                        <LineSegment Point="400,100" />
                        <ArcSegment Size="50,50"
                                    RotationAngle="45"
                                    IsLargeArc="True"
                                    SweepDirection="Clockwise"
                                    Point="200,100"/>
                    </PathFigure.Segments>
                </PathFigure>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

В этом примере `BezierSegment` сначала определяется с помощью четырех точек. Затем в примере добавляется объект `LineSegment` , который рисуется между конечной точкой объекта и `BezierSegment` точкой, заданной `LineSegment` . Наконец, объект `ArcSegment` рисуется от конечной точки `LineSegment` до точки, заданной в `ArcSegment` .

Еще более сложные геометрические объекты можно создавать с помощью нескольких `PathFigure` объектов в `PathGeometry` . В следующем примере создается объект `PathGeometry` из семи `PathFigure` объектов, некоторые из которых содержат несколько `PathSegment` объектов:

```xaml
<Path Stroke="Red"
      StrokeThickness="12"
      StrokeLineJoin="Round">
    <Path.Data>
        <PathGeometry>
            <!-- H -->
            <PathFigure StartPoint="0,0">
                <LineSegment Point="0,100" />
            </PathFigure>
            <PathFigure StartPoint="0,50">
                <LineSegment Point="50,50" />
            </PathFigure>
            <PathFigure StartPoint="50,0">
                <LineSegment Point="50,100" />
            </PathFigure>

            <!-- E -->
            <PathFigure StartPoint="125, 0">
                <BezierSegment Point1="60, -10"
                               Point2="60, 60"
                               Point3="125, 50" />
                <BezierSegment Point1="60, 40"
                               Point2="60, 110"
                               Point3="125, 100" />
            </PathFigure>

            <!-- L -->
            <PathFigure StartPoint="150, 0">
                <LineSegment Point="150, 100" />
                <LineSegment Point="200, 100" />
            </PathFigure>

            <!-- L -->
            <PathFigure StartPoint="225, 0">
                <LineSegment Point="225, 100" />
                <LineSegment Point="275, 100" />
            </PathFigure>

            <!-- O -->
            <PathFigure StartPoint="300, 50">
                <ArcSegment Size="25, 50"
                            Point="300, 49.9"
                            IsLargeArc="True" />
            </PathFigure>
        </PathGeometry>
    </Path.Data>
</Path>
```

В этом примере слово Hello рисуется с помощью сочетания `LineSegment` `BezierSegment` объектов и, а также с одним `ArcSegment` объектом:

![Несколько объектов PathFigure](geometry-images/multiple-pathfigures.png "Несколько объектов PathFigure")

## <a name="composite-geometries"></a>Составные геометрические объекты

Составные геометрические объекты можно создавать с помощью `GeometryGroup` . `GeometryGroup`Класс создает составную геометрию из одного или нескольких `Geometry` объектов. `Geometry`В можно добавить любое количество объектов `GeometryGroup` .

Класс `GeometryGroup` определяет следующие свойства:

- `Children`Тип `GeometryCollection` , который виды цветов объекты, определяющие `GeomtryGroup` . Объект `GeometryCollection` является `ObservableCollection` набором `Geometry` объектов.
- `FillRule`Тип `FillRule` , который определяет способ объединения пересекающихся областей в `GeometryGroup` . Значение по умолчанию этого свойства равно `FillRule.EvenOdd`.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

> [!NOTE]
> `Children`Свойство является свойством `ContentProperty` `GeometryGroup` класса, поэтому его не нужно явно задавать из XAML.

Дополнительные сведения о `FillRule` перечислении см. в разделе [ Xamarin.Forms фигуры: правила заливки](fillrules.md).

Чтобы нарисовать составную геометрию, установите необходимые `Geometry` объекты в качестве дочерних элементов `GeometryGroup` и отобразите их с помощью `Path` объекта. Ниже приведен пример кода XAML.

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

В этом примере `EllipseGeometry` объединяются четыре объекта с одинаковыми координатами x-RADIUS и y-радиуса, но с разными координатами центра. При этом создаются четыре перекрывающихся круга, внутренние области которых заполняются оранжевый из-за правила заполнения по умолчанию `EvenOdd` :

![GeometryGroup](geometry-images/geometrygroup.png "GeometryGroup")

### <a name="roundrectanglegeometry"></a>раундректанглежеометри

Круглая прямоугольная геометрия представляет геометрию прямоугольника или квадрата со скругленными углами и определяется радиусом угла и [`Rect`](xref:Xamarin.Forms.Rect) структурой, указывающей ее относительное расположение и высоту и ширину.

`RoundRectangleGeometry`Класс, производный от `GeometryGroup` класса, определяет следующие свойства:

- `CornerRadius`Тип [`CornerRadius`](xref:Xamarin.Forms.CornerRadius) , являющийся угловой радиусом геометрии.
- `Rect`Тип [`Rect`](xref:Xamarin.Forms.Rect) , который представляет размеры прямоугольника.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

> [!NOTE]
> Правило заполнения, используемое объектом, `RoundRectangleGeometry` — `FillRule.Nonzero` . Дополнительные сведения о правилах заливки см. в разделе [ Xamarin.Forms фигуры: правила заливки](fillrules.md).

В следующем примере показано, как создать и визуализировать `RoundRectangleGeometry` `Path` объект в объекте.

```xaml
<Path Fill="Blue"
      Stroke="Red">
    <Path.Data>
        <RoundRectangleGeometry CornerRadius="5"
                                Rect="10,10,150,100" />
    </Path.Data>
</Path>
```

Расположение и размеры прямоугольника определяются [`Rect`](xref:Xamarin.Forms.Rect) структурой. В этом примере используется значение (10, 10), ширина — 150, а высота — 100 единиц, независимых от устройства. Кроме того, угловые углы округляются с радиусом 5 единиц, не зависящих от устройства.

## <a name="clip-with-a-geometry"></a>Обрезка с геометрическим объектом

[`VisualElement`](xref:Xamarin.Forms.VisualElement)Класс имеет `Clip` свойство типа `Geometry` , которое определяет контур содержимого элемента. Если `Clip` для свойства задан `Geometry` объект, видима будет только область, которая находится в пределах области `Geometry` .

В следующем примере показано, как использовать `Geometry` объект в качестве области отсечения для [`Image`](xref:Xamarin.Forms.Image) :

```xaml
<Image Source="monkeyface.png">
    <Image.Clip>
        <EllipseGeometry RadiusX="100"
                         RadiusY="100"
                         Center="180,180" />
    </Image.Clip>
</Image>
```

В этом примере в качестве `EllipseGeometry` `RadiusX` значения типа и `RadiusY` используется значение 100, а в качестве `Center` значения (180 180) — `Clip` свойство объекта [`Image`](xref:Xamarin.Forms.Image) . Будет отображена только часть изображения, расположенная в области эллипса:

![Обрезать изображение с помощью EllipseGeometry](geometries-images/clip-ellipsegeometry.png "Обрезать изображение с помощью EllipseGeometry")

> [!NOTE]
> Для обрезки объектов можно использовать простые геометрические объекты, геометрические контуры и составные геометрии [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

## <a name="other-features"></a>Другие возможности

`GeometryHelper`Класс предоставляет следующие вспомогательные методы:

- `FlattenGeometry`, который выполняет сведение в `Geometry` `PathGeometry` .
- `FlattenCubicBezier`, который выполняет сведение кривой Безье третьего порядка в `List<Point>` коллекцию.
- `FlattenQuadraticBezier`, который выравнивает кривую Безье квадратичных кривых в `List<Point>` коллекцию.
- `FlattenArc`, который выполняет сведение эллиптической дуги в `List<Point>` коллекцию.

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Многоугольник](index.md)
- [Xamarin.Forms Фигуры: правила заливки](fillrules.md)
