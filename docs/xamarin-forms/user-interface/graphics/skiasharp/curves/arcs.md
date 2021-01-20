---
title: Три способа нарисовать дугу
description: В этой статье объясняется, как использовать SkiaSharp для определения Дуг тремя разными способами и демонстрируется в примере кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: F1DA55E4-0182-4388-863C-5C340213BF3C
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6aa52ff13dccbf6c7b65f7006195997971a7cac2
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98609785"
---
# <a name="three-ways-to-draw-an-arc"></a>Три способа нарисовать дугу

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Узнайте, как использовать SkiaSharp для определения Дуг тремя разными способами._

Дуга — это кривая окружности эллипса, например скругленные части этого знака бесконечности:

![Знак бесконечности](arcs-images/arcsample.png)

Несмотря на простоту определения, не существует способа определить функцию Arc-Drawing, удовлетворяющую всем потребностям, и поэтому нет согласия между графическими системами лучших способов рисования дуги. По этой причине класс не `SKPath` ограничивает себя только одним подходом.

`SKPath` Определяет [`AddArc`](xref:SkiaSharp.SKPath.AddArc*) метод, пять различных [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*) методов и два относительных [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*) метода. Эти методы делятся на три категории, представляющие три очень разные подходы к указанию дуги. Используемый способ зависит от информации, доступной для определения дуги, а также от того, как эта дуга помещается вместе с другими графическими изображениями.

## <a name="the-angle-arc"></a>Дуга угла

Для рисования Дуг необходимо указать прямоугольник, ограничивающий эллипс. Дуга на окружности этого эллипса обозначается углами от центра эллипса, указывающего начало дуги и ее длину. Два разных метода рисуют угловые дуги. Это [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single)) метод и [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKRect,System.Single,System.Single,System.Boolean)) метод:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Эти методы идентичны [`AddArc`](xref:Android.Graphics.Path.AddArc*) методам Android и [ `ArcTo` ] XREF: Android. Graphics. Path. аркто *). Метод iOS [`AddArc`](xref:CoreGraphics.CGPath.AddArc(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.Boolean)) аналогичен, но ограничен дугами по окружности окружности, а не обрисована эллипсом.

Оба метода начинаются со `SKRect` значения, определяющего расположение и размер эллипса:

![Овал, начинающий дугу угла](arcs-images/anglearcoval.png)

Дуга является частью окружности этого эллипса.

`startAngle`Аргумент — это угол по часовой стрелке в градусах относительно горизонтальной линии, нарисованной от центра эллипса вправо. `sweepAngle`Аргумент задается относительно `startAngle` . Ниже приведены `startAngle` `sweepAngle` значения в 60 градусах и 100 градусов соответственно:

![Углы, определяющие дугу угла](arcs-images/anglearcangles.png)

Дуга начинается с начального угла. Его длина регулируется углом поворота. Дуга показана красным цветом:

![Выделенная дуга угла](arcs-images/anglearchighlight.png)

Кривая, добавленная в путь с `AddArc` помощью `ArcTo` метода или, является просто частью окружности эллипса:

![Дуга угла по себе](arcs-images/anglearc.png)

`startAngle`Аргументы или `sweepAngle` могут быть отрицательными: Дуга по часовой стрелке используется для положительных значений `sweepAngle` и по часовой стрелке для отрицательных значений.

Однако не `AddArc` определяет  закрытый профиль. Если вы `LineTo` `AddArc` выберете после, линия будет выведена с конца дуги на точку в `LineTo` методе, и то же самое справедливо для `ArcTo` .

`AddArc` автоматически запускает новый профиль и функционально эквивалентен вызову `ArcTo` с последним аргументом `true` :

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Последний аргумент вызывается `forceMoveTo` , и он фактически вызывает `MoveTo` вызов в начале дуги. Это начало нового профиля. Это не так с последним аргументом `false` :

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Эта версия `ArcTo` рисует строку от текущей позицией до начала дуги. Это означает, что дуга может находиться где-то в середине более крупного профиля.

На странице **угловая дуга** можно использовать два ползунка для задания углов начала и поворота. Файл XAML создает экземпляры двух `Slider` элементов и `SKCanvasView` . `PaintCanvas`Обработчик в файле [**AngleArcPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) рисует овал и дугу с помощью двух `SKPaint` объектов, определенных как поля:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

Как видите, начальный и угол поворота могут принимать отрицательные значения:

[![Тройной снимок экрана со страницей угла дуги](arcs-images/anglearc-small.png)](arcs-images/anglearc-large.png#lightbox)

Этот подход к формированию дуги алгоритмически простейшим, и можно легко получить несложное уравнение, описывающее дугу. Зная размер и расположение эллипса, а также углы начала и поворота, начальная и конечная точки дуги могут быть вычислены с помощью простого тригонометрических:

`x = oval.MidX + (oval.Width / 2) * cos(angle)`

`y = oval.MidY + (oval.Height / 2) * sin(angle)`

`angle`Значение может быть `startAngle` или `startAngle + sweepAngle` .

Использование двух углов для определения дуги лучше подходит для тех случаев, когда вам известна угловая длина дуги, которую нужно нарисовать, например, для создания круговой диаграммы. Это показано на странице **разрезанная круговая диаграмма** . [`ExplodedPieChartPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs)Класс использует внутренний класс для определения некоторых данных и цветов:

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

`PaintSurface`Обработчик сначала просматривает элементы, чтобы вычислить `totalValues` число. В результате он может определить размер каждого элемента как часть итога и преобразовать его в угол:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

`SKPath`Для каждого среза круговой диаграммы создается новый объект. Путь состоит из линии из центра, а затем `ArcTo` для рисования дуги и другой строки в центре результатов `Close` вызова. Эта программа отображает разрезанные срезы круга, перемещая их из центра на 50 пикселей. Эта задача требует вектор в направлении середины угла поворота для каждого среза:

[![Тройной снимок экрана страницы разрезанной круговой диаграммы](arcs-images/explodedpiechart-small.png)](arcs-images/explodedpiechart-large.png#lightbox)

Чтобы увидеть, как он выглядит без "развертывания", просто закомментируйте `Translate` вызов:

[![Тройной снимок экрана развернутой страницы круговой диаграммы без развертывания](arcs-images/explodedpiechartunexploded-small.png)](arcs-images/explodedpiechartunexploded-large.png#lightbox)

## <a name="the-tangent-arc"></a>Дуга касательной

Второй тип дуги, поддерживаемой, `SKPath` — это *дуга*, так что она вызывается, так как дуга является окружностью окружности, которая является тангенсом двух соединенных линий.

Дуга касательно добавляется в путь с вызовом  [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) метода с двумя `SKPoint` параметрами или [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,System.Single,System.Single)) перегрузкой с отдельными `Single` параметрами для точек:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

Этот `ArcTo` метод аналогичен [`arct`](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) функции PostScript (Page 532) и [`AddArcToPoint`](xref:CoreGraphics.CGPath.AddArcToPoint(System.nfloat,System.nfloat,System.nfloat,System.nfloat,System.nfloat)) метода iOS.

`ArcTo`Метод включает три точки:

- Текущая точка контура или точка (0, 0), если `MoveTo` не был вызван
- Первый аргумент точки для `ArcTo` метода, называемый *угловой точкой*
- Второй аргумент точки для `ArcTo` , называемый *конечной точкой*:

![Три точки, начинающиеся с дуги по касательной](arcs-images/tangentarcthreepoints.png)

Эти три точки определяют две соединенные линии:

![Линии, соединяющие три точки дуги тангенса](arcs-images/tangentarcconnectinglines.png)

Если три точки являются колинейной &mdash; , то есть, если они находятся на одной прямой линии, &mdash; дуга не будет рисоваться.

`ArcTo`Метод также включает `radius` параметр. Он определяет радиус круга:

![Окружность дуги тангенса](arcs-images/tangentarccircle.png)

Дуга касательно не является обобщенной для эллипса.

Если две строки соответствуют любому из углов, окружность можно вставить между этими строками, чтобы они были касательными к обеим строкам:

![Окружность дуги между двумя линиями](arcs-images/tangentarctangentcircle.png)

Кривая, добавленная в профиль, не затрагивает ни одну из точек, указанных в `ArcTo` методе. Он состоит из прямой линии от текущей точки до первой касательной и дуги, завершающейся во второй точке касательной, показанной здесь красным цветом:

![На схеме показана Предыдущая диаграмма с заметками красной линией, которая показывает выделенную дугу тангенса между двумя линиями.](arcs-images/tangentarchighlight.png)

Вот итоговая прямая линия и дуга, добавленная в профиль:

![Выделенная дуга тангенса между двумя линиями](arcs-images/tangentarc.png)

Профиль можно продолжить с второй точки касательной.

На странице **дуги тангенса** можно поэкспериментировать с дугой касательной. Это первая из нескольких страниц, производных от [`InteractivePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/InteractivePage.cs) , которая определяет несколько удобных `SKPaint` объектов и выполняет `TouchPoint` обработку:

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

Класс `TangentArcPage` является производным от `InteractivePage`. Конструктор в файле [**TangentArcPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) отвечает за создание и инициализацию `touchPoints` массива, а также установку `baseCanvasView` (в `InteractivePage` ) `SKCanvasView` объекта, созданного в файле [**танжентаркпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) :

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

`PaintSurface`Обработчик использует `ArcTo` метод для рисования дуги на основе сенсорных точек и `Slider` , но также алгоритмически вычисляет круг, на котором основан угол.

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }

    // Vector methods
    SKPoint Normalize(SKPoint v)
    {
        float magnitude = Magnitude(v);
        return new SKPoint(v.X / magnitude, v.Y / magnitude);
    }

    float Magnitude(SKPoint v)
    {
        return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
    }
}
```

На этой странице в качестве **дуги** используется:

[![Тройной снимок экрана со страницей дуги на Тангенсе](arcs-images/tangentarc-small.png)](arcs-images/tangentarc-large.png#lightbox)

Дуга касательно идеально подходит для создания скругленных углов, таких как скругленный прямоугольник. Поскольку `SKPath` уже включает `AddRoundedRect` метод, на странице **скругленная хептагон** демонстрируется использование `ArcTo` для округления углов трехмерного многоугольника. (Код обобщен для любого обычного многоугольника.)

`PaintSurface`Обработчик [`RoundedHeptagonPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) класса содержит один `for` цикл для вычисления координат семи вершин хептагон, а второй — для вычисления средних точек из семи Сторон из этих вершин. Затем эти средние точки используются для создания пути:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

