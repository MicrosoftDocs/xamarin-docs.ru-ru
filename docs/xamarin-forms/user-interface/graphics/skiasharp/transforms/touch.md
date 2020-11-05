---
title: Манипуляции сенсорного ввода
description: В этой статье объясняется, как использовать преобразования матрицы для реализации сенсорного перетаскивания, сжатия и вращения, а также демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: davidbritch
ms.author: dabritch
ms.date: 09/14/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ee69ca1e95f7dcffa60387579e89c3a2d3e985da
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374294"
---
# <a name="touch-manipulations"></a>Манипуляции сенсорного ввода

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Использование преобразований матрицы для реализации сенсорного перетаскивания, сжатия и вращения_

В средах с несколькими сенсорными устройствами, например на мобильных устройствах, пользователи часто используют свои пальцы для работы с объектами на экране. Стандартные жесты, такие как перетаскивание с двумя пальцами, могут перемещать и масштабировать объекты или даже вращать их. Эти жесты обычно реализуются с помощью матриц преобразования, и в этой статье показано, как это сделать.

![Точечный рисунок, который подлежит преобразованию, масштабированию и повороту](touch-images/touchmanipulationsexample.png)

Все приведенные здесь примеры используют Xamarin.Forms эффект сенсорного отслеживания, представленный в статье [**вызов событий из эффектов**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

## <a name="dragging-and-translation"></a>Перетаскивание и перевод

Одним из наиболее важных приложений преобразований матрицы является Сенсорная обработка. Одно [`SKMatrix`](xref:SkiaSharp.SKMatrix) значение может консолидировать ряд операций касания. 

Для перетаскивания одним пальцем `SKMatrix` значение выполняет перевод. Это показано на странице **перетаскивания растрового изображения** . XAML-файл создает экземпляр `SKCanvasView` в Xamarin.Forms `Grid` . `TouchEffect`К коллекции этого объекта добавлен объект `Effects` `Grid` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.BitmapDraggingPage"
             Title="Bitmap Dragging">
    
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Теоретически, `TouchEffect` объект можно добавить непосредственно в `Effects` коллекцию `SKCanvasView` , но это не работает на всех платформах. Поскольку `SKCanvasView` имеет тот же размер, что и `Grid` в этой конфигурации, присоединение его к элементу `Grid` также работает.

Файл кода программной части загружает ресурс точечного рисунка в своем конструкторе и отображает его в `PaintSurface` обработчике:

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    // Bitmap and matrix for display
    SKBitmap bitmap;
    SKMatrix matrix = SKMatrix.MakeIdentity();
    ···

    public BitmapDraggingPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, new SKPoint());
    }
}
```

Без дальнейшего кода `SKMatrix` значение всегда является матрицей Identify, и это не повлияет на отображение растрового изображения. Цель обработчика, заданная `OnTouchEffectAction` в файле XAML, — изменить значение матрицы, чтобы отразить манипуляции с сенсорными элементами.

`OnTouchEffectAction`Обработчик начинает с преобразования Xamarin.Forms `Point` значения в `SKPoint` значение SkiaSharp. Это простой вопрос масштабирования, основанный на `Width` `Height` свойствах и `SKCanvasView` (которые являются аппаратно-независимыми единицами) и `CanvasSize` свойством, которое находится в пикселях:

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    ···
    // Touch information
    long touchId = -1;
    SKPoint previousPoint;
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point = 
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point))
                {
                    touchId = args.Id;
                    previousPoint = point;
                }
                break;

            case TouchActionType.Moved:
                if (touchId == args.Id)
                {
                    // Adjust the matrix for the new position
                    matrix.TransX += point.X - previousPoint.X;
                    matrix.TransY += point.Y - previousPoint.Y;
                    previousPoint = point;
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = -1;
                break;
        }
    }
    ···
}
```

Когда палец впервые касается экрана, срабатывает событие типа `TouchActionType.Pressed` . Первая задача — определить, касается ли палец растрового изображения. Такая задача часто называется _проверкой попадания_. В этом случае проверку попадания можно выполнить, создав `SKRect` значение, соответствующее точечному рисунку, применив матричное преобразование к нему с помощью `MapRect` , а затем определив, находится ли точка касания внутри преобразованного прямоугольника.

Если это так, то в `touchId` поле задается идентификатор касания, и сохраняется его расположение.

