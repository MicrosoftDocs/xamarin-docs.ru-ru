---
title: Эффекты пути в SkiaSharp
description: В этой статье объясняются различные эффекты SkiaSharp Path, позволяющие использовать пути для обводки и заливки, а также демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: davidbritch
ms.author: dabritch
ms.date: 07/29/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 21e06560bd67683496b10c8e8c9c3fff520fc36a
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368171"
---
# <a name="path-effects-in-skiasharp"></a>Эффекты пути в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Обнаружение различных эффектов пути, позволяющих использовать пути для обводки и заливки_

*Результатом создания пути* является экземпляр [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) класса, который создается с помощью одного из восьми статических методов создания, определенных классом. `SKPathEffect`Затем объект присваивается [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) свойству [`SKPaint`](xref:SkiaSharp.SKPaint) объекта для различных интересных эффектов, например для обводки строки с помощью небольшого реплицированного пути:

![Пример связанной цепочки](effects-images/patheffectsample.png)

Эффекты пути позволяют:

- Обводка линии с точками и тире
- Обводка линии любым заполненным контуром
- Заполнение области штриховой линиями
- Заполнение области с помощью мозаичного пути
- Округлять острые углы
- Добавление случайных "колебаний" к линиям и кривым

Кроме того, можно объединить два или более эффекта пути.

В этой статье также показано, как использовать [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath*) метод `SKPaint` для преобразования одного пути в другой путем применения свойств `SKPaint` , включая `StrokeWidth` и `PathEffect` . Это приводит к некоторым интересным приемам, таким как получение пути, который является контуром другого пути. `GetFillPath` также полезно в связи с эффектами пути.

## <a name="dots-and-dashes"></a>Точки и тире

Использование [`PathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash(System.Single[],System.Single)) метода описано в разделе [**точки и тире**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md). Первым аргументом метода является массив, содержащий четное число двух или более значений, с чередованием длиной тире и длиной зазоров между тире:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Эти значения *не* зависят от ширины штриха. Например, если толщина штриха равна 10, и требуется строка, состоящая из квадратных штрихов и квадратных пробелов, задайте `intervals` для массива значение {10, 10}. `phase`Аргумент указывает, где начинается строка в коде штриха. В этом примере, если нужно, чтобы строка начиналась с квадратного зазора, установите значение `phase` 10.

На концы штрихов влияет `StrokeCap` свойство объекта `SKPaint` . Для расширенной ширины штрихов очень часто для этого свойства устанавливается значение `SKStrokeCap.Round` , чтобы округлить концы штрихов. В этом случае значения в `intervals` массиве *не* содержат дополнительной длины, полученной в результате округления. Это означает, что для круглой точки требуется указать нулевую ширину. Для ширины штриха, равной 10, для создания линии с круглыми точками и промежутками между точками одного диаметра используйте `intervals` массив {0, 20}.

**Анимированная пунктирная текстовая** страница похожа на **текстовую** страницу, описанную в статье [**Интеграция текста и графики**](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) в том, что в ней отображаются контурные текстовые символы путем присвоения `Style` свойству `SKPaint` объекта значения `SKPaintStyle.Stroke` . Кроме того, **анимированный точечный текст** используется `SKPathEffect.CreateDash` для того, чтобы дать этому контуру точечный вид, а программа также анимирует `phase` аргумент `SKPathEffect.CreateDash` метода, чтобы точки перемещаются вокруг текстовых символов. Это страница в альбомном режиме:

[![Тройной снимок экрана анимированной точечной текстовой страницы](effects-images/animateddottedtext-small.png)](effects-images/animateddottedtext-large.png#lightbox)

[`AnimatedDottedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs)Класс начинается с определения некоторых констант, а также переопределяет `OnAppearing` методы и `OnDisappearing` для анимации:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    const string text = "DOTTED";
    const float strokeWidth = 10;
    static readonly float[] dashArray = { 0, 2 * strokeWidth };

    SKCanvasView canvasView;
    bool pageIsActive;

    public AnimatedDottedTextPage()
    {
        Title = "Animated Dotted Text";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;

        Device.StartTimer(TimeSpan.FromSeconds(1f / 60), () =>
        {
            canvasView.InvalidateSurface();
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

`PaintSurface`Обработчик начинается с создания `SKPaint` объекта для вывода текста. `TextSize`Свойство регулируется на основе ширины экрана:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create an SKPaint object to display the text
        using (SKPaint textPaint = new SKPaint
            {
                Style = SKPaintStyle.Stroke,
                StrokeWidth = strokeWidth,
                StrokeCap = SKStrokeCap.Round,
                Color = SKColors.Blue,
            })
        {
            // Adjust TextSize property so text is 95% of screen width
            float textWidth = textPaint.MeasureText(text);
            textPaint.TextSize *= 0.95f * info.Width / textWidth;

            // Find the text bounds
            SKRect textBounds = new SKRect();
            textPaint.MeasureText(text, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Animate the phase; t is 0 to 1 every second
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 1 / 1);
            float phase = -t * 2 * strokeWidth;

            // Create dotted line effect based on dash array and phase
            using (SKPathEffect dashEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                // Set it to the paint object
                textPaint.PathEffect = dashEffect;

                // And draw the text
                canvas.DrawText(text, xText, yText, textPaint);
            }
        }
    }
}
```

В конце метода `SKPathEffect.CreateDash` метод вызывается с помощью объекта `dashArray` , который определен как поле, и анимированное `phase` значение. `SKPathEffect`Экземпляру присваивается `PathEffect` свойство `SKPaint` объекта для вывода текста.

Кроме того, можно задать `SKPathEffect` объект для `SKPaint` объекта до измерения текста и его центрирования на странице. Однако в этом случае анимированные точки и тире приводят к некоторым вариациям размера отображаемого текста, и текст, как правило, вибрировало немного. (Попробуйте!)

Также обратите внимание на то, что в виде анимированных точек вокруг текстовых символов есть определенная точка в каждой замкнутой кривой, где точки кажутся невидимыми. Здесь начинается и заканчивается путь, определяющий структуру символов. Если длина пути не кратна длине шаблона штриха (в данном случае 20 пикселей), то в конце пути может уместиться только часть этого шаблона.

Длину шаблона штриха можно изменить в соответствии с длиной пути, но для этого требуется определить длину пути, метод, который рассматривается в статье [**сведения о пути и перечисление**](information.md).

Программа « **точка/тире** » выполняет анимацию самого шаблона штриха, чтобы штрихи могли разбиваться на точки, которые объединяются в виде тире.

[![Тройной снимок экрана страницы преобразования "пунктирное тире"](effects-images/dotdashmorph-small.png)](effects-images/dotdashmorph-large.png#lightbox)

[`DotDashMorphPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs)Класс переопределяет `OnAppearing` методы и `OnDisappearing` так же, как и предыдущая программа, но класс определяет `SKPaint` объект как поле:

