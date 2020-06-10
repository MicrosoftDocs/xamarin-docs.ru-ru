---
Title: "обнаруженное распознавания эмоцийное распознавание с помощью API распознавания лиц" Description: "API распознавания лиц принимает выражение лица в изображении в качестве входных данных и возвращает данные, включающие уровни достоверности по набору эмоции для каждого лица в изображении. В этой статье объясняется, как использовать API распознавания лиц для распознавания распознавания эмоций, чтобы оценить Xamarin.Forms приложение. "
MS. произв. Xamarin MS. AssetID: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 05/10/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="perceived-emotion-recognition-using-the-face-api"></a>Воспринимаемое распознавание распознавания эмоций с помощью API распознавания лиц

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

API распознавания лиц может выполнять обнаружение распознавания эмоций для обнаружения гнев, неприятия, отвращение, аномалии, счастье, нейтрального, грусть и удивительного в выражении лица, основанного на воспринимаемых заметках человеческими программистами. Важно отметить, однако, что только выражения лица могут необязательно представлять внутренние состояния людей.

Помимо возврата результата распознавания эмоций для выражения лица, API распознавания лиц может также возвращать ограничивающий прямоугольник для обнаруженных сторон.

Распознавание распознавания эмоций можно выполнить с помощью клиентской библиотеки и с помощью REST API. В этой статье рассматривается выполнение распознавания эмоцийного распознавания с помощью REST API. Дополнительные сведения о REST API см. в разделе [REST API лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

API распознавания лиц также можно использовать для распознавания выражений лица в видео и получения сводки по их эмоции. Дополнительные сведения см. [в статье анализ видео в режиме реального времени](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

Чтобы использовать API распознавания лиц, необходимо получить ключ API. Это можно получить на [Cognitive Services попытки](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Дополнительные сведения о API распознавания лиц см. в разделе [API распознавания лиц](/azure/cognitive-services/face/overview/).

## <a name="authentication"></a>Проверка подлинности

Для каждого запроса, выполненного в API распознавания лиц, требуется ключ API, который должен быть указан в качестве значения `Ocp-Apim-Subscription-Key` заголовка. В следующем примере кода показано, как добавить ключ API в `Ocp-Apim-Subscription-Key` заголовок запроса:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Сбой передачи допустимого ключа API в API распознавания лиц приведет к ошибке ответа 401.

## <a name="perform-emotion-recognition"></a>Выполнение распознавания распознавания эмоций

Распознавание распознавания эмоций выполняется путем создания запроса POST, содержащего изображение, в `detect` API `https://[location].api.cognitive.microsoft.com/face/v1.0` , где `[location]]` — это регион, который вы использовали для получения ключа API. Необязательные параметры запроса:

- `returnFaceId`— Указывает, следует ли возвращать Фацеидс обнаруженных сторон. Значение по умолчанию — `true`.
- `returnFaceLandmarks`— Указывает, следует ли возвращать ориентиры обнаруженных лиц. Значение по умолчанию — `false`.
- `returnFaceAttributes`— следует ли анализировать и возвращать один или несколько указанных атрибутов лиц. Поддерживаются следующие атрибуты лиц: `age` , `gender` , `headPose` ,,,, `smile` `facialHair` `glasses` `emotion` , `hair` , `makeup` , `occlusion` ,,, `accessories` `blur` `exposure` и `noise` . Обратите внимание, что анализ атрибутов лица имеет дополнительные вычислительные и временные затраты.

Содержимое изображения должно быть помещено в текст запроса POST в виде URL-адреса или двоичных данных.

> [!NOTE]
> Поддерживаются форматы файлов изображений JPEG, PNG, GIF и BMP, а размер разрешенного файла — от 1 КБ до 4 МБ.

В примере приложения процесс распознавания распознавания эмоций вызывается путем вызова `DetectAsync` метода:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

Этот вызов метода задает поток, содержащий данные изображения, которые Фацеидс должны быть возвращены, и эти ориентиры не должны возвращаться и распознавания эмоций изображения. Он также указывает, что результаты будут возвращены в виде массива `Face` объектов. В свою очередь, `DetectAsync` метод вызывает `detect` REST API, который выполняет распознавание распознавания эмоций:

```csharp
public async Task<Face[]> DetectAsync(Stream imageStream, bool returnFaceId, bool returnFaceLandmarks, IEnumerable<FaceAttributeType> returnFaceAttributes)
{
  var requestUrl =
    $"{Constants.FaceEndpoint}/detect?returnFaceId={returnFaceId}" +
    "&returnFaceLandmarks={returnFaceLandmarks}" +
    "&returnFaceAttributes={GetAttributeString(returnFaceAttributes)}";
  return await SendRequestAsync<Stream, Face[]>(HttpMethod.Post, requestUrl, imageStream);
}
```

Этот метод создает URI запроса, а затем отправляет запрос `detect` API через `SendRequestAsync` метод.

> [!NOTE]
> Вы должны использовать один и тот же регион в API распознавания лицных вызовах, как вы использовали для получения ключей подписки. Например, если вы получили ключи подписки из `westus` региона, конечная точка обнаружения лиц будет иметь значение `https://westus.api.cognitive.microsoft.com/face/v1.0/detect` .

### <a name="send-the-request"></a>Отправка запроса

`SendRequestAsync`Метод выполняет запрос POST к API распознавания лиц и возвращает результат в виде `Face` массива:

```csharp
async Task<TResponse> SendRequestAsync<TRequest, TResponse>(HttpMethod httpMethod, string requestUrl, TRequest requestBody)
{
  var request = new HttpRequestMessage(httpMethod, Constants.FaceEndpoint);
  request.RequestUri = new Uri(requestUrl);
  if (requestBody != null)
  {
    if (requestBody is Stream)
    {
      request.Content = new StreamContent(requestBody as Stream);
      request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    }
    else
    {
      // If the image is supplied via a URL
      request.Content = new StringContent(JsonConvert.SerializeObject(requestBody, s_settings), Encoding.UTF8, "application/json");
    }
  }

  HttpResponseMessage responseMessage = await _client.SendAsync(request);
  if (responseMessage.IsSuccessStatusCode)
  {
    string responseContent = null;
    if (responseMessage.Content != null)
    {
      responseContent = await responseMessage.Content.ReadAsStringAsync();
    }
    if (!string.IsNullOrWhiteSpace(responseContent))
    {
      return JsonConvert.DeserializeObject<TResponse>(responseContent, s_settings);
    }
    return default(TResponse);
  }
  else
  {
    ...
  }
  return default(TResponse);
}
```

Если образ предоставляется через поток, метод создает запрос POST путем заключения потока изображения в `StreamContent` экземпляр, который предоставляет содержимое HTTP на основе потока. Кроме того, если образ предоставляется через URL-адрес, метод создает запрос POST путем заключения URL-адреса в `StringContent` экземпляр, который предоставляет содержимое HTTP на основе строки.

Затем запрос POST отправляется в `detect` API. Ответ считывается, десериализуется и возвращается вызывающему методу.

`detect`API отправит код состояния HTTP 200 (ОК) в ответе при условии, что запрос действителен, что означает, что запрос выполнен успешно и запрошенные сведения находятся в ответе. Список возможных ответов об ошибках см. в разделе [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

### <a name="process-the-response"></a>Обработка ответа

Ответ API возвращается в формате JSON. В следующих данных JSON показано типичное успешное сообщение ответа, которое предоставляет данные, запрошенные образцом приложения:

```json
[  
   {  
      "faceId":"8a1a80fe-1027-48cf-a7f0-e61c0f005051",
      "faceRectangle":{  
         "top":192,
         "left":164,
         "width":339,
         "height":339
      },
      "faceAttributes":{  
         "emotion":{  
            "anger":0.0,
            "contempt":0.0,
            "disgust":0.0,
            "fear":0.0,
            "happiness":1.0,
            "neutral":0.0,
            "sadness":0.0,
            "surprise":0.0
         }
      }
   }
]
```

Сообщение об успешном ответе состоит из массива записей лиц, ранжированных по размеру прямоугольника в убывающем порядке, в то время как пустой ответ означает, что лица не обнаружены. Каждое распознанное лицо включает ряд дополнительных атрибутов, которые задаются `returnFaceAttributes` аргументом для `DetectAsync` метода.

В примере приложения ответ JSON десериализуется в массив `Face` объектов. При интерпретации результатов API распознавания лиц, обнаруженный распознавания эмоций должен интерпретироваться как распознавания эмоций с наивысшим рейтингом, так как оценки нормализованы до одной суммы. Таким образом, пример приложения отображает распознанный распознавания эмоций с наивысшим рейтингом для наибольшего обнаруженного лица в изображении. Для этого используется такой код:

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

На следующем снимке экрана показан результат процесса распознавания распознавания эмоций в примере приложения:

![](emotion-recognition-images/emotion-recognition.png "Emotion Recognition")

## <a name="related-links"></a>Связанные ссылки

- [API распознавания лиц](/azure/cognitive-services/face/overview/).
- [Cognitive Services ToDo (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [REST API лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
