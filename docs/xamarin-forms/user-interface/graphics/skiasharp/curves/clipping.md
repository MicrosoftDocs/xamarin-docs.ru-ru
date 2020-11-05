---
title: Обрезка изображения по границам области с помощью путей
description: В этой статье объясняется, как использовать SkiaSharpные пути для обрезки изображений в определенные области и для создания регионов, а также демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 79c32595053cc80ffae91b5434e18690ca2dce4a
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370719"
---
# <a name="clipping-with-paths-and-regions"></a>Обрезка изображения по границам области с помощью путей

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Использование контуров для обрезки изображений в определенные области и для создания регионов_

Иногда требуется ограничить отрисовку графики в определенной области. Это называется *обрезанием*. Обрезка можно использовать для специальных эффектов, таких как изображение обезьяны, видимой через кэйхоле:

![Обезьяна через кэйхоле](clipping-images/clippingsample.png)

*Область обрезки* — это область экрана, в которой визуализируются графические объекты. Все, что отображается за пределами области обрезки, не отображается. Область обрезки обычно определяется прямоугольником или [`SKPath`](xref:SkiaSharp.SKPath) объектом, но можно также определить область обрезки с помощью [`SKRegion`](xref:SkiaSharp.SKRegion) объекта. Эти два типа объектов в первый взгляд связаны с тем, что можно создать регион из пути. Однако нельзя создать путь из области, и они сильно отличаются внутри: контур состоит из ряда линий и кривых, а область — с помощью ряда горизонтальных строк развертки.

