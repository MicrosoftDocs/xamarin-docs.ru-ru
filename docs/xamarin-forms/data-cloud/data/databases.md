---
title: Xamarin.Forms Локальные базы данных
description: Xamarin.Forms поддерживает приложения, управляемые базой данных, с помощью обработчика базы данных SQLite, что дает возможность загружать и сохранять объекты в общем коде. В этой статье описывается Xamarin.Forms , как приложения могут считывать и записывать данные в локальную базу данных SQLite с помощью SQLite.NET.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6c5390057baf48634056101d44540020648ea709
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563111"
---
# <a name="no-locxamarinforms-local-databases"></a>Xamarin.Forms Локальные базы данных

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

Обработчик базы данных SQLite позволяет Xamarin.Forms приложениям загружать и сохранять объекты данных в общем коде. Пример приложения использует таблицу базы данных SQLite для хранения элементов Todo. В этой статье описывается, как использовать SQLite.Net в общем коде для хранения и извлечения информации в локальной базе данных.

[![Снимки экрана приложения ToDoList в iOS и Android](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "Приложение ToDoList в iOS и Android")

Интегрируйте SQLite.NET в мобильные приложения, выполнив следующие действия.

1. [Установите пакет NuGet](#install-the-sqlite-nuget-package).
1. [Настройка констант](#configure-app-constants).
1. [Создание класса доступа к базе данных](#create-a-database-access-class).
1. [Доступ к данным Xamarin.Forms в ](#access-data-in-xamarinforms).
1. [Расширенная конфигурация](#advanced-configuration).

## <a name="install-the-sqlite-nuget-package"></a>Установка пакета NuGet для SQLite

Используйте диспетчер пакетов NuGet для поиска **SQLite-NET-PCL** и добавления последней версии в проект общего кода.

Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:

- **Идентификатор:** sqlite-net-pcl
- **Автор(ы)** : SQLite-net
- **Владелец:** praeclarum
- **URL-адрес проекта:** https://github.com/praeclarum/sqlite-net
- **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Пакет NuGet **sqlite-net-pcl** следует использовать даже в проектах .NET Standard.

## <a name="configure-app-constants"></a>Настройка констант приложения

Пример проекта включает файл **Constants.CS** , который предоставляет общие данные конфигурации:

```csharp
public static class Constants
{
    public const string DatabaseFilename = "TodoSQLite.db3";

    public const SQLite.SQLiteOpenFlags Flags =
        // open the database in read/write mode
        SQLite.SQLiteOpenFlags.ReadWrite |
        // create the database if it doesn't exist
        SQLite.SQLiteOpenFlags.Create |
        // enable multi-threaded database access
        SQLite.SQLiteOpenFlags.SharedCache;

    public static string DatabasePath
    {
        get
        {
            var basePath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
            return Path.Combine(basePath, DatabaseFilename);
        }
    }
}
```

В файле констант задаются значения перечисления по умолчанию `SQLiteOpenFlag` , используемые для инициализации подключения к базе данных. `SQLiteOpenFlag`Перечисление поддерживает следующие значения:

- `Create`: Соединение будет автоматически создавать файл базы данных, если он не существует.
- `FullMutex`: Соединение открывается в режиме сериализованного потока.
- `NoMutex`: Соединение открывается в режиме многопоточной обработки.
- `PrivateCache`: Соединение не будет участвовать в общем кэше, даже если оно включено.
- `ReadWrite`: Соединение может считывать и записывать данные.
- `SharedCache`: Соединение будет участвовать в общем кэше, если он включен.
- `ProtectionComplete`: Файл зашифрован и недоступен, пока устройство заблокировано.
- `ProtectionCompleteUnlessOpen`: Файл шифруется до открытия, но доступен, даже если пользователь блокирует устройство.
- `ProtectionCompleteUntilFirstUserAuthentication`: Файл шифруется до тех пор, пока пользователь не загрузил и не разблокирует устройство.
- `ProtectionNone`: Файл базы данных не зашифрован.

Может потребоваться указать различные флаги в зависимости от того, как будет использоваться база данных. Дополнительные сведения о `SQLiteOpenFlags` см. в разделе [Открытие нового подключения к базе данных](https://www.sqlite.org/c3ref/open.html) в SQLite.org.

## <a name="create-a-database-access-class"></a>Создание класса доступа к базе данных

Класс-оболочка базы данных абстрагирует уровень доступа к данным от остальной части приложения. Этот класс централизует логику запросов и упрощает управление инициализацией базы данных, что упрощает рефакторинг или расширение операций с данными по мере роста приложения. Приложение Todo определяет `TodoItemDatabase` класс для этой цели.

### <a name="lazy-initialization"></a>Отложенная инициализация

`TodoItemDatabase`Использует `Lazy` класс .NET для задержки инициализации базы данных до первого обращения к ней. Использование отложенной инициализации предотвращает задержку запуска приложения в процессе загрузки базы данных. Дополнительные сведения см. в разделе [Lazy &lt; T &gt; Class](xref:System.Lazy`1).

```csharp
public class TodoItemDatabase
{
    static readonly Lazy<SQLiteAsyncConnection> lazyInitializer = new Lazy<SQLiteAsyncConnection>(() =>
    {
        return new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    });

    static SQLiteAsyncConnection Database => lazyInitializer.Value;
    static bool initialized = false;

    public TodoItemDatabase()
    {
        InitializeAsync().SafeFireAndForget(false);
    }

    async Task InitializeAsync()
    {
        if (!initialized)
        {
            if (!Database.TableMappings.Any(m => m.MappedType.Name == typeof(TodoItem).Name))
            {
                await Database.CreateTablesAsync(CreateFlags.None, typeof(TodoItem)).ConfigureAwait(false);
            }
            initialized = true;
        }
    }

    //...
}
```

Соединение с базой данных — это статическое поле, которое обеспечивает использование одного подключения к базе данных в течение всего жизненного цикла приложения. Постоянное статическое подключение обеспечивает лучшую производительность, чем несколько раз открывать и закрывать соединения в течение одного сеанса приложения.

`InitializeAsync`Метод отвечает за проверку наличия таблицы для хранения `TodoItem` объектов. Этот метод автоматически создает таблицу, если она не существует.

### <a name="the-safefireandforget-extension-method"></a>Метод расширения Сафефиреандфоржет

При `TodoItemDatabase` создании экземпляра класса необходимо инициализировать подключение к базе данных, которое является асинхронным процессом. Но:

- Конструкторы классов не могут быть асинхронными.
- Асинхронный метод, который не ожидается, не будет вызывать исключения.
- Использование `Wait` метода блокирует поток _и_ поглощает исключения.

Чтобы запустить асинхронную инициализацию, не блокируя выполнение и иметь возможность перехватывать исключения, в образце приложения используется метод расширения с именем `SafeFireAndForget` . `SafeFireAndForget`Метод расширения предоставляет `Task` классу дополнительные функции.

```csharp
public static class TaskExtensions
{
    // NOTE: Async void is intentional here. This provides a way
    // to call an async method from the constructor while
    // communicating intent to fire and forget, and allow
    // handling of exceptions
    public static async void SafeFireAndForget(this Task task,
        bool returnToCallingContext,
        Action<Exception> onException = null)
    {
        try
        {
            await task.ConfigureAwait(returnToCallingContext);
        }

        // if the provided action is not null, catch and
        // pass the thrown exception
        catch (Exception ex) when (onException != null)
        {
            onException(ex);
        }
    }
}
```

`SafeFireAndForget`Метод ожидает асинхронное выполнение предоставленного `Task` объекта и позволяет присоединить метод `Action` , который вызывается при возникновении исключения.

Дополнительные сведения см. в разделе [асинхронная модель на основе задач (TAP)](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap).

### <a name="data-manipulation-methods"></a>Методы обработки данных

`TodoItemDatabase`Класс включает методы для четырех типов обработки данных: создание, чтение, изменение и удаление. Библиотека SQLite.NET предоставляет простую объектную реляционную карту (ORM), позволяющую хранить и извлекать объекты без написания инструкций SQL.

```csharp
public class TodoItemDatabase {

    // ...

    public Task<List<TodoItem>> GetItemsAsync()
    {
        return Database.Table<TodoItem>().ToListAsync();
    }

    public Task<List<TodoItem>> GetItemsNotDoneAsync()
    {
        // SQL queries are also possible
        return Database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
    }

    public Task<TodoItem> GetItemAsync(int id)
    {
        return Database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
    }

    public Task<int> SaveItemAsync(TodoItem item)
    {
        if (item.ID != 0)
        {
            return Database.UpdateAsync(item);
        }
        else
        {
            return Database.InsertAsync(item);
        }
    }

    public Task<int> DeleteItemAsync(TodoItem item)
    {
        return Database.DeleteAsync(item);
    }
}
```

## <a name="access-data-in-no-locxamarinforms"></a>Доступ к данным в Xamarin.Forms

Xamarin.Forms `App` Класс предоставляет экземпляр `TodoItemDatabase` класса:

```csharp
static TodoItemDatabase database;
public static TodoItemDatabase Database
{
    get
    {
        if (database == null)
        {
            database = new TodoItemDatabase();
        }
        return database;
    }
}
```

Это свойство позволяет Xamarin.Forms компонентам вызывать методы получения и обработки данных в `Database` экземпляре в ответ на взаимодействие с пользователем. Например:

```csharp
var saveButton = new Button { Text = "Save" };
saveButton.Clicked += async (sender, e) =>
{
    var todoItem = (TodoItem)BindingContext;
    await App.Database.SaveItemAsync(todoItem);
    await Navigation.PopAsync();
};
```

## <a name="advanced-configuration"></a>Расширенная конфигурация

SQLite предоставляет надежный API с дополнительными функциями, чем описано в этой статье и примере приложения. В следующих разделах описываются функции, важные для масштабируемости.

Дополнительные сведения см. в [документации по SQLite](https://www.sqlite.org/docs.html) в SQLite.org.

### <a name="write-ahead-logging"></a>Упреждающее ведение журнала

По умолчанию SQLite использует традиционный журнал отката. Копия неизмененного содержимого базы данных записывается в отдельный файл отката, после чего изменения записываются непосредственно в файл базы данных. ФИКСАЦИя происходит при удалении журнала отката.

Ведение журнала с упреждающей записью (WAL) сначала записывает изменения в отдельный файл WAL. В режиме WAL ФИКСАЦИя — это специальная запись, которая добавляется в файл WAL, что позволяет нескольким транзакциям выполняться в одном файле WAL. Файл WAL объединяется в файл базы данных в специальной операции, называемой _контрольной точкой_.

WAL может быть быстрее для локальных баз данных, так как модули чтения и записи не блокируют друг друга, что позволяет выполнять операции чтения и записи одновременно. Однако режим WAL не допускает изменения _размера страницы_, добавляет к базе данных дополнительные ассоциации файлов и добавляет операцию дополнительной _контрольной точки_ .

Чтобы включить WAL в SQLite.NET, вызовите `EnableWriteAheadLoggingAsync` метод для `SQLiteAsyncConnection` экземпляра:

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

Дополнительные сведения см. в разделе [SQLite Write-упреждающее ведение журнала](https://www.sqlite.org/wal.html) на SQLite.org.

### <a name="copying-a-database"></a>Копирование базы данных

Существует несколько случаев, когда может потребоваться скопировать базу данных SQLite:

- База данных поставляется с приложением, но ее необходимо скопировать или переместить в хранилище, доступное для записи на мобильном устройстве.
- Необходимо создать резервную копию или копию базы данных.
- Необходимо выполнить версию, переместить или переименовать файл базы данных.

В общем случае перемещение, переименование или копирование файла базы данных выполняется так же, как и для любого другого типа файлов, с помощью нескольких дополнительных соображений.

- Прежде чем пытаться переместить файл базы данных, необходимо закрыть все подключения к базе данных.
- Если вы используете [ведение журнала с упреждающей записью](#write-ahead-logging), SQLite создаст файл с доступом к общей памяти (. SHM) и файл (с расширением WAL). Убедитесь, что вы также применяете все изменения к этим файлам.

Дополнительные сведения см. [в разделе Обработка файлов Xamarin.Forms в ](~/xamarin-forms/data-cloud/data/files.md).

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения Todo](/samples/xamarin/xamarin-forms-samples/todo)
- [Пакет NuGet SQLite.NET](https://www.nuget.org/packages/sqlite-net-pcl/)
- [Документация по SQLite](https://www.sqlite.org/docs.html)
- [Использование SQLite с Android](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [Использование SQLite с iOS](~/ios/data-cloud/data/using-sqlite-orm.md)
- [Асинхронный шаблон, основанный на задачах (TAP)](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [&lt;Класс Lazy T &gt;](xref:System.Lazy`1)