Для `TouchActionType.Moved` события коэффициенты перевода `SKMatrix` значения корректируются на основе текущей должности пальца и новой должности пальца. Эта новая позицией сохраняется в следующий раз, а элемент `SKCanvasView` становится недействительным.

При эксперименте с этой программой Обратите внимание, что можно перетащить только точечный рисунок, когда палец касается области, в которой отображается точечный рисунок. Хотя это ограничение не очень важно для этой программы, оно становится важным при управлении несколькими точечными рисунками.

## <a name="pinching-and-scaling"></a>Сжатие и масштабирование

Что нужно сделать, когда два пальца поменяют точечный рисунок? Если два пальца переходят в параллельный, вам, вероятно, потребуется переместить точечный рисунок вместе с пальцами. Если два пальца выполняют операцию сжатия или растяжения, вы можете поворачивать точечный рисунок (будет обсуждаться в следующем разделе) или масштабировать. При масштабировании точечного рисунка имеет смысл, чтобы два пальца оставались в тех же позициях, что и точечный рисунок, и для масштабирования растрового изображения соответствующим образом.

Одновременная обработка двух пальцев кажется сложной, но помните, что `TouchAction` обработчик получает только сведения об одном палеце за раз. Если к точечному рисунку обрабатываются два пальца, то при каждом событии у одного пальца изменилось расположение, но другое не изменилось. В приведенном ниже коде страницы **масштабирования растрового изображения** палец, который не изменил позицию, называется _основной точкой вращения_ , так как преобразование отсчитывается относительно этой точки.

Одно различие между этой программой и предыдущей программой состоит в том, что необходимо сохранить несколько идентификаторов касаний. Для этой цели используется словарь, где Touch ID — ключ словаря, а значение словаря — это текущая координата пальца:

```csharp
public partial class BitmapScalingPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point) && !touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Add(args.Id, point);
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger scale and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index of non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points involved in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Scaling factors are ratios of those
                        float scaleX = newVector.X / oldVector.X;
                        float scaleY = newVector.Y / oldVector.Y;

                        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
                            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
                        {
                            // If something bad hasn't happened, calculate a scale and translation matrix
                            SKMatrix scaleMatrix = 
                                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);

                            SKMatrix.PostConcat(ref matrix, scaleMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }
    ···
}
```

Обработка `Pressed` действия почти аналогична предыдущей программе за исключением того, что идентификатор и точка касания добавляются в словарь. `Released`Действия и `Cancelled` удаляют запись словаря.

`Moved`Однако обработка действия является более сложной. При наличии только одного пальца обработка очень похожа на предыдущую программу. Для двух и более пальцев программа также должна получить информацию из словаря, включающего палец, который не перемещается. Это достигается путем копирования ключей словаря в массив и последующего сравнения первого ключа с ИДЕНТИФИКАТОРом перемещаемого пальца. Это позволяет программе получить точку вращения, соответствующую палецу, который не перемещается.

Затем программа вычисляет два вектора новой позиции пальца относительно точки вращения, а также старую позицию пальца относительно точки вращения. Соотношение этих векторов — коэффициенты масштабирования. Поскольку деление на ноль является возможным, они должны проверяться на бесконечные значения или значения NaN (не числа). Если все хорошо, преобразование масштабирования объединяется со `SKMatrix` значением, сохраненным в виде поля.

При эксперименте с этой страницей можно заметить, что можно перетащить точечный рисунок одним или двумя пальцами или масштабировать его двумя пальцами. Масштабирование является _анизотропным_ , то есть масштабирование может отличаться в горизонтальных и вертикальных направлениях. Это искажает пропорции, но также позволяет перевернуть точечный рисунок для создания зеркального изображения. Вы также можете обнаружить, что растровое изображение можно сжать до нулевого измерения и исчезнет. В рабочем коде необходимо защититься от этого.

## <a name="two-finger-rotation"></a>Вращение с двумя пальцами

Страница **повернуть растровое изображение** позволяет использовать два пальца для вращения или исотропик масштабирования. Точечный рисунок всегда содержит правильные пропорции. Два пальца для вращения и анизотропного масштабирования не работают очень хорошо, так как перемещение пальцев очень похоже на обе задачи.

