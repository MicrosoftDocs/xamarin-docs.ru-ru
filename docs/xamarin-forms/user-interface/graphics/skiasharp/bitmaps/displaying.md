---
Title: "Отображение точечных рисунков SkiaSharp" Description: "сведения о том, как отображать точечные рисунки SkiaSharp в размерах пикселей и разворачиваться для заполнения прямоугольников с сохранением пропорций".
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: 8E074F8D-4715-4146-8CC0-FD7A8290EDE9 Автор: давидбритч MS. author: дабритч MS. Дата: 07/17/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="displaying-skiasharp-bitmaps"></a>Отображение точечных рисунков SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Тема точечных рисунков SkiaSharp была представлена в статье **[основы битовой карты в SkiaSharp](../basics/bitmaps.md)**. В этой статье показано три способа загрузки точечных рисунков и три способа отобразить точечные рисунки. В этой статье рассматриваются методы загрузки растровых изображений и более подробное использование `DrawBitmap` методов `SKCanvas` .

![Отображение образца](displaying-images/DisplayingSample.png "Отображение образца")

`DrawBitmapLattice`Методы и `DrawBitmapNinePatch` обсуждаются в статье **[Сегментированное отображение точечных рисунков SkiaSharp](segmented.md)**.

Примеры на этой странице относятся к приложению **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** . На домашней странице этого приложения выберите **точечные рисунки SkiaSharp**, а затем перейдите к разделу **отображение растровых изображений** .

## <a name="loading-a-bitmap"></a>Загрузка точечного рисунка

Точечный рисунок, используемый приложением SkiaSharp, обычно поступает из одного из трех различных источников:

- Из Интернета
- Из ресурса, внедренного в исполняемый файл
- Из библиотеки фотографий пользователя

Кроме того, приложение SkiaSharp может создать новое растровое изображение, а затем нарисовать на нем или установить битовую карту алгоритмически. Эти методы обсуждаются в статьях **[Создание и рисование растровых изображений SkiaSharp](drawing.md)** и **[доступ к SkiaSharp растровых пикселов](pixel-bits.md)**.

В следующих трех примерах кода для загрузки точечного рисунка предполагается, что класс содержит поле типа `SKBitmap` :

```csharp
SKBitmap bitmap;
```

Как и в **[SkiaSharp](../basics/bitmaps.md)** , для загрузки точечного рисунка в Интернете лучше всего использовать [`HttpClient`](xref:System.Net.Http.HttpClient) класс. Один экземпляр класса может быть определен как поле:

```csharp
HttpClient httpClient = new HttpClient();
```

При использовании `HttpClient` с приложениями iOS и Android необходимо задать свойства проекта, как описано в документах по **[протоколу TLS 1,2](~/cross-platform/app-fundamentals/transport-layer-security.md)**.

Код, который использует `HttpClient` часто, включает `await` оператор, поэтому он должен находиться в `async` методе:

```csharp
try
{
    using (Stream stream = await httpClient.GetStreamAsync("https:// ··· "))
    using (MemoryStream memStream = new MemoryStream())
    {
        await stream.CopyToAsync(memStream);
        memStream.Seek(0, SeekOrigin.Begin);

        bitmap = SKBitmap.Decode(stream);
        ···
    };
}
catch
{
    ···
}
```

Обратите внимание, что `Stream` объект, полученный из `GetStreamAsync` , копируется в `MemoryStream` . Android не разрешает `Stream` обработку из в `HttpClient` основном потоке, за исключением асинхронных методов. 

[`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream))Выполняет массу работы: `Stream` объект, переданный в него, ссылается на блок памяти, содержащий все точечные рисунки в одном из стандартных форматов файлов точечных рисунков, обычно JPEG, PNG или GIF. `Decode`Метод должен определить формат, а затем декодировать файл растрового изображения в собственный внутренний точечный формат SkiaSharp.

После вызова кода `SKBitmap.Decode` он, вероятно, сделает недействительным значение, `CanvasView` чтобы `PaintSurface` обработчик мог отобразить только что загруженный точечный рисунок.

Второй способ загрузки точечного рисунка заключается в том, что он включает точечный рисунок как внедренный ресурс в библиотеку .NET Standard, на которую ссылаются проекты отдельных платформ. Идентификатор ресурса передается в [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) метод. Этот идентификатор ресурса состоит из имени сборки, имени папки и имени файла ресурса, разделенного точками:

```csharp
string resourceID = "assemblyName.folderName.fileName";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    bitmap = SKBitmap.Decode(stream);
    ···
}
```

Файлы точечных рисунков также могут храниться в виде ресурсов в проекте отдельной платформы для iOS, Android и универсальная платформа Windows (UWP). Однако для загрузки этих точечных рисунков требуется код, расположенный в проекте платформы.

Третьим подходом к получению точечного рисунка является библиотека изображений пользователя. В следующем коде используется служба зависимостей, включенная в приложение **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** . Библиотека .NET Standard **скиашарпформсдемо** включает `IPhotoLibrary` интерфейс, тогда как каждый проект платформы содержит `PhotoLibrary` класс, реализующий этот интерфейс.

```csharp
IPhotoicturePicker picturePicker = DependencyService.Get<IPhotoLibrary>();

