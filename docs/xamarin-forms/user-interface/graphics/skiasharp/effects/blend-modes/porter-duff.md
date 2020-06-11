---
Title: "Портер-Дуфф Blend Mode" Description: "использование режимов смешения Портер-Дуфф для создания сцен на основе исходных и конечных образов".
MS. произв. Xamarin MS. Technology: Xamarin-skiasharp MS. AssetID: 57F172F8-BA03-43EC-A215-ED6B78696BB5 Автор: давидбритч MS. author: дабритч MS. Дата: 08/23/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="porter-duff-blend-modes"></a>Режимы смешения Портер-Дуфф

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Режимы наложения Портер-Дуфф наименованы после Томас Портер и Tom Дуфф, которые разработали многофакторную композицию при работе с Лукасфилм. [_Цифровые изображения_](https://graphics.pixar.com/library/Compositing/paper.pdf) , составленные на бумаге, были опубликованы в июле 1984 _, а также_на страницах 253 – 259. Эти режимы наложения являются обязательными для компоновки, что заключается в сборке различных изображений в составной сцене:

![Портер — пример Дуфф](porter-duff-images/PorterDuffSample.png "Портер — пример Дуфф")

## <a name="porter-duff-concepts"></a>Портер — основные понятия Дуфф

Предположим, что прямоугольник с коричневой азбукой занимает слева и сверху два трети поверхности экрана:

![Портер — назначение Дуфф](porter-duff-images/PorterDuffDst.png "Портер — назначение Дуфф")

Эта область называется _назначением_ или иногда _фоном_ или _заднем_.

Вы хотите нарисовать следующий прямоугольник, размер которого совпадает с размером места назначения. Прямоугольник является прозрачным, за исключением области блуиш, которая занимает справа и снизу две трети:

![Портер — источник Дуфф](porter-duff-images/PorterDuffSrc.png "Портер — источник Дуфф")

Это называется _источником_ или иногда _передним планом_.

При отображении исходного кода в месте назначения происходит следующее:

![Портер — источник Дуфф](porter-duff-images/PorterDuffSrcOver.png "Портер — источник Дуфф")

Прозрачные пикселы источника позволяют отображать фон, а исходные пиксели блуиш закрывают фон. Это обычная ситуация, и она называется в SkiaSharp как `SKBlendMode.SrcOver` . Это значение является значением по умолчанию для `BlendMode` свойства при `SKPaint` первом создании объекта.

Однако для другого действия можно указать другой режим смешения. Если указать `SKBlendMode.DstOver` , то в области, где пересекается источник и назначение, вместо источника появится место назначения.

![Портер-Дуфф назначение](porter-duff-images/PorterDuffDstOver.png "Портер-Дуфф назначение")

`SKBlendMode.DstIn`Режим Blend отображает только область, в которой назначение и источник пересекается с цветом назначения.

![Портер — назначение Дуфф в](porter-duff-images/PorterDuffDstIn.png "Портер — назначение Дуфф в")

Режим смешения `SKBlendMode.Xor` (исключающее или) приводит к тому, что ничего не отображается при перекрытии двух областей:

![Портер-Дуфф Exclusive или](porter-duff-images/PorterDuffXor.png "Портер-Дуфф Exclusive или")

Цветные прямоугольники назначения и источника фактически делят поверхность отображения на четыре уникальных области, которые могут быть окрашены различными способами, соответствующими наличию целевого и исходного прямоугольников.

![Портер — Дуфф](porter-duff-images/PorterDuff.png "Портер — Дуфф")

Верхний правый и нижний левый прямоугольник всегда пуст, так как назначение и источник прозрачны в этих областях. Целевой цвет занимают левую верхнюю область, поэтому область может быть либо окрашена в цвет назначения, либо вообще не отображаться. Аналогичным образом исходный цвет занимают нижнюю правую область, поэтому область может быть окрашена в цвет источника или вообще не отображаться. Пересечение целевого объекта и источника в середине может быть окрашено в цвет назначения, исходный цвет или вообще.

Общее число комбинаций равно 2 (для верхнего левого) времени 2 (для нижнего правого), то есть 3 (для центра) или 12. Это 12 основных режима компоновки Портер-Дуфф.

В конце _компоновки цифровых изображений_ (стр. 256) Портер и Дуфф добавляют более 13-й режим с названием _плюс_ (соответствующий `SKBlendMode.Plus` элементу SkiaSharp и режиму W3C, который не следует путать с режимом " _осветлить_ " W3C). _Lighter_ Этот `Plus` режим добавляет цвета назначения и источника, процесс, который будет описан более подробно в ближайшее время.

СКИА добавляет режим 14 с именем `Modulate` , который очень похож на, `Plus` за исключением того, что цвета назначения и источника умножаются. Он может рассматриваться как дополнительный режим Портер-Дуфф Blend.

Ниже приведены 14 режимов Портер-Дуфф, как определено в SkiaSharp. В таблице показано, как цвет каждого из трех непустых областей на схеме выше:

| Режим       | Назначение | Крайне | Источник |
| ---------- |:-----------:|:------------:|:------:|
| `Clear`    |             |              |        |
| `Src`      |             | Источник       | X      |
| `Dst`      | X           | Назначение  |        |
| `SrcOver`  | X           | Источник       | X      |
| `DstOver`  | X           | Назначение  | X      |
| `SrcIn`    |             | Источник       |        |
| `DstIn`    |             | Назначение  |        |
| `SrcOut`   |             |              | X      |
| `DstOut`   | X           |              |        |
| `SrcATop`  | X           | Источник       |        |
| `DstATop`  |             | Назначение  | X      |
| `Xor`      | X           |              | X      |
| `Plus`     | X           | SUM          | X      |
| `Modulate` |             | Продукт      |        | 

Эти режимы наложения являются симметричными. Источник и назначение могут быть заменены, и все режимы по-прежнему доступны.

Соглашение об именовании режимов соответствует следующим простым правилам.

- **Src** или **DST** само по себе означает, что видимы только исходные или конечные Пиксели.
- Суффикс **over** указывает, что отображается в пересечениех. Источник или назначение нарисованы "поверх других".
- Суффикс **in** означает, что окрашено только пересечение. Выходные данные ограничиваются только частью источника или назначения, которая является "в другом".
- Суффикс **out** означает, что пересечение не окрашено. Выходными данными является только часть источника или назначения, которая является "out" пересечения.
- Суффикс **поверх** — это объединение **в** и из **.** Он включает область, где источник или назначение — это «поверх» другого.

Обратите внимание на разницу `Plus` с `Modulate` режимами и. Эти режимы выполняют вычисления другого типа в исходном и целевом точках. Более подробно они описаны ниже.

На странице **сетки Портер-Дуфф** показаны все 14 режимов на одном экране в виде сетки. Каждый режим является отдельным экземпляром `SKCanvasView` . По этой причине класс является производным от `SKCanvasView` именованного `PorterDuffCanvasView` . Статический конструктор создает два точечных рисунка одинакового размера: один с пустым прямоугольником в левой области, а другой — с блуиш прямоугольником:

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    static SKBitmap srcBitmap, dstBitmap;

    static PorterDuffCanvasView()
    {
        dstBitmap = new SKBitmap(300, 300);
        srcBitmap = new SKBitmap(300, 300);

        using (SKPaint paint = new SKPaint())
        {
            using (SKCanvas canvas = new SKCanvas(dstBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0xC0, 0x80, 0x00);
                canvas.DrawRect(new SKRect(0, 0, 200, 200), paint);
            }
            using (SKCanvas canvas = new SKCanvas(srcBitmap))
            {
                canvas.Clear();
                paint.Color = new SKColor(0x00, 0x80, 0xC0);
                canvas.DrawRect(new SKRect(100, 100, 300, 300), paint);
            }
        }
    }
    ···
}
```

Конструктор экземпляра имеет параметр типа `SKBlendMode` . Он сохраняет этот параметр в поле. 

```csharp
class PorterDuffCanvasView : SKCanvasView
{
    ···
    SKBlendMode blendMode;

