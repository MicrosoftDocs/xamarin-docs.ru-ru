---
title: Данные о пути SVG в SkiaSharp
description: В этой статье объясняется, как определить SkiaSharp пути с помощью текстовых строк в формате масштабируемой векторной графики и демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2571375e7ad28acbf367870b5c48e19d3a7525e7
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931291"
---
# <a name="svg-path-data-in-skiasharp"></a>Данные о пути SVG в SkiaSharp

[![Скачать пример](~/media/shared/download.png) Скачайте пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Определение путей с помощью текстовых строк в формате масштабируемой векторной графики_

[`SKPath`](xref:SkiaSharp.SKPath)Класс поддерживает определение всех объектов Path из текстовых строк в формате, установленном спецификацией масштабируемых векторных графики (SVG). Далее в этой статье показано, как можно представить весь путь, например, в текстовой строке:

![Образец пути, определенный с помощью данных пути SVG](path-data-images/pathdatasample.png)

SVG — это язык программирования графики на основе XML для веб-страниц. Поскольку в языке SVG необходимо разрешить определение путей в разметке, а не в ряде вызовов функций, стандарт SVG включает очень краткий способ указания всего графического контура в виде текстовой строки.

В SkiaSharp этот формат называется "SVG Path-Data". Формат также поддерживается в средах программирования на основе XAML в Windows, включая Windows Presentation Foundation и универсальная платформа Windows, где он называется [синтаксисом разметки пути](/dotnet/framework/wpf/graphics-multimedia/path-markup-syntax) или [синтаксисом команд Move и Draw](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Он также может использоваться в качестве формата обмена для векторных изображений, особенно в текстовых файлах, таких как XML.

[`SKPath`](xref:SkiaSharp.SKPath)Класс определяет два метода с словами `SvgPathData` в их именах:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Статический [`ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) метод преобразует строку в `SKPath` объект, а [`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData) `SKPath` объект преобразует в строку.

Ниже приведена SVG-строка для пяти звезд, центрированных в точке (0, 0) с радиусом 100:

```
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Буквы являются командами, которые создают `SKPath` объект: `M` обозначает `MoveTo` вызов, `L` имеет значение `LineTo` , и `Z` — `Close` закрытие контура. Каждая пара чисел предоставляет координату X и Y точки. Обратите внимание, что `L` за командой следуют несколько точек, разделенных запятыми. В ряде координат и точек запятые и пробелы обрабатываются одинаково. Некоторые программисты предпочитают добавлять запятые между координатами X и Y, а не между точками, но запятые или пробелы необходимы только для того, чтобы избежать неоднозначности. Это вполне допустимо:

```
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Синтаксис данных пути SVG формально описан в [разделе 8,3 спецификации SVG](https://www.w3.org/TR/SVG11/paths.html#PathData). Ниже приведено краткое описание.

## <a name="moveto"></a>**MoveTo**

```
M x y
```

Он начинает новый профиль в пути, устанавливая текущую точку. Данные пути должны всегда начинаться с `M` команды.

## <a name="lineto"></a>**LineTo**

```
L x y ...
```

Эта команда добавляет прямую линию (или линии) к пути и устанавливает новую текущую точку в конец последней строки. Можно выполнить `L` команду с несколькими парами координат *x* и *y* .

## <a name="horizontal-lineto"></a>**Горизонтально LineTo**

```
H x ...
```

Эта команда добавляет горизонтальную линию к контуру и устанавливает новую текущую точку в конец строки. Можно выполнить `H` команду с несколькими координатами *x* , но это не имеет смысла.

## <a name="vertical-line"></a>**Вертикальная линия**

```
V y ...
```

Эта команда добавляет вертикальную линию к контуру и устанавливает новую текущую точку в конец строки.

## <a name="close"></a>**Закрыть**

```
Z
```

`C`Команда закрывает профиль, добавляя прямую линию с текущей позицией в начало контура.

## <a name="arcto"></a>**аркто**

Команда для добавления эллиптической дуги к профилю — это самая сложная команда во всей спецификации данных Path SVG. Это единственная команда, в которой числа могут представлять нечто, отличное от значений координат.

```
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

Параметры *RX* и *дые* — это горизонтальные и вертикальные Радиусы эллипса. *Угол поворота* — по часовой стрелке в градусах.

Установите *флаг Large-Arc* в значение 1 для большой дуги или значение 0 для небольшой дуги.

Установите *флаг поворота* равным 1 в качестве значения по часовой стрелке и 0 для счетчика — по часовой стрелке.

Дуга рисуется до точки (*x*, *y*), которая становится новой текущей позицией.

## <a name="cubicto"></a>**кубикто**

```
C x1 y1 x2 y2 x3 y3 ...
```

Эта команда добавляет кривую Безье третьего порядка к текущему положению (*X3*, *Y3*), которая станет новой текущей позицией. Точки (*x1*, *Y1*) и (*x2*, *Y2*) являются контрольными точками.

Несколько кривых Безье можно указать с помощью одной `C` команды. Число точек должно быть кратным 3.

Имеется также гладкая команда кривой Безье:

```
S x2 y2 x3 y3 ...
```

Эта команда должна следовать обычной команде Безье (хотя это не является обязательным требованием). Команда Smooth Безье вычисляет первую контрольную точку таким образом, что она является отражением второй контрольной точки предыдущей Безье вокруг своей взаимной точки. Таким образом, эти три точки являются колинейными, а соединение между двумя кривыми Безье является гладким.

## <a name="quadto"></a>**куадто**

```
Q x1 y1 x2 y2 ...
```

Для квадратичных кривых Безье количество точек должно быть кратным 2. Контрольная точка — (*x1*, *Y1*), конечная точка (и Новая текущая позиции) — (*x2*, *Y2*).

Также существует команда гладкого квадратичного изгиба:

```
T x2 y2 ...
```

Контрольная точка вычисляется на основе контрольной точки предыдущей квадратичной кривой.

Все эти команды также доступны в относительных версиях, где точки координат зависят от текущей позиции. Эти относительные команды начинаются с строчных букв, например, `c` вместо `C` относительных версий команды кубической Безье.

Это экстент определения данных SVG Path. Не существует средства для повторяющихся групп команд или для выполнения вычислений любого типа. Команды для `ConicTo` или другие типы спецификаций Arc недоступны.

Статический [`SKPath.ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) метод принимает допустимую строку команд SVG. При обнаружении любой синтаксической ошибки метод возвращает значение `null` . Это единственное обозначение ошибки.

[`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData)Метод удобен для получения данных о контуре SVG из существующего `SKPath` объекта для перемещения в другую программу или для хранения в текстовом формате файла, например XML. ( `ToSvgPathData` Метод не показан в примере кода в этой статье.) *Не* `ToSvgPathData` следует возвращать строку, соответствующую только вызовам методов, создавшим путь. В частности, вы обнаружите, что дуги преобразуются в несколько `QuadTo` команд, и вот как они отображаются в данных пути, возвращаемых `ToSvgPathData` .

Страница « **Hello Data Path** » записывает слово «Hello» с использованием данных о пути SVG. Оба `SKPath` объекта и `SKPaint` определены как поля в [`PathDataHelloPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) классе:

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

Путь, определяющий текстовую строку, начинается с левого верхнего угла в точке (0, 0). Каждая буква имеет ширину 50 единиц и 100 единицы измерения, а буквы разделяются на 25 единиц, что означает, что весь путь имеет ширину 350 единиц.

"H" для "Hello" состоит из 3 1-линейных демонстраций, а "E" — две соединенные кривые Безье третьего порядка. Обратите внимание, что `C` за командой следуют шесть точек, а две контрольных точки имеют координаты y — 10 и 110, что помещает их за пределы диапазона координат y других букв. "L" — две соединенные линии, а "O" — эллипс, отображаемый с помощью `A` команды.

Обратите внимание, что `M` команда, которая начинает последний профиль, задает позицию в точке (350, 50), которая является вертикальной центральной точкой слева от "O". Как показывает первые цифры, следующие за `A` командой, эллипс имеет горизонтальный радиус 25 и вертикальный радиус 50. Конечная точка обозначается последней парой чисел в `A` команде, которая представляет точку (300, 49,9). Это намеренно немного отличается от начальной точки. Если конечная точка установлена равна начальной точке, дуга не будет подготовлена к просмотру. Чтобы нарисовать полный эллипс, необходимо установить конечную точку в значение (но не равно) начальную точку или использовать две или более `A` команды, каждая для части полного эллипса.

Может потребоваться добавить следующую инструкцию в конструктор страницы, а затем задать точку останова для проверки результирующей строки:

```csharp
string str = helloPath.ToSvgPathData();
```

Вы обнаружите, что дуга заменена длинной серией `Q` команд для поэтапного приближения дуги с использованием квадратичных кривых Безье.

`PaintSurface`Обработчик получает ограниченные границы пути, которые не включают контрольные точки для кривых "E" и "O". Три преобразования перемещают центр пути к точке (0, 0), масштабируют путь до размера холста (но при этом учитывается ширина штриха), а затем перемещается центр контура в центр холста:

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

Путь заполняет холст, который выглядит более разумным при просмотре в альбомном режиме:

[![Тройной снимок экрана страницы "Hello Data Path"](path-data-images/pathdatahello-small.png)](path-data-images/pathdatahello-large.png#lightbox "Тройной снимок экрана страницы "Hello Data Path"")

Страница **Data Cat** выглядит аналогично. Объекты Path и Paint определены как поля в [`PathDataCatPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) классе:

```csharp
public class PathDataCatPage : ContentPage
{
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

В качестве заголовка Cat используется окружность, и в нем отображается две `A` команды, каждая из которых рисует полукруг. Обе `A` команды для Head определяют горизонтальный и вертикальный радиусы 100. Первая Дуга начинается в (240, 100) и заканчивается в (240, 300), которая станет начальной точкой для второй дуги, которая заканчивается обратно (240, 100).

Эти два глаза также отображаются с двумя `A` командами, как и в головке Cat, вторая `A` команда заканчивается в той же точке, что и начало первой `A` команды. Однако эти пары `A` команд не определяют эллипс. Для каждой дуги используется 40 единиц, а радиус — также 40 единиц. Это означает, что эти дуги не являются полными полукругами.

`PaintSurface`Обработчик выполняет аналогичные преобразования, как в предыдущем примере, но устанавливает один `Scale` фактор для сохранения пропорций и предоставляет небольшое поле, чтобы блочнойность не намерена прокасаться сторон на экране.

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

Вот работающая программа:

[![Тройной снимок экрана страницы "данные пути"](path-data-images/pathdatacat-small.png)](path-data-images/pathdatacat-large.png#lightbox "Тройной снимок экрана страницы "данные пути"")

Как правило, если `SKPath` объект определен как поле, то в конструкторе или другом методе должны быть определены контуры пути. Однако при использовании данных контура SVG вы видели, что путь может быть указан полностью в определении поля.

В примере ранее **некрасивого аналогового часов** в статье [**вращение преобразования**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) показаны руки часов в виде простых линий. Программа **довольно аналоговых часов** , приведенная ниже, заменяет эти строки `SKPath` объектами, определенными как поля в [`PrettyAnalogClockPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) классе вместе с `SKPaint` объектами.

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

В час и минуту у руки теперь есть заключенные в них области. Чтобы сделать эти руки разными, они рисуются черным контуром и серой заливкой с помощью `handStrokePaint` `handFillPaint` объектов и.

В предыдущем примере **некрасивого аналогового времени** небольшие круги, помеченные часами и минутами, были прорисованы в цикле. В этом **простом** примере используется совершенно другой подход: часы и минуты — это пунктирные линии, нарисованные с помощью `minuteMarkPaint` объектов и `hourMarkPaint` :

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

В статье " [**точки и тире**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) " обсуждалось, как можно использовать [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash*) метод для создания пунктирной линии. Первый аргумент — это `float` массив, который обычно содержит два элемента: первый элемент — это длина дефисов, второй элемент — разрыв между тире. Если `StrokeCap` свойство имеет значение `SKStrokeCap.Round` , скругленные концы штриха фактически дополняют длину тире на толщину штриха на обеих сторонах тире. Поэтому при установке первого элемента массива равным 0 создается пунктирная линия.

Расстояние между этими точками регулируется вторым элементом массива. Вскоре вы увидите, что эти два `SKPaint` объекта используются для рисования кругов с радиусом 90 единиц. Таким образом, длина окружности этого круга составляет 180 π. Это означает, что 60 минут должны находиться в каждом 3π единицах, которое является вторым значением в `float` массиве `minuteMarkPaint` . 12-часовые метки должны находиться в каждой единице 15π, которая является значением во втором `float` массиве.

`PrettyAnalogClockPage`Класс устанавливает таймер на недействительность поверхности каждые 16 миллисекунд, а `PaintSurface` обработчик вызывается с такой частотой. Более ранние определения `SKPath` объектов и `SKPaint` позволяют выполнять очень чистый код рисования:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

Однако что-то особенно сделано с другой рукой. Так как часы обновляются каждые 16 миллисекунд, `Millisecond` свойство `DateTime` значения может быть использовано для анимации второй руки, а не для перемещения в дискретных переходах с секунды на секунду. Но этот код не допускает плавность перемещения. Вместо этого он использует Xamarin.Forms [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) функции плавности анимации и для другого рода движения. Эти функции плавности приводят к тому, что вторая рука перемещается в жеркиерм виде &mdash; , а затем немного перемещается в место назначения, что, увы, не может быть воспроизведено в этих статических снимках экрана:

[![Тройной снимок экрана со страницей очень аналоговых часов](path-data-images/prettyanalogclock-small.png)](path-data-images/prettyanalogclock-large.png#lightbox "Тройной снимок экрана со страницей очень аналоговых часов")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
