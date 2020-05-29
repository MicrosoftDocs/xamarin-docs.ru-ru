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
ms.openlocfilehash: b9c89d4d426884d678e77687ffa226cced97be58
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136387"
---
# <a name="skiasharp-color-filters"></a>Фильтры по цвету SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Цветовые фильтры могут переводить цвета в точечном рисунке (или другом изображении) в другие цвета для таких эффектов, как Афиша:

![Пример цветовых фильтров](color-filters-images/ColorFiltersExample.png "Пример цветовых фильтров")

Чтобы использовать фильтр цветов, задайте [`ColorFilter`](xref:SkiaSharp.SKPaint.ColorFilter) свойство `SKPaint` объекта типа, [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) созданного одним из статических методов в этом классе. В этой статье показано следующее: 

- преобразование цвета, созданное с помощью [`CreateColorMatrix`](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) метода.
- Таблица цветов, созданная с помощью [`CreateTable`](xref:SkiaSharp.SKColorFilter.CreateTable*) метода.

## <a name="the-color-transform"></a>Преобразование цвета

Преобразование цветов включает использование матрицы для изменения цветов. Как и в большинстве двумерных графических систем, SkiaSharp использует матрицы в основном для преобразования точек координат как искуссед в [**матрице преобразований в SkiaSharp**](../transforms/matrix.md). [`SKColorFilter`](xref:SkiaSharp.SKColorFilter)Также поддерживает преобразования матрицы, но матрица преобразует цвета RGB. Для понимания этих преобразований цветов необходимо знание концепций матрицы. 

Матрица преобразования цветов имеет измерение из четырех строк и пять столбцов:

<pre>
| M11 M12 M13 M14 M15 |
| M21 M22 M23 M24 M25 |
| M31 M32 M33 M34 M35 |
| M41 M42 M43 M44 M45 |
</pre>

Он преобразует исходный цвет RGB (R, G, B, A) в целевой цвет (R ', G ', B ', A '). 

При подготовке к умножению матрицы исходный цвет преобразуется в матрицу размером 5 × 1:

<pre>
| R |
| G |
| B |
| A |
| 1 |
</pre>

Эти значения R, G, B и Values являются исходными байтами в диапазоне от 0 до 255. Они _не_ нормализованы до значений с плавающей запятой в диапазоне от 0 до 1.

