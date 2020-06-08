---
название: "Xamarin.Essentials: Flashlight"; описание: "В этом документе описывается класс Flashlight в Xamarin.Essentials, который позволяет включить или выключить вспышку камеры устройства и превратить ее в фонарик".
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136 author: jamesmontemagno ms.custom: video ms.author: jamont ms.date: 04.11.2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials. Фонарик

Класс **Flashlight** позволяет включить или выключить вспышку камеры устройства и превратить ее в фонарик.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Чтобы получить доступ к функциям класса **Flashlight**, нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Требуются разрешения Flashlight и Camera, которые следует настроить в проекте Android. Для этого можно применить любой из следующих методов:

Откройте файл **AssemblyInfo.cs** в папке **Свойства** и добавьте в него:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

ИЛИ обновите манифест Android:

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

ИЛИ щелкните правой кнопкой мыши проект Android и откройте свойства проекта. В разделе **Манифест Android** найдите область **Требуемые разрешения:** и установите флажок для разрешений **FLASHLIGHT** и **CAMERA**. Это действие автоматически обновляет файл **AndroidManifest.xml**.

После добавления этих разрешений [Google Play будет автоматически отфильтровать устройства](https://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) без конкретного оборудования. Можно обойти это, добавив в файл AssemblyInfo.cs в проекте Android следующий код:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

[!include[](~/essentials/includes/android-permissions.md)]

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="using-flashlight"></a>Использование класса Flashlight

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Фонарик можно включить и выключить с помощью методов `TurnOnAsync` и `TurnOffAsync`:

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

### <a name="android"></a>[Android](#tab/android)

Класс Flashlight был оптимизирован на основе операционной системы устройства.

#### <a name="api-level-23-and-higher"></a>API уровня 23 и более поздних версий

В более поздних уровнях API режим [Torch Mode](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) будет использоваться для включения и выключения вспышки устройства.

#### <a name="api-level-22-and-lower"></a>API уровня 22 и более старых версий

Текстура поверхности камеры предусмотрена для включения или выключения режима `FlashMode` камеры.

### <a name="ios"></a>[iOS](#tab/ios)

[AVCaptureDevice](xref:AVFoundation.AVCaptureDevice) используется для включения и отключения таких режимов устройства, как Torch и Flash.

### <a name="uwp"></a>[UWP](#tab/uwp)

[Lamp](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp) используется для обнаружения первого фонарика на обратной стороне устройства, который можно включить и выключить.

-----

## <a name="api"></a>API

- [Исходный код Flashlight](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Документация по API Flashlight](xref:Xamarin.Essentials.Flashlight)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Flashlight-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
