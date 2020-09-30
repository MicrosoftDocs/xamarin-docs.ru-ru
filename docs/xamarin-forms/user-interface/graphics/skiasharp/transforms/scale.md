---
title: Преобразование масштаба
description: В статье СХИС рассматривается преобразование масштабирования SkiaSharp для масштабирования объектов на различные размеры и демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5ea4fa88925925ba7c81f47c30875c3419603ab8
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91555467"
---
# <a name="the-scale-transform"></a>Преобразование масштаба

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Обнаружение преобразования масштабирования SkiaSharp для масштабирования объектов до различных размеров_

Как видно из статьи [**Преобразование преобразований**](translate.md) , преобразование «миграция» может перемещать графический объект из одного расположения в другое. В отличие от этого, преобразование масштабирования изменяет размер графического объекта:

![Высокое масштабирование слова по размеру](scale-images/scaleexample.png)

Преобразование масштаба также часто приводит к тому, что координаты графики будут перемещаться по мере увеличения.

Ранее вы видели две формулы преобразования, описывающие влияние коэффициентов перевода на `dx` и `dy` :

x "= x + DX

y "= y + DY

Коэффициенты масштабирования `sx` и `sy` являются мультипликативные, а не аддитивными:

x "= SX · x

y "= SY · &

По умолчанию коэффициенты преобразования имеют значение 0; по умолчанию коэффициенты масштабирования имеют значение 1.

`SKCanvas`Класс определяет четыре `Scale` метода. Первый [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single)) метод предназначен для случаев, когда требуется один и тот же коэффициент горизонтального и вертикального масштабирования:

```csharp
public void Scale (Single s)
```

Это называется *исотропик* масштабированием масштабирования &mdash; в обоих направлениях. Масштабирование исотропик сохраняет пропорции объекта.

Второй [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) метод позволяет указать различные значения для горизонтального и вертикального масштабирования:

```csharp
public void Scale (Single sx, Single sy)
```

Это приводит к некоторому *масштабированию* .
Третий [`Scale`](xref:SkiaSharp.SKCanvas.Scale(SkiaSharp.SKPoint)) метод объединяет два коэффициента масштабирования в одно `SKPoint` значение:

```csharp
public void Scale (SKPoint size)
```

Четвертый `Scale` метод будет описан в ближайшее время.

На странице " **базовый масштаб** " показан `Scale` метод. Файл [**басикскалепаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) содержит два `Slider` элемента, которые позволяют выбрать коэффициенты горизонтального и вертикального масштабирования от 0 до 10. Файл кода программной части [**BasicScalePage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) использует эти значения для вызова `Scale` перед отображением скругленного прямоугольника, обозначенного пунктирной линией, и размера, чтобы вместить некоторый текст в левом верхнем углу холста:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Может возникнуть вопрос: как коэффициенты масштабирования влияют на значение, возвращаемое `MeasureText` методом `SKPaint` ? Ответ: не все. `Scale` является методом `SKCanvas` . Он не влияет на все действия с `SKPaint` объектом, пока вы не используете этот объект для визуализации чего-либо на холсте.

Как видите, все, что отображается после `Scale` вызова, увеличивается пропорционально:

