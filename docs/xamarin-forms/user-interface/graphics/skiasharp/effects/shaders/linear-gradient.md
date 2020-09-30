---
title: Линейный градиент SkiaSharp
description: Узнайте, как обводка линий или заливки областей с градиентами, состоящими из постепенного смешения двух цветов.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 20A2A8C4-FEB7-478D-BF57-C92E26117B6A
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2cc0806af28360cf4bf2bb7e382e8d0a423abab9
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91555532"
---
# <a name="the-skiasharp-linear-gradient"></a>Линейный градиент SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[`SKPaint`](xref:SkiaSharp.SKPaint)Класс определяет [`Color`](xref:SkiaSharp.SKPaint.Color) свойство, используемое для обводки линий или заливки областей сплошным цветом. Можно также обводка линий или заливки с помощью _градиентов_, которые являются постепенным смешением цветов.

![Пример линейного градиента](linear-gradient-images/LinearGradientSample.png "Пример линейного градиента")

Самый простой тип градиента — _линейный_ градиент. Смешение цветов выполняется в строке ( _линия градиента_) с одной точки на другую. Линии, перпендикулярные линии градиента, имеют одинаковый цвет. Линейный градиент можно создать с помощью одного из двух статических [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) методов. Разница между двумя перегрузками состоит в том, что одна из них включает преобразование матрицы, а другая — нет. 

Эти методы возвращают объект типа [`SKShader`](xref:SkiaSharp.SKShader) , для которого задается [`Shader`](xref:SkiaSharp.SKPaint.Shader) свойство `SKPaint` . Если `Shader` свойство не имеет значение null, оно переопределяет `Color` свойство. Любая линия с обводкой или любая область, заполненная с помощью этого `SKPaint` объекта, основана на градиенте, а не на сплошном цвете.

