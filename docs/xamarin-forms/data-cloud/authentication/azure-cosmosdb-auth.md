---
title: Проверка подлинности пользователей с помощью Azure Cosmos DB базы данных документов иXamarin.Forms
description: В этой статье объясняется, как объединить контроль доступа с Azure Cosmos DB секционированными коллекциями, чтобы пользователь мог получить доступ к собственным документам в Xamarin.Forms приложении.
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 05547e960ba1ea141a830396f803dfc265283627
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936465"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Проверка подлинности пользователей с помощью Azure Cosmos DB базы данных документов иXamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Базы данных документов Azure Cosmos DB поддерживают секционированные коллекции, которые могут охватывать несколько серверов и секций при поддержке неограниченного объема хранилища и пропускной способности. В этой статье объясняется, как объединить контроль доступа с секционированными коллекциями, чтобы пользователь мог получить доступ к собственным документам в Xamarin.Forms приложении._

## <a name="overview"></a>Обзор

При создании секционированной коллекции необходимо указать ключ секции, а документы с тем же ключом секции будут храниться в одной секции. Таким образом, указание удостоверения пользователя в качестве ключа секции приведет к созданию секционированной коллекции, в которой будут храниться только документы для этого пользователя. Это также гарантирует, что Azure Cosmos DB База данных документов будет масштабироваться по мере увеличения числа пользователей и элементов.

Доступ должен быть предоставлен любой коллекции, а модель управления доступом API SQL определяет два типа конструкций доступа:

- **Главные ключи** обеспечивают полный административный доступ ко всем ресурсам в учетной записи Cosmos DB и создаются при создании учетной записи Cosmos DB.
- **Маркеры ресурсов** захватывают связь между пользователем базы данных и разрешениями, которые пользователь имеет для конкретного Cosmos DB ресурса, например коллекции или документа.

Предоставление доступа к главному ключу открывает учетную запись Cosmos DB для использования злоумышленником или небрежного. Однако Azure Cosmos DB маркеры ресурсов предоставляют надежный механизм, позволяющий клиентам читать, записывать и удалять определенные ресурсы в учетной записи Azure Cosmos DB в соответствии с предоставленными разрешениями.

Типичный подход к запросу, созданию и доставке маркеров ресурсов мобильному приложению — использование брокера маркера ресурса. На следующей схеме показан общий обзор того, как пример приложения использует брокер маркера ресурса для управления доступом к данным в базе данных документов.

![Документирование процесса проверки подлинности базы данных](azure-cosmosdb-auth-images/documentdb-authentication.png)

Брокер маркера ресурса — это служба веб-API среднего уровня, размещенная в службе приложений Azure, которая владеет главным ключом учетной записи Cosmos DB. Пример приложения использует брокер маркера ресурса для управления доступом к данным базы данных документов следующим образом:

1. При входе Xamarin.Forms приложение обращается к службе приложений Azure, чтобы инициировать поток проверки подлинности.
1. Служба приложений Azure выполняет процесс проверки подлинности OAuth с помощью Facebook. По завершении потока проверки подлинности Xamarin.Forms приложение получает маркер доступа.
1. Xamarin.FormsПриложение использует маркер доступа для запроса маркера ресурса от брокера маркера ресурса.
1. Брокер маркеров ресурсов использует маркер доступа для запроса удостоверения пользователя из Facebook. Затем удостоверение пользователя используется для запроса маркера ресурса из Cosmos DB, который используется для предоставления доступа для чтения и записи к секционированной коллекции пользователя, прошедшего проверку подлинности.
1. Xamarin.FormsПриложение использует маркер ресурса для прямого доступа к ресурсам Cosmos DB с разрешениями, определенными маркером ресурса.

> [!NOTE]
> По истечении срока действия маркера ресурса последующие запросы к базе данных документов получат исключение 401. На этом этапе Xamarin.Forms приложения должны повторно установить удостоверение и запросить новый маркер ресурса.

