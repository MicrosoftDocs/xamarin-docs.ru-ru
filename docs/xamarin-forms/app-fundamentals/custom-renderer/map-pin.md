---
title: 'title: "Настройка закрепления карты" description: "В этой статье содержатся сведения о создании настраиваемого отрисовщика для элемента управления Map, который отображает собственную карту с настраиваемыми закреплением и представлением закрепленных данных на каждой платформе".'
description: 'ms.prod: xamarin ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 11/06/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/06/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 06ff88f1d4f272d9b77737d2168418c007afe8bc
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573902"
---
# <a name="customizing-a-map-pin"></a>Настройка закрепления карты

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-pin)

_В этой статье содержатся сведения о создании настраиваемого отрисовщика для элемента управления Map, который отображает собственную карту с настраиваемым закреплением и настраиваемым представлением закрепленных данных на каждой платформе._

Каждое представление Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления. Когда [`Map`](xref:Xamarin.Forms.Maps.Map) отображается в приложении Xamarin.Forms, на платформе iOS создается класс `MapRenderer`, который, в свою очередь, создает собственный элемент управления `MKMapView`. На платформе Android класс `MapRenderer` создает собственный элемент управления `MapView`. На универсальной платформе Windows (UWP) класс `MapRenderer` создает собственный элемент управления `MapControl`. Дополнительные сведения об отрисовщике и классах собственных элементов управления, с которыми сопоставляются элементы управления Xamarin.Forms, см. в статье [Базовые классы и собственные элементы управления отрисовщика](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между классом [`Map`](xref:Xamarin.Forms.Maps.Map) и соответствующими собственными элементами управления, которые его реализуют:

![](map-pin-images/map-classes.png "Relationship Between the Map Control and the Implementing Native Controls")

Процесс отрисовки можно использовать для реализации настроек конкретных платформ путем создания настраиваемого отрисовщика для [`Map`](xref:Xamarin.Forms.Maps.Map) на каждой платформе. Этот процесс выглядит следующим образом:

1. [создание](#creating-the-custom-map) настраиваемой карты Xamarin.Forms;
1. [использование](#consuming-the-custom-map) настраиваемой карты в Xamarin.Forms;
1. [создание](#creating-the-custom-renderer-on-each-platform) настраиваемого отрисовщика для карты на каждой платформе.

Далее мы рассмотрим каждый элемент отдельно в целях реализации отрисовщика `CustomMap`, который отображает собственную карту с настраиваемым закреплением, а также настраиваемое представление закрепленных данных на каждой платформе.

> [!NOTE]
> Перед использованием необходимо инициализировать и настроить [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps). Дополнительные сведения см. на веб-сайте [`Maps Control`](~/xamarin-forms/user-interface/map/index.md).

## <a name="creating-the-custom-map"></a>Создание пользовательской карты

Настраиваемый элемент управления картой можно создать путем создания подклассов класса [`Map`](xref:Xamarin.Forms.Maps.Map), как показано в следующем примере кода:

```csharp
public class CustomMap : Map
{
    public List<CustomPin> CustomPins { get; set; }
}
```

Элемент управления `CustomMap` создается в проекте библиотеки .NET Standard и определяет API для настраиваемой карты. Настраиваемая карта предоставляет свойство `CustomPins`, представляющее коллекцию объектов `CustomPin`, которые будут отрисовываться собственным элементом управления картой на каждой платформе. Класс `CustomPin` показан в следующем примере кода:

```csharp
public class CustomPin : Pin
{
    public string Name { get; set; }
    public string Url { get; set; }
}
```

Этот класс определяет `CustomPin` путем наследования свойств класса [`Pin`](xref:Xamarin.Forms.Maps.Pin) с добавлением свойств `Name` и `Url`.

## <a name="consuming-the-custom-map"></a>Использование пользовательской карты

На элемент управления `CustomMap` можно ссылаться в XAML в проекте библиотеки .NET Standard, объявив пространство имен для его расположения и используя префикс пространства имен в настраиваемом элементе управления картой. В следующем примере кода показано, как элемент управления `CustomMap` может использоваться страницей XAML:

```xaml
<ContentPage ...
                   xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <local:CustomMap x:Name="customMap"
                   MapType="Street" />
</ContentPage>

```

Префикс пространства имен `local` может иметь любое имя. Тем не менее значения `clr-namespace` и `assembly` должны соответствовать данным настраиваемой карты. После объявления пространства имен префикс используется для ссылки на настраиваемую карту.

В следующем примере кода показано, как элемент управления `CustomMap` может использоваться страницей C#:

```csharp
public class MapPageCS : ContentPage
{
    public MapPageCS()
    {
        CustomMap customMap = new CustomMap
        {
            MapType = MapType.Street
        };
        // ...
        Content = customMap;
    }
}
```

Экземпляр `CustomMap` будет использоваться для отображения собственной карты на каждой платформе. Его свойство [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) задает стиль отображения для [`Map`](xref:Xamarin.Forms.Maps.Map). При этом возможные значения определяются в перечислении [`MapType`](xref:Xamarin.Forms.Maps.MapType).

Местоположение на карте и содержащиеся на ней закрепления инициализируются способом, показанным в следующем примере кода:

```csharp
public MapPage()
{
    // ...
    CustomPin pin = new CustomPin
    {
        Type = PinType.Place,
        Position = new Position(37.79752, -122.40183),
        Label = "Xamarin San Francisco Office",
        Address = "394 Pacific Ave, San Francisco CA",
        Name = "Xamarin",
        Url = "http://xamarin.com/about/"
    };
    customMap.CustomPins = new List<CustomPin> { pin };
    customMap.Pins.Add(pin);
    customMap.MoveToRegion(MapSpan.FromCenterAndRadius(new Position(37.79752, -122.40183), Distance.FromMiles(1.0)));
}
```

При инициализации добавляются настраиваемые закрепление и позиции, а затем представление карты позиционируется с помощью метода [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*). При этом положение и масштаб карты изменяются путем создания объекта [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) на основе [`Position`](xref:Xamarin.Forms.Maps.Position) и [`Distance`](xref:Xamarin.Forms.Maps.Distance).

Теперь в каждый проект приложения можно добавить настраиваемый отрисовщик для настройки собственных элементов управления картой.

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского отрисовщика на каждой платформе

Процесс создания класса пользовательского отрисовщика выглядит следующим образом:

1. Создайте подкласс класса `MapRenderer`, который отрисовывает настраиваемую карту.
1. Переопределите метод `OnElementChanged`, который отрисовывает настраиваемую карту, и напишите логику для ее настройки. Этот метод вызывается, когда создается соответствующая настраиваемая карта Xamarin.Forms.
1. Добавьте атрибут `ExportRenderer` в класс настраиваемого отрисовщика, чтобы указать, что он будет использоваться для отрисовки настраиваемой карты Xamarin.Forms. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

> [!NOTE]
> Предоставлять пользовательский отрисовщик в проекте для каждой платформы не обязательно. Если пользовательский отрисовщик не зарегистрирован, будет использоваться отрисовщик по умолчанию для базового класса элемента управления.

На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![](map-pin-images/solution-structure.png "CustomMap Custom Renderer Project Responsibilities")

Настраиваемый элемент управления `CustomMap` отрисовывается с помощью зависящих от платформы классов отрисовщика, которые являются производными от класса `MapRenderer` для каждой платформы. Это приводит к тому, что каждый элемент управления `CustomMap` отрисовывается с помощью зависящих от платформы элементов управления, как показано на следующих снимках экрана:

![](map-pin-images/screenshots.png "CustomMap on each Platform")

Класс `MapRenderer` предоставляет метод `OnElementChanged`, который вызывается при создании настраиваемой карты Xamarin.Forms для отрисовки соответствующего собственного элемента управления. Этот метод принимает параметр `ElementChangedEventArgs`, содержащий свойства `OldElement` и `NewElement`. Эти свойства представляют элемент Xamarin.Forms, к которому *был* присоединен отрисовщик, и элемент Xamarin.Forms, к которому *присоединен* отрисовщик, соответственно. В примере приложения свойство `OldElement` будет иметь значение `null`, а свойство `NewElement` будет содержать ссылку на экземпляр `CustomMap`.

Настройка собственного элемента управления выполняется в переопределенной версии метода `OnElementChanged` в классе отрисовщика для каждой платформы. Типизированную ссылку на собственный элемент управления, используемый на платформе, можно получить через свойство `Control`. Кроме того, ссылку на отрисовываемый элемент управления Xamarin.Forms можно получить с помощью свойства `Element`.

Будьте осторожны при подписке на обработчики событий в методе `OnElementChanged`, как показано в следующем примере кода.

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.View> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null)
  {
      // Unsubscribe from event handlers
  }

  if (e.NewElement != null)
  {
      // Configure the native control and subscribe to event handlers
  }
}
```

Собственный элемент управления следует настраивать и подписывать на обработчики событий только при присоединении настраиваемого отрисовщика к новому элементу Xamarin.Forms. Аналогично, от всех обработчиков событий, на которые были созданы подписки, следует отписываться только при изменении элемента, к которому присоединен отрисовщик. Реализовав этот подход, вы сможете создать пользовательский отрисовщик, на который не влияют утечки памяти.

Каждый класс пользовательского отрисовщика дополняется атрибутом `ExportRenderer`, который регистрирует отрисовщик в Xamarin.Forms. Атрибут принимает два параметра — имя типа отрисовываемого пользовательского элемента управления Xamarin.Forms и имя типа пользовательского отрисовщика. Префикс `assembly` в атрибуте указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматривается реализация класса пользовательского отрисовщика для каждой платформы.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского отрисовщика в iOS

На следующих снимках экрана показана карта до и после настройки:

![](map-pin-images/map-layout-ios.png "Map Control Before and After Customization")

В iOS закрепление называется *заметкой* и может представлять собой настраиваемое изображение или задаваемое системой закрепление различных цветов. В заметках также может отображаться *выноска*, которая выводится на экран при выборе заметки пользователем. В выноске отображаются свойства `Label` и `Address` экземпляра `Pin` с необязательными левым и правым вспомогательными представлениями. На представленном выше снимке экрана левое вспомогательное представление содержит изображение обезьяны, а в правом находится кнопка *Сведения*.

В следующем примере кода показан пользовательский отрисовщик для платформы iOS:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        UIView customPinView;
        List<CustomPin> customPins;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null)
                {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        // ...
    }
}
```

Метод `OnElementChanged` выполняет следующие настройки для экземпляра [`MKMapView`](xref:MapKit.MKMapView), если настраиваемый отрисовщик присоединен к новому элементу Xamarin.Forms:

- Свойство [`GetViewForAnnotation`](xref:MapKit.MKMapView.GetViewForAnnotation*) содержит метод `GetViewForAnnotation`. Этот метод вызывается в том случае, когда [на карте отображается местоположение заметки](#displaying-the-annotation) и используется для настройки заметки перед ее выводом на экран.
- Обработчики событий для событий `CalloutAccessoryControlTapped`, `DidSelectAnnotationView` и `DidDeselectAnnotationView` регистрируются. Эти события вызываются в том случае, если пользователь [касается правого вспомогательного представления в выноске](#tapping-on-the-right-callout-accessory-view), а также когда пользователь [выбирает](#selecting-the-annotation) заметку и [отменяет ее выбор](#deselecting-the-annotation) соответственно. Подписка на эти события отменяется, если изменяется элемент Xamarin.Forms, к которому присоединен отрисовщик.

#### <a name="displaying-the-annotation"></a>Отображение заметки

Метод `GetViewForAnnotation` вызывается, когда на карте отображается местоположение заметки, и используется для настройки заметки перед ее выводом на экран. Заметка состоит из двух частей:

- `MkAnnotation` — содержит заголовок, подзаголовок и местоположение заметки.
- `MkAnnotationView` — содержит изображение, которое представляет заметку, а также может содержать выноску, которая отображается в том случае, если пользователь касается заметки.

Метод `GetViewForAnnotation` принимает объект `IMKAnnotation`, который содержит данные заметки, и возвращает объект `MKAnnotationView`, отображаемый на карте, как показано в следующем примере кода:

```csharp
protected override MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null)
    {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Name);
    if (annotationView == null)
    {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Name);
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Name = customPin.Name;
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

Этот метод позволяет вывести в качестве заметки настраиваемое изображение вместо определяемого системой закрепления, а также отобразить выноску с дополнительным содержимым слева и справа от заголовка и адреса заметки в том случае, если пользователь коснется заметки. Это реализуется следующим образом:

1. Вызывается метод `GetCustomPin`, который возвращает данные настраиваемого закрепления для заметки.
1. В целях экономии памяти представление заметки помещается в пул для повторного использования путем вызова [`DequeueReusableAnnotation`](xref:MapKit.MKMapView.DequeueReusableAnnotation*).
1. Класс `CustomMKAnnotationView` расширяет класс `MKAnnotationView` с использованием свойств `Name` и `Url`, которые соответствуют аналогичным свойствам экземпляра `CustomPin`. Новый экземпляр `CustomMKAnnotationView` создается в том случае, если заметка имеет значение `null`:
    - Свойство `CustomMKAnnotationView.Image` содержит изображение, которое будет представлять заметку на карте.
    - Свойство `CustomMKAnnotationView.CalloutOffset` содержит объект `CGPoint`, который указывает, что выноска будет выровнена по центру относительно заметки.
    - Свойство `CustomMKAnnotationView.LeftCalloutAccessoryView` содержит изображение, которое будет отображаться слева от заголовка и адреса заметки.
    - Свойство `CustomMKAnnotationView.RightCalloutAccessoryView` содержит кнопку *Сведения*, которая будет отображаться справа от заголовка и адреса заметки.
    - Свойство `CustomMKAnnotationView.Name` содержит свойство `CustomPin.Name`, возвращаемое методом `GetCustomPin`. Это позволяет идентифицировать заметку для [дальнейшей настройки выноски](#selecting-the-annotation), если это требуется.
    - Свойство `CustomMKAnnotationView.Url` содержит свойство `CustomPin.Url`, возвращаемое методом `GetCustomPin`. URL-адрес, по которому будет выполнен переход в том случае, если пользователь [коснется кнопки, отображаемой в правом вспомогательном представлении выноски](#tapping-on-the-right-callout-accessory-view).
1. Свойству [`MKAnnotationView.CanShowCallout`](xref:MapKit.MKAnnotationView.CanShowCallout*) присваивается значение `true`, которое задает отображение выноски в том случае, если пользователь коснется заметки.
1. Возвращается заметка, которая будет отображаться на карте.

#### <a name="selecting-the-annotation"></a>Выбор заметки

Когда пользователь касается заметки, вызывается событие `DidSelectAnnotationView`, которое, в свою очередь, выполняет метод `OnDidSelectAnnotationView`:

```csharp
void OnDidSelectAnnotationView(object sender, MKAnnotationViewEventArgs e)
{
    CustomMKAnnotationView customView = e.View as CustomMKAnnotationView;
    customPinView = new UIView();

    if (customView.Name.Equals("Xamarin"))
    {
        customPinView.Frame = new CGRect(0, 0, 200, 84);
        var image = new UIImageView(new CGRect(0, 0, 200, 84));
        image.Image = UIImage.FromFile("xamarin.png");
        customPinView.AddSubview(image);
        customPinView.Center = new CGPoint(0, -(e.View.Frame.Height + 75));
        e.View.AddSubview(customPinView);
    }
}
```

Этот метод расширяет существующую выноску, которая содержит левое и правое вспомогательные представления, добавляя к ней экземпляр `UIView` с изображением логотипа Xamarin при условии, что свойству `Name` заметки присвоено значение `Xamarin`. Это позволяет реализовать сценарии, в которых для разных заметок могут отображаться разные выноски. Экземпляр `UIView` будет отображаться по центру над существующей выноской.

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Касание правого вспомогательного представления выноски

Когда пользователь касается кнопки *Сведения* в правом вспомогательном представлении выноски, вызывается событие `CalloutAccessoryControlTapped`, которое, в свою очередь, выполняет метод `OnCalloutAccessoryControlTapped`:

```csharp
void OnCalloutAccessoryControlTapped(object sender, MKMapViewAccessoryTappedEventArgs e)
{
    CustomMKAnnotationView customView = e.View as CustomMKAnnotationView;
    if (!string.IsNullOrWhiteSpace(customView.Url))
    {
        UIApplication.SharedApplication.OpenUrl(new Foundation.NSUrl(customView.Url));
    }
}
```

Этот метод открывает веб-браузер и выполняет переход по адресу, который хранится в свойстве `CustomMKAnnotationView.Url`. Обратите внимание, что этот адрес определяется при создании коллекции `CustomPin` в проекте библиотеки .NET Standard.

#### <a name="deselecting-the-annotation"></a>Отмена выбора заметки

Если заметка отображается на экране и пользователь касается карты, вызывается событие `DidDeselectAnnotationView`, которое, в свою очередь, выполняет метод `OnDidDeselectAnnotationView`:

```csharp
void OnDidDeselectAnnotationView(object sender, MKAnnotationViewEventArgs e)
{
    if (!e.View.Selected)
    {
        customPinView.RemoveFromSuperview();
        customPinView.Dispose();
        customPinView = null;
    }
}
```

Этот метод гарантирует, что в тех случаях, когда существующая выноска не выбрана, ее дополнительные части (изображение логотипа Xamarin) также скрываются, а соответствующие ресурсы высвобождаются.

Дополнительные сведения о настройке экземпляра `MKMapView` см. в статье [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского отрисовщика в Android

На следующих снимках экрана показана карта до и после настройки:

![](map-pin-images/map-layout-android.png "Map Control Before and After Customization")

В Android закрепление называется *маркером* и может представлять собой настраиваемое изображение или задаваемый системой маркер различных цветов. Маркер может содержать *информационное окно*, которое отображается, когда пользователь касается маркера. В информационном окне отображаются свойства `Label` и `Address` экземпляра `Pin`, однако при необходимости в него можно включить другое содержимое. Тем не менее одновременно может отображаться только одно информационное окно.

В следующем примере кода показан пользовательский отрисовщик для платформы Android:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.Droid
{
    public class CustomMapRenderer : MapRenderer, GoogleMap.IInfoWindowAdapter
    {
        List<CustomPin> customPins;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                NativeMap.InfoWindowClick -= OnInfoWindowClick;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                customPins = formsMap.CustomPins;
            }
        }

        protected override void OnMapReady(GoogleMap map)
        {
            base.OnMapReady(map);

            NativeMap.InfoWindowClick += OnInfoWindowClick;
            NativeMap.SetInfoWindowAdapter(this);
        }
        ...
    }
}
```

Если пользовательский отрисовщик подключен к новому элементу Xamarin.Forms, метод `OnElementChanged` извлекает список пользовательских ПИН-кодов из элемента управления. После получения доступа к экземпляру `GoogleMap` вызывается переопределение `OnMapReady`. Этот метод регистрирует обработчик для события `InfoWindowClick`, которое возникает при [щелчке в информационном окне](#clicking-on-the-info-window). Подписка на это событие отменяется только в том случае, если изменяется элемент, к которому присоединен отрисовщик. Переопределение `OnMapReady` также вызывает метод `SetInfoWindowAdapter`, указывая, что методы настройки информационного окна будут предоставлены экземпляром класса `CustomMapRenderer`.

Класс `CustomMapRenderer` реализует интерфейс `GoogleMap.IInfoWindowAdapter` для [настройки информационного окна](#customizing-the-info-window). Этот интерфейс указывает на необходимость реализации следующих методов:

- `public Android.Views.View GetInfoWindow(Marker marker)` — при вызове этого метода возвращается настраиваемое информационное окно для маркера. Если возвращается значение `null`, будет использоваться метод отрисовки окна по умолчанию. Если возвращается объект `View`, этот объект `View` будет помещен внутрь рамки информационного окна.
- `public Android.Views.View GetInfoContents(Marker marker)` — при вызове этого метода возвращается объект `View` с содержимым информационного окна. Этот метод вызывается только в том случае, если метод `GetInfoWindow` возвращает `null`. Если этот метод возвращает `null`, будет использоваться метод отрисовки содержимого окна по умолчанию.

В этом примере приложения выполняется настройка только содержимого информационного окна, поэтому метод `GetInfoWindow` возвращает `null`.

#### <a name="customizing-the-marker"></a>Настройка маркера

Для настройки значка, который представляет маркер, следует вызвать метод `MarkerOptions.SetIcon`. Для этого можно переопределить метод `CreateMarker`, который вызывается для каждого объекта `Pin`, добавляемого на карту:

```csharp
protected override MarkerOptions CreateMarker(Pin pin)
{
    var marker = new MarkerOptions();
    marker.SetPosition(new LatLng(pin.Position.Latitude, pin.Position.Longitude));
    marker.SetTitle(pin.Label);
    marker.SetSnippet(pin.Address);
    marker.SetIcon(BitmapDescriptorFactory.FromResource(Resource.Drawable.pin));
    return marker;
}
```

Этот метод создает новый экземпляр `MarkerOption` для каждого экземпляра `Pin`. После установки позиции, метки и адреса маркера его значок задается с помощью метода `SetIcon`. Этот метод принимает объект `BitmapDescriptor`, содержащий необходимые для отрисовки значка данные, с классом `BitmapDescriptorFactory`, в котором предоставляются вспомогательные методы, упрощающие создание объекта `BitmapDescriptor`. Дополнительные сведения об использовании класса `BitmapDescriptorFactory` для настройки маркера см. в статье [Настройка маркера](~/android/platform/maps-and-location/maps/maps-api.md).

> [!NOTE]
> При необходимости метод `GetMarkerForPin` можно вызвать в отрисовщике карты, чтобы извлечь объект `Marker` из объекта `Pin`.

#### <a name="customizing-the-info-window"></a>Настройка информационного окна

Когда пользователь касается маркера, выполняется метод `GetInfoContents` при условии, что метод `GetInfoWindow` вернул `null`. Метод `GetInfoContents` показан в следующем примере кода:

```csharp
public Android.Views.View GetInfoContents(Marker marker)
{
    var inflater = Android.App.Application.Context.GetSystemService(Context.LayoutInflaterService) as Android.Views.LayoutInflater;
    if (inflater != null)
    {
        Android.Views.View view;

        var customPin = GetCustomPin(marker);
        if (customPin == null)
        {
            throw new Exception("Custom pin not found");
        }

        if (customPin.Name.Equals("Xamarin"))
        {
            view = inflater.Inflate(Resource.Layout.XamarinMapInfoWindow, null);
        }
        else
        {
            view = inflater.Inflate(Resource.Layout.MapInfoWindow, null);
        }

        var infoTitle = view.FindViewById<TextView>(Resource.Id.InfoWindowTitle);
        var infoSubtitle = view.FindViewById<TextView>(Resource.Id.InfoWindowSubtitle);

        if (infoTitle != null)
        {
            infoTitle.Text = marker.Title;
        }
        if (infoSubtitle != null)
        {
            infoSubtitle.Text = marker.Snippet;
        }

        return view;
    }
    return null;
}
```

Этот метод возвращает объект `View` с содержимым информационного окна. Это реализуется следующим образом:

- Извлекается экземпляр `LayoutInflater`. Он используется для создания экземпляра XML-файла макета в соответствующем объекте `View`.
- Вызывается метод `GetCustomPin`, который возвращает данные настраиваемого закрепления для информационного окна.
- Если свойство `CustomPin.Name` имеет значение `Xamarin`, макет `XamarinMapInfoWindow` увеличивается. В противном случае увеличивается макет `MapInfoWindow`. Это позволяет реализовать сценарии, в которых для разных маркеров могут отображаться разные макеты информационных окон.
- Из увеличенного макета извлекаются ресурсы `InfoWindowTitle` и `InfoWindowSubtitle`, свойствам `Text` которых присваиваются соответствующие данные из экземпляра `Marker`, при условии, что ресурсы не имеют значения `null`.
- Возвращается экземпляр `View`, который будет отображаться на карте.

> [!NOTE]
> Информационное окно не является динамическим объектом `View`. Вместо этого ОС Android будет преобразовывать объект `View` в статическое растровое изображение и выводить на экран его. Это значит, что информационное окно может реагировать на события щелчка, однако не отвечает на события касания или жестов. При этом отдельные элементы управления в информационном окне не могут реагировать на собственные события щелчка.

#### <a name="clicking-on-the-info-window"></a>Щелчок в информационном окне

Когда пользователь делает щелчок в информационном окне, вызывается событие `InfoWindowClick`, которое, в свою очередь, выполняет метод `OnInfoWindowClick`:

```csharp
void OnInfoWindowClick(object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    var customPin = GetCustomPin(e.Marker);
    if (customPin == null)
    {
        throw new Exception("Custom pin not found");
    }

    if (!string.IsNullOrWhiteSpace(customPin.Url))
    {
        var url = Android.Net.Uri.Parse(customPin.Url);
        var intent = new Intent(Intent.ActionView, url);
        intent.AddFlags(ActivityFlags.NewTask);
        Android.App.Application.Context.StartActivity(intent);
    }
}
```

Этот метод открывает веб-браузер и выполняет переход по адресу, который хранится в свойстве `Url` извлеченного экземпляра `CustomPin` для объекта `Marker`. Обратите внимание, что этот адрес определяется при создании коллекции `CustomPin` в проекте библиотеки .NET Standard.

Дополнительные сведения о настройке экземпляра `MapView` см. в статье [API карт](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Создание пользовательского отрисовщика на универсальной платформе Windows

На следующих снимках экрана показана карта до и после настройки:

![](map-pin-images/map-layout-uwp.png "Map Control Before and After Customization")

На универсальной платформе Windows (UWP) закрепление называется *значком карты* и может представлять собой настраиваемое изображение или задаваемое системой изображение по умолчанию. Значок карты может содержать объект `UserControl`, который отображается, когда пользователь касается этого значка. Объект `UserControl` может отображать любое содержимое, в том числе свойства `Label` и `Address` экземпляра `Pin`.

В следующем примере кода показан настраиваемый отрисовщик для платформы UWP:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        MapControl nativeMap;
        List<CustomPin> customPins;
        XamarinMapOverlay mapOverlay;
        bool xamarinOverlayShown = false;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                nativeMap.MapElementClick -= OnMapElementClick;
                nativeMap.Children.Clear();
                mapOverlay = null;
                nativeMap = null;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                nativeMap = Control as MapControl;
                customPins = formsMap.CustomPins;

                nativeMap.Children.Clear();
                nativeMap.MapElementClick += OnMapElementClick;

                foreach (var pin in customPins)
                {
                    var snPosition = new BasicGeoposition { Latitude = pin.Pin.Position.Latitude, Longitude = pin.Pin.Position.Longitude };
                    var snPoint = new Geopoint(snPosition);

                    var mapIcon = new MapIcon();
                    mapIcon.Image = RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///pin.png"));
                    mapIcon.CollisionBehaviorDesired = MapElementCollisionBehavior.RemainVisible;
                    mapIcon.Location = snPoint;
                    mapIcon.NormalizedAnchorPoint = new Windows.Foundation.Point(0.5, 1.0);

                    nativeMap.MapElements.Add(mapIcon);                    
                }
            }
        }
        ...
    }
}
```

Метод `OnElementChanged` выполняет указанные ниже операции, если настраиваемый отрисовщик подключен к новому элементу Xamarin.Forms:

- Этот метод выполняет очистку коллекции `MapControl.Children`, удаляя с карты любые существующие элементы пользовательского интерфейса, после чего регистрирует обработчик событий для события `MapElementClick`. Это событие вызывается, когда пользователь касается элемента `MapElement` на объекте `MapControl` или щелкает его. Подписка на него отменяется только в том случае, если изменяется элемент, к которому присоединен отрисовщик.
- Каждое закрепление в коллекции `customPins` отображается на карте в соответствующем географическом местоположении следующим образом:
  - Местоположение закрепления создается в виде экземпляра `Geopoint`.
  - Создается экземпляр `MapIcon`, представляющий закрепление.
  - Изображение, представляющее объект `MapIcon`, задается с помощью свойства `MapIcon.Image`. Тем не менее изображение значка карты будет видно не во всех случаях, поскольку его могут скрывать другие элементы на карте. В связи с этим свойству `CollisionBehaviorDesired` значка карты присваивается значение `MapElementCollisionBehavior.RemainVisible`, гарантирующее его постоянную видимость.
  - Местоположение объекта `MapIcon` задается с помощью свойства `MapIcon.Location`.
  - В свойстве `MapIcon.NormalizedAnchorPoint` задается примерное местоположение указателя на изображении. Если этому свойству присвоено значение по умолчанию (0,0), которое соответствует левому верхнему углу изображения, при изменении масштаба карты изображение может указывать на другое место.
  - Экземпляр `MapIcon` добавляется в коллекцию `MapControl.MapElements`. Значок карты отображается на объекте `MapControl`.

> [!NOTE]
> Если для нескольких значков карты используется одно и то же изображение, для оптимальной производительности экземпляр `RandomAccessStreamReference` следует объявлять на уровне страницы или приложения.

#### <a name="displaying-the-usercontrol"></a>Отображение объекта UserControl

Когда пользователь касается значка карты, выполняется метод `OnMapElementClick`. Этот метод показан в следующем примере кода:

```csharp
private void OnMapElementClick(MapControl sender, MapElementClickEventArgs args)
{
    var mapIcon = args.MapElements.FirstOrDefault(x => x is MapIcon) as MapIcon;
    if (mapIcon != null)
    {
        if (!xamarinOverlayShown)
        {
            var customPin = GetCustomPin(mapIcon.Location.Position);
            if (customPin == null)
            {
                throw new Exception("Custom pin not found");
            }

            if (customPin.Name.Equals("Xamarin"))
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Position.Latitude, Longitude = customPin.Position.Longitude };
                var snPoint = new Geopoint(snPosition);

                nativeMap.Children.Add(mapOverlay);
                MapControl.SetLocation(mapOverlay, snPoint);
                MapControl.SetNormalizedAnchorPoint(mapOverlay, new Windows.Foundation.Point(0.5, 1.0));
                xamarinOverlayShown = true;
            }
        }
        else
        {
            nativeMap.Children.Remove(mapOverlay);
            xamarinOverlayShown = false;
        }
    }
}
```

Этот метод создает экземпляр `UserControl`, который отображает информацию о закреплении. Это реализуется следующим образом:

- Извлекается экземпляр `MapIcon`.
- Вызывается метод `GetCustomPin`, возвращающий данные настраиваемого закрепления, которые требуется отобразить.
- Создается экземпляр `XamarinMapOverlay` для отображения данных настраиваемого закрепления. Этот класс представляет пользовательский элемент управления.
- Географическое расположение, в котором отображается экземпляр `XamarinMapOverlay` на объекте `MapControl`, создается в виде экземпляра `Geopoint`.
- Экземпляр `XamarinMapOverlay` добавляется в коллекцию `MapControl.Children`. Эта коллекция содержит элементы пользовательского интерфейса XAML, которые будут отображаться на карте.
- Географическое местоположение `XamarinMapOverlay`экземпляра на карте задается посредством вызова метода `SetLocation`.
- Относительное положение экземпляра `XamarinMapOverlay`, которое соответствует указанному местоположению, задается посредством вызова метода `SetNormalizedAnchorPoint`. Благодаря этому гарантируется, что при изменении масштаба карты экземпляр `XamarinMapOverlay` всегда будет отображаться в правильном местоположении.

Также, если на карте уже отображается информация о закреплении, при касании карты экземпляр `XamarinMapOverlay` удаляется из коллекции `MapControl.Children`.

#### <a name="tapping-on-the-information-button"></a>Касание кнопки "Сведения"

Когда пользователь касается кнопки *Сведения* в пользовательском элементе управления `XamarinMapOverlay`, вызывается событие `Tapped`, которое, в свою очередь, вызывает метод `OnInfoButtonTapped`:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Этот метод открывает веб-браузер и выполняет переход по адресу, который хранится в свойстве `Url` экземпляра `CustomPin`. Обратите внимание, что этот адрес определяется при создании коллекции `CustomPin` в проекте библиотеки .NET Standard.

Дополнительные сведения о настройке экземпляра `MapControl` см. в статье [Общие сведения о картах и местоположениях](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) на сайте MSDN.

## <a name="related-links"></a>Связанные ссылки

- [Элемент управления Maps](~/xamarin-forms/user-interface/map/index.md)
- [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)
- [API карт](~/android/platform/maps-and-location/maps/maps-api.md)
- [Настраиваемое закрепление (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-pin)
