---
title: Сведения о пути и перечисление
description: В этой статье объясняется, как получить сведения о путях SkiaSharp и перечислить содержимое, а также демонстрируется пример кода.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cf9ebb819d5b424963170d563575c4900bbed28b
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556364"
---
# <a name="path-information-and-enumeration"></a>Сведения о пути и перечисление

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Получение сведений о путях и перечисление содержимого_

[`SKPath`](xref:SkiaSharp.SKPath)Класс определяет несколько свойств и методов, позволяющих получить сведения о пути. [`Bounds`](xref:SkiaSharp.SKPath.Bounds)Свойства и [`TightBounds`](xref:SkiaSharp.SKPath.TightBounds) (и связанные методы) получают метрики пути. [`Contains`](xref:SkiaSharp.SKPath.Contains(System.Single,System.Single))Метод позволяет определить, находится ли конкретная точка в пределах пути.

Иногда бывает полезно определить общую длину всех линий и кривых, составляющих путь. Вычисление этой длины не является алгоритмически простой задачей, поэтому [`PathMeasure`](xref:SkiaSharp.SKPathMeasure) на нее помещается весь класс с именем.

Также иногда бывает полезно получить все операции рисования и точки, составляющие путь. Сначала это может показаться ненужным: Если программа создала путь, программа уже знает содержимое. Однако вы видели, что пути можно также создавать с помощью [эффектов пути](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) и путем преобразования [текстовых строк в пути](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Вы также можете получить все операции рисования и точки, составляющие эти пути. Одна из возможных действий — применение алгоритмного преобразования ко всем точкам, например, для заключения текста вокруг полушария:

![Текст, заключенный в полушарие](information-images/pathenumerationsample.png)

## <a name="getting-the-path-length"></a>Получение длины пути

В статье « [**пути и текст**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) » вы узнали, как использовать [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) метод для отрисовки текстовой строки, базовая папка которой соответствует пути. Но что делать, если нужно изменить размер текста так, чтобы он точно соответствовал пути? Рисование текста вокруг окружности несложно, так как окружность окружности легко вычислить. Но длина окружности эллипса или длины кривой Безье не настолько проста.

Этот [`SKPathMeasure`](xref:SkiaSharp.SKPathMeasure) класс может помочь. [Конструктор](xref:SkiaSharp.SKPathMeasure.%23ctor(SkiaSharp.SKPath,System.Boolean,System.Single)) принимает `SKPath` аргумент, и [`Length`](xref:SkiaSharp.SKPathMeasure.Length) свойство раскрывает его длину.

Этот класс показан в примере **длины пути** , который основан на странице **кривой Безье** . Файл [**пасленгспаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) является производным от `InteractivePage` и включает интерфейс Touch:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Curves.PathLengthPage"
                       Title="Path Length">
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</local:InteractivePage>
```

Файл кода программной части [**PathLengthPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) позволяет перемещать четыре сенсорные точки для определения конечных точек и контрольных точек кривой Безье третьего порядка. Три поля определяют текстовую строку, `SKPaint` объект и вычисляемую ширину текста:

```csharp
public partial class PathLengthPage : InteractivePage
{
    const string text = "Compute length of path";

    static SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Black,
        TextSize = 10,
    };

    static readonly float baseTextWidth = textPaint.MeasureText(text);
    ...
}
```

`baseTextWidth`Поле является шириной текста в зависимости от `TextSize` значения, равного 10.

`PaintSurface`Обработчик формирует кривую Безье, а затем изменяет размер текста в соответствии с его полной длиной:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);

        // Get path length
        SKPathMeasure pathMeasure = new SKPathMeasure(path, false, 1);

        // Find new text size
        textPaint.TextSize = pathMeasure.Length / baseTextWidth * 10;

        // Draw text on path
        canvas.DrawTextOnPath(text, path, 0, 0, textPaint);
    }
    ...
}
```

`Length`Свойство созданного `SKPathMeasure` объекта получает длину пути. Длина пути делится на `baseTextWidth` значение (ширина текста зависит от размера текста, равного 10), а затем умножается на базовый размер текста, равный 10. В результате получается новый размер текста для отображения текста по этому пути:

