---
title: Xamarin.Essentials. Параметры
description: В этом документе описан класс Preferences в Xamarin.Essentials, который позволяет сохранять параметры приложения в хранилище данных типа "ключ-значение". Здесь рассматривается использование класса и типы данных, которые можно сохранить.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 01/15/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 07bfcabc7ffef20bee43531bfab3e78155beb9a9
ms.sourcegitcommit: 58247fe066ad271ee43c8967ac3301fdab6ca2d1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92629578"
---
# <a name="no-locxamarinessentials-preferences"></a>Xamarin.Essentials. Параметры

Класс **Preferences** помогает хранить параметры приложения в хранилище ключей и значений.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-preferences"></a>Использование Preferences

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Чтобы сохранить значение определенного _ключа_ в параметрах, используйте следующий код:

```csharp
Preferences.Set("my_key", "my_value");
```

Чтобы извлечь значение из настроек или значение по умолчанию (если значение не задано в настройках):

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Чтобы проверить, существует ли определенный _ключ_ в параметрах, используйте следующий код:

```csharp
bool hasKey = Preferences.ContainsKey("my_key");
```

Чтобы удалить _ключ_ из параметров, используйте следующий код:

```csharp
Preferences.Remove("my_key");
```

Чтобы удалить все параметры, используйте следующий код:

```csharp
Preferences.Clear();
```

> [!TIP]
> Приведенные выше методы принимают необязательный параметр типа `string` с именем `sharedName`. Этот параметр используется для создания дополнительных контейнеров для настроек, которые полезны в некоторых вариантах использования. Один из сценариев заключается в том, что приложение должно обмениваться настройками с расширениями или приложением наблюдения. Особенности реализации для платформ см. ниже.

## <a name="supported-data-types"></a>Поддерживаемые типы данных

Следующие типы данных поддерживаются в классе **Preferences** :

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="integrate-with-system-settings"></a>Интеграция с параметрами системы

Настройки хранятся в собственном коде, что позволяет интегрировать параметры в собственные системные параметры. Для интеграции с платформой используйте документацию и примеры платформы:

* Apple: [Реализация пакета параметров iOS](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/UserDefaults/Preferences/Preferences.html)
* [Пример настроек приложения для iOS](/samples/xamarin/ios-samples/appprefs/)
* [Параметры watchOS](https://developer.xamarin.com/guides/ios/watch/working-with/settings/)
* Android: [Начало работы с экранами параметров](https://developer.android.com/guide/topics/ui/settings.html)

## <a name="implementation-details"></a>Сведения о реализации

Значения `DateTime` хранятся в 64-разрядном двоичном формате (длинное целое) с использованием двух методов, определенных классом `DateTime`. Метод [`ToBinary`](xref:System.DateTime.ToBinary) используется для кодирования значения `DateTime`, а метод [`FromBinary`](xref:System.DateTime.FromBinary(System.Int64)) декодирует значение. Ознакомьтесь с документацией об этих методах для возможной корректировки декодированных значений, если сохраняется значение `DateTime` не в формате UTC.

## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

Все данные хранятся в объекте [SharedPreferences](https://developer.android.com/training/data-storage/shared-preferences.html). Если свойство `sharedName` не указано, используются общие настройки по умолчанию. В противном случае используется имя для получения **частных** общих настроек с указанным именем.

# <a name="ios"></a>[iOS](#tab/ios)

[NSUserDefaults](../ios/app-fundamentals/user-defaults.md) используются для хранения значений на устройствах iOS. Если свойство `sharedName` не указано, используется `StandardUserDefaults`. В противном случае используется имя для создания `NSUserDefaults` с определенным именем, используемым для `NSUserDefaultsType.SuiteName`.

# <a name="uwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](/uwp/api/windows.storage.applicationdatacontainer) используется для хранения значений на устройстве. Если свойство `sharedName` не указано, используется `LocalSettings`. В противном случае используется имя для создания контейнера в `LocalSettings`.

`LocalSettings` также имеет следующее ограничение: длина имени каждого параметра не может превышать 255 символов. Каждый параметр может иметь размер до 8 КБ, а каждый составной параметр — до 64 КБ.

--------------

## <a name="persistence"></a>Сохраняемость

Удаление приложения приводит к удалению всех _настроек_ за одним исключением: приложения, которые выполняются на платформе Android 6.0 (API уровня 23) или более поздних версий и используют [__автоматическое резервное копирование__](https://developer.android.com/guide/topics/data/autobackup). Эта функция включена по умолчанию и сохраняет данные приложения, включая __общие параметры__ , которые использует API **параметров**. Ее можно отключить, выполнив следующие шаги в [документации Google](https://developer.android.com/guide/topics/data/autobackup).

## <a name="limitations"></a>Ограничения

При сохранении строки этот API сохраняет небольшие блоки текста. Производительность может быть низкой, если сохранять большие объемы текста.

## <a name="api"></a>API

- [Исходный код класса Preferences](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Preferences)
- [Документация по API параметров](xref:Xamarin.Essentials.Preferences)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Preferences-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
