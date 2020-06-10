---
Title: "преобразование отклонения" Description: "в этой статье объясняется, как преобразование «наклон» может создавать графические объекты с наклоном в SkiaSharp и демонстрирует это с помощью образца кода».
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616 Автор: давидбритч MS. author: дабритч MS. Дата: 03/20/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="the-skew-transform"></a>Преобразование наклона

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Узнайте, как преобразование «наклон» может создавать графические объекты с наклоном в SkiaSharp_

В SkiaSharp преобразование «наклон» приводит к наклону графических объектов, таких как тень в этом изображении:

![](skew-images/skewexample.png "An example of skewing from the Skew Shadow Text program")

Асимметрия превращает прямоугольник в параллелограмм, но наклонный эллипс по-прежнему является эллипсом.

Хотя Xamarin.Forms определяет свойства для преобразования, масштабирования и вращения, в для смещения нет соответствующего свойства Xamarin.Forms .

[`Skew`](xref:SkiaSharp.SKCanvas.Skew(System.Single,System.Single))Метод `SKCanvas` принимает два аргумента для горизонтальной наклона и наклона по вертикали:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Второй [`Skew`](xref:SkiaSharp.SKCanvas.Skew(SkiaSharp.SKPoint)) метод объединяет эти аргументы в одно `SKPoint` значение:

```csharp
public void Skew (SKPoint skew)
```

Однако маловероятно, что вы будете использовать любой из этих двух методов в изоляции.

