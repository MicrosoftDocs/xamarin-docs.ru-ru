---
title: Использование SQLite.NET с Xamarin. iOS
description: Библиотека NuGet PCL SQLite.NET предоставляет простой механизм доступа к данным для приложений Xamarin. iOS. В этом документе содержатся общие сведения об использовании этой библиотеки.
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/18/2018
ms.openlocfilehash: 5b169323c606e48bf7eb0d721029c6d9a9db2831
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571716"
---
# <a name="using-sqlitenet-with-xamarinios"></a>Использование SQLite.NET с Xamarin. iOS

Библиотека SQLite.NET, которую Xamarin рекомендует, — это базовый ORM, позволяющий хранить и извлекать объекты в локальной базе данных SQLite на устройстве iOS.
ORM означает объектно-ориентированное сопоставление — API, позволяющий сохранять и извлекать "объекты" из базы данных без написания инструкций SQL.

<a name="Usage"></a>

## <a name="usage"></a>Использование

Чтобы включить библиотеку SQLite.NET в приложение Xamarin, добавьте в проект следующий пакет NuGet:

- **Имя пакета:** SQLite-NET-PCL
- **Автор**: Фрэнк А. Крюгер (Frank A. Krueger)
- **Идентификатор:** sqlite-net-pcl
- **URL-адрес:** [NuGet.org/Packages/SQLite-NET-PCL](https://www.nuget.org/packages/sqlite-net-pcl/)

[![Пакет NuGet SQLite.NET](using-sqlite-orm-images/image1a-sml.png "Пакет NuGet SQLite.NET")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Доступно несколько различных пакетов SQLite. Убедитесь, что выбран правильный вариант (возможно, он не является самым максимальным результатом поиска).

Получив доступ к библиотеке SQLite.NET, выполните следующие три шага, чтобы использовать ее для доступа к базе данных.

1. **Добавление оператора using** . Добавьте следующий оператор в файлы C#, где требуется доступ к данным:

    ```csharp
    using SQLite;
    ```

1. **Создание пустой базы данных** . ссылку на базу данных можно создать, передав путь к файлу в конструкторе класса склитеконнектион. Нет необходимости проверять, существует ли файл — он будет автоматически создан при необходимости, в противном случае будет открыт существующий файл базы данных.

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

    Переменная dbPath должна быть определена в соответствии с правилами, описанными ранее в этом документе.

1. **Сохранить данные** — после создания объекта склитеконнектион команды базы данных выполняются путем вызова своих методов, таких как CreateTable и INSERT следующим образом:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

1. **Получение данных** . чтобы получить объект (или список объектов), используйте следующий синтаксис:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Пример базового доступа к данным

Пример кода *DataAccess_Basic* для этого документа выглядит следующим образом при работе в iOS. В коде показано, как выполнять простые операции SQLite.NET и выводить результаты в виде текста в главном окне приложения.

**iOS**

 [![Образец SQLite.NET для iOS](using-sqlite-orm-images/image2-sml.png)](using-sqlite-orm-images/image2-sml.png#lightbox)

В следующем примере кода показано взаимодействие базы данных с использованием библиотеки SQLite.NET для инкапсуляции базового доступа к базе данных. Он показывает:

1. Создание файла базы данных
1. Вставка данных путем создания объектов и последующего их сохранения
1. Запрос данных

Необходимо включить следующие пространства имен:

```csharp
using SQLite; // from the github SQLite.cs class
```

Для этого необходимо добавить SQLite в проект, как [здесь](#Usage)выделено. Обратите внимание, что таблица базы данных SQLite определяется путем добавления атрибутов к классу ( `Stock` классу), а не CREATE TABLE команде.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

Использование `[Table]` атрибута без указания параметра имени таблицы приведет к тому, что имя базовой таблицы базы данных будет совпадать с именем класса (в данном случае — «фондовой биржи»). Фактическое имя таблицы важно, если вы пишете SQL-запросы непосредственно к базе данных, а не используете методы доступа к данным ORM. Аналогичным образом `[Column("_id")]` атрибут является необязательным, и при отсутствии столбца в таблицу будет добавлен столбец с тем же именем, что и у свойства в классе.

## <a name="sqlite-attributes"></a>Атрибуты SQLite

Общие атрибуты, которые можно применять к классам для управления их хранением в базовой базе данных, включают:

- **[PrimaryKey]** — этот атрибут можно применить к целочисленному свойству, чтобы принудительно использовать первичный ключ базовой таблицы. Составные первичные ключи не поддерживаются.
- **[Автоувеличение]** — этот атрибут приводит к тому, что значение целочисленного свойства будет автоматически увеличиваться для каждого нового объекта, вставленного в базу данных.
- **[Column (Name)]** &ndash; `name`Параметр задает имя базового столбца базы данных.
- **[Таблица (имя)]** — помечает класс как способный храниться в базовой таблице SQLite с указанным именем.
- **[MaxLength (значение)]** — ограничение длины свойства текста при попыток вставки базы данных. Использование кода должно проверить это перед вставкой объекта, так как этот атрибут является "checked" только при попытке выполнить операцию вставки или обновления базы данных.
- **[Ignore]** — приводит к тому, что SQLite.NET игнорирует это свойство. Это особенно полезно для свойств, которые имеют тип, который не может храниться в базе данных, или свойства, которые не могут быть автоматически разрешены в виде SQLite.
- **[Unique]** — гарантирует уникальность значений в столбце базы данных.

Большинство этих атрибутов являются необязательными. Всегда следует указывать целочисленный первичный ключ, чтобы можно было эффективно выполнять запросы на выборку и удаление данных.

## <a name="more-complex-queries"></a>Более сложные запросы

`SQLiteConnection`Для выполнения других операций с данными можно использовать следующие методы.

- **INSERT** — добавляет новый объект в базу данных.
- **Получить \<T> ** — Пытается получить объект с помощью первичного ключа.
- **Таблица \<T> ** — Возвращает все объекты в таблице.
- **Удалить** — удаляет объект, используя его первичный ключ.
- **Запрос \<T> ** — Выполнение SQL-запроса, возвращающего ряд строк (в виде объектов).
- **EXECUTE** — используйте этот метод (а не `Query` ), если не предполагается, что строки возвращаются из SQL (например, инструкции INSERT, Update и DELETE).

### <a name="getting-an-object-by-the-primary-key"></a>Получение объекта по первичному ключу

SQLite.Net предоставляет метод Get для получения одного объекта на основе его первичного ключа.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Выбор объекта с помощью LINQ

Методы, возвращающие коллекции, поддерживают IEnumerable \<T> , поэтому можно использовать LINQ для запроса или сортировки содержимого таблицы. В следующем коде показан пример использования LINQ для фильтрации всех записей, начинающихся с буквы "A":

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>Выбор объекта с помощью SQL

Несмотря на то, что SQLite.Net может предоставлять доступ к данным на основе объектов, иногда может потребоваться более сложный запрос, чем разрешено LINQ (или может потребоваться более высокая производительность). Вы можете использовать команды SQL с методом query, как показано ниже:

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!IMPORTANT]
> При написании инструкций SQL напрямую вы создаете зависимость от имен таблиц и столбцов в базе данных, которые были созданы из классов и их атрибутов. При изменении этих имен в коде следует помнить, что необходимо обновить все инструкции SQL, написанные вручную.

### <a name="deleting-an-object"></a>Удаление объекта

Первичный ключ используется для удаления строки, как показано ниже:

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Вы можете проверить, `rowcount` сколько строк было затронуто (удалено в этом случае).

## <a name="using-sqlitenet-with-multiple-threads"></a>Использование SQLite.NET с несколькими потоками

SQLite поддерживает три различных режима работы с потоками: *один поток*, *несколько потоков*и *сериализованный*. Если требуется получить доступ к базе данных из нескольких потоков без каких бы то ни было ограничений, можно настроить SQLite для использования **сериализованного** режима потоков. Важно установить этот режим на ранних этапах работы приложения (например, в начале `OnCreate` метода).

Чтобы изменить режим потоковой обработки, вызовите, `SqliteConnection.SetConfig` который находится в `Mono.Data.Sqlite` пространстве имен. Например, эта строка кода настраивает SQLite для **сериализованного** режима:

```csharp
using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

## <a name="related-links"></a>Связанные ссылки

- [Базовый доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Расширенный доступ к данным (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Доступ к данным Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)
