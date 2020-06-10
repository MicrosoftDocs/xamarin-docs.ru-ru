---
Title: "доступ к битам точечного рисунка SkiaSharp" Описание ":" Узнайте о различных методах доступа и изменения пиксельных битов SkiaSharp точечных рисунков ".
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: DBB58522-F816-4A8C-96A5-E0236F16A5C6 Автор: давидбритч MS. author: дабритч MS. Дата: 07/11/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="accessing-skiasharp-bitmap-pixel-bits"></a>Доступ к битовым точкам точечного рисунка SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Как было показано в статье [**Сохранение точечных рисунков SkiaSharp в файлах**](saving.md), точечные рисунки обычно хранятся в файлах в сжатом формате, таком как JPEG или PNG. В большинстве случаях точечный рисунок SkiaSharp, хранящийся в памяти, не сжимается. Он хранится в виде последовательности пикселей. Этот формат без сжатия упрощает перенос точечных рисунков на экранную поверхность.

Блок памяти, занимаемый точечным рисунком SkiaSharp, организован очень просто: он начинается с первой строки пикселей слева направо, а затем продолжается со второй строки. Для полноразмерных точечных рисунков каждая точка состоит из четырех байтов, что означает, что общее пространство памяти, необходимое для точечного рисунка, будет в четыре раза больше, чем произведение его ширины и высоты.

В этой статье описывается, как приложение может получить доступ к этим точкам непосредственно путем доступа к блоку памяти точечного рисунка или косвенно. В некоторых случаях программе может потребоваться проанализировать пикселы изображения и создать гистограмму с некоторой сортировкой. Чаще всего приложения могут создавать уникальные образы, алгоритмически создавая Пиксели, составляющие точечный рисунок:

![Примеры битов пикселей](pixel-bits-images/PixelBitsSample.png "Пример точечного бита")

## <a name="the-techniques"></a>Методы

SkiaSharp предоставляет несколько методов для доступа к битовым битам точечного рисунка. Выбор одного из них обычно является компромиссом между удобством написания кода (которое связано с обслуживанием и простотой отладки) и производительностью. В большинстве случаев `SKBitmap` для доступа к пикселям точечного рисунка используется один из следующих методов и свойств.

- `GetPixel`Методы и `SetPixel` позволяют получить или задать цвет одного пикселя.
- `Pixels`Свойство получает массив цветов пикселей для всего растрового изображения или задает массив цветов.
- `GetPixels`Возвращает адрес пиксельной памяти, используемой точечным рисунком.
- `SetPixels`заменяет адрес памяти пикселя, используемый точечным рисунком.

Первые два метода можно представить как "высокий уровень", а второй — как "низкий уровень". Существуют и другие методы и свойства, которые можно использовать, но они являются наиболее ценными.

Чтобы увидеть различия в производительности между этими методами, приложение [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) содержит страницу с именем **градиентного растрового изображения** , которая создает точечный рисунок с пикселами, которые объединяют красные и синие тени для создания градиента. Программа создает восемь различных копий этого точечного рисунка, используя разные методы настройки пикселей точечного рисунка. Каждый из этих восьми битовых карт создается в отдельном методе, который также задает краткое описание метода и вычисляет время, необходимое для установки всех пикселов. Каждый метод циклически проходит логику настройки пикселей 100 раз, чтобы получить лучшую оценку производительности.

### <a name="the-setpixel-method"></a>Метод SetPixel

Если необходимо задать или получить несколько отдельных пикселов, то [`SetPixel`](xref:SkiaSharp.SKBitmap.SetPixel(System.Int32,System.Int32,SkiaSharp.SKColor)) [`GetPixel`](xref:SkiaSharp.SKBitmap.GetPixel(System.Int32,System.Int32)) методы и являются идеальными. Для каждого из этих двух методов указывается целочисленный столбец и строка. Независимо от формата пикселей эти два метода позволяют получить или задать пиксель в качестве `SKColor` значения:

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

`col`Аргумент должен находиться в диапазоне от 0 до значения, меньшего `Width` , чем свойство растрового изображения, и в `row` диапазоне от 0 до одного меньше значения `Height` Свойства.

