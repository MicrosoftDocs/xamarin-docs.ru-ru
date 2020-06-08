---
название: "Xamarin.Essentials: Share"; описание: "Класс Share вXamarin.Essentials позволяет приложению обмениваться данными, такими как текст и веб-ссылки, с другими приложениями на устройстве".
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665 author: jamesmontemagno ms.author: jamont ms.date: 06.01.2020 ms.custom: video no-loc: [Xamarin.Forms, Xamarin.Essentials]
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

![Общий доступ](images/share.png)

## <a name="files"></a>Файлы

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

## <a name="presentation-location"></a>Расположение презентации

Запрашивая общую папку на iPadOS, вы можете использовать всплывающий элемент поверх элемента управления. Расположение можно указать с помощью свойства `PresentationSourceBounds`:

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

- [Исходный код Share](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Share)
- [Документация по API для Share](xref:Xamarin.Essentials.Share)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Share-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
