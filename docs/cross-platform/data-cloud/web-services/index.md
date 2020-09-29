---
title: Введение в веб-службы
description: В этом руководстве показано, как использовать различные технологии веб-служб. Рассматриваемые темы включают взаимодействие со службами RESTFUL, службами SOAP и службами Windows Communication Foundation.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 2131dd7611d802b8e1b1a22d1fbab42b5ac64746
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457969"
---
# <a name="introduction-to-web-services"></a>Введение в веб-службы

_В этом руководстве показано, как использовать различные технологии веб-служб. Рассматриваемые темы включают взаимодействие со службами RESTFUL, службами SOAP и службами Windows Communication Foundation._

Для правильной работы многие мобильные приложения зависят от облака, и поэтому интеграция веб-служб в мобильные приложения является распространенным сценарием. Платформа Xamarin поддерживает использование различных технологий веб-служб и включает в себя встроенную и сторонние службы для использования служб RESTFUL, ASMX и Windows Communication Foundation (WCF).

Для клиентов, использующих Xamarin. Forms, существуют полные примеры использования каждой из этих технологий в документации по [веб-службам Xamarin. Forms](~/xamarin-forms/data-cloud/index.yml) .

> [!IMPORTANT]
> В iOS 9 Защита транспорта приложений (ATS) обеспечивает безопасное подключение между Интернет-ресурсами (например, серверным сервером приложения) и приложением, тем самым предотвращая случайное раскрытие конфиденциальной информации.
> Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут подвергаться требованиям безопасности ATS. Если соединения не соответствуют этим требованиям, они завершатся сбоем с исключением.

Можно отказаться от ATS, если невозможно использовать `HTTPS` протокол и обеспечить безопасную связь для Интернет-ресурсов. Это можно сделать, обновив файл **info. plist** приложения. Дополнительные сведения см. в статье [Безопасность транспорта приложений](~/ios/app-fundamentals/ats.md).

## <a name="rest"></a>REST

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

Простота работы с другими позволила сделать ее основным методом доступа к веб-службам в мобильных приложениях.

## <a name="consuming-rest-services"></a>Использование служб RESTFUL

Существует несколько библиотек и классов, которые можно использовать для использования служб RESTFUL, а следующие подразделы обсуждают их. Дополнительные сведения об использовании службы RESTFUL см. [в статье Использование веб-службы RESTful](~/xamarin-forms/data-cloud/web-services/rest.md).

### <a name="httpclient"></a>HttpClient

