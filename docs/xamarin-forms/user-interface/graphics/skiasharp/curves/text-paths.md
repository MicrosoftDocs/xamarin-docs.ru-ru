---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b0cbb7d26a2aea02a3255fc75947c20a3d803b86
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84131902"
---
# <a name="paths-and-text-in-skiasharp"></a>Пути и текст в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Изучение пересечения путей и текста_

В современных графических системах текстовые шрифты — это наборы контуров символов, обычно определяемые квадратичными кривыми Безье. Следовательно, многие современные системы графики включают средство для преобразования текстовых символов в графический путь.

Вы уже видели, что вы можете обменять контуры текстовых символов и заполнять их. Это позволяет отображать эти контуры символов с определенной толщиной штриха и даже эффектом пути, как описано в статье [**эффекты пути**](effects.md) . Но также можно преобразовать символьную строку в `SKPath` объект. Это означает, что текстовые контуры можно использовать для обрезки с помощью методов, описанных в статье [**Обрезка с помощью путей и регионов**](clipping.md) .

Помимо использования эффекта контура для обводки контура символов, можно также создать эффекты контуров, основанные на пути, который является производным от символьной строки, и можно даже объединить два эффекта:

![](text-paths-images/pathsandtextsample.png "Text Path Effect")

В предыдущей статье, посвященной [**влиянию пути**](effects.md), вы увидели, как [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) метод `SKPaint` может получить контур контура с обводкой. Этот метод также можно использовать с путями, полученными из контуров символов.

Наконец, в этой статье показано другое пересечение путей и текста: [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) метод `SKCanvas` позволяет отобразить текстовую строку таким образом, чтобы базовая линия текста обозначает изогнутый путь.

## <a name="text-to-path-conversion"></a>Преобразование текста в путь

[`GetTextPath`](xref:SkiaSharp.SKPaint.GetTextPath(System.String,System.Single,System.Single))Метод `SKPaint` преобразования символьной строки в `SKPath` объект:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x`Аргументы и `y` указывают начальную точку базового плана левой части текста. Они играют ту же роль, что и в `DrawText` методе `SKCanvas` . В пределах контура базовая линия левой части текста будет иметь координаты (x, y).

`GetTextPath`Метод является избыточным, если нужно просто заполнить или обрисовать результирующий путь. `DrawText`Для этого используется стандартный метод. `GetTextPath`Метод более удобен для других задач, использующих пути.

Одна из этих задач обрезается. На странице **Обрезка текста** создается обтравочный контур на основе символов слова «Code». Этот путь растягивается до размера страницы для обрезки точечного рисунка, содержащего изображение исходного **текста обрезки** :

[![](text-paths-images/clippingtext-small.png "Triple screenshot of the Clipping Text page")](text-paths-images/clippingtext-large.png#lightbox "Triple screenshot of the Clipping Text page")

[`ClippingTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs)Конструктор класса загружает точечный рисунок, который хранится в виде внедренного ресурса в папке **мультимедиа** решения:

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

`PaintSurface`Обработчик начинается с создания объекта, `SKPaint` подходящего для текста. Свойство устанавливается так же, как и `Typeface` `TextSize` , хотя для этого конкретного приложения `TextSize` свойство является исключительно произвольным. Также обратите внимание, что `Style` параметр отсутствует.

`TextSize` `Style` Параметры свойств и не требуются, так как этот `SKPaint` объект используется исключительно для `GetTextPath` вызова с использованием текстовой строки "Code". Затем обработчик измеряет результирующий `SKPath` объект и применяет три преобразования для центрирования и масштабирования до размера страницы. Затем путь можно задать в качестве обтравочного пути:

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap,
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

После установки контура обрезки можно отобразить точечный рисунок, который будет обрезан до контуров символов. Обратите внимание на использование [`AspectFill`](xref:SkiaSharp.SKRect.AspectFill(SkiaSharp.SKSize)) метода `SKRect` , который вычисляет прямоугольник для заполнения страницы, сохраняя пропорции.

На странице **эффектов текстового пути** один символ амперсанда преобразуется в путь для создания результата одномерного пути. Объект Paint с этим результатом будет использоваться для обводки контура более крупной версии того же символа:

[![](text-paths-images/textpatheffect-small.png "Triple screenshot of the Text Path Effect page")](text-paths-images/textpatheffect-large.png#lightbox "Triple screenshot of the Text Path Effect page")

Большая часть работы в [`TextPathEffectPath`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) классе возникает в полях и конструкторе. Два `SKPaint` объекта, определенные как поля, используются в двух разных целях: первый (именованный `textPathPaint` ) используется для преобразования амперсанда с `TextSize` 50 в путь для результата одномерного пути. Вторая ( `textPaint` ) используется для вывода более крупной версии амперсанда с этим результатом. По этой причине для второго объекта Paint задается значение `Style` `Stroke` , но `StrokeWidth` свойство не задано, так как это свойство не требуется при использовании результата 1d-пути:

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds = new SKRect();
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character,
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

Конструктор сначала использует `textPathPaint` объект для измерения амперсанда с `TextSize` 50. Отрицательные координаты центральных координат этого прямоугольника передаются в `GetTextPath` метод для преобразования текста в путь. Результирующий путь содержит точку (0, 0) в центре символа, что идеально подходит для одномерного пути.

Можно подумать, что `SKPathEffect` объект, созданный в конце конструктора, может быть задан `PathEffect` свойством, `textPaint` а не сохранен как поле. Но это не работает очень хорошо, так как искажает результаты `MeasureText` вызова в `PaintSurface` обработчике:

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

Этот `MeasureText` вызов используется для центрирования символа на странице. Чтобы избежать проблем, `PathEffect` свойству задается объект Paint после измерения текста, но до его отображения.

## <a name="outlines-of-character-outlines"></a>Контуры символов

Обычно [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) метод `SKPaint` преобразует один путь в другой, применяя свойства Paint, в особенности толщину и воздействие штриха. При использовании без эффектов пути `GetFillPath` фактически создает путь, который описывает другой путь. Это было продемонстрировано при **касании страницы пути** в статье [**эффекты пути**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) .

Можно также вызвать `GetFillPath` по пути, возвращенному из, `GetTextPath` но сначала вы не сможете полностью убедиться, что это будет выглядеть.

Эта методика показана на странице « **контур структуры символов** ». Весь соответствующий код находится в `PaintSurface` обработчике [`CharacterOutlineOutlinesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) класса.

Конструктор начинается с создания `SKPaint` объекта `textPaint` с именем и `TextSize` свойством в зависимости от размера страницы. Это преобразование преобразуется в путь с помощью `GetTextPath` метода. Аргументы координат для `GetTextPath` эффективного центрирования пути на экране:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds = new SKRect();
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

`PaintSurface`Затем обработчик создает новый путь с именем `outlinePath` . Он преобразуется в целевой путь в вызове `GetFillPath` . `StrokeWidth`Свойство, состоящего из 25, приводит `outlinePath` к тому, что контур на уровне 25 пикселей обозначается обводками текстовых символов. Этот контур отображается красным цветом со штриховым штрихом, равным 5:

[![](text-paths-images/characteroutlineoutlines-small.png "Triple screenshot of the Character Outline Outlines page")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Triple screenshot of the Character Outline Outlines page")

Внимательно взгляните, и вы увидите перекрытие, где контур контура образует острый угол. Это обычные компоненты этого процесса.

## <a name="text-along-a-path"></a>Текст вдоль пути

Обычно текст отображается в горизонтальной базовой линии. Текст можно поворачивать, чтобы он выполнялся вертикально или по диагонали, но базовый план по-прежнему является прямой линией.

Однако иногда требуется, чтобы текст запускался вдоль кривой. Это назначение [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) метода `SKCanvas` :

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Текст, указанный в первом аргументе, будет выполнен по пути, указанному в качестве второго аргумента. Текст можно начинать с смещения, начиная с начала пути с `hOffset` аргументом. Обычно путь образует базовый уровень текста: два верхних элемента находятся на одной стороне пути, а текст по убыванию — в другом. Но можно сместить базовую базу текста по пути с `vOffset` аргументом.

Этот метод не предоставляет рекомендаций по установке `TextSize` свойства объекта для того, чтобы `SKPaint` Размер текста идеально выполнялся с начала пути до конца. Иногда можно определить размер текста самостоятельно. В других случаях потребуется использовать функции измерения пути, которые будут описаны в следующей статье, посвященной [**сведениям о пути и перечислению**](information.md).

**Круглая текстовая** программа заключает текст вокруг окружности. Можно легко определить длину окружности, чтобы размер текста можно было точно вписать. `PaintSurface`Обработчик [`CircularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) класса вычисляет радиус круга на основе размера страницы. Этот круг превращается в `circularPath` :

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

`TextSize`Свойство объекта `textPaint` корректируется таким образом, чтобы ширина текста совпадала с окружностью окружности:

[![](text-paths-images/circulartext-small.png "Triple screenshot of the Circular Text page")](text-paths-images/circulartext-large.png#lightbox "Triple screenshot of the Circular Text page")

Сам текст был выбран и в некоторой степени круга: слово «Circle» является как темой предложения, так и объектом предположенной фразы.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
