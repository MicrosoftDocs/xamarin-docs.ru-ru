---
title: Цели сборки
description: В этом документе перечислены все цели, поддерживаемые процессом сборки Xamarin.Android.
ms.prod: xamarin
ms.assetid: 17DE89FF-F316-4620-B865-EF6E0963A29C
ms.technology: xamarin-android
author: jonpryor
ms.author: jopryo
ms.date: 09/17/2020
ms.openlocfilehash: 4482e6c4bfe2a6952d59d896b7c79ca82432b42b
ms.sourcegitcommit: 38496cfd4d30fd40a011011f303a31de639bd699
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91247229"
---
# <a name="build-targets"></a>Цели сборки

Для проектов Xamarin.Android определены указанные ниже целевые объекты сборки.

## <a name="build"></a>Сборка

Выполняет сборку исходного кода проекта и всех зависимостей.

Эта цель *не* создает пакет Android (файл `.apk`).
Чтобы создать пакет Android, используйте цель [SignAndroidPackage](#signandroidpackage) *или* задайте для свойства [`$(AndroidBuildApplicationPackage)](~/android/deploy-test/building-apps/build-properties.md#androidbuildapplicationpackage) значение True при сборке:

```shell
msbuild /p:AndroidBuildApplicationPackage=True App.sln
```

## <a name="buildandstartaotprofiling"></a>BuildAndStartAotProfiling

Позволяет создать приложение с внедренным профилировщиком AOT, установить для профилировщика порт TCP [`$(AndroidAotProfilerPort)`](~/android/deploy-test/building-apps/build-properties.md#androidaotprofilerport) и запустить действие по умолчанию.

По умолчанию используется порт TCP `9999`.

Добавлено в Xamarin.Android версии 10.2.

## <a name="clean"></a>Clean

Удаляет все файлы, созданные в процессе сборки.

## <a name="finishaotprofiling"></a>FinishAotProfiling

Необходимо вызывать *после* цели [BuildAndStartAotProfiling](#buildandstartaotprofiling).

Обеспечивает сбор данных профилировщика AOT с устройства или из эмулятора через TCP-порт [`$(AndroidAotProfilerPort)`](~/android/deploy-test/building-apps/build-properties.md#androidaotprofilerport)
и их запись в [`$(AndroidAotCustomProfilePath)`](~/android/deploy-test/building-apps/build-properties.md#androidaotcustomprofilepath).

По умолчанию для порта и пользовательского профиля используются значения `9999` и `custom.aprof`.

Чтобы передать в `aprofutil` дополнительные параметры, задайте их в свойстве [`$(AProfUtilExtraOptions)`](~/android/deploy-test/building-apps/build-properties.md#aprofutilextraoptions)
.

Это соответствует следующей записи:

```shell
aprofutil $(AProfUtilExtraOptions) -s -v -f -p $(AndroidAotProfilerPort) -o "$(AndroidAotCustomProfilePath)"
```

Добавлено в Xamarin.Android версии 10.2.

## <a name="install"></a>Установка

[Создает, подписывает](#signandroidpackage) и устанавливает пакет Android на виртуальном устройстве или устройстве по умолчанию.

Свойство [`$(AdbTarget)`](~/android/deploy-test/building-apps/build-properties.md#adbtarget) указывает целевое устройство Android, на котором может быть установлен или удален пакет Android.

```bash
# Install package onto emulator via -e
# Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
```

## <a name="signandroidpackage"></a>SignAndroidPackage

Создает и подписывает файл пакета Android (`.apk`).

Используется с `/p:Configuration=Release` для создания автономных пакетов выпуска.

## <a name="startandroidactivity"></a>StartAndroidActivity

Позволяет запустить действие по умолчанию на устройстве или в работающем эмуляторе.

Чтобы запустить другое действие, задайте для свойства [`$(AndroidLaunchActivity)`](~/android/deploy-test/building-apps/build-properties.md#androidlaunchactivity)
имя действия.

Это соответствует следующей записи:

```shell
adb shell am start @PACKAGE_NAME@/$(AndroidLaunchActivity)
```

Добавлено в Xamarin.Android версии 10.2.

## <a name="stopandroidpackage"></a>StopAndroidPackage

Позволяет полностью остановить пакет приложения на устройстве или в работающем эмуляторе.

Это соответствует следующей записи:

```shell
adb shell am force-stop @PACKAGE_NAME@
```

Добавлено в Xamarin.Android версии 10.2.

## <a name="uninstall"></a>Удаление

Удаляет пакет Android на виртуальном устройстве или устройстве по умолчанию.

Свойство [`$(AdbTarget)`](~/android/deploy-test/building-apps/build-properties.md#adbtarget) указывает целевое устройство Android, на котором может быть установлен или удален пакет Android.

## <a name="updateandroidresources"></a>UpdateAndroidResources

Изменяет файл `Resource.designer.cs`.

Этот целевой объект обычно вызывается средой IDE при добавлении новых ресурсов в проект.
