---
title: Создание и рисование на точечных рисунках SkiaSharp
description: Узнайте, как создавать точечные рисунки SkiaSharp, а затем рисовать на этих точечных рисунках, создав на их основе холст.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 79BD3266-D457-4E50-BDDF-33450035FA0F
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c045e297beca675c0582efc2f75b1d6b2bcedcf8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573299"
---
# <a name="creating-and-drawing-on-skiasharp-bitmaps"></a>Создание и рисование на точечных рисунках SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Вы узнали, как приложение может загружать растровые изображения из Интернета, из ресурсов приложения и из библиотеки фотографий пользователя. В приложении также можно создавать новые точечные рисунки. Простейший подход включает один из конструкторов [`SKBitmap`](xref:SkiaSharp.SKBitmap.%23ctor(System.Int32,System.Int32,System.Boolean)) :

```csharp
SKBitmap bitmap = new SKBitmap(width, height);
```

`width`Параметры и `height` являются целыми числами и задают размеры точечного рисунка в пикселях. Этот конструктор создает полное цветное растровое изображение с четырьмя байтами на пиксель: по одному байту для красного, зеленого, синего и альфа-компонентов (Opacity).

После создания нового растрового изображения необходимо получить что-то на поверхности точечного рисунка. Обычно это делается одним из двух способов:

- Рисование на точечном рисунке с помощью стандартных `Canvas` методов рисования.
- Доступ к битам пикселей напрямую.

В этой статье демонстрируется первый подход:

![Пример рисования](drawing-images/DrawingSample.png "Пример рисования")

Второй подход обсуждается в статье [**доступ к пикселям SkiaSharp Bitmap**](pixel-bits.md).

## <a name="drawing-on-the-bitmap"></a>Рисование на точечном рисунке

Рисование на поверхности точечного рисунка аналогично рисованию на дисплее. Чтобы нарисовать изображение на дисплее, вы получаете `SKCanvas` объект из `PaintSurface` аргументов события. Для рисования на точечном рисунке необходимо создать `SKCanvas` объект с помощью [`SKCanvas`](xref:SkiaSharp.SKCanvas.%23ctor(SkiaSharp.SKBitmap)) конструктора:

```csharp
SKCanvas canvas = new SKCanvas(bitmap);
```

После завершения рисования на точечном рисунке можно удалить `SKCanvas` объект. По этой причине `SKCanvas` конструктор обычно вызывается в `using` операторе:

```csharp
using (SKCanvas canvas = new SKCanvas(bitmap))
{
    ··· // call drawing function
}
```

Затем можно отобразить точечный рисунок. В дальнейшем программа может создать новый `SKCanvas` объект на основе этого же растрового изображения и нарисовать его еще больше.

Страница " **Hello Bitmap** " в приложении **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** записывает текст "Hello, Bitmap!" на точечном рисунке, а затем отображает это растровое изображение несколько раз.

Конструктор `HelloBitmapPage` начинается с создания `SKPaint` объекта для отображения текста. Он определяет размеры текстовой строки и создает точечный рисунок с этими измерениями. Затем он создает `SKCanvas` объект на основе этого растрового изображения, вызывает `Clear` , а затем вызывает `DrawText` . Всегда рекомендуется вызывать `Clear` с новым точечным рисунком, так как только что созданный точечный рисунок может содержать случайные данные.

Конструктор завершается созданием `SKCanvasView` объекта для показа точечного рисунка.

```csharp
public partial class HelloBitmapPage : ContentPage
{
    const string TEXT = "Hello, Bitmap!";
    SKBitmap helloBitmap;

    public HelloBitmapPage()
    {
        Title = TEXT;

        // Create bitmap and draw on it
        using (SKPaint textPaint = new SKPaint { TextSize = 48 })
        {
            SKRect bounds = new SKRect();
            textPaint.MeasureText(TEXT, ref bounds);

            helloBitmap = new SKBitmap((int)bounds.Right,
                                       (int)bounds.Height);

            using (SKCanvas bitmapCanvas = new SKCanvas(helloBitmap))
            {
                bitmapCanvas.Clear();
                bitmapCanvas.DrawText(TEXT, 0, -bounds.Top, textPaint);
            }
        }

        // Create SKCanvasView to view result
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Aqua);

        for (float y = 0; y < info.Height; y += helloBitmap.Height)
            for (float x = 0; x < info.Width; x += helloBitmap.Width)
            {
                canvas.DrawBitmap(helloBitmap, x, y);
            }
    }
}
```

