---
Title: " Xamarin.Forms Map Control" Описание: "элемент управления" карта "представляет собой кросс-платформенное представление для отображения и аннотирования карт. Он использует собственный элемент управления картой для каждой платформы, обеспечивая быстрый и знакомый интерфейс карт для пользователей. "
MS. произв. Xamarin MS. AssetID: 22C99029-0B16-43A6-BF58-26B48C4AED38 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 10/29/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-map-control"></a>Xamarin.FormsMap Control

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Map`](xref:Xamarin.Forms.Maps.Map)Элемент управления представляет собой кросс-платформенное представление для отображения и аннотирования карт. Он использует собственный элемент управления картой для каждой платформы, обеспечивая быстрый и знакомый интерфейс карт для пользователей:

[![Снимок экрана элемента управления картой в iOS и Android](map-images/map-default.png "Элемент управления картой")](map-images/map-default-large.png#lightbox "Элемент управления картой")

[`Map`](xref:Xamarin.Forms.Maps.Map)Класс определяет следующие свойства, которые управляют внешним видом и поведением карт:

- [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)Тип `bool` — указывает, отображается ли на карте текущее расположение пользователя.
- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)Тип `IEnumerable` , который указывает коллекцию `IEnumerable` элементов для отображения.
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , который задает объект, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) применяемый к каждому элементу в коллекции отображаемых элементов.
- `ItemTemplateSelector`Тип [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , который указывает [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , который будет использоваться для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) элемента во время выполнения.
- [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)Тип определяет, `bool` разрешена ли на карте прокрутка.
- [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled), тип `bool` , определяет, разрешено ли на карте возможность масштабирования.
- `MapElements`Тип — `IList<MapElement>` представляет список элементов на карте, например многоугольники и ломаные линии.
- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)Тип [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) — указывает стиль отображения для схемы.
- `MoveToLastRegionOnLayoutChange`, тип `bool` , определяет, будет ли передвигаться отображаемая область карт из текущей области в ее ранее заданную область при изменении макета.
- [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)Тип — `IList<Pin>` представляет список ПИН-кодов на карте.
- [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion), типа [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) , возвращает текущую отображаемую область на карте.

Эти свойства, за исключением `MapElements` `Pins` свойств, и `VisibleRegion` , поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целями привязок данных.

[`Map`](xref:Xamarin.Forms.Maps.Map)Класс также определяет `MapClicked` событие, которое возникает при нажатии на карту. `MapClickedEventArgs`Объект, сопровождающий событие, имеет одно свойство с именем `Position` типа [`Position`](xref:Xamarin.Forms.Maps.Position) . При срабатывании события `Position` свойству присваивается расположение, которое было касанием. Сведения о [`Position`](xref:Xamarin.Forms.Maps.Position) структуре см. в разделе [Map Disposition and Distance](position-distance.md).

Дополнительные сведения о [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) свойствах, и `ItemTemplateSelector` см. в разделе [Отображение коллекции закрепления](pins.md#display-a-pin-collection).

## <a name="display-a-map"></a>Отображение схемы

[`Map`](xref:Xamarin.Forms.Maps.Map)Можно отобразить, добавив его в макет или страницу:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <maps:Map x:Name="map" />
</ContentPage>
```

> [!NOTE]
> `xmlns`Для ссылки на необходимо дополнительное определение пространства имен Xamarin.Forms . Элементы управления Maps. В предыдущем примере на `Xamarin.Forms.Maps` пространство имен ссылается `maps` ключевое слово.

Эквивалентный код на C# выглядит так:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Maps;

