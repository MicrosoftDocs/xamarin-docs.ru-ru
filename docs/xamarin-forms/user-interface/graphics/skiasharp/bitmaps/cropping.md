---
title: Обрезка точечных рисунков SkiaSharp
description: Узнайте, как использовать SkiaSharp для создания пользовательского интерфейса для интерактивного десрибинг прямоугольника обрезки.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 0A79AB27-C69F-4376-8FFE-FF46E4783F30
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d613c4f73c0a377a599b0137ce2f2b557c04ad6a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84572342"
---
# <a name="cropping-skiasharp-bitmaps"></a>Обрезка точечных рисунков SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

В статье [**Создание и рисование растровых изображений SkiaSharp**](drawing.md) описано, как `SKBitmap` объект может быть передан `SKCanvas` конструктору. Любой метод рисования, вызываемый на этом холсте, приводит к отрисовке графики на точечном рисунке. Эти методы рисования включают `DrawBitmap` , что это означает, что этот метод позволяет передавать часть или все одно растровое изображение другому точечному рисунку, возможно, с применением преобразований.

Этот метод можно использовать для обрезки точечного рисунка путем вызова [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) метода с исходными и конечными прямоугольниками:

```csharp
canvas.DrawBitmap(bitmap, sourceRect, destRect);
```

Однако приложения, реализующие обрезку, часто предоставляют интерфейс для пользователя в интерактивном режиме выбора прямоугольника кадрирования:

![Пример обрезки](cropping-images/CroppingSample.png "Пример обрезки")

Эта статья посвящена этому интерфейсу.

## <a name="encapsulating-the-cropping-rectangle"></a>Инкапсуляция прямоугольника обрезки

Полезно изолировать некоторую логику обрезки в классе с именем `CroppingRectangle` . Параметры конструктора включают в себя максимальный прямоугольник, который обычно является размером обрезанного растрового изображения и необязательным пропорциями. Конструктор сначала определяет начальный прямоугольник обрезки, который делает его открытым в `Rect` свойстве типа `SKRect` . Этот начальный прямоугольник обрезки имеет размер 80% от ширины и высоты прямоугольника точечного рисунка, но затем корректируется, если задано пропорции.

```csharp
class CroppingRectangle
{
    ···
    SKRect maxRect;             // generally the size of the bitmap
    float? aspectRatio;

    public CroppingRectangle(SKRect maxRect, float? aspectRatio = null)
    {
        this.maxRect = maxRect;
        this.aspectRatio = aspectRatio;

        // Set initial cropping rectangle
        Rect = new SKRect(0.9f * maxRect.Left + 0.1f * maxRect.Right,
                          0.9f * maxRect.Top + 0.1f * maxRect.Bottom,
                          0.1f * maxRect.Left + 0.9f * maxRect.Right,
                          0.1f * maxRect.Top + 0.9f * maxRect.Bottom);

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            SKRect rect = Rect;
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;
                rect.Left = (maxRect.Width - width) / 2;
                rect.Right = rect.Left + width;
            }
            else
            {
                float height = rect.Width / aspect;
                rect.Top = (maxRect.Height - height) / 2;
                rect.Bottom = rect.Top + height;
            }

            Rect = rect;
        }
    }

    public SKRect Rect { set; get; }
    ···
}
```

Одним из полезных данных, которые `CroppingRectangle` также становятся доступными, является массив `SKPoint` значений, соответствующих четырем углам прямоугольника обрезки в верхнем левом углу, верхнем правом, нижнем правом и нижнем левом углу:

```csharp
class CroppingRectangle
{
    ···
    public SKPoint[] Corners
    {
        get
        {
            return new SKPoint[]
            {
                new SKPoint(Rect.Left, Rect.Top),
                new SKPoint(Rect.Right, Rect.Top),
                new SKPoint(Rect.Right, Rect.Bottom),
                new SKPoint(Rect.Left, Rect.Bottom)
            };
        }
    }
    ···
}
```

