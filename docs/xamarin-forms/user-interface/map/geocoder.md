---
title: Xamarin.FormsГеокодирование карт
description: В этой статье объясняется, как выполнять геокод и реверсировать данные карт геокодирования с помощью Xamarin.Forms . Сопоставляет класс геокодирования.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fe099235857f6bd0531539e3aa84e41bf59b50ba
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139871"
---
# <a name="xamarinforms-map-geocoding"></a>Xamarin.FormsГеокодирование карт

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)Пространство имен предоставляет [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) класс, который выполняет преобразование между строковыми адресами и координатами широты и долготы, хранящимися в [`Position`](xref:Xamarin.Forms.Maps.Position) объектах. Дополнительные сведения о [`Position`](xref:Xamarin.Forms.Maps.Position) структуре см. в разделе [Map Disposition and Distance](position-distance.md).

## <a name="geocode-an-address"></a>Геокодирование адреса

Адрес улицы может быть геокодирован в координаты широты и долготы путем создания [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) экземпляра и вызова [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) метода в `Geocoder` экземпляре:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

[`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*)Метод принимает `string` аргумент, представляющий адрес, и асинхронно Возвращает коллекцию [`Position`](xref:Xamarin.Forms.Maps.Position) объектов, которые могут представлять адрес.

## <a name="reverse-geocode-an-address"></a>Обратный геокодировании адреса

Координаты широты и долготы могут быть обратными геокодированы в адрес улицы путем создания [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) экземпляра и вызова [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) метода в `Geocoder` экземпляре:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

[`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*)Метод принимает [`Position`](xref:Xamarin.Forms.Maps.Position) аргумент, состоящий из координат широты и долготы, и асинхронно Возвращает коллекцию строк, представляющих адреса, расположенные рядом с позицией.

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.FormsРасположение и расстояние на карте](position-distance.md)
- [API-интерфейс геокодирования](xref:Xamarin.Forms.Maps.Geocoder)