> [!NOTE]
> `Shader`Свойство игнорируется при включении `SKPaint` объекта в `DrawBitmap` вызов. Свойство объекта можно использовать `Color` `SKPaint` для установки уровня прозрачности для отображения точечного рисунка (как описано в статье, [отображающей SkiaSharp растровые изображения](../../bitmaps/displaying.md#displaying-in-pixel-dimensions)), но нельзя использовать `Shader` свойство для отображения растрового изображения с градиентной прозрачностью. Для отображения растровых изображений с прозрачными градиентами можно использовать другие методы. они описаны в статьях [SkiaSharp циклические градиенты](circular-gradients.md#radial-gradients-for-masking) и [SkiaSharp компоновки и режимы смешения](../blend-modes/porter-duff.md#gradient-transparency-and-transitions).

## <a name="corner-to-corner-gradients"></a>Поугловой градиент

Часто линейный градиент распространяется от одного угла прямоугольника к другому. Если начальная точка находится в левом верхнем углу прямоугольника, градиент может расшириться следующим образом:

- вертикально в левый нижний угол
- по горизонтали в правый верхний угол
- по диагонали в правый нижний угол

Диагональный линейный градиент показан на первой странице в разделе **SkiaSharp шейдеры и другие эффекты** в примере [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) . Страница **градиента в угловом** углу создает `SKCanvasView` в своем конструкторе. `PaintSurface`Обработчик создает `SKPaint` объект в `using` операторе, а затем определяет прямоугольник размером 300 пикселей в центре холста:

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    ···
    public CornerToCornerGradientPage ()
    {
        Title = "Corner-to-Corner Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ···
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Create 300-pixel square centered rectangle
            float x = (info.Width - 300) / 2;
            float y = (info.Height - 300) / 2;
            SKRect rect = new SKRect(x, y, x + 300, y + 300);

            // Create linear gradient from upper-left to lower-right
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { SKColors.Red, SKColors.Blue },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Repeat);

            // Draw the gradient on the rectangle
            canvas.DrawRect(rect, paint);
            ···
        }
    }
}
```

`Shader`Свойству `SKPaint` присваивается `SKShader` возвращаемое значение из статического `SKShader.CreateLinearGradient` метода. Ниже приведены пять аргументов.

- Начальная точка градиента, заданная здесь в левом верхнем углу прямоугольника
- Конечная точка градиента, установленная здесь в нижний правый угол прямоугольника
- Массив из двух или более цветов, которые вносят вклад в градиент.
- Массив `float` значений, указывающий относительное расположение цветов в линии градиента
- Элемент [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) перечисления, указывающий, как градиент ведет себя за концом линии градиента

После создания объекта градиента `DrawRect` метод рисует прямоугольник размером 300 пикселей с помощью `SKPaint` объекта, включающего шейдер. Здесь он работает в iOS, Android и универсальная платформа Windows (UWP):

[![Градиентный угол в углу](linear-gradient-images/CornerToCornerGradient.png "Градиентный угол в углу")](linear-gradient-images/CornerToCornerGradient-Large.png#lightbox)

Линия градиента определяется двумя точками, указанными в качестве первых двух аргументов. Обратите внимание, что эти точки находятся относительно _холста_ , а _не_ для графического объекта, отображаемого с градиентом. Вдоль линии градиента цвет постепенно переводится от красного в верхнем левом углу к синему правому нижнему углу. Любая строка, перпендикулярная линии градиента, имеет постоянный цвет.

Массив значений, `float` указанных в качестве четвертого аргумента, имеет корреспонденцию "один к одному" с массивом цветов. Значения обозначают относительное расположение вдоль линии градиента, в которой находятся эти цвета. Здесь 0 означает, что `Red` происходит в начале линии градиента, а 1 означает, что `Blue` в конце строки. Числа должны быть по возрастанию и находиться в диапазоне от 0 до 1. Если они не находятся в этом диапазоне, они будут скорректированы в этом диапазоне.

Двум значениям в массиве можно присвоить значение, отличное от 0 до 1. Попробуйте выполнить следующий код:

```csharp
new float[] { 0.25f, 0.75f }
```

Теперь весь первый квартал линии градиента имеет чистый красный цвет, а последний квартал — чисто синий. Сочетание красного и синего цветов ограничено Центральной половиной линии градиента.

Как правило, эти значения должно быть в равном интервале от 0 до 1. Если это так, можно просто указать в `null` качестве четвертого аргумента `CreateLinearGradient` .

Хотя этот градиент определен между двумя углами квадратного прямоугольника размером 300 пикселей, он не ограничивается заполнением этого прямоугольника. Страница **градиента в угловых** скобках содержит дополнительный код, реагирующий на касания или щелчки мышью на странице. `drawBackground`Поле переключается между `true` и `false` при каждом касании. Если значение равно `true` , то `PaintSurface` обработчик использует тот же `SKPaint` объект для заполнения всего холста, а затем рисует черный прямоугольник, указывающий на меньший прямоугольник: 

```csharp
public class CornerToCornerGradientPage : ContentPage
{
    bool drawBackground;

    public CornerToCornerGradientPage ()
    {
        ···
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            drawBackground ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPaint paint = new SKPaint())
        {
            ···
            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                canvas.DrawRect(info.Rect, paint);

                // Outline the smaller rectangle
                paint.Shader = null;
                paint.Style = SKPaintStyle.Stroke;
                paint.Color = SKColors.Black;
                canvas.DrawRect(rect, paint);
            }
        }
    }
}
```

Вот что вы увидите после касания экрана:

[![Заполнение градиента в угловом углу](linear-gradient-images/CornerToCornerGradientFull.png "Заполнение градиента в угловом углу")](linear-gradient-images/CornerToCornerGradientFull-Large.png#lightbox)

Обратите внимание, что градиент повторяется в том же шаблоне за пределами точек, определяющих линию градиента. Это повторение происходит, так как последний аргумент `CreateLinearGradient` — `SKShaderTileMode.Repeat` . (В ближайшее время вы увидите другие варианты.)

Также обратите внимание, что точки, используемые для указания линии градиента, не являются уникальными. Линии, перпендикулярные линии градиента, имеют одинаковый цвет, поэтому для одного и того же результата можно указать бесконечное количество линий градиента. Например, при заполнении прямоугольника горизонтальным градиентом можно указать верхний левый и верхний правый угол, либо нижний левый и нижний правый угол или любые две точки, которые находятся в разных строках и параллельно.

## <a name="interactively-experiment"></a>Интерактивный эксперимент

Вы можете интерактивно экспериментировать с линейными градиентами с помощью страницы « **интерактивная линейный градиент** ». На этой странице используется `InteractivePage` класс, представленный в статье [**три способа рисования дуги**](../../curves/arcs.md). `InteractivePage` обрабатывает [`TouchEffect`](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md) события, чтобы поддерживать коллекцию `TouchPoint` объектов, которые можно перемещать с помощью пальца или мыши.

XAML-файл присоединяет `TouchEffect` к родителю элемента, `SKCanvasView` а также включает в себя `Picker` , который позволяет выбрать один из трех членов [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) перечисления:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.InteractiveLinearGradientPage"
                       Title="Interactive Linear Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid BackgroundColor="White"
              Grid.Row="0">
            <skiaforms:SKCanvasView x:Name="canvasView"
                                    PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Picker x:Name="tileModePicker" 
                Grid.Row="1"
                Title="Shader Tile Mode" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKShaderTileMode}">
                    <x:Static Member="skia:SKShaderTileMode.Clamp" />
                    <x:Static Member="skia:SKShaderTileMode.Repeat" />
                    <x:Static Member="skia:SKShaderTileMode.Mirror" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>
    </Grid>
</local:InteractivePage>
```

