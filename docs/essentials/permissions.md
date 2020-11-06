---
title: Xamarin.Essentials. Разрешения
description: В этом документе описывается класс Permissions в Xamarin.Essentials, который позволяет проверять и запрашивать разрешения среды выполнения.
ms.assetid: 34062D84-3E55-4AF7-A688-8551068B1E57
author: jamesmontemagno
ms.author: jamont
ms.custom: video
ms.date: 09/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 01902942c750a3cd278d648fa82499af4c5d3ab6
ms.sourcegitcommit: dac04cec56290fb19034f3e135708f6966a8f035
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92169973"
---
# <a name="no-locxamarinessentials-permissions"></a>Xamarin.Essentials. Разрешения

Класс **Permissions** позволяет проверять и запрашивать разрешения среды выполнения.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

[!include[](~/essentials/includes/android-permissions.md)]

## <a name="using-permissions"></a>Использование разрешений

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

## <a name="checking-permissions"></a>Проверка разрешений

Чтобы проверить текущее состояние разрешения, используйте метод `CheckStatusAsync` вместе с конкретным разрешением, состояние которого необходимо получить.

```csharp
var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
```

Если требуемое разрешение не объявлено, происходит исключение `PermissionException`.

Прежде чем запрашивать разрешение, рекомендуется проверить его состояние. Если пользователь не получал запрос, каждая операционная система возвращает разные состояния по умолчанию. iOS возвращает `Unknown`, тогда как другие ОС возвращают `Denied`. Если отображается состояние `Granted`, то нет необходимости выполнять другие вызовы. В iOS, если отображается состояние `Denied`, необходимо попросить пользователя изменить разрешение в параметрах. В Android, можно вызвать `ShouldShowRationale` для проверки того, отклонил ли пользователь разрешение в прошлом.

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


## <a name="explain-why-permission-is-needed"></a>Пояснение причины, по которой требуется разрешение

![Предварительный выпуск API](~/media/shared/preview.png)

Рекомендуется объяснять, почему приложению требуется определенное разрешение. В iOS необходимо указать строку, которая будет отображаться для пользователя. В Android нет такой возможности, и по умолчанию разрешение имеет состояние "Отключено". Из-за этого не так легко определить, отказался ли пользователь от предоставления разрешения или оно запрашивается впервые. Чтобы определить, следует ли отображать пользовательский интерфейс с пояснением, можно использовать метод `ShouldShowRationale`. Если метод возвращает `true`, значит пользователь отклонил или отключил разрешение в прошлом. На других платформах при вызове этого метода всегда возвращается `false`.

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
| Sms | ![Поддерживается для Android](~/media/shared/yes.png "Поддерживается для Android") | ![Поддерживается для iOS](~/media/shared/yes.png "Поддерживается для iOS") | ![Не поддерживается для UWP](~/media/shared/no.png "Не поддерживается для UWP") | ![Не поддерживается для watchOS](~/media/shared/no.png "Не поддерживается для watchOS") | ![Не поддерживается для tvOS](~/media/shared/no.png "Не поддерживается для tvOS") | ![Не поддерживается для Tizen](~/media/shared/no.png "Не поддерживается для Tizen") |
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
    
    if (status == PermissionStatus.Granted)
        return status;
        
    
    if (status == PermissionStatus.Denied && DeviceInfo.Platform == DevicePlatform.iOS)
    {
        // Prompt the user to turn on in settings
        // On iOS once a permission has been denied it may not be requested again from the application
        return status;
    }
    
    if (Permissions.ShouldShowRationale<Permissions.LocationWhenInUse>())
    {
        // Prompt the user with additional information as to why the permission is needed
    }   

    status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();

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

API разрешений обеспечивает гибкость и расширяемость для приложений, требующих дополнительной проверки или разрешений, которые не включены в Xamarin.Essentials. Создайте класс, наследуемый от `BasePermission`, и реализуйте необходимые абстрактные методы.

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

При реализации разрешения на определенной платформе класс `BasePlatformPermission` может быть унаследован. Это позволяет получить дополнительные вспомогательные методы платформы для автоматической проверки объявлений и может помочь при создании настраиваемых разрешений для группирования. Например, вы можете запросить доступ для чтения и записи к хранилищу на Android, используя следующее настраиваемое разрешение.

```csharp
public class ReadWriteStoragePermission : Xamarin.Essentials.Permissions.BasePlatformPermission
{
    public override (string androidPermission, bool isRuntime)[] RequiredPermissions => new List<(string androidPermission, bool isRuntime)>
    {
        (Android.Manifest.Permission.ReadExternalStorage, true),
        (Android.Manifest.Permission.WriteExternalStorage, true)
    }.ToArray();
}
```

После этого вы сможете вызвать новое разрешение из проекта Android.

```csharp
await Permissions.RequestAsync<ReadWriteStoragePermission>();
```

Если бы вы хотели вызывать этот API из общего кода, можно было бы создать интерфейс и использовать [службу зависимостей](../xamarin-forms/app-fundamentals/dependency-service/index.md) для регистрации и получения реализации.

```csharp
public interface IReadWritePermission
{        
    Task<PermissionStatus> CheckStatusAsync();
    Task<PermissionStatus> RequestAsync();
}
```

Затем реализуйте интерфейс в проекте платформы:

```csharp
public class ReadWriteStoragePermission : Xamarin.Essentials.Permissions.BasePlatformPermission, IReadWritePermission
{
    public override (string androidPermission, bool isRuntime)[] RequiredPermissions => new List<(string androidPermission, bool isRuntime)>
    {
        (Android.Manifest.Permission.ReadExternalStorage, true),
        (Android.Manifest.Permission.WriteExternalStorage, true)
    }.ToArray();
}
```

После этого можно зарегистрировать конкретную реализацию:

```csharp
DependencyService.Register<IReadWritePermission, ReadWriteStoragePermission>();
```
Далее можно разрешить и использовать ее из общего проекта:

```csharp
var readWritePermission = DependencyService.Get<IReadWritePermission>();
var status = await readWritePermission.CheckStatusAsync();
if (status != PermissionStatus.Granted)
{
    status = await readWritePermission.RequestAsync();
}
```

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

У разрешений должны быть соответствующие атрибуты, заданные в файле манифеста Android. Состояние разрешения по умолчанию — "Отклонено".

Дополнительные сведения см. в статье [Разрешения в Xamarin.Android](../android/app-fundamentals/permissions.md).

# <a name="ios"></a>[iOS](#tab/ios)

У разрешений должна быть совпадающая строка в файле `Info.plist`. После того, как разрешение будет запрошено и отклонено, при повторном запросе этого разрешения всплывающее окно отображаться не будет. Вы должны запрашивать у пользователя вручную настроить параметры на экране параметров приложений в iOS. Состояние разрешения по умолчанию — "Неизвестно".

Дополнительные сведения о компонентах обеспечения безопасности и конфиденциальности в iOS см. [здесь](../ios/app-fundamentals/security-privacy.md).

# <a name="uwp"></a>[UWP](#tab/uwp)

Разрешения должны иметь соответствующие возможности, объявленные в манифесте пакета. В большинстве случаев состояние разрешения по умолчанию — "Неизвестно".

Дополнительные сведения об объявлении возможностей приложения см. [здесь](/windows/uwp/packaging/app-capability-declarations).

--------------

## <a name="api"></a>API

- [Исходный код разрешений](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Permissions)
- [Документация по API разрешений](xref:Xamarin.Essentials.Permissions)


## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Permissions-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
