---
title: Три типа кривых Безье
description: В этой статье объясняется, как с помощью SkiaSharp визуализировать кривые Безье, кубические и Коник Безье в Xamarin.Forms приложениях и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: davidbritch
ms.author: dabritch
ms.date: 05/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9193cef76a5f474f3681b15a1315e5840b41d88a
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562981"
---
# <a name="three-types-of-bzier-curves"></a>Три типа кривых Безье

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Узнайте, как использовать SkiaSharp для отрисовки кривых Безье в кубических, квадратичных и коникях_

Кривая Безье именуется после Пьера Безье (1910 – 1999), французского инженера в автомобильной компании Ренаулт, который использовал кривую для ролевого проектирования тел автомобилей.

Кривые Безье хорошо подходят для интерактивного проектирования: они хорошо работают с &mdash; другими словами, не существует единственных случаев, которые приводят к бесконечной или неудобной кривой, &mdash; и обычно визуальноы.

![Пример кривой Безье](beziers-images/beziersample.png)

Символьные контуры шрифтов на компьютере обычно определяются кривыми Безье.

Статья Википедии по [**кривой Безье**](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) содержит некоторые полезные фундаментальные сведения. Термин « *Кривая Безье* » фактически относится к семейству схожих кривых. SkiaSharp поддерживает три типа кривых Безье, которые называются *кубическими*, *квадратными*и *Коник*. Коник также называется *рациональным квадратичным*.

## <a name="the-cubic-bzier-curve"></a>Кривая Безье третьего порядка

Кубический тип — это кривая Безье, которую большинство разработчиков думают, когда возникает тема кривых Безье.

Можно добавить кривую Безье третьего порядка к `SKPath` объекту с помощью [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKPoint)) метода с тремя `SKPoint` параметрами или [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo(System.Single,System.Single,System.Single,System.Single,System.Single,System.Single)) перегрузки с отдельными `x` параметрами и `y` :

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Кривая начинается с текущей точки контура. Полная кривая Безье третьего порядка определяется четырьмя точками:

- Начальная точка: текущая точка в контуре или (0, 0), если `MoveTo` не был вызван
- Первая контрольная точка: `point1` в `CubicTo` вызове
- Вторая контрольная точка: `point2` в `CubicTo` вызове
- Конечная точка: `point3` в `CubicTo` вызове

Результирующая Кривая начинается с начальной точки и заканчивается в конечной точке. Кривая обычно не проходит через две контрольные точки; Вместо этого контрольные точки работают так же, как магнитные линии, чтобы извлекать кривую к ним.

Лучший способ получить впечатление от кривой Безье третьего порядка — эксперименты. Это цель страницы **кривой Безье** , которая является производной от `InteractivePage` . В файле [**безиеркурвепаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) создаются экземпляры `SKCanvasView` и `TouchEffect` . Файл кода программной части [**BezierCurvePage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) создает четыре `TouchPoint` объекта в своем конструкторе. `PaintSurface`Обработчик событий создает объект `SKPath` для отрисовки кривой Безье на основе четырех `TouchPoint` объектов, а также рисует линии с точками по касательной от контрольных точек к конечным точкам:

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
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

Здесь он работает:

[![Тройной снимок экрана со страницей кривой Безье](beziers-images/beziercurve-small.png)](beziers-images/beziercurve-large.png#lightbox)

Кривая — это степень полинома в кубических изгибах. Кривая пересекает прямую линию в трех точках. В начальной точке кривая всегда имеет тангенс и в том же направлении, что и, прямая линия от начальной точки до первой контрольной точки. В конечной точке кривая всегда имеет тангенс и в том же направлении, что и, прямая линия от второй контрольной точки до конечной точки.

Кривая Безье третьего порядка всегда ограничена выпуклой грани четырехсторонней, соединяющей четыре точки. Это называется *выпуклой поверхности*. Если контрольные точки находятся на прямой линии между начальной и конечной точками, то кривая Безье выводится в виде прямой линии. Но кривая также может пересекаться с самим собой, как показано на третьем снимке экрана.

Профиль пути может содержать несколько соединенных кривых Безье третьего порядка, но соединение между двумя кривыми Безье третьего порядка будет гладким, только если следующие три точки являются ковариантными (то есть расположены на прямой линии):

- Вторая контрольная точка первой кривой
- Конечная точка первой кривой, которая также является начальной точкой второй кривой
- Первая контрольная точка второй кривой

В следующей статье по [**данным о пути SVG**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)вы обнаружите средство для упрощения определения гладко Соединенных кривых Безье.

Иногда бывает полезно изучить базовые математические уравнения, которые отображают кривую Безье третьего порядка. Для *t* в диапазоне от 0 до 1 значения параметрической формулы выглядят следующим образом:

x (t) = (1 – t) ³ x ₀ + 3T (1 – t) ² x ₁ + 3T ² (1 – t) x ₂ + t ³ x ₃

y (t) = (1 – t) ³ y ₀ + 3T (1 – t) ² y ₁ + 3T ² (1 – t) y ₂ + t ³ y ₃

Наибольший показатель степени 3 подтверждает, что эти данные имеют значение в кубических полиномах. Очень просто проверить, что, если `t` равно 0, точка — (x ₀, y ₀), то есть начальная точка, а если `t` равно 1, то точка имеет значение (x ₃, y ₃), то есть конечная точка. Рядом с начальной точкой (для младших значений `t` ) Первая контрольная точка (x ₁, y ₁) имеет усиленный результат, а ближе к конечной точке (большие значения 't ') вторая контрольная точка (x ₂, y ₂) имеет высокий результат.

## <a name="bezier-curve-approximation-to-circular-arcs"></a>Приближение кривой Безье к круговым дугам

Иногда удобно использовать кривую Безье для визуализации дуги окружности. Кривая Безье третьего порядка может приблизительно оценить дугу окружности до четвертого круга, поэтому четыре Соединенные кривые Безье могут определять целую окружность. Эта аппроксимация обсуждается в двух статьях, опубликованных более 25 лет назад:

> Tor Доккен, et al, «хорошее приближение кругов по кривизне непрерывных кривых Безье», « *компьютерно-геометрическое проектирование 7* (1990), 33-41.

> Майкл Голдапп, "приближение круглых дуг на значение полинома в кубических," *компьютерно-геометрическое проектирование 8* (1991), 227-238.

На следующей диаграмме показаны четыре точки, помеченные как `pto` , `pt1` , `pt2` и, `pt3` определяющие кривую Безье (показанную красным цветом), которая приблизительно представляет собой дугу окружности:

![Приближение дуги окружности к кривой Безье](beziers-images/bezierarc45.png)

Строки начальной и конечной точек указываются касательно к окружности и кривой Безье и имеют длину *L*. Первая статья, упомянутая выше, указывает, что кривая Безье лучше всего приближена к окружности, если эта длина *L* вычисляется следующим образом:

L = 4 × Tan (α/4)/3

На рисунке показан угол в 45 градусов, так что L равно 0,265. В коде это значение умножается на требуемый радиус окружности.

**Круглая страница дуги Безье** позволяет поэкспериментировать с определением кривой Безье, чтобы приблизительно оценить круг дуги для углов в диапазоне от до 180 градусов. Файл [**безиерЦиркулараркпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) создает экземпляр `SKCanvasView` и `Slider` для выбора угла. `PaintSurface`Обработчик событий в файле кода программной части [**BezierCircularArgPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) использует преобразование, чтобы задать точку (0, 0) в центре холста. Он рисует круг на этой точке для сравнения, а затем вычисляет две контрольные точки для кривой Безье:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
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

```

Начальная и конечная точки ( `point0` и `point3` ) рассчитываются исходя из обычных параметрической формул для окружности. Поскольку окружность выравнивается по центру (0, 0), эти точки также можно рассматривать как радиальные векторы от центра окружности до окружности. Контрольные точки расположены на линиях, которые являются касательными к окружности, поэтому они расположены на правой углы этих радиальных векторов. Вектор, расположенный справа от другого, является просто исходным вектором, в котором координаты X и Y меняются местами, и один из них стал отрицательным.

Вот программа, работающая с разными углами:

[![Тройной снимок экрана круговой страницы дуги Безье](beziers-images/beziercirculararc-small.png)](beziers-images/beziercirculararc-large.png#lightbox)

Внимательно взгляните на третий снимок экрана, и вы увидите, что кривая Безье особенно отклоняется от полукруга, если угол равен 180 градусам, но на экране iOS видно, что он по-видимому подходит для четвертой окружности, если угол равен 90 градусам.

Вычисление координат двух контрольных точек выполняется довольно просто, если окружность круга ориентирована следующим образом:

![Приближение окружности круга с кривой Безье](beziers-images/bezierarc90.png)

Если радиус окружности — 100, то *L* — 55, и это просто число, которое нужно запомнить.

Возведение в круг на странице **«окружность** » анимируется на фигуре между кругом и квадратом. Окружность приближена к четырем кривым Безье, координаты которых показаны в первом столбце определения массива в [`SquaringTheCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) классе:

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