[![Тройной снимок экрана страницы "базовый масштаб"](scale-images/basicscale-small.png)](scale-images/basicscale-large.png#lightbox "Тройной снимок экрана страницы "базовый масштаб"")

Текст, ширина пунктирной линии, длина дефисов в этой строке, округление углов и 10-пиксельное поле между левым и верхним краями холста и скругленным прямоугольником имеют одинаковые коэффициенты масштабирования.

> [!IMPORTANT]
> Универсальная платформа Windows не отображает правильно масштабируемый текст анисотропикли.

Анизотропное масштабирование приводит к тому, что ширина штриха будет отличаться для линий, выровненных по горизонтальной и вертикальной осям. (Это также очевидно из первого изображения на этой странице.) Если коэффициенты масштабирования не должны влиять на толщину штриха, задайте для него значение 0, и это всегда будет иметь ширину в один пиксель, независимо от `Scale` значения параметра.

Масштабирование происходит относительно верхнего левого угла холста. Это может быть именно то, что вам нужно, но это может быть не так. Предположим, что вы хотите разместить текст и прямоугольник в другом месте на холсте и масштабировать его относительно центра. В этом случае можно использовать четвертую версию [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) метода, которая включает два дополнительных параметра для указания центра масштабирования:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px`Параметры и `py` определяют точку, которая иногда называется *центром масштабирования* , но в документации SkiaSharp называется *точкой вращения*. Это точка относительно верхнего левого угла холста, на который не влияет масштабирование. Все масштабирование происходит относительно этого центра.

На странице [**центрированный масштаб**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) показано, как это работает. `PaintSurface`Обработчик похож на **обычную программу масштабирования** , за исключением того, что `margin` значение вычисляется для центрирования текста по горизонтали, что означает, что программа лучше работает в книжном режиме:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Левый верхний угол скругленного прямоугольника располагается в `margin` пикселях слева от холста и `margin` пикселов сверху. Два последних аргумента для `Scale` метода устанавливаются в эти значения плюс ширину и высоту текста, а также ширину и высоту скругленного прямоугольника. Это означает, что масштабирование происходит относительно центра этого прямоугольника:

[![Тройной снимок экрана со страницей центрированного масштабирования](scale-images/centeredscale-small.png)](scale-images/centeredscale-large.png#lightbox "Тройной снимок экрана со страницей центрированного масштабирования")

`Slider`Элементы в этой программе имеют диапазон от &ndash; 10 до 10. Как видите, отрицательные значения вертикального масштабирования (например, на экране Android в центре) приводят к тому, что объекты переворачиваются вокруг горизонтальной оси, проходящей через центр масштабирования. Отрицательные значения горизонтального масштабирования (например, на экране UWP справа) приводят к тому, что объекты переворачиваются вокруг вертикальной оси, проходящей через центр масштабирования.

Версия [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single,System.Single,System.Single)) метода с точками вращения является ярлыком для последовательности из трех `Translate` `Scale` вызовов и. Возможно, вы захотите увидеть, как это работает, заменив `Scale` метод на странице **центрированный масштаб** следующим образом:

```csharp
canvas.Translate(-px, -py);
```

Это отрицательные значения координат точек вращения.

Запустите программу повторно. Вы увидите, что прямоугольник и текст сдвигаются, так что центр находится в левом верхнем углу холста. Его можно увидеть очень едва. Эти ползунки не работают, так как теперь программа не масштабируется вообще.

Теперь добавьте базовый `Scale` вызов (без центра масштабирования) *перед* `Translate` вызовом:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Если вы знакомы с этим упражнением в других системах программирования графики, то можете подумать, что это не так, но не так. СКИА обрабатывает последовательные вызовы, немного иначе, чем вы можете быть знакомы.

После последовательного `Scale` и `Translate` вызова центр скругленного прямоугольника остается в левом верхнем углу, но теперь его можно масштабировать относительно верхнего левого угла холста, который также является центром скругленного прямоугольника.

Теперь перед этим `Scale` вызовом добавьте еще один `Translate` вызов с центральными значениями:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

При этом масштабируемый результат перемещается обратно в исходное расположение. Эти три вызова эквивалентны следующим:

```csharp
canvas.Scale(sx, sy, px, py);
```

Отдельные преобразования являются составными, поэтому формула общего преобразования:

 x "= SX · (x – px) + px

 y "= SY · (y – корректировка) + корректировка

Помните, что значения по умолчанию `sx` и `sy` равны 1. Можно легко убедить, что точка вращения (px, корректировка) не преобразуется в эти формулы. Он остается в том же расположении, что и холст.

При объединении `Translate` и `Scale` вызовах порядок важен. Если `Translate` после `Scale` , коэффициенты перевода эффективно масштабируются по коэффициентам масштабирования. Если значение `Translate` предшествует `Scale` , коэффициенты перевода не масштабируются. Этот процесс становится более четким (хотя и более математическим) при введении темы матриц преобразования.

`SKPath`Класс определяет свойство, доступное только [`Bounds`](xref:SkiaSharp.SKPath.Bounds) для чтения, которое возвращает значение, `SKRect` определяющее экстент координат в пути. Например, если `Bounds` свойство получено из пути хендекаграм, созданного ранее, `Left` `Top` Свойства и прямоугольника приблизительно – 100, а свойства и — приблизительно `Right` `Bottom` 100, а `Width` `Height` Свойства и — примерно 200. (Большинство фактических значений немного меньше, поскольку точки звезды определяются кругом с радиусом 100, но только верхняя точка параллельна горизонтальной или вертикальной осями.)

Доступность этих сведений подразумевает, что можно получить коэффициенты масштабирования и преобразования, которые подходят для масштабирования пути до размера холста. На странице [**анизотропное масштабирование**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) это показано с помощью 11-дюймовой звезды. *Анизотропная* шкала означает, что она не равна в горизонтальном и вертикальном направлениях, что означает, что звездочка не будет сохранять исходные пропорции. Ниже приведен соответствующий код в `PaintSurface` обработчике:

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

`pathBounds`Прямоугольник получается в верхней части этого кода, а затем используется позже с шириной и высотой холста в `Scale` вызове. Этот вызов будет масштабировать координаты пути, когда он визуализируется `DrawPath` вызовом, но звезда будет центрирована в правом верхнем углу холста. Его необходимо сдвинуть вниз и влево. Это задание `Translate` вызова. Это два свойства `pathBounds` приблизительно – 100, поэтому коэффициенты перевода — около 100. Поскольку `Translate` вызов происходит после `Scale` вызова, эти значения эффективно масштабируются по коэффициентам масштабирования, поэтому они перемещают центр звезды в центр холста:

[![Тройной снимок экрана страницы "анизотропное масштабирование"](scale-images/anisotropicscaling-small.png)](scale-images/anisotropicscaling-large.png#lightbox "Тройной снимок экрана страницы "анизотропное масштабирование"")

Кроме того, можно подумать о `Scale` `Translate` вызовах и, чтобы определить воздействие в обратную последовательность: `Translate` вызов смещает путь, чтобы он стал полностью видимым, но ориентированным в левом верхнем углу холста. `Scale`Затем метод делает эту звезду больше относительно левого верхнего угла.

На самом деле, это кажется, что звезда немного больше холста. Проблема — толщина штриха. `Bounds`Свойство объекта `SKPath` указывает размеры координат, закодированных в пути, и именно то, что программа использует для масштабирования. При отрисовке пути с определенной толщиной обводки отображаемый путь больше холста.

Чтобы устранить эту проблему, необходимо компенсировать ее. Одним из простых подходов в этой программе является добавление следующей инструкции непосредственно перед `Scale` вызовом:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Это увеличит `pathBounds` прямоугольник на 1,5 единиц на всех четырех сторонах. Это разумное решение, только если линия объединения штрихов округляется. Угловое соединение может быть длинным и трудным для вычисления.

Аналогичную методику также можно использовать с текстом, как показано на странице **анизотропный текст** . Ниже приведена соответствующая часть `PaintSurface` обработчика из [`AnisotropicTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) класса:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

