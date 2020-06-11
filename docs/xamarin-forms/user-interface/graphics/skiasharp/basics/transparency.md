---
Title: "SkiaSharp прозрачность" Description: "использование прозрачности для объединения нескольких объектов в одной сцене".
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: B62F9487-C30E-4C63-BAB1-4C091FF50378 Автор: давидбритч MS. author: дабритч MS. Дата: 08/23/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="skiasharp-transparency"></a>Прозрачность SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Как вы видели, [`SKPaint`](xref:SkiaSharp.SKPaint) класс включает [`Color`](xref:SkiaSharp.SKPaint.Color) свойство типа [`SKColor`](xref:SkiaSharp.SKColor) . `SKColor`включает альфа-канал, поэтому все цвета со `SKColor` значением могут быть частично прозрачными. 

Часть прозрачности была продемонстрирована в статье [**основная анимация в SkiaSharp**](animation.md) . В этой статье более подробно рассматривается прозрачность для объединения нескольких объектов в одной сцене, способ, который иногда называется _наложением_. Более сложные методики наложения обсуждаются в статьях в разделе [**SkiaSharp шейдеры**](../effects/shaders/index.md) .

Уровень прозрачности можно задать при первом создании цвета с помощью конструктора с четырьмя параметрами [`SKColor`](xref:SkiaSharp.SKColor.%23ctor(System.Byte,System.Byte,System.Byte,System.Byte)) :

```csharp
SKColor (byte red, byte green, byte blue, byte alpha);
```

Альфа-значение 0 является полностью прозрачным, а альфа-значение 0xFF — полностью непрозрачным. Значения между этими двумя крайними значениями создают частично прозрачные цвета.

Кроме того, `SKColor` определяет удобный [`WithAlpha`](xref:SkiaSharp.SKColor.WithAlpha*) метод, создающий новый цвет из существующего цвета, но с указанным уровнем альфа:

```csharp
SKColor halfTransparentBlue = SKColors.Blue.WithAlpha(0x80);
```

Использование частично прозрачного текста демонстрируется в кодовой странице **код больше** в примере [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) . На этой странице два текстовых строки постепенно перетекает, включая прозрачность `SKColor` значений:

```csharp
public class CodeMoreCodePage : ContentPage
{
    SKCanvasView canvasView;
    bool isAnimating;
    Stopwatch stopwatch = new Stopwatch();
    double transparency;

    public CodeMoreCodePage ()
    {
        Title = "Code More Code";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

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
        const int duration = 5;     // seconds
        double progress = stopwatch.Elapsed.TotalSeconds % duration / duration;
        transparency = 0.5 * (1 + Math.Sin(progress * 2 * Math.PI));
        canvasView.InvalidateSurface();

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        const string TEXT1 = "CODE";
        const string TEXT2 = "MORE";

        using (SKPaint paint = new SKPaint())
        {
            // Set text width to fit in width of canvas
            paint.TextSize = 100;
            float textWidth = paint.MeasureText(TEXT1);
            paint.TextSize *= 0.9f * info.Width / textWidth;

            // Center first text string
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT1, ref textBounds);

            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
            canvas.DrawText(TEXT1, xText, yText, paint);

            // Center second text string
            textBounds = new SKRect();
            paint.MeasureText(TEXT2, ref textBounds);

            xText = info.Width / 2 - textBounds.MidX;
            yText = info.Height / 2 - textBounds.MidY;

            paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
            canvas.DrawText(TEXT2, xText, yText, paint);
        }
    }
}
```

`transparency`Поле анимируется в зависимости от 0 до 1 и обратно в синусоидальной цикл. Первая текстовая строка отображается с альфа-значением, вычисленным путем вычитания `transparency` значения из 1:

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * (1 - transparency)));
```

[`WithAlpha`](xref:SkiaSharp.SKColor.WithAlpha*)Метод задает альфа-компонент существующего цвета, который в данном случае имеет значение `SKColors.Blue` . Вторая текстовая строка использует значение альфа, вычисленное на основе `transparency` самого значения:

```csharp
paint.Color = SKColors.Blue.WithAlpha((byte)(0xFF * transparency));
```

В анимации между двумя словами, понуждая пользователя на "код больше" (или, возможно, запрашивается "дополнительный код"):

[![Код для более подробного кода](transparency-images/CodeMoreCode.png "Код для более подробного кода")](transparency-images/CodeMoreCode-Large.png#lightbox)

В предыдущей статье, посвященной [**основам битовых рисунков в SkiaSharp**](bitmaps.md), вы узнали, как отображать точечные рисунки с помощью одного из [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap*) методов `SKCanvas` . Все `DrawBitmap` методы включают в `SKPaint` себя объект в качестве последнего параметра. По умолчанию этот параметр имеет значение, `null` и его можно игнорировать. 

Кроме того, можно задать `Color` свойство этого объекта, `SKPaint` чтобы отобразить точечный рисунок с определенным уровнем прозрачности. Установка уровня прозрачности в `Color` свойстве `SKPaint` позволяет появление и уменьшение растровых изображений, а также разрешение одного растрового изображения в другой. 

Прозрачность растрового изображения показана на странице « **разрешение растрового изображения** ». XAML-файл создает экземпляр `SKCanvasView` и `Slider` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BitmapDissolvePage"
             Title="Bitmap Dissolve">
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="progressSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части загружает два ресурса точечного рисунка. Эти точечные рисунки имеют разные размеры, но они имеют одинаковое соотношение сторон.

```csharp
public partial class BitmapDissolvePage : ContentPage
{
    SKBitmap bitmap1;
    SKBitmap bitmap2;

    public BitmapDissolvePage()
    {
        InitializeComponent();

        // Load two bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(
                                "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg"))
        {
            bitmap1 = SKBitmap.Decode(stream);
        }
        using (Stream stream = assembly.GetManifestResourceStream(
                                "SkiaSharpFormsDemos.Media.FacePalm.jpg"))
        {
            bitmap2 = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find rectangle to fit bitmap
        float scale = Math.Min((float)info.Width / bitmap1.Width,
                                (float)info.Height / bitmap1.Height);
        SKRect rect = SKRect.Create(scale * bitmap1.Width,
                                    scale * bitmap1.Height);
        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Get progress value from Slider
        float progress = (float)progressSlider.Value;

        // Display two bitmaps with transparency
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = paint.Color.WithAlpha((byte)(0xFF * (1 - progress)));
            canvas.DrawBitmap(bitmap1, rect, paint);

            paint.Color = paint.Color.WithAlpha((byte)(0xFF * progress));
            canvas.DrawBitmap(bitmap2, rect, paint);
        }
    }
}
```

`Color` `SKPaint` Для свойства объекта задано два дополнительных уровня альфа-канала для двух точечных рисунков. При использовании `SKPaint` с точечными рисунками не имеет значения, чем остальная часть `Color` значения. Все это имеет значение альфа-канал. Здесь код просто вызывает `WithAlpha` метод для значения свойства по умолчанию `Color` .

Перемещение `Slider` разрешающей карты между одним точечным рисунком и другим:

[![Распознаватель точечного рисунка](transparency-images/BitmapDissolve.png "Распознаватель точечного рисунка")](transparency-images/BitmapDissolve-Large.png#lightbox)

В нескольких прошлых статьях вы узнали, как использовать SkiaSharp для рисования текста, кругов, эллипсов, скругленных прямоугольников и точечных рисунков. Следующий шаг — [SkiaSharp строки и пути](../paths/index.md) , в которых вы узнаете, как нарисовать подключенные линии в графическом контуре.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
