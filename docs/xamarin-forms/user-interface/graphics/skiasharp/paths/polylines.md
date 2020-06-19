---
title: Ломаные линии и параметрические уравнения
description: В этой статье объясняется, как использовать SkiaSharp для подготовки к просмотру любой строки, которую можно определить с помощью параметрической формулы, и демонстрируется в примере кода.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b435e99180791b64e0a8ad975527fb3cb5316b7d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140222"
---
# <a name="polylines-and-parametric-equations"></a>Ломаные линии и параметрические уравнения

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Использование SkiaSharp для подготовки к просмотру любой строки, которую можно определить с помощью параметрической формулы_

В разделе « [**кривые и пути SkiaSharp**](../curves/index.md) » этого раздела вы увидите различные методы, [`SKPath`](xref:SkiaSharp.SKPath) определяющие отрисовку определенных типов кривых. Однако иногда необходимо нарисовать тип кривой, который не поддерживается напрямую `SKPath` . В этом случае можно использовать ломаную линию (коллекцию соединенных линий), чтобы нарисовать любую кривую, которую можно определить математически. Если сделать строки достаточно мелкими и достаточно большими, результат будет выглядеть как кривая. Эта спираль на самом деле 3 600 небольшие линии:

![](polylines-images/spiralexample.png "A spiral")

Как правило, лучше определить кривую с точки зрения пары параметрической уравнений. Это уравнения для координат X и Y, которые зависят от третьей переменной, которая иногда называется `t` для времени. Например, следующие параметрической уравнения определяют круг с радиусом 1 по центру в точке (0, 0) для *t* с 0 до 1:

`x = cos(2πt)`

`y = sin(2πt)`

 Если вы хотите, чтобы радиус больше 1, можно просто умножить значения синуса и косинуса на радиус, а если необходимо переместить центр в другое расположение, добавьте следующие значения:

`x = xCenter + radius·cos(2πt)`

`y = yCenter + radius·sin(2πt)`

Для эллипса с осями, параллельными горизонтальным и вертикальным, используются два радиуса:

`x = xCenter + xRadius·cos(2πt)`

`y = yCenter + yRadius·sin(2πt)`

Затем можно вставить эквивалентный код SkiaSharp в цикл, который вычисляет различные точки и добавляет их к контуру. Следующий код SkiaSharp создает `SKPath` объект для эллипса, который заполняет поверхность отображения. Цикл проходит через 360 градусов напрямую. Центр — это половина ширины и высоты поверхности отображения, поэтому это два радиуса:

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

Это приводит к эллипсу, определяемому 360 маленькими линиями. При подготовке к просмотру он выглядит гладким.

Конечно, вам не нужно создавать эллипс с помощью ломаной линии, так как `SKPath` включает `AddOval` метод, который делает это за вас. Но может потребоваться нарисовать визуальный объект, не предоставляемый `SKPath` .

На странице **Арчимедеан спирали** есть код, аналогичный коду эллипса, но с самым важным различием. Он циклически проходит через 360 градусов круга в 10 раз, постоянно настраивая радиус:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

Результат также называется *арифметической спираль* , так как смещение между циклами является константой:

[![](polylines-images/archimedeanspiral-small.png "Triple screenshot of the Archimedean Spiral page")](polylines-images/archimedeanspiral-large.png#lightbox "Triple screenshot of the Archimedean Spiral page")

Обратите внимание, что объект `SKPath` создается в `using` блоке. Это `SKPath` потребляет больше памяти `SKPath` , чем объекты в предыдущих программах, что предполагает, что `using` блок более подходит для удаления любых неуправляемых ресурсов.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
