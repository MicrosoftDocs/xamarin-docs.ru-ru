---
title: Круговые градиенты SkiaSharp
description: Сведения о различных типах градиентов на основе кругов.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 400AE23A-6A0B-4FA8-BD6B-DE4146B04732
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ec84ac906ac146f37ba5b161a898582ce483bc95
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556676"
---
# <a name="the-skiasharp-circular-gradients"></a>Круговые градиенты SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[`SKShader`](xref:SkiaSharp.SKShader)Класс определяет статические методы для создания четырех различных типов градиентов. В статье о [**линейном градиенте SkiaSharp**](linear-gradient.md) обсуждается [`CreateLinearGradient`](xref:SkiaSharp.SKShader.CreateLinearGradient*) метод. В этой статье описываются другие три типа градиентов, все из которых основаны на кругах.

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient*)Метод создает градиент, основывается от центра окружности:

![Пример радиального градиента](circular-gradients-images/RadialGradientSample.png)

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient*)Метод создает градиент, который вращается вокруг центра окружности:

![Образец градиента очистки](circular-gradients-images/SweepGradientSample.png)

Третий тип градиента довольно необычен. Он называется градиентом двух точек и определяется [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient*) методом. Градиентный цвет расширяется с одного круга на другой:

![Пример градиента с применением конусов](circular-gradients-images/ConicalGradientSample.png)

Если два круга имеют разный размер, то градиент принимает форму конуса.

В этой статье более подробно рассматриваются эти градиенты.

## <a name="the-radial-gradient"></a>Радиальный градиент

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))Метод имеет следующий синтаксис:

```csharp
public static SKShader CreateRadialGradient (SKPoint center, 
                                             Single radius, 
                                             SKColor[] colors, 
                                             Single[] colorPos, 
                                             SKShaderTileMode mode)
```

[`CreateRadialGradient`](xref:SkiaSharp.SKShader.CreateRadialGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))Перегрузка также включает параметр матрицы преобразования.

Первые два аргумента задают центр окружности и радиуса. Градиент начинается в этом центре и распространяется наружу на `radius` Пиксели. Что происходит вне `radius` зависимости от [`SKShaderTileMode`](xref:SkiaSharp.SKShaderTileMode) аргумента. `colors`Параметр является массивом из двух или более цветов (как в методах линейного градиента) и `colorPos` является массивом целых чисел в диапазоне от 0 до 1. Эти целые числа указывают относительное положение цветов вдоль этой `radius` линии. Можно задать для этого аргумента `null` одинаковое пространство цветов.

При использовании `CreateRadialGradient` для заполнения окружности можно задать центр градиента по центру окружности, а радиус градиента — радиусу круга. В этом случае `SKShaderTileMode` аргумент не влияет на отрисовку градиента. Но если область, заполненная градиентом, больше круга, определенного градиентом, то `SKShaderTileMode` аргумент имеет более глубокое воздействие на то, что происходит вне круга.

Результат `SKShaderMode` показан на странице **радиальный градиент** в примере [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) . XAML-файл для этой страницы создает экземпляр `Picker` , который позволяет выбрать один из трех членов `SKShaderTileMode` перечисления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.RadialGradientPage"
             Title="Radial Gradient">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="0"
                                PaintSurface="OnCanvasViewPaintSurface" />

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
</ContentPage>
```

В файле кода программной части весь холст выделяется радиальным градиентом. Центр градиента устанавливается в центре холста, а радиус — на 100 пикселей. Градиент состоит только из двух цветов: черного и белого.

```csharp
public partial class RadialGradientPage : ContentPage
{
    public RadialGradientPage ()
    {
        InitializeComponent ();
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

        SKShaderTileMode tileMode =
            (SKShaderTileMode)(tileModePicker.SelectedIndex == -1 ?
                                        0 : tileModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                new SKPoint(info.Rect.MidX, info.Rect.MidY),
                                100,
                                new SKColor[] { SKColors.Black, SKColors.White },
                                null,
                                tileMode);

            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Этот код создает градиент с черным центром в центре, постепенно плавное к белому 100 пикселам от центра. Что происходит за пределами радиуса, зависит от `SKShaderTileMode` аргумента:

[![Радиальный градиент](circular-gradients-images/RadialGradient.png "Радиальный градиент")](circular-gradients-images/RadialGradient-Large.png#lightbox)

Во всех трех случаях градиент заполняет холст. На экране iOS в левой части градиент, отличный от радиуса, переходит к последнему цвету, который является белым. Это результат `SKShaderTileMode.Clamp` . Экран Android показывает результат `SKShaderTileMode.Repeat` : с 100 пикселей от центра, градиент начинается еще раз с первым цветом, который является черным. Градиент повторяется каждые 100 пикселей RADIUS. 

На экране универсальная платформа Windows справа показано, как `SKShaderTileMode.Mirror` порождает градиенты в альтернативные направления. Первый градиент находится от черного в центре к белому по радиусу 100 пикселей. Далее будет белый цвет от радиуса 100 пикселя до черного с 200-пиксельным радиусом, а следующий градиент снова будет реверсирован.

В радиальном градиенте можно использовать более двух цветов. Образец **градиента дуги на Радуга** создает массив из восьми цветов, соответствующих цветам Радуга и заканчивая красным, а также массивом из восьми значений позиции:

```csharp
public class RainbowArcGradientPage : ContentPage
{
    public RainbowArcGradientPage ()
    {
        Title = "Rainbow Arc Gradient";

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
            float rainbowWidth = Math.Min(info.Width, info.Height) / 4f;

            // Center of arc and gradient is lower-right corner
            SKPoint center = new SKPoint(info.Width, info.Height);

            // Find outer, inner, and middle radius
            float outerRadius = Math.Min(info.Width, info.Height);
            float innerRadius = outerRadius - rainbowWidth;
            float radius = outerRadius - rainbowWidth / 2;

            // Calculate the colors and positions
            SKColor[] colors = new SKColor[8];
            float[] positions = new float[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
                positions[i] = (i + (7f - i) * innerRadius / outerRadius) / 7f;
            }

            // Create sweep gradient based on center and outer radius
            paint.Shader = SKShader.CreateRadialGradient(center, 
                                                         outerRadius, 
                                                         colors, 
                                                         positions, 
                                                         SKShaderTileMode.Clamp);
            // Draw a circle with a wide line
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = rainbowWidth;

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

Предположим, что минимальная ширина и высота холста — 1000, то есть `rainbowWidth` значение равно 250. Значения и задаются `outerRadius` `innerRadius` равными 1000 и 750 соответственно. Эти значения используются для вычисления `positions` массива; восемь значений находятся в диапазоне от 0,75 f до 1. `radius`Значение используется для обводки круга. Значение 875 означает, что ширина штриха 250 пикселя расширяется между радиусом 750 пикселей и радиусом для 1000 пикселей:

[![Градиент дуги (Радуга)](circular-gradients-images/RainbowArcGradient.png "Градиент дуги (Радуга)")](circular-gradients-images/RainbowArcGradient-Large.png#lightbox)

Если вы заполнили весь холст с помощью этого градиента, вы увидите, что он красный в пределах внутреннего радиуса. Это происходит потому, что `positions` массив не начинается с 0. Первый цвет используется для смещений 0 по первому значению массива. Градиент также выделяется красным за пределами внешнего радиуса. Это результат `Clamp` режима плитки. Так как градиент используется для обводки жирной линии, эти красные области не видны.

## <a name="radial-gradients-for-masking"></a>Радиальные градиенты для маскирования

Как и линейные градиенты, радиальные градиенты могут включать прозрачные или частично прозрачные цвета. Эта функция полезна для процесса, называемого _маскированием_, который скрывает часть изображения для акцентирования другой части изображения.

Пример показан на странице **маски радиального градиента** . Программа загружает один из битовых рисунков ресурсов. `CENTER`Поля и `RADIUS` были определены с помощью проверки растрового изображения и ссылаются на область, которую следует выделить. `PaintSurface`Обработчик начинает с вычисления прямоугольника для отображения точечного рисунка, а затем отображает его в этом прямоугольнике:

```csharp
public class RadialGradientMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(RadialGradientMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public RadialGradientMaskPage ()
    {
        Title = "Radial Gradient Mask";

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

        // Find rectangle to display bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width,
                                (float)info.Height / bitmap.Height);

        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);

        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap in rectangle
        canvas.DrawBitmap(bitmap, rect);

        // Adjust center and radius for scaled and offset bitmap
        SKPoint center = new SKPoint(scale * CENTER.X + x,
                                     scale * CENTER.Y + y);
        float radius = scale * RADIUS;

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                center,
                                radius,
                                new SKColor[] { SKColors.Transparent,
                                                SKColors.White },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            // Display rectangle using that gradient
            canvas.DrawRect(rect, paint);
        }
    }
}
```

После рисования точечного рисунка часть простого кода преобразуется в `CENTER` `RADIUS` `center` и `radius` , который ссылается на выделенную область точечного рисунка, которая была масштабирована и смещена для отображения. Эти значения используются для создания радиального градиента с этим центром и радиусом. Два цвета начинаются с прозрачности в центре и для первого 60% радиуса. Затем градиент постепенно переходит к белому цвету:

[![Маска радиального градиента](circular-gradients-images/RadialGradientMask.png "Маска радиального градиента")](circular-gradients-images/RadialGradientMask-Large.png#lightbox)

Этот подход не является лучшим способом маскирования растрового изображения. Проблема заключается в том, что маска в основном имеет цвет белого цвета, который был выбран в соответствии с фоном холста. Если фон является другим цветом &mdash; или, возможно, &mdash; он не соответствует самому градиенту. Лучший подход к маскировке показан в статье [SkiaSharp Портер-Дуфф Blend modes](../blend-modes/porter-duff.md).

## <a name="radial-gradients-for-specular-highlights"></a>Радиальные градиенты для отраженных светов

Когда лампочка разбивается на скругленную поверхность, она отражает освещение во многих направлениях, но некоторые из них переводятся непосредственно в глаза средства просмотра. Это часто создает впечатление нечеткой белой области на поверхности, называемой _зеркальным выделением_.

В трехмерной графике отраженные блики часто являются следствием алгоритмов, используемых для определения светлых контуров и заливки. В двухмерной графике иногда добавляются отраженные блики, чтобы предложить внешний вид трехмерной поверхности. Зеркальное выделение может преобразовать плоский красный кружок в круглый красный шар.

На странице **радиальное отраженное выделение** для точного использования используется радиальный градиент. `PaintSurface`Обработчик существ, вычисляя радиус для окружности, два `SKPoint` значения &mdash; a `center` и `offCenter` , которые находятся в середине между центром и верхним левым краями круга:

```csharp
public class RadialSpecularHighlightPage : ContentPage
{
    public RadialSpecularHighlightPage()
    {
        Title = "Radial Specular Highlight";

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

        float radius = 0.4f * Math.Min(info.Width, info.Height);
        SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
        SKPoint offCenter = center - new SKPoint(radius / 2, radius / 2);

        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateRadialGradient(
                                offCenter,
                                radius / 2,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

`CreateRadialGradient`Вызов создает градиент, который начинается с белого, `offCenter` и заканчивается красным на расстоянии половины радиуса. Он выглядит следующим образом.

[![Радиальное отраженное выделение](circular-gradients-images/RadialSpecularHighlight.png "Радиальное отраженное выделение")](circular-gradients-images/RadialSpecularHighlight-Large.png#lightbox)

Если вы внимательно взгляните на этот градиент, то можете решить, что он уязвим. Градиент выравнивается по определенному моменту, и вам может потребоваться, чтобы он был немного менее симметричным для отражения скругленной поверхности. В этом случае вы можете предпочесть отраженному изображению, показанному ниже, в разделе [**градиенты для отражения отраженных**](#conical-gradients-for-specular-highlights)светов.

## <a name="the-sweep-gradient"></a>Градиентная очистка

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[]))Метод имеет самый простой синтаксис всех методов создания градиента:

```csharp
public static SKShader CreateSweepGradient (SKPoint center, 
                                            SKColor[] colors, 
                                            Single[] colorPos)
```

Это всего лишь центр, массив цветов и позиции цвета. Градиент начинается справа от центральной точки и вытекает 360 градусов по часовой стрелке вокруг центра. Обратите внимание, что `SKShaderTileMode` параметр отсутствует.

[`CreateSweepGradient`](xref:SkiaSharp.SKShader.CreateSweepGradient(SkiaSharp.SKPoint,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKMatrix))Также доступна перегрузка с параметром преобразования матрицы. Для изменения начальной точки можно применить преобразование поворота к градиенту. Можно также применить преобразование шкалы, чтобы изменить направление с часовой стрелки на счетчик-по часовой стрелке.

Страница « **градиентная очистка** » использует градиент поворота для цвета круга с толщиной штриха 50 пикселей.

[![Градиентная очистка](circular-gradients-images/SweepGradient.png "Градиентная очистка")](circular-gradients-images/SweepGradient-Large.png#lightbox)

`SweepGradientPage`Класс определяет массив из восьми цветов с разными значениями оттенка. Обратите внимание, что массив начинается и заканчивается красным (значение оттенка 0 или 360), которое отображается в крайнем правом снимке экрана.

```csharp
public class SweepGradientPage : ContentPage
{
    bool drawBackground;

    public SweepGradientPage ()
    {
        Title = "Sweep Gradient";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

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
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            // Define an array of rainbow colors
            SKColor[] colors = new SKColor[8];

            for (int i = 0; i < colors.Length; i++)
            {
                colors[i] = SKColor.FromHsl(i * 360f / 7, 100, 50);
            }

            SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);

            // Create sweep gradient based on center of canvas
            paint.Shader = SKShader.CreateSweepGradient(center, colors, null);

            // Draw a circle with a wide line
            const int strokeWidth = 50;
            paint.Style = SKPaintStyle.Stroke;
            paint.StrokeWidth = strokeWidth;

            float radius = (Math.Min(info.Width, info.Height) - strokeWidth) / 2;
            canvas.DrawCircle(center, radius, paint);

            if (drawBackground)
            {
                // Draw the gradient on the whole canvas
                paint.Style = SKPaintStyle.Fill;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

Программа также реализует интерфейс `TapGestureRecognizer` , который позволяет выполнить некоторый код в конце `PaintSurface` обработчика. Этот код использует тот же градиент для заполнения холста:

[![Полная очистка градиента](circular-gradients-images/SweepGradientFull.png "Полная очистка градиента")](circular-gradients-images/SweepGradientFull-Large.png#lightbox)

На этих снимках экрана показано, что градиентное заполнение цветом любой области. Если градиент не начинается и заканчивается тем же цветом, то справа от центральной точки будет ненепрерывно.

## <a name="the-two-point-conical-gradient"></a>Одноточечный градиент с двумя точками

[`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode))Метод имеет следующий синтаксис:

```csharp
public static SKShader CreateTwoPointConicalGradient (SKPoint startCenter, 
                                                      Single startRadius, 
                                                      SKPoint endCenter, 
                                                      Single endRadius, 
                                                      SKColor[] colors, 
                                                      Single[] colorPos, 
                                                      SKShaderTileMode mode)
```

Параметры начинаются с центральных точек и радиусов для двух кругов, которые называются _начальным_ и _конечным_ кругом. Остальные три параметра те же, что и для `CreateLinearGradient` , и `CreateRadialGradient` . [`CreateTwoPointConicalGradient`](xref:SkiaSharp.SKShader.CreateTwoPointConicalGradient(SkiaSharp.SKPoint,System.Single,SkiaSharp.SKPoint,System.Single,SkiaSharp.SKColor[],System.Single[],SkiaSharp.SKShaderTileMode,SkiaSharp.SKMatrix))Перегрузка включает преобразование матрицы.

Градиент начинается с начала круга и заканчивается в конце. `SKShaderTileMode`Параметр управляет тем, что происходит за пределами двух кругов. Градиент с двумя точками — единственный градиент, который не полностью заполняет область. Если два круга имеют одинаковый радиус, градиент ограничивается прямоугольником, ширина которого совпадает с диаметром кругов. Если два круга имеют разные радиусы, градиент образует конус.

Скорее всего, вы захотите поэкспериментировать с градиентом, сооснованным на двух-точечных, так что страница с **градиентом** в реальном мире является производной от, `InteractivePage` чтобы разрешить перемещение двух точек касания для двух круговых радиусов:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
                       xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Effects.ConicalGradientPage"
                       Title="Conical Gradient">
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

Файл кода программной части определяет два `TouchPoint` объекта с фиксированными радиусами 50 и 100:

```csharp
public partial class ConicalGradientPage : InteractivePage
{
    const int RADIUS1 = 50;
    const int RADIUS2 = 100;

    public ConicalGradientPage ()
    {
        touchPoints = new TouchPoint[2];

        touchPoints[0] = new TouchPoint
        {
            Center = new SKPoint(100, 100),
            Radius = RADIUS1
        };

        touchPoints[1] = new TouchPoint
        {
            Center = new SKPoint(300, 300),
            Radius = RADIUS2
        };

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
            paint.Shader = SKShader.CreateTwoPointConicalGradient(touchPoints[0].Center,
                                                                  RADIUS1,
                                                                  touchPoints[1].Center,
                                                                  RADIUS2,
                                                                  colors,
                                                                  null,
                                                                  tileMode);
            canvas.DrawRect(info.Rect, paint);
        }

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
        }
    }
}
```

`colors`Массив имеет красный, зеленый и синий. Код, направленный в нижнюю часть `PaintSurface` обработчика, рисует две сенсорные точки как черные кружки, чтобы они не переходили на градиент.

Обратите внимание, что `DrawRect` вызов использует градиент для выделения цветом всего холста. Однако в общем случае большая часть холста остается нецветной с градиентом. Ниже приведена программа, демонстрирующая три возможные конфигурации:

[![Градиентное применение в виде конуса](circular-gradients-images/ConicalGradient.png "Градиентное применение в виде конуса")](circular-gradients-images/ConicalGradient-Large.png#lightbox)

Экран iOS в левой части показывает результат `SKShaderTileMode` установки параметра `Clamp` . Градиент начинается с красного на границе меньшего круга, который является противоположным рядом со вторым кружком. `Clamp`Значение также приводит к тому, что красный цвет переходит к точке конуса. Градиент заканчивается синим краями на внешнем крае большего круга, который ближе всего к первому кругу, но остается синим в пределах этого круга и больше.

Экран Android аналогичен, но имеет `SKShaderTileMode` `Repeat` . Теперь ясно, что градиент начинается внутри первого круга и заканчивается снаружи второго круга. Этот `Repeat` параметр приводит к тому, что градиент повторяется красным цветом внутри большего круга.

На экране UWP показано, что происходит при перемещении меньшего круга целиком в большую окружность. Градиентная остановка является конусом, а вместо этого заполняет всю область. Этот результат аналогичен радиальному градиенту, но он является асимметричным, если маленький кружок не полностью выровнен по центру в большем круге.

Возможно, вы сомневаетесь в целесообразности, если один круг вложен в другой, но идеально подходит для зеркального выделения.

## <a name="conical-gradients-for-specular-highlights"></a>Градиенты для отражения отраженных светов

Ранее в этой статье вы узнали, как использовать радиальный градиент для создания отраженного выделения. Для этой цели можно также использовать градиент с двумя точками, и вы можете предпочесть его вид:

[![Отраженное выделение в виде конусов](circular-gradients-images/ConicalSpecularHighlight.png "Отраженное выделение в виде конусов")](circular-gradients-images/ConicalSpecularHighlight-Large.png#lightbox)

Асимметричный внешний вид лучше предлагает скругленную поверхность объекта. 

Код рисования на странице с **отраженным** отражением в виде конусов совпадает со страницей **радиального выделения** , за исключением шейдера:

```csharp
public class ConicalSpecularHighlightPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        using (SKPaint paint = new SKPaint())
        {
            paint.Shader = SKShader.CreateTwoPointConicalGradient(
                                offCenter,
                                1,
                                center,
                                radius,
                                new SKColor[] { SKColors.White, SKColors.Red },
                                null,
                                SKShaderTileMode.Clamp);

            canvas.DrawCircle(center, radius, paint);
        }
    }
}
```

Два круга имеют центры `offCenter` и `center` . Кружок, центрированный по, `center` связан с радиусом, охватывающим весь шар, но круг в центре `offCenter` имеет радиус всего одного пикселя. Градиент эффективно начинает с этого момента и заканчивается на границе шарика.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)