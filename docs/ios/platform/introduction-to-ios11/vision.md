---
title: Инфраструктура концепции в Xamarin. iOS
description: В этом документе описывается, как использовать платформу для работы с iOS 11 в Xamarin. iOS. В частности, он описывает обнаружение прямоугольников и обнаружение лиц.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/31/2017
ms.openlocfilehash: 9a31cd31f7cbfb4748991d45204ca867b5d62c8c
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436624"
---
# <a name="vision-framework-in-xamarinios"></a>Инфраструктура концепции в Xamarin. iOS

Инфраструктура концепции добавляет ряд новых функций обработки изображений в iOS 11, включая:

- [Определение прямоугольника](#rectangles)
- [обнаружение лиц](#faces)
- Анализ изображений Машинное обучение (рассматривается в [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Определение штрихкода
- Анализ выравнивания изображений
- Обнаружение текста
- Обнаружение горизонта
- Отслеживание & обнаружения объектов

![Фотография с обнаруженными тремя прямоугольниками](vision-images/found-rectangles-tiny.png) ![Обнаружена фотография с двумя лицами](vision-images/xamarin-home-faces-tiny.png)

Обнаружение и обнаружение лиц прямоугольников рассматриваются более подробно ниже.

<a name="rectangles"></a>

## <a name="rectangle-detection"></a>Определение прямоугольника

В [примере висионректс](/samples/xamarin/ios-samples/ios11-visionrectangles) показано, как обработать изображение и нарисовать обнаруженные на нем прямоугольники.

### <a name="1-initialize-the-vision-request"></a>1. Инициализация запроса концепции

В `ViewDidLoad` Создайте объект `VNDetectRectanglesRequest` , который ссылается на `HandleRectangles` метод, который будет вызываться в конце каждого запроса:

`MaximumObservations`Свойство также должно быть задано, в противном случае будет использоваться значение по умолчанию 1, и будет возвращен только один результат.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Начните обработку концепции

Следующий код начинает обработку запроса. В примере **висионректс** этот код выполняется после выбора пользователем изображения:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Этот обработчик передает объект `ciImage` в инфраструктуру концепции `VNDetectRectanglesRequest` , созданную на шаге 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. обработка результатов обработки концепции

После завершения обнаружения прямоугольника платформа выполняет `HandleRectangles` метод, а также сводку, показанную ниже.

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Отображение результатов

`OverlayRectangles`Метод в примере **висионректанглес** имеет три функции:

- Подготовка к просмотру исходного изображения,
- Рисование прямоугольника для указания места обнаружения каждого из них и
- Добавление текстовой метки для каждого прямоугольника с помощью Кореграфикс.

Просмотрите [Источник образца](/samples/xamarin/ios-samples/ios11-visionrectangles) для точного метода кореграфикс.

![Фотография с обнаруженными тремя прямоугольниками](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Дальнейшая обработка

Обнаружение прямоугольника часто является просто первым шагом в цепочке операций, например в [этом коремлвисион примере](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), где прямоугольники передаются в модель CoreML для анализа рукописных цифр.

<a name="faces"></a>

## <a name="face-detection"></a>Обнаружение лиц

[Пример висионфацес](/samples/xamarin/ios-samples/ios11-visionfaces) работает аналогично образцу **висионректанглес** , используя другой класс запроса концепции.

### <a name="1-initialize-the-vision-request"></a>1. Инициализация запроса концепции

В `ViewDidLoad` Создайте объект `VNDetectFaceRectanglesRequest` , который ссылается на `HandleRectangles` метод, который будет вызываться в конце каждого запроса.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Начните обработку концепции

Следующий код начинает обработку запроса. В примере **висионфацес** этот код выполняется после выбора пользователем изображения:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Этот обработчик передает объект `ciImage` в инфраструктуру концепции `VNDetectFaceRectanglesRequest` , созданную на шаге 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. обработка результатов обработки концепции

После завершения обнаружения распознавания лиц обработчик выполняет `HandleRectangles` метод, который выполняет обработку ошибок и отображает границы обнаруженных лиц и вызывает метод `OverlayRectangles` для рисования ограничивающих прямоугольников на исходном изображении:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Отображение результатов

`OverlayRectangles`Метод в примере **висионфацес** имеет три функции:

- Подготовка к просмотру исходного изображения,
- Рисование прямоугольника для каждого обнаруженного лица и
- Добавление текстовой метки для каждой грани с помощью Кореграфикс.

Просмотрите [Источник образца](/samples/xamarin/ios-samples/ios11-visionfaces) для точного метода кореграфикс.

![Обнаружена фотография с двумя лицами](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Дальнейшая обработка

Инфраструктура концепции включает дополнительные возможности для определения возможностей лиц, таких как глаза и рот. Используйте `VNDetectFaceLandmarksRequest` тип, который возвратит результаты, `VNFaceObservation` как на шаге 3 выше, но с дополнительными `VNFaceLandmark` данными.

## <a name="related-links"></a>Связанные ссылки

- [Прямоугольники концепции (пример)](/samples/xamarin/ios-samples/ios11-visionrectangles)
- [Лица с видением (пример)](/samples/xamarin/ios-samples/ios11-visionfaces)
- [Усовершенствования в основном образе — фильтры, металл, концепция и многое другое (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/510/)