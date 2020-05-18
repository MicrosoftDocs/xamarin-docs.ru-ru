---
title: 'Xamarin.Essentials: Разрешения'
description: В этом документе описывается класс разрешений Xamarin.Essentials, который позволяет проверять и запрашивать разрешения среды выполнения.
ms.assetid: 34062D84-3E55-4AF7-A688-8551068B1E57
author: jamesmontemagno
ms.author: jamont
ms.custom: video
ms.date: 01/06/2020
ms.openlocfilehash: fbce02300363c3ec68c35c11afb25342f06f4be1
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/13/2020
ms.locfileid: "83150071"
---
# <a name="xamarinessentials-permissions"></a>Xamarin.Essentials: Разрешения

Класс **Permissions** позволяет проверять и запрашивать разрешения среды выполнения.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

[!include[](~/essentials/includes/android-permissions.md)]

## <a name="using-permissions"></a>Использование разрешений

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

## <a name="checking-permissions"></a>Проверка разрешений

Чтобы проверить текущее состояние разрешения, используйте метод `CheckStatusAsync` вместе с конкретным разрешением, состояние которого необходимо получить.

```csharp
var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
```

Если требуемое разрешение не объявлено, происходит исключение `PermissionException`.

Прежде чем запрашивать разрешение, рекомендуется проверить его состояние. Если пользователь не получал запрос, каждая операционная система возвращает разные состояния по умолчанию. iOS возвращает `Unknown`, тогда как другие ОС возвращают `Denied`.

## <a name="requesting-permissions"></a>Запрос прав доступа

Чтобы запросить разрешение у пользователей, используйте метод `RequestAsync` вместе с конкретным разрешением для запроса. Если пользователь ранее предоставил разрешение и не отменил его, этот метод возвратит `Granted` сразу же, не отображая диалоговое окно.

```csharp
var status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
```

Если требуемое разрешение не объявлено, происходит исключение `PermissionException`.

Обратите внимание, что на некоторых платформах запрос разрешения может быть активирован только один раз. Для последующих запросов разработчику необходимо проверять, находится ли разрешение в состоянии `Denied`, и просить пользователя активировать его вручную.

## <a name="permission-status"></a>Состояние разрешения

При использовании `CheckStatusAsync` или `RequestAsync` будет возвращен объект `PermissionStatus`, который можно использовать для определения следующих шагов:

* Unknown — состояние разрешения неизвестно.
* Denied — пользователь отклонил запрос на разрешение.
* Disabled — эта возможность отключена на устройстве.
* Granted — пользователь предоставил разрешение или оно предоставляется автоматически.
* Restricted — в ограниченном состоянии.

## <a name="available-permissions"></a>Доступные разрешения

Xamarin.Essentials пытается абстрагировать максимально возможное число разрешений. Однако каждая операционная система имеет свой набор разрешений среды выполнения. Кроме того, есть различия при использовании одного API для некоторых разрешений. Ниже приведено руководство по доступным сейчас разрешениям:

Условные обозначения:

* ![Полная поддержка](~/media/shared/yes.png "Полная поддержка") — поддерживается.
* ![Не поддерживается](~/media/shared/no.png "Не поддерживается или не требуется") — не поддерживается или не требуется.