Конструктор в файле кода программной части создает два `TouchPoint` объекта для начальной и конечной точек линейного градиента. `PaintSurface`Обработчик определяет массив из трех цветов (для градиента от красного к зеленому и синего) и получает текущий объект `SKShaderTileMode` из `Picker` :

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    public InteractiveLinearGradientPage ()
    {
        InitializeComponent ();

        touchPoints = new TouchPoint[2];

        for (int i = 0; i < 2; i++)
        { 
            touchPoints[i] = new TouchPoint
            {
                Center = new SKPoint(100 + i * 200, 100 + i * 200)
            };
        }

        InitializeComponent();
        baseCanvasView = canvasView;
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

        SKColor[] colors = { SKColors.Red, SKColors.Green, SKColors.Blue };
        SKShaderTileMode tileMode =
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ?
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateLinearGradient(touchPoints[0].Center,
                                                         touchPoints[1].Center,
                                                         colors,
                                                         null,
                                                         tileMode);
            canvas.DrawRect(info.Rect, paint);
        }
        ···
    }
}
```

`PaintSurface`Обработчик создает `SKShader` объект на основе всех этих данных и использует его для выделения цветом всего холста. Массив `float` значений имеет значение `null` . В противном случае для одинакового пространства, равного трем цветам, параметру необходимо присвоить массиву значения 0, 0,5 и 1.

Основная часть `PaintSurface` обработчика предназначена для отображения нескольких объектов: сенсорные точки в виде контуров, линии градиента и линий, перпендикулярных линиям градиента в сенсорных точках:

```csharp
public partial class InteractiveLinearGradientPage : InteractivePage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Display the touch points here rather than by TouchPoint
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;

            foreach (TouchPoint touchPoint in touchPoints)
            {
                canvas.DrawCircle(touchPoint.Center, touchPoint.Radius, paint);
            }

            // Draw gradient line connecting touchpoints
            canvas.DrawLine(touchPoints[0].Center, touchPoints[1].Center, paint);

            // Draw lines perpendicular to the gradient line
            SKPoint vector = touchPoints[1].Center - touchPoints[0].Center;
            float length = (float)Math.Sqrt(Math.Pow(vector.X, 2) +
                                            Math.Pow(vector.Y, 2));
            vector.X /= length;
            vector.Y /= length;
            SKPoint rotate90 = new SKPoint(-vector.Y, vector.X);
            rotate90.X *= 200;
            rotate90.Y *= 200;

            canvas.DrawLine(touchPoints[0].Center, 
                            touchPoints[0].Center + rotate90, 
                            paint);

            canvas.DrawLine(touchPoints[0].Center,
                            touchPoints[0].Center - rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center + rotate90,
                            paint);

            canvas.DrawLine(touchPoints[1].Center,
                            touchPoints[1].Center - rotate90,
                            paint);
        }
    }
}
```

Линию градиента, соединяющую два таучпоинтс, легко изображать, но перпендикулярные строки нуждаются в некоторой дополнительной работе. Линия градиента преобразуется в вектор, нормализованная с длиной в одну единицу, а затем поворачивается на 90 градусов. Затем для этого вектора задается длина в 200 пикселей. Он используется для рисования четырех линий, которые расширяются из точек касания, перпендикулярны линии градиента.

Перпендикулярные линии совпадают с началом и концом градиента. То, что происходит за пределами этих строк, зависит от значения `SKShaderTileMode` перечисления.

[![Интерактивный линейный градиент](linear-gradient-images/InteractiveLinearGradient.png "Интерактивный линейный градиент")](linear-gradient-images/InteractiveLinearGradient-Large.png#lightbox)

На трех снимках экрана показаны результаты трех различных значений [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) . На снимке экрана iOS показано `SKShaderTileMode.Clamp` , что просто расширяет цвета на границе градиента. `SKShaderTileMode.Repeat`Параметр на снимке экрана Android показывает, как повторяется шаблон градиента. `SKShaderTileMode.Mirror`Параметр на снимке экрана UWP также повторяет шаблон, но шаблон отменяется каждый раз, что приводит к ненепрерывности цветов.

## <a name="gradients-on-gradients"></a>Градиенты на градиентах

`SKShader`Класс не определяет открытые свойства или методы, кроме `Dispose` . `SKShader`Поэтому объекты, созданные статическими методами, являются неизменяемыми. Даже при использовании одного градиента для двух различных объектов, скорее всего, потребуется немного изменить градиент. Для этого необходимо создать новый `SKShader` объект.

На странице **текста градиента** отображается текст и фоновом, которые окрашены с одинаковыми градиентами:

[![Текст градиента](linear-gradient-images/GradientText.png "Текст градиента")](linear-gradient-images/GradientText-Large.png#lightbox)

Единственными различиями в градиентах являются начальная и конечная точки. Градиент, используемый для отображения текста, основан на двух точках в углах ограничивающего прямоугольника для текста. Для фона две точки основаны на всем холсте. Вот этот код:

```csharp
public class GradientTextPage : ContentPage
{
    const string TEXT = "GRADIENT";

