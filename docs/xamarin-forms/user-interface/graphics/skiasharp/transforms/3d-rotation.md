---
Title: "трехмерные вращения в SkiaSharp" Описание: "в этой статье объясняется, как использовать неаффинное преобразования для вращения двумерных объектов в трехмерном пространстве и демонстрируется в примере кода".
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9 Автор: давидбритч MS. author: дабритч MS. Дата: 04/14/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="3d-rotations-in-skiasharp"></a>Трехмерные повороты в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Используйте неаффинного преобразования для вращения двумерных объектов в трехмерном пространстве._

Одним из распространенных приложений неаффинных преобразований является имитация вращения 2D-объекта в трехмерном пространстве:

![](3d-rotation-images/3drotationsexample.png "A text string rotated in 3D space")

Это задание включает работу с трехмерными поворотами, а затем создает неаффинное `SKMatrix` Преобразование, которое выполняет эти трехмерные повороты.

Сложно разработать это преобразование, которое `SKMatrix` работает исключительно в двух измерениях. Задание становится намного проще, когда матрица с 3 по 3 является производной от 4 на 4 матрицы, используемой в трехмерной графике. SkiaSharp включает [`SKMatrix44`](xref:SkiaSharp.SKMatrix44) класс для этой цели, но некоторый фон трехмерной графики необходим для понимания трехмерных поворотов и матрицы преобразований 4 на 4.

Трехмерная система координат добавляет третью ось с названием Z. концептуально ось Z находится на экране справа. Точки координат в трехмерном пространстве обозначаются тремя числами: (x, y, z). В трехмерной системе координат, используемой в этой статье, увеличение значений X — вправо и увеличение значений Y вниз, так же как и в двух измерениях. Увеличение положительных значений Z выходит за пределы экрана. Источник — это верхний левый угол, как в 2D-графике. Вы можете представить экран в виде плоскости XY с осью Z в правой части до этой плоскости.

Это называется левой системой координат. Если вы назначите указательным пальцем для левой руки в направлении положительных координат X (справа), и средний палец в направлении увеличения координат Y (вниз), то точки Thumb в направлении увеличения координат по оси Z — расширяются с экрана.

В трехмерной графике преобразования основаны на матрице размером 4 на 4. Ниже приведена Матрица идентификации 4 по 4:

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

При работе с матрицей размером 4 на 4 удобно указывать ячейки с номерами строк и столбцов:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Однако `Matrix44` класс SkiaSharp немного отличается. Единственный способ задать или получить отдельные значения ячеек в заключается в `SKMatrix44` использовании [`Item`](xref:SkiaSharp.SKMatrix44.Item(System.Int32,System.Int32)) индексатора. Индексы строк и столбцов отсчитываются от нуля, а не от единицы, а строки и столбцы меняются местами. Доступ к ячейке, M14 на приведенной выше схеме, осуществляется с помощью индексатора `[3, 0]` в `SKMatrix44` объекте.

В трехмерной графической системе трехмерная точка (x, y, z) преобразуется в матрицу с 1 по 4 для умножения на матрицу преобразования 4 по 4.

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Аналогично 2D-преобразованиям, которые выполняются в трех измерениях, предполагается, что трехмерные преобразования выполняются в четырех измерениях. Четвертое измерение называется W, а трехмерное пространство — в области 4D, в которой координаты W равны 1. Формулы преобразования выглядят следующим образом:

`x' = M11·x + M21·y + M31·z + M41`

`y' = M12·x + M22·y + M32·z + M42`

`z' = M13·x + M23·y + M33·z + M43`

`w' = M14·x + M24·y + M34·z + M44`

Очевидно, что из формул преобразования, которые ячейки `M11` , `M22` , `M33` являются коэффициентами масштабирования в направлениях x, y и z, и, и `M41` `M42` `M43` являются коэффициентами перевода в направлениях x, y и z.

Чтобы преобразовать эти координаты обратно в трехмерное пространство, где W равняется 1, координаты x, y и z делятся на W ":

