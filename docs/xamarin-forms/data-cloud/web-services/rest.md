---
title: Использование веб-службы RESTFUL
description: Интеграция веб-службы в приложение является распространенным сценарием. В этой статье показано, как использовать веб-службу RESTFUL из Xamarin.Forms приложения.
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/03/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a3fd59ecbaf85f24515deba8562060aadc6d2165
ms.sourcegitcommit: 10c7dd16fe78226053d1d036492b6c9102fc421b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/04/2021
ms.locfileid: "99540965"
---
# <a name="consume-a-restful-web-service"></a>Использование веб-службы RESTFUL

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/webservices-todorest)

_Интеграция веб-службы в приложение является распространенным сценарием. В этой статье показано, как использовать веб-службу RESTFUL из Xamarin.Forms приложения._

Перестроение данных о состоянии (остальное) — это архитектурный стиль для создания веб-служб. Запросы RESTFUL выполняются по протоколу HTTP с использованием тех же глаголов HTTP, которые используются веб-браузерами для получения веб-страниц и отправки данных на серверы. Команды:

- **Get** — эта операция используется для получения данных из веб-службы.
- **POST** — эта операция используется для создания нового элемента данных в веб-службе.
- **Разместить** — эта операция используется для обновления элемента данных в веб-службе.
- **Patch** — эта операция используется для обновления элемента данных веб-службы путем описания набора инструкций по изменению элемента. Эта команда не используется в примере приложения.
- **Delete** — эта операция используется для удаления элемента данных в веб-службе.

API веб-службы, которые соответствуют ОСТАВШИМся, называются API RESTFUL и определяются с помощью:

- Базовый URI.
- Методы HTTP, такие как GET, POST, WHERE, PATCH или DELETE.
- Тип мультимедиа для данных, например нотация объектов JavaScript (JSON).

