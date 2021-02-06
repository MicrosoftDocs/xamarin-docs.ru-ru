---
title: 'Xamarin.Forms Фигуры: линия'
description: Xamarin.FormsКласс Line можно использовать для рисования линий.
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/05/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 962800c5deb546ddf6a74d1f52ffaf60734ae463
ms.sourcegitcommit: 06701714021545eb5e932847829b876082194ffc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "99585849"
---
# <a name="xamarinforms-shapes-line"></a>Xamarin.Forms Фигуры: линия

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

Чтобы нарисовать линию, создайте `Line` объект и задайте его `X1` Свойства и `Y1` для его начальной точки, а `X2` Свойства и — `Y` для конечной точки. Кроме того, присвойте его `Stroke` свойству [`Brush`](xref:Xamarin.Forms.Brush) производный объект, так как линия без штриха невидима. Дополнительные сведения об `Brush` объектах см. в разделе [ Xamarin.Forms кисти](~/xamarin-forms/user-interface/brushes/index.md).

> [!NOTE]
> Установка `Fill` свойства объекта `Line` не оказывает влияния, так как линия не имеет внутренней части.

В следующем примере XAML показано, как нарисовать линию:

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="Red" />
```

В этом примере красная диагональная линия отрисовывается от (40, 0) до (0120):

![Диагональная линия](line-images/line.png "Линия")

Поскольку `X1` свойства, `Y1` , `X2` и `Y2` имеют значения по умолчанию 0, можно нарисовать некоторые строки с минимальным синтаксисом:

```xaml
<Line Stroke="Red"
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
      StrokeDashArray="1,1"
      StrokeDashOffset="6" />
```

В этом примере темно-синяя пунктирная диагональная линия отрисовывается от (40, 0) до (0120):

![Пунктирная линия](line-images/dashed-line.png "Пунктирная линия")

Дополнительные сведения о рисовании пунктирной линии см. в разделе [Рисование пунктирных фигур](index.md#draw-dashed-shapes).

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Многоугольник](index.md)
- [Кисти Xamarin.Forms](~/xamarin-forms/user-interface/brushes/index.md)
