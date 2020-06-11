---
Title: «SkiaSharp Image Filters» Description:» Узнайте, как использовать фильтр изображений для создания размытий и теней.
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: 173E7B22-AEC8-4F12-B657-1C0CEE01AD63 Автор: давидбритч MS. author: дабритч MS. Дата: 08/27/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="skiasharp-image-filters"></a>Фильтры изображений SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Фильтры изображений — это эффекты, которые работают со всеми цветовыми битами пикселов, составляющих изображение. Они являются более гибкими, чем фильтры маски, которые работают только с альфа-каналом, как описано в статье [**SkiaSharp Mask Filters**](mask-filters.md). Чтобы использовать фильтр изображений, задайте [`ImageFilter`](xref:SkiaSharp.SKPaint.ImageFilter) `SKPaint` для свойства объекта тип [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) , который вы создали, вызвав один из статических методов класса.

Лучший способ знакомства с фильтрами масок — эксперименты с этими статическими методами. Фильтр маски можно использовать для размытия всего растрового изображения:

![Пример размытия](image-filters-images/ImageFilterExample.png "Пример размытия")

В этой статье также показано, как использовать фильтр изображений для создания тени и для приподнятия и енгравинг эффектов.

## <a name="blurring-vector-graphics-and-bitmaps"></a>Размытие векторной графики и растровых изображений

Эффект размытия, созданный [`SKImageFilter.CreateBlur`](xref:SkiaSharp.SKImageFilter.CreateBlur*) статическим методом, имеет значительное преимущество над методами размытия в [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) классе: фильтр изображений может размытие всего растрового изображения. Метод имеет следующий синтаксис:

```csharp
public static SkiaSharp.SKImageFilter CreateBlur (float sigmaX, float sigmaY,
                                                  SKImageFilter input = null,
                                                  SKImageFilter.CropRect cropRect = null);
```

Метод имеет два значения &mdash; , первый для экстента размытия в горизонтальном направлении, а второй для вертикального направления. Фильтры с изображениями можно каскадировать, указав другой фильтр изображений в качестве необязательного третьего аргумента. Также можно указать прямоугольник обрезки.

Страница **эксперимента с размытием изображения** в [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) включает два `Slider` представления, которые позволяют экспериментировать с установкой различных уровней размытия.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.ImageBlurExperimentPage"
             Title="Image Blur Experiment">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части использует два `Slider` значения для вызова `SKImageFilter.CreateBlur` объекта, `SKPaint` используемого для показа как текста, так и точечного рисунка:

```csharp
public partial class ImageBlurExperimentPage : ContentPage
{
    const string TEXT = "Blur My Text";

    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                            typeof(MaskBlurExperimentPage),
                            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public ImageBlurExperimentPage ()
    {
        InitializeComponent ();
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

        canvas.Clear(SKColors.Pink);

        // Get values from sliders
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = (info.Width - 100) / (TEXT.Length / 2);
            paint.ImageFilter = SKImageFilter.CreateBlur(sigmaX, sigmaY);

            // Get text bounds and calculate display rectangle
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);
            SKRect textRect = new SKRect(0, 0, info.Width, textBounds.Height + 50);

            // Center the text in the display rectangle
            float xText = textRect.Width / 2 - textBounds.MidX;
            float yText = textRect.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);

            // Calculate rectangle for bitmap
            SKRect bitmapRect = new SKRect(0, textRect.Bottom, info.Width, info.Height);
            bitmapRect.Inflate(-50, -50);

            canvas.DrawBitmap(bitmap, bitmapRect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

На трех снимках экрана показаны различные параметры `sigmaX` для `sigmaY` параметров и.

[![Эксперимент размытия изображения](image-filters-images/ImageBlurExperiment.png "Эксперимент размытия изображения")](image-filters-images/ImageBlurExperiment-Large.png#lightbox)

Чтобы обеспечить единообразное размытие между различными размерами и разрешениями экрана, задайте `sigmaX` `sigmaY` значения и для значений, пропорционально отображаемому размеру в пикселе изображения, к которому применяется размытие.

## <a name="drop-shadow"></a>Отбрасывание тени

[`SKImageFilter.CreateDropShadow`](xref:SkiaSharp.SKImageFilter.CreateDropShadow*)Статический метод создает `SKImageFilter` объект для тени с тенью:

```csharp
public static SKImageFilter CreateDropShadow (float dx, float dy,
                                              float sigmaX, float sigmaY,
                                              SKColor color,
                                              SKDropShadowImageFilterShadowMode shadowMode,
                                              SKImageFilter input = null,
                                              SKImageFilter.CropRect cropRect = null);
