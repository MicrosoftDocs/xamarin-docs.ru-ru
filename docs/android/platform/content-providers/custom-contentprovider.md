---
title: Создание настраиваемого поставщика ContentProvider
description: В предыдущем разделе было показано, как получать данные из встроенной реализации ContentProvider. В этом разделе объясняется, как создать пользовательский объект ContentProvider и получать из него данные.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 7583cfcb4f1a22167b95370f1376aa70d61188a0
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454056"
---
# <a name="creating-a-custom-contentprovider"></a>Создание настраиваемого поставщика ContentProvider

_В предыдущем разделе было показано, как получать данные из встроенной реализации ContentProvider. В этом разделе объясняется, как создать пользовательский объект ContentProvider и получать из него данные._

## <a name="about-contentproviders"></a>Сведения о ContentProvider

Класс поставщика содержимого должен наследоваться от `ContentProvider`. Он должен состоять из внутреннего хранилища данных, которое используется для реагирования на запросы, и должен предоставлять URI и типы MIME в качестве констант, чтобы в вызывающем его коде было проще создавать допустимые запросы к данным.

### <a name="uri-authority"></a>URI (центр сертификации)

В Android обращение к `ContentProviders` происходит через URI. Приложение, которое предоставляет `ContentProvider`, задает URI, на которые оно будет реагировать, в файле **AndroidManifest.xml**. При установке приложения эти URI регистрируются в системе, чтобы другие приложения могли обращаться к ним.

В Mono для Android класс поставщика содержимого должен иметь атрибут `[ContentProvider]`, в котором указываются URI для включения в **AndroidManifest.xml**.

### <a name="mime-type"></a>Тип MIME

Стандартный формат для типов MIME состоит из двух частей. В `ContentProviders` Android для первой части типа MIME обычно используются следующие две строки:

1. `vnd.android.cursor.item` — для представления одной строки используйте в коде константу `ContentResolver.CursorItemBaseType`;

1. `vnd.android.cursor.dir` — для нескольких строк используйте в коде константу `ContentResolver.CursorDirBaseType`.

Вторая часть типа MIME относится к конкретному приложению и должна соблюдать стандарт обратного DNS-имени с префиксом `vnd.`. В этом примере кода используется `vnd.com.xamarin.sample.Vegetables`.

### <a name="data-model-metadata"></a>Метаданные модели данных

В приложениях, которые будут использовать этот ресурс, нужно создать запросы URI для доступа к разным типам данных. Базовый URI можно расширить, чтобы он указывал на определенную таблицу данных и необязательные параметры для фильтрации результатов. Также нужно объявить столбцы и предложения, которые будут использоваться в полученном курсоре для отображения данных.

Чтобы создавать только допустимые запросы URI, обычно допустимые строковые значения предоставляются в виде констант. Это упрощает организацию доступа к `ContentProvider`, так как нужные значения обнаруживаются функцией завершения кода, что позволяет избежать опечаток.

В предыдущем примере класс `android.provider.ContactsContract` предоставлял метаданные для данных контактов (Contacts). В пользовательском `ContentProvider` мы предоставим константы в самом классе.

## <a name="implementation"></a>Реализация

Создание и использование настраиваемых `ContentProvider` состоит из трех этапов.

1. **Создание класса базы данных** — реализуйте `SQLiteOpenHelper`.

2. **Создание класса `ContentProvider`**  — реализуйте `ContentProvider` с экземпляром базы данных, метаданными в формате констант и методами для доступа к данным.

3. **Обращение к `ContentProvider` через соответствующий URI** — заполните `CursorAdapter` с помощью `ContentProvider`, обращаясь к нему через URI.

Как уже говорилось ранее, `ContentProviders` можно использовать не только в тех в приложениях, где они определены. В нашем примере данные используются в одном приложении, но следует помнить, что другие приложения также могут обращаться к нему, если им известны URI и структура схемы (которые обычно предоставляются в виде констант).

## <a name="create-a-database"></a>Создание базы данных

Большинство реализаций `ContentProvider` будут основываться на базе данных `SQLite`. Пример кода для работы с базой данных в **SimpleContentProvider/VegetableDatabase.cs** создает очень простую базу данных с двумя столбцами, как показано ниже.

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

  public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
  public override void OnCreate(SQLiteDatabase db)
  {
    db.ExecSQL(create_table_sql);
    // seed with data
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
  }
  public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
  {
    throw new NotImplementedException();
  }
}
```

Чтобы предоставлять базу данных через `ContentProvider`, не нужно соблюдать особых требований к реализации, но если вы планируете привязать данные `ContentProvider's` к элементу управления `ListView`, то в результирующий набор должен входить столбец с именем `_id`, содержащий уникальные целочисленные значения. Дополнительные сведения об использовании элемента управления `ListView` см. в документации [по ListView и адаптерам](~/android/user-interface/layouts/list-view/index.md).

## <a name="create-the-contentprovider"></a>Создание ContentProvider

Оставшуюся часть этого раздела занимает пошаговая инструкция по созданию класса для примера **SimpleContentProvider/VegetableProvider.cs**.

