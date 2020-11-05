---
title: 'Xamarin.Essentials: Средство выбора мультимедиа'
description: Класс MediaPicker в Xamarin.Essentials позволяет пользователю выбрать или снять фото или видео на устройстве.
ms.assetid: 23460875-6cf9-4440-a97b-46c55b0bca69
author: jamesmontemagno
ms.author: jamont
ms.date: 09/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 32d05208250a4e9927aac50caa5a42c1c6c59bcb
ms.sourcegitcommit: 4f0223cf13e14d35c52fa72a026b1c7696bf8929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278368"
---
# <a name="no-locxamarinessentials-media-picker"></a>Xamarin.Essentials: Средство выбора мультимедиа

Класс **MediaPicker** позволяет пользователю выбрать или снять фото или видео на устройстве.

![Предварительный выпуск API](~/media/shared/preview.png)

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Для доступа к функции **MediaPicker** нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Указанные ниже разрешения являются обязательными и должны быть настроены в проекте Android. Для этого можно применить любой из следующих методов:

Откройте файл **AssemblyInfo.cs** в папке **Свойства** и добавьте в него:

```csharp
// Needed for Picking photo/video
[assembly: UsesPermission(Android.Manifest.Permission.ReadExternalStorage)]

// Needed for Taking photo/video
[assembly: UsesPermission(Android.Manifest.Permission.WriteExternalStorage)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]

// Add these properties if you would like to filter out devices that do not have cameras, or set to false to make them optional
[assembly: UsesFeature("android.hardware.camera", Required = true)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = true)]
```

ИЛИ обновите манифест Android:

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**.

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.CAMERA" />
```

ИЛИ щелкните правой кнопкой мыши проект Android и откройте свойства проекта. В разделе **Манифест Android** найдите область **Требуемые разрешения:** и установите флажки для этих разрешений. Это действие автоматически обновляет файл **AndroidManifest.xml**.

# <a name="ios"></a>[iOS](#tab/ios)

В `Info.plist` добавьте следующие ключи:

```xml
<key>NSCameraUsageDescription</key>
<string>This app needs access to the camera to take photos.</string>
<key>NSMicrophoneUsageDescription</key>
<string>This app needs access to microphone for taking videos.</string>
<key>NSPhotoLibraryAddUsageDescription</key>
<string>This app needs access to the photo gallery for picking photos and videos.</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>This app needs access to photos gallery for picking photos and videos.</string>
```

Для каждого ключа необходимо изменить `<string>` на требуемое для вашего приложения описание, которое будет отображаться для пользователей.

# <a name="uwp"></a>[UWP](#tab/uwp)

В `Package.appxmanifest` в разделе **Возможности**  должны быть выбраны возможности `Microphone` и `Webcam`.

-----

## <a name="using-media-picker"></a>Использование средства выбора мультимедиа

Класс `MediaPicker` содержит указанные ниже методы, которые возвращают `FileResult` и могут использоваться для получения расположения файлов или его считывания как `Stream`.

* `PickPhotoAsync`: открывает браузер мультимедиа для выбора фотографии.
* `CapturePhotoAsync`: открывает камеру, чтобы сделать снимок.
* `PickVideoAsync`: открывает браузер мультимедиа для выбора видео.
* `CaptureVideoAsync`: открывает камеру, чтобы снять видео.

Каждый метод может принимать параметр `MediaPickerOptions`, который позволяет задавать в некоторых операционных системах заголовок `Title`, отображаемый для пользователей.

> [!TIP]
> Все методы должны вызываться в потоке пользовательского интерфейса, так как проверки разрешений и запросы автоматически обрабатываются Xamarin.Essentials.

## <a name="general-usage"></a>Общие сведения об использовании

```csharp
async Task TakePhotoAsync()
{
    try
    {
        var photo = await MediaPicker.CapturePhotoAsync();
        await LoadPhotoAsync(photo);
        Console.WriteLine($"CapturePhotoAsync COMPLETED: {PhotoPath}");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"CapturePhotoAsync THREW: {ex.Message}");
    }
}

async Task LoadPhotoAsync(FileResult photo)
{
    // canceled
    if (photo == null)
    {
        PhotoPath = null;
        return;
    }
    // save the file into local storage
    var newFile = Path.Combine(FileSystem.CacheDirectory, photo.FileName);
    using (var stream = await photo.OpenReadAsync())
    using (var newStream = File.OpenWrite(newFile))
        await stream.CopyToAsync(newStream);

    PhotoPath = newFile;
}
```


## <a name="api"></a>API

- [Исходный код MediaPicker](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/MediaPicker)
- [Документация по API MediaPicker](xref:Xamarin.Essentials.MediaPicker)
