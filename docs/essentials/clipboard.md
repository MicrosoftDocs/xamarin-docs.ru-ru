---
title: Xamarin.Essentials. буфер обмена
description: В этом документе описывается класс Clipboard в Xamarin.Essentials, который позволяет копировать текст из одного приложения в системный буфер обмена и вставить его в другое приложение.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d0a984f0f3bf27447e250c12e38fd9adcfb0029f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802454"
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

- [Исходный код Clipboard](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Clipboard)
- [Документация по API Clipboard](xref:Xamarin.Essentials.Clipboard)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Clipboard-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