namespace WorkingWithMaps
{
    public class MapTypesPageCode : ContentPage
    {
        public MapTypesPageCode()
        {
            Map map = new Map();
            Content = map;
        }
    }
}
```

В этом примере вызывается конструктор по умолчанию [`Map`](xref:Xamarin.Forms.Maps.Map) , который выравнивает карту по Рим:

[![Снимок экрана элемента управления Map с расположением по умолчанию в iOS и Android](map-images/map-default.png "Элемент управления Map с расположением по умолчанию")](map-images/map-default-large.png#lightbox "Элемент управления Map с расположением по умолчанию")

Кроме того, [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) аргумент можно передать в [`Map`](xref:Xamarin.Forms.Maps.Map) конструктор, чтобы задать центральную точку и уровень масштабирования для схемы при ее загрузке. Дополнительные сведения см. в разделе [Отображение определенного расположения на карте](#display-a-specific-location-on-a-map).

## <a name="map-types"></a>Типы карт

[`Map.MapType`](xref:Xamarin.Forms.Maps.Map.MapType)Свойство может быть задано для [`MapType`](xref:Xamarin.Forms.Maps.MapType) элемента перечисления, чтобы определить стиль отображения для схемы. Перечисление `MapType` определяет следующие члены:

- `Street`Указывает, что будет отображаться улица.
- `Satellite`Указывает, что будет отображена схема, содержащая сопутствующие изображения.
- `Hybrid`Указывает, что будет отображено сочетание улицы и вспомогательных данных на карте.

По умолчанию [`Map`](xref:Xamarin.Forms.Maps.Map) отображает карту улицы, если [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) свойство не определено. Кроме того, `MapType` свойству может быть присвоено одно из [`MapType`](xref:Xamarin.Forms.Maps.MapType) членов перечисления:

```xaml
<maps:Map MapType="Satellite" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    MapType = MapType.Satellite
};
```

На следующих снимках экрана показано, [`Map`](xref:Xamarin.Forms.Maps.Map) когда [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) свойство имеет значение `Street` .

[![Снимок экрана элемента управления Map с типом схемы улицы в iOS и Android](map-images/maptype-street.png "Сопоставьте элемент управления с маптипе улицы")](map-images/maptype-street-large.png#lightbox "Map Control с типом карт улицы")

На следующих снимках экрана показано, [`Map`](xref:Xamarin.Forms.Maps.Map) когда [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) свойство имеет значение `Satellite` .

[![Снимок экрана элемента управления Map с типом вспомогательной схемы в iOS и Android](map-images/maptype-satellite.png "Отображение элемента управления с помощью вспомогательной маптипе")](map-images/maptype-satellite-large.png#lightbox "Сопоставьте элемент управления с типом вспомогательной схемы")

На следующих снимках экрана показано, [`Map`](xref:Xamarin.Forms.Maps.Map) когда [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType) свойство имеет значение `Hybrid` .

[![Снимок экрана элемента управления Map с типом гибридной схемы в iOS и Android](map-images/maptype-hybrid.png "Управление картой с помощью гибридного маптипе")](map-images/maptype-hybrid-large.png#lightbox "Сопоставьте элемент управления с типом гибридной схемы")

## <a name="display-a-specific-location-on-a-map"></a>Отображение определенного расположения на карте

Область отображения, отображаемая при загрузке схемы, может быть задана путем передачи [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) аргумента в [`Map`](xref:Xamarin.Forms.Maps.Map) конструктор:

```xaml
<maps:Map>
    <x:Arguments>
        <maps:MapSpan>
            <x:Arguments>
                <maps:Position>
                    <x:Arguments>
                        <x:Double>36.9628066</x:Double>
                        <x:Double>-122.0194722</x:Double>
                    </x:Arguments>
                </maps:Position>
                <x:Double>0.01</x:Double>
                <x:Double>0.01</x:Double>
            </x:Arguments>
        </maps:MapSpan>
    </x:Arguments>
