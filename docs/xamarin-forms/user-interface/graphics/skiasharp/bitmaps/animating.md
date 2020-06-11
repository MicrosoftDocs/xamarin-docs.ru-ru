---
Title: "анимирование растровых изображений SkiaSharp" Description: "Узнайте, как выполнять растровую анимацию путем последовательного отображения ряда точечных рисунков и визуализации анимированных GIF-файлов".
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD Автор: давидбритч MS. author: дабритч MS. Дата: 07/12/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="animating-skiasharp-bitmaps"></a>Анимация точечных рисунков SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Приложения, которые анимировать SkiaSharp графику, обычно вызываются по `InvalidateSurface` `SKCanvasView` фиксированной частоте, часто каждые 16 миллисекунд. Недействительность поверхности вызывает `PaintSurface` обработчик для перерисовки отображения. По мере перерисовки визуальных элементов 60 раз в секунду они выглядят плавно анимированными.

Однако если графика слишком сложна для подготовки к просмотру в 16 миллисекунд, то анимация может оказаться нестабильной. Программист может уменьшить частоту обновления до 30 раз или 15 раз в секунду, но иногда даже недостаточна. Иногда графика настолько сложна, что они просто не могут быть отображены в режиме реального времени.

Одно из решений состоит в подготовке анимации к просмотру отдельных кадров анимации для ряда точечных рисунков. Чтобы отобразить анимацию, необходимо последовательное отображение этих растровых изображений последовательно 60 раза в секунду.

Разумеется, это может быть большой объем точечных рисунков, но именно здесь создаются трехмерные 3D-анимационные фильмы. Трехмерная графика слишком сложна для подготовки к просмотру в режиме реального времени. Для отображения каждого кадра требуется много времени обработки. То, что вы видите при просмотре фильма, — это, по сути, серия точечных рисунков.

Вы можете сделать что-то подобное в SkiaSharp. В этой статье показаны два типа растровой анимации. Первый пример — это анимация набора Мандельброта:

![Пример анимации](animating-images/AnimatingSample.png "Пример анимации")

Во втором примере показано, как использовать SkiaSharp для визуализации анимированного GIF-файла.

## <a name="bitmap-animation"></a>Растровая анимация

Установка фрактала Мандельброта визуально захватывающая, но компутионалли длина. (Описание набора фрактала Мандельброта и математических операций, использованных здесь, см. в [главе 20 _Создание мобильных приложений с помощью Xamarin. Forms_ ](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) , начиная со страницы 666. В следующем описании предполагается, что основные знания.)

Пример [**анимации фрактала Мандельброта**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima) использует растровую анимацию для имитации непрерывного масштабирования фиксированной точки в наборе Мандельброта. Для масштабирования следует выполнить уменьшение масштаба, а затем цикл повторяется непрерывно или до завершения программы.

Программа готовится к этой анимации, создавая до 50 битовых карт, которые он хранит в локальном хранилище приложения. Каждый точечный рисунок охватывает половину ширины и высоты сложной плоскости в виде предыдущего растрового изображения. (В программе эти точечные рисунки говорят, что представляют собой целочисленные _уровни масштабирования_.) Растровые изображения отображаются последовательно. Масштабирование каждого растрового изображения анимируется для обеспечения плавного продвижения от одного растрового изображения к другому.

Как и в финальной программе, описанной в главе 20 _Создание мобильных приложений с помощью Xamarin. Forms_, вычисление фрактала Мандельброта в **анимации фрактала Мандельброта** — это асинхронный метод с восемью параметрами. Параметры включают в себя комплексную центральную точку, а также ширину и высоту сложной плоскости, окружающей эту центральную точку. Следующие три параметра — Ширина и высота пикселя создаваемого растрового изображения, а также максимальное количество итераций для рекурсивного вычисления. `progress`Параметр используется для просмотра хода выполнения этого вычисления. `cancelToken`Параметр не используется в этой программе:

```csharp
static class Mandelbrot
{
    public static Task<BitmapInfo> CalculateAsync(Complex center,
                                                  double width, double height,
                                                  int pixelWidth, int pixelHeight,
                                                  int iterations,
                                                  IProgress<double> progress,
                                                  CancellationToken cancelToken)
    {
        return Task.Run(() =>
        {
            int[] iterationCounts = new int[pixelWidth * pixelHeight];
            int index = 0;

            for (int row = 0; row < pixelHeight; row++)
            {
                progress.Report((double)row / pixelHeight);
                cancelToken.ThrowIfCancellationRequested();

                double y = center.Imaginary + height / 2 - row * height / pixelHeight;

                for (int col = 0; col < pixelWidth; col++)
                {
                    double x = center.Real - width / 2 + col * width / pixelWidth;
                    Complex c = new Complex(x, y);

                    if ((c - new Complex(-1, 0)).Magnitude < 1.0 / 4)
                    {
                        iterationCounts[index++] = -1;
                    }
                    // http://www.reenigne.org/blog/algorithm-for-mandelbrot-cardioid/
                    else if (c.Magnitude * c.Magnitude * (8 * c.Magnitude * c.Magnitude - 3) < 3.0 / 32 - c.Real)
                    {
                        iterationCounts[index++] = -1;
                    }
                    else
                    {
                        Complex z = 0;
                        int iteration = 0;

                        do
                        {
                            z = z * z + c;
                            iteration++;
                        }
                        while (iteration < iterations && z.Magnitude < 2);

                        if (iteration == iterations)
                        {
                            iterationCounts[index++] = -1;
                        }
                        else
                        {
                            iterationCounts[index++] = iteration;
                        }
                    }
                }
            }
            return new BitmapInfo(pixelWidth, pixelHeight, iterationCounts);
        }, cancelToken);
    }
}
```

Метод возвращает объект типа `BitmapInfo` , предоставляющий сведения для создания точечного рисунка.

```csharp
class BitmapInfo
{
    public BitmapInfo(int pixelWidth, int pixelHeight, int[] iterationCounts)
    {
        PixelWidth = pixelWidth;
        PixelHeight = pixelHeight;
        IterationCounts = iterationCounts;
    }

    public int PixelWidth { private set; get; }

    public int PixelHeight { private set; get; }

    public int[] IterationCounts { private set; get; }
}
```

XAML-файл **анимации фрактала Мандельброта** включает два `Label` представления `ProgressBar` :, и a, а `Button` также `SKCanvasView` .

```csharp
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="MandelAnima.MainPage"
             Title="Mandelbrot Animation">

    <StackLayout>
        <Label x:Name="statusLabel"
               HorizontalTextAlignment="Center" />
        <ProgressBar x:Name="progressBar" />

        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     Padding="5">
            <Label x:Name="storageLabel"
                   VerticalOptions="Center" />

            <Button x:Name="deleteButton"
                    Text="Delete All"
                    HorizontalOptions="EndAndExpand"
                    Clicked="OnDeleteButtonClicked" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Файл кода программной части начинается с определения трех важных констант и массива растровых изображений:

```csharp
public partial class MainPage : ContentPage
{
    const int COUNT = 10;           // The number of bitmaps in the animation.
                                    // This can go up to 50!

    const int BITMAP_SIZE = 1000;   // Program uses square bitmaps exclusively

    // Uncomment just one of these, or define your own
    static readonly Complex center = new Complex(-1.17651152924355, 0.298520986549558);
    //   static readonly Complex center = new Complex(-0.774693089457127, 0.124226621261617);
    //   static readonly Complex center = new Complex(-0.556624880053304, 0.634696788141351);

    SKBitmap[] bitmaps = new SKBitmap[COUNT];   // array of bitmaps
    ···
}
```

В некоторый момент вы, вероятно, захотите изменить `COUNT` значение на 50, чтобы увидеть весь диапазон анимации. Значения выше 50 не используются. На уровне масштабирования 48 или выше разрешение чисел с плавающей запятой двойной точности станет недостаточным для вычисления установки фрактала Мандельброта. Эта проблема обсуждается на странице 684, посвященной _созданию мобильных приложений с помощью Xamarin. Forms_.

`center`Это значение очень важно. Это фокус масштабирования анимации. Три значения в файле — это те, которые используются в трех последних снимках экрана в главе 20 _создания мобильных приложений с помощью Xamarin. Forms_ на странице 684, но вы можете поэкспериментировать с программой в этой главе, чтобы получить одно из собственных значений.

Пример **анимации фрактала Мандельброта** сохраняет эти `COUNT` растровые изображения в локальном хранилище приложений. 50. точечные рисунки на устройстве должны иметь более 20 мегабайт памяти, поэтому может потребоваться выяснить, какой объем хранилища занимают эти точечные рисунки, и в какой момент вы можете их удалить. Это назначение этих двух методов в нижней части `MainPage` класса:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void TallyBitmapSizes()
    {
        long fileSize = 0;

        foreach (string filename in Directory.EnumerateFiles(FolderPath()))
        {
            fileSize += new FileInfo(filename).Length;
        }

        storageLabel.Text = $"Total storage: {fileSize:N0} bytes";
    }

    void OnDeleteButtonClicked(object sender, EventArgs args)
    {
        foreach (string filepath in Directory.EnumerateFiles(FolderPath()))
        {
            File.Delete(filepath);
        }

        TallyBitmapSizes();
    }
}
```

