---
title: Фильтры маски SkiaSharp
description: Узнайте, как использовать фильтр маски для создания размытого и других альфа-эффектов.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 940422A1-8BC0-4039-8AD7-26C61320F858
author: davidbritch
ms.author: dabritch
ms.date: 08/27/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a9291a56ffa1a05f8e2041033279363f8ec4d34
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374151"
---
# <a name="skiasharp-mask-filters"></a>Фильтры маски SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Фильтры маски — это эффекты, управляющие геометрией и альфа-каналом графических объектов. Чтобы использовать фильтр маски, задайте [`MaskFilter`](xref:SkiaSharp.SKPaint.MaskFilter) `SKPaint` для свойства объекта тип [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) , который вы создали, вызвав один из `SKMaskFilter` статических методов.

Лучший способ знакомства с фильтрами масок — эксперименты с этими статическими методами. Наиболее полезный фильтр маски создает размытие:

![Пример размытия](mask-filters-images/MaskFilterExample.png "Пример размытия")

Это единственная функция фильтра маски, описанная в этой статье. Следующая статья по [**фильтрам изображений SkiaSharp**](image-filters.md) также описывает эффект размытия, который вы можете предпочесть. 

Статический [`SKMaskFilter.CreateBlur`](xref:SkiaSharp.SKMaskFilter.CreateBlur(SkiaSharp.SKBlurStyle,System.Single)) метод имеет следующий синтаксис:

```csharp
public static SKMaskFilter CreateBlur (SKBlurStyle blurStyle, float sigma);
```

Перегрузки позволяют задавать флаги для алгоритма, используемого для создания размытия, и прямоугольник, чтобы избежать размытия в областях, которые будут охвачены другими графическими объектами.

[`SKBlurStyle`](xref:SkiaSharp.SKBlurStyle) является перечислением со следующими членами:

- `Normal`
- `Solid`
- `Outer`
- `Inner`

Эффекты этих стилей показаны в приведенных ниже примерах. `sigma`Параметр задает экстент размытия. В более ранних версиях СКИА экстент размытия был обозначен значением радиуса. Если значение радиуса предпочтительнее для вашего приложения, то существует статический [`SKMaskFilter.ConvertRadiusToSigma`](xref:SkiaSharp.SKMaskFilter.ConvertRadiusToSigma*) метод, который может преобразовать из одного в другой. Метод умножает радиус на 0,57735 и добавляет 0,5.

На странице **эксперимент размытия маски** в образце [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) можно поэкспериментировать с стилями размытия и Сигма. XAML-файл создает экземпляр `Picker` с четырьмя `SKBlurStyle` членами перечисления и `Slider` для указания значения Сигма:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.MaskBlurExperimentPage"
             Title="Mask Blur Experiment">
    
    <StackLayout>
        <skiaforms:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blurStylePicker" 
                Title="Filter Blur Style" 
                Margin="10, 0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlurStyle}">
                    <x:Static Member="skia:SKBlurStyle.Normal" />
                    <x:Static Member="skia:SKBlurStyle.Solid" />
                    <x:Static Member="skia:SKBlurStyle.Outer" />
                    <x:Static Member="skia:SKBlurStyle.Inner" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="sigmaSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaSlider},
                              Path=Value,
                              StringFormat='Sigma = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части использует эти значения для создания `SKMaskFilter` объекта и задания его `MaskFilter` свойству `SKPaint` объекта. Этот `SKPaint` объект используется для рисования текстовой строки и точечного рисунка:

```csharp
public partial class MaskBlurExperimentPage : ContentPage
{
    const string TEXT = "Blur My Text";

    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                            typeof(MaskBlurExperimentPage), 
                            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public MaskBlurExperimentPage ()
    {
        InitializeComponent ();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
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

        // Get values from XAML controls
        SKBlurStyle blurStyle =
            (SKBlurStyle)(blurStylePicker.SelectedIndex == -1 ?
                                        0 : blurStylePicker.SelectedItem);

        float sigma = (float)sigmaSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = (info.Width - 100) / (TEXT.Length / 2);
            paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);

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

Вот программа, работающая на iOS, Android и универсальная платформа Windows (UWP) с `Normal` стилем размытия и возрастающими `sigma` уровнями:

[![Эффект размытия маски — нормальный](mask-filters-images/MaskBlurExperiment-Normal.png "Эффект размытия маски — нормальный")](mask-filters-images/MaskBlurExperiment-Normal-Large.png#lightbox)

Обратите внимание, что размытие затрагивает только края точечного рисунка. `SKMaskFilter`Класс не является правильным эффектом для использования, если требуется размытие всего растрового изображения. Для этого необходимо использовать [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) класс, как описано в следующей статье о [**фильтрах изображений SkiaSharp**](image-filters.md).

Текст становится более размытым с увеличением значений `sigma` аргумента. В экспериментах с этой программой вы заметите, что для определенного `sigma` значения Размытие более экстремальо на рабочем столе Windows 10. Это различие обусловлено тем, что плотность пикселя меньше на настольном мониторе, чем на мобильных устройствах, поэтому высота текста в пикселях ниже. `sigma`Значение пропорционально экстенту размытия в пикселях, поэтому для заданного `sigma` значения результат будет более крайним, чем на экранах с более низким разрешением. В рабочем приложении, вероятно, потребуется вычислить `sigma` значение, пропорциональное размеру изображения. 

Попробуйте несколько значений, прежде чем приступить к сопоставлению уровня размытия, который лучше подходит для вашего приложения. Например, на странице " **эксперимент размытия маски** " попробуйте установить `sigma` подобный вид:

```csharp
sigma = paint.TextSize / 18;
paint.MaskFilter = SKMaskFilter.CreateBlur(blurStyle, sigma);
```

Теперь `Slider` действие не оказывает никакого влияния, но степень размытия согласована между платформами:

[![Размытие эксперимента — согласуется](mask-filters-images/MaskBlurExperiment-Consistent.png "Размытие эксперимента — согласуется")](mask-filters-images/MaskBlurExperiment-Consistent-Large.png#lightbox)

Все снимки экрана до сих пор показали размытие, созданное с помощью `SKBlurStyle.Normal` члена перечисления. На следующих снимках экрана показаны эффекты `Solid` `Outer` `Inner` стилей размытия, и.

[![Эксперимент размытия маски](mask-filters-images/MaskBlurExperiment.png "Эксперимент размытия маски")](mask-filters-images/MaskBlurExperiment-Large.png#lightbox)

Снимок экрана iOS показывает `Solid` стиль: текстовые символы по-прежнему представляются сплошными черными штрихами, а размытие добавляется к внешним знакам текста. 

Снимок экрана Android в среднем отображает `Outer` стиль. сами по себе символы обрабатываются (как в виде точечного рисунка), а размытие окружает пустое пространство, где проявлялись текстовые символы. 

На снимке экрана UWP справа отображается `Inner` стиль. Размытие ограничено областью, обычно занятой текстовыми символами.

В статье [**линейного градиента SkiaSharp**](shaders/linear-gradient.md#transparency-and-gradients) описана программа **градиента отражения** , которая использовала линейный градиент и преобразование для имитации отражения текстовой строки:

[![Градиентное отражение](shaders/linear-gradient-images/ReflectionGradient.png "Градиентное отражение")](shaders/linear-gradient-images/ReflectionGradient-Large.png#lightbox)

Страница **отражения размытия** добавляет в этот код одну инструкцию:

```csharp
public class BlurryReflectionPage : ContentPage
{
    const string TEXT = "Reflection";

    public BlurryReflectionPage()
    {
        Title = "Blurry Reflection";

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
            // Set text color to blue
            paint.Color = SKColors.Blue;

            // Set text size to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to position text above center
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2;

            // Draw unreflected text
            canvas.DrawText(TEXT, xText, yText, paint);

            // Shift textBounds to match displayed text
            textBounds.Offset(xText, yText);

            // Use those offsets to create a gradient for the reflected text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, textBounds.Top),
                                new SKPoint(0, textBounds.Bottom),
                                new SKColor[] { paint.Color.WithAlpha(0),
                                                paint.Color.WithAlpha(0x80) },
                                null,
                                SKShaderTileMode.Clamp);

            // Create a blur mask filter
            paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);

            // Scale the canvas to flip upside-down around the vertical center
            canvas.Scale(1, -1, 0, yText);

            // Draw reflected text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Новая инструкция добавляет фильтр размытия для текста, основанного на размере текста:

```csharp
paint.MaskFilter = SKMaskFilter.CreateBlur(SKBlurStyle.Normal, paint.TextSize / 36);
```

Этот фильтр размытия приводит к появлению более реалистичного отражения:

[![Отражение с размытием](mask-filters-images/BlurryReflection.png "Отражение с размытием")](mask-filters-images/BlurryReflection-Large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)