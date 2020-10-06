---
title: 'Xamarin.Essentials: Средство выбора файлов'
description: Класс FilePicker в Xamarin.Essentials позволяет пользователю выбрать один или несколько файлов на устройстве.
ms.assetid: 00bdbd57-56b1-47ca-8abe-cebe1b01f61a
author: jamesmontemagno
ms.author: jamont
ms.date: 09/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b3ee20de5534329f9e4686c908fe923ba94f3fff
ms.sourcegitcommit: 744f977b0595f489c592e29c8a3ba548fde02b6f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2020
ms.locfileid: "91414693"
---
# <a name="no-locxamarinessentials-file-picker"></a>Xamarin.Essentials: Средство выбора файлов

Класс **FilePicker** позволяет пользователю выбрать один или несколько файлов на устройстве.

![Предварительный выпуск API](~/media/shared/preview.png)

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Для доступа к функции **FilePicker** нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Требуется разрешение `ReadExternalStorage`, которое следует настроить в проекте Android. Для этого можно применить любой из следующих методов:

Откройте файл **AssemblyInfo.cs** в папке **Свойства** и добавьте в него:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.ReadExternalStorage)]
```

ИЛИ обновите манифест Android:

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**.

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

ИЛИ щелкните правой кнопкой мыши проект Android и откройте свойства проекта. В разделе **Манифест Android** найдите область **Требуемые разрешения:** и установите флажок для этого разрешения. Это действие автоматически обновляет файл **AndroidManifest.xml**.

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="pick-file"></a>Выбор файла

Метод `FilePicker.PickAsync()` позволяет пользователю выбрать файл на устройстве. Вы можете указать различные параметры `PickOptions` при вызове метода, что позволяет задать отображаемый заголовок и типы файлов, которые пользователю разрешено выбирать. По умолчанию 

```csharp
async Task<FileResult> PickAndShow(PickOptions options)
{
    try
    {
        var result = await FilePicker.PickAsync();
        if (result != null)
        {
            Text = $"File Name: {result.FileName}";
            if (result.FileName.EndsWith("jpg", StringComparison.OrdinalIgnoreCase) ||
                result.FileName.EndsWith("png", StringComparison.OrdinalIgnoreCase))
            {
                var stream = await result.OpenReadAsync();
                Image = ImageSource.FromStream(() => stream);
            }
        }
    }
    catch (Exception ex)
    {
        // The user canceled or something went wrong
    }
}
```

Типы файлов по умолчанию предоставляются с помощью `FilePickerFileType.Images`, `FilePickerFileType.Png` и `FilePickerFilerType.Videos`. При создании `PickOptions` можно указать пользовательские типы файлов для каждой платформы. Например, указать типы файлов комиксов можно следующим образом:

```csharp
var customFileType =
    new FilePickerFileType(new Dictionary<DevicePlatform, IEnumerable<string>>
    {
        { DevicePlatform.iOS, new[] { "public.my.comic.extension" } }, // or general UTType values
        { DevicePlatform.Android, new[] { "application/comics" } },
        { DevicePlatform.UWP, new[] { ".cbr", ".cbz" } },
        { DevicePlatform.Tizen, new[] { "*/*" } },
        { DevicePlatform.macOS, new[] { "cbr", "cbz" } }, // or general UTType values
    });
var options = new PickOptions
{
    PickerTitle = "Please select a comic file",
    FileTypes = customFileType,
};
```

## <a name="pick-multiple-files"></a>Выбор нескольких файлов

Если вы хотите, чтобы пользователь мог выбрать несколько файлов, вызовите метод `FilePicker.PickMultipleAsync()`. Он также принимает параметр `PickOptions` для указания дополнительных сведений. Результат такой же, как и при вызове метода `PickAsync`, но вместо одного объекта `FileResult` возвращается `IEnumerable<FileResult>`, который можно перебирать.

## <a name="api"></a>API

- [Исходный код FilePicker](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/FilePicker)
- [Документация по API FilePicker](xref:Xamarin.Essentials.FilePicker)