Вы можете удалить точечные рисунки в локальном хранилище, пока программа анимирована эти растровые изображения, так как программа удерживает их в памяти. Но при следующем запуске программы потребуется повторно создать точечные рисунки.

Точечные рисунки, хранящиеся в локальном хранилище приложений `center` , включают значение в имена файлов, поэтому при изменении `center` параметра существующие растровые изображения не заменяются в хранилище и продолжают занимать место.

Ниже приведены методы, `MainPage` используемые для создания имен файлов, а также `MakePixel` метод определения значения пикселя на основе цветовых компонентов:

```csharp
public partial class MainPage : ContentPage
{
    ···
    // File path for storing each bitmap in local storage
    string FolderPath() =>
        Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);

    string FilePath(int zoomLevel) =>
        Path.Combine(FolderPath(),
                     String.Format("R{0}I{1}Z{2:D2}.png", center.Real, center.Imaginary, zoomLevel));

    // Form bitmap pixel for Rgba8888 format
    uint MakePixel(byte alpha, byte red, byte green, byte blue) =>
        (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

`zoomLevel`Параметр в `FilePath` диапазоне от 0 до `COUNT` константы минус 1.

`MainPage`Конструктор вызывает `LoadAndStartAnimation` метод:

```csharp
public partial class MainPage : ContentPage
{
    ···
    public MainPage()
    {
        InitializeComponent();

        LoadAndStartAnimation();
    }
    ···
}
```

`LoadAndStartAnimation`Метод отвечает за доступ к локальному хранилищу приложения для загрузки любых растровых изображений, которые могли быть созданы при выполнении программы ранее. Он проходит по `zoomLevel` значениям от 0 до `COUNT` . Если файл существует, он загружается в `bitmaps` массив. В противном случае ему нужно создать точечный рисунок для `center` определенных `zoomLevel` значений и, вызвав метод `Mandelbrot.CalculateAsync` . Этот метод получает количество итераций для каждого пикселя, который преобразуется в цвета:

```csharp
public partial class MainPage : ContentPage
{
    ···
    async void LoadAndStartAnimation()
    {
        // Show total bitmap storage
        TallyBitmapSizes();

        // Create progressReporter for async operation
        Progress<double> progressReporter =
            new Progress<double>((double progress) => progressBar.Progress = progress);

        // Create (unused) CancellationTokenSource for async operation
        CancellationTokenSource cancelTokenSource = new CancellationTokenSource();

        // Loop through all the zoom levels
        for (int zoomLevel = 0; zoomLevel < COUNT; zoomLevel++)
        {
            // If the file exists, load it
            if (File.Exists(FilePath(zoomLevel)))
            {
                statusLabel.Text = $"Loading bitmap for zoom level {zoomLevel}";

                using (Stream stream = File.OpenRead(FilePath(zoomLevel)))
                {
                    bitmaps[zoomLevel] = SKBitmap.Decode(stream);
                }
            }
            // Otherwise, create a new bitmap
            else
            {
                statusLabel.Text = $"Creating bitmap for zoom level {zoomLevel}";

                CancellationToken cancelToken = cancelTokenSource.Token;

                // Do the (generally lengthy) Mandelbrot calculation
                BitmapInfo bitmapInfo =
                    await Mandelbrot.CalculateAsync(center,
                                                    4 / Math.Pow(2, zoomLevel),
                                                    4 / Math.Pow(2, zoomLevel),
                                                    BITMAP_SIZE, BITMAP_SIZE,
                                                    (int)Math.Pow(2, 10), progressReporter, cancelToken);

                // Create bitmap & get pointer to the pixel bits
                SKBitmap bitmap = new SKBitmap(BITMAP_SIZE, BITMAP_SIZE, SKColorType.Rgba8888, SKAlphaType.Opaque);
                IntPtr basePtr = bitmap.GetPixels();

                // Set pixel bits to color based on iteration count
                for (int row = 0; row < bitmap.Width; row++)
                    for (int col = 0; col < bitmap.Height; col++)
                    {
                        int iterationCount = bitmapInfo.IterationCounts[row * bitmap.Width + col];
                        uint pixel = 0xFF000000;            // black

                        if (iterationCount != -1)
                        {
                            double proportion = (iterationCount / 32.0) % 1;
                            byte red = 0, green = 0, blue = 0;

                            if (proportion < 0.5)
                            {
                                red = (byte)(255 * (1 - 2 * proportion));
                                blue = (byte)(255 * 2 * proportion);
                            }
                            else
                            {
                                proportion = 2 * (proportion - 0.5);
                                green = (byte)(255 * proportion);
                                blue = (byte)(255 * (1 - proportion));
                            }

                            pixel = MakePixel(0xFF, red, green, blue);
                        }

                        // Calculate pointer to pixel
                        IntPtr pixelPtr = basePtr + 4 * (row * bitmap.Width + col);

                        unsafe     // requires compiling with unsafe flag
                        {
                            *(uint*)pixelPtr.ToPointer() = pixel;
                        }
                    }

                // Save as PNG file
                SKData data = SKImage.FromBitmap(bitmap).Encode();

                try
                {
                    File.WriteAllBytes(FilePath(zoomLevel), data.ToArray());
                }
                catch
                {
                    // Probably out of space, but just ignore
                }

                // Store in array
                bitmaps[zoomLevel] = bitmap;

                // Show new bitmap sizes
                TallyBitmapSizes();
            }

            // Display the bitmap
            bitmapIndex = zoomLevel;
            canvasView.InvalidateSurface();
        }

        // Now start the animation
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }
    ···
}
```

Обратите внимание, что программа сохраняет эти точечные рисунки в локальном хранилище приложений, а не в библиотеке фотографий устройства. Библиотека .NET Standard 2,0 позволяет использовать знакомые `File.OpenRead` методы и `File.WriteAllBytes` для этой задачи.

После того как все точечные рисунки созданы или загружены в память, метод запускает `Stopwatch` объект и вызывает `Device.StartTimer` . `OnTimerTick`Метод вызывается каждые 16 миллисекунд.

`OnTimerTick`вычисляет `time` значение в миллисекундах в диапазоне от 0 до 6000 раз `COUNT` , которое выводит шесть секунд для каждого растрового изображения. `progress`Значение использует `Math.Sin` значение для создания анимации синусоидальной, которая будет выполняться медленнее в начале цикла, и медленнее в конце, так как в обратном направлении.

`progress`Значение лежит в диапазоне от 0 до `COUNT` . Это означает, что целая часть `progress` является индексом в `bitmaps` массиве, а дробная часть `progress` указывает на уровень масштабирования для этого конкретного точечного рисунка. Эти значения хранятся в `bitmapIndex` полях и и `bitmapProgress` отображаются в `Label` и `Slider` в файле XAML. `SKCanvasView`Является недействительным для обновления экрана растрового изображения:

```csharp
public partial class MainPage : ContentPage
{
    ···
    Stopwatch stopwatch = new Stopwatch();      // for the animation
    int bitmapIndex;
    double bitmapProgress = 0;
    ···
    bool OnTimerTick()
    {
        int cycle = 6000 * COUNT;       // total cycle length in milliseconds

        // Time in milliseconds from 0 to cycle
        int time = (int)(stopwatch.ElapsedMilliseconds % cycle);

        // Make it sinusoidal, including bitmap index and gradation between bitmaps
        double progress = COUNT * 0.5 * (1 + Math.Sin(2 * Math.PI * time / cycle - Math.PI / 2));

        // These are the field values that the PaintSurface handler uses
        bitmapIndex = (int)progress;
        bitmapProgress = progress - bitmapIndex;

        // It doesn't often happen that we get up to COUNT, but an exception would be raised
        if (bitmapIndex < COUNT)
        {
            // Show progress in UI
            statusLabel.Text = $"Displaying bitmap for zoom level {bitmapIndex}";
            progressBar.Progress = bitmapProgress;

            // Update the canvas
            canvasView.InvalidateSurface();
        }

        return true;
    }
    ···
}
```

Наконец, `PaintSurface` обработчик `SKCanvasView` вычисляет прямоугольник назначения, чтобы отображать точечный рисунок максимально крупным образом при сохранении пропорций. Исходный прямоугольник основан на `bitmapProgress` значении. `fraction`Значение, вычисленное здесь, находится в диапазоне от 0 `bitmapProgress` , если равно 0, чтобы отобразить всю битовую карту, до 0,25, если `bitmapProgress` значение равно 1, чтобы отобразить половину ширины и высоты точечного рисунка. эффективное масштабирование:

```csharp
public partial class MainPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        if (bitmaps[bitmapIndex] != null)
        {
            // Determine destination rect as square in canvas
            int dimension = Math.Min(info.Width, info.Height);
            float x = (info.Width - dimension) / 2;
            float y = (info.Height - dimension) / 2;
            SKRect destRect = new SKRect(x, y, x + dimension, y + dimension);

            // Calculate source rectangle based on fraction:
            //  bitmapProgress == 0: full bitmap
            //  bitmapProgress == 1: half of length and width of bitmap
            float fraction = 0.5f * (1 - (float)Math.Pow(2, -bitmapProgress));
            SKBitmap bitmap = bitmaps[bitmapIndex];
            int width = bitmap.Width;
            int height = bitmap.Height;
            SKRect sourceRect = new SKRect(fraction * width, fraction * height,
                                           (1 - fraction) * width, (1 - fraction) * height);

            // Display the bitmap
            canvas.DrawBitmap(bitmap, sourceRect, destRect);
        }
    }
    ···
}
```

Вот работающая программа:

[![Анимация фрактала Мандельброта](animating-images/MandelbrotAnimation.png "Анимация фрактала Мандельброта")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>Анимация GIF

Спецификация GIF включает в себя функцию, которая позволяет одному GIF-файлу содержать несколько последовательных кадров сцены, которые могут отображаться в непрерывном виде, часто в цикле. Эти файлы называются _анимированными GIF_. Веб-браузеры могут воспроизводить анимированные GIF, а SkiaSharp позволяет приложению извлекать кадры из анимированного GIF-файла и отображать их последовательно.

Пример [скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) включает в себя анимированный ресурс GIF с именем **Newtons_cradle_animation_book_2.gif** , созданный демонделуксе и скачанный на странице [док Ньютона](https://en.wikipedia.org/wiki/Newton%27s_cradle) в Википедии. **Анимированная страница GIF** содержит файл XAML, который предоставляет эти сведения и создает экземпляр `SKCanvasView` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.AnimatedGifPage"
             Title="Animated GIF">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="GIF file by DemonDeLuxe from Wikipedia Newton's Cradle page"
               Grid.Row="1"
               Margin="0, 5"
               HorizontalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Файл кода программной части не является обобщенным для воспроизведения любого анимированного GIF-файла. Он игнорирует некоторые доступные сведения, в частности число повторений, и просто воспроизводит анимированный GIF в цикле.

Использование Скисшарп для извлечения кадров анимированного GIF-файла не задокументировано в любом месте, поэтому описание следующего кода более подробно, чем обычно:

Декодирование анимированного GIF-файла происходит в конструкторе страницы и требует, `Stream` чтобы объект, ссылающийся на точечный рисунок, использовался для создания `SKManagedStream` объекта, а затем [`SKCodec`](xref:SkiaSharp.SKCodec) объект. [`FrameCount`](xref:SkiaSharp.SKCodec.FrameCount)Свойство указывает число кадров, составляющих анимацию.

В конечном итоге эти кадры сохраняются как отдельные точечные рисунки, поэтому конструктор использует `FrameCount` для выделения массива типа, а `SKBitmap` также двух `int` массивов в течение каждого кадра и (для упрощения логики анимации) накопленные длительности.

[`FrameInfo`](xref:SkiaSharp.SKCodec.FrameInfo)Свойство `SKCodec` класса является массивом [`SKCodecFrameInfo`](xref:SkiaSharp.SKCodecFrameInfo) значений, по одному для каждого кадра, но единственное, что эта программа получает из этой структуры, — это значение [`Duration`](xref:SkiaSharp.SKCodecFrameInfo.Duration) кадра в миллисекундах.

`SKCodec`Определяет свойство с именем [`Info`](xref:SkiaSharp.SKCodec.Info) типа [`SKImageInfo`](xref:SkiaSharp.SKImageInfo) , но это `SKImageInfo` значение обозначает (по крайней мере для этого изображения) тип цвета `SKColorType.Index8` , что означает, что каждый пиксель является индексом типа цвета. Чтобы не тратиться на таблицы цветов, программа использует [`Width`](xref:SkiaSharp.SKImageInfo.Width) [`Height`](xref:SkiaSharp.SKImageInfo.Height) информацию и из этой структуры для создания собственного значения полного цвета `ImageInfo` . Каждый `SKBitmap` из них создается на основе.

`GetPixels`Метод `SKBitmap` возвращает объект `IntPtr` , ссылающийся на пиксельные биты этого растрового изображения. Эти пиксельные биты еще не установлены. , `IntPtr` Передаваемый в один из [`GetPixels`](xref:SkiaSharp.SKCodec.GetPixels(SkiaSharp.SKImageInfo,System.IntPtr,SkiaSharp.SKCodecOptions)) методов `SKCodec` . Этот метод копирует кадр из GIF-файла в пространство памяти, на которое ссылается `IntPtr` . [`SKCodecOptions`](xref:SkiaSharp.SKCodecOptions)Конструктор указывает номер кадра:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;
    ···

    public AnimatedGifPage ()
    {
        InitializeComponent ();

        string resourceID = "SkiaSharpFormsDemos.Media.Newtons_cradle_animation_book_2.gif";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        using (SKCodec codec = SKCodec.Create(skStream))
        {
            // Get frame count and allocate bitmaps
            int frameCount = codec.FrameCount;
            bitmaps = new SKBitmap[frameCount];
            durations = new int[frameCount];
            accumulatedDurations = new int[frameCount];

            // Note: There's also a RepetitionCount property of SKCodec not used here

            // Loop through the frames
            for (int frame = 0; frame < frameCount; frame++)
            {
                // From the FrameInfo collection, get the duration of each frame
                durations[frame] = codec.FrameInfo[frame].Duration;

                // Create a full-color bitmap for each frame
                SKImageInfo imageInfo = code.new SKImageInfo(codec.Info.Width, codec.Info.Height);
                bitmaps[frame] = new SKBitmap(imageInfo);

                // Get the address of the pixels in that bitmap
                IntPtr pointer = bitmaps[frame].GetPixels();

                // Create an SKCodecOptions value to specify the frame
                SKCodecOptions codecOptions = new SKCodecOptions(frame, false);

                // Copy pixels from the frame into the bitmap
                codec.GetPixels(imageInfo, pointer, codecOptions);
            }

            // Sum up the total duration
            for (int frame = 0; frame < durations.Length; frame++)
            {
                totalDuration += durations[frame];
            }

            // Calculate the accumulated durations
            for (int frame = 0; frame < durations.Length; frame++)
            {
                accumulatedDurations[frame] = durations[frame] +
                    (frame == 0 ? 0 : accumulatedDurations[frame - 1]);
            }
        }
    }
    ···
}
```