[Клиентские библиотеки Microsoft HTTP](https://www.nuget.org/packages/Microsoft.Net.Http) предоставляют `HttpClient` класс, который используется для отправки и получения запросов по протоколу HTTP. Он предоставляет функциональные возможности для отправки HTTP-запросов и получения HTTP-ответов из ресурса, идентифицированного с помощью URI. Каждый запрос отправляется как асинхронная операция. Дополнительные сведения об асинхронных операциях см. в разделе [Общие сведения о поддержке асинхронных](~/cross-platform/platform/async.md)операций.

`HttpResponseMessage`Класс представляет сообщение HTTP-ответа, полученное от веб службы после выполнения запроса HTTP. Он содержит сведения о ответе, включая код состояния, заголовки и текст. `HttpContent` Класс представляет тело HTTP и заголовки содержимого, таких как `Content-Type` и `Content-Encoding`. Содержимое может быть считано с помощью любого из `ReadAs` методов, например `ReadAsStringAsync` и `ReadAsByteArrayAsync` , в зависимости от формата данных.

Дополнительные сведения о `HttpClient` классе см. в разделе [Создание объекта HttpClient](~/xamarin-forms/data-cloud/web-services/rest.md).

<a name="Using_HTTPWebRequest"></a>

### <a name="httpwebrequest"></a>HTTPWebRequest

Для вызова веб-служб `HTTPWebRequest` используется:

- Создание экземпляра запроса для конкретного URI.
- Задание различных свойств HTTP для экземпляра запроса.
- Получение  `HttpWebResponse` из запроса.
- Считывание данных из ответа.

Например, следующий код извлекает данные из национальной библиотеки США веб-службы медицина:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
request.ContentType = "application/json";
request.Method = "GET";

using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
  if (response.StatusCode != HttpStatusCode.OK)
     Console.Out.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
  using (StreamReader reader = new StreamReader(response.GetResponseStream()))
  {
               var content = reader.ReadToEnd();
               if(string.IsNullOrWhiteSpace(content)) {
                       Console.Out.WriteLine("Response contained empty body...");
               }
               else {
                       Console.Out.WriteLine("Response Body: \r\n {0}", content);
               }

               Assert.NotNull(content);
  }
}
```

В приведенном выше примере создается объект `HttpWebRequest` , который будет возвращать данные в формате JSON. Данные возвращаются в `HttpWebResponse` , из которого `StreamReader` можно получить для чтения данных.

<a name="Using_RESTSHARP"></a>

### <a name="restsharp"></a>RestSharp

Другой подход к использованию служб RESTFUL — использование библиотеки [рестшарп](http://restsharp.org/) . Рестшарп инкапсулирует HTTP-запросы, включая поддержку получения результатов в виде содержимого необработанной строки или в виде десериализованного объекта C#. Например, следующий код выполняет запрос к международной библиотеке медицина веб-службы США и получает результаты в виде строки в формате JSON:

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` — Это метод, который принимает необработанную строку JSON из `RestSharp.RestResponse.Content` Свойства и преобразует ее в объект C#. Десериализация данных, возвращаемых веб-службами, обсуждается далее в этой статье.

<a name="Using_NSUrlconnection"></a>

### <a name="nsurlconnection"></a>нсурлконнектион

Помимо классов, доступных в библиотеке базовых классов Mono (BCL), например `HttpWebRequest` , и сторонних библиотек C#, таких как рестшарп, классы для конкретных платформ также доступны для использования веб-служб. Например, в iOS `NSUrlConnection` `NSMutableUrlRequest` можно использовать классы и.

В следующем примере кода показано, как вызвать стандартную библиотеку США для веб-службы медицина с помощью классов iOS:

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
       NSUrlRequestCachePolicy.ReloadRevalidatingCacheData, 20);
request["Accept"] = "application/json";

var connectionDelegate = new RxTermNSURLConnectionDelegate();
var connection = new NSUrlConnection(request, connectionDelegate);
connection.Start();

public class RxTermNSURLConnectionDelegate : NSUrlConnectionDelegate
{
       StringBuilder _ResponseBuilder;
       public bool IsFinishedLoading { get; set; }
       public string ResponseContent { get; set; }

       public RxTermNSURLConnectionDelegate()
               : base()
       {
               _ResponseBuilder = new StringBuilder();
       }

       public override void ReceivedData(NSUrlConnection connection, NSData data)
       {
               if(data != null) {
                       _ResponseBuilder.Append(data.ToString());
               }
       }
       public override void FinishedLoading(NSUrlConnection connection)
       {
               IsFinishedLoading = true;
               ResponseContent = _ResponseBuilder.ToString();
       }
}
```

Как правило, классы платформы для использования веб-служб должны быть ограничены сценариями, в которых машинный код переносится в C#. Там, где это возможно, код доступа к веб-службе должен быть переносимым, чтобы его можно было совместно использовать на разных платформах.

<a name="Using_ServiceStack_Client"></a>

### <a name="servicestack"></a>ServiceStack

Другим вариантом вызова веб-служб является библиотека [стека обслуживания](https://servicestack.net) . Например, в следующем коде показано, как использовать метод стека службы `IServiceClient.GetAsync` для выдаче запроса на обслуживание:

```csharp
client.GetAsync<CustomersResponse>("",
          (response) => {
               foreach(var c in response.Customers) {
                       Console.WriteLine(c.CompanyName);
               }
       },
       (response, ex) => {
               Console.WriteLine(ex.Message);
       });
