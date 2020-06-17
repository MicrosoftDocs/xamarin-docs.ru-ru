---
title: "Xamarin.Essentials: PhoneDialer" description: "Класс PhoneDialer в Xamarin.Essentials позволяет приложению открывать номер телефона в набирателе номера" ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F author: jamesmontemagno ms.custom: video ms.author: jamont ms.date: 07/02/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials. Телефон

Класс **PhoneDialer** позволяет приложению открывать номер телефона в набирателе номера.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

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

- [Исходный код PhoneDialer](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [Документация по API PhoneDialer](xref:Xamarin.Essentials.PhoneDialer)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Phone-Dialer-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
