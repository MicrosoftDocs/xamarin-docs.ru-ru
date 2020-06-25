---
title: 'Xamarin.FormsФигуры: правила заливки'
description: Xamarin.FormsПравила заливки фигур определяют, находится ли точка в области заполнения фигуры.
ms.prod: xamarin
ms.assetid: 5CABB22B-C6BE-43D1-91D9-6E90A4BD5622
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f2ad0104ee7ea5b44df0c6c24ac20e750d1b1be0
ms.sourcegitcommit: 8f6cc5208f675c8cfb645bd9ffb0fc1f8ea71411
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/24/2020
ms.locfileid: "85326475"
---
# <a name="xamarinforms-shapes-fill-rules"></a>Xamarin.FormsФигуры: правила заливки

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Некоторые Xamarin.Forms классы фигур имеют `FillRule` свойства типа `FillRule` . К ним относятся `Polygon` , `Polyline` и `GeometryGroup` .

`FillRule`Перечисление определяет `EvenOdd` и `Nonzero` члены. Каждый элемент представляет другое правило для определения того, находится ли точка в области заливки фигуры.

> [!IMPORTANT]
> Все фигуры считаются закрытыми в целях правил заполнения.

## <a name="evenodd"></a>EvenOdd

`EvenOdd`Правило заполнения рисует луч с точки до бесконечности в любом направлении и подсчитывает количество сегментов в фигуре, пересекающих луч. Если это число нечетное, то точка находится внутри. Если это число четное, точка находится вне.

В следующем примере XAML создается и отрисовывается составная фигура со `FillRule` значением по умолчанию `EvenOdd` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
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
      StrokeThickness="1"
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

![Составная фигура с ненулевым правилом заполнения](fillrule-images/nonzero.png "Составная фигура с ненулевым правилом заполнения")

Обратите внимание, что в составной фигуре заполнены все кольца. Это связано с тем, что все сегменты выполняются в одном направлении, поэтому луч, нарисованный из любой точки, будет пересекать один или несколько сегментов, а сумма пересечений не будет равняться нулю:

![Составная фигура с заметками с ненулевым правилом заполнения](fillrule-images/nonzero-annotated.png "Составная фигура с заметками с ненулевым правилом заполнения")

На изображении над красными стрелками обозначает направление рисования сегментов, а черная стрелка — произвольный луч, выполняемый с точки во внутреннем кольце. Начиная с нуля, для каждого сегмента, который пересекает луч, значение 1 добавляется, так как сегмент пересекает луч слева направо.

Более сложная форма с сегментами, работающими в разных направлениях, необходима для лучшего демонстрации поведения `Nonzero` правила заполнения. В следующем примере XAML создается похожая форма для предыдущего примера, за исключением того, что она создается с помощью `PathGeometry` вместо `EllipseGeometry` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
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

![Составная фигура с ненулевым правилом заполнения](fillrule-images/nonzero-gaps.png "Составная фигура с ненулевым правилом заполнения")

На приведенном выше рисунке третья дуга от центра не заполнена. Это обусловлено тем, что сумма значений данного луча, пересекает сегменты в пути, равна нулю:

![Составная фигура с заметками с ненулевым правилом заполнения](fillrule-images/nonzero-gaps-annotated.png "Составная фигура с заметками с ненулевым правилом заполнения")

На рисунке выше красный кружок представляет точку, черные линии представляют произвольные лучи, которые выходят из точки в незаполненной области, а красные стрелки — направление рисования сегментов. Как можно видеть, сумма значений с лучей, пересечением сегментов, равна нулю:

- Произвольный луч, который перемещается по диагонали вправо, пересекает два сегмента, которые выполняются в разных направлениях. Таким образом, сегменты отменяют друг друга, предоставляя нулевое значение.
- Произвольный луч, который перемещается по диагонали влево, пересекают шесть сегментов. Однако пересечение отменяет друг друга, так что ноль является окончательной суммой.

Сумма, равная нулю, не заполняется кольцом.

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsМногоугольник](index.md)
