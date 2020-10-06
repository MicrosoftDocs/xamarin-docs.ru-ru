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
ms.openlocfilehash: bd239a8dcf192c0bdbc6265769208f4fc989bbbe
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434489"
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

## <a name="picking-a-contact"></a>Выбор контакта

При вызове `Contacts.PickContactAsync()` открывается диалоговое окно контакта, в котором пользователь может получить сведения о контакте.


```csharp
try
{
    var contact = await Contacts.PickContactAsync();

    if(contact == null)
        return;

    var name = contact.Name;
    var contactType = contact.ContactType; // Unknown, Personal, Work
    var numbers = contact.Numbers; // List of phone numbers
    var emails = contact.Emails; // List of email addresses 
    
}
catch (Exception ex)
{
    // Handle exception here.
}
```


## <a name="api"></a>API

- [Исходный код Contacts](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Contacts)
- [Документация по API Contacts](xref:Xamarin.Essentials.Contacts)
