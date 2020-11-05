---
title: Режимы смешения не отделяемых
description: Используйте режимы смешения, отличные от отделяемых, для изменения тона, насыщенности или яркости.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97FA2730-87C0-4914-8C9F-C64A02CF9EEF
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ea590c0390ab045e5cf8b526aee66c2408d1b784
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374671"
---
# <a name="the-non-separable-blend-modes"></a>Режимы смешения не отделяемых

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Как было показано в статье [**SkiaSharp отделяемых Blend modes**](separable.md), режимы смешения отделяемых выполняют операции с красным, зеленым и синим каналами отдельно. Режимы отделяемых Blend не являются. При работе с цветами оттенков, насыщенностью и яркости цвета отделяемых режимы наложения могут изменять цвета в интересных целях:

![Образец, не отделяемых](non-separable-images/NonSeparableSample.png "Образец, не отделяемых")

## <a name="the-hue-saturation-luminosity-model"></a>Модель оттенок — насыщенность-яркость

Чтобы разобраться в отделяемых режимах наложения, необходимо рассматривать целевые и исходные Пиксели как цвета в модели оттенок-насыщенность-яркость. (Яркость также называется освещением.)

Цветовая модель HSL обсуждалась в статье [**Интеграция с Xamarin.Forms**](../../basics/integration.md) , а пример программы в этой статье позволяет ЭКСПЕРИМЕНТИРОВАТЬ с цветами HSL. Можно создать значение, `SKColor` используя оттенок, насыщенность и значения яркости с помощью статического [`SKColor.FromHsl`](xref:SkiaSharp.SKColor.FromHsl*) метода.

Цветовой тон представляет доминирующее вавеленгс цвета. Значения оттенка находятся в диапазоне от 0 до 360 и проходят через аддитивные и вычитаемые первичные цвета: красный — значение 0, желтый — 60, зеленый — 120, голубой — 180, синий — 240, пурпурный — 300, а цикл — как красный в 360.

Если &mdash; , например, нет основного цвета, цвет будет белым или черным или серым затененным, а &mdash; оттенок не определен и обычно имеет значение 0. 

Значения насыщенности могут находиться в диапазоне от 0 до 100 и указывать чистоту цвета. Значение насыщенности 100 — это самый чистый цвет, а значения ниже 100 приводят к тому, что цвет становится более серой. Значение насыщенности 0 приводит к оттенок серого.

Значение «яркость» (или «освещение») указывает, насколько яркий цвет. Значение яркости 0 — черный, независимо от других параметров. Аналогично, значение яркости 100 — белый. 

Значение HSL (0, 100, 50) — это значение RGB (FF, 00, 00), которое является чисто красным. Значение HSL (180, 100, 50) — это значение RGB (00, FF, FF), чистый голубой. По мере уменьшения насыщенности уменьшается компонент основного цвета, а остальные компоненты увеличиваются. На уровне насыщенности 0 все компоненты одинаковы, а цвет — серым цветом. Уменьшить яркость, чтобы вернуться к черному; Увеличьте яркость, чтобы вернуться к белому.

## <a name="the-blend-modes-in-detail"></a>Подробные сведения о режимах смешения

Как и в других режимах наложения, четыре режима отделяемых Blend используют назначение (которое часто является растровым изображением) и источник, который часто является одним цветом или градиентом. Режимы наложения объединяют значения тона, насыщенности и яркости из назначения и источника:

| Режим смешения   | Компоненты из источника | Компоненты из места назначения |
| ------------ | ---------------------- | --------------------------- |
| `Hue`        | Оттенок                    | Насыщенность и яркость   |
| `Saturation` | Насыщенность             | Оттенок и яркость          |
| `Color`      | Цветовой тон и насыщенность     | Яркости                  | 
| `Luminosity` | Яркости             | Цветовой тон и насыщенность          | 

