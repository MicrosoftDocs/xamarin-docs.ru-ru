---
title: Xamarin.Essentials. Общий доступ
description: Класс Share в Xamarin.Essentials позволяет приложению обмениваться данными, такими как текст и веб-ссылки, с другими приложениями на устройстве.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b6bc8383f1c19e94c6760a213b4b9813ea77139a
ms.sourcegitcommit: 2a7bbe9cbee3727ba20ee755c1713bcfdb4d8ecb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98950965"
---
# <a name="xamarinessentials-share"></a>Xamarin.Essentials. Общий доступ

Класс **Share** позволяет приложению обмениваться данными, такими как текст и веб-ссылки, с другими приложениями на устройстве.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-share"></a>Использование класса Share

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Чтобы использовать функции класса Share, вызовите метод `RequestAsync` с полезными данными запроса, включающего сведения для обмена с другими приложениями. Текст и URI могут сочетаться, и каждая платформа будет выполнять фильтрацию по содержимому.

```csharp

public class ShareTest
{
    public async Task ShareText(string text)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await Share.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

Пользовательский интерфейс для общего использования с внешним приложением, отображаемый при запросе:

![Пользовательский интерфейс для общего использования с внешним приложением](images/share.png)

## <a name="file"></a>Файл

Эта функция позволяет приложению предоставлять общий доступ к файлам для других приложений на устройстве. Xamarin.Essentials автоматически обнаруживает тип файла (MIME) и запрашивает его добавление в общий доступ. Каждая платформа может поддерживать только определенные расширения файлов.

Ниже приведен пример записи текста на диск и предоставления общего доступа к нему другим приложениям.

```csharp
var fn =  "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file)
});
```

## <a name="multiple-files"></a>несколько файлов

Использование нескольких общих файлов отличается от использования одного файла только возможностью одновременно отправлять несколько файлов:

```csharp
var file1 = Path.Combine(FileSystem.CacheDirectory, "Attachment1.txt");
File.WriteAllText(file, "Content 1");
var file2 = Path.Combine(FileSystem.CacheDirectory, "Attachment2.txt");
File.WriteAllText(file, "Content 2");

await Share.RequestAsync(new ShareMultipleFilesRequest
{
    Title = ShareFilesTitle,
    Files = new ShareFile[] { new ShareFile(file1), new ShareFile(file2) }
});
```

## <a name="presentation-location"></a>Расположение презентации

[!include[](~/essentials/includes/ios-PresentationSourceBounds.md)]

## <a name="platform-differences"></a>Различия платформ

# <a name="android"></a>[Android](#tab/android)

- Свойство `Subject` используется для требуемой темы сообщения.

# <a name="ios"></a>[iOS](#tab/ios)

- `Subject` не используется.
- `Title` не используется.

# <a name="uwp"></a>[UWP](#tab/uwp)

- В `Title` будет использоваться значение по умолчанию (имя приложения), если значение не задано.
- `Subject` не используется.

-----

## <a name="api"></a>API

- [Исходный код Share](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Share)
- [Документация по API для Share](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