</maps:Map>
```

Эквивалентный код на C# выглядит так:

```csharp
Position position = new Position(36.9628066, -122.0194722);
MapSpan mapSpan = new MapSpan(position, 0.01, 0.01);
Map map = new Map(mapSpan);
```

В этом примере создается [`Map`](xref:Xamarin.Forms.Maps.Map) объект, который показывает область, указанную [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) объектом. Объект выравнивается по `MapSpan` центру широты и долготы, представленные [`Position`](xref:Xamarin.Forms.Maps.Position) объектом, и охватывает 0,01 широты и 0,01 долготы. Сведения о [`Position`](xref:Xamarin.Forms.Maps.Position) структуре см. в разделе [Map Disposition and Distance](position-distance.md). Дополнительные сведения о передаче аргументов в XAML см. в разделе [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md).

Результат заключается в том, что при отображении карт она выравнивается по центру в определенном месте и охватывает определенное количество широт и долготы:

[![Снимок экрана элемента управления Map с указанным расположением в iOS и Android](map-images/map-region.png "Элемент управления Map с указанным расположением")](map-images/map-region-large.png#lightbox "Элемент управления Map с указанным расположением")

## <a name="create-a-mapspan-object"></a>Создание объекта Мапспан

Существует ряд подходов к созданию [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) объектов. Распространенный подход — предоставить `MapSpan` конструктору необходимые аргументы. Это Широта и Долгота, представленные [`Position`](xref:Xamarin.Forms.Maps.Position) объектом, и `double` значения, представляющие градусы широты и долготы, которые являются частью `MapSpan` . Сведения о [`Position`](xref:Xamarin.Forms.Maps.Position) структуре см. в разделе [Map Disposition and Distance](position-distance.md).

Кроме того, в классе есть три метода [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) , которые возвращают новые `MapSpan` объекты:

1. [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude*)Возвращает объект `MapSpan` с тем же, `LongitudeDegrees` что и экземпляр класса метода, и радиус, определенный его `north` `south` аргументами и.
1. [`FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius*)Возвращает объект `MapSpan` , определяемый его [`Position`](xref:Xamarin.Forms.Maps.Position) [`Distance`](xref:Xamarin.Forms.Maps.Distance) аргументами и.
1. [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)Возвращает объект `MapSpan` с тем же центром, что и экземпляр класса метода, но с радиусом, умноженным на `double` аргумент.

Сведения о [`Distance`](xref:Xamarin.Forms.Maps.Distance) структуре см. в разделе [Map Disposition and Distance](position-distance.md).

После [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) создания можно получить доступ к следующим свойствам, чтобы получить данные о нем:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center), который представляет объект [`Position`](xref:Xamarin.Forms.Maps.Position) в географическом центре `MapSpan` .
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees), который представляет градусы широты, которые входят в состав `MapSpan` .
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees), представляющий градусы долготы, охваченные `MapSpan` .
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius), представляющий `MapSpan` радиус.

## <a name="move-the-map"></a>Перемещение схемы

