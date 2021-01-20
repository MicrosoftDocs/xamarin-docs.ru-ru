---
title: Пиксели и аппаратно-независимые единицы
description: В этой статье рассматриваются различия между координатами и Xamarin.Forms координатами SkiaSharp и демонстрируется пример кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: davidbritch
ms.author: dabritch
ms.date: 02/09/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 04f0a056b83c3ebb298e284fefc93d83dfc57b52
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98609005"
---
# <a name="pixels-and-device-independent-units"></a>Пиксели и аппаратно-независимые единицы

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Изучите различия между координатами и Xamarin.Forms координатами SkiaSharp_

В этой статье рассматриваются различия в системе координат, используемой в SkiaSharp и Xamarin.Forms . Вы можете получить информацию для преобразования между двумя системами координат, а также рисовать графику, заполняющую определенную область:

![Овал, который заполняет экран](pixels-images/screenfillexample.png)

Если вы еще не работали с программированием Xamarin.Forms , вы можете иметь представление о Xamarin.Forms координатах и размерах. Круги, вырисованные в двух предыдущих статьях, могут показаться немного маленькими.

Эти кружки *являются* небольшими по сравнению с Xamarin.Forms размерами. По умолчанию SkiaSharp рисует в единицах измерения, в то время как Xamarin.Forms базовые координаты и размеры в единицах, независимых от устройства, устанавливаются базовой платформой. (Дополнительные сведения о Xamarin.Forms системе координат можно найти в [главе 5. Работа с размерами](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) книги *Создание мобильных приложений с помощью Xamarin.Forms*.)

Страница в программе [**скевшарпформсдемос**](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) с **соответствующим размером поверхности** использует SkiaSharp Text OUTPUT для отображения размера отображаемой поверхности из трех различных источников:

- Обычные Xamarin.Forms [`Width`](xref:Xamarin.Forms.VisualElement.Width) Свойства и [`Height`](xref:Xamarin.Forms.VisualElement.Height) `SKCanvasView` объекта.
- [`CanvasSize`](xref:SkiaSharp.Views.Forms.SKCanvasView.CanvasSize)Свойство `SKCanvasView` объекта.
- [`Size`](xref:SkiaSharp.SKImageInfo.Size)Свойство `SKImageInfo` значения, которое согласуется со `Width` свойствами и, `Height` используемыми в двух предыдущих страницах.