`x" = x' / w'`

`y" = y' / w'`

`z" = z' / w'`

`w" = w' / w' = 1`

Это деление на w обеспечивает перспективу трехмерного пространства. Если w ' равно 1, то перспектива не возникает.

Вращение в трехмерном пространстве может быть довольно сложным, но простейшими поворотами являются значения осей X, Y и Z. Поворот угла, α вокруг оси X, — это матрица:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Значения X остаются неизменными при этом преобразовании. Поворот вокруг оси Y оставляет значения Y без изменений:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Поворот вокруг оси Z такой же, как и в двухмерной графике:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Направление вращения подразумевается правой или левойом системы координат. Это левая система, поэтому, если вы назначите бегунок слева, чтобы увеличить значения для определенной оси, вправо для поворота по оси X, для вращения вокруг оси Y и для поворота вокруг оси Z, то кривая других пальцев указывает направление поворота для положительных углов.

`SKMatrix44`содержит обобщенные статические [`CreateRotation`](xref:SkiaSharp.SKMatrix44.CreateRotation(System.Single,System.Single,System.Single,System.Single)) [`CreateRotationDegrees`](xref:SkiaSharp.SKMatrix44.CreateRotationDegrees(System.Single,System.Single,System.Single,System.Single)) методы и, которые позволяют указать ось, вокруг которой происходит поворот:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

Для поворота вокруг оси X установите первые три аргумента в значение 1, 0, 0. Для поворота вокруг оси Y задайте для них значение 0, 1, 0, а для поворота вокруг оси Z задайте для них значение 0, 0, 1.

Четвертый столбец с 4 по 4 — для перспективы. `SKMatrix44`Метод не имеет методов для создания преобразований перспективы, но вы можете создать его самостоятельно, используя следующий код:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Причина имени аргумента `depth` будет очевидна чуть позже. Этот код создает матрицу:

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Формулы преобразования приводят к следующему вычислению w ':

`w' = –z / depth + 1`

Это позволяет уменьшить координаты X и Y, если значения Z меньше нуля (концептуально за плоскости XY) и для увеличения координат X и Y для положительных значений Z. Если координата Z равна `depth` , то w равен нулю, а координаты становятся бесконечными. Трехмерные графические системы создаются на основе метафоры камеры, а `depth` значение здесь представляет расстояние камеры от источника координат. Если графический объект имеет координату Z, которая является `depth` единицей от начала координат, она концептуально касается линзы камеры и становится бесконечно большой.

Помните, что вы, вероятно, используете это `perspectiveMatrix` значение в сочетании с матрицами вращения. Если объект Graphics, для которого выполняется вращение, имеет координаты X или Y `depth` , превышающие значение, то поворот этого объекта в трехмерном пространстве, скорее всего, будет заключаться в Z-координаты больше `depth` . Это следует избегать. При создании `perspectiveMatrix` нужно присвоить `depth` значение достаточно большим для всех координат графического объекта независимо от того, как оно поворачивается. Это гарантирует, что ни один из делений не будет равен нулю.

Объединение трехмерных поворотов и перспективы требует одновременного умножения матриц 4 на 4. Для этой цели `SKMatrix44` определяет методы объединения. Если `A` и `B` являются `SKMatrix44` объектами, следующий код задает значение, равное x B:

```csharp
A.PostConcat(B);
```

Когда матрица преобразования размером 4 на 4 используется в двухмерной графической системе, она применяется к 2D-объектам. Эти объекты являются плоскими и считаются нулевыми координатами Z. Умножение преобразования немного упрощает преобразование, показанное ранее:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Значение 0 для z приводит к формулам преобразования, которые не охватывают ячейки в третьей строке матрицы:

x "= M11 · x + M21 · y + M41

y "= M12 · x + M22 · y + M42

z ' = M13 · x + M23 · y + M43

w ' = M14 · x + M24 · y + M44