```

> [!IMPORTANT]
> Хотя такие средства, как ServiceStack и Рестшарп, упрощают вызов и использование служб RESTFUL, иногда нетривиальным является использование XML или JSON, который не соответствует стандартным соглашениям о сериализации _DataContract_ . При необходимости вызовите запрос и выполните соответствующую сериализацию явным образом с помощью библиотеки ServiceStack. Text, описанной ниже.

<a name="Options_for_consuming_RESTful_data"></a>

## <a name="consuming-restful-data"></a>Использование данных RESTFUL

Веб-службы RESTFUL обычно используют сообщения JSON для возврата данных клиенту. JSON — это текстовый формат обмена данными, создающий компактные полезные данные, что приводит к снижению требований к пропускной способности при отправке данных. В этом разделе будут проверяться механизмы использования ответов RESTFUL в JSON и обычных-Old-XML (POX).

<a name="Using_System.JSON"></a>

### <a name="systemjson"></a>System.JSНА

Платформа Xamarin поставляется без поддержки JSON. С помощью `JsonObject` можно получить результаты, как показано в следующем примере кода:

```csharp
var obj = JsonObject.Parse(json);
var properties = obj["rxtermsProperties"];
term.BrandName = properties["brandName"];
term.DisplayName = properties["displayName"];
term.Synonym = properties["synonym"];
term.FullName = properties["fullName"];
term.FullGenericName = properties["fullGenericName"];
term.Strength = properties["strength"];
```

Однако важно помнить, что `System.Json` средства полностью загружают данные в память.

<a name="Using_JSON.NET"></a>

### <a name="jsonnet"></a>JSON.NET

[Библиотека NewtonSoft JSON.NET](https://www.newtonsoft.com/json) — широко используемая библиотека для сериализации и десериализации сообщений JSON. В следующем примере кода показано, как использовать JSON.NET для десериализации сообщения JSON в объект C#:

```csharp
var term = new RxTerm();
var properties = JObject.Parse(json)["rxtermsProperties"];
term.BrandName = properties["brandName"].Value<string>();
term.DisplayName = properties["displayName"].Value<string>();
term.Synonym = properties["synonym"].Value<string>();;
term.FullName = properties["fullName"].Value<string>();;
term.FullGenericName = properties["fullGenericName"].Value<string>();;
term.Strength = properties["strength"].Value<string>();
term.RxCUI = properties["rxcui"].Value<string>();
```

<a name="Using_ServiceStack.Text"></a>

### <a name="servicestacktext"></a>ServiceStack. Text

ServiceStack. Text — это библиотека сериализации JSON, предназначенная для работы с библиотекой ServiceStack. В следующем примере кода показано, как выполнить синтаксический анализ JSON с помощью `ServiceStack.Text.JsonObject` :

```csharp
var result = JsonObject.Parse(json).Object("rxtermsProperties")
       .ConvertTo(x => new RxTerm {
               BrandName = x.Get("brandName"),
               DisplayName = x.Get("displayName"),
               Synonym = x.Get("synonym"),
               FullName = x.Get("fullName"),
               FullGenericName = x.Get("fullGenericName"),
               Strength = x.Get("strength"),
               RxTermDoseForm = x.Get("rxtermsDoseForm"),
               Route = x.Get("route"),
               RxCUI = x.Get("rxcui"),
               RxNormDoseForm = x.Get("rxnormDoseForm"),
       });
```

<a name="Using_System.Xml.Linq"></a>

### <a name="systemxmllinq"></a>System.Xml.Linq

В случае использования веб-службы RESTFUL на основе XML LINQ to XML можно использовать для анализа XML и заполнения встроенного объекта C#, как показано в следующем примере кода:

```csharp
var doc = XDocument.Parse(xml);
var result = doc.Root.Descendants("rxtermsProperties")
.Select(x=> new RxTerm()
       {
               BrandName = x.Element("brandName").Value,
               DisplayName = x.Element("displayName").Value,
               Synonym = x.Element("synonym").Value,
               FullName = x.Element("fullName").Value,
               FullGenericName = x.Element("fullGenericName").Value,
               //bind more here...
               RxCUI = x.Element("rxcui").Value,
       });
