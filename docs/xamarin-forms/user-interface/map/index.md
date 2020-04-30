---
title: Схема Xamarin. Forms
description: Элемент управления картой отображает карту и требует пакет NuGet Xamarin. Forms. Maps.
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: ffd8f7cc31707d09bb3442c180a867d31afcef0f
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517495"
---
# <a name="xamarinforms-map"></a>Схема Xamarin. Forms

## <a name="initialization-and-configuration"></a>[Инициализация и настройка](setup.md)

Для использования функций карт в приложении требуется пакет NuGet [Xamarin. Forms. Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) . Кроме того, для доступа к расположению пользователя требуются разрешения на расположение, предоставленные приложению.

## <a name="map-control"></a>[Map Control](map.md)

[`Map`](xref:Xamarin.Forms.Maps.Map) Элемент управления представляет собой кросс-платформенное представление для отображения и аннотирования карт. Он использует собственный элемент управления картой для каждой платформы, обеспечивая быстрый и знакомый интерфейс карт для пользователей.

## <a name="position-and-distance"></a>[Расположение и расстояние](position-distance.md)

[`Position`](xref:Xamarin.Forms.Maps.Position) Структура обычно используется при размещении схемы и ее ПИН-кодов, а также [`Distance`](xref:Xamarin.Forms.Maps.Distance) структуры, которая при необходимости может использоваться при размещении схемы.

## <a name="pins"></a>[Контактов](pins.md)

[`Map`](xref:Xamarin.Forms.Maps.Map) Элемент управления позволяет помечать расположения [`Pin`](xref:Xamarin.Forms.Maps.Pin) объектами. `Pin` — Это маркер на карте, который открывает информационное окно при касании.

## <a name="polygons-polylines-and-circles"></a>[Многоугольники, ломаные и круги](polygons.md)

`Polygon`элементы `Polyline`, и `Circle` позволяют выделять определенные области на карте. Объект `Polygon` является полностью замкнутой фигурой, которая может иметь цвет обводки и заливки. А `Polyline` — это строка, которая не полностью охватывает область. `Circle` Выделяет круглую область на карте.

## <a name="geocoding"></a>[Геокодирование](geocoder.md)

[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) Класс выполняет преобразование между строковыми адресами и координатами широты и долготы, хранящимися в [`Position`](xref:Xamarin.Forms.Maps.Position) объектах.

## <a name="launch-the-native-map-app"></a>[Запуск собственного приложения для карт](native-map-app.md)

Собственное приложение Map на каждой платформе можно запустить из приложения Xamarin. Forms с помощью класса Xamarin. Essentials `Launcher` .