Это аналогичная логика, а текст расширяется до размера страницы на основе границ текста, возвращаемых прямоугольником `MeasureText` (что немного больше, чем фактический текст):

[![Тройной снимок экрана страницы "анизотропное тестирование"](scale-images/anisotropictext-small.png)](scale-images/anisotropictext-large.png#lightbox "Тройной снимок экрана страницы "анизотропное тестирование"")

Если необходимо сохранить пропорции графических объектов, необходимо использовать масштабирование исотропик. На странице **масштабирование исотропик** показано это для 11-ориентированной звезды. По сути, шаги для отображения графического объекта в центре страницы с масштабированием исотропик:

- Перевести центр графического объекта в верхний левый угол.
- Масштабировать объект на основе минимума горизонтальных и вертикальных размеров страницы, деленную на размеры графических объектов.
- Перевести центр масштабируемого объекта в центр страницы.

[`IsotropicScalingPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs)Перед отображением звездочки выполняет следующие действия в порядке.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

Код также отображает звезду 10 больше раз, каждый раз уменьшая коэффициент масштабирования на 10% и постепенно изменяя цвет с красного на синий:

[![Тройной снимок экрана со страницей масштабирования Исотропик](scale-images/isotropicscaling-small.png)](scale-images/isotropicscaling-large.png#lightbox "Тройной снимок экрана со страницей масштабирования Исотропик")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)