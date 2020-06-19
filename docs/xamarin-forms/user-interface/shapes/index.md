---
title: Xamarin.FormsМногоугольник
description: Xamarin.FormsФигуры — это типы представлений, которые позволяют рисовать фигуры на экране.
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6f8cb91a3699ce7e18e7f4d2e891fafa1ff552c2
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990756"
---
# <a name="xamarinforms-shapes"></a>Xamarin.FormsМногоугольник

![](~/media/shared/preview.png "This API is currently pre-release")

`Shape`— Это тип [`View`](xref:Xamarin.Forms.View) , который позволяет нарисовать форму на экране. `Shape`объекты могут использоваться внутри классов макета и большинства элементов управления, так как `Shape` класс является производным от `View` класса.

Xamarin.FormsФигуры доступны в `Xamarin.Forms.Shapes` пространстве имен в iOS, Android, macOS, универсальная платформа Windows (UWP) и Windows Presentation Foundation (WPF).

> [!IMPORTANT]
> Xamarin.FormsФигуры в настоящее время экспериментальны и могут использоваться только путем установки `Shapes_Experimental` флага. Дополнительные сведения см. в разделе [экспериментальные флаги](~/xamarin-forms/internals/experimental-flags.md).

`Shape` определяет следующие свойства:

- `Aspect`, типа `Stretch` , описывает, как фигура заполняет выделенное место. Значение по умолчанию этого свойства равно `Stretch.None`.
- `Fill`Тип `Color` — указывает цвет, используемый для заполнения внутренней фигуры.
- `Stroke`Тип — `Color` указывает цвет, используемый для рисования контура фигуры.
- `StrokeDashArray`Тип `DoubleCollection` , представляющий коллекцию `double` значений, которые указывают шаблон штрихов и пропусков, используемых для формирования контура фигуры.
- `StrokeDashOffset`, тип `double` , задает расстояние в шаблоне штриха, с которого начинается тире. Значение этого свойства по умолчанию равно 0,0.
- `StrokeLineCap`, типа `PenLineCap` , описывает фигуру в начале и в конце линии или сегмента. Значение по умолчанию этого свойства равно `PenLineCap.Flat`.
- `StrokeLineJoin`Тип `PenLineJoin` — задает тип объединения, используемый в вершинах фигуры. Значение по умолчанию этого свойства равно `PenLineJoin.Miter`.
- `StrokeThickness`Тип — `double` указывает ширину контура фигуры. Значение этого свойства по умолчанию равно 1,0.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

Xamarin.FormsОпределяет число объектов, производных от `Shape` класса. Это операторы `Ellipse`, `Line`, `Path`, `Polygon`, `Polyline` и `Rectangle`.

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