Дополнительные сведения о секционировании Cosmos DB см. [в разделе как секционировать и масштабировать в Azure Cosmos DB](/azure/cosmos-db/partition-data/). Дополнительные сведения об управлении доступом Cosmos DB см. в разделе [Защита доступа к Cosmos DB данных](/azure/cosmos-db/secure-access-to-data/) и [Управление доступом в API SQL](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Установка

Процесс интеграции брокера маркеров ресурсов в Xamarin.Forms приложение выглядит следующим образом:

1. Создайте учетную запись Cosmos DB, которая будет использовать управление доступом. Дополнительные сведения см. в разделе [Azure Cosmos DB Configuration](#azure-cosmos-db-configuration).
1. Создайте службу приложений Azure для размещения брокера маркера ресурса. Дополнительные сведения см. в статье [Конфигурация службы приложений Azure](#azure-app-service-configuration).
1. Создайте приложение Facebook для выполнения проверки подлинности. Дополнительные сведения см. в разделе [Конфигурация приложения Facebook](#facebook-app-configuration).
1. Настройка службы приложений Azure для выполнения простой проверки подлинности с помощью Facebook. Дополнительные сведения см. в статье [Настройка проверки подлинности в службе приложений Azure](#azure-app-service-authentication-configuration).
1. Настройте Xamarin.Forms пример приложения для взаимодействия со службой приложений Azure и Cosmos DB. Дополнительные сведения см. в разделе [ Xamarin.Forms Конфигурация приложения](#xamarinforms-application-configuration).

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

### <a name="azure-cosmos-db-configuration"></a>Конфигурация Azure Cosmos DB

Процесс создания учетной записи Cosmos DB, которая будет использовать управление доступом, выглядит следующим образом:

1. Создайте учетную запись Cosmos DB. Дополнительные сведения см. [в разделе Создание учетной записи Azure Cosmos DB](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. В учетной записи Cosmos DB создайте новую коллекцию с именем `UserItems` , указав ключ раздела `/userid` .

### <a name="azure-app-service-configuration"></a>Конфигурация службы приложений Azure

Процесс размещения брокера маркера ресурса в службе приложений Azure выглядит следующим образом:

1. В портал Azure создайте новое веб-приложение службы приложений. Дополнительные сведения см. [в разделе Создание веб-приложения в среда службы приложений](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. В портал Azure откройте колонку параметры приложения для веб-приложения и добавьте следующие параметры:
    - `accountUrl`— значение должно быть URL-адресом учетной записи Cosmos DB из колонки "ключи" учетной записи Cosmos DB.
    - `accountKey`— значение должно быть Cosmos DB главным ключом (первичной или вторичной) в колонке ключи учетной записи Cosmos DB.
    - `databaseId`— значение должно быть именем Cosmos DB базы данных.
    - `collectionId`— значение должно быть именем коллекции Cosmos DB (в данном случае `UserItems` ).
    - `hostUrl`— значение должно быть URL-адресом приложения в колонке "Обзор" учетной записи службы приложений.

    Эта конфигурация показана на следующем снимке экрана:

    [![Параметры веб-приложения службы приложений](azure-cosmosdb-auth-images/azure-web-app-settings.png)](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "Параметры веб-приложения службы приложений")

1. Опубликуйте решение брокера маркеров ресурсов в веб-приложении службы приложений Azure.

### <a name="facebook-app-configuration"></a>Конфигурация приложения Facebook

Процесс создания приложения Facebook для выполнения проверки подлинности выглядит следующим образом:

1. Создайте приложение Facebook. Дополнительные сведения см. в статье [Регистрация и настройка приложения](https://developers.facebook.com/docs/apps/register) в центре разработчиков Facebook.
1. Добавьте в приложение продукт для входа в Facebook. Дополнительные сведения см. в статье [Добавление имени входа Facebook в приложение или на веб-сайт](https://developers.facebook.com/docs/facebook-login) в центре разработчиков Facebook.
1. Настройте имя входа Facebook следующим образом:
   - Включите имя входа клиента OAuth.
   - Включить вход в веб-узел OAuth.
   - Задайте допустимый URI перенаправления OAuth для URI веб-приложения службы приложений с `/.auth/login/facebook/callback` добавлением.

  Эта конфигурация показана на следующем снимке экрана:

  ![Параметры OAuth для входа в Facebook](azure-cosmosdb-auth-images/facebook-oauth-settings.png)

Дополнительные сведения см. в статье [Регистрация приложения в Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

### <a name="azure-app-service-authentication-configuration"></a>Настройка проверки подлинности в службе приложений Azure

Процесс настройки простой проверки подлинности службы приложений выглядит следующим образом:

1. На портале Azure перейдите к веб-приложению службы приложений.
1. На портале Azure откройте колонку проверка подлинности и авторизация и выполните следующую настройку.
    - Необходимо включить проверку подлинности службы приложений.
    - Действие, выполняемое, если запрос не прошел проверку подлинности, должен быть установлен для **входа в с помощью Facebook**.

    Эта конфигурация показана на следующем снимке экрана:

    [![Параметры проверки подлинности веб-приложения службы приложений](azure-cosmosdb-auth-images/app-service-authentication-settings.png)](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "Параметры проверки подлинности веб-приложения службы приложений")

Веб-приложение службы приложений также должно быть настроено для взаимодействия с приложением Facebook, чтобы включить поток проверки подлинности. Это можно сделать, выбрав поставщик удостоверений Facebook и введя значения **идентификатора приложения** и **секрета приложения** из параметров приложения Facebook в центре разработчиков Facebook. Дополнительные сведения см. [в разделе Добавление сведений Facebook в приложение](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

### <a name="xamarinforms-application-configuration"></a>Xamarin.FormsКонфигурация приложения

Процесс настройки Xamarin.Forms примера приложения выглядит следующим образом:

1. Откройте Xamarin.Forms решение.
1. Откройте `Constants.cs` и обновите значения следующих констант.
    - `EndpointUri`— значение должно быть URL-адресом учетной записи Cosmos DB из колонки "ключи" учетной записи Cosmos DB.
    - `DatabaseName`— значение должно быть именем базы данных документов.
    - `CollectionName`— значение должно быть именем коллекции баз данных документов (в данном случае `UserItems` ).
    - `ResourceTokenBrokerUrl`— в колонке Обзор учетной записи службы приложений значение должно быть URL-адресом Web App брокера маркеров ресурсов.

## <a name="initiating-login"></a>Идет Инициация входа

Пример приложения инициирует процесс входа, перенаправляя браузер на URL-адрес поставщика удостоверений, как показано в следующем примере кода:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Это приводит к инициации потока проверки подлинности OAuth между службой приложений Azure и Facebook, которая отображает страницу входа Facebook:

![Имя входа Facebook](azure-cosmosdb-auth-images/login.png)

Имя входа можно отменить, нажав кнопку **Отмена** в iOS или нажав кнопку **назад** на Android. в этом случае пользователь остается непроверенным и пользовательский интерфейс поставщика удостоверений удаляется с экрана.

## <a name="obtaining-a-resource-token"></a>Получение маркера ресурса

После успешной проверки подлинности `WebRedirectAuthenticator.Completed` создается событие. В следующем примере кода демонстрируется обработка этого события:

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

Результатом успешной проверки подлинности является маркер доступа, который является доступным `AuthenticatorCompletedEventArgs.Account` свойством. Маркер доступа извлекается и используется в запросе GET к API брокера маркера ресурса `resourcetoken` .

`resourcetoken`API использует маркер доступа для запроса удостоверения пользователя из Facebook, который, в свою очередь, используется для запроса маркера ресурса от Cosmos DB. Если для пользователя в базе данных документов уже существует допустимый документ разрешений, он извлекается, а в приложение возвращается документ JSON, содержащий маркер ресурса Xamarin.Forms . Если для пользователя не существует допустимого документа разрешения, в базе данных документов создается пользователь и разрешение, а маркер ресурса извлекается из документа разрешения и возвращается в Xamarin.Forms приложение в документе JSON.

> [!NOTE]
> Пользователь базы данных документов — это ресурс, связанный с базой данных документов, и каждая база данных может содержать от нуля до нескольких пользователей. Разрешение "база данных документов" — это ресурс, связанный с пользователем базы данных документов, и каждый пользователь может содержать ноль или более разрешений. Ресурс разрешения предоставляет доступ к маркеру безопасности, который требуется пользователю при попытке доступа к ресурсу, например к документу.

Если `resourcetoken` API успешно завершается, он отправляет код состояния HTTP 200 (ОК) в ответе вместе с документом JSON, содержащим маркер ресурса. В следующих данных JSON показано типичное сообщение об успешном ответе:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed`Обработчик событий считывает ответ от `resourcetoken` API и извлекает маркер ресурса и идентификатор пользователя. Затем маркер ресурса передается в качестве аргумента в `DocumentClient` конструктор, который инкапсулирует конечную точку, учетные данные и политику подключения, используемые для доступа к Cosmos DB, и используется для настройки и выполнения запросов к Cosmos DB. Маркер ресурса отправляется с каждым запросом для прямого доступа к ресурсу и указывает, что предоставлен доступ на чтение и запись для секционированной коллекции пользователей, прошедших проверку подлинности.

## <a name="retrieving-documents"></a>Получение документов

Получение документов, которые относятся только к прошедшему проверку пользователю, может быть достигнуто путем создания запроса документа, включающего идентификатор пользователя в качестве ключа секции, и демонстрируется в следующем примере кода:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

Запрос асинхронно извлекает все документы, принадлежащие пользователю, прошедшему проверку подлинности, из указанной коллекции и помещает их в `List<TodoItem>` коллекцию для вывода.

`CreateDocumentQuery<T>`Метод задает `Uri` аргумент, представляющий коллекцию, для которой необходимо запросить документы, и `FeedOptions` объект. `FeedOptions`Объект указывает, что запрос может вернуть неограниченное количество элементов и идентификатор пользователя в качестве ключа секции. Это гарантирует, что в результате будут возвращены только документы в секционированной коллекции пользователя.

> [!NOTE]
> Обратите внимание, что документы разрешений, создаваемые брокером маркера ресурса, хранятся в той же коллекции документов, что и документы, созданные Xamarin.Forms приложением. Поэтому запрос документа содержит `Where` предложение, которое применяет предикат фильтрации к запросу к коллекции документов. Это предложение гарантирует, что документы разрешений не возвращаются из коллекции документов.

Дополнительные сведения о получении документов из коллекции документов см. в разделе [Получение документов коллекции документов](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#retrieving-document-collection-documents).

## <a name="inserting-documents"></a>Вставка документов

Перед вставкой документа в коллекцию документов `TodoItem.UserId` свойство должно быть обновлено значением, которое используется в качестве ключа секции, как показано в следующем примере кода:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Это гарантирует, что документ будет вставлен в секционированную коллекцию пользователя.

Дополнительные сведения о вставке документа в коллекцию документов см. в разделе [Вставка документа в коллекцию документов](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting-a-document-into-a-document-collection).

## <a name="deleting-documents"></a>Удаление документов

Значение ключа секции должно быть указано при удалении документа из секционированной коллекции, как показано в следующем примере кода:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Это гарантирует, что Cosmos DB будет знать, из какой секционированной коллекции следует удалить документ.

Дополнительные сведения об удалении документа из коллекции документов см. в разделе [Удаление документа из коллекции документов](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting-a-document-from-a-document-collection).

## <a name="summary"></a>Сводка

В этой статье объясняется, как объединить контроль доступа с секционированными коллекциями, чтобы пользователь мог получить доступ к собственным документам базы данных документов в Xamarin.Forms приложении. Указание удостоверения пользователя в качестве ключа секции гарантирует, что секционированная коллекция сможет хранить только документы для этого пользователя.

## <a name="related-links"></a>Связанные ссылки

- [Проверка подлинности Azure Cosmos DB ToDo (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Использование базы данных документов Azure Cosmos DB](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Защита доступа к данным Azure Cosmos DB](/azure/cosmos-db/secure-access-to-data/)
- [Управление доступом в API SQL](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Секционирование и масштабирование в базе данных Azure Cosmos DB](/azure/cosmos-db/partition-data/)
- [Клиентская библиотека Azure Cosmos DB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [API Azure Cosmos DB](https://msdn.microsoft.com/library/azure/dn948556.aspx)