    public PorterDuffCanvasView(SKBlendMode blendMode)
    {
        this.blendMode = blendMode;
    }

    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find largest square that fits
        float rectSize = Math.Min(info.Width, info.Height);
        float x = (info.Width - rectSize) / 2;
        float y = (info.Height - rectSize) / 2;
        SKRect rect = new SKRect(x, y, x + rectSize, y + rectSize);

        // Draw destination bitmap
        canvas.DrawBitmap(dstBitmap, rect);

        // Draw source bitmap
        using (SKPaint paint = new SKPaint())
        {
            paint.BlendMode = blendMode;
            canvas.DrawBitmap(srcBitmap, rect, paint);
        }

        // Draw outline
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 2;
            rect.Inflate(-1, -1);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

`OnPaintSurface`Переопределение изображает два растровых изображения. Первый рисуется обычным образом:

```csharp
canvas.DrawBitmap(dstBitmap, rect);
```

Второй объект рисуется с помощью `SKPaint` объекта, в котором `BlendMode` свойству было присвоено значение аргумента конструктора:

```csharp
using (SKPaint paint = new SKPaint())
{
    paint.BlendMode = blendMode;
    canvas.DrawBitmap(srcBitmap, rect, paint);
}
```

Оставшаяся часть `OnPaintSurface` переопределения рисует прямоугольник вокруг точечного рисунка, чтобы показать их размеры.

`PorterDuffGridPage`Класс создает экземпляры четырнадцать, по `PorterDurffCanvasView` одному для каждого элемента `blendModes` массива. Порядок `SKBlendModes` элементов в массиве немного отличается от порядка таблицы, чтобы располагать похожие друг в друга режимы. 14 экземпляров `PorterDuffCanvasView` организованы вместе с метками в `Grid` :

```csharp
public class PorterDuffGridPage : ContentPage
{
    public PorterDuffGridPage()
    {
        Title = "Porter-Duff Grid";

        SKBlendMode[] blendModes =
        {
            SKBlendMode.Src, SKBlendMode.Dst, SKBlendMode.SrcOver, SKBlendMode.DstOver,
            SKBlendMode.SrcIn, SKBlendMode.DstIn, SKBlendMode.SrcOut, SKBlendMode.DstOut,
            SKBlendMode.SrcATop, SKBlendMode.DstATop, SKBlendMode.Xor, SKBlendMode.Plus,
            SKBlendMode.Modulate, SKBlendMode.Clear
        };

        Grid grid = new Grid
        {
            Margin = new Thickness(5)
        };

        for (int row = 0; row < 4; row++)
        {
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
            grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Star });
        }

        for (int col = 0; col < 3; col++)
        {
            grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });
        }

        for (int i = 0; i < blendModes.Length; i++)
        {
            SKBlendMode blendMode = blendModes[i];
            int row = 2 * (i / 4);
            int col = i % 4;

            Label label = new Label
            {
                Text = blendMode.ToString(),
                HorizontalTextAlignment = TextAlignment.Center
            };
            Grid.SetRow(label, row);
            Grid.SetColumn(label, col);
            grid.Children.Add(label);

            PorterDuffCanvasView canvasView = new PorterDuffCanvasView(blendMode);

            Grid.SetRow(canvasView, row + 1);
            Grid.SetColumn(canvasView, col);
            grid.Children.Add(canvasView);
        }

        Content = grid;
    }
}
```

Ниже приведен результат.

[![Портер-Дуфф, сетка](porter-duff-images/PorterDuffGrid.png "Портер-Дуфф, сетка")](porter-duff-images/PorterDuffGrid-Large.png#lightbox)

Необходимо убедить, что прозрачность важна для правильной работы режимов Портер-Дуфф Blend. `PorterDuffCanvasView`Класс содержит всего три вызова `Canvas.Clear` метода. Все они используют метод без параметров, который задает прозрачность всех пикселей:

```csharp
canvas.Clear();
```

Попробуйте изменить любой из этих вызовов, чтобы Пиксели были установлены в непрозрачный белый цвет:

```csharp
canvas.Clear(SKColors.White);
```

После этого изменения некоторые режимы наложения будут работать, но другие не будут. Если задать для фона исходного точечного рисунка белый цвет, то `SrcOver` режим не будет работать, так как в исходном точечном рисунке нет прозрачных пикселов, позволяющих отобразить назначение. Если задать фон конечного точечного рисунка или холста как белый, то не будет `DstOver` работать, так как в месте назначения отсутствуют прозрачные пиксели.

Может существовать искушения для замены точечных рисунков на странице **сетки Портер-Дуфф** с более простыми `DrawRect` вызовами. Это будет работать для прямоугольника назначения, но не для исходного прямоугольника. Исходный прямоугольник должен охватывать не только блуиш область. Исходный прямоугольник должен включать прозрачную область, соответствующую цветовой области назначения. Только после этого будут работать эти режимы наложения.

## <a name="using-mattes-with-porter-duff"></a>Использование Мэтт с Портер-Дуфф

На странице "композиция" в области **композиции** отображается пример классической задачи компоновки: необходимо собрать изображение из нескольких частей, включая точечный рисунок с фоновым рисунком, который необходимо устранить. Вот **SeatedMonkey.jpg** точечный рисунок с проблемным фоном:

![Заданная обезьяна](porter-duff-images/SeatedMonkey.jpg "Заданная обезьяна")

При подготовке к композиции создается соответствующая _матовая_ рамка, которая представляет собой еще один черный рисунок, где изображение должно отображаться и прозрачно в противном случае. Этот файл называется **SeatedMonkeyMatte.png** и находится в ресурсах в папке **Media** примера [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) :

![Матовая обезьяна](porter-duff-images/SeatedMonkeyMatte.png "Матовая обезьяна")

Это _не_ слишком экспертное создание. В оптимальном случае матовая должна содержать частично прозрачные пикселы вокруг границы черного пикселя, а это не так.

XAML-файл для страницы "композиция" на **основе кирпича** создает экземпляр `SKCanvasView` и `Button` , который помогает пользователю выполнить процесс составления окончательного образа:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.BrickWallCompositingPage"
             Title="Brick-Wall Compositing">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Button Text="Show sitting monkey"
                HorizontalOptions="Center"
                Margin="0, 10"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

Файл кода программной части загружает два необходимых точечных рисунка и обрабатывает `Clicked` событие `Button` . При каждом `Button` щелчке `step` поле увеличивается и `Text` для свойства задается новое свойство `Button` . Когда `step` достигает 5, возвращается значение 0:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    SKBitmap monkeyBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap matteBitmap = BitmapExtensions.LoadBitmapResource(
        typeof(BrickWallCompositingPage), 
        "SkiaSharpFormsDemos.Media.SeatedMonkeyMatte.png");

    int step = 0;

    public BrickWallCompositingPage ()
    {
        InitializeComponent ();
    }

    void OnButtonClicked(object sender, EventArgs args)
    {
        Button btn = (Button)sender;
        step = (step + 1) % 5;

        switch (step)
        {
            case 0: btn.Text = "Show sitting monkey"; break;
            case 1: btn.Text = "Draw matte with DstIn"; break;
            case 2: btn.Text = "Draw sidewalk with DstOver"; break;
            case 3: btn.Text = "Draw brick wall with DstOver"; break;
            case 4: btn.Text = "Reset"; break;
        }

        canvasView.InvalidateSurface();
    }
    
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        ···
    }
}
```

При первом запуске программы ничего не отображается, за исключением `Button` :

[![Шаг 0 при компоновке кирпича](porter-duff-images/BrickWallCompositing0.png "Шаг 0 при компоновке кирпича")](porter-duff-images/BrickWallCompositing0-Large.png#lightbox)

Нажатие кнопки " `Button` один раз" приводит к `step` увеличению значения 1, а `PaintSurface` обработчик теперь отображает **SeatedMonkey.jpg**:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        float x = (info.Width - monkeyBitmap.Width) / 2;
        float y = info.Height - monkeyBitmap.Height;

        // Draw monkey bitmap
        if (step >= 1)
        {
            canvas.DrawBitmap(monkeyBitmap, x, y);
        }
        ···
    }
}
```

