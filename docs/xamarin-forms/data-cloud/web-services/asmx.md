---
title: ''
description: В этой статье показано, как использовать службу ASMX SOAP из Xamarin.Forms приложения.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1f7a0d04d1e7b6abc9931c05c0e46ef49f8ba09c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138467"
---
# <a name="consume-an-aspnet-web-service-asmx"></a>Использование веб-службы ASP.NET (ASMX)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)

_ASMX предоставляет возможность создавать веб-службы, которые отправляют сообщения с помощью протокола SOAP. SOAP — это независимый от платформы и не зависящий от языка протокол для создания веб-служб и доступа к ним. Потребители службы ASMX не должны знать ничего о платформе, объектной модели или языке программирования, используемом для реализации службы. Им нужно только разобраться, как отправлять и получать сообщения SOAP. В этой статье показано, как использовать службу ASMX SOAP из Xamarin.Forms приложения._

Сообщение SOAP — это XML-документ, содержащий следующие элементы:

- Корневой элемент с именем *конверт* , ОПРЕДЕЛЯЮЩИЙ XML-документ как сообщение SOAP.
- Необязательный элемент *заголовка* , содержащий сведения, относящиеся к приложению, такие как данные проверки подлинности. Если элемент *Header* представлен, он должен быть первым дочерним элементом элемента *конверта* .
- Обязательный элемент *Body* , СОДЕРЖАЩИЙ сообщение SOAP, предназначенное для получателя.
- Необязательный элемент *fault* , который используется для указания сообщений об ошибках. Если элемент *fault* имеется, он должен быть дочерним элементом элемента *Body* .

SOAP может использовать несколько транспортных протоколов, включая HTTP, SMTP, TCP и UDP. Однако служба ASMX может взаимодействовать только по протоколу HTTP. Платформа Xamarin поддерживает стандартные реализации SOAP 1,1 по протоколу HTTP и включает поддержку многих стандартных конфигураций службы ASMX.

Этот пример включает мобильные приложения, работающие на физических или эмулированных устройствах, и службу ASMX, которая предоставляет методы для получения, добавления, изменения и удаления данных. При запуске мобильных приложений они подключаются к размещенной локально службе ASMX, как показано на следующем снимке экрана:

![](asmx-images/portal.png "Sample Application")