Второй столбец содержит координаты четырех кривых Безье, определяющих квадрат, область которого приблизительно совпадает с областью круга. (Рисование квадрата с *точной* областью в виде заданной окружности является классической неразрешимой геометрической проблемой [возведения в круг](https://en.wikipedia.org/wiki/Squaring_the_circle).) Для отрисовки квадрата с кривыми Безье две контрольные точки для каждой кривой одинаковы, и они линейно связаны с начальной и конечной точками, поэтому кривая Безье визуализируется как прямая линия.

Третий столбец массива предназначен для интерполяции значений анимации. На странице задается таймер для 16 миллисекунд, а `PaintSurface` обработчик вызывается по этой ставке:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

Точки интерполируются на основе значения осЦиллатинг синусоидалли `t` . Затем интерполяция точек используется для создания серии из четырех соединенных кривых Безье. Анимация запущена:

[![Тройной снимок экрана возведения в круг страницы окружности](beziers-images/squaringthecircle-small.png)](beziers-images/squaringthecircle-large.png#lightbox)

Такую анимацию было бы невозможно без алгоритмически, достаточно гибкой для подготовки к просмотру как круглых дуг, так и прямых линий.

Страница « **бесконечность** » также использует преимущества кривой Безье для приблизительной окружности дуги. Вот `PaintSurface` обработчик [`BezierInfinityPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) класса:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
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

Может оказаться хорошим упражнением для отображения этих координат на графике, чтобы увидеть, как они связаны. Знак бесконечности располагается вокруг точки (0, 0), а два цикла имеют центры (– 150, 0) и (150, 0) и радиусы 100. В серии `CubicTo` команд можно видеть координаты X точек управления, принимающих значения от – 95 и – 205 (эти значения – 150 Plus и минус 55), 205 и 95 (150 Plus и минус 55), а также 250 и-250 для прав и левых сторон. Единственным исключением является то, что знак бесконечности пересекает себя в центре. В этом случае контрольные точки имеют координаты с сочетанием 50 и – 50, чтобы выровнять кривую вблизи от центра.

Вот знак бесконечности:

[![Тройной снимок экрана страницы бесконечной Безье](beziers-images/bezierinfinity-small.png)](beziers-images/bezierinfinity-large.png#lightbox)

Это несколько ближе к центру, чем знак бесконечности, отображаемый на странице **Arc Infinity** , из [**трех способов рисования статьи дуги**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) .

## <a name="the-quadratic-bzier-curve"></a>Квадратичная кривая Безье

Квадратичная кривая Безье имеет только одну контрольную точку, а кривая определяется только тремя точками: начальной точкой, контрольной точкой и конечной точкой. Параметрической уравнения очень похожи на кривую Безье третьего порядка, за исключением того, что наибольший показатель степени равен 2, поэтому кривая является квадратичным полиномом:

x (t) = (1 – t) ² x ₀ + 2T (1 – t) x ₁ + t ² x ₂

y (t) = (1 – t) ² y ₀ + 2T (1 – t) y ₁ + t ² y ₂

Чтобы добавить в контур квадратичную кривую Безье, используйте [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint)) метод или [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo(System.Single,System.Single,System.Single,System.Single)) перегрузку с отдельными `x` и `y` координатами:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Методы добавляют кривую из текущей позиции в `point2` и в `point1` качестве контрольной точки.

Вы можете поэкспериментировать с квадратичными кривыми Безье с помощью страницы **квадратичной кривой** , которая очень похожа на страницу **кривой Безье** , за исключением трех точек касания. Вот `PaintSurface` обработчик в файле кода программной части [**QuadraticCurve.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

И здесь он работает:

[![Тройной снимок экрана со страницей кривой с квадратичным изгибом](beziers-images/quadraticcurve-small.png)](beziers-images/quadraticcurve-large.png#lightbox)

Пунктирные линии являются касательными к кривой в начальной и конечной точках и встречаются в контрольной точке.

Квадратичная Безье является хорошим решением, если требуется кривая общей фигуры, но предпочтительнее использовать только одну контрольную точку, а не две. Квадратичная Безье визуализируется более эффективно, чем любая другая кривая, поэтому она используется внутренне в СКИА для отрисовки эллиптических дуг.

Однако форма квадратичной кривой Безье не является эллиптической, поэтому для приблизительной дуги Бéзиерс требуется несколько квадратичных кривых. Квадратичная Безье является сегментом парабола.

## <a name="the-conic-bzier-curve"></a>Кривая Безье Коник

Кривая Безье Коник &mdash; также известна как рациональная квадратичная кривая Безье &mdash; — это относительно Последнее дополнение к семейству кривых Безье. Как и квадратичная кривая Безье, рациональная кривая Безье представляет собой начальную точку, конечную точку и одну контрольную точку. Но рациональная кривая Безье — также требуется значение *веса* . Это называется *рациональным* квадратичным квадратом, так как непараметрической формулы подразумевают соотношение.

Параметрической уравнения для X и Y — это коэффициенты, имеющие одинаковый знаменатель. Ниже приведено уравнение для знаменателя *в диапазоне от* 0 до 1 и значения веса *w*:

d (t) = (1 – t) ² + 2wt (1 – t) + t ²

Теоретически, рациональный квадратичный квадрат может содержать три отдельных значения веса, по одному для каждого из трех терминов, но их можно упростить до одного значения веса в среднем выражении.

Параметрической уравнения для координат X и Y аналогичны параметрам параметрической формулы для квадратичной Безье, за исключением того, что в среднем выражении также содержится значение веса, а выражение делится на знаменатель:

x (t) = ((1 – t) ² x ₀ + 2wt (1 – t) x ₁ + t ² x ₂)) a d (t)

y (t) = ((1 – t) ² y ₀ + 2wt (1 – t) y ₁ + t ² y ₂)) a d (t)

Рациональные квадратичные кривые Безье также называются *коникс* , поскольку они могут точно представлять сегменты любых Коник разделов &mdash; хиперболас, параболас, эллипсов и кругов.

Чтобы добавить рациональную квадратичную кривую Безье к контуру, используйте [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(SkiaSharp.SKPoint,SkiaSharp.SKPoint,System.Single)) метод или [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo(System.Single,System.Single,System.Single,System.Single,System.Single)) перегрузку с отдельными `x` и `y` координатами:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Обратите внимание на последний `weight` параметр.

Страница **кривой Коник** позволяет экспериментировать с этими кривыми. Класс `ConicCurvePage` является производным от `InteractivePage`. Файл [**кониккурвепаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) создает экземпляр, `Slider` чтобы выбрать значение веса от – 2 до 2. Файл кода программной части [**ConicCurvePage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) создает три `TouchPoint` объекта, а `PaintSurface` обработчик просто выводит результирующую кривую с касательными к контрольным точкам:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Здесь он работает:

[![Тройной снимок экрана со страницей кривой Коник](beziers-images/coniccurve-small.png)](beziers-images/coniccurve-large.png#lightbox)

Как видите, контрольная точка, по-видимому, будет извлекать кривую к ней больше, когда вес больше. Если весовой коэффициент равен нулю, кривая становится прямой линией от начальной точки до конечной точки.

Теоретически возможны отрицательные веса, и кривая *отбрасывается* от контрольной точки. Однако весовые коэффициенты – 1 или ниже приводят к тому, что знаменатель в параметрической уравнениях становится отрицательным для определенных значений *t*. Возможно, по этой причине отрицательные веса игнорируются в `ConicTo` методах. Программа **кривой Коник** позволяет задать отрицательные веса, но, как можно увидеть, эксперименты, отрицательные весовые коэффициенты имеют тот же результат, что и нулевой вес, и приводят к отрисовке прямой линии.

Очень просто получить контрольную точку и весовой коэффициент, чтобы использовать `ConicTo` метод для рисования дуги окружности вверх (но не включая) полукруга. На следующей схеме линии касательно от начальной и конечной точек соответствуют контрольной точке.

![Коникная дуга, Визуализация дуги окружности](beziers-images/conicarc.png)

Можно использовать тригонометрию для определения расстояния точки управления от центра окружности: это радиус круга, деленный на косинус половины угла α. Чтобы нарисовать дугу окружности между начальной и конечной точками, установите вес равным этому же косинусу половины угла. Обратите внимание, что если угол равен 180 градусам, то линии касательно никогда не соблюдаются, а вес равен нулю. Но для углов менее 180 градусов математический подход работает нормально.

Это показано на странице **дуги окружности Коник** . Файл [**коникЦиркуларарк. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) создает экземпляр `Slider` для выбора угла. `PaintSurface`Обработчик в файле кода программной части [**ConicCircularArc.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) вычисляет контрольную точку и вес:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

Как видите, отсутствует визуальная разница между `ConicTo` контуром, показанным красным, и базовым кругом, отображаемым для справки:

[![Тройной снимок экрана Коник круглой страницы дуги](beziers-images/coniccirculararc-small.png)](beziers-images/coniccirculararc-large.png#lightbox)

Но установите угол 180 градусов, и математика завершится ошибкой.

В данном случае это не `ConicTo` поддерживает отрицательные веса, поскольку в теории (на основе параметрической уравнений) окружность может быть выполнена с помощью другого вызова `ConicTo` с теми же точками, но с отрицательным значением веса. Это позволит создать весь круг с двумя `ConicTo` кривыми на основе любого угла между (но не включая) нулями и 180 градусами.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)