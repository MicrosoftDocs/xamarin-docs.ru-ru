---
title: ''
description: В этой статье объясняется, как использовать пальцы для рисования на холсте SkiaSharp в Xamarin.Forms приложении и демонстрируется пример кода.
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 61ae651a2402204f69f642235d74d8d641b47988
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139026"
---
# <a name="finger-painting-in-skiasharp"></a>Рисование пальцами в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Используйте пальцы для рисования на холсте._

`SKPath`Объект может постоянно обновляться и отображаться. Эта функция позволяет использовать путь для интерактивного рисования, например в программе рисования пальца.

![](finger-paint-images/fingerpaintsample.png "An exercise in finger painting")

Поддержка сенсорного ввода в Xamarin.Forms не позволяет отслеживать отдельные пальцы на экране, поэтому Xamarin.Forms для обеспечения дополнительной поддержки сенсорного ввода был разработан такой режим. Этот эффект описан в статье [**вызов событий из эффектов**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Демонстрационные видеоролики программы с примерами с [**поддержкой сенсорного экрана**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/) включают две страницы, использующие SkiaSharp, в том числе программу рисования пальца.

Решение [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) включает в себя это событие отслеживания касаний. Проект библиотеки .NET Standard включает `TouchEffect` класс, `TouchActionType` перечисление, `TouchActionEventHandler` делегат и `TouchActionEventArgs` класс. Каждый из проектов платформы включает `TouchEffect` класс для этой платформы. Проект iOS также содержит `TouchRecognizer` класс.

Страница **прорисовки пальца** в **скиашарпформсдемос** — это упрощенная реализация рисования пальца. Он не позволяет выбрать цвет или толщину штриха, он не может очистить холст. Конечно, вы не можете сохранить иллюстрацию.

Файл [**финжерпаинтпаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml) помещает `SKCanvasView` в одну ячейку `Grid` и присоединяет `TouchEffect` к нему `Grid` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

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

Присоединение `TouchEffect` непосредственно к элементу `SKCanvasView` не работает на всех платформах.

Файл кода программной части [**FingerPaintPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/FingerPaintPage.xaml.cs) определяет две коллекции для хранения `SKPath` объектов, а также `SKPaint` объект для визуализации этих путей:

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

Как видно из названия, в `inProgressPaths` словаре хранятся пути, которые в данный момент рисуются одним или несколькими пальцами. Ключ словаря — это сенсорный идентификатор, сопровождающий события касания. `completedPaths`Поле представляет собой коллекцию путей, которые были завершены, когда палец, рисующий контур, ликвидируется с экрана.

`TouchAction`Обработчик управляет этими двумя коллекциями. Когда палец впервые касается экрана, к нему `SKPath` добавляется новый `inProgressPaths` . При перемещении пальца к пути добавляются дополнительные точки. При отпускании пальца путь передается в `completedPaths` коллекцию. Можно рисовать одновременно несколькими пальцами. После каждого изменения в одном из путей или коллекций объект `SKCanvasView` становится недействительным.

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

Точки, сопровождающие события сенсорного отслеживания, — это Xamarin.Forms координаты, которые должны быть преобразованы в SkiaSharp координаты, которые являются пикселями. Это цель `ConvertToPixel` метода.

`PaintSurface`Затем обработчик просто визуализирует обе коллекции путей. Ранее завершенные пути отображаются под выполняемыми путями:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

Ваш палец картин только по вашему автору:

[![](finger-paint-images/fingerpaint-small.png "Triple screenshot of the Finger Paint page")](finger-paint-images/fingerpaint-large.png#lightbox "Triple screenshot of the Finger Paint page")

Теперь вы узнали, как рисовать линии и определять кривые с помощью параметрической уравнения. В следующем разделе [**SkiaSharp кривых и paths**](../curves/index.md) рассматриваются различные типы кривых, которые `SKPath` поддерживаются. Но полезным условием является исследование [**преобразований SkiaSharp**](../transforms/index.md).

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Демонстрации эффектов для отслеживания касаний (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-touchtrackingeffect/)
- [Вызов событий из эффекта](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
