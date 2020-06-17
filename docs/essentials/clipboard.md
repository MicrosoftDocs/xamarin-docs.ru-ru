---
title: "Xamarin.Essentials: Clipboard" description: "В этом документе описывается класс Clipboard в Xamarin.Essentials, который позволяет копировать текст в системный буфер обмена и вставлять его между приложениями".
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0 author: jamesmontemagno ms.author: jamont ms.date: 06.01.2020 ms.custom: video no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials. буфер обмена

Класс **Clipboard** позволяет копировать текст в системный буфер обмена и вставлять его между приложениями.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-clipboard"></a>Использование Clipboard

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Чтобы проверить, есть ли в классе **Clipboard** текст, готовый для вставки, используйте следующий код:

```csharp
var hasText = Clipboard.HasText;
```

Чтобы поместить текст в класс **Clipboard**, используйте следующий код:

```csharp
await Clipboard.SetTextAsync("Hello World");
```

Чтобы прочесть текст из класса **Clipboard**, используйте следующий код:

```csharp
var text = await Clipboard.GetTextAsync();
```

При изменении содержимого буфера обмена инициируется событие.

```csharp
public class ClipboardTest
{
    public ClipboardTest()
    {
        // Register for clipboard changes, be sure to unsubscribe when needed
        Clipboard.ClipboardContentChanged += OnClipboardContentChanged;
    }

    void OnClipboardContentChanged(object sender, EventArgs    e)
    {
        Console.WriteLine($"Last clipboard change at {DateTime.UtcNow:T}";);
    }
}
```

> [!TIP]
> Доступ к буферу обмена должен осуществляться в основном потоке пользовательского интерфейса. Подробные сведения о том, как вызвать методы в основном потоке пользовательского интерфейса, см. в статье об API [MainThread](~/essentials/main-thread.md).

## <a name="api"></a>API

- [Исходный код Clipboard](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Документация по API Clipboard](xref:Xamarin.Essentials.Clipboard)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Clipboard-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
