---
title: Поиск данных с помощью поиска Azure иXamarin.Forms
description: В этой статье показано, как использовать библиотеку поиска Microsoft Azure для интеграции службы поиска Azure в Xamarin.Forms приложение.
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 29e73f4051eda9117663992af9e710483e4b772b
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934099"
---
# <a name="search-data-with-azure-search-and-xamarinforms"></a>Поиск данных с помощью поиска Azure иXamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачайте пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)

_Поиск Azure — это облачная служба, которая предоставляет возможности индексирования и запросов для отправленных данных. При этом удаляются требования к инфраструктуре и сложные алгоритмы поиска, которые традиционно связаны с реализацией функции поиска в приложении. В этой статье показано, как использовать библиотеку поиска Microsoft Azure для интеграции службы поиска Azure в Xamarin.Forms приложение._

## <a name="overview"></a>Обзор

Данные хранятся в службе поиска Azure как индексы и документы. *Индекс* — это хранилище данных, которое может быть просмотрено службой поиска Azure и концептуально похоже на таблицу базы данных. *Документ* — это единая единица данных для поиска в индексе, которая концептуально напоминает строку базы данных. При передаче документов и отправке поисковых запросов в службу поиска Azure запросы выполняются по определенному индексу в службе "Поиск".

Каждый запрос, выполненный в службе поиска Azure, должен включать имя службы и ключ API. Существует два типа ключа API:

- *Ключи администратора* предоставляют полные права всем операциям. Это включает в себя управление службой, создание и удаление индексов и источников данных.
- *Ключи запроса* предоставляют доступ только для чтения к индексам и документам и должны использоваться приложениями, которые выдают запросы на поиск.

Наиболее распространенный запрос поиска Azure — выполнение запроса. Существует два типа запросов, которые могут быть отправлены:

- *Поисковый запрос выполняет* Поиск одного или нескольких элементов во всех полях с возможностью поиска в индексе. Поисковые запросы создаются с использованием упрощенного синтаксиса или синтаксиса запроса Lucene. Дополнительные сведения см. в статьях [простой синтаксис запросов в службе поиска Azure](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/)и [синтаксис запросов Lucene в службе поиска Azure](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/).
- Запрос *фильтра* вычисляет логическое выражение для всех фильтруемых полей в индексе. Запросы фильтра создаются с использованием подмножества языка фильтра OData. Дополнительные сведения см. в статье [синтаксис выражений OData для поиска Azure](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/).

Поисковые запросы и фильтровать запросы можно использовать отдельно или вместе. При совместном использовании запрос фильтра применяется сначала ко всему индексу, а затем поисковый запрос выполняется по результатам запроса фильтра.