> [!NOTE]
> В iOS 9 и более поздних версиях Защита транспорта приложений (ATS) обеспечивает безопасное подключение между Интернет-ресурсами (например, серверным сервером приложения) и приложением, тем самым предотвращая случайное раскрытие конфиденциальной информации. Поскольку ATS включена по умолчанию в приложениях, созданных для iOS 9, все подключения будут подвергаться требованиям безопасности ATS. Если соединения не соответствуют этим требованиям, они завершатся сбоем с исключением.
> ATS можно отключать, если невозможно использовать `HTTPS` протокол и безопасное взаимодействие с Интернет-ресурсами. Это можно сделать, обновив файл **info. plist** приложения. Дополнительные сведения см. в статье [Безопасность транспорта приложений](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Использование веб-службы

Служба ASMX предоставляет следующие операции:

|Операция|Описание|Параметры|
|--- |--- |--- |
|жеттодоитемс|Получение списка элементов задач|
|креатетодоитем|Создать новый элемент задачи|Сериализованный XML TodoItem|
|едиттодоитем|Обновление элемента задачи|Сериализованный XML TodoItem|
|делететодоитем|Удаление элемента задачи|Сериализованный XML TodoItem|

Дополнительные сведения о модели данных, используемой в приложении, см. в разделе [моделирование данных](~/xamarin-forms/data-cloud/web-services/introduction.md).

## <a name="create-the-todoservice-proxy"></a>Создание прокси-сервера файле todoservice

Класс прокси, вызываемый `TodoService` , расширяет `SoapHttpClientProtocol` и предоставляет методы для взаимодействия со службой ASMX по протоколу HTTP. Прокси-сервер создается путем добавления веб-ссылки на каждый проект конкретной платформы в Visual Studio 2019 или Visual Studio 2017. Веб-ссылка создает методы и события для каждого действия, определенного в документе языка описания веб-служб (WSDL) службы.

Например, `GetTodoItems` действие службы приводит к `GetTodoItemsAsync` методу и `GetTodoItemsCompleted` событию в прокси-сервере. Созданный метод имеет тип возвращаемого значения void и вызывает `GetTodoItems` действие для родительского `SoapHttpClientProtocol` класса. Когда вызванный метод получает ответ от службы, он запускает `GetTodoItemsCompleted` событие и предоставляет данные ответа в `Result` свойстве события.

## <a name="create-the-isoapservice-implementation"></a>Создание реализации Исоапсервице

Чтобы обеспечить работу многоплатформенного проекта для работы со службой, в примере определяется `ISoapService` интерфейс, который следует за [моделью асинхронного программирования задач в C#](/dotnet/csharp/programming-guide/concepts/async/). Каждая платформа реализует `ISoapService` для предоставления прокси-сервера, зависящего от платформы. В примере используются `TaskCompletionSource` объекты для предоставления прокси в качестве асинхронного интерфейса задачи. Сведения об использовании `TaskCompletionSource` приведены в реализациях каждого типа действия в следующих разделах.

Пример `SoapService` :

1. Создает экземпляр в `TodoService` качестве экземпляра уровня класса
1. Создает коллекцию, вызываемую `Items` для хранения `TodoItem` объектов
1. Указывает пользовательскую конечную точку для необязательного `Url` свойства в`TodoService`

```csharp
public class SoapService : ISoapService
{
    ASMXService.TodoService todoService;
    public List<TodoItem> Items { get; private set; } = new List<TodoItem>();

    public SoapService ()
    {
        todoService = new ASMXService.TodoService ();
        todoService.Url = Constants.SoapUrl;
        ...
    }
}
```

### <a name="create-data-transfer-objects"></a>Создание объектов для обмена данными

Пример приложения использует `TodoItem` класс для моделирования данных. Чтобы сохранить `TodoItem` элемент в веб-службе, необходимо сначала преобразовать его в тип, созданный прокси-сервером `TodoItem` . Это достигается `ToASMXServiceTodoItem` методом, как показано в следующем примере кода:

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
    return new ASMXService.TodoItem {
        ID = item.ID,
        Name = item.Name,
        Notes = item.Notes,
        Done = item.Done
    };
}
```

Этот метод создает новый `ASMService.TodoItem` экземпляр и присваивает каждому свойству идентичное свойство из `TodoItem` экземпляра.

Аналогично, когда данные извлекаются из веб-службы, их необходимо преобразовать из типа, созданного прокси-сервером, `TodoItem` в `TodoItem` экземпляр. Это достигается с помощью `FromASMXServiceTodoItem` метода, как показано в следующем примере кода:

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
{
    return new TodoItem {
        ID = item.ID,
        Name = item.Name,
        Notes = item.Notes,
        Done = item.Done
    };
}
```

Этот метод извлекает данные из типа, созданного прокси-сервером `TodoItem` , и задает его в созданном `TodoItem` экземпляре.

### <a name="retrieve-data"></a>Извлечение данных

`ISoapService`Интерфейсу `RefreshDataAsync` требуется, чтобы метод возвращал объект `Task` со коллекцией элементов. Однако `TodoService.GetTodoItemsAsync` метод возвращает значение void. Чтобы удовлетворить шаблон интерфейса, необходимо вызвать `GetTodoItemsAsync` , дождаться `GetTodoItemsCompleted` срабатывания события и заполнить коллекцию. Это позволяет вернуть в пользовательский интерфейс допустимую коллекцию.

Приведенный ниже пример создает новый `TaskCompletionSource` , начинает асинхронный вызов в `RefreshDataAsync` методе и ожидает объект, `Task` предоставленный `TaskCompletionSource` . При `TodoService_GetTodoItemsCompleted` вызове обработчика события он заполняет `Items` коллекцию и обновляет `TaskCompletionSource` :

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> getRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.GetTodoItemsCompleted += TodoService_GetTodoItemsCompleted;
    }

    public async Task<List<TodoItem>> RefreshDataAsync()
    {
        getRequestComplete = new TaskCompletionSource<bool>();
        todoService.GetTodoItemsAsync();
        await getRequestComplete.Task;
        return Items;
    }

    private void TodoService_GetTodoItemsCompleted(object sender, ASMXService.GetTodoItemsCompletedEventArgs e)
    {
        try
        {
            getRequestComplete = getRequestComplete ?? new TaskCompletionSource<bool>();

            Items = new List<TodoItem>();
            foreach (var item in e.Result)
            {
                Items.Add(FromASMXServiceTodoItem(item));
            }
            getRequestComplete?.TrySetResult(true);
        }
        catch (Exception ex)
        {
            Debug.WriteLine(@"\t\tERROR {0}", ex.Message);
        }
    }

    ...
}
```

Дополнительные сведения см. в статьях [асинхронная модель программирования](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm) и [TPL и традиционная .NET Framework асинхронное программирование](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming).

### <a name="create-or-edit-data"></a>Создание или изменение данных

При создании или изменении данных необходимо реализовать `ISoapService.SaveTodoItemAsync` метод. Этот метод определяет, `TodoItem` является ли элемент новым или обновленным и вызывает соответствующий метод для `todoService` объекта. `CreateTodoItemCompleted` `EditTodoItemCompleted` Обработчики событий и также должны быть реализованы, чтобы вы узнали, когда `todoService` получил ответ от службы ASMX (они могут быть объединены в один обработчик, так как они выполняют одну и ту же операцию). В следующем примере демонстрируются реализации интерфейса и обработчика событий, а также объект, `TaskCompletionSource` используемый для асинхронной обработки:

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> saveRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.CreateTodoItemCompleted += TodoService_SaveTodoItemCompleted;
        todoService.EditTodoItemCompleted += TodoService_SaveTodoItemCompleted;
    }

    public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
    {
        try
        {
            var todoItem = ToASMXServiceTodoItem(item);
            saveRequestComplete = new TaskCompletionSource<bool>();
            if (isNewItem)
            {
                todoService.CreateTodoItemAsync(todoItem);
            }
            else
            {
                todoService.EditTodoItemAsync(todoItem);
            }
            await saveRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_SaveTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        saveRequestComplete?.TrySetResult(true);
    }

    ...
}
```

