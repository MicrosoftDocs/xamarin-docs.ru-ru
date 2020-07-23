---
title: Карты в Xamarin. iOS
description: В этом документе описывается платформа iOS Мапкит Framework и ее использование с Xamarin. iOS. В нем рассказывается, как добавить карту, изменить ее стиль, панораму и масштаб, работать с расположением пользователя, добавлять заметки, работать с выносками и наложениеми, а также выполнять локальный поиск.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 177701b8b50edea965e97da225265912f1f0c198
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932331"
---
# <a name="maps-in-xamarinios"></a>Карты в Xamarin. iOS

Карты являются распространенной функцией во всех современных мобильных операционных системах. iOS предлагает встроенную поддержку сопоставления с помощью платформы Map Kit Framework. С помощью Map Kit приложения могут легко добавлять насыщенные Интерактивные карты. Эти карты могут быть настроены различными способами, такими как добавление заметок для маркировки расположений на карте и переопределение изображений произвольных фигур. Map Kit даже имеет встроенную поддержку отображения текущего расположения устройства.

## <a name="adding-a-map"></a>Добавление схемы

Добавление схемы к приложению осуществляется путем добавления `MKMapView` экземпляра к иерархии представлений, как показано ниже:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

`MKMapView`— `UIView` подкласс, отображающий карту. Простое добавление схемы с помощью приведенного выше кода создает интерактивную карту:

![Пример схемы](images/00-map.png)

## <a name="map-style"></a>Стиль схемы

`MKMapView`поддерживает 3 различных стиля карт. Чтобы применить стиль схемы, просто присвойте `MapType` свойству значение из `MKMapType` перечисления:

```csharp
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
```

На следующем снимке экрана показаны различные доступные стили карт.

![На этом снимке экрана показаны различные доступные стили карт](images/01-mapstyles.png)

## <a name="panning-and-zooming"></a>Панорамирование и масштаб

`MKMapView`включает поддержку функций для интерактивной работы с картой, таких как:

- Масштаб с помощью жеста сжатия
- Панорамирование с помощью жеста панорамирования

Эти функции можно включить или отключить, просто задав `ZoomEnabled` `ScrollEnabled` Свойства и `MKMapView` экземпляра, где значение по умолчанию — true для обоих. Например, чтобы отобразить статическую карту, просто установите для соответствующих свойств значение false:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Местонахождение пользователей

Помимо взаимодействия с пользователем, `MKMapView` также имеет встроенную поддержку отображения расположения устройства. Это делается с помощью *базовой платформы расположения* . Прежде чем можно будет получить доступ к расположению пользователя, необходимо запросить пользователя. Для этого создайте экземпляр `CLLocationManager` и вызовите `RequestWhenInUseAuthorization` .

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Обратите внимание, что в версиях iOS до 8,0 попытка вызвать `RequestWhenInUseAuthorization` ошибку приведет к ошибке. Обязательно проверьте версию iOS перед выполнением этого вызова, если планируется поддержка версий до 8.

Доступ к расположению пользователя также требует внесения изменений в **info. plist**. Необходимо задать следующие ключи, связанные с данными о расположении:

- **NSLocationWhenInUseUsageDescription** для доступа к расположению пользователя во время его взаимодействия с приложением.
- **NSLocationAlwaysUsageDescription** для доступа приложения к расположению пользователя в фоновом режиме.

Эти ключи можно добавить, открыв **info. plist** и выбрав *Source (источник* ) в нижней части редактора.

Когда вы обновите **info. plist** и предложит пользователю разрешение на доступ к своему расположению, можно отобразить расположение пользователя на карте, задав `ShowsUserLocation` для свойства значение true:

```csharp
map.ShowsUserLocation = true;
```

 ![Предупреждение о доступе к расположению](images/02-location-alert.png)

## <a name="annotations"></a>Заметки

 `MKMapView`также поддерживает отображение изображений, известных как заметки, на карте. Это могут быть пользовательские образы или заданные системой ПИН-коды различных цветов. Например, на следующем снимке экрана показана схема с ПИН-кодом и пользовательским изображением:

 ![На этом снимке экрана показана схема с ПИН-кодом и пользовательским изображением](images/03-annotations.png)

