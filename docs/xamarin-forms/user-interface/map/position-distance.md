---
title: Расположение и расстояние карт Xamarin. Forms
description: Пространство имен Xamarin. Forms. Maps содержит структуру Position, которая обычно используется при размещении карты и ее ПИН-кодов, а также структуры расстояния, которая при необходимости может использоваться при размещении карты.
ms.prod: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
ms.openlocfilehash: 567e1b5620378f0c1b64d17c56c8fb451f18abc3
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517451"
---
# <a name="xamarinforms-map-position-and-distance"></a>Расположение и расстояние карт Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) Пространство имен содержит [`Position`](xref:Xamarin.Forms.Maps.Position) структуру, которая обычно используется при размещении схемы и ее ПИН- [`Distance`](xref:Xamarin.Forms.Maps.Distance) кодов, а также структура, которую можно использовать при размещении схемы.

## <a name="position"></a>Положение

[`Position`](xref:Xamarin.Forms.Maps.Position) Структура инкапсулирует расположение, хранящееся в виде значений широты и долготы. Эта структура определяет два свойства только для чтения:

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)Тип `double`, представляющий широту расположения в десятичных градусах.
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)Тип `double`, представляющий долготу расположения в десятичных градусах.

[`Position`](xref:Xamarin.Forms.Maps.Position)объекты создаются с помощью `Position` конструктора, для которого требуются аргументы широты и долготы `double` , указанные в качестве значений:

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

При создании `Position` объекта значение широты будет относиться между-90,0 и 90,0, а значение долготы будет относиться между-180,0 и 180,0.

> [!NOTE]
> `GeographyUtils` Класс `ToRadians` имеет метод расширения, который `double` преобразует значение из градусов в радианы и метод `ToDegrees` расширения, который преобразует `double` значение из радиан в градусы.

## <a name="distance"></a>Distance

[`Distance`](xref:Xamarin.Forms.Maps.Distance) Структура инкапсулирует расстояние, хранящееся в виде `double` значения, которое представляет расстояние в метрах. Эта структура определяет три свойства только для чтения:

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)Тип `double`, который представляет расстояние в километрах, охваченное `Distance`.
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)Тип `double`, который представляет расстояние в метрах, занимаемое `Distance`.
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)Тип `double`, который представляет расстояние в милях, охваченное `Distance`.

[`Distance`](xref:Xamarin.Forms.Maps.Distance)объекты можно создавать с помощью `Distance` конструктора, для чего требуется аргумент метрах, заданный как `double`:

```csharp
Distance distance = new Distance(1450.5);
```

Кроме того [`Distance`](xref:Xamarin.Forms.Maps.Distance) , объекты можно создавать с помощью [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*)методов [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*), [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*), и `BetweenPositions` фабрик:

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
Distance distance4 = Distance.BetweenPositions(position1, position2);
```

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