using (Stream stream = await picturePicker.GetImageStreamAsync())
{
    if (stream != null)
    {
        bitmap = SKBitmap.Decode(stream);
        ···
    }
}
```

Как правило, такой код также делает недействительным, `CanvasView` чтобы `PaintSurface` обработчик мог отобразить новое растровое изображение.

`SKBitmap`Класс определяет несколько полезных свойств, включая [`Width`](xref:SkiaSharp.SKBitmap.Width) и, которые выводят [`Height`](xref:SkiaSharp.SKBitmap.Height) размеры точечного рисунка в пикселях, а также многие методы, включая методы для создания точечных рисунков, их копирования и для представления битов пикселов. 

## <a name="displaying-in-pixel-dimensions"></a>Отображение в измерениях в пикселях

Класс SkiaSharp [`Canvas`](xref:SkiaSharp.SKCanvas) определяет четыре `DrawBitmap` метода. Эти методы позволяют отображать точечные рисунки двумя принципиально разными способами: 

- При указании `SKPoint` значения (или `x` отдельных `y` значений) отображается точечный рисунок в его размерах в пикселях. Пикселы точечного рисунка сопоставлены непосредственно с пикселями экрана видео.
- Задание прямоугольника приводит к растяжению точечного рисунка до размера и формы прямоугольника. 

Точечный рисунок отображается в его размерах в пикселях с помощью [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKPoint,SkiaSharp.SKPaint)) `SKPoint` параметра с параметром или [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) с отдельными `x` `y` параметрами и.

```csharp
DrawBitmap(SKBitmap bitmap, SKPoint pt, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, float x, float y, SKPaint paint = null)
```

Эти два метода функционально идентичны. Указанная точка указывает расположение верхнего левого угла точечного рисунка относительно холста. Так как разрешение в пикселах мобильных устройств настолько велико, небольшие точечные рисунки обычно отображаются на этих устройствах очень маленькими.

Необязательный `SKPaint` параметр позволяет отображать растровое изображение с помощью прозрачности. Для этого создайте `SKPaint` объект и задайте `Color` для свойства любое `SKColor` значение с альфа-каналом меньше 1. Пример.

```csharp
paint.Color = new SKColor(0, 0, 0, 0x80);
```

Значение 0x80, переданное в качестве последнего аргумента, указывает на прозрачность 50%. Альфа-канал также можно установить на одном из предварительно определенных цветов:

```csharp
paint.Color = SKColors.Red.WithAlpha(0x80);
```

Однако сам цвет не важен. Только альфа-канал проверяется при использовании `SKPaint` объекта в `DrawBitmap` вызове.

`SKPaint`Объект также играет роль при отображении растровых изображений с помощью режимов смешения или фильтров. Они показаны в статьях [SkiaSharp компоновки и режимы смешения](../effects/blend-modes/index.md) и [Фильтры изображений SkiaSharp](../effects/image-filters.md).

На странице **измерения в пикселах** образца программы **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** отображается ресурс точечного рисунка размером 320 пикселей в ширину на 240 пикселей в высоком уровне:

```csharp
public class PixelDimensionsPage : ContentPage
{
    SKBitmap bitmap;