Нет `SKPaint` объекта и, следовательно, режима смешения. Точечный рисунок появится в нижней части экрана:

[![Действие 1 для компоновки кирпича](porter-duff-images/BrickWallCompositing1.png "Действие 1 для компоновки кирпича")](porter-duff-images/BrickWallCompositing1-Large.png#lightbox)

Снова нажмите кнопку `Button` и `step` увеличьте значение до 2. Это важный шаг при отображении файла **SeatedMonkeyMatte.png** :

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw matte to exclude monkey's surroundings
        if (step >= 2)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.BlendMode = SKBlendMode.DstIn;
                canvas.DrawBitmap(matteBitmap, x, y, paint);
            }
        }
        ···
    }
}
```

Режим смешения — `SKBlendMode.DstIn` , что означает, что место назначения будет сохранено в областях, соответствующих непрозрачным областям источника. Оставшаяся часть прямоугольника назначения, соответствующая исходному точечному рисунку, станет прозрачной:

[![Этап 2 при компоновке кирпича](porter-duff-images/BrickWallCompositing2.png "Этап 2 при компоновке кирпича")](porter-duff-images/BrickWallCompositing2-Large.png#lightbox)

Фон был удален. 

Следующим шагом является рисование прямоугольника, похожего на тротуара, на котором находится обезьяна. Внешний вид этого тротуара основан на композиции двух шейдеров: сплошной шейдер цвета и шейдер шума Perl:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        const float sidewalkHeight = 80;
        SKRect rect = new SKRect(info.Rect.Left, info.Rect.Bottom - sidewalkHeight,
                                 info.Rect.Right, info.Rect.Bottom);

        // Draw gravel sidewalk for monkey to sit on
        if (step >= 3)
        {
            using (SKPaint paint = new SKPaint())
            {
                paint.Shader = SKShader.CreateCompose(
                                    SKShader.CreateColor(SKColors.SandyBrown),
                                    SKShader.CreatePerlinNoiseTurbulence(0.1f, 0.3f, 1, 9));

                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(rect, paint);
            }
        }
        ···
    }
}
```