```csharp
public class DotDashMorphPage : ContentPage
{
    const float strokeWidth = 30;
    static readonly float[] dashArray = new float[4];

    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint ellipsePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = strokeWidth,
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create elliptical path
        using (SKPath ellipsePath = new SKPath())
        {
            ellipsePath.AddOval(new SKRect(50, 50, info.Width - 50, info.Height - 50));

            // Create animated path effect
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 3 / 3);
            float phase = 0;

            if (t < 0.25f)  // 1, 0, 1, 2 --> 0, 2, 0, 2
            {
                float tsub = 4 * t;
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2 * tsub;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2;
            }
            else if (t < 0.5f)  // 0, 2, 0, 2 --> 1, 2, 1, 0
            {
                float tsub = 4 * (t - 0.25f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2 * (1 - tsub);
                phase = strokeWidth * tsub;
            }
            else if (t < 0.75f) // 1, 2, 1, 0 --> 0, 2, 0, 2
            {
                float tsub = 4 * (t - 0.5f);
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2 * tsub;
                phase = strokeWidth * (1 - tsub);
            }
            else               // 0, 2, 0, 2 --> 1, 0, 1, 2
            {
                float tsub = 4 * (t - 0.75f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2 * (1 - tsub);
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2;
            }

            using (SKPathEffect pathEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                ellipsePaint.PathEffect = pathEffect;
                canvas.DrawPath(ellipsePath, ellipsePaint);
            }
        }
    }
}
```

`PaintSurface`Обработчик создает путь в виде эллипса на основе размера страницы и выполняет длинный раздел кода, который задает `dashArray` `phase` переменные и. Так как анимированная переменная находится в `t` диапазоне от 0 до 1, `if` блоки разбиваются на четыре квартала и в каждом из этих кварталов также находятся в `tsub` диапазоне от 0 до 1. В самом итоге программа создает `SKPathEffect` и присваивает ей `SKPaint` объект для рисования.

## <a name="from-path-to-path"></a>От пути к пути

