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
ms.openlocfilehash: fdab7d422040aee5a00b16bb1e301f917b0c4883
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243767"
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

## <a name="draw-dashed-shapes"></a>Рисование пунктирных фигур

`Shape`объекты имеют `StrokeDashArray` свойство типа `DoubleCollection` . Это свойство представляет коллекцию `double` значений, которые указывают шаблон штрихов и пробелов, используемых для формирования контура фигуры. `DoubleCollection`— Это `ObservableCollection` значение типа `double` . Каждый `double` в коллекции задает длину тире или зазора. Первый элемент в коллекции, расположенный по индексу 0, задает длину тире. Второй элемент в коллекции, расположенный по индексу 1, задает длину промежутка. Таким образом, объекты с четным значением индекса задают тире, а объекты с нечетным значением индекса указывают пробелы.

`Shape`объекты также имеют `StrokeDashOffset` свойство типа `double` , которое задает расстояние в шаблоне штриха, с которого начинается тире. Если установить это свойство не удается, будет `Shape` иметь сплошной контур.

Пунктирные фигуры можно нарисовать, установив `StrokeDashArray` `StrokeDashOffset` Свойства и. `StrokeDashArray`Свойству должно быть присвоено одно или несколько `double` значений, при этом каждая пара отделяется одной запятой и (или) одним или несколькими пробелами. Например, допустимыми являются значения "0,5 1,0" и "0,5, 1.0".

В следующем примере XAML показано, как нарисовать пунктирный прямоугольник:

```xaml
<Rectangle Fill="DarkBlue"
           Stroke="Red"
           StrokeThickness="4"
           StrokeDashArray="1,1"
           StrokeDashOffset="6"
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

В этом примере рисуется прямоугольник с пунктирным штрихом:

![Пунктирный прямоугольник](images/dashed-rectangle.png "Пунктирная линия")

## <a name="control-line-ends"></a>Управление концами строк

Линия состоит из трех частей: начало, текст строки и конечное ограничение. Начальная и конечная заглушки описывают фигуру в начале и в конце строки или сегмента.

`Shape`объекты имеют `StrokeLineCap` свойство типа `PenLineCap` , описывающее фигуру в начале и в конце строки или сегмент. Перечисление `PenLineCap` определяет следующие члены:

- `Flat`, представляющий наконечник, который не выходит за последнюю точку линии. Это сравнимо с отсутствием конца строки и значением свойства по умолчанию `StrokeLineCap` .
- `Square`, представляющий прямоугольник, высота которого равна толщине линии, и длина, равная половине толщины линии.
- `Round`, представляющий полукруг с диаметром, равным толщине линии.

> [!IMPORTANT]
> `StrokeLineCap`Свойство не оказывает никакого влияния, если оно задано для фигуры, не имеющей начальной или конечной точки. Например, это свойство не действует, если оно задано для `Ellipse` , или `Rectangle` .

В следующем коде XAML показано, как задать `StrokeLineCap` свойство:

```xaml
<Line X1="0"
      Y1="20"
      X2="300"
      Y2="20"
      StrokeLineCap="Round"
      Stroke="Red"
      StrokeThickness="12" />
```

В этом примере красная линия округляется в начале и в конце строки:

![Концы строк](images/linecap.png "Концы строк")

## <a name="control-line-joins"></a>Управление соединением линий

`Shape`объекты имеют `StrokeLineJoin` свойство типа `PenLineJoin` , которое указывает тип объединения, используемого на вершинах фигуры. Перечисление `PenLineJoin` определяет следующие члены:

- `Miter`, представляющий обычные угловые вершины. Это значение по умолчанию для свойства `StrokeLineJoin`.
- `Bevel`, представляющий багетные вершины.
- `Round`, представляющий скругленные вершины.

В следующем коде XAML показано, как задать `StrokeLineJoin` свойство:

```xaml
<Polyline Points="20 20,250 50,20 120"
          Stroke="DarkBlue"
          StrokeThickness="20"
          StrokeLineJoin="Round" />
```

В этом примере темно-синяя Ломаная линия имеет скругленные объединения на вершинах:

![Соединение линий](images/linejoin.png "Соединение линий")

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Цвета вXamarin.Forms](~/xamarin-forms/user-interface/colors.md)