[![Тройной снимок экрана страницы длины пути](information-images/pathlength-small.png)](information-images/pathlength-large.png#lightbox "Тройной снимок экрана страницы длины пути")

Так как кривая Безье становится больше или короче, можно увидеть изменение размера текста.

## <a name="traversing-the-path"></a>Обход пути

`SKPathMeasure` можно сделать больше, чем просто измерять длину пути. Для любого значения в диапазоне от нуля до длины пути `SKPathMeasure` объект может получить позицию на пути, а тангенс — к кривой пути в этой точке. Тангенс доступен в виде вектора в виде `SKPoint` объекта или вращения, инкапсулированного в `SKMatrix` объекте. Ниже приведены способы `SKPathMeasure` получения этих сведений различными и гибкими способами.

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

Члены [`SKPathMeasureMatrixFlags`](xref:SkiaSharp.SKPathMeasureMatrixFlags) перечисления:

- `GetPosition`
- `GetTangent`
- `GetPositionAndTangent`

Страница **половинной черты уницикле** анимирует фигуру на уницикле, который кажется пере в кривую Безье третьего порядка:

[![Тройной снимок экрана страницы Уницикле половина канала](information-images/unicyclehalfpipe-small.png)](information-images/unicyclehalfpipe-large.png#lightbox "Тройной снимок экрана страницы Уницикле половина канала")

`SKPaint`Объект, используемый для обводки половинной черты и уницикле, определяется как поле в `UnicycleHalfPipePage` классе. Также определяется `SKPath` объект для уницикле:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };

    SKPath unicyclePath = SKPath.ParseSvgPathData(
        "M 0 0" +
        "A 25 25 0 0 0 0 -50" +
        "A 25 25 0 0 0 0 0 Z" +
        "M 0 -25 L 0 -100" +
        "A 15 15 0 0 0 0 -130" +
        "A 15 15 0 0 0 0 -100 Z" +
        "M -25 -85 L 25 -85");
    ...
}
```

Класс содержит стандартные переопределения `OnAppearing` `OnDisappearing` методов и для анимации. `PaintSurface`Обработчик создает путь для половины канала и затем рисует его. `SKPathMeasure`Затем создается объект на основе этого пути:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath pipePath = new SKPath())
        {
            pipePath.MoveTo(50, 50);
            pipePath.CubicTo(0, 1.25f * info.Height,
                             info.Width - 0, 1.25f * info.Height,
                             info.Width - 50, 50);

            canvas.DrawPath(pipePath, strokePaint);

            using (SKPathMeasure pathMeasure = new SKPathMeasure(pipePath))
            {
                float length = pathMeasure.Length;

                // Animate t from 0 to 1 every three seconds
                TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
                float t = (float)(timeSpan.TotalSeconds % 5 / 5);

                // t from 0 to 1 to 0 but slower at beginning and end
                t = (float)((1 - Math.Cos(t * 2 * Math.PI)) / 2);

                SKMatrix matrix;
                pathMeasure.GetMatrix(t * length, out matrix,
                                      SKPathMeasureMatrixFlags.GetPositionAndTangent);

                canvas.SetMatrix(matrix);
                canvas.DrawPath(unicyclePath, strokePaint);
            }
        }
    }
}
```

`PaintSurface`Обработчик вычисляет значение `t` , которое переходит от 0 к 1 каждые пять секунд. Затем она использует `Math.Cos` функцию для преобразования этого значения в `t` диапазон от 0 до 1 и обратно в 0, где 0 соответствует уницикле в начале сверху слева, а 1 соответствует уницикле в правом верхнем углу. Функция косинус приводит к самой медленной скорости в верхней части канала и от самой быстрой в нижней части.

Обратите внимание, что значение `t` должно быть умножено на длину пути для первого аргумента `GetMatrix` . Затем матрица применяется к `SKCanvas` объекту для рисования пути уницикле.

## <a name="enumerating-the-path"></a>Перечисление пути

Два внедренных класса `SKPath` позволяют перечислить содержимое пути. Эти классы являются [`SKPath.Iterator`](xref:SkiaSharp.SKPath.Iterator) и [`SKPath.RawIterator`](xref:SkiaSharp.SKPath.RawIterator) . Эти два класса очень похожи, но `SKPath.Iterator` могут удалить элементы в пути с нулевой длиной или близкие к нулевой длине. `RawIterator`Используется в примере ниже.

