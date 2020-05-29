---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5c3909271580d0568d7c603de0d434ff5b3f3bc4
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138675"
---
# <a name="segmented-display-of-skiasharp-bitmaps"></a>Сегментированное отображение точечных рисунков SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Объект SkiaSharp `SKCanvas` определяет метод с именем `DrawBitmapNinePatch` и два метода с именем `DrawBitmapLattice` , которые очень похожи. Оба эти метода отображают точечный рисунок на размер прямоугольника назначения, но вместо того, чтобы растянуть растровое изображение единообразно, они отображают части точечного рисунка в своих размерах в пикселях и растягивают другие части точечного рисунка таким образом, чтобы он соответствовал прямоугольнику:

![Сегментированные примеры](segmented-images/SegmentedSample.png "Сегментированный пример")

Эти методы обычно используются для отрисовки точечных рисунков, образующих часть объектов пользовательского интерфейса, таких как кнопки. При проектировании кнопки обычно требуется, чтобы размер кнопки основывался на содержимом кнопки, но, вероятно, требуется, чтобы граница кнопки была одинаковой ширины независимо от содержимого кнопки. Это идеальное приложение `DrawBitmapNinePatch` .

`DrawBitmapNinePatch`является особым случаем, `DrawBitmapLattice` но это более простой из двух методов для использования и понимания.

## <a name="the-nine-patch-display"></a>Дисплей с девятью исправлениями 

По сути, [`DrawBitmapNinePatch`](xref:SkiaSharp.SKCanvas.DrawBitmapNinePatch(SkiaSharp.SKBitmap,SkiaSharp.SKRectI,SkiaSharp.SKRect,SkiaSharp.SKPaint)) точечный рисунок разбивается на девять прямоугольников:

![Девять исправлений](segmented-images/NinePatch.png "Девять исправлений")

Прямоугольники на четырех углах отображаются в размерах пикселов. Как показывают стрелки, другие области на краях точечного рисунка растягиваются горизонтально или вертикально до области прямоугольника назначения. Прямоугольник в центре растягивается как по горизонтали, так и по вертикали. 

Если в прямоугольнике назначения недостаточно места для отображения даже четырех углов в их размерах, они масштабируются вниз до доступного размера, но отображаются четыре угла.

Чтобы разделить точечный рисунок на эти девять прямоугольников, необходимо только указать прямоугольник в центре. Это синтаксис `DrawBitmapNinePatch` метода:

```csharp
canvas.DrawBitmapNinePatch(bitmap, centerRectangle, destRectangle, paint);
```

Центральный прямоугольник задается относительно точечного рисунка. Это `SKRectI` значение (целочисленная версия `SKRect` ), а все координаты и размеры находятся в пикселях. Прямоугольник назначения задается относительно поверхности отображения. Аргумент `paint` не обязателен.

На **девяти страницах с отображением исправлений** в образце [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) сначала используется статический конструктор для создания открытого статического свойства типа `SKBitmap` :

```csharp
public partial class NinePatchDisplayPage : ContentPage
{
    static NinePatchDisplayPage()
    {
        using (SKCanvas canvas = new SKCanvas(FiveByFiveBitmap))
        using (SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 10
        })
        {
            for (int x = 50; x < 500; x += 100)
                for (int y = 50; y < 500; y += 100)
                {
                    canvas.DrawCircle(x, y, 40, paint);
                }
        }
    }

    public static SKBitmap FiveByFiveBitmap { get; } = new SKBitmap(500, 500);
    ···
}
```

Две другие страницы в этой статье используют тот же самый точечный рисунок. Точечный рисунок имеет размер 500 пикселей в квадрате и состоит из массива из 25 кругов, каждый из которых занимает квадратную площадь в 100 пикселя:

![Круглая сетка](segmented-images/CircleGrid.png "Круглая сетка")

Конструктор экземпляров программы создает `SKCanvasView` с `PaintSurface` обработчиком, который использует `DrawBitmapNinePatch` для отображения растрового изображения, растянутого для всей поверхности отображения:

```csharp
public class NinePatchDisplayPage : ContentPage
{
    ···
    public NinePatchDisplayPage()
    {
        Title = "Nine-Patch Display";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRectI centerRect = new SKRectI(100, 100, 400, 400);
        canvas.DrawBitmapNinePatch(FiveByFiveBitmap, centerRect, info.Rect);
    }
}
```

`centerRect`Прямоугольник охватывает Центральный массив из 16 кругов. Круги в углах отображаются в размерах пикселов, а все остальное растягивается соответствующим образом:

[![Отображение девяти исправлений](segmented-images/NinePatchDisplay.png "Отображение девяти исправлений")](segmented-images/NinePatchDisplay-Large.png#lightbox)

Страница UWP имеет размер 500 пикселей в ширину и поэтому отображает верхние и нижние строки в виде ряда кругов такого же размера. В противном случае все круги, которые не находятся в углах, растягиваются в виде эллипсов.

Для нестранного вывода объектов, состоящих из сочетания кругов и эллипсов, попробуйте определить Центральный прямоугольник так, чтобы он перекрывали строки и столбцы кругов.

```csharp
SKRectI centerRect = new SKRectI(150, 150, 350, 350);
```

## <a name="the-lattice-display"></a>Lattice дисплей

Эти два `DrawBitmapLattice` метода похожи на `DrawBitmapNinePatch` , но они обобщены для любого числа горизонтальных или вертикальных делений. Эти подразделения определяются массивами целых чисел, соответствующими пикселям. 

[`DrawBitmapLattice`](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,System.Int32[],System.Int32[],SkiaSharp.SKRect,SkiaSharp.SKPaint))Метод с параметрами для этих массивов целых чисел не работает. [`DrawBitmapLattice`](xref:SkiaSharp.SKCanvas.DrawBitmapLattice(SkiaSharp.SKBitmap,SkiaSharp.SKLattice,SkiaSharp.SKRect,SkiaSharp.SKPaint))Метод с параметром типа `SKLattice` работает, и он используется в примерах, показанных ниже.

