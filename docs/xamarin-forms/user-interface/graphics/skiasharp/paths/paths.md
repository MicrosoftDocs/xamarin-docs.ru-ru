---
Title: "основы пути в SkiaSharp" Description: "в этой статье рассматривается объект SkiaSharp Скпас для объединения Соединенных линий и кривых и демонстрируется пример кода".
MS. произв. Xamarin MS. AssetID: A7EDA6C2-3921-4021-89F3-211551E430F1 MS. Technology: Xamarin-skiasharp Автор: давидбритч MS. author: дабритч МС. Дата: 03/10/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="path-basics-in-skiasharp"></a>Основы пути в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Изучите объект SkiaSharp Скпас для объединения Соединенных линий и кривых_

Одной из наиболее важных возможностей графического контура является возможность определить, когда должно быть установлено соединение с несколькими линиями и когда они не должны быть подключены. Разница может быть значительной, так как верхние части этих двух треугольников демонстрируют:

![](paths-images/connectedlinesexample.png "Two triangles showing the difference between connected and disconnected lines")

Графический путь инкапсулируется [`SKPath`](xref:SkiaSharp.SKPath) объектом. Путь — это коллекция из одного или нескольких *контуров*. Каждый профиль является коллекцией *Соединенных* прямых линий и кривых. Контуры не связаны друг с другом, но могут визуально перекрываться. Иногда один профиль может перекрывать сам себя.

Обычно контур начинается с вызова следующего метода `SKPath` :

- [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo*)Начало нового профиля

Аргумент для этого метода является одной точкой, которую можно выразить как `SKPoint` значение или как отдельные координаты X и Y. `MoveTo`Вызов устанавливает точку в начале контура и начальную *текущую точку*. Чтобы продолжить профиль с линией или кривой от текущей точки до точки, указанной в методе, которая затем станет новой текущей точкой, можно вызвать следующие методы:

- [`LineTo`](xref:SkiaSharp.SKPath.LineTo*)Добавление прямой линии к контуру
- [`ArcTo`](xref:SkiaSharp.SKPath.ArcTo*)Добавление дуги, которая является линией окружности круга или эллипса
- [`CubicTo`](xref:SkiaSharp.SKPath.CubicTo*)Добавление сплайна Безье третьего порядка
- [`QuadTo`](xref:SkiaSharp.SKPath.QuadTo*)Добавление квадратичного сплайна Безье
- [`ConicTo`](xref:SkiaSharp.SKPath.ConicTo*)Добавление рационального сплайна Безье квадратичных кривых, который может точно визуализировать Коник разделы (многоточия, параболас и хиперболас)

Ни один из этих пяти методов не содержит всю информацию, необходимую для описания линии или кривой. Каждый из этих пяти методов работает вместе с текущей точкой, установленной непосредственно перед ним вызовом метода. Например, `LineTo` метод добавляет прямую линию к контуру на основе текущей точки, поэтому параметр `LineTo` является единственной точкой.

`SKPath`Класс также определяет методы, имена которых совпадают с именами этих шести методов, но с a `R` в начале:

- [`RMoveTo`](xref:SkiaSharp.SKPath.RMoveTo*)
- [`RLineTo`](xref:SkiaSharp.SKPath.RLineTo*)
- [`RArcTo`](xref:SkiaSharp.SKPath.RArcTo*)
- [`RCubicTo`](xref:SkiaSharp.SKPath.RCubicTo*)
- [`RQuadTo`](xref:SkiaSharp.SKPath.RQuadTo*)
- [`RConicTo`](xref:SkiaSharp.SKPath.RConicTo*)

`R`Обозначает *относительный*. Эти методы имеют тот же синтаксис, что и соответствующие методы, `R` но они относятся к текущей точке. Они удобны для рисования похожих частей пути в методе, который вызывается несколько раз.

Контур заканчивается другим вызовом `MoveTo` или `RMoveTo` , который начинает новый профиль, или вызовом метода `Close` , который закрывает профиль. `Close`Метод автоматически добавляет прямую линию от текущей точки к первой точке контура и помечает путь как закрытый, что означает, что он будет визуализирован без наконечников обводки.

