---
title: Преобразование переноса
description: В этой статье рассматривается использование преобразования «Преобразование» для сдвига SkiaSharp графики в Xamarin.Forms приложениях и демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0eb3b4a6b37d59363984c9248cc39de91a6819e0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138259"
---
# <a name="the-translate-transform"></a>Преобразование переноса

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Сведения об использовании преобразования "преобразование" для сдвига SkiaSharp графики_

Простейший тип преобразования в SkiaSharp — *Преобразование или* *перевод* . Это преобразование сдвигает графические объекты в горизонтальные и вертикальные направления. В определенном смысле, перевод является наиболее ненужным преобразованием, так как обычно можно добиться того же результата, просто изменив координаты, используемые в функции рисования. Однако при отрисовке пути все координаты инкапсулируются в пути, поэтому гораздо проще применить преобразование «миграция» для сдвига всего пути.

Перевод также полезен для анимации и для простых текстовых эффектов:

![](translate-images/translateexample.png "Text shadow, engraving, and embossing with translation")

[`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single))Метод в `SKCanvas` имеет два параметра, которые приводят к тому, что графические объекты будут сдвинуты по горизонтали и вертикали:

```csharp
public void Translate (Single dx, Single dy)
```

Эти аргументы могут быть отрицательными. Второй [`Translate`](xref:SkiaSharp.SKCanvas.Translate(SkiaSharp.SKPoint)) метод объединяет два значения перевода в одно `SKPoint` значение:

```csharp
public void Translate (SKPoint point)
```

На странице **накопленный перевод** примера программы [**скиашарпформс**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) показано, что несколько вызовов `Translate` метода являются кумулятивными. [`AccumulatedTranslatePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs)Класс отображает 20 версий одного и того же прямоугольника, каждое смещение от предыдущего прямоугольника достаточно просто для растяжения по диагонали. Вот `PaintSurface` обработчик событий:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Color = SKColors.Black;
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = 3;

        int rectangleCount = 20;
        SKRect rect = new SKRect(0, 0, 250, 250);
        float xTranslate = (info.Width - rect.Width) / (rectangleCount - 1);
        float yTranslate = (info.Height - rect.Height) / (rectangleCount - 1);

        for (int i = 0; i < rectangleCount; i++)
        {
            canvas.DrawRect(rect, strokePaint);
            canvas.Translate(xTranslate, yTranslate);
        }
    }
}
```

Последовательные прямоугольники тонкого вниз на странице:

[![](translate-images/accumulatedtranslate-small.png "Triple screenshot of the Accumulated Translate page")](translate-images/accumulatedtranslate-large.png#lightbox "Triple screenshot of the Accumulated Translate page")

Если накопленные коэффициенты перевода имеют значение `dx` и `dy` , а точка, указанная в функции рисования, имеет значение ( `x` , `y` ), то графический объект визуализируется в точке ( `x'` , `y'` ), где:

x "= x + DX

y "= y + DY

Они называются *формулами преобразования* для перевода. Значения по умолчанию `dx` `dy` для и для нового `SKCanvas` равны 0.

Обычно используется преобразование «миграция» для эффектов тени и аналогичных методов, как показано на странице « **перевод текстовых эффектов** ». Ниже приведена соответствующая часть `PaintSurface` обработчика в [`TranslateTextEffectsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) классе:

```csharp
float textSize = 150;

using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = textSize;
    textPaint.FakeBoldText = true;

    float x = 10;
    float y = textSize;

    // Shadow
    canvas.Translate(10, 10);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("SHADOW", x, y, textPaint);
    canvas.Translate(-10, -10);
    textPaint.Color = SKColors.Pink;
    canvas.DrawText("SHADOW", x, y, textPaint);

    y += 2 * textSize;

    // Engrave
    canvas.Translate(-5, -5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("ENGRAVE", x, y, textPaint);
    canvas.ResetMatrix();
    textPaint.Color = SKColors.White;
    canvas.DrawText("ENGRAVE", x, y, textPaint);

    y += 2 * textSize;

    // Emboss
    canvas.Save();
    canvas.Translate(5, 5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("EMBOSS", x, y, textPaint);
    canvas.Restore();
    textPaint.Color = SKColors.White;
    canvas.DrawText("EMBOSS", x, y, textPaint);
}
```

В каждом из трех примеров `Translate` вызывается для отображения текста, который будет смещен из расположения, заданного `x` `y` переменными и. Затем текст отображается еще раз в другом цвете без эффектов перевода:

[![](translate-images/translatetexteffects-small.png "Triple screenshot of the Translate Text Effects page")](translate-images/translatetexteffects-large.png#lightbox "Triple screenshot of the Translate Text Effects page")

В каждом из трех примеров показан другой способ отрицания `Translate` вызова:

Первый пример просто вызывает `Translate` повторно, но с отрицательными значениями. Поскольку `Translate` вызовы являются кумулятивными, эта последовательность вызовов просто восстанавливает Общий перевод до значений по умолчанию, равных нулю.

Во втором примере вызывается метод [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix) . Это приводит к тому, что все преобразования возвращаются к их состоянию по умолчанию.

В третьем примере состояние `SKCanvas` объекта сохраняется с вызовом метода [`Save`](xref:SkiaSharp.SKCanvas.Save) , а затем восстанавливается состояние с вызовом [`Restore`](xref:SkiaSharp.SKCanvas.Restore) . Это наиболее универсальный способ управления преобразованиями для ряда операций рисования. Эти `Save` `Restore` функции и вызывают функцию вроде стека: можно вызвать `Save` несколько раз, а затем вызвать `Restore` в обратной последовательности, чтобы вернуться к предыдущим состояниям. `Save`Метод возвращает целое число, и вы можете передать это целое число в, [`RestoreToCount`](xref:SkiaSharp.SKCanvas.RestoreToCount*) чтобы эффективно вызывать `Restore` несколько раз. [`SaveCount`](xref:SkiaSharp.SKCanvas.SaveCount)Свойство возвращает число состояний, сохраненных в стеке в данный момент.

Можно также использовать [`SKAutoCanvasRestore`](xref:SkiaSharp.SKAutoCanvasRestore) класс для восстановления состояния холста. Конструктор этого класса предназначен для вызова в `using` инструкции; состояние холста автоматически восстанавливается в конце `using` блока.

Однако нет необходимости беспокоиться о преобразованиях, которые применяют один вызов `PaintSurface` обработчика к следующему. Каждый новый вызов `PaintSurface` доставляет новые `SKCanvas` объекты с преобразованиями по умолчанию.

Другое распространенное применение `Translate` преобразования — визуализация визуального объекта, созданного с помощью координат, удобного для рисования. Например, можно указать координаты для аналоговых часов с центром в точке (0, 0). Затем можно использовать преобразования для показа часов там, где это необходимо. Этот метод показан на странице [**Хендекаграм Array**]. [`HendecagramArrayPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramArrayPage.cs)Класс начинается с создания `SKPath` объекта для 11-ориентированной звездочки. `HendecagramPath`Объект определен как открытый, статический и доступный только для чтения, поэтому к нему можно получить доступ из других демонстрационных программ. Он создается в статическом конструкторе:

```csharp
public class HendecagramArrayPage : ContentPage
{
    ...
    public static readonly SKPath HendecagramPath;

    static HendecagramArrayPage()
    {
        // Create 11-pointed star
        HendecagramPath = new SKPath();
        for (int i = 0; i < 11; i++)
        {
            double angle = 5 * i * 2 * Math.PI / 11;
            SKPoint pt = new SKPoint(100 * (float)Math.Sin(angle),
                                    -100 * (float)Math.Cos(angle));
            if (i == 0)
            {
                HendecagramPath.MoveTo(pt);
            }
            else
            {
                HendecagramPath.LineTo(pt);
            }
        }
        HendecagramPath.Close();
    }
}
```

Если центр звезды является точкой (0, 0), все точки звезды находятся на круге, окружающем эту точку. Каждая точка представляет собой сочетание синусов и косинусов угла, увеличивающееся на 5 и 11 360 градусов. (Можно также создать 11-дюймовую звезду, увеличив угол на 2/11, 3/11 или 4/11 в кружке.) Радиус этого круга устанавливается как 100.

Если этот путь отображается без преобразований, центр будет размещается в левом верхнем углу `SKCanvas` , и будет виден только квартал. `PaintSurface` `HendecagramPage` Вместо этого обработчик использует `Translate` для мозаичного заполнения холста несколькими копиями звезды, каждый из которых имеет случайное цветное значение:

```csharp
public class HendecagramArrayPage : ContentPage
{
    Random random = new Random();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            for (int x = 100; x < info.Width + 100; x += 200)
                for (int y = 100; y < info.Height + 100; y += 200)
                {
                    // Set random color
                    byte[] bytes = new byte[3];
                    random.NextBytes(bytes);
                    paint.Color = new SKColor(bytes[0], bytes[1], bytes[2]);

                    // Display the hendecagram
                    canvas.Save();
                    canvas.Translate(x, y);
                    canvas.DrawPath(HendecagramPath, paint);
                    canvas.Restore();
                }
        }
    }
}

```

Ниже приведен результат.

[![](translate-images/hendecagramarray-small.png "Triple screenshot of the Hendecagram Array page")](translate-images/hendecagramarray-large.png#lightbox "Triple screenshot of the Hendecagram Array page")

Анимации часто подразумевают преобразования. Страница **анимации хендекаграм** перемещает звезду, указывающую на 11, вокруг круга. [`HendecagramAnimationPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs)Класс начинается с некоторых полей и переопределений `OnAppearing` `OnDisappearing` методов и для запуска и завершения Xamarin.Forms таймера:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    const double cycleTime = 5000;      // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float angle;

    public HendecagramAnimationPage()
    {
        Title = "Hedecagram Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            angle = (float)(360 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`angle`Поле анимируется от 0 градусов до 360 градусов каждые 5 секунд. `PaintSurface`Обработчик использует `angle` свойство двумя способами: для указания оттенка цвета в `SKColor.FromHsl` методе и в качестве аргумента для `Math.Sin` методов и, `Math.Cos` чтобы управлять расположением звезды:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.Translate(info.Width / 2, info.Height / 2);
        float radius = (float)Math.Min(info.Width, info.Height) / 2 - 100;

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColor.FromHsl(angle, 100, 50);

            float x = radius * (float)Math.Sin(Math.PI * angle / 180);
            float y = -radius * (float)Math.Cos(Math.PI * angle / 180);
            canvas.Translate(x, y);
            canvas.DrawPath(HendecagramPage.HendecagramPath, paint);
        }
    }
}
```

`PaintSurface`Обработчик вызывает `Translate` метод дважды, сначала для перевода в центр холста, а затем для преобразования окружности окружности вокруг (0,0). Радиус круга установлен как возможный, и по-прежнему удерживает звездочку в пределах страницы:

[![](translate-images/hendecagramanimation-small.png "Triple screenshot of the Hendecagram Animation page")](translate-images/hendecagramanimation-large.png#lightbox "Triple screenshot of the Hendecagram Animation page")

Обратите внимание, что звезда поддерживает ту же ориентацию, что и вокруг центра страницы. Он не вращается вообще. Это задание для преобразования «вращение».

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
