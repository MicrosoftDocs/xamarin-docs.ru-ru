---
title: 'Xamarin.Essentials: Тактильная обратная связь'
description: В этом документе описывается класс HapticFeedback в Xamarin.Essentials, который позволяет управлять тактильной обратной связью на устройстве.
ms.assetid: 4462936c-4018-443b-906d-d63da6d0ed7d
author: dimonovdd
ms.author: jamont
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0b039a8ba7db7b98d30a49b74454b8c0c101f040
ms.sourcegitcommit: 995ee23d93e08dceb8754cc6c682cd2f4594345b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972309"
---
# <a name="no-locxamarinessentials-haptic-feedback"></a>Xamarin.Essentials: Тактильная обратная связь

Класс **HapticFeedback** позволяет управлять тактильной обратной связью на устройстве.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Для доступа к функции **HapticFeedback** нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Требуется разрешение Vibrate (Вибрация), которое следует настроить в проекте Android. Для этого можно применить любой из следующих методов:

Откройте файл **AssemblyInfo.cs** в папке **Свойства** и добавьте в него:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

ИЛИ обновите манифест Android:

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

ИЛИ щелкните правой кнопкой мыши проект Android и откройте свойства проекта. В разделе **Манифест Android** найдите область **Требуемые разрешения:** и установите флажок для разрешения **Vibrate** (Вибрация). Это действие автоматически обновляет файл **AndroidManifest.xml**.

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Различия платформ отсутствуют.

-----

## <a name="using-haptic-feedback"></a>Использование тактильной обратной связи

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Функцию тактильной обратной связи можно использовать с типом обратной связи `Click` или `LongPress`.

```csharp
try
{
    // Perform click feedback
    HapticFeedback.Perform(HapticFeedbackType.Click);

    // Or use long press    
    HapticFeedback.Perform(HapticFeedbackType.LongPress);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="api"></a>API

- [Исходный код HapticFeedback](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/HapticFeedback)
- [Документация по API HapticFeedback](xref:Xamarin.Essentials.HapticFeedback)
