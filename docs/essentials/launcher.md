---
title: Класс Launcher в Xamarin.Essentials
description: Класс Launcher в Xamarin.Essentials позволяет приложению открыть URI средствами системы.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 08/20/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1e68755778522fa61d593d25e763fae1569724cf
ms.sourcegitcommit: 2a7bbe9cbee3727ba20ee755c1713bcfdb4d8ecb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98950980"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials. Средство запуска

Класс **Launcher** позволяет приложению открыть URI средствами системы. Это часто используется при глубокой привязке к пользовательским схемам URI другого приложения. Если нужно открыть в браузере веб-сайт, следует обратиться к API **[Browser](open-browser.md)** .

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-launcher"></a>Использование класса Launcher

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Чтобы использовать функции класса Launcher, вызовите метод `OpenAsync` и передайте параметр `string` или `Uri` для открытия. При необходимости с помощью метода `CanOpenAsync` можно проверить, может ли приложение на устройстве обработать схему URI.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

Это можно объединить в один вызов с помощью `TryOpenAsync`, который проверяет, можно ли открыть параметр, и, если да, открывает его.

```csharp
public class LauncherTest
{
    public async Task<bool> OpenRideShareAsync()
    {
        return await Launcher.TryOpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

### <a name="additional-platform-setup"></a>Настройка дополнительных платформ

# <a name="android"></a>[Android](#tab/android)

Дополнительная настройка не требуется.

# <a name="ios"></a>[iOS](#tab/ios)

В iOS 9 и более поздних версиях Apple обеспечивает применение схем, которые может запрашивать приложение. Чтобы определить схемы, которые вы хотите использовать, необходимо указать `LSApplicationQueriesSchemes` в файле `Info.plist`.

```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>lyft</string>  
    <string>fb</string>
</array>
```

# <a name="uwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="files"></a>Файлы

Эта функция позволяет приложению запрашивать другие приложения для открытия и просмотра файла. Xamarin.Essentials автоматически обнаруживает тип файла (MIME) и запрашивает его открытие.

Ниже приведен пример записи текста на диск и запрос на открытие.

```csharp
var fn = "File.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Launcher.OpenAsync(new OpenFileRequest
{
    File = new ReadOnlyFile(file)
});
```

## <a name="presentation-location-when-opening-files"></a>Расположение презентации при открытии файлов

[!include[](~/essentials/includes/ios-PresentationSourceBounds.md)]

## <a name="platform-differences"></a>Различия платформ

# <a name="android"></a>[Android](#tab/android)

Задача, возвращенная методом `CanOpenAsync`, выполняется немедленно.

# <a name="ios"></a>[iOS](#tab/ios)

Если конечное приложение на этом устройстве никогда ранее не открывалось с помощью метода `OpenAsync` из вашего приложения, iOS однократно предложит пользователю разрешить открывать его с помощью вашего приложения.

Задача, возвращенная методом `CanOpenAsync`, выполняется немедленно.

Дополнительные сведения о реализации iOS доступны [здесь](xref:UIKit.UIApplication.CanOpenUrl*).

# <a name="uwp"></a>[UWP](#tab/uwp)

Различия платформ отсутствуют.

-----

## <a name="api"></a>API

- [Исходный код класса Launcher](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Launcher)
- [Документация по API Launcher](xref:Xamarin.Essentials.Launcher)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Launcher-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
