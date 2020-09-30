---
title: Неаффинные преобразования
description: В этой статье объясняется, как создать эффекты перспективы и конуса со третьим столбцом матрицы преобразования и как это демонстрируется с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6de5e21c509203c5402ed8c7e75908b54808d140
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556897"
---
# <a name="non-affine-transforms"></a>Неаффинные преобразования

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Создание эффектов перспективы и конуса с помощью третьего столбца матрицы преобразования_

Преобразование, масштабирование, вращение и асимметрия классифицируются как *аффинное* преобразования. Аффинное преобразование сохраняет параллельные линии. Если две строки являются параллельными до преобразования, они остаются параллельно после преобразования. Прямоугольники всегда преобразуются в параллелограммs.

Однако SkiaSharp также поддерживает неаффинных преобразований, которые имеют возможность преобразования прямоугольника в любой выпуклой грани четырехсторонней:

![Растровое изображение, преобразованное в выпуклой грани четырехсторонней](non-affine-images/nonaffinetransformexample.png)

Выпуклой грани четырехсторонней — это 4-сторонняя фигура, в которой внутренние углы всегда менее 180 градусов и сторон, которые не пересекаются друг с другом.

Неаффинное преобразование приводит к тому, что если третья строка матрицы преобразований имеет значения, отличные от 0, 0 и 1. Полное `SKMatrix` умножение:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Результирующие формулы преобразования:

x "= ScaleX · x + Скевкс · y + Транскс

y "= отклонение · x + Scale · y + TX

z ' = Persp0 · x + Persp1 · y + Persp2

Фундаментальным правилом использования матрицы размером 3 на 3 для двумерных преобразований является то, что все остается на плоскости, где Z равно 1. Если `Persp0` и `Persp1` равны 0 и равны `Persp2` 1, преобразование переместило координаты Z с этой плоскости.

Чтобы восстановить это преобразование в двухмерный, необходимо переместить координаты обратно на эту плоскость. Требуется еще один шаг. Значения x ", y" и z "должны быть разделены на z":

x "= x"/z "

y "= y '/z '

z "= z '/z ' = 1

Они называются *однородными координатами* , и они были разработаны с помощью МасематиЦиан августа Фердинанд мöбиус, намного лучше известного для его топологическом Оддити, мöбиус ленты.

Если z имеет значение 0, деление приводит к бесконечным координатам. На самом деле, одна из Мöбиус'с причин для разработки однородных координат была возможность представлять бесконечные значения с конечными числами.

Однако при отображении графики необходимо избегать отрисовки с помощью координат, преобразуемых в бесконечные значения. Эти координаты не будут подготавливаться к просмотру. Все, что находится в окружающих координатах, будет очень большим и, вероятно, не было визуально согласовано.

В этом уравнении не нужно, чтобы значение z было равно нулю:

z ' = Persp0 · x + Persp1 · y + Persp2

Следовательно, эти значения имеют некоторые практические ограничения. 

`Persp2`Ячейка может быть либо нулем, либо не нулем. Если значение `Persp2` равно нулю, то z "имеет нулевое значение для точки (0, 0), и это, как правило, нежелательно, поскольку эта точка очень распространена в двухмерной графике. Если значение `Persp2` не равно нулю, то не происходит потери общего вида, если `Persp2` фиксировано значение 1. Например, если определить, что `Persp2` должно равняться 5, то можно просто разделить все ячейки в матрице на 5, то есть `Persp2` равное 1, и результат будет одинаковым.

По этим причинам `Persp2` часто фиксируется 1, то есть то же значение, что и в матрице идентификаторов.

Обычно `Persp0` и `Persp1` являются небольшими числами. Например, предположим, что начинается с матрицы идентификаторов, но имеет значение `Persp0` 0,01:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Формулы преобразования:

x "= x/(0,01 · x + 1)

y "= y/(0,01 · x + 1)

Теперь используйте это преобразование для отрисовки прямоугольника размером 100 пикселей, расположенного в источнике. Вот как преобразуются четыре угловые углы:

(0, 0) > (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

Если x равно 100, то знаменатель z равен 2, поэтому координаты x и y фактически являются незначительной половиной. Правая часть поля будет короче левой части:

![Поле, подлежащим преобразованию, которое не является аффинное](non-affine-images/nonaffinetransform.png)

`Persp`Часть этих имен ячеек относится к "перспективе", так как ракурс предполагает, что поле теперь находится справа от средства просмотра.

Страница **Перспектива теста** позволяет поэкспериментировать со значениями `Persp0` и, `Pers1` чтобы приступить к работе. Разумные значения этих ячеек матрицы настолько малы, что `Slider` в универсальная платформа Windows не может правильно их обработать. Чтобы удовлетворить проблему UWP, два `Slider` элемента в [**тестперспективе. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) должны быть инициализированы в диапазоне от – 1 до 1:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.TestPerspectivePage"
             Title="Test Perpsective">
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
                    <Setter Property="Minimum" Value="-1" />
                    <Setter Property="Maximum" Value="1" />
                    <Setter Property="Margin" Value="20, 0" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="persp0Slider"
                Grid.Row="0"
                ValueChanged="OnPersp0SliderValueChanged" />

        <Label x:Name="persp0Label"
               Text="Persp0 = 0.0000"
               Grid.Row="1" />

        <Slider x:Name="persp1Slider"
                Grid.Row="2"
                ValueChanged="OnPersp1SliderValueChanged" />

        <Label x:Name="persp1Label"
               Text="Persp1 = 0.0000"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Обработчики событий для ползунков в [`TestPerspectivePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) файле кода программной части делят значения на 100, чтобы они были в диапазоне от – 0,01 до 0,01. Кроме того, конструктор загружает растровое изображение:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    SKBitmap bitmap;

    public TestPerspectivePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnPersp0SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp0Label.Text = String.Format("Persp0 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }

    void OnPersp1SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp1Label.Text = String.Format("Persp1 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }
    ...
}
```

`PaintSurface`Обработчик вычисляет `SKMatrix` значение с именем `perspectiveMatrix` на основе значений этих двух ползунков, деленных на 100. Он сочетается с двумя преобразованиями преобразования, которые помещают центр этого преобразования в центре точечного рисунка:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Calculate perspective matrix
        SKMatrix perspectiveMatrix = SKMatrix.MakeIdentity();
        perspectiveMatrix.Persp0 = (float)persp0Slider.Value / 100;
        perspectiveMatrix.Persp1 = (float)persp1Slider.Value / 100;

        // Center of screen
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);
        SKMatrix.PostConcat(ref matrix, perspectiveMatrix);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(xCenter, yCenter));

        // Coordinates to center bitmap on canvas
        float x = xCenter - bitmap.Width / 2;
        float y = yCenter - bitmap.Height / 2;

        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

Ниже приведены некоторые примеры изображений.

[![Тройной снимок экрана страницы "Перспектива тестирования"](non-affine-images/testperspective-small.png)](non-affine-images/testperspective-large.png#lightbox "Тройной снимок экрана страницы "Перспектива тестирования"")

При эксперименте с ползунками вы обнаружите, что значения, превышающие 0,0066 или ниже – 0,0066, приводят к тому, что изображение внезапно становится фрактуред и несогласованным. Преобразуемый точечный рисунок имеет размер 300 пикселей в квадрате. Он преобразуется относительно его центра, поэтому координаты диапазона битовой карты от – 150 до 150. Помните, что значение z:

z ' = Persp0 · x + Persp1 · y + 1

Если `Persp0` или `Persp1` больше 0,0066 или ниже – 0,0066, то всегда имеется определенная координата точечного рисунка, которая приводит к нулевому значению z. Это приводит к тому, что деление на ноль будет незапутанным. При использовании неаффинных преобразований необходимо избегать отрисовки любых элементов с координатами, вызывающими деление на ноль.

Как правило, установка `Persp0` и изоляция не выполняется `Persp1` . Также часто требуется задать другие ячейки в матрице для получения определенных типов преобразований, не являющихся аффинных.

Одно такое преобразование, не являющееся аффинное, является *преобразованием конуса*. Этот тип неаффинных преобразований содержит общие размеры прямоугольника, но с конусом на одну сторону:

![Поле, подлежащим преобразованию конуса](non-affine-images/tapertransform.png)

[`TaperTransform`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs)Класс выполняет обобщенное вычисление неаффинное преобразования на основе этих параметров:

- прямоугольный размер преобразуемого изображения,
- Перечисление, указывающее сторону прямоугольника, который находится в конусах,
- другое перечисление, указывающее, как это конусное, и
- степень конуса.

Вот этот код:

```csharp
enum TaperSide { Left, Top, Right, Bottom }

enum TaperCorner { LeftOrTop, RightOrBottom, Both }

static class TaperTransform
{
    public static SKMatrix Make(SKSize size, TaperSide taperSide, TaperCorner taperCorner, float taperFraction)
    {
        SKMatrix matrix = SKMatrix.MakeIdentity();

        switch (taperSide)
        {
            case TaperSide.Left:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp0 = (taperFraction - 1) / size.Width;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Top:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp1 = (taperFraction - 1) / size.Height;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Right:
                matrix.ScaleX = 1 / taperFraction;
                matrix.Persp0 = (1 - taperFraction) / (size.Width * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        break;
                }
                break;

            case TaperSide.Bottom:
                matrix.ScaleY = 1 / taperFraction;
                matrix.Persp1 = (1 - taperFraction) / (size.Height * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        break;
                }
                break;
        }
        return matrix;
    }
}
```

Этот класс используется на странице **Преобразование конусов** . XAML-файл создает два `Picker` элемента для выбора значений перечисления и `Slider` для выбора доли конуса. [`PaintSurface`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55)Обработчик сочетает преобразование «вращение» с двумя преобразованиями преобразования, чтобы сделать преобразование относительно левого верхнего угла точечного рисунка:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedItem;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedItem;
    float taperFraction = (float)taperFractionSlider.Value;

    SKMatrix taperMatrix =
        TaperTransform.Make(new SKSize(bitmap.Width, bitmap.Height),
                            taperSide, taperCorner, taperFraction);

    // Display the matrix in the lower-right corner
    SKSize matrixSize = matrixDisplay.Measure(taperMatrix);

    matrixDisplay.Paint(canvas, taperMatrix,
        new SKPoint(info.Width - matrixSize.Width,
                    info.Height - matrixSize.Height));

    // Center bitmap on canvas
    float x = (info.Width - bitmap.Width) / 2;
    float y = (info.Height - bitmap.Height) / 2;

    SKMatrix matrix = SKMatrix.MakeTranslation(-x, -y);
    SKMatrix.PostConcat(ref matrix, taperMatrix);
    SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(x, y));

    canvas.SetMatrix(matrix);
    canvas.DrawBitmap(bitmap, x, y);
}
```

Ниже приводится несколько примеров.

[![Тройной снимок экрана страницы "преобразование конусов"](non-affine-images/tapertransform-small.png)](non-affine-images/tapertransform-large.png#lightbox "Тройной снимок экрана страницы "преобразование конусов"")

Другим типом обобщенных неаффинных преобразований является трехмерный поворот, который демонстрируется в следующей статье: [**трехмерные повороты**](3d-rotation.md).

Преобразование, которое не является аффинное, может преобразовывать прямоугольник в любой выпуклой грани четырехсторонней. Это продемонстрировано на странице « **Показывать Неаффинное матрицу** ». Она очень похожа на страницу **Показывать аффинное матрицу** из статьи [**преобразования матрицы**](matrix.md) за исключением того, что у нее есть четвертый `TouchPoint` объект для обработки четвертого угла точечного рисунка:

[![Тройной снимок экрана страницы "Отображение неаффинного матрицы"](non-affine-images/shownonaffinematrix-small.png)](non-affine-images/shownonaffinematrix-large.png#lightbox "Тройной снимок экрана страницы "Отображение неаффинного матрицы"")

Если вы не попытаетесь сделать внутреннюю часть одного из углов точечного рисунка более 180 градусов или сделать две стороны пересекать друг друга, программа успешно вычисляет преобразование с помощью этого метода из [`ShowNonAffineMatrixPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) класса:

```csharp
static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL, SKPoint ptLR)
{
    // Scale transform
    SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

    // Affine transform
    SKMatrix A = new SKMatrix
    {
        ScaleX = ptUR.X - ptUL.X,
        SkewY = ptUR.Y - ptUL.Y,
        SkewX = ptLL.X - ptUL.X,
        ScaleY = ptLL.Y - ptUL.Y,
        TransX = ptUL.X,
        TransY = ptUL.Y,
        Persp2 = 1
    };

    // Non-Affine transform
    SKMatrix inverseA;
    A.TryInvert(out inverseA);
    SKPoint abPoint = inverseA.MapPoint(ptLR);
    float a = abPoint.X;
    float b = abPoint.Y;

    float scaleX = a / (a + b - 1);
    float scaleY = b / (a + b - 1);

    SKMatrix N = new SKMatrix
    {
        ScaleX = scaleX,
        ScaleY = scaleY,
        Persp0 = scaleX - 1,
        Persp1 = scaleY - 1,
        Persp2 = 1
    };

    // Multiply S * N * A
    SKMatrix result = SKMatrix.MakeIdentity();
    SKMatrix.PostConcat(ref result, S);
    SKMatrix.PostConcat(ref result, N);
    SKMatrix.PostConcat(ref result, A);

    return result;
}
```

Для простоты вычислений этот метод получает общее преобразование в виде произведения трех отдельных преобразований, которые в своюмся порядке имеют стрелки, показывающие, как эти преобразования изменяют четыре угла точечного рисунка:

(0, 0) > (0, 0) > (0,0, 0) → (x0, y0) (верхний левый)

(0, H) > (0, 1) > (0, 1) → (x1, y1) (внизу слева)

(W, 0) > (1, 0) → (1, 0) → (x2, Y2) (справа вверху)

(W, H) → (1, 1) > (a, b) → (X3, Y3) (в нижнем правом углу)

Последние координаты справа — это четыре точки, связанные с четырьмя сенсорными точками. Это конечные координаты углов точечного рисунка.

W и H представляют ширину и высоту точечного рисунка. Первое преобразование `S` просто масштабирует точечный рисунок до 1 пиксельного квадрата. Второе преобразование — это преобразование, не являющееся аффинное `N` , а третье — аффинное преобразование `A` . Такое аффинное преобразование основано на трех точках, поэтому оно аналогично предыдущему аффинное [`ComputeMatrix`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) методу и не затрагивает четвертую строку с точкой (a, b).

`a`Значения и `b` вычисляются, чтобы третье преобразование было аффинное. Код получает инверсию аффинных преобразований, а затем использует его для отображения правого нижнего угла. Это точка (a, b).

Другим применением неаффинных преобразований является имитация трехмерной графики. В следующей статье показаны [**трехмерные повороты**](3d-rotation.md) , посвященные повороту двухмерного изображения в трехмерном пространстве.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)