```

Присвойте этому объекту `ImageFilter` свойство `SKPaint` объекта, и все, что рисуется с помощью этого объекта, будет иметь тень.

`dx`Параметры и `dy` указывают горизонтальное и вертикальное смещение тени в пикселях от графического объекта. В этом случае в двухмерной графике предполагается, что источник освещения находится в левом верхнем углу, что означает, что оба этих аргумента должны быть положительными для размещения тени ниже и справа от графического объекта.

`sigmaX`Параметры и `sigmaY` являются размытыми факторами для тени перетаскивания.

`color`Параметр — это цвет тени. Это `SKColor` значение может включать прозрачность. Одна из возможных вариантов — значение цвета, `SKColors.Black.WithAlpha(0x80)` чтобы затемнить цвет фона.

Последние два параметра являются необязательными.

Программа **удаления теневых экспериментов** позволяет экспериментировать со значениями `dx` , `dy` , `sigmaX` и `sigmaY` для вывода текстовой строки с тенью. Файл XAML создает четыре `Slider` представления для установки этих значений:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.DropShadowExperimentPage"
             Title="Drop Shadow Experiment">
    <ContentPage.Resources>
        <Style TargetType="Slider">
            <Setter Property="Margin" Value="10, 0" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="HorizontalTextAlignment" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="dxSlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dxSlider},
                              Path=Value,
                              StringFormat='Horizontal offset = {0:F1}'}" />

        <Slider x:Name="dySlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dySlider},
                              Path=Value,
                              StringFormat='Vertical offset = {0:F1}'}" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части использует эти значения для создания Красной тени на синей текстовой строке:

```csharp
public partial class DropShadowExperimentPage : ContentPage
{
    const string TEXT = "Drop Shadow";

