---
title: Xamarin.Essentials. Общий доступ
description: Класс Share в Xamarin.Essentials позволяет приложению обмениваться данными, такими как текст и веб-ссылки, с другими приложениями на устройстве.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0870dd94c15f1bd94d5c6864b3d4caeb96349f32
ms.sourcegitcommit: 83793378b28e8ef8624406309b4ecd41aa1a3a14
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94503271"
---
# <a name="no-locxamarinessentials-share"></a>Xamarin.Essentials. Общий доступ

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

![Общий доступ](images/share.png)

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

![Предварительный выпуск API](~/media/shared/preview.png)

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

Запрашивая общую папку на iPadOS, вы можете использовать всплывающий элемент поверх элемента управления. При этом определяется место отображения всплывающего элемента, а также указывается стрелка на него. В этом расположении обычно указывается элемент управления, который инициировал действие. Расположение можно указать с помощью свойства `PresentationSourceBounds`:

```csharp
await Share.RequestAsync(new ShareFileRequest
{
    Title = Title,
    File = new ShareFile(file),
    PresentationSourceBounds = DeviceInfo.Platform== DevicePlatform.iOS && DeviceInfo.Idiom == DeviceIdiom.Tablet
                            ? new System.Drawing.Rectangle(0, 20, 0, 0)
                            : System.Drawing.Rectangle.Empty
});
```

Если вы используете Xamarin.Forms, можете передать `View` и вычислить границы:


```
public static class ViewHelpers
{
    public static Rectangle GetAbsoluteBounds(this Xamarin.Forms.View element)
    {
        Element looper = element;

        var absoluteX = element.X + element.Margin.Top;
        var absoluteY = element.Y + element.Margin.Left;

        // Add logic to handle titles, headers, or other non-view bars

        while (looper.Parent != null)
        {
            looper = looper.Parent;
            if (looper is Xamarin.Forms.View v)
            {
                absoluteX += v.X + v.Margin.Top;
                absoluteY += v.Y + v.Margin.Left;
            }
        }

        return new Rectangle(absoluteX, absoluteY, element.Width, element.Height);
    }

    public static System.Drawing.Rectangle ToSystemRectangle(this Rectangle rect) =>
        new System.Drawing.Rectangle((int)rect.X, (int)rect.Y, (int)rect.Width, (int)rect.Height);
}
```

Затем при вызове `RequstAsync` можете использовать следующий код:

```csharp
public Command<Xamarin.Forms.View> ShareCommand { get; } = new Command<Xamarin.Forms.View>(Share);
async void Share(Xamarin.Forms.View element)
{
    try
    {
        Analytics.TrackEvent("ShareWithFriends");
        var bounds = element.GetAbsoluteBounds();

        await Share.RequestAsync(new ShareTextRequest
        {
            PresentationSourceBounds = bounds.ToSystemRectangle(),
            Title = "Title",
            Text = "Text"
        });
    }
    catch (Exception)
    {
        // Handle exception that share failed
    }
}
```

Вы можете передать вызывающий элемент при активации `Command`:

```xml
<Button Text="Share"
        Command="{Binding ShareWithFriendsCommand}"
        CommandParameter="{Binding Source={RelativeSource Self}}"/>
```

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
