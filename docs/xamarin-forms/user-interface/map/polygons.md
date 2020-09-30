---
title: Xamarin.Forms Многоугольники, ломаные и круговые схемы
description: В этой статье объясняется, как создавать многоугольники, ломаные и круги на Xamarin.Forms экземпляре Map.
ms.prod: xamarin
ms.assetid: CDAF0B02-1AA8-4AD6-94A7-ABFC18006A2D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d4b82cc9c04dd711cc99b558475c2e003e557252
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91559783"
---
# <a name="no-locxamarinforms-map-polygons-and-polylines"></a>Xamarin.Forms Многоугольники и ломаные линии карт

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

`Polygon``Polyline`элементы, и `Circle` позволяют выделять определенные области на карте. Объект `Polygon` является полностью замкнутой фигурой, которая может иметь цвет обводки и заливки. А `Polyline` — это строка, которая не полностью охватывает область. A `Circle` выделяет круглую область на карте:

[![Снимок экрана многоугольника и ломаной линии на iOS и Android](polygons-images/polygon-polyline.png "Многоугольник и ломаная линия на карте")](polygons-images/polygon-polyline-large.png#lightbox "Многоугольник и ломаная линия на карте") 
 [ ![Снимок экрана с кругом карт на iOS и Android](polygons-images/circle.png "Окружность на карте")](polygons-images/circle-large.png#lightbox "Окружность на карте")

`Polygon`Классы, `Polyline` и `Circle` являются производными от `MapElement` класса, который предоставляет следующие связываемые свойства:

- `StrokeColor``Color`объект, определяющий цвет линии.
- `StrokeWidth``float`объект, определяющий толщину линии.

`Polygon`Класс определяет дополнительное свойство BIND:

- `FillColor``Color`объект, определяющий цвет фона многоугольника.

Кроме того, `Polygon` классы и `Polyline` определяют `GeoPath` свойство, которое представляет собой список [`Position`](xref:Xamarin.Forms.Maps.Position) объектов, указывающих точки фигуры.

`Circle`Класс определяет следующие привязываемые свойства:

- `Center`[`Position`](xref:Xamarin.Forms.Maps.Position)объект, определяющий центр окружности в широте и долготе.
- `Radius`[`Distance`](xref:Xamarin.Forms.Maps.Distance)объект, определяющий радиус круга в метрах, километрах или милях.
- `FillColor``Color`свойство, определяющее цвет в периметре круга.

> [!NOTE]
> Если `StrokeColor` свойство не задано, штрих будет по умолчанию черным. Если `FillColor` свойство не задано, заливка по умолчанию будет прозрачной. Таким образом, если ни одно из свойств не указано, фигура будет иметь черный контур без заливки.

## <a name="create-a-polygon"></a>Создание многоугольника

`Polygon`Объект можно добавить в карту, создав его экземпляр и добавив его в `MapElements` коллекцию Map. Это можно сделать в XAML следующим образом:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polygon StrokeColor="#FF9900"
                           StrokeWidth="8"
                           FillColor="#88FF9900">
                 <maps:Polygon.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6368678</x:Double>
                             <x:Double>-122.137305</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polygon.Geopath>
             </maps:Polygon>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};

// instantiate a polygon
Polygon polygon = new Polygon
{
    StrokeWidth = 8,
    StrokeColor = Color.FromHex("#1BA1E2"),
    FillColor = Color.FromHex("#881BA1E2"),
    Geopath =
    {
        new Position(47.6368678, -122.137305),
        new Position(47.6368894, -122.134655),
        new Position(47.6359424, -122.134655),
        new Position(47.6359496, -122.1325521),
        new Position(47.6424124, -122.1325199),
        new Position(47.642463,  -122.1338932),
        new Position(47.6406414, -122.1344833),
        new Position(47.6384943, -122.1361248),
        new Position(47.6372943, -122.1376912)
    }
};

// add the polygon to the map's MapElements collection
map.MapElements.Add(polygon);
```

`StrokeColor`Свойства и `StrokeWidth` задаются для настройки контура многоугольника. `FillColor`Значение свойства соответствует `StrokeColor` значению свойства, но имеет заданное альфа-значение, которое делает его прозрачным, позволяя отображать базовую карту через фигуру. `GeoPath`Свойство содержит список `Position` объектов, определяющих географические координаты точек многоугольника. `Polygon`Объект отображается на карте после добавления в `MapElements` коллекцию `Map` .

> [!NOTE]
> Объект `Polygon` является полностью замкнутой фигурой. Первая и последняя точки будут автоматически подключены, если они не совпадают.

## <a name="create-a-polyline"></a>Создание ломаной линии

`Polyline`Объект можно добавить в карту, создав его экземпляр и добавив его в `MapElements` коллекцию Map. Это можно сделать в XAML следующим образом:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polyline StrokeColor="Blue"
                            StrokeWidth="12">
                 <maps:Polyline.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6381401</x:Double>
                             <x:Double>-122.1317367</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polyline.Geopath>
             </maps:Polyline>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
// instantiate a polyline
Polyline polyline = new Polyline
{
    StrokeColor = Color.Blue,
    StrokeWidth = 12,
    Geopath =
    {
        new Position(47.6381401, -122.1317367),
        new Position(47.6381473, -122.1350841),
        new Position(47.6382847, -122.1353094),
        new Position(47.6384582, -122.1354703),
        new Position(47.6401136, -122.1360819),
        new Position(47.6403883, -122.1364681),
        new Position(47.6407426, -122.1377019),
        new Position(47.6412558, -122.1404056),
        new Position(47.6414148, -122.1418647),
        new Position(47.6414654, -122.1432702)
    }
};

// add the polyline to the map's MapElements collection
map.MapElements.Add(polyline);
```

`StrokeColor`Свойства и `StrokeWidth` задаются для настройки линии. `GeoPath`Свойство содержит список `Position` объектов, определяющих географические координаты точек ломаной линии. `Polyline`Объект отображается на карте после добавления в `MapElements` коллекцию `Map` .

## <a name="create-a-circle"></a>Создание круга

`Circle`Объект можно добавить в карту, создав его экземпляр и добавив его в `MapElements` коллекцию Map. Это можно сделать в XAML следующим образом:

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
      <maps:Map>
          <maps:Map.MapElements>
              <maps:Circle StrokeColor="#88FF0000"
                           StrokeWidth="8"
                           FillColor="#88FFC0CB">
                  <maps:Circle.Center>
                      <maps:Position>
                          <x:Arguments>
                              <x:Double>37.79752</x:Double>
                              <x:Double>-122.40183</x:Double>
                          </x:Arguments>
                      </maps:Position>
                  </maps:Circle.Center>
                  <maps:Circle.Radius>
                      <maps:Distance>
                          <x:Arguments>
                              <x:Double>250</x:Double>
                          </x:Arguments>
                      </maps:Distance>
                  </maps:Circle.Radius>
              </maps:Circle>             
          </maps:Map.MapElements>
          ...
      </maps:Map>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map();

// Instantiate a Circle
Circle circle = new Circle
{
    Center = new Position(37.79752, -122.40183),
    Radius = new Distance(250),
    StrokeColor = Color.FromHex("#88FF0000"),
    StrokeWidth = 8,
    FillColor = Color.FromHex("#88FFC0CB")
};

// Add the Circle to the map's MapElements collection
map.MapElements.Add(circle);
```

Расположение объекта `Circle` на карте определяется значениями `Center` `Radius` свойств и. `Center`Свойство определяет центр окружности в широте и долготе, а `Radius` свойство определяет радиус круга в метрах. `StrokeColor`Свойства и `StrokeWidth` задаются для настройки контура круга. `FillColor`Значение свойства определяет цвет в пределах периметра круга. Оба значения цвета задают альфа-канал, позволяя отображать базовую карту через окружность. `Circle`Объект отображается на карте после добавления в `MapElements` коллекцию `Map` .

> [!NOTE]
> `GeographyUtils`Класс имеет `ToCircumferencePositions` метод расширения, который преобразует `Circle` объект (который определяет `Center` `Radius` значения свойств и) в список `Position` объектов, составляющих координаты широты и долготы периметра окружности.

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](/samples/xamarin/xamarin-forms-samples/workingwithmaps)