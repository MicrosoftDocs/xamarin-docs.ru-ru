---
title: Преобразования матрицы в SkiaSharp
description: Эта статья подробно глубже в SkiaSharp преобразований с помощью универсальной матрицы преобразования и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: davidbritch
ms.author: dabritch
ms.date: 04/12/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 911365b6293fecd3bf309f3e61d9b232d90b7a13
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556559"
---
# <a name="matrix-transforms-in-skiasharp"></a>Преобразования матрицы в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Подробное рассмотрение SkiaSharp преобразований с помощью универсальной матрицы преобразования_

Все преобразования, примененные к `SKCanvas` объекту, объединяются в один экземпляр [`SKMatrix`](xref:SkiaSharp.SKMatrix) структуры. Это стандартная матрица преобразований размером 3 на 3, аналогичная той, которая относится ко всем современным системам двухмерной графики.

Как вы уже видели, вы можете использовать преобразования в SkiaSharp, не зная матрицы преобразований, но матрица преобразования важна с теоретической точки зрения, и это крайне важно при использовании преобразований для изменения путей или для обработки сложных сенсорных входных данных, которые демонстрируются в этой статье и далее.

![Точечный рисунок, подчиняются аффинное преобразованию](matrix-images/matrixtransformexample.png)

Текущая матрица преобразования, примененная к, `SKCanvas` доступна в любое время, обращаясь к свойству, доступному только для чтения [`TotalMatrix`](xref:SkiaSharp.SKCanvas.TotalMatrix) . Новую матрицу преобразования можно задать с помощью [`SetMatrix`](xref:SkiaSharp.SKCanvas.SetMatrix(SkiaSharp.SKMatrix)) метода, а эту матрицу преобразования можно восстановить в значения по умолчанию, вызвав [`ResetMatrix`](xref:SkiaSharp.SKCanvas.ResetMatrix) .

Единственный другой `SKCanvas` элемент, который напрямую работает с матричным преобразованием холста, [`Concat`](xref:SkiaSharp.SKCanvas.Concat(SkiaSharp.SKMatrix@)) объединяет две матрицы, умножая их друг на друга.

Матрица преобразования по умолчанию — это матрица идентификации, которая состоит из 1 в диагональных ячейках и 0 в любом месте:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Матрицу идентификаторов можно создать с помощью статического  [`SKMatrix.MakeIdentity`](xref:SkiaSharp.SKMatrix.MakeIdentity) метода:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix`Конструктор по умолчанию *не* Возвращает матрицу удостоверений. Он возвращает матрицу со всеми ячейками, для которых установлено нулевое значение. Не используйте конструктор, `SKMatrix` Если вы не планируете задавать эти ячейки вручную.

Когда SkiaSharp визуализирует графический объект, каждая точка (x, y) фактически преобразуется в матрицу размером 1 на 3 с 1 в третьем столбце:

<pre>
| x  y  1 |
</pre>

Матрица размером 1 на 3 представляет трехмерную точку с координатой Z, установленной в 1. Существуют математические причины (обсуждаются далее), почему для многомерного преобразования требуется работать в трех измерениях. Эту матрицу размером 1 на 3 можно представить как точку в трехмерной системе координат, но всегда на двумерной плоскости, где Z равно 1.

Эта матрица с 1 по 3 затем умножается на матрицу преобразования, а результатом является точка, отображаемая на холсте:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Используя стандартное умножение матрицы, преобразованные точки будут выглядеть следующим образом:

`x' = x`

`y' = y`

`z' = 1`

Это преобразование по умолчанию.

При `Translate` вызове метода для `SKCanvas` объекта `tx` `ty` аргументы и для `Translate` метода становятся первыми двумя ячейками в третьей строке матрицы преобразования:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Умножение теперь выполняется следующим образом:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Ниже приведены формулы преобразования.

`x' = x + tx`

`y' = y + ty`

Коэффициенты масштабирования имеют значение по умолчанию 1. При вызове `Scale` метода для нового `SKCanvas` объекта матрица результирующего преобразования содержит `sx` `sy` аргументы и в диагональных ячейках:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Формулы преобразования выглядят следующим образом:

`x' = sx · x`

`y' = sy · y`

Матрица преобразования после вызова `Skew` содержит два аргумента в ячейках матрицы, смежных с коэффициентами масштабирования:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Формулы преобразования:

`x' = x + xSkew · y`

`y' = ySkew · x + y`

Для вызова `RotateDegrees` или `RotateRadians` для угла α матрица преобразования выглядит следующим образом:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Ниже приведены формулы преобразования.

`x' = cos(α) · x - sin(α) · y`

