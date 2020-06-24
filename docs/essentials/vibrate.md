---
title: Xamarin.Essentials. Вибрация
description: В этом документе описан класс Vibration в Xamarin.Essentials, который позволяет запускать и останавливать вибрацию в течение требуемого времени.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e2ed2dfdca6b6811089d4dbb5d6e90e794c79591
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801807"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials. Вибрация

Класс **Vibration** позволяет запускать и останавливать вибрацию в течение требуемого времени.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Чтобы получить доступ к функции **Vibration**, нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Требуется разрешение Vibrate (Вибрация), которое следует настроить в проекте Android. Для этого можно применить любой из следующих методов:

Откройте файл **AssemblyInfo.cs** в папке **Свойства** и добавьте в него:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

ИЛИ обновите манифест Android:

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

ИЛИ щелкните правой кнопкой мыши проект Android и откройте свойства проекта. В разделе **Манифест Android** найдите область **Требуемые разрешения:** и установите флажок для разрешения **Vibrate** (Вибрация). Это действие автоматически обновляет файл **AndroidManifest.xml**.

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Различия платформ отсутствуют.

-----

## <a name="using-vibration"></a>Использование Vibration

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Возможность Vibration можно запросить для заданного промежутка времени или по умолчанию на 500 миллисекунд.

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

Отмену вибрации устройства можно запросить с помощью метода `Cancel`:

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="platform-differences"></a>Различия платформ

# <a name="android"></a>[Android](#tab/android)

Различия платформ отсутствуют.

# <a name="ios"></a>[iOS](#tab/ios)

- Устройство вибрирует, только если задан параметр Vibrate on ring (Вибрация при вызове).
- Постоянная вибрация в течение 500 миллисекунд.
- Невозможно отменить вибрацию.

# <a name="uwp"></a>[UWP](#tab/uwp)

Различия платформ отсутствуют.

-----

## <a name="api"></a>API

- [Исходный код класса Vibration](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Vibration)
- [Документация по API вибрации](xref:Xamarin.Essentials.Vibration)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Vibration-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
