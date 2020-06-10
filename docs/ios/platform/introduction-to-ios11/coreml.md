---
title: Общие сведения о CoreML в Xamarin. iOS
description: В этом документе описывается CoreML, который обеспечивает машинное обучение в iOS. В этом документе описывается, как приступить к работе с CoreML и его использованию с инфраструктурой концепции.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 572ba31a1f19ab099765cc92bb1b389ba1115d1b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84564697"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Общие сведения о CoreML в Xamarin. iOS

CoreML переносит машинное обучение в iOS — приложения могут воспользоваться обученными моделями машинного обучения для выполнения всех задач, от решения проблем до распознавания изображений.

Это введение охватывает следующие аспекты:

- [начало работы с CoreML](#coreml)
- [Использование CoreML с инфраструктурой концепции](#coremlvision)

<a name="coreml"></a>

## <a name="getting-started-with-coreml"></a>начало работы с CoreML

Эти шаги описывают добавление CoreML в проект iOS. Практический пример см. в [образце Habitatного расценок для Mars](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/) .

![Снимок экрана примера прогноза цен habitat для Mars](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. Добавление модели CoreML в проект

Добавьте модель CoreML (файл с расширением **млмодел** ) в каталог **ресурсов** проекта. 

В свойствах файла модели его **действие сборки** имеет значение **коремлмодел**. Это означает, что при сборке приложения она будет скомпилирована в файл **. млмоделк** .

### <a name="2-load-the-model"></a>2. Загрузка модели

Загрузите модель с помощью `MLModel.Create` статического метода:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Задание параметров

Параметры модели передаются и исправляются с помощью класса контейнера, реализующего интерфейс `IMLFeatureProvider` .

Классы поставщиков функций ведут себя как словарь строк и `MLFeatureValue` s, где каждое значение функции может быть простой строкой или числом, массивом или данными или буфером пикселей, содержащим изображение.

Ниже показан код для поставщика функций с одним значением:

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

С помощью таких классов входные параметры могут быть предоставлены способом, понятным CoreML. Имена функций (например `myParam` , в примере кода) должны соответствовать предполагаемой модели.

### <a name="4-run-the-model"></a>4. запуск модели

Для использования модели необходимо, чтобы был создан экземпляр поставщика компонентов и были заданы параметры, а затем `GetPrediction` вызывался метод:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Извлечение результатов

Результат прогноза `outFeatures` также является экземпляром `IMLFeatureProvider` ; к выходным значениям можно обращаться с использованием `GetFeatureValue` имени каждого выходного параметра (например `theResult` ,), как показано в следующем примере:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision"></a>

## <a name="using-coreml-with-the-vision-framework"></a>Использование CoreML с инфраструктурой концепции

CoreML также можно использовать вместе с инфраструктурой концепции для выполнения операций на изображении, таких как распознавание фигур, идентификация объектов и другие задачи.

Описанные ниже действия описывают, как CoreML и концепция используются вместе в [примере коремлвисион](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision). Этот пример сочетает [Распознавание прямоугольников](~/ios/platform/introduction-to-ios11/vision.md#rectangles) от концепции инфраструктуры с моделью _мнинстклассифиер_ CoreML для идентификации рукописной цифры на фотографии.

![Распознавание изображений с номером 3](coreml-images/vision3.png) ![Распознавание изображений с номером 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Создание модели CoreML концепции

CoreML модель _мнистклассифиер_ загружается и затем упаковывается в, `VNCoreMLModel` что делает модель доступной для задач представления. Этот код также создает два запроса концепции: первый — для поиска прямоугольников в изображении, а затем для обработки прямоугольника с помощью модели CoreML:

```csharp
// Load the ML model
var bundle = NSBundle.MainBundle;
var assetPath = bundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
NSError mlErr, vnErr;
var mlModel = MLModel.Create(assetPath, out mlErr);
var model = VNCoreMLModel.FromMLModel(mlModel, out vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(model, HandleClassification);
```

Классу по-прежнему требуется реализовать `HandleRectangles` `HandleClassification` методы и для запросов к концепции, показанные в шагах 3 и 4 ниже.

### <a name="2-start-the-vision-processing"></a>2. Начните обработку концепции

Следующий код начинает обработку запроса. В примере **коремлвисион** этот код выполняется после выбора пользователем изображения:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Этот обработчик передает объект `ciImage` в инфраструктуру концепции `VNDetectRectanglesRequest` , созданную на шаге 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. обработка результатов обработки концепции

После завершения обнаружения прямоугольника он выполняет `HandleRectangles` метод, который обрезает изображение для извлечения первого прямоугольника, преобразует изображение прямоугольника в оттенки серого и передает его в модель CoreML для классификации.

`request`Параметр, переданный в этот метод, содержит подробные сведения о запросе концепции и `GetResults<VNRectangleObservation>()` метод, который возвращает список прямоугольников, найденных в изображении. Первый прямоугольник `observations[0]` извлекается и передается в модель CoreML:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] {ClassificationRequest}, out NSError err);
  });
}
```

`ClassificationRequest`Был инициализирован на шаге 1 для использования `HandleClassification` метода, определенного на следующем шаге.

### <a name="4-handle-the-coreml"></a>4. Обработайте CoreML

`request`Параметр, переданный в этот метод, содержит сведения о запросе CoreML и `GetResults<VNClassificationObservation>()` метод, который возвращает список возможных результатов, упорядоченных по достоверности (с наибольшей достоверностью):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  // ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```

## <a name="samples"></a>Примеры

Существует три примера CoreML, которые можно попробовать:

- [Образец прогнозируемого Habitat цен на Mars](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/) имеет простые числовые входные и выходные данные.

- [Образец & CoreML "концепция](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision) " принимает параметр Image и использует инфраструктуру видения для идентификации квадратных областей в изображении, которые передаются в модель CoreML, распознающую одиночные цифры.

- Наконец, [Пример распознавания изображений CoreML](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition) использует CoreML для поиска компонентов в фотографии. По умолчанию используется меньшая модель **скуизенет** (5 МБ), но она была написана так, чтобы можно было загрузить и внедрить более крупную модель **VGG16** (553MB). Дополнительные сведения см. в [файле сведений для примера](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).

## <a name="related-links"></a>Связанные ссылки

- [Машинное обучение (Apple)](https://developer.apple.com/machine-learning/)
- [Пример CoreML (режим MARS Habitat) (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/)
- [CoreML и концепция (Распознавание номеров) (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)
- [Распознавание изображений CoreML (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition)
- [CoreML с Пользовательское визуальное распознавание Azure (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlazuremodel)
- [Введение в CoreML (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/703/)
