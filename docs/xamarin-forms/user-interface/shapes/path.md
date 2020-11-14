---
title: 'Xamarin.Forms Shapes: путь'
description: Xamarin.FormsКласс Path можно использовать для рисования кривых и сложных фигур.
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1ba44903f20d0431c27f2d49429b2feb9c9a51e0
ms.sourcegitcommit: f920ac0724f09e5c9b4f36be1995a5a17a6d9f95
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2020
ms.locfileid: "94591064"
---
# <a name="no-locxamarinforms-shapes-path"></a>Xamarin.Forms Shapes: путь

![Предварительный выпуск API](~/media/shared/preview.png)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Path`Класс является производным от `Shape` класса и может использоваться для рисования кривых и сложных фигур. Эти кривые и фигуры часто описываются с помощью `Geometry` объектов. Сведения о свойствах, которые `Path` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Path` определяет следующие свойства:

- `Data`Тип `Geometry` , который указывает рисуемую фигуру.
- `RenderTransform`Тип `Transform` , который представляет преобразование, применяемое к геометрии пути перед его прорисовкой.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

Дополнительные сведения о преобразованиях см. в разделе [ Xamarin.Forms преобразования пути](path-transforms.md).

## <a name="create-a-path"></a>Создание пути

Чтобы нарисовать путь, создайте `Path` объект и задайте его `Data` свойство. Существует два способа задания `Data` Свойства:

- Можно задать строковое значение для `Data` в XAML, используя синтаксис разметки пути. При таком подходе `Path.Data` значение использует формат сериализации для графики. Как правило, это строковое значение не редактируется вручную после создания. Вместо этого используйте средства проектирования для управления данными и их экспорта в виде строкового фрагмента, который может использоваться `Data` свойством.
- Можно задать `Data` для свойства `Geometry` объект. Это может быть конкретный `Geometry` объект или, `GeometryGroup` который выступает в качестве контейнера, который может объединять несколько объектов Geometry в один объект.

### <a name="create-a-path-with-path-markup-syntax"></a>Создание пути с синтаксисом разметки пути

В следующем примере XAML показано, как нарисовать треугольник с помощью синтаксиса разметки пути:

```xaml
<Path Data="M 10,100 L 100,100 100,50Z"
      Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start" />
```

`Data`Строка начинается с команды Move, обозначенной параметром `M` , который устанавливает абсолютную начальную точку для пути. `L` команда line, которая создает прямую линию от начальной точки до указанной конечной точки. `Z` Команда Close, которая создает линию, соединяющую текущую точку с начальной точкой. Результат представляет собой треугольник:

![Треугольник пути](path-images/triangle.png "Треугольник пути")

Дополнительные сведения о синтаксисе разметки пути см. в разделе [ Xamarin.Forms синтаксис разметки пути](path-markup-syntax.md).

### <a name="create-a-path-with-geometry-objects"></a>Создание пути с геометрическими объектами

Кривые и фигуры можно описать с помощью `Geometry` объектов, которые используются для задания `Path` `Data` свойства объекта. Существует множество `Geometry` объектов для выбора. `EllipseGeometry`Классы, `LineGeometry` и `RectangleGeometry` описывают относительно простые фигуры. Чтобы создать более сложные фигуры или создать кривые, используйте `PathGeometry` .

`PathGeometry` объекты состоят из одного или нескольких `PathFigure` объектов. Каждый `PathFigure` объект представляет отдельную форму. Каждый `PathFigure` объект состоит из одного или нескольких `PathSegment` объектов, каждый из которых представляет часть фигуры соединения. Типы сегментов включают следующие `LineSegment` `BezierSegment` классы, и `ArcSegment` .

В следующем примере XAML показано, как нарисовать треугольник с помощью `PathGeometry` объекта:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
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

В этом примере начальная точка треугольника — (10 100). Сегмент линии рисуется от (10 100) до (100 100) и от (100 100) до (100, 50). Затем соединяются первый и последний сегменты, так как `PathFigure.IsClosed` свойство имеет значение `true` . Результат представляет собой треугольник:

![Треугольник пути](path-images/triangle.png "Треугольник пути")

Дополнительные сведения о геометрических фигурах см. в разделе [ Xamarin.Forms геометрические объекты](geometries.md).

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Многоугольник](index.md)
- [Xamarin.Forms Объекты](geometries.md)
- [Xamarin.Forms Синтаксис разметки пути](path-markup-syntax.md)
- [Xamarin.Forms Преобразования пути](path-transforms.md)
