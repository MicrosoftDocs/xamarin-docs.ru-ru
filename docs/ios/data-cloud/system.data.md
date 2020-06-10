---
title: System. Data в Xamarin. iOS
description: В этом документе описывается, как использовать System. Data и Mono. Data. SQLite. dll для доступа к данным SQLite в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: 736d70aebcf861b5557d5f076a42ff0a3dcfc043
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569961"
---
# <a name="systemdata-in-xamarinios"></a>System. Data в Xamarin. iOS

В Xamarin. iOS 8,10 добавлена поддержка [System. Data](xref:System.Data), включая `Mono.Data.Sqlite.dll` поставщика ADO.NET. Поддержка включает в себя добавление следующих [сборок](~/cross-platform/internals/available-assemblies.md):

- `System.Data.dll`
- `System.Data.Service.Client.dll`
- `System.Transactions.dll`
- `Mono.Data.Tds.dll`
- `Mono.Data.Sqlite.dll`

<a name="Example"></a>

## <a name="example"></a>Пример

Следующая программа создает базу данных в `Documents/mydb.db3` , а если база данных не существует, она заполняется демонстрационными данными. База данных запрашивается, а выходные данные записываются в `stderr` .

### <a name="add-references"></a>Добавить ссылки

Сначала щелкните правой кнопкой мыши узел **ссылки** и выберите **изменить ссылки...** , а затем выберите `System.Data` и `Mono.Data.Sqlite` :

[![](system.data-images/edit-references-sml.png "Adding new references")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Пример кода

В следующем коде показан простой пример создания таблицы и вставки строк с помощью внедренных команд SQL:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;

class Demo {
    static void Main (string [] args)
    {
        var connection = GetConnection ();
        using (var cmd = connection.CreateCommand ()) {
            connection.Open ();
            cmd.CommandText = "SELECT * FROM People";
            using (var reader = cmd.ExecuteReader ()) {
                while (reader.Read ()) {
                    Console.Error.Write ("(Row ");
                    Write (reader, 0);
                    for (nint i = 1; i < reader.FieldCount; ++i) {
                        Console.Error.Write(" ");
                        Write (reader, i);
                    }
                    Console.Error.WriteLine(")");
                }
            }
            connection.Close ();
        }
    }

    static SqliteConnection GetConnection()
    {
        var documents = Environment.GetFolderPath (
                Environment.SpecialFolder.Personal);
        string db = Path.Combine (documents, "mydb.db3");
        bool exists = File.Exists (db);
        if (!exists)
            SqliteConnection.CreateFile (db);
        var conn = new SqliteConnection("Data Source=" + db);
        if (!exists) {
            var commands = new[] {
            "CREATE TABLE People (PersonID INTEGER NOT NULL, FirstName ntext, LastName ntext)",
            // WARNING: never insert user-entered data with embedded parameter values
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (1, 'First', 'Last')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (2, 'Dewey', 'Cheatem')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (3, 'And', 'How')",
            };
            conn.Open ();
            foreach (var cmd in commands) {
                using (var c = conn.CreateCommand()) {
                    c.CommandText = cmd;
                    c.CommandType = CommandType.Text;
                    c.ExecuteNonQuery ();
                }
            }
            conn.Close ();
        }
        return conn;
    }

    static void Write(SqliteDataReader reader, int index)
    {
        Console.Error.Write("({0} '{1}')",
                reader.GetName(index),
                reader [index]);
    }
}
```

> [!IMPORTANT]
> Как упоминалось в примере кода выше, не рекомендуется внедрять строки в команды SQL, так как это делает код уязвимым для [внедрения кода SQL](https://en.wikipedia.org/wiki/SQL_injection).

### <a name="using-command-parameters"></a>Использование параметров команды

В следующем коде показано, как использовать параметры команды для безопасного ввода текста, введенного пользователем, в базу данных (даже если текст содержит специальные символы SQL, например одинарные апострофы):

```csharp
// user input from Textbox control
var fname = fnameTextbox.Text;
var lname = lnameTextbox.Text;
// use command parameters to safely insert into database
using (var addCmd = conn.CreateCommand ()) {
    addCmd.CommandText = "INSERT INTO [People] (PersonID, FirstName, LastName) VALUES (@COL1, @COL2, @COL3)";
    addCmd.CommandType = System.Data.CommandType.Text;
    addCmd.AddParameterWithValue ("@COL1", 1);
    addCmd.AddParameterWithValue ("@COL2", fname);
    addCmd.AddParameterWithValue ("@COL3", lname);
    addCmd.ExecuteNonQuery ();
}
```

<a name="Missing_Functionality"></a>

## <a name="missing-functionality"></a>Отсутствуют функциональные возможности

В **System. Data** и **Mono. Data. SQLite** отсутствуют некоторые функциональные возможности.

<a name="System.Data"></a>

### <a name="systemdata"></a>System.Data

Функции, отсутствующие в **System. Data. dll** , состоят из следующих компонентов:

- Все, что требует [System. CodeDom](xref:System.CodeDom) (например,  [System. Data. TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
- Поддержка XML-файлов конфигурации (например,  [System. Data. Common. дбпровидерконфигуратионхандлер](xref:System.Data.Common.DbProviderConfigurationHandler) )
- [System. Data. Common. дбпровидерфакториес](xref:System.Data.Common.DbProviderFactories) (зависит от поддержки XML-файла конфигурации)
- [System.Data.OleDb](xref:System.Data.OleDb)
- [System.Data.Odbc](xref:System.Data.Odbc)
- `System.EnterpriseServices.dll`Зависимость *удалена* из `System.Data.dll` , что привело к удалению метода [SqlConnection. енлистдистрибутедтрансактион (ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*) .

<a name="Mono.Data.Sqlite"></a>

### <a name="monodatasqlite"></a>Mono. Data. SQLite

В то же время, **Mono. Data. SQLite. dll** не имеет изменений исходного кода, но вместо этого может размещаться ряд проблем со *временем выполнения* , начиная с `Mono.Data.Sqlite.dll` привязки SQLite 3,5. с iOS 8, в то же время поставляется с SQLite 3.8.5. Достаточно сказать, что некоторые вещи изменились между двумя версиями.

Более старая версия iOS поставляется со следующими версиями SQLite:

- Версия-3.7.13 **iOS 7** .
- Версия-3.7.13 **iOS 6** .
- Версия-3.7.7 **iOS 5** .
- **iOS 4** — версия 3.6.22.

Чаще всего возникают проблемы, связанные с запросами к схеме базы данных, например определение во время выполнения, какие столбцы существуют в данной таблице, например `Mono.Data.Sqlite.SqliteConnection.GetSchema` (переопределение [DbConnection. GetSchema](xref:System.Data.Common.DbConnection.GetSchema) и `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` (переопределение [DbDataReader. GetSchemaTable](xref:System.Data.Common.DbDataReader.GetSchemaTable)). Вкратце, кажется, что все, что использует [DataTable](xref:System.Data.DataTable) , вряд ли будет работать.

<a name="Data_Binding"></a>

## <a name="data-binding"></a>Привязка данных

Привязка данных в настоящее время не поддерживается.
