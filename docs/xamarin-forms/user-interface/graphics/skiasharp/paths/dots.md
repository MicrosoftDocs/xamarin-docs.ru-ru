---
title: Точки и тире в SkiaSharp
description: В этой статье рассматриваются принципы рисования пунктирных и пунктирных линий в SkiaSharp, а также демонстрируется пример кода.
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5064a53b140c26acdc5149f5495cc002e657a9b0
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91564008"
---
# <a name="dots-and-dashes-in-skiasharp"></a>Точки и тире в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Основные сложности рисования пунктирных и пунктирных линий в SkiaSharp_

SkiaSharp позволяет рисовать линии, которые не являются сплошными, а состоят из точек и тире:

![Пунктирная линия](dots-images/dottedlinesample.png)

Это делается с помощью *результата пути*, который является экземпляром [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) класса, установленного для [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) свойства `SKPaint` . Можно создать эффект пути (или объединить эффекты пути) с помощью одного из статических методов создания, определенных в `SKPathEffect` . ( `SKPathEffect` является одним из шести эффектов, поддерживаемых SkiaSharp. другие описаны в разделе [**SkiaSharp Effect**](../effects/index.md).)

Для рисования пунктирных или пунктирных линий используется [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) статический метод. Существует два аргумента: сначала это массив `float` значений, указывающий длины точек и тире, а также длину пробелов между ними. Этот массив должен содержать четное число элементов, и должно быть по крайней мере два элемента. (В массиве могут быть нулевые элементы, но это приводит к сплошной линии.) Если имеется два элемента, первый — это длина точки или тире, а вторая — длину разрыва перед следующей точкой или тире. Если имеется более двух элементов, они находятся в следующем порядке: длина тире, длина зазора, длина тире, длина зазора и т. д.

Как правило, необходимо сделать так, чтобы длина штриха и зазора была кратной ширине штриха. Например, если ширина штриха равна 10 пикселям, то массив {10, 10} будет рисовать пунктирную линию, в которой точки и зазоры имеют ту же длину, что и толщина штриха.

Однако `StrokeCap` параметр `SKPaint` объекта также влияет на эти точки и тире. Как вы увидите вскоре, это повлияет на элементы этого массива.

Пунктирные и пунктирные линии показаны на странице « **точки и тире** ». В файле [**дотсанддашеспаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml) создаются два `Picker` представления: один для выбора конца штриха, а второй — для выбора массива тире:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKStrokeCap}">
                    <x:Static Member="skia:SKStrokeCap.Butt" />
                    <x:Static Member="skia:SKStrokeCap.Round" />
                    <x:Static Member="skia:SKStrokeCap.Square" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>10, 10</x:String>
                    <x:String>30, 10</x:String>
                    <x:String>10, 10, 30, 10</x:String>
                    <x:String>0, 20</x:String>
                    <x:String>20, 20</x:String>
                    <x:String>0, 20, 20, 20</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

 Первые три элемента `dashArrayPicker` предполагают, что ширина штриха равна 10 пикселям. Массив {10, 10} предназначен для пунктирной линии, {30, 10} — для штриховой линии, а {10, 10, 30, 10} — для линии с точками и тире. (Другие три будут обсуждаться в ближайшее время.)

[`DotsAndDashesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/DotsAndDashesPage.xaml.cs)Файл кода программной части содержит `PaintSurface` обработчик событий и пару вспомогательных подпрограмм для доступа к `Picker` представлениям:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem,
        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string str = (string)picker.SelectedItem;
    string[] strs = str.Split(new char[] { ' ', ',' }, StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

На следующих снимках экрана на экране iOS в дальнем левом углу отображается пунктирная линия:

[![Тройной снимок экрана со страницей "точки и тире"](dots-images/dotsanddashes-small.png)](dots-images/dotsanddashes-large.png#lightbox "Тройной снимок экрана со страницей "точки и тире"")

Однако экран Android также должен отображать пунктирную линию, используя массив {10, 10}, а строку сплошной. Что произошло? Проблема заключается в том, что экран Android также имеет параметр «Установка с прописными буквами `Square` ». Это расширяет все тире на половину толщины штрихов, что приводит к заполнению пропусков.

Чтобы обойти эту проблему при использовании наконечника обводки `Square` или `Round` , необходимо уменьшить размер штриха в массиве на длину штриха (иногда это приводит к нулевой длине тире) и увеличить длину зазора по длине штриха. Вот как вычисляются последние три массива штрихов в `Picker` файле XAML:

- {10, 10} превращается в {0, 20} на пунктирную линию
- {30, 10} преобразуется в {20, 20} для штриховой линии
- {10, 10, 30, 10} становятся {0, 20, 20, 20} для пунктирной и пунктирной линии

На экране UWP отображается пунктирная и пунктирная линии для наконечника штриха `Round` . `Round`Наконечник штриха часто обеспечивает лучший вид точек и штрихов в жирных линиях.

До сих пор не было упоминания о втором параметре `SKPathEffect.CreateDash` метода. Этот параметр называется `phase` , и он ссылается на смещение в шаблоне с точкой и тире для начала строки. Например, если массив тире имеет значение {10, 10}, а значение `phase` равно 10, то строка начинается с пробела, а не с точки.

Одно интересное приложение `phase` параметра находится в анимации. **Анимированная страница с спиралью** похожа на страницу « **арчимедеанка** », за исключением того, что [`AnimatedSpiralPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/AnimatedSpiralPage.cs) класс анимирует `phase` параметр с помощью Xamarin.Forms `Device.Timer` метода:

```csharp
public class AnimatedSpiralPage : ContentPage
{
    const double cycleTime = 250;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float dashPhase;

    public AnimatedSpiralPage()
    {
        Title = "Animated Spiral";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            dashPhase = (float)(10 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }
    ···  
}
```

Конечно, для просмотра анимации необходимо запустить программу.

[![Тройной снимок экрана с анимированной страницей](dots-images/animatedspiral-small.png)](dots-images/animatedspiral-large.png#lightbox "Тройной снимок экрана с анимированной страницей")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)