    public DropShadowExperimentPage ()
    {
        InitializeComponent ();
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

        // Get values from sliders
        float dx = (float)dxSlider.Value;
        float dy = (float)dySlider.Value;
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = info.Width / 7;
            paint.Color = SKColors.Blue;
            paint.ImageFilter = SKImageFilter.CreateDropShadow(
                                    dx,
                                    dy,
                                    sigmaX,
                                    sigmaY,
                                    SKColors.Red,
                                    SKDropShadowImageFilterShadowMode.DrawShadowAndForeground);

            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Center the text in the display rectangle
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Вот работающая программа:

[![Удаление теневого эксперимента](image-filters-images/DropShadowExperiment.png "Удаление теневого эксперимента")](image-filters-images/DropShadowExperiment-Large.png#lightbox)

Отрицательные значения смещения на снимке экрана универсальная платформа Windows в крайнем правом углу приводят к отображению тени над текстом и слева от него. Это подразумевает источник освещения в правом нижнем углу, который не является соглашением для компьютерной графики. Но это не кажется неправильной стороной, возможно, потому, что тень также становится очень размытой и кажется более декоративной, чем большинство теней.

## <a name="lighting-effects"></a>Эффекты освещения

`SKImageFilter`Класс определяет шесть методов, которые имеют похожие имена и параметры, перечисленные ниже в порядке возрастания сложности:

- [`CreateDistantLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateDistantLitDiffuse*)
- [`CreateDistantLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateDistantLitSpecular*)
- [`CreatePointLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreatePointLitDiffuse*)
- [`CreatePointLitSpecular`](xref:SkiaSharp.SKImageFilter.CreatePointLitSpecular*)
- [`CreateSpotLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateSpotLitDiffuse*)
- [`CreateSpotLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateSpotLitSpecular*)

Эти методы создают фильтры изображений, имитирующие воздействие различных видов освещения на трехмерных поверхностях. Результирующий фильтр изображений освещает двухмерные объекты так, как если бы они существовали в трехмерном пространстве, что может привести к тому, что эти объекты появятся с повышенными или рецессед или с выделением отражения.

`Distant`Методы освещения предполагают, что освещение поступает далеко с дальнего расстояния. Для освещения объектов предполагается, что свет указывает на единое направление в трехмерном пространстве, похоже на солнце в небольшой области земли. `Point`Методы освещения имитируют лампочку, расположенную в трехмерном пространстве, которая создает освещение во всех направлениях. `Spot`Источник имеет как расположение, так и направление, подобно фонарик.

Расположения и направления в трехмерном пространстве указываются с помощью значений [`SKPoint3`](xref:SkiaSharp.SKPoint3) структуры, которые похожи на, `SKPoint` но с тремя свойствами с именами `X` , `Y` и `Z` .

Число и сложность параметров этих методов затрудняют эксперименты. Чтобы приступить к работе, на странице **удаленных экспериментов** можно поэкспериментировать с параметрами `CreateDistantLightDiffuse` метода:

```csharp
public static SKImageFilter CreateDistantLitDiffuse (SKPoint3 direction,
                                                     SKColor lightColor,
                                                     float surfaceScale,
                                                     float kd,
                                                     SKImageFilter input = null,
                                                     SKImageFilter.CropRect cropRect = null);
```

На странице не используются последние два необязательных параметра.

Три `Slider` представления в файле XAML позволяют выбрать `Z` координату `SKPoint3` значения, `surfaceScale` параметр и `kd` параметр, который определен в документации по API как «рассеянное освещение»:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaLightExperiment.MainPage"
             Title="Distant Light Experiment">

    <StackLayout>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           VerticalOptions="FillAndExpand" />

        <Slider x:Name="zSlider"
                Minimum="-10"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zSlider},
                              Path=Value,
                              StringFormat='Z = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="surfaceScaleSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference surfaceScaleSlider},
                              Path=Value,
                              StringFormat='Surface Scale = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="lightConstantSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference lightConstantSlider},
                              Path=Value,
                              StringFormat='Light Constant = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части получает эти три значения и использует их для создания фильтра изображений для вывода текстовой строки:

```csharp
public partial class DistantLightExperimentPage : ContentPage
{
    const string TEXT = "Lighting";

    public DistantLightExperimentPage()
    {
        InitializeComponent();
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

        float z = (float)zSlider.Value;
        float surfaceScale = (float)surfaceScaleSlider.Value;
        float lightConstant = (float)lightConstantSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.IsAntialias = true;

            // Size text to 90% of canvas width
            paint.TextSize = 100;
            float textWidth = paint.MeasureText(TEXT);
            paint.TextSize *= 0.9f * info.Width / textWidth;

            // Find coordinates to center text
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            float xText = info.Rect.MidX - textBounds.MidX;
            float yText = info.Rect.MidY - textBounds.MidY;

            // Create distant light image filter
            paint.ImageFilter = SKImageFilter.CreateDistantLitDiffuse(
                                    new SKPoint3(2, 3, z),
                                    SKColors.White,
                                    surfaceScale,
                                    lightConstant);

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Первый аргумент `SKImageFilter.CreateDistantLitDiffuse` является направлением освещения. Положительные координаты X и Y указывают на то, что освещение направлено вправо и вниз. Положительные координаты Z на экране. XAML-файл позволяет выбрать отрицательные значения Z, но это только позволяет увидеть, что происходит: концептуально, отрицательные координаты Z приводят к тому, что источник данных появляется на экране. Для любых других небольших отрицательных значений эффекты освещения перестают работать.

`surfaceScale`Аргумент может принимать значения от – 1 до 1. (Более высокие или более низкие значения не влияют на дальнейшие действия.) Это относительные значения на оси Z, указывающие смещение графического объекта (в данном случае текстовую строку) с поверхности холста. Используйте отрицательные значения, чтобы увеличить текстовую строку над поверхностью холста, и положительные значения, чтобы Депресс его на холст.

`lightConstant`Значение должно быть положительным. (Программа допускает отрицательные значения, чтобы вы могли увидеть, что они приведут к прекращению работы.) Более высокие значения приводят к более интенсивному освещению.

Эти факторы могут быть сбалансированы, чтобы получить эффект приподнятости при `surfaceScale` отрицательном значении (как при работе с снимками экрана iOS и Android) и утопленном эффекте при `surfaceScale` положительном виде, как на снимке экрана UWP справа:

[![Отдаленный неяркий эксперимент](image-filters-images/DistantLightExperiment.png "Отдаленный неяркий эксперимент")](image-filters-images/DistantLightExperiment-Large.png#lightbox)

Снимок экрана Android имеет значение Z 0, означающее, что светлое указывает только вниз и вправо. Фон не загорается, а поверхность текстовой строки не загорается. Светлое воздействие влияет только на границы текста, что очень незаметно.

Другой подход к выступающим и утопленным текстам был продемонстрирован в статье [Преобразование](../transforms/translate.md)преобразования: текстовая строка отображается дважды с различными цветами, которые немного отличаются друг от друга.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
