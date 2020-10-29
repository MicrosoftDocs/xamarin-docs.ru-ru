---
title: Элементы сборки
description: В этом документе перечислены все группы элементов, поддерживаемые процессом сборки Xamarin.Android.
ms.prod: xamarin
ms.assetid: 5EBEE1A5-3879-45DD-B1DE-5CD4327C2656
ms.technology: xamarin-android
author: jonpryor
ms.author: jopryo
ms.date: 09/23/2020
ms.openlocfilehash: 8a23e973687ac9f775042685122d558788fc7be7
ms.sourcegitcommit: 1550019cd1e858d4d13a4ae6dfb4a5947702f24b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2020
ms.locfileid: "92897446"
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

## <a name="androidlintconfig"></a>AndroidLintConfig

Действие сборки AndroidLintConfig следует использовать в сочетании со свойством [`$(AndroidLintEnabled)`](~/android/deploy-test/building-apps/build-properties.md#androidlintenabled)
. Файлы с этим действием сборки объединяются друг с другом и передаются инструментам Android `lint`. Это должны быть XML-файлы, которые содержат информацию о том, какие тесты требуется включить или отключить.

Дополнительные сведения см. в [документации по lint](https://developer.android.com/studio/write/lint).

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

## <a name="linkdescription"></a>LinkDescription

Файлы с действием сборки *LinkDescription* используются для [управления поведением компоновщика](~/cross-platform/deploy-test/linker.md).

## <a name="proguardconfiguration"></a>ProguardConfiguration

Файлы с действием сборки *ProguardConfiguration* содержат параметры, которые используются для управления поведением `proguard`. Дополнительные сведения об этом действии см. в разделе [ProGuard](~/android/deploy-test/release-prep/proguard.md).

Эти файлы игнорируются, если свойство MSBuild [`$(EnableProguard)`](~/android/deploy-test/building-apps/build-properties.md#enableproguard)
не имеет значения `True`.