[`Map.MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)Метод может быть вызван для изменения расположения и масштаба на карте. Этот метод принимает [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) аргумент, который определяет область отображаемой схемы и ее масштаб.

В следующем коде показан пример перемещения отображаемой области на карте:

```csharp
MapSpan mapSpan = MapSpan.FromCenterAndRadius(position, Distance.FromKilometers(0.444));
map.MoveToRegion(mapSpan);
```

## <a name="zoom-the-map"></a>Изменение масштаба карты

Уровень масштабирования [`Map`](xref:Xamarin.Forms.Maps.Map) можно изменить без изменения его расположения. Это можно сделать с помощью пользовательского интерфейса Map или программным путем, вызвав [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) метод с [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) аргументом, который использует текущее расположение в качестве [`Position`](xref:Xamarin.Forms.Maps.Position) аргумента:

```csharp
double zoomLevel = 0.5;
double latlongDegrees = 360 / (Math.Pow(2, zoomLevel));
if (map.VisibleRegion != null)
{
    map.MoveToRegion(new MapSpan(map.VisibleRegion.Center, latlongDegrees, latlongDegrees));
}
```

В этом примере [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) метод вызывается с [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) аргументом, указывающим текущее расположение Map, через [`Map.VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) свойство и уровень масштабирования в градусах широты и долготы. Общий результат заключается в том, что уровень масштабирования в карте изменяется, но его расположение — нет. Альтернативный подход к реализации масштабирования на карте заключается в использовании [`MapSpan.WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*) метода для управления коэффициентом масштабирования.

> [!IMPORTANT]
> При изменении масштаба схемы (с помощью пользовательского интерфейса Map или программным путем) требуется, чтобы [`Map.HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) свойство было равно `true` . Дополнительные сведения об этом свойстве см. в разделе [Отключение масштабирования](#disable-zoom).

## <a name="customize-map-behavior"></a>Настройка поведения карт

Поведение объекта [`Map`](xref:Xamarin.Forms.Maps.Map) можно настроить, задав некоторые его свойства и обрабатывая `MapClicked` событие.

> [!NOTE]
> Дополнительные настройки поведения карт можно получить, создав пользовательский модуль подготовки к отображению карт. Дополнительные сведения см. [в разделе Настройка Xamarin.Forms схемы](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md).

### <a name="disable-scroll"></a>Отключить прокрутку

[`Map`](xref:Xamarin.Forms.Maps.Map)Класс определяет [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) свойство типа `bool` . По умолчанию это свойство имеет значение `true` , указывающее, что карте разрешена прокрутка. Если для этого свойства задано значение `false` , то на карте не выполняется прокрутка. В следующем примере показано задание этого свойства:

```xaml
<maps:Map HasScrollEnabled="false" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    HasScrollEnabled = false
};
```

### <a name="disable-zoom"></a>Отключить масштаб

[`Map`](xref:Xamarin.Forms.Maps.Map)Класс определяет [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) свойство типа `bool` . По умолчанию это свойство имеет значение `true` , которое указывает на то, что на карте можно выполнить масштаб. Если это свойство имеет значение `false` , карту нельзя изменить. В следующем примере показано задание этого свойства:

```xaml
<maps:Map HasZoomEnabled="false" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    HasZoomEnabled = false
};
```

### <a name="show-the-users-location"></a>Отображение расположения пользователя

[`Map`](xref:Xamarin.Forms.Maps.Map)Класс определяет [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) свойство типа `bool` . По умолчанию это свойство имеет значение `false` , указывающее, что на карте не отображается текущее местоположение пользователя. Если для этого свойства задано значение `true` , то на карте отображается текущее расположение пользователя. В следующем примере показано задание этого свойства:

```xaml
<maps:Map IsShowingUser="true" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    IsShowingUser = true
};
```

> [!IMPORTANT]
> В iOS, Android и универсальная платформа Windows для доступа к расположению пользователя требуются разрешения на расположение, предоставленные приложению. Дополнительные сведения см. в разделе [Конфигурация платформы](setup.md#platform-configuration).

### <a name="maintain-map-region-on-layout-change"></a>Сохранить регион на карте при изменении макета

[`Map`](xref:Xamarin.Forms.Maps.Map)Класс определяет `MoveToLastRegionOnLayoutChange` свойство типа `bool` . По умолчанию это свойство имеет значение `true` , которое указывает, что отображаемая область отображения будет перемещена из текущей области в ее ранее заданную область при изменении макета, например при смене устройства. Если для этого свойства задано значение `false` , отображаемая область отображения останется в центре при изменении макета. В следующем примере показано задание этого свойства:

```xaml
<maps:Map MoveToLastRegionOnLayoutChange="false" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map
{
    MoveToLastRegionOnLayoutChange = false
};
```

### <a name="map-clicks"></a>Кнопки карт

[`Map`](xref:Xamarin.Forms.Maps.Map)Класс определяет `MapClicked` событие, возникающее при нажатии на карту. `MapClickedEventArgs`Объект, сопровождающий событие, имеет одно свойство с именем `Position` типа [`Position`](xref:Xamarin.Forms.Maps.Position) . При срабатывании события `Position` свойству присваивается расположение, которое было касанием. Сведения о [`Position`](xref:Xamarin.Forms.Maps.Position) структуре см. в разделе [Map Disposition and Distance](position-distance.md).

В следующем примере кода показан обработчик событий для `MapClicked` события:

```csharp
void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

В этом примере `OnMapClicked` обработчик событий выводит широту и долготу, представляющую расположение касания. Обработчик событий можно зарегистрировать в `MapClicked` событии следующим образом:

```xaml
<maps:Map MapClicked="OnMapClicked" />
```

Эквивалентный код на C# выглядит так:

```csharp
Map map = new Map();
map.MapClicked += OnMapClicked;
```

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Расположение и расстояние на карте](position-distance.md)
- [Настройка Xamarin.Forms схемы](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md)