### <a name="adding-an-annotation"></a>Добавление заметки

Сама Аннотация состоит из двух частей:

- `MKAnnotation`Объект, который содержит данные модели о заметке, такие как заголовок и расположение заметки.
- Объект `MKAnnotationView` , который содержит изображение для отображения и, при необходимости, выноску, отображаемую при касании пользователем заметки.

Map Kit использует шаблон делегирования iOS для добавления заметок к сопоставлению, где `Delegate` свойство объекта `MKMapView` задается экземпляру `MKMapViewDelegate` . Это реализация делегата, которая отвечает за возврат `MKAnnotationView` для заметки.

Чтобы добавить заметку, сначала заметка добавляется путем вызова метода `AddAnnotations` в `MKMapView` экземпляре:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Когда расположение заметки становится видимым на карте, `MKMapView` вызывается метод своего делегата, `GetViewForAnnotation` чтобы получить объект `MKAnnotationView` для отображения.

Например, следующий код возвращает предоставленную системой `MKPinAnnotationView` :

```csharp
string pId = "PinAnnotation";

public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
{
    if (annotation is MKUserLocation)
        return null;

    // create pin annotation view
    MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

    if (pinView == null)
        pinView = new MKPinAnnotationView (annotation, pId);

    ((MKPinAnnotationView)pinView).PinColor = MKPinAnnotationColor.Red;
    pinView.CanShowCallout = true;

    return pinView;
}
```

### <a name="reusing-annotations"></a>Повторное использование заметок

Для экономии памяти позволяет составить `MKMapView` представление аннотации в пул для повторного использования, аналогично тому, как ячейки таблицы повторно используются. Получение представления заметок из пула выполняется с помощью вызова `DequeueReusableAnnotation` :

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Отображение выносок

Как упоминалось ранее, Аннотация может содержать выноску. Чтобы отобразить выноску, просто задайте `CanShowCallout` для свойства значение true `MKAnnotationView` . Это приводит к отображению заголовка заметки при касании заметки, как показано ниже.

 ![Отображаемый заголовок заметок](images/04-callout.png)

### <a name="customizing-the-callout"></a>Настройка внешнего вызова

Выноску также можно настроить для отображения левых и правых представлений аксессуаров, как показано ниже:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Этот код приводит к следующей выноски:

 ![Пример выноски](images/05-callout-accessories.png)

Чтобы обрабатывали пользователя, коснувшись нужного аксессуара, просто реализуйте `CalloutAccessoryControlTapped` метод в `MKMapViewDelegate` :

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Наложения

Другой способ слоя графики на карте — использовать наложенные слои. Наложения позволяют рисовать графическое содержимое, которое масштабируется вместе с картой. iOS обеспечивает поддержку нескольких типов наложения, в том числе:

- Многоугольники — обычно используются для выделения некоторой области на карте.
- Ломаные линии — часто отображаются при отображении маршрута.
- Окружности — используется для выделения круглой области на карте.

Кроме того, можно создать пользовательские наложения для отображения произвольных геометрических объектов с детализированным, настроенным кодом рисования. Например, прогноз погоды будет хорошим кандидатом для пользовательского наложения.

#### <a name="adding-an-overlay"></a>Добавление наложения

Как и в случае с примечаниями, добавление наложения состоит из двух частей:

- Создание объекта модели для оверлея и добавление его в `MKMapView` .
- Создание представления для наложения в `MKMapViewDelegate` .

Модель для наложения может быть любым `MKShape` подклассом. Xamarin. iOS включает `MKShape` подклассы для многоугольников, ломаных и кругов, `MKPolygon` через `MKPolyline` классы, и `MKCircle` соответственно.

Например, следующий код используется для добавления `MKCircle` :

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Представление для оверлея — это `MKOverlayView` экземпляр, возвращаемый объектом `GetViewForOverlay` в `MKMapViewDelegate` . Каждый `MKShape` имеет соответствующий объект `MKOverlayView` , который знает, как отобразить данную фигуру. Для этого `MKPolygon` есть `MKPolygonView` . Аналогичным образом `MKPolyline` соответствует `MKPolylineView` , и для `MKCircle` есть `MKCircleView` .

