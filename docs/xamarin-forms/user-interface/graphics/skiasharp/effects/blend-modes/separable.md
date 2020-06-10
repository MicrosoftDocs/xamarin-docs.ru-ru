---
Title: "Описание отделяемых Blend Mode" Description: "использование режимов смешения отделяемых для изменения красного, зеленого и синего цветов".
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: 66D1A537-A247-484E-B5B9-FBCB7838FBE9 Автор: давидбритч MS. author: дабритч MS. Дата: 08/23/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="the-separable-blend-modes"></a>Режимы смешения отделяемых

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Как было показано в статье [**SkiaSharp Портер-Дуфф Blend**](porter-duff.md)modes, в режимах Портер-Дуфф Blend обычно выполняются операции обрезки. Режимы наложения отделяемых различаются. Режимы отделяемых изменяют отдельные цветовые компоненты красного, зеленого и синего цветов изображения. В режимах смешения отделяемых можно смешивать цвета, чтобы продемонстрировать, что сочетание красного, зеленого и синего цветов действительно белый:

![Основные цвета](separable-images/SeparableSample.png "Основные цвета")

## <a name="lighten-and-darken-two-ways"></a>Осветление и затемнение двух способов 

Обычно точечный рисунок имеет слишком темный или слишком светлый. Можно использовать режимы наложения отделяемых для осветления или затемнения ИМАБЕ.  Действительно, два из режимов смешения отделяемых в [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) перечислении имеют имена `Lighten` и `Darken` . 

Эти два режима показаны на странице **осветление и затемнение** . Файл XAML создает два `SKCanvasView` объекта и два `Slider` представления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.LightenAndDarkenPage"
             Title="Lighten and Darken">
    <StackLayout>
        <skia:SKCanvasView x:Name="lightenCanvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="lightenSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />

        <skia:SKCanvasView x:Name="darkenCanvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="darkenSlider"
                Margin="10"
                ValueChanged="OnSliderValueChanged" />
    </StackLayout>