Этот массив используется в следующем методе, который называется `HitTest` . `SKPoint`Параметр — это точка, соответствующая сенсору пальца или щелчку мышью. Метод возвращает индекс (0, 1, 2 или 3), соответствующий углу, на котором затронут палец или указатель мыши, на расстоянии, заданном `radius` параметром:

```csharp
class CroppingRectangle
{
    ···
    public int HitTest(SKPoint point, float radius)
    {
        SKPoint[] corners = Corners;

        for (int index = 0; index < corners.Length; index++)
        {
            SKPoint diff = point - corners[index];

            if ((float)Math.Sqrt(diff.X * diff.X + diff.Y * diff.Y) < radius)
            {
                return index;
            }
        }

        return -1;
    }
    ···
}
```

Если касание или точка мыши не находятся в `radius` единицах какого либо угла, метод возвращает &ndash; 1.

Последний метод в вызывается `CroppingRectangle` `MoveCorner` , который вызывается в ответ на перемещение касания или мыши. Два параметра указывают индекс перемещаемого угла и новое расположение этого угла. Первая половина метода настраивает прямоугольник обрезки на основе нового расположения угла, но всегда в пределах границ `maxRect` , что является размером растрового изображения. Эта логика также учитывает поле, `MINIMUM` чтобы избежать свертывания прямоугольника кадрирования в Nothing:

```csharp
class CroppingRectangle
{
    const float MINIMUM = 10;   // pixels width or height
    ···
    public void MoveCorner(int index, SKPoint point)
    {
        SKRect rect = Rect;

        switch (index)
        {
            case 0: // upper-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 1: // upper-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Top = Math.Min(Math.Max(point.Y, maxRect.Top), rect.Bottom - MINIMUM);
                break;

            case 2: // lower-right
                rect.Right = Math.Max(Math.Min(point.X, maxRect.Right), rect.Left + MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;

            case 3: // lower-left
                rect.Left = Math.Min(Math.Max(point.X, maxRect.Left), rect.Right - MINIMUM);
                rect.Bottom = Math.Max(Math.Min(point.Y, maxRect.Bottom), rect.Top + MINIMUM);
                break;
        }

        // Adjust for aspect ratio
        if (aspectRatio.HasValue)
        {
            float aspect = aspectRatio.Value;

            if (rect.Width > aspect * rect.Height)
            {
                float width = aspect * rect.Height;

                switch (index)
                {
                    case 0:
                    case 3: rect.Left = rect.Right - width; break;
                    case 1:
                    case 2: rect.Right = rect.Left + width; break;
                }
            }
            else
            {
                float height = rect.Width / aspect;

                switch (index)
                {
                    case 0:
                    case 1: rect.Top = rect.Bottom - height; break;
                    case 2:
                    case 3: rect.Bottom = rect.Top + height; break;
                }
            }
        }

        Rect = rect;
    }
}
```

Вторая половина метода корректирует дополнительные пропорции.

Помните, что все в этом классе находятся в блоках пикселей.

## <a name="a-canvas-view-just-for-cropping"></a>Представление холста только для обрезки

`CroppingRectangle`Класс, который вы только что видели `PhotoCropperCanvasView` , используется классом, который является производным от `SKCanvasView` . Этот класс отвечает за отображение точечного рисунка и прямоугольника обрезки, а также обработку событий касания или мыши для изменения прямоугольника кадрирования.