Например, следующий код возвращает `MKCircleView` для `MKCircle` :

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

На карте отображается окружность, как показано ниже.

 ![Круг, отображаемый на карте](images/06-circle-overlay.png)

## <a name="local-search"></a>Локальный поиск

iOS включает в себя API локального поиска с помощью Map Kit, который позволяет выполнять асинхронный поиск интересующих точек в указанном географическом регионе.

Для выполнения локального поиска приложение должно выполнить следующие действия.

1. Создать `MKLocalSearchRequest` объект.
1. Создайте `MKLocalSearch` объект из `MKLocalSearchRequest` .
1. Вызовите `Start` метод для `MKLocalSearch` объекта.
1. Получите `MKLocalSearchResponse` объект в обратном вызове.

Интерфейс API локального поиска не предоставляет пользовательский интерфейс. Для него даже не требуется использовать карту. Однако для использования локального поиска приложение должно предоставить какой-либо способ указать поисковый запрос и отобразить результаты. Кроме того, поскольку результаты будут содержать данные о расположении, часто имеет смысл отобразить их на карте.

<a name="Adding_a_Local_Search_UI"></a>

### <a name="adding-a-local-search-ui"></a>Добавление пользовательского интерфейса локального поиска

Одним из способов принятия входных данных поиска является с `UISearchBar` , который предоставляется `UISearchController` и отображает результаты в таблице.

Следующий код добавляет объект `UISearchController` (который имеет свойство панели поиска) в `ViewDidLoad` методе `MapViewController` :

```csharp
//Creates an instance of a custom View Controller that holds the results
var searchResultsController = new SearchResultsViewController (map);

//Creates a search controller updater
var searchUpdater = new SearchResultsUpdator ();
searchUpdater.UpdateSearchResults += searchResultsController.Search;

//add the search controller
searchController = new UISearchController (searchResultsController) {
                SearchResultsUpdater = searchUpdater
            };

//format the search bar
searchController.SearchBar.SizeToFit ();
searchController.SearchBar.SearchBarStyle = UISearchBarStyle.Minimal;
searchController.SearchBar.Placeholder = "Enter a search query";

//the search bar is contained in the navigation bar, so it should be visible
searchController.HidesNavigationBarDuringPresentation = false;

//Ensure the searchResultsController is presented in the current View Controller
DefinesPresentationContext = true;

//Set the search bar in the navigation bar
NavigationItem.TitleView = searchController.SearchBar;
```

Обратите внимание, что вы несете ответственность за включение объекта панели поиска в пользовательский интерфейс. В этом примере мы присвоили его Титлевиев панели навигации, но если вы не используете контроллер навигации в приложении, вам придется найти другое место для его показа.

В этом фрагменте кода мы создали еще один настраиваемый контроллер представления — `searchResultsController` , который отображает результаты поиска, а затем использовал этот объект для создания объекта контроллера поиска. Мы также создали новый поисковый элемент, который станет активным, когда пользователь взаимодействует с панелью поиска. Он получает уведомления о поиске при каждом нажатии клавиши и отвечает за обновление пользовательского интерфейса.
Мы рассмотрим, как реализовать `searchResultsController` и `searchResultsUpdater` Далее в этом руководство.

Это приводит к отображению панели поиска на карте, как показано ниже:

 ![Панель поиска, отображаемая на карте](images/07-searchbar.png)

### <a name="displaying-the-search-results"></a>Отображение результатов поиска

Чтобы отобразить результаты поиска, необходимо создать пользовательский контроллер представления. Обычно `UITableViewController` . Как показано выше, объект `searchResultsController` передается конструктору `searchController` при создании.
В следующем примере кода показано, как создать этот контроллер пользовательского представления.