Более того, координата z также не имеет значения. При отображении трехмерного объекта в двухмерной графической системе она сворачивается в двухмерный объект, игнорируя значения координат Z. Формулы преобразования в действительности представляют собой следующие два:

`x" = x' / w'`

`y" = y' / w'`

Это означает, что третья строка *и* третий столбец матрицы с 4 по 4 можно игнорировать.

Но если это так, то почему матрица с 4 по 4 даже требуется в первую очередь?

Третья строка и третий столбец 4 на 4 не имеют значения для двумерных преобразований, а третья строка и *столбец играют роль* до тех пор, пока различные `SKMatrix44` значения умножаются вместе. Например, предположим, что вы умножаете поворот на ось Y с помощью преобразования «перспектива»:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

В этом продукте ячейка `M14` теперь содержит значение перспективы. Если необходимо применить эту матрицу к 2D-объектам, то третья строка и столбец исключаются для их преобразования в матрицу 3 на 3:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Теперь его можно использовать для преобразования двухмерной точки.

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Формулы преобразования:

`x' = cos(α)·x`

`y' = y`

`z' = (sin(α)/depth)·x + 1`

Теперь разделите все на z:

`x" = cos(α)·x / ((sin(α)/depth)·x + 1)`

`y" = y / ((sin(α)/depth)·x + 1)`

Когда двумерные объекты поворачиваются с положительным углом вокруг оси Y, положительные значения X переходят в фон, а отрицательные значения X — на передний план. Значения X находятся ближе к оси Y (которая регулируется значением косинуса), так как координаты в самой оси Y становятся меньше или больше по мере того, как они перемещаются в средство просмотра или ближе к средству просмотра.

При использовании `SKMatrix44` выполните все операции 3D-вращения и перспективы, умножив различные `SKMatrix44` значения. Затем можно извлечь двухмерную матрицу размером 3 на 3 из матрицы 4 на 4 с помощью [`Matrix`](xref:SkiaSharp.SKMatrix44.Matrix) свойства `SKMatrix44` класса. Это свойство возвращает знакомое `SKMatrix` значение.

С помощью **многостраничного вращения можно** поэкспериментировать с поворотом объемной графики. В файле [**Rotation3DPage. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) создаются четыре ползунки, чтобы задать поворот вокруг осей X, Y и Z, а также задать значение глубины:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.Rotation3DPage"
             Title="Rotation 3D">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
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
                    <Setter Property="Maximum" Value="360" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="xRotateSlider"
                Grid.Row="0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference xRotateSlider},
                              Path=Value,
                              StringFormat='X-Axis Rotation = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="yRotateSlider"
                Grid.Row="2"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference yRotateSlider},
                              Path=Value,
                              StringFormat='Y-Axis Rotation = {0:F0}'}"
               Grid.Row="3" />

        <Slider x:Name="zRotateSlider"
                Grid.Row="4"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zRotateSlider},
                              Path=Value,
                              StringFormat='Z-Axis Rotation = {0:F0}'}"
               Grid.Row="5" />

        <Slider x:Name="depthSlider"
                Grid.Row="6"
                Maximum="2500"
                Minimum="250"
                ValueChanged="OnSliderValueChanged" />

        <Label Grid.Row="7"
               Text="{Binding Source={x:Reference depthSlider},
                              Path=Value,
                              StringFormat='Depth = {0:F0}'}" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="8"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Обратите внимание, что `depthSlider` инициализируется со `Minimum` значением 250. Это означает, что поворот 2D-объекта имеет координаты X и Y, ограниченные кругом, определенным радиусом в 250 пикселя по отношению к источнику. Любой поворот этого объекта в трехмерном пространстве всегда приведет к тому, что значения координат будут меньше 250.

Файл кода программной части [**Rotation3DPage.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) загружается в битовую карту 300 пикселей в квадрате:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Если трехмерное преобразование центрируется по этому точечному рисунку, то диапазоны координат X и Y находятся в диапазоне от – 150 до 150, а углы — 212 пикселей от центра, так что все находится в радиусе на уровне 250 пикселя.