```

<a name="asmx"></a>

## <a name="aspnet-web-service-asmx"></a>Веб-служба ASP.NET (ASMX)

ASMX предоставляет возможность создавать веб-службы, которые отправляют сообщения с помощью протокола SOAP. SOAP — это независимый от платформы и не зависящий от языка протокол для создания веб-служб и доступа к ним. Потребители службы ASMX не должны знать ничего о платформе, объектной модели или языке программирования, используемом для реализации службы. Им нужно только разобраться, как отправлять и получать сообщения SOAP.

Сообщение SOAP — это XML-документ, содержащий следующие элементы:

- Корневой элемент с именем *конверт* , ОПРЕДЕЛЯЮЩИЙ XML-документ как сообщение SOAP.
- Необязательный элемент *заголовка* , содержащий сведения, относящиеся к приложению, такие как данные проверки подлинности. Если элемент *Header* представлен, он должен быть первым дочерним элементом элемента *конверта* .
- Обязательный элемент *Body* , СОДЕРЖАЩИЙ сообщение SOAP, предназначенное для получателя.
- Необязательный элемент *fault* , который используется для указания сообщений об ошибках. Если элемент *fault* имеется, он должен быть дочерним элементом элемента *Body* .

SOAP может использовать несколько транспортных протоколов, включая HTTP, SMTP, TCP и UDP. Однако служба ASMX может взаимодействовать только по протоколу HTTP. Платформа Xamarin поддерживает стандартные реализации SOAP 1,1 по протоколу HTTP и включает поддержку многих стандартных конфигураций службы ASMX.

### <a name="generating-a-proxy"></a>Создание учетной записи-посредника

Для использования службы ASMX необходимо создать *прокси-сервер* , который позволяет приложению подключаться к службе. Прокси-сервер создается путем использования метаданных службы, определяющих методы и конфигурацию связанной службы. Эти метаданные предоставляются в виде документа языка описания веб-служб (WSDL), созданного веб-службой. Прокси-сервер строится с помощью Visual Studio для Mac или Visual Studio, чтобы добавить веб-ссылку на веб – службу для проектов конкретной платформы.

URL веб-службы может быть размещенным удаленным источником или локальным ресурсом файловой системы, доступным по `file:///` префиксу пути, например:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![URL веб-службы может быть размещенным удаленным источником или локальным ресурсом файловой системы, доступным через префикс пути к файлу.](images/add-webreference-dialog.png)](images/add-webreference-dialog.png#lightbox)

При этом создается прокси-сервер в папке Web или Service References проекта. Так как прокси-сервер создан, он не должен изменяться.

<a name="Manually_adding_a_proxy_to_a_project"></a>

#### <a name="manually-adding-a-proxy-to-a-project"></a>Добавление прокси-сервера в проект вручную

При наличии существующего прокси-сервера, созданного с помощью совместимых средств, эти выходные данные можно использовать при включении в состав проекта. В Visual Studio для Mac используйте **Добавление файлов...** пункт меню, чтобы добавить прокси-сервер. Кроме того, для этого требуется явная ссылка на *System.Web.Services.dll* с помощью команды **Добавить ссылки...** .

### <a name="consuming-the-proxy"></a>Использование прокси-сервера

Созданные классы прокси предоставляют методы для использования веб-службы, использующей шаблон разработки модели асинхронного программирования (APM). В этом шаблоне асинхронная операция реализуется в виде двух методов с именами *бегиноператионнаме* и *ендоператионнаме*, которые начинают и завершают асинхронную операцию.