```csharp
public class SearchResultsViewController : UITableViewController
{
    static readonly string mapItemCellId = "mapItemCellId";
    MKMapView map;

    public List<MKMapItem> MapItems { get; set; }

    public SearchResultsViewController (MKMapView map)
    {
        this.map = map;

        MapItems = new List<MKMapItem> ();
    }

    public override nint RowsInSection (UITableView tableView, nint section)
    {
        return MapItems.Count;
    }

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (mapItemCellId);

        if (cell == null)
            cell = new UITableViewCell ();

        cell.TextLabel.Text = MapItems [indexPath.Row].Name;
        return cell;
    }

    public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
    {
        // add item to map
        CLLocationCoordinate2D coord = MapItems [indexPath.Row].Placemark.Location.Coordinate;
        map.AddAnnotations (new MKPointAnnotation () {
            Title = MapItems [indexPath.Row].Name,
            Coordinate = coord
        });

        map.SetCenterCoordinate (coord, true);

        DismissViewController (false, null);
    }

    public void Search (string forSearchString)
    {
        // create search request
        var searchRequest = new MKLocalSearchRequest ();
        searchRequest.NaturalLanguageQuery = forSearchString;
        searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

        // perform search
        var localSearch = new MKLocalSearch (searchRequest);

        localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
            if (response != null && error == null) {
                this.MapItems = response.MapItems.ToList ();
                this.TableView.ReloadData ();
            } else {
                Console.WriteLine ("local search error: {0}", error);
            }
        });

    }
}
```

### <a name="updating-the-search-results"></a>Обновление результатов поиска

Объект `SearchResultsUpdater` выступает в качестве медиатора между `searchController` панелью поиска и результатами поиска.

В этом примере необходимо сначала создать метод Search в `SearchResultsViewController` . Для этого необходимо создать `MKLocalSearch` объект и использовать его для выполнения поиска. `MKLocalSearchRequest` результаты извлекаются в обратном вызове, переданном `Start` методу `MKLocalSearch` объекта. Затем результаты возвращаются в `MKLocalSearchResponse` объекте, содержащем массив `MKMapItem` объектов:

```csharp
public void Search (string forSearchString)
{
    // create search request
    var searchRequest = new MKLocalSearchRequest ();
    searchRequest.NaturalLanguageQuery = forSearchString;
    searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

    // perform search
    var localSearch = new MKLocalSearch (searchRequest);

    localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
        if (response != null && error == null) {
            this.MapItems = response.MapItems.ToList ();
            this.TableView.ReloadData ();
        } else {
            Console.WriteLine ("local search error: {0}", error);
        }
    });

}
```

Затем, в нашем примере, `MapViewController` мы создадим пользовательскую реализацию `UISearchResultsUpdating` , которая назначается `SearchResultsUpdater` свойству нашего `searchController` в разделе [Добавление локального пользовательского интерфейса поиска](#Adding_a_Local_Search_UI) :

```csharp
public class SearchResultsUpdator : UISearchResultsUpdating
{
    public event Action<string> UpdateSearchResults = delegate {};

    public override void UpdateSearchResultsForSearchController (UISearchController searchController)
    {
        this.UpdateSearchResults (searchController.SearchBar.Text);
    }
}
```

Приведенная выше реализация добавляет к карте заметку при выборе элемента из результатов, как показано ниже:

 ![Заметка, добавленная к сопоставлению при выборе элемента из результатов](images/08-search-results.png)

> [!IMPORTANT]
> `UISearchController`реализован в iOS 8. Если вы хотите поддерживать устройства более ранней версии, вам потребуется использовать `UISearchDisplayController` .

## <a name="summary"></a>Итоги

В этой статье мы рассмотрели платформу *Map* *Kit* для iOS. Во первых, он рассматривал, как `MKMapView` класс разрешает включение интерактивных карт в приложение. Затем демонстрируется дальнейшая настройка карт с помощью заметок и наложений. Наконец, мы рассмотрели возможности локального поиска, добавленные в комплекте Map с iOS 6,1, и покажу, как использовать запросы на основе расположения для интересующих точек и добавлять их на карту.

## <a name="related-links"></a>Связанные ссылки

- [сеарчконтроллер](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [Мапдемо (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/mapdemo)
