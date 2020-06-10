---
title: Общие сведения о ARKit в Xamarin. iOS
description: В этом документе описывается дополненная реальность в iOS 11 с ARKit. В нем обсуждается добавление трехмерной модели в приложение, Настройка представления, реализация делегата сеанса, размещение трехмерной модели в мире и приостановка расширенного сеанса реальности.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 51b28ec05af91dea21b1291956de30c549b1868e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571679"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Общие сведения о ARKit в Xamarin. iOS

_Дополненная реальность для iOS 11_

ARKit обеспечивает широкий спектр расширенных приложений и игр в реальности. В этом разделе описываются следующие темы:

- [начало работы с ARKit](#gettingstarted)
- [Использование ARKit с UrhoSharp](urhosharp.md)

<a name="gettingstarted"></a>

## <a name="getting-started-with-arkit"></a>начало работы с ARKit

Чтобы приступить к работе с дополненной реальности, в следующих инструкциях рассматривается простое приложение: размещение трехмерной модели и предоставление ARKit возможности сохранения модели с функциями отслеживания.

![Трехмерная модель Jet с плавающей запятой в образе камеры](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Добавление трехмерной модели

Ресурсы должны быть добавлены в проект с помощью действия сборки **сценекитассет** .

![SceneKit активы в проекте](images/scene-assets.png)

### <a name="2-configure-the-view"></a>2. Настройка представления

В методе контроллера представления `ViewDidLoad` Загрузите ресурс сцены и задайте `Scene` свойство в представлении:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. при необходимости реализуйте делегат сеанса

Хотя это и не является обязательным для простых случаев, реализация делегата сеанса может быть полезной для отладки состояния сеанса ARKit (и в реальных приложениях, предлагая пользователю отзыв). Создайте простой делегат, используя приведенный ниже код.

```csharp
public class SessionDelegate : ARSessionDelegate
{
  public SessionDelegate() {}
  public override void CameraDidChangeTrackingState(ARSession session, ARCamera camera)
  {
    Console.WriteLine("{0} {1}", camera.TrackingState, camera.TrackingStateReason);
  }
}
```

Назначьте делегат в `ViewDidLoad` методе:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. размещение трехмерной модели в мире

В `ViewWillAppear` следующий код устанавливает сеанс ARKit и задает расположение трехмерной модели в пространстве относительно камеры устройства:

```csharp
// Create a session configuration
var configuration = new ARWorldTrackingConfiguration {
  PlaneDetection = ARPlaneDetection.Horizontal,
  LightEstimationEnabled = true
};

// Run the view's session
SceneView.Session.Run(configuration, ARSessionRunOptions.ResetTracking);

// Find the ship and position it just in front of the camera
var ship = SceneView.Scene.RootNode.FindChildNode("ship", true);

ship.Position = new SCNVector3(2f, -2f, -9f);
```

При каждом запуске или возобновлении работы приложения трехмерная модель будет размещена перед камерой. Когда модель будет размещена, переместите камеру и посмотрите, как ARKit сохраняет модель в положении.

### <a name="5-pause-the-augmented-reality-session"></a>5. Приостановка расширенного сеанса реальности

Рекомендуется приостанавливать сеанс ARKit, когда контроллер представления не отображается (в `ViewWillDisappear` методе:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Сводка

Приведенный выше код приводит к попросту ARKit приложению. Более сложные примеры предполагают, что контроллер представления, на котором размещается реализованный сеанс реальности, реализуется `IARSCNViewDelegate` , а также реализуются дополнительные методы.

ARKit предоставляет множество более сложных функций, таких как отслеживание поверхности и взаимодействие с пользователем. Пример объединения отслеживания ARKit с UrhoSharp см. в [демонстрационном примере UrhoSharp](urhosharp.md) .

## <a name="related-links"></a>Связанные ссылки

- [Дополненная реальность (Apple)](https://developer.apple.com/arkit/)
- [Использование ARKit с UrhoSharp](urhosharp.md)
- [Пример простого ARKit (Jet)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitsample)
- [ARKit размещения объектов (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitplacingobjects)
- [Введение в ARKit для iOS (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/602/)
