---
title: Использование ADO.NET с Xamarin. iOS
description: В этом документе описывается использование ADO.NET в качестве метода для доступа к SQLite в приложении Xamarin. iOS. Здесь обсуждаются ссылки на сборки, Mono. Data. SQLite и пример Басикдатаакцесс.
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: d72a9722a9d48ea52932e4fd6516c0712dbd693c
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436254"
---
# <a name="using-adonet-with-xamarinios"></a>Использование ADO.NET с Xamarin. iOS

Xamarin имеет встроенную поддержку для базы данных SQLite, доступную в iOS, которая предоставляется с помощью привычного синтаксиса ADO.NET Like. Для использования этих API необходимо написать инструкции SQL, обрабатываемые SQLite, такие как `CREATE TABLE` , `INSERT` и `SELECT` .

## <a name="assembly-references"></a>Ссылки на сборки

Чтобы использовать SQLite для доступа через ADO.NET, необходимо добавить `System.Data` и `Mono.Data.Sqlite` ссылки на проект iOS, как показано здесь (примеры в Visual Studio для Mac и Visual Studio):

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

 ![Ссылки на сборки в Visual Studio для Mac](using-adonet-images/image4.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

  ![Ссылки на сборки в Visual Studio](using-adonet-images/image6.png)

-----

Щелкните правой кнопкой мыши **ссылку > изменить ссылки...** затем щелкните, чтобы выбрать необходимые сборки.

## <a name="about-monodatasqlite"></a>Сведения о Mono. Data. SQLite

Мы будем использовать `Mono.Data.Sqlite.SqliteConnection` класс для создания пустого файла базы данных, а затем создаем экземпляры `SqliteCommand` объектов, которые можно использовать для выполнения инструкций SQL в базе данных.

1. **Создание пустой базы данных** . вызовите `CreateFile` метод с допустимым путем к файлу (IE. WRITED). Перед вызовом этого метода следует убедиться, что файл уже существует, в противном случае новая (пустая) база данных будет создана в верхней части старой, а данные в старом файле будут потеряны:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > `dbPath`Переменная должна быть определена в соответствии с правилами, описанными выше в этом документе.

2. **Создание подключения к базе данных** . После создания файла базы данных SQLite можно создать объект соединения для доступа к данным. Соединение создается со строкой подключения, которая принимает форму `Data Source=file_path` , как показано ниже:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Как упоминалось ранее, соединение не следует использовать повторно в разных потоках. Если вы сомневаетесь, создайте подключение по мере необходимости и закройте его по завершении. но учитывать делать это чаще, чем требуется.

3. **Создание и выполнение команды базы данных** . когда у нас есть подключение, мы можем выполнять на нем произвольные команды SQL. В приведенном ниже коде показана выполняемая инструкция CREATE TABLE.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

При выполнении SQL непосредственно в базе данных необходимо принять обычные меры предосторожности, не позволяющие делать недопустимые запросы, например пытаться создать уже существующую таблицу. Следите за структурой базы данных, чтобы не вызывались Склитиксцептион, например "Таблица ошибок SQLite [элементы] уже существует".

## <a name="basic-data-access"></a>Базовый доступ к данным

Пример кода *DataAccess_Basic* для этого документа выглядит следующим образом при работе в iOS:

 ![Образец ADO.NET для iOS](using-adonet-images/image9.png)

В приведенном ниже коде показано, как выполнять простые операции SQLite и выводить результаты в виде текста в главном окне приложения.

Необходимо включить следующие пространства имен:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

В следующем образце кода показано взаимодействие базы данных целиком.

1. Создание файла базы данных
2. Вставка данных
3. Запрос данных

Эти операции обычно появляются в коде в нескольких местах. Например, можно создать файл базы данных и таблицы при первом запуске приложения и выполнять операции чтения и записи данных на отдельных экранах в приложении. В приведенном ниже примере для этого примера были сгруппированы один метод:

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}
```

## <a name="more-complex-queries"></a>Более сложные запросы

Поскольку SQLite позволяет выполнять произвольные команды SQL для данных, можно выполнять любые инструкции CREATE, INSERT, UPDATE, DELETE или SELECT. Сведения о командах SQL, поддерживаемых SQLite, можно узнать на веб-сайте SQLite. Инструкции SQL выполняются с помощью одного из трех методов объекта Склитекомманд:

- **ExecuteNonQuery** — обычно используется для создания таблицы или вставки данных. Возвращаемым значением для некоторых операций является число затронутых строк, в противном случае — значение-1.
- **ExecuteReader** — используется, когда коллекция строк должна возвращаться в виде  `SqlDataReader` .
- **ExecuteScalar** — извлекает одно значение (например, статистическое выражение).

### <a name="executenonquery"></a>EXECUTENONQUERY

Инструкции INSERT, UPDATE и DELETE возвращают количество затронутых строк. Все остальные инструкции SQL возвращают-1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Следующий метод показывает предложение WHERE в инструкции SELECT. Поскольку код является созданием полной инструкции SQL, он должен позаботиться о необходимости экранирования зарезервированных символов, таких как кавычки (') вокруг строк.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

Метод ExecuteReader возвращает объект Склитедатареадер. В дополнение к методу Read, показанному в примере, другие полезные свойства включают:

- **Ровсаффектед** — количество строк, затронутых запросом.
- **Хасровс** — указывает, были ли возвращены какие-либо строки.

### <a name="executescalar"></a>EXECUTESCALAR

Используйте его для инструкций SELECT, возвращающих одно значение (например, статистическое выражение).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar`Тип возвращаемого значения метода — `object` – необходимо привести результат в зависимости от запроса к базе данных. Результатом может быть целое число в запросе на ВЫБОРку из запроса или строки из одного столбца. Обратите внимание, что это отличается для других методов Execute, возвращающих объект Reader или количество затронутых строк.

## <a name="microsoftdatasqlite"></a>Microsoft.Data.Sqlite

Существует другая библиотека `Microsoft.Data.Sqlite` , которую можно [установить из NuGet](https://www.nuget.org/packages/Microsoft.Data.Sqlite), которая функционально эквивалентна `Mono.Data.Sqlite` и позволяет использовать те же типы запросов.

[Между двумя библиотеками](/dotnet/standard/data/sqlite/compare) и некоторыми [сведениями о Xamarin](/dotnet/standard/data/sqlite/xamarin)есть сравнение. Самое важное для приложений Xamarin. iOS. необходимо включить вызов инициализации:

```csharp
// required for Xamarin.iOS
SQLitePCL.Batteries_V2.Init();
```

## <a name="related-links"></a>Связанные ссылки

- [Базовый доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Расширенный доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Рецепты на данные iOS](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Доступ к данным Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)