Для фактора преобразования требуется обязательная ячейка. Это аналогично использованию матрицы размером 3 × 3 для преобразования двумерных точек координат, как описано в разделе [**Причина матрицы с 3 по 3**](../transforms/matrix.md#the-reason-for-the-3-by-3-matrix) в статье об использовании матриц для преобразования точек координат.

Матрица размером 4 × 5 умножается на матрицу размером 5 × 1, а продукт является матрицей размером 4 × 1 с преобразованным цветом:

<pre>
| M11 M12 M13 M14 M15 |    | R |   | R' |
| M21 M22 M23 M24 M25 |    | G |   | G' |
| M31 M32 M33 M34 M35 |  × | B | = | B' |
| M41 M42 M43 M44 M45 |    | A |   | A' |
                           | 1 |
</pre>

Ниже приведены отдельные формулы для R ', G ', B ' и ':

`R' = M11·R + M12·G + M13·B + M14·A + M15` 

`G' = M21·R + M22·G + M23·B + M24·A + M25` 

`B' = M31·R + M32·G + M33·B + M34·A + M35` 

`A' = M41·R + M42·G + M43·B + M44·A + M45` 

Большая часть матрицы состоит из мультипликативные факторов, которые обычно находятся в диапазоне от 0 до 2. Однако последний столбец (M15 до M45) содержит значения, которые добавляются в формулы. Эти значения обычно находятся в диапазоне от 0 до 255. Результаты отправляются между значениями 0 и 255.

Матрица идентификаторов:

<pre>
| 1 0 0 0 0 |
| 0 1 0 0 0 |
| 0 0 1 0 0 |
| 0 0 0 1 0 |
</pre>

Это не приводит к изменению цветов. Формулы преобразования:

`R' = R` 

`G' = G`

`B' = B`

`A' = A`

Ячейка M44 очень важна, так как она сохраняет непрозрачность. Обычно это так, что M41, M42 и M43 равны нулю, так как, вероятно, не требуется, чтобы непрозрачность основывалась на значениях красного, зеленого и синего. Но если M44 равен нулю, то "будет равняться нулю, и ничего отображаться не будет.

Одним из наиболее распространенных способов использования цветовой матрицы является преобразование цветового изображения в растровое изображение с серым масштабом. Это включает формулу для взвешенного среднего значения красного, зеленого и синего значений. Для отображения видео с использованием цветового пространства sRGB ("стандартное красное зеленое синее") Эта формула:

серый — затенение = 0,2126 · R + 0,7152 · G + 0,0722 · &

Чтобы преобразовать цветовой точечный рисунок в растровое изображение с серым масштабом, результаты R ', G ' и B ' должны быть равны одному значению. Матрица:

<pre>
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0    0    0    1 0 |
</pre>

Отсутствует тип данных SkiaSharp, соответствующий этой матрице. Вместо этого матрица должна представляться как массив из 20 `float` значений в порядке строк: первая строка, вторая строка и т. д.

Статический [`SKColorFilter.CreateColorMatrix`](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) метод имеет следующий синтаксис:

```csharp
public static SKColorFilter CreateColorMatrix (float[] matrix);
```

где `matrix` — массив из 20 `float` значений. При создании массива в C# можно легко отформатировать числа, чтобы они наглядели как матрица размером 4 × 5. Это показано на странице **Матрица серого масштаба** в примере [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) :

```csharp
public class GrayScaleMatrixPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(CenteredTilesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg");

    public GrayScaleMatrixPage()
    {
        Title = "Gray-Scale Matrix";

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

        using (SKPaint paint = new SKPaint())
        {
            paint.ColorFilter =
                SKColorFilter.CreateColorMatrix(new float[]
                {
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0,     0,     0,     1, 0
                });

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

`DrawBitmap`Метод, используемый в этом коде, находится в файле **BitmapExtension.CS** , входящем в образец [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) . 

Вот результат, выполняемый в iOS, Android и универсальная платформа Windows:

[![Матрица серого масштаба](color-filters-images/GrayScaleMatrix.png "Матрица серого масштаба")](color-filters-images/GrayScaleMatrix-Large.png#lightbox)

Просмотрите значение в четвертой и четвертой столбцах. Это ключевой фактор, умноженный на значение исходного цвета для значения «A» преобразованного цвета. Если эта ячейка равна нулю, то ничего не отображается, и проблема может быть сложно обнаружить.

При эксперименте с матрицами цветов преобразование можно рассматривать как с точки зрения источника, так и с точки зрения назначения. Как замещается красный пиксел источника на красный, зеленый и синий Пиксели места назначения? Это определяется значениями в первом _столбце_ матрицы. Кроме того, как должны влиять красные, зеленые и синие пикселы источника на конечный пиксель назначения? Это определяется первой _строкой_ матрицы.

Некоторые идеи по использованию преобразований цветов см. на страницах « [**Перекрасить изображения**](https://docs.microsoft.com/dotnet/framework/winforms/advanced/recoloring-images) ». Обсуждение Windows Forms, а матрица имеет другой формат, но основные понятия одинаковы.

**Матрица «пастель** » рассчитывает красный пиксель назначения путем затухания исходного Красного пикселя и слегка акцентирования к красному и зеленому пикселам. Этот процесс происходит аналогично зеленому и синему пикселам:

```csharp
public class PastelMatrixPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PastelMatrixPage),
                        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    public PastelMatrixPage()
    {
        Title = "Pastel Matrix";

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

        using (SKPaint paint = new SKPaint())
        {
            paint.ColorFilter =
                SKColorFilter.CreateColorMatrix(new float[]
                {
                    0.75f, 0.25f, 0.25f, 0, 0,
                    0.25f, 0.75f, 0.25f, 0, 0,
                    0.25f, 0.25f, 0.75f, 0, 0,
                    0, 0, 0, 1, 0
                });

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

Результат заключается в отключении интенсивности цветов, как показано ниже:

[![Пастельная матрица](color-filters-images/PastelMatrix.png "Пастельная матрица")](color-filters-images/PastelMatrix-Large.png#lightbox)

## <a name="color-tables"></a>Таблицы цветов

Статический [`SKColorFilter.CreateTable`](xref:SkiaSharp.SKColorFilter.CreateTable*) метод поставляется в двух версиях:

```csharp
public static SKColorFilter CreateTable (byte[] table);

public static SKColorFilter CreateTable (byte[] tableA, byte[] tableR, byte[] tableG, byte[] tableB);
```

Массивы всегда содержат 256 записей. В `CreateTable` методе с одной таблицей для компонентов красного, зеленого и синего используется одна и та же таблица. Это простая таблица поиска: Если исходный цвет имеет значение (R, G, B), а целевой цвет — (R ', B ', G '), то целевые компоненты получаются путем индексирования `table` с исходными компонентами:

`R' = table[R]`

`G' = table[G]`

`B' = table[B]`

Во втором методе каждый из четырех компонентов цвета может иметь отдельную таблицу цветов или одни и те же таблицы цветов могут совместно использоваться двумя или более компонентами.

Если вы хотите задать один из аргументов для второго `CreateTable` метода в таблице цветов, которая содержит значения от 0 до 255, можно использовать `null` вместо этого. Очень часто `CreateTable` вызов имеет `null` первый аргумент для альфа-канала.

В разделе, посвященном **афише** , в статье о [доступе к битам точечного рисунка SkiaSharp](../bitmaps/pixel-bits.md#posterization), вы узнали, как изменить отдельные биты точечного рисунка, чтобы уменьшить его разрешение цвета. Это метод, называемый _афишой_. 

Также можно постеризе растровое изображение с таблицей цветов. Конструктор страницы **таблицы постеризе** создает таблицу цветов, которая сопоставляет индекс со значением 6 младших битов, равным нулю:

```csharp
public class PosterizeTablePage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PosterizeTablePage),
                        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    byte[] colorTable = new byte[256];

    public PosterizeTablePage()
    {
        Title = "Posterize Table";

        // Create color table
        for (int i = 0; i < 256; i++)
        {
            colorTable[i] = (byte)(0xC0 & i);
        }

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

        using (SKPaint paint = new SKPaint())
        {
            paint.ColorFilter =
                SKColorFilter.CreateTable(null, null, colorTable, colorTable);

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

Программа выбирает использование этой таблицы цветов только для зеленых и синих каналов. Красный канал все равно будет иметь полное разрешение:

[![Таблица постеризе](color-filters-images/PosterizeTable.png "Таблица постеризе")](color-filters-images/PosterizeTable-Large.png#lightbox)

Можно использовать различные таблицы цветов для различных цветовых каналов для различных эффектов. 

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
