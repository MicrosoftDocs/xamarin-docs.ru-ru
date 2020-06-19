---
title: Типы заполнения пути
description: В этой статье рассматриваются различные эффекты, возможные для типов заливки пути SkiaSharp, и демонстрируется пример кода.
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c8c54f3d3815e418d2f71960dc7733711cb40ae2
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139052"
---
# <a name="the-path-fill-types"></a>Типы заполнения пути

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Узнайте о различных эффектах, возможных для типов заливки пути SkiaSharp_

Два контура в контуре могут перекрываться, а линии, составляющие один контур, могут перекрываться. Любая заданная область может быть заполнена, но вы можете не заполнять все закрытые области. Ниже приведен пример:

![](fill-types-images/filltypeexample.png "Five-pointed star partially filles")

У вас есть небольшой контроль над этим. Алгоритм заполнения регулируется [`SKFillType`](xref:SkiaSharp.SKPath.FillType) свойством `SKPath` , для которого задается член [`SKPathFillType`](xref:SkiaSharp.SKPathFillType) перечисления:

- `Winding`, значение по умолчанию;
- `EvenOdd`
- `InverseWinding`
- `InverseEvenOdd`

Алгоритмы «обмотка» и «четный-нечетный» определяют, заполнена ли любая из заключенной области на основе гипотетической строки, нарисованной из этой области, в бесконечность. Эта строка пересекает одну или несколько линий границ, составляющих путь. В режиме поворота, если количество линий границ, рисуемых в одном направлении, распределяет количество линий, рисуемых в другом направлении, то область не заполняется. В противном случае область заполняется. Алгоритм «четный-нечетный» заполняет область, если число линий границ нечетное.

При использовании многих путей подпрограмм алгоритм поворота часто заполняет все вложенные области пути. Алгоритм «четный-нечетный» обычно дает более интересные результаты.

Классический пример — это 5-направленная звезда, как показано на **5-** элементной странице звезды. Файл [**фивепоинтедстарпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FivePointedStarPage.xaml) создает два `Picker` представления для выбора типа заливки пути, а также указывает, является ли контур обводкым или заполненным или обоими, и в каком порядке:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPathFillType}">
                    <x:Static Member="skia:SKPathFillType.Winding" />
                    <x:Static Member="skia:SKPathFillType.EvenOdd" />
                    <x:Static Member="skia:SKPathFillType.InverseWinding" />
                    <x:Static Member="skia:SKPathFillType.InverseEvenOdd" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Fill only</x:String>
                    <x:String>Stroke only</x:String>
                    <x:String>Stroke then Fill</x:String>
                    <x:String>Fill then Stroke</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2"
                                PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Файл кода программной части использует оба `Picker` значения для рисования пять-элементной звездочки:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)fillTypePicker.SelectedItem
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle),
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch ((string)drawingModePicker.SelectedItem)
    {
        case "Fill only":
            canvas.DrawPath(path, fillPaint);
            break;

        case "Stroke only":
            canvas.DrawPath(path, strokePaint);
            break;

        case "Stroke then Fill":
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case "Fill then Stroke":
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

Как правило, тип заливки пути должен влиять только на заливки и не штрихи, но два `Inverse` режима влияют как на заливку, так и на штрихи. Для заливок два типа заполняют `Inverse` области в обратном направлении, чтобы заполнить область за пределами звезды. Для штрихов в двух `Inverse` типах цвета все, кроме штриха. Использование этих типов необратных заливок может привести к нечетным результатам, так как на снимке экрана iOS показано следующее:

[![](fill-types-images/fivepointedstar-small.png "Triple screenshot of the Five-Pointed Star page")](fill-types-images/fivepointedstar-large.png#lightbox "Triple screenshot of the Five-Pointed Star page")

На снимке экрана Android показаны типичные эффекты «четный-нечетный» и «обмотка», но порядок штрихов и заливки также влияет на результаты.

Алгоритм поворота зависит от направления рисования линий. Обычно при создании контура можно управлять этим направлением, когда вы указываете, что линии отображаются из одной точки в другую. Однако `SKPath` класс также определяет такие методы, как `AddRect` и `AddCircle` , которые рисуют целые контуры. Для управления обрисовкой этих объектов методы включают параметр типа [`SKPathDirection`](xref:SkiaSharp.SKPathDirection) , который имеет два члена:

- `Clockwise`
- `CounterClockwise`

Методы в `SKPath` , которые включают `SKPathDirection` параметр, присвойте ему значение по умолчанию `Clockwise` .

На **перекрывающейся странице с кружками** создается контур с четырьмя перекрывающимися круговыми типами заливки пути с четными нечетными путями:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

Это интересный образ, созданный с минимальным объемом кода:

[![](fill-types-images/overlappingcircles-small.png "Triple screenshot of the Overlapping Circles page")](fill-types-images/overlappingcircles-large.png#lightbox "Triple screenshot of the Overlapping Circles page")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
