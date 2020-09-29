---
title: Часть 5. Практические стратегии совместного использования кода
description: В этом документе обсуждаются практические стратегии совместного использования кода для таких сценариев, как базы данных, доступ к файлам, сетевые операции и асинхронный код.
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: d10917471c37d91fa02db2585895f5694a5a4a60
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457449"
---
# <a name="part-5---practical-code-sharing-strategies"></a>Часть 5. Практические стратегии совместного использования кода

В этом разделе приводятся примеры совместного использования кода для распространенных сценариев приложений.

## <a name="data-layer"></a>Уровень данных

Уровень данных состоит из подсистемы хранилища и методов чтения и записи информации. Для повышения производительности, гибкости и кросс-платформенной совместимости для кросс-платформенных приложений Xamarin рекомендуется ядро базы данных SQLite.
Он работает на различных платформах, включая Windows, Android, iOS и Mac.

### <a name="sqlite"></a>SQLite

SQLite — это реализация базы данных с открытым кодом. Источник и документация можно найти по адресу [SQLite.org](https://www.sqlite.org/). Поддержка SQLite доступна на каждой мобильной платформе:

- **iOS** — встроена в операционную систему.
- **Android** — встроена в операционную систему с версии Android 2,2 (API уровня 10).
- **Windows** — см. в разделе [расширение SQLite для универсальная платформа Windows](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Даже если ядро СУБД доступно на всех платформах, собственные методы доступа к базе данных отличаются. Как iOS, так и Android предлагают встроенные API-интерфейсы для доступа к SQLite, которые можно использовать из Xamarin. iOS или Xamarin. Android. Однако использование собственных методов SDK не дает возможности совместно использовать код (за исключением SQL-запросов, предполагая, что они хранятся в виде строк). Дополнительные сведения о поиске функций базы данных `CoreData` в iOS и Android см. в статье `SQLiteOpenHelper` , так как эти параметры не являются межплатформенными, они выходят за рамки данного документа.

### <a name="adonet"></a>ADO.NET

Поддержка Xamarin. iOS и Xamarin. Android `System.Data` `Mono.Data.Sqlite` . Дополнительные сведения см. в [документации](~/ios/data-cloud/system.data.md) по Xamarin. iOS.
Использование этих пространств имен позволяет писать код ADO.NET, который работает на обеих платформах. Измените ссылки проекта на include `System.Data.dll` и `Mono.Data.Sqlite.dll` добавьте эти операторы using в код:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

Затем будет работать следующий пример кода:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

Реальные реализации ADO.NET, очевидно, будут разделяться между различными методами и классами (этот пример предназначен только для демонстрационных целей).

### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET — межплатформенная модель ORM

ORM (или объектно-реляционное средство сопоставления) пытается упростить хранение данных, смоделированных в классах. Вместо того, чтобы вручную создавать SQL-запросы, которые создают таблицы или выбирают, ВСТАВЛЯЮТ и УДАЛЯют данные, извлеченные из полей и свойств класса вручную, ORM добавляет слой кода, который делает это автоматически. Используя отражение для изучения структуры классов, ORM может автоматически создавать таблицы и столбцы, соответствующие классу, и создавать запросы для чтения и записи данных. Это позволяет коду приложения просто отправить и извлечь экземпляры объектов в ORM, что позаботится о всех операциях SQL.

SQLite-NET выступает в качестве простой ORM, который позволит сохранять и извлекать классы в SQLite. Он скрывает сложность межплатформенного доступа SQLite с помощью сочетания директив компилятора и других приемов.

Функции SQLite-NET:

- Таблицы определяются путем добавления атрибутов к классам модели.
- Экземпляр базы данных представлен подклассом  `SQLiteConnection` класса, основным классом в библиотеке SQLite-NET.
- Данные можно вставлять, запрашивать и удалять с помощью объектов. Инструкции SQL не требуются (хотя при необходимости можно написать инструкции SQL).
- Базовые запросы LINQ можно выполнять с коллекциями, возвращаемыми SQLite-NET.

Исходный код и документация для SQLite-NET доступны в [SQLite-NET на GitHub](https://github.com/praeclarum/sqlite-net) и реализованы в обоих примерах. Ниже приведен простой пример кода SQLite-NET (из примера *задачи Pro* ).

Во-первых, `TodoItem` класс использует атрибуты, чтобы определить поле как первичный ключ базы данных.

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Это позволяет `TodoItem` создать таблицу со следующей строкой кода (и без инструкций SQL) в `SQLiteConnection` экземпляре:

```csharp
CreateTable<TodoItem> ();
```

Данные в таблице также можно манипулировать с помощью других методов в `SQLiteConnection` (опять же, без инструкций SQL):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

Полные примеры см. в разделе пример исходного кода примера.

## <a name="file-access"></a>Доступ к файлам

Доступ к файлам является ключевой частью любого приложения. Ниже приведены распространенные примеры файлов, которые могут входить в состав приложения.

- Файлы базы данных SQLite.
- Данные, созданные пользователем (текст, изображения, звук, видео).
- Скачанные данные для кэширования (изображения, файлы HTML или PDF).

### <a name="systemio-direct-access"></a>Прямой доступ System.IO

Как Xamarin. iOS, так и Xamarin. Android обеспечивают доступ к файловой системе с помощью классов в `System.IO` пространстве имен.

У каждой платформы есть разные ограничения доступа, которые необходимо учитывать:

- приложения iOS выполняются в песочнице с очень ограниченным доступом к файловой системе. Компания Apple дополнительно определяет, как следует использовать файловую систему, указав определенные расположения для резервного копирования (и другие). Дополнительные сведения см. в разделе  [Работа с файловой системой в руководстве по Xamarin. iOS](~/ios/app-fundamentals/file-system.md) .
- Android также ограничен доступ к определенным каталогам, связанным с приложением, но также поддерживает внешний носитель (например, Карты SD) и доступ к общим данным.
- Windows Phone 8 (Silverlight) запретить прямой доступ к файлам — файлы можно манипулировать только с помощью `IsolatedStorage` .
- Проекты Windows 8.1 WinRT и Windows 10 UWP предлагают только асинхронные операции с файлами через `Windows.Storage` API-интерфейсы, которые отличаются от других платформ.

#### <a name="example-for-ios-and-android"></a>Пример для iOS и Android

Ниже показан тривиальный пример, в котором выполняется запись и чтение текстового файла.
Использование `Environment.GetFolderPath` позволяет выполнять один и тот же код в iOS и Android, каждый из которых возвращает допустимый каталог на основе соглашений о файловой системе.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.File.ReadAllText (filePath));
```

Дополнительные сведения о функциональных возможностях файловой системы для iOS см. в документации по Xamarin. iOS, [работающей с документом в файловой системе](~/ios/app-fundamentals/file-system.md) . При написании кода межплатформенного доступа к файлам Помните, что некоторые файловые системы чувствительны к регистру и имеют разные разделители каталогов. Рекомендуется всегда использовать один и тот же регистр для имен файлов и `Path.Combine()` метод при создании путей к файлам или каталогам.

### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows. Storage для Windows 8 и Windows 10

*Создание мобильных приложений с помощью Xamarin. Forms* [Book](https://developer.xamarin.com/r/xamarin-forms/book/) 
 [главы 20. Асинхронные и файловые операции ввода-вывода](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) включают [примеры для Windows 8.1 и Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

С помощью [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) этих платформ можно считывать и файловые файлы на этих платформах, используя поддерживаемые API:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Дополнительные сведения см. в [разделе книги](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) .

<a name="Isolated_Storage"></a>

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Изолированное хранилище на Windows Phone 7 & 8 (Silverlight)

Изолированное хранилище — это стандартный интерфейс API для сохранения и загрузки файлов на всех платформах iOS, Android и более старых Windows Phone.

Это механизм по умолчанию для доступа к файлам в Windows Phone (Silverlight), реализованный в Xamarin. iOS и Xamarin. Android, чтобы обеспечить написание общего кода для доступа к файлам. На этот `System.IO.IsolatedStorage` класс можно ссылаться во всех трех платформах в [общем проекте](~/cross-platform/app-fundamentals/shared-projects.md).

Дополнительные сведения см. в [обзоре изолированного хранилища Windows Phone](/previous-versions/windows/apps/ff402541(v=vs.105)) .

Интерфейсы API изолированного хранилища недоступны в [переносимых библиотеках классов](~/cross-platform/app-fundamentals/pcl.md). Одним из альтернативных вариантов для PCL является [Пклстораже NuGet](https://pclstorage.codeplex.com/) .

### <a name="cross-platform-file-access-in-pcls"></a>Кросс-платформенный доступ к файлам в PCL

Кроме того, существует совместимая с PCL NuGet — [пклстораже](https://www.nuget.org/packages/PCLStorage/) , которая поддерживает доступ к файлам на разных платформах для платформ, поддерживаемых Xamarin, и последних API-интерфейсов Windows.

## <a name="network-operations"></a>Операции сети

Большинство мобильных приложений будет иметь сетевые компоненты, например:

- Загрузка изображений, видео и аудио (например, эскизы, фотографии, музыка).
- Загрузка документов (например, HTML, PDF).
- Отправка пользовательских данных (например, фотографий или текста).
- Доступ к веб-службам или сторонним API (включая SOAP, XML или JSON).

.NET Framework предоставляет несколько различных классов для доступа к сетевым ресурсам: `HttpClient` , `WebClient` и `HttpWebRequest` .

### <a name="httpclient"></a>HttpClient

`HttpClient`Класс в `System.Net.Http` пространстве имен доступен в Xamarin. iOS, Xamarin. Android и большинстве платформ Windows. Существует [Клиентская библиотека Microsoft HTTP NuGet](https://www.nuget.org/packages/Microsoft.Net.Http/) , которую можно использовать для переноса этого API в переносимые библиотеки классов (и Windows Phone 8 Silverlight).

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

`WebClient`Класс предоставляет простой API для извлечения удаленных данных с удаленных серверов.

Универсальная платформа Windows операции *должны* быть асинхронными, хотя Xamarin. iOS и Xamarin. Android поддерживают синхронные операции (которые могут выполняться в фоновых потоках).

Код для простой `WebClient` операции асинхронного:

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` также имеет `DownloadFileCompleted` и `DownloadFileAsync` для извлечения двоичных данных.

<a name="HttpWebRequest"></a>

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` предлагает больше настроек `WebClient` , чем, и в результате требует больше кода для использования.

Вот код для простой синхронной `HttpWebRequest` операции:

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

В [документации по веб-службам](~/cross-platform/data-cloud/web-services/index.md)приведен пример.

 <a name="Reachability"></a>

### <a name="reachability"></a>Доступность

Мобильные устройства работают с различными состояниями сети от быстрых подключений Wi-Fi или 4G к слабым и медленным каналам передачи данных. В связи с этим рекомендуется определить, доступна ли сеть, и если да, то какой тип сети доступен, прежде чем пытаться подключиться к удаленным серверам.

Действия, которые может выполнять мобильное приложение в следующих ситуациях:

- Если сеть недоступна, посоветуйте пользователю. Если они отключены вручную (например, Режим "в самолете" или отключение Wi-Fi), то они могут устранить проблему.
- Если это подключение — 3G, приложения могут вести себя по-разному (например, Apple не разрешает загрузку приложений размером более 20 МБ через 3G). Приложения могут использовать эти сведения, чтобы предупредить пользователя о чрезмерном времени загрузки при извлечении больших файлов.
- Даже если сеть доступна, перед инициацией других запросов рекомендуется проверить подключение к целевому серверу. Это позволит избежать повторного истечения времени ожидания сетевых операций приложения, а также предоставить пользователю более информативное сообщение об ошибке.

## <a name="webservices"></a>WebServices

См. нашу документацию по [работе с веб-службами](~/cross-platform/data-cloud/web-services/index.md), которая охватывает доступ к конечным точкам RESTFUL, SOAP и WCF с помощью Xamarin. iOS. Можно вручную создавать запросы веб-службы и анализировать ответы, однако существуют библиотеки, которые делают это гораздо проще, включая Azure, Рестшарп и ServiceStack. Даже базовые операции WCF могут быть доступны в приложениях Xamarin.

### <a name="azure"></a>Azure

Microsoft Azure — это облачная платформа, которая предоставляет широкий спектр служб для мобильных приложений, включая хранение и синхронизацию данных, а также push-уведомления.

Посетите [Azure.Microsoft.com](https://azure.microsoft.com/) , чтобы испытать его бесплатно.

### <a name="restsharp"></a>RestSharp

Рестшарп — это библиотека .NET, которая может быть включена в мобильные приложения для предоставления клиента RESTFUL, который упрощает доступ к веб-службам. Он помогает, предоставляя простой API для запроса данных и анализа ответа на остальное. Рестшарп может быть полезным

На [веб-сайте рестшарп](http://restsharp.org/) содержится [Документация](https://github.com/restsharp/RestSharp/wiki) по реализации клиента RESTful с помощью рестшарп.
Рестшарп предоставляет примеры Xamarin. iOS и Xamarin. Android на [GitHub](https://github.com/restsharp/RestSharp/).

В [документации по веб-службам](~/cross-platform/data-cloud/web-services/index.md)также есть фрагмент кода Xamarin. iOS.

 <a name="ServiceStack"></a>

### <a name="servicestack"></a>ServiceStack

В отличие от Рестшарп, ServiceStack является как решением на стороне сервера для размещения веб-службы, так и клиентской библиотекой, которая может быть реализована в мобильных приложениях для доступа к этим службам.

На [веб-сайте ServiceStack](http://servicestack.net/) объясняется назначение проекта и приводятся ссылки на примеры документов и кода. Примеры включают полную серверную реализацию веб-службы, а также различные клиентские приложения, которые имеют доступ к ней.

### <a name="wcf"></a>WCF

Средства Xamarin могут помочь в использовании некоторых служб Windows Communication Foundation (WCF). Как правило, Xamarin поддерживает одно и то же подмножество WCF на стороне клиента, которое поставляется со средой выполнения Silverlight. Сюда входят наиболее распространенные реализации кодирования и протоколов WCF: сообщения SOAP, закодированные в текстовом формате, по транспортному протоколу HTTP с помощью `BasicHttpBinding` .

Из-за размера и сложности WCF Framework могут существовать текущие и будущие реализации служб, которые будут находиться вне области, поддерживаемой доменом подмножества клиента Xamarin. Кроме того, поддержка WCF требует использования средств, доступных только в среде Windows для создания учетной записи-посредника.

 <a name="Threading"></a>

## <a name="threading"></a>Потоки

Скорость реагирования приложения важна для мобильных приложений — пользователи хотят, чтобы приложения загружались и выполнялись быстро. Зафиксированный экран, останавливающий прием вводимых пользователем данных, покажет, что приложение завершило работу, поэтому важно не привязывать поток пользовательского интерфейса к долго выполняющимся блокировкам, таким как сетевые запросы или медленно выполняемые локальные операции (например, распаковка файла). В частности, процесс запуска не должен содержать длительно выполняющиеся задачи. все мобильные платформы будут завершать работу приложения, которое занимает слишком много времени для загрузки.

Это означает, что в пользовательском интерфейсе должен быть реализован индикатор хода выполнения или непригодный для использования пользовательский интерфейс, который можно быстро отобразить, а также асинхронные задачи для выполнения фоновых операций. Выполнение фоновых задач требует использования потоков, что означает, что фоновым задачам требуется способ обмена данными с основным потоком, чтобы указать ход выполнения или время их завершения.

 <a name="Parallel_Task_Library"></a>

### <a name="parallel-task-library"></a>Библиотека параллельных задач

Задачи, созданные с помощью библиотеки параллельных задач, могут выполняться асинхронно и возвращаться к вызывающему потоку, что делает их очень полезными для запуска длительных операций без блокировки пользовательского интерфейса.

Простая операция параллельной задачи может выглядеть следующим образом:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

Ключ, `TaskScheduler.FromCurrentSynchronizationContext()` который будет повторно использовать SynchronizationContext. текущий поток, вызывающий метод (здесь выполняется основной поток), в `MainThreadMethod` качестве способа для обратного маршалирования вызовов к этому потоку. Это означает, что если метод вызывается в потоке пользовательского интерфейса, операция будет выполнена `ContinueWith` обратно в потоке пользовательского интерфейса.

Если код запускает задачи из других потоков, используйте следующий шаблон для создания ссылки на поток пользовательского интерфейса, и задача по-прежнему может выполнить обратный вызов.

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread"></a>

### <a name="invoking-on-the-ui-thread"></a>Вызов в потоке пользовательского интерфейса

Для кода, который не использует библиотеку параллельных задач, каждая платформа имеет собственный синтаксис для упаковки операций в поток пользовательского интерфейса:

- **iOS** — `owner.BeginInvokeOnMainThread(new NSAction(action))`
- **Android** — `owner.RunOnUiThread(action)`
- **Xamarin. Forms** — `Device.BeginInvokeOnMainThread(action)`
- **Windows** — `Deployment.Current.Dispatcher.BeginInvoke(action)`

Для синтаксиса iOS и Android требуется доступность класса "Context", что означает, что код должен передать этот объект в любые методы, требующие обратного вызова в потоке пользовательского интерфейса.

Чтобы вызвать поток пользовательского интерфейса в общем коде, выполните [Пример идиспатчонуисреад](https://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (с помощью [@follesoe](https://twitter.com/follesoe) ). Объявите и запрограммировать в `IDispatchOnUIThread` интерфейс в общем коде, а затем реализовать классы для конкретных платформ, как показано ниже:

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Разработчики Xamarin. Forms следует использовать [`Device.BeginInvokeOnMainThread`](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads) в общем коде (общие проекты или PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation"></a>

## <a name="platform-and-device-capabilities-and-degradation"></a>Возможности и снижение производительности платформы и устройства

Более подробные примеры работы с различными возможностями приведены в документации по возможностям платформы. Он имеет дело с обнаружением различных возможностей и корректной появления приложения для обеспечения хорошего взаимодействия с пользователем, даже если приложение не может работать с его полным потенциалом.