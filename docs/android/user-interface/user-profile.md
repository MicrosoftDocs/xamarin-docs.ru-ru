---
title: Профиль пользователя
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/22/2018
ms.openlocfilehash: 1b7202a199315bf0be1664bf34725c26d12a66c8
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91458084"
---
# <a name="user-profile"></a>Профиль пользователя

В Android поддерживается перечисление контактов с поставщиком [контактсконтракт](xref:Android.Provider.ContactsContract) с момента API уровня 5. Например, список контактов прост с использованием класса [контактконтрактс. Contacts](xref:Android.Provider.ContactsContract.Contacts) , как показано в следующем примере кода:

```csharp
// Get the URI for the user's contacts:
var uri = ContactsContract.Contacts.ContentUri;

// Setup the "projection" (columns we want) for only the ID and display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id,
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the user's contacts data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();

// Print the contact data to the console if reading back succeeds:
if (cursor != null)
{
    if (cursor.MoveToFirst())
    {
        do
        {
            Console.WriteLine("Contact ID: {0}, Contact Name: {1}",
                               cursor.GetString(cursor.GetColumnIndex(projection[0])),
                               cursor.GetString(cursor.GetColumnIndex(projection[1])));
        } while (cursor.MoveToNext());
    }
}
```

Начиная с Android 4 (API уровня 14) класс [контактсконтакт. Profile](xref:Android.Provider.ContactsContract.Profile) доступен через `ContactsContract` поставщик. `ContactsContact.Profile`Предоставляет доступ к личному профилю для владельца устройства, который содержит контактные данные, такие как имя владельца устройства и номер телефона.

## <a name="required-permissions"></a>Необходимые разрешения

Для чтения и записи контактных данных приложения должны запрашивать `READ_CONTACTS` `WRITE_CONTACTS` разрешения и соответственно.
Кроме того, для чтения и изменения профиля пользователя приложения должны запрашивать `READ_PROFILE` разрешения и `WRITE_PROFILE` .

## <a name="updating-profile-data"></a>Обновление данных профиля

После установки этих разрешений приложение может использовать обычные методы Android для взаимодействия с данными профиля пользователя. Например, чтобы обновить отображаемое имя профиля, вызовите [ContentResolver. Update](xref:Android.Content.ContentResolver.Update*) с параметром, `Uri` полученным через свойство [Контактсконтракт. Profile. контентравконтактсури](xref:Android.Provider.ContactsContract.Profile.ContentRawContactsUri) , как показано ниже:

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>Чтение данных профиля

Выполнив запрос к [контактсконтакт. Profile. контентури](xref:Android.Provider.ContactsContract.Profile.ContentUri) , он считывает данные профиля. Например, следующий код будет считывать отображаемое имя профиля пользователя:

```csharp
// Read the profile
var uri = ContactsContract.Profile.ContentUri;

// Setup the "projection" (column we want) for only the display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();
if (cursor != null)
{
    if (cursor.MoveToFirst ())
    {
        Console.WriteLine(cursor.GetString (cursor.GetColumnIndex (projection [0])));
    }
}
```

## <a name="navigating-to-the-user-profile"></a>Переход к профилю пользователя

Наконец, чтобы выполнить переход к профилю пользователя, создайте намерение с `ActionView` действием и `ContactsContract.Profile.ContentUri` затем передайте его в `StartActivity` метод следующим образом:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);
StartActivity (intent);
```

При выполнении приведенного выше кода профиль пользователя отображается, как показано на следующем снимке экрана:

[![Снимок экрана: профиль, отображающий профиль пользователя Джон Петров](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Работа с профилем пользователя аналогична взаимодействию с другими данными в Android и обеспечивает дополнительный уровень персонализации устройств.

## <a name="related-links"></a>Связанные ссылки

- [Контактспровидердемо (пример)](/samples/xamarin/monodroid-samples/contactsproviderdemo)