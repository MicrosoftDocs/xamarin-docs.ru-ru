---
title: Xamarin.Forms Расположение и расстояние на карте
description: Объект Xamarin.Forms . Пространство имен Maps содержит структуру расположения, которая обычно используется при размещении карты и ее ПИН-кодов, а также структуры расстояния, которая при необходимости может использоваться при размещении карты.
ms.prod: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ccdcfc28e1d439b390459be242b959f53d0bd915
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367716"
---
# <a name="no-locxamarinforms-map-position-and-distance"></a>Xamarin.Forms Расположение и расстояние на карте

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)Пространство имен содержит [`Position`](xref:Xamarin.Forms.Maps.Position) структуру, которая обычно используется при размещении схемы и ее ПИН-кодов, а также [`Distance`](xref:Xamarin.Forms.Maps.Distance) структура, которую можно использовать при размещении схемы.

## <a name="position"></a>Положение

[`Position`](xref:Xamarin.Forms.Maps.Position)Структура инкапсулирует расположение, хранящееся в виде значений широты и долготы. Эта структура определяет два свойства только для чтения:

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)Тип `double` , представляющий широту расположения в десятичных градусах.
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)Тип `double` , представляющий долготу расположения в десятичных градусах.

[`Position`](xref:Xamarin.Forms.Maps.Position) объекты создаются с помощью `Position` конструктора, для которого требуются аргументы широты и долготы, указанные в качестве `double` значений:

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

При создании `Position` объекта значение широты будет относиться между-90,0 и 90,0, а значение долготы будет относиться между-180,0 и 180,0.

> [!NOTE]
> `GeographyUtils`Класс имеет `ToRadians` метод расширения, который преобразует `double` значение из градусов в радианы и `ToDegrees` метод расширения, который преобразует `double` значение из радиан в градусы.

## <a name="distance"></a>расстояние;

[`Distance`](xref:Xamarin.Forms.Maps.Distance)Структура инкапсулирует расстояние `double` , хранящееся в виде значения, которое представляет расстояние в метрах. Эта структура определяет три свойства только для чтения:

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)Тип `double` , который представляет расстояние в километрах, охваченное `Distance` .
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)Тип `double` , который представляет расстояние в метрах, занимаемое `Distance` .
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)Тип `double` , который представляет расстояние в милях, охваченное `Distance` .

[`Distance`](xref:Xamarin.Forms.Maps.Distance) объекты можно создавать с помощью `Distance` конструктора, для чего требуется аргумент метрах, заданный как `double` :

```csharp
Distance distance = new Distance(1450.5);
```

Кроме того, [`Distance`](xref:Xamarin.Forms.Maps.Distance) объекты можно создавать с помощью [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*) методов,, [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*) [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*) и `BetweenPositions` фабрик:

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
Distance distance4 = Distance.BetweenPositions(position1, position2);
```

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](/samples/xamarin/xamarin-forms-samples/workingwithmaps)