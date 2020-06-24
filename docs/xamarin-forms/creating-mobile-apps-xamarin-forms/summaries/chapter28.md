---
title: Сводка по главе 28. Расположение и карты
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводка по главе 28. Расположение и карты
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 301dc65c7909603e117717a993959e3c73fa2d32
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84133410"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Сводка по главе 28. Расположение и карты

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

> [!NOTE]
> Примечания на этой странице указывают области, в которых Xamarin.Forms имеет расхождения с материалом, представленным в книге.

Xamarin.Forms поддерживает элемент [`Map`](xref:Xamarin.Forms.Maps.Map), который наследует от `View`. Из-за специальных требований платформы, связанных с использованием карт, они реализуются в отдельной сборке **Xamarin.Forms.Maps** и используют другое пространство имен: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Географическая система координат

Географическая система координат определяет позиции на сфероиде (или близком к нему теле), например на нашей планете. Координаты включают угловые значения *широты* и *долготы*.

Большой круг `equator` (экватор) находится ровно посередине между двумя полюсами, через которые проходит воображаемая ось вращения Земли.

### <a name="parallels-and-latitude"></a>Параллели и широта

Угол отклонения от экватора к северу или к югу, измеряемый относительно центра земли, дает линии с одинаковым значением широты, которые называются *параллелями*. Значения широты изменяются в диапазоне от 0 градусов (на экваторе) до 90 градусов на северном и южном полюсах. Отклонение к северу от экватора принято считать положительным, а к югу — отрицательным.

### <a name="longitude-and-meridians"></a>Долгота и меридианы

Половины больших кругов, проведенные от северного до южного полюса, считаются линиями с одинаковым значением долготы и называются *меридианами*. Они отсчитываются от "нулевого меридиана", который проходит через Гринвич (Англия). Отклонения долготы от нулевого меридиана в восточном направлении принято считать положительными (от 0 до 180 градусов), а в западном направлении — отрицательными (также от 0 до &ndash;180 градусов).

### <a name="the-equirectangular-projection"></a>Равнопромежуточная проекция

Любая плоская карта Земли имеет искажения. Если все линии параллелей и меридианов сделать прямыми, а равные различия значений широты и долготы будут соответствовать равным расстояниям на карте, мы получим *равнопромежуточную проекцию*. Такая карта искажает области возле полюсов, сильно растягивая их горизонтально.

### <a name="the-mercator-projection"></a>Проекция Меркатора

В популярной *проекции Меркатора* сделана попытка компенсировать горизонтальное растяжение, одновременно растягивая эти области по вертикали. Это приводит к тому, что на карте все близкие к полюсам области выглядят намного больше, чем на самом деле, но зато для расположенных рядом областей неплохо сохраняются реальные соотношения.

### <a name="map-services-and-tiles"></a>Службы и плитки карт

Службы карт используют разновидность проекции Меркатора, именуемую `Web Mercator`. Службы карт предоставляют клиенту плитки растровых изображений для выбранного расположения и масштаба.

## <a name="getting-the-users-location"></a>Получение расположения пользователя

Классы `Map` в Xamarin.Forms не имеют средств для получения географического расположения пользователя, но это нужная функция для работы с картами, поэтому ее нужно поручить службе зависимостей.

> [!NOTE]
> Вместо этого приложения Xamarin.Forms могут использовать класс [`Geolocation`](~/essentials/geolocation.md), входящий в Xamarin.Essentials.

### <a name="the-location-tracker-api"></a>API отслеживания расположения

Решение [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) содержит код для API отслеживания расположения. Структура [`GeographicLocation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) инкапсулирует широту и долготу. Интерфейс [`ILocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) определяет два метода для запуска и приостановки отслеживания расположения, а также событие, информирующее о доступности нового расположения.

#### <a name="the-ios-location-manager"></a>Диспетчер расположения в iOS

В iOS `ILocationTracker` реализуется через класс [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs), который использует [`CLLocationManager`](xref:CoreLocation.CLLocationManager) платформы iOS.

#### <a name="the-android-location-manager"></a>Диспетчер расположения в Android

В Android `ILocationTracker` реализуется через класс [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs), который использует класс [`LocationManager`](xref:Android.Locations.LocationManager) платформы Android.

#### <a name="the-uwp-geo-locator"></a>Геолокатор UWP

