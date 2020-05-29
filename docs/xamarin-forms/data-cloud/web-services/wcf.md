---
title: ''
description: В этой статье показано, как использовать службу протокола SOAP из Xamarin.Forms приложения.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cf95427807e0179a608b428bc7e02499c9616fe7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139156"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Использование веб-службы Windows Communication Foundation (WCF)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF — это единая платформа Майкрософт для создания приложений, ориентированных на службы. Она позволяет разработчикам создавать безопасные, надежные, транзакционные и совместимые распределенные приложения. В этой статье показано, как использовать службу протокола SOAP из Xamarin.Forms приложения._

WCF описывает службу с различными контрактами, в том числе:

- **Контракты данных** — определяют структуры данных, которые формируют базу содержимого в сообщении.
- **Контракты сообщений** — Составление сообщений из существующих контрактов данных.
- **Контракты ошибок** — разрешить указывать пользовательские ошибки SOAP.
- **Контракты служб** — укажите операции, которые поддерживаются службами, и сообщения, необходимые для взаимодействия с каждой операцией. Они также указывают любое пользовательское поведение при сбое, которое может быть связано с операциями в каждой службе.

Существуют различия между ASP.NET Web Services (ASMX) и WCF, но WCF поддерживает те же возможности, которые предоставляет ASMX — сообщения SOAP по протоколу HTTP. Дополнительные сведения об использовании службы ASMX см. в разделе [Использование веб-служб ASP.NET (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md).

> [!IMPORTANT]
> Поддержка платформы Xamarin для WCF ограничена текстовыми сообщениями SOAP по протоколу HTTP/HTTPS с помощью `BasicHttpBinding` класса.
>
> Поддержка WCF требует использования средств, доступных в среде Windows, для создания прокси-сервера и размещения Тодовкфсервице. Для сборки и тестирования приложения iOS потребуется развернуть Тодовкфсервице на компьютере Windows или в качестве веб-службы Azure.
>
> Собственные приложения Xamarin Forms обычно используют код совместно с библиотекой классов .NET Standard. Однако .NET Core в настоящее время не поддерживает WCF, поэтому общий проект должен быть унаследованной переносимой библиотекой классов. Сведения о поддержке WCF в .NET Core см. в разделе [Выбор между .NET Core и .NET Framework для серверных приложений](/dotnet/standard/choosing-core-framework-server).

Решение примера приложения включает службу WCF, которая может быть запущена локально и показана на следующем снимке экрана:

![](wcf-images/portal.png "Sample Application")

> [!NOTE]
> В iOS 9 и более поздних версиях Защита транспорта приложений (ATS) обеспечивает безопасное подключение между Интернет-ресурсами (например, серверным сервером приложения) и приложением, тем самым предотвращая случайное раскрытие конфиденциальной информации. Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут подвергаться требованиям безопасности ATS. Если соединения не соответствуют этим требованиям, они завершатся сбоем с исключением.
>
> ATS можно отключать, если невозможно использовать `HTTPS` протокол и безопасное взаимодействие с Интернет-ресурсами. Это можно сделать, обновив файл **info. plist** приложения. Дополнительные сведения см. в статье [Безопасность транспорта приложений](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Использование веб-службы

Служба WCF предоставляет следующие операции:

|Операция|Описание|Параметры|
|--- |--- |--- |
|жеттодоитемс|Получение списка элементов задач|
|креатетодоитем|Создать новый элемент задачи|Сериализованный XML TodoItem|
|едиттодоитем|Обновление элемента задачи|Сериализованный XML TodoItem|
|делететодоитем|Удаление элемента задачи|Сериализованный XML TodoItem|

Дополнительные сведения о модели данных, используемой в приложении, см. в разделе [моделирование данных](~/xamarin-forms/data-cloud/web-services/introduction.md).

Для использования службы WCF необходимо создать *прокси-сервер* , который позволяет приложению подключаться к службе. Прокси-сервер создается путем использования метаданных службы, определяющих методы и связанную с ней конфигурацию службы. Эти метаданные представлены в виде документа языка описания веб-служб (WSDL), созданного веб-службой. Прокси-сервер можно построить с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017, чтобы добавить ссылку на службу для веб-службы в библиотеку .NET Standard. Альтернативой созданию прокси-сервера с помощью Microsoft WCF Web Service Reference Provider в Visual Studio 2017 является использование средства служебной программы метаданных ServiceModel (Svcutil. exe). Дополнительные сведения см. в разделе [средство служебной программы метаданных ServiceModel (Svcutil. exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Созданные классы прокси предоставляют методы для использования веб-служб, использующих шаблон разработки модели асинхронного программирования (APM). В этом шаблоне асинхронная операция реализуется в виде двух методов с именами *бегиноператионнаме* и *ендоператионнаме*, которые начинают и завершают асинхронную операцию.

Метод *бегиноператионнаме* начинает асинхронную операцию и возвращает объект, реализующий `IAsyncResult` интерфейс. После вызова *бегиноператионнаме*приложение может продолжить выполнение инструкций в вызывающем потоке, в то время как асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *бегиноператионнаме*приложение должно также вызывать *ендоператионнаме* для получения результатов операции. Возвращаемое значение *ендоператионнаме* имеет тот же тип, что и синхронный метод веб-службы. Например, `EndGetTodoItems` метод возвращает коллекцию `TodoItem` экземпляров. Метод *ендоператионнаме* также включает `IAsyncResult` параметр, который должен быть установлен в экземпляр, возвращаемый соответствующим вызовом метода *бегиноператионнаме* .

Библиотека параллельных задач (TPL) может упростить процесс использования пары методов Begin и End APM, инкапсулирующие асинхронные операции в одном `Task` объекте. Это инкапсуляция обеспечивается несколькими перегрузками `TaskFactory.FromAsync` метода.

Дополнительные сведения о APM см. в разделе [асинхронная модель программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [TPL и традиционная .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на MSDN.

### <a name="create-the-todoserviceclient-object"></a>Создание объекта Тодосервицеклиент

Созданный прокси-класс предоставляет `TodoServiceClient` класс, который используется для взаимодействия со службой WCF по протоколу HTTP. Он предоставляет функциональные возможности для вызова методов веб-службы как асинхронных операций из определенного экземпляра службы URI. Дополнительные сведения об асинхронных операциях см. в разделе [Общие сведения о поддержке асинхронных](~/cross-platform/platform/async.md)операций.

`TodoServiceClient`Экземпляр объявляется на уровне класса, чтобы объект находился в течение всего времени, когда приложению требуется использовать службу WCF, как показано в следующем примере кода:

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

Для `TodoServiceClient` экземпляра настраивается информация о привязке и адрес конечной точки. Привязка используется для указания транспорта, кодировки и сведений о протоколе, необходимых приложениям и службам для взаимодействия друг с другом. `BasicHttpBinding`Указывает, что сообщения SOAP, закодированные в виде текста, будут отправляться через транспортный протокол HTTP. Указание адреса конечной точки позволяет приложению подключаться к разным экземплярам службы WCF при условии, что существует несколько опубликованных экземпляров.

Дополнительные сведения о настройке ссылки на службу см. [в разделе Настройка ссылки на службу](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="create-data-transfer-objects"></a>Создание объектов для обмена данными

Пример приложения использует `TodoItem` класс для моделирования данных. Чтобы сохранить `TodoItem` элемент в веб-службе, необходимо сначала преобразовать его в тип, созданный прокси-сервером `TodoItem` . Это достигается `ToWCFServiceTodoItem` методом, как показано в следующем примере кода:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Этот метод просто создает новый `TodoWCFService.TodoItem` экземпляр и присваивает каждому свойству идентичное свойство от `TodoItem` экземпляра.

Аналогично, когда данные извлекаются из веб-службы, их необходимо преобразовать из типа, созданного прокси-сервером, `TodoItem` в `TodoItem` экземпляр. Это достигается с помощью `FromWCFServiceTodoItem` метода, как показано в следующем примере кода:

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

Этот метод просто извлекает данные из типа, созданного прокси-сервером `TodoItem` , и задает его в новом `TodoItem` экземпляре.

### <a name="retrieve-data"></a>Извлечение данных

`TodoServiceClient.BeginGetTodoItems`Методы и `TodoServiceClient.EndGetTodoItems` используются для вызова `GetTodoItems` операции, предоставляемой веб-службой. Эти асинхронные методы инкапсулируются в `Task` объекте, как показано в следующем примере кода:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems)
  {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync`Метод создает `Task` , который выполняет `TodoServiceClient.EndGetTodoItems` метод после `TodoServiceClient.BeginGetTodoItems` завершения метода, с `null` параметром, указывающим, что данные не передаются в `BeginGetTodoItems` делегат. Наконец, значение `TaskCreationOptions` перечисления указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

`TodoServiceClient.EndGetTodoItems`Метод возвращает `ObservableCollection` коллекцию `TodoWCFService.TodoItem` экземпляров, которые затем преобразуются в `List` `TodoItem` экземпляры для вывода.

### <a name="create-data"></a>Создание данных

`TodoServiceClient.BeginCreateTodoItem`Методы и `TodoServiceClient.EndCreateTodoItem` используются для вызова `CreateTodoItem` операции, предоставляемой веб-службой. Эти асинхронные методы инкапсулируются в `Task` объекте, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`Метод создает `Task` , который выполняет `TodoServiceClient.EndCreateTodoItem` метод после `TodoServiceClient.BeginCreateTodoItem` завершения метода, а `todoItem` параметр — данные, передаваемые в делегат, чтобы указать объект, который должен `BeginCreateTodoItem` `TodoItem` быть создан веб-службой. Наконец, значение `TaskCreationOptions` перечисления указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Веб-служба создает исключение `FaultException` , если не удается создать объект `TodoItem` , который обрабатывается приложением.

### <a name="update-data"></a>Обновление данных

`TodoServiceClient.BeginEditTodoItem`Методы и `TodoServiceClient.EndEditTodoItem` используются для вызова `EditTodoItem` операции, предоставляемой веб-службой. Эти асинхронные методы инкапсулируются в `Task` объекте, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`Метод создает `Task` , который выполняет `TodoServiceClient.EndEditTodoItem` метод после `TodoServiceClient.BeginCreateTodoItem` завершения метода, а `todoItem` параметр — это данные, передаваемые в `BeginEditTodoItem` делегат для указания `TodoItem` обновляемой веб-службой. Наконец, значение `TaskCreationOptions` перечисления указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Веб-служба создает исключение `FaultException` , если не удается выполнить обнаружение или обновление `TodoItem` , которое обрабатывается приложением.

### <a name="delete-data"></a>Удаление данных

`TodoServiceClient.BeginDeleteTodoItem`Методы и `TodoServiceClient.EndDeleteTodoItem` используются для вызова `DeleteTodoItem` операции, предоставляемой веб-службой. Эти асинхронные методы инкапсулируются в `Task` объекте, как показано в следующем примере кода:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`Метод создает `Task` , который выполняет `TodoServiceClient.EndDeleteTodoItem` метод после `TodoServiceClient.BeginDeleteTodoItem` завершения метода, а `id` параметр — данные, передаваемые в делегат, чтобы указать объект, который должен `BeginDeleteTodoItem` `TodoItem` быть удален веб-службой. Наконец, значение `TaskCreationOptions` перечисления указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Веб-служба создает исключение `FaultException` , если не удается выполнить обнаружение или удаление `TodoItem` , которое обрабатывается приложением.

## <a name="configure-remote-access-to-iis-express"></a>Настройка удаленного доступа к IIS Express
В Visual Studio 2017 или Visual Studio 2019 вы сможете протестировать приложение UWP на ПК без дополнительной настройки. Для тестирования клиентов Android и iOS могут потребоваться дополнительные действия в этом разделе. Дополнительные сведения см. [в статье подключение к локальным веб-службам из симуляторов iOS и эмуляторов Android](~/cross-platform/deploy-test/connect-to-local-web-services.md) .

По умолчанию IIS Express будет отвечать только на запросы к `localhost` . Удаленные устройства (например, устройства Android, iPhone или даже симулятор) не будут иметь доступа к локальной службе WCF. Вам нужно будет знать IP-адрес рабочей станции Windows 10 в локальной сети. В этом примере предположим, что у рабочей станции есть IP-адрес `192.168.1.143` . Ниже описаны действия по настройке Windows 10 и IIS Express для приема удаленных подключений и подключения к службе из физического или виртуального устройства.

1. **Добавьте исключение в брандмауэр Windows**. Необходимо открыть порт через брандмауэр Windows, который приложения в подсети могут использовать для взаимодействия со службой WCF. Создайте правило входящего трафика, открыв порт 49393 в брандмауэре. В командной строке администратора выполните следующую команду:

    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **Настройте IIS Express для приема удаленных подключений**. IIS Express можно настроить, изменив файл конфигурации для IIS Express в **папке [каталог решения] \. вс\конфиг\аппликатионхост.конфиг**. Найдите `site` элемент с именем `TodoWCFService` . Он должен выглядеть, как в следующем коде XML:

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
        </bindings>
    </site>
    ```

    Вам потребуется добавить два элемента, `binding` чтобы открыть порт 49393 для внешнего трафика и эмулятора Android. Привязка использует `[IP address]:[port]:[hostname]` Формат, указывающий, как IIS Express будет отвечать на запросы. Внешние запросы будут иметь имена узлов, которые должны быть указаны в виде `binding` . Добавьте следующий XML-код в `bindings` элемент, заменив IP-адрес своим IP-адресом:

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    После внесения изменений `bindings` элемент должен выглядеть следующим образом:

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
            <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
            <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
        </bindings>
    </site>
    ```

    >[!IMPORTANT]
    >По умолчанию IIS Express не будет принимать подключения из внешних источников по соображениям безопасности. Чтобы разрешить подключения с удаленных устройств, необходимо запустить IIS Express с разрешениями администратора. Самый простой способ сделать это — запустить Visual Studio 2017 с правами администратора. Это приведет к запуску IIS Express с правами администратора при запуске Тодовкфсервице.

    После выполнения этих действий вы сможете запустить Тодовкфсервице и подключиться с других устройств в подсети. Это можно проверить, запустив приложение и посетив страницу `http://localhost:49393/TodoService.svc` . Если при посещении этого URL-адреса появляется сообщение об ошибке **неправильного запроса** , `bindings` возможно, в IIS Express конфигурации возникла ошибка (запрос приближается IIS Express но отклоняется). Если возникает другая ошибка, возможно, приложение не работает или брандмауэр настроен неправильно.

    Чтобы разрешить IIS Express продолжать работу и обслуживать службу, отключите параметр " **изменить и продолжить** " в **свойствах проекта > отладчики веб->**.

1. **Настройте конечные устройства для доступа к службе**. Этот шаг включает настройку клиентского приложения, работающего на физическом или эмулированного устройстве, для доступа к службе WCF.

    Эмулятор Android использует внутренний прокси-сервер, который не защищает эмулятор от прямого доступа к `localhost` адресу главного компьютера. Вместо этого адрес `10.0.2.2` эмулятора направляется на `localhost` главный компьютер через внутренний прокси-сервер. Эти прокси-запросы будут иметь в `127.0.0.1` качестве имени узла в заголовке запроса, поэтому вы создали привязку IIS Express для этого имени узла в описанных выше шагах.

    Симулятор iOS выполняется на узле сборки Mac, даже если вы используете [Удаленный симулятор iOS для Windows](~/tools/ios-simulator/index.md). Сетевые запросы от симулятора будут иметь IP-адрес рабочей станции в локальной сети в качестве имени узла (в данном примере это `192.168.1.143` , но фактический IP-адрес будет отличаться). Именно поэтому вы создали привязку IIS Express для этого имени узла, как описано выше.

    Убедитесь `SoapUrl` , что свойство в файле **Constants.CS** в проекте тодовкф (Portable) имеет правильные значения для вашей сети:

    ```csharp
    public static string SoapUrl
    {
        get
        {
            var defaultUrl = "http://localhost:49393/TodoService.svc";

            if (Device.RuntimePlatform == Device.Android)
            {
                defaultUrl = "http://10.0.2.2:49393/TodoService.svc";
            }
            else if (Device.RuntimePlatform == Device.iOS)
            {
                defaultUrl = "http://192.168.1.143:49393/TodoService.svc";
            }

            return defaultUrl;
        }
    }
    ```

    После настройки **Constants.CS** с соответствующими конечными точками вы сможете подключиться к тодовкфсервице, работающему на рабочей станции Windows 10, с физических или виртуальных устройств.

## <a name="related-links"></a>Связанные ссылки

- [Тодовкф (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [Практическое руководство. Создание клиента Windows Communication Foundation](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Средство служебной программы метаданных ServiceModel (Svcutil. exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
