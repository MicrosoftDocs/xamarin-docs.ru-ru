---
название: "Xamarin.Essentials: Сведения о приложении"; описание: "В этом документе описывается класс AppInfo в Xamarin.Essentials, с помощью которого можно получить сведения о вашем приложении. Например, он предоставляет имя и версию приложения".
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA author: jamesmontemagno ms.author: jamont ms.date: 29.01.2019 ms.custom: video no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials. Сведения о приложении

Класс **AppInfo** предоставляет сведения о приложении.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-appinfo"></a>Использование AppInfo

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>Получение сведений о приложении:

Через API предоставляются следующие сведения:

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="displaying-application-settings"></a>Отображение параметров приложения

Класс **AppInfo** также позволяет отобразить страницу параметров, поддерживаемых операционной системой для приложения:

```csharp
// Display settings page
AppInfo.ShowSettingsUI();
```

На этой странице параметров пользователь может изменять разрешения приложения и выполнять другие задачи для платформы.

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

Сведения о приложении получены из следующих полей `AndroidManifest.xml`:

- **Build** — `android:versionCode` на узле `manifest`
- **Name** - `android:label` на узле `application`
- **PackageName**: `package` на узле `manifest`
- **VersionString** — `android:versionName` на узле `application`

# <a name="ios"></a>[iOS](#tab/ios)

Сведения о приложении получены из следующих полей `Info.plist`:

- **Build** — `CFBundleVersion`
- **Name** - `CFBundleDisplayName`, если задано. В противном случае `CFBundleName`
- **PackageName**: `CFBundleIdentifier`
- **VersionString** — `CFBundleShortVersionString`

# <a name="uwp"></a>[UWP](#tab/uwp)

Сведения о приложении получены из следующих полей `Package.appxmanifest`:

- **Build** — использует `Build` из `Version` на узле `Identity`
- **Name** - `DisplayName` на узле `Properties`
- **PackageName**: `Name` на узле `Identity`
- **VersionString** –— `Version` на узле `Identity`

--------------

## <a name="api"></a>API

- [Исходный код AppInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Документация по API AppInfo](xref:Xamarin.Essentials.AppInfo)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/App-Information-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
