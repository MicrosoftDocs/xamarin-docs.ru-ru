---
Title: "использование базы данных документов Azure Cosmos DB в Xamarin.Forms " Description: "в этой статье объясняется, как использовать клиентскую библиотеку Azure Cosmos DB .NET Standard для интеграции базы данных документов Azure Cosmos DB в Xamarin.Forms приложение".
MS. произв. Xamarin MS. AssetID: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C MS. Technology: Xamarin-Forms MS. Custom: ксаму — автор видео: давидбритч MS. author: дабритч МС. Дата: 06/16/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="consume-an-azure-cosmos-db-document-database-in-xamarinforms"></a>Использование базы данных документов Azure Cosmos DB вXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)

_База данных документов Azure Cosmos DB — это база данных NoSQL, которая обеспечивает доступ к документам JSON с низкой задержкой, предлагая быструю, высокодоступную и масштабируемую службу баз данных для приложений, которым требуется эффективное масштабирование и Глобальная репликация. В этой статье объясняется, как использовать клиентскую библиотеку Azure Cosmos DB .NET Standard для интеграции базы данных документов Azure Cosmos DB в Xamarin.Forms приложение._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Видео Microsoft Azure Cosmos DB**

Учетную запись базы данных документов Azure Cosmos DB можно подготовить с помощью подписки Azure. Каждая учетная запись базы данных может иметь ноль или более баз данных. База данных документов в Azure Cosmos DB является логическим контейнером для коллекций документов и пользователей.

База данных документов Azure Cosmos DB может содержать ноль или более коллекций документов. Каждая коллекция документов может иметь другой уровень производительности, что позволяет указать дополнительную пропускную способность для часто используемых коллекций и меньше пропускной способности для нечасто запрашиваемых коллекций.

Каждая коллекция документов состоит из нуля или более документов JSON. Документы в коллекции не являются схемами, и поэтому не требуется совместно использовать одну и ту же структуру или поля. По мере добавления документов в коллекцию документов Cosmos DB автоматически индексирует их и становятся доступными для запроса.

В целях разработки базу данных документов также можно использовать в эмуляторе. С помощью эмулятора приложения можно разрабатывать и тестировать локально, не создавая подписку Azure или не тратя никаких затрат. Дополнительные сведения об эмуляторе см. в статье [Локальная разработка с помощью эмулятора Azure Cosmos DB](/azure/cosmos-db/local-emulator/).

В этой статье и прилагаемом примере приложения демонстрируется приложение списка дел, в котором задачи хранятся в Azure Cosmos DB базе данных документов. Дополнительные сведения о примере приложения см. в разделе [основные](~/xamarin-forms/data-cloud/web-services/introduction.md)сведения о примере.

Дополнительные сведения о Azure Cosmos DB см. в [документации по Azure Cosmos DB](/azure/cosmos-db/).

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

## <a name="setup"></a>Настройка

Процесс интеграции Azure Cosmos DB базы данных документов в приложение выглядит следующим образом Xamarin.Forms :

1. Создайте учетную запись Cosmos DB. Дополнительные сведения см. [в разделе Создание учетной записи Azure Cosmos DB](/azure/cosmos-db/sql-api-dotnetcore-get-started#create-an-azure-cosmos-account).
1. Добавьте пакет NuGet [Azure Cosmos DB .NET Standard клиентской библиотеки](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) в проекты платформы в Xamarin.Forms решении.
1. Добавьте `using` директивы для `Microsoft.Azure.Documents` `Microsoft.Azure.Documents.Client` пространств имен, и `Microsoft.Azure.Documents.Linq` в классы, которые будут обращаться к учетной записи Cosmos DB.

После выполнения этих действий можно использовать клиентскую библиотеку Azure Cosmos DB .NET Standard для настройки и выполнения запросов к базе данных документов.

> [!NOTE]
> Клиентскую библиотеку Azure Cosmos DB .NET Standard можно установить только в проекты платформы, а не в проект переносимой библиотеки классов (PCL). Поэтому пример приложения является проектом общего доступа (SAP), чтобы избежать дублирования кода. Однако `DependencyService` класс можно использовать в проекте PCL для вызова Azure Cosmos DB .NET Standard кода клиентской библиотеки, содержащегося в проектах для конкретных платформ.

## <a name="consuming-the-azure-cosmos-db-account"></a>Использование учетной записи Azure Cosmos DB

`DocumentClient`Тип инкапсулирует конечную точку, учетные данные и политику подключения, используемые для доступа к учетной записи Azure Cosmos DB, и используется для настройки и выполнения запросов к учетной записи. В следующем примере кода показано, как создать экземпляр этого класса:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Конструктору необходимо предоставить Cosmos DB универсальный код ресурса (URI) и первичный ключ `DocumentClient` . Их можно получить на портале Azure. Дополнительные сведения см. [в статье подключение к учетной записи Azure Cosmos DB](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect).

### <a name="creating-a-database"></a>Создание базы данных

База данных документов — это логический контейнер для коллекций документов и пользователей, который можно создать на портале Azure или программно с помощью `DocumentClient.CreateDatabaseIfNotExistsAsync` метода:

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

`CreateDatabaseIfNotExistsAsync`Метод задает `Database` объект в качестве аргумента с `Database` объектом, указывающим имя базы данных в качестве `Id` Свойства. `CreateDatabaseIfNotExistsAsync`Метод создает базу данных, если она не существует, или возвращает базу данных, если она уже существует. Однако в примере приложения игнорируются все данные, возвращаемые `CreateDatabaseIfNotExistsAsync` методом.

> [!NOTE]
> `CreateDatabaseIfNotExistsAsync`Метод возвращает `Task<ResourceResponse<Database>>` объект, и код состояния ответа можно проверить, чтобы определить, была ли создана база данных или была возвращена существующая база данных.

### <a name="creating-a-document-collection"></a>Создание коллекции документов

Коллекция документов — это контейнер для документов JSON, который можно создать на портале Azure или программно с помощью `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` метода:

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

`CreateDocumentCollectionIfNotExistsAsync`Метод требует два аргумента — имя базы данных, заданное как `Uri` , и `DocumentCollection` объект. `DocumentCollection`Объект представляет коллекцию документов, имя которой указано в `Id` свойстве. `CreateDocumentCollectionIfNotExistsAsync`Метод создает коллекцию документов, если она не существует, или возвращает коллекцию документов, если она уже существует. Однако в примере приложения игнорируются все данные, возвращаемые `CreateDocumentCollectionIfNotExistsAsync` методом.

> [!NOTE]
> `CreateDocumentCollectionIfNotExistsAsync`Метод возвращает `Task<ResourceResponse<DocumentCollection>>` объект, и код состояния ответа можно проверить, чтобы определить, была ли создана коллекция документов, или была возвращена существующая коллекция документов.

При необходимости `CreateDocumentCollectionIfNotExistsAsync` метод также может указывать `RequestOptions` объект, который инкапсулирует параметры, которые могут быть заданы для запросов, выданных учетной записи Cosmos DB. `RequestOptions.OfferThroughput`Свойство используется для определения уровня производительности коллекции документов, а в примере приложения — 400 единиц запросов в секунду. Это значение должно быть увеличено или уменьшено в зависимости от того, будет ли осуществляться доступ к коллекции часто или редко.

> [!IMPORTANT]
> Обратите внимание, что `CreateDocumentCollectionIfNotExistsAsync` метод создаст новую коллекцию с зарезервированной пропускной способностью, которая влияет на цены.

### <a name="retrieving-document-collection-documents"></a>Получение документов коллекции документов

Содержимое коллекции документов может быть извлечено путем создания и выполнения запроса документа. Запрос документа создается с помощью `DocumentClient.CreateDocumentQuery` метода:

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

Этот запрос асинхронно извлекает все документы из указанной коллекции и помещает документы в `List<TodoItem>` коллекцию для вывода.

`CreateDocumentQuery<T>`Метод задает `Uri` аргумент, представляющий коллекцию, к которой необходимо запросить документы. В этом примере `collectionLink` переменная является полем уровня класса, которое указывает `Uri` коллекцию документов для извлечения документов.

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

`CreateDocumentQuery<T>`Метод создает запрос, который выполняется синхронно и возвращает `IQueryable<T>` объект. Однако `AsDocumentQuery` метод преобразует `IQueryable<T>` объект в `IDocumentQuery<T>` объект, который может быть выполнен асинхронно. Асинхронный запрос выполняется с помощью `IDocumentQuery<T>.ExecuteNextAsync` метода, который извлекает следующую страницу результатов из базы данных документов со `IDocumentQuery<T>.HasMoreResults` свойством, указывающим, имеются ли дополнительные результаты, возвращаемые запросом.

Документы можно отфильтровать на стороне сервера, включив `Where` в запрос предложение, которое применяет предикат фильтрации к запросу к коллекции документов:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

Этот запрос получает все документы из коллекции, `Done` свойство которой равно `false` .

### <a name="inserting-a-document-into-a-document-collection"></a>Вставка документа в коллекцию документов

Документы — это определяемое пользователем содержимое JSON, которые можно вставить в коллекцию документов с помощью `DocumentClient.CreateDocumentAsync` метода:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

`CreateDocumentAsync`Метод задает `Uri` аргумент, представляющий коллекцию, в которую должен быть вставлен документ, и `object` аргумент, представляющий вставляемый документ.

### <a name="replacing-a-document-in-a-document-collection"></a>Замена документа в коллекции документов

Документы можно заменить в коллекции документов с помощью `DocumentClient.ReplaceDocumentAsync` метода:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

`ReplaceDocumentAsync`Метод задает `Uri` аргумент, представляющий документ в коллекции, который необходимо заменить, и `object` аргумент, представляющий обновленные данные документа.

### <a name="deleting-a-document-from-a-document-collection"></a>Удаление документа из коллекции документов

Документ можно удалить из коллекции документов с помощью `DocumentClient.DeleteDocumentAsync` метода:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

`DeleteDocumentAsync`Метод задает `Uri` аргумент, представляющий документ в коллекции, который необходимо удалить.

### <a name="deleting-a-document-collection"></a>Удаление коллекции документов

Коллекцию документов можно удалить из базы данных с помощью `DocumentClient.DeleteDocumentCollectionAsync` метода:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

`DeleteDocumentCollectionAsync`Метод задает `Uri` аргумент, представляющий удаляемую коллекцию документов. Обратите внимание, что при вызове этого метода будут также удалены документы, хранящиеся в коллекции.

### <a name="deleting-a-database"></a>Удаление базы данных

Базу данных можно удалить из учетной записи базы данных Cosmos DB с помощью `DocumentClient.DeleteDatabaesAsync` метода:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

`DeleteDatabaseAsync`Метод задает `Uri` аргумент, представляющий удаляемую базу данных. Обратите внимание, что при вызове этого метода будут также удалены коллекции документов, хранящиеся в базе данных, и документы, хранящиеся в коллекциях документов.

## <a name="summary"></a>Сводка

В этой статье объясняется, как использовать клиентскую библиотеку Azure Cosmos DB .NET Standard для интеграции базы данных документов Azure Cosmos DB в Xamarin.Forms приложение. База данных документов Azure Cosmos DB — это база данных NoSQL, которая обеспечивает доступ к документам JSON с низкой задержкой, предлагая быструю, высокодоступную и масштабируемую службу баз данных для приложений, которым требуется эффективное масштабирование и Глобальная репликация.

## <a name="related-links"></a>Связанные ссылки

- [Azure Cosmos DB ToDo (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)
- [Документация по Azure Cosmos DB](/azure/cosmos-db/)
- [Клиентская библиотека Azure Cosmos DB .NET Standard](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [API Azure Cosmos DB](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
