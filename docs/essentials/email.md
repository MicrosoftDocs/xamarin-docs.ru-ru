---
title: Xamarin.Essentials. Адрес эл. почты
description: Класс Email в Xamarin.Essentials позволяет приложению открывать приложение электронной почты по умолчанию с указанной информацией, включая тему, текст и получателей (TO, CC, BCC).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 09/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 059405d4e3219162022b3f8c0208ee5cc4ac2d38
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434538"
---
# <a name="no-locxamarinessentials-email"></a>Xamarin.Essentials. Адрес эл. почты

Класс **Email** позволяет приложению открывать приложение электронной почты по умолчанию с указанной информацией, включая тему, текст и получателей (TO, CC, BCC).

Для доступа к функции **Email** нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Если целевой версией Android для проекта является **Android 11 (API R 30)** , необходимо обновить манифест Android с помощью запросов, которые используются с новыми [требованиями к видимости пакета](https://developer.android.com/preview/privacy/package-visibility).

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**:

```xml
<queries>
  <intent>
    <action android:name="android.intent.action.SENDTO" />
    <data android:scheme="mailto" />
  </intent>
</queries>
```

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Различия платформ отсутствуют.

-----

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

> [!TIP]
> Чтобы использовать API электронной почты в iOS, запустите его на физическом устройстве, в противном случае будет создано исключение.

## <a name="using-email"></a>Использование Email

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Функция Email выполняется путем вызова метода `ComposeAsync` и `EmailMessage`, содержащего сведения о сообщении:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            };
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occurred
        }
    }
}
```

## <a name="file-attachments"></a>Вложения файлов

Эта функция позволяет приложению отправлять файлы по электронной почте в почтовых клиентах на устройстве. Xamarin.Essentials автоматически обнаруживает тип файла (MIME) и запрашивает его добавление в качестве вложения. Почтовые клиенты отличаются друг от друга. Какие-то из них могут поддерживать только файлы с определенными расширениями или не поддерживать файлы вообще.

Ниже приведен пример записи текста на диск и добавления его в качестве вложения электронной почты.

```csharp
var message = new EmailMessage
{
    Subject = "Hello",
    Body = "World",
};

var fn = "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

message.Attachments.Add(new EmailAttachment(file));

await Email.ComposeAsync(message);
```

## <a name="platform-differences"></a>Различия платформ

# <a name="android"></a>[Android](#tab/android)

Не все почтовые клиенты для Android поддерживают `Html`. Так как не существует способа определить это, при отправке писем рекомендуем использовать `PlainText`.

# <a name="ios"></a>[iOS](#tab/ios)

Различия платформ отсутствуют.

# <a name="uwp"></a>[UWP](#tab/uwp)

Поддерживается только `PlainText`, так как если `BodyFormat` попытается отправить `Html`, возникнет исключение `FeatureNotSupportedException`.

Не все почтовые клиенты поддерживают отправку вложений. Дополнительные сведения см. в [документации](/windows/uwp/contacts-and-calendar/sending-email).

-----

## <a name="api"></a>API

- [Исходный код Email](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Email)
- [Документация по API Email](xref:Xamarin.Essentials.Email)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Email-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]