Объект типа можно получить `SKPath.RawIterator` , вызвав [`CreateRawIterator`](xref:SkiaSharp.SKPath.CreateRawIterator) метод `SKPath` . Перечисление по пути достигается путем многократного вызова [`Next`](xref:SkiaSharp.SKPath.RawIterator.Next*) метода. Передайте ему массив из четырех `SKPoint` значений:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next`Метод возвращает член [`SKPathVerb`](xref:SkiaSharp.SKPathVerb) типа перечисления. Эти значения указывают на определенную команду рисования в пути. Количество допустимых точек, вставляемых в массив, зависит от этой команды:

- `Move` с одной точкой
- `Line` с двумя точками
- `Cubic` с четырьмя точками
- `Quad` с тремя точками
- `Conic` с тремя точками (и также вызовом [`ConicWeight`](xref:SkiaSharp.SKPath.RawIterator.ConicWeight*) метода для веса);
- `Close` с одной точкой
- `Done`

`Done`Команда указывает, что перечисление пути завершено.

Обратите внимание, что `Arc` команд нет. Это означает, что все дуги преобразуются в кривые Безье при добавлении к пути.

Некоторые сведения в `SKPoint` массиве являются избыточными. Например, если `Move` за командой следует `Line` глагол, то первая из двух точек, сопровождающих объект, является той же, что и `Line` `Move` точка. На практике эта избыточность очень полезна. При получении `Cubic` команды она сопровождается всеми четырьмя точками, определяющими кривую Безье третьего порядка. Не нужно хранить текущую точку, установленную предыдущей командой.

Однако команда, вызывающая проблемы, имеет значение `Close` . Эта команда рисует прямую линию от текущей позицией до начала контура, установленного ранее `Move` командой. В идеале `Close` команда должна предоставлять эти две точки, а не только одну точку. Что еще хуже, точка, сопровождающая `Close` глагол, всегда имеет значение (0, 0). При перечислении по пути, вероятно, потребуется хранить `Move` точку и текущую позицию.

## <a name="enumerating-flattening-and-malforming"></a>Перечисление, сведение и Малформинг

Иногда желательно применить алгоритмическое преобразование к пути, чтобы малформ его каким-либо образом:

![Текст, заключенный в полушарие](information-images/pathenumerationsample.png)

Большая часть этих букв состоит из прямых линий, но эти прямые линии, очевидно, были изогнуты на кривые. Как это возможно?

Ключ состоит в том, что оригинальные прямые линии разбиваются на ряд меньших прямых линий. Эти отдельные небольшие прямые линии можно манипулировать различными способами, образуя кривую.

Чтобы упростить этот процесс, пример [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) содержит статический [`PathExtensions`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) класс с `Interpolate` методом, который разделяет прямую линию на несколько коротких строк, которые имеют длину только одну единицу. Кроме того, класс содержит несколько методов, которые преобразуют три типа кривых Безье в ряд маленьких прямых линий, приближенных к кривой. (Параметрической формулы представлены в статье [**три типа кривых Безье**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) Этот процесс называется _плоской_ кривой:

```csharp
static class PathExtensions
{
    ...
    static SKPoint[] Interpolate(SKPoint pt0, SKPoint pt1)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * pt0.X + t * pt1.X;
            float y = (1 - t) * pt0.Y + t * pt1.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenCubic(SKPoint pt0, SKPoint pt1, SKPoint pt2, SKPoint pt3)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2) + Length(pt2, pt3));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * (1 - t) * pt0.X +
                        3 * t * (1 - t) * (1 - t) * pt1.X +
                        3 * t * t * (1 - t) * pt2.X +
                        t * t * t * pt3.X;
            float y = (1 - t) * (1 - t) * (1 - t) * pt0.Y +
                        3 * t * (1 - t) * (1 - t) * pt1.Y +
                        3 * t * t * (1 - t) * pt2.Y +
                        t * t * t * pt3.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenQuadratic(SKPoint pt0, SKPoint pt1, SKPoint pt2)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenConic(SKPoint pt0, SKPoint pt1, SKPoint pt2, float weight)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float denominator = (1 - t) * (1 - t) + 2 * weight * t * (1 - t) + t * t;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * weight * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * weight * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            x /= denominator;
            y /= denominator;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

На все эти методы ссылается метод расширения, который `CloneWithTransform` также включается в этот класс и показан ниже. Этот метод создает точную копию пути, перечисляя команды пути и создавая новый путь на основе данных. Однако новый путь состоит только из `MoveTo` `LineTo` вызовов и. Все кривые и прямые линии сокращаются до ряда маленьких строк.

При вызове `CloneWithTransform` передается методу a `Func<SKPoint, SKPoint>` , который является функцией с `SKPaint` параметром, возвращающим `SKPoint` значение. Эта функция вызывается для каждой точки, чтобы применить пользовательское алгоритмическое преобразование:

```csharp
static class PathExtensions
{
    public static SKPath CloneWithTransform(this SKPath pathIn, Func<SKPoint, SKPoint> transform)
    {
        SKPath pathOut = new SKPath();

        using (SKPath.RawIterator iterator = pathIn.CreateRawIterator())
        {
            SKPoint[] points = new SKPoint[4];
            SKPathVerb pathVerb = SKPathVerb.Move;
            SKPoint firstPoint = new SKPoint();
            SKPoint lastPoint = new SKPoint();

            while ((pathVerb = iterator.Next(points)) != SKPathVerb.Done)
            {
                switch (pathVerb)
                {
                    case SKPathVerb.Move:
                        pathOut.MoveTo(transform(points[0]));
                        firstPoint = lastPoint = points[0];
                        break;

                    case SKPathVerb.Line:
                        SKPoint[] linePoints = Interpolate(points[0], points[1]);

                        foreach (SKPoint pt in linePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[1];
                        break;

                    case SKPathVerb.Cubic:
                        SKPoint[] cubicPoints = FlattenCubic(points[0], points[1], points[2], points[3]);

                        foreach (SKPoint pt in cubicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[3];
                        break;

                    case SKPathVerb.Quad:
                        SKPoint[] quadPoints = FlattenQuadratic(points[0], points[1], points[2]);

                        foreach (SKPoint pt in quadPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Conic:
                        SKPoint[] conicPoints = FlattenConic(points[0], points[1], points[2], iterator.ConicWeight());

                        foreach (SKPoint pt in conicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Close:
                        SKPoint[] closePoints = Interpolate(lastPoint, firstPoint);

                        foreach (SKPoint pt in closePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        firstPoint = lastPoint = new SKPoint(0, 0);
                        pathOut.Close();
                        break;
                }
            }
        }
        return pathOut;
    }
    ...
}
```

Так как клонированный контур уменьшается до маленьких прямых линий, функция преобразования имеет возможность преобразования прямых линий в кривые.

Обратите внимание, что метод оставляет первую точку каждого контура в переменной, которая называется `firstPoint` , и текущую позицию после каждой команды рисования в переменной `lastPoint` . Эти переменные необходимы для создания окончательной закрывающей строки при `Close` обнаружении глагола.

В примере **глобулартекст** этот метод расширения используется для заключения текста вокруг полушария в трехмерном эффекте:

[![Тройной снимок экрана с текстовой страницей Глобулар](information-images/globulartext-small.png)](information-images/globulartext-large.png#lightbox "Тройной снимок экрана с текстовой страницей Глобулар")

[`GlobularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs)Конструктор класса выполняет это преобразование. Он создает `SKPaint` объект для текста, а затем получает `SKPath` объект из `GetTextPath` метода. Это путь, передаваемый `CloneWithTransform` методу расширения вместе с функцией преобразования:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;

    public GlobularTextPage()
    {
        Title = "Globular Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.Typeface = SKTypeface.FromFamilyName("Times New Roman");
            textPaint.TextSize = 100;

            using (SKPath textPath = textPaint.GetTextPath("HELLO", 0, 0))
            {
                SKRect textPathBounds;
                textPath.GetBounds(out textPathBounds);

                globePath = textPath.CloneWithTransform((SKPoint pt) =>
                {
                    double longitude = (Math.PI / textPathBounds.Width) *
                                            (pt.X - textPathBounds.Left) - Math.PI / 2;
                    double latitude = (Math.PI / textPathBounds.Height) *
                                            (pt.Y - textPathBounds.Top) - Math.PI / 2;

                    longitude *= 0.75;
                    latitude *= 0.75;

                    float x = (float)(Math.Cos(latitude) * Math.Sin(longitude));
                    float y = (float)Math.Sin(latitude);

                    return new SKPoint(x, y);
                });
            }
        }
    }
    ...
}
```

Функция преобразования сначала вычисляет два значения с именем `longitude` и в `latitude` диапазоне от – π/2 сверху и слева от текста до π/2 в правой и нижней части текста. Диапазон этих значений не является визуально удовлетворительным, поэтому они сокращаются путем умножения на 0,75. (Попробуйте выполнить код без этих корректировок. Текст станет слишком запутанным на северном и Южной полюсам и слишком тонкий на сторонах.) Эти трехмерные сферические координаты преобразуются в двухмерные `x` и `y` координаты по стандартным формулам.

Новый путь хранится в виде поля. `PaintSurface`Затем обработчик просто должен центрировать и масштабировать путь, чтобы отобразить его на экране:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint pathPaint = new SKPaint())
        {
            pathPaint.Style = SKPaintStyle.Fill;
            pathPaint.Color = SKColors.Blue;
            pathPaint.StrokeWidth = 3;
            pathPaint.IsAntialias = true;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(0.45f * Math.Min(info.Width, info.Height));     // radius
            canvas.DrawPath(globePath, pathPaint);
        }
    }
}
```

Это очень гибкий метод. Если массив эффектов пути, описанных в статье [**эффекты пути**](effects.md) , не включает в себя что-то, что вы намерены добавить, это способ заполнять пропуски.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)