Так как этот тротуара должен идти позади самой обезьяны, режим смешения имеет значение `DstOver` . Назначение отображается только в том случае, если фон прозрачен:

[![Шаг 3. Компоновка модуля-стена](porter-duff-images/BrickWallCompositing3.png "Шаг 3. Компоновка модуля-стена")](porter-duff-images/BrickWallCompositing3-Large.png#lightbox)

Последним шагом является добавление кирпича. Программа использует плитку точечного рисунка на стороне модуля, доступную в качестве статического свойства `BrickWallTile` в `AlgorithmicBrickWallPage` классе. Преобразование перевода добавляется к `SKShader.CreateBitmap` вызову для сдвига плиток, чтобы нижняя строка была полной плиткой:

```csharp
public partial class BrickWallCompositingPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        ···
        // Draw bitmap tiled brick wall behind monkey
        if (step >= 4)
        {
            using (SKPaint paint = new SKPaint())
            {
                SKBitmap bitmap = AlgorithmicBrickWallPage.BrickWallTile;
                float yAdjust = (info.Height - sidewalkHeight) % bitmap.Height;

                paint.Shader = SKShader.CreateBitmap(bitmap,
                                                     SKShaderTileMode.Repeat,
                                                     SKShaderTileMode.Repeat,
                                                     SKMatrix.MakeTranslation(0, yAdjust));
                paint.BlendMode = SKBlendMode.DstOver;
                canvas.DrawRect(info.Rect, paint);
            }
        }
    }
}
```

Для удобства `DrawRect` вызов отображает этот шейдер на всем холсте, но `DstOver` режим вывода ограничивается только областью холста, которая остается прозрачной.

[![Этап 4 при компоновке кирпича](porter-duff-images/BrickWallCompositing4.png "Этап 4 при компоновке кирпича")](porter-duff-images/BrickWallCompositing4-Large.png#lightbox)

Очевидно, существуют и другие способы создания этой сцены. Она может быть построена в фоновом режиме и передаваться на передний план. Но использование режимов наложения обеспечивает большую гибкость. В частности, использование Мэтт позволяет исключить фон растрового изображения из составной сцены.

Как вы узнали из статьи, [вырезая с помощью путей и регионов](../../curves/clipping.md), `SKCanvas` класс определяет три типа обрезки, соответствующие [`ClipRect`](xref:SkiaSharp.SKCanvas.ClipRect*) [`ClipPath`](xref:SkiaSharp.SKCanvas.ClipPath*) [`ClipRegion`](xref:SkiaSharp.SKCanvas.ClipRegion*) методам, и. Режимы наложения «Портер-Дуфф Blend» добавляют еще один тип обрезки, что позволяет ограничивать изображение любым, что можно нарисовать, включая растровые изображения. Мэтт, используемый при **компоновке кирпичного модуля** , по сути определяет область обрезки.

## <a name="gradient-transparency-and-transitions"></a>Прозрачность градиента и переходы

Примеры Портер-Дуфф Blend Mode, приведенные выше в этой статье, содержат все связанные изображения, состоящие из непрозрачных и прозрачных пикселов, но не частично прозрачных пикселей. Функции режима Blend также определяются для этих пикселов. Следующая таблица является более формальным определением режимов смешения Портер-Дуфф, в которых используется нотация, найденная в [**ссылке СКИА скблендмоде**](https://skia.org/user/api/SkBlendMode_Reference). (Поскольку **ссылка скблендмоде** является ссылкой на СКИА, используется синтаксис C++.)

По сути, красный, зеленый, синий и альфа-компоненты каждого пикселя преобразуются из байтов в числа с плавающей запятой в диапазоне от 0 до 1. Для альфа-канала 0 является полностью прозрачным, а 1 — полностью непрозрачным

Нотация в приведенной ниже таблице использует следующие аббревиатуры:

- **Da** — это альфа-канал назначения
- **Контроллер домена** является ЦЕЛЕВЫМ цветом RGB
- **SA** является исходным альфа-каналом
- **SC** — цвет исходного RGB

Цвета RGB предварительно умножаются на альфа-значение. Например, если **SC** представляет чисто красный, а **SA** — 0X80, то цвет RGB — **(0x80, 0, 0)**. Если **SA** равно 0, то все компоненты RGB также равны нулю.

Результат отображается в квадратных скобках с альфа-каналом, а цвет RGB отделяется запятой: **[альфа, Color]**. Для цвета вычисление выполняется отдельно для красного, зеленого и синего компонентов:

| Режим       | Операция |
| ---------- | --------- |
| `Clear`    | [0, 0]    |
| `Src`      | [SA, SC]  |
| `Dst`      | [Da, DC]  |
| `SrcOver`  | [SA + DA · (1 – SA), SC + DC · (1 – SA) | 
| `DstOver`  | [DA + SA · (1 – Da), DC + SC · (1 – Da) |
| `SrcIn`    | Срок Da, SC · Da |
| `DstIn`    | Da SA, DC · Срок |
| `SrcOut`   | Срок (1 – Da), SC · (1 – Da)] |
| `DstOut`   | Da (1 – SA), DC · (1 – SA)] |
| `SrcATop`  | [Da, SC · Da + DC · (1 – SA)] |
| `DstATop`  | [SA, DC · SA + SC · (1 – Da)] |
| `Xor`      | [SA + DA – 2 · Срок Da, SC · (1 – Da) + DC · (1 – SA)] |
| `Plus`     | [SA + DA, SC + DC] |
| `Modulate` | Срок Da, SC · Постоянный | 

Эти операции проще анализировать, если **Da** и **SA** имеют значение 0 или 1. Например, для режима по умолчанию `SrcOver` , если **SA** имеет значение 0, то **SC** также имеет значение 0, а результат — **[Da, DC]**, альфа-канал и цвет. Если **SA** имеет значение 1, то результатом будет **[SA, SC]**, альфа-канал и цвет, или **[1, SC]**.

`Plus`Режимы и `Modulate` отличаются от других в том, что новые цвета могут быть результатом сочетания источника и назначения. `Plus`Режим может интерпретироваться как с байт-компонентами, так и с компонентами с плавающей запятой. На странице **сетки Портер-Дуфф** , показанной ранее, целевой цвет — **(0xC0, 0x80, 0x00)** , а исходный цвет — **(0x00, 0x80, 0xC0)**. Каждая пара компонентов добавляется, но сумма копируется по 0xFF. Результатом является цвет **(0xC0, 0xFF, 0xC0)**. Это цвет, отображаемый в пересечениех.

Для `Modulate` режима значения RGB необходимо преобразовать в тип с плавающей запятой. Целевой цвет — **(0,75, 0,5, 0)** , а источник — **(0, 0,5, 0,75)**. Каждый из компонентов RGB умножается вместе, а результат равен **(0, 0,25, 0)**. Это цвет, который отображается на пересечении на странице **сетки Портер-Дуфф** для этого режима.

Страница **прозрачность Портер-Дуфф** позволяет исследовать, как режимы наложения Портер-Дуфф работают с графическими объектами, частично прозрачными. Файл XAML включает в себя `Picker` режимы Портер-Дуфф:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaviews="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.PorterDuffTransparencyPage"
             Title="Porter-Duff Transparency">

    <StackLayout>
        <skiaviews:SKCanvasView x:Name="canvasView"
                                VerticalOptions="FillAndExpand"
                                PaintSurface="OnCanvasViewPaintSurface" />

        <Picker x:Name="blendModePicker"
                Title="Blend Mode"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKBlendMode}">
                    <x:Static Member="skia:SKBlendMode.Clear" />
                    <x:Static Member="skia:SKBlendMode.Src" />
                    <x:Static Member="skia:SKBlendMode.Dst" />
                    <x:Static Member="skia:SKBlendMode.SrcOver" />
                    <x:Static Member="skia:SKBlendMode.DstOver" />
                    <x:Static Member="skia:SKBlendMode.SrcIn" />
                    <x:Static Member="skia:SKBlendMode.DstIn" />
                    <x:Static Member="skia:SKBlendMode.SrcOut" />
                    <x:Static Member="skia:SKBlendMode.DstOut" />
                    <x:Static Member="skia:SKBlendMode.SrcATop" />
                    <x:Static Member="skia:SKBlendMode.DstATop" />
                    <x:Static Member="skia:SKBlendMode.Xor" />
                    <x:Static Member="skia:SKBlendMode.Plus" />
                    <x:Static Member="skia:SKBlendMode.Modulate" />
                </x:Array>
            </Picker.ItemsSource>

            <Picker.SelectedIndex>
                3
            </Picker.SelectedIndex>
        </Picker>
    </StackLayout>
</ContentPage>
```

Файл кода программной части заполняет два прямоугольника одинакового размера с помощью линейного градиента. Градиент назначения находится в правом верхнем углу слева направо. Она остается в правом верхнем углу, а затем в центре начинает плавно прозрачно и прозрачна в левом нижнем углу. 

Исходный прямоугольник имеет градиент от левого верхнего угла к нижнему правому. Левый верхний угол — блуиш, но снова плавно прозрачный и прозрачный в правом нижнем углу. 

```csharp
public partial class PorterDuffTransparencyPage : ContentPage
{
    public PorterDuffTransparencyPage()
    {
        InitializeComponent();
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

        // Make square display rectangle smaller than canvas
        float size = 0.9f * Math.Min(info.Width, info.Height);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        SKRect rect = new SKRect(x, y, x + size, y + size);

        using (SKPaint paint = new SKPaint())
        {
            // Draw destination
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Right, rect.Top),
                                new SKPoint(rect.Left, rect.Bottom),
                                new SKColor[] { new SKColor(0xC0, 0x80, 0x00),
                                                new SKColor(0xC0, 0x80, 0x00, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            canvas.DrawRect(rect, paint);

            // Draw source
            paint.Shader = SKShader.CreateLinearGradient(
                                new SKPoint(rect.Left, rect.Top),
                                new SKPoint(rect.Right, rect.Bottom),
                                new SKColor[] { new SKColor(0x00, 0x80, 0xC0), 
                                                new SKColor(0x00, 0x80, 0xC0, 0) },
                                new float[] { 0.4f, 0.6f },
                                SKShaderTileMode.Clamp);

            // Get the blend mode from the picker
            paint.BlendMode = blendModePicker.SelectedIndex == -1 ? 0 :
                                    (SKBlendMode)blendModePicker.SelectedItem;

            canvas.DrawRect(rect, paint);

            // Stroke surrounding rectangle
            paint.Shader = null;
            paint.BlendMode = SKBlendMode.SrcOver;
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Black;
            paint.StrokeWidth = 3;
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Эта программа показывает, что режимы смешения Портер-Дуфф можно использовать с графическими объектами, отличными от точечных рисунков. Однако источник должен включать прозрачную область. Это происходит, так как градиент заполняет прямоугольник, но часть градиента является прозрачной.

Ниже приведены три примера.

[![Прозрачность Портер-Дуфф](porter-duff-images/PorterDuffTransparency.png "Прозрачность Портер-Дуфф")](porter-duff-images/PorterDuffTransparency-Large.png#lightbox)

Конфигурация назначения и источника очень похожа на схемы, показанные на странице 255 исходной бумаги для создания [_цифровых изображений_](https://graphics.pixar.com/library/Compositing/paper.pdf) Портер-Дуфф, но на этой странице показано, что режимы наложения хорошо работают для областей частичной прозрачности.

Для некоторых различных эффектов можно использовать прозрачные градиенты. Одна из возможных вариантов — это маскировка, аналогичная методике, показанной в разделе [**Радиальные градиенты для маскирования**](../shaders/circular-gradients.md#radial-gradients-for-masking) **страницы круговые градиенты SkiaSharp**. Большая часть страницы **маски компоновки** аналогична предыдущей программе. Он загружает ресурс точечного рисунка и определяет прямоугольник, в котором он отображается. Радиальный градиент создается на основе предварительно определенного центра и радиуса:

```csharp
public class CompositingMaskPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
        typeof(CompositingMaskPage),
        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    static readonly SKPoint CENTER = new SKPoint(180, 300);
    static readonly float RADIUS = 120;

    public CompositingMaskPage ()
    {
        Title = "Compositing Mask";

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
                                new SKColor[] { SKColors.Black,
                                                SKColors.Transparent },
                                new float[] { 0.6f, 1 },
                                SKShaderTileMode.Clamp);

            paint.BlendMode = SKBlendMode.DstIn;

            // Display rectangle using that gradient and blend mode
            canvas.DrawRect(rect, paint);
        }

        canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
    }
}
```

Отличие этой программы заключается в том, что градиент начинается с черного в центре и заканчивается прозрачностью. Он отображается на точечном рисунке с режимом смешения `DstIn` , который показывает назначение только в тех областях источника, которые не являются прозрачными.

После `DrawRect` вызова вся поверхность холста прозрачна, за исключением круга, определенного радиальным градиентом. Выполняется окончательный вызов:

```csharp
canvas.DrawColor(SKColors.Pink, SKBlendMode.DstOver);
```

Все прозрачные области холста окрашены в розовый цвет:

[![Маска компоновки](porter-duff-images/CompositingMask.png "Маска компоновки")](porter-duff-images/CompositingMask-Large.png#lightbox)

Можно также использовать режимы Портер-Дуфф и частично прозрачные градиенты для переходов от одного изображения к другому. На странице **переходы градиента** имеется элемент, `Slider` указывающий на уровень хода выполнения в переходе от 0 до 1, а также `Picker` для выбора требуемого типа перехода.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.GradientTransitionsPage"
             Title="Gradient Transitions">
    
    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="progressSlider"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference progressSlider},
                              Path=Value,
                              StringFormat='Progress = {0:F2}'}"
               HorizontalTextAlignment="Center" />

        <Picker x:Name="transitionPicker" 
                Title="Transition" 
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged" />
        
    </StackLayout>
</ContentPage>
```

Файл кода программной части загружает два ресурса точечного рисунка для демонстрации перехода. Это те же два изображения, которые используются на странице **устранения точечных рисунков** , приведенной выше в этой статье. Код также определяет перечисление с тремя элементами, соответствующими трем типам градиентов &mdash; : линейный, радиальный и поворот. Эти значения загружаются в `Picker` :

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    SKBitmap bitmap1 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    SKBitmap bitmap2 = BitmapExtensions.LoadBitmapResource(
        typeof(GradientTransitionsPage),
        "SkiaSharpFormsDemos.Media.FacePalm.jpg");

    enum TransitionMode
    {
        Linear,
        Radial,
        Sweep
    };

    public GradientTransitionsPage ()
    {
        InitializeComponent ();

        foreach (TransitionMode mode in Enum.GetValues(typeof(TransitionMode)))
        {
            transitionPicker.Items.Add(mode.ToString());
        }

        transitionPicker.SelectedIndex = 0;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        canvasView.InvalidateSurface();
    }
    ···
}
```

Файл кода программной части создает три `SKPaint` объекта. `paint0`Объект не использует режим смешения. Этот объект Paint используется для рисования прямоугольника с градиентом, который перемещается от черного к прозрачному, как указано в `colors` массиве. `positions`Массив основан на положении объекта `Slider` , но был настроен несколько. Если значение параметра `Slider` равно минимуму или максимуму, то `progress` значения равны 0 или 1, а одно из двух растровых изображений должно быть полностью видимым. `positions`Для этих значений массив должен быть задан соответствующим образом.

Если `progress` значение равно 0, `positions` массив содержит значения-0,1 и 0. SkiaSharp настроит это первое значение равным 0, что означает, что градиент будет черным только в 0 и прозрачным в противном случае. Если `progress` значение равно 0,5, массив содержит значения 0,45 и 0,55. Градиент имеет черный цвет от 0 до 0,45, затем переходит в прозрачный режим и полностью прозрачен с 0,55 до 1. Если `progress` значение равно 1, то `positions` массив равен 1 и 1,1, что означает, что градиент имеет черный цвет от 0 до 1.

`colors` `position` Массивы и используются в трех методах `SKShader` , которые создают градиент. На основе выбора создается только один из этих шейдеров `Picker` : 

```csharp
public partial class GradientTransitionsPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Assume both bitmaps are square for display rectangle
        float size = Math.Min(info.Width, info.Height);
        SKRect rect = SKRect.Create(size, size);
        float x = (info.Width - size) / 2;
        float y = (info.Height - size) / 2;
        rect.Offset(x, y);

        using (SKPaint paint0 = new SKPaint())
        using (SKPaint paint1 = new SKPaint())
        using (SKPaint paint2 = new SKPaint())
        {
            SKColor[] colors = new SKColor[] { SKColors.Black,
                                               SKColors.Transparent };

            float progress = (float)progressSlider.Value;

            float[] positions = new float[]{ 1.1f * progress - 0.1f,
                                             1.1f * progress };

            switch ((TransitionMode)transitionPicker.SelectedIndex)
            {
                case TransitionMode.Linear:
                    paint0.Shader = SKShader.CreateLinearGradient(
                                        new SKPoint(rect.Left, 0),
                                        new SKPoint(rect.Right, 0),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Radial:
                    paint0.Shader = SKShader.CreateRadialGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        (float)Math.Sqrt(Math.Pow(rect.Width / 2, 2) +
                                                         Math.Pow(rect.Height / 2, 2)),
                                        colors,
                                        positions,
                                        SKShaderTileMode.Clamp);
                    break;

                case TransitionMode.Sweep:
                    paint0.Shader = SKShader.CreateSweepGradient(
                                        new SKPoint(rect.MidX, rect.MidY),
                                        colors,
                                        positions);
                    break;
            }

            canvas.DrawRect(rect, paint0);

            paint1.BlendMode = SKBlendMode.SrcOut;
            canvas.DrawBitmap(bitmap1, rect, paint1);

            paint2.BlendMode = SKBlendMode.DstOver;
            canvas.DrawBitmap(bitmap2, rect, paint2);
        }
    }
}
```

Этот градиент отображается в прямоугольнике без режима смешения. После выполнения этого `DrawRect` вызова холст просто содержит градиент от черного к прозрачному. Количество черного увеличивается с более высоким `Slider` значением.

В последних четырех инструкциях `PaintSurface` обработчика отображаются два растровых изображения. `SrcOut`Режим смешения означает, что первое растровое изображение отображается только в прозрачных областях фона. `DstOver`Режим для второго точечного рисунка означает, что второй точечный рисунок отображается только в тех областях, где первое растровое изображение не отображается.

На следующих снимках экрана показаны три различных типа переходов, каждый из которых имеет метку 50%.

[![Градиентные переходы](porter-duff-images/GradientTransitions.png "Градиентные переходы")](porter-duff-images/GradientTransitions-Large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
