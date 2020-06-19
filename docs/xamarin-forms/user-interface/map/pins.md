---
title: Xamarin.FormsКонтакты карт
description: В этой статье объясняется, как создавать ПИН-коды на Xamarin.Forms карте.
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e22888291a430863b8e45ee21d359a5acec750f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138441"
---
# <a name="xamarinforms-map-pins"></a>Xamarin.FormsКонтакты карт

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Xamarin.Forms [`Map`](xref:Xamarin.Forms.Maps.Map) Элемент управления позволяет помечать расположения [`Pin`](xref:Xamarin.Forms.Maps.Pin) объектами. `Pin`— Это маркер на карте, который открывает информационное окно при касании:

[![Снимок экрана: ПИН-код и информационное окно на устройстве iOS и Android](pins-images/pin-and-information-window.png "Отображение ПИН-кода с помощью окна сведений")](pins-images/pin-and-information-window-large.png#lightbox "Отображение ПИН-кода с помощью окна сведений")

При [`Pin`](xref:Xamarin.Forms.Maps.Pin) добавлении объекта в [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) коллекцию на карте отображается ПИН-код.

[`Pin`](xref:Xamarin.Forms.Maps.Pin)Класс имеет следующие свойства:

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)Тип `string` , который обычно представляет адрес для расположения ПИН-кода. Однако это может быть любое `string` содержимое, а не только адрес.
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)Тип `string` , который обычно представляет заголовок ПИН-кода.
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)Тип [`Position`](xref:Xamarin.Forms.Maps.Position) , который представляет широту и долготу ПИН-кода.
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)Тип [`PinType`](xref:Xamarin.Forms.Maps.PinType) , который представляет тип ПИН-кода.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что `Pin` может быть целевым объектом привязок данных. Дополнительные сведения об объектах привязки данных `Pin` см. [в разделе Отображение коллекции закрепления](#display-a-pin-collection).

Кроме того, [`Pin`](xref:Xamarin.Forms.Maps.Pin) класс определяет `MarkerClicked` и `InfoWindowClicked` события. `MarkerClicked`Событие возникает при касании ПИН-кода, а `InfoWindowClicked` событие возникает при касании информационного окна. `PinClickedEventArgs`Объект, сопровождающий оба события, имеет одно `HideInfoWindow` свойство типа `bool` .

## <a name="display-a-pin"></a>Отображение ПИН-кода

[`Pin`](xref:Xamarin.Forms.Maps.Pin)Можно добавить в [`Map`](xref:Xamarin.Forms.Maps.Map) XAML:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map x:Name="map"
               IsShowingUser="True"
               MoveToLastRegionOnLayoutChange="False">
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
         <maps:Map.Pins>
             <maps:Pin Label="Santa Cruz"
                       Address="The city with a boardwalk"
                       Type="Place">
                 <maps:Pin.Position>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                 </maps:Pin.Position>
             </maps:Pin>
         </maps:Map.Pins>
     </maps:Map>
</ContentPage>
```

Этот XAML создает [`Map`](xref:Xamarin.Forms.Maps.Map) объект, который показывает область, заданную [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) объектом. Объект выравнивается по `MapSpan` центру широты и долготы, представленные [`Position`](xref:Xamarin.Forms.Maps.Position) объектом, который расширяет 0,01 широты и долготы. [`Pin`](xref:Xamarin.Forms.Maps.Pin)Объект добавляется в [`Map.Pins`](xref:Xamarin.Forms.Maps.Pin) коллекцию и рисуется на элементе в `Map` расположении, заданном [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) свойством. Сведения о [`Position`](xref:Xamarin.Forms.Maps.Position) структуре см. в разделе [Map Disposition and Distance](position-distance.md). Сведения о передаче аргументов в XAML в объекты, у которых отсутствуют конструкторы по умолчанию, см. [в разделе Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md).

Эквивалентный код на C# выглядит так:

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
Pin pin = new Pin
{
  Label = "Santa Cruz",
  Address = "The city with a boardwalk",
  Type = PinType.Place,
  Position = new Position(36.9628066, -122.0194722)
};
map.Pins.Add(pin);
```

> [!WARNING]
> [`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label)Если не установить свойство, будет `ArgumentException` выдано исключение при [`Pin`](xref:Xamarin.Forms.Maps.Pin) добавлении в [`Map`](xref:Xamarin.Forms.Maps.Map) .

Этот пример кода приводит к отображению одного ПИН-кода на карте:

[![Снимок экрана с ПИН-кодом для карт на iOS и Android](pins-images/pin-only.png "ПИН-код Map")](pins-images/pin-only-large.png#lightbox "ПИН-код Map")

## <a name="interact-with-a-pin"></a>Взаимодействие с ПИН-кодом

По умолчанию при [`Pin`](xref:Xamarin.Forms.Maps.Pin) касании отображается окно сведений:

[![Снимок экрана: ПИН-код и информационное окно на устройстве iOS и Android](pins-images/pin-and-information-window.png "Отображение ПИН-кода с помощью окна сведений")](pins-images/pin-and-information-window-large.png#lightbox "Отображение ПИН-кода с помощью окна сведений")

Если коснуться на карте, окно сведений закрывается.

[`Pin`](xref:Xamarin.Forms.Maps.Pin)Класс определяет `MarkerClicked` событие, которое срабатывает при `Pin` касании. Нет необходимости в обработке этого события для вывода информационного окна. Вместо этого это событие должно быть обработано при наличии требования, чтобы получать уведомления о касании определенного ПИН-кода.

[`Pin`](xref:Xamarin.Forms.Maps.Pin)Класс также определяет `InfoWindowClicked` событие, которое возникает при касании информационного окна. Это событие должно быть обработано при наличии требования, чтобы получать уведомления о касании конкретного информационного окна.

В следующем коде показан пример обработки этих событий:

```csharp
using Xamarin.Forms.Maps;
// ...
Pin boardwalkPin = new Pin
{
    Position = new Position(36.9641949, -122.0177232),
    Label = "Boardwalk",
    Address = "Santa Cruz",
    Type = PinType.Place
};
boardwalkPin.MarkerClicked += async (s, args) =>
{
    args.HideInfoWindow = true;
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
};

Pin wharfPin = new Pin
{
    Position = new Position(36.9571571, -122.0173544),
    Label = "Wharf",
    Address = "Santa Cruz",
    Type = PinType.Place
};
wharfPin.InfoWindowClicked += async (s, args) =>
{
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
};
```

`PinClickedEventArgs`Объект, сопровождающий оба события, имеет одно `HideInfoWindow` свойство типа `bool` . Если это свойство имеет значение `true` внутри обработчика событий, окно сведений будет скрыто.

## <a name="pin-types"></a>Типы ПИН-кодов

[`Pin`](xref:Xamarin.Forms.Maps.Pin)объекты включают [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) свойство типа [`PinType`](xref:Xamarin.Forms.Maps.PinType) , представляющее тип ПИН-кода. Перечисление `PinType` определяет следующие члены:

- `Generic`представляет универсальный ПИН-код.
- `Place`, представляет ПИН-код для места.
- `SavedPin`, представляет ПИН-код для сохраненного расположения.
- `SearchResult`представляет ПИН-код для результата поиска.

Однако установка [`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type) свойства для любого [`PinType`](xref:Xamarin.Forms.Maps.PinType) члена не изменяет внешний вид отображаемого ПИН-кода. Вместо этого необходимо создать пользовательский модуль подготовки отчетов для настройки внешнего вида ПИН-кода. Дополнительные сведения см. [в разделе Настройка ПИН-кода Map](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md).

## <a name="display-a-pin-collection"></a>Отображение коллекции закрепленных элементов

[`Map`](xref:Xamarin.Forms.Maps.Map)Класс определяет следующие свойства:

- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)Тип `IEnumerable` , который указывает коллекцию `IEnumerable` элементов для отображения.
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)Тип [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , который задает объект, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) применяемый к каждому элементу в коллекции отображаемых элементов.
- `ItemTemplateSelector`Тип [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , который указывает [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , который будет использоваться для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) элемента во время выполнения.

> [!IMPORTANT]
> [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)Свойство имеет приоритет, если `ItemTemplate` `ItemTemplateSelector` заданы свойства и.

[`Map`](xref:Xamarin.Forms.Maps.Map)Можно заполнить ПИН-кодов с помощью привязки данных, чтобы привязать его [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource) свойство к `IEnumerable` коллекции:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)Данные свойства привязываются к `Locations` свойству подключенного ViewModel, который возвращает `ObservableCollection` коллекцию `Location` объектов, которая является пользовательским типом. Каждый `Location` объект определяет `Address` и `Description` свойства типа и `string` `Position` свойства типа [`Position`](xref:Xamarin.Forms.Maps.Position) .