    public PixelDimensionsPage()
    {
        Title = "Pixel Dimensions";

        // Load the bitmap from a resource
        string resourceID = "SkiaSharpFormsDemos.Media.Banana.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        // Create the SKCanvasView and set the PaintSurface handler
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

        float x = (info.Width - bitmap.Width) / 2;
        float y = (info.Height - bitmap.Height) / 2;

        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

`PaintSurface`Обработчик выравнивает точечный рисунок по центру, вычисляя `x` `y` значения и на основе размеров пикселов отображаемой поверхности и размеров точечного рисунка:

[![Размеры в пикселях](displaying-images/PixelDimensions.png "Размеры в пикселях")](displaying-images/PixelDimensions-Large.png#lightbox)

Если приложение хочет отобразить точечный рисунок в левом верхнем углу, он просто передаст координаты (0, 0). 

## <a name="a-method-for-loading-resource-bitmaps"></a>Метод загрузки точечных рисунков ресурсов

Многие из примеров, которые потребуются для загрузки ресурсов точечного рисунка. Статический `BitmapExtensions` класс в решении **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** содержит метод, помогающий:

```csharp
static class BitmapExtensions
{
    public static SKBitmap LoadBitmapResource(Type type, string resourceID)
    {
        Assembly assembly = type.GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            return SKBitmap.Decode(stream);
        }
    }
    ···
}
```

Обратите внимание на `Type` параметр. Это может быть `Type` объект, связанный с любым типом в сборке, в которой хранится ресурс Bitmap.

Этот `LoadBitmapResource` метод будет использоваться во всех последующих примерах, требующих ресурсов точечного рисунка.

## <a name="stretching-to-fill-a-rectangle"></a>Растяжение для заполнения прямоугольника

`SKCanvas`Класс также определяет [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) метод, который визуализирует растровое изображение в прямоугольник и другой [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) метод, который визуализирует прямоугольное подмножество точечного рисунка прямоугольнику:

```
DrawBitmap(SKBitmap bitmap, SKRect dest, SKPaint paint = null)

DrawBitmap(SKBitmap bitmap, SKRect source, SKRect dest, SKPaint paint = null)
```

В обоих случаях точечный рисунок растягивается для заполнения прямоугольника с именем `dest` . Во втором методе `source` прямоугольник позволяет выбрать подмножество точечного рисунка. `dest`Прямоугольник задается относительно выходного устройства; `source` прямоугольник задается относительно точечного рисунка.

На странице « **прямоугольник заливки** » показано первое из этих двух методов, отображая тот же рисунок, который использовался в предыдущем примере в прямоугольнике того же размера, что и холст: 

```csharp
public class FillRectanglePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public FillRectanglePage ()
    {
        Title = "Fill Rectangle";

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

Обратите внимание на использование нового `BitmapExtensions.LoadBitmapResource` метода для задания `SKBitmap` поля. Прямоугольник назначения получается из [`Rect`](xref:SkiaSharp.SKImageInfo.Rect) свойства `SKImageInfo` , которое описывает размер отображаемой поверхности:

[![Заливка прямоугольника](displaying-images/FillRectangle.png "Заливка прямоугольника")](displaying-images/FillRectangle-Large.png#lightbox)

Обычно это _не_ то, что нужно. Изображение искажается путем растягивания по горизонтали и по вертикали. При отображении точечного рисунка, отличного от размера пикселя, обычно требуется сохранить исходные пропорции растрового изображения.

## <a name="stretching-while-preserving-the-aspect-ratio"></a>Растяжение с сохранением пропорций

Растяжение растрового изображения с сохранением пропорций является процессом, также известным как _однородное масштабирование_. Этот термин предполагает алгоритмный подход. Одно из возможных решений показано на странице **Равномерное масштабирование** :

```csharp
public class UniformScalingPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(UniformScalingPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public UniformScalingPage()
    {
        Title = "Uniform Scaling";

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

        float scale = Math.Min((float)info.Width / bitmap.Width, 
                               (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect destRect = new SKRect(x, y, x + scale * bitmap.Width, 
                                           y + scale * bitmap.Height);

        canvas.DrawBitmap(bitmap, destRect);
    }
}
```

`PaintSurface`Обработчик вычисляет `scale` коэффициент, который является минимальным отношением ширины и высоты отображаемого значения к ширине и высоте растрового изображения. `x` `y` Затем значения и можно вычислить для центрирования масштабированного растрового изображения в пределах отображаемой ширины и высоты. Прямоугольник назначения содержит левый верхний угол `x` и `y` и правый нижний угол этих значений, а также масштабированную ширину и высоту точечного рисунка.

[![Унифицированное масштабирование](displaying-images/UniformScaling.png "Унифицированное масштабирование")](displaying-images/UniformScaling-Large.png#lightbox)

Превратите телефон в сторону, чтобы увидеть точечный рисунок, растянутый в этой области:

[![Ландшафт равномерного масштабирования](displaying-images/UniformScaling-Landscape.png "Ландшафт равномерного масштабирования")](displaying-images/UniformScaling-Landscape-Large.png#lightbox)

Преимущества использования этого `scale` фактора становятся очевидными, если требуется реализовать немного другой алгоритм. Предположим, что необходимо сохранить пропорции растрового изображения, но также заполнить прямоугольник назначения. Единственное, что можно сделать, — обрезать часть изображения, но можно реализовать этот алгоритм просто, изменив `Math.Min` на `Math.Max` в приведенном выше коде. Ниже приведен результат. 

[![Альтернатива унифицированного масштабирования](displaying-images/UniformScaling-Alternative.png "Альтернатива унифицированного масштабирования")](displaying-images/UniformScaling-Alternative-Large.png#lightbox)

Пропорции растрового изображения сохраняются, но области слева и справа от растрового изображения обрезаются.

## <a name="a-versatile-bitmap-display-function"></a>Универсальная функция вывода растрового изображения

В средах программирования на основе XAML (таких как UWP и Xamarin.Forms ) предусмотрена возможность увеличения или уменьшения размера точечных рисунков с сохранением пропорций. Хотя SkiaSharp не включает эту функцию, ее можно реализовать самостоятельно. `BitmapExtensions`Класс, входящий в приложение [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) , показывает, как это делать. Класс определяет два новых `DrawBitmap` метода, которые выполняют вычисление пропорций. Эти новые методы являются методами расширения `SKCanvas` .

Новые `DrawBitmap` методы включают параметр типа `BitmapStretch` , перечисление, определенное в файле **BitmapExtensions.CS** :

```csharp
public enum BitmapStretch
{
    None,
    Fill,
    Uniform,
    UniformToFill,
    AspectFit = Uniform,
    AspectFill = UniformToFill
}
```

`None`Элементы, `Fill` , `Uniform` и совпадают с `UniformToFill` элементами [`Stretch`](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) перечисления UWP. Аналогичное Xamarin.Forms [`Aspect`](xref:Xamarin.Forms.Aspect) перечисление определяет члены `Fill` , `AspectFit` и `AspectFill` .

Показанная выше страница с **равномерным масштабированием** выравнивает точечный рисунок внутри прямоугольника, но могут потребоваться другие параметры, например размещение растрового изображения в левой или правой части прямоугольника, сверху или снизу. Это назначение `BitmapAlignment` перечисления:

```csharp
public enum BitmapAlignment
{
    Start,
    Center,
    End
}
```

Параметры выравнивания не действуют при использовании с `BitmapStretch.Fill` .

Первая `DrawBitmap` функция расширения содержит прямоугольник назначения, но не имеет исходного прямоугольника. Значения по умолчанию определяются таким образом, что если нужно, чтобы точечный рисунок был выровнен по центру, необходимо указать только `BitmapStretch` элемент.

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect dest, 
                                  BitmapStretch stretch, 
                                  BitmapAlignment horizontal = BitmapAlignment.Center, 
                                  BitmapAlignment vertical = BitmapAlignment.Center, 
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / bitmap.Width, dest.Height / bitmap.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * bitmap.Width, scale * bitmap.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, display, paint);
        }
    }
    ···
}
```

Основной целью этого метода является вычисление коэффициента масштабирования с именем `scale` , который затем применяется к ширине и высоте растрового изображения при вызове `CalculateDisplayRect` метода. Это метод, который вычисляет прямоугольник для отображения точечного рисунка на основе горизонтального и вертикального выравнивания:

```csharp
static class BitmapExtensions
{
    ···
    static SKRect CalculateDisplayRect(SKRect dest, float bmpWidth, float bmpHeight, 
                                       BitmapAlignment horizontal, BitmapAlignment vertical)
    {
        float x = 0;
        float y = 0;

        switch (horizontal)
        {
            case BitmapAlignment.Center:
                x = (dest.Width - bmpWidth) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                x = dest.Width - bmpWidth;
                break;
        }

        switch (vertical)
        {
            case BitmapAlignment.Center:
                y = (dest.Height - bmpHeight) / 2;
                break;

            case BitmapAlignment.Start:
                break;

            case BitmapAlignment.End:
                y = dest.Height - bmpHeight;
                break;
        }

        x += dest.Left;
        y += dest.Top;

        return new SKRect(x, y, x + bmpWidth, y + bmpHeight);
    }
}
```

`BitmapExtensions`Класс содержит дополнительный `DrawBitmap` метод с исходным прямоугольником для указания подмножества точечного рисунка. Этот метод аналогичен первому за исключением того, что коэффициент масштабирования вычисляется на основе `source` прямоугольника, а затем применяется к `source` прямоугольнику в вызове `CalculateDisplayRect` :

```csharp
static class BitmapExtensions
{
    ···
    public static void DrawBitmap(this SKCanvas canvas, SKBitmap bitmap, SKRect source, SKRect dest,
                                  BitmapStretch stretch,
                                  BitmapAlignment horizontal = BitmapAlignment.Center,
                                  BitmapAlignment vertical = BitmapAlignment.Center,
                                  SKPaint paint = null)
    {
        if (stretch == BitmapStretch.Fill)
        {
            canvas.DrawBitmap(bitmap, source, dest, paint);
        }
        else
        {
            float scale = 1;

            switch (stretch)
            {
                case BitmapStretch.None:
                    break;

                case BitmapStretch.Uniform:
                    scale = Math.Min(dest.Width / source.Width, dest.Height / source.Height);
                    break;

                case BitmapStretch.UniformToFill:
                    scale = Math.Max(dest.Width / source.Width, dest.Height / source.Height);
                    break;
            }

            SKRect display = CalculateDisplayRect(dest, scale * source.Width, scale * source.Height, 
                                                  horizontal, vertical);

            canvas.DrawBitmap(bitmap, source, display, paint);
        }
    }
    ···
}
```

Первый из этих двух новых `DrawBitmap` методов показан на странице **режимы масштабирования** . Файл XAML содержит три `Picker` элемента, которые позволяют выбрать члены `BitmapStretch` `BitmapAlignment` перечислений и.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.ScalingModesPage"
             Title="Scaling Modes">

    <Grid Padding="10">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <skia:SKCanvasView x:Name="canvasView" 
                           Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="Stretch:"
               Grid.Row="1" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="stretchPicker"
                Grid.Row="1" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapStretch}">
                    <x:Static Member="local:BitmapStretch.None" />
                    <x:Static Member="local:BitmapStretch.Fill" />
                    <x:Static Member="local:BitmapStretch.Uniform" />
                    <x:Static Member="local:BitmapStretch.UniformToFill" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Horizontal Alignment:"
               Grid.Row="2" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="horizontalPicker" 
                Grid.Row="2" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Label Text="Vertical Alignment:"
               Grid.Row="3" Grid.Column="0"
               VerticalOptions="Center" />

        <Picker x:Name="verticalPicker" 
                Grid.Row="3" Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:BitmapAlignment}">
                    <x:Static Member="local:BitmapAlignment.Start" />
                    <x:Static Member="local:BitmapAlignment.Center" />
                    <x:Static Member="local:BitmapAlignment.End" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</ContentPage>
```

Файл кода программной части просто делает недействительным значение `CanvasView` при изменении какого-либо `Picker` элемента. `PaintSurface`Обработчик обращается к трем `Picker` представлениям для вызова `DrawBitmap` метода расширения:

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public ScalingModesPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, dest, stretch, horizontal, vertical);
    }
}
```

Ниже приведены некоторые сочетания параметров.

[![Режимы масштабирования](displaying-images/ScalingModes.png "Режимы масштабирования")](displaying-images/ScalingModes-Large.png#lightbox)

Страница **подмножества прямоугольников** имеет практически тот же XAML-файл, что и **режимы масштабирования**, но файл кода программной части определяет прямоугольное подмножество точечного рисунка, заданное `SOURCE` полем: 

```csharp
public partial class ScalingModesPage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(ScalingModesPage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");

    static readonly SKRect SOURCE = new SKRect(94, 12, 212, 118);

    public RectangleSubsetPage()
    {
        InitializeComponent();
    }

    private void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect dest = new SKRect(0, 0, info.Width, info.Height);

        BitmapStretch stretch = (BitmapStretch)stretchPicker.SelectedItem;
        BitmapAlignment horizontal = (BitmapAlignment)horizontalPicker.SelectedItem;
        BitmapAlignment vertical = (BitmapAlignment)verticalPicker.SelectedItem;

        canvas.DrawBitmap(bitmap, SOURCE, dest, stretch, horizontal, vertical);
    }
}
```

Этот исходный прямоугольник изолирует головную обезьяну, как показано на снимках экрана:

[![Подмножество прямоугольников](displaying-images/RectangleSubset.png "Подмножество прямоугольников")](displaying-images/RectangleSubset-Large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
