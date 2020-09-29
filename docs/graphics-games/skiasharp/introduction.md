---
title: Примеры, независимые от платформы SkiaSharp
description: Этот документ содержит краткое введение в основные понятия SkiaSharp. В частности, он обсуждает получение и рисование на Скканвас.
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2018
ms.openlocfilehash: fd8926edce6b2310271e15418f2498162723ba49
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436446"
---
# <a name="skiasharp-platform-independent-examples"></a>Примеры, независимые от платформы SkiaSharp

_Это дает краткое независимое от платформы Введение в основные понятия, лежащие в основе SkiaSharp_

SkiaSharp предоставляет богатый и мощный API двухмерной графики, который можно использовать для отображения в плоских буферах.  Их можно использовать для реализации настраиваемых элементов пользовательского интерфейса и двухмерной графики, которые могут быть включены в приложение. SkiaSharp — это привязка .NET к библиотеке [СКИА](https://skia.org) , которая наследует функции и возможности этой библиотеки.

В настоящее время библиотека доступна в качестве кросс-платформенного [пакета NuGet](https://www.nuget.org/packages/SkiaSharp). ее можно добавить в проект, добавив ссылку NuGet.

Для рисования код создаст объект, `SkCanvas` описывающий поверхность, в которой будут выполняться операции рисования.

## <a name="obtaining-an-skcanvas"></a>Получение Скканвас

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>Рисование на Скканвас

`SKCanvas`Использует модель рисования, похожую на "другие модели рисования, с которыми вы можете ознакомиться, она использует цвета с дополнительным каналом прозрачности и может рисовать линии, дуги, текст и изображения.

Ниже приведены лишь некоторые из множества различных вещей, которые можно выполнить с помощью SkiaSharp.  В приведенных ниже примерах переменная `canvas` имеет тип скканвас.

### <a name="drawing-xamagon"></a>Рисование Ксамагон

В этом примере эмблема Xamarin рисуется на Ксамагон:

```csharp
// clear the canvas / fill with white
canvas.Clear (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x2c, 0x3e, 0x50);
  paint.StrokeCap = SKStrokeCap.Round;

  // create the Xamagon path
  using (var path = new SKPath ()) {
    path.MoveTo (71.4311121f, 56f);
    path.CubicTo (68.6763107f, 56.0058575f, 65.9796704f, 57.5737917f, 64.5928855f, 59.965729f);
    path.LineTo (43.0238921f, 97.5342563f);
    path.CubicTo (41.6587026f, 99.9325978f, 41.6587026f, 103.067402f, 43.0238921f, 105.465744f);
    path.LineTo (64.5928855f, 143.034271f);
    path.CubicTo (65.9798162f, 145.426228f, 68.6763107f, 146.994582f, 71.4311121f, 147f);
    path.LineTo (114.568946f, 147f);
    path.CubicTo (117.323748f, 146.994143f, 120.020241f, 145.426228f, 121.407172f, 143.034271f);
    path.LineTo (142.976161f, 105.465744f);
    path.CubicTo (144.34135f, 103.067402f, 144.341209f, 99.9325978f, 142.976161f, 97.5342563f);
    path.LineTo (121.407172f, 59.965729f);
    path.CubicTo (120.020241f, 57.5737917f, 117.323748f, 56.0054182f, 114.568946f, 56f);
    path.LineTo (71.4311121f, 56f);
    path.Close ();

    // draw the Xamagon path
    canvas.DrawPath (path, paint);
  }
}
```

### <a name="drawing-text"></a>Рисование текста

```csharp
// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.TextSize = 64.0f;
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x42, 0x81, 0xA4);
  paint.IsStroke = false;

  // draw the text
  canvas.DrawText ("Skia", 0.0f, 64.0f, paint);
}
```

### <a name="drawing-bitmaps"></a>Рисование растровых изображений

```csharp
Stream fileStream = File.OpenRead ("MyImage.png");

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  canvas.DrawBitmap(bitmap, SKRect.Create(Width, Height), paint);
}
```

### <a name="drawing-with-image-filters"></a>Рисование с помощью фильтров изображений

```csharp
Stream fileStream = File.OpenRead ("MyImage.png"); // open a stream to an image file

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  // create the image filter
  using (var filter = SKImageFilter.CreateBlur(5, 5)) {
    paint.ImageFilter = filter;

    // draw the bitmap through the filter
    canvas.DrawBitmap(bitmap, SKRect.Create(width, height), paint);
  }
}
```

## <a name="more-information"></a>Дополнительные сведения

Дополнительные сведения об использовании SkiaSharp можно найти в документации по [API](/dotnet/api/skiasharp) .