[`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,System.Single))Метод `SKPaint` преобразует один путь в другой на основе параметров в `SKPaint` объекте. Чтобы увидеть, как это работает, замените `canvas.DrawPath` вызов в предыдущей программе следующим кодом:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

В этом новом коде `GetFillPath` вызов преобразует `ellipsePath` (который является просто овалом) в `newPath` , который затем отображается с помощью `newPaint` . `newPaint`Объект создается со всеми параметрами свойств по умолчанию, за исключением того, что `Style` свойство задается на основе логического возвращаемого значения `GetFillPath` .

Визуальные элементы идентичны, за исключением цвета, который задается в, `ellipsePaint` но не `newPaint` . Вместо простого эллипса, определенного в `ellipsePath` , `newPath` содержит множество контуров пути, определяющих ряд точек и штрихов. Это результат применения различных свойств (в частности,, `ellipsePaint` `StrokeWidth` `StrokeCap` и `PathEffect` ) к `ellipsePath` и помещения результирующего пути в `newPath` . `GetFillPath`Метод возвращает логическое значение, указывающее, должен ли быть заполнен целевой путь. в этом примере возвращаемое значение предназначено `true` для заполнения пути.

Попробуйте изменить `Style` параметр в `newPaint` на, и вы увидите контуры `SKPaintStyle.Stroke` отдельных путей, которые обработаны линией толщиной в один пиксель.

## <a name="stroking-with-a-path"></a>Обводка с помощью контура

[`SKPathEffect.Create1DPath`](xref:SkiaSharp.SKPathEffect.Create1DPath(SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPath1DPathEffectStyle))Метод концептуально похож на `SKPathEffect.CreateDash` , за исключением того, что вы указываете путь, а не шаблон штрихов и пропусков. Этот путь реплицируется несколько раз для обводки линии или кривой.

Синтаксис:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

В общем случае путь, который вы передаете, `Create1DPath` будет небольшим и центрироваться вокруг точки (0, 0). `advance`Параметр указывает расстояние между центрами пути по мере репликации пути в строке. Обычно для этого аргумента задана Приблизительная ширина пути. `phase`Аргумент играет ту же роль здесь, как и в `CreateDash` методе.

[`SKPath1DPathEffectStyle`](xref:SkiaSharp.SKPath1DPathEffectStyle)Имеет три члена:

- `Translate`
- `Rotate`
- `Morph`

`Translate`Элемент заставляет путь оставаться на той же ориентации, что и репликация вдоль линии или кривой. Для `Rotate` параметр path поворачивается на основе тангенса кривой. У пути обычная ориентация для горизонтальных линий. `Morph` аналогичен `Rotate` за исключением того, что сам путь также является изогнутым, чтобы соответствовать кривизне линии, в которой выполняется штрих.

Эти три варианта показаны на странице **эффекты по одномерному пути** . Файл [**онедименсионалпасеффектпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) определяет средство выбора, содержащее три элемента, соответствующие трем элементам перечисления:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.OneDimensionalPathEffectPage"
             Title="1D Path Effect">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="effectStylePicker"
                Title="Effect Style"
                Grid.Row="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Translate</x:String>
                    <x:String>Rotate</x:String>
                    <x:String>Morph</x:String>
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1" />
    </Grid>
</ContentPage>
```

Файл кода программной части [**OneDimensionalPathEffectPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) определяет три `SKPathEffect` объекта как поля. Все они создаются с помощью `SKPathEffect.Create1DPath` `SKPath` объектов, созданных с помощью `SKPath.ParseSvgPathData` . Первый является простым полем, второй — ромбом, а третий — прямоугольником. Они используются для демонстрации трех стилей эффектов:

```csharp
public partial class OneDimensionalPathEffectPage : ContentPage
{
    SKPathEffect translatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 -10 L 10 -10, 10 10, -10 10 Z"),
                                  24, 0, SKPath1DPathEffectStyle.Translate);

    SKPathEffect rotatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 0 L 0 -10, 10 0, 0 10 Z"),
                                  20, 0, SKPath1DPathEffectStyle.Rotate);

    SKPathEffect morphPathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -25 -10 L 25 -10, 25 10, -25 10 Z"),
                                  55, 0, SKPath1DPathEffectStyle.Morph);

    SKPaint pathPaint = new SKPaint
    {
        Color = SKColors.Blue
    };

    public OneDimensionalPathEffectPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath path = new SKPath())
        {
            path.MoveTo(new SKPoint(0, 0));
            path.CubicTo(new SKPoint(2 * info.Width, info.Height),
                         new SKPoint(-info.Width, info.Height),
                         new SKPoint(info.Width, 0));

            switch ((string)effectStylePicker.SelectedItem))
            {
                case "Translate":
                    pathPaint.PathEffect = translatePathEffect;
                    break;

                case "Rotate":
                    pathPaint.PathEffect = rotatePathEffect;
                    break;

                case "Morph":
                    pathPaint.PathEffect = morphPathEffect;
                    break;
            }

            canvas.DrawPath(path, pathPaint);
        }
    }
}
```

`PaintSurface`Обработчик создает кривую Безье, которая циклически выполняет цикл, и обращается к средству выбора, чтобы определить, какие из `PathEffect` них следует использовать для обводки. Три параметра — `Translate` , `Rotate` и `Morph` — отображаются слева направо:

[![Тройной снимок экрана со страницей эффектов для пути 1D](effects-images/1dpatheffect-small.png)](effects-images/1dpatheffect-large.png#lightbox)

Путь, указанный в `SKPathEffect.Create1DPath` методе, всегда заполняется. Путь, указанный в `DrawPath` методе, всегда обводки, если `SKPaint` свойство объекта имеет `PathEffect` значение, равное «одномерному пути». Обратите внимание, что `pathPaint` объект не имеет `Style` параметра, который обычно по умолчанию имеет значение `Fill` , но контур передается независимо от него.

Поле, используемое в `Translate` примере, имеет значение 20 пикселей в квадрате, а `advance` аргумент имеет значение 24. Это различие приводит к разрыву между полями при горизонтальном или вертикальном направлении, но прямоугольники перекрываются немного, если линия является диагональной, поскольку по диагонали прямоугольника отображается 28,3 пикселей.

Форма ромба в `Rotate` примере также имеет значение 20 пикселей в ширину. Параметр `advance` имеет значение 20, чтобы точки продолжали касаться, когда ромб поворачивается вместе с кривизной линии.

Фигура прямоугольника в `Morph` примере имеет размер 50 пикселей в ширину и `advance` значение 55, чтобы сделать небольшой зазор между прямоугольниками, так как они изогнуты на кривой Безье.

Если `advance` аргумент меньше размера пути, реплицированные пути могут перекрываться. Это может привести к некоторым интересным последствиям. На странице « **связанная цепочка** » отображается ряд пересекающихся кругов, похожих на связанную цепочку, которая зависает в отличительной форме катенари:

[![Тройной снимок экрана страницы связанной цепочки](effects-images/linkedchain-small.png)](effects-images/linkedchain-large.png#lightbox)

Взгляните очень близко, и вы увидите, что они на самом деле не являются круговыми. Каждая ссылка в цепочке — это две дуги, размер и положение, поэтому они могут соединяться с соседними ссылками.

Цепочка или кабель равномерного распределения зависает в форме катенари. Дуга, построенная в виде инвертированного катенариного преимущества от равномерного распределения давления от веса в виде Arch. Катенари имеет довольно простое математическое описание:

`y = a · cosh(x / a)`

*Cosh* является гиперболическим косинусом. Для *x* , равного 0, *cosh* равен нулю, а *y* равно *a*. Это центр катенари. Как и в *косинусе* , *cosh* считается *четным* , а это означает, что *cosh (– x)* равен *cosh (x)* , а значения увеличиваются для увеличения положительных или отрицательных аргументов. Эти значения описывают кривые, которые формируют стороны катенари.

Нахождение правильного *значения для катенари к* размерам страницы телефона не является прямым вычислением. Если *w* и *h* — ширина и высота прямоугольника, оптимальное значение *выражения удовлетворяет следующему* уравнению:

`cosh(w / 2 / a) = 1 + h / a`

Следующий метод в [`LinkedChainPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) классе включает это равенство, ссылаясь на два выражения слева и справа от знака равенства `left` и `right` . Для небольших *значений,* `left` больше `right` ; для больших значений объекта *a* `left` меньше `right` . `while`Цикл ограничивается в зависимости от оптимального значения. *a*

```csharp
float FindOptimumA(float width, float height)
{
    Func<float, float> left = (float a) => (float)Math.Cosh(width / 2 / a);
    Func<float, float> right = (float a) => 1 + height / a;

    float gtA = 1;         // starting value for left > right
    float ltA = 10000;     // starting value for left < right

    while (Math.Abs(gtA - ltA) > 0.1f)
    {
        float avgA = (gtA + ltA) / 2;

        if (left(avgA) < right(avgA))
        {
            ltA = avgA;
        }
        else
        {
            gtA = avgA;
        }
    }

    return (gtA + ltA) / 2;
}
```

`SKPath`Объект для ссылок создается в конструкторе класса, а результирующий `SKPathEffect` объект устанавливается в `PathEffect` свойство `SKPaint` объекта, которое хранится в виде поля:

```csharp
public class LinkedChainPage : ContentPage
{
    const float linkRadius = 30;
    const float linkThickness = 5;

    Func<float, float, float> catenary = (float a, float x) => (float)(a * Math.Cosh(x / a));

    SKPaint linksPaint = new SKPaint
    {
        Color = SKColors.Silver
    };

    public LinkedChainPage()
    {
        Title = "Linked Chain";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the individual links
        SKRect outer = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
        SKRect inner = outer;
        inner.Inflate(-linkThickness, -linkThickness);

        using (SKPath linkPath = new SKPath())
        {
            linkPath.AddArc(outer, 55, 160);
            linkPath.ArcTo(inner, 215, -160, false);
            linkPath.Close();

            linkPath.AddArc(outer, 235, 160);
            linkPath.ArcTo(inner, 395, -160, false);
            linkPath.Close();

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(linkPath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);
        }
    }
    ...
}
```

Основным заданием `PaintSurface` обработчика является создание пути для самого катенари. Определив оптимальный *объект* и сохранив его в `optA` переменной, также необходимо вычислить смещение в верхней части окна. Затем он может накапливать коллекцию `SKPoint` значений для катенари, превратить его в путь и нарисовать путь с ранее созданным `SKPaint` объектом:

```csharp
public class LinkedChainPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Width and height of catenary
        int width = info.Width;
        float height = info.Height - linkRadius;

        // Find the optimum 'a' for this width and height
        float optA = FindOptimumA(width, height);

        // Calculate the vertical offset for that value of 'a'
        float yOffset = catenary(optA, -width / 2);

        // Create a path for the catenary
        SKPoint[] points = new SKPoint[width];

        for (int x = 0; x < width; x++)
        {
            points[x] = new SKPoint(x, yOffset - catenary(optA, x - width / 2));
        }

        using (SKPath path = new SKPath())
        {
            path.AddPoly(points, false);

            // And render that path with the linksPaint object
            canvas.DrawPath(path, linksPaint);
        }
    }
    ...
}
```

Эта программа определяет путь, используемый в, `Create1DPath` чтобы точка (0, 0) была в центре. Это кажется разумным, поскольку точка (0, 0) пути соответствует линии или кривой, в которой он оформлен. Тем не менее для некоторых специальных эффектов можно использовать нецентрированную точку (0, 0).

Страница транспортер **конвейера** создает траекторию, похожую на облонг конвейерной транспортер с изогнутыми верхними и нижними краями, размер которых соответствует размерам окна. Этот контур `SKPaint` обходится простым объектом с 20 пикселами в ширину и оттенком серого, а затем перештрихом на другой `SKPaint` объект с `SKPathEffect` объектом, ссылающимся на путь, похожий на маленький контейнер:

[![Тройной снимок экрана: страница "лента конвейера"](effects-images/conveyorbelt-small.png)](effects-images/conveyorbelt-large.png#lightbox)

Точка (0, 0) пути к контейнеру является маркером, поэтому при `phase` анимации аргумента контейнеры кажутся частью конвейерной транспортера, возможно, скупинг воды в нижней части и выводит ее в начало.

[`ConveyorBeltPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs)Класс реализует анимацию с переопределениями `OnAppearing` `OnDisappearing` методов и. Путь к контейнеру определяется в конструкторе страницы:

```csharp
public class ConveyorBeltPage : ContentPage
{
    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint conveyerPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 20,
        Color = SKColors.DarkGray
    };

    SKPath bucketPath = new SKPath();

    SKPaint bucketsPaint = new SKPaint
    {
        Color = SKColors.BurlyWood,
    };

    public ConveyorBeltPage()
    {
        Title = "Conveyor Belt";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the bucket starting with the handle
        bucketPath.AddRect(new SKRect(-5, -3, 25, 3));

        // Sides
        bucketPath.AddRoundedRect(new SKRect(25, -19, 27, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);
        bucketPath.AddRoundedRect(new SKRect(63, -19, 65, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);

        // Five slats
        for (int i = 0; i < 5; i++)
        {
            bucketPath.MoveTo(25, -19 + 8 * i);
            bucketPath.LineTo(25, -13 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.CounterClockwise, 65, -13 + 8 * i);
            bucketPath.LineTo(65, -19 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.Clockwise, 25, -19 + 8 * i);
            bucketPath.Close();
        }

        // Arc to suggest the hidden side
        bucketPath.MoveTo(25, -17);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.Clockwise, 65, -17);
        bucketPath.LineTo(65, -19);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.CounterClockwise, 25, -19);
        bucketPath.Close();

        // Make it a little bigger and correct the orientation
        bucketPath.Transform(SKMatrix.MakeScale(-2, 2));
        bucketPath.Transform(SKMatrix.MakeRotationDegrees(90));
    }
    ...
```

Код создания контейнера завершается двумя преобразованиями, которые делают контейнер немного больше и преобразуются в сторону. Применение этих преобразований стало проще, чем настройка всех координат в предыдущем коде.

`PaintSurface`Обработчик начинается с определения пути для самой лента конвейера. Это просто пара линий и половина кругов, которые рисуются с темно-серой линией с шириной в 20 пикселей:

```csharp
public class ConveyorBeltPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float width = info.Width / 3;
        float verticalMargin = width / 2 + 150;

        using (SKPath conveyerPath = new SKPath())
        {
            // Straight verticals capped by semicircles on top and bottom
            conveyerPath.MoveTo(width, verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, 2 * width, verticalMargin);
            conveyerPath.LineTo(2 * width, info.Height - verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, width, info.Height - verticalMargin);
            conveyerPath.Close();

            // Draw the conveyor belt itself
            canvas.DrawPath(conveyerPath, conveyerPaint);

            // Calculate spacing based on length of conveyer path
            float length = 2 * (info.Height - 2 * verticalMargin) +
                           2 * ((float)Math.PI * width / 2);

            // Value will be somewhere around 200
            float spacing = length / (float)Math.Round(length / 200);

            // Now animate the phase; t is 0 to 1 every 2 seconds
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 2 / 2);
            float phase = -t * spacing;

            // Create the buckets PathEffect
            using (SKPathEffect bucketsPathEffect =
                        SKPathEffect.Create1DPath(bucketPath, spacing, phase,
                                                  SKPath1DPathEffectStyle.Rotate))
            {
                // Set it to the Paint object and draw the path again
                bucketsPaint.PathEffect = bucketsPathEffect;
                canvas.DrawPath(conveyerPath, bucketsPaint);
            }
        }
    }
}
```

Логика рисования конвейерной транспортера не работает в альбомном режиме.

Контейнеры должны располагаться на расстоянии около 200 пикселей в транспортере конвейера. Однако лента конвейера, вероятно, не кратна 200 пикселям, что означает, что, как `phase` аргумент `SKPathEffect.Create1DPath` является анимированным, контейнеры будут открываться и выходить из них.

По этой причине программа сначала вычисляет значение с именем `length` , которое является длиной конвейерной транспортера. Так как лента конвейера состоит из прямых линий и разделенных кругов, это простое вычисление. Затем количество сегментов вычисляется делением `length` на 200. Это значение округляется до ближайшего целого числа, которое затем делится на `length` . Результатом является интервал для целого числа сегментов. `phase`Аргумент — это просто часть этого.

## <a name="from-path-to-path-again"></a>От пути к пути

В нижней части `DrawSurface` обработчика **конвейерной транспортера** закомментируйте `canvas.DrawPath` вызов и замените его следующим кодом:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

Как и в предыдущем примере `GetFillPath` , вы увидите, что результаты одинаковы, за исключением цвета. После выполнения `GetFillPath` `newPath` объект содержит несколько копий пути к контейнеру, каждый из которых расположен в той же области, что и анимация, на момент вызова.

## <a name="hatching-an-area"></a>Штриховки области

[`SKPathEffect.Create2DLines`](xref:SkiaSharp.SKPathEffect.Create2DLine(System.Single,SkiaSharp.SKMatrix))Метод заполняет область параллельными линиями, часто называемыми *штриховыми линиями*. Метод имеет следующий синтаксис:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width`Аргумент задает толщину штрихов штрихов линий. `matrix`Параметр представляет собой сочетание масштабирования и необязательного вращения. Коэффициент масштабирования указывает шаг пикселя, который СКИА использует для пробелов штриховыми линиями. Разделение строк является коэффициентом масштабирования, за вычетом `width` аргумента. Если коэффициент масштабирования меньше или равен `width` значению, то между линиями штриховки не будет пробела, а область будет заполнена. Укажите одно и то же значение для горизонтального и вертикального масштабирования.

По умолчанию линии штриховки горизонтальны. Если `matrix` параметр содержит поворот, линии штриховки поворачиваются по часовой стрелке.

На странице **Заливка штриховки** показан этот результат. [`HatchFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs)Класс определяет три эффекта пути как поля, первый для горизонтальных линий штриховки с шириной в 3 пикселя с коэффициентом масштабирования, указывающий на то, что они расположены на расстоянии 6 пикселей друг от друга. Таким образом, разделение линий происходит в три пикселя. Второй контур предназначен для вертикальных штрихов, ширина которых составляет 6 пикселей, разбивается на 24 пикселя (так что разделение составляет 18 пикселей), а третья — диагональные линии 12 пикселей в ширину 36 пикселей.