Вот работающая программа:

[![Тройной снимок экрана со скругленной Хептагон страницей](arcs-images/roundedheptagon-small.png)](arcs-images/roundedheptagon-large.png#lightbox)

## <a name="the-elliptical-arc"></a>Эллиптическая дуга

Эллиптическая дуга добавляется в путь с вызовом [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,SkiaSharp.SKPoint)) метода, имеющего два `SKPoint` параметра, или  [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo(System.Single,System.Single,System.Single,SkiaSharp.SKPathArcSize,SkiaSharp.SKPathDirection,System.Single,System.Single)) перегрузки с отдельными координатами X и Y:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Эллиптическая дуга согласуется с [эллиптической дугой](https://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) , включенной в масштабируемый векторный график (SVG) и [`ArcSegment`](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) класс универсальная платформа Windows.

Эти `ArcTo` методы нарисуют дугу между двумя точками, которые являются текущей точкой контура, и последним параметром `ArcTo` метода ( `xy` параметр или отдельные `x` `y` Параметры и):

![Две точки, которые задают эллиптическую дугу](arcs-images/ellipticalarcpoints.png)

Первый параметр точки `ArcTo` метода ( `r` или `rx` и `ry` ) не является точкой, а вместо этого указывает горизонтальный и вертикальный Радиусы эллипса;

![Эллипс, который определил эллиптическую дугу](arcs-images/ellipticalarcellipse.png)

`xAxisRotate`Параметр — это число градусов по часовой стрелке для поворота эллипса:

![Наклон эллипса, который определил эллиптическую дугу](arcs-images/ellipticalarctiltedellipse.png)

Если затем этот наклонный эллипс размещается таким образом, что он соприкасается с двумя точками, точки соединяются двумя разными дуг:

![Первый набор эллиптических дуг](arcs-images/ellipticalarcellipse1.png)

Эти две дуги могут различаться двумя способами: верхняя дуга больше, чем нижняя дуга, и по мере того, как дуга рисуется слева направо, верхняя дуга рисуется по часовой стрелке, а нижняя дуга — в направлении против часовой стрелки.

Также можно разместить эллипс между двумя точками другим способом:

![Второй набор эллиптических дуг](arcs-images/ellipticalarcellipse2.png)

Теперь у вас есть меньшая дуга, отображаемая по часовой стрелке, и крупная дуга в нижней части, отображаемой по часовой стрелке.

Таким образом, эти две точки могут быть соединены дугой, определяемой наклонным эллипсом, в общем виде четырьмя способами:

![Все четыре эллиптических дуги](arcs-images/ellipticalarccolors.png)

Эти четыре дуги различаются четырьмя комбинациями [`SKPathArcSize`](xref:SkiaSharp.SKPathArcSize) [`SKPathDirection`](xref:SkiaSharp.SKPathDirection) аргументов типа перечисления и с `ArcTo` методом:

- Красный: Скпасарксизе. Large и Скпасдиректион. по часовой стрелке
- Зеленый: Скпасарксизе. Малый и Скпасдиректион. по часовой стрелке
- синий: Скпасарксизе. Малый и Скпасдиректион. против часовой стрелки
- Пурпурный: Скпасарксизе. Large и Скпасдиректион. против часовой стрелки

Если наклонный эллипс не достаточен для размещения между двумя точками, то он равномерно масштабируется до тех пор, пока он не станет достаточно большим. Только две уникальные дуги соединяют две точки в этом случае. Они могут различаться с помощью `SKPathDirection` параметра.

Несмотря на то, что этот подход к определению Дуг звучит сложно при первом обнаружении, это единственный подход, позволяющий определить дугу с повернутым эллипсом, и часто самый простой подход, когда необходимо интегрировать дуги с другими частями контура.

**Эллиптическая страница дуги** позволяет интерактивно задать две точки, а также размер и поворот эллипса. `EllipticalArcPage`Класс является производным от `InteractivePage` , а `PaintSurface` обработчик в файле кода программной части [**EllipticalArcPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) рисует четыре дуги:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

Здесь он работает:

[![Тройной снимок экрана эллиптической страницы дуги](arcs-images/ellipticalarc-small.png)](arcs-images/ellipticalarc-large.png#lightbox)

Страница **дуга бесконечности** использует эллиптическую дугу для рисования знака бесконечности. Бесконечная подпись основана на двух кругах с радиусами 100 единиц, разделенными на 100 единиц:

![Два круга](arcs-images/infinitycircles.png)

Две строки, пересекающие друг друга, являются касательными к обоим кругам:

![Два круга с касательными линиями](arcs-images/infinitycircleslines.png)

Знак бесконечности — это сочетание частей этих кругов и двух строк. Чтобы использовать эллиптическую дугу для рисования знака бесконечности, необходимо определить координаты, где две линии являются касательными к кругам.

Создайте правый прямоугольник в одном из кругов:

![Два круга с касательными и внедренными кругами](arcs-images/infinitytriangle.png)

Радиус окружности — 100 единиц, а гипотенуза треугольника — 150 единиц, поэтому угол α — это арксинус (обратный синус) 100, деленный на 150 или 41,8 градусы. Длина второй стороны треугольника — 150 раз косинус 41,8 градусов или 112, которую также можно вычислить с помощью теорему теорема.

Координаты касательной точки затем могут быть вычислены с использованием следующих сведений:

`x = 112·cos(41.8) = 83`

`y = 112·sin(41.8) = 75`

Все четыре точки касательно необходимы для отрисовки знака бесконечности в центре в точке (0, 0) с кругом радиусов 100:

![Два круга с касательными и координатами](arcs-images/infinitycoordinates.png)

`PaintSurface`Обработчик в [`ArcInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) классе размещает знак бесконечности, чтобы точка (0, 0) была размещена в центре страницы, а путь масштабируется до размера экрана:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
                              info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

Код использует `Bounds` свойство объекта `SKPath` для определения размеров бесконечности синуса, чтобы масштабировать его до размера холста:

[![Тройной снимок экрана со страницей "бесконечность" для дуги](arcs-images/arcinfinity-small.png)](arcs-images/arcinfinity-large.png#lightbox)

Результат выглядит немного небольшим, что предполагает, что `Bounds` свойство объекта `SKPath` сообщает о размере, превышающем путь.

На внутреннем уровне СКИА приблизительная дуга с использованием нескольких квадратичных кривых Безье. Эти кривые (как можно увидеть в следующем разделе) содержат контрольные точки, которые определяют, как Кривая рисуется, но не является частью отображаемой кривой. `Bounds`Свойство включает эти контрольные точки.

Чтобы получить более тесное соответствие, используйте `TightBounds` свойство, которое исключает контрольные точки. Ниже приведена программа, работающая в альбомном режиме, и `TightBounds` свойство используется для получения границ пути:

[![Тройной снимок экрана страницы с жесткими границами на странице "бесконечность"](arcs-images/arcinfinitytightbounds-small.png)](arcs-images/arcinfinitytightbounds-large.png#lightbox)

Несмотря на то, что соединения между дугами и прямыми линиями выполняются математически, переход от дуги к прямой линии может показаться немного резкой. Более высокий знак бесконечности представлен в следующей статье по [**трем типам кривых Безье**](beziers.md).

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)