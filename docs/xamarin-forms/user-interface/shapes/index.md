---
title: Xamarin.FormsМногоугольник
description: Xamarin.FormsФигуры — это типы представлений, которые позволяют рисовать фигуры на экране.
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 13c79d7597325a3bf8dbabfa2983d55a92309c4b
ms.sourcegitcommit: dc49ba58510eeb52048a866e5d3daf5f1f68fbd2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2020
ms.locfileid: "85130886"
---
# <a name="xamarinforms-shapes"></a>Xamarin.FormsМногоугольник

![](~/media/shared/preview.png "This API is currently pre-release")

`Shape`— Это тип [`View`](xref:Xamarin.Forms.View) , который позволяет нарисовать форму на экране. `Shape`объекты могут использоваться внутри классов макета и большинства элементов управления, так как `Shape` класс является производным от `View` класса.

Xamarin.FormsФигуры доступны в `Xamarin.Forms.Shapes` пространстве имен в iOS, Android, macOS, универсальная платформа Windows (UWP) и Windows Presentation Foundation (WPF).

> [!IMPORTANT]
> Xamarin.FormsФигуры в настоящее время экспериментальны и могут использоваться только путем установки `Shapes_Experimental` флага. Дополнительные сведения см. в разделе [экспериментальные флаги](~/xamarin-forms/internals/experimental-flags.md).

`Shape` определяет следующие свойства:

- `Aspect`, типа `Stretch` , описывает, как фигура заполняет выделенное место. Значение по умолчанию этого свойства равно `Stretch.None`.
- `Fill`Тип [`Color`](xref:Xamarin.Forms.Color) — указывает цвет, используемый для заполнения внутренней фигуры.
- `Stroke`Тип — [`Color`](xref:Xamarin.Forms.Color) указывает цвет, используемый для рисования контура фигуры.
- `StrokeDashArray`Тип `DoubleCollection` , представляющий коллекцию `double` значений, которые указывают шаблон штрихов и пропусков, используемых для формирования контура фигуры.
- `StrokeDashOffset`, тип `double` , задает расстояние в шаблоне штриха, с которого начинается тире. Значение этого свойства по умолчанию равно 0,0.
- `StrokeLineCap`, типа `PenLineCap` , описывает фигуру в начале и в конце линии или сегмента. Значение по умолчанию этого свойства равно `PenLineCap.Flat`.
- `StrokeLineJoin`Тип `PenLineJoin` — задает тип объединения, используемый в вершинах фигуры. Значение по умолчанию этого свойства равно `PenLineJoin.Miter`.
- `StrokeThickness`Тип — `double` указывает ширину контура фигуры. Значение этого свойства по умолчанию равно 1,0.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

Xamarin.FormsОпределяет число объектов, производных от `Shape` класса. Это —,,,, `Ellipse` `Line` `Path` `Polygon` `Polyline` и `Rectangle` .

## <a name="paint-shapes"></a>Заливка фигур

[`Color`](xref:Xamarin.Forms.Color)объекты используются для рисования фигур `Stroke` и `Fill` :

```xaml
<Ellipse Fill="DarkBlue"
         Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

В этом примере указываются штрих и Fill элемента `Ellipse` .

![Заливка фигур](images/ellipse.png "Заливка фигур")

> [!IMPORTANT]
> Если значение для параметра не указано [`Color`](xref:Xamarin.Forms.Color) `Stroke` или равно `StrokeThickness` 0, то граница вокруг фигуры не рисуется.

Дополнительные сведения о допустимых [`Color`](xref:Xamarin.Forms.Color) значениях см. [в Xamarin.Forms разделе цвета в ](~/xamarin-forms/user-interface/colors.md).

## <a name="stretch-shapes"></a>Растяжение фигур

`Shape`объекты имеют `Aspect` свойство типа `Stretch` . Это свойство определяет способ `Shape` растяжения содержимого объекта для заполнения `Shape` пространства макета объекта. `Shape`Пространство макета объекта — это объем пространства, `Shape` выделенного Xamarin.Forms системой макета, из-за явной `WidthRequest` `HeightRequest` настройки, или из-за `HorizontalOptions` `VerticalOptions` параметров и.

Перечисление `Stretch` определяет следующие члены:

- `None`, который указывает, что содержимое сохраняет свой исходный размер. Это значение по умолчанию для свойства `Shape.Aspect`.
- `Fill`, который указывает, что размер содержимого изменяется для заполнения измерений назначения. Пропорции не сохраняются.
- `Uniform`, который указывает, что размер содержимого изменяется в соответствии с размерами места назначения, сохраняя пропорции.
- `UniformToFill`Указывает, что размер содержимого изменяется для заполнения измерений назначения, сохраняя пропорции. Если пропорции целевого прямоугольника отличаются от пропорций источника, исходное содержимое обрезается в соответствии с размерами назначения.

В следующем коде XAML показано, как задать `Aspect` свойство:

```xaml
<Path Aspect="Uniform"
      Stroke="Yellow"
      StrokeThickness="1"
      Fill="Red"
      BackgroundColor="LightGray"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
    <Path.Data>
        <!-- Path data goes here -->
    </Path.Data>  
</Path>      
```

В этом примере `Path` объект рисует сердце. `Path`Свойства объекта и задаются `WidthRequest` `HeightRequest` в 100 единиц, независимых от устройства, а `Aspect` свойство имеет значение `Uniform` . В результате размер содержимого объекта изменяется в соответствии с размерами назначения, сохраняя пропорции.

![Растяжение фигур](images/aspect.png "Растяжение фигур")

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Цвета вXamarin.Forms](~/xamarin-forms/user-interface/colors.md)