Метод *бегиноператионнаме* начинает асинхронную операцию и возвращает объект, реализующий `IAsyncResult` интерфейс. После вызова *бегиноператионнаме*приложение может продолжить выполнение инструкций в вызывающем потоке, в то время как асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *бегиноператионнаме*приложение должно также вызывать *ендоператионнаме* для получения результатов операции. Возвращаемое значение *ендоператионнаме* имеет тот же тип, что и синхронный метод веб-службы. В следующем коде показан пример такого действия:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Библиотека параллельных задач (TPL) может упростить процесс использования пары методов Begin и End APM, инкапсулирующие асинхронные операции в одном `Task` объекте. Это инкапсуляция обеспечивается несколькими перегрузками `Task.Factory.FromAsync` метода. Этот метод создает `Task` , который выполняет `TodoService.EndGetTodoItems` метод после `TodoService.BeginGetTodoItems` завершения метода с `null` параметром, указывающим, что данные не передаются в `BeginGetTodoItems` делегат. Наконец, значение `TaskCreationOptions` перечисления указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Дополнительные сведения о APM см. в статьях [асинхронная модель программирования](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm) и [TPL и традиционная .NET Framework асинхронное программирование](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming) на сайте MSDN.

Дополнительные сведения об использовании службы ASMX см. [в статье Использование веб-службы ASP.NET (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

<a name="wcf"></a>

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF — это единая платформа Майкрософт для создания приложений, ориентированных на службы. Она позволяет разработчикам создавать безопасные, надежные, транзакционные и совместимые распределенные приложения.

WCF описывает службу с множеством различных контрактов, которые включают следующее:

- **Контракты данных** — определяют структуры данных, которые формируют базу содержимого в сообщении.
- **Контракты сообщений** — Составление сообщений из существующих контрактов данных.
- **Контракты ошибок** — разрешить указывать пользовательские ошибки SOAP.
- **Контракты служб** — укажите операции, которые поддерживаются службами, и сообщения, необходимые для взаимодействия с каждой операцией. Они также указывают любое пользовательское поведение при сбое, которое может быть связано с операциями в каждой службе.

Существуют различия между ASP.NET Web Services (ASMX) и WCF, но важно понимать, что WCF поддерживает те же возможности, которые предоставляет ASMX — сообщения SOAP по протоколу HTTP.

> [!IMPORTANT]
> Поддержка платформы Xamarin для WCF ограничена текстовыми сообщениями SOAP по протоколу HTTP/HTTPS с помощью `BasicHttpBinding` класса. Кроме того, поддержка WCF требует использования средств, доступных только в среде Windows для создания учетной записи-посредника.

### <a name="generating-a-proxy"></a>Создание учетной записи-посредника

Для использования службы WCF необходимо создать *прокси-сервер* , который позволяет приложению подключаться к службе. Прокси-сервер создается путем использования метаданных службы, определяющих методы и связанную с ней конфигурацию службы. Эти метаданные представлены в виде документа языка описания веб-служб (WSDL), созданного веб-службой. Прокси-сервер можно построить с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017, чтобы добавить ссылку на службу для веб-службы в библиотеку .NET Standard.

Альтернативой созданию прокси-сервера с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 является использование средства служебной программы метаданных ServiceModel (svcutil.exe). Дополнительные сведения см. в разделе [средство служебной программы метаданных ServiceModel (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security"></a>

### <a name="configuring-the-proxy"></a>Настройка прокси-сервера

Настройка созданного прокси-сервера обычно принимает два аргумента конфигурации (в зависимости от SOAP 1.1/ASMX или WCF) во время инициализации: `EndpointAddress` и/или связанные сведения о привязке, как показано в следующем примере:

```csharp
var binding = new BasicHttpBinding () {
       Name= "basicHttpBinding",
       MaxReceivedMessageSize = 67108864,
};

binding.ReaderQuotas = new System.Xml.XmlDictionaryReaderQuotas() {
       MaxArrayLength = 2147483646,
       MaxStringContentLength = 5242880,
};

var timeout = new TimeSpan(0,1,0);
binding.SendTimeout= timeout;
binding.OpenTimeout = timeout;
binding.ReceiveTimeout = timeout;

client = new Service1Client (binding, new EndpointAddress ("http://192.168.1.100/Service1.svc"));
```

Привязка используется для указания транспорта, кодировки и сведений о протоколе, необходимых приложениям и службам для взаимодействия друг с другом. `BasicHttpBinding`Указывает, что сообщения SOAP, закодированные в виде текста, будут отправляться через транспортный протокол HTTP. Указание адреса конечной точки позволяет приложению подключаться к разным экземплярам службы WCF при условии, что существует несколько опубликованных экземпляров.

### <a name="consuming-the-proxy"></a>Использование прокси-сервера

Созданные классы прокси предоставляют методы для использования веб-служб, использующих шаблон разработки модели асинхронного программирования (APM). В этом шаблоне асинхронная операция реализуется в виде двух методов с именами *бегиноператионнаме* и *ендоператионнаме*, которые начинают и завершают асинхронную операцию.

Метод *бегиноператионнаме* начинает асинхронную операцию и возвращает объект, реализующий `IAsyncResult` интерфейс. После вызова *бегиноператионнаме*приложение может продолжить выполнение инструкций в вызывающем потоке, в то время как асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *бегиноператионнаме*приложение должно также вызывать *ендоператионнаме* для получения результатов операции. Возвращаемое значение *ендоператионнаме* имеет тот же тип, что и синхронный метод веб-службы. В следующем коде показан пример такого действия:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Библиотека параллельных задач (TPL) может упростить процесс использования пары методов Begin и End APM, инкапсулирующие асинхронные операции в одном `Task` объекте. Это инкапсуляция обеспечивается несколькими перегрузками `Task.Factory.FromAsync` метода. Этот метод создает `Task` , который выполняет `TodoServiceClient.EndGetTodoItems` метод после `TodoServiceClient.BeginGetTodoItems` завершения метода с `null` параметром, указывающим, что данные не передаются в `BeginGetTodoItems` делегат. Наконец, значение `TaskCreationOptions` перечисления указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Дополнительные сведения о APM см. в статьях [асинхронная модель программирования](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm) и [TPL и традиционная .NET Framework асинхронное программирование](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming) на сайте MSDN.

Дополнительные сведения об использовании службы WCF см. [в разделе Использование веб-службы Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/web-services/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security"></a>

#### <a name="using-transport-security"></a>Использование безопасности транспорта

Службы WCF могут применять безопасность транспортного уровня для защиты от перехвата сообщений. Платформа Xamarin поддерживает привязки, которые используют безопасность на транспортном уровне с помощью SSL. Однако могут возникнуть ситуации, в которых стеку может потребоваться проверить сертификат, что приводит к непредвиденному поведению. Проверка может быть переопределена путем регистрации `ServerCertificateValidationCallback` делегата перед вызовом службы, как показано в следующем примере кода:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Это обеспечивает шифрование транспорта при игнорировании проверки сертификата на стороне сервера. Однако этот подход эффективно игнорирует проблемы доверия, связанные с сертификатом, и может быть неуместной. Дополнительные сведения см. в разделе [using Trusted корни респектфулли](https://www.mono-project.com/UsingTrustedRootsRespectfully) On [Mono-Project.com](https://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security"></a>

#### <a name="using-client-credential-security"></a>Использование безопасности учетных данных клиента

Службы WCF также могут требовать, чтобы клиенты службы прошли проверку подлинности с помощью учетных данных. Платформа Xamarin не поддерживает протокол WS-Security, который позволяет клиентам передавать учетные данные внутри конверта сообщения SOAP. Однако платформа Xamarin поддерживает возможность отправки учетных данных обычной проверки подлинности HTTP на сервер, указав соответствующий параметр `ClientCredentialType` :

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Затем можно указать учетные данные обычной проверки подлинности:

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

Дополнительные сведения о обычной проверке подлинности HTTP, хотя в контексте веб-службы RESTFUL, см. в разделе [Проверка подлинности веб-службы RESTful](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="related-links"></a>Связанные ссылки

- [Веб-службы в Xamarin. Forms](~/xamarin-forms/data-cloud/index.yml)
- [Средство служебной программы метаданных ServiceModel (svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](/dotnet/api/system.servicemodel.basichttpbinding)