</ContentPage>
```

В первой `SKCanvasView` и `Slider` демонстрации `SKBlendMode.Lighten` демонстрируется вторая пара `SKBlendMode.Darken` . Два `Slider` представления используют один и тот же `ValueChanged` обработчик, и два `SKCanvasView` имеют одинаковый `PaintSurface` обработчик. Оба обработчика событий проверяют, какой объект вызывает событие:

```csharp
public partial class LightenAndDarkenPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                typeof(SeparableBlendModesPage),
                "SkiaSharpFormsDemos.Media.Banana.jpg");

    public LightenAndDarkenPage ()
    {
        InitializeComponent ();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if ((Slider)sender == lightenSlider)
        {
            lightenCanvasView.InvalidateSurface();
        }
        else
        {
            darkenCanvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find largest size rectangle in canvas
        float scale = Math.Min((float)info.Width / bitmap.Width,
                               (float)info.Height / bitmap.Height);
        SKRect rect = SKRect.Create(scale * bitmap.Width, scale * bitmap.Height);
        float x = (info.Width - rect.Width) / 2;
        float y = (info.Height - rect.Height) / 2;
        rect.Offset(x, y);

        // Display bitmap
        canvas.DrawBitmap(bitmap, rect);

        // Display gray rectangle with blend mode
        using (SKPaint paint = new SKPaint())
        {
            if ((SKCanvasView)sender == lightenCanvasView)
            {
                byte value = (byte)(255 * lightenSlider.Value);
                paint.Color = new SKColor(value, value, value);
                paint.BlendMode = SKBlendMode.Lighten;
            }
            else
            {
                byte value = (byte)(255 * (1 - darkenSlider.Value));
                paint.Color = new SKColor(value, value, value);
                paint.BlendMode = SKBlendMode.Darken;
            }

            canvas.DrawRect(rect, paint);
        }
    }
}
```

`PaintSurface`Обработчик вычисляет прямоугольник, подходящий для точечного рисунка. Обработчик отображает это растровое изображение, а затем отображает прямоугольник над точечным рисунком с помощью `SKPaint` объекта со `BlendMode` свойством, имеющим значение `SKBlendMode.Lighten` или `SKBlendMode.Darken` . `Color`Свойство является серым затенением на основе `Slider` . Для `Lighten` режима диапазон цветов от черного к белому, но для режима он находится в `Darken` диапазоне от белого до черного.

Снимки экрана слева направо показывают все более большие `Slider` значения, так как верхнее изображение становится светлее, а нижняя — темнее:

[![Осветление и затемнение](separable-images/LightenAndDarken.png "Осветление и затемнение")](separable-images/LightenAndDarken-Large.png#lightbox)

Эта программа демонстрирует нормальный способ использования режимов смешения отделяемых: назначение — это изображение некоторой сортировки, очень часто точечное изображение. Источником является прямоугольник, отображаемый с помощью `SKPaint` объекта со `BlendMode` свойством, установленным в режим отделяемых Blend. Прямоугольник может быть сплошным цветом (как здесь) или градиентом. Прозрачность обычно _не_ используется с режимами смешения отделяемых.

Поэкспериментируя с этой программой, вы обнаружите, что эти два режима наложения не доходят и затемнить изображение единообразно. Вместо этого, `Slider` скорее всего, устанавливается пороговое значение для некоторой сортировки. Например, при увеличении `Slider` для `Lighten` режима более темные области изображения становятся светлее, в то время как светлые области остаются неизменными.

Для `Lighten` режима, если точка назначения — это значение цвета RGB (Dr, SG, DB), а исходный пиксель — цвет (SR, SG, SB), то выходные данные (или, OG, OB) рассчитываются следующим образом:

 `Or = max(Dr, Sr)` `Og = max(Dg, Sg)`
 `Ob = max(Db, Sb)`

Для красного, зеленого и синего по отдельности результат — это больше места назначения и источника. Это приводит к подаче осветления темных областей назначения.

`Darken`Режим аналогичен, за исключением того, что результатом является меньше места назначения и источника:

 `Or = min(Dr, Sr)` `Og = min(Dg, Sg)`
 `Ob = min(Db, Sb)`

Компоненты красного, зеленого и синего обрабатываются отдельно, поэтому эти режимы наложения называются режимами _отделяемых_ Blend. По этой причине можно использовать аббревиатуры **DC** и **SC** для цветов назначения и источника, а также понимать, что вычисления применяются к каждому из компонентов красного, зеленого и синего по отдельности.

В следующей таблице показаны все режимы смешения отделяемых с кратким объяснением того, что они делают. Во втором столбце показан исходный цвет, не создающий изменений:

| Режим смешения   | Без изменения. | Операция |
| ------------ | --------- | --------- |
| `Plus`       | Черный     | Досветлить, добавив цвета: SC + DC |
| `Modulate`   | White     | Затемнение путем умножения цветов: SC · Постоянный | 
| `Screen`     | Черный     | Дополнение продукта к дополнением: SC + DC &ndash; SC · Постоянный |
| `Overlay`    | Серый      | Обратная часть`HardLight` |
| `Darken`     | White     | Минимальное число цветов: min (SC, DC) |
| `Lighten`    | Черный     | Максимум цветов: Max (SC, DC) |
| `ColorDodge` | Черный     | Назначение яркости на основе источника |
| `ColorBurn`  | White     | Затемнение места назначения на основе источника | 
| `HardLight`  | Серый      | Аналогично результату Харша Spotlight |
| `SoftLight`  | Серый      | Аналогично последствием мягкого прожектора | 
| `Difference` | Черный     | Вычитает темную оттенок от более светлого: ABS (DC &ndash; SC) | 
| `Exclusion`  | Черный     | Аналогично, `Difference` но с меньшим контрастом |
| `Multiply`   | White     | Затемнение путем умножения цветов: SC · Постоянный |

Более подробные алгоритмы можно найти в спецификации W3C по [**компоновке и смешению 1**](https://www.w3.org/TR/compositing-1/) , а также в [**справочнике**](https://skia.org/user/api/SkBlendMode_Reference)по СКИА скблендмоде, хотя нотация в этих двух источниках не одинакова. Помните, что `Plus` обычно рассматривается как режим Портер-Дуфф Blend и `Modulate` не является частью спецификации W3C.

Если источник является прозрачным, то для всех режимов смешения отделяемых, кроме `Modulate` , режим смешения не действует. Как было показано ранее, `Modulate` режим наложения включает альфа-канал в умножение. В противном случае `Modulate` имеет тот же результат, что и `Multiply` . 

Обратите внимание на два режима с именами `ColorDodge` и `ColorBurn` . Слова « _осветление_ » и « _запись_ » поступили в даркрум методики. Изображение, выводимое фотографией, распечатается с помощью негативного освещения. При отсутствии светлой страницы печать будет белой. Печать становится более темной по мере того, как в течение более длительного периода времени нападает на печать. Печать — производители часто используют руки или небольшие объекты, чтобы заблокировать часть освещения в определенной части печати, сделав эту область светлой. Это называется _додгинг_. И наоборот, непрозрачный материал с отверстием в нем (или практический блок, блокирующий большую часть освещения) можно использовать для перенаправления большей освещенности в определенном месте, называемой _записью_.

Программа **осветления и записи** очень похожа на ее **осветление и затемнение**. XAML-файл структурирован так же, но с разными именами элементов, и файл кода программной части аналогичен, но результат этих двух режимов наложения сильно отличается:

[![Осветление и запись](separable-images/DodgeAndBurn.png "Осветление и запись")](separable-images/DodgeAndBurn-Large.png#lightbox)

Для небольших `Slider` значений `Lighten` режим сначала становится светлее темными областями, а при этом `ColorDodge` становится более равномерным.

Программы обработки изображений часто позволяют додгинг и записи быть ограничены конкретными областями, как в даркрум. Это можно сделать с помощью градиентов или растрового изображения с различными оттенками серого.

## <a name="exploring-the-separable-blend-modes"></a>Изучение режимов смешения отделяемых

На странице **режимы смешения отделяемых** вы можете изучить все режимы наложения отделяемых. Он отображает место назначения точечного рисунка и цветовой прямоугольник с помощью одного из режимов смешения. 

XAML-файл определяет `Picker` (для выбора режима смешения) и четырех ползунков. Первые три ползунки позволяют задать красный, зеленый и синий компоненты источника. Четвертый ползунок предназначен для переопределения этих значений путем установки серого оттенка. Отдельные ползунки не определяются, но цвета указывают на их функции:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.SeparableBlendModesPage"
             Title="Separable Blend Modes">

    <StackLayout>
        <skiaviews:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blendModePicker"
                Title="Blend Mode"
                Margin="10, 0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlendMode}">
                    <x:Static Member="skia:SKBlendMode.Plus" />
                    <x:Static Member="skia:SKBlendMode.Modulate" />
                    <x:Static Member="skia:SKBlendMode.Screen" />
                    <x:Static Member="skia:SKBlendMode.Overlay" />
                    <x:Static Member="skia:SKBlendMode.Darken" />
                    <x:Static Member="skia:SKBlendMode.Lighten" />
                    <x:Static Member="skia:SKBlendMode.ColorDodge" />
                    <x:Static Member="skia:SKBlendMode.ColorBurn" />
                    <x:Static Member="skia:SKBlendMode.HardLight" />
                    <x:Static Member="skia:SKBlendMode.SoftLight" />
                    <x:Static Member="skia:SKBlendMode.Difference" />
                    <x:Static Member="skia:SKBlendMode.Exclusion" />
                    <x:Static Member="skia:SKBlendMode.Multiply" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="redSlider"
                MinimumTrackColor="Red"
                MaximumTrackColor="Red"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="greenSlider"
                MinimumTrackColor="Green"
                MaximumTrackColor="Green"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="blueSlider"
                MinimumTrackColor="Blue"
                MaximumTrackColor="Blue"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="graySlider"
                MinimumTrackColor="Gray"
                MaximumTrackColor="Gray"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="colorLabel"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Файл кода программной части загружает один из ресурсов точечного рисунка и рисует его дважды, один раз в верхней половине холста и снова в нижней половине холста:

```csharp
public partial class SeparableBlendModesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(SeparableBlendModesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg"); 

    public SeparableBlendModesPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        if (sender == graySlider)
        {
            redSlider.Value = greenSlider.Value = blueSlider.Value = graySlider.Value;
        }

        colorLabel.Text = String.Format("Color = {0:X2} {1:X2} {2:X2}",
                                        (byte)(255 * redSlider.Value),
                                        (byte)(255 * greenSlider.Value),
                                        (byte)(255 * blueSlider.Value));

        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw bitmap in top half
        SKRect rect = new SKRect(0, 0, info.Width, info.Height / 2);
        canvas.DrawBitmap(bitmap, rect, BitmapStretch.Uniform);

        // Draw bitmap in bottom halr
        rect = new SKRect(0, info.Height / 2, info.Width, info.Height);
        canvas.DrawBitmap(bitmap, rect, BitmapStretch.Uniform);

        // Get values from XAML controls
        SKBlendMode blendMode =
            (SKBlendMode)(blendModePicker.SelectedIndex == -1 ?
                                        0 : blendModePicker.SelectedItem);

        SKColor color = new SKColor((byte)(255 * redSlider.Value),
                                    (byte)(255 * greenSlider.Value),
                                    (byte)(255 * blueSlider.Value));

        // Draw rectangle with blend mode in bottom half
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = color;
            paint.BlendMode = blendMode;
            canvas.DrawRect(rect, paint);
        }
    }
}
```

В нижней части `PaintSurface` обработчика прямоугольник рисуется поверх второго точечного рисунка с выбранным режимом наложения и выбранным цветом. Вы можете сравнить измененный точечный рисунок снизу с исходным точечным рисунком в верхней части:

[![Отделяемые режимы смешения](separable-images/SeparableBlendModes.png "Отделяемые режимы смешения")](separable-images/SeparableBlendModes-Large.png#lightbox)

## <a name="additive-and-subtractive-primary-colors"></a>Аддитивные и вычитаемые основные цвета

На странице **Основные цвета** рисуется три перекрывающихся кружков: красный, зеленый и синий.

[![Аддитивные основные цвета](separable-images/PrimaryColors-Additive.png "Аддитивные основные цвета")](separable-images/PrimaryColors-Additive.png#lightbox)

Это Аддитивные основные цвета. Сочетания любого из двух типов — голубой, пурпурный и желтый, а сочетание всех трех — белый.

Эти три круга рисуются в `SKBlendMode.Plus` режиме, но `Screen` `Lighten` `Difference` для одного и того же результата можно использовать, или. Вот эта программа:

```csharp
public class PrimaryColorsPage : ContentPage
{
    bool isSubtractive;

