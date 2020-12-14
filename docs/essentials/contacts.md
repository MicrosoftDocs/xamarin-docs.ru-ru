---
title: 'Xamarin.Essentials: Контакты'
description: Класс Contacts в Xamarin.Essentials позволяет пользователю выбрать контакт и получить сведения о нем.
ms.assetid: 02280c42-720a-44c3-979e-4818a20c9821
author: jamesmontemagno
ms.author: jamont
ms.date: 09/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 19c9929706b7cf285b22562b094d2de2a25ff77d
ms.sourcegitcommit: 0a41c4aa6db72cd2d0cecbe0dc893024cecac71d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/07/2020
ms.locfileid: "96749895"
---
# <a name="no-locxamarinessentials-contacts"></a>Xamarin.Essentials: Контакты

Класс **Contacts** позволяет пользователю выбрать контакт и получить сведения о нем.

![Предварительный выпуск API](~/media/shared/preview.png)

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Для доступа к функции **Contacts** нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Требуется разрешение `ReadContacts`, которое следует настроить в проекте Android. Для этого можно применить любой из следующих методов:

Откройте файл **AssemblyInfo.cs** в папке **Свойства** и добавьте в него:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.ReadContacts)]
```

ИЛИ обновите манифест Android:

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**.

```xml
<uses-permission android:name="android.permission.READ_CONTACTS" /> />
```

ИЛИ щелкните правой кнопкой мыши проект Android и откройте свойства проекта. В разделе **Манифест Android** найдите область **Требуемые разрешения:** и установите флажок для этого разрешения. Это действие автоматически обновляет файл **AndroidManifest.xml**.

# <a name="ios"></a>[iOS](#tab/ios)

В `Info.plist` добавьте следующие ключи:

```xml
<key>NSContactsUsageDescription</key>
<string>This app needs access to contacts to pick a contact and get info.</string>
```

`<string>` в описании необходимо изменить на требуемый для вашего приложения текст, который будет отображаться для пользователей.

# <a name="uwp"></a>[UWP](#tab/uwp)

В `Package.appxmanifest` в разделе **Возможности**  должна быть выбрана возможность `Contact`.

-----

## <a name="pick-a-contact"></a>Выбор контакта

При вызове `Contacts.PickContactAsync()` открывается диалоговое окно контакта, в котором пользователь может получить сведения о контакте.


```csharp
try
{
    var contact = await Contacts.PickContactAsync();

    if(contact == null)
        return;

    var id = contact.Id;
    var namePrefix = contact.NamePrefix;
    var givenName = contact.GivenName;
    var middleName = contact.MiddleName;
    var familyName = contact.FamilyName;
    var nameSuffix = contact.NameSuffix;
    var displayName = contact.DisplayName;
    var phones = contact.Phones; // List of phone numbers
    var emails = contact.Emails; // List of email addresses
}
catch (Exception ex)
{
    // Handle exception here.
}
```

## <a name="get-all-contacts"></a>Получения списка всех контактов

```csharp
ObservableCollection<Contact> contactsCollect = new ObservableCollection<Contact>();

try
{
    // cancellationToken parameter is optional
    var cancellationToken = default(CancellationToken);
    var contacts = await Contacts.GetAllAsync(cancellationToken);

    if (contacts == null)
        return;

    foreach (var contact in contacts)
        contactsCollect.Add(contact);
}
catch (Exception ex)
{
    // Handle exception here.
}
```

## <a name="platform-differences"></a>Различия между платформами

# <a name="android"></a>[Android](#tab/android)

- Параметр `cancellationToken` в методе `GetAllAsync` используется только в UWP.

# <a name="ios"></a>[iOS](#tab/ios)

- Параметр `cancellationToken` в методе `GetAllAsync` используется только в UWP.
- Платформа iOS изначально не поддерживает свойство `DisplayName`, поэтому значение `DisplayName` формируется в таком виде: GivenName FamilyName.

# <a name="uwp"></a>[UWP](#tab/uwp)

Различия платформ отсутствуют.

-----


## <a name="api"></a>API

- [Исходный код Contacts](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Contacts)
- [Документация по API Contacts](xref:Xamarin.Essentials.Contacts)
