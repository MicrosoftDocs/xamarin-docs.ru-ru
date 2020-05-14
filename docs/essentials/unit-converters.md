---
title: Преобразователи единиц в Xamarin.Essentials
description: Класс UnitConverters в Xamarin.Essentials предоставляет несколько преобразователей единиц, чтобы помочь разработчикам при использовании Xamarin.Essentials.
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: 866842cbed9f97dc957e3631c037fa8d27d20076
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149773"
---
# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials: Преобразователи единиц

Класс **UnitConverters** предоставляет несколько преобразователей единиц, чтобы помочь разработчикам при использовании Xamarin.Essentials.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>Использование преобразователей единиц

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Все преобразователи единиц доступны с помощью статического класса `UnitConverters` в Xamarin.Essentials. Например, можно легко преобразовать градусы Фаренгейта в градусы Цельсия.

```csharp
var celsius = UnitConverters.FahrenheitToCelsius(32.0);
```

Ниже приведен список доступных преобразований:

- FahrenheitToCelsius
- CelsiusToFahrenheit
- CelsiusToKelvin
- KelvinToCelsius
- MilesToMeters
- MilesToMeters
- KilometersToMiles
- MetersToInternationalFeet
- InternationalFeetToMeters
- DegreesToRadians
- RadiansToDegrees
- DegreesPerSecondToRadiansPerSecond
- RadiansPerSecondToDegreesPerSecond
- DegreesPerSecondToHertz
- DegreesPerSecondToHertz
- HertzToDegreesPerSecond
- HertzToDegreesPerSecond
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- AtmospheresToPascals
- PascalsToAtmospheres
- CoordinatesToMiles
- CoordinatesToMiles
- KilogramsToPounds
- PoundsToKilograms
- StonesToPounds
- PoundsToStones

## <a name="api"></a>API

- [Исходный код преобразователей единиц](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [Документация по API преобразователей единиц](xref:Xamarin.Essentials.UnitConverters)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Unit-Conversion-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