На странице « **эксперимент** » можно поэкспериментировать со значениями смещения в диапазоне от – 10 до 10. Текстовая строка располагается в левом верхнем углу страницы со значениями наклона, полученными из двух `Slider` элементов. Ниже приведен `PaintSurface` обработчик в [`SkewExperimentPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) классе:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        canvas.Skew((float)xSkewSlider.Value, (float)ySkewSlider.Value);
        canvas.DrawText(text, 0, -textBounds.Top, textPaint);
    }
}
```

Значения `xSkew` аргумента сдвигают нижнюю часть текста вправо для положительных значений или слева для отрицательных значений. Значения `ySkew` сдвига вправо текста вниз для положительных значений или для отрицательных значений.

[![](skew-images/skewexperiment-small.png "Triple screenshot of the Skew Experiment page")](skew-images/skewexperiment-large.png#lightbox "Triple screenshot of the Skew Experiment page")

Если `xSkew` значение равно отрицательному `ySkew` значению, то результат будет поворотом, но также немного масштабируется.

Формулы преобразования выглядят следующим образом:

x "= x + Ксскев · &

y "= Искев · x + y

Например, для положительного `xSkew` значения преобразованное `x'` значение увеличивается по мере `y` роста. Это приводит к появлению наклона.

Если треугольник 200 пикселей в ширину и 100 пикселей в высоком положении располагается в верхнем левом углу в точке (0, 0) и отображается со `xSkew` значением 1,5, то результат параллелограмма выглядит следующим образом:

![](skew-images/skeweffect.png "The effect of the skew transform on a rectangle")

Координаты нижнего края имеют `y` значения 100, поэтому он смещается на 150 пикселей вправо.

Для ненулевых значений `xSkew` или `ySkew` , только точка (0, 0) остается неизменной. Эту точку можно рассматривать как центр наклона. Если требуется, чтобы центр наклона был каким-то другим (как правило, это так), то нет `Skew` метода, который его предоставляет. Необходимо явным образом объединить `Translate` вызовы с `Skew` вызовом. Чтобы центрировать наклон на `px` и `py` , сделайте следующие вызовы:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Формулы составного преобразования:

x "= x + Ксскев · (y – копировать)

y "= Искев · (x – px) + y

Если `ySkew` параметр равен нулю, `px` значение не используется. Значение не имеет значения и аналогично для `ySkew` и `py` .

Вы можете более удобно указать асимметрию в качестве угла наклона, например угла α на этой схеме:

![](skew-images/skewangleeffect.png "The effect of the skew transform on a rectangle with a skewing angle indicated")

Отношение сдвига 150 пикселей к 100-пиксельному вертикали — это тангенс этого угла, в данном примере — 56,3 градусов.

XAML-файл страницы **эксперимента с углом наклона** похож на страницу **угла наклона** , за исключением того, что элементы находятся в `Slider` диапазоне от – 90 градусов до 90 градусов. [`SkewAngleExperiment`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs)Файл кода программной части выравнивает текст на странице и использует `Translate` для установки центра наклона по центру страницы. Короткий `SkewDegrees` метод в нижней части кода преобразует углы в значения наклона:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;

        canvas.Translate(xCenter, yCenter);
        SkewDegrees(canvas, xSkewSlider.Value, ySkewSlider.Value);
        canvas.Translate(-xCenter, -yCenter);
        canvas.DrawText(text, xText, yText, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

Когда угол приближается к положительному или отрицательному 90 градусам, тангенс приближается к бесконечности, но углы до 80 градусов или поэтому можно использовать.

[![](skew-images/skewangleexperiment-small.png "Triple screenshot of the Skew Angle Experiment page")](skew-images/skewangleexperiment-large.png#lightbox "Triple screenshot of the Skew Angle Experiment page")

Небольшой отрицательный сдвиг по горизонтали может имитировать наклонное или курсивное начертание текста, так как демонстрируется страница **наклонного текста** . [`ObliqueTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs)Класс показывает, как это делается:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Maroon,
        TextAlign = SKTextAlign.Center,
        TextSize = info.Width / 8       // empirically determined
    })
    {
        canvas.Translate(info.Width / 2, info.Height / 2);
        SkewDegrees(canvas, -20, 0);
        canvas.DrawText(Title, 0, 0, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

`TextAlign`Свойство объекта `SKPaint` имеет значение `Center` . Без каких-либо преобразований `DrawText` вызов с координатами (0, 0) будет располагать текст горизонтально по центру базового плана в левом верхнем углу. Объект `SkewDegrees` наклоняет текст на 20 градусов по горизонтали относительно базовых показателей. `Translate`Вызов перемещает горизонтальный центр базового плана текста в центр холста:

[![](skew-images/obliquetext-small.png "Triple screenshot of the Oblique Text page")](skew-images/obliquetext-large.png#lightbox "Triple screenshot of the Oblique Text page")

На странице « **наклонный текст тени** » показано, как использовать сочетание наклона и вертикального масштабирования в 45 градусов для создания тени текста, которая будет отклоняться от текста. Ниже приведена сопутствующая часть `PaintSurface` обработчика:

```csharp
using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = info.Width / 6;   // empirically determined

    // Common to shadow and text
    string text = "Shadow";
    float xText = 20;
    float yText = info.Height / 2;

    // Shadow
    textPaint.Color = SKColors.LightGray;
    canvas.Save();
    canvas.Translate(xText, yText);
    canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
    canvas.Scale(1, 3);
    canvas.Translate(-xText, -yText);
    canvas.DrawText(text, xText, yText, textPaint);
    canvas.Restore();

    // Text
    textPaint.Color = SKColors.Blue;
    canvas.DrawText(text, xText, yText, textPaint);
}
```

Сначала отображается тень, а затем текст:

[![](skew-images/skewshadowtext1-small.png "Triple screenshot of the Skew Shadow Text page")](skew-images/skewshadowtext1-large.png#lightbox "Triple screenshot of the Skew Shadow Text page")

Вертикальная координата, передаваемая `DrawText` методу, указывает положение текста относительно базовой линии. Это та же вертикальная координата, которая используется для центра наклона. Этот метод не будет работать, если текстовая строка содержит нижние выносные строки. Например, замените слово "небезопасные" для "Shadow" и вот результат:

[![](skew-images/skewshadowtext2-small.png "Triple screenshot of the Skew Shadow Text page with an alternative word with descenders")](skew-images/skewshadowtext2-large.png#lightbox "Triple screenshot of the Skew Shadow Text page with an alternative word with descenders")

Тень и текст по-прежнему согласовываются по базовому плану, но результат просто выглядит неправильно. Чтобы устранить эту проблему, необходимо получить границы текста:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate`Для вызовов необходимо настроить высоту нижних выносных элементов:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Теперь тень расширяется от нижней части этих нижних выносных элементов:

[![](skew-images/skewshadowtext3-small.png "Triple screenshot of the Skew Shadow Text page with adjustments for descenders")](skew-images/skewshadowtext3-large.png#lightbox "Triple screenshot of the Skew Shadow Text page with adjustments for descenders")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
