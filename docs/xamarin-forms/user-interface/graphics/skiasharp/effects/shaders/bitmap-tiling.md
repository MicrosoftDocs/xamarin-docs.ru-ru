---
Title: "SkiaSharp мозаичное заполнение" Description: "мозаичная область с использованием точечных рисунков повторяется горизонтально и вертикально".
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: 9ED14E07-4DC8-4B03-8A33-772838BF51EA Автор: давидбритч MS. author: дабритч MS. Дата: 08/23/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="skiasharp-bitmap-tiling"></a>Разбиение растрового изображения SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock)

Как видно в двух предыдущих статьях, [`SKShader`](xref:SkiaSharp.SKShader) класс может создавать линейные или кольцевые градиенты. В этой статье основное внимание уделяется `SKShader` объекту, который использует точечный рисунок для мозаичного заполнения области. Точечный рисунок может повторяться горизонтально и вертикально, либо в исходной ориентации, либо в альтернативном направлении по горизонтали и вертикали. Зеркальное отображение позволяет избежать небесперебойности между плитками:

![Пример мозаичного заполнения растрового изображения](bitmap-tiling-images/BitmapTilingSample.png "Пример мозаичного заполнения растрового изображения")

Статический [`SKShader.CreateBitmap`](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode)) метод, создающий этот шейдер, имеет `SKBitmap` параметр и два члена [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) перечисления:

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy)
```

Два параметра указывают режимы, используемые для горизонтального мозаичного заполнения и разбиения по вертикали. Это то же `SKShaderTileMode` перечисление, которое также используется с методами градиента.

[`CreateBitmap`](xref:SkiaSharp.SKShader.CreateBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKShaderTileMode,SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))Перегрузка включает `SKMatrix` аргумент для преобразования мозаичных растровых изображений:

```csharp
public static SKShader CreateBitmap (SKBitmap src, SKShaderTileMode tmx, SKShaderTileMode tmy, SKMatrix localMatrix)
```

Эта статья содержит несколько примеров использования этого преобразования матрицы с мозаичными точечными рисунками.

## <a name="exploring-the-tile-modes"></a>Изучение режимов плиток

Первая программа в разделе **Мозаичное заполнение растрового изображения** на странице **шейдеры и другие эффекты** в образце [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) демонстрирует влияние двух `SKShaderTileMode` аргументов. Файл XAML **режима отражения плитки растрового изображения** создает `SKCanvasView` и два `Picker` представления, которые позволяют выбрать `SKShaderTilerMode` значение для горизонтального и вертикального мозаичного заполнения. Обратите внимание, что `SKShaderTileMode` в разделе определен массив элементов `Resources` :

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BitmapTileFlipModesPage"
             Title="Bitmap Tile Flip Modes">

    <ContentPage.Resources>
        <x:Array x:Key="tileModes"
                 Type="{x:Type skia:SKShaderTileMode}">
            <x:Static Member="skia:SKShaderTileMode.Clamp" />
            <x:Static Member="skia:SKShaderTileMode.Repeat" />
            <x:Static Member="skia:SKShaderTileMode.Mirror" />
        </x:Array>
    </ContentPage.Resources>

    <StackLayout>
        <skiaforms:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="xModePicker"
                Title="Tile X Mode"
                Margin="10, 0"
                ItemsSource="{StaticResource tileModes}"
                SelectedIndex="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />

        <Picker x:Name="yModePicker"
                Title="Tile Y Mode"
                Margin="10, 10"
                ItemsSource="{StaticResource tileModes}"
                SelectedIndex="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />

    </StackLayout>
</ContentPage>
```

Конструктор файла кода программной части загружается в ресурс точечного рисунка, в котором показана обезьяна. Сначала изображение обрезается с помощью [`ExtractSubset`](xref:SkiaSharp.SKBitmap.ExtractSubset(SkiaSharp.SKBitmap,SkiaSharp.SKRectI)) метода `SKBitmap` , чтобы головной элемент и опоры соприкасаются с краями растрового изображения. Затем конструктор использует [`Resize`](xref:SkiaSharp.SKBitmap.Resize(SkiaSharp.SKImageInfo,SkiaSharp.SKBitmapResizeMethod)) метод, чтобы создать еще одну битовую карту половины размера. Эти изменения делают точечный рисунок немного более подходящим для мозаичного заполнения:

```csharp
public partial class BitmapTileFlipModesPage : ContentPage
{
    SKBitmap bitmap;

    public BitmapTileFlipModesPage ()
    {
        InitializeComponent ();

        SKBitmap origBitmap = BitmapExtensions.LoadBitmapResource(
            GetType(), "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

        // Define cropping rect
        SKRectI cropRect = new SKRectI(5, 27, 296, 260);

        // Get the cropped bitmap
        SKBitmap croppedBitmap = new SKBitmap(cropRect.Width, cropRect.Height);
        origBitmap.ExtractSubset(croppedBitmap, cropRect);

        // Resize to half the width and height
        SKImageInfo info = new SKImageInfo(cropRect.Width / 2, cropRect.Height / 2);
        bitmap = croppedBitmap.Resize(info, SKBitmapResizeMethod.Box);
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get tile modes from Pickers
        SKShaderTileMode xTileMode =
            (SKShaderTileMode)(xModePicker.SelectedIndex == -1 ?
                                        0 : xModePicker.SelectedItem);
        SKShaderTileMode yTileMode =
            (SKShaderTileMode)(yModePicker.SelectedIndex == -1 ?
                                        0 : yModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(bitmap, xTileMode, yTileMode);
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

`PaintSurface`Обработчик получает `SKShaderTileMode` параметры из двух `Picker` представлений и создает `SKShader` объект на основе точечного рисунка и этих двух значений. Этот шейдер используется для заполнения холста:

[![Режимы отражения плитки точечного рисунка](bitmap-tiling-images/BitmapTileFlipModes.png "Режимы отражения плитки точечного рисунка")](bitmap-tiling-images/BitmapTileFlipModes-Large.png#lightbox)

На экране iOS слева отображается результат использования значений по умолчанию `SKShaderTileMode.Clamp` . Точечный рисунок располагается в левом верхнем углу. Под точечным рисунком Нижняя строка пикселей повторяется все так же. Справа от точечного рисунка крайний правый столбец пикселей повторяется в течение всего. Оставшаяся часть холста окрашена на темно-коричневый пиксель в правом нижнем углу растрового изображения. Следует быть очевидным, что `Clamp` параметр практически никогда не используется с мозаичным разбиением на карты!

Экран Android в центре показывает результат `SKShaderTileMode.Repeat` для обоих аргументов. Плитка повторяется горизонтально и вертикально. Появится экран универсальная платформа Windows `SKShaderTileMode.Mirror` . Плитки повторяются, но перемещаются по горизонтали и по вертикали. Преимущество этого варианта заключается в том, что между плитками нет ненепрерывных работ.

Помните, что можно использовать различные параметры для горизонтального и вертикального повторения. В качестве второго аргумента можно указать, а в `SKShaderTileMode.Mirror` `CreateBitmap` `SKShaderTileMode.Repeat` качестве третьего аргумента —. В каждой строке монкэйс по-прежнему переключается между обычным и зеркальным изображениями, но ни один из монкэйс не переворачивается.

## <a name="patterned-backgrounds"></a>Узорные фоны

Мозаичное разбиение по точечным рисункам обычно используется для создания фонового фона на основе относительно небольшого точечного рисунка. Классический пример — это настенный кирпич.

На странице « **алгоритмическая кирпич** » создается небольшой точечный рисунок, напоминающий весь модуль, и две половины модуля, разделенные на физические. Поскольку этот модуль также используется в следующем примере, он создается статическим конструктором и становится общедоступным со статическим свойством:

```csharp
public class AlgorithmicBrickWallPage : ContentPage
{
    static AlgorithmicBrickWallPage()
    {
        const int brickWidth = 64;
        const int brickHeight = 24;
        const int morterThickness = 6;
        const int bitmapWidth = brickWidth + morterThickness;
        const int bitmapHeight = 2 * (brickHeight + morterThickness);

        SKBitmap bitmap = new SKBitmap(bitmapWidth, bitmapHeight);

        using (SKCanvas canvas = new SKCanvas(bitmap))
        using (SKPaint brickPaint = new SKPaint())
        {
            brickPaint.Color = new SKColor(0xB2, 0x22, 0x22);

            canvas.Clear(new SKColor(0xF0, 0xEA, 0xD6));
            canvas.DrawRect(new SKRect(morterThickness / 2,
                                       morterThickness / 2,
                                       morterThickness / 2 + brickWidth,
                                       morterThickness / 2 + brickHeight),
                                       brickPaint);

            int ySecondBrick = 3 * morterThickness / 2 + brickHeight;

            canvas.DrawRect(new SKRect(0,
                                       ySecondBrick,
                                       bitmapWidth / 2 - morterThickness / 2,
                                       ySecondBrick + brickHeight),
                                       brickPaint);

            canvas.DrawRect(new SKRect(bitmapWidth / 2 + morterThickness / 2,
                                       ySecondBrick,
                                       bitmapWidth,
                                       ySecondBrick + brickHeight),
                                       brickPaint);
        }

        // Save as public property for other programs
        BrickWallTile = bitmap;
    }