Первое большое различие в этой программе — логика проверки попадания. Предыдущие программы использовали метод, `Contains` `SKRect` чтобы определить, находится ли точка касания в преобразованном прямоугольнике, соответствующем точечному рисунку. Но по мере того, как пользователь манипулирует точечным рисунком, точечный рисунок может быть повернут и `SKRect` не может правильно представлять повернутый прямоугольник. Можно заметить, что логика проверки попадания в этом случае должна реализовывать довольно сложную аналитическую геометрию.

Однако доступно сочетание клавиш: определение того, находится ли точка внутри границ преобразованного прямоугольника, аналогична определению того, находится ли обратная преобразованная точка в пределах границ непреобразованного прямоугольника. Это гораздо более простой расчет, и логика может продолжать использовать удобный `Contains` метод:

```csharp
public partial class BitmapRotationPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!touchDictionary.ContainsKey(args.Id))
                {
                    // Invert the matrix
                    if (matrix.TryInvert(out SKMatrix inverseMatrix))
                    {
                        // Transform the point using the inverted matrix
                        SKPoint transformedPoint = inverseMatrix.MapPoint(point);

                        // Check if it's in the untransformed bitmap rectangle
                        SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);

                        if (rect.Contains(transformedPoint))
                        {
                            touchDictionary.Add(args.Id, point);
                        }
                    }
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger rotate, scale, and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Find angles from pivot point to touch points
                        float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                        float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                        // Calculate rotation matrix
                        float angle = newAngle - oldAngle;
                        SKMatrix touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                        // Effectively rotate the old vector
                        float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                        oldVector.X = magnitudeRatio * newVector.X;
                        oldVector.Y = magnitudeRatio * newVector.Y;

                        // Isotropic scaling!
                        float scale = Magnitude(newVector) / Magnitude(oldVector);

                        if (!float.IsNaN(scale) && !float.IsInfinity(scale))
                        {
                            SKMatrix.PostConcat(ref touchMatrix,
                                SKMatrix.MakeScale(scale, scale, pivotPoint.X, pivotPoint.Y));

                            SKMatrix.PostConcat(ref matrix, touchMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }

    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
    ···
}
```

Логика `Moved` события начинается, как и предыдущая программа. Два вектора с именами `oldVector` и `newVector` рассчитываются на основе предыдущей и текущей точки перемещения пальца и точки вращения для неперемещенного пальца. Однако углы этих векторов определяются, а различие — угол поворота.

Также может быть задействовано масштабирование, поэтому старый вектор поворачивается на основе угла поворота. Относительная величина двух векторов теперь является коэффициентом масштабирования. Обратите внимание, что одно и то же `scale` значение используется для горизонтального и вертикального масштабирования, чтобы масштабирование исотропик. `matrix`Поле регулируется как матрица вращения, так и матрица шкалы.

Если приложению требуется реализовать сенсорную обработку для одного точечного рисунка (или другого объекта), можно адаптировать код из этих трех примеров для собственного приложения. Но если необходимо реализовать сенсорную обработку для нескольких точечных рисунков, вам, вероятно, потребуется инкапсулировать эти операции касания в других классах.

## <a name="encapsulating-the-touch-operations"></a>Инкапсуляция операций касания

