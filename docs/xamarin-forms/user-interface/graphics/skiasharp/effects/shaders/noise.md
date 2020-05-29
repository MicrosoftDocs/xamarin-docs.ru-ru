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
ms.openlocfilehash: 45ec48c0b7b58e26fa47d7343e96bb49591cb339
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127768"
---
# <a name="skiasharp-noise-and-composing"></a>SkiaSharp шум и составление

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Простая векторная графика, как правило, выглядит неестественным. Прямые линии, гладкие кривые и сплошные цвета не похожи на реальные объекты. Работая с созданными компьютером графическими изображениями для 1982 Movie _трон_, компьютер анализу Алексей Perl начал разрабатывать алгоритмы, использующие случайные процессы для предоставления этих изображений более реалистичным текстурам. В 1997 Алексей Perl выиграл Academy награду за технические достижения. Его работа известна как шум Perl и поддерживается в SkiaSharp. Приведем пример:

![Пример шума на Perl](noise-images/NoiseSample.png "Пример шума на Perl")

Как видите, каждый пиксель не является случайным значением цвета. Непрерывность в результате от пикселя до пикселя приводит к возникновению случайных фигур.

Поддержка шума в Perl в СКИА основана на спецификации W3C для CSS и SVG. Раздел 8,20 из [**модуля эффектов фильтра уровень 1**](https://www.w3.org/TR/filter-effects-1/#feTurbulenceElement) включает в себя базовые алгоритмы для Perl в коде на языке C.

## <a name="exploring-perlin-noise"></a>Исследование шума в Perl

[`SKShader`](xref:SkiaSharp.SKShader)Класс определяет два разных статических метода для создания шума Perl: [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise*) и [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseTurbulence*) . Параметры идентичны:

```csharp
public static SkiaSharp CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);

public static SkiaSharp.SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed);
```

Оба метода также существуют в перегруженных версиях с дополнительным `SKPointI` параметром. Эти перегрузки обсуждаются в разделе [**мозаичный шум на Perl**](#tiling-perlin-noise) .

Два `baseFrequency` аргумента являются положительными значениями, определенными в документации SkiaSharp, как в диапазоне от 0 до 1, но для них также можно задать более высокие значения. Чем выше значение, тем больше изменение в случайном изображении в горизонтальном и вертикальном направлениях.

`numOctaves`Значение представляет собой целое число, равное 1 или выше. Он связан с коэффициентом итерации в алгоритмах. Каждый дополнительный Октава повлияет на пополовину предыдущего Октава, поэтому этот результат сокращается с помощью более высоких значений Октава.

`seed`Параметр является отправной точкой генератора случайных чисел. Хотя в качестве значения с плавающей запятой, дробная часть усекается до ее использования, а значение 0 равно 1.

На странице « **шум Perl** » в [ **скиашарпформсдемос**)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) можно экспериментировать с различными значениями `baseFrequency` `numOctaves` аргументов и. Вот файл XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PerlinNoisePage"
             Title="Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="baseFrequencyXSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyXText"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="baseFrequencyYSlider"
                Maximum="4"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="baseFrequencyYText"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference octavesStepper},
                                  Path=Value,
                                  StringFormat='Number of Octaves: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="octavesStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Он использует два `Slider` представления для двух `baseFrequency` аргументов. Чтобы увеличить диапазон нижних значений, ползунки являются логарифмами. Файл кода программной части вычисляет аргументы для `SKShader` методов от степеней `Slider` значений. В `Label` представлениях отображаются вычисляемые значения:

```csharp
float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);
```

`Slider`Значение 1 соответствует 0,001, `Slider` значение OS 2 соответствует 0,01, `Slider` значения 3 соответствуют 0,1, а `Slider` значение 4 соответствует 1.

Вот файл кода программной части, включающий этот код:

```csharp
public partial class PerlinNoisePage : ContentPage
{
    public PerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader =
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0);

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader =
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0);

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Вот программа, выполняемая на устройствах iOS, Android и универсальная платформа Windows (UWP). Шум фрактала отображается в верхней половине холста. Шум турбуленце находится в нижней половине:

[![Шум Perl](noise-images/PerlinNoise.png "Шум Perl")](noise-images/PerlinNoise-Large.png#lightbox)

Те же аргументы всегда создают тот же шаблон, который начинается в левом верхнем углу. Такая согласованность очевидна при изменении ширины и высоты окна UWP. Когда Windows 10 перерисовывает экран, шаблон в верхней половине холста остается неизменным.

Шаблон шума включает различные степени прозрачности. Прозрачность станет очевидной, если задать цвет в `canvas.Clear()` вызове. Этот цвет станет заметным в шаблоне. Вы также увидите этот результат в разделе [**Объединение нескольких шейдеров**](#combining-multiple-shaders).

Эти шаблоны шума Perl редко используются сами по себе. Часто они накладываются на режимы наложения и фильтры цветов, обсуждаемые в последующих статьях.

## <a name="tiling-perlin-noise"></a>Разбиение шума на Perl

Два статических `SKShader` метода для создания шума в Perl также существуют в перегрузках версий. [`CreatePerlinNoiseFractalNoise`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) [`CreatePerlinNoiseTurbulence`](xref:SkiaSharp.SKShader.CreatePerlinNoiseFractalNoise(System.Single,System.Single,System.Int32,System.Single,SkiaSharp.SKPointI)) Перегрузки и имеют дополнительный `SKPointI` параметр:

```csharp
public static SKShader CreatePerlinNoiseFractalNoise (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);

public static SKShader CreatePerlinNoiseTurbulence (float baseFrequencyX, float baseFrequencyY, int numOctaves, float seed, SKPointI tileSize);
```

[`SKPointI`](xref:SkiaSharp.SKPointI)Структура — это целочисленная версия знакомой [`SKPoint`](xref:SkiaSharp.SKPoint) структуры. `SKPointI`Определяет `X` и `Y` свойства типа, `int` а не `float` .

Эти методы создают Повторяющийся шаблон указанного размера. В каждой плитке правый край совпадает с левым ребром, а верхний край — с нижней границей. Эта характеристика показана на странице « **мозаичные помехи Perl** ». XAML-файл подобен предыдущему примеру, но он имеет только `Stepper` представление для изменения `seed` аргумента:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.TiledPerlinNoisePage"
             Title="Tiled Perlin Noise">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     HorizontalOptions="Center"
                     Margin="10">

            <Label Text="{Binding Source={x:Reference seedStepper},
                                  Path=Value,
                                  StringFormat='Seed: {0:F0}'}"
                   VerticalOptions="Center" />

            <Stepper x:Name="seedStepper"
                     Minimum="1"
                     ValueChanged="OnStepperValueChanged" />

        </StackLayout>
    </StackLayout>
</ContentPage>
```

Файл кода программной части определяет константу для размера плитки. `PaintSurface`Обработчик создает точечный рисунок этого размера и `SKCanvas` для рисования в этом растровом изображении. `SKShader.CreatePerlinNoiseTurbulence`Метод создает шейдер с размером плитки. Этот шейдер рисуется на точечном рисунке:

```csharp
public partial class TiledPerlinNoisePage : ContentPage
{
    const int TILE_SIZE = 200;

    public TiledPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get seed value from stepper
        float seed = (float)seedStepper.Value;

        SKRect tileRect = new SKRect(0, 0, TILE_SIZE, TILE_SIZE);

        using (SKBitmap bitmap = new SKBitmap(TILE_SIZE, TILE_SIZE))
        {
            using (SKCanvas bitmapCanvas = new SKCanvas(bitmap))
            {
                bitmapCanvas.Clear();

                // Draw tiled turbulence noise on bitmap
                using (SKPaint paint = new SKPaint())
                {
                    paint.Shader = SKShader.CreatePerlinNoiseTurbulence(
                                        0.02f, 0.02f, 1, seed,
                                        new SKPointI(TILE_SIZE, TILE_SIZE));

                    bitmapCanvas.DrawRect(tileRect, paint);
                }
            }

            // Draw tiled bitmap shader on canvas
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat);
                canvas.DrawRect(info.Rect, paint);
            }

            // Draw rectangle showing tile
            using (SKPaint paint = new SKPaint())
            {
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                paint.StrokeWidth = 2;

                canvas.DrawRect(tileRect, paint);
            }
        }
    }
}
```

После создания точечного рисунка `SKPaint` используется другой объект для создания шаблона мозаичного точечного рисунка путем вызова метода `SKShader.CreateBitmap` . Обратите внимание на два аргумента `SKShaderTileMode.Repeat` :

```csharp
paint.Shader = SKShader.CreateBitmap(bitmap,
                                     SKShaderTileMode.Repeat,
                                     SKShaderTileMode.Repeat);
```

