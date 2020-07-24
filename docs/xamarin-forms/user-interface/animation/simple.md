---
title: Простые анимации вXamarin.Forms
description: Класс Виевекстенсионс предоставляет методы расширения, которые можно использовать для создания простых анимаций. В этой статье показано, как создавать и отменять анимацию с помощью класса Виевекстенсионс.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/05/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b13ec7ab079dcf7069b5f4b0dccbb52faf25f927
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933800"
---
# <a name="simple-animations-in-xamarinforms"></a>Простые анимации вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)

_Класс Виевекстенсионс предоставляет методы расширения, которые можно использовать для создания простых анимаций. В этой статье показано, как создавать и отменять анимацию с помощью класса Виевекстенсионс._

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)Класс предоставляет следующие методы расширения, которые можно использовать для создания простых анимаций:

- [ `TranslateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Транслатето ( Xamarin.Forms . Висуалелемент, System. Double, System. Double, System. UInt32, Xamarin.Forms . Замедление)) анимируется [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) Свойства и объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)анимируется [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойство объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- `ScaleXTo`анимируется [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) свойство объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- `ScaleYTo`анимируется [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) свойство объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RelScaleTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Релскалето ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) применяет анимированное добавочное увеличение или уменьшение к [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойству объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RotateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. RotateTo ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) анимируется [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) свойство объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RelRotateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Релротатето ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) применяет анимированное добавочное увеличение или уменьшение к [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) свойству объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RotateXTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Ротатексто ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) анимируется [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) свойство объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RotateYTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Ротатэйто ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) анимируется [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) свойство объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `FadeTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Фадето ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) анимируется [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) свойство объекта [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

По умолчанию каждая анимация займет 250 миллисекунд. Однако длительность каждой анимации может быть задана при создании анимации.

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)Класс также включает [ `CancelAnimations` ] (xref: Xamarin.Forms . Виевекстенсионс. Канцеланиматионс ( Xamarin.Forms . Висуалелемент)), который можно использовать для отмены любых анимаций.

> [!NOTE]
> [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)Класс предоставляет [ `LayoutTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Лайаутто ( Xamarin.Forms . Висуалелемент, Xamarin.Forms . Прямоугольник, System. UInt32, Xamarin.Forms . Замедление)) метод расширения. Однако этот метод предназначен для использования в макетах для анимации переходов между состояниями макета, которые содержат изменения размера и расположения. Поэтому он должен использоваться только [`Layout`](xref:Xamarin.Forms.Layout) подклассами.

Методы расширения анимации в [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) классе являются асинхронными и возвращают `Task<bool>` объект. Возвращаемое значение равно `false` , если анимация завершена, и `true` если анимация отменена. Таким образом, методы анимации обычно следует использовать с `await` оператором, что позволяет легко определить время завершения анимации. Кроме того, можно создать последовательную анимацию с последующими методами анимации, выполняемыми после завершения предыдущего метода. Дополнительные сведения см. в разделе [Составные анимации](#compound-animations).

Если есть требование разрешить выполнение анимации в фоновом режиме, `await` оператор можно опустить. В этом сценарии методы расширения анимации быстро возвращаются после запуска анимации с анимацией, которая происходит в фоновом режиме. Эту операцию можно использовать при создании составных анимаций. Дополнительные сведения см. в разделе [Составные анимации](#composite-animations).

Дополнительные сведения об `await` операторе см. в разделе [Общие сведения о поддержке асинхронных](~/cross-platform/platform/async.md)операций.

## <a name="single-animations"></a>Отдельные анимации

Каждый метод расширения в [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) реализует одну операцию анимации, которая постепенно изменяет свойство из одного значения на другое в течение определенного периода времени. В этом разделе рассматриваются все операции анимации.

### <a name="rotation"></a>Смена

В следующем примере кода показано использование [ `RotateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. RotateTo ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) метод для анимации [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) свойства объекта [`Image`](xref:Xamarin.Forms.Image) :

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Этот код анимируется [`Image`](xref:Xamarin.Forms.Image) экземпляр путем поворота до 360 градусов в течение 2 секунд (2000 миллисекунд). [ `RotateTo` ] (Xref: Xamarin.Forms . Виевекстенсионс. RotateTo ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление). метод получает текущее [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) значение свойства для начала анимации, а затем поворачивается от этого значения к первому аргументу (360). После завершения анимации [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) свойство изображения сбрасывается в 0. Это гарантирует, что `Rotation` свойство не останется в 360 после завершения анимации, что помешает дополнительному повороту.