    public GradientTextPage ()
    {
        Title = "Gradient Text";

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
            // Create gradient for background
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                new SKPoint(info.Width, info.Height),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw background
            canvas.DrawRect(info.Rect, paint);

            // Set TextSize to fill 90% of width
            paint.TextSize = 100;
            float width = paint.MeasureText(TEXT);
            float scale = 0.9f * info.Width / width;
            paint.TextSize *= scale;

            // Get text bounds
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Shift textBounds by that amount
            textBounds.Offset(xText, yText);

            // Create gradient for text
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(textBounds.Left, textBounds.Top),
                                new SKPoint(textBounds.Right, textBounds.Bottom),
                                new SKColor[] { new SKColor(0x40, 0x40, 0x40),
                                                new SKColor(0xC0, 0xC0, 0xC0) },
                                null,
                                SKShaderTileMode.Clamp);

            // Draw text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

`Shader`Свойство `SKPaint` объекта сначала устанавливается для вывода градиента, охватывающего фон. Точки градиента задаются в левом верхнем и нижнем правом углу холста.

Код задает `TextSize` свойство `SKPaint` объекта таким образом, чтобы текст отображался в 90% от ширины холста. Границы текста используются для вычисления `xText` `yText` значений и для передачи в `DrawText` метод для центрирования текста.

Однако точки градиента для второго `CreateLinearGradient` вызова должны ссылаться на верхний левый и правый нижний угол текста относительно холста при его отображении. Это достигается путем сдвига `textBounds` прямоугольника на те же `xText` `yText` значения и.

```csharp
textBounds.Offset(xText, yText);
```

Теперь можно использовать верхний левый и нижний правый угол прямоугольника, чтобы задать начальную и конечную точки градиента.

## <a name="animating-a-gradient"></a>Анимация градиента

Существует несколько способов анимации градиента. Один из подходов состоит в том, чтобы анимировать начальную и конечную точки. Страница « **анимация градиента** » перемещает две точки вокруг окружности, расположенной по центру холста. Радиус этого круга составляет половину ширины или высоты холста, в зависимости от того, что меньше. Начальная и конечная точки отменяются друг от друга в этом круге, а градиент переходит от белого к черному в `Mirror` режиме плитки:

[![Градиентная анимация](linear-gradient-images/GradientAnimation.png "Градиентная анимация")](linear-gradient-images/GradientAnimation-Large.png#lightbox)

Конструктор создает `SKCanvasView` . `OnAppearing`Методы и `OnDisappearing` обработают логику анимации:

```csharp
public class GradientAnimationPage : ContentPage
{
    SKCanvasView canvasView;
    bool isAnimating;
    double angle;
    Stopwatch stopwatch = new Stopwatch();

    public GradientAnimationPage()
    {
        Title = "Gradient Animation";

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
        const int duration = 3000;
        angle = 2 * Math.PI * (stopwatch.ElapsedMilliseconds % duration) / duration;
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

`OnTimerTick`Метод вычисляет `angle` значение, которое анимируется от 0 до 2π каждые 3 секунды. 

Вот один из способов вычисления двух точек градиента. `SKPoint`Значение с именем `vector` вычисляется для расширения от центра холста до точки на радиусе круга. Направление этого вектора основано на значениях синуса и косинуса угла. Затем вычисляется две противоположные точки градиента: одна точка вычисляется вычитанием этого вектора из центральной точки, а другая точка вычисляется путем добавления вектора в центральную точку:

```csharp
public class GradientAnimationPage : ContentPage
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
            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
            int radius = Math.Min(info.Width, info.Height) / 2;
            SKPoint vector = new SKPoint((float)(radius * Math.Cos(angle)),
                                         (float)(radius * Math.Sin(angle)));

            paint.Shader = SKShader.CreateLinearGradient(
                                center - vector,
                                center + vector,
                                new SKColor[] { SKColors.White, SKColors.Black },
                                null,
                                SKShaderTileMode.Mirror);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Несколько других подходов требуют меньшего объема кода. При таком подходе используется [`SKShader.CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient(SkiaSharp.SKPoint,SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix)) метод перегрузки с матричным преобразованием в качестве последнего аргумента. Этот подход является версией в примере [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) :

```csharp
public class GradientAnimationPage : ContentPage
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
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(0, 0),
                                info.Width < info.Height ? new SKPoint(info.Width, 0) : 
                                                           new SKPoint(0, info.Height),
                                new SKColor[] { SKColors.White, SKColors.Black },
                                new float[] { 0, 1 },
                                SKShaderTileMode.Mirror,
                                SKMatrix.MakeRotation((float)angle, info.Rect.MidX, info.Rect.MidY));

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Если ширина холста меньше, чем высота, то для двух точек градиента устанавливается значение (0, 0) и ( `info.Width` , 0). Преобразование «Вращение» передается в качестве последнего аргумента для `CreateLinearGradient` эффективного поворота этих двух точек вокруг центра экрана.

Обратите внимание, что если угол равен 0, то поворот не выполняется, а две точки градиента — верхний левый и правый верхний угол холста. Эти точки не имеют одинаковых точек градиента, как показано в предыдущем `CreateLinearGradient` вызове. Но эти точки _параллельны_ линии горизонтального градиента, бисектс центр холста, и они приводят к идентичному градиенту.

**Градиент Радуга**

Страница **градиента Радуга** рисует Радуга от левого верхнего угла холста до правого нижнего угла. Но этот градиент не похож на реальную. Он является прямым, а не изогнутым, но основан на восьми цветах HSL (оттенок-насыщенность), которые определяются циклическими значениями оттенков от 0 до 360:

```csharp
SKColor[] colors = new SKColor[8];

for (int i = 0; i < colors.Length; i++)
{
    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
}
```

Этот код является частью `PaintSurface` обработчика, показанного ниже. Обработчик начинается с создания пути, определяющего шесть многоугольников, которые выходят из левого верхнего угла холста в правый нижний угол:

```csharp
public class RainbowGradientPage : ContentPage
{
    public RainbowGradientPage ()
    {
        Title = "Rainbow Gradient";

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

        using (SKPath path = new SKPath())
        {
            float rainbowWidth = Math.Min(info.Width, info.Height) / 2f;

            // Create path from upper-left to lower-right corner
            path.MoveTo(0, 0);
            path.LineTo(rainbowWidth / 2, 0);
            path.LineTo(info.Width, info.Height - rainbowWidth / 2);
            path.LineTo(info.Width, info.Height);
            path.LineTo(info.Width - rainbowWidth / 2, info.Height);
            path.LineTo(0, rainbowWidth / 2);
            path.Close();

            using (SKPaint paint = new SKPaint())
            {
                SKColor[] colors = new SKColor[8];

                for (int i = 0; i < colors.Length; i++)
                {
                    colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
                }

                paint.Shader = SKShader.CreateLinearGradient(
                                    new SKPoint(0, rainbowWidth / 2), 
                                    new SKPoint(rainbowWidth / 2, 0),
                                    colors,
                                    null,
                                    SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

Две точки градиента в `CreateLinearGradient` методе основаны на двух точках, определяющих этот контур: обе точки находятся ближе к левому верхнему углу. Первый — на верхнем крае холста, а второй — на левом крае холста. Ниже приведен результат:

[![Сбой градиента Радуга](linear-gradient-images/RainbowGradientFaulty.png "Сбой градиента Радуга")](linear-gradient-images/RainbowGradientFaulty-Large.png#lightbox)

Это интересный образ, но это не совсем цель. Проблема заключается в том, что при создании линейного градиента линии постоянного цвета перпендикулярны линии градиента. Линия градиента основана на точках, в которых фигура касается верхней и левой сторон, и эта линия обычно не перпендикулярна краям фигуры, которая распространяется на нижний правый угол. Этот подход будет работать только в том случае, если холст был квадратным.

Чтобы создать правильный градиент Радуга, линия градиента должна быть перпендикулярна границе Радуга. Это более сложный расчет. Вектор должен быть определен параллельно с длинной стороной рисунка. Вектор поворачивается на 90 градусов, так что он перпендикулярен этой стороне. Затем она становится шириной фигуры, умноженной на `rainbowWidth` . Две точки градиента рассчитываются на основе точки на стороне фигуры, а также с точки зрения вектора. Ниже приведен код, отображаемый на странице " **градиентная шкала** " в образце [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) :

```csharp
public class RainbowGradientPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPath path = new SKPath())
        {
            ···
            using (SKPaint paint = new SKPaint())
            {
                ···
                // Vector on lower-left edge, from top to bottom 
                SKPoint edgeVector = new SKPoint(info.Width - rainbowWidth / 2, info.Height) - 
                                     new SKPoint(0, rainbowWidth / 2);

                // Rotate 90 degrees counter-clockwise:
                SKPoint gradientVector = new SKPoint(edgeVector.Y, -edgeVector.X);

                // Normalize
                float length = (float)Math.Sqrt(Math.Pow(gradientVector.X, 2) +
                                                Math.Pow(gradientVector.Y, 2));
                gradientVector.X /= length;
                gradientVector.Y /= length;

                // Make it the width of the rainbow
                gradientVector.X *= rainbowWidth;
                gradientVector.Y *= rainbowWidth;

                // Calculate the two points
                SKPoint point1 = new SKPoint(0, rainbowWidth / 2);
                SKPoint point2 = point1 + gradientVector;

                paint.Shader = SKShader.CreateLinearGradient(point1,
                                                             point2,
                                                             colors,
                                                             null,
                                                             SKShaderTileMode.Repeat);

                canvas.DrawPath(path, paint);
            }
        }
    }
}
```

Теперь цвета Радуга вычисляются вместе с фигурой:

[![Градиент Радуга](linear-gradient-images/RainbowGradient.png "Градиент Радуга")](linear-gradient-images/RainbowGradient-Large.png#lightbox)

**Цвета бесконечности**

Градиент Радуга также используется на странице **цвета бесконечности** . На этой странице рисуется знак бесконечности с использованием объекта пути, описанного в статье [**три типа кривых Безье**](../../curves/beziers.md#bezier-curve-approximation-to-circular-arcs). Затем изображение окрашивается с помощью анимированного градиента Радуга, который постоянно вращается на изображении.

Конструктор создает объект, `SKPath` описывающий знак бесконечности. После создания пути конструктор также может получить прямоугольные границы пути. Затем вычисляется значение с именем `gradientCycleLength` . Если градиент основан на верхнем левом и нижнем правом углу `pathBounds` прямоугольника, это `gradientCycleLength` значение равно общей горизонтальной ширине шаблона градиента:

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    SKCanvasView canvasView;

    // Path information 
    SKPath infinityPath;
    SKRect pathBounds;
    float gradientCycleLength;

    // Gradient information
    SKColor[] colors = new SKColor[8];
    ···

    public InfinityColorsPage ()
    {
        Title = "Infinity Colors";

        // Create path for infinity sign
        infinityPath = new SKPath();
        infinityPath.MoveTo(0, 0);                                  // Center
        infinityPath.CubicTo(  50,  -50,   95, -100,  150, -100);   // To top of right loop
        infinityPath.CubicTo( 205, -100,  250,  -55,  250,    0);   // To far right of right loop
        infinityPath.CubicTo( 250,   55,  205,  100,  150,  100);   // To bottom of right loop
        infinityPath.CubicTo(  95,  100,   50,   50,    0,    0);   // Back to center  
        infinityPath.CubicTo( -50,  -50,  -95, -100, -150, -100);   // To top of left loop
        infinityPath.CubicTo(-205, -100, -250,  -55, -250,    0);   // To far left of left loop
        infinityPath.CubicTo(-250,   55, -205,  100, -150,  100);   // To bottom of left loop
        infinityPath.CubicTo( -95,  100, - 50,   50,    0,    0);   // Back to center
        infinityPath.Close();

        // Calculate path information 
        pathBounds = infinityPath.Bounds;
        gradientCycleLength = pathBounds.Width +
            pathBounds.Height * pathBounds.Height / pathBounds.Width;

        // Create SKColor array for gradient
        for (int i = 0; i < colors.Length; i++)
        {
            colors[i] = SKColor.FromHsl(i * 360f / (colors.Length - 1), 100, 50);
        }

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

Конструктор также создает `colors` массив для Радуга и `SKCanvasView` объект.

Переопределения `OnAppearing` `OnDisappearing` методов и выполняют дополнительную нагрузку на анимацию. `OnTimerTick`Метод анимируется `offset` поле от 0 до `gradientCycleLength` каждых двух секунд:

```csharp
public class InfinityColorsPage : ContentPage
{
    ···
    // For animation
    bool isAnimating;
    float offset;
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
        const int duration = 2;     // seconds
        double progress = stopwatch.Elapsed.TotalSeconds % duration / duration;
        offset = (float)(gradientCycleLength * progress);
        canvasView.InvalidateSurface();

