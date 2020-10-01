---
title: Использование контактов ContentProvider
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: 8c3608837132e221bec12b2c00fb2dd0b1730e76
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454082"
---
# <a name="using-the-contacts-contentprovider"></a>Использование контактов ContentProvider

Код, который использует данные для доступа, предоставленные в `ContentProvider`, не требует наличия ссылок на класс `ContentProvider`. Вместо этого в нем используется URI для создания курсора на основе данных, предоставляемых `ContentProvider`. Android использует этот URI для поиска установленного в системе приложения, которое предоставляет `ContentProvider` с указанным идентификатором. URI имеет строковое значение, обычно в формате обратного DNS, например `com.android.contacts/data`.

Чтобы разработчикам не нужно было запоминать эту строку, поставщик Android *Contacts* предоставляет ее метаданные в классе `android.provider.ContactsContract`. Этот класс позволяет определить URI для `ContentProvider`, а также имена таблиц и столбцов, к которым можно обращаться в запросах.

Для доступа к некоторым типам данных также требуется специальное разрешение. Для встроенного списка контактов должно присутствовать разрешение `android.permission.READ_CONTACTS` в файле **AndroidManifest.xml**.

Существует три способа создания курсора на основе URI.

1. **ManagedQuery()** &ndash; это рекомендуемый подход в Android 2.3 (API уровня 10) и более ранних версиях, при котором `ManagedQuery` возвращает курсор и автоматически управляет обновлением данных и закрытием этого курсора. Этот метод является нерекомендуемым, начиная с Android 3.0 (API уровня 11).

1. **ContentResolver.Query()** &ndash; возвращает неуправляемый курсор, что означает необходимость обновлять его и закрывать явным образом в коде.

1. **CursorLoader().LoadInBackground()** &ndash; добавленный в Android 3.0 (API уровня 11) вариант, теперь `CursorLoader` является предпочтительным для использования `ContentProvider`. `CursorLoader` запрашивает `ContentResolver` в фоновом потоке, чтобы не блокировать пользовательский интерфейс.
   К этому классу можно обращаться и в более ранних версиях Android, используя библиотеку совместимости версии 4.

Каждый из этих методов предоставляет одинаковый набор базовых входных данных.

- **Uri** &ndash; полное доменное имя для `ContentProvider`.
- **Projection** &ndash; определяет выбор столбцов для использования в курсоре.
- **Selection** &ndash; действует аналогично предложению `WHERE` в SQL.
- **SelectionArgs** &ndash; это параметры для замены в аргументе Selection.
- **SortOrder** &ndash; список столбцов для сортировки.

## <a name="creating-inputs-for-a-query"></a>Создание входных данных для запроса

В примере кода `ContactsProvider` выполняется очень простой запрос к встроенному поставщику контактов Android (Contacts). Вам не нужно знать реальный URI или имена столбцов, так как все необходимые сведения для запроса к Contacts `ContentProvider` доступны в виде констант, предоставляемых классом `ContactsContract`.

Независимо от того, какой метод используется для получения курсора, в роли параметров выступают одни и те же объекты, как показано в файле *ContactsProvider/ContactsAdapter.cs*.

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

В этом примере `selection`, `selectionArgs` и `sortOrder` игнорируются, и для них установлено значение `null`.

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Создание курсора из URI поставщика содержимого

После создания объектов параметров их можно использовать одним из следующих трех способов.

### <a name="using-a-managed-query"></a>Использование управляемого запроса

В приложениях, предназначенных для Android 2.3 (API уровня 10) или более ранней версии, должен использоваться следующий метод:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Этот курсор будет управляться Android, поэтому вам не нужно его закрывать.

### <a name="using-contentresolver"></a>Использование ContentResolver

Можно получить курсор для `ContentProvider`, обращаясь к `ContentResolver` напрямую следующим образом:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Этот курсор является неуправляемым, то есть вам нужно его закрыть, когда он больше не требуется.
Обязательно закройте в коде все открытые курсоры, в противном случае возникнет ошибка.

```csharp
cursor.Close();
```

Кроме того, вы можете вызывать `StartManagingCursor()` и `StopManagingCursor()`, чтобы "управлять" курсором. Управляемые курсоры отключаются и повторно запрашиваются автоматически при остановке и перезапуске действий.

### <a name="using-cursorloader"></a>Использование CursorLoader

В приложениях, разработанных для Android 3.0 (API уровня 11) или более новой версии, следует использовать следующий метод:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

Здесь `CursorLoader` гарантирует, что все операции с курсорами выполняются в фоновом потоке, а также рациональным образом используют уже существующий курсор для других экземпляров действия при его перезапуске (например, из-за изменения конфигурации), что позволяет обойтись без повторной загрузки данных.

Более ранние версии Android также могут использовать класс `CursorLoader` с помощью [библиотек поддержки версии 4](https://developer.android.com/tools/support-library/index.html).

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Отображение данных курсора с помощью пользовательского адаптера

Чтобы отобразить изображение контакта, мы применим пользовательский адаптер, который позволяет вручную разрешить ссылку `PhotoId` в путь к файлу изображения.

Чтобы отобразить данные с помощью пользовательского адаптера, этот пример использует `CursorLoader` для получения всех данных о контактах в локальную коллекцию в методе **FillContacts** из файла **ContactsProvider/ContactsAdapter.cs**:

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

Затем реализуйте методы BaseAdapter с помощью коллекции `contactList`. Этот адаптер реализуется так же, как и для любой другой коллекции. &ndash;Особые методы обработки не требуются, так как источником данных является `ContentProvider`:

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

Изображение (если оно существует) отображается по значению URI, которое обозначает нужный файл изображения на устройстве. Это приложение выглядит примерно так:

[![Снимок экрана приложения, которое отображает контакты в ListView и изображение одного контакта слева от записи](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

Используя аналогичный шаблон кода, приложение может обращаться к любым системным данным, включая фотографии, видео и музыку пользователя.
Некоторые типы данных должны запрашивать специальные разрешения в файле **AndroidManifest.xml** проекта.

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Отображение данных курсора с помощью SimpleCursorAdapter

Курсор также может отображаться с помощью `SimpleCursorAdapter` (хотя здесь отображается только имя, но не фотография). В этом коде показано, как использовать `ContentProvider` с `SimpleCursorAdapter` (этот код не включен в пример):

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

Дополнительные сведения о реализации `SimpleCursorAdapter` см. в руководстве [по ListViews и адаптерам](~/android/user-interface/layouts/list-view/index.md).

## <a name="related-links"></a>Связанные ссылки

- [Демонстрационный пример для ContactsAdapter](/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)