Ниже приведен метод в **битовой карте градиента** , который задает содержимое растрового изображения с помощью `SetPixel` метода. Битовая карта 256 на 256 пикселей, а `for` циклы жестко запрограммированы диапазоном значений:

```csharp
public class GradientBitmapPage : ContentPage
{
    const int REPS = 100;

    Stopwatch stopwatch = new Stopwatch();
    ···
    SKBitmap FillBitmapSetPixel(out string description, out int milliseconds)
    {
        description = "SetPixel";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        for (int rep = 0; rep < REPS; rep++)
            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    bitmap.SetPixel(col, row, new SKColor((byte)col, 0, (byte)row));
                }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
}
```

Цветовой набор для каждого пикселя имеет красный компонент, равный столбцу Bitmap, а синий компонент равен строке. Итоговый точечный рисунок имеет черный цвет в левом верхнем углу, красный в правом верхнем углу, синий в нижнем левом углу и пурпурный в нижнем правом углу с градиентами в любом месте.

`SetPixel`Метод вызывается 65 536 раз, и, независимо от того, насколько эффективно этот метод, обычно не рекомендуется делать это множество вызовов API, если доступна альтернатива. К счастью, существует несколько альтернативных вариантов.

### <a name="the-pixels-property"></a>Свойство пикселей

`SKBitmap`Определяет [`Pixels`](xref:SkiaSharp.SKBitmap.Pixels) свойство, которое возвращает массив `SKColor` значений для всего растрового изображения. Можно также использовать `Pixels` , чтобы задать массив цветовых значений для точечного рисунка:

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

Пиксели упорядочены в массиве, начиная с первой строки, слева направо, второй строки и т. д. Общее число цветов в массиве равно числу значений ширины и высоты растрового изображения.

Несмотря на то, что это свойство является эффективным, помните, что Пиксели копируются из растрового изображения в массив, а затем из массива обратно в точечный рисунок, а пикселы преобразуются из и в `SKColor` значения.

Ниже приведен метод в `GradientBitmapPage` классе, который задает точечный рисунок с помощью `Pixels` Свойства. Метод выделяет `SKColor` массив требуемого размера, но может использовать `Pixels` свойство для создания этого массива:

```csharp
SKBitmap FillBitmapPixelsProp(out string description, out int milliseconds)
{
    description = "Pixels property";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    SKColor[] pixels = new SKColor[256 * 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                pixels[256 * row + col] = new SKColor((byte)col, 0, (byte)row);
            }

    bitmap.Pixels = pixels;

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Обратите внимание, что индекс `pixels` массива должен быть вычислен из `row` `col` переменных и. Строка умножается на число пикселей в каждой строке (в данном случае 256), а затем добавляется столбец.

`SKBitmap`также определяет аналогичное `Bytes` свойство, которое возвращает массив байтов для всего растрового изображения, но это более громоздки для полных точечных рисунков.

### <a name="the-getpixels-pointer"></a>Указатель в точках

Возможно, самый эффективный способ доступа к растровым пикселам — [`GetPixels`](xref:SkiaSharp.SKBitmap.GetPixels) не путать с `GetPixel` методом или `Pixels` свойством. Вы сразу же заметите разницу с `GetPixels` в том, что она возвращает нечто, не очень распространенное в программировании на C#:

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

Тип .NET [`IntPtr`](xref:System.IntPtr) представляет указатель. Он вызывается `IntPtr` , так как это длина целого числа на машинном процессоре компьютера, на котором выполняется программа, как правило, 32 бит или 64 бит. `IntPtr`Объект, который `GetPixels` возвращает, является адресом фактического блока памяти, который используется объектом Bitmap для хранения пикселов.

Можно преобразовать в `IntPtr` тип указателя C# с помощью [`ToPointer`](xref:System.IntPtr.ToPointer) метода. Синтаксис указателя C# такой же, как и в C и C++:

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

`ptr`Переменная имеет тип Byte- _pointer_. Эта `ptr` переменная позволяет получить доступ к отдельным байтам памяти, используемым для хранения пикселей растрового изображения. Используйте следующий код для считывания байта из этой памяти или записи байта в память:

```csharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

