---
title: Xamarin.Essentials. Преобразование текста в речь
description: Класс TextToSpeech в Xamarin.Essentials позволяет приложению использовать встроенные механизмы преобразования текста в речь, чтобы озвучивать текст на устройстве, а также запрашивать доступные языки, которые может поддерживать подсистема.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 452a54637c270f80c2e1add4d6cadedbb4b27077
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801833"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials. Преобразование текста в речь

Класс **TextToSpeech** позволяет приложению использовать встроенные механизмы преобразования текста в речь, чтобы озвучивать текст на устройстве, а также запрашивать доступные языки, которые может поддерживать подсистема.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-text-to-speech"></a>Использование преобразования текста в речь

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

При преобразовании текста в речь вызывается метод `SpeakAsync` с текстом и дополнительными параметрами и выполняется возврат после завершения фрагмента речи.

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) =>
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

Этот метод принимает необязательный параметр `CancellationToken`, чтобы остановить высказывание после его запуска.

```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

// Cancel speech if a cancellation token exists & hasn't been already requested.
public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? true)
        return;

    cts.Cancel();
}
```

Функция преобразования текста в речь автоматически ставит в очередь запросы на озвучивание речи из одного и того же потока.

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>Настройки речи

Для более высокого уровня контроля над воспроизведением звука применяется параметр `SpeechOptions`, который позволяет установить громкость, высоту тона и языковой стандарт.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeechOptions()
        {
            Volume = .75f,
            Pitch = 1.0f
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

Ниже приведены поддерживаемые значения этих параметров.

| Параметр | Минимум | Максимум |
| --- | :---: | :---: |
| Высота тона | 0 | 2.0 |
| Громкость | 0 | 1.0 |

### <a name="speech-locales"></a>Языковой стандарт

Каждая платформа поддерживает разные языковые стандарты, чтобы применять разные языки и акценты. Платформы используют разные коды и способы указания языкового стандарта. Поэтому Xamarin.Essentials предоставляет кроссплатформенный класс `Locale` и способ запросить их с помощью `GetLocalesAsync`.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeechOptions()
        {
            Volume = .75f,
            Pitch = 1.0f,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>Ограничения

- Порядок очереди запросов на озвучивание не гарантируется при вызове нескольких потоков.
- Воспроизведение фонового звука официально не поддерживается.

## <a name="api"></a>API

- [Исходный код TextToSpeech](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/TextToSpeech)
- [Документация по API TextToSpeech](xref:Xamarin.Essentials.TextToSpeech)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Text-to-Speech-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