`y' = sin(α) · x - cos(α) · y`

Если α имеет значение 0 градусов, это матрица идентификаторов. Если α имеет значение 180 градусов, матрица преобразования выглядит следующим образом:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

Поворот на 180 градусов эквивалентен зеркальному отображению объекта по горизонтали и вертикали, что также достигается путем установки коэффициентов масштабирования – 1.

Все эти типы преобразований классифицируются как *аффинное* преобразования. Аффинных преобразований никогда не затрагивает третий столбец матрицы, который остается в значениях по умолчанию 0, 0 и 1. В статье [**неаффинных преобразований**](non-affine.md) обсуждаются преобразования, не являющиеся аффинных.

## <a name="matrix-multiplication"></a>умножение матриц

Одним из существенных преимуществ использования матрицы преобразований является то, что составные преобразования могут быть получены путем умножения матрицы, которое часто упоминается в документации SkiaSharp как *сцепление*. Многие методы, связанные с преобразованием, в разделе `SKCanvas` «pre-СЦЕПИТЬ» или «pre-Concat». Это относится к порядку умножения, что важно, так как перемножение матриц не коммутативной.

Например, документация по [`Translate`](xref:SkiaSharp.SKCanvas.Translate(System.Single,System.Single)) методу говорит о том, что она «предварительно отменяет текущую матрицу с указанным переводом», а документация для [`Scale`](xref:SkiaSharp.SKCanvas.Scale(System.Single,System.Single)) метода говорит о том, что она «заранее применяет текущую матрицу с указанной шкалой».

Это означает, что преобразование, заданное вызовом метода, является множителем (левый операнд), а текущая матрица преобразования — множимое (правый операнд).

Предположим, что `Translate` вызывается, за которым следует `Scale` :

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale`Преобразование умножается на `Translate` преобразование для матрицы составного преобразования.

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` метод можно вызвать до `Translate` следующего вида:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

В этом случае порядок умножения изменяется на обратный, а коэффициенты масштабирования эффективно применяются к факторам перевода:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Ниже приведен `Scale` метод с точкой вращения.

```csharp
canvas.Scale(sx, sy, px, py);
```

Это эквивалентно следующим вызовам преобразования и масштабирования:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Три матрицы преобразования умножаются в обратном порядке от того, как методы отображаются в коде:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

## <a name="the-skmatrix-structure"></a>Структура Скматрикс

`SKMatrix`Структура определяет девять свойств чтения и записи типа `float` , соответствующих девяти ячейкам матрицы преобразования:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` также определяет свойство с именем [`Values`](xref:SkiaSharp.SKMatrix.Values) типа `float[]` . Это свойство можно использовать для задания или получения девяти значений в одном снимке в порядке,,,,,,, `ScaleX` `SkewX` `TransX` `SkewY` `ScaleY` `TransY` `Persp0` `Persp1` и `Persp2` .

`Persp0`Ячейки, `Persp1` и `Persp2` обсуждаются в статье [**неаффинных преобразований**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Если значения по умолчанию для этих ячеек равны 0, 0 и 1, то преобразование умножается на точку координат следующего вида:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

`x' = ScaleX · x + SkewX · y + TransX`

`y' = SkewX · x + ScaleY · y + TransY`

`z' = 1`

Это полное двухмерный аффинное преобразование. Аффинное преобразование сохраняет параллельные линии, т. е. прямоугольник никогда не преобразуется в ничего, кроме параллелограмма.

`SKMatrix`Структура определяет несколько статических методов для создания `SKMatrix` значений. Все возвращаемые `SKMatrix` значения:

- [`MakeTranslation`](xref:SkiaSharp.SKMatrix.MakeTranslation(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single))
- [`MakeScale`](xref:SkiaSharp.SKMatrix.MakeScale(System.Single,System.Single,System.Single,System.Single)) с точкой вращения
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single)) для угла в радианах
- [`MakeRotation`](xref:SkiaSharp.SKMatrix.MakeRotation(System.Single,System.Single,System.Single)) угол в радианах с точкой вращения
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single))
- [`MakeRotationDegrees`](xref:SkiaSharp.SKMatrix.MakeRotationDegrees(System.Single,System.Single,System.Single)) с точкой вращения
- [`MakeSkew`](xref:SkiaSharp.SKMatrix.MakeSkew(System.Single,System.Single))

`SKMatrix` также определяет несколько статических методов, объединяющих две матрицы, что означает их умножение. Эти методы называются [`Concat`](xref:SkiaSharp.SKMatrix.Concat*) , [`PostConcat`](xref:SkiaSharp.SKMatrix.PostConcat*) и [`PreConcat`](xref:SkiaSharp.SKMatrix.PreConcat*) , и существует две версии каждого из них. Эти методы не имеют возвращаемых значений; Вместо этого они ссылаются на существующие `SKMatrix` значения с помощью `ref` аргументов. В следующем примере,, `A` `B` и `R` (для "Result") являются `SKMatrix` значениями.

Два `Concat` метода вызываются следующим образом:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Они выполняют следующее умножение:

`R = B × A`

Другие методы имеют только два параметра. Первый параметр изменяется, и при возврате из вызова метода содержит произведение двух матриц. Два `PostConcat` метода вызываются следующим образом:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Эти вызовы выполняют следующую операцию:

`A = A × B`

Эти два `PreConcat` метода похожи:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Эти вызовы выполняют следующую операцию:

`A = B × A`

Версии этих методов со всеми `ref` аргументами немного более эффективны при вызове базовых реализаций, но это может вызвать путаницу при чтении кода и при условии, что все `ref` аргументы с аргументом изменяются методом. Более того, часто удобно передавать аргумент, являющийся результатом одного из `Make` методов, например:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

Будет создана следующая матрица:

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

Это преобразование масштаба умножается на преобразование преобразования. В этом конкретном случае `SKMatrix` Структура предоставляет ярлык с методом с именем [`SetScaleTranslate`](xref:SkiaSharp.SKMatrix.SetScaleTranslate(System.Single,System.Single,System.Single,System.Single)) :

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Это один из нескольких случаев, когда использование конструктора является надежным `SKMatrix` . `SetScaleTranslate`Метод задает все девять ячеек матрицы. Также можно использовать `SKMatrix` конструктор с статическими `Rotate` `RotateDegrees` методами и:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Эти методы *не* выполняют сцепление преобразования «вращение» с существующим преобразованием. Методы задают все ячейки матрицы. Они функционально идентичны `MakeRotation` `MakeRotationDegrees` методам и, за исключением того, что они не создают экземпляр `SKMatrix` значения.

Предположим, у вас есть `SKPath` объект, который вы хотите отобразить, но вы предпочитаете, что у него несколько различной ориентации или другая центральная точка. Вы можете изменить все координаты этого пути, вызвав [`Transform`](xref:SkiaSharp.SKPath.Transform(SkiaSharp.SKMatrix)) метод `SKPath` с `SKMatrix` аргументом. На странице **Преобразование пути** показано, как это сделать. [`PathTransform`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs)Класс ссылается на `HendecagramPath` объект в поле, но использует его конструктор для применения преобразования к этому пути:

```csharp
public class PathTransformPage : ContentPage
{
    SKPath transformedPath = HendecagramArrayPage.HendecagramPath;

    public PathTransformPage()
    {
        Title = "Path Transform";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        SKMatrix matrix = SKMatrix.MakeScale(3, 3);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(360f / 22));
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(300, 300));

        transformedPath.Transform(matrix);
    }
    ...
}
```

`HendecagramPath`Объект имеет центр в (0, 0), и 11 точек звезды выравниваются наружу по центру на 100 единиц во всех направлениях. Это означает, что путь имеет как положительные, так и отрицательные координаты. Страница **Преобразование пути** предпочитает работать с звезды три раза в большую, а все положительные координаты. Более того, не нужно, чтобы одна точка звезды указывала на прямую. Это требуется для того, чтобы одна точка звезды указывала на прямую. (Поскольку звездочка имеет 11 точек, она не может иметь и то и другое). Для этого требуется Вращение звезды на 360 градусов на 22.

Конструктор создает `SKMatrix` объект из трех отдельных преобразований с помощью `PostConcat` метода со следующим шаблоном, где A, B и C являются экземплярами `SKMatrix` :

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Это последовательность последовательных операций умножения, поэтому результат выглядит следующим образом:

`A × B × C`

Последовательные умножения помогают понять, что делает каждое преобразование. Преобразование шкалы увеличивает размер координат пути в 3 раза, поэтому диапазон координат составляет от – 300 до 300. Преобразование «Вращение» поворачивает звезду вокруг начала координат. Преобразование «миграция» затем сдвигает его на 300 пикселей вправо и вниз, поэтому все координаты становятся положительными.

Существуют другие последовательности, создающие одну и ту же матрицу. Вот еще одно:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

Это сначала поворачивает контур вокруг центра, а затем переводит его в 100 пикселов вправо и вниз, чтобы все координаты были положительными. Затем звезда увеличивается по размеру относительно нового верхнего левого угла, который является точкой (0, 0).

`PaintSurface`Обработчик может просто отобразить этот путь:

```csharp
public class PathTransformPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Magenta;
            paint.StrokeWidth = 5;

            canvas.DrawPath(transformedPath, paint);
        }
    }
}

```

Он отображается в левом верхнем углу холста:

[![Тройной снимок экрана страницы "преобразование пути"](matrix-images/pathtransform-small.png)](matrix-images/pathtransform-large.png#lightbox "Тройной снимок экрана страницы "преобразование пути"")

Конструктор этой программы применяет матрицу к пути со следующим вызовом:

```csharp
transformedPath.Transform(matrix);
```

В пути *не* поддерживается эта матрица как свойство. Вместо этого он применяет преобразование ко всем координатам пути. Если `Transform` вызывается снова, преобразование применяется снова, и единственным способом вернуться к возврату является применение другой матрицы, которая отменяет преобразование. К счастью, `SKMatrix` структура определяет [`TryInvert`](xref:SkiaSharp.SKMatrix.TryInvert*) метод, который получает матрицу, которая обращается к заданной матрице:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Метод вызывается `TryInverse` , так как не все матрицы являются обратимыми, но необратимая матрица, скорее всего, не будет использоваться для преобразования графики.

Можно также применить преобразование матрицы к `SKPoint` значению, массиву точек, `SKRect` или даже просто одному числу в программе. `SKMatrix`Структура поддерживает эти операции с коллекцией методов, начинающихся со слова `Map` , например:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

При использовании последнего метода следует помнить, что `SKRect` структура не может представлять повернутый прямоугольник. Метод имеет смысл только для значения, `SKMatrix` представляющего перевод и масштабирование.

## <a name="interactive-experimentation"></a>Интерактивное экспериментирование

Одним из способов сделать аффинное преобразование является Интерактивное перемещение трех углов точечного рисунка вокруг экрана и просмотр результатов преобразования. Это идея на странице « **Показывать аффинное матрицу** ». На этой странице требуются два других класса, которые также используются в других демонстрациях:

[`TouchPoint`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs)Класс отображает полупрозрачный круг, который можно перетащить вокруг экрана. `TouchPoint` требует, чтобы `SKCanvasView` элемент или, являющийся родительским для, `SKCanvasView` имел [`TouchEffect`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) присоединенный объект. Задайте для свойства `Capture` значение `true`. В `TouchAction` обработчике событий программа должна вызывать `ProcessTouchEvent` метод в `TouchPoint` для каждого `TouchPoint` экземпляра. Метод возвращает значение, `true` Если событие касания привело к перемещению сенсорной точки. Кроме того, `PaintSurface` обработчик должен вызвать `Paint` метод в каждом `TouchPoint` экземпляре, передав ему `SKCanvas` объект.

`TouchPoint` демонстрирует распространенный способ, которым SkiaSharp визуальный элемент можно инкапсулировать в отдельный класс. Класс может определять свойства для определения характеристик визуального элемента, а метод `Paint` с именем с `SKCanvas` аргументом может визуализировать его.

`Center`Свойство объекта `TouchPoint` указывает расположение объекта. Это свойство можно задать для инициализации расположения. свойство изменяется, когда пользователь перетаскивает окружность вокруг холста.

Для **страницы «показывать аффинное матрицу** » также требуется [`MatrixDisplay`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) класс. Этот класс отображает ячейки `SKMatrix` объекта. Он имеет два открытых метода: `Measure` для получения измерений отображаемой матрицы и `Paint` для ее отображения. Класс содержит `MatrixPaint` свойство типа `SKPaint` , которое можно заменить другим размером или цветом шрифта.

Файл [**шоваффинематрикспаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) создает экземпляр `SKCanvasView` и присоединяет `TouchEffect` . Файл кода программной части [**ShowAffineMatrixPage.XAML.CS**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) создает три `TouchPoint` объекта, а затем устанавливает их в позиции, соответствующие трем углам точечного рисунка, который он загружает из внедренного ресурса:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    SKMatrix matrix;
    SKBitmap bitmap;
    SKSize bitmapSize;

    TouchPoint[] touchPoints = new TouchPoint[3];

    MatrixDisplay matrixDisplay = new MatrixDisplay();

    public ShowAffineMatrixPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        touchPoints[0] = new TouchPoint(100, 100);                  // upper-left corner
        touchPoints[1] = new TouchPoint(bitmap.Width + 100, 100);   // upper-right corner
        touchPoints[2] = new TouchPoint(100, bitmap.Height + 100);  // lower-left corner

        bitmapSize = new SKSize(bitmap.Width, bitmap.Height);
        matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                           touchPoints[1].Center,
                                           touchPoints[2].Center);
    }
    ...
}
```

Аффинное матрица уникально определяется тремя точками. Три `TouchPoint` объекта соответствуют верхнему левому, верхнему правому и нижнему левому углу точечного рисунка. Так как аффинное матрица поддерживает преобразование прямоугольника в параллелограмм, четвертая точка подразумевается другими тремя. Конструктор завершается вызовом метода `ComputeMatrix` , который вычисляет ячейки `SKMatrix` объекта из этих трех точек.

`TouchAction`Обработчик вызывает `ProcessTouchEvent` метод каждого из них `TouchPoint` . `scale`Значение преобразуется из Xamarin.Forms координат в Пиксели:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = canvasView.CanvasSize.Width / (float)canvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                               touchPoints[1].Center,
                                               touchPoints[2].Center);
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Если `TouchPoint` объект был перемещен, метод снова вызывается `ComputeMatrix` и делает недействительным поверхность.

`ComputeMatrix`Метод определяет матрицу, подразумеваемую этими тремя точками. Матрица, называемая преобразованием `A` квадратного прямоугольника с одним пикселем в параллелограмм на основе трех точек, а масштабирование масштаба — `S` масштабирует точечный рисунок в однопиксельный квадратный прямоугольник. Составная матрица имеет значение `S` × `A` :

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL)
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

        SKMatrix result = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Наконец, `PaintSurface` метод визуализирует точечный рисунок на основе этой матрицы, отображает матрицу в нижней части экрана и отображает сенсорные точки на трех углах точечного рисунка:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap using the matrix
        canvas.Save();
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(matrix);

        matrixDisplay.Paint(canvas, matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));

        // Display the touchpoints
        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }
  }
```

На экране iOS ниже показана битовая карта при первой загрузке страницы, а на двух других экранах она отображается после некоторой манипуляции:

[![Тройной снимок экрана страницы "показывать аффинное матрицу"](matrix-images/showaffinematrix-small.png)](matrix-images/showaffinematrix-large.png#lightbox "Тройной снимок экрана страницы "показывать аффинное матрицу"")

Хотя кажется, что сенсорные точки перетаскивать углы точечного рисунка, это лишь иллюзия. Матрица, вычисленная из сенсорных точек, преобразует точечный рисунок таким образом, чтобы углы совпадали с сенсорными точками.

Более естественным является то, что пользователи могут перемещать, изменять размер и вращать точечные рисунки, не перетаскивая углы, но используя один или два пальца непосредственно над объектом для перетаскивания, сжатия и поворота. Это описано в следующей статье: [**сенсорная манипуляция**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

## <a name="the-reason-for-the-3-by-3-matrix"></a>Причина матрицы с 3 по 3

Может быть, что для двухмерной графической системы потребуется только Матрица преобразования размером 2 на 2:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Это работает для масштабирования, вращения и даже наклона, но не может быть самым базовым для преобразований, которые являются переводом.

Проблема заключается в том, что матрица размером 2 на 2 представляет *линейное* преобразование в двух измерениях. Линейное преобразование сохраняет некоторые базовые арифметические операции, но одно из этих последствий заключается в том, что линейное преобразование никогда не изменяет точку (0, 0). Линейное преобразование делает перевод невозможным.

В трех измерениях Матрица линейного преобразования выглядит следующим образом:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Ячейка с меткой `SkewXY` означает, что значение расклоняет координату x на основе значений Y; ячейка `SkewXZ` означает, что значение наклонено координату x на основе значений Z, а значения отклоняются аналогично другим `Skew` ячейкам.

Эту матрицу трехмерного преобразования можно ограничить на двухмерную плоскость, задав значения `SkewZX` 0 и равные `SkewZY` `ScaleZ` 1.

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Если двухмерные графические объекты полностью рисуются на плоскости в трехмерном пространстве, где Z равно 1, умножение преобразования выглядит следующим образом:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Все они остаются на двумерной плоскости, где Z равно 1, но `SkewXZ` `SkewYZ` ячейки и фактически становятся двухмерный коэффициенты преобразования.

Таким образом трехмерное линейное преобразование служит двумерным нелинейным преобразованием. (Аналогово, преобразование трехмерной графики основано на матрице 4 на 4.)

`SKMatrix`Структура в SkiaSharp определяет свойства для этой третьей строки:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Ненулевые значения `Persp0` и `Persp1` приводят к преобразованию, которое перемещает объекты за пределы двухмерной плоскости, где Z равно 1. Что происходит при перемещении этих объектов обратно в эту плоскость, рассматривается в статье о [**неаффинных преобразованиях**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)