На странице **сенсорная манипуляция** демонстрируется сенсорное управление одним растровым изображением, но с использованием нескольких других файлов, которые инкапсулируют большую часть логики, показанной выше. Первый из этих файлов — это [`TouchManipulationMode`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) перечисление, которое указывает различные типы манипуляций касания, реализованные кодом, который вы будете видеть:

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly` — Это перетаскивание одним пальцем, реализованное с помощью перевода. Все последующие параметры также включают панорамирование, но включают два пальца: `IsotropicScale` — это операция сжатия, которая приводит к равномерному масштабированию объектов в горизонтальном и вертикальном направлениях. `AnisotropicScale` разрешает неодинаковое масштабирование.

`ScaleRotate`Параметр предназначен для масштабирования и вращения с двумя пальцами. Масштабирование — исотропик. Как упоминалось ранее, реализация двустороннего вращения с помощью анизотропного масштабирования является проблематичной, так как движения пальца по сути одинаковы.

`ScaleDualRotate`Параметр добавляет поворот одним пальцем. Когда один палец перетаскивает объект, перетаскиваемый объект сначала поворачивается вокруг его центра, чтобы центр объекта настроился с помощью вектора перетаскивания.

Файл [**таучманипулатионпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) включает объект `Picker` с элементами `TouchManipulationMode` перечисления:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos.Transforms"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:TouchManipulationMode}">
                    <x:Static Member="local:TouchManipulationMode.None" />
                    <x:Static Member="local:TouchManipulationMode.PanOnly" />
                    <x:Static Member="local:TouchManipulationMode.IsotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.AnisotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.ScaleRotate" />
                    <x:Static Member="local:TouchManipulationMode.ScaleDualRotate" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>
        
        <Grid BackgroundColor="White"
              Grid.Row="1">
            
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

В нижней части находится объект `SKCanvasView` и, `TouchEffect` присоединенный к одной ячейке `Grid` , в которой она заключена.

Файл кода программной части [**TouchManipulationPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) имеет `bitmap` поле, но не имеет тип `SKBitmap` . Тип `TouchManipulationBitmap` (класс, который вы увидите вскоре):

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            SKBitmap bitmap = SKBitmap.Decode(stream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Конструктор создает экземпляр `TouchManipulationBitmap` объекта, передавая ему конструктор, `SKBitmap` полученный из внедренного ресурса. Конструктор завершается путем установки `Mode` свойства `TouchManager` `TouchManipulationBitmap` объекта свойства в член `TouchManipulationMode` перечисления.

`SelectedIndexChanged`Обработчик для `Picker` также задает это `Mode` свойство:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            bitmap.TouchManager.Mode = (TouchManipulationMode)picker.SelectedItem;
        }
    }
    ...
}
```

`TouchAction`Обработчик `TouchEffect` экземпляра, созданного в файле XAML, вызывает два метода `TouchManipulationBitmap` с именами `HitTest` и `ProcessTouchEvent` :

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Если `HitTest` метод возвращает `true` &mdash; значение, означающее, что палец затронул экран в области, занятой точечным рисунком, то в &mdash; коллекцию добавляется сенсорный идентификатор `TouchIds` . Этот идентификатор представляет последовательность событий касания для этого пальца, пока палец не отрывается от экрана. Если точечное изображение затрагивает несколько пальцев, то `touchIds` коллекция содержит сенсорный идентификатор для каждого пальца.

`TouchAction`Обработчик также вызывает `ProcessTouchEvent` класс в `TouchManipulationBitmap` . Здесь происходит некоторое (но не все) фактической обработки касаний.

[`TouchManipulationBitmap`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs)Класс является классом-оболочкой для `SKBitmap` , который содержит код для отображения битовых и триадных событий. Он работает в сочетании с более обобщенным кодом в `TouchManipulationManager` классе (который вы увидите чуть ниже).

`TouchManipulationBitmap`Конструктор сохраняет `SKBitmap` и создает экземпляры двух свойств, `TouchManager` свойство типа `TouchManipulationManager` и `Matrix` свойство типа `SKMatrix` :

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

Это `Matrix` свойство является накопленным преобразованием, полученным в результате всех действий сенсорного ввода. Как вы увидите, каждое событие касания разрешается в матрицу, которая затем объединяется со `SKMatrix` значением, хранящимся в `Matrix` свойстве.

`TouchManipulationBitmap`Объект рисует себя в своем `Paint` методе. Аргумент является `SKCanvas` объектом. `SKCanvas`Возможно, к нему уже применено преобразование, поэтому `Paint` Метод сцепляет `Matrix` свойство, связанное с растровым изображением, с существующим преобразованием и восстанавливает холст после его завершения:

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

`HitTest`Метод возвращает значение, `true` Если пользователь касается экрана в точке в границах точечного рисунка. В этом случае используется логика, показанная выше на странице **вращения растрового изображения** :

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

Второй открытый метод в `TouchManipulationBitmap` имеет значение `ProcessTouchEvent` . При вызове этого метода он уже установил, что событие касания относится к определенному точечному рисунку. Метод поддерживает словарь [`TouchManipulationInfo`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) объектов, который является просто предыдущей точкой и новой точкой каждого пальца:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Вот словарь и `ProcessTouchEvent` сам метод:

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

