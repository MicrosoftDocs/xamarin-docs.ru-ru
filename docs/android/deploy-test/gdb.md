---
title: GDB
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
ms.openlocfilehash: 85516800b2fcc410a9eba3abd0c12fd040cee3da
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571952"
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>Обзор

В Xamarin.Android версии 4.10 добавлена частичная поддержка применения `gdb` через целевое устройство MSBuild `_Gdb`. 

> [!NOTE]
> Для поддержки `gdb` необходимо установить Android NDK.

`gdb` можно использовать тремя способами.

1. [Отладочная сборка с быстрым развертыванием](#Debug_Builds_with_Fast_Deployment).
1. [Отладочная сборка без быстрого развертывания](#Debug_Builds_without_Fast_Deployment).
1. [Сборки выпуска](#Release_Builds).

Если у вас возникли проблемы, переходите к разделу [Устранение неполадок](#Troubleshooting).

<a name="Debug_Builds_with_Fast_Deployment"></a>

### <a name="debug-builds-with-fast-deployment"></a>Отладочная сборка с быстрым развертыванием

При создании и развертывании отладочной сборки с быстрым развертыванием `gdb` можно подключить с помощью целевого устройства MSBuild `_Gdb`.

Прежде всего установите приложение. Это можно сделать в интегрированной среде разработки или с помощью командной строки:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

Затем запустите целевое устройство `_Gdb`. Завершив работу, он выведет командную строку для запуска `gdb`:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

Целевое устройство `_Gdb` запустит произвольное действие запуска, которое объявлено в вашем файле `AndroidManifest.xml`. Чтобы явным образом указать действие, используйте свойство MSBuild `RunActivity`. Запуск службы и других конструкций Android в настоящее время не поддерживается.

Целевое устройство `_Gdb` создаст каталог `gdb-symbols` и скопирует в него содержимое каталогов `/system/lib` и `$APPDIR/lib` из целевого устройства.

> [!NOTE]
> Содержимое каталога `gdb-symbols` связано с целевым устройством Android, в котором вы выполняете развертывание, и не будет автоматически заменяться при изменении целевого устройства. (Можно считать это поведение ошибкой.) Изменив целевое устройство Android, этот каталог необходимо удалить вручную.

И наконец, скопируйте созданную команду `gdb` и выполните ее в вашей оболочке:

```bash
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) bt
#0  0x40082e84 in nanosleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#1  0x4008ffe6 in sleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#2  0x74e46240 in ?? ()
#3  0x74e46240 in ?? ()
(gdb) c
```

<a name="Debug_Builds_without_Fast_Deployment"></a>

## <a name="debug-builds-without-fast-deployment"></a>Отладочная сборка без быстрого развертывания

Отладочные сборки *с* быстрым развертыванием просто копируют программу `gdbserver` Android NDK в каталог быстрого развертывания `.__override__`. Если быстрое развертывание отключено, этот каталог может отсутствовать.

У вас есть два варианта обойти эту проблему.

- Задайте системное свойство `debug.mono.log`, чтобы создался каталог `.__override__`.
- Включите `gdbserver` в `.apk`.

### <a name="setting-the-debugmonolog-system-property"></a>Настройка системного свойства `debug.mono.log`

Чтобы задать системное свойство `debug.mono.log`, используйте команду `adb`:

```bash
$ adb shell setprop debug.mono.log gc
```

Настроив значение системного свойства, запустите целевое устройство `_Gdb` выполните созданную им команду `gdb`, так же как и для отладочной сборки с быстрым развертыванием:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

### <a name="including-gdbserver-in-your-app"></a>Добавление `gdbserver` в приложение

Чтобы включить `gdbserver` в приложение, выполните следующие действия.

1. Найдите `gdbserver` в Android NDK (обычно в каталоге **$ANDROID\_NDK\_PATH/prebuilt/android-arm/gdbserver/gdbserver**) и скопируйте в каталог проекта.

2. Переименуйте `gdbserver` в **libs/armeabi-v7a/libgdbserver.so**.

3. Добавьте **libs/armeabi-v7a/libgdbserver.so** в проект, используя **действие построения**`AndroidNativeLibrary`.

4. Перестройте и повторно установите приложение.

Когда установка приложения завершится, запустите целевое устройство `_Gdb` выполните созданную им команду `gdb`, так же как и для отладочной сборки с быстрым развертыванием:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

<a name="Release_Builds"></a>

## <a name="release-builds"></a>Построения выпуска

Для поддержки `gdb` нужно выполнить три условия.

1. Предоставить разрешение `INTERNET`.
2. Включить отладку приложения.
3. Предоставить доступный `gdbserver`.

Разрешение `INTERNET` по умолчанию включается в приложениях для отладки. Если в вашем приложении оно отсутствует, добавьте его, отредактировав файл **Properties/AndroidManifest.xml** или изменив [свойства проекта](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest).

Чтобы включить отладку приложений, установите свойство настраиваемого атрибута [ApplicationAttribute.Debugging](xref:Android.App.ApplicationAttribute.Debuggable) в значение `true` или отредактируйте файл **Properties/AndroidManifest.xml**, присвоив атрибуту `//application/@android:debuggable` значение`true`:

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

Чтобы предоставить доступный `gdbserver`, выполните инструкции из раздела [Отладочная сборка без быстрого развертывания](#Debug_Builds_without_Fast_Deployment).

Одна досадная мелочь: целевая сборка MSBuild `_Gdb` принудительно завершит все ранее запущенные экземпляры приложения. Это не выполняется на целевых устройствах с версией Android ниже 4.0.

<a name="Troubleshooting"></a>

## <a name="troubleshooting"></a>Устранение неполадок

### <a name="mono_pmip-doesnt-work"></a>`mono_pmip` не работает.

Функция `mono_pmip` (она полезна для [получения кадров управляемого стека](https://www.mono-project.com/docs/debug+profile/debug/#debugging-with-gdb)), экспортируется из `libmonosgen-2.0.so`, который в настоящее время не запрашивается целевым устройством `_Gdb`. (Эта ошибка будет исправлена в будущих выпусках.)

Чтобы включить вызов функций, расположенных в файле `libmonosgen-2.0.so`, скопируйте его с целевого устройства в каталог `gdb-symbols`:

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

После копирования перезапустите сеанс отладки.

### <a name="bus-error-10-when-running-the-gdb-command"></a>"Bus error: 10" при выполнении команды `gdb`

Когда команда `gdb` завершается ошибкой `"Bus error: 10"`, перезапустите устройство Android.

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### <a name="no-stack-trace-after-attach"></a>После присоединения не выполняется трассировка стека

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

Обычно это связано с тем, что содержимое каталога `gdb-symbols` не синхронизируются с целевым устройством Android. (Может быть, вы изменили целевое устройство Android?)

Удалите каталог `gdb-symbols` и повторите попытку.
