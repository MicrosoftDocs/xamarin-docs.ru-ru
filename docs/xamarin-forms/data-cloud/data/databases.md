---
title: Xamarin.Forms Локальные базы данных
description: Xamarin.Forms поддерживает приложения, управляемые базой данных, с помощью обработчика базы данных SQLite, что дает возможность загружать и сохранять объекты в общем коде. В этой статье описывается Xamarin.Forms , как приложения могут считывать и записывать данные в локальную базу данных SQLite с помощью SQLite.NET.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 03/01/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a7dd5ea8963fed079c82ac6944d571176002486e
ms.sourcegitcommit: 322e7bcf9fb8c1ad52ab8e929bea95d45e280834
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751448"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms Локальные базы данных

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/todo)

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
- **Авторы:** SQLite-net
- **Владелец:** praeclarum
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

`TodoItemDatabase`Использует асинхронную отложенную инициализацию, представленную пользовательским `AsyncLazy<T>` классом, чтобы отложить инициализацию базы данных до первого обращения к ней:

```csharp
public class TodoItemDatabase
{
    static SQLiteAsyncConnection Database;

    public static readonly AsyncLazy<TodoItemDatabase> Instance = new AsyncLazy<TodoItemDatabase>(async () =>
    {
        var instance = new TodoItemDatabase();
        CreateTableResult result = await Database.CreateTableAsync<TodoItem>();
        return instance;
    });

    public TodoItemDatabase()
    {
        Database = new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    }

    //...
}
```

`Instance`Поле используется для создания таблицы базы данных для `TodoItem` объекта, если он еще не существует, и возвращает в `TodoItemDatabase` виде одноэлементного экземпляра. `Instance`Поле типа формируется при `AsyncLazy<TodoItemDatabase>` первом его ожидании. Если несколько потоков пытаются получить доступ к полю одновременно, все они будут использовать одну конструкцию. После завершения построения все `await` операции завершаются. Кроме того, все `await` операции после завершения построения будут выполняться немедленно, так как это значение доступно.

> [!NOTE]
> Соединение с базой данных — это статическое поле, которое обеспечивает использование одного подключения к базе данных в течение всего жизненного цикла приложения. Постоянное статическое подключение обеспечивает лучшую производительность, чем несколько раз открывать и закрывать соединения в течение одного сеанса приложения.

### <a name="asynchronous-lazy-initialization"></a>Асинхронная отложенная инициализация

Чтобы начать инициализацию базы данных, не блокируя выполнение и иметь возможность перехватывать исключения, в примере приложения используется асинхронная отложенная сбой инициализации, представленная `AsyncLazy<T>` классом:

```csharp
public class AsyncLazy<T> : Lazy<Task<T>>
{
    readonly Lazy<Task<T>> instance;

    public AsyncLazy(Func<T> factory)
    {
        instance = new Lazy<Task<T>>(() => Task.Run(factory));
    }

    public AsyncLazy(Func<Task<T>> factory)
    {
        instance = new Lazy<Task<T>>(() => Task.Run(factory));
    }

    public TaskAwaiter<T> GetAwaiter()
    {
        return instance.Value.GetAwaiter();
    }
}
```

`AsyncLazy`Класс сочетает `Lazy<T>` `Task<T>` типы и для создания задачи с отложенной инициализацией, представляющей инициализацию ресурса. Делегат фабрики, передаваемый в конструктор, может быть либо синхронным, либо асинхронным. Делегаты фабрики будут выполняться в потоке пула потоков и не будут выполняться более одного раза (даже если несколько потоков пытаются одновременно запустить их). После завершения делегата фабрики становится доступным отложенно инициализированное значение, а все методы, ожидающие экземпляра, `AsyncLazy<T>` получают значение. Дополнительные сведения см. в разделе [AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/).

### <a name="data-manipulation-methods"></a>Методы обработки данных

`TodoItemDatabase`Класс включает методы для четырех типов обработки данных: создание, чтение, изменение и удаление. Библиотека SQLite.NET предоставляет простую объектную реляционную карту (ORM), позволяющую хранить и извлекать объекты без написания инструкций SQL.

```csharp
public class TodoItemDatabase
{
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

## <a name="access-data-in-xamarinforms"></a>Доступ к данным в Xamarin.Forms

`TodoItemDatabase`Класс предоставляет `Instance` поле, через которое можно вызывать операции доступа к данным в `TodoItemDatabase` классе:

```csharp
async void OnSaveClicked(object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    TodoItemDatabase database = await TodoItemDatabase.Instance;
    await database.SaveItemAsync(todoItem);

    // Navigate backwards
    await Navigation.PopAsync();
}
```

## <a name="advanced-configuration"></a>Расширенная конфигурация

SQLite предоставляет надежный API с дополнительными функциями, чем описано в этой статье и примере приложения. В следующих разделах описываются функции, важные для масштабируемости.

Дополнительные сведения см. в [документации по SQLite](https://www.sqlite.org/docs.html) в SQLite.org.

### <a name="write-ahead-logging"></a>Ведение журнала с упреждающей записью

По умолчанию SQLite использует традиционный журнал отката. Копия неизмененного содержимого базы данных записывается в отдельный файл отката, после чего изменения записываются непосредственно в файл базы данных. ФИКСАЦИя происходит при удалении журнала отката.

Write-Ahead ведение журнала (WAL) сначала записывает изменения в отдельный файл WAL. В режиме WAL ФИКСАЦИя — это специальная запись, которая добавляется в файл WAL, что позволяет нескольким транзакциям выполняться в одном файле WAL. Файл WAL объединяется в файл базы данных в специальной операции, называемой _контрольной точкой_.

WAL может быть быстрее для локальных баз данных, так как модули чтения и записи не блокируют друг друга, что позволяет выполнять операции чтения и записи одновременно. Однако режим WAL не допускает изменения _размера страницы_, добавляет к базе данных дополнительные ассоциации файлов и добавляет операцию дополнительной _контрольной точки_ .

Чтобы включить WAL в SQLite.NET, вызовите `EnableWriteAheadLoggingAsync` метод для `SQLiteAsyncConnection` экземпляра:

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

Дополнительные сведения см. в статье [SQLite Write-Ahead Logging](https://www.sqlite.org/wal.html) on SQLite.org.

### <a name="copy-a-database"></a>Копирование базы данных

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
- [асинклази](https://devblogs.microsoft.com/pfxteam/asynclazyt/)
