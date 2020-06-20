---
title: 'Xamarin.FormsФигуры: ломаная линия'
description: Xamarin.FormsКласс ломаной линии может использоваться для рисования ряда Соединенных прямых линий.
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b6c884caa7bfb301f949f353f3c6c3445704aa89
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101354"
---
# <a name="xamarinforms-shapes-polyline"></a>Xamarin.FormsФигуры: ломаная линия

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Polyline`Класс является производным от `Shape` класса и может использоваться для рисования ряда Соединенных прямых линий. Сведения о свойствах, которые `Polyline` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Polyline` определяет следующие свойства:

- `Points`Тип `PointCollection` , представляющий собой коллекцию `Point` структур, описывающих точки вершин ломаной линии.
- `FillRule`Тип `FillRule` , который определяет способ объединения пересекающихся областей в ломаной линии. Значение по умолчанию этого свойства равно `FillRule.EvenOdd`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

## <a name="create-a-polyline"></a>Создание ломаной линии

В следующем примере XAML показано, как нарисовать многоугольник:

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          HeightRequest="100"
          WidthRequest="500" />
```

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.FormsМногоугольник](index.md)
