---
title: Процесс сборки
description: В этом документе приводятся общие сведения о процессе сборки Xamarin.Android.
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2021
ms.openlocfilehash: bc47c9a939188a8bf4d70e11e81e9c43c948f8f4
ms.sourcegitcommit: 3aa9bdcaaedca74ab5175cb2338a1df122300243
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101749308"
---
# <a name="build-process"></a>Процесс сборки

Процесс сборки Xamarin.Android отвечает за объединение процессов: [создание `Resource.designer.cs`](~/android/internals/api-design.md), поддержку [действий сборки](~/android/deploy-test/building-apps/build-items.md) [`@(AndroidAsset)`](~/android/deploy-test/building-apps/build-items.md#androidasset), [`@(AndroidResource)`](~/android/deploy-test/building-apps/build-items.md#androidresource) и других, создание [вызываемых в Android оболочек](~/android/platform/java-integration/android-callable-wrappers.md) и файла `.apk` для выполнения на устройствах Android.

## <a name="application-packages"></a>Пакеты приложений

В широком смысле существует два типа пакетов приложений для Android (файлы `.apk`), которые может создавать система сборки Xamarin.Android:

- Сборки **выпуска**, которые полностью автономны и не требуют дополнительных пакетов для выполнения. Это пакеты, которые будут предоставлены в магазине приложений.

- Сборки **отладки**, которые противоположны сборкам выпуска.

Эти типы пакетов соответствуют MSBuild, `Configuration` который создает пакет.

## <a name="shared-runtime"></a>Общая среда выполнения

До Xamarin.Android 11.2 *общая среда выполнения* представляла собой пару дополнительных пакетов Android, которые предоставляют библиотеку базовых классов (`mscorlib.dll` и т. д.) и библиотеку привязок Android (`Mono.Android.dll` и т. д.). Отладочные сборки используют общую среду выполнения вместо библиотеки базовых классов и привязок в пакете приложения для Android, благодаря чему размер пакета отладки меньше.

Общую среду выполнения можно отключить в отладочных сборках, установив для свойства [`$(AndroidUseSharedRuntime)`](~/android/deploy-test/building-apps/build-properties.md#androidusesharedruntime)
значение `False`.

Поддержка общей среды выполнения была удалена в Xamarin.Android 11.2.

<a name="Fast_Deployment"></a>

## <a name="fast-deployment"></a>Быстрое развертывание

*Быстрое развертывание* обеспечивает дополнительное сжатие пакета приложения Android. Это достигается за счет исключения сборок приложения из пакета, а также развертывания сборок приложения непосредственно во внутреннем каталоге `files` приложения, обычно расположенном в `/data/data/com.some.package`. Внутренний каталог `files` не является глобальной папкой, доступной для записи, поэтому для выполнения всех команд для копирования файлов в этот каталог используется средство `run-as`.

Этот процесс ускоряет цикл сборки, развертывания и отладки, потому что если изменяются *только* сборки, пакет не переустанавливается.
C целевым устройством повторно синхронизируются только обновленные сборки.

> [!ПРЕДУПРЕЖДЕНИЕ> Быстрое развертывание на устройствах, которые блокируют `run-as`, в число которых зачастую входят устройства старше Android 5.0, завершается сбоем.

Быстрое развертывание включено по умолчанию. Чтобы отключить его в отладочных сборках, нужно установить для свойства `$(EmbedAssembliesIntoApk)` значение `True`.

Совместно с этой функцией можно использовать [расширенный режим развертывания](~/android/deploy-test/building-apps/build-properties.md#androidfastdeploymenttype), чтобы еще больше ускорить развертывание.
Обе сборки, собственные библиотеки, карты типов будут развернуты в каталоге `files`. Но это необходимо только в том случае, если вы изменяете собственные библиотеки, привязки или код Java.

## <a name="msbuild-projects"></a>Проекты MSBuild

Процесс сборки Xamarin.Android основан на MSBuild, который также является файлом проекта и используется в Visual Studio для Mac и Visual Studio.
Обычно пользователям не нужно вручную редактировать файлы MSBuild: IDE создает полностью функциональные проекты и обновляет их по мере внесения изменений, а также автоматически вызывает целевые объекты сборки по мере необходимости.

Опытным пользователям может потребоваться сделать что-то, не поддерживаемое графическим интерфейсом IDE, поэтому процесс сборки настраивается путем непосредственного редактирования файла проекта.
Здесь рассматриваются только специфические для Xamarin.Android функции и настройки. С обычными элементами, свойствами и целями MSBuild можно выполнять многие другие вещи.

<a name="Build_Targets"></a>

## <a name="binding-projects"></a>Привязка проектов

С [проектами привязки](~/android/platform/binding-java-library/index.md) используются следующие свойства MSBuild:

- [`$(AndroidClassParser)`](~/android/deploy-test/building-apps/build-properties.md#androidclassparser)
- [`$(AndroidCodegenTarget)`](~/android/deploy-test/building-apps/build-properties.md#androidcodegentarget)

## <a name="resourcedesignercs-generation"></a>Создание `Resource.designer.cs`

Для управления созданием файла `Resource.designer.cs` используются следующие свойства MSBuild:

- [`$(AndroidAapt2CompileExtraArgs)`](~/android/deploy-test/building-apps/build-properties.md#androidaapt2compileextraargs)
- [`$(AndroidAapt2LinkExtraArgs)`](~/android/deploy-test/building-apps/build-properties.md#androidaapt2linkextraargs)
- [`$(AndroidExplicitCrunch)`](~/android/deploy-test/building-apps/build-properties.md#androidexplicitcrunch)
- [`$(AndroidR8IgnoreWarnings)`](~/android/deploy-test/building-apps/build-properties.md#androidr8ignorewarnings)
- [`$(AndroidResgenExtraArgs)`](~/android/deploy-test/building-apps/build-properties.md#androidresgenextraargs)
- [`$(AndroidResgenFile)`](~/android/deploy-test/building-apps/build-properties.md#androidresgenfile)
- [`$(AndroidUseAapt2)`](~/android/deploy-test/building-apps/build-properties.md#androiduseaapt2)
- [`$(MonoAndroidResourcePrefix)`](~/android/deploy-test/building-apps/build-properties.md#monoandroidresourceprefix)

## <a name="signing-properties"></a>Свойства подписи

Свойства подписи контролируют, как подписывается пакет приложений, чтобы его можно было установить на устройство Android. Чтобы ускорить итерацию сборки, задачи Xamarin.Android не подписывают пакеты во время процесса сборки, потому что это выполняется довольно медленно. Вместо этого они подписываются (если необходимо) перед установкой или во время экспорта с помощью IDE или целевого объекта *Install*. Вызов целевого объекта *SignAndroidPackage* приведет к созданию пакета с суффиксом `-Signed.apk` в выходном каталоге.

По умолчанию целевой объект подписи генерирует ключ подписи отладки, если это необходимо. Если вы хотите использовать определенный ключ, например, на сервере сборки, можно использовать следующие свойства MSBuild:

- [`$(AndroidDebugKeyAlgorithm)`](~/android/deploy-test/building-apps/build-properties.md#androiddebugkeyalgorithm)
- [`$(AndroidDebugKeyValidity)`](~/android/deploy-test/building-apps/build-properties.md#androiddebugkeyvalidity)
- [`$(AndroidDebugStoreType)`](~/android/deploy-test/building-apps/build-properties.md#androiddebugstoretype)
- [`$(AndroidKeyStore)`](~/android/deploy-test/building-apps/build-properties.md#androidkeystore)
- [`$(AndroidSigningKeyAlias)`](~/android/deploy-test/building-apps/build-properties.md#androidsigningkeyalias)
- [`$(AndroidSigningKeyPass)`](~/android/deploy-test/building-apps/build-properties.md#androidsigningkeypass)
- [`$(AndroidSigningKeyStore)`](~/android/deploy-test/building-apps/build-properties.md#androidsigningkeystore)
- [`$(AndroidSigningStorePass)`](~/android/deploy-test/building-apps/build-properties.md#androidsigningstorepass)
- [`$(JarsignerTimestampAuthorityCertificateAlias)`](~/android/deploy-test/building-apps/build-properties.md#jarsignertimestampauthoritycertificatealias)
- [`$(JarsignerTimestampAuthorityUrl)`](~/android/deploy-test/building-apps/build-properties.md#jarsignertimestampauthorityurl)

### <a name="keytool-option-mapping"></a>Сопоставление параметра `keytool`

Рассмотрим следующий вызов `keytool`:

```shell
$ keytool -genkey -v -keystore filename.keystore -alias keystore.alias -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password: keystore.filename password
Re-enter new password: keystore.filename password
...
Is CN=... correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: ...
Enter key password for keystore.alias
        (RETURN if same as keystore password): keystore.alias password
[Storing filename.keystore]
```

Чтобы использовать хранилище ключей, созданное выше, используйте группу свойств:

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

## <a name="build-extension-points"></a>Точки расширения сборки

Система сборки Xamarin.Android предоставляет несколько общедоступных точек расширения для пользователей, желающих присоединиться к нашему процессу сборки. Чтобы использовать одну из этих точек расширения, необходимо добавить настраиваемый целевой объект в соответствующее свойство MSBuild в `PropertyGroup`. Пример:

```xml
<PropertyGroup>
   <AfterGenerateAndroidManifest>
      $(AfterGenerateAndroidManifest);
      YourTarget;
   </AfterGenerateAndroidManifest>
</PropertyGroup>
```

К точкам расширения относятся:

- [`$(AfterGenerateAndroidManifest)](~/android/deploy-test/building-apps/build-properties.md#aftergenerateandroidmanifest)
- [`$(BeforeGenerateAndroidManifest)](~/android/deploy-test/building-apps/build-properties.md#beforegenerateandroidmanifest)

Предупреждение о расширении процесса сборки. Если расширения сборки написаны неправильно, они могут повлиять на производительность сборки, особенно если выполняются при каждой сборке. Настоятельно рекомендуется ознакомиться с [документацией](/visualstudio/msbuild/msbuild) по MSBuild перед реализацией таких расширений.

## <a name="target-definitions"></a>Определения целевых объектов

Относящиеся к Xamarin.Android части процесса сборки определены в `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets`, но для создания сборки также требуются обычные языковые файлы целей сборки, такие как *Microsoft.CSharp.targets*.

Следующие свойства сборки следует задать перед импортом языковых файлов целей сборки:

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

Все эти целевые объекты и свойства могут быть включены в C # путем импорта *Xamarin.Android.CSharp.targets*:

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

Этот файл можно легко адаптировать для других языков.
