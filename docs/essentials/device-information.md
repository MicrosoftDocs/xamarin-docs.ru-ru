---
title: Xamarin.Essentials. Сведения об устройстве
description: В этом документе описывается класс DeviceInfo в Xamarin.Essentials, позволяющий получить сведения об устройстве, на котором выполняется приложение.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 70619097baa2c5f10321835b087f693c4fbac0c4
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802388"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials. Сведения об устройстве

Класс **DeviceInfo** предоставляет сведения об устройстве, в котором выполняется приложение.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-deviceinfo"></a>Использование класса DeviceInfo

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Через API предоставляются следующие сведения:

```csharp
// Device Model (SMG-950U, iPhone10,6)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## <a name="platforms"></a>Платформы

[`DeviceInfo.Platform`](xref:Xamarin.Essentials.DeviceInfo.Platform) устанавливает корреляцию с постоянной строкой, которая сопоставляется с операционной системой. Значения можно проверить с помощью структуры `DevicePlatform`:

- **DevicePlatform.iOS** — iOS
- **DevicePlatform.Android** — Android
- **DevicePlatform.UWP** — UWP
- **DevicePlatform.Unknown** — неизвестно

## <a name="idioms"></a>Идиомы

[`DeviceInfo.Idiom`](xref:Xamarin.Essentials.DeviceInfo.Idiom) коррелирует постоянную строку, сопоставляемую с типом устройства, на котором выполняется приложение. Значения можно проверить с помощью структуры `DeviceIdiom`:

- **DeviceIdiom.Phone** — телефон
- **DeviceIdiom.Tablet** — планшет
- **DeviceIdiom.Desktop** — компьютер
- **DeviceIdiom.TV** — телевизор
- **DeviceIdiom.Watch** — часы
- **DeviceIdiom.Unknown** — неизвестно

## <a name="device-type"></a>Тип устройства

Тип `DeviceInfo.DeviceType` коррелирует перечисление, чтобы определить тип устройства, на котором выполняется приложение (физическое или виртуальное). Виртуальное устройство является симулятором или эмулятором.

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="ios"></a>[iOS](#tab/ios)

iOS не предоставляет API, позволяющий разработчикам получить модель конкретного устройства iOS. Вместо этого возвращается идентификатор оборудования, например _iPhone10,6_, который относится к iPhone X. Сопоставление этих идентификаторов не предоставляется Apple, но их можно найти в неофициальных источниках [The iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (Википедия iPhone) и [Get iOS Model](https://github.com/dannycabrera/Get-iOS-Model) (Получение модели iOS).

--------------

## <a name="api"></a>API

- [Исходный код DeviceInfo](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/DeviceInfo)
- [Документация по API DeviceInfo](xref:Xamarin.Essentials.DeviceInfo).

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Device-Information-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
