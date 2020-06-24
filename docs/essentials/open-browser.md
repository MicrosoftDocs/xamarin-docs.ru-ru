---
title: Xamarin.Essentials Открытие браузера
description: Класс Browser в Xamarin.Essentials позволяет приложению открыть веб-ссылку в предпочитаемом браузере оптимизированной системы или внешнем браузере.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 997c6b66b5dba43eb440130f3f58d31a5a274815
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802247"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials. Браузер

Класс **Browser** позволяет приложению открыть веб-ссылку в предпочитаемом браузере оптимизированной системы или внешнем браузере.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-browser"></a>Использование класса Browser

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Чтобы использовать функции класса Browser, вызовите метод `OpenAsync` с параметрами `Uri` и `BrowserLaunchMode`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchMode.SystemPreferred);
    }
}
```

Этот метод возвращает значение при _запуске_ (и не обязательно _закрытии_) браузера пользователем.  Результат `bool` показывает, был ли запуск успешным.

## <a name="customization"></a>Настройка

При использовании предпочтительного браузера в системе есть несколько вариантов настройки, доступных для iOS и Android. Сюда входят `TitleMode` (только Android) и параметры цвета для `Toolbar` (iOS и Android) и `Controls` (только iOS).

Эти параметры задаются с помощью `BrowserLaunchOptions` при вызове `OpenAsync`.

```csharp
await Browser.OpenAsync(uri, new BrowserLaunchOptions
                {
                    LaunchMode = BrowserLaunchMode.SystemPreferred,
                    TitleMode = BrowserTitleMode.Show,
                    PreferredToolbarColor = Color.AliceBlue,
                    PreferredControlColor = Color.Violet
                });
```

![Параметры браузера](images/browser-options.png)

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

Режим запуска определяет, как запускается браузер:

## <a name="system-preferred"></a>Предпочитаемый режим системы

Браузер будет пытаться использовать [пользовательские вкладки](https://developer.chrome.com/multidevice/android/customtabs) для загрузки URI и учета текущей навигации.

## <a name="external"></a>Внешняя

С помощью `Intent` будет запрошено открытие Uri в обычном системном браузере.

# <a name="ios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Предпочитаемый режим системы

С помощью [SFSafariViewController](xref:SafariServices.SFSafariViewController) выполняется загрузка Uri с сохранением навигации.

## <a name="external"></a>Внешняя

С помощью стандартного параметра `OpenUrl` в главном приложении запускается браузер по умолчанию за пределами приложения.

# <a name="uwp"></a>[UWP](#tab/uwp)

Всегда будет запускаться браузер, установленный у пользователя по умолчанию, независимо от `BrowserLaunchMode`.

--------------

## <a name="api"></a>API

- [Исходный код класса Browser](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Browser)
- [Документация по API Browser](xref:Xamarin.Essentials.Browser)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Open-Browser-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