        return isAnimating;
    }
    ···
}
```

Наконец, `PaintSurface` обработчик отображает знак бесконечности. Поскольку контур содержит отрицательные и положительные координаты вокруг центральной точки (0, 0), `Translate` для сдвига на холсте используется преобразование в центре. За преобразованием преобразования следует `Scale` Преобразование, которое применяет коэффициент масштабирования, который делает знак бесконечности максимально большим, при этом продолжая оставаться в пределах 95% ширины и высоты холста. 

Обратите внимание, что `STROKE_WIDTH` константа добавляется к ширине и высоте ограничивающего прямоугольника контура. Контур будет обработано линией этой ширины, поэтому размер отображаемого размера бесконечности увеличивается на половину этой ширины на всех четырех сторонах:

```csharp
public class InfinityColorsPage : ContentPage
{
    const int STROKE_WIDTH = 50;
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transforms to shift path to center and scale to canvas size
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.95f * 
            Math.Min(info.Width / (pathBounds.Width + STROKE_WIDTH),
                     info.Height / (pathBounds.Height + STROKE_WIDTH)));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = STROKE_WIDTH;
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(pathBounds.Left, pathBounds.Top),
                                new SKPoint(pathBounds.Right, pathBounds.Bottom),
                                colors,
                                null,
                                SKShaderTileMode.Repeat,
                                SKMatrix.MakeTranslation(offset, 0));

            canvas.DrawPath(infinityPath, paint);
        }
    }
}
```

Взгляните на точки, передаваемые в качестве первых двух аргументов `SKShader.CreateLinearGradient` . Эти точки основаны на прямоугольнике, ограничивающем исходный контур. Первая точка — ( &ndash; 250, &ndash; 100), а вторая — (250, 100). Внутри SkiaSharp эти точки подчиняются текущему преобразованию холста, поэтому они правильно соответствуют отображаемому знаку бесконечности.

Если последний аргумент не равен `CreateLinearGradient` , то вы увидите градиент Радуга, который находится в левом верхнем углу знака бесконечности в нижнем правом углу. (Фактически градиент распространяется от левого верхнего угла к правому нижнему углу ограничивающего прямоугольника. Отображаемый знак бесконечности больше, чем ограничивающий прямоугольник, на половину `STROKE_WIDTH` значения на всех сторонах. Так как градиент выделяется красным цветом как в начале, так и в конце, а градиент создается с помощью `SKShaderTileMode.Repeat` , разница не заметна.)

Если последний аргумент имеет значение `CreateLinearGradient` , шаблон градиента непрерывно вращается по изображению:

[![Цвета бесконечности](linear-gradient-images/InfinityColors.png "Цвета бесконечности")](linear-gradient-images/InfinityColors-Large.png#lightbox)

## <a name="transparency-and-gradients"></a>Прозрачность и градиенты

Цвета, влияющие на градиент, могут включать прозрачность. Вместо градиента, который постепенно вращается от одного цвета к другому, градиент может постепенно покрасить цвет в прозрачный. 

Этот метод можно использовать для некоторых интересных эффектов. В одном из классических примеров показан графический объект с его отражением:

[![Градиентное отражение](linear-gradient-images/ReflectionGradient.png "Градиентное отражение")](linear-gradient-images/ReflectionGradient-Large.png#lightbox)

Перевернутый текст окрашивается градиентом, который находится на 50% прозрачно в верхней части до полного прозрачности внизу. Эти уровни прозрачности связаны с альфа-значениями 0x80 и 0.

`PaintSurface`Обработчик на странице **градиента отражения** изменяет размер текста до 90% ширины холста. Затем он вычисляет `xText` значения и, `yText` чтобы разместить текст горизонтально по центру, но находящийся на базовом плане, соответствующем вертикальной центральной части страницы:

```csharp
public class ReflectionGradientPage : ContentPage
{
    const string TEXT = "Reflection";