`PaintSurface`Обработчик отображает точечный рисунок несколько раз в строках и столбцах отображения. Обратите внимание, что `Clear` метод в `PaintSurface` обработчике имеет аргумент `SKColors.Aqua` , который цвета фона поверхности отображения:

[![Привет, битовая карта!](drawing-images/HelloBitmap.png "Привет, битовая карта!")](drawing-images/HelloBitmap-Large.png#lightbox)

Внешний вид голубого фона показывает, что точечный рисунок является прозрачным, за исключением текста.

## <a name="clearing-and-transparency"></a>Очистка и прозрачность

При отображении страницы " **Hello Bitmap** " показано, что создаваемая программа является прозрачной, за исключением черного текста. По этой причине отображается голубой цвет поверхности отображения.

Документация по `Clear` методам `SKCanvas` описывает их с помощью оператора: "заменяет все пиксели в текущем клипе холста". Использование слова «reзамены» показывает важную характеристику этих методов: все методы рисования `SKCanvas` для добавления чего-либо в существующую поверхность отображения. `Clear`Методы _заменяют_ то, что уже есть.

`Clear`существует в двух разных версиях:

- [`Clear`](xref:SkiaSharp.SKCanvas.Clear(SkiaSharp.SKColor))Метод с `SKColor` параметром заменяет Пиксели отображаемой области пикселами этого цвета.

- [`Clear`](xref:SkiaSharp.SKCanvas.Clear)Метод без параметров замещает Пиксели [`SKColors.Empty`](xref:SkiaSharp.SKColors.Empty) цветом, который является цветом, в котором все компоненты (красный, зеленый, синий и альфа) имеют нулевое значение. Этот цвет иногда называют "прозрачным черным".

Вызов `Clear` без аргументов в новом битовом рисунке инициализирует весь точечный рисунок полностью прозрачным. Все, что повышено рисуется на точечном рисунке, обычно имеет непрозрачный или частично непрозрачный.

Вот что нужно сделать: на странице **приветствия!** замените `Clear` метод, примененный к, следующим `bitmapCanvas` :

```csharp
bitmapCanvas.Clear(new SKColor(255, 0, 0, 128));
```

`SKColor`Параметры конструктора имеют красный цвет, зеленый цвет, синий цвет и альфа-канал, где каждое значение может находиться в диапазоне от 0 до 255. Помните, что альфа-значение 0 является прозрачным, а альфа-значение 255 — непрозрачным.

Значение (255, 0, 0, 128) очищает Пиксели точечного рисунка до красных пикселей с непрозрачностью 50%. Это означает, что фон точечного рисунка частично прозрачен. Полупрозрачный красный фон точечного рисунка сочетается с голубым фоном поверхности отображения для создания серого фона.

Попробуйте установить прозрачный черный цвет текста, поместив в инициализатор следующее назначение `SKPaint` :

```csharp
Color = new SKColor(0, 0, 0, 0)
```

Вы можете подумать, что этот прозрачный текст создаст полностью прозрачные области точечного рисунка, через который вы увидите цветовой фон поверхности отображения. Но это не так. Текст отображается поверх того, что уже есть на точечном рисунке. Прозрачный текст не будет виден вообще.

Ни один `Draw` из методов не делает точечный рисунок более прозрачным. `Clear`Это возможно только для этого.

## <a name="bitmap-color-types"></a>Типы цветовых битов

Простейший `SKBitmap` конструктор позволяет задать целочисленную ширину и высоту в пикселях для точечного рисунка. Другие `SKBitmap` Конструкторы являются более сложными. Для этих конструкторов требуются аргументы двух типов перечисления: [`SKColorType`](xref:SkiaSharp.SKColorType) и [`SKAlphaType`](xref:SkiaSharp.SKAlphaType) . Другие конструкторы используют [`SKImageInfo`](xref:SkiaSharp.SKImageInfo) структуру, которая объединяет эти сведения.

`SKColorType`Перечисление содержит 9 членов. Каждый из этих членов описывает конкретный способ хранения пикселей растрового изображения.

- `Unknown`
- `Alpha8`&mdash;каждый пиксель имеет 8 бит, представляющий альфа-значение от полностью прозрачного до полностью непрозрачного
- `Rgb565`&mdash;каждый пиксель имеет 16 бит, 5 бит для красного и синего и 6 для зеленого
- `Argb4444`&mdash;каждый пиксель имеет размер 16 бит, 4 для альфа, красного, зеленого и синего.
- `Rgba8888`&mdash;каждый пиксель имеет 32 бит, 8 для красного, зеленого, синего и альфа-канала.
- `Bgra8888`&mdash;каждый пиксель имеет 32 бит, 8 каждый для синего, зеленого, красного и альфа-канала
- `Index8`&mdash;каждый пиксель имеет 8 бит и представляет индекс в[`SKColorTable`](xref:SkiaSharp.SKColorTable)
- `Gray8`&mdash;каждый пиксель имеет 8 битов, представляющих серый оттенк с черного до белого
- `RgbaF16`&mdash;каждый пиксель имеет 64 бит, красный, зеленый, синий и альфа-канал в 16-разрядном формате с плавающей запятой

Два формата, где каждый пиксель имеет 32 пикселей (4 байта), часто называются _полноценными_ форматами. Многие другие форматы даты с момента отображения видео не поддерживают полный цвет. Точечные рисунки ограниченного цвета были достаточными для этих экранов и разрешенных точечных рисунков, чтобы занимать меньше места в памяти.

В эти дни программисты практически всегда используют полные растровые изображения и не работают с другими форматами. Исключением является `RgbaF16` Формат, который позволяет большему разрешению цвета, чем даже форматы полного цвета. Однако этот формат используется для специализированных целей, например для медицинских изображений, и не имеет смысла при использовании со стандартными полноэкранными экранами.

Эта серия статей будет ограничена `SKBitmap` цветовыми форматами, используемыми по умолчанию, если ни один `SKColorType` элемент не указан. Этот формат по умолчанию основан на базовой платформе. Для платформ Xamarin.Forms , поддерживаемых, тип цвета по умолчанию:

- `Rgba8888`для iOS и Android
- `Bgra8888`для UWP

Единственное отличие состоит в том, что 4 байта в памяти, и это становится проблемой только при непосредственном доступе к битам пикселей. Это не станет важным, пока вы не получите доступ к статье [**SkiaSharp Bitmap пикселей**](pixel-bits.md).

`SKAlphaType`Перечисление состоит из четырех элементов:

- `Unknown`
- `Opaque`&mdash;точечный рисунок не имеет прозрачности
- `Premul`&mdash;компоненты цвета предварительно умножаются на альфа-компонент
- `Unpremul`&mdash;компоненты цвета не были предварительно умножены на альфа-компонент

Вот 4-байтовый пиксель в виде красного растрового изображения с прозрачностью 50%, где байты показаны в порядке красный, зеленый, синий, альфа:

0xFF 0x00 0x00 0x80

Если точечный рисунок, содержащий полупрозрачные пиксели, отображается на поверхности отображения, то цветовые компоненты каждого пикселя растрового изображения должны быть умножены на альфа-значение пикселя, а цветовые компоненты соответствующего пикселя поверхности экрана должны быть умножены на 255 минус Альфа-значение. Затем можно объединить два пикселя. Точечный рисунок может быть воспроизведен быстрее, если компоненты цвета в пикселах растрового изображения уже были предварительно мулитплиед по альфа-значению. Этот же красный пиксель будет храниться следующим образом в предварительно умноженном формате:

0x80 0x00 0x00 0x80

Это улучшение производительности заключается в том, почему `SkiaSharp` по умолчанию точечные рисунки создаются с использованием `Premul` формата. Но опять же, это необходимо понять только при доступе к битам пикселей и управлении ими.

## <a name="drawing-on-existing-bitmaps"></a>Рисование на существующих точечных рисунках

Нет необходимости создавать новый точечный рисунок для рисования. Можно также рисовать на существующем растровом изображении.

На странице " **обезьяна маустаче** " для загрузки образа **MonkeyFace.png** используется конструктор. Затем он создает `SKCanvas` объект на основе этого растрового изображения и использует `SKPaint` `SKPath` объекты и для рисования маустаче на нем.

```csharp
public partial class MonkeyMoustachePage : ContentPage
{
    SKBitmap monkeyBitmap;

    public MonkeyMoustachePage()
    {
        Title = "Monkey Moustache";

        monkeyBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MonkeyFace.png");

        // Create canvas based on bitmap
        using (SKCanvas canvas = new SKCanvas(monkeyBitmap))
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 24;
                paint.StrokeCap = SKStrokeCap.Round;

                using (SKPath path = new SKPath())
                {
                    path.MoveTo(380, 390);
                    path.CubicTo(560, 390, 560, 280, 500, 280);

                    path.MoveTo(320, 390);
                    path.CubicTo(140, 390, 140, 280, 200, 280);

                    canvas.DrawPath(path, paint);
                }
            }
        }

        // Create SKCanvasView to view result
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
        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Конструктор завершается созданием `SKCanvasView` `PaintSurface` обработчика, который просто отображает результат:

[![Обезьяна Маустаче](drawing-images/MonkeyMoustache.png "Обезьяна Маустаче")](drawing-images/MonkeyMoustache-Large.png#lightbox)

## <a name="copying-and-modifying-bitmaps"></a>Копирование и изменение точечных рисунков

Методы `SKCanvas` , которые можно использовать для рисования на точечном рисунке, включают `DrawBitmap` . Это означает, что можно нарисовать одно растровое изображение на другом, обычно изменяя его каким бы то ни было образом.

Самым универсальным способом изменения точечного рисунка является доступ к фактическим разрядам пикселей, предмет, который рассматривается в статье **[доступ к SkiaSharp точечным рисункам](pixel-bits.md)**. Но существует много других методов изменения точечных рисунков, которые не нуждаются в доступе к битам пикселей.

Следующий рисунок, включенный в приложение **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** , составляет 360 пикселей в ширину и 480 пикселей в высоту:

![Горные вырасти](drawing-images/MountainClimbers.jpg "Горные вырасти")

Предположим, вы не получили разрешение на публикацию этой фотографии в левой части. Одно из решений заключается в скрытии поверхности обезьяны с помощью методики, именуемой _разпиксельностью_. Пиксели грани заменяются блоками цвета, поэтому вы не сможете делать эти возможности. Блоки цвета обычно являются производными от исходного изображения путем усреднения цветов пикселов, соответствующих этим блокам. Но вам не нужно выполнять усреднение. Это происходит автоматически при копировании растрового изображения в измерение с меньшим размером в пикселях.

Левая часть обезьяны занимает примерно 72 пикселей в квадрате с верхним левым углом в точке (112, 238). Давайте запустим в 72-пиксельном квадрате массив цветных блоков, каждый из которых имеет размер в 8 – 8 пикселей.

Страница **образа пикселизе** загружается в это растровое изображение и сначала создает маленькую 9-пиксельное точечное изображение с именем `faceBitmap` . Это место назначения для копирования только грани обезьяны. Прямоугольник назначения — это всего лишь 9 пикселей квадрата, но исходный прямоугольник — 72 пикселей. Каждый 8-х-8 блок исходных пикселей консолидируется в один пиксел, усредненный по цвету.

Следующим шагом является копирование исходного растрового изображения в новое растровое изображение такого же размера с именем `pixelizedBitmap` . Затем крошечный объект `faceBitmap` копируется поверх этого с прямоугольным прямоугольником назначения размером в 72 пикселей, поэтому каждый пиксель `faceBitmap` разворачивается в 8 раз по размеру:

```csharp
public class PixelizedImagePage : ContentPage
{
    SKBitmap pixelizedBitmap;

    public PixelizedImagePage ()
    {
        Title = "Pixelize Image";

        SKBitmap originalBitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        // Create tiny bitmap for pixelized face
        SKBitmap faceBitmap = new SKBitmap(9, 9);

        // Copy subset of original bitmap to that
        using (SKCanvas canvas = new SKCanvas(faceBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(originalBitmap,
                              new SKRect(112, 238, 184, 310),   // source
                              new SKRect(0, 0, 9, 9));          // destination

        }

        // Create full-sized bitmap for copy
        pixelizedBitmap = new SKBitmap(originalBitmap.Width, originalBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(pixelizedBitmap))
        {
            canvas.Clear();

            // Draw original in full size
            canvas.DrawBitmap(originalBitmap, new SKPoint());

            // Draw tiny bitmap to cover face
            canvas.DrawBitmap(faceBitmap,
                              new SKRect(112, 238, 184, 310));  // destination
        }

        // Create SKCanvasView to view result
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
        canvas.DrawBitmap(pixelizedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Конструктор завершается созданием `SKCanvasView` для вывода результата.

[![Изображение пикселизе](drawing-images/PixelizeImage.png "Изображение пикселизе")](drawing-images/PixelizeImage-Large.png#lightbox)

## <a name="rotating-bitmaps"></a>Вращение точечных рисунков

Другой распространенной задачей является вращение точечных рисунков. Это особенно полезно при извлечении точечных рисунков из библиотеки фотографий iPhone или iPad. Если при создании фотографии устройство не удерживается в определенной ориентации, изображение, скорее всего, будет перенаправлено вверх или вниз.

Чтобы перевернуть точечный рисунок, необходимо создать другой точечный рисунок того же размера, что и первый, а затем установить преобразование для поворота на 180 градусов при копировании первого элемента во второй. Во всех примерах в этом разделе `bitmap` — `SKBitmap` объект, который необходимо повернуть:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(180, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

При повороте на 90 градусов необходимо создать точечный рисунок, размер которого отличается от размера оригинала путем замены высоты и ширины. Например, если исходное растровое изображение имеет размер 1200 пикселей в ширину и 800 пикселов высотой, то повернутый точечный рисунок имеет значение 800 пикселей в ширину и 1200 пикселей в ширину. Задайте преобразование и поворот, чтобы растровое изображение поворачиваться вокруг левого верхнего угла, а затем было перемещено в представление. (Помните, что `Translate` `RotateDegrees` методы и вызываются в обратном порядке того, как они применяются.) Ниже приведен код для поворота 90 градусов по часовой стрелке.

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(bitmap.Height, 0);
    canvas.RotateDegrees(90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

И вот аналогичная функция для поворота счетчика 90 градусов по часовой стрелке:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.Translate(0, bitmap.Width);
    canvas.RotateDegrees(-90);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Эти два метода используются на страницах **Фотоголоволомки** , описанных в статье [**Обрезка точечных рисунков SkiaSharp**](cropping.md#cropping-skiasharp-bitmaps).

Программа, позволяющая пользователю вращать точечный рисунок в шагах с шагом в 90 градусов, должна реализовывать только одну функцию для поворота на 90 градусов. После этого пользователь может вращать любой шаг в 90 градусов за счет повторного выполнения этой функции.

Программа также может вращать точечный рисунок любым размером. Одним из простых подходов является изменение функции, которая поворачивается на 180 градусов путем замены 180 на обобщенную `angle` переменную:

```csharp
SKBitmap rotatedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
{
    canvas.Clear();
    canvas.RotateDegrees(angle, bitmap.Width / 2, bitmap.Height / 2);
    canvas.DrawBitmap(bitmap, new SKPoint());
}
```

Однако в общем случае эта логика будет обрезать углы повернутого растрового изображения. Лучшим подходом является вычисление размера повернутого растрового изображения с помощью тригонометрии для включения этих углов.

Эта тригонометрия показана на странице **вращения растрового изображения** . XAML-файл создает экземпляр `SKCanvasView` и `Slider` , который может находиться в диапазоне от 0 до 360 градусов с `Label` отображением текущего значения:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapRotatorPage"
             Title="Bitmap Rotator">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="slider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Rotate by {0:F0}&#x00B0;'}"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Файл кода программной части загружает ресурс точечного рисунка и сохраняет его как статическое поле только для чтения с именем `originalBitmap` . В обработчике отображается точечный рисунок `PaintSurface` `rotatedBitmap` , для которого изначально задано значение `originalBitmap` :

```csharp
public partial class BitmapRotatorPage : ContentPage
{
    static readonly SKBitmap originalBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap rotatedBitmap = originalBitmap;

    public BitmapRotatorPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(rotatedBitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double angle = args.NewValue;
        double radians = Math.PI * angle / 180;
        float sine = (float)Math.Abs(Math.Sin(radians));
        float cosine = (float)Math.Abs(Math.Cos(radians));
        int originalWidth = originalBitmap.Width;
        int originalHeight = originalBitmap.Height;
        int rotatedWidth = (int)(cosine * originalWidth + sine * originalHeight);
        int rotatedHeight = (int)(cosine * originalHeight + sine * originalWidth);

        rotatedBitmap = new SKBitmap(rotatedWidth, rotatedHeight);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear(SKColors.LightPink);
            canvas.Translate(rotatedWidth / 2, rotatedHeight / 2);
            canvas.RotateDegrees((float)angle);
            canvas.Translate(-originalWidth / 2, -originalHeight / 2);
            canvas.DrawBitmap(originalBitmap, new SKPoint());
        }

        canvasView.InvalidateSurface();
    }
}
```

`ValueChanged`Обработчик объекта `Slider` выполняет операции, которые создают новый `rotatedBitmap` на основе угла вращения. Новая ширина и высота основываются на абсолютных значениях синусов и косинусов исходных значений ширины и высоты. Преобразования, используемые для рисования исходного растрового изображения повернутого растрового изображения, перемещают исходный центр растрового изображения в начало координат, затем поворачивают его на указанное число градусов, а затем переводят его в центр повернутого растрового изображения. ( `Translate` Методы и `RotateDegrees` вызываются в обратном порядке по сравнению с тем, как они применяются.)

Обратите внимание на использование `Clear` метода для создания фона `rotatedBitmap` светло-розовый. Это только показывает размер `rotatedBitmap` на экране:

[![Вращения растрового изображения](drawing-images/BitmapRotator.png "Вращения растрового изображения")](drawing-images/BitmapRotator-Large.png#lightbox)

Повернутый точечный рисунок достаточно большой, чтобы включить все исходное растровое изображение, но не больше.

## <a name="flipping-bitmaps"></a>Зеркальное отображение точечных рисунков

Другая операция, обычно выполняемая с точечными рисунками, называется _зеркальным отображением_. По сути, точечный рисунок поворачивается в три измерения вокруг вертикальной оси или горизонтальной оси через центр точечного рисунка. При вертикальном отражении создается зеркальный образ.

Эти процессы показаны на странице **Bitmap во флиппере** в приложении **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** . Файл XAML содержит `SKCanvasView` две кнопки для зеркального отображения по вертикали и горизонтали:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.BitmapFlipperPage"
             Title="Bitmap Flipper">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Flip Vertical"
                Grid.Row="1" Grid.Column="0"
                Margin="0, 10"
                Clicked="OnFlipVerticalClicked" />

        <Button Text="Flip Horizontal"
                Grid.Row="1" Grid.Column="1"
                Margin="0, 10"
                Clicked="OnFlipHorizontalClicked" />
    </Grid>
</ContentPage>
```

Файл кода программной части реализует эти две операции в `Clicked` обработчиках для кнопок:

```csharp
public partial class BitmapFlipperPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(BitmapRotatorPage),
            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public BitmapFlipperPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnFlipVerticalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(-1, 1, bitmap.Width / 2, 0);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnFlipHorizontalClicked(object sender, ValueChangedEventArgs args)
    {
        SKBitmap flippedBitmap = new SKBitmap(bitmap.Width, bitmap.Height);

        using (SKCanvas canvas = new SKCanvas(flippedBitmap))
        {
            canvas.Clear();
            canvas.Scale(1, -1, 0, bitmap.Height / 2);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = flippedBitmap;
        canvasView.InvalidateSurface();
    }
}
```

Вертикальное перелистывание достигается преобразованием масштабирования с горизонтальным коэффициентом масштабирования, равным &ndash; 1. Центр масштабирования — вертикальный центр точечного рисунка. Перелистывание по горизонтали — это преобразование масштабирования с коэффициентом вертикального масштабирования, равным &ndash; 1.

Как видно из обратной буквы в футболе, зеркальное отображение не совпадает с поворотом. Но, как показано на снимке экрана UWP справа, зеркальное отображение как по горизонтали, так и по вертикали аналогично повороту 180 градусов:

[![Точечный рисунок во флиппере](drawing-images/BitmapFlipper.png "Точечный рисунок во флиппере")](drawing-images/BitmapFlipper-Large.png#lightbox)

Другая распространенная задача, которая может быть обработана с помощью схожих методов, заключается в обрезке точечного рисунка до прямоугольного подмножества. Это описано в следующей статье [**Обрезка точечных рисунков SkiaSharp**](cropping.md).

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
