---
title: 'Xamarin.Forms Фигуры: линия'
description: Xamarin.FormsКласс Line можно использовать для рисования линий.
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 78425617d95758d3663cb9f2c5ac9bebb463bd68
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374762"
---
# <a name="no-locxamarinforms-shapes-line"></a>Xamarin.Forms Фигуры: линия

![Предварительный выпуск API](~/media/shared/preview.png)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Line`Класс является производным от `Shape` класса и может использоваться для рисования линий. Сведения о свойствах, которые `Line` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Line` определяет следующие свойства:

- `X1`тип Double Указывает координату x начальной точки линии. Значение этого свойства по умолчанию равно 0,0.
- `Y1`тип Double Указывает координату y начальной точки линии. Значение этого свойства по умолчанию равно 0,0.
- `X2`тип Double Указывает координату x конечной точки линии. Значение этого свойства по умолчанию равно 0,0.
- `Y2`тип Double Указывает координату y конечной точки линии. Значение этого свойства по умолчанию равно 0,0.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

Дополнительные сведения об управлении прорисовкой концов линий см. в разделе [Управление завершением строк](index.md#control-line-ends).

## <a name="create-a-line"></a>Создание линии

Чтобы нарисовать линию, создайте `Line` объект и задайте его `X1` Свойства и `Y1` для его начальной точки, а `X2` Свойства и — `Y` для конечной точки. Кроме того, присвойте `Stroke` свойству значение, [`Color`](xref:Xamarin.Forms.Color) поскольку линия без штриха невидима.

> [!NOTE]
> Установка `Fill` свойства объекта `Line` не оказывает влияния, так как линия не имеет внутренней части.

В следующем примере XAML показано, как нарисовать линию:

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="Red"
      StrokeThickness="1" />
```

В этом примере красная диагональная линия отрисовывается от (40, 0) до (0120):

![Line](line-images/line.png "Линия")

Поскольку `X1` свойства, `Y1` , `X2` и `Y2` имеют значения по умолчанию 0, можно нарисовать некоторые строки с минимальным синтаксисом:

```xaml
<Line Stroke="Red"
      StrokeThickness="1"
      X2="200" />
```

В этом примере определена горизонтальная линия, которая имеет длину не зависящее от устройства 200 единиц. Поскольку другие свойства по умолчанию равны 0, линия рисуется от (0, 0) до (200, 0).

В следующем примере XAML показано, как нарисовать пунктирную линию:

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeThickness="1"
      StrokeDashArray="1,1"
      StrokeDashOffset="6" />
```

В этом примере темно-синяя пунктирная диагональная линия отрисовывается от (40, 0) до (0120):

![Пунктирная линия](line-images/dashed-line.png "Пунктирная линия")

Дополнительные сведения о рисовании пунктирной линии см. в разделе [Рисование пунктирных фигур](index.md#draw-dashed-shapes).

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Многоугольник](index.md)