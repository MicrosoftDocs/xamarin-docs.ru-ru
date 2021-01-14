---
title: 'Xamarin.Essentials: Снимок экрана'
description: В этом документе описывается класс Screenshot в Xamarin.Essentials, который позволяет сделать снимок текущего содержимого экрана приложения.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e907cf06814a5b14678e4584f9aabc56f39bb164
ms.sourcegitcommit: 995ee23d93e08dceb8754cc6c682cd2f4594345b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972270"
---
# <a name="no-locxamarinessentials-screenshot"></a>Xamarin.Essentials: Снимок экрана

Класс **Screenshot** позволяет сделать снимок текущего содержимого экрана приложения.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-screenshot"></a>Использование класса Screenshot

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Затем вызовите метод `CaptureAsync`, чтобы сделать снимок текущего содержимого экрана выполняющегося приложения. Он вернет объект `ScreenshotResult`, который можно использовать для получения `Width`, `Height` и `Stream` сделанного снимка экрана.


```csharp
async Task CaptureScreenshot()
{
    var screenshot = await Screenshot.CaptureAsync();
    var stream = await screenshot.OpenReadAsync();

    Image = ImageSource.FromStream(() => stream);
}
```


## <a name="api"></a>API

- [Исходный код Screenshot](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Screenshot)
- [Документация по API Screenshot](xref:Xamarin.Essentials.Screenshot)
