---
title: Интеграция текста и графики
description: В этой статье объясняется, как определить размер отображаемой текстовой строки для интеграции текста с SkiaSharp графикой в Xamarin.Forms приложения и демонстрируется в примере кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 032a01a1e4e0f2b3e3d394aec6a30bd215fd84f8
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562422"
---
# <a name="integrating-text-and-graphics"></a>Интеграция текста и графики

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_См. раздел Определение размера отображаемой текстовой строки для интеграции текста с SkiaSharp графикой._

В этой статье показано, как измерять текст, масштабировать текст до определенного размера и интегрировать текст с другими рисунками:

![Текст, окруженный прямоугольниками](text-images/textandgraphicsexample.png)

Этот образ также содержит скругленный прямоугольник. Класс SkiaSharp `Canvas` содержит [`DrawRect`](xref:SkiaSharp.SKCanvas.DrawRect*) методы для рисования прямоугольника и [`DrawRoundRect`](xref:SkiaSharp.SKCanvas.DrawRoundRect*) методов для рисования прямоугольника со скругленными углами. Эти методы позволяют прямоугольнику определяться как `SKRect` значение или другими способами.

**Текстовая страница в рамке** выравнивает короткую текстовую строку на странице и окружает ее рамкой, состоящей из пары скругленных прямоугольников. [`FramedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs)Класс показывает, как это делается.

В SkiaSharp `SKPaint` класс используется для задания атрибутов текста и шрифта, но его также можно использовать для получения отображаемого размера текста. Начало следующего `PaintSurface` обработчика событий вызывает два разных `MeasureText` метода. Первый [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String)) вызов имеет простой `string` аргумент и возвращает ширину текста в пикселях, исходя из текущих атрибутов шрифта. Затем программа вычисляет новое `TextSize` свойство `SKPaint` объекта на основе отображаемой ширины, текущего `TextSize` Свойства и ширины области отображения. Это вычисление предназначено для `TextSize` того, чтобы текстовая строка отображалась в 90% от ширины экрана:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

Второй [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String,SkiaSharp.SKRect@)) вызов имеет `SKRect` аргумент, поэтому он получает как ширину, так и высоту отображаемого текста. `Height`Свойство этого `SKRect` значения зависит от наличия прописных букв, верхних и нижних колонтитулов в текстовой строке. `Height`Для текстовых строк «MOM», «Cat» и «собака» сообщается о различных значениях.

`Left`Свойства и `Top` `SKRect` структуры указывают координаты левого верхнего угла отображаемого текста, если текст отображается `DrawText` вызовом с позициями X и Y, равными 0. Например, если эта программа работает в симуляторе iPhone 7, `TextSize` ей присваивается значение 90,6254 в результате вычисления после первого вызова `MeasureText` . `SKRect`Значение, полученное во втором вызове, `MeasureText` содержит следующие значения свойств:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664,8214
- `Height` = 88;

Помните, что координаты X и Y, передаваемые в метод, `DrawText` указывают левую часть текста в базовом плане. `Top`Значение указывает, что текст расширяется на 68 пикселей над этим базовым планом и (вычитание 68 из 88) 20 пикселей ниже базового плана. `Left`Значение 6 указывает, что текст начинается на шесть пикселей справа от значения X в `DrawText` вызове. Это позволяет выполнять обычные межсимвольные интервалы. Если вы хотите отобразить текст снугли в левом верхнем углу экрана, передайте отрицательные `Left` значения этих и `Top` значений в координаты X и Y `DrawText` , в данном примере — &ndash; 6 и 68.

`SKRect`Структура определяет несколько полезных свойств и методов, некоторые из которых используются в оставшейся части `PaintSurface` обработчика. `MidX`Значения и `MidY` указывают координаты центра прямоугольника. (В примере для iPhone 7 эти значения 338,4107 и &ndash; 24.) В следующем коде эти значения используются для простейшего вычисления координат для центрирования текста на экране.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

`SKImageInfo`Информационная структура также определяет [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) свойство типа `SKRect` , поэтому можно также вычислить `xText` и `yText` следующим образом:

```csharp
float xText = info.Rect.MidX - textBounds.MidX;
float yText = info.Rect.MidY - textBounds.MidY;
```

`PaintSurface`Обработчик завершается двумя вызовами метода `DrawRoundRect` , для обоих из которых требуются аргументы `SKRect` . Это `SKRect` значение основано на `SKRect` значении, полученном от `MeasureText` метода, но не может совпадать. Во-первых, он должен быть небольшим большим, чтобы скругленный прямоугольник не нарисован над краями текста. Во-вторых, его необходимо сдвинуть в пространстве, чтобы `Left` `Top` значения и соответствовали левому верхнему углу, где расположен прямоугольник. Эти два задания выполняются с помощью [`Offset`](xref:SkiaSharp.SKRect.Offset*) методов и, [`Inflate`](xref:SkiaSharp.SKRect.Inflate*) определенных `SKRect` :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

Далее, оставшаяся часть метода имеет прямую переадресацию. Он создает другой `SKPaint` объект для границ и вызывает его `DrawRoundRect` дважды. Второй вызов использует прямоугольник, который расширяется другими 10 пикселами. Первый вызов задает радиус угла 20 пикселей. Второй угол имеет радиус угла 30 пикселей, поэтому они выглядят параллельно:

 [![Тройной снимок экрана с текстовой страницей в рамке](text-images/framedtext-small.png)](text-images/framedtext-large.png#lightbox "Тройной снимок экрана с текстовой страницей в рамке")

Вы можете превратить свой телефон или симулятор в сторону, чтобы увеличить размер текста и фрейма.

Если на экране нужно только центрировать текст, можно сделать это приблизительно без измерения текста. Вместо этого задайте [`TextAlign`](xref:SkiaSharp.SKPaint.TextAlign) свойство элемента `SKPaint` перечисления [`SKTextAlign.Center`](xref:SkiaSharp.SKTextAlign) . Координата X, указываемая в `DrawText` методе, указывает, где располагается горизонтальный центр текста. Если в метод передается середина экрана `DrawText` , текст будет горизонтально выровнен по центру и *почти* вертикально, так как базовый план будет вертикально выровнен по центру.

Текст может обрабатываться во многом подобно любому другому графическому объекту. Один из простых вариантов — отображение контура текстовых символов:

[![Тройной снимок страницы «Контурная текстовая страница»](text-images/outlinedtext-small.png)](text-images/outlinedtext-large.png#lightbox "Тройной снимок экрана с выставляемой страницей текста")

Для этого достаточно изменить значение свойства "Обычная" `Style` `SKPaint` объекта с установленного по умолчанию `SKPaintStyle.Fill` на `SKPaintStyle.Stroke` , а также задать толщину штриха. `PaintSurface`Обработчик **текстовой** страницы показывает, как это делается:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

Другим распространенным графическим объектом является точечный рисунок. Это большой раздел, подробно описанный в разделе [**SkiaSharp точечные рисунки**](../bitmaps/index.md), но Следующая статья, [**основы битовой карты в SkiaSharp**](bitmaps.md), предоставляет краткое введение.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)