Веб-службы RESTFUL обычно используют сообщения JSON для возврата данных клиенту. JSON — это формат обмена данными на основе текста, который создает компактные полезные данные, что приводит к снижению требований к пропускной способности при отправке данных. Пример приложения использует [библиотеку NewtonSoft JSON.NET](https://www.newtonsoft.com/json) с открытым исходным кодом для сериализации и десериализации сообщений.

Простота работы с другими позволила сделать ее основным методом доступа к веб-службам в мобильных приложениях.

При запуске примера приложения он подключается к размещенной локально службе RESTFUL, как показано на следующем снимке экрана:

![Образец приложения](rest-images/portal.png)

> [!NOTE]
> В iOS 9 и более поздних версиях Защита транспорта приложений (ATS) обеспечивает безопасное подключение между Интернет-ресурсами (например, серверным сервером приложения) и приложением, тем самым предотвращая случайное раскрытие конфиденциальной информации. Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут подвергаться требованиям безопасности ATS. Если соединения не соответствуют этим требованиям, они завершатся сбоем с исключением.
>
>ATS можно отключать, если невозможно использовать протокол **HTTPS** и безопасную связь с Интернет-ресурсами. Это можно сделать, обновив файл **info. plist** приложения. Дополнительные сведения см. в статье [Безопасность транспорта приложений](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Использование веб-службы

Служба RESTFUL написана с помощью ASP.NET Core и предоставляет следующие операции:

|Операция|Метод HTTP|Относительный URI|Параметры|
|--- |--- |--- |--- |
|Получение списка элементов задач|GET|/апи/тодоитемс/|
|Создать новый элемент задачи|POST|/апи/тодоитемс/|TodoItem в формате JSON|
|Обновление элемента задачи|PUT|/апи/тодоитемс/|TodoItem в формате JSON|
|Удаление элемента задачи|DELETE|/апи/тодоитемс/{ид}|

Большинство URI содержат `TodoItem` идентификатор в пути. Например, чтобы удалить, `TodoItem` чей идентификатор — `6bb8a868-dba1-4f1a-93b7-24ebce87e243` , клиент отправляет запрос на удаление в `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243` . Дополнительные сведения о модели данных, используемой в примере приложения, см. в разделе [моделирование данных](~/xamarin-forms/data-cloud/web-services/introduction.md).

Когда платформа веб-API получает запрос, он направляет запрос в действие. Эти действия являются просто открытыми методами в `TodoItemsController` классе. Платформа использует таблицу маршрутизации для определения действия, которое необходимо вызвать в ответ на запрос, как показано в следующем примере кода:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

Таблица маршрутизации содержит шаблон маршрута, и когда платформа веб-API получает запрос HTTP, он пытается сопоставить URI с шаблоном маршрута в таблице маршрутизации. Если не удается найти соответствующий маршрут, клиент получает ошибку 404 (не найдено). Если найден соответствующий маршрут, веб-API выбирает контроллер и действие следующим образом:

- Чтобы найти контроллер, веб-API добавляет "Controller" к значению переменной *{Controller}* .
- Чтобы найти действие, веб-API просматривает метод HTTP и выполняет поиск действий контроллера, которые снабжены одним и тем же методом HTTP, что и атрибут.
- Переменная-заполнитель *{ID}* сопоставлена с параметром действия.

Служба RESTFUL использует обычную проверку подлинности. Дополнительные сведения см. [в статье Проверка подлинности веб-службы RESTful](~/xamarin-forms/data-cloud/authentication/rest.md). Дополнительные сведения о маршрутизации веб-API ASP.NET см. [в разделе Маршрутизация в веб-API ASP.NET](https://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) на веб-сайте ASP.NET. Дополнительные сведения о создании службы RESTFUL с помощью ASP.NET Core см. в разделе [Создание серверных служб для собственных мобильных приложений](/aspnet/core/mobile/native-mobile-backend/).

`HttpClient`Класс используется для отправки и получения запросов по протоколу HTTP. Он предоставляет функциональные возможности для отправки HTTP-запросов и получения HTTP-ответов от идентифицированного ресурса URI. Каждый запрос отправляется как асинхронная операция. Дополнительные сведения об асинхронных операциях см. в разделе [Общие сведения о поддержке асинхронных](~/cross-platform/platform/async.md)операций.

`HttpResponseMessage`Класс представляет сообщение HTTP-ответа, полученное от веб службы после выполнения запроса HTTP. Он содержит сведения об ответе, включая код состояния, заголовков и любой текст. `HttpContent` Класс представляет тело HTTP и заголовки содержимого, таких как `Content-Type` и `Content-Encoding`. Содержимое может быть считано с помощью любого из `ReadAs` методов, например `ReadAsStringAsync` и `ReadAsByteArrayAsync` , в зависимости от формата данных.

### <a name="creating-the-httpclient-object"></a>Создание объекта HTTPClient

`HttpClient`Экземпляр объявляется на уровне класса, чтобы объект находился в течение всего времени, когда приложение должно выполнить HTTP-запросы, как показано в следующем примере кода:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
  }
  ...
}
```

### <a name="retrieving-data"></a>Извлечение данных

`HttpClient.GetAsync`Метод используется для отправки запроса GET в веб-службу, заданную с помощью URI, а затем получает ответ от веб-службы, как показано в следующем примере кода:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  Uri uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));
  ...
  HttpResponseMessage response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode)
  {
      string content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

Служба RESTFUL отправляет код состояния HTTP в `HttpResponseMessage.IsSuccessStatusCode` свойстве, чтобы указать, успешно ли был выполнен HTTP-запрос. Для этой операции служба RESTFUL отправляет код состояния HTTP 200 (ОК) в ответе. Это означает, что запрос выполнен успешно и запрошенные сведения находятся в ответе.

Если операция HTTP выполнена успешно, содержимое ответа считывается для вывода. `HttpResponseMessage.Content`Свойство представляет содержимое HTTP-ответа, а `HttpContent.ReadAsStringAsync` метод асинхронно записывает содержимое HTTP в строку. Затем это содержимое преобразуется из JSON в `List` `TodoItem` экземпляры.

> [!WARNING]
> Использование `ReadAsStringAsync` метода для получения большого ответа может отрицательно сказаться на производительности. В таких обстоятельствах ответ должен быть напрямую десериализован, чтобы избежать его полного буферизации.

### <a name="creating-data"></a>Создание данных

`HttpClient.PostAsync`Метод используется для отправки запроса POST в веб-службу, заданную с помощью URI, а затем для получения ответа от веб-службы, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  Uri uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));

  ...
  string json = JsonConvert.SerializeObject (item);
  StringContent content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem)
  {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully saved.");
  }
  ...
}
```

`TodoItem`Экземпляр преобразуется в полезные данные JSON для отправки в веб-службу. Затем эти полезные данные внедряются в тело содержимого HTTP, которое будет отправлено веб-службе до выполнения запроса с помощью `PostAsync` метода.

Служба RESTFUL отправляет код состояния HTTP в `HttpResponseMessage.IsSuccessStatusCode` свойстве, чтобы указать, успешно ли был выполнен HTTP-запрос. Общие ответы для этой операции:

- **201 (создано)** — запрос привел к созданию нового ресурса перед отправкой ответа.
- **400 (НЕдопустимый запрос)** — запрос не распознан сервером.
- **409 (конфликт)** — запрос не может быть выполнен из-за конфликта на сервере.

### <a name="updating-data"></a>Обновление данных

`HttpClient.PutAsync`Метод используется для отправки запроса на размещение в веб-службу, заданную с помощью URI, и последующего получения ответа от веб-службы, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```

Операция `PutAsync` метода идентична `PostAsync` методу, который используется для создания данных в веб-службе. Однако возможные отклики, отправляемые из веб-службы, отличаются.

Служба RESTFUL отправляет код состояния HTTP в `HttpResponseMessage.IsSuccessStatusCode` свойстве, чтобы указать, успешно ли был выполнен HTTP-запрос. Общие ответы для этой операции:

- **204 (нет содержимого)** — запрос успешно обработан, и ответ намеренно пуст.
- **400 (НЕдопустимый запрос)** — запрос не распознан сервером.
- **404 (не найдено)** — запрошенный ресурс не существует на сервере.

### <a name="deleting-data"></a>Удаление данных

`HttpClient.DeleteAsync`Метод используется для отправки запроса на удаление в веб-службу, заданную с помощью URI, и последующего получения ответа от веб-службы, как показано в следующем примере кода:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  Uri uri = new Uri (string.Format (Constants.TodoItemsUrl, id));
  ...
  HttpResponseMessage response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully deleted.");
  }
  ...
}
```

Служба RESTFUL отправляет код состояния HTTP в `HttpResponseMessage.IsSuccessStatusCode` свойстве, чтобы указать, успешно ли был выполнен HTTP-запрос. Общие ответы для этой операции:

- **204 (нет содержимого)** — запрос успешно обработан, и ответ намеренно пуст.
- **400 (НЕдопустимый запрос)** — запрос не распознан сервером.
- **404 (не найдено)** — запрошенный ресурс не существует на сервере.

### <a name="local-development"></a>Локальная разработка

При разработке веб-службы RESTFUL локально с помощью такой платформы, как ASP.NET Core веб-API, можно одновременно выполнять отладку веб-службы и мобильного приложения. В этом сценарии необходимо включить HTTP-трафик с открытым текстом для эмулятора iOS симуалтор и Android. Сведения о настройке проекта для подключения см. в разделе [Подключение к локальным веб-службам](~/cross-platform/deploy-test/connect-to-local-web-services.md).

## <a name="related-links"></a>Связанные ссылки

- [Microsoft Learn: использование веб-служб RESTFUL в приложениях Xamarin](/learn/modules/consume-rest-services/)
- [Microsoft Learn: создание веб-API с помощью ASP.NET Core](/learn/modules/build-web-api-aspnet-core/)
- [Создание серверных служб для собственных мобильных приложений](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (пример)](/samples/xamarin/xamarin-forms-samples/webservices-todorest)
- [API HttpClient](xref:System.Net.Http.HttpClient)
- [Конфигурация безопасности сети Android](https://devblogs.microsoft.com/xamarin/cleartext-http-android-network-security/)
- [Безопасность транспорта приложений iOS](~/ios/app-fundamentals/ats.md)
- [Подключение к локальным веб-службам](~/cross-platform/deploy-test/connect-to-local-web-services.md)
