---
title: 'Xamarin.Essentials: Снимок экрана'
description: В этом документе описывается класс Screenshot в Xamarin.Essentials, который позволяет сделать снимок текущего содержимого экрана приложения.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 09/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 085da722aa2e893f97efb1c89f20b03da330ac3e
ms.sourcegitcommit: 744f977b0595f489c592e29c8a3ba548fde02b6f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2020
ms.locfileid: "91414727"
---
# <a name="no-locxamarinessentials-screenshot"></a>Xamarin.Essentials: Снимок экрана

Класс **Screenshot** позволяет сделать снимок текущего содержимого экрана приложения.

![Предварительный выпуск API](~/media/shared/preview.png)


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