Разница между открытыми и закрытыми контурами показана на **двух треугольных** страницах, где `SKPath` для отрисовки двух треугольников используется объект с двумя контурами. Первый профиль открыт, а второй — закрытым. Вот [`TwoTriangleContoursPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/TwoTriangleContoursPage.cs) класс:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

Первый профиль состоит из вызова [`MoveTo`](xref:SkiaSharp.SKPath.MoveTo(System.Single,System.Single)) с использованием координат X и Y, а не `SKPoint` значения, за которым следуют три вызова для [`LineTo`](xref:SkiaSharp.SKPath.LineTo(System.Single,System.Single)) рисования трех сторон треугольника. Второй профиль имеет только два вызова `LineTo` , но завершает профиль с помощью вызова [`Close`](xref:SkiaSharp.SKPath.Close) , который закрывает профиль. Разница существенна:

[![](paths-images/twotrianglecontours-small.png "Triple screenshot of the Two Triangle Contours page")](paths-images/twotrianglecontours-large.png#lightbox "Triple screenshot of the Two Triangle Contours page")

Как видите, первый профиль, очевидно, представляет собой последовательность из трех соединенных линий, но конец не подключается с самого начала. Две строки перекрываются сверху. Второй контур, очевидно, закрыт и был выполнен с одним меньшим числом `LineTo` вызовов, так как `Close` метод автоматически добавляет завершающую строку для закрытия контура.

`SKCanvas`определяет только один [`DrawPath`](xref:SkiaSharp.SKCanvas.DrawPath(SkiaSharp.SKPath,SkiaSharp.SKPaint)) метод, который в этой демонстрации вызывается дважды для заполнения и обводки контура. Все контуры заполнены, даже те, которые не закрыты. В целях заполнения незакрытых путей предполагается, что между начальной и конечной точками контуров существует прямая линия. Если удалить последнюю `LineTo` из первого профиля или удалить `Close` вызов из второго контура, то каждый профиль будет иметь только две стороны, но будет заполнен так, как если бы он был треугольником.

`SKPath`Определяет множество других методов и свойств. Следующие методы добавляют в путь все пути, которые могут быть закрыты или закрыты в зависимости от метода:

- [`AddRect`](xref:SkiaSharp.SKPath.AddRect*)
- [`AddRoundedRect`](xref:SkiaSharp.SKPath.AddRoundedRect(SkiaSharp.SKRect,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddCircle`](xref:SkiaSharp.SKPath.AddCircle(System.Single,System.Single,System.Single,SkiaSharp.SKPathDirection))
- [`AddOval`](xref:SkiaSharp.SKPath.AddOval(SkiaSharp.SKRect,SkiaSharp.SKPathDirection))
- [`AddArc`](xref:SkiaSharp.SKPath.AddArc(SkiaSharp.SKRect,System.Single,System.Single))Добавление кривой для окружности эллипса
- [`AddPath`](xref:SkiaSharp.SKPath.AddPath*)Добавление другого пути к текущему пути
- [`AddPathReverse`](xref:SkiaSharp.SKPath.AddPathReverse(SkiaSharp.SKPath))Добавление другого пути в обратную

Помните, что `SKPath` объект определяет только геометрическую &mdash; последовательность точек и соединений. Только если объект `SKPath` объединен с объектом, то `SKPaint` контур отображается с определенным цветом, шириной штрихов и т. д. Кроме того, помните, что `SKPaint` объект, переданный в `DrawPath` метод, определяет характеристики всего пути. Если нужно нарисовать нечто, требующее нескольких цветов, необходимо использовать отдельный контур для каждого цвета.

Точно так же, как внешний вид начала и конца линии определяется наконечником обводки, внешний вид соединения между двумя строками определяется *соединением с обводкой*. Это можно указать, задав [`StrokeJoin`](xref:SkiaSharp.SKPaint.StrokeJoin) для свойства значение `SKPaint` члена [`SKStrokeJoin`](xref:SkiaSharp.SKStrokeJoin) перечисления:

- `Miter`для точки подключения
- `Round`Для скругленного объединения
- `Bevel`для неровного объединения

На странице « **Присоединение штрихов** » показаны три объединения штрихов с кодом, похожим на страницу « **штрихи** ». Это `PaintSurface` обработчик событий в [`StrokeJoinsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeJoinsPage.cs) классе:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Right
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

Вот работающая программа:

[![](paths-images/strokejoins-small.png "Triple screenshot of the Stroke Joins page")](paths-images/strokejoins-large.png#lightbox "Triple screenshot of the Stroke Joins page")

Угловое соединение состоит из острой точки, в которой соединяются линии. При соединении двух строк с небольшим углом угловое соединение может стать довольно длинным. Во избежание чрезмерно длинных срезов соединения длина углового соединения ограничена значением [`StrokeMiter`](xref:SkiaSharp.SKPaint.StrokeMiter) свойства `SKPaint` . Угловое соединение, размер которого превышает эту длину, вырезается, чтобы стать рельефным соединением.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
