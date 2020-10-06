---
title: Xamarin.Essentials. SMS
description: Класс SMS в Xamarin.Essentials позволяет приложению открыть приложение для SMS по умолчанию с указанным сообщением, которое нужно отправить получателю.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d981a7ed2bffbbff12cf69ee4d0cda27ce319040
ms.sourcegitcommit: 3a15d9b29d65139b18dcf0871fe00cffb2a56357
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91353386"
---
# <a name="no-locxamarinessentials-sms"></a>Xamarin.Essentials. SMS

Класс **Sms** позволяет приложению открыть приложение SMS по умолчанию с указанным сообщением, которое нужно отправить получателю.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Для доступа к функции **Sms** нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Если целевой версией Android для проекта является **Android 11 (API R 30)** , необходимо обновить манифест Android с помощью запросов, которые используются с новыми [требованиями к видимости пакета](https://developer.android.com/preview/privacy/package-visibility).

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**:

```xml
<queries>
  <intent>
    <action android:name="android.intent.action.VIEW" />
    <data android:scheme="smsto"/>
  </intent>
</queries>
```

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Различия платформ отсутствуют.

-----

## <a name="using-sms"></a>Использование класса Sms

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Для отправки SMS нужно вызвать метод `ComposeAsync` класса `SmsMessage`, передав получателя и тело сообщения. Оба параметра являются необязательными.

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, new []{ recipient });
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

Дополнительно в метод `SmsMessage` можно передать несколько получателей:

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string[] recipients)
    {
        try
        {
            var message = new SmsMessage(messageText, recipients);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Исходный код Sms](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Sms).
- [Документация по API Sms](xref:Xamarin.Essentials.Sms).

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/SMS-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