`PaintSurface`Обработчик создает `SKMatrix44` объекты на основе ползунков и умножает их вместе с помощью `PostConcat` . `SKMatrix`Значение, извлеченное из последнего `SKMatrix44` объекта, окружено преобразованиями «сдвиг» для центрирования вращения в центре экрана.

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
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

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Use 3D matrix for 3D rotations and perspective
        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, (float)xRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, (float)yRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, (float)zRotateSlider.Value));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / (float)depthSlider.Value;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the bitmap
        canvas.SetMatrix(matrix);
        float xBitmap = xCenter - bitmap.Width / 2;
        float yBitmap = yCenter - bitmap.Height / 2;
        canvas.DrawBitmap(bitmap, xBitmap, yBitmap);
    }
}
```

При эксперименте с четвертым ползунком вы заметите, что другие параметры глубины не перемещают объект дальше от средства просмотра, а вместо этого изменяют область воздействия перспективы:

[![](3d-rotation-images/rotation3d-small.png "Triple screenshot of the Rotation 3D page")](3d-rotation-images/rotation3d-large.png#lightbox "Triple screenshot of the Rotation 3D page")

**Анимированный поворот 3D** также используется `SKMatrix44` для анимации текстовой строки в трехмерном пространстве. `textPaint`Объект, заданный как поле, используется в конструкторе для определения границ текста:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    SKCanvasView canvasView;
    float xRotationDegrees, yRotationDegrees, zRotationDegrees;
    string text = "SkiaSharp";
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        TextSize = 100,
        StrokeWidth = 3,
    };
    SKRect textBounds;

    public AnimatedRotation3DPage()
    {
        Title = "Animated Rotation 3D";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Measure the text
        textPaint.MeasureText(text, ref textBounds);
    }
    ...
}
```

`OnAppearing`Переопределение определяет три Xamarin.Forms `Animation` объекта для анимации `xRotationDegrees` полей, `yRotationDegrees` и `zRotationDegrees` с разной скоростью. Обратите внимание, что для периодов этих анимаций заданы простые числа (5 секунд, 7 секунд и 11 секунд), поэтому общее сочетание будет повторяться только каждые 385 секунд или более 10 минут:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();

        new Animation((value) => xRotationDegrees = 360 * (float)value).
            Commit(this, "xRotationAnimation", length: 5000, repeat: () => true);

        new Animation((value) => yRotationDegrees = 360 * (float)value).
            Commit(this, "yRotationAnimation", length: 7000, repeat: () => true);

        new Animation((value) =>
        {
            zRotationDegrees = 360 * (float)value;
            canvasView.InvalidateSurface();
        }).Commit(this, "zRotationAnimation", length: 11000, repeat: () => true);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        this.AbortAnimation("xRotationAnimation");
        this.AbortAnimation("yRotationAnimation");
        this.AbortAnimation("zRotationAnimation");
    }
    ...
}
```

Как и в предыдущей программе, `PaintCanvas` обработчик создает `SKMatrix44` значения для поворота и перспективы и умножает их вместе:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Scale so text fits
        float scale = Math.Min(info.Width / textBounds.Width,
                               info.Height / textBounds.Height);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(scale, scale));

        // Calculate composite 3D transforms
        float depth = 0.75f * scale * textBounds.Width;

        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, xRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, yRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, zRotationDegrees));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / depth;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the text
        canvas.SetMatrix(matrix);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;
        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Этот трехмерный поворот заключается в использовании нескольких двумерных преобразований для перемещения центра вращения в центр экрана и для масштабирования размера текстовой строки таким образом, чтобы она совпадала с шириной экрана:

[![](3d-rotation-images/animatedrotation3d-small.png "Triple screenshot of the Animated Rotation 3D page")](3d-rotation-images/animatedrotation3d-large.png#lightbox "Triple screenshot of the Animated Rotation 3D page")

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