Поиск Azure также поддерживает получение предложений на основе входных данных поиска. Дополнительные сведения см. в разделе [запросы предложений](#suggestion-queries).

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

## <a name="setup"></a>Установка

Процесс интеграции службы поиска Azure в Xamarin.Forms приложение выглядит следующим образом:

1. Создайте службу поиска Azure. Дополнительные сведения см. [в статье Создание службы поиска Azure с помощью портала Azure](/azure/search/search-create-service-portal/).
1. Удалите Silverlight как целевую платформу из Xamarin.Forms переносимой библиотеки классов (PCL) решения. Это можно сделать, изменив профиль PCL на любой профиль, поддерживающий кросс-платформенную разработку, но не поддерживающий Silverlight, например профиль 151 или профиль 92.
1. Добавьте Microsoft Azure пакет NuGet [библиотеки поиска](https://www.nuget.org/packages/Microsoft.Azure.Search) в проект PCL в Xamarin.Forms решении.

После выполнения этих действий API библиотеки Microsoft Search можно использовать для управления индексами поиска и источниками данных, отправки документов и управления ими, а также выполнения запросов.

## <a name="creating-the-azure-search-index"></a>Создание индекса поиска Azure

Должна быть определена схема индекса, сопоставленная с структурой данных для поиска. Это можно сделать на портале Azure или программно с помощью `SearchServiceClient` класса. Этот класс управляет подключениями к службе поиска Azure и может использоваться для создания индекса. В следующем примере кода показано, как создать экземпляр этого класса:

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient`Перегрузка конструктора принимает имя службы поиска и `SearchCredentials` объект в качестве аргументов, при этом объект создает `SearchCredentials` оболочку для *ключа администратора* службы поиска Azure. Для создания индекса требуется *ключ администратора* .

> [!NOTE]
> Один `SearchServiceClient` экземпляр следует использовать в приложении, чтобы не открывать слишком много подключений к службе поиска Azure.

Индекс определяется `Index` объектом, как показано в следующем примере кода:

```csharp
static void CreateSearchIndex()
{
  var index = new Index()
  {
    Name = Constants.Index,
    Fields = new[]
    {
      new Field("id", DataType.String) { IsKey = true, IsRetrievable = true },
      new Field("name", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("location", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("details", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSearchable = true },
      new Field("imageUrl", DataType.String) { IsRetrievable = true }
    },
    Suggesters = new[]
    {
      new Suggester("nameSuggester", SuggesterSearchMode.AnalyzingInfixMatching, new[] { "name" })
    }
  };

  searchClient.Indexes.Create(index);
}
```

`Index.Name`Свойству должно быть присвоено имя индекса, а `Index.Fields` свойству должно быть присвоено значение массива `Field` объектов. Каждый `Field` экземпляр задает имя, тип и все свойства, которые определяют способ использования поля. Эти свойства включают в себя:

- `IsKey`— Указывает, является ли поле ключом индекса. Только одно поле в индексе типа `DataType.String` должно быть назначено в качестве ключевого поля.
- `IsFacetable`— Указывает, возможно ли выполнение многоаспектной навигации по этому полю. Значение по умолчанию — `false`.
- `IsFilterable`— Указывает, можно ли использовать поле в запросах фильтра. Значение по умолчанию — `false`.
- `IsRetrievable`— Указывает, можно ли извлечь поле в результатах поиска. Значение по умолчанию — `true`.
- `IsSearchable`— Указывает, включено ли поле в полнотекстовый поиск. Значение по умолчанию — `false`.
- `IsSortable`— Указывает, можно ли использовать поле в `OrderBy` выражениях. Значение по умолчанию — `false`.

> [!NOTE]
> Изменение индекса после его развертывания включает в себя перестроение и повторную загрузку данных.

`Index`Объект может дополнительно указывать `Suggesters` свойство, которое определяет поля в индексе, используемые для поддержки автозавершения или запросов предложений поиска. `Suggesters`Для свойства необходимо задать массив `Suggester` объектов, определяющих поля, используемые для построения результатов поиска предложений.

После создания `Index` объекта индекс создается путем вызова метода `Indexes.Create` в `SearchServiceClient` экземпляре.

> [!NOTE]
> При создании индекса из приложения, которое должно быть откликом, используйте `Indexes.CreateAsync` метод.

Дополнительные сведения см. [в статье Создание индекса службы поиска Azure с помощью пакета SDK для .NET](/azure/search/search-create-index-dotnet/).

## <a name="deleting-the-azure-search-index"></a>Удаление индекса службы поиска Azure

Индекс можно удалить, вызвав метод `Indexes.Delete` в `SearchServiceClient` экземпляре:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Отправка данных в индекс службы поиска Azure

После определения индекса данные можно передать в него с помощью одной из двух моделей:

- **Модель извлечения** — данные периодически принимаются от Azure Cosmos DB, базы данных SQL Azure, хранилища BLOB-объектов azure или SQL Server, размещенных на виртуальной машине Azure.
- **Модель push-уведомлений** — данные программно отправляются в индекс. Это модель, принятая в этой статье.

`SearchIndexClient`Для импорта данных в индекс необходимо создать экземпляр. Это можно сделать, вызвав `SearchServiceClient.Indexes.GetClient` метод, как показано в следующем примере кода:

```csharp
static void UploadDataToSearchIndex()
{
  var indexClient = searchClient.Indexes.GetClient(Constants.Index);

  var monkeyList = MonkeyData.Monkeys.Select(m => new
  {
    id = Guid.NewGuid().ToString(),
    name = m.Name,
    location = m.Location,
    details = m.Details,
    imageUrl = m.ImageUrl
  });

  var batch = IndexBatch.New(monkeyList.Select(IndexAction.Upload));
  try
  {
    indexClient.Documents.Index(batch);
  }
  catch (IndexBatchException ex)
  {
    // Sometimes when the Search service is under load, indexing will fail for some
    // documents in the batch. Compensating actions like delaying and retrying should be taken.
    // Here, the failed document keys are logged.
    Console.WriteLine("Failed to index some documents: {0}",
      string.Join(", ", ex.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
  }
}
```

Данные, которые необходимо импортировать в индекс, упаковываются в виде `IndexBatch` объекта, который инкапсулирует коллекцию `IndexAction` объектов. Каждый `IndexAction` экземпляр содержит документ и свойство, которое сообщает Azure, какое действие следует выполнить с документом. В приведенном выше примере кода `IndexAction.Upload` указано действие, в результате чего документ вставляется в индекс, если он является новым, или заменяется, если он уже существует. `IndexBatch`Затем объект отправляется в индекс путем вызова `Documents.Index` метода для `SearchIndexClient` объекта. Дополнительные сведения о других действиях индексации см. [в разделе Выбор действия индексации для использования](/azure/search/search-import-data-dotnet#decide-which-indexing-action-to-use).

> [!NOTE]
> В один запрос индексирования можно включать только 1000 документов.

Обратите внимание, что в приведенном выше примере кода `monkeyList` коллекция создается как анонимный объект из коллекции `Monkey` объектов. Это приведет к созданию данных для `id` поля и разрешению сопоставления `Monkey` имен свойств case в стиле Pascal с именами полей индекса поиска Case. Кроме того, это сопоставление можно выполнить, добавив `[SerializePropertyNamesAsCamelCase]` атрибут к `Monkey` классу.

Дополнительные сведения см. [в статье отправка данных в службу поиска Azure с помощью пакета SDK для .NET](/azure/search/search-import-data-dotnet/).

## <a name="querying-the-azure-search-index"></a>Запрос индекса службы поиска Azure

`SearchIndexClient`Экземпляр должен быть создан для запроса индекса. Когда приложение выполняет запросы, рекомендуется следовать принципу минимальных привилегий и напрямую создавать его `SearchIndexClient` , передавая *ключ запроса* в качестве аргумента. Это гарантирует, что пользователи имеют доступ только для чтения к индексам и документам. Этот подход показан в следующем примере кода:

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient`Перегрузка конструктора принимает в качестве аргументов имя службы поиска, имя индекса и `SearchCredentials` объект, при этом `SearchCredentials` объект создает оболочку для *ключа запроса* службы поиска Azure.

### <a name="search-queries"></a>Поисковые запросы

Индекс можно запросить, вызвав `Documents.SearchAsync` метод в `SearchIndexClient` экземпляре, как показано в следующем примере кода:

```csharp
async Task AzureSearch(string text)
{
  Monkeys.Clear();

  var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text);
  foreach (SearchResult<Monkey> result in searchResults.Results)
  {
    Monkeys.Add(new Monkey
    {
      Name = result.Document.Name,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SearchAsync`Метод принимает текстовый аргумент поиска и необязательный `SearchParameters` объект, который можно использовать для дальнейшего уточнения запроса. Поисковый запрос задается в качестве аргумента текста поиска, а запрос фильтра можно задать, задав `Filter` свойство `SearchParameters` аргумента. В следующем примере кода показаны оба типа запросов:

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

Этот запрос фильтра применяется ко всему индексу и удаляет документы из результатов, в которых `location` поле не равно Китае, а не равно Вьетнам. После фильтрации поисковый запрос выполняется по результатам запроса фильтра.

> [!NOTE]
> Чтобы выполнить фильтрацию без поиска, передайте в `*` качестве аргумента Поиск текста.

`SearchAsync`Метод возвращает `DocumentSearchResult` объект, содержащий результаты запроса. Этот объект перечисляется, `Document` и каждый объект создается как `Monkey` объект и добавляется в `Monkeys` `ObservableCollection` для вывода. На следующих снимках экрана показаны результаты запроса поиска, возвращенные в службе поиска Azure:

![Результаты поиска](azure-search-images/search.png)

Дополнительные сведения о поиске и фильтрации см. [в статье запрос индекса службы поиска Azure с помощью пакета SDK для .NET](/azure/search/search-query-dotnet/).

### <a name="suggestion-queries"></a>Запросы предложений

Поиск Azure позволяет запрашивать предложения на основе поискового запроса, вызывая `Documents.SuggestAsync` метод для `SearchIndexClient` экземпляра. Это продемонстрировано в следующем примере кода:

```csharp
async Task AzureSuggestions(string text)
{
  Suggestions.Clear();

  var parameters = new SuggestParameters()
  {
    UseFuzzyMatching = true,
    HighlightPreTag = "[",
    HighlightPostTag = "]",
    MinimumCoverage = 100,
    Top = 10
  };

  var suggestionResults =
    await indexClient.Documents.SuggestAsync<Monkey>(text, "nameSuggester", parameters);

  foreach (var result in suggestionResults.Results)
  {
    Suggestions.Add(new Monkey
    {
      Name = result.Text,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SuggestAsync`Метод принимает текстовый аргумент поиска, имя используемого средства подбора (которое определено в индексе) и необязательный `SuggestParameters` объект, который можно использовать для дальнейшего уточнения запроса. `SuggestParameters`Экземпляр задает следующие свойства:

- `UseFuzzyMatching`— Если задано значение `true` , Поиск Azure будет искать предложения, даже если в искомом тексте есть замещающий или отсутствующий символ.
- `HighlightPreTag`— тег, добавленный в начало предложения.
- `HighlightPostTag`— тег, добавляемый к попаданию в предложение.
- `MinimumCoverage`— представляет процентную долю индекса, которая должна охватывать запрос предложения, чтобы запрос был успешно отправлен. Значение по умолчанию — 80.
- `Top`— число предложений для извлечения. Оно должно быть целым числом от 1 до 100 со значением по умолчанию 5.

Общий результат заключается в том, что первые 10 результатов из индекса будут возвращены с выделением совпадений, а результаты будут содержать документы, включающие аналогичные слова для поиска.

`SuggestAsync`Метод возвращает `DocumentSuggestResult` объект, содержащий результаты запроса. Этот объект перечисляется, `Document` и каждый объект создается как `Monkey` объект и добавляется в `Monkeys` `ObservableCollection` для вывода. На следующих снимках экрана показаны результаты предложения, возвращаемые службой поиска Azure:

![Результаты предложения](azure-search-images/suggest.png)

Обратите внимание, что в примере приложения `SuggestAsync` метод вызывается только после того, как пользователь завершает ввод условия поиска. Однако его также можно использовать для поддержки автоматического завершения поисковых запросов, выполняя при каждом нажатии клавиши.

## <a name="summary"></a>Итоги

В этой статье показано, как использовать библиотеку поиска Microsoft Azure для интеграции поиска Azure в Xamarin.Forms приложение. Поиск Azure — это облачная служба, которая предоставляет возможности индексирования и запросов для отправленных данных. При этом удаляются требования к инфраструктуре и сложные алгоритмы поиска, которые традиционно связаны с реализацией функции поиска в приложении.

## <a name="related-links"></a>Связанные ссылки

- [Поиск Azure (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)
- [Документация по поиску Azure](/azure/search/)
- [Библиотека поиска Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Search/)