Для `PhotoCropperCanvasView` конструктора требуется точечный рисунок. Пропорции являются необязательными. Конструктор создает экземпляр объекта типа `CroppingRectangle` на основе этого растрового изображения и пропорций и сохраняет его как поле:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        this.bitmap = bitmap;

        SKRect bitmapRect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
        croppingRect = new CroppingRectangle(bitmapRect, aspectRatio);
        ···
    }
    ···
}
```

Поскольку этот класс является производным от `SKCanvasView` , ему не нужно устанавливать обработчик для `PaintSurface` события. Вместо этого он может переопределить свой `OnPaintSurface` метод. Метод отображает точечный рисунок и использует пару объектов, `SKPaint` сохраненных в виде полей, для отрисовки текущего прямоугольника обрезки:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    const int CORNER = 50;      // pixel length of cropper corner
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;
    ···
    // Drawing objects
    SKPaint cornerStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 10
    };

    SKPaint edgeStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.White,
        StrokeWidth = 2
    };
    ···
    protected override void OnPaintSurface(SKPaintSurfaceEventArgs args)
    {
        base.OnPaintSurface(args);

        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Gray);

        // Calculate rectangle for displaying bitmap
        float scale = Math.Min((float)info.Width / bitmap.Width, (float)info.Height / bitmap.Height);
        float x = (info.Width - scale * bitmap.Width) / 2;
        float y = (info.Height - scale * bitmap.Height) / 2;
        SKRect bitmapRect = new SKRect(x, y, x + scale * bitmap.Width, y + scale * bitmap.Height);
        canvas.DrawBitmap(bitmap, bitmapRect);

        // Calculate a matrix transform for displaying the cropping rectangle
        SKMatrix bitmapScaleMatrix = SKMatrix.MakeIdentity();
        bitmapScaleMatrix.SetScaleTranslate(scale, scale, x, y);

        // Display rectangle
        SKRect scaledCropRect = bitmapScaleMatrix.MapRect(croppingRect.Rect);
        canvas.DrawRect(scaledCropRect, edgeStroke);

        // Display heavier corners
        using (SKPath path = new SKPath())
        {
            path.MoveTo(scaledCropRect.Left, scaledCropRect.Top + CORNER);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Left + CORNER, scaledCropRect.Top);

            path.MoveTo(scaledCropRect.Right - CORNER, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Top + CORNER);

            path.MoveTo(scaledCropRect.Right, scaledCropRect.Bottom - CORNER);
            path.LineTo(scaledCropRect.Right, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Right - CORNER, scaledCropRect.Bottom);

            path.MoveTo(scaledCropRect.Left + CORNER, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom);
            path.LineTo(scaledCropRect.Left, scaledCropRect.Bottom - CORNER);

            canvas.DrawPath(path, cornerStroke);
        }

        // Invert the transform for touch tracking
        bitmapScaleMatrix.TryInvert(out inverseBitmapMatrix);
    }
    ···
}
```

Код в `CroppingRectangle` классе основывает прямоугольник обрезки на основе размера точечного рисунка. Однако отображение растрового изображения с помощью `PhotoCropperCanvasView` класса масштабируется в зависимости от размера отображаемой области. Значение, `bitmapScaleMatrix` вычисляемое в `OnPaintSurface` картах переопределения, от пикселей растрового изображения до размера и расположения растрового изображения по мере отображения. Эта матрица затем используется для преобразования прямоугольника обрезки, чтобы его можно было отобразить относительно точечного рисунка.

Последняя строка `OnPaintSurface` переопределения принимает обратное значение `bitmapScaleMatrix` и сохраняет его в качестве `inverseBitmapMatrix` поля. Используется для сенсорной обработки.

