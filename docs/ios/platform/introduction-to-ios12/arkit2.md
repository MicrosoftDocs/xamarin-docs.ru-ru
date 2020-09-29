---
title: ARKit 2 в Xamarin. iOS
description: В этом документе описывается обновление ARKit в iOS 12. Он посвящен использованию эталонных объектов и изображений для обнаружения, включает код для текстурирования окружающей среды и обсуждает распространенные проблемы в программировании ARKit.
ms.prod: xamarin
ms.assetid: af758092-1523-4ab7-aa53-c37a81fb156a
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/22/2018
ms.openlocfilehash: c460eb28197ded7e56c6e11cde98abe201bd06b5
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434285"
---
# <a name="arkit-2-in-xamarinios"></a>ARKit 2 в Xamarin. iOS

ARKit был значительно развит с момента его введения в прошлый год в iOS 11. В первую очередь, теперь можно определять вертикальную и горизонтальную плоскости, что значительно повышает практичность дополнений к реальному. Кроме того, существуют новые возможности:

- Распознавание эталонных изображений и объектов в качестве точки соединения между реальными и цифровыми изображениями
- Новый режим освещения, имитирующий реальное освещение
- Возможность совместного использования и сохранения сред AR
- Новый формат файла, предпочтительный для хранения содержимого AR

## <a name="recognizing-reference-objects"></a>Распознавание ссылочных объектов

Одной из демонстрационных функций в ARKit 2 является возможность распознавания эталонных изображений и объектов. Образы ссылок могут загружаться из обычных файлов изображений (см.[ниже](#more-tracking-configurations)), но объекты ссылок должны быть проверены с помощью ориентированного на разработчика [`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration) .

### <a name="sample-app-scanning-and-detecting-3d-objects"></a>Пример приложения: сканирование и обнаружение трехмерных объектов

Пример « [сканирование и обнаружение трехмерных объектов](/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects) » — это порт [проекта Apple](https://developer.apple.com/documentation/arkit/scanning_and_detecting_3d_objects?language=objc) , который демонстрирует:

- Управление состоянием приложения с помощью [`NSNotification`](xref:Foundation.NSNotification) объектов
- Пользовательская визуализация
- Сложные жесты
- Просмотр объектов
- Сохранение [`ARReferenceObject`](xref:ARKit.ARReferenceObject)

Сканирование объекта-образца — это батарея, а более старые устройства часто не могут столкнуться с достижением стабильного отслеживания.

### <a name="state-management-using-nsnotification-objects"></a>Управление состоянием с помощью объектов Нснотификатион

Это приложение использует конечный автомат, который переходит между следующими состояниями:

- `AppState.StartARSession`
- `AppState.NotReady`
- `AppState.Scanning`
- `AppState.Testing`

И дополнительно использует внедренный набор состояний и переходов в `AppState.Scanning` :

- `Scan.ScanState.Ready`
- `Scan.ScanState.DefineBoundingBox`
- `Scan.ScanState.Scanning`
- `Scan.ScanState.AdjustingOrigin`

Приложение использует реактивную архитектуру, которая отправляет уведомления о смене состояния в [`NSNotificationCenter`](xref:Foundation.NSNotificationCenter) эти уведомления и подписывается на них. Программа установки выглядит следующим образом `ViewController.cs` :

```csharp
// Configure notifications for application state changes
var notificationCenter = NSNotificationCenter.DefaultCenter;

notificationCenter.AddObserver(Scan.ScanningStateChangedNotificationName, State.ScanningStateChanged);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxCreatedNotificationName, State.GhostBoundingBoxWasCreated);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxRemovedNotificationName, State.GhostBoundingBoxWasRemoved);
notificationCenter.AddObserver(ScannedObject.BoundingBoxCreatedNotificationName, State.BoundingBoxWasCreated);
notificationCenter.AddObserver(BoundingBox.ScanPercentageChangedNotificationName, ScanPercentageChanged);
notificationCenter.AddObserver(BoundingBox.ExtentChangedNotificationName, BoundingBoxExtentChanged);
notificationCenter.AddObserver(BoundingBox.PositionChangedNotificationName, BoundingBoxPositionChanged);
notificationCenter.AddObserver(ObjectOrigin.PositionChangedNotificationName, ObjectOriginPositionChanged);
notificationCenter.AddObserver(NSProcessInfo.PowerStateDidChangeNotification, DisplayWarningIfInLowPowerMode);

```

Типичный обработчик уведомлений обновит пользовательский интерфейс и, возможно, изменит состояние приложения, например обработчик, который обновляется при сканировании объекта:

```csharp
private void ScanPercentageChanged(NSNotification notification)
{
    var pctNum = TryGet<NSNumber>(notification.UserInfo, BoundingBox.ScanPercentageUserKey);
    if (pctNum == null)
    {
        return;
    }
    double percentage = pctNum.DoubleValue;
    // Switch to the next state if scan is complete
    if (percentage >= 100.0)
    {
        State.SwitchToNextState();
    }
    else
    {
        DispatchQueue.MainQueue.DispatchAsync(() => navigationBarController.SetNavigationBarTitle($"Scan ({percentage})"));
    }
}

```

Наконец, `Enter{State}` методы изменяют модель и UX в соответствии с новым состоянием:

```csharp
internal void EnterStateTesting()
{
    navigationBarController.SetNavigationBarTitle("Testing");
    navigationBarController.ShowBackButton(false);
    loadModelButton.Hidden = true;
    flashlightButton.Hidden = false;
    nextButton.Enabled = true;
    nextButton.SetTitle("Share", UIControlState.Normal);

    testRun = new TestRun(sessionInfo, sceneView);
    TestObjectDetection();
    CancelMaxScanTimeTimer();
}
```

### <a name="custom-visualization"></a>Пользовательская визуализация

Приложение показывает низкоуровневые "облачные" точки объекта, содержащегося в ограничивающем прямоугольнике, на обнаруженной горизонтальной плоскости.

Это облако точки доступно для разработчиков в [`ARFrame.RawFeaturePoints`](xref:ARKit.ARFrame.RawFeaturePoints) свойстве. Эффективное Визуализация облачной точки может быть непростой задачей. Проход по точкам, а затем создание и размещение нового узла SceneKit для каждой точки приведет к уничтожению частоты кадров. Кроме того, при асинхронном завершении произойдет задержка. Пример поддерживает производительность с помощью стратегии из трех частей:

- Использование ненадежного кода для закрепления данных на месте и интерпретации данных в виде необработанного буфера байтов.
- Преобразование этого необработанного буфера в [`SCNGeometrySource`](xref:SceneKit.SCNGeometrySource) и создание [`SCNGeometryElement`](xref:SceneKit.SCNGeometryElement) объекта шаблона.
- Быстро «объединяя» необработанные данные и шаблон с помощью [`SCNGeometry.Create(SCNGeometrySource[], SCNGeometryElement[])`](xref:SceneKit.SCNGeometry.Create(SceneKit.SCNGeometrySource[],SceneKit.SCNGeometryElement[]))

```csharp
internal static SCNGeometry CreateVisualization(NVector3[] points, UIColor color, float size)
{
  if (points.Length == 0)
  {
    return null;
  }

  unsafe
  {
    var stride = sizeof(float) * 3;

    // Pin the data down so that it doesn't move
    fixed (NVector3* pPoints = &amp;points[0])
    {
      // Important: Don't unpin until after `SCNGeometry.Create`, because geometry creation is lazy

      // Grab a pointer to the data and treat it as a byte buffer of the appropriate length
      var intPtr = new IntPtr(pPoints);
      var pointData = NSData.FromBytes(intPtr, (System.nuint) (stride * points.Length));

      // Create a geometry source (factory) configured properly for the data (3 vertices)
      var source = SCNGeometrySource.FromData(
        pointData,
        SCNGeometrySourceSemantics.Vertex,
        points.Length,
        true,
        3,
        sizeof(float),
        0,
        stride
      );

      // Create geometry element
      // The null and bytesPerElement = 0 look odd, but this is just a template object
      var template = SCNGeometryElement.FromData(null, SCNGeometryPrimitiveType.Point, points.Length, 0);
      template.PointSize = 0.001F;
      template.MinimumPointScreenSpaceRadius = size;
      template.MaximumPointScreenSpaceRadius = size;

      // Stitch the data (source) together with the template to create the new object
      var pointsGeometry = SCNGeometry.Create(new[] { source }, new[] { template });
      pointsGeometry.Materials = new[] { Utilities.Material(color) };
      return pointsGeometry;
    }
  }
}
```

Результат имеет следующий вид:

![point_cloud](images/arkit_point_cloud.jpeg)

### <a name="complex-gestures"></a>Сложные жесты

Пользователь может масштабировать, вращать и перетаскивать ограничивающий прямоугольник, который окружает целевой объект. В связанных распознавателях есть два интересных вещи.

Во-первых, все распознаватели жестов активируются только после передачи порогового значения; Например, палец переместился на несколько пикселей или поворот превышает некоторый угол. Метод предназначен для накопления перемещения до тех пор, пока не будет превышено пороговое значение, а затем применит его добавочно:

```csharp
// A custom rotation gesture recognizer that fires only when a threshold is passed
internal partial class ThresholdRotationGestureRecognizer : UIRotationGestureRecognizer
{
    // The threshold after which this gesture is detected.
    const double threshold = Math.PI / 15; // (12°)

    // Indicates whether the currently active gesture has exceeded the threshold
    private bool thresholdExceeded = false;

    private double previousRotation = 0;
    internal double RotationDelta { get; private set; }

    internal ThresholdRotationGestureRecognizer(IntPtr handle) : base(handle)
    {
    }

    // Observe when the gesture's state changes to reset the threshold
    public override UIGestureRecognizerState State
    {
        get => base.State;
        set
        {
            base.State = value;

            switch(value)
            {
                case UIGestureRecognizerState.Began :
                case UIGestureRecognizerState.Changed :
                    break;
                default :
                    // Reset threshold check
                    thresholdExceeded = false;
                    previousRotation = 0;
                    RotationDelta = 0;
                    break;
            }
        }
    }

    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);

        if (thresholdExceeded)
        {
            RotationDelta = Rotation - previousRotation;
            previousRotation = Rotation;
        }

        if (! thresholdExceeded && Math.Abs(Rotation) > threshold)
        {
            thresholdExceeded = true;
            previousRotation = Rotation;
        }
    }
}
```

Вторая интересная вещь, связанная с жестами, заключается в способе перемещения ограничивающего прямоугольника по отношению к обнаруженным реальным плоскостям. Этот аспект обсуждается в [этой записи блога Xamarin](https://blog.xamarin.com/exploring-new-ios-12-arkit-capabilities-with-xamarin/).

## <a name="other-new-features-in-arkit-2"></a>Другие новые возможности в ARKit 2

### <a name="more-tracking-configurations"></a>Дополнительные конфигурации отслеживания

Теперь вы можете использовать любой из следующих элементов в качестве основания для работы в смешанной реальности:

- Только акселерометр устройства ( [`AROrientationTrackingConfiguration`](xref:ARKit.AROrientationTrackingConfiguration) iOS 11)
- Лица ( [`ARFaceTrackingConfiguration`](xref:ARKit.ARFaceTrackingConfiguration) , iOS 11)
- Эталонные изображения ( [`ARImageTrackingConfiguration`](xref:ARKit.ARImageTrackingConfiguration) , iOS 12)
- Сканирование объемных объектов ( [`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration) , iOS 12)
- Визуальный инерциый одометри ( [`ARWorldTrackingConfiguration`](xref:ARKit.ARWorldTrackingConfiguration) Улучшенный в iOS 12)

`AROrientationTrackingConfiguration`, обсуждаемые в [этой записи блога и примере F #](https://github.com/lobrien/FSharp_Face_AR), является наиболее ограниченным и предоставляет неплохой опыт работы в смешанной реальности, так как он лишь помещает только цифровые объекты в связи с движением устройства, не пытаясь привязать устройство и экран к реальному миру.

`ARImageTrackingConfiguration`Позволяет распознать реальные 2D-изображения (картин, логотипы и т. д.) и использовать их для привязки цифровых изображений:

```csharp
var imagesAndWidths = new[] {
    ("cover1.jpg", 0.185F),
    ("cover2.jpg", 0.185F),
     //...etc...
    ("cover100.jpg", 0.185F),
};

var referenceImages = new NSSet<ARReferenceImage>(
    imagesAndWidths.Select( imageAndWidth =>
    {
      // Tuples cannot be destructured in lambda arguments
        var (image, width) = imageAndWidth;
        // Read the image
        var img = UIImage.FromFile(image).CGImage;
        return new ARReferenceImage(img, ImageIO.CGImagePropertyOrientation.Up, width);
    }).ToArray());

configuration.TrackingImages = referenceImages;
```

Существует два интересных аспекта этой конфигурации:

- Он эффективен и может использоваться с потенциально большим количеством эталонных образов.
- Цифровая картинка привязывается к изображению, даже если этот образ перемещается в реальном мире (например, если изображение книги распознается, книга будет отслежена по мере ее извлечения, размещения и т. д.).

`ARObjectScanningConfiguration` [Ранее](#recognizing-reference-objects) обсуждалось и является ориентированной на разработчика конфигурацией для сканирования трехмерных объектов. Это сильно ресурсоемкий процессор и аккумулятор, и его не следует использовать в приложениях для конечных пользователей. Пример [сканирования и обнаружения трехмерных объектов](/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects) демонстрирует использование этой конфигурации.

Окончательная конфигурация отслеживания, `ARWorldTrackingConfiguration` , — это основных большинства возможностей смешанной реальности. В этой конфигурации используется "визуальный инерциый одометри" для связи реальных "точек компонентов" с цифровыми изображениями. Цифровые геометрии или спрайты привязаны к реальным горизонтальным и вертикальным плоскостям или по отношению к обнаруженным `ARReferenceObject` экземплярам. В этой конфигурации источник мира — это исходная положение камеры в пространстве с горизонтальной осью Z и цифровыми объектами «Оставайтесь на месте» относительно объектов в реальном мире.

### <a name="environmental-texturing"></a>Текстурирование окружающей среды

ARKit 2 поддерживает «текстурирование окружающей среды», который использует захваченные изображения для оценки освещения и даже применения отраженных светов к объектам. Кубической карты окружающей среды динамически создается, а после того, как камера просмотрела все направления, может создать неблагоприятную реалистичную работу:

![Демонстрационное изображение для текстурирования окружающей среды](images/arkit_env_texturing.png)

Для использования текстурирования окружающей среды:

- [`SCNMaterial`](xref:SceneKit.SCNMaterial)Объекты должны использовать [`SCNLightingModel.PhysicallyBased`](xref:SceneKit.SCNLightingModel.PhysicallyBased) и присвоить значение в диапазоне от 0 до 1 для [`Metalness.Contents`](xref:SceneKit.SCNMaterial.Metalness) и [`Roughness.Contents`](xref:SceneKit.SCNMaterialProperty.Contents) и
- Настройка отслеживания должна быть задана [`EnvironmentTexturing`](xref:ARKit.ARWorldTrackingConfiguration.EnvironmentTexturing)  =  [`AREnvironmentTexturing.Automatic`](xref:ARKit.AREnvironmentTexturing.Automatic) :

```csharp
var sphere = SCNSphere.Create(0.33F);
sphere.FirstMaterial.LightingModelName = SCNLightingModel.PhysicallyBased;
// Shiny metallic sphere
sphere.FirstMaterial.Metalness.Contents = new NSNumber(1.0F);
sphere.FirstMaterial.Roughness.Contents = new NSNumber(0.0F);

// Session configuration:
var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic
};
```

Хотя вполне отражающаяся текстура, показанная в предыдущем фрагменте кода, является приятной для примера, текстурирование окружающей среды, вероятно, лучше использовать с ограничения допустим, что он запускает «нестандартные» ответы (текстура — это лишь оценка, основанная на том, что записывает камера).

### <a name="shared-and-persistent-ar-experiences"></a>Общие и устойчивые возможности AR

Еще одним основным дополнением к ARKit 2 является [`ARWorldMap`](xref:ARKit.ARWorldMap) класс, который позволяет предоставлять общий доступ или хранить данные для отслеживания мира. Вы получаете текущую карту мира с помощью [`ARSession.GetCurrentWorldMapAsync`](xref:ARKit.ARSession.GetCurrentWorldMapAsync) или [`GetCurrentWorldMap(Action<ARWorldMap,NSError>)`](xref:ARKit.ARSession.GetCurrentWorldMap(System.Action{ARKit.ARWorldMap,Foundation.NSError})) :

```csharp
// Local storage
var PersistentWorldPath => Environment.GetFolderPath(Environment.SpecialFolder.Personal) + "/arworldmap";

// Later, after scanning the environment thoroughly...
var worldMap = await Session.GetCurrentWorldMapAsync();
if (worldMap != null)
{
    var data = NSKeyedArchiver.ArchivedDataWithRootObject(worldMap, true, out var err);
    if (err != null)
    {
        Console.WriteLine(err);
    }
    File.WriteAllBytes(PersistentWorldPath, data.ToArray());
}
```

Чтобы поделиться или восстановить карту мира, сделайте следующее:

1. Загрузите данные из файла,
2. Разархивируйте его в `ARWorldMap` объект,
3. Используйте это значение в качестве значения для [`ARWorldTrackingConfiguration.InitialWorldMap`](xref:ARKit.ARWorldTrackingConfiguration.InitialWorldMap) Свойства:

```csharp
var data = NSData.FromArray(File.ReadAllBytes(PersistentWorldController.PersistenWorldPath));
var worldMap = (ARWorldMap)NSKeyedUnarchiver.GetUnarchivedObject(typeof(ARWorldMap), data, out var err);

var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic,
    InitialWorldMap = worldMap
};
```

`ARWorldMap`Он содержит только невидимые данные для отслеживания мира и [`ARAnchor`](xref:ARKit.ARAnchor) объекты, но _не_ содержит цифровых ресурсов. Чтобы поделиться геометрией или изображениями, необходимо разработать собственную стратегию, соответствующую Вашему варианту использования (возможно, путем сохранения/передачи только расположения и ориентации геометрии и его применения к статическому `SCNGeometry` или, возможно, путем хранения и передачи сериализованных объектов). Преимущество состоит в том `ARWorldMap` , что активы, которые после их размещения относительно общего ресурса `ARAnchor` , будут постоянно отображаться между устройствами или сеансами.

### <a name="universal-scene-description-file-format"></a>Формат файла описания универсальной сцены

Последняя функция заголовков ARKit 2 — это принятие компанией Apple формата файла [описания универсальной сцены](https://graphics.pixar.com/usd/docs/Introduction-to-USD.html) Пиксар. Этот формат заменяет формат DAE файл Collada в качестве предпочтительного формата для совместного использования и хранения ресурсов ARKit. Поддержка визуализации ресурсов встроена в iOS 12 и Можаве. Расширение файла УСДЗ является несжатым и незашифрованным zip-архивом, содержащим файлы USD. Пиксар [предоставляет средства для работы с файлами долл. США](https://graphics.pixar.com/usd/docs/USD-Toolset.html#USDToolset-usdedit) , но еще не имеет поддержки сторонних производителей.

## <a name="arkit-programming-tips"></a>Советы по программированию ARKit

### <a name="manual-resource-management"></a>Ручное управление ресурсами

В ARKit важно управлять ресурсами вручную. Это позволяет не только обеспечить высокую частоту кадров, _но и избежать_ путаницы при «замораживании экрана». Платформа ARKit является ленивой для предоставления нового кадра камеры ( [`ARSession.CurrentFrame`](xref:ARKit.ARSession.CurrentFrame) . До тех пор пока [`ARFrame`](xref:ARKit.ARFrame) `Dispose()` не будет вызвана текущая ошибка, ARKit не предоставит новый кадр! Это приводит к тому, что видео зависает, даже если остальная часть приложения отвечает. Решение заключается в том, чтобы всегда обращаться к `ARSession.CurrentFrame` `using` блоку или вызывать `Dispose()` его вручную.

Все объекты, производные от `NSObject` , `IDisposable` и `NSObject` реализуют [шаблон Dispose](/dotnet/standard/design-guidelines/dispose-pattern), поэтому обычно следует следовать [этому шаблону для реализации `Dispose` в производном классе](/dotnet/standard/garbage-collection/implementing-dispose).

### <a name="manipulating-transform-matrices"></a>Обработка матриц преобразований

В любом трехмерном приложении вы будете иметь дело с матрицами преобразования 4x4, которые в сжатой области описывают перемещение, поворот и наклон объекта через трехмерное пространство. В SceneKit это [`SCNMatrix4`](xref:SceneKit.SCNMatrix4) объекты.  

[`SCNNode.Transform`](xref:SceneKit.SCNNode.Transform)Свойство возвращает `SCNMatrix4` матрицу преобразования для, которая [`SCNNode`](xref:SceneKit.SCNNode) _является_ `simdfloat4x4` типом строки. Итак, например:

```csharp
var node = new SCNNode { Position = new SCNVector3(2, 3, 4) };  
var xform = node.Transform;
Console.WriteLine(xform);
// Output is: "(1, 0, 0, 0)\n(0, 1, 0, 0)\n(0, 0, 1, 0)\n(2, 3, 4, 1)"
```  

Как видите, это расположение кодируется в первых трех элементах нижней строки.

В Xamarin общий тип для манипулирования матрицами преобразований — это `NVector4` , что по соглашению интерпретируется как основной способ столбца. Это означает, что компонент перевода или позиционирования ожидается в M14, M24, m34, а не M41, M42, M43:

![строка — основной и столбец — основной](images/arkit_row_vs_column.png)

Единообразие в выборе интерпретации матрицы крайне важно для правильного поведения. Так как матрицы трехмерного преобразования представляют собой 4x4, ошибки согласованности не создают никаких исключений во время компиляции или даже исключения времени выполнения — это просто операции, которые будут выполняться неожиданно. Если объекты SceneKit/ARKit, вероятно, были задержаны, пролетают или имеют неправильные матрицы преобразования, это хорошая возможность. Решение является простым: [`NMatrix4.Transpose`](xref:OpenTK.NMatrix4.Transpose*) выполняет на месте транспоситион элементов.

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения — сканирование и обнаружение трехмерных объектов](/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects)
- [Новые возможности в ARKit 2 (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/602/)
- [Основные сведения об отслеживании и обнаружении ARKit (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/610/)
- [Общие сведения о ARKit в Xamarin. iOS](../introduction-to-ios11/arkit/index.md)