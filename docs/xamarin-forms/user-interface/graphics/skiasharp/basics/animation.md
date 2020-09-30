---
title: Базовая анимация в SkiaSharp
description: В этой статье объясняется, как анимировать графику SkiaSharp в Xamarin.Forms приложениях и демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3052220b914b09f18490846bbd2558bbf07e4d3a
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562266"
---
# <a name="basic-animation-in-skiasharp"></a>Базовая анимация в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Узнайте, как анимировать график SkiaSharp_

Вы можете анимировать график SkiaSharp в Xamarin.Forms , вызывая регулярный `PaintSurface` вызов метода, каждый раз рисуя графику немного иначе. Ниже приведена анимация, показанная далее в этой статье, с концентрическими круговами, которые кажутся раскрытием по центру:

![В центре кажется, что несколько концентрических кругов расширяются.](animation-images/animationexample.png)

Страница **эллипса пулсатинг** в программе [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) анимирует две оси эллипса, чтобы они были пулсатинг, и вы даже можете контролировать скорость этого пулсатион. Файл [**пулсатинжеллипсепаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) создает экземпляр и, Xamarin.Forms `Slider` `Label` чтобы отобразить текущее значение ползунка. Это распространенный способ интеграции `SKCanvasView` с другими Xamarin.Forms представлениями:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Файл кода программной части создает экземпляр `Stopwatch` объекта, который служит часами высокой точности. `OnAppearing`Переопределение задает `pageIsActive` для поля значение `true` и вызывает метод с именем `AnimationLoop` . `OnDisappearing`Переопределение задает `pageIsActive` для этого поля значение `false` :

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

`AnimationLoop`Метод запускает, `Stopwatch` а затем выполняет цикл, пока `pageIsActive` имеет значение `true` . По сути это «бесконечный цикл», когда страница активна, но это не приводит к зависанию программы, поскольку цикл завершается вызовом `Task.Delay` `await` оператора с оператором, который позволяет выполнять другие части программы. Аргумент, в `Task.Delay` результате чего он завершается через 1/30-й секунда. Определяет частоту кадров анимации.

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

`while`Цикл начинается с получения времени цикла из `Slider` . Это время в секундах, например 5. Вторая инструкция вычисляет значение `t` для *времени*. В случае с `cycleTime` 5 `t` увеличивается от 0 до 1 каждые 5 секунд. Аргумент `Math.Sin` функции во второй инструкции находится в диапазоне от 0 до 2π каждые 5 секунд. `Math.Sin`Функция возвращает значение в диапазоне от 0 до 1 в 0, а затем в &ndash; 1 и 0 каждые 5 секунд, но со значениями, которые меняются медленнее, когда значение приближается к 1 или – 1. Значение 1 добавляется, чтобы значения всегда были положительными, а затем разделены на 2, поэтому значения в диапазоне от 1/2 до 1/2 — от 0 до 1/2, но медленнее, если значение равно 1 и 0. Он хранится в `scale` поле, а `SKCanvasView` является недействительным.

`PaintSurface`Метод использует это `scale` значение для вычисления двух осей эллипса:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

Метод вычисляет максимальный радиус на основе размера отображаемой области и минимального радиуса на основе максимального радиуса. `scale`Значение анимируется в диапазоне от 0 до 1 и возвращается к 0, поэтому метод использует его для вычислить, `xRadius` а также `yRadius` диапазон между `minRadius` и `maxRadius` . Эти значения используются для рисования и заполнения эллипса.

[![Тройной снимок экрана со страницей эллипса Пулсатинг](animation-images/pulsatingellipse-small.png)](animation-images/pulsatingellipse-large.png#lightbox "Тройной снимок экрана со страницей эллипса Пулсатинг")

Обратите внимание, что `SKPaint` объект создается в `using` блоке. Как и многие классы SkiaSharp `SKPaint` являются производными от класса `SKObject` , который является производным от `SKNativeObject` , который реализует [`IDisposable`](xref:System.IDisposable) интерфейс. `SKPaint` переопределяет `Dispose` метод, чтобы освободить неуправляемые ресурсы.

 Помещение `SKPaint` `using` блока гарантирует, что `Dispose` метод вызывается в конце блока, чтобы освободить неуправляемые ресурсы. Это происходит в любом случае, когда память, используемая `SKPaint` объектом, освобождается сборщиком мусора .NET, но в коде анимации лучше проактивно освободить память более упорядоченным образом.

 Лучшим решением в этом случае является создание двух `SKPaint` объектов один раз и сохранение их в виде полей.

Это именно то, что делает **расширяемая круговая** анимация. [`ExpandingCirclesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs)Класс начинается с определения нескольких полей, включая `SKPaint` объект:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

Эта программа использует другой подход к анимации на основе Xamarin.Forms `Device.StartTimer` метода. `t`Поле анимируется от 0 до 1 каждую `cycleTime` миллисекунду:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`PaintSurface`Обработчик формирует пять концентрических кругов с анимированными радиусами. Если `baseRadius` переменная вычисляется как 100, то как `t` анимируется от 0 до 1, радиусы пяти кругов увеличиваются с 0 до 100, 100 – 200, от 200 до 300, от 300 до 400 и от 400 до 500. Для большей части кругов `strokeWidth` — 50, а для первого круга — `strokeWidth` анимация от 0 до 50. Для большинства кругов цвет выделяется синим цветом, но для последнего круга цвет анимируется от синего к прозрачному. Обратите внимание на четвертый аргумент `SKColor` конструктора, который задает непрозрачность:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

В результате изображение выглядит одинаково, когда `t` равно 0, как если `t` равно 1, а окружности, по-видимому, продолжают расширяться непрерывно:

[![Тройной снимок экрана с развернутой страницей кругов](animation-images/expandingcircles-small.png)](animation-images/expandingcircles-large.png#lightbox "Тройной снимок экрана с развернутой страницей кругов")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)