Несмотря на `IntPtr` значение, код не `unsafe` требуется, так как `IntPtr` никогда не преобразуется в значение указателя C#.

После извлечения каждого кадра конструктор суммирует длительность всех кадров, а затем инициализирует другой массив с накопленной длительностью.

Оставшаяся часть файла с выделенным кодом выделяется для анимации. `Device.StartTimer`Метод используется для запуска таймера, а `OnTimerTick` обратный вызов использует `Stopwatch` объект для определения затраченного времени в миллисекундах. Для поиска текущего кадра достаточно прокрутить массив накопленных значений длительности:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;

    Stopwatch stopwatch = new Stopwatch();
    bool isAnimating;

    int currentFrame;
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
        int msec = (int)(stopwatch.ElapsedMilliseconds % totalDuration);
        int frame = 0;

        // Find the frame based on the elapsed time
        for (frame = 0; frame < accumulatedDurations.Length; frame++)
        {
            if (msec < accumulatedDurations[frame])
            {
                break;
            }
        }

        // Save in a field and invalidate the SKCanvasView.
        if (currentFrame != frame)
        {
            currentFrame = frame;
            canvasView.InvalidateSurface();
        }

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Get the bitmap and center it
        SKBitmap bitmap = bitmaps[currentFrame];
        canvas.DrawBitmap(bitmap,info.Rect, BitmapStretch.Uniform);
    }
}
```

Каждый раз, когда `currentframe` переменная изменяется, `SKCanvasView` становится недействительным и отображается новый кадр:

[![Анимированный GIF](animating-images/AnimatedGif.png "Анимированный GIF")](animating-images/AnimatedGif-Large.png#lightbox)

Конечно, вам нужно самостоятельно запустить программу, чтобы увидеть анимацию.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Анимация фрактала Мандельброта (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)