В UWP (универсальная платформа Windows) `ILocationTracker` реализуется через класс [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs), который использует [`Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator) платформы UWP.

### <a name="display-the-phones-location"></a>Отображение расположения телефона

В примере [**WhereAmI**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) по данным средства отслеживания расположения отображается расположения телефона: в текстовой форме и на равнопромежуточной карте.

### <a name="the-required-overhead"></a>Необходимые служебные данные

Чтобы пример **WhereAmI** мог использовать отслеживание расположения, требуются определенные служебные данные. Во-первых, все проекты в решении **WhereAmI** должны иметь ссылки на соответствующие проекты в **Xamarin.FormsBook.Platform**, а каждый проект **WhereAmI** должен вызывать метод `Toolkit.Init`.

Некоторые дополнительные служебные данные, зависящие от платформы, связаны с предоставлением разрешений на отслеживание расположения.

#### <a name="location-permission-for-ios"></a>Разрешения на отслеживание расположения в iOS

На платформе iOS файл **info.plist** должен содержать элементы с текстом вопроса, который предлагает пользователю разрешить получение данных о расположении этого пользователя.

#### <a name="location-permissions-for-android"></a>Разрешения на отслеживание расположения в Android

Приложения Android, которым требуется доступ к данным о расположении пользователя, должны иметь разрешение ACCESS_FILE_LOCATION в файле AndroidManifest.xml.

#### <a name="location-permissions-for-the-uwp"></a>Разрешения на отслеживание расположения в UWP

Приложения UWP должны иметь возможность устройства `location`, отмеченную в файле Package.appxmanifest.

## <a name="working-with-xamarinformsmaps"></a>Работа с Xamarin.Forms.Maps

К использованию класса `Map` применяются сразу несколько требований.

### <a name="the-nuget-package"></a>Пакет NuGet

В решение приложения необходимо добавить библиотеку NuGet **Xamarin.Forms.Maps**. Номер версии должен быть таким же, как у установленного пакета **Xamarin.Forms** .

### <a name="initializing-the-maps-package"></a>Инициализация пакета Maps

Проекты приложения должны вызывать метод `Xamarin.FormsMaps.Init` после вызова `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Включение служб карт

Поскольку `Map` может получать расположение пользователя, приложение должно получить разрешение пользователя, как описано выше в этой главе.

#### <a name="enabling-ios-maps"></a>Включение карт в iOS

Чтобы использовать `Map`, приложение для iOS должно иметь две строки в файле info.plist.

#### <a name="enabling-android-maps"></a>Включение карт в Android

Чтобы использовать Карты Google, требуется ключ авторизации. Этот ключ помещается в файл **AndroidManifest.xml**. Кроме того, файл **AndroidManifest.xml** должен иметь теги `manifest`, которые участвуют в получении расположения пользователя.

#### <a name="enabling-uwp-maps"></a>Включение карт в UWP

Чтобы использовать Карты Bing, приложение UWP должно иметь ключ авторизации. Этот ключ передается в качестве аргумента в метод `Xamarin.FormsMaps.Init`. Кроме того, в приложении должно быть включено использование служб местоположения.

### <a name="the-unadorned-map"></a>Неоформленная карта

Пример [**MapDemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) состоит из файла [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) и файла кода программной части [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs), который позволяет переходить к нескольким демонстрационным программам.

В файле [BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) показано, как отобразить представление [`Map`](xref:Xamarin.Forms.Maps.Map). По умолчанию отображается город Рим, но пользователь может управлять картой.

Чтобы отключить горизонтальную и вертикальную прокрутку, задайте для свойства [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) значение `false`. Чтобы отключить масштабирование, задайте для [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) значение `false`. Эти свойства могут работать не на всех платформах.

### <a name="streets-and-terrain"></a>Улицы и ландшафт

Вы можете отображать разные типы карт, задавая свойство [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) объекта `Map` с типом [`MapType`](xref:Xamarin.Forms.Maps.MapType), которое представляет собой перечисление с тремя элементами:

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street) (стандартный вариант);
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

В файле [MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) показано, как использовать переключатель для выбора типа карты. Здесь используется класс [`RadioButtonManager`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) и класс на основе файла [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml).

### <a name="map-coordinates"></a>Координаты на карте

Программа может узнать текущую область, которую отображает `Map`, с помощью свойства [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion). Это свойство *не* подкрепляется привязываемым свойством и не использует никаких механизмов для извещения об изменении, поэтому для мониторинга этого свойства программе придется применить собственный механизм, например таймер.

`VisibleRegion` имеет тип [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan), это класс с четырьмя свойствами только для чтения:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center) типа [`Position`](xref:Xamarin.Forms.Maps.Position).
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees) с типом `double`, обозначает высоту отображаемой на карте области;
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) с типом `double`, обозначает ширину отображаемой на карте области;
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius) с типом [`Distance`](xref:Xamarin.Forms.Maps.Distance), обозначает размер максимально возможной области круглой формы на отображаемой части карты.