В этом контексте Звездочка является _оператором косвенного обращения_ C# и используется для ссылки на содержимое памяти, на которую указывает `ptr` . Изначально `ptr` указывает на первый байт первой строки точечного рисунка, но можно выполнять арифметические действия над `ptr` переменной, чтобы переместить ее в другие расположения в пределах точечного рисунка.

Недостаток заключается в том, что эту `ptr` переменную можно использовать только в блоке кода, помеченном `unsafe` ключевым словом. Кроме того, сборка должна быть помечена как разрешающая ненадежные блоки. Это делается в свойствах проекта.

Использование указателей в C# является очень мощным, но также очень опасным. Необходимо соблюдать осторожность, чтобы не обращаться к памяти за пределами того, на который должен ссылаться указатель. Именно поэтому использование указателя связано со словом «unsafe».

Ниже приведен метод в `GradientBitmapPage` классе, который использует `GetPixels` метод. Обратите внимание на `unsafe` блок, охватывающий весь код с помощью указателя Byte:

```csharp
SKBitmap FillBitmapBytePtr(out string description, out int milliseconds)
{
    description = "GetPixels byte ptr";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            byte* ptr = (byte*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (byte)(col);   // red
                    *ptr++ = 0;             // green
                    *ptr++ = (byte)(row);   // blue
                    *ptr++ = 0xFF;          // alpha
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

При `ptr` первом получении переменной из `ToPointer` метода она указывает на первый байт крайнего левого пикселя первой строки точечного рисунка. `for`Циклы для `row` и `col` настраиваются таким образом, чтобы их `ptr` можно было увеличивать с помощью `++` оператора после установки каждого байта каждого пикселя. Для других 99 циклов по пикселям `ptr` необходимо установить обратно в начало точечного рисунка.

Каждый пиксель имеет четыре байта памяти, поэтому каждый байт должен быть задан отдельно. В коде предполагается, что байты находятся в порядке красного, зеленого, синего и альфа-канала, который согласуется с `SKColorType.Rgba8888` типом цвета. Вы можете вспомнить, что это тип цвета по умолчанию для iOS и Android, но не для универсальная платформа Windows. По умолчанию UWP создает точечные рисунки с `SKColorType.Bgra8888` типом цвета. По этой причине на этой платформе должны отображаться различные результаты.

Можно привести значение, возвращаемое `ToPointer` , к `uint` указателю, а не к `byte` указателю. Это позволяет получить доступ ко всему пикселю в одной инструкции. Применение `++` оператора к этому указателю увеличивает его на четыре байта, чтобы указать на следующий пиксель:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    SKBitmap FillBitmapUintPtr(out string description, out int milliseconds)
    {
        description = "GetPixels uint ptr";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        IntPtr pixelsAddr = bitmap.GetPixels();

        unsafe
        {
            for (int rep = 0; rep < REPS; rep++)
            {
                uint* ptr = (uint*)pixelsAddr.ToPointer();

                for (int row = 0; row < 256; row++)
                    for (int col = 0; col < 256; col++)
                    {
                        *ptr++ = MakePixel((byte)col, 0, (byte)row, 0xFF);
                    }
            }
        }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    uint MakePixel(byte red, byte green, byte blue, byte alpha) =>
            (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

Пиксел задается с помощью `MakePixel` метода, который строит целочисленный пиксел от красного, зеленого, синего и альфа-компонентов. Помните, что `SKColorType.Rgba8888` Формат имеет примерно пиксельный порядок следования байтов:

RR GG BB AA

Но целое число, соответствующее этим байтам:

ааббггрр

Минимальный значащий байт целого числа сохраняется первым в соответствии с архитектурой с прямым порядком байтов. Этот `MakePixel` метод не будет правильно работать для точечных рисунков с `Bgra8888` типом цвета.

`MakePixel`Метод помечается с помощью [`MethodImplOptions.AggressiveInlining`](xref:System.Runtime.CompilerServices.MethodImplOptions) параметра, чтобы обеспечить компилятору возможность избегать создания отдельного метода, а для компиляции кода, в котором вызывается метод. Это должно повысить производительность.

Интересно, что `SKColor` структура определяет явное преобразование из `SKColor` в целое число без знака, что означает, что `SKColor` можно создать значение и можно использовать преобразование, а `uint` не `MakePixel` :

```csharp
SKBitmap FillBitmapUintPtrColor(out string description, out int milliseconds)
{
    description = "GetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            uint* ptr = (uint*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (uint)new SKColor((byte)col, 0, (byte)row);
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Единственный вопрос: является ли целочисленный формат `SKColor` значения в порядке `SKColorType.Rgba8888` типа цвета, `SKColorType.Bgra8888` типа цвета или что-нибудь еще? Ответ на этот вопрос будет приведен в ближайшее время.

### <a name="the-setpixels-method"></a>Метод Сетпикселс

`SKBitmap`также определяет метод с именем [`SetPixels`](xref:SkiaSharp.SKBitmap.SetPixels(System.IntPtr)) , который вызывается следующим образом:

```csharp
bitmap.SetPixels(intPtr);
```

Помните, что `GetPixels` получает `IntPtr` ссылку на блок памяти, используемый точечным рисунком для хранения пикселов. `SetPixels`Вызов _заменяет_ этот блок памяти блоком памяти, на который ссылается `IntPtr` указанный в качестве `SetPixels` аргумента. Затем точечный рисунок освобождает блок памяти, который использовался ранее. В следующий раз `GetPixels` вызывается блок памяти с параметром `SetPixels` .

На первый взгляд, кажется, что это `SetPixels` дает больше энергии и производительности, чем `GetPixels` менее удобно. `GetPixels`Вы получаете блок памяти Bitmap и доступ к нему. `SetPixels`При выделении и доступе к некоторой памяти, а затем задании этого значения в качестве блока битовой памяти.

Но с помощью `SetPixels` предложения можно использовать разные синтаксические преимущества: Это позволяет получить доступ к битовым точкам растрового изображения с помощью массива. Ниже приведен метод `GradientBitmapPage` , демонстрирующий этот метод. Метод сначала определяет многомерный массив байтов, соответствующий байтам пикселей растрового изображения. Первое измерение — это строка, второе измерение — это столбец, а третье измерение корресондс с четырьмя компонентами каждого пикселя:

```csharp
SKBitmap FillBitmapByteBuffer(out string description, out int milliseconds)
{
    description = "SetPixels byte buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    byte[,,] buffer = new byte[256, 256, 4];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col, 0] = (byte)col;   // red
                buffer[row, col, 1] = 0;           // green
                buffer[row, col, 2] = (byte)row;   // blue
                buffer[row, col, 3] = 0xFF;        // alpha
            }

    unsafe
    {
        fixed (byte* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

Затем, после заполнения массива пикселами, `unsafe` блок и `fixed` оператор используются для получения указателя байтов, указывающего на этот массив. Этот указатель байтов можно затем привести к типу `IntPtr` для передачи в `SetPixels` .

Создаваемый массив не обязательно должен быть массивом байтов. Это может быть целочисленный массив с двумя измерениями для строки и столбца:

```csharp
SKBitmap FillBitmapUintBuffer(out string description, out int milliseconds)
{
    description = "SetPixels uint buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = MakePixel((byte)col, 0, (byte)row, 0xFF);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

`MakePixel`Метод снова используется для объединения цветовых компонентов в 32-разрядный пиксель.

Для полноты вот тот же код, но с `SKColor` приведением значения к целому числу без знака:

```csharp
SKBitmap FillBitmapUintBufferColor(out string description, out int milliseconds)
{
    description = "SetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = (uint)new SKColor((byte)col, 0, (byte)row);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

### <a name="comparing-the-techniques"></a>Сравнение методов

Конструктор страницы **цвета градиента** вызывает все восемь показанных выше методов и сохраняет результаты:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    string[] descriptions = new string[8];
    SKBitmap[] bitmaps = new SKBitmap[8];
    int[] elapsedTimes = new int[8];

    SKCanvasView canvasView;

    public GradientBitmapPage ()
    {
        Title = "Gradient Bitmap";

        bitmaps[0] = FillBitmapSetPixel(out descriptions[0], out elapsedTimes[0]);
        bitmaps[1] = FillBitmapPixelsProp(out descriptions[1], out elapsedTimes[1]);
        bitmaps[2] = FillBitmapBytePtr(out descriptions[2], out elapsedTimes[2]);
        bitmaps[4] = FillBitmapUintPtr(out descriptions[4], out elapsedTimes[4]);
        bitmaps[6] = FillBitmapUintPtrColor(out descriptions[6], out elapsedTimes[6]);
        bitmaps[3] = FillBitmapByteBuffer(out descriptions[3], out elapsedTimes[3]);
        bitmaps[5] = FillBitmapUintBuffer(out descriptions[5], out elapsedTimes[5]);
        bitmaps[7] = FillBitmapUintBufferColor(out descriptions[7], out elapsedTimes[7]);

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

Конструктор завершается созданием `SKCanvasView` для вывода итоговых точечных рисунков. `PaintSurface`Обработчик делит свою поверхность на восемь прямоугольников и вызовы `Display` для отображения каждого из них:

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        int width = info.Width;
        int height = info.Height;

        canvas.Clear();

        Display(canvas, 0, new SKRect(0, 0, width / 2, height / 4));
        Display(canvas, 1, new SKRect(width / 2, 0, width, height / 4));
        Display(canvas, 2, new SKRect(0, height / 4, width / 2, 2 * height / 4));
        Display(canvas, 3, new SKRect(width / 2, height / 4, width, 2 * height / 4));
        Display(canvas, 4, new SKRect(0, 2 * height / 4, width / 2, 3 * height / 4));
        Display(canvas, 5, new SKRect(width / 2, 2 * height / 4, width, 3 * height / 4));
        Display(canvas, 6, new SKRect(0, 3 * height / 4, width / 2, height));
        Display(canvas, 7, new SKRect(width / 2, 3 * height / 4, width, height));
    }

    void Display(SKCanvas canvas, int index, SKRect rect)
    {
        string text = String.Format("{0}: {1:F1} msec", descriptions[index],
                                    (double)elapsedTimes[index] / REPS);

        SKRect bounds = new SKRect();

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.TextSize = (float)(12 * canvasView.CanvasSize.Width / canvasView.Width);
            textPaint.TextAlign = SKTextAlign.Center;
            textPaint.MeasureText("Tly", ref bounds);

            canvas.DrawText(text, new SKPoint(rect.MidX, rect.Bottom - bounds.Bottom), textPaint);
            rect.Bottom -= bounds.Height;
            canvas.DrawBitmap(bitmaps[index], rect, BitmapStretch.Uniform);
        }
    }
}
```

Чтобы разрешить компилятору оптимизировать код, эта страница была запущена в режиме **выпуска** . Ниже приведена страница, работающая в симуляторе iPhone 8 на MacBook Pro, телефоне с Android 5 и Surface Pro 3 под управлением Windows 10. Из-за различий в оборудовании не следует сравнивать время снижения производительности между устройствами, а просматривать относительные моменты времени на каждом устройстве:

[![Растровое изображение градиента](pixel-bits-images/GradientBitmap.png "Растровое изображение градиента")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

Ниже приведена таблица, которая объединяет время выполнения в миллисекундах:

| API       | Тип данных | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel  |           | 3,17 |   10,77 | 3.49 |
| Перекрытых    |           | 0,32 |    1,23 | 0,07 |
| В точках | byte      | 0,09 |    0.24 | 0,10 |
|           | uint      | 0,06 |    0,26 | 0,05 |
|           | скколор   | 0,29 |    0.99 | 0,07 |
| сетпикселс | byte      | 1.33 |    6,78 | 0,11 |
|           | uint      | 0.14 |    0,69 | 0,06 |
|           | скколор   | 0,35 |    1,93 | 0,10 |

Как и ожидалось, вызов `SetPixel` 65 536 раз является наименьшим еффеиЦиент способом установки пикселей точечного рисунка. Заполнение `SKColor` массива и задание `Pixels` свойства гораздо лучше, и даже сравнение с некоторыми `GetPixels` `SetPixels` методами и. Работа с `uint` значениями пикселей обычно выполняется быстрее, чем установка отдельных `byte` компонентов, а преобразование `SKColor` значения в целое число без знака увеличивает нагрузку на процесс.

Также интересно сравнить различные градиенты: верхние строки каждой платформы одинаковы и отображают градиент в том виде, в котором он предназначался. Это означает, что `SetPixel` метод и `Pixels` свойство правильно создают Пиксели из цветов независимо от базового формата пикселей.

Следующие две строки снимков экрана iOS и Android также одинаковы, что подтверждает, что маленький `MakePixel` метод правильно определен для формата пикселей по умолчанию `Rgba8888` для этих платформ.

Нижняя строка снимков экрана iOS и Android находится в обратном направлении, что означает, что целое число без знака, полученное путем приведения `SKColor` значения, имеет вид:

AARRGGBB

Байты находятся в указанном порядке:

BB GG RR AA

Это `Bgra8888` упорядочение, а не `Rgba8888` порядок. Этот `Brga8888` формат используется по умолчанию для универсальной платформы Windows, поэтому градиенты в последней строке этого снимка экрана совпадают с первой строкой. Но средние две строки неверны, так как код, создающий эти точечные рисунки, предполагает `Rgba8888` упорядочение.

Если вы хотите использовать один и тот же код для доступа к битовым битам на каждой платформе, можно явно создать объект `SKBitmap` с использованием либо формата, либо `Rgba8888` `Bgra8888` . Если необходимо привести `SKColor` значения к растровым пикселам, используйте `Bgra8888` .

## <a name="random-access-of-pixels"></a>Произвольный доступ пикселей

`FillBitmapBytePtr`Методы и `FillBitmapUintPtr` на странице **битовой карты градиента** имеют преимущество от `for` циклов, предназначенных для заполнения точечного рисунка последовательно, от верхней строки до нижней строки и в каждой строке слева направо. Пиксел можно задать с помощью той же инструкции, которая увеличивает указатель.

Иногда требуется обращаться к пикселам случайным образом, а не последовательно. Если вы используете этот `GetPixels` подход, необходимо вычислить указатели на основе строки и столбца. Это показано на странице **синуса** , которая создает растровое изображение, показывающее Радуга в виде одного цикла синуса.

Цвета Радуга проще всего создавать с помощью цветовой модели HSL (цветовой тон, насыщенность, яркость). `SKColor.FromHsl`Метод создает `SKColor` значение, используя значения оттенка в диапазоне от 0 до 360 (например, углы круга, но от красного и синего до зеленого), а также значения насыщенности и яркости в диапазоне от 0 до 100. Для цветов «Радуга» насыщенность должна быть равна 100, а «яркость» — средней точке 50.

**Радуга синуса** создает это изображение, перепроходя по строкам точечного рисунка, а затем прокручивать значения оттенков 360. Из каждого значения оттенка вычисляется столбец битовой карты, который также основывается на значении синуса:

```csharp
public class RainbowSinePage : ContentPage
{
    SKBitmap bitmap;

    public RainbowSinePage()
    {
        Title = "Rainbow Sine";

        bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);

        unsafe
        {
            // Pointer to first pixel of bitmap
            uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();

            // Loop through the rows
            for (int row = 0; row < bitmap.Height; row++)
            {
                // Calculate the sine curve angle and the sine value
                double angle = 2 * Math.PI * row / bitmap.Height;
                double sine = Math.Sin(angle);

                // Loop through the hues
                for (int hue = 0; hue < 360; hue++)
                {
                    // Calculate the column
                    int col = (int)(360 + 360 * sine + hue);

                    // Calculate the address
                    uint* ptr = basePtr + bitmap.Width * row + col;

                    // Store the color value
                    *ptr = (uint)SKColor.FromHsl(hue, 100, 50);
                }
            }
        }

        // Create the SKCanvasView
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
        canvas.DrawBitmap(bitmap, info.Rect);
    }
}
```

Обратите внимание, что конструктор создает точечный рисунок на основе `SKColorType.Bgra8888` формата:

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

Это позволяет программе использовать преобразование `SKColor` значений в `uint` Пиксели, не беспокоясь. Хотя она не воспроизводит роль в этой конкретной программе, при использовании `SKColor` преобразования для установки пикселов следует также указать, `SKAlphaType.Unpremul` так как не выполняет преобразование `SKColor` компонентов цвета по альфа-значению.

Затем конструктор использует `GetPixels` метод для получения указателя на первый пиксель точечного рисунка:

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

Для каждой конкретной строки и столбца значение смещения должно быть добавлено к `basePtr` . Это смещение представляет собой строку, в которую входит ширина растрового изображения и столбец:

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

`SKColor`Значение хранится в памяти с помощью этого указателя:

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

В `PaintSurface` обработчике `SKCanvasView` растровое изображение растягивается для заполнения отображаемой области:

[![Синус Радуга](pixel-bits-images/RainbowSine.png "Синус Радуга")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>Из одного точечного рисунка в другой

Очень многие задачи обработки изображений подразумевают изменение пикселов при их передаче из одного растрового изображения в другой. Этот метод показан на странице **настройки цвета** . Страница загружает один из ресурсов точечного рисунка, а затем позволяет изменить образ, используя три `Slider` представления:

[![Настройка цвета](pixel-bits-images/ColorAdjustment.png "Настройка цвета")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

Для каждого цвета пикселя первый `Slider` добавляет значение от 0 до 360 к оттенок, а затем использует оператор остатка от деления для сохранения результата между 0 и 360, фактически сдвигя цвета вдоль спектра (как показано на снимке экрана UWP). Второй `Slider` позволяет выбрать мультипликативные фактор между 0,5 и 2, чтобы применить к насыщенности, а третий — `Slider` то же самое для яркости, как показано на снимке экрана Android.

Программа поддерживает два точечных рисунка, исходный точечный рисунок с именем `srcBitmap` и скорректированное растровое изображение назначения с именем `dstBitmap` . При каждом `Slider` перемещении программа вычисляет все новые Пиксели в `dstBitmap` . Конечно, пользователи будут поэкспериментировать, быстро перемещая `Slider` представления, чтобы обеспечить максимальную производительность, которую вы можете управлять. Это подразумевает `GetPixels` метод как для исходного, так и для целевого растрового изображения.

Страница « **Коррекция цвета** » не управляет форматом цвета исходного и конечного точечных рисунков. Вместо этого он содержит немного другую логику `SKColorType.Rgba8888` для `SKColorType.Bgra8888` форматов и. Источник и назначение могут быть разными форматами, и программа по-прежнему будет работать.

Ниже приведена программа, за исключением ключевого `TransferPixels` метода, который передает Пиксели источник в место назначения. Конструктор задает значение `dstBitmap` равным `srcBitmap` . `PaintSurface`Обработчик отображает `dstBitmap` :

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    SKBitmap srcBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKBitmap dstBitmap;

    public ColorAdjustmentPage()
    {
        InitializeComponent();

        dstBitmap = new SKBitmap(srcBitmap.Width, srcBitmap.Height);
        OnSliderValueChanged(null, null);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        float hueAdjust = (float)hueSlider.Value;
        hueLabel.Text = $"Hue Adjustment: {hueAdjust:F0}";

        float saturationAdjust = (float)Math.Pow(2, saturationSlider.Value);
        saturationLabel.Text = $"Saturation Adjustment: {saturationAdjust:F2}";

        float luminosityAdjust = (float)Math.Pow(2, luminositySlider.Value);
        luminosityLabel.Text = $"Luminosity Adjustment: {luminosityAdjust:F2}";

        TransferPixels(hueAdjust, saturationAdjust, luminosityAdjust);
        canvasView.InvalidateSurface();
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(dstBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

`ValueChanged`Обработчик для `Slider` представлений вычисляет значения и вызовы корректировки `TransferPixels` .

Весь `TransferPixels` метод помечается как `unsafe` . Сначала он получает указатели байтов на битовые точки обоих растровых изображений, а затем выполняет цикл по всем строкам и столбцам. Из исходного растрового изображения метод получает четыре байта для каждого пикселя. Они могут находиться в `Rgba8888` `Bgra8888` порядке или. Проверка типа цвета позволяет `SKColor` создать значение. Затем компоненты HSL извлекаются, корректируются и используются для повторного создания `SKColor` значения. В зависимости от того, имеет ли точечный рисунок назначения значение `Rgba8888` или `Bgra8888` , байты хранятся в целевом битмп:

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    ···
    unsafe void TransferPixels(float hueAdjust, float saturationAdjust, float luminosityAdjust)
    {
        byte* srcPtr = (byte*)srcBitmap.GetPixels().ToPointer();
        byte* dstPtr = (byte*)dstBitmap.GetPixels().ToPointer();

        int width = srcBitmap.Width;       // same for both bitmaps
        int height = srcBitmap.Height;

        SKColorType typeOrg = srcBitmap.ColorType;
        SKColorType typeAdj = dstBitmap.ColorType;

        for (int row = 0; row < height; row++)
        {
            for (int col = 0; col < width; col++)
            {
                // Get color from original bitmap
                byte byte1 = *srcPtr++;         // red or blue
                byte byte2 = *srcPtr++;         // green
                byte byte3 = *srcPtr++;         // blue or red
                byte byte4 = *srcPtr++;         // alpha

                SKColor color = new SKColor();

                if (typeOrg == SKColorType.Rgba8888)
                {
                    color = new SKColor(byte1, byte2, byte3, byte4);
                }
                else if (typeOrg == SKColorType.Bgra8888)
                {
                    color = new SKColor(byte3, byte2, byte1, byte4);
                }

                // Get HSL components
                color.ToHsl(out float hue, out float saturation, out float luminosity);

                // Adjust HSL components based on adjustments
                hue = (hue + hueAdjust) % 360;
                saturation = Math.Max(0, Math.Min(100, saturationAdjust * saturation));
                luminosity = Math.Max(0, Math.Min(100, luminosityAdjust * luminosity));

                // Recreate color from HSL components
                color = SKColor.FromHsl(hue, saturation, luminosity);

                // Store the bytes in the adjusted bitmap
                if (typeAdj == SKColorType.Rgba8888)
                {
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Alpha;
                }
                else if (typeAdj == SKColorType.Bgra8888)
                {
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Alpha;
                }
            }
        }
    }
    ···
}
```

Вполне вероятно, что производительность этого метода может быть улучшена еще больше за счет создания отдельных методов для различных сочетаний цветов исходного и конечного точечных рисунков и предотвращения проверки типа для каждого пикселя. Другим вариантом является наличие нескольких `for` циклов для `col` переменной на основе типа цвета.

## <a name="posterization"></a>Афиша

Другим распространенным заданием, включающим доступ к битам пикселей, является _Афиша_. Число, если цвета, закодированные в пикселах растрового изображения, уменьшаются таким образом, что результат напоминает руки, нарисованной вручную, с использованием ограниченной цветовой палитры.

На странице **постеризе** этот процесс выполняется на одном из образов обезьян:

```csharp
public class PosterizePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public PosterizePage()
    {
        Title = "Posterize";

        unsafe
        {
            uint* ptr = (uint*)bitmap.GetPixels().ToPointer();
            int pixelCount = bitmap.Width * bitmap.Height;

            for (int i = 0; i < pixelCount; i++)
            {
                *ptr++ &= 0xE0E0E0FF;
            }
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
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform;
    }
}
```

Код в конструкторе обращается к каждому пикселю, выполняет побитовую операцию и со значением 0xE0E0E0FF, а затем сохраняет результат обратно в точечный рисунок. Значения 0xE0E0E0FF сохраняются старшие 3 бита каждого компонента цвета и устанавливают младшие 5 бит в значение 0. Вместо 2<sup>24</sup> или 16 777 216 цветов битовая карта сокращается до 2<sup>9</sup> или 512 цветов:

[![Постеризация](pixel-bits-images/Posterize.png "Постеризация")](pixel-bits-images/Posterize-Large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
