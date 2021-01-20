---
title: 'Xamarin.Forms Фигуры: правила заливки'
description: Xamarin.Forms Правила заливки фигур определяют, находится ли точка в области заполнения фигуры.
ms.prod: xamarin
ms.assetid: 5CABB22B-C6BE-43D1-91D9-6E90A4BD5622
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3daf08c688be41652ae2573b0bf58e2ace2072c6
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98609148"
---
# <a name="no-locxamarinforms-shapes-fill-rules"></a>Xamarin.Forms Фигуры: правила заливки

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Некоторые Xamarin.Forms классы фигур имеют `FillRule` свойства типа `FillRule` . К ним относятся `Polygon` , `Polyline` и `GeometryGroup` .

`FillRule`Перечисление определяет `EvenOdd` и `Nonzero` члены. Каждый элемент представляет другое правило для определения того, находится ли точка в области заливки фигуры.

> [!IMPORTANT]
> Все фигуры считаются закрытыми в целях правил заполнения.

## <a name="evenodd"></a>EvenOdd

`EvenOdd`Правило заполнения рисует луч с точки до бесконечности в любом направлении и подсчитывает количество сегментов в фигуре, пересекающих луч. Если это число нечетное, то точка находится внутри. Если это число четное, точка находится вне.

В следующем примере XAML создается и отрисовывается составная фигура со `FillRule` значением по умолчанию `EvenOdd` :

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <!-- FillRule doesn't need to be set, because EvenOdd is the default. -->
        <GeometryGroup>
            <EllipseGeometry RadiusX="50"
                             RadiusY="50"
                             Center="75,75" />
            <EllipseGeometry RadiusX="70"
                             RadiusY="70"
                             Center="75,75" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="75,75" />
            <EllipseGeometry RadiusX="120"
                             RadiusY="120"
                             Center="75,75" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

В этом примере отображается составная фигура, состоящие из серии концентрических колец:

![Составная фигура с правилом заполнения EvenOdd](fillrule-images/evenodd.png "Составная фигура с правилом заполнения EvenOdd")

Обратите внимание, что в составной фигуре не заполнен центр и третий звонок. Это происходит потому, что луч, нарисованный из любой точки в любом из двух колец, проходит через четное число сегментов:

![Составная фигура с аннотацией с правилом заполнения EvenOdd](fillrule-images/evenodd-annotated.png "Составная фигура с аннотацией с правилом заполнения EvenOdd")

На изображении выше красные кружки обозначают точки, а линии представляют произвольные лучи. Для верхней точки два произвольных луча, каждый из которых проходят через четное число сегментов линии. Следовательно, кольцо не заполнено. В нижней точке каждый из двух произвольных лучей проходит через нечетное число сегментов строк. Таким образом, точка звонка заполнена.

## <a name="nonzero"></a>Отличные

`Nonzero`Правило заполнения рисует луч от точки до бесконечности в любом направлении, а затем проверяет места, в которых сегмент фигуры пересекает луч. Начиная с нуля счетчик увеличивается каждый раз, когда сегмент пересекает луч слева направо и уменьшается каждый раз, когда сегмент пересекает луч справа налево. После подсчета пересечения, если результат равен нулю, точка находится за пределами многоугольника. В противном случае он находится внутри.

В следующем примере XAML создается и визуализируется составная фигура с параметром `FillRule` `Nonzero` :

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <GeometryGroup FillRule="Nonzero">
            <EllipseGeometry RadiusX="50"
                             RadiusY="50"
                             Center="75,75" />
            <EllipseGeometry RadiusX="70"
                             RadiusY="70"
                             Center="75,75" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="75,75" />
            <EllipseGeometry RadiusX="120"
                             RadiusY="120"
                             Center="75,75" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

В этом примере отображается составная фигура, состоящие из серии концентрических колец:

![На схеме показаны четыре концентрические круги, заполненные.](fillrule-images/nonzero.png "Составная фигура с ненулевым правилом заполнения")

Обратите внимание, что в составной фигуре заполнены все кольца. Это связано с тем, что все сегменты выполняются в одном направлении, поэтому луч, нарисованный из любой точки, будет пересекать один или несколько сегментов, а сумма пересечений не будет равняться нулю:

![На схеме показаны круги из предыдущей диаграммы с стрелками направления и лучом, снабженный + 1 для каждого кружка.](fillrule-images/nonzero-annotated.png "Составная фигура с заметками с ненулевым правилом заполнения")

На изображении над красными стрелками обозначает направление рисования сегментов, а черная стрелка — произвольный луч, выполняемый с точки во внутреннем кольце. Начиная с нуля, для каждого сегмента, который пересекает луч, значение 1 добавляется, так как сегмент пересекает луч слева направо.

Более сложная форма с сегментами, работающими в разных направлениях, необходима для лучшего демонстрации поведения `Nonzero` правила заполнения. В следующем примере XAML создается похожая форма для предыдущего примера, за исключением того, что она создается с помощью `PathGeometry` вместо `EllipseGeometry` :

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF">
     <Path.Data>
         <GeometryGroup FillRule="Nonzero">
             <PathGeometry>
                 <PathGeometry.Figures>
                     <!-- Inner ring -->
                     <PathFigure StartPoint="120,120">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="50,50"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,120" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Second ring -->
                     <PathFigure StartPoint="120,100">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="70,70"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,100" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Third ring  -->
                         <PathFigure StartPoint="120,70">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="100,100"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,70" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Outer ring -->
                     <PathFigure StartPoint="120,300">
                         <PathFigure.Segments>
                             <ArcSegment Size="130,130"
                                         IsLargeArc="True"
                                         SweepDirection="Clockwise"
                                         Point="140,300" />
                         </PathFigure.Segments>
                     </PathFigure>
                 </PathGeometry.Figures>
             </PathGeometry>
         </GeometryGroup>
     </Path.Data>
 </Path>
```

В этом примере рисуется ряд сегментов дуги, которые не закрыты:

![На схеме показаны четыре концентрические окружности, а также самые и третьи из самого внешнего заполнения.](fillrule-images/nonzero-gaps.png "Составная фигура с ненулевым правилом заполнения")

На приведенном выше рисунке третья дуга от центра не заполнена. Это обусловлено тем, что сумма значений данного луча, пересекает сегменты в пути, равна нулю:

![На схеме показаны круги из предыдущей диаграммы с стрелками направления и двумя метками, помеченными + 1 или – 1 для каждого круга, на который они пересекаются.](fillrule-images/nonzero-gaps-annotated.png "Составная фигура с заметками с ненулевым правилом заполнения")

На рисунке выше красный кружок представляет точку, черные линии представляют произвольные лучи, которые выходят из точки в незаполненной области, а красные стрелки — направление рисования сегментов. Как можно видеть, сумма значений с лучей, пересечением сегментов, равна нулю:

- Произвольный луч, который перемещается по диагонали вправо, пересекает два сегмента, которые выполняются в разных направлениях. Таким образом, сегменты отменяют друг друга, предоставляя нулевое значение.
- Произвольный луч, который перемещается по диагонали влево, пересекают шесть сегментов. Однако пересечение отменяет друг друга, так что ноль является окончательной суммой.

Сумма, равная нулю, не заполняется кольцом.

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Многоугольник](index.md)
