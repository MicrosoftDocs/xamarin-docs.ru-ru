---
title: Преобразование текста с помощью API-интерфейса переводчика
description: API Microsoft Translator можно использовать для перевода речи и текста с помощью REST API. В этой статье объясняется, как использовать API перевода текстов Майкрософт для перевода текста с одного языка на другой в Xamarin.Forms приложении.
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 857a4e465a4f42d2fd6a2a4d977334820fb2ea4f
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366091"
---
# <a name="text-translation-using-the-translator-api"></a>Преобразование текста с помощью API-интерфейса переводчика

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_API Microsoft Translator можно использовать для перевода речи и текста с помощью REST API. В этой статье объясняется, как использовать API перевода текстов Майкрософт для перевода текста с одного языка на другой в Xamarin.Forms приложении._

## <a name="overview"></a>Обзор

API-интерфейс переводчика имеет два компонента:

- Преобразование текста REST API для перевода текста с одного языка на другой в тексте на другом языке. API автоматически определяет язык текста, отправленного перед его переводом.
- Перевод речи REST API, чтобы транскрипция речь с одного языка на текст на другом языке. API также сочетает в себе возможности преобразования текста в речь для обратного перевода переведенного текста.

Эта статья посвящена преобразованию текста с одного языка на другой с помощью API перевода текстов.

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

Чтобы использовать API перевода текстов, необходимо получить ключ API. Это можно получить с [помощью подписки на API перевода текстов Майкрософт](/azure/cognitive-services/translator/translator-text-how-to-signup/).

Дополнительные сведения о API перевода текстов Майкрософт см. в [документации по API перевода текстов](/azure/cognitive-services/translator/).

## <a name="authentication"></a>Аутентификация

Для каждого запроса к API перевода текстов требуется маркер доступа JSON Web Token (JWT), который можно получить из службы маркеров работы со службами в `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` . Маркер можно получить, выполнив запрос POST к службе маркеров, указав `Ocp-Apim-Subscription-Key` заголовок, содержащий ключ API в качестве значения.

В следующем примере кода показано, как запросить маркер доступа из службы маркеров:

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Возвращаемый маркер доступа, который является текстом Base64, имеет время действия 10 минут. Таким образом, пример приложения обновляет маркер доступа каждые 9 минут.

Маркер доступа должен быть указан в каждом API перевода текстов вызове в качестве `Authorization` заголовка с префиксом строки `Bearer` , как показано в следующем примере кода:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Дополнительные сведения о службе маркеров для переприятных служб см. в разделе [Проверка подлинности](/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="performing-text-translation"></a>Выполнение преобразования текста

Преобразование текста можно достичь, создав запрос GET к `translate` API в `https://api.microsofttranslator.com/v2/http.svc/translate` . В примере приложения `TranslateTextAsync` метод вызывает процесс преобразования текста:

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

`TranslateTextAsync`Метод создает URI запроса и извлекает маркер доступа из службы маркеров. Затем запрос на перевод текста отправляется в `translate` API, который возвращает XML-ответ, содержащий результат. XML-ответ анализируется, и результат преобразования возвращается вызывающему методу для вывода.

Дополнительные сведения о интерфейсах API для преобразования текста см. в разделе [API перевода текстов](/azure/cognitive-services/translator/reference/v3-0-reference).

### <a name="configuring-text-translation"></a>Настройка преобразования текста

Процесс преобразования текста можно настроить, указав параметры HTTP-запроса:

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Этот метод задает текст для перевода и язык перевода текста. Список языков, поддерживаемых Microsoft Translator, см. [в статье Поддерживаемые языки в API перевода текстов Microsoft](/azure/cognitive-services/translator/languages/).

> [!NOTE]
> Если приложению необходимо знать, на каком языке находится текст, `Detect` можно вызвать API, чтобы определить язык текстовой строки.

### <a name="sending-the-request"></a>Идет отправка запроса

`SendRequestAsync`Метод делает запрос GET к преобразованию текста REST API и возвращает ответ:

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Этот метод создает запрос GET, добавляя маркер доступа к `Authorization` заголовку с префиксом строки `Bearer` . Затем запрос GET отправляется в `translate` API, с URL-адресом запроса, который указывает текст для перевода, и язык, на который нужно перевести текст. Затем ответ считывается и возвращается вызывающему методу.

`translate`API отправит код состояния HTTP 200 (ОК) в ответе при условии, что запрос действителен, что означает, что запрос выполнен успешно и запрошенные сведения находятся в ответе. Список возможных ответов об ошибках см. в разделе ответные сообщения при [получении](/azure/cognitive-services/translator/reference/v3-0-translate).

### <a name="processing-the-response"></a>Обработка ответа

Ответ API возвращается в формате XML. В следующих XML-данных показано типичное сообщение об успешном ответе:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

В примере приложения XML-ответ разбивается на `XDocument` экземпляр, при этом корневое значение XML возвращается вызывающему методу для отображения, как показано на следующих снимках экрана:

![Перевод текста на немецкий](text-translation-images/text-translation.png)

## <a name="summary"></a>Сводка

В этой статье объясняется, как использовать API перевода текстов Майкрософт для перевода текста с одного языка на другой язык в Xamarin.Forms приложении. Помимо перевода текста, API-интерфейс Microsoft Translator также может транскрипция речь с одного языка на текст на другом языке.

## <a name="related-links"></a>Связанные ссылки

- [Документация по API перевода текстов](/azure/cognitive-services/translator/)
- [Использование веб-службы RESTFUL](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Cognitive Services ToDo (пример)](/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [API Перевода текстов](/azure/cognitive-services/translator/reference/v3-0-reference)