Этот шейдер используется для покрытия холста. Наконец, `SKPaint` для обводки прямоугольника, показывающего размер исходного растрового изображения, используется другой объект.

`seed`Из пользовательского интерфейса может быть выбран только параметр. Если на каждой платформе используется один и тот же `seed` шаблон, они будут показывать один и тот же шаблон. Различные `seed` значения приводят к различным шаблонам:

[![Мозаичный шум в Perl](noise-images/TiledPerlinNoise.png "Мозаичный шум в Perl")](noise-images/TiledPerlinNoise-Large.png#lightbox)

Шаблон 200-пиксельного квадрата в левом верхнем углу полностью переносится в другие плитки.

## <a name="combining-multiple-shaders"></a>Объединение нескольких шейдеров

`SKShader`Класс включает [`CreateColor`](xref:SkiaSharp.SKShader.CreateColor*) метод, создающий шейдер с заданным сплошным цветом. Этот шейдер не очень удобен для себя, так как можно просто задать этот цвет для `Color` свойства `SKPaint` объекта и установить `Shader` свойство в значение null.

Этот `CreateColor` метод будет полезен в другом методе, который `SKShader` определяет. Этот метод [`CreateCompose`](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader)) сочетает два шейдера. Ниже приведен синтаксис.

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader);
```

`srcShader`(Исходный шейдер) фактически нарисовывается поверх `dstShader` (шейдер назначения). Если исходный шейдер является сплошным цветом или градиентом без прозрачности, целевой шейдер будет полностью скрыт.

Шейдер шума Perl содержит прозрачность. Если этот шейдер является источником, шейдер назначения будет отображаться через прозрачные области.

На странице **составной страницы Perl** имеется XAML-файл, который практически идентичен первой странице **шума Perl** . Файл кода программной части также аналогичен. Но исходная страница **Perl «шум** » устанавливает `Shader` свойство объекта `SKPaint` на шейдер, возвращаемый статическими `CreatePerlinNoiseFractalNoise` `CreatePerlinNoiseTurbulence` методами и. Это **состояло из Perl** -вызовов страниц шума `CreateCompose` для комбинированного шейдера. Назначение — это сплошной синий шейдер, созданный с помощью `CreateColor` . Источником является шейдер шума Perl:

```csharp
public partial class ComposedPerlinNoisePage : ContentPage
{
    public ComposedPerlinNoisePage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders and stepper
        float baseFreqX = (float)Math.Pow(10, baseFrequencyXSlider.Value - 4);
        baseFrequencyXText.Text = String.Format("Base Frequency X = {0:F4}", baseFreqX);

        float baseFreqY = (float)Math.Pow(10, baseFrequencyYSlider.Value - 4);
        baseFrequencyYText.Text = String.Format("Base Frequency Y = {0:F4}", baseFreqY);

        int numOctaves = (int)octavesStepper.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseFractalNoise(baseFreqX,
                                                       baseFreqY,
                                                       numOctaves,
                                                       0));

            SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
            canvas.DrawRect(rect, paint);

            paint.Shader = SKShader.CreateCompose(
                SKShader.CreateColor(SKColors.Blue),
                SKShader.CreatePerlinNoiseTurbulence(baseFreqX,
                                                     baseFreqY,
                                                     numOctaves,
                                                     0));

            rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Шейдер шума фрактала Мандельброта находится в верхней части; Шейдер турбуленце находится в нижней части:

[![Состоялся шум Perl](noise-images/ComposedPerlinNoise.png "Состоялся шум Perl")](noise-images/ComposedPerlinNoise-Large.png#lightbox)

Обратите внимание на то, насколько синим являются эти шейдеры, отличные от тех, которые отображаются на странице **помехи Perl** . Разница показывает степень прозрачности в шейдерах шума.

Существует также перегрузка [`CreateCompose`](xref:SkiaSharp.SKShader.CreateCompose(SkiaSharp.SKShader,SkiaSharp.SKShader,SkiaSharp.SKBlendMode)) метода:

```csharp
public static SKShader CreateCompose (SKShader dstShader, SKShader srcShader, SKBlendMode blendMode);
```

Последний параметр является членом `SKBlendMode` перечисления — Перечисление с 29 членами, которое обсуждается в следующей серии статей по [**SkiaSharp композиции и режимам смешения**](../blend-modes/index.md).

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