```csharp
public class HatchFillPage : ContentPage
{
    SKPaint fillPaint = new SKPaint();

    SKPathEffect horzLinesPath = SKPathEffect.Create2DLine(3, SKMatrix.MakeScale(6, 6));

    SKPathEffect vertLinesPath = SKPathEffect.Create2DLine(6,
        Multiply(SKMatrix.MakeRotationDegrees(90), SKMatrix.MakeScale(24, 24)));

    SKPathEffect diagLinesPath = SKPathEffect.Create2DLine(12,
        Multiply(SKMatrix.MakeScale(36, 36), SKMatrix.MakeRotationDegrees(45)));

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Обратите внимание на `Multiply` метод Matrix. Так как коэффициенты горизонтального и вертикального масштабирования одинаковы, порядок, в котором умножаются матрицы масштабирования и вращения, не имеет значения.

`PaintSurface`Обработчик использует эти три эффекта контура с тремя различными цветами в сочетании с `fillPaint` , чтобы залить размер скругленного прямоугольника в соответствии со страницей. Свойство, заданное параметром, `Style` `fillPaint` игнорируется; если `SKPaint` объект включает в себя эффекты пути, созданные из `SKPathEffect.Create2DLine` , область заполняется независимо от того, что:

```csharp
public class HatchFillPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath roundRectPath = new SKPath())
        {
            // Create a path
            roundRectPath.AddRoundedRect(
                new SKRect(50, 50, info.Width - 50, info.Height - 50), 100, 100);

            // Horizontal hatch marks
            fillPaint.PathEffect = horzLinesPath;
            fillPaint.Color = SKColors.Red;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Vertical hatch marks
            fillPaint.PathEffect = vertLinesPath;
            fillPaint.Color = SKColors.Blue;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Diagonal hatch marks -- use clipping
            fillPaint.PathEffect = diagLinesPath;
            fillPaint.Color = SKColors.Green;

            canvas.Save();
            canvas.ClipPath(roundRectPath);
            canvas.DrawRect(new SKRect(0, 0, info.Width, info.Height), fillPaint);
            canvas.Restore();

            // Outline the path
            canvas.DrawPath(roundRectPath, strokePaint);
        }
    }
    ...
}
```

Если вы внимательно взгляните на результаты, вы увидите, что красная и синяя штриховые линии не имеют точности к скругленному прямоугольнику. (Это, очевидно, характеристика базового кода СКИА.) Если это неудовлетворительно, для диагональных линий в зеленой линии отображается альтернативный подход: скругленный прямоугольник используется в качестве обтравочного контура, а линии штриховки рисуются на всей странице.

`PaintSurface`Обработчик завершается вызовом просто обводки скругленного прямоугольника, чтобы можно было увидеть расхождение с красной и синей линиями.

[![Тройной снимок экрана со страницей заливки штриховки](effects-images/hatchfill-small.png)](effects-images/hatchfill-large.png#lightbox)

Экран Android выглядит не так: масштабирование на снимке экрана привело к тому, что тонкие красные линии и тонкие пробелы объединяются в красные и более широкие.

## <a name="filling-with-a-path"></a>Заполнение путем

[`SKPathEffect.Create2DPath`](xref:SkiaSharp.SKPathEffect.Create2DPath(SkiaSharp.SKMatrix,SkiaSharp.SKPath))Позволяет заполнить область контуром, который реплицируется горизонтально и вертикально, в результате мозаичного заполнения области:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix`Коэффициенты масштабирования указывают горизонтальный и вертикальный пробелы в реплицированном пути. Но вы не можете повернуть путь с помощью этого `matrix` аргумента. Если нужно поворачивать путь, поверните сам путь с помощью `Transform` метода, определенного параметром `SKPath` .

