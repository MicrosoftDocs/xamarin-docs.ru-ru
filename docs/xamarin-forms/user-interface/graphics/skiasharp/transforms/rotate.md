---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 520c4c3b61049bf17c2c964523714db196da6839
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84132188"
---
# <a name="the-rotate-transform"></a>Преобразование циклического сдвига

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Изучите эффекты и анимации, доступные с помощью преобразования «SkiaSharp вращение»_

При использовании графических объектов SkiaSharp графические объекты не имеют ограничений на выравнивание по горизонтальной и вертикальной осям:

![](rotate-images/rotateexample.png "Text rotated around a center")

Для поворота графического объекта вокруг точки (0, 0) SkiaSharp поддерживает как [`RotateDegrees`](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single)) метод, так и [`RotateRadians`](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single)) метод:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Окружность в 360 градусов аналогична твоπ радианам, поэтому можно легко выполнить преобразование между двумя единицами. Используйте любой из удобных. Все тригонометрические функции в [`Math`](xref:System.Math) классе .NET используют единицы в радианах.

Поворот по часовой стрелке для увеличения углов. (Хотя поворот в декартовой системе координат — это по часовой стрелке в соответствии с соглашением, поворот по часовой стрелке согласован с координатами Y, увеличивающимися в SkiaSharp.) Отрицательные углы и углы, превышающие 360 градусов, разрешены.

Формулы преобразования для вращения сложнее, чем для преобразования и масштабирования. Для угла α формулы преобразования имеют следующие преимущества:

x "= x • cos (α) — y • Sin (α)   

y "= x • Sin (α) + y • cos (α)

На странице **Простая поворот** демонстрируется `RotateDegrees` метод. В файле [**BasicRotate.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) отображается текст со своей базовой линией на странице и он поворачивается на основе a `Slider` с диапазоном от – 360 до 360. Ниже приведена соответствующая часть `PaintSurface` обработчика:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Поскольку поворот выравнивается по левому верхнему углу холста, для большинства углов, заданных в этой программе, текст поворачивается за пределы экрана:

[![](rotate-images/basicrotate-small.png "Triple screenshot of the Basic Rotate page")](rotate-images/basicrotate-large.png#lightbox "Triple screenshot of the Basic Rotate page")

Очень часто требуется поворачивать что-то по центру вокруг указанной точки вращения с помощью следующих версий [`RotateDegrees`](xref:SkiaSharp.SKCanvas.RotateDegrees(System.Single,System.Single,System.Single)) [`RotateRadians`](xref:SkiaSharp.SKCanvas.RotateRadians(System.Single,System.Single,System.Single)) методов и:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**Центральная страница вращения** аналогична **базовой** , за исключением того, что развернутая версия используется `RotateDegrees` для установки центра вращения на ту же точку, которая используется для размещения текста:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Теперь текст поворачивается вокруг точки, используемой для размещения текста, который является горизонтальным центром базового плана текста:

[![](rotate-images/centeredrotate-small.png "Triple screenshot of the Centered Rotate page")](rotate-images/centeredrotate-large.png#lightbox "Triple screenshot of the Centered Rotate page")

Как и в случае с Центральной версией `Scale` метода, Центральная версия `RotateDegrees` вызова является ярлыком. Ниже приведен метод.

```csharp
RotateDegrees (degrees, px, py);
```

Этот вызов эквивалентен следующему:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Вы обнаружите, что иногда можно объединять `Translate` вызовы с помощью `Rotate` вызовов. Например, ниже приведены `RotateDegrees` `DrawText` вызовы и в **Центральной странице вращения** .

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees`Вызов эквивалентен двум `Translate` вызовам и не выровнен по центру `RotateDegrees` :

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText`Вызов для вывода текста в определенном месте эквивалентен `Translate` вызову для этого расположения, за которым следует `DrawText` точка (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Два последовательных `Translate` вызова отменяют друг друга:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

По сути, два преобразования применяются в порядке, противоположном тому, как они отображаются в коде. `DrawText`Вызов отображает текст в левом верхнем углу холста. `RotateDegrees`Вызов поворачивает этот текст относительно левого верхнего угла. Затем `Translate` вызов перемещает текст в центр холста.

Обычно существует несколько способов объединения вращения и перевода. **Повернутая текстовая** страница создает следующее отображение:

[![](rotate-images/rotatedtext-small.png "Triple screenshot of the Rotated Text page")](rotate-images/rotatedtext-large.png#lightbox "Triple screenshot of the Rotated Text page")

Вот `PaintSurface` обработчик [`RotatedTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) класса:

```csharp
static readonly string text = "    ROTATE";
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 72
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float yText = yCenter - textBounds.Height / 2 - textBounds.Top;

        for (int degrees = 0; degrees < 360; degrees += 30)
        {
            canvas.Save();
            canvas.RotateDegrees(degrees, xCenter, yCenter);
            canvas.DrawText(text, xCenter, yText, textPaint);
            canvas.Restore();
        }
    }
}

```

`xCenter`Значения и `yCenter` указывают центр холста. `yText`Значение является небольшим смещением. Это значение является координатой по оси Y, необходимой для размещения текста таким образом, чтобы он действительно был по вертикали выровнен по центру страницы. `for`Затем в цикле устанавливается поворот на основе центра холста. Поворот выполняется с шагом в 30 градусов. Текст отображается с использованием `yText` значения. Число пробелов перед словом «ВРАЩЕНИЕ» в `text` значении было определено, чтобы сделать так, чтобы соединение между этими 12 текстовыми строками было Додекагон.

Одним из способов упрощения этого кода является увеличение угла вращения на 30 градусов каждый раз через цикл после `DrawText` вызова. Это устраняет необходимость в вызовах `Save` и `Restore` . Обратите внимание, что `degrees` переменная больше не используется в теле `for` блока:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Можно также использовать простую форму `RotateDegrees` , предмещая цикл с вызовом `Translate` для перемещения всех элементов в центр холста:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Измененное `yText` Вычисление больше не включается `yCenter` . Теперь `DrawText` центр обработки вызовов выравнивает текст по вертикали в верхней части холста.

Поскольку преобразования концептуально применяются в отличие от того, как они отображаются в коде, часто можно начать с более глобальных преобразований, за которыми следуют дополнительные локальные преобразования. Часто это самый простой способ объединения вращения и перевода.

Например, предположим, что необходимо нарисовать графический объект, повернутый вокруг его центра, примерно так же, как планета вращается на своей оси. Но необходимо также, чтобы этот объект был связан с центром экрана примерно так же, как планета, включающий солнце.

Это можно сделать, разместив объект в левом верхнем углу холста, а затем используя анимацию, чтобы повернуть его вокруг этого угла. Затем переведите объект по горизонтали, например Орбитал RADIUS. Теперь примените второй анимированный поворот, который также находится вокруг источника. В результате объект подается вокруг угла. Теперь преобразовывать в центр холста.

Вот `PaintSurface` обработчик, который содержит эти вызовы преобразования в обратный порядок:

```csharp
float revolveDegrees, rotateDegrees;
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Red
    })
    {
        // Translate to center of canvas
        canvas.Translate(info.Width / 2, info.Height / 2);

        // Rotate around center of canvas
        canvas.RotateDegrees(revolveDegrees);

        // Translate horizontally
        float radius = Math.Min(info.Width, info.Height) / 3;
        canvas.Translate(radius, 0);

        // Rotate around center of object
        canvas.RotateDegrees(rotateDegrees);

        // Draw a square
        canvas.DrawRect(new SKRect(-50, -50, 50, 50), fillPaint);
    }
}
```

`revolveDegrees`Поля и `rotateDegrees` являются анимированными. Эта программа использует другой метод анимации на основе Xamarin.Forms [`Animation`](xref:Xamarin.Forms.Animation) класса. (Этот класс описан в [главе 22 о *создании мобильных приложений с помощью Xamarin.Forms * ](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) `OnAppearing` . переопределение создает два `Animation` объекта с методами обратного вызова и затем вызывает `Commit` их для длительности анимации:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    new Animation((value) => revolveDegrees = 360 * (float)value).
        Commit(this, "revolveAnimation", length: 10000, repeat: () => true);

    new Animation((value) =>
    {
        rotateDegrees = 360 * (float)value;
        canvasView.InvalidateSurface();
    }).Commit(this, "rotateAnimation", length: 1000, repeat: () => true);
}
```

Первый `Animation` объект анимируется `revolveDegrees` от 0 до 360 градусов в течение 10 секунд. Второй объект анимируется `rotateDegrees` от 0 градусов до 360 градусов каждые 1 секунду, а также делает недействительной поверхность для создания другого вызова `PaintSurface` обработчика. `OnDisappearing`Переопределение отменяет эти две анимации:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**Неприятная аналоговая программа часов** (так как в более поздних статьях будут описаны более привлекательные аналоговые часы) использует вращение для отображения минут и часов часов и для поворота стрелок. Программа рисует часы с помощью произвольной системы координат на основе круга, центрированного в точке (0, 0) с радиусом 100. Он использует преобразование и масштабирование для расширения и центрирования круга на странице.

`Translate`Вызовы и `Scale` применяются глобально к часам, поэтому они являются первыми методами, которые должны вызываться после инициализации `SKPaint` объектов:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    using (SKPaint fillPaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.Color = SKColors.Black;
        strokePaint.StrokeCap = SKStrokeCap.Round;

        fillPaint.Style = SKPaintStyle.Fill;
        fillPaint.Color = SKColors.Gray;

        // Transform for 100-radius circle centered at origin
        canvas.Translate(info.Width / 2f, info.Height / 2f);
        canvas.Scale(Math.Min(info.Width / 200f, info.Height / 200f));
        ...
    }
}
```

Имеется 60 меток двух разных размеров, которые должны быть изображены вокруг часов. `DrawCircle`Вызов рисует окружность в точке (0, – 90), которая относительно центра часов соответствует 12:00. `RotateDegrees`Вызов увеличивает угол вращения на 6 градусов после каждого деления. `angle`Переменная используется только для определения того, рисуется большой круг или маленький круг:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        // Hour and minute marks
        for (int angle = 0; angle < 360; angle += 6)
        {
            canvas.DrawCircle(0, -90, angle % 30 == 0 ? 4 : 2, fillPaint);
            canvas.RotateDegrees(6);
        }
    ...
    }
}
```

Наконец, `PaintSurface` обработчик получает текущее время и вычисляет угол поворота для часа, минуты и секунды. Каждая рука рисуется в положении 12:00, чтобы угол вращения был относительным:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        DateTime dateTime = DateTime.Now;

        // Hour hand
        strokePaint.StrokeWidth = 20;
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawLine(0, 0, 0, -50, strokePaint);
        canvas.Restore();

        // Minute hand
        strokePaint.StrokeWidth = 10;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawLine(0, 0, 0, -70, strokePaint);
        canvas.Restore();

        // Second hand
        strokePaint.StrokeWidth = 2;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Second);
        canvas.DrawLine(0, 10, 0, -80, strokePaint);
        canvas.Restore();
    }
}
```

Часы наверняка работают, хотя руки довольно грубый:

[![](rotate-images/uglyanalogclock-small.png "Triple screenshot of the Ugly Analog Clock Text page")](rotate-images/uglyanalogclock-large.png#lightbox "Triple screenshot of the Ugly Analog page")

Более привлекательные часы см. в статье [**данные о пути SVG в SkiaSharp**](../curves/path-data.md).

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