    public PrimaryColorsPage ()
    {
        Title = "Primary Colors";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;

        // Switch between additive and subtractive primaries at tap
        TapGestureRecognizer tap = new TapGestureRecognizer();
        tap.Tapped += (sender, args) =>
        {
            isSubtractive ^= true;
            canvasView.InvalidateSurface();
        };
        canvasView.GestureRecognizers.Add(tap);

        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Rect.MidX, info.Rect.MidY);
        float radius = Math.Min(info.Width, info.Height) / 4;
        float distance = 0.8f * radius;     // from canvas center to circle center
        SKPoint center1 = center + 
            new SKPoint(distance * (float)Math.Cos(9 * Math.PI / 6),
                        distance * (float)Math.Sin(9 * Math.PI / 6));
        SKPoint center2 = center +
            new SKPoint(distance * (float)Math.Cos(1 * Math.PI / 6),
                        distance * (float)Math.Sin(1 * Math.PI / 6));
        SKPoint center3 = center +
            new SKPoint(distance * (float)Math.Cos(5 * Math.PI / 6),
                        distance * (float)Math.Sin(5 * Math.PI / 6));

        using (SKPaint paint = new SKPaint())
        {
            if (!isSubtractive)
            {
                paint.BlendMode = SKBlendMode.Plus; 
                System.Diagnostics.Debug.WriteLine(paint.BlendMode);

                paint.Color = SKColors.Red;
                canvas.DrawCircle(center1, radius, paint);

                paint.Color = SKColors.Lime;    // == (00, FF, 00)
                canvas.DrawCircle(center2, radius, paint);

                paint.Color = SKColors.Blue;
                canvas.DrawCircle(center3, radius, paint);
            }
            else
            {
                paint.BlendMode = SKBlendMode.Multiply
                System.Diagnostics.Debug.WriteLine(paint.BlendMode);

                paint.Color = SKColors.Cyan;
                canvas.DrawCircle(center1, radius, paint);

                paint.Color = SKColors.Magenta;
                canvas.DrawCircle(center2, radius, paint);

                paint.Color = SKColors.Yellow;
                canvas.DrawCircle(center3, radius, paint);
            }
        }
    }
}
```

Программа включает `TabGestureRecognizer` . При касании или щелчке на экране программа использует `SKBlendMode.Multiply` для отображения трех вычитаемых первичных значений:

[![Вычитаемые основные цвета](separable-images/PrimaryColors-Subtractive.png "Вычитаемые основные цвета")](separable-images/PrimaryColors-Subtractive-Large.png#lightbox)

`Darken`Режим также работает для этого же результата.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
