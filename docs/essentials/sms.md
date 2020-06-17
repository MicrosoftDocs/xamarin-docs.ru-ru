---
title: "Xamarin.Essentials: SMS'' description: "Класс SMS в Xamarin.Essentials позволяет приложению открыть приложение SMS по умолчанию с указанным сообщением, которое нужно отправить получателю".
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58 author: jamesmontemagno ms.custom: video ms.author: jamont ms.date: 04.11.2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-sms"></a>Xamarin.Essentials. SMS

Класс **Sms** позволяет приложению открыть приложение SMS по умолчанию с указанным сообщением, которое нужно отправить получателю.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

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

- [Исходный код Sms](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms).
- [Документация по API Sms](xref:Xamarin.Essentials.Sms).

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/SMS-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