| Разрешение | Android | iOS | UWP | watchOS | tvOS | Tizen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---:
| CalendarRead   | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Поддерживается для watchOS](~/media/shared/yes.png "Поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| CalendarWrite | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Поддерживается для watchOS](~/media/shared/yes.png "Поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| Камера | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Поддерживается для Tizen](~/media/shared/yes.png "Поддерживается для Tizen") |
| ContactsRead | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Поддерживается для UWP](~/media/shared/yes.png "Поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| ContactsWrite | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Поддерживается для UWP](~/media/shared/yes.png "Поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| Фонарик | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Не поддерживается для iOS](~/media/shared/no.png "Не поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Поддерживается для Tizen](~/media/shared/yes.png "Поддерживается для Tizen") |
| LocationWhenInUse | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Поддерживается для UWP](~/media/shared/yes.png "Поддерживается для UWP") | ![Поддерживается для watchOS](~/media/shared/yes.png "Поддерживается для watchOS") | ![Поддерживается для tvOS](~/media/shared/yes.png "Поддерживается для tvOS")  | ![Поддерживается для Tizen](~/media/shared/yes.png "Поддерживается для Tizen") |
| LocationAlways | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Поддерживается для UWP](~/media/shared/yes.png "Поддерживается для UWP") | ![Поддерживается для watchOS](~/media/shared/yes.png "Поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Поддерживается для Tizen](~/media/shared/yes.png "Поддерживается для Tizen") |
| Мультимедиа | ![Не поддерживается для Android](~/media/shared/no.png "Не поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| Микрофон | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Поддерживается для UWP](~/media/shared/yes.png "Поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Поддерживается для Tizen](~/media/shared/yes.png "Поддерживается для Tizen") |
| Телефон | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| Фотографии | ![Не поддерживается для Android](~/media/shared/no.png "Не поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Поддерживается для tvOS](~/media/shared/yes.png "Поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| Напоминания | ![Не поддерживается для Android](~/media/shared/no.png "Не поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Поддерживается для watchOS](~/media/shared/yes.png "Поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| Датчики | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Поддерживается для UWP](~/media/shared/yes.png "Поддерживается для UWP") | ![Поддерживается для watchOS](~/media/shared/yes.png "Поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| SMS | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| Речь | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| StorageRead | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Не поддерживается для iOS](~/media/shared/no.png "Не поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
| StorageWrite | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Не поддерживается для iOS](~/media/shared/no.png "Не поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |

Если разрешение помечено ![не поддерживается](~/media/shared/no.png "не поддерживается"), то оно всегда будет возвращать `Granted` при проверке или запросе.

## <a name="general-usage"></a>Общие сведения об использовании
Ниже приведен пример общих сведений об использовании для обработки разрешений.

```csharp
public async Task<PermissionStatus> CheckAndRequestLocationPermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
    }

    // Additionally could prompt the user to turn on in settings

    return status;
}
```

Каждый тип разрешения может иметь созданный экземпляр, чтобы методы могли вызываться напрямую.

```csharp
public async Task GetLocationAsync()
{
    var status = await CheckAndRequestPermissionAsync(new Permissions.LocationWhenInUse());
    if (status != PermissionStatus.Granted)
    {
        // Notify user permission was denied
        return;
    }

    var location = await Geolocation.GetLocationAsync();
}

public async Task<PermissionStatus> CheckAndRequestPermissionAsync<T>(T permission)
            where T : BasePermission
{
    var status = await permission.CheckStatusAsync();
    if (status != PermissionStatus.Granted)
    {
        status = await permission.RequestAsync();
    }

    return status;
}
```

## <a name="extending-permissions"></a>Расширение разрешений

API разрешений обеспечивает гибкость и расширяемость для приложений, требующих дополнительной проверки или разрешений, которые не предусмотрены в Xamarin.Essentials. Создайте класс, наследуемый от `BasePermission`, и реализуйте необходимые абстрактные методы. Следующее действие

```csharp
public class MyPermission : BasePermission
{
    // This method checks if current status of the permission
    public override Task<PermissionStatus> CheckStatusAsync()
    {
        throw new System.NotImplementedException();
    }

    // This method is optional and a PermissionException is often thrown if a permission is not declared
    public override void EnsureDeclared()
    {
        throw new System.NotImplementedException();
    }

    // Requests the user to accept or deny a permission
    public override Task<PermissionStatus> RequestAsync()
    {
        throw new System.NotImplementedException();
    }
}
```

При реализации разрешения на определенной платформе возможно наследование от класса `BasePlatformPermission`. Это позволяет получить дополнительные вспомогательные методы платформы для автоматической проверки объявлений и может помочь при создании настраиваемых разрешений для группирования. Например, вы можете запросить доступ для чтения и записи к хранилищу на Android, используя следующее настраиваемое разрешение.

Создайте новое разрешение в проекте, из которого вы вызываете разрешения.

```csharp
public partial class ReadWriteStoragePermission  : Xamarin.Essentials.Permissions.BasePlatformPermission
{

}
```

В своем проекте Android дополните это разрешение теми разрешениями, которые вам нужно запросить.

```csharp
public partial class ReadWriteStoragePermission : Xamarin.Essentials.Permissions.BasePlatformPermission
{
    public override (string androidPermission, bool isRuntime)[] RequiredPermissions => new List<(string androidPermission, bool isRuntime)>
    {
        (Android.Manifest.Permission.ReadExternalStorage, true),
        (Android.Manifest.Permission.WriteExternalStorage, true)
    }.ToArray();
}
```

После этого вы сможете вызвать новое разрешение из общей логики.

```csharp
await Permissions.RequestAsync<ReadWriteStoragePermission>();
```

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

У разрешений должны быть соответствующие атрибуты, заданные в файле манифеста Android.

Дополнительные сведения см. в статье [Permissions in Xamarin.Android](https://docs.microsoft.com/xamarin/android/app-fundamentals/permissions) (Разрешения в Xamarin.Android).

# <a name="ios"></a>[iOS](#tab/ios)

У разрешений должна быть совпадающая строка в файле `Info.plist`. После того, как разрешение будет запрошено и отклонено, при повторном запросе этого разрешения всплывающее окно отображаться не будет. Вы должны запрашивать у пользователя вручную настроить параметры на экране параметров приложений в iOS.

Дополнительные сведения о компонентах обеспечения безопасности и конфиденциальности в iOS см. [здесь](https://docs.microsoft.com/xamarin/ios/app-fundamentals/security-privacy).

# <a name="uwp"></a>[UWP](#tab/uwp)

Разрешения должны иметь соответствующие возможности, объявленные в манифесте пакета.

Дополнительные сведения об объявлении возможностей приложения см. [здесь](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

--------------

## <a name="api"></a>API

- [Исходный код разрешений](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Permissions)
- [Документация по API разрешений](xref:Xamarin.Essentials.Permissions)


## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Permissions-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
