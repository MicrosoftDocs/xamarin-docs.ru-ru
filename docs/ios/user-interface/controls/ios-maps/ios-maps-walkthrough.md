---
title: Заметки и наложенные наложения в Xamarin. iOS
description: В этой статье представлено пошаговое пошаговое руководство, в котором показано, как работать с заметками и наложением комплекта Map Kit. В нем показано, как добавить карту в приложение, которое отображает заметку и наложение в расположении конференции Xamarin развиваться 2013.
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 56306b04b57295a3625804bdbbb6a9810675be2b
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436770"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>Заметки и наложенные наложения в Xamarin. iOS

В этом пошаговом руководстве приложение, которое планируется создать, показано ниже:

 [![Пример приложения Мапкит](ios-maps-walkthrough-images/00-map-overlay.png)](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)

Полный код можно найти в [примере пошагового руководства по картам](/samples/xamarin/ios-samples/mapswalkthrough).

Начнем с создания нового **пустого проекта iOS**и присвоив ему соответствующее имя. Начнем с добавления кода в наш контроллер представления для отображения MapView, а затем создадим новые классы для нашего Мапделегате и пользовательские заметки. Для этого выполните следующие действия:

## <a name="viewcontroller"></a>ViewController

1. Добавьте следующие пространства имен в `ViewController` :

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Добавьте `MKMapView` переменную экземпляра в класс вместе с `MapDelegate` экземпляром. Вскоре мы создадим `MapDelegate` :

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. В `LoadView` методе контроллера добавьте `MKMapView` и задайте для `View` Свойства контроллера:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Далее мы добавим код для инициализации Map в методе "ViewDidLoad".

1. В поле `ViewDidLoad` добавить код, чтобы задать тип схемы, показать расположение пользователя и разрешить масштаб и панорамирование:

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;

    ```

1. Затем добавьте код, чтобы центрировать карту и задать ее регион:

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;

    ```

1. Создайте новый экземпляр  `MapDelegate` и присвойте его параметру `Delegate` объекта `MKMapView` . Опять же, мы будем реализовывать `MapDelegate` чуть ниже.

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;
    ```

1. Начиная с iOS 8, следует запрашивать авторизацию пользователя для использования своего расположения, поэтому добавим это в наш пример. Сначала определите `CLLocationManager` переменную уровня класса:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. В `ViewDidLoad` методе мы хотим проверить, использует ли устройство, на котором работает приложение, iOS 8, и, если это приложение будет запрашивать авторизацию при использовании приложения:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
        locationManager.RequestWhenInUseAuthorization ();
    }
    ```

1. Наконец, необходимо изменить файл **info. plist** , чтобы уведомить пользователей о причинах запроса их расположения. В меню **источник** файла **info. plist**добавьте следующий ключ:

    `NSLocationWhenInUseUsageDescription`

    и строка:

    `Maps Walkthrough Docs Sample`.

## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs — класс для пользовательских заметок

1. Мы будем использовать пользовательский класс для заметки с именем `ConferenceAnnotation` . Добавьте в проект следующий класс:

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;

    namespace MapsWalkthrough
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;

            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }

            public override string Title {
                get {
                    return title;
                }
            }

            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController — Добавление заметки и наложенного наложения

1. На `ConferenceAnnotation` месте мы можем добавить его к карте. Вернитесь к `ViewDidLoad` методу `ViewController` , добавьте заметку в координате центра на карте:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter));
    ```

1. Мы также хотим наложить наложение отеля. Добавьте следующий код, чтобы создать `MKPolygon` с использованием координат для указанного Гостиницы, и добавьте его в Map по вызову `AddOverlay` :

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });

    map.AddOverlay (hotelOverlay);
    ```

Это завершает код в `ViewDidLoad` . Теперь нам нужно реализовать наш `MapDelegate` класс для создания представлений заметок и наложений соответственно.

## <a name="mapdelegate"></a>мапделегате

1. Создайте класс с именем `MapDelegate` , наследуемый от `MKMapViewDelegate` и включающий `annotationId` переменную для использования в качестве идентификатора повторного использования для аннотации:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```

    Здесь есть только одна Аннотация, поэтому код повторного использования не является обязательным, но рекомендуется включить его.

1. Реализуйте `GetViewForAnnotation` метод, чтобы возвратить представление для `ConferenceAnnotation` с использованием образа **conference.png** , входящего в состав этого пошагового руководства:

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;

        if (annotation is MKUserLocation)
            return null;

        if (annotation is ConferenceAnnotation) {

            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);

            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);

            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        }

        return annotationView;
    }
    ```

1. Когда пользователь отменяет заметку, нам нужно отобразить изображение, показывающее город в Остине. Добавьте следующие переменные в `MapDelegate` для изображения и представления для его отображения:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. Затем, чтобы отобразить изображение при нажатии заметки, реализуйте `DidSelectAnnotation` метод следующим образом:

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {

            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);

            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. Чтобы скрыть изображение, когда пользователь отменит выделение заметки, коснувшись в любом другом месте на карте, реализуйте `DidDeselectAnnotationView` метод следующим образом:

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {

            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```

    Теперь у нас есть код для заметки на месте. Остается только добавить код в, `MapDelegate` чтобы создать представление для наложения отеля.

1. Добавьте следующую реализацию `GetViewForOverlay` в `MapDelegate` :

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

Запустите приложение. Теперь у нас есть Интерактивная схема с пользовательской заметкой и наложением! Коснитесь заметки, и изображение Остин отобразится, как показано ниже:

 [![Коснитесь заметки, и появится изображение Остине](ios-maps-walkthrough-images/01-map-image.png)](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели, как добавить заметку к карте, а также как добавить наложение для указанного многоугольника. Мы также показали, как добавить поддержку сенсорного ввода в заметку для анимации изображения на карте.

## <a name="related-links"></a>Связанные ссылки

- [Пример пошагового руководства по картам](/samples/xamarin/ios-samples/mapswalkthrough)
- [Пример демонстрационной версии Map](/samples/xamarin/ios-samples/mapdemo)
- [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)