[`SurfaceSizePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs)Класс показывает, как отобразить эти значения. Конструктор сохраняет `SKCanvasView` объект как поле, поэтому к нему можно получить доступ в `PaintSurface` обработчике событий:

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas` включает шесть различных `DrawText` методов, но этот [`DrawText`](xref:SkiaSharp.SKCanvas.DrawText(System.String,System.Single,System.Single,SkiaSharp.SKPaint)) метод является простейшим:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Вы указываете текстовую строку, координаты X и Y, где начинается текст, и `SKPaint` объект. Координата X указывает, где располагается левая часть текста, но следует просмотреть: координата Y задает положение *базовой линии* текста. Если вы когда-нибудь записали руки в бумажном документе, то базовая линия представляет собой линию, в которой располагаются символы, и, по убыванию, буквы g, p, q и y.

`SKPaint`Объект позволяет указать цвет текста, семейство шрифтов и размер текста. По умолчанию [`TextSize`](xref:SkiaSharp.SKPaint.TextSize) свойство имеет значение 12, что приводит к небольшому тексту на устройствах с высоким разрешением, таких как телефоны. В любом случае, кроме простейших приложений, вам также потребуется информация о размере отображаемого текста. `SKPaint`Класс определяет [`FontMetrics`](xref:SkiaSharp.SKPaint.FontMetrics) свойство и несколько [`MeasureText`](xref:SkiaSharp.SKPaint.MeasureText(System.String)) методов, но для менее отдельных потребностей [`FontSpacing`](xref:SkiaSharp.SKPaint.FontSpacing) свойство предоставляет рекомендуемое значение для пробельных строк текста.

Следующий `PaintSurface` обработчик создает `SKPaint` объект для `TextSize` 40 пикселей, который представляет нужную вертикальную высоту текста от верхнего края до нижней части нижних. `FontSpacing`Значение, которое `SKPaint` возвращает объект, немного больше, чем 47 пикселей.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

Метод начинает первую строку текста с координатой X 20 (для небольшого поля слева) и координатой Y `fontSpacing` , которая немного больше, чем необходимо для отображения полной высоты первой строки текста в верхней части поверхности отображения. После каждого вызова `DrawText` координата Y увеличивается на один или два шага `fontSpacing` .

Вот работающая программа:

[![Снимки экрана показывают приложение размера поверхности, работающее на двух мобильных устройствах.](pixels-images/surfacesize-small.png)](pixels-images/surfacesize-large.png#lightbox "Тройной снимок экрана страницы размера поверхности")

Как видите, `CanvasSize` свойство объекта `SKCanvasView` и `Size` свойство `SKImageInfo` значения согласуются в отчетах об измерениях в пикселях. `Height`Свойства и `Width` класса `SKCanvasView` являются свойствами и Xamarin.Forms сообщают размер представления в аппаратно-независимых единицах, определенных платформой.

Имитатор iOS семь в левой части имеет два пикселя на единицу, не зависящее от устройства, а в расположении Android 5 в центре есть три пикселя на единицу. Вот почему у простого кружка, показанного выше, разные размеры на разных платформах.

Если вы предпочитаете работать полностью в аппаратно-независимых единицах, это можно сделать, задав `IgnorePixelScaling` для свойства объекта значение `SKCanvasView` `true` . Однако результаты могут быть непохожими. SkiaSharp визуализирует графику на поверхности устройства меньшего размера с размером в пикселях, равным размеру представления в единицах, не зависящих от устройства. (Например, SkiaSharp будет использовать поверхность отображения 360 x 512 пикселей для хранилища 5.) Затем размер изображения увеличивается в размере, что приводит к заметному жаггиесу битовой карты.

Чтобы обеспечить одинаковое разрешение изображения, лучше написать собственные простые функции для преобразования между двумя системами координат.

Помимо `DrawCircle` метода, `SKCanvas` также определяет два `DrawOval` метода, которые рисуют эллипс. Эллипс определяется двумя радиусами, а не одним RADIUS-сервером. Они называются *основным радиусом* и *дополнительным радиусом*. `DrawOval`Метод рисует эллипс с двумя радиусами параллельно с осями X и Y. (Если необходимо нарисовать эллипс с осями, не параллельными с осями X и Y, можно использовать преобразование поворота, как описано в статье [**Преобразование вращения**](../transforms/rotate.md) или графический путь, как описано в статье [**три способа рисования дуги**](../curves/arcs.md)). Эта перегрузка [`DrawOval`](xref:SkiaSharp.SKCanvas.DrawOval(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) метода называет два параметра радиуса `rx` и `ry` указывает, что они являются параллельными по осям X и Y:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Можно ли нарисовать эллипс, который заполняет поверхность отображения? На странице **заливка эллипса** показано, как это делать. `PaintSurface`Обработчик событий в классе [**EllipseFillPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) вычитает половину толщины штриха из `xRadius` `yRadius` значений и для подбора всего эллипса и его контура в области отображения:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

Здесь он работает:

[![Снимки экрана показывают приложение для заливки эллипса, работающее на двух мобильных устройствах.](pixels-images/ellipsefill-small.png)](pixels-images/ellipsefill-large.png#lightbox "Тройной снимок экрана страницы размера поверхности")

Другой [`DrawOval`](xref:SkiaSharp.SKCanvas.DrawOval(SkiaSharp.SKRect,SkiaSharp.SKPaint)) метод имеет [`SKRect`](xref:SkiaSharp.SKRect) аргумент, который является прямоугольником, определяемым координатами X и Y верхнего левого угла и правого нижнего угла. Овал заполняет этот прямоугольник, который предполагает, что его можно использовать на странице **заливка эллипса** следующим образом:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Однако это усекает все края контура эллипса на четырех сторонах. Необходимо настроить все `SKRect` аргументы конструктора на основе, `strokeWidth` чтобы сделать это правильно:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)