    public static SKBitmap BrickWallTile { private set; get; }
    ···
}
```

Итоговый точечный рисунок составляет 70 пикселей в ширину и 60 пикселей в высоком масштабе:

![Плитка "алгоритмическая кирпичная стенка"](bitmap-tiling-images/AlgorithmicBrickWallTile.png "Плитка "алгоритмическая кирпичная стенка"")

Оставшаяся часть страницы « **межмодульная розетка** » создает `SKShader` объект, который повторяет этот образ по горизонтали и вертикали:

```csharp
public class AlgorithmicBrickWallPage : ContentPage
{
    ···
    public AlgorithmicBrickWallPage ()
    {
        Title = "Algorithmic Brick Wall";

        // Create SKCanvasView
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
            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(BrickWallTile,
                                                 SKShaderTileMode.Repeat,
                                                 SKShaderTileMode.Repeat);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Ниже приведен результат.

[![Настенный стенной кирпич](bitmap-tiling-images/AlgorithmicBrickWall.png "Настенный стенной кирпич")](bitmap-tiling-images/AlgorithmicBrickWall-Large.png#lightbox)

Вы можете предпочесть нечто более реалистичное. В этом случае можно взять фотографию фактической кирпичной стенки и затем обрезать ее. Это битовая карта шириной 300 пикселей и высотой 150 пикселей:

![Плитка с кирпичной стеной](bitmap-tiling-images/BrickWallTile.jpg "Плитка с кирпичной стеной")

Это растровое изображение используется на странице « **фотография кирпича** »:

```csharp
public class PhotographicBrickWallPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PhotographicBrickWallPage),
                        "SkiaSharpFormsDemos.Media.BrickWallTile.jpg");

    public PhotographicBrickWallPage()
    {
        Title = "Photographic Brick Wall";

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
            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Обратите внимание, что `SKShaderTileMode` для `CreateBitmap` обоих аргументов задано значение `Mirror` . Этот параметр обычно необходим при использовании плиток, созданных из реальных изображений. Зеркальное отображение плиток позволяет избежать небесперебойности:

[![Фотографическая кирпичная стенка](bitmap-tiling-images/PhotographicBrickWall.png "Фотографическая кирпичная стенка")](bitmap-tiling-images/PhotographicBrickWall-Large.png#lightbox)

Для получения подходящего точечного рисунка для плитки требуется некоторое задание. Это не работает слишком хорошо, так как более темный модуль оказывается слишком большим. Он отображается регулярно в повторяющихся изображениях, что приводит к тому, что этот кирпич был создан из уменьшенного точечного рисунка.

В папку **Media** образца [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) также входит этот образ стены фишки:

![Плитка стены фишек](bitmap-tiling-images/StoneWallTile.jpg "Плитка стены фишек")

Однако исходное точечное изображение слишком велико для плитки. Его можно изменить, но `SKShader.CreateBitmap` метод также может изменить размер плитки, применив к ней преобразование. Этот параметр показан на странице « **стенка** ».

```csharp
public class StoneWallPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(StoneWallPage),
                        "SkiaSharpFormsDemos.Media.StoneWallTile.jpg");

    public StoneWallPage()
    {
        Title = "Stone Wall";

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
            // Create scale transform
            SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);

            // Create bitmap tiling
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror,
                                                 matrix);
            // Draw background
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

`SKMatrix`Значение создается для масштабирования изображения до половины исходного размера:

[![Стена](bitmap-tiling-images/StoneWall.png "Стена")](bitmap-tiling-images/StoneWall-Large.png#lightbox)

Работает ли преобразование с исходным точечным рисунком, используемым в `CreateBitmap` методе? Или преобразует результирующий массив плиток? 

Простой способ ответить на этот вопрос — включить вращение в состав преобразования:

```csharp
SKMatrix matrix = SKMatrix.MakeScale(0.5f, 0.5f);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(15));
```

Если преобразование применяется к отдельной плитке, то каждое повторное изображение плитки должно быть повернуто, а результат будет содержать много непустых строк. Но очевидно, что из этого снимка экрана преобразуется составной массив плиток:

[![Повернутая стенка](bitmap-tiling-images/StoneWallRotated.png "Повернутая стенка")](bitmap-tiling-images/StoneWallRotated-Large.png#lightbox)

В разделе [**Выравнивание мозаики**](#tile-alignment)раздела вы увидите пример преобразования Преобразование, примененного к шейдеру.

Пример автономного типа данных [**Cat**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock) (не часть **скиашарпформсдемос**) имитирует фон в виде мозаичного изображения, основанного на этом битовом рисунке размером 240 пикселей.

![Деревянное зерно](bitmap-tiling-images/WoodGrain.png "Деревянное зерно")

Это фотография деревянного этажа. `SKShaderTileMode.Mirror`Параметр позволяет отобразить его как гораздо большую область.

[![Часы CAT](bitmap-tiling-images/CatClock.png "Часы CAT")](bitmap-tiling-images/CatClock-Large.png#lightbox)

## <a name="tile-alignment"></a>Выравнивание плитки

Все приведенные выше примеры использовали шейдер, созданный, `SKShader.CreateBitmap` для покрытия всего холста. В большинстве случаев вы будете использовать мозаичное заполнение для хранения меньших областей или (более редко) для заполнения частей толстых линий. Вот плитка с графическим кирпичем, используемая для прямоугольника меньшего размера:

[![Выравнивание плитки](bitmap-tiling-images/TileAlignment.png "Выравнивание плитки")](bitmap-tiling-images/TileAlignment-Large.png#lightbox)

Это может показаться без проблем или нет. Возможно, вы перейдете, что шаблон мозаичного заполнения не начинается с полного модуля в левом верхнем углу прямоугольника. Это обусловлено тем, что шейдеры согласовываются с холстом, а не с графическим объектом, в котором они оформлены.

Исправление является простым. Создание `SKMatrix` значения на основе преобразования перевода. Это преобразование фактически сдвигает мозаичный шаблон на точку, в которой должен быть выравниваемая левый верхний угол плитки. Этот подход показан на странице **Выравнивание плиток** , которая создала изображение невыровненных плиток:

```csharp
public class TileAlignmentPage : ContentPage
{
    bool isAligned;

    public TileAlignmentPage()
    {
        Title = "Tile Alignment";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;

        // Add tap handler
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            isAligned ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);

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
            SKRect rect = new SKRect(info.Width / 7,
                                     info.Height / 7,
                                     6 * info.Width / 7,
                                     6 * info.Height / 7);

            // Get bitmap from other program
            SKBitmap bitmap = AlgorithmicBrickWallPage.BrickWallTile;

            // Create bitmap tiling
            if (!isAligned)
            {
                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat);
            }
            else
            {
                SKMatrix matrix = SKMatrix.MakeTranslation(rect.Left, rect.Top);

                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat,
                                                     matrix);
            }

            // Draw rectangle
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Страница **Выравнивание плиток** включает `TapGestureRecognizer` . Нажмите или щелкните экран, и программа переключается на `SKShader.CreateBitmap` метод с `SKMatrix` аргументом. Это преобразование сдвигает шаблон таким образом, чтобы верхний левый угол содержал полный модуль:

[![Выравнивание плитки касанием](bitmap-tiling-images/TileAlignmentTapped.png "Выравнивание плитки касанием")](bitmap-tiling-images/TileAlignmentTapped-Large.png#lightbox)

Этот метод также можно использовать для обеспечения центрирования шаблона мозаичного растрового изображения в области, в которой он рисуется. На странице **центрированные плитки** `PaintSurface` обработчик сначала вычисляет координаты так, как если бы оно отображало одно растровое изображение в центре холста. Затем он использует эти координаты, чтобы создать преобразование преобразования для `SKShader.CreateBitmap` . Это преобразование сдвигает весь шаблон таким образом, чтобы плитка выровнять по центру:

```csharp
public class CenteredTilesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(CenteredTilesPage),
                        "SkiaSharpFormsDemos.Media.monkey.png");

    public CenteredTilesPage ()
    {
        Title = "Centered Tiles";

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

        // Find coordinates to center bitmap in canvas...
        float x = (info.Width - bitmap.Width) / 2f;
        float y = (info.Height - bitmap.Height) / 2f;

        using (SKPaint paint = new SKPaint())
        {
            // ... but use them to create a translate transform
            SKMatrix matrix = SKMatrix.MakeTranslation(x, y);
            paint.Shader = SKShader.CreateBitmap(bitmap, 
                                                 SKShaderTileMode.Repeat, 
                                                 SKShaderTileMode.Repeat, 
                                                 matrix);

            // Use that tiled bitmap pattern to fill a circle
            canvas.DrawCircle(info.Rect.MidX, info.Rect.MidY,
                              Math.Min(info.Width, info.Height) / 2,
                              paint);
        }
    }
}
```

`PaintSurface`Обработчик завершает рисованием окружности в центре холста. Достаточно уверенно, что одна из плиток полностью находится в центре окружности, а другие — в симметричном шаблоне:

[![Центрированные плитки](bitmap-tiling-images/CenteredTiles.png "Центрированные плитки")](bitmap-tiling-images/CenteredTiles-Large.png#lightbox)

Другой подход по центру на самом деле немного проще. Вместо создания преобразования «Преобразование», которое помещает плитку в центре, можно выровнять угол мозаичного шаблона. В `SKMatrix.MakeTranslation` вызове Используйте аргументы для центра холста:

```csharp
SKMatrix matrix = SKMatrix.MakeTranslation(info.Rect.MidX, info.Rect.MidY);
```

Шаблон по-прежнему выравнивается по центру и симметрично, но в центре нет плитки:

[![Альтернативное центрирование плиток](bitmap-tiling-images/CenteredTilesAlternate.png "Альтернативное центрирование плиток")](bitmap-tiling-images/CenteredTilesAlternate-Large.png#lightbox)

## <a name="simplification-through-rotation"></a>Упрощение через поворот

Иногда использование преобразования поворота в `SKShader.CreateBitmap` методе может упростить плитку точечного рисунка. Это стало очевидно при попытке определить плитку для ограждения связи между цепочками. Файл **ChainLinkTile.CS** создает плитку, показанную здесь (с розовым фоном в целях ясности):

![Плитка с жесткой цепочкой](bitmap-tiling-images/HardChainLinkTile.png "Плитка с жесткой цепочкой")

Плитка должна включать две ссылки, чтобы код разбивает плитку на четыре квадранты. Левый верхний и правый нижний квадрант одинаковы, но они не являются полными. У проводов есть небольшие деления, которые необходимо обработать с помощью некоторой дополнительной прорисовки в правом верхнем и нижнем левом углу. Файл, выполняющий всю эту работу, составляет 174 строк.

Гораздо проще создать эту плитку:

![Плитка с более простым связыванием](bitmap-tiling-images/EasierChainLinkTile.png "Плитка с более простым связыванием")

Если повернутый шейдер с растровым изображением поворачивается на 90 градусов, визуальные элементы практически одинаковы.

Код для создания более простой плитки связи — это часть **плитки связи между цепочкой** . Конструктор определяет размер плитки на основе типа устройства, на котором запущена программа, а затем вызывает метод `CreateChainLinkTile` , который рисует на точечном рисунке, используя линии, контуры и градиентные шейдеры:

```csharp
public class ChainLinkFencePage : ContentPage
{
    ···
    SKBitmap tileBitmap;

    public ChainLinkFencePage ()
    {
        Title = "Chain-Link Fence";

        // Create bitmap for chain-link tiling
        int tileSize = Device.Idiom == TargetIdiom.Desktop ? 64 : 128;
        tileBitmap = CreateChainLinkTile(tileSize);

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    SKBitmap CreateChainLinkTile(int tileSize)
    {
        tileBitmap = new SKBitmap(tileSize, tileSize);
        float wireThickness = tileSize / 12f;

        using (SKCanvas canvas = new SKCanvas(tileBitmap))
        using (SKPaint paint = new SKPaint())
        {
            canvas.Clear();
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = wireThickness;
            paint.IsAntialias = true;

            // Draw straight wires first
            paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                         new SKPoint(0, tileSize),
                                                         new SKColor[] { SKColors.Silver, SKColors.Black },
                                                         new float[] { 0.4f, 0.6f },
                                                         SKShaderTileMode.Clamp);

            canvas.DrawLine(0, tileSize / 2,
                            tileSize / 2, tileSize / 2 - wireThickness / 2, paint);

            canvas.DrawLine(tileSize, tileSize / 2,
                            tileSize / 2, tileSize / 2 + wireThickness / 2, paint);

            // Draw curved wires
            using (SKPath path = new SKPath())
            {
                path.MoveTo(tileSize / 2, 0);
                path.LineTo(tileSize / 2 - wireThickness / 2, tileSize / 2);
                path.ArcTo(wireThickness / 2, wireThickness / 2,
                           0,
                           SKPathArcSize.Small,
                           SKPathDirection.CounterClockwise,
                           tileSize / 2, tileSize / 2 + wireThickness / 2);

                paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                             new SKPoint(0, tileSize),
                                                             new SKColor[] { SKColors.Silver, SKColors.Black },
                                                             null,
                                                             SKShaderTileMode.Clamp);
                canvas.DrawPath(path, paint);

                path.Reset();
                path.MoveTo(tileSize / 2, tileSize);
                path.LineTo(tileSize / 2 + wireThickness / 2, tileSize / 2);
                path.ArcTo(wireThickness / 2, wireThickness / 2,
                           0,
                           SKPathArcSize.Small,
                           SKPathDirection.CounterClockwise,
                           tileSize / 2, tileSize / 2 - wireThickness / 2);

                paint.Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0),
                                                             new SKPoint(0, tileSize),
                                                             new SKColor[] { SKColors.White, SKColors.Silver },
                                                             null,
                                                             SKShaderTileMode.Clamp);
                canvas.DrawPath(path, paint);
            }
            return tileBitmap;
        }
    }
    ···
}
```

За исключением проводов, плитка прозрачна, а это означает, что ее можно отобразить поверх чего-нибудь другого. Программа загружает один из ресурсов точечного рисунка, отображает его для заполнения холста, а затем рисует шейдер поверх остальных элементов:

```csharp
public class ChainLinkFencePage : ContentPage
{
    SKBitmap monkeyBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(ChainLinkFencePage), "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");
    ···

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawBitmap(monkeyBitmap, info.Rect, BitmapStretch.UniformToFill, 
                            BitmapAlignment.Center, BitmapAlignment.Start);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(tileBitmap, 
                                                 SKShaderTileMode.Repeat,
                                                 SKShaderTileMode.Repeat,
                                                 SKMatrix.MakeRotationDegrees(45));
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Обратите внимание, что шейдер поворачивается в 45 градусов, поэтому он ориентирован как реальная огражденная цепочка связей:

[![Ограждение связи между цепочкой](bitmap-tiling-images/ChainLinkFence.png "Ограждение связи между цепочкой")](bitmap-tiling-images/ChainLinkFence-Large.png#lightbox)

## <a name="animating-bitmap-tiles"></a>Анимация плиток с растровыми изображениями

Анимацию преобразования матрицы можно анимировать целиком с помощью анимации. Возможно, вы хотите, чтобы шаблон переместились горизонтально или вертикально, или оба. Это можно сделать, создав преобразование перевода на основе координат сдвига.

Можно также рисовать на небольшом точечном рисунке или для обработки битов пикселов растрового изображения с частотой 60 раз в секунду. Затем этот точечный рисунок можно использовать для мозаичного заполнения, и весь шаблон мозаичного заполнения может показаться анимированным. 

Этот подход показан на странице **плитки анимированного растрового изображения** . Растровое изображение создается в виде поля, которое будет иметь значение 64 пикселей в квадрате. Конструктор вызывает метод `DrawBitmap` , чтобы присвоить ему исходный вид. Если `angle` поле равно нулю (как это происходит при первом вызове метода), то битовая карта содержит две строки, которые были перепутаны как X. Строки выполняются достаточно долго, чтобы всегда достигать границы точечного рисунка независимо от `angle` значения: 

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    const int SIZE = 64;

    SKCanvasView canvasView;
    SKBitmap bitmap = new SKBitmap(SIZE, SIZE);
    float angle;
    ···

    public AnimatedBitmapTilePage ()
    {
        Title = "Animated Bitmap Tile";

        // Initialize bitmap prior to animation
        DrawBitmap();

        // Create SKCanvasView 
        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
    void DrawBitmap()
    {
        using (SKCanvas canvas = new SKCanvas(bitmap))
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = SIZE / 8;

            canvas.Clear();
            canvas.Translate(SIZE / 2, SIZE / 2);
            canvas.RotateDegrees(angle);
            canvas.DrawLine(-SIZE, -SIZE, SIZE, SIZE, paint);
            canvas.DrawLine(-SIZE, SIZE, SIZE, -SIZE, paint);
        }
    }
    ···
}
```

Затраты на анимацию возникают в `OnAppearing` `OnDisappearing` переопределениях и. `OnTimerTick`Метод анимируется `angle` значение от 0 градусов до 360 градусов каждые 10 секунд для поворота X фигуры в точечном рисунке:

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    ···
    // For animation
    bool isAnimating;
    Stopwatch stopwatch = new Stopwatch();
    ···

    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        const int duration = 10;     // seconds
        angle = (float)(360f * (stopwatch.Elapsed.TotalSeconds % duration) / duration);
        DrawBitmap();
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

Из-за симметрии X, это то же самое, что поворот `angle` значения от 0 градусов до 90 градусов каждые 2,5 секунд.

`PaintSurface`Обработчик создает шейдер из точечного рисунка и использует объект Paint для выделения цветом всего холста:

```csharp
public class AnimatedBitmapTilePage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateBitmap(bitmap,
                                                 SKShaderTileMode.Mirror,
                                                 SKShaderTileMode.Mirror);
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

`SKShaderTileMode.Mirror`Параметры гарантируют, что руки x в каждом битовом соединении соединяются с x в смежных точечных рисунках, чтобы создать общую анимированную модель, которая кажется гораздо более сложной, чем предложит простую анимацию:

[![Анимированная плитка точечного рисунка](bitmap-tiling-images/AnimatedBitmapTile.png "Анимированная плитка точечного рисунка")](bitmap-tiling-images/AnimatedBitmapTile-Large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Катклокк (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/catclock)