`Position` и `Distance` являются структурами. `Position` определяет два свойства только для чтения, которые можно задать с помощью [конструктора `Position`](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double)).

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` предоставляет независимую от единиц изменения информацию о расстоянии, конвертируя величины между метрической и имперской системами измерения. Значение `Distance` можно создать несколькими способами:

- [конструктор `Distance`](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double)) с расстоянием в метрах;
- статический метод [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double));
- статический метод [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double));
- статический метод [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)).

Это значение можно получить из трех свойств:

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters) типа `double`
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers) типа `double`
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles) типа `double`

Файл [MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) содержит несколько элементов `Label` для отображения информации `MapSpan`. Файл кода программной части [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) использует таймер для обновления этой информации по мере того, как пользователь оперирует картой.

### <a name="position-extensions"></a>Расширения для определения положения

Впервые в этой книге упоминается библиотека [**Xamarin.FormsBook.Toolkit.Maps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps), которая содержит типы, зависящие от реализации карты, но не зависящие от платформы. Класс [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) имеет метод `ToString` для `Position` и другой метод для вычисления расстояния между двумя значениями `Position`.

### <a name="setting-an-initial-location"></a>Настройка исходного положения

Вы можете вызвать метод [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) из `Map`, чтобы программным образом задать расположение и уровень масштабирования на карте. Аргумент имеет тип `MapSpan`. Создать объект `MapSpan` можно двумя способами:

- использовать [`MapSpan` конструктор](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) с `Position` и диапазоном широты и долготы;
- использовать [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) с `Position` и радиусом.

Кроме того, вы можете создать новый тип `MapSpan` из уже существующего, используя методы [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) и [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double)).

Файл [WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) и файл кода программной части [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) демонстрируют применение метода `MoveToRegion` для отображения карты штата Вайоминг.

Для инициализации расположения на карте можно также применить [`Map` конструктор](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) с объектом `MapSpan`. Файл [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) демонстрирует, как выполнить это на чистом XAML на примере карты офиса Xamarin в Сан-Франциско.

### <a name="dynamic-zooming"></a>Динамическое масштабирование

С помощью `Slider` можно динамически масштабировать карту. Файл [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) и файл кода программной части [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) демонстрируют, как изменить радиус карты на основе значения `Slider`.

Файл [LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) и файл кода программной части [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) демонстрируют альтернативный подход, который хорошо подходит для Android. К сожалению, ни один из этих подходов не подходит для платформ Windows.

### <a name="the-phones-location"></a>Расположение телефона

Свойство [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) класса `Map` работает на разных платформах немного по-разному. Это видно из примера [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml).

- В iOS синяя точка обозначает расположение телефона, но к этому месту нужно перемещать карту вручную.
- В Android отображается значок, при нажатии которого карта перемещается к текущему расположению телефона.
- В UWP поведение похоже на iOS, но иногда карта автоматически перемещается к расположению телефона.

Проект **MapDemos** пытается имитировать подход, реализованный в Android, определяя кнопку со значком на основе файла [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) и файла кода программной части [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs).

Файл [GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) и файл кода программной части [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) используют эту кнопку для перехода к расположению телефона.

### <a name="pins-and-science-museums"></a>Маркеры и научные музеи

Напоследок класс `Map` определяет свойство [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins) с типом `IList<Pin>`. Класс [`Pin`](xref:Xamarin.Forms.Maps.Pin) определяет четыре свойства:

- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) с типом `string`, обязательное свойство;
- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) с типом `string`, необязательный адрес в понятной для человека форме;
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) с типом `Position`, отображает расположение маркера на карте;
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) с типом [`PinType`](xref:Xamarin.Forms.Maps.PinType), перечисление, не используется.

Проект **MapDemos** содержит файл [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), который перечисляет все научные музеи США, а также классы [`Locations`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) и [`Site`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) для десериализации этих данных.

Файл [ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) и файл кода программной части [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) отображают на карте маркеры, обозначающие эти научные музеи. Когда пользователь касается маркера, отображается адрес и веб-сайт соответствующего музея.

### <a name="the-distance-between-two-points"></a>Расстояние между двумя точками

Класс [`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) содержит метод [`DistanceTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) с упрощенным методом расчета расстояния между двумя географическими точками.

Это используется в файле [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) и файле кода программной части [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) для отображения расстояния между музеем и текущим расположением пользователя.

[![Три снимка экрана со страницей музеев на местности](images/ch28fg28-small.png "Расстояние до расположения")](images/ch28fg28-large.png#lightbox "Расстояние до расположения")

Также эта программа демонстрирует, как динамически ограничивать количество маркеров в зависимости от расположения на карте.

## <a name="geocoding-and-back-again"></a>Геокодирование и завершение работы

Сборка [ **Xamarin.Forms.Maps**](xref:Xamarin.Forms.Maps) содержит также класс [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) с методом [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)), который преобразует текстовый адрес в нуль или более допустимые географические расположения, и еще один метод [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) для обратного преобразования.

Файл [GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) и файл кода программной части [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) демонстрируют применение этой возможности.

## <a name="related-links"></a>Связанные ссылки

- [Глава 28, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Примеры для главы 28](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Карта Xamarin.Forms](~/xamarin-forms/user-interface/map/index.md)
