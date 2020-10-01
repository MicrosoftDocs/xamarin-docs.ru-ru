---
title: Xamarin.Essentials. Тема приложения
description: В этой статье описывается API запрошенной темы приложения в Xamarin.Essentials, предоставляющий сведения о том, какой стиль темы запрашивается для работающего приложения.
ms.assetid: F6F6D496-A8A9-4B9A-AF1A-370D937E5073
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 01/06/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c68f00b77f0b9f88d014334dc56e1e58ed057986
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436885"
---
# <a name="no-locxamarinessentials-app-theme"></a>Xamarin.Essentials. Тема приложения

API **RequestedTheme** входит в состав класса [`AppInfo`](app-information.md) и предоставляет сведения о том, какая тема запрашивается системой для работающего приложения.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-requestedtheme"></a>Использование RequestedTheme

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-theme-information"></a>Получение сведений о теме

Запрошенную тему приложения можно обнаружить с помощью API следующим образом:

```csharp
AppTheme appTheme = AppInfo.RequestedTheme;

```

Это позволит системе получить текущую запрошенную тему для вашего приложения. Будет возвращаться одно из следующих значений:

* Не указан
* Светлый
* Темный

Если операционная система не имеет определенного стиля пользовательского интерфейса для запроса, то будет возвращено значение "Не указан". Например, на устройствах под управлением iOS более поздних версий, чем 13.0.


## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

В Android используются режимы конфигурации для указания типа темы, запрашиваемой у пользователя. В зависимости от версии Android пользователь может изменить его, а также он изменяется при включенном режиме экономии заряда.

Дополнительные сведения о темной теме для Android см. [здесь](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme).


# <a name="ios"></a>[iOS](#tab/ios)

Значение "Не указан" всегда будет возвращаться для iOS более поздних версий, чем 13.0.


# <a name="uwp"></a>[UWP](#tab/uwp)

Вызов `RequestedTheme` должен выполняться в потоке пользовательского интерфейса, иначе возникнет исключение.

Приложения UWP будут учитывать значение в приложении UWP. XAML в разделе **RequestedTheme**. Если задана конкретная тема, Xamarin.Essentials всегда будет возвращать этот параметр. Чтобы использовать динамическую тему операционной системы, удалите этот узел из приложения, а затем при его запуске приложение вернет тему, заданную пользователем в параметрах Windows (**Параметры > Персонализация > Цвета > выберите режим приложения по умолчанию**).

Дополнительные сведения по запрошенной теме UWP см. [здесь](/uwp/api/windows.ui.xaml.application.requestedtheme).

--------------

## <a name="api"></a>API

- [Исходный код AppInfo](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/AppInfo)
- [Документация по API AppInfo](xref:Xamarin.Essentials.AppInfo)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Theme-Detection-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]