Реплицированный путь обычно выстраивается по левому и верхнему краю экрана, а не к заполняемой области. Это поведение можно переопределить, предоставив коэффициенты перевода между 0 и коэффициентами масштабирования, чтобы указать горизонтальное и вертикальное смещение от левой и верхней сторон.

На странице **Заливка на плитке пути** показан этот результат. Путь, используемый для мозаичного заполнения области, определяется как поле в [`PathTileFillPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) классе. Координаты по горизонтали и вертикали находятся в диапазоне от – 40 до 40. Это означает, что этот путь составляет 80 пикселей в квадрат:

```csharp
public class PathTileFillPage : ContentPage
{
    SKPath tilePath = SKPath.ParseSvgPathData(
        "M -20 -20 L 2 -20, 2 -40, 18 -40, 18 -20, 40 -20, " +
        "40 -12, 20 -12, 20 12, 40 12, 40 40, 22 40, 22 20, " +
        "-2 20, -2 40, -20 40, -20 8, -40 8, -40 -8, -20 -8 Z");
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Red;

            using (SKPathEffect pathEffect =
                   SKPathEffect.Create2DPath(SKMatrix.MakeScale(64, 64), tilePath))
            {
                paint.PathEffect = pathEffect;

                canvas.DrawRoundRect(
                    new SKRect(50, 50, info.Width - 50, info.Height - 50),
                    100, 100, paint);
            }
        }
    }
}
```

В `PaintSurface` обработчике `SKPathEffect.Create2DPath` вызовы устанавливают горизонтальный и вертикальный пробелы равными 64, что приводит к перекрытию трехмерных плиток размером 80 пикселей. К счастью, путь похож на элемент головоломки, так как он прекрасно располагается рядом с соседними плитками:

[![Тройной снимок экрана со страницей заливки плитки пути](effects-images/pathtilefill-small.png)](effects-images/pathtilefill-large.png#lightbox)

Масштабирование с исходного снимка экрана вызывает некоторое искажение, особенно на экране Android.

Обратите внимание, что эти плитки всегда отображаются целиком и никогда не усекаются. На первых двух снимках экрана не очевидно, что заполняемая область является скругленным прямоугольником. Если вы хотите усечь эти плитки в определенной области, используйте обтравочный контур.

Попробуйте задать `Style` для свойства объекта значение `SKPaint` `Stroke` , и вы увидите отдельные плитки, которые будут выровняться, а не заполнены.

Можно также заполнить область мозаичным растровым изображением, как показано в статье [**SkiaSharp Bitmap**](../effects/shaders/bitmap-tiling.md).

## <a name="rounding-sharp-corners"></a>Округление острых углов

Программа **округления хептагон** , представленная в [**трех способах рисования статьи дуги**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) , использовала дугу касательно для изгиба точек на семь сторонах фигуры. **Другая скругленная хептагон** страница показывает гораздо более простой подход, использующий эффекты пути, созданные на основе [`SKPathEffect.CreateCorner`](xref:SkiaSharp.SKPathEffect.CreateCorner(System.Single)) метода:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

Хотя для одного аргумента задано имя `radius` , необходимо задать для него половину радиуса нужного угла. (Это характеристика базового кода СКИА.)

Вот `PaintSurface` обработчик в [`AnotherRoundedHeptagonPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) классе:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);
    SKPoint[] vertices = new SKPoint[numVertices];
    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    float cornerRadius = 100;

    // Create the path
    using (SKPath path = new SKPath())
    {
        path.AddPoly(vertices, true);

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            // Set argument to half the desired corner radius!
            paint.PathEffect = SKPathEffect.CreateCorner(cornerRadius / 2);

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);

            // Uncomment DrawCircle call to verify corner radius
            float offset = cornerRadius / (float)Math.Sin(Math.PI * (numVertices - 2) / numVertices / 2);
            paint.Color = SKColors.Green;
            // canvas.DrawCircle(vertices[0].X, vertices[0].Y + offset, cornerRadius, paint);
        }
    }
}
```

Этот результат можно использовать с обводки или заливкой на основе `Style` свойства `SKPaint` объекта. Здесь он работает:

[![Тройной снимок экрана другой скругленной Хептагон страницы](effects-images/anotherroundedheptagon-small.png)](effects-images/anotherroundedheptagon-large.png#lightbox)

Вы увидите, что этот округленный хептагон идентичен предыдущей программе. Если вам нужно более убедительно, что радиус преломления действительно 100, а не 50, заданный в `SKPathEffect.CreateCorner` вызове, можно снять комментарий к завершающей инструкции в программе и увидеть круг, заключенный в конце 100-радиуса в углу.

## <a name="random-jitter"></a>Случайная колебание

Иногда безупречные линии компьютерной графики не совсем настолько, насколько вам нужны, и требуется небольшая случайность. В этом случае необходимо попробовать [`SKPathEffect.CreateDiscrete`](xref:SkiaSharp.SKPathEffect.CreateDiscrete(System.Single,System.Single,System.UInt32)) метод:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

Этот результат пути можно использовать для обводки или заполнения. Строки разделены на Соединенные сегменты — Приблизительная длина, заданная параметром, `segLength` и расширение в разных направлениях. Степень отклонения от исходной строки задается параметром `deviation` .

Последний аргумент — это начальное значение, используемое для создания последовательности псевдо-Random, используемой для этого результата. Результат противодействия будет немного отличаться для разных начальных значений. Аргумент имеет нулевое значение по умолчанию, что означает, что при запуске программы этот результат будет одинаковым. Если требуется разная колебание при перерисовки экрана, можно задать начальное `Millisecond` `DataTime.Now` значение свойства значения (например,).

Страница **эксперимента с нарушением колебаний** позволяет экспериментировать с различными значениями при обводках прямоугольника:

[![Тройной снимок экрана страницы Життерексперимент](effects-images/jitterexperiment-small.png)](effects-images/jitterexperiment-large.png#lightbox)

Эта программа проста. Файл [**життерекспериментпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) создает экземпляры двух `Slider` элементов и `SKCanvasView` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.JitterExperimentPage"
             Title="Jitter Experiment">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Minimum" Value="0" />
                    <Setter Property="Maximum" Value="100" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="segLengthSlider"
                Grid.Row="0"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference segLengthSlider},
                              Path=Value,
                              StringFormat='Segment Length = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="deviationSlider"
                Grid.Row="2"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference deviationSlider},
                              Path=Value,
                              StringFormat='Deviation = {0:F0}'}"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

`PaintSurface`Обработчик в файле кода программной части [**JitterExperimentPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) вызывается при каждом `Slider` изменении значения. Он вызывает `SKPathEffect.CreateDiscrete` использование двух `Slider` значений и использует его для обводки прямоугольника:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float segLength = (float)segLengthSlider.Value;
    float deviation = (float)deviationSlider.Value;

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.StrokeWidth = 5;
        paint.Color = SKColors.Blue;

        using (SKPathEffect pathEffect = SKPathEffect.CreateDiscrete(segLength, deviation))
        {
            paint.PathEffect = pathEffect;

            SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Этот результат можно использовать для заполнения. в этом случае контур заполненной области будет подвергаться этим случайным отклонениям. На странице **текст с нарушением колебаний** показано, как использовать этот результат пути для вывода текста. Большая часть кода в `PaintSurface` обработчике [`JitterTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) класса предназначена для изменения размера и центрирования текста:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "FUZZY";

    using (SKPaint textPaint = new SKPaint())
    {
        textPaint.Color = SKColors.Purple;
        textPaint.PathEffect = SKPathEffect.CreateDiscrete(3f, 10f);

        // Adjust TextSize property so text is 95% of screen width
        float textWidth = textPaint.MeasureText(text);
        textPaint.TextSize *= 0.95f * info.Width / textWidth;

        // Find the text bounds
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

В этом режиме он работает в альбомной ориентации:

[![Тройной снимок экрана страницы Життертекст](effects-images/jittertext-small.png)](effects-images/jittertext-large.png#lightbox)

## <a name="path-outlining"></a>Структурирование пути

Вы уже видели два маленьких примера [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath*) метода `SKPaint` , который существует в двух версиях:

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale = 1)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale = 1)
```

Требуются только первые два аргумента. Метод обращается к пути, на который ссылается `src` аргумент, изменяет данные пути на основе свойств обводки в `SKPaint` объекте (включая `PathEffect` свойство), а затем записывает результаты в `dst` путь. `resScale`Параметр позволяет уменьшить точность, чтобы создать меньший путь назначения, а `cullRect` аргумент может исключить контуры за пределами прямоугольника.

Один из основных способов использования этого метода не влияет на эффекты пути. Если `SKPaint` `Style` для объекта свойство имеет значение `SKPaintStyle.Stroke` , и не имеет *not* `PathEffect` набора, то `GetFillPath` создает путь, представляющий *контур* исходного пути, как если бы он был штриховкой свойств Paint.

Например, если `src` контур представляет собой простую окружность радиуса 500, а `SKPaint` объект задает ширину штриха 100, то `dst` путь становится двумя концентрическими круговами, один с радиусом 450, а другой — с радиусом 550. Метод вызывается, `GetFillPath` так как заливка этого `dst` пути совпадает с обводками `src` контура. Но можно также обчертировать `dst` контур, чтобы увидеть контуры пути.

Это демонстрируется при **касании** контура. Объекты `SKCanvasView` и `TapGestureRecognizer` создаются в файле [**таптуутлинесепаспаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) . Файл кода программной части [**TapToOutlineThePathPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) определяет три `SKPaint` объекта как поля, два для обводки с шириной штрихов 100 и 20, а третий для заполнения:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    bool outlineThePath = false;

    SKPaint redThickStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 100
    };

    SKPaint redThinStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 20
    };

    SKPaint blueFill = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    public TapToOutlineThePathPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewTapped(object sender, EventArgs args)
    {
        outlineThePath ^= true;
        (sender as SKCanvasView).InvalidateSurface();
    }
    ...
}
```