[`SKLattice`](xref:SkiaSharp.SKLattice)Структура определяет четыре свойства:

- [`XDivs`](xref:SkiaSharp.SKLattice.XDivs), массив целых чисел
- [`YDivs`](xref:SkiaSharp.SKLattice.YDivs), массив целых чисел
- [`Flags`](xref:SkiaSharp.SKLattice.Flags), массив `SKLatticeFlags` типа перечисления
- [`Bounds`](xref:SkiaSharp.SKLattice.Bounds)типа `Nullable<SKRectI>` , чтобы указать необязательный исходный прямоугольник в пределах точечного рисунка

`XDivs`Массив делит ширину растрового изображения на вертикальные полосы. Первая полоса расширяется с точки 0 слева направо на `XDivs[0]` . Эта полоса отображается по ширине в пикселях. Вторая полоса расширяется с `XDivs[0]` на `XDivs[1]` и растягивается. Третья полоса расширяется с `XDivs[1]` на `XDivs[2]` и подготавливается к просмотру по ширине в пикселях. Последняя полоса расширяется от последнего элемента массива до правого края растрового изображения. Если массив содержит четное число элементов, то он отображается в его ширине в пикселе. В противном случае он растягивается. Общее число вертикальных полос больше числа элементов в массиве.

`YDivs`Массив аналогичен. Он делит высоту массива на горизонтальные полосы.

Вместе `XDivs` `YDivs` массив и разделяет растровое изображение на прямоугольники. Число прямоугольников равно произведению количества горизонтальных и вертикальных полос.

В соответствии с документацией по СКИА, `Flags` массив содержит по одному элементу для каждого прямоугольника, сначала к верхней строке прямоугольников, второй строке и т. д. `Flags`Массив имеет тип [`SKLatticeFlags`](xref:SkiaSharp.SKLatticeFlags) , перечисление со следующими элементами:

- `Default`со значением 0
- `Transparent`со значением 1

Однако эти флаги не работают так, как ожидается, и их лучше игнорировать. Но не присвойте `Flags` свойству значение `null` . Задайте для него массив значений, `SKLatticeFlags` достаточно больших для охватывающего общее количество прямоугольников.

Страница **исправления Lattice девять** используется `DrawBitmapLattice` для имитации `DrawBitmapNinePatch` . Он использует то же самое растровое изображение, созданное в `NinePatchDisplayPage` :

```csharp
public class LatticeNinePatchPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeNinePatchPage ()
    {
        Title = "Lattice Nine-Patch";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    `
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 400 };
        lattice.YDivs = new int[] { 100, 400 };
        lattice.Flags = new SKLatticeFlags[9]; 

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

`XDivs` `YDivs` Свойства и задаются как массивы всего двух целых чисел, разделив точечный рисунок на три полосы по горизонтали и вертикали: от точки 0 до пикселя 100 (отображаемый в размере в пикселях), от пикселя 100 до пикселя 400 (растянуто) и от пикселей 400 до пиксель 500 (размер в пикселях). Вместе `XDivs` и `YDivs` Определите сумму 9 прямоугольников, которая является размером `Flags` массива. Для создания массива значений достаточно просто создать массив `SKLatticeFlags.Default` .

Отображение идентично предыдущей программе:

[![Lattice девять обновлений](segmented-images/LatticeNinePatch.png "Lattice девять обновлений")](segmented-images/LatticeNinePatch-Large.png#lightbox)

Страница **Lattice отображает** точечный рисунок на 16 прямоугольников:

```csharp
public class LatticeDisplayPage : ContentPage
{
    SKBitmap bitmap = NinePatchDisplayPage.FiveByFiveBitmap;

    public LatticeDisplayPage()
    {
        Title = "Lattice Display";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKLattice lattice = new SKLattice();
        lattice.XDivs = new int[] { 100, 200, 400 };
        lattice.YDivs = new int[] { 100, 300, 400 };

        int count = (lattice.XDivs.Length + 1) * (lattice.YDivs.Length + 1);
        lattice.Flags = new SKLatticeFlags[count];

        canvas.DrawBitmapLattice(bitmap, lattice, info.Rect);
    }
}
```

`XDivs` `YDivs` Массивы и являются несколько разными, что приводит к недостаточности симметричного представления, как в предыдущих примерах:

[![Lattice дисплей](segmented-images/LatticeDisplay.png "Lattice дисплей")](segmented-images/LatticeDisplay-Large.png#lightbox)

В образах iOS и Android в левой части отображаются только меньшие круги в размерах пикселов. Все остальное растягивается.

На странице **Отображение Lattice** обобщается создание `Flags` массива, что позволяет экспериментировать с `XDivs` и `YDivs` проще. В частности, необходимо увидеть, что происходит при установке первого элемента `XDivs` `YDivs` массива или в значение 0. 

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