    public ReflectionGradientPage ()
    {
        Title = "Reflection Gradient";

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

            // Scale the canvas to flip upside-down around the vertical center
            canvas.Scale(1, -1, 0, yText);

            // Draw reflected text
            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Эти `xText` `yText` значения и имеют те же значения, которые используются для отображения отраженного текста в `DrawText` вызове в нижней части `PaintSurface` обработчика. Однако перед этим кодом вы увидите вызов `Scale` метода `SKCanvas` . Этот `Scale` метод масштабируется по горизонтали на 1 (что не делает ничего), но вертикально на &ndash; 1, что фактически переворачивает все перевернутые элементы. Центром вращения задается точка (0, `yText` ), где `yText` — вертикальный центр холста, первоначально вычисленный как `info.Height` разделенный на 2.

Помните, что СКИА использует градиент для цветовых графических объектов перед преобразованием холста. После прорисовки неотраженного текста `textBounds` прямоугольник смещается так, чтобы он соответствовал отображаемому тексту:

```csharp
textBounds.Offset(xText, yText);
```

`CreateLinearGradient`Вызов определяет градиент от верхнего края этого прямоугольника до конца. Градиент находится от полностью прозрачного синего ( `paint.Color.WithAlpha(0)` ) до 50% прозрачного синего ( `paint.Color.WithAlpha(0x80)` ). Преобразование Canvas переворачивает текст в перевернутом виде, поэтому 50% прозрачное синее начинается с базовой линии и превращается в верхнюю часть текста.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)