См. спецификацию [**КОМПОНОВКИ W3C и уровень смешения 1**](https://www.w3.org/TR/compositing-1/) для алгоритмов.

Страница **отделяемых Blend modes** содержит, `Picker` чтобы выбрать один из этих режимов наложения и три `Slider` представления для выбора цвета HSL:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.NonSeparableBlendModesPage"
             Title="Non-Separable Blend Modes">

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
                    <x:Static Member="skia:SKBlendMode.Hue" />
                    <x:Static Member="skia:SKBlendMode.Saturation" />
                    <x:Static Member="skia:SKBlendMode.Color" />
                    <x:Static Member="skia:SKBlendMode.Luminosity" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="satSlider"
                Maximum="100"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Slider x:Name="lumSlider"
                Maximum="100"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <StackLayout Orientation="Horizontal">
            <Label x:Name="hslLabel"
                   HorizontalOptions="CenterAndExpand" />

            <Label x:Name="rgbLabel"
                   HorizontalOptions="CenterAndExpand" />

        </StackLayout>
    </StackLayout>
</ContentPage>
```

Для экономии пространства три `Slider` представления не определяются в пользовательском интерфейсе программы. Необходимо помнить, что порядок — оттенок, насыщенность и яркость. В двух `Label` представлениях в нижней части страницы показаны значения цвета HSL и RGB.

Файл кода программной части загружает один из ресурсов точечного рисунка, показывает, насколько это возможно на холсте, а затем охватывает холст прямоугольником. Цвет прямоугольника основан на трех `Slider` представлениях, а режим наложения — на один, выбранный в `Picker` :

```csharp
public partial class NonSeparableBlendModesPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(NonSeparableBlendModesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKColor color;

    public NonSeparableBlendModesPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        // Calculate new color based on sliders
        color = SKColor.FromHsl((float)hueSlider.Value,
                                (float)satSlider.Value,
                                (float)lumSlider.Value);

        // Use labels to display HSL and RGB color values
        color.ToHsl(out float hue, out float sat, out float lum);

        hslLabel.Text = String.Format("HSL = {0:F0} {1:F0} {2:F0}",
                                      hue, sat, lum);

        rgbLabel.Text = String.Format("RGB = {0:X2} {1:X2} {2:X2}",
                                      color.Red, color.Green, color.Blue);

        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);

        // Get blend mode from Picker
        SKBlendMode blendMode =
            (SKBlendMode)(blendModePicker.SelectedIndex == -1 ?
                                        0 : blendModePicker.SelectedItem);

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = color;
            paint.BlendMode = blendMode;
            canvas.DrawRect(info.Rect, paint);
        }
    }
}
```

Обратите внимание, что программа не отображает значение цвета HSL, выбранное тремя ползунками. Вместо этого он создает значение цвета на этих ползунках, а затем использует [`ToHsl`](xref:SkiaSharp.SKColor.ToHsl*) метод для получения значений тона, насыщенности и яркости. Это обусловлено тем `FromHsl` , что метод преобразует цвет HSL в цвет RGB, который хранится внутри `SKColor` структуры. `ToHsl`Метод преобразует из RGB в HSL, но результат не всегда будет исходным значением. 

Например, `FromHsl` преобразует значение HSL (180, 50, 0) в цвет RGB (0, 0, 0), поскольку `Luminosity` равно нулю. `ToHsl`Метод преобразует цвет RGB (0, 0, 0) в цвет HSL (0, 0, 0), так как значения тона и насыщенности несущественны. При использовании этой программы лучше видеть представление цвета HSL, которое используется программой, а не той, которую вы указали с помощью ползунков.

`SKBlendModes.Hue`Режим Blend использует уровень оттенка источника, сохраняя насыщенность и уровни яркости назначения. При тестировании этого режима наложения ползунки насыщенности и яркости должны иметь значение, отличное от 0 или 100, поскольку в таких случаях оттенок не определяется уникальным образом.

[![Режимы смешения не отделяемых — оттенок](non-separable-images/NonSeparableBlendModes-Hue.png "Режимы смешения не отделяемых — оттенок")](non-separable-images/NonSeparableBlendModes-Hue-Large.png#lightbox)

При использовании параметра установить ползунок равным 0 (как и на снимке экрана iOS слева) все превращается в реддиш. Но это не означает, что изображение полностью отсутствует зеленым и синим. Очевидно, что в результате присутствуют серые тени. Например, цвет RGB (40, 40, C0) эквивалентен цвету HSL (240, 50, 50). Цветовой тон — синий, но значение насыщенности 50 означает, что также имеются красные и зеленые компоненты. Если для оттенка установлено значение 0 `SKBlendModes.Hue` , то цвет HSL — (0, 50, 50), то есть цвет RGB (C0, 40, 40). По-прежнему остались синие и зеленые компоненты, но теперь главный компонент — красный.

`SKBlendModes.Saturation`Режим Blend объединяет уровень насыщенности источника с Оттенокм и яркостью назначения. Как и цветовой тон, насыщенность нечетко определяется, если яркость равна 0 или 100. Теоретически, все параметры яркости между этими двумя крайними значениями должны работать. Однако параметр «яркость», как следствие, влияет на результат. Задайте для параметра яркость значение 50, чтобы увидеть, как можно задать уровень насыщенности изображения:

[![Режимы смешения не отделяемых — насыщенность](non-separable-images/NonSeparableBlendModes-Saturation.png "Режимы смешения не отделяемых — насыщенность")](non-separable-images/NonSeparableBlendModes-Saturation-Large.png#lightbox)

Этот режим наложения можно использовать для увеличения насыщенности цвета тусклого изображения, или можно уменьшить насыщенность до нуля (как на снимке экрана iOS слева) для результирующего изображения, состоящего только из серых оттенков.

`SKBlendModes.Color`Режим наложения удерживает яркость назначения, но использует оттенок и насыщенность источника. Опять же, это означает, что все параметры ползунка «яркость» должны работать в любом месте между крайними значениями. 

[![Режимы Blend (не отделяемых) — цвет](non-separable-images/NonSeparableBlendModes-Color.png "Режимы Blend (не отделяемых) — цвет")](non-separable-images/NonSeparableBlendModes-Color-Large.png#lightbox)

В ближайшее время вы увидите приложение этого режима наложения.

Наконец, `SKBlendModes.Luminosity` режим смешения является противоположным `SKBlendModes.Color` . Он оставляет цветовой тон и насыщенность назначения, но использует яркость источника. `Luminosity`Режим наложения является наиболее mysteriousм пакета: ползунки «Цветовой тон» и «насыщенность» влияют на изображение, но даже на средней яркости изображение не отличается:

[![Режимы наложения «не отделяемых» — «яркость»](non-separable-images/NonSeparableBlendModes-Luminosity.png "Режимы наложения «не отделяемых» — «яркость»")](non-separable-images/NonSeparableBlendModes-Luminosity-Large.png#lightbox)

Теоретически, увеличение или уменьшение яркости изображения должно сделать его более светлым или темным. Что интересно, [Пример яркости в **справочнике** по СКИА скблендмоде](https://skia.org/user/api/SkBlendMode_Reference#Luminosity) весьма похож.

Обычно это не так, что в качестве источника, который состоит из одного цвета, применяемого ко всему целевому изображению, следует использовать один из режимов Blend, отличный от отделяемых. Этот результат просто слишком велик. Необходимо ограничить воздействие на одну часть изображения. В этом случае источник, вероятно, будет включать транспаранци или, возможно, источник будет ограничен графиком меньшего размера.

## <a name="a-matte-for-a-separable-mode"></a>Матовая для режима отделяемых

Ниже приведено одно из точечных рисунков, входящих в состав примера [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) . Имя файла — **Banana.jpg** :

![Обезьяна в полукруглой](non-separable-images/Banana.jpg "Обезьяна в полукруглой")

Можно создать матовый, охватывающий только «полукруглый». Это также является ресурсом в примере [**скиашарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) . Имя файла — **BananaMatte.png** :

![Полукруглая матовая](non-separable-images/BananaMatte.png "Полукруглая матовая")

Помимо черной формы, оставшаяся часть точечного рисунка является прозрачной.

Синяя страница « **синевато-сталь** » использует эту подложку, чтобы изменить цветовой тон и насыщенность, которую удерживает обезьяна, но чтобы ничего не менять в изображении. 

В следующем `BlueBananaPage` классе **Banana.jpg** точечный рисунок загружается как поле. Конструктор загружает точечный рисунок **BananaMatte.png** в качестве `matteBitmap` объекта, но не оставляет этот объект за пределами конструктора. Вместо этого создается третье точечное изображение с именем `blueBananaBitmap` . Объект `matteBitmap` отображается на, `blueBananaBitmap` за которым следует объект `SKPaint` со `Color` значением синего цвета и `BlendMode` значением `SKBlendMode.SrcIn` . Объект `blueBananaBitmap` остается в основном прозрачным, но с чисто чистым синим изображением «стального»:

```csharp
public class BlueBananaPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BlueBananaPage),
        "SkiaSharpFormsDemos.Media.Banana.jpg");

    SKBitmap blueBananaBitmap;

    public BlueBananaPage()
    {
        Title = "Blue Banana";

        // Load banana matte bitmap (black on transparent)
        SKBitmap matteBitmap = BitmapExtensions.LoadBitmapResource(
            typeof(BlueBananaPage),
            "SkiaSharpFormsDemos.Media.BananaMatte.png");

        // Create a bitmap with a solid blue banana and transparent otherwise
        blueBananaBitmap = new SKBitmap(matteBitmap.Width, matteBitmap.Height);

        using (SKCanvas canvas = new SKCanvas(blueBananaBitmap))
        {
            canvas.Clear();
            canvas.DrawBitmap(matteBitmap, new SKPoint(0, 0));

            using (SKPaint paint = new SKPaint())
            {
                paint.Color = SKColors.Blue;
                paint.BlendMode = SKBlendMode.SrcIn;
                canvas.DrawPaint(paint);
            }
        }

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

        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);

        using (SKPaint paint = new SKPaint())
        {
            paint.BlendMode = SKBlendMode.Color;
            canvas.DrawBitmap(blueBananaBitmap, 
                              info.Rect, 
                              BitmapStretch.Uniform, 
                              paint: paint);
        }
    }
}
```

`PaintSurface`Обработчик рисует растровое изображение с обезьяной, которая удерживает меня. За этим кодом следует отображение `blueBananaBitmap` с помощью `SKBlendMode.Color` . С точки зрения, цветовой тон и насыщенность каждого пикселя заменяются сплошным синим цветом, который соответствует значению оттенка 240 и значению насыщенности 100. Тем не менее, яркость остается неизменной. Это означает, что в этом случае по-прежнему отображается реалистичная текстура, несмотря на новый цвет:

[![Синий](non-separable-images/BlueBanana.png "Синий")](non-separable-images/BlueBanana-Large.png#lightbox)

Попробуйте изменить режим смешения на `SKBlendMode.Saturation` . Значение «неизменность» остается желтым, но это более сильный желтый цвет. В реальных приложениях это может быть более предпочтительным, чем поворот голубого электричества.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)