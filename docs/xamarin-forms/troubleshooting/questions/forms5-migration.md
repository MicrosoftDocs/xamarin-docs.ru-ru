---
title: Разделы справки перенести приложение в Xamarin.Forms 5,0?
description: Как выполнить миграцию приложения на Xamarin.Forms 5,0 с фокусировкой Android на платформе UWP.
ms.assetid: AD04FEE9-B8F5-4CA5-AB31-EF1225867E4B
ms.prod: xamarin
ms.technology: xamarin-forms
ms.topic: troubleshooting
author: davidbritch
ms.author: dabritch
ms.date: 10/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a8aa964dd2f18998e15d68df72f3cee1bd0ac5a
ms.sourcegitcommit: 86663f94f8eddb808eb4504cd32ddaf217b6406c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98166645"
---
# <a name="how-do-i-migrate-my-app-to-no-locxamarinforms-50"></a>Разделы справки перенести приложение в Xamarin.Forms 5,0?

Xamarin.Forms 5,0 включает следующие критические изменения:

- `Expander` был перемещен в набор инструментов Xamarin Community Toolkit. Дополнительные сведения см. в разделе [компоненты, Xamarin.Forms перемещенные из ](https://github.com/xamarin/XamarinCommunityToolkit/wiki/Features-moved-from-Xamarin.Forms).
- `MediaElement` был перемещен в набор инструментов Xamarin Community Toolkit. Дополнительные сведения см. в разделе [компоненты, Xamarin.Forms перемещенные из ](https://github.com/xamarin/XamarinCommunityToolkit/wiki/Features-moved-from-Xamarin.Forms).
- Страницы и связанные проекты были удалены из Xamarin.Forms .
- `MasterDetailPage` переименован в `FlyoutPage`. Точно так же `MasterBehavior` перечисление переименовано в `FlyoutLayoutBehavior` .
- Ссылки на были `UIWebView` удалены из Xamarin.Forms в iOS.
- Поддержка Visual Studio 2017 была удалена.
- Ксфкорепостпроцессор. Tasks удалены. Этот проект внедрен в IL для обеспечения Xamarin.Forms совместимости 2,5.

Кроме того, для проектов Android и UWP, созданных с помощью 5,0, потребуется Xamarin.Forms обновление.

## <a name="android"></a>Android

Для проектов Android, созданных с помощью Xamarin.Forms 5,0, требуется, чтобы платформа андроидкс (Android 10,0) была установлена в среде разработки. Это можно сделать с помощью диспетчера пакет SDK для Android. Дополнительные сведения о Андроидкс см. [в статье миграция Xamarin.Forms андроидкс в ](~/xamarin-forms/platform/android/androidx-migration.md).

Для правильной сборки проектов Android потребуется несколько обновлений.

### <a name="minimum-targetframeworkversion"></a>Минимум TargetFrameworkVersion

Xamarin.Forms 5,0 требуется минимальная версия целевой платформы 10,0 (Андроидкс) для проектов Android. Целевую версию .NET Framework можно задать в Visual Studio или в файле Android. csproj:

```xml
<TargetFrameworkVersion>v10.0</TargetFrameworkVersion>
```

Если минимальные требования не соблюдены, будет создана ошибка сборки:

```
error XF005: The $(TargetFrameworkVersion) for MyProject.Android (v9.0) is less than the minimum required $(TargetFrameworkVersion) for Xamarin.Forms (10.0). You need to increase the $(TargetFrameworkVersion) for MyProject.Android.
```

### <a name="minimum-targetsdkversion"></a>Минимум TargetSDKVersion

Для Андроидкс требуется, чтобы манифест Android был установлен в значение `targetSdkVersion` 29 +:

```xml
<uses-sdk android:minSdkVersion="21" android:targetSdkVersion="29" />
```

В противном случае `targetSdkVersion` для будет задано значение `minSdkVersion` . Кроме того, в некоторых случаях в `Android.Views.InflateException` случае `targetSdkVersion` неправильного задания будет создано следующее:

```
Android.View.InflateException has been thrown.

Binary XML file line #1 in com.companyname.myproject:layout/toolbar: Binary XML file line #1 in com.companyname.myproject:/layout/toolbar: Error inflating class android.support.v7.widget.Toolbar
```

> [!NOTE]
> В Visual Studio 2019 манифест Android будет автоматически обновлен для указания `targetSdkVersion` API 29, если для целевой версии .NET Framework задано значение v 10.0.

### <a name="automatic-migration-to-androidx"></a>Автоматическая миграция на Андроидкс

Если проект Android ссылается на любые библиотеки поддержки Android, как прямые зависимости или транзитивные зависимости, эти зависимости и привязки библиотеки поддерживают автоматическую замену зависимостей Андроидкс в процессе сборки. Дополнительные сведения об автоматическом переносе Андроидкс см. [в статье Xamarin.Forms автоматическая миграция в ](~/xamarin-forms/platform/android/androidx-migration.md#automatic-migration-in-xamarinforms).

### <a name="manual-migration-to-androidx"></a>Миграция вручную в Андроидкс

Если проект Android не имеет прямых или транзитивных зависимостей от библиотек поддержки Android, но по-прежнему пытается использовать типы библиотек поддержки с помощью кода, потребуется вручную перенести приложение в Андроидкс. Это можно сделать с помощью типов Андроидкс, удалив все ненастроенные файлы AXML.

> [!TIP]
> Миграция вручную в Андроидкс приведет к быстрому процессу сборки для вашего приложения.

#### <a name="use-androidx-types"></a>Использование типов Андроидкс

Андроидкс заменяет библиотеки поддержки Android, поэтому все ссылки на типы библиотек поддержки Android должны быть заменены ссылками на типы Андроидкс.

Это можно сделать, обновив `using` инструкции `AndroidX` , используя пространства имен, а не `Android.Support` пространства имен. В следующей таблице перечислены некоторые распространенные изменения пространств имен при перемещении из библиотек поддержки Android в Андроидкс:

| Пространство имен библиотеки поддержки Android | Пространство имен Андроидкс |
| --- | --- |
| `Android.Support.V4.App` | `AndroidX.Core.App` |
| `Android.Support.V4.Content` | `AndroidX.Core.Content` |
| `Android.Support.V4.App` | `AndroidX.Fragment.App` |
| `Android.Support.V7.App` | `AndroidX.AppCompat.App` |
| `Android.Support.V7.Widget` | `AndroidX.AppCompat.Widget` |

Полный список сопоставлений классов из библиотек поддержки в Андроидкс см. в разделе [сопоставления классов андроидкс](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-class-mapping.csv) в GitHub.com. Полный список сопоставлений сборок из библиотек поддержки в Андроидкс см. в разделе [сборки андроидкс](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-assemblies.csv) на GitHub.com.

#### <a name="remove-axml-files"></a>Удаление файлов AXML

Следует удалить все файлы AXML из проекта Android при условии, что он не использует настраиваемые файлы AXML. После удаления следующие строки следует удалить из `MainActivity` класса:

```csharp
TabLayoutResource = Resource.Layout.Tabbar;
ToolbarResource = Resource.Layout.Toolbar;
```

> [!IMPORTANT]
> Проекты Android с настроенными файлами AXML должны быть обновлены таким образом, чтобы эти файлы использовали типы Андроидкс.

## <a name="uwp"></a>UWP

Xamarin.Forms 5,0 рекомендует версию целевой платформы >= 10.0.18362.0 для проектов UWP. Версию целевой платформы можно задать в Visual Studio или в файле UWP. csproj:

```xml
<TargetPlatformVersion Condition=" '$(TargetPlatformVersion)' == '' ">10.0.18362.0</TargetPlatformVersion>
```

Если проект UWP использует более раннюю версию целевой платформы, будет создано предупреждение сборки.

## <a name="related-links"></a>Связанные ссылки

- [Компоненты, перемещенные из Xamarin.Forms](https://github.com/xamarin/XamarinCommunityToolkit/wiki/Features-moved-from-Xamarin.Forms)
- [Андроидкс миграция в Xamarin.Forms](~/xamarin-forms/platform/android/androidx-migration.md)
- [Сопоставления классов Андроидкс](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-class-mapping.csv)
- [Андроидкс сборки](https://github.com/xamarin/AndroidX/blob/master/mappings/androidx-assemblies.csv)