Приведенный выше образ был создан **обезьяной через страницу кэйхоле** . [`MonkeyThroughKeyholePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs)Класс определяет путь, использующий данные SVG, и использует конструктор для загрузки точечного рисунка из ресурсов программы:

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    SKBitmap bitmap;
    SKPath keyholePath = SKPath.ParseSvgPathData(
        "M 300 130 L 250 350 L 450 350 L 400 130 A 70 70 0 1 0 300 130 Z");

    public MonkeyThroughKeyholePage()
    {
        Title = "Monkey through Keyhole";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

Несмотря на то `keyholePath` , что объект описывает контур кэйхоле, координаты являются совершенно произвольными и показывают, что было удобно при создании данных пути. По этой причине `PaintSurface` обработчик получает границы этого пути и вызовы `Translate` и `Scale` перемещает путь в центр экрана и делает его почти так же, как и на экране:

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transform to center and enlarge clip path to window height
        SKRect bounds;
        keyholePath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.98f * info.Height / bounds.Height);
        canvas.Translate(-bounds.MidX, -bounds.MidY);

        // Set the clip path
        canvas.ClipPath(keyholePath);

        // Reset transforms
        canvas.ResetMatrix();

        // Display monkey to fill height of window but maintain aspect ratio
        canvas.DrawBitmap(bitmap,
            new SKRect((info.Width - info.Height) / 2, 0,
                       (info.Width + info.Height) / 2, info.Height));
    }
}
```

Но путь не подготавливается к просмотру. Вместо этого после преобразований путь используется для задания области отсечения с помощью этой инструкции:

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface`Затем обработчик сбрасывает преобразования с помощью вызова `ResetMatrix` и Рисует точечный рисунок для расширения до полной высоты экрана. В этом коде предполагается, что точечный рисунок имеет квадратный цвет, а это конкретное точечное изображение. Растровое изображение подготавливается к просмотру только в области, определенной путем обрезки:

[![Тройной снимок экрана обезьяны на странице Кэйхоле](clipping-images/monkeythroughkeyhole-small.png)](clipping-images/monkeythroughkeyhole-large.png#lightbox)

Путь обрезки подчиняется преобразованиям, которые применяются при `ClipPath` вызове метода, а не на преобразованиях, действующих при отображении графического объекта (например, точечного рисунка). Обтравочный контур является частью состояния холста, который сохраняется вместе с `Save` методом и восстанавливается с помощью `Restore` метода.

## <a name="combining-clipping-paths"></a>Объединение контуров обрезки

Строго говоря, область обрезки не задается `ClipPath` методом. Вместо этого он объединяется с существующим контуром обрезки, который начинается как прямоугольник, равный размеру холста. Прямоугольные границы области обрезки можно получить с помощью [`ClipBounds`](xref:SkiaSharp.SKCanvas.ClipBounds) свойства или свойства [`ClipDeviceBounds`](xref:SkiaSharp.SKCanvas.ClipDeviceBounds) . `ClipBounds`Свойство возвращает `SKRect` значение, отражающее все преобразования, которые могут быть применены. `ClipDeviceBounds`Свойство возвращает `RectI` значение. Это прямоугольник с целочисленными измерениями, который описывает область обрезки в реальных измерениях в пикселях.

Любой вызов `ClipPath` сокращает область обрезки путем объединения области обрезки с новой областью. Полный синтаксис [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath(SkiaSharp.SKPath,SkiaSharp.SKClipOperation,System.Boolean)) метода:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Существует также [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect(SkiaSharp.SKRect,SkiaSharp.SKClipOperation,System.Boolean)) метод, объединяющий область обрезки с прямоугольником:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

По умолчанию результирующая область обрезки представляет собой пересечение существующей области обрезки и объекта `SKPath` или `SKRect` , указанного в `ClipPath` `ClipRect` методе или. Это продемонстрировано на **четвертой странице клипа-пересечения кругов** . `PaintSurface`Обработчик в [`FourCircleInteresectClipPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) классе повторно использует тот же `SKPath` объект для создания четырех перекрывающихся кругов, каждый из которых сокращает область обрезки посредством последовательных вызовов `ClipPath` :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float size = Math.Min(info.Width, info.Height);
    float radius = 0.4f * size;
    float offset = size / 2 - radius;

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    using (SKPath path = new SKPath())
    {
        path.AddCircle(-offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(-offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColors.Blue;
            canvas.DrawPaint(paint);
        }
    }
}
```

Слева находится пересечение этих четырех кругов:

[![Тройной снимок экрана с четырьмя пересекающимися кругами страниц](clipping-images//fourcircleintersectclip-small.png)](clipping-images/fourcircleintersectclip-large.png#lightbox)

[`SKClipOperation`](xref:SkiaSharp.SKClipOperation)Перечисление имеет только два члена:

- `Difference` Удаляет указанный путь или прямоугольник из существующей области обрезки

- `Intersect` пересекает указанный путь или прямоугольник с существующей областью отсечения

Если заменить четыре `SKClipOperation.Intersect` аргумента в классе на `FourCircleIntersectClipPage` `SKClipOperation.Difference` , вы увидите следующее:

[![Тройной снимок экрана 4-пересекающаяся страница клипа с операцией разности](clipping-images//fourcircledifferenceclip-small.png)](clipping-images/fourcircledifferenceclip-large.png#lightbox)

Из области обрезки были удалены четыре перекрывающихся кружков.

На странице **операции клипа** показано различие между этими двумя операциями только парой кругов. Первый кружок слева добавляется в область обрезки с помощью операции обрезания по умолчанию `Intersect` , а второй кружок справа добавляется в область обрезки с помощью операции Clip, обозначенной текстовой меткой:

[![Тройной снимок экрана страницы "операции с клипами"](clipping-images//clipoperations-small.png)](clipping-images/clipoperations-large.png#lightbox)

[`ClipOperationsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs)Класс определяет два `SKPaint` объекта как поля, а затем разделяет экран на две прямоугольные области. Эти области различаются в зависимости от того, находится ли телефон в режиме книжной или альбомной ориентации. `DisplayClipOp`Затем класс отображает текст и вызовы `ClipPath` с двумя контурами круга, чтобы продемонстрировать каждую операцию с клипами:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;

    foreach (SKClipOperation clipOp in Enum.GetValues(typeof(SKClipOperation)))
    {
        // Portrait mode
        if (info.Height > info.Width)
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width, y + info.Height / 2), clipOp);
            y += info.Height / 2;
        }
        // Landscape mode
        else
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width / 2, y + info.Height), clipOp);
            x += info.Width / 2;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKClipOperation clipOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(clipOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    canvas.Save();

    using (SKPath path1 = new SKPath())
    {
        path1.AddCircle(xCenter - radius / 2, yCenter, radius);
        canvas.ClipPath(path1);

        using (SKPath path2 = new SKPath())
        {
            path2.AddCircle(xCenter + radius / 2, yCenter, radius);
            canvas.ClipPath(path2, clipOp);

            canvas.DrawPaint(fillPaint);
        }
    }

    canvas.Restore();
}
```

Вызов, `DrawPaint` как правило, приводит к заполнению всего холста этим `SKPaint` объектом, но в данном случае метод просто закрашивается внутри области обрезки.

## <a name="exploring-regions"></a>Исследование регионов

Можно также определить область обрезки с точки зрения [`SKRegion`](xref:SkiaSharp.SKRegion) объекта.

Вновь созданный `SKRegion` объект описывает пустую область. Обычно первый вызов объекта заключается в том [`SetRect`](xref:SkiaSharp.SKRegion.SetRect(SkiaSharp.SKRectI)) , что область описывает прямоугольную область. Параметр `SetRect` — это `SKRectI` значение &mdash; прямоугольника с целыми координатами, так как оно указывает прямоугольник в виде пикселей. Затем можно вызвать [`SetPath`](xref:SkiaSharp.SKRegion.SetPath(SkiaSharp.SKPath,SkiaSharp.SKRegion)) с помощью `SKPath` объекта. При этом создается область, которая совпадает с внутренней частью пути, но обрезается по исходной прямоугольной области.

Регион также можно изменить, вызвав одну из [`Op`](xref:SkiaSharp.SKRegion.Op*) перегрузок метода, например:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

[`SKRegionOperation`](xref:SkiaSharp.SKRegionOperation)Перечисление аналогично, `SKClipOperation` но имеет больше элементов:

- `Difference`

- `Intersect`

- `Union`

- `XOR`

- `ReverseDifference`

- `Replace`

Регион, в котором выполняется вызов, `Op` объединяется с регионом, указанным в качестве параметра на основе `SKRegionOperation` элемента. Когда вы наконец получаете область, подходящую для обрезки, вы можете задать ее в качестве области обрезки с помощью [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion(SkiaSharp.SKRegion,SkiaSharp.SKClipOperation)) метода `SKCanvas` :

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

На следующем снимке экрана показаны области обрезки, основанные на шести операциях региона. Левый кружок — это регион, в котором `Op` вызывается метод, а правый кружок — это регион, передаваемый `Op` методу:

[![Тройной снимок экрана страницы "операции с регионами"](clipping-images//regionoperations-small.png)](clipping-images/regionoperations-large.png#lightbox)

Существуют ли все возможности объединения этих двух кругов? Рассмотрим итоговый образ как сочетание трех компонентов, которые сами по себе отображаются в `Difference` `Intersect` `ReverseDifference` операциях, и. Общее число комбинаций равно двум третьим степеням или восьми. Отсутствующие два из них — исходный регион (который не вызывается вообще `Op` ) и полностью пустой регион.

Для обрезки труднее использовать регионы, так как сначала необходимо создать путь, а затем регион из этого пути, а затем объединить несколько регионов. Общая структура страницы операций с **областями** очень похожа на **операции Clip** , но [`RegionOperationsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) класс разделяет экран на шесть областей и показывает дополнительную работу, необходимую для использования регионов для этого задания:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;
    float width = info.Height > info.Width ? info.Width / 2 : info.Width / 3;
    float height = info.Height > info.Width ? info.Height / 3 : info.Height / 2;

    foreach (SKRegionOperation regionOp in Enum.GetValues(typeof(SKRegionOperation)))
    {
        DisplayClipOp(canvas, new SKRect(x, y, x + width, y + height), regionOp);

        if ((x += width) >= info.Width)
        {
            x = 0;
            y += height;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKRegionOperation regionOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(regionOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    SKRectI recti = new SKRectI((int)rect.Left, (int)rect.Top,
                                (int)rect.Right, (int)rect.Bottom);

    using (SKRegion wholeRectRegion = new SKRegion())
    {
        wholeRectRegion.SetRect(recti);

        using (SKRegion region1 = new SKRegion(wholeRectRegion))
        using (SKRegion region2 = new SKRegion(wholeRectRegion))
        {
            using (SKPath path1 = new SKPath())
            {
                path1.AddCircle(xCenter - radius / 2, yCenter, radius);
                region1.SetPath(path1);
            }

            using (SKPath path2 = new SKPath())
            {
                path2.AddCircle(xCenter + radius / 2, yCenter, radius);
                region2.SetPath(path2);
            }

            region1.Op(region2, regionOp);

            canvas.Save();
            canvas.ClipRegion(region1);
            canvas.DrawPaint(fillPaint);
            canvas.Restore();
        }
    }
}
```

Ниже приведена большая разница между `ClipPath` методом и `ClipRegion` методом.

> [!IMPORTANT]
> В отличие от `ClipPath` метода, `ClipRegion` Преобразование не влияет на метод.

Чтобы понять причину этой разницы, полезно понять, что такое регион. Если вы подумали о том, как операции с клипами или области могут быть реализованы внутренним образом, это, вероятно, кажется очень сложным. Несколько потенциально очень сложных путей объединяются, а структура результирующего пути, скорее всего, является алгоритмным кошмаром.

Это задание значительно упрощается, если каждый путь сокращается до ряда горизонтальных строк развертки, например в старых ТВ-лампах чистильщика. Каждая строка сканирования — это просто горизонтальная линия с начальной и конечной точками. Например, круг с радиусом 10 пикселов можно разложить на 20 горизонтальных строк развертки, каждая из которых начинается с левой части круга и заканчивается в правой части. Объединение двух кругов с любой операцией региона стало очень простым, так как это просто вопрос проверки начальной и конечной координат каждой пары соответствующих строк развертки.

Это область: ряд горизонтальных линий просмотра, определяющих область.

Однако при уменьшении области до ряда строк сканирования эти строки просмотра основываются на определенном измерении в пикселях. Строго говоря, регион не является объектом векторной графики. Это ближе к сжатому монохромному точечному рисунку, чем к пути. Соответственно, области нельзя масштабировать или поворачивать без потери точности, и по этой причине они не преобразуются при использовании для области обрезки.

Однако можно применять преобразования к регионам для рисования. Программа **рисования региона** наглядно демонстрирует внутреннюю природу регионов. [`RegionPaintPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs)Класс создает `SKRegion` объект на основе `SKPath` 10-модульного круга радиуса. Затем преобразование расширяет круг, чтобы заполнить страницу:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int radius = 10;

    // Create circular path
    using (SKPath circlePath = new SKPath())
    {
        circlePath.AddCircle(0, 0, radius);

        // Create circular region
        using (SKRegion circleRegion = new SKRegion())
        {
            circleRegion.SetRect(new SKRectI(-radius, -radius, radius, radius));
            circleRegion.SetPath(circlePath);

            // Set transform to move it to center and scale up
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(Math.Min(info.Width / 2, info.Height / 2) / radius);

            // Fill region
            using (SKPaint fillPaint = new SKPaint())
            {
                fillPaint.Style = SKPaintStyle.Fill;
                fillPaint.Color = SKColors.Orange;

                canvas.DrawRegion(circleRegion, fillPaint);
            }

            // Stroke path for comparison
            using (SKPaint strokePaint = new SKPaint())
            {
                strokePaint.Style = SKPaintStyle.Stroke;
                strokePaint.Color = SKColors.Blue;
                strokePaint.StrokeWidth = 0.1f;

                canvas.DrawPath(circlePath, strokePaint);
            }
        }
    }
}
```

`DrawRegion`Вызов заполняет область оранжевым цветом, в то время как `DrawPath` вызов обводки исходного контура синим для сравнения:

[![Тройной снимок экрана страницы "область рисования"](clipping-images//regionpaint-small.png)](clipping-images/regionpaint-large.png#lightbox)

Область является явной серией дискретных координат.

Если не нужно использовать преобразования в связи с областями обрезки, можно использовать регионы для обрезки, так как клевера страница с **четырьмя листами** . [`FourLeafCloverPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs)Класс создает составную область из четырех циклических областей, задает этот составной регион в качестве вырезанной области, а затем рисует ряд 360 прямых линий, оптимизации из центра страницы:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float xCenter = info.Width / 2;
    float yCenter = info.Height / 2;
    float radius = 0.24f * Math.Min(info.Width, info.Height);

    using (SKRegion wholeScreenRegion = new SKRegion())
    {
        wholeScreenRegion.SetRect(new SKRectI(0, 0, info.Width, info.Height));

        using (SKRegion leftRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion rightRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion topRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion bottomRegion = new SKRegion(wholeScreenRegion))
        {
            using (SKPath circlePath = new SKPath())
            {
                // Make basic circle path
                circlePath.AddCircle(xCenter, yCenter, radius);

                // Left leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, 0));
                leftRegion.SetPath(circlePath);

                // Right leaf
                circlePath.Transform(SKMatrix.MakeTranslation(2 * radius, 0));
                rightRegion.SetPath(circlePath);

                // Make union of right with left
                leftRegion.Op(rightRegion, SKRegionOperation.Union);

                // Top leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, -radius));
                topRegion.SetPath(circlePath);

                // Combine with bottom leaf
                circlePath.Transform(SKMatrix.MakeTranslation(0, 2 * radius));
                bottomRegion.SetPath(circlePath);

                // Make union of top with bottom
                bottomRegion.Op(topRegion, SKRegionOperation.Union);

                // Exclusive-OR left and right with top and bottom
                leftRegion.Op(bottomRegion, SKRegionOperation.XOR);

                // Set that as clip region
                canvas.ClipRegion(leftRegion);

                // Set transform for drawing lines from center
                canvas.Translate(xCenter, yCenter);

                // Draw 360 lines
                for (double angle = 0; angle < 360; angle++)
                {
                    float x = 2 * radius * (float)Math.Cos(Math.PI * angle / 180);
                    float y = 2 * radius * (float)Math.Sin(Math.PI * angle / 180);

                    using (SKPaint strokePaint = new SKPaint())
                    {
                        strokePaint.Color = SKColors.Green;
                        strokePaint.StrokeWidth = 2;

                        canvas.DrawLine(0, 0, x, y, strokePaint);
                    }
                }
            }
        }
    }
}
```

Он не выглядит как клевера с четырьмя листами, но это изображение, которое в противном случае может быть трудно визуализировать без усечения:

[![Тройной снимок экрана страницы Four-Leaf клевера](clipping-images//fourleafclover-small.png)](clipping-images/fourleafclover-large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)