### <a name="delete-data"></a>Удаление данных

Для удаления данных требуется аналогичная реализация. Определите `TaskCompletionSource` , реализуйте обработчик событий и `ISoapService.DeleteTodoItemAsync` метод:

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> deleteRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.DeleteTodoItemCompleted += TodoService_DeleteTodoItemCompleted;
    }

    public async Task DeleteTodoItemAsync (string id)
    {
        try
        {
            deleteRequestComplete = new TaskCompletionSource<bool>();
            todoService.DeleteTodoItemAsync(id);
            await deleteRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_DeleteTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        deleteRequestComplete?.TrySetResult(true);
    }

    ...
}
```

## <a name="test-the-web-service"></a>Тестирование веб-службы

Для тестирования физических или эмулированных устройств с помощью локально размещенной службы требуются пользовательские конфигурации IIS, адреса конечных точек и правила брандмауэра. Дополнительные сведения о настройке среды для тестирования см. в разделе [Настройка удаленного доступа к IIS Express](wcf.md#configure-remote-access-to-iis-express). Единственное различие между тестированием WCF и ASMX — номер порта файле todoservice.

## <a name="related-links"></a>Связанные ссылки

- [Тодоасмкс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [Объектом](https://docs.microsoft.com/dotnet/api/system.iasyncresult)