В `Moved` событиях и `Released` метод вызывает `Manipulate` . В данный момент элемент `touchDictionary` содержит один или несколько `TouchManipulationInfo` объектов. Если `touchDictionary` содержит один элемент, скорее всего, `PreviousPoint` `NewPoint` значения и не равны и представляют движение пальца. Если точечное изображение касается нескольких пальцев, словарь содержит более одного элемента, но только один из этих элементов имеет разные `PreviousPoint` `NewPoint` значения и. Все остальные имеют одинаковые `PreviousPoint` значения и `NewPoint` .

Это важно. `Manipulate` метод может предположить, что он обрабатывает перемещение только одного пальца. Во время этого вызова ни один из других пальцев не перемещается, и если они действительно перемещаются (как и скорее всего), эти перемещения будут обрабатываться в будущих вызовах `Manipulate` .

`Manipulate`Метод сначала копирует словарь в массив для удобства. Он игнорирует все, кроме первых двух записей. Если вы пытаетесь управлять точечным рисунком более чем двумя пальцами, остальные игнорируются. `Manipulate` является последним членом `TouchManipulationBitmap` :

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

Если один палец выполняет манипуляцию с точечным рисунком, `Manipulate` вызывает `OneFingerManipulate` метод `TouchManipulationManager` объекта. Для двух пальцев он вызывает `TwoFingerManipulate` . Аргументы для этих методов одинаковы: `prevPoint` `newPoint` аргументы и представляют перемещающий палец. Но `pivotPoint` аргумент отличается для двух вызовов:

Для манипуляции одним пальцем `pivotPoint` является центр точечного рисунка. Это позволяет выполнить поворот одним пальцем. Для манипуляций с двумя пальцами событие указывает на перемещение только одного пальца, чтобы он `pivotPoint` не перемещался.

В обоих случаях `TouchManipulationManager` возвращает `SKMatrix` значение, которое метод объединяет с текущим `Matrix` свойством, `TouchManipulationPage` используемым для отрисовки точечного рисунка.

`TouchManipulationManager` является обобщенным и не использует другие файлы, кроме `TouchManipulationMode` . Вы можете использовать этот класс без изменений в собственных приложениях. Он определяет единственное свойство типа `TouchManipulationMode`.

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```

Тем не менее, вы, вероятно, захотите избежать этого `AnisotropicScale` . С помощью этого параметра можно легко управлять точечным рисунком, чтобы один из коэффициентов масштабирования стал нулевым. Это делает растровое изображение недоступным и не возвращается. Если вам действительно требуется анизотропное масштабирование, необходимо улучшить логику, чтобы избежать нежелательных результатов.

`TouchManipulationManager` использует векторы, но, поскольку `SKVector` Структура в SkiaSharp отсутствует, `SKPoint` вместо нее используется. `SKPoint` поддерживает оператор вычитания, и результат может рассматриваться как вектор. Единственное, что нужно добавить логику для вектора, — это `Magnitude` Вычисление:

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

Если выбрано вращение, то сначала обработайте поворот с помощью методов обработки одним пальцем и двумя пальцами. При обнаружении какого-либо вращения компонент вращения фактически удаляется. То, что остается, интерпретируется как панорамирование и масштабирование.

Ниже приведен `OneFingerManipulate` метод. Если поворот одного пальца не включен, то логика проста. &mdash; она просто использует предыдущую точку и новую точку для создания вектора с именем `delta` , который точно соответствует преобразованию. При включенном повороте одним пальцем метод использует углы из точки вращения (центра точечного рисунка) к предыдущей точке и новую точку для создания матрицы вращения:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

В `TwoFingerManipulate` методе точка вращения является положением пальца, который не перемещается в это особое событие касания. Поворот очень похож на поворот одного пальца, а затем `oldVector` для вращения корректируется вектор с именем (на основе предыдущей точки). Оставшееся перемещение интерпретируется как масштабирование:

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

Вы заметите, что в этом методе нет явного перевода. Однако оба `MakeRotation` `MakeScale` метода и основаны на точке вращения и включают неявное преобразование. Если вы используете два пальца на точечном рисунке и перетаскивайте их в одном направлении, `TouchManipulation` получит ряд сенсорных событий, изменяющихся между двумя пальцами. По мере того, как каждый палец перемещается относительно другого, масштабирования или вращения, но он инвертирует движением другого пальца, а результат — перевод.

Единственная оставшаяся часть страницы **сенсорного управления** — это `PaintSurface` обработчик в `TouchManipulationPage` файле кода программной части. Это вызывает `Paint` метод объекта `TouchManipulationBitmap` , который применяет матрицу, представляющую накопленное действие касания:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

`PaintSurface`Обработчик завершается отображением объекта, `MatrixDisplay` отображающего накопленную сенсорную матрицу:

[![Тройной снимок экрана страницы "Управление касанием"](touch-images/touchmanipulation-small.png)](touch-images/touchmanipulation-large.png#lightbox "Тройной снимок экрана страницы "Управление касанием"")

## <a name="manipulating-multiple-bitmaps"></a>Управление несколькими точечными рисунками

Одним из преимуществ изоляции кода сенсорной обработки в классах, таких как `TouchManipulationBitmap` и, `TouchManipulationManager` является возможность повторного использования этих классов в программе, позволяющей пользователю манипулировать несколькими точечными рисунками.

На странице точечное **представление точечных рисунков** показано, как это делается. Вместо определения поля типа `TouchManipulationBitmap` [`BitmapScatterPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) класс определяет `List` объекты Bitmap:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                {
                    SKBitmap bitmap = SKBitmap.Decode(stream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

Конструктор загружает все точечные рисунки, доступные в виде внедренных ресурсов, и добавляет их в `bitmapCollection` . Обратите внимание, что `Matrix` свойство инициализируется для каждого `TouchManipulationBitmap` объекта, поэтому верхние левые углы каждого растрового изображения смещаются на 100 пикселей.

`BitmapScatterView`Страница также должна поддерживать события касания для нескольких точечных рисунков. Вместо того чтобы определять `List` идентификаторы сенсоров для объектов, работающих в данный момент `TouchManipulationBitmap` , программе требуется словарь:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Обратите внимание на то, как `Pressed` логика проходит через `bitmapCollection` обратный. Точечные рисунки часто перекрываются друг с другом. Растровые изображения, представленные далее в коллекции, визуально находятся поверх точечных рисунков ранее в коллекции. Если на экране есть несколько точечных рисунков, которые нажимают на экран, самая верхняя из них должна совпадать с изображением пальца.

Также обратите внимание, что `Pressed` логика перемещает этот точечный рисунок в конец коллекции, так что он визуально перемещается вверх по стопке других точечных рисунков.

В `Moved` событиях и `Released` `TouchAction` обработчик вызывает `ProcessingTouchEvent` метод в точно так же, `TouchManipulationBitmap` как и в предыдущей программе.

Наконец, `PaintSurface` обработчик вызывает `Paint` метод для каждого `TouchManipulationBitmap` объекта:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

Код выполняет цикл по коллекции и отображает кучу точечных рисунков от начала коллекции до конца:

[![Тройной снимок экрана со страницей точечного представления точечного рисунка](touch-images/bitmapscatterview-small.png)](touch-images/bitmapscatterview-large.png#lightbox "Тройной снимок экрана со страницей точечного представления точечного рисунка")

## <a name="single-finger-scaling"></a>Масштабирование Single-Finger

Операция масштабирования обычно требует жеста сжатия, используя два пальца. Однако можно реализовать масштабирование одним пальцем, перемещая углы точечного рисунка.

Это показано на странице **масштабирования с одним пальцем** . Поскольку в этом примере используется более разный тип масштабирования, чем реализовано в `TouchManipulationManager` классе, он не использует этот класс или `TouchManipulationBitmap` класс. Вместо этого вся логика сенсорного ввода находится в файле кода программной части. Это довольно простая логика, чем обычно, так как она отслеживает только один палец за раз и просто игнорирует все дополнительные пальцы, которые могут касаться экрана.

На странице [**синглефинжеркорнерскале. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) создается экземпляр `SKCanvasView` класса и `TouchEffect` объект для отслеживания событий сенсорного ввода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.SingleFingerCornerScalePage"
             Title="Single Finger Corner Scale">

    <Grid BackgroundColor="White"
          Grid.Row="1">

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction"   />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Файл [**SingleFingerCornerScalePage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) загружает ресурс точечного рисунка из каталога **носителя** и отображает его с помощью `SKMatrix` объекта, определенного как поле:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();
    ···

    public SingleFingerCornerScalePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.SetMatrix(currentMatrix);
        canvas.DrawBitmap(bitmap, 0, 0);
    }
    ···
}
```

Этот `SKMatrix` объект изменяется сенсорной логикой, показанной ниже.

Оставшаяся часть файла кода программной части — это `TouchEffect` обработчик событий. Он начинает с преобразования текущего положения пальца в `SKPoint` значение. Для `Pressed` типа действия обработчик проверяет, что никакой другой палец не касается экрана, и что палец находится внутри границ точечного рисунка.

Важной частью кода является `if` оператор, включающий два вызова `Math.Pow` метода. Эта математическая проверка, если положение пальца находится за пределами эллипса, который заполняет точечный рисунок. Если да, то это операция масштабирования. Палец находится ближе к одному из углов точечного рисунка, и определяется точка вращения, которая является противоположным углом. Если палец находится внутри этого эллипса, это обычная операция панорамирования:

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();

    // Information for translating and scaling
    long? touchId = null;
    SKPoint pressedLocation;
    SKMatrix pressedMatrix;

    // Information for scaling
    bool isScaling;
    SKPoint pivotPoint;
    ···

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Track only one finger
                if (touchId.HasValue)
                    return;

                // Check if the finger is within the boundaries of the bitmap
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = currentMatrix.MapRect(rect);
                if (!rect.Contains(point))
                    return;

                // First assume there will be no scaling
                isScaling = false;

                // If touch is outside interior ellipse, make this a scaling operation
                if (Math.Pow((point.X - rect.MidX) / (rect.Width / 2), 2) +
                    Math.Pow((point.Y - rect.MidY) / (rect.Height / 2), 2) > 1)
                {
                    isScaling = true;
                    float xPivot = point.X < rect.MidX ? rect.Right : rect.Left;
                    float yPivot = point.Y < rect.MidY ? rect.Bottom : rect.Top;
                    pivotPoint = new SKPoint(xPivot, yPivot);
                }

                // Common for either pan or scale
                touchId = args.Id;
                pressedLocation = point;
                pressedMatrix = currentMatrix;
                break;

            case TouchActionType.Moved:
                if (!touchId.HasValue || args.Id != touchId.Value)
                    return;

                SKMatrix matrix = SKMatrix.MakeIdentity();

                // Translating
                if (!isScaling)
                {
                    SKPoint delta = point - pressedLocation;
                    matrix = SKMatrix.MakeTranslation(delta.X, delta.Y);
                }
                // Scaling
                else
                {
                    float scaleX = (point.X - pivotPoint.X) / (pressedLocation.X - pivotPoint.X);
                    float scaleY = (point.Y - pivotPoint.Y) / (pressedLocation.Y - pivotPoint.Y);
                    matrix = SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);
                }

                // Concatenate the matrices
                SKMatrix.PreConcat(ref matrix, pressedMatrix);
                currentMatrix = matrix;
                canvasView.InvalidateSurface();
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = null;
                break;
        }
    }
}
```

`Moved`Тип действия вычисляет матрицу, соответствующую сенсорному действию со времени, когда палец нажал на экран до этого времени. Она объединяет эту матрицу с матрицей, которая действует во время первой нажатия на точечный рисунок. Операция масштабирования всегда выполняется относительно угла, противоположного тому, на который затронул палец.

Для небольших или облонг точечных рисунков внутренний эллипс может занимать большую часть точечного рисунка и оставлять небольшие области на углах, чтобы масштабировать точечный рисунок. Вы можете предпочесть несколько других подходов. в этом случае можно заменить весь `if` блок, которому присваивается `isScaling` `true` следующий код:

```csharp
float halfHeight = rect.Height / 2;
float halfWidth = rect.Width / 2;

// Top half of bitmap
if (point.Y < rect.MidY)
{
    float yRelative = (point.Y - rect.Top) / halfHeight;

    // Upper-left corner
    if (point.X < rect.MidX - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Bottom);
    }
    // Upper-right corner
    else if (point.X > rect.MidX + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Bottom);
    }
}
// Bottom half of bitmap
else
{
    float yRelative = (point.Y - rect.MidY) / halfHeight;

    // Lower-left corner
    if (point.X < rect.Left + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Top);
    }
    // Lower-right corner
    else if (point.X > rect.Right - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Top);
    }
}
```

Этот код эффективно разделяет область точечного рисунка на внутреннюю фигуру ромба и четыре треугольники на углах. Это позволяет гораздо более крупные области на углах для захвата и масштабирования точечного рисунка.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Вызов событий из эффекта](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)