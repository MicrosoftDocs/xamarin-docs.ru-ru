---
title: 'Xamarin.FormsShapes: путь'
description: Xamarin.FormsКласс Path можно использовать для рисования кривых и сложных фигур.
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cb2e3ea68f181f0a6e6392c012db221ef35d65bd
ms.sourcegitcommit: 16847681df17ed59b3b3528761c02e8fb48ffc4f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104303"
---
# <a name="xamarinforms-shapes-path"></a>Xamarin.FormsShapes: путь

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Path`Класс является производным от `Shape` класса и может использоваться для рисования кривых и сложных фигур. Эти кривые и фигуры часто описываются с помощью `Geometry` объектов. Сведения о свойствах, которые `Path` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Path` определяет следующие свойства:

- `Data`Тип `Geometry` , который указывает рисуемую фигуру.
- `RenderTransform`Тип `Transform` , который представляет преобразование, применяемое к геометрии пути перед его прорисовкой.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

Дополнительные сведения о преобразованиях см. в разделе [ Xamarin.Forms преобразования пути](path-transforms.md).

## <a name="create-a-path"></a>Создание пути

В следующем примере XAML показано, как нарисовать многоугольник с помощью специального сокращенного синтаксиса, известного как синтаксис разметки пути:

```xaml
<Path Data="M 10,50 L 200,70"
      Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100" />
```

`Data`Строка начинается с команды "MoveTo", обозначенной параметром `M` , который устанавливает начальную точку для пути. `L`команда line, которая создает прямую линию от начальной точки до указанной конечной точки.

> [!NOTE]
> Синтаксис разметки пути доступен только в XAML.

Дополнительные сведения о синтаксисе разметки пути см. в разделе [ Xamarin.Forms синтаксис разметки пути](path-markup-syntax.md).

## <a name="path-geometry"></a>Геометрическая контур

Кривые и фигуры можно описать с помощью `Geometry` объектов, которые используются для задания `Path` `Data` свойства объекта.

Существует множество `Geometry` объектов для выбора. `EllipseGeometry`Классы, `LineGeometry` и `RectangleGeometry` описывают относительно простые фигуры. Чтобы создать более сложные фигуры или создать кривые, используйте `PathGeometry` .

`PathGeometry`объекты состоят из одного или нескольких `PathFigure` объектов. Каждый `PathFigure` объект представляет отдельную форму. Каждый `PathFigure` объект состоит из одного или нескольких `PathSegment` объектов, каждый из которых представляет часть фигуры соединения. Типы сегментов включают следующие: `LineSegment` , `BezierSegment` и `ArcSegment` .

В следующем примере `Path` для рисования треугольника используется.

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
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

Дополнительные сведения о геометрических фигурах см. в разделе [ Xamarin.Forms геометрические объекты](geometries.md).

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.FormsМногоугольник](index.md)
- [Xamarin.FormsОбъекты](geometries.md)
- [Xamarin.FormsСинтаксис разметки пути](path-markup-syntax.md)
- [Xamarin.FormsПреобразования пути](path-transforms.md)
