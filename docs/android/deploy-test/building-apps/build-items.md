---
title: Элементы сборки
description: В этом документе перечислены все группы элементов, поддерживаемые процессом сборки Xamarin.Android.
ms.prod: xamarin
ms.assetid: 5EBEE1A5-3879-45DD-B1DE-5CD4327C2656
ms.technology: xamarin-android
author: jonpryor
ms.author: jopryo
ms.date: 03/01/2021
ms.openlocfilehash: 2ba9c4203b6bd196242183915127dd74fa6ddee7
ms.sourcegitcommit: 3aa9bdcaaedca74ab5175cb2338a1df122300243
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101749295"
---
# <a name="build-items"></a>Элементы сборки

Элементы сборки определяют то, как выполняется сборка приложения Xamarin.Android или проекта библиотеки.

## <a name="androidasset"></a>AndroidAsset

Поддерживает [ресурсы Android](https://developer.android.com/guide/topics/resources/providing-resources#OriginalFiles), то есть файлы, которые включаются в папку `assets` в проекте Android на Java.

## <a name="androidaarlibrary"></a>AndroidAarLibrary

Действие сборки `AndroidAarLibrary` следует использовать для прямой ссылки на файлы `.aar`. Действие сборки будет наиболее часто использоваться компонентами Xamarin. То есть они будут использовать его для включения ссылок на файлы `.aar`, которые необходимы для работы Google Play и других служб.

Файлы с этим действием сборки будут обрабатываться так же, как внедренные ресурсы, расположенные в проектах библиотек. Файлы `.aar` будут извлекаться в промежуточный каталог. Затем все активы, ресурсы и файлы `.jar` будут включены в соответствующие группы элементов.

## <a name="androidaotprofile"></a>AndroidAotProfile

Используется для предоставления профиля AOT, который будет применяться для профильного AOT.

Его также можно использовать из Visual Studio, задав для действия сборки `AndroidAotProfile` файл, содержащий профиль AOT.

## <a name="androidboundlayout"></a>AndroidBoundLayout

Указывает, что для файла макета следует создавать код программной части в случае, когда свойство `AndroidGenerateLayoutBindings` задано как `false`. В остальном он аналогичен `AndroidResource`, описанному выше. Это действие может использоваться **только** с файлами макета:

```xml
<AndroidBoundLayout Include="Resources\layout\Main.axml" />
```

## <a name="androidenvironment"></a>AndroidEnvironment

Файлы с действием сборки `AndroidEnvironment` используются для [инициализации переменных среды и свойств системы во время запуска процесса](~/android/deploy-test/environment.md).
Действие сборки `AndroidEnvironment` может быть применено к нескольким файлам, и они будут оцениваться без какого либо порядка (поэтому не указывайте одну и ту же переменную среды или системное свойство в нескольких файлах).

## <a name="androidfragmenttype"></a>AndroidFragmentType

Указывает полный тип по умолчанию, используемый для всех элементов макета `<fragment>` при создании кода привязок макета. По умолчанию свойство использует стандартный тип Android `Android.App.Fragment`.

## <a name="androidjavalibrary"></a>AndroidJavaLibrary

Файлы с действием сборки `AndroidJavaLibrary` — это архивы Java (файлы `.jar`), которые будут включены в окончательный пакет Android.

## <a name="androidjavasource"></a>AndroidJavaSource

Файлы с действием сборки `AndroidJavaSource` — это исходный код Java, который будет включен в окончательный пакет Android.

## <a name="androidlibrary"></a>AndroidLibrary

**AndroidLibrary** — это новое действие сборки для упрощения процесса включения файлов `.jar` и `.aar` в проекты.

Можно указать любой проект:

```xml
<ItemGroup>
  <AndroidLibrary Include="foo.jar" />
  <AndroidLibrary Include="bar.aar" />
</ItemGroup>
```

Результат выполнения приведенного выше фрагмента кода будет разным для каждого типа проекта Xamarin.Android:

* Проекты приложений и библиотек классов:
  * `foo.jar` сопоставляется с [**AndroidJavaLibrary**](#androidjavalibrary).
  * `bar.aar` сопоставляется с [**AndroidAarLibrary**](#androidaarlibrary).
* Проекты привязки Java:
  * `foo.jar` сопоставляется с [**EmbeddedJar**](#embeddedjar).
  * `foo.jar` сопоставляется с [**EmbeddedReferenceJar**](#embeddedreferencejar), если добавлены метаданные `Bind="false"`.
  * `bar.aar` сопоставляется с [**LibraryProjectZip**](#libraryprojectzip).

Это упрощение означает, что вы можете использовать **AndroidLibrary** везде.

Это действие сборки было добавлено в Xamarin.Android 11.2.

## <a name="androidlintconfig"></a>AndroidLintConfig

Действие сборки AndroidLintConfig следует использовать в сочетании со свойством [`$(AndroidLintEnabled)`](~/android/deploy-test/building-apps/build-properties.md#androidlintenabled)
. Файлы с этим действием сборки объединяются друг с другом и передаются инструментам Android `lint`. Это должны быть XML-файлы, которые содержат информацию о том, какие тесты требуется включить или отключить.

Дополнительные сведения см. в [документации по lint](https://developer.android.com/studio/write/lint).

## <a name="androidmanifestoverlay"></a>AndroidManifestOverlay

Действие сборки `AndroidManifestOverlay` можно использовать для предоставления дополнительных файлов `AndroidManifest.xml` средству [слияния манифестов](https://developer.android.com/studio/build/manifest-merge).
Файлы с этим действием сборки будут переданы в средство слияния манифестов вместе с основным файлом `AndroidManifest.xml` и любыми дополнительными файлами манифеста из ссылок. Затем они будут объединены в окончательный манифест.

Это действие сборки можно использовать для предоставления приложению дополнительных изменений и параметров в зависимости от конфигурации сборки. Например, если требуется определенное разрешение только во время отладки, можно использовать наложение, чтобы внедрить это разрешение при отладке. Например, имеется следующее содержимое файла наложения:

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
  <uses-permission android:name="android.permission.CAMERA" />
</manifest>
```

Чтобы добавить его в отладочную сборку, можно использовать следующую команду:

```
<ItemGroup>
  <AndroidManifestOverlay Include="DebugPermissions.xml" Condition=" '$(Configuration)' == 'Debug' " />
</ItemGroup>
```

Это действие сборки было представлено в Xamarin.Android 11.2.

## <a name="androidnativelibrary"></a>AndroidNativeLibrary

[Собственные библиотеки](~/android/platform/native-libraries.md) можно добавить в сборку, указав для них действие сборки `AndroidNativeLibrary`.

Так как Android поддерживает несколько бинарных интерфейсов приложений (ABI), система сборки должна знать, для какого ABI создана собственная библиотека. Это можно сделать двумя способами:

1. Сканирование пути.
2. С помощью атрибута элемента `Abi`.

При сканировании пути имя родительского каталога собственной библиотеки используется для указания целевого ABI библиотеки. Таким образом при добавлении `lib/armeabi-v7a/libfoo.so` к сборке ABI будет сканироваться как `armeabi-v7a`.

### <a name="item-attribute-name"></a>Имя атрибута элемента

ABI&ndash; — указывает ABI собственной библиотеки.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi-v7a</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```

## <a name="androidresource"></a>AndroidResource

Все файлы с действием *AndroidResource* компилируются в ресурсы Android во время процесса сборки и становятся доступными с помощью `$(AndroidResgenFile)`.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

Более опытным пользователям может потребоваться использовать разные ресурсы в разных конфигурациях, но с тем же эффективным путем. Этого можно достичь за счет наличия нескольких каталогов ресурсов и файлов с одинаковыми относительными путями в этих каталогах и благодаря использованию условий MSBuild для условного включения разных файлов в различные конфигурации. Пример:

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
<ItemGroup  Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug\values\strings.xml"/>
</ItemGroup>
<PropertyGroup>
  <MonoAndroidResourcePrefix>Resources;Resources-Debug</MonoAndroidResourcePrefix>
</PropertyGroup>
```

**LogicalName** &ndash; явно указывает путь к ресурсу. Позволяет &ldquo;сглаживание&rdquo; файлах таким образом, чтобы они были доступны как различные ресурсы.

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources/values/strings.xml"/>
</ItemGroup>
<ItemGroup Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug/values/strings.xml">
    <LogicalName>values/strings.xml</LogicalName>
  </AndroidResource>
</ItemGroup>
```

## <a name="androidresourceanalysisconfig"></a>AndroidResourceAnalysisConfig

Действие сборки `AndroidResourceAnalysisConfig` помечает файл как файл конфигурации уровней серьезности для средства диагностики макета в Xamarin Android Designer. Сейчас он используется только в редакторе макета, но не для сообщений сборки.

Дополнительную информацию см. в [документации по анализу ресурсов Android](../../user-interface/android-designer/diagnostics.md).

Добавлено в Xamarin.Android версии 10.2.

## <a name="content"></a>Content

Обычное действие сборки `Content` не поддерживается (потому что мы не выяснили, как его поддерживать без затратного шага первого запуска).

Начиная с Xamarin.Android 5.1 при попытке использования действия сборки `@(Content)` возникает предупреждение `XA0101`.

## <a name="embeddedjar"></a>EmbeddedJar

В проекте привязки Xamarin.Android действие сборки **EmbeddedJar** привязывает библиотеку Java/Kotlin и внедряет файл `.jar` в библиотеку. Когда проект приложения Xamarin.Android использует библиотеку, он будет иметь доступ к API-интерфейсам Java/Kotlin из C#, а также включит код Java/Kotlin в окончательное приложение Android.

Начиная с Xamarin.Android 11.2 в качестве альтернативы можно использовать действие сборки [**AndroidLibrary**](#androidlibrary), например:

```xml
<Project>
  <ItemGroup>
    <AndroidLibrary Include="Library.jar" />
  </ItemGroup>
</Project>
```

## <a name="embeddednativelibrary"></a>EmbeddedNativeLibrary

В библиотеке классов Xamarin.Android или проекте привязки Java действие сборки **EmbeddedNativeLibrary** объединяет собственную библиотеку, например `lib/armeabi-v7a/libfoo.so`, в библиотеку. Когда приложение Xamarin.Android использует библиотеку, файл `libfoo.so` будет добавлен в окончательное приложение Android.

Начиная с Xamarin.Android 11.2 в качестве альтернативы можно использовать действие сборки [**AndroidNativeLibrary**](#androidnativelibrary).

## <a name="embeddedreferencejar"></a>EmbeddedReferenceJar

В проекте привязки Xamarin.Android действие сборки **EmbeddedReferenceJar** внедряет файл `.jar` в библиотеку, но не создает привязку C#, как это делает [**EmbeddedJar**](#embeddedjar). Когда проект приложения Xamarin.Android использует библиотеку, он будет включать код Java/Kotlin в окончательное приложение Android.

Начиная с Xamarin.Android 11.2 в качестве альтернативы можно использовать действие сборки [**AndroidLibrary**](#androidlibrary), например `<AndroidLibrary Include="..." Bind="false" />`:

```xml
<Project>
  <ItemGroup>
    <!-- A .jar file to bind & embed -->
    <AndroidLibrary Include="Library.jar" />
    <!-- A .jar file to only embed -->
    <AndroidLibrary Include="Dependency.jar" Bind="false" />
  </ItemGroup>
</Project>
```

## <a name="javadocjar"></a>JavaDocJar

В проекте привязки Xamarin.Android действие сборки **JavaDocJar** используется для файлов `.jar`, содержащих *Javadoc HTML*.  Javadoc HTML анализируется для извлечения имен параметров.

Поддерживаются только определенные "диалекты Javadoc HTML", включая следующие.

  * Выходные данные JDK 1.7 `javadoc`.
  * Выходные данные JDK 1.8 `javadoc`.
  * Выходные данные Droiddoc.

Это действие сборки является устаревшим в Xamarin.Android 11.3 и не будет поддерживаться в .NET 6.
Действие сборки `@(JavaSourceJar)` является предпочтительным.

## <a name="javasourcejar"></a>JavaSourceJar

В проекте привязки Xamarin.Android действие сборки **JavaSourceJar** используется для файлов `.jar`, содержащих *исходный код Java*, который содержит [комментарии документации Javadoc](https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html).

До Xamarin.Android 11.3 во время сборки Javadoc преобразовывался в HTML с помощью служебной программы `javadoc`, а затем преобразовывался в XML-документацию.

Начиная с Xamarin.Android 11.3 Javadoc будет преобразовываться в [комментарии к XML-документации C#](/dotnet/csharp/codedoc) в созданном исходном коде привязки.

`$(AndroidJavadocVerbosity)` определяет, насколько "подробным" или "полным" будет импортированный Javadoc.

Начиная с Xamarin.Android 11.3 поддерживаются следующие метаданные MSBuild:

* `%(CopyrightFile)`: путь к файлу, содержащему сведения об авторских правах для содержимого Javadoc, которое будет добавлено ко всей импортированной документации.

* `%(UrlPrefix)`: префикс URL-адреса для поддержки ссылки на интерактивную документацию в импортированной документации.

* `%(UrlStyle)`: стиль URL-адресов, создаваемых при связывании с интерактивной документацией.  В настоящее время поддерживается только один стиль: `developer.android.com/reference@2020-Nov`.


## <a name="libraryprojectzip"></a>LibraryProjectZip

В проекте привязки Xamarin.Android действие сборки **LibraryProjectZip** привязывает библиотеку Java/Kotlin и внедряет файл `.zip` или `.aar` в библиотеку. Когда проект приложения Xamarin.Android использует библиотеку, он будет иметь доступ к API-интерфейсам Java/Kotlin из C#, а также включит код Java/Kotlin в окончательное приложение Android.

> [!NOTE]
> В проект привязки Xamarin.Android можно включить только одно действие **LibraryProjectZip**. Это ограничение будет удалено в .NET 6.

## <a name="linkdescription"></a>LinkDescription

Файлы с действием сборки *LinkDescription* используются для [управления поведением компоновщика](~/cross-platform/deploy-test/linker.md).

## <a name="proguardconfiguration"></a>ProguardConfiguration

Файлы с действием сборки *ProguardConfiguration* содержат параметры, которые используются для управления поведением `proguard`. Дополнительные сведения об этом действии см. в разделе [ProGuard](~/android/deploy-test/release-prep/proguard.md).

Эти файлы игнорируются, если свойство MSBuild [`$(EnableProguard)`](~/android/deploy-test/building-apps/build-properties.md#enableproguard)
не имеет значения `True`.
