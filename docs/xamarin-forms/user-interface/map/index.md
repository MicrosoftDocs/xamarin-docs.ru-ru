---
title: Xamarin.FormsТаблица
description: Элемент управления картой отображает карту и требует Xamarin.Forms . Сопоставляет пакет NuGet.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2461ffa8168207e6a57fae005f752be48772a34a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139832"
---
# <a name="xamarinforms-map"></a>Xamarin.FormsТаблица

## <a name="initialization-and-configuration"></a>[Инициализация и настройка](setup.md)

Объект [ Xamarin.Forms . ](https://www.nuget.org/packages/Xamarin.Forms.Maps/)Для использования функций карт в приложении требуется сопоставить пакет NuGet. Кроме того, для доступа к расположению пользователя требуются разрешения на расположение, предоставленные приложению.

## <a name="map-control"></a>[Map Control](map.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)Элемент управления представляет собой кросс-платформенное представление для отображения и аннотирования карт. Он использует собственный элемент управления картой для каждой платформы, обеспечивая быстрый и знакомый интерфейс карт для пользователей.

## <a name="position-and-distance"></a>[Размещение и расстояние](position-distance.md)

[`Position`](xref:Xamarin.Forms.Maps.Position)Структура обычно используется при размещении схемы и ее ПИН-кодов, а также [`Distance`](xref:Xamarin.Forms.Maps.Distance) структуры, которая при необходимости может использоваться при размещении схемы.

## <a name="pins"></a>[Закрепления](pins.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)Элемент управления позволяет помечать расположения [`Pin`](xref:Xamarin.Forms.Maps.Pin) объектами. `Pin`— Это маркер на карте, который открывает информационное окно при касании.

## <a name="polygons-polylines-and-circles"></a>[Многоугольники, ломаные линии и круги](polygons.md)

`Polygon``Polyline`элементы, и `Circle` позволяют выделять определенные области на карте. Объект `Polygon` является полностью замкнутой фигурой, которая может иметь цвет обводки и заливки. А `Polyline` — это строка, которая не полностью охватывает область. `Circle`Выделяет круглую область на карте.

## <a name="geocoding"></a>[Геокодирование](geocoder.md)

[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)Класс выполняет преобразование между строковыми адресами и координатами широты и долготы, хранящимися в [`Position`](xref:Xamarin.Forms.Maps.Position) объектах.

## <a name="launch-the-native-map-app"></a>[Запуск собственного приложения для работы с картой](native-map-app.md)

Собственное приложение Map на каждой платформе можно запустить из Xamarin.Forms приложения с помощью Xamarin.Essentials `Launcher` класса.
