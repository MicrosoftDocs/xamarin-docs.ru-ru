---
title: 'Xamarin.Forms Фигуры: многоугольник'
description: Xamarin.FormsКласс Polygon можно использовать для рисования многоугольников, Соединенных рядом линий, образующих замкнутые фигуры.
ms.prod: xamarin
ms.assetid: D6539F60-A5AC-46EF-86EB-E9F508EB1FA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/05/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2928e3bd32d7dd2722f01ad5a51dbaab127afd77
ms.sourcegitcommit: 06701714021545eb5e932847829b876082194ffc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "99585888"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.Forms Фигуры: многоугольник

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polygon`Класс является производным от `Shape` класса и может использоваться для рисования многоугольников, Соединенных рядом линий, образующих замкнутые фигуры. Сведения о свойствах, которые `Polygon` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Polygon` определяет следующие свойства:

- `Points`Тип `PointCollection` , представляющий собой коллекцию [`Point`](xref:Xamarin.Forms.Point) структур, описывающих точки вершин многоугольника.
- `FillRule`Тип `FillRule` , который указывает, как определяется внутреннее заполнение фигуры. Значение по умолчанию этого свойства равно `FillRule.EvenOdd`.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

`PointsCollection`Тип является типом объекта `ObservableCollection` [`Point`](xref:Xamarin.Forms.Point) . `Point`Структура определяет `X` и `Y` свойства типа `double` , представляющие пару координат x и y в двухмерном пространстве. Таким образом, `Points` свойству следует присвоить список пар координат x и y, описывающих точки вершин многоугольников, разделенных одной запятой и/или одним или несколькими пробелами. Например, допустимыми являются значения 40, 10 70, 80 и 40 10, 70 80.

Дополнительные сведения о `FillRule` перечислении см. в разделе [ Xamarin.Forms фигуры: правила заливки](fillrules.md).

## <a name="create-a-polygon"></a>Создание многоугольника

Чтобы нарисовать многоугольник, создайте `Polygon` объект и присвойте его `Points` свойству вершины фигуры. Автоматически рисуется линия, соединяющая первую и последнюю точки. Чтобы закрасить внутри многоугольника, присвойте его `Fill` свойству [`Brush`](xref:Xamarin.Forms.Brush) производный объект. Чтобы задать контур многоугольника, присвойте его `Stroke` свойству [`Brush`](xref:Xamarin.Forms.Brush) производный объект. `StrokeThickness`Свойство определяет толщину контура многоугольника. Дополнительные сведения об `Brush` объектах см. в разделе [ Xamarin.Forms кисти](~/xamarin-forms/user-interface/brushes/index.md).

В следующем примере XAML показано, как нарисовать закрашенный многоугольник.

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5" />
```

В этом примере рисуется закрашенный многоугольник, представляющий треугольник:

![Закрашенный многоугольник](polygon-images/filled.png "Закрашенный многоугольник")

В следующем примере XAML показано, как нарисовать пунктирные многоугольники.

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         StrokeDashArray="1,1"
         StrokeDashOffset="6" />
```

В этом примере контур многоугольника имеет штрих-пунктир:

![Пунктирный многоугольник](polygon-images/dashed.png "Пунктирный многоугольник")

Дополнительные сведения о рисовании пунктирных многоугольников см. в разделе [Рисование пунктирных фигур](index.md#draw-dashed-shapes).

В следующем примере XAML показан многоугольник, использующий правило заполнения по умолчанию:

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Blue"
         Stroke="Red"
         StrokeThickness="3" />
```

В этом примере поведение заливки каждого многоугольника определяется с помощью `EvenOdd` правила заполнения.

![EvenOdd, многоугольник](polygon-images/evenodd.png "EvenOdd, многоугольник")

В следующем примере XAML показан многоугольник, использующий `Nonzero` правило заполнения:

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Black"
         FillRule="Nonzero"
         Stroke="Yellow"
         StrokeThickness="3" />
```

![Ненулевое значение многоугольника](polygon-images/nonzero.png "Ненулевое значение многоугольника")

В этом примере поведение заливки каждого многоугольника определяется с помощью `Nonzero` правила заполнения.

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Многоугольник](index.md)
- [Xamarin.Forms Фигуры: правила заливки](fillrules.md)
- [Кисти Xamarin.Forms](~/xamarin-forms/user-interface/brushes/index.md)