`TouchEffect`Объект создается в виде поля, а конструктор присоединяет обработчик к `TouchAction` событию, но его необходимо `TouchEffect` Добавить в `Effects` коллекцию _родителя_ `SKCanvasView` производного класса, чтобы сделать это в `OnParentSet` переопределении:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    const int RADIUS = 100;     // pixel radius of touch hit-test
    ···
    CroppingRectangle croppingRect;
    SKMatrix inverseBitmapMatrix;

    // Touch tracking
    TouchEffect touchEffect = new TouchEffect();
    struct TouchPoint
    {
        public int CornerIndex { set; get; }
        public SKPoint Offset { set; get; }
    }

    Dictionary<long, TouchPoint> touchPoints = new Dictionary<long, TouchPoint>();
    ···
    public PhotoCropperCanvasView(SKBitmap bitmap, float? aspectRatio = null)
    {
        ···
        touchEffect.TouchAction += OnTouchEffectTouchAction;
    }
    ···
    protected override void OnParentSet()
    {
        base.OnParentSet();

        // Attach TouchEffect to parent view
        Parent.Effects.Add(touchEffect);
    }
    ···
    void OnTouchEffectTouchAction(object sender, TouchActionEventArgs args)
    {
        SKPoint pixelLocation = ConvertToPixel(args.Location);
        SKPoint bitmapLocation = inverseBitmapMatrix.MapPoint(pixelLocation);

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Convert radius to bitmap/cropping scale
                float radius = inverseBitmapMatrix.ScaleX * RADIUS;

                // Find corner that the finger is touching
                int cornerIndex = croppingRect.HitTest(bitmapLocation, radius);

                if (cornerIndex != -1 && !touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = new TouchPoint
                    {
                        CornerIndex = cornerIndex,
                        Offset = bitmapLocation - croppingRect.Corners[cornerIndex]
                    };

                    touchPoints.Add(args.Id, touchPoint);
                }
                break;

            case TouchActionType.Moved:
                if (touchPoints.ContainsKey(args.Id))
                {
                    TouchPoint touchPoint = touchPoints[args.Id];
                    croppingRect.MoveCorner(touchPoint.CornerIndex,
                                            bitmapLocation - touchPoint.Offset);
                    InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchPoints.ContainsKey(args.Id))
                {
                    touchPoints.Remove(args.Id);
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Xamarin.Forms.Point pt)
    {
        return new SKPoint((float)(CanvasSize.Width * pt.X / Width),
                           (float)(CanvasSize.Height * pt.Y / Height));
    }
}
```

События сенсорного ввода, обрабатываемые `TouchAction` обработчиком, находятся в аппаратно-независимых единицах. Сначала необходимо преобразовать их в пиксели с помощью `ConvertToPixel` метода в нижней части класса, а затем преобразовать в `CroppingRectangle` единицы с помощью `inverseBitmapMatrix` .

Для `Pressed` событий `TouchAction` обработчик вызывает `HitTest` метод `CroppingRectangle` . Если возвращается индекс, отличный от &ndash; 1, то обрабатываются один из углов прямоугольника обрезки. Этот индекс и смещение фактической точки касания от угла хранятся в `TouchPoint` объекте и добавляются в `touchPoints` словарь.

Для `Moved` события `MoveCorner` метод `CroppingRectangle` вызывается для перемещения угла с возможными корректировками пропорций.

В любой момент времени программа, использующая, `PhotoCropperCanvasView` может получить доступ к `CroppedBitmap` свойству. Это свойство использует `Rect` свойство объекта, `CroppingRectangle` чтобы создать новое растровое изображение с обрезанным размером. `DrawBitmap`После этого версия с конечными и исходными прямоугольниками извлекает подмножество исходного точечного рисунка:

```csharp
class PhotoCropperCanvasView : SKCanvasView
{
    ···
    SKBitmap bitmap;
    CroppingRectangle croppingRect;
    ···
    public SKBitmap CroppedBitmap
    {
        get
        {
            SKRect cropRect = croppingRect.Rect;
            SKBitmap croppedBitmap = new SKBitmap((int)cropRect.Width,
                                                  (int)cropRect.Height);
            SKRect dest = new SKRect(0, 0, cropRect.Width, cropRect.Height);
            SKRect source = new SKRect(cropRect.Left, cropRect.Top,
                                       cropRect.Right, cropRect.Bottom);

            using (SKCanvas canvas = new SKCanvas(croppedBitmap))
            {
                canvas.DrawBitmap(bitmap, source, dest);
            }

            return croppedBitmap;
        }
    }
    ···
}
```

## <a name="hosting-the-photo-cropper-canvas-view"></a>Размещение представления холста кадрирования фотографий

С этими двумя классами, обрабатывающими логику обрезки, страница **кадрирования фотографий** в приложении **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** имеет очень мало усилий. XAML-файл создает экземпляр `Grid` для размещения `PhotoCropperCanvasView` кнопки and Done ( **Готово** ):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoCroppingPage"
             Title="Photo Cropping">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid x:Name="canvasViewHost"
              Grid.Row="0"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="1"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

`PhotoCropperCanvasView`Невозможно создать экземпляр в файле XAML, так как для него требуется параметр типа `SKBitmap` .

Вместо этого `PhotoCropperCanvasView` экземпляр создается в конструкторе файла кода программной части с помощью одного из битовых карт ресурсов:

```csharp
public partial class PhotoCroppingPage : ContentPage
{
    PhotoCropperCanvasView photoCropper;
    SKBitmap croppedBitmap;

    public PhotoCroppingPage ()
    {
        InitializeComponent ();

        SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(GetType(),
            "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

        photoCropper = new PhotoCropperCanvasView(bitmap);
        canvasViewHost.Children.Add(photoCropper);
    }

    void OnDoneButtonClicked(object sender, EventArgs args)
    {
        croppedBitmap = photoCropper.CroppedBitmap;

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
        canvas.DrawBitmap(croppedBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

Затем пользователь может манипулировать прямоугольником обрезки:

[![Обрезка 1](cropping-images/PhotoCropping1.png "Обрезка 1")](cropping-images/PhotoCropping1-Large.png#lightbox)

Когда был определен хороший прямоугольник кадрирования, нажмите кнопку Done ( **Готово** ). `Clicked`Обработчик получает обрезанное растровое изображение из `CroppedBitmap` свойства `PhotoCropperCanvasView` и заменяет все содержимое страницы новым `SKCanvasView` объектом, который отображает обрезанный точечный рисунок:

[![Обрезка 2](cropping-images/PhotoCropping2.png "Обрезка 2")](cropping-images/PhotoCropping2-Large.png#lightbox)

Попробуйте задать для второго аргумента `PhotoCropperCanvasView` значение отображается 1,78 f (например,):

```csharp
photoCropper = new PhotoCropperCanvasView(bitmap, 1.78f);
```

Вы увидите, что прямоугольник обрезки ограничен характеристиками пропорций с глубиной 16 и 9.

## <a name="dividing-a-bitmap-into-tiles"></a>Деление растрового изображения на плитки

Версия известной Xamarin.Forms головоломки 14-15, появилось в главе 22 книги [_Создание мобильных приложений с помощью Xamarin. Forms_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) и может быть загружена как [**ксамагонксуззле**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle). Однако головоломка становится более интересной (и зачастую более сложной), если она основана на изображении из собственной фотобиблиотеки.

Эта версия головоломки 14-15 является частью приложения **[скиашарпформсдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)** и состоит из ряда страниц с названием " **головоломка в фотографии**".

Файл **PhotoPuzzlePage1. XAML** состоит из `Button` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage1"
             Title="Photo Puzzle">

    <Button Text="Pick a photo from your library"
            VerticalOptions="CenterAndExpand"
            HorizontalOptions="CenterAndExpand"
            Clicked="OnPickButtonClicked"/>

</ContentPage>
```

Файл кода программной части реализует `Clicked` обработчик, использующий `IPhotoLibrary` службу зависимостей, чтобы позволить пользователю выбрать фотографию из библиотеки фотографий:

```csharp
public partial class PhotoPuzzlePage1 : ContentPage
{
    public PhotoPuzzlePage1 ()
    {
        InitializeComponent ();
    }

    async void OnPickButtonClicked(object sender, EventArgs args)
    {
        IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
        using (Stream stream = await photoLibrary.PickPhotoAsync())
        {
            if (stream != null)
            {
                SKBitmap bitmap = SKBitmap.Decode(stream);

                await Navigation.PushAsync(new PhotoPuzzlePage2(bitmap));
            }
        }
    }
}
```

Затем метод переходит к `PhotoPuzzlePage2` , передавая в конструктор выбранного растрового изображения.

Возможно, фотография, выбранная из библиотеки, не является ориентацией, как она появилась в библиотеке фотографий, но поворачивается или перемещается вверх. (Это особенно проблема с устройствами iOS.) По этой причине `PhotoPuzzlePage2` вы можете поворачивать изображение до нужной ориентации. XAML-файл содержит три кнопки, обозначенные как **90&#x00B0; вправо** (по часовой стрелке), **90&#x00B0; влево** (против часовой стрелки) и **Готово**.

Файл кода программной части реализует логику вращения растрового изображения, показанную в статье **[Создание и рисование растровых изображений SkiaSharp](drawing.md#rotating-bitmaps)**. Пользователь может вращать изображение 90 градусов по часовой стрелке или против часовой стрелки в любое количество раз:

```csharp
public partial class PhotoPuzzlePage2 : ContentPage
{
    SKBitmap bitmap;

    public PhotoPuzzlePage2 (SKBitmap bitmap)
    {
        this.bitmap = bitmap;

        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform);
    }

    void OnRotateRightButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(bitmap.Height, 0);
            canvas.RotateDegrees(90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    void OnRotateLeftButtonClicked(object sender, EventArgs args)
    {
        SKBitmap rotatedBitmap = new SKBitmap(bitmap.Height, bitmap.Width);

        using (SKCanvas canvas = new SKCanvas(rotatedBitmap))
        {
            canvas.Clear();
            canvas.Translate(0, bitmap.Width);
            canvas.RotateDegrees(-90);
            canvas.DrawBitmap(bitmap, new SKPoint());
        }

        bitmap = rotatedBitmap;
        canvasView.InvalidateSurface();
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        await Navigation.PushAsync(new PhotoPuzzlePage3(bitmap));
    }
}
```

Когда пользователь нажимает кнопку **Готово** , `Clicked` обработчик переходит к `PhotoPuzzlePage3` , передавая окончательное повернутое растровое изображение в конструктор страницы.

`PhotoPuzzlePage3`позволяет обрезать фотографию. Программе требуется квадратный точечный рисунок для деления на сетку 4 на 4 элемента мозаики.

Файл **PhotoPuzzlePage3. XAML** содержит `Label` , `Grid` для размещения `PhotoCropperCanvasView` и еще одной кнопки **done** :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SkiaSharpFormsDemos.Bitmaps.PhotoPuzzlePage3"
             Title="Photo Puzzle">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Label Text="Crop the photo to a square"
               Grid.Row="0"
               FontSize="Large"
               HorizontalTextAlignment="Center"
               Margin="5" />

        <Grid x:Name="canvasViewHost"
              Grid.Row="1"
              BackgroundColor="Gray"
              Padding="5" />

        <Button Text="Done"
                Grid.Row="2"
                HorizontalOptions="Center"
                Margin="5"
                Clicked="OnDoneButtonClicked" />
    </Grid>
</ContentPage>
```

Файл кода программной части создает экземпляр `PhotoCropperCanvasView` с растровым изображением, переданным в конструктор. Обратите внимание, что в качестве второго аргумента передается значение 1 `PhotoCropperCanvasView` . Этот пропорции 1 принуждает прямоугольник обрезки быть квадратом:

```csharp
public partial class PhotoPuzzlePage3 : ContentPage
{
    PhotoCropperCanvasView photoCropper;

    public PhotoPuzzlePage3(SKBitmap bitmap)
    {
        InitializeComponent ();

        photoCropper = new PhotoCropperCanvasView(bitmap, 1f);
        canvasViewHost.Children.Add(photoCropper);
    }

    async void OnDoneButtonClicked(object sender, EventArgs args)
    {
        SKBitmap croppedBitmap = photoCropper.CroppedBitmap;
        int width = croppedBitmap.Width / 4;
        int height = croppedBitmap.Height / 4;

        ImageSource[] imgSources = new ImageSource[15];

        for (int row = 0; row < 4; row++)
        {
            for (int col = 0; col < 4; col++)
            {
                // Skip the last one!
                if (row == 3 && col == 3)
                    break;

                // Create a bitmap 1/4 the width and height of the original
                SKBitmap bitmap = new SKBitmap(width, height);
                SKRect dest = new SKRect(0, 0, width, height);
                SKRect source = new SKRect(col * width, row * height, (col + 1) * width, (row + 1) * height);

                // Copy 1/16 of the original into that bitmap
                using (SKCanvas canvas = new SKCanvas(bitmap))
                {
                    canvas.DrawBitmap(croppedBitmap, source, dest);
                }

                imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
            }
        }

        await Navigation.PushAsync(new PhotoPuzzlePage4(imgSources));
    }
}
```

Обработчик кнопки **done** получает ширину и высоту обрезанного растрового изображения (эти два значения должны быть одинаковыми), а затем делит их на 15 отдельных точечных рисунков, каждый из которых равен 1/4 ширине и высоте оригинала. (Последняя из возможных 16 растровых изображений не создается.) `DrawBitmap`Метод с исходным и конечным прямоугольником позволяет создать точечный рисунок на основе подмножества большего точечного рисунка.

## <a name="converting-to-xamarinforms-bitmaps"></a>Преобразование в Xamarin.Forms точечные рисунки

В `OnDoneButtonClicked` методе массив, созданный для 15 битовых карт, имеет тип [`ImageSource`](xref:Xamarin.Forms.ImageSource) :

```csharp
ImageSource[] imgSources = new ImageSource[15];
```

`ImageSource`Xamarin.Formsбазовый тип, инкапсулирующий точечный рисунок. К счастью, SkiaSharp позволяет преобразовывать точечные рисунки SkiaSharp в Xamarin.Forms точечные рисунки. Сборка **SkiaSharp. views. Forms** определяет [`SKBitmapImageSource`](xref:SkiaSharp.Views.Forms.SKBitmapImageSource) класс, производный от, `ImageSource` но может быть создан на основе `SKBitmap` объекта SkiaSharp. `SKBitmapImageSource`даже определяет преобразования между `SKBitmapImageSource` и `SKBitmap` , и именно то, как `SKBitmap` объекты хранятся в виде Xamarin.Forms точечных рисунков:

```csharp
imgSources[4 * row + col] = (SKBitmapImageSource)bitmap;
```

Этот массив точечных рисунков передается в качестве конструктора в `PhotoPuzzlePage4` . Эта страница полностью Xamarin.Forms и не использует никаких SkiaSharp. Он очень похож на [**ксамагонксуззле**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle), поэтому он не описан здесь, но он отображает выбранную фотографию, разделенную на 15 квадратных плиток:

[![Головоломка с фотографией 1](cropping-images/PhotoPuzzle1.png "Головоломка с фотографией 1")](cropping-images/PhotoPuzzle1-Large.png#lightbox)

Нажатие кнопки « **случайное** » прикладывает ко всем плиткам:

[![Головоломка для фото 2](cropping-images/PhotoPuzzle2.png "Головоломка для фото 2")](cropping-images/PhotoPuzzle2-Large.png#lightbox)

Теперь их можно вернуть в правильном порядке. Все плитки в той же строке или столбце, что и пустой квадрат, можно коснуться, чтобы переместить их в пустой квадрат.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