Если экран не был касанием, `PaintSurface` обработчик использует `blueFill` `redThickStroke` объекты и для отображения циклического пути:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circlePath = new SKPath())
        {
            circlePath.AddCircle(info.Width / 2, info.Height / 2,
                                 Math.Min(info.Width / 2, info.Height / 2) -
                                 redThickStroke.StrokeWidth);

            if (!outlineThePath)
            {
                canvas.DrawPath(circlePath, blueFill);
                canvas.DrawPath(circlePath, redThickStroke);
            }
            else
            {
                using (SKPath outlinePath = new SKPath())
                {
                    redThickStroke.GetFillPath(circlePath, outlinePath);

                    canvas.DrawPath(outlinePath, blueFill);
                    canvas.DrawPath(outlinePath, redThinStroke);
                }
            }
        }
    }
}
```

Круг заполняется и обводки, как и следовало бы ожидать:

[![Тройной снимок экрана с обычным касанием для структурирования страницы пути](effects-images/taptooutlinethepathnormal-small.png)](effects-images/taptooutlinethepathnormal-large.png#lightbox)

При касании экрана устанавливается `outlineThePath` в значение `true` , и `PaintSurface` обработчик создает новый `SKPath` объект и использует его в качестве конечного пути при вызове в `GetFillPath` `redThickStroke` объекте Paint. Затем этот конечный путь заполняется и обводки `redThinStroke` , что приводит к следующему результату:

[![Тройной снимок экрана со структурным касанием для структурирования страницы пути](effects-images/taptooutlinethepathoutlined-small.png)](effects-images/taptooutlinethepathoutlined-large.png#lightbox)

Два красного кружка четко указывают на то, что исходный круглый контур был преобразован в два круга.

Этот метод может быть очень полезен при разработке путей для использования в `SKPathEffect.Create1DPath` методе. Пути, указанные в этих методах, всегда заполняются при репликации путей. Если вы не хотите, чтобы весь путь был заполнен, необходимо тщательно определить контуры.

Например, в примере « **связанная цепочка** » ссылки были определены с помощью ряда четырех дуг, каждая из которых основана на двух радиусах, чтобы структурировать область контура для заполнения. Можно заменить код в `LinkedChainPage` классе на немного иначе.

Сначала необходимо переопределить `linkRadius` константу:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath`Теперь это просто две дуги, основанные на одном радиусе, с нужными начальными углами и углами поворота:

```csharp
using (SKPath linkPath = new SKPath())
{
    SKRect rect = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
    linkPath.AddArc(rect, 55, 160);
    linkPath.AddArc(rect, 235, 160);

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = linkThickness;

        using (SKPath outlinePath = new SKPath())
        {
            strokePaint.GetFillPath(linkPath, outlinePath);

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(outlinePath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);

        }

    }
}
```

`outlinePath`Затем объект является получателем контура `linkPath` при его обводки свойствами, указанными в `strokePaint` .

Другой пример, использующий этот метод, будет находиться рядом с путем, используемым в методе.

## <a name="combining-path-effects"></a>Объединение эффектов контуров

Два итоговых статических метода создания `SKPathEffect` — [`SKPathEffect.CreateSum`](xref:SkiaSharp.SKPathEffect.CreateSum(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) и [`SKPathEffect.CreateCompose`](xref:SkiaSharp.SKPathEffect.CreateCompose(SkiaSharp.SKPathEffect,SkiaSharp.SKPathEffect)) :

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Оба эти метода объединяют два эффекта пути для создания эффекта составного контура. `CreateSum`Метод создает эффект пути, похожий на два эффекта пути, применяемых отдельно, в то время как `CreateCompose` применяет один эффект пути ( `inner` ), а затем применяет `outer` к этому.

Вы уже видели, как `GetFillPath` метод `SKPaint` может преобразовать один путь в другой путь на основе `SKPaint` свойств (включая `PathEffect` ), поэтому он не должен быть *слишком* Mysterious, как `SKPaint` объект может выполнить эту операцию дважды с двумя эффектами пути, указанными `CreateSum` в `CreateCompose` методах или.

Одним из очевидных применений `CreateSum` является определение `SKPaint` объекта, который заполняет контур с помощью одного контура, и обводки контура с другим результатом. Это продемонстрировано в образце " **кошки в кадре** ", который отображает массив кошки в кадре с зубчатыми краями:

[![Тройной снимок экрана со страницей "кошки на рамке"](effects-images/catsinframe-small.png)](effects-images/catsinframe-large.png#lightbox)

[`CatsInFramePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs)Класс начинается с определения нескольких полей. Первое поле из класса можно распознать [`PathDataCatPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) из статьи данных о [**пути SVG**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) . Второй путь основан на линии и дуги для шаблона зубца в кадре:

```csharp
public class CatsInFramePage : ContentPage
{
    // From PathDataCatPage.cs
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint catStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5
    };

    SKPath scallopPath =
        SKPath.ParseSvgPathData("M 0 0 L 50 0 A 60 60 0 0 1 -50 0 Z");

    SKPaint framePaint = new SKPaint
    {
        Color = SKColors.Black
    };
    ...
}
```

`catPath`Может использоваться в `SKPathEffect.Create2DPath` методе, если `SKPaint` `Style` свойство объекта имеет значение `Stroke` . Однако если используется `catPath` непосредственно в этой программе, будет заполнен весь заголовок Cat, а блочной даже не станет видимым. (Попробуйте!) Необходимо получить структуру этого пути и использовать этот контур в `SKPathEffect.Create2DPath` методе.

Конструктор выполняет это задание. Сначала он применяет два преобразования для `catPath` перемещения точки (0, 0) в центр и масштабирует его по размеру. `GetFillPath` Получает все контуры в `outlinedCatPath` , и этот объект используется в `SKPathEffect.Create2DPath` вызове. Коэффициенты масштабирования в `SKMatrix` значении немного больше, чем горизонтальный и вертикальный размер объекта Cat, чтобы предоставить небольшой буфер между плитками, тогда как коэффициенты перевода были довольно сложны, так что в левом верхнем углу фрейма отображается полный объект Cat:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    public CatsInFramePage()
    {
        Title = "Cats in Frame";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Move (0, 0) point to center of cat path
        catPath.Transform(SKMatrix.MakeTranslation(-240, -175));

        // Now catPath is 400 by 250
        // Scale it down to 160 by 100
        catPath.Transform(SKMatrix.MakeScale(0.40f, 0.40f));

        // Get the outlines of the contours of the cat path
        SKPath outlinedCatPath = new SKPath();
        catStroke.GetFillPath(catPath, outlinedCatPath);

        // Create a 2D path effect from those outlines
        SKPathEffect fillEffect = SKPathEffect.Create2DPath(
            new SKMatrix { ScaleX = 170, ScaleY = 110,
                           TransX = 75, TransY = 80,
                           Persp2 = 1 },
            outlinedCatPath);

        // Create a 1D path effect from the scallop path
        SKPathEffect strokeEffect =
            SKPathEffect.Create1DPath(scallopPath, 75, 0, SKPath1DPathEffectStyle.Rotate);

        // Set the sum the effects to frame paint
        framePaint.PathEffect = SKPathEffect.CreateSum(fillEffect, strokeEffect);
    }
    ...
}
```

Затем конструктор вызывает `SKPathEffect.Create1DPath` для зубчатого фрейма. Обратите внимание, что ширина пути равна 100 пикселов, а высота — 75 пикселя, так что реплицированный путь перекрывается вокруг рамки. Последняя инструкция конструктора вызывает метод, `SKPathEffect.CreateSum` чтобы объединить два эффекта пути и задать результат для `SKPaint` объекта.

Все эти операции позволяют `PaintSurface` обработчику быть довольно простым. Необходимо только определить прямоугольник и нарисовать его с помощью `framePaint` :

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect rect = new SKRect(50, 50, info.Width - 50, info.Height - 50);
        canvas.ClipRect(rect);
        canvas.DrawRect(rect, framePaint);
    }
}
```

Алгоритмы, в которых используются пути, всегда заставляют весь путь, используемый для обводки или заливки, что может привести к отображению некоторых визуальных элементов за пределами прямоугольника. `ClipRect`Вызов до `DrawRect` вызова функции позволяет значительно очиститель визуальных элементов. (Попробуйте без усечения!)

Обычно используется `SKPathEffect.CreateCompose` для добавления некоторой задержки к другому результату пути. Безусловно, вы можете экспериментировать с вами, но вот немного другой пример:

**Пунктирные** штриховые линии заливка эллипса штриховой линией. Большая часть работы в [`DashedHatchLinesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) классе выполняется прямо в определениях полей. Эти поля определяют эффекты пунктира и штриховки. Они определяются так, как если `static` бы они ссылались в `SKPathEffect.CreateCompose` вызове в `SKPaint` определении:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    static SKPathEffect dashEffect =
        SKPathEffect.CreateDash(new float[] { 30, 30 }, 0);

    static SKPathEffect hatchEffect = SKPathEffect.Create2DLine(20,
        Multiply(SKMatrix.MakeScale(60, 60),
                 SKMatrix.MakeRotationDegrees(45)));

    SKPaint paint = new SKPaint()
    {
        PathEffect = SKPathEffect.CreateCompose(dashEffect, hatchEffect),
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

`PaintSurface`Обработчик должен содержать только стандартные издержки и один вызов `DrawOval` :

```csharp
public class DashedHatchLinesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawOval(info.Width / 2, info.Height / 2,
                        0.45f * info.Width, 0.45f * info.Height,
                        paint);
    }
    ...
}
```

Как уже было обнаружено, линии штриховки точно не ограничиваются внутренней областью, и в этом примере они всегда начинаются слева с целым тире:

[![Тройной снимок экрана со страницей пунктирных линий штриховки](effects-images/dashedhatchlines-small.png)](effects-images/dashedhatchlines-large.png#lightbox)

Теперь, когда вы видите эффекты пути от простых точек и штрихов до странных комбинаций, используйте воображение и посмотрите, что можно создать.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)