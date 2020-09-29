---
title: Новые возможности Мапкит в iOS 11
description: 'В этом документе описываются новые функции Мапкит в iOS 11: группирование маркеров, кнопка компаса, представление масштабирования и кнопка отслеживания пользователей.'
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 8ccafec971a97a05dbf16488974d7a8e8e349708
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437339"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Новые возможности Мапкит в iOS 11

iOS 11 добавляет следующие новые функции в Мапкит:

- [Кластеризация заметок](#clustering)
- [Кнопка компаса](#compass)
- [Масштабное представление](#scale)
- [Кнопка "Отслеживание пользователей"](#user-tracking)

![Схема, показывающая кластеризованные маркеры и кнопку компаса](mapkit-images/cyclemap-heading.png)

<a name="clustering"></a>

## <a name="automatically-grouping-markers-while-zooming"></a>Автоматически группировать маркеры при изменении масштаба

В примере [Мапкит Sample "тандм"](/samples/xamarin/ios-samples/ios11-mapkitsample) показано, как реализовать новую функцию кластеризации заметок iOS 11.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Создание `MKPointAnnotation` подкласса

Класс заметки Point представляет каждый маркер на карте. Их можно добавлять отдельно с помощью `MapView.AddAnnotation()` или из массива с помощью `MapView.AddAnnotations()` .

Классы заметок к точкам не имеют визуального представления, они необходимы только для представления данных, связанных с маркером (самое важное, `Coordinate` свойство, которое является его широтой и долготой на карте), и любые пользовательские свойства:

```csharp
public class Bike : MKPointAnnotation
{
  public BikeType Type { get; set; } = BikeType.Tricycle;
  public Bike(){}
  public Bike(NSNumber lat, NSNumber lgn, NSNumber type)
  {
    Coordinate = new CLLocationCoordinate2D(lat.NFloatValue, lgn.NFloatValue);
    switch(type.NUIntValue) {
      case 0:
        Type = BikeType.Unicycle;
        break;
      case 1:
        Type = BikeType.Tricycle;
        break;
    }
  }
}
```

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Создание `MKMarkerAnnotationView` подкласса для отдельных маркеров

Представление заметок маркера — это визуальное представление каждой заметки, которое имеет стиль с помощью таких свойств, как:

- **Маркертинтколор** — цвет маркера.
- **Глифтекст** — текст, отображаемый в маркере.
- **Глифимаже** — задает изображение, отображаемое в маркере.
- **Дисплайприорити** — определяет z-порядок (режим работы с стеком) при переполнении Map маркерами. Используйте один из `Required` , `DefaultHigh` или `DefaultLow` .

Для поддержки автоматической кластеризации необходимо также задать:

- **Клустерингидентифиер** — определяет, какие маркеры объединяются в кластер. Можно использовать один и тот же идентификатор для всех маркеров или использовать разные идентификаторы для управления способом их группирования.

```csharp
[Register("BikeView")]
public class BikeView : MKMarkerAnnotationView
{
  public static UIColor UnicycleColor = UIColor.FromRGB(254, 122, 36);
  public static UIColor TricycleColor = UIColor.FromRGB(153, 180, 44);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;

      var bike = value as Bike;
      if (bike != null){
        ClusteringIdentifier = "bike";
        switch(bike.Type){
          case BikeType.Unicycle:
            MarkerTintColor = UnicycleColor;
            GlyphImage = UIImage.FromBundle("Unicycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultLow;
            break;
          case BikeType.Tricycle:
            MarkerTintColor = TricycleColor;
            GlyphImage = UIImage.FromBundle("Tricycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
            break;
        }
      }
    }
  }
```

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Создание объекта `MKAnnotationView` для представления кластеров маркеров

Хотя представление заметок, представляющее собой кластер маркеров, _может_ быть простым изображением, пользователи хотят, чтобы приложение представило визуальные подсказки о том, сколько маркеров было сгруппировано.

В [образце кода](/samples/xamarin/ios-samples/ios11-mapkitsample) используется кореграфикс для отображения количества маркеров в кластере, а также представления круговой диаграммы пропорции каждого типа маркера.

Необходимо также задать:

- **Дисплайприорити** — определяет z-порядок (режим работы с стеком) при переполнении Map маркерами. Используйте один из `Required` , `DefaultHigh` или `DefaultLow` .
- **Коллисионмоде** — `Circle` или `Rectangle` .

```csharp
[Register("ClusterView")]
public class ClusterView : MKAnnotationView
{
  public static UIColor ClusterColor = UIColor.FromRGB(202, 150, 38);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;
      var cluster = MKAnnotationWrapperExtensions.UnwrapClusterAnnotation(value);
      if (cluster != null)
      {
        var renderer = new UIGraphicsImageRenderer(new CGSize(40, 40));
        var count = cluster.MemberAnnotations.Length;
        var unicycleCount = CountBikeType(cluster.MemberAnnotations, BikeType.Unicycle);

        Image = renderer.CreateImage((context) => {
          // Fill full circle with tricycle color
          BikeView.TricycleColor.SetFill();
          UIBezierPath.FromOval(new CGRect(0, 0, 40, 40)).Fill();
          // Fill pie with unicycle color
          BikeView.UnicycleColor.SetFill();
          var piePath = new UIBezierPath();
          piePath.AddArc(new CGPoint(20,20), 20, 0, (nfloat)(Math.PI * 2.0 * unicycleCount / count), true);
          piePath.AddLineTo(new CGPoint(20, 20));
          piePath.ClosePath();
          piePath.Fill();
          // Fill inner circle with white color
          UIColor.White.SetFill();
          UIBezierPath.FromOval(new CGRect(8, 8, 24, 24)).Fill();
          // Finally draw count text vertically and horizontally centered
          var attributes = new UIStringAttributes() {
            ForegroundColor = UIColor.Black,
            Font = UIFont.BoldSystemFontOfSize(20)
          };
          var text = new NSString($"{count}");
          var size = text.GetSizeUsingAttributes(attributes);
          var rect = new CGRect(20 - size.Width / 2, 20 - size.Height / 2, size.Width, size.Height);
          text.DrawString(rect, attributes);
        });
      }
    }
  }
  public ClusterView(){}
  public ClusterView(MKAnnotation annotation, string reuseIdentifier) : base(annotation, reuseIdentifier)
  {
    DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
    CollisionMode = MKAnnotationViewCollisionMode.Circle;
    // Offset center point to animate better with marker annotations
    CenterOffset = new CoreGraphics.CGPoint(0, -10);
  }
  private nuint CountBikeType(IMKAnnotation[] members, BikeType type) {
    nuint count = 0;
    foreach(Bike member in members){
      if (member.Type == type) ++count;
    }
    return count;
  }
}
```

### <a name="4-register-the-view-classes"></a>4. регистрация классов представлений

При создании элемента управления "представление" и добавлении его в представление зарегистрируйте типы представлений заметок, чтобы включить автоматическое поведение кластеризации, так как масштабирование и уменьшение размера схемы:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. отрисовка Map!

При подготовке к просмотру на карте маркеры заметки будут кластеризованы или отображены в зависимости от уровня масштаба. При изменении масштаба маркеры отменяются и изменяются в кластерах.

![Симулятор, демонстрирующий кластеризованные маркеры на карте](mapkit-images/cyclemap-sml.png)

Дополнительные сведения о отображении данных с помощью Мапкит см. в [разделе Maps](~/ios/user-interface/controls/ios-maps/index.md) .

<a name="compass"></a>

## <a name="compass-button"></a>Кнопка компаса

в iOS 11 добавлена возможность открыть Компас из схемы и визуализировать ее в любом расположении. Пример см. в примере [приложения тандм](/samples/xamarin/ios-samples/ios11-mapkitsample) .

Создайте кнопку, которая выглядит как компас (включая динамическую анимацию при изменении ориентации на карте) и визуализирует ее в другом элементе управления.

![Кнопка компаса на панели навигации](mapkit-images/compass-sml.png)

Приведенный ниже код создает компасную кнопку и визуализирует ее на панели навигации:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass`Свойство можно использовать для управления видимостью компаса по умолчанию в представлении отображения.

<a name="scale"></a>

## <a name="scale-view"></a>Масштабное представление

Добавьте масштаб в другое место представления с помощью `MKScaleView.FromMapView()` метода, чтобы получить экземпляр масштабируемого представления, чтобы добавить его в другое место в иерархии представлений.

![Масштабированное представление, наложенное на карту](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale`Свойство можно использовать для управления видимостью компаса по умолчанию в представлении отображения.

<a name="user-tracking"></a>

## <a name="user-tracking-button"></a>Кнопка "Отслеживание пользователей"

Кнопка отслеживания пользователей выравнивает карту по текущему расположению пользователя. Используйте `MKUserTrackingButton.FromMapView()` метод для получения экземпляра кнопки, применения изменений форматирования и добавления в другое место в иерархии представлений.

![Кнопка "расположение пользователя", наложенная на карту](mapkit-images/user-location-sml.png)

```csharp
var button = MKUserTrackingButton.FromMapView(MapView);
button.Layer.BackgroundColor = UIColor.FromRGBA(255,255,255,80).CGColor;
button.Layer.BorderColor = UIColor.White.CGColor;
button.Layer.BorderWidth = 1;
button.Layer.CornerRadius = 5;
button.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(button); // constraints omitted for simplicity
```

## <a name="related-links"></a>Связанные ссылки

- [Мапкит пример "Тандм"](/samples/xamarin/ios-samples/ios11-mapkitsample)
- [мккомпассбуттон](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Новые возможности в Мапкит (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/237/)