### <a name="initialize-the-database"></a>Инициализация базы данных

Прежде всего создайте подкласс `ContentProvider` и добавьте базу данных, которую он будет использовать.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

Остальная часть кода составляет фактическую реализацию поставщика содержимого, который поддерживает обнаружение данных и запрос к ним.

## <a name="add-metadata-for-consumers"></a>Добавление метаданных для объектов-получателей

Существует четыре типа метаданных, которые мы будем предоставлять в классе `ContentProvider`. Из них только центр сертификации является обязательным, для остальных существует соглашение.

- **Authority**. Атрибут `ContentProvider` *обязательно* добавляется в класс, чтобы он был зарегистрирован в Android при установке приложения.

- **URI**. `CONTENT_URI` предоставляется как константа, что позволяет легко использовать его в коде. Его значение совпадает со значением Authority, но с добавлением схемы и базового пути.

- **Типы MIME**. Списки результатов и отдельные результаты рассматриваются как разные типы содержимого, поэтому мы определим два типа MIME для их представления.

- **InterfaceConsts**. Укажите значение в виде константы для каждого имени столбца данных, чтобы в вызывающем коде было проще находить их и указывать ссылки без риска опечаток.

В этом примере кода показано, как реализуется каждый из этих элементов в дополнение к определению базы данных из предыдущего шага:

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```

## <a name="implement-the-uri-parsing-helper"></a>Реализация вспомогательного метода для синтаксического анализа URI

Поскольку в вызывающем коде URI используется для запроса к `ContentProvider`, нам нужна возможность анализировать эти запросы, чтобы правильно найти возвращаемые данные. Класс `UriMatcher` может помочь при анализе URI, если ему передать шаблоны URI, поддерживаемых в `ContentProvider`.

В нашем примере `UriMatcher` инициализируется двумя шаблонами URI:

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; запрос на получение полного списка овощей.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"*  — где \# выполняет роль заполнителя для числового параметра (значение `_id` для строки в базе данных). Для соответствия текстовому параметру можно использовать символ звездочки ("\*") в качестве заполнителя.

В этом коде мы используем константы для ссылки на такие значения метаданных, как AUTHORITY (центр сертификации) и BASE\_PATH (базовый путь). Коды возврата будут использоваться в методах, которые выполняют синтаксический анализ URI, чтобы определить возвращаемые данные.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

Этот код является закрытым для класса `ContentProvider`. Дополнительные сведения см. в [документации по Google UriMatcher](xref:Android.Content.UriMatcher).

## <a name="implement-the-querymethod"></a>Реализация QueryMethod

Из всех методов `ContentProvider` проще всего реализовать метод `Query`. В приведенной ниже реализации `UriMatcher` используется для синтаксического анализа параметра `uri` и вызова правильного метода базы данных. Если `uri` содержит параметр ID, то анализ возвращает целое число (с помощью `LastPathSegment`), которое используется в запросе к базе данных.

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

Метод `GetType` также нужно переопределить. Этот метод может быть вызван для определения типа содержимого, возвращаемого для указанного URI.
Вызывающему приложению это поможет понять, как обрабатывать эти данные.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```

## <a name="implement-the-other-overrides"></a>Реализация других переопределений

Наш простой пример не позволяет изменять или удалять данные, но методы Insert, Update и Delete являются обязательными, поэтому мы добавим их без реализации:

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

На этом простейшая реализация `ContentProvider` завершена. После установки приложения предоставляемые им данные будут доступны не только внутри приложения, но и для любого другого приложения, которое знает URI для ссылки на эти данные.

## <a name="access-the-contentprovider"></a>Использование ContentProvider

Создав реализацию `VegetableProvider`, к ней можно обращаться так же, как к поставщику контактов в начале этого документа: получите курсор по указанному URI и примените адаптер для доступа к данным.

## <a name="bind-a-listview-to-a-contentprovider"></a>Привязка ListView к ContentProvider

Чтобы заполнить `ListView` данными, мы используем URI, соответствующий неотфильтрованному списку овощей. В коде мы используем значение константы `VegetableProvider.CONTENT_URI`, которое разрешается в `com.xamarin.sample.vegetableprovider/vegetables`. Наша реализация `VegetableProvider.Query` вернет курсор, который затем можно привязать к `ListView`.

Код в `SimpleContentProvider/HomeScreen.cs` демонстрирует, насколько просто можно вывести данные из `ContentProvider`:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

Полный вариант приложения выглядит примерно так:

[![Снимок экрана для приложения со списком овощей, фруктов, цветочных бутонов, бобов, луковиц, клубней](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Получение одного элемента из ContentProvider

Вызывающему приложению может потребоваться доступ к отдельным строкам данных, и для этого можно создать другой URI, который (например) указывает конкретную строку.

Используйте `ContentResolver` для прямого доступа к одному элементу, создав URI с требуемым `Id`.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

Полный код метода выглядит следующим образом:

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```

## <a name="related-links"></a>Связанные ссылки

- [SimpleContentProvider (пример)](/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)