На следующих снимках экрана показан ход вращения на каждой платформе:

![Анимация вращения](simple-images/rotateto.png)

> [!NOTE]
> В дополнение к [ `RotateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. RotateTo ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)), также есть [ `RotateXTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Ротатексто ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) и [ `RotateYTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Ротатэйто ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) методы, которые анимированы [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) Свойства и соответственно.

### <a name="relative-rotation"></a>Относительный поворот

В следующем примере кода показано использование [ `RelRotateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Релротатето ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) для инкрементного увеличения или уменьшения [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) свойства объекта [`Image`](xref:Xamarin.Forms.Image) :

```csharp
await image.RelRotateTo (360, 2000);
```

Этот код выполняет анимацию [`Image`](xref:Xamarin.Forms.Image) экземпляра, поворачивая 360 градусов с начальной позицией более 2 секунд (2000 миллисекунд). [ `RelRotateTo` ] (Xref: Xamarin.Forms . Виевекстенсионс. Релротатето ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление). метод получает текущее [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) значение свойства для начала анимации, а затем поворачивает из этого значения в значение плюс его первый аргумент (360). Это гарантирует, что каждая анимация всегда будет начинаться с 360 градусов с начальной позицией. Таким образом, если новая анимация вызывается, пока анимация уже выполняется, она начнется с текущей позиции и может заканчиваться на позиции, которая не является инкрементом в 360 градусов.

На следующих снимках экрана показано, как выполняется относительное вращение на каждой платформе.

![Анимация относительного вращения](simple-images/relrotateto.png)

### <a name="scaling"></a>Масштабирование

В следующем примере кода показано использование [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) метода для анимации свойства объекта [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`Image`](xref:Xamarin.Forms.Image) .

```csharp
await image.ScaleTo (2, 2000);
```

Этот код анимируется [`Image`](xref:Xamarin.Forms.Image) экземпляр путем увеличения масштаба до двух секунд (2000 миллисекунд). [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)Метод получает текущее [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) значение свойства (значение по умолчанию 1) для начала анимации, а затем масштабируется от этого значения до первого аргумента (2). Это приведет к увеличению размера изображения в два раза больше его размера.

На следующих снимках экрана показано, как выполняется масштабирование на каждой платформе:

![Анимация масштабирования](simple-images/scaleto.png)

> [!NOTE]
> Помимо [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) метода, существуют также `ScaleXTo` `ScaleYTo` методы и, которые анимировать [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) Свойства и соответственно.

### <a name="relative-scaling"></a>Относительное масштабирование

В следующем примере кода показано использование [ `RelScaleTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Релскалето ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) метод для анимации [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойства объекта [`Image`](xref:Xamarin.Forms.Image) :

```csharp
await image.RelScaleTo (2, 2000);
```

Этот код анимируется [`Image`](xref:Xamarin.Forms.Image) экземпляр путем увеличения масштаба до двух секунд (2000 миллисекунд). [ `RelScaleTo` ] (Xref: Xamarin.Forms . Виевекстенсионс. Релскалето ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление). метод получает текущее [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) значение свойства для начала анимации, а затем масштабирует от этого значения до значения плюс его первый аргумент (2). Это гарантирует, что каждая анимация всегда будет масштабироваться на 2 из начального положения.

### <a name="scaling-and-rotation-with-anchors"></a>Масштабирование и поворот с помощью привязок

[`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)Свойства и [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) устанавливают центр масштабирования или вращения для [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойств и. Таким образом, их значения также влияют на [ `RotateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. RotateTo ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление) и [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) методы.

При наличии объекта [`Image`](xref:Xamarin.Forms.Image) , помещенного в центр макета, в следующем примере кода показано вращение изображения вокруг центра макета путем установки его [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) Свойства:

```csharp
double radius = Math.Min(absoluteLayout.Width, absoluteLayout.Height) / 2;
image.AnchorY = radius / image.Height;
await image.RotateTo(360, 2000);
```

Чтобы повернуть [`Image`](xref:Xamarin.Forms.Image) экземпляр вокруг центра макета, [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) Свойства и должны быть установлены в значения, относящиеся к ширине и высоте `Image` . В этом примере центр определяется как `Image` центр макета, поэтому значение по умолчанию `AnchorX` 0,5 не требует изменения. Однако `AnchorY` свойство переопределяется как значение от верха `Image` до центральной точки макета. Это обеспечит `Image` полный поворот на 360 градусов вокруг центральной точки макета, как показано на следующих снимках экрана:

![Анимация вращения с привязками](simple-images/rotate-anchors.png)

### <a name="translation"></a>Перевод

В следующем примере кода показано использование [ `TranslateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Транслатето ( Xamarin.Forms . Висуалелемент, System. Double, System. Double, System. UInt32, Xamarin.Forms . Замедление)) метод для [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) анимации [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) свойств и объекта [`Image`](xref:Xamarin.Forms.Image) :

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Этот код анимирует [`Image`](xref:Xamarin.Forms.Image) экземпляр, переведя его по горизонтали и вертикали в течение 1 секунды (1000 миллисекунд). [ `TranslateTo` ] (Xref: Xamarin.Forms . Виевекстенсионс. Транслатето ( Xamarin.Forms . Висуалелемент, System. Double, System. Double, System. UInt32, Xamarin.Forms . Замедление)). метод одновременно преобразовывает изображение 100 пикселей влево и 100 пикселей в сторону. Это связано с тем, что первый и второй аргументы являются отрицательными числами. При предоставлении положительных чисел изображение будет переводиться вправо и вниз.

На следующих снимках экрана показано, как выполняется трансляция на каждой платформе:

![Анимация перевода](simple-images/translateto.png)

> [!NOTE]
> Если элемент изначально размещается на экране, а затем преобразуется на экран, после перевода входной макет элемента остается вне экрана и пользователь не может взаимодействовать с ним. Поэтому рекомендуется размещать представление в окончательном положении, а затем выполнять все необходимые переводы.

### <a name="fading"></a>Исчезание

В следующем примере кода показано использование [ `FadeTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Фадето ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) метод для анимации [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) свойства объекта [`Image`](xref:Xamarin.Forms.Image) :

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Этот код выполняет анимацию [`Image`](xref:Xamarin.Forms.Image) экземпляра, поменяя его на более 4 секунд (4000 миллисекунд). [ `FadeTo` ] (Xref: Xamarin.Forms . Виевекстенсионс. Фадето ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление). метод получает текущее [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) значение свойства для начала анимации, а затем исчезает из этого значения в первый аргумент (1).

На следующих снимках экрана показано, как выполняется выцветание на каждой платформе:

![Анимация затухания](simple-images/fadeto.png)

## <a name="compound-animations"></a>Составные анимации

Составная анимация — это последовательная комбинация анимации, которую можно создать с помощью `await` оператора, как показано в следующем примере кода:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

В этом примере [`Image`](xref:Xamarin.Forms.Image) преобразуются более 6 секунд (6000 миллисекунд). В переводе `Image` используется пять анимаций с `await` оператором, указывающим, что каждая анимация выполняется последовательно. Поэтому последующие методы анимации выполняются после завершения предыдущего метода.

## <a name="composite-animations"></a>Составные анимации

Составная анимация — это сочетание анимаций, в которых одновременно выполняются две или более анимаций. Составные анимации можно создавать путем смешивания ожидающих и неожидаемых анимаций, как показано в следующем примере кода:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

В этом примере [`Image`](xref:Xamarin.Forms.Image) масштабирование масштабируется и одновременно поворачивается более 4 секунд (4000 миллисекунд). Масштабирование `Image` использует две последовательные анимации, которые происходят одновременно с поворотом. [ `RotateTo` ] (Xref: Xamarin.Forms . Виевекстенсионс. RotateTo ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)). метод выполняется без `await` оператора и возвращается сразу же, начиная с первой [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) анимации. `await`Оператор в первом `ScaleTo` вызове метода задерживает второй `ScaleTo` вызов метода до тех пор, пока `ScaleTo` не завершится первый вызов метода. На этом этапе `RotateTo` анимация является полугодией и `Image` будет повернута на 180 градусов. В течение последних 2 секунд (2000 миллисекунд) вторая `ScaleTo` анимация и `RotateTo` анимация завершаются.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Параллельное выполнение нескольких асинхронных методов

`static` `Task.WhenAny` Методы и `Task.WhenAll` используются для параллельного выполнения нескольких асинхронных методов, поэтому их можно использовать для создания составных анимаций. Оба метода возвращают `Task` объект и принимают коллекцию методов, каждый из которых возвращает `Task` объект. `Task.WhenAny`Метод завершается, когда выполнение любого метода в его коллекции завершается, как показано в следующем примере кода:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

В этом примере `Task.WhenAny` вызов метода содержит две задачи. Первая задача поворачивает изображение в течение 4 секунд (4000 миллисекунд), а вторая задача масштабирует изображение в течение 2 секунд (2000 миллисекунд). По завершении второй задачи `Task.WhenAny` вызов метода завершается. Однако несмотря на то, что [ `RotateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. RotateTo ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)), второй [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) метод может начать работу.

`Task.WhenAll`Метод завершается, когда все методы в его коллекции завершены, как показано в следующем примере кода:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

В этом примере `Task.WhenAll` вызов метода содержит три задачи, каждый из которых выполняется более 10 минут. Каждый `Task` из них имеет различное число 307 поворотов 360 градусов для [ `RotateTo` ] (xref: Xamarin.Forms . Виевекстенсионс. RotateTo ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)), 251 вращения для [ `RotateXTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Ротатексто ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)) и 199 ротации для [ `RotateYTo` ] (xref: Xamarin.Forms . Виевекстенсионс. Ротатэйто ( Xamarin.Forms . Висуалелемент, System. Double, System. UInt32, Xamarin.Forms . Замедление)). Эти значения являются простыми числами, поэтому они гарантируют, что вращение не синхронизированы и, следовательно, не будут приводить к созданию повторяющихся шаблонов.

На следующих снимках экрана показано, как выполняется несколько поворотов на каждой платформе:

![Составная анимация](simple-images/multiple-rotations.png)

## <a name="canceling-animations"></a>Отмена анимации

Приложение может отменить одну или несколько анимаций с помощью вызова `static` [ `ViewExtensions.CancelAnimations` ] (xref: Xamarin.Forms . Виевекстенсионс. Канцеланиматионс ( Xamarin.Forms . Висуалелемент)), как показано в следующем примере кода:

```csharp
ViewExtensions.CancelAnimations (image);
```

В результате будут немедленно отменены все анимации, выполняемые в данный момент на [`Image`](xref:Xamarin.Forms.Image) экземпляре.

## <a name="summary"></a>Сводка

В этой статье показано, как создавать и отменять анимацию с помощью [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) класса. Этот класс предоставляет методы расширения, которые можно использовать для создания простых анимаций, повернутых, масштабируемых, преобразованных и конусных [`VisualElement`](xref:Xamarin.Forms.VisualElement) экземпляров.

## <a name="related-links"></a>Связанные ссылки

- [Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)
- [Базовая анимация (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)
- [виевекстенсионс](xref:Xamarin.Forms.ViewExtensions)
