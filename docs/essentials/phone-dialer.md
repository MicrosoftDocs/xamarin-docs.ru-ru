---
title: Xamarin.Essentials. Телефон
description: Класс PhoneDialer в Xamarin.Essentials позволяет приложению открывать номер телефона в набирателе номера.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 07/02/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9bd281a61fd53ef3f6d0d3d2307f78a218f33cf4
ms.sourcegitcommit: db423d51356cf5a2dfa1b3925204797b1baf3cd9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92734780"
---
# <a name="no-locxamarinessentials-phone-dialer"></a>Xamarin.Essentials. Телефон

Класс **PhoneDialer** позволяет приложению открывать номер телефона в набирателе номера.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

# <a name="android"></a>[Android](#tab/android)

Если целевой версией Android для проекта является **Android 11 (API R 30)** , необходимо обновить манифест Android с помощью запросов, которые используются с новыми [требованиями к видимости пакета](https://developer.android.com/preview/privacy/package-visibility).

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest** :

```xml
<queries>
  <intent>
    <action android:name="android.intent.action.DIAL" />
    <data android:scheme="tel"/>
  </intent>
</queries>
```

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Различия платформ отсутствуют.

-----

## <a name="using-phone-dialer"></a>Использование PhoneDialer

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Функция PhoneDialer выполняется путем вызова метода `Open` с использованием номера телефона, который нужно открыть в набирателе номера. При запросе команды `Open` API будет автоматически пытаться отформатировать номер на основе кода страны, если он указан.

```csharp
public class PhoneDialerTest
{
    public void PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Исходный код PhoneDialer](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/PhoneDialer)
- [Документация по API PhoneDialer](xref:Xamarin.Essentials.PhoneDialer)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Phone-Dialer-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
