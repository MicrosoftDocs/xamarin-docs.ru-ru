---
title: 'Xamarin.FormsФигуры: многоугольник'
description: Xamarin.FormsКласс Polygon можно использовать для рисования многоугольников, Соединенных рядом линий, образующих замкнутые фигуры.
ms.prod: xamarin
ms.assetid: D6539F60-A5AC-46EF-86EB-E9F508EB1FA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 628218f47f318aebe7e4d0ff30efdce9617eb2c6
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937648"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.FormsФигуры: многоугольник

![API предварительного выпуска](~/media/shared/preview.png "Этот API-интерфейс сейчас доступен в предварительной версии.")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polygon`Класс является производным от `Shape` класса и может использоваться для рисования многоугольников, Соединенных рядом линий, образующих замкнутые фигуры. Сведения о свойствах, которые `Polygon` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Polygon` определяет следующие свойства:

- `Points`Тип `PointCollection` , представляющий собой коллекцию [`Point`](xref:Xamarin.Forms.Point) структур, описывающих точки вершин многоугольника.
- `FillRule`Тип `FillRule` , который указывает, как определяется внутреннее заполнение фигуры. Значение по умолчанию этого свойства равно `FillRule.EvenOdd`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

`PointsCollection`Тип является типом объекта `ObservableCollection` [`Point`](xref:Xamarin.Forms.Point) . `Point`Структура определяет `X` и `Y` свойства типа `double` , представляющие пару координат x и y в двухмерном пространстве. Таким образом, `Points` свойству следует присвоить список пар координат x и y, описывающих точки вершин многоугольников, разделенных одной запятой и/или одним или несколькими пробелами. Например, допустимыми являются значения 40, 10 70, 80 и 40 10, 70 80.

Дополнительные сведения о `FillRule` перечислении см. в разделе [ Xamarin.Forms фигуры: правила заливки](fillrules.md).

## <a name="create-a-polygon"></a>Создание многоугольника

Чтобы нарисовать многоугольник, создайте `Polygon` объект и присвойте его `Points` свойству вершины фигуры. Автоматически рисуется линия, соединяющая первую и последнюю точки. Чтобы закрасить внутри многоугольника, задайте `Fill` для его свойства значение [`Color`](xref:Xamarin.Forms.Color) . Чтобы задать контур многоугольника, задайте для его `Stroke` свойства значение [`Color`](xref:Xamarin.Forms.Color) . `StrokeThickness`Свойство определяет толщину контура многоугольника.

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

- [Шапедемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsМногоугольник](index.md)
- [Xamarin.FormsФигуры: правила заливки](fillrules.md)
