---
Title: "Проверка орфографии с помощью API Bing для проверки орфографии" Description: "Проверка орфографии Bing выполняет контекстную проверку орфографии для текста, предоставляя встроенные предложения по написанию слов с ошибками. В этой статье объясняется, как использовать Проверка орфографии Bing REST API для исправления орфографических ошибок в Xamarin.Forms приложении ".
MS. произв. Xamarin MS. AssetID: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 02/08/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="spell-checking-using-the-bing-spell-check-api"></a>Проверка орфографии с помощью API Bing для проверки орфографии

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Проверка орфографии Bing выполняет контекстную проверку орфографии для текста, предоставляя встроенные предложения по написанию слов с ошибками. В этой статье объясняется, как использовать REST API Проверка орфографии Bing для исправления орфографических ошибок в Xamarin.Forms приложении._

## <a name="overview"></a>Обзор

Проверка орфографии Bing REST API имеет два режима работы, и при выполнении запроса к API необходимо указать режим.

- `Spell`Исправление короткого текста (до 9 слов) без изменения регистра.
- `Proof`Исправление длинного текста, преобразование регистра и основные знаки препинания, а также подавляет агрессивные исправления.

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

Чтобы использовать API Bing для проверки орфографии, необходимо получить ключ API. Это можно получить в [пробной Cognitive Services](https://azure.microsoft.com/try/cognitive-services/)

Список языков, поддерживаемых API Bing для проверки орфографии, см. в разделе [Поддерживаемые языки](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/). Дополнительные сведения о API Bing для проверки орфографии см. в [документации по проверка орфографии Bing](/azure/cognitive-services/bing-spell-check/).

## <a name="authentication"></a>Проверка подлинности

Для каждого запроса, выполненного в API Bing для проверки орфографии, требуется ключ API, который должен быть указан в качестве значения `Ocp-Apim-Subscription-Key` заголовка. В следующем примере кода показано, как добавить ключ API в `Ocp-Apim-Subscription-Key` заголовок запроса:

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Сбой передачи допустимого ключа API в API Bing для проверки орфографии приведет к ошибке ответа 401.

## <a name="performing-spell-checking"></a>Выполнение проверки орфографии

Проверка орфографии может быть достигнута путем создания запроса GET или POST к `SpellCheck` API в `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck` . При выполнении запроса GET текст для проверки орфографии отправляется как параметр запроса. При выполнении запроса POST текст для проверки орфографии отправляется в тексте запроса. Запросы GET ограничены проверкой орфографии 1500 символов из-за ограничения длины строки параметра запроса. Поэтому запросы POST обычно должны выполняться, если не выполняется проверка орфографии для коротких строк.

В примере приложения `SpellCheckTextAsync` метод вызывает процесс проверки орфографии:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

`SpellCheckTextAsync`Метод создает URI запроса, а затем отправляет запрос в `SpellCheck` API, который ВОЗВРАЩАЕТ ответ JSON, содержащий результат. Ответ JSON десериализуется, и результат возвращается вызывающему методу для вывода.

### <a name="configuring-spell-checking"></a>Настройка проверки орфографии

Процесс проверки орфографии можно настроить, указав параметры HTTP-запроса:

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

Этот метод задает текст для проверки орфографии и режим проверки орфографии.

Дополнительные сведения о Проверка орфографии Bing REST API см. в разделе [Справочник по API проверки орфографии версии 7](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/).

### <a name="sending-the-request"></a>Идет отправка запроса

`SendRequestAsync`Метод выполняет запрос GET к Проверка орфографии Bing REST API и возвращает ответ:

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Этот метод отправляет в API запрос GET `SpellCheck` с URL-адресом запроса, указывающим текст для перевода, и режим проверки орфографии. Затем ответ считывается и возвращается вызывающему методу.

`SpellCheck`API отправит код состояния HTTP 200 (ОК) в ответе при условии, что запрос действителен, что означает, что запрос выполнен успешно и запрошенные сведения находятся в ответе. Список объектов ответов см. в разделе [объекты ответов](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects).

### <a name="processing-the-response"></a>Обработка ответа

Ответ API возвращается в формате JSON. В следующих данных JSON показано ответное сообщение для неправильно написанного текста `Go shappin tommorow` :

```json
{  
   "_type":"SpellCheck",
   "flaggedTokens":[  
      {  
         "offset":3,
         "token":"shappin",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"shopping",
               "score":1
            }
         ]
      },
      {  
         "offset":11,
         "token":"tommorow",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"tomorrow",
               "score":1
            }
         ]
      }
   ],
   "correctionType":"High"
}
```

`flaggedTokens`Массив содержит массив слов в тексте, помеченный как неверно написанный или грамматически неверный. Если орфографические или грамматические ошибки не найдены, массив будет пустым. В массиве используются следующие Теги:

- `offset`— Отсчитываемое от нуля смещение от начала текстовой строки до слова, помеченного как.
- `token`— слово в текстовой строке, написанное неправильно или грамматически неверным.
- `type`— тип ошибки, вызвавшей пометку слова. Существует два возможных значения: `RepeatedToken` и `UnknownToken` .
- `suggestions`— массив слов, которые будут исправлять ошибку правописания или грамматики. Массив состоит из `suggestion` и `score` , который указывает уровень достоверности, который является правильным исправлением.

В примере приложения ответ JSON десериализуется в `SpellCheckResult` экземпляр, а результат возвращается вызывающему методу для вывода. В следующем примере кода показано, как `SpellCheckResult` экземпляр обрабатывается для отображения:

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

Этот код проходит по `FlaggedTokens` коллекции и заменяет все орфографические или грамматически неверные слова в исходном тексте первым предложением. Следующие снимки экрана отображаются до и после проверки орфографии:

![](spell-check-images/before-spell-check.png "Before Spell Check")

![](spell-check-images/after-spell-check.png "After Spell Check")

> [!NOTE]
> Приведенный выше пример использует `Replace` для простоты, но в большом объеме текста он может заменить неверный маркер. API предоставляет `offset` значение, которое должно использоваться в рабочих приложениях для определения правильного расположения в исходном тексте для выполнения обновления.

## <a name="summary"></a>Сводка

В этой статье объясняется, как использовать REST API Проверка орфографии Bing для исправления орфографических ошибок в Xamarin.Forms приложении. Проверка орфографии Bing выполняет контекстную проверку орфографии для текста, предоставляя встроенные предложения по написанию слов с ошибками.

## <a name="related-links"></a>Связанные ссылки

- [Документация по Проверка орфографии Bing](/azure/cognitive-services/bing-spell-check/)
- [Использование веб-службы RESTFUL](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Cognitive Services ToDo (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Справочник по API "Проверка орфографии Bing" версии 7](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
