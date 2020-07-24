---
title: Рисование простого круга в SkiaSharp
description: В этой статье объясняются основы рисования SkiaSharp, включая холсты и объекты рисования, в Xamarin.Forms приложениях, а также демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 785532d1a8fedfaef367c8fb8ae437220c3de9c4
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938181"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>Рисование простого круга в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Изучите основы рисования SkiaSharp, включая холсты и объекты Paint_

В этой статье рассматриваются основные понятия рисования графики в Xamarin.Forms с помощью SkiaSharp, включая создание `SKCanvasView` объекта для размещения графики, обработку `PaintSurface` события и использование `SKPaint` объекта для указания цвета и других атрибутов рисования.

Программа [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) содержит весь пример кода для этой серии статей SkiaSharp. Первая страница имеет право на **простой круг** и вызывает класс страницы [`SimpleCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) . В этом коде показано, как нарисовать окружность в центре страницы с радиусом 100 пикселей. Контур окружности красного цвета, а внутренняя часть окружности — синим.

![Синий круг, выделенный красным цветом](circle-images/circleexample.png)

[`SimpleCircle`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs)Класс Page является производным от `ContentPage` и содержит две `using` директивы для пространств имен SkiaSharp:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Следующий конструктор класса создает [`SKCanvasView`](xref:SkiaSharp.Views.Forms.SKCanvasView) объект, присоединяет обработчик для [`PaintSurface`](xref:SkiaSharp.Views.Forms.SKCanvasView.PaintSurface) события и задает `SKCanvasView` объект в качестве содержимого страницы:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

Объект `SKCanvasView` занимает всю область содержимого страницы. Можно также объединить `SKCanvasView` с другими Xamarin.Forms `View` производными, как показано в других примерах.

`PaintSurface`Обработчик событий выполняет все действия по рисованию. Этот метод можно вызывать несколько раз во время выполнения программы, поэтому он должен поддерживать всю информацию, необходимую для воссоздания графики.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[`SKPaintSurfaceEventArgs`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs)Объект, сопровождающий событие, имеет два свойства:

- [`Info`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info) типа [`SKImageInfo`](xref:SkiaSharp.SKImageInfo).
- [`Surface`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface) типа [`SKSurface`](xref:SkiaSharp.SKSurface).

`SKImageInfo`Структура содержит сведения о поверхности рисования, что самое важное, ширину и высоту в пикселях. `SKSurface`Объект представляет саму поверхность рисования. В этой программе поверхность рисования является видеоизображением, но в других программах `SKSurface` объект может также представлять точечный рисунок, который используется SkiaSharp для рисования.

Наиболее важным свойством для `SKSurface` является [`Canvas`](xref:SkiaSharp.SKSurface.Canvas) тип [`SKCanvas`](xref:SkiaSharp.SKCanvas) . Этот класс является контекстом графического рисования, который используется для выполнения фактического рисования. `SKCanvas`Объект инкапсулирует состояние графики, которое включает преобразования графики и обрезание.

Ниже приведен типичный запуск `PaintSurface` обработчика событий.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

[`Clear`](xref:SkiaSharp.SKCanvas.Clear)Метод очищает холст с прозрачным цветом. Перегрузка позволяет указать цвет фона для холста.

Цель этой задачи — нарисовать красный круг, заполненный синим. Так как это конкретное графическое изображение содержит два разных цвета, задание должно выполняться в два этапа. Первым шагом является рисование контура окружности. Чтобы указать цвет и другие характеристики линии, создайте и инициализируйте [`SKPaint`](xref:SkiaSharp.SKPaint) объект:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

[`Style`](xref:SkiaSharp.SKPaint.Style)Свойство указывает, что требуется *Обводка* линии (в данном случае контура окружности), а не *заливки* внутренней. [`SKPaintStyle`](xref:SkiaSharp.SKPaintStyle)Ниже приведены три члена перечисления.

- [`Fill`](xref:SkiaSharp.SKPaintStyle.Fill)
- [`Stroke`](xref:SkiaSharp.SKPaintStyle.Stroke)
- [`StrokeAndFill`](xref:SkiaSharp.SKPaintStyle.StrokeAndFill)

Значение по умолчанию — `Fill`. Используйте третий параметр для обводки линии и заполнения внутренней области тем же цветом.

Присвойте [`Color`](xref:SkiaSharp.SKPaint.Color) свойству значение типа [`SKColor`](xref:SkiaSharp.SKColor) . Одним из способов получения `SKColor` значения является преобразование Xamarin.Forms `Color` значения в `SKColor` значение с помощью метода расширения [`ToSKColor`](xref:SkiaSharp.Views.Forms.Extensions.ToSKColor*) . [`Extensions`](xref:SkiaSharp.Views.Forms.Extensions)Класс в `SkiaSharp.Views.Forms` пространстве имен включает другие методы, которые преобразуют Xamarin.Forms значения и SkiaSharp значения.

[`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth)Свойство указывает толщину линии. Здесь устанавливается значение 25 пикселей.

Этот объект используется `SKPaint` для рисования окружности:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Координаты задаются относительно левого верхнего угла отображаемой поверхности. Координаты X увеличиваются вправо, а координаты Y увеличиваются. В обсуждении о графике часто используется математическая нотация (x, y) для обозначения точки. Точка (0, 0) является верхним левым углом отображаемой поверхности и часто называется *источником*.

Первые два аргумента `DrawCircle` указывают координаты X и Y центра окружности. Они назначаются в половину ширины и высоты поверхности отображения, чтобы разместить центр окружности в центре поверхности отображения. Третий аргумент задает радиус окружности, а последний аргумент — `SKPaint` объект.

Чтобы заполнить внутреннюю часть круга, можно изменить два свойства `SKPaint` объекта и вызвать метод `DrawCircle` еще раз. Этот код также показывает альтернативный способ получения `SKColor` значения из одного из многих полей [`SKColors`](xref:SkiaSharp.SKColors) структуры:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```

На этот раз `DrawCircle` вызов заполняет круг с помощью новых свойств `SKPaint` объекта.

Вот программа, выполняемая в iOS и Android:

[![Тройной снимок экрана простой круговой страницы](circle-images/simplecircle-small.png)](circle-images/simplecircle-large.png#lightbox "Тройной снимок экрана простой круговой страницы")

При самостоятельном запуске программы можно превратить телефон или симулятор в сторону, чтобы увидеть, как изображение перерисовывается. Каждый раз, когда необходимо перерисовать график, `PaintSurface` обработчик событий вызывается снова.

Также можно окрасить графические объекты с помощью градиентов или растровых элементов. Эти параметры обсуждаются в разделе [**SkiaSharp шейдеры**](../effects/shaders/index.md).

`SKPaint`Объект немного больше, чем коллекция свойств графического рисунка. Эти объекты являются легковесными. Можно повторно использовать `SKPaint` объекты, как это делает программа, или можно создать несколько `SKPaint` объектов для различных сочетаний свойств рисования. Вы можете создавать и инициализировать эти объекты за пределами `PaintSurface` обработчика событий, а также сохранять их в качестве полей в классе страницы.

> [!NOTE]
> `SKPaint`Класс определяет, [`IsAntialias`](xref:SkiaSharp.SKPaint.IsAntialias) чтобы включить сглаживание в отрисовке графики. Сглаживание обычно приводит к визуально более гладким краям, поэтому вам, вероятно, потребуется задать это свойство `true` в большинстве `SKPaint` объектов. В целях простоты это свойство _не_ задано в большинстве примеров страниц.

Несмотря на то, что ширина контура окружности равна 25 пикселям &mdash; или одному кварталу радиуса круга &mdash; , она выглядит более тонкой, и есть веская причина: половина ширины линии скрыта синим кружком. Аргументы `DrawCircle` метода определяют абстрактные геометрические координаты окружности. Размер синей внутренней части изменяется до ближайшего пикселя, но 25-пиксельный контур охватывает геометрический круг &mdash; половины внутри и половины снаружи.

Следующий пример в разделе [Интеграция с Xamarin.Forms ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) этой статьей демонстрирует визуальную визуализацию.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