Внешний вид каждого элемента в `IEnumerable` коллекции определяется путем присвоения [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate) свойству значения [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , содержащего [`Pin`](xref:Xamarin.Forms.Maps.Pin) объект, который привязывает данные к соответствующим свойствам.

На следующих снимках экрана показано, как отобразить [`Map`](xref:Xamarin.Forms.Maps.Map) [`Pin`](xref:Xamarin.Forms.Maps.Pin) коллекцию с помощью привязки данных:

[![Снимок экрана с привязками к данным в iOS и Android](pins-images/pins-itemsource.png "Сопоставьте с закрепленными данными")](pins-images/pins-itemsource-large.png#lightbox "Сопоставьте с закрепленными данными")

### <a name="choose-item-appearance-at-runtime"></a>Выбор внешнего вида элемента во время выполнения

Внешний вид каждого элемента в `IEnumerable` коллекции можно выбрать во время выполнения на основе значения элемента, задав `ItemTemplateSelector` для свойства значение [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) .

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <!-- Change the property values, or the properties that are bound to. -->
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

В следующем примере показан `MapItemTemplateSelector` класс:

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

`MapItemTemplateSelector`Класс определяет `DefaultTemplate` Свойства и `XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , для которых установлены разные шаблоны данных. `OnSelectTemplate`Метод возвращает `XamarinTemplate` , который отображает Xamarin в качестве метки при `Pin` касании, если элемент имеет адрес, содержащий "Сан-Франциско". Если у элемента нет адреса, содержащего "Сан Франциско", `OnSelectTemplate` метод возвращает `DefaultTemplate` .

> [!NOTE]
> Вариант использования для этой функции — Привязка свойств вложенных классов [`Pin`](xref:Xamarin.Forms.Maps.Pin) объектов к различным свойствам на основе `Pin` подтипа.

Дополнительные сведения о селекторах шаблонов данных см. [в разделе Создание Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Преобразование пользовательского модуля подготовки отчетов](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md)
- [Создание Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
