---
title: Интеграция с Xamarin.Forms
description: В этой статье объясняется, как создать график SkiaSharp, реагирующий на касание и Xamarin.Forms элементы, и демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8288ad3238babe1ce6abb6d9517cdae71373d27c
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366624"
---
# <a name="integrating-with-xamarinforms"></a>Интеграция с Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Создание графики SkiaSharp, которая реагирует на касание и Xamarin.Forms элементы_

SkiaSharp графика может интегрироваться с остальными элементами Xamarin.Forms несколькими способами. Можно объединить SkiaSharp холст и Xamarin.Forms элементы на одной странице и даже располагать Xamarin.Forms элементы поверх SkiaSharp Canvas:

![Выбор цвета с помощью ползунков](integration-images/integrationexample.png)

Другой подход к созданию интерактивной SkiaSharp графики в Xamarin.Forms заключается в использовании сенсорного ввода.
Второй страницей программы [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) является право **коснуться кнопки переключить заливку**. Он рисует простую окружность двумя способами &mdash; без заливки и с заливкой &mdash; , переключаемой касанием. [`TapToggleFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs)Класс показывает, как можно изменить график SkiaSharp в ответ на вводимые пользователем данные.

Для этой страницы `SKCanvasView` создается экземпляр класса в файле [таптогглефилл. XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) , который также задает в Xamarin.Forms [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) представлении:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.TapToggleFillPage"
             Title="Tap Toggle Fill">

    <skia:SKCanvasView PaintSurface="OnCanvasViewPaintSurface">
        <skia:SKCanvasView.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnCanvasViewTapped" />
        </skia:SKCanvasView.GestureRecognizers>
    </skia:SKCanvasView>
</ContentPage>
```

Обратите внимание на `skia` объявление пространства имен XML.

`Tapped`Обработчик для `TapGestureRecognizer` объекта просто переключает значение логического поля и вызывает [`InvalidateSurface`](xref:SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface) метод из `SKCanvasView` :

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Вызов для `InvalidateSurface` эффективного создания вызова `PaintSurface` обработчика, который использует `showFill` поле для заполнения или не заполнения окружности:

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
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 50
    };
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);

    if (showFill)
    {
        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.Blue;
        canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    }
}
```

`StrokeWidth`Для акцентирования разницы в свойстве задано значение 50. Можно также увидеть всю ширину линии, нарисовав внутреннюю часть, а затем контур. По умолчанию графические изображения, нарисованные позже в `PaintSurface` обработчике событий, скрывают те, которые были выведены ранее в обработчике.

На странице « **Обзор цветов** » показано, как можно интегрировать SkiaSharp график с другими Xamarin.Forms элементами, а также демонстрирует разницу между двумя альтернативными методами определения цветов в SkiaSharp. Статический [`SKColor.FromHsl`](xref:SkiaSharp.SKColor.FromHsl(System.Single,System.Single,System.Single,System.Byte)) метод создает `SKColor` значение на основе модели оттенок-насыщенность-освещение:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Статический [`SKColor.FromHsv`](xref:SkiaSharp.SKColor.FromHsv(System.Single,System.Single,System.Single,System.Byte)) метод создает `SKColor` значение на основе аналогичной модели «оттенок-насыщенность-значение»:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

В обоих случаях аргумент находится в `h` диапазоне от 0 до 360. `s`Аргументы, `l` и находятся в `v` диапазоне от 0 до 100. `a`Аргумент (альфа или Opacity) находится в диапазоне от 0 до 255.

Файл [**колорексплорепаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) создает два `SKCanvasView` объекта в `StackLayout` параллельном режиме с `Slider` `Label` представлениями и, которые позволяют пользователю выбирать значения цвета HSL и HSV:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Basics.ColorExplorePage"
             Title="Color Explore">
    <StackLayout>
        <!-- Hue slider -->
        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference hueSlider},
                              Path=Value,
                              StringFormat='Hue = {0:F0}'}" />

        <!-- Saturation slider -->
        <Slider x:Name="saturationSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference saturationSlider},
                              Path=Value,
                              StringFormat='Saturation = {0:F0}'}" />

        <!-- Lightness slider -->
        <Slider x:Name="lightnessSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference lightnessSlider},
                              Path=Value,
                              StringFormat='Lightness = {0:F0}'}" />

        <!-- HSL canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hslCanvasView"
                               PaintSurface="OnHslCanvasViewPaintSurface" />

            <Label x:Name="hslLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>

        <!-- Value slider -->
        <Slider x:Name="valueSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference valueSlider},
                              Path=Value,
                              StringFormat='Value = {0:F0}'}" />

        <!-- HSV canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hsvCanvasView"
                               PaintSurface="OnHsvCanvasViewPaintSurface" />

            <Label x:Name="hsvLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Два `SKCanvasView` элемента находятся в одной ячейке `Grid` с элементом, расположенным `Label` сверху, для отображения результирующего значения цвета RGB.

Файл кода программной части [**ColorExplorePage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) относительно прост. Общий `ValueChanged` обработчик для трех `Slider` элементов просто делает недействительными оба `SKCanvasView` элемента. `PaintSurface`Обработчики удаляют холст с цветом, указанным в `Slider` элементах, а также задает элемент, `Label` расположенный поверх `SKCanvasView` элементов:

```csharp
public partial class ColorExplorePage : ContentPage
{
    public ColorExplorePage()
    {
        InitializeComponent();

        hueSlider.Value = 0;
        saturationSlider.Value = 100;
        lightnessSlider.Value = 50;
        valueSlider.Value = 100;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        hslCanvasView.InvalidateSurface();
        hsvCanvasView.InvalidateSurface();
    }

    void OnHslCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsl((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)lightnessSlider.Value);
        args.Surface.Canvas.Clear(color);

        hslLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }

    void OnHsvCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsv((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)valueSlider.Value);
        args.Surface.Canvas.Clear(color);

        hsvLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }
}
```

В цветовых моделях HSL и HSV значение оттенка варьируется от 0 до 360 и обозначает главный оттенок цвета. Это традиционные цвета «Радуга»: красный, оранжевый, желтый, зеленый, синий, индиго, фиолетовый и обратно в круге до красного.

В модели HSL значение 0 для освещенности всегда является черным, а значение 100 — всегда белым. Если значение насыщенности равно 0, значения освещенности в диапазоне от 0 до 100 являются оттенками серого. Увеличение насыщенности увеличивает цвет. Чистые цвета (которые являются значениями RGB с одним компонентом, равным 255, второй равно 0, а третий — от 0 до 255), если насыщенность равна 100, а освещение — 50.

В модели HSV в чистом цвете, если насыщенность и значение равны 100. Если значение равно 0, независимо от других параметров, цвет будет черным. Серые тени происходят, когда насыщенность равна 0, а диапазоны значений — от 0 до 100.

Но лучший способ познакомиться с этими двумя моделями — это поэкспериментировать с ними самостоятельно:

[![Тройной снимок экрана страницы "Обзор цветов"](integration-images/colorexplore-large.png)](integration-images/colorexplore-small.png#lightbox "Тройной снимок экрана страницы "Обзор цветов"")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)