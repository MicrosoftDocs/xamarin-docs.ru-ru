---
title: Свойства сборки
description: В этом документе перечислены все свойства, поддерживаемые процессом сборки Xamarin.Android.
ms.prod: xamarin
ms.assetid: FC0DBC08-EBCB-4D2D-AB3F-76B54E635C22
ms.technology: xamarin-android
author: jonpryor
ms.author: jopryo
ms.date: 03/01/2021
ms.openlocfilehash: 5c26c171c198698580e2b2170a6692d8fb80a171
ms.sourcegitcommit: 3aa9bdcaaedca74ab5175cb2338a1df122300243
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101749321"
---
# <a name="build-properties"></a>Свойства сборки

Свойства MSBuild управляют поведением [целевых объектов](~/android/deploy-test/building-apps/build-targets.md).
Они указываются в файле проекта, например **MyApp.csproj**, в элементе [MSBuild PropertyGroup](/visualstudio/msbuild/propertygroup-element-msbuild).

## <a name="adbtarget"></a>AdbTarget

Свойство `$(AdbTarget)` указывает целевое устройство Android, на котором может быть установлен или удален пакет Android.
Значение этого свойства совпадает со значением [параметра `adb` целевого устройства](https://developer.android.com/tools/help/adb.html#issuingcommands).

## <a name="aftergenerateandroidmanifest"></a>AfterGenerateAndroidManifest

Целевые объекты, перечисленные в этом свойстве, будут запускаться непосредственно после внутреннего целевого объекта `_GenerateJavaStubs`, в котором создается файл `AndroidManifest.xml` в `$(IntermediateOutputPath)`. Если вы хотите внести изменения в созданный файл `AndroidManifest.xml`, это можно сделать с помощью этой точки расширения.

Добавлено в Xamarin.Android версии 9.4.

## <a name="androidaapt2compileextraargs"></a>AndroidAapt2CompileExtraArgs

Указывает дополнительные параметры командной строки для передачи команде **aapt2 compile** при обработке активов и ресурсов Android.

Свойство добавлено в Xamarin.Android версии 9.1.

## <a name="androidaapt2linkextraargs"></a>AndroidAapt2LinkExtraArgs

Указывает дополнительные параметры командной строки для передачи команде **aapt2 link** при обработке активов и ресурсов Android.

Свойство добавлено в Xamarin.Android версии 9.1.

## <a name="androidaddkeepalives"></a>AndroidAddKeepAlives

Логическое свойство, определяющее, будет ли компоновщик вставлять вызовы `GC.KeepAlive()` внутри проектов привязки для предотвращения преждевременного сбора объектов.

Значение по умолчанию для сборок конфигурации выпуска — `True`.

Это свойство было добавлено в Xamarin.Android 11.2.

## <a name="androidaotcustomprofilepath"></a>AndroidAotCustomProfilePath

Файл, который `aprofutil` должен создать для хранения данных профилировщика.

## <a name="androidaotprofiles"></a>AndroidAotProfiles

Строковое свойство, которое позволяет разработчику добавлять профили AOT из командной строки. Это список абсолютных путей, разделенных точками с запятой или запятыми.
Добавлено в Xamarin.Android версии 10.1.

## <a name="androidaotprofilerport"></a>AndroidAotProfilerPort

Порт, к которому `aprofutil` должен подключаться при получении данных профилирования.

## <a name="androidapkdigestalgorithm"></a>AndroidApkDigestAlgorithm

Строковое значение, которое указывает алгоритм хэш-кода для использования с `jarsigner -digestalg`.

Значение по умолчанию — `SHA-256`. В Xamarin.Android 10.0 и более ранних версий по умолчанию использовалось значение `SHA1`.

Добавлено в Xamarin.Android версии 9.4.

## <a name="androidapksigneradditionalarguments"></a>AndroidApkSignerAdditionalArguments

Строковое свойство, которое позволяет разработчику предоставлять дополнительные аргументы для средства `apksigner`.

Свойство добавлено в Xamarin.Android версии 8.2.

## <a name="androidapksigningalgorithm"></a>AndroidApkSigningAlgorithm

Строковое значение, которое указывает алгоритм подписи для использования с `jarsigner -sigalg`.

Значение по умолчанию — `SHA256withRSA`. В Xamarin.Android 10.0 и более ранних версий по умолчанию использовалось значение `md5withRSA`.

Свойство добавлено в Xamarin.Android версии 8.2.

## <a name="androidapplication"></a>AndroidApplication

Логическое значение, указывающее, для чего предназначен проект: для приложения Android (`True`) или для библиотеки Android (`False` или не задано).

Только один проект со значением `<AndroidApplication>True</AndroidApplication>` может присутствовать в пакете Android. (К сожалению, это еще не проверено и может привести к различным ошибкам по отношению к ресурсам Android.)

## <a name="androidapplicationjavaclass"></a>AndroidApplicationJavaClass

Полное имя класса Java для использования вместо `android.app.Application`, когда класс наследуется от [Android.App.Application](xref:Android.App.Application).

Это свойство обычно задается *другими* свойствами, такими как свойство `$(AndroidEnableMultiDex)` MSBuild.

Свойство добавлено в Xamarin.Android версии 6.1.

## <a name="androidbinutilspath"></a>AndroidBinUtilsPath

Путь к каталогу, который содержит средства [binutil][binutils] для Android, такие как собственный компоновщик `ld` и собственный ассемблер `as`. Эти средства являются частью пакета NDK для Android и включены также в установку Xamarin.Android.

Значение по умолчанию — `$(MonoAndroidBinDirectory)\ndk\`.

Добавлено в Xamarin.Android версии 10.0.

[binutils]: https://android.googlesource.com/toolchain/binutils/

## <a name="androidboundexceptiontype"></a>AndroidBoundExceptionType

Строковое значение, которое указывает способ распространения исключений, когда предоставленный Xamarin.Android тип реализует тип или интерфейс .NET в формате типов Java, например `Android.Runtime.InputStreamInvoker` и `System.IO.Stream` или `Android.Runtime.JavaDictionary` и `System.Collections.IDictionary`.

- `Java`. Исходный тип исключения Java распространяется "как есть".

  Это означает, к примеру, что `InputStreamInvoker` неправильно реализует API `System.IO.Stream`, так как `Java.IO.IOException` может вызываться из `Stream.Read()` вместо `System.IO.IOException`.

  Такое поведение распространения исключений действует во всех выпусках Xamarin.Android до версии 11.1.

  Это значение по умолчанию в Xamarin.Android 11.1.

- `System`. Исходный тип исключения Java перехватывается и упаковывается в соответствующий тип исключения .NET.

  Это означает, в частности, что с помощью `InputStreamInvoker` правильно реализуется `System.IO.Stream`, а с помощью `Stream.Read()` *не* создаются экземпляры `Java.IO.IOException`  (вместо этого может создаваться `System.IO.IOException` со значением `Java.IO.IOException` для параметра `Exception.InnerException`).

  В .NET 6.0 это станет значением по умолчанию.

Добавлено в Xamarin.Android версии 10.2.

## <a name="androidbuildapplicationpackage"></a>AndroidBuildApplicationPackage

Логическое значение, указывающее, следует ли создавать и подписывать пакет (APK-файл). Присвоение этому свойству значения `True` эквивалентно использованию [`SignAndroidPackage`](~/android/deploy-test/building-apps/build-targets.md#install)
в качестве целевого объекта сборки.

Поддержка этого свойства была добавлена в Xamarin.Android ​​после версии 7.1.

По умолчанию это свойство имеет значение `False`.

## <a name="androidbundleconfigurationfile"></a>AndroidBundleConfigurationFile

Указывает имя файла, который [ будет использовать в качестве ][bundle-config-format]файла конфигурации`bundletool` при создании пакета приложения Android. Этот файл управляет некоторыми аспектами создания пакетов APK из пакета, например определяет, по каким характеристикам пакет разбивается для создания APK. Обратите внимание, что Xamarin.Android автоматически настраивает некоторые из этих параметров, включая список расширений файлов, которые нужно оставить несжатыми.

Это свойство используется, только если [`$(AndroidPackageFormat)`](#androidpackageformat) имеет значение `aab`.

Добавлено в Xamarin.Android версии 10.3.

[bundle-config-format]: https://developer.android.com/studio/build/building-cmdline#bundleconfig

## <a name="androidclassparser"></a>AndroidClassParser

Строковое свойство, которое определяет способ анализа файлов `.jar`. Ниже перечислены возможные значения.

- **class-parse**: использует `class-parse.exe` для непосредственного синтаксического анализа байт-кода Java без использования виртуальной машины Java. Это значение является экспериментальным.

- **jar2xml**: использует `jar2xml.jar` для отражения Java, чтобы извлекать типы и элементы из файла `.jar`.

Ниже приведены преимущества `class-parse` над `jar2xml`.

- `class-parse` может извлекать имена параметров из байт-кода Java, который содержит *отладочные* символы (байт-код, скомпилированный с помощью `javac -g`).

- `class-parse` не пропускает классы, которые наследуются от членов неразрешимых типов или содержат их.

**Экспериментальное**. Добавлено в Xamarin.Android версии 6.0.

Значение по умолчанию — `jar2xml`.

Значение `jar2xml` устарело и `jar2xml` перестанет поддерживаться в .NET 6.

## <a name="androidcodegentarget"></a>AndroidCodegenTarget

Строковое свойство, которое определяет целевой ABI создания кода.
Ниже перечислены возможные значения.

- **XamarinAndroid**: использует API привязки JNI, представленный в Mono для Android 1.0. Сборки привязки, созданные с помощью Xamarin.Android 5.0 или более поздней версии, можно запускать только в Xamarin.Android 5.0 или более поздней версии (добавление API/ABI), но *источник* совместим с предыдущими версиями продукта.

- **XAJavaInterop1**: использует Java.Interop для вызова JNI. Сборки привязки с `XAJavaInterop1` можно создавать и выполнять только с помощью Xamarin.Android 6.1 или более поздней версии. Xamarin.Android 6.1 и более поздних версий связывает `Mono.Android.dll` с этим значением.

Ниже приведены преимущества `XAJavaInterop1`.

- Сборки меньшего размера.

- Кэширование `jmethodID` для вызова метода `base` при условии, что все прочие типы привязки в иерархии наследования созданы с помощью `XAJavaInterop1` или более поздней версии.

- Кэширование `jmethodID` конструкторов JCW для управляемых подклассов.

Значение по умолчанию — `XAJavaInterop1`.

## <a name="androidcreatepackageperabi"></a>AndroidCreatePackagePerAbi

Логическое свойство, определяющее, следует ли создать *набор* файлов для наборов ABI, указанных в [`$(AndroidSupportedAbis)`](#androidsupportedabis), вместо поддержки всех ABI в одном файле `.apk`.

См. также руководство [Создание пакетов APK для конкретного ABI](~/android/deploy-test/building-apps/abi-specific-apks.md).

## <a name="androiddebugkeyalgorithm"></a>AndroidDebugKeyAlgorithm

Указывает алгоритм по умолчанию для `debug.keystore`. По умолчанию для него используется значение `RSA`.

## <a name="androiddebugkeyvalidity"></a>AndroidDebugKeyValidity

Указывает срок действия по умолчанию для `debug.keystore`. По умолчанию используются значения `10950`, `30 * 365` или `30 years`.

## <a name="androiddebugstoretype"></a>AndroidDebugStoreType

Указывает формат файла хранилища ключей, используемый для `debug.keystore`. По умолчанию для него используется значение `pkcs12`.

Добавлено в Xamarin.Android версии 10.2.


## <a name="androiddeviceuserid"></a>AndroidDeviceUserId

Разрешает развертывание и отладку приложения с использованием гостевой или рабочей учетной записи. Значением является значение `uid`, полученное из следующей команды adb:

```
adb shell pm list users
```

Возвращаются следующие данные:

```
Users:
    UserInfo{0:Owner:c13} running
    UserInfo{10:Guest:404}
```

`uid` — первое целочисленное значение. В примере это `0` и `10`.

Это свойство было добавлено в Xamarin.Android 11.2.

## <a name="androiddextool"></a>AndroidDexTool

Свойство стиля перечисления с допустимыми значениями `dx` или `d8`. Указывает, какой [DEX][dex]-компилятор Android используется во время сборки Xamarin.Android.
Сейчас по умолчанию имеет значение `dx`. Дополнительные сведения см. в документации по [D8 и R8][d8-r8].

[dex]: https://source.android.com/devices/tech/dalvik/dalvik-bytecode
[d8-r8]: https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/D8andR8.md

## <a name="androidenabledesugar"></a>AndroidEnableDesugar

Логическое свойство, которое определяет, включен ли `desugar`. Android пока поддерживает не все функции Java 8, и цепочка инструментов по умолчанию реализует новые языковые функции, выполняя преобразования байт-кода, которые называются `desugar`, на выходе компилятора `javac`. По умолчанию используется `False` при использовании `AndroidDexTool=dx` и `True` при использовании [`$(AndroidDexTool)`](#androiddextool)=`d8`.

## <a name="androidenablegoogleplaystorechecks"></a>AndroidEnableGooglePlayStoreChecks

Логическое свойство, позволяющее разработчикам отключить следующие проверки Магазина Google Play: XA1004, XA1005 и XA1006. Это полезно для разработчиков, которые создают приложения не для Google Play Маркет и не хотят выполнять эти проверки.

Добавлено в Xamarin.Android версии 9.4.

## <a name="androidenablemultidex"></a>AndroidEnableMultiDex

Логическое свойство, которое определяет, будет ли поддерживаться Multi-DEX в окончательном файле `.apk`.

Поддержка этого свойства была добавлена в Xamarin.Android версии 5.1.

По умолчанию это свойство имеет значение `False`.

## <a name="androidenablepreloadassemblies"></a>AndroidEnablePreloadAssemblies

Логическое свойство, которое определяет, все ли управляемые сборки, объединенные в пакет приложения, загружаются во время запуска процесса.

Если задано значение `True`, все сборки, объединенные в пакет приложения, будут загружены во время запуска процесса до вызова кода приложения.
Это согласуется с возможностями Xamarin.Android, присутствовавшими до выхода Xamarin.Android 9.2.

Если задано значение `False`, сборки загружаются только по мере необходимости.
Это позволяет приложениям запускаться быстрее, а также лучше согласуется с семантикой .NET для настольных систем.  Чтобы убедиться в ускорении работы, добавьте `timing` в системное свойство `debug.mono.log` и в `adb logcat` найдите сообщение `Finished loading assemblies: preloaded`.

Приложениям или библиотекам, использующим внедрение зависимостей, может *требоваться*, чтобы это свойство имело значение `True`, если они в свою очередь требуют, чтобы метод `AppDomain.CurrentDomain.GetAssemblies()` возвращал все сборки в пакете приложения, даже в том случае, если сборка не нужна.

По умолчанию свойству задано значение `True`.

Свойство добавлено в Xamarin.Android версии 9.2.

## <a name="androidenableprofiledaot"></a>AndroidEnableProfiledAot

Логическое свойство, которое определяет, используются ли профили AOT во время компиляции Ahead Of Time.

Профили перечислены в группе элементов [`@(AndroidAotProfile)`](~/android/deploy-test/building-apps/build-items.md#androidaotprofile)
. Эта ItemGroup содержит профили по умолчанию. Ее можно переопределить, удалив существующие и добавив собственные профили AOT.

Поддержка этого свойства была добавлена в Xamarin.Android 9.4.

По умолчанию это свойство имеет значение `False`.

## <a name="androidenablesgenconcurrent"></a>AndroidEnableSGenConcurrent

Логическое свойство, которое определяет, будет ли использоваться [параллельный сборщик мусора](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen) Mono.

Поддержка этого свойства была добавлена в Xamarin.Android версии 7.2.

По умолчанию это свойство имеет значение `False`.

## <a name="androiderroroncustomjavaobject"></a>AndroidErrorOnCustomJavaObject

Логическое свойство, которое определяет, можно ли реализовать типы `Android.Runtime.IJavaObject`
*без* наследования от `Java.Lang.Object` или `Java.Lang.Throwable`:

```csharp
class BadType : IJavaObject {
    public IntPtr Handle {
        get {return IntPtr.Zero;}
    }

    public void Dispose()
    {
    }
}
```

Если значение равно true, такие типы будут вызывать ошибку XA4212. Если значение false, будет сформировано предупреждение XA4212.

Поддержка этого свойства добавлена в Xamarin.Android версии 8.1.

По умолчанию это свойство имеет значение `True`.

## <a name="androidexplicitcrunch"></a>AndroidExplicitCrunch

Больше не поддерживается в Xamarin.Android 11.0.

## <a name="androidextraaotoptions"></a>AndroidExtraAotOptions

Строковое свойство, позволяющее передавать дополнительные параметры в компилятор Mono во время выполнения задачи `Aot` для проектов, у которых [`$(AndroidEnableProfiledAot)`](#androidenableprofiledaot) или [`$(AotAssemblies)`](#aotassemblies) имеет значение `true`.
Строковое значение этого свойства добавляется в файл ответов при вызове кросс-компилятора Mono.

Как правило, это свойство следует оставлять пустым. Но в некоторых особых случаях оно может обеспечить полезные гибкие возможности.

Обратите внимание, что это свойство отличается от связанного свойства `$(AndroidAotAdditionalArguments)`. Это свойство помещает аргументы с разделителями-запятыми в параметр `--aot` компилятора Mono. В свою очередь, `$(AndroidExtraAotOptions)` передает в компилятор разделенные пробелами полные автономные параметры, такие как `--verbose` или `--debug`.

Добавлено в Xamarin.Android версии 10.2.

<a name="AndroidFastDeploymentType"></a>

## <a name="androidfastdeploymenttype"></a>AndroidFastDeploymentType

Список разделенных двоеточиями (`:`) значений для управления типами, которые можно развернуть в [каталоге быстрого развертывания](~/android/deploy-test/building-apps/build-process.md#Fast_Deployment) на целевом устройстве, если свойство MSBuild [`$(EmbedAssembliesIntoApk)`](#embedassembliesintoapk) имеет значение `False`. Если ресурс быстро развернут, он *не* встраивается в создаваемый файл `.apk`, что может ускорить развертывание. (Чем быстрее выполняется развертывание, тем реже файл `.apk` необходимо перестраивать, что ускоряет процесс установки.) Допустимы следующие значения:

- `Assemblies`. развертывание сборок приложения.
- `Dexes`: развертывание файлов `.dex`, собственных библиотек и карт типов.
  **: это значение можно использовать *только* на устройствах под управлением Android 4.4 или более поздней версии (API-19).**

Значение по умолчанию — `Assemblies`.

Поддержка быстро развертываемых ресурсов и активов через эту систему была удалена в фиксации [f0d565fe](https://github.com/xamarin/xamarin-android/commit/f0d565fe4833f16df31378c77bbb492ffd2904b9). Это было вызвано тем, что для работы требуются устаревшие API.

**Экспериментальное**. Это свойство было добавлено в Xamarin.Android 6.1.

## <a name="androidgeneratejnimarshalmethods"></a>AndroidGenerateJniMarshalMethods

Логическое свойство, которое включает создание методов маршалинга JNI в процессе сборки. Это существенно сокращает использование `System.Reflection` во вспомогательном коде привязки.

По умолчанию установлено значение False. Если разработчики хотят использовать новые методы маршалинга JNI, методы можно настроить

```xml
<AndroidGenerateJniMarshalMethods>True</AndroidGenerateJniMarshalMethods>
```

в собственном файле `.csproj`. Вы также можете указать свойства в командной строке с помощью параметра

```shell
/p:AndroidGenerateJniMarshalMethods=True
```

**Экспериментальное**. Свойство добавлено в Xamarin.Android версии 9.2.
Значение по умолчанию — False.

## <a name="androidgeneratejnimarshalmethodsadditionalarguments"></a>AndroidGenerateJniMarshalMethodsAdditionalArguments

Строковое свойство, которое может использоваться для добавления дополнительных параметров в вызов `jnimarshalmethod-gen.exe`.  Это полезно для отладки, поэтому можно использовать такие параметры, как `-v`, `-d` и `--keeptemp`.

Значение по умолчанию — пустая строка. Его можно задать в файле `.csproj` или в командной строке. Пример:

```xml
<AndroidGenerateJniMarshalMethodsAdditionalArguments>-v -d --keeptemp</AndroidGenerateJniMarshalMethodsAdditionalArguments>
```

или

```shell
/p:AndroidGenerateJniMarshalMethodsAdditionalArguments="-v -d --keeptemp"
```

Свойство добавлено в Xamarin.Android версии 9.2.

## <a name="androidgeneratelayoutbindings"></a>AndroidGenerateLayoutBindings

Включает создание [кода программной части макета](https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/LayoutCodeBehind.md), если присвоить значение `true`, или полностью отключает его, если задать значение `false`. Значение по умолчанию — `false`.

## <a name="androidhttpclienthandlertype"></a>AndroidHttpClientHandlerType

Управляет стандартной реализацией `System.Net.Http.HttpMessageHandler`, которую будет использовать конструктор по умолчанию `System.Net.Http.HttpClient`. Значение — имя типа с указанием сборки подкласса `HttpMessageHandler`, подходящее для использования с [`System.Type.GetType(string)`](/dotnet/api/system.type.gettype#System_Type_GetType_System_String_).
Наиболее распространенные значения для этого свойства:

- `Xamarin.Android.Net.AndroidClientHandler`. используйте интерфейсы API Android Java для выполнения сетевых запросов. Это обеспечивает доступ к URL-адресам TLS 1.2, если базовая версия Android поддерживает TLS 1.2. Только Android 5.0 и более поздних версий обеспечивает надежную поддержку TLS 1.2 через Java.

  Это соответствует параметру **Android** на страницах свойств Visual Studio и параметру **AndroidClientHandler** на страницах свойств Visual Studio для Mac.

  Мастер создания проектов выбирает этот вариант для новых проектов, если указана **минимальная версия Android** **Android 5.0 (Lollipop)** или выше в Visual Studio или если для **целевых платформ** установлено значение **Последняя и самая поздняя** в Visual Studio для Mac.

- Удаляет пустую строку. Это эквивалентно `System.Net.Http.HttpClientHandler, System.Net.Http`

  Это соответствует параметру **по умолчанию** на страницах свойств Visual Studio.

  Мастер создания проектов выбирает этот параметр для новых проектов, если указана **минимальная версия Android** **Android 4.4.87** или более ранняя в Visual Studio или если для **целевых платформа** установлено **Современная разработка** или **Максимальная совместимость** в Visual Studio для Mac.

- `System.Net.Http.HttpClientHandler, System.Net.Http`. используйте управляемый `HttpMessageHandler`.

  Это соответствует параметру **Управляемый** на страницах свойств Visual Studio.

> [!NOTE]
> Если требуется поддержка TLS 1.2 в версиях Android ниже 5.0 *или* если поддержка TLS 1.2 необходима для `System.Net.WebClient` и связанных API, следует использовать [`$(AndroidTlsProvider)`](#androidtlsprovider).

> [!NOTE]
> Чтобы работала поддержка этого свойства, необходимо настроить переменную среды [`XA_HTTP_CLIENT_HANDLER_TYPE`](~/android/deploy-test/environment.md).
> Значение `$XA_HTTP_CLIENT_HANDLER_TYPE`, которое находится в файле с действием сборки [`@(AndroidEnvironment)`](~/android/deploy-test/building-apps/build-items.md#androidenvironment),
> будет иметь приоритет.

Свойство добавлено в Xamarin.Android версии 6.1.

## <a name="androidincludewrapsh"></a>AndroidIncludeWrapSh

Логическое значение, указывающее, следует ли упаковывать скрипт-оболочку Android ([`wrap.sh`](https://developer.android.com/ndk/guides/wrap-script)) в APK. Значение свойства по умолчанию — `false`, поскольку скрипт-оболочка может значительно повлиять на способ запуска приложения и их работу; скрипт следует включать только при необходимости, например для отладки или изменения поведения при запуске или во время выполнения приложения.

Скрипт добавляется в проект с помощью действия сборки [`@(AndroidNativeLibrary)`](~/android/deploy-test/building-apps/build-items.md#androidnativelibrary),
поскольку оно размещается в том же каталоге, что и собственные библиотеки, зависящие от архитектуры. Имя должно быть `wrap.sh`.

Самый простой способ указать путь к скрипту `wrap.sh` — это разместить его в каталоге, имя которого совпадает с конечной архитектурой. Это сработает только в том случае, если у вас только один вариант `wrap.sh` для архитектуры:

```xml
<AndroidNativeLibrary Include="path/to/arm64-v8a/wrap.sh" />
```

Однако если в проекте требуется более одного решения `wrap.sh` для каждой архитектуры, этот подход не будет работать.
Вместо этого в таких случаях для указания имени можно использовать метаданные `Link` `AndroidNativeLibrary`:

```xml
<AndroidNativeLibrary Include="/path/to/my/arm64-wrap.sh">
  <Link>lib\arm64-v8a\wrap.sh</Link>
</AndroidNativeLibrary>
```

Если используются метаданные `Link`, путь, указанный в его значении, должен представлять собой допустимый путь к собственной библиотеке для конкретной архитектуры относительно корневого каталога APK. Формат пути — `lib\ARCH\wrap.sh`, где `ARCH` может быть одним из следующего:

+ `arm64-v8a`
+ `armeabi-v7a`
+ `x86_64`
+ `x86`


## <a name="androidkeystore"></a>AndroidKeyStore

Логическое значение, указывающее, следует ли использовать пользовательские данные подписи. Значение по умолчанию — `False`. Это означает, что для подписания пакетов будет использоваться ключ подписи отладки по умолчанию.

## <a name="androidlaunchactivity"></a>AndroidLaunchActivity

Действие Android для запуска.

## <a name="androidlinkmode"></a>AndroidLinkMode

Указывает, какой тип [компоновки](~/android/deploy-test/linker.md) должен быть применен для сборок, содержащихся в пакете Android. Используется только в проектах приложений Android. Значение по умолчанию: *SdkOnly*. Допустимые значения:

- **None**: компоновка не будет выполнена.

- **SdkOnly**: компоновка будет выполнена только для библиотеки базовых классов, но не сборок пользователя.

- **Full**: компоновка будет выполнена для библиотеки базовых классов и сборок пользователя.

  > [!NOTE]
  > Использование для свойства `AndroidLinkMode` значения *Full* часто приводит к ненадлежащей работе приложений, особенно при использовании отражения. Используйте это значение, только если это *действительно* необходимо.

```xml
<AndroidLinkMode>SdkOnly</AndroidLinkMode>
```

## <a name="androidlinkskip"></a>AndroidLinkSkip

Указывает список разделенных точкой с запятой (`;`) имен сборок, которые не должны быть связаны, без расширений имен файлов. Используется только в проектах приложений Android.

```xml
<AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
```

## <a name="androidlinktool"></a>AndroidLinkTool

Свойство стиля перечисления с допустимыми значениями `proguard` или `r8`. Указывает, какое средство для сокращения кода используется для кода Java. Значение по умолчанию является пустой строкой или `proguard`, если `$(AndroidEnableProguard)` — `True`. Дополнительные сведения см. в документации по [D8 и R8][d8-r8].

[d8-r8]: https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/D8andR8.md

## <a name="androidlintenabled"></a>AndroidLintEnabled

Логическое свойство, которое позволяет разработчику запускать инструмент Android `lint` в процессе упаковки.

При `$(AndroidLintEnabled)`=True используются следующие свойства:

- [`$(AndroidLintEnabledIssues)`](#androidlintenabledissues):
- [`$(AndroidLintDisabledIssues)`](#androidlintdisabledissues):
- [`$(AndroidLintCheckIssues)`](#androidlintcheckissues):

Можно также использовать следующие действия сборки:

- [`@(AndroidLintConfig)`](~/android/deploy-test/building-apps/build-items.md#androidlintconfig):

См. [справку по Lint](https://developer.android.com/studio/write/lint), чтобы узнать больше об инструментах Android `lint`.

## <a name="androidlintenabledissues"></a>AndroidLintEnabledIssues

Это свойство используется только в том случае, если [`$(AndroidLintEnabled)`](#androidlintenabled)=True.

Разделенный запятыми список проблем анализ кода для включения.

## <a name="androidlintdisabledissues"></a>AndroidLintDisabledIssues

Это свойство используется только в том случае, если [`$(AndroidLintEnabled)`](#androidlintenabled)=True.

Разделенный запятыми список проблем анализ кода для отключения.

## <a name="androidlintcheckissues"></a>AndroidLintCheckIssues

Это свойство используется только в том случае, если [`$(AndroidLintEnabled)`](#androidlintenabled)=True.

Разделенный запятыми список проблем анализ кода для проверки.

Примечание. Проверяться будут только эти проблемы.

## <a name="androidmanagedsymbols"></a>AndroidManagedSymbols

Логическое свойство, которое определяет, создаются ли точки последовательности, чтобы можно было извлечь имя файла и номер строки из трассировки стека `Release`.

Свойство добавлено в Xamarin.Android версии 6.1.

## <a name="androidmanifest"></a>AndroidManifest

Определяет имя файла, которое будет использоваться в качестве шаблона для манифеста [`AndroidManifest.xml`](~/android/platform/android-manifest.md) приложения.
Во время сборки необходимые значения будут объединены для создания фактического файла `AndroidManifest.xml`.
`$(AndroidManifest)` должен содержать имя пакета в атрибуте `/manifest/@package`.

## <a name="androidmanifestmerger"></a>AndroidManifestMerger

Указывает реализацию для слияния файлов *AndroidManifest.xml*. Это свойство стиля перечисления, где `legacy` выбирает исходную реализацию C#, а `manifestmerger.jar` выбирает реализацию Java в Google.

По умолчанию сейчас используется значение `legacy`. Оно изменится на `manifestmerger.jar` в будущем выпуске, чтобы согласовать поведение с Android Studio.

Средство слияния Google обеспечивает поддержку `xmlns:tools="http://schemas.android.com/tools"`, как описано в [документации по Android][manifest-merger].

Добавлено в Xamarin.Android версии 10.2.

[manifest-merger]: https://developer.android.com/studio/build/manifest-merge

## <a name="androidmanifestplaceholders"></a>AndroidManifestPlaceholders

Разделенный точками с запятой список заменяемых пар "ключ-значение" для *AndroidManifest.xml*, где каждая пара имеет формат `key=value`.

Например, значение свойства `assemblyName=$(AssemblyName)` определяет заполнитель `${assemblyName}`, который затем может появиться в *AndroidManifest.xml*:

```xml
<application android:label="${assemblyName}"
```

Это позволяет вставлять переменные из процесса сборки в файл *AndroidManifest.xml*.

## <a name="androidmultidexclasslistextraargs"></a>AndroidMultiDexClassListExtraArgs

Строковое свойство, которое позволяет разработчикам передавать дополнительные аргументы в `com.android.multidex.MainDexListBuilder` при создании файла `multidex.keep`.

Один из частных случаев — появление следующей ошибки во время компиляции `dx`.

```text
com.android.dex.DexException: Too many classes in --main-dex-list, main dex capacity exceeded
```

Если возникает эта ошибка, в файл `.csproj` можно добавить следующее:

```xml
<DxExtraArguments>--force-jumbo </DxExtraArguments>
<AndroidMultiDexClassListExtraArgs>--disable-annotation-resolution-workaround</AndroidMultiDexClassListExtraArgs>
```

Это позволит успешно выполнить шаг `dx`.

Свойство добавлено в Xamarin.Android версии 8.3.

## <a name="androidpackageformat"></a>AndroidPackageFormat

Свойство стиля перечисления с допустимыми значениями `apk` или `aab`. Это означает, что вы хотите упаковать приложение Android как [файл APK][apk] или [пакет приложений Android][bundle]. Пакеты приложений — это новый формат для сборок `Release`, предназначенных для отправки на Google Play. Текущее значение по умолчанию: `apk`.

Если параметр `$(AndroidPackageFormat)` имеет значение `aab`, то устанавливаются другие свойства MSBuild, которые необходимы для пакетов приложений Android.

- [`$(AndroidUseAapt2)`](~/android/deploy-test/building-apps/build-properties.md#androiduseaapt2) имеет значение `True`.
- [`$(AndroidUseApkSigner)`](#androiduseapksigner) имеет значение `False`.
- [`$(AndroidCreatePackagePerAbi)`](#androidcreatepackageperabi) имеет значение `False`.

[apk]: https://en.wikipedia.org/wiki/Android_application_package
[bundle]: https://developer.android.com/platform/technology/app-bundle

## <a name="androidpackagenamingpolicy"></a>AndroidPackageNamingPolicy

Свойство перечисления для указания имен пакетов Java созданного исходного кода Java.

Xamarin.Android 10.2 и более поздних версий поддерживает только значение `LowercaseCrc64`.

Кроме того, в Xamarin.Android 10.1 было доступно переходное значение `LowercaseMD5`, которое позволяло возвращаться к исходному стилю имен пакетов Java, который использовался в Xamarin.Android 10.0 и более ранних версий. Этот вариант был удален в Xamarin.Android 10.2 для улучшения совместимости со средами сборки, в которых реализованы требования соответствия FIPS.

Добавлено в Xamarin.Android версии 10.1.

## <a name="androidproguardmappingfile"></a>AndroidProguardMappingFile

Задает правило ProGuard `-printmapping` для `r8`. Это означает, что файл `mapping.txt` будет создан в папке `$(OutputPath)`. Этот файл можно затем использовать при отправке пакетов в Магазин Google PlayPlay.

Значение по умолчанию — `$(OutputPath)mapping.txt`.

Это свойство было добавлено в Xamarin.Android 11.2.

## <a name="androidr8ignorewarnings"></a>AndroidR8IgnoreWarnings

Задает правило ProGuard `-ignorewarnings` для `r8`. Это позволяет `r8` продолжать компиляцию DEX, даже если обнаружены определенные предупреждения. Значение по умолчанию — `True`, но для обеспечения более строгого поведения можно задать значение `False`. Дополнительные сведения см. в [руководстве по ProGuard](https://www.guardsquare.com/products/proguard/manual/usage).

Добавлено в Xamarin.Android версии 10.3.

## <a name="androidr8jarpath"></a>AndroidR8JarPath

Путь к `r8.jar` для использования с DEX-компилятором и средством сжатия кода r8. По умолчанию используется путь установки Xamarin.Android. Дополнительные сведения см. в документации по [D8 и R8][d8-r8].

## <a name="androidresgenextraargs"></a>AndroidResgenExtraArgs

Указывает дополнительные параметры командной строки для передачи команде **aapt** при обработке активов и ресурсов Android.

## <a name="androidresgenfile"></a>AndroidResgenFile

Задает имя создаваемого файла ресурсов. По умолчанию шаблон задает `Resource.designer.cs`.

## <a name="androidsdkbuildtoolsversion"></a>AndroidSdkBuildToolsVersion

Пакет средств сборки SDK для Android, который, помимо прочих, включает средства **aapt** и **zipalign**. Одновременно могут быть установлены несколько различных версий пакета средств сборки. Пакет средств сборки, выбранный для упаковки, создается путем проверки и использования "предпочтительной" версии, если она присутствует. Если такая версия *отсутствует*, то используется установленный пакет средств сборки последней версии.

Свойство MSBuild `$(AndroidSdkBuildToolsVersion)` содержит предпочтительную версию средств сборки. Система сборки Xamarin.Android предоставляет значение по умолчанию в `Xamarin.Android.Common.targets`, которое можно переопределить в файле проекта, чтобы выбрать альтернативную версию средств сборки, если (например) aapt последней версии завершается сбоем, а предыдущая версия aapt работает.

## <a name="androidsigningkeyalias"></a>AndroidSigningKeyAlias

Указывает псевдоним для ключа в хранилище ключей. Это значение **keytool -alias**, используемое при создании хранилища ключей.

## <a name="androidsigningkeypass"></a>AndroidSigningKeyPass

Указывает пароль для ключа в файле хранилища ключей. Это значение вводится, когда `keytool` запрашивает **ввести пароль ключа для $(AndroidSigningKeyAlias)**.

В Xamarin.Android 10.0 и более ранних версий это свойство поддерживает только пароли в виде обычного текста.

В Xamarin.Android 10.1 и более поздних версий это свойство также поддерживает префиксы `env:` и `file:`, которые позволяют указать переменную среды или файл со значением пароля. Эти варианты позволяют избежать отображения пароля в журналах сборки.

Например, так можно применить переменную среды с именем *AndroidSigningPassword*:

```xml
<PropertyGroup>
    <AndroidSigningKeyPass>env:AndroidSigningPassword</AndroidSigningKeyPass>
</PropertyGroup>
```

Так можно применить файл, расположенный в `C:\Users\user1\AndroidSigningPassword.txt`:

```xml
<PropertyGroup>
    <AndroidSigningKeyPass>file:C:\Users\user1\AndroidSigningPassword.txt</AndroidSigningKeyPass>
</PropertyGroup>
```

> [!NOTE]
> Префикс `env:` не поддерживается, если для [`$(AndroidPackageFormat)`](#androidpackageformat) задано значение `aab`.

## <a name="androidsigningkeystore"></a>AndroidSigningKeyStore

Указывает имя файла хранилища ключей, созданного с помощью `keytool`. Это соответствует значению, указанному для параметра **keytool -keystore**.

## <a name="androidsigningstorepass"></a>AndroidSigningStorePass

Задает пароль к [`$(AndroidSigningKeyStore)`](#androidsigningkeystore).
Это значение, предоставляемое для `keytool` при создании файла хранилища ключей и запросе **ввести пароль хранилища ключей**.

В Xamarin.Android 10.0 и более ранних версий это свойство поддерживает только пароли в виде обычного текста.

В Xamarin.Android 10.1 и более поздних версий это свойство также поддерживает префиксы `env:` и `file:`, которые позволяют указать переменную среды или файл со значением пароля. Эти варианты позволяют избежать отображения пароля в журналах сборки.

Например, так можно применить переменную среды с именем *AndroidSigningPassword*:

```xml
<PropertyGroup>
    <AndroidSigningStorePass>env:AndroidSigningPassword</AndroidSigningStorePass>
</PropertyGroup>
```

Так можно применить файл, расположенный в `C:\Users\user1\AndroidSigningPassword.txt`:

```xml
<PropertyGroup>
    <AndroidSigningStorePass>file:C:\Users\user1\AndroidSigningPassword.txt</AndroidSigningStorePass>
</PropertyGroup>
```

> [!NOTE]
> Префикс `env:` не поддерживается, если для [`$(AndroidPackageFormat)`](#androidpackageformat) задано значение `aab`.

## <a name="androidsupportedabis"></a>AndroidSupportedAbis

Строковое свойство, которое содержит разделенный точками с запятой (`;`) список ABI, которые должны быть включены в файл `.apk`.

Допустимые значения:

- `armeabi-v7a`
- `x86`
- `arm64-v8a`: требуется Xamarin.Android 5.1 и более поздней версии.
- `x86_64`: требуется Xamarin.Android 5.1 и более поздней версии.

## <a name="androidtlsprovider"></a>AndroidTlsProvider

Строковое значение, которое указывает, какой поставщик TLS следует использовать в приложении. Доступны следующие значения:

- Удаляет пустую строку. В Xamarin.Android 7.3 или более поздней версии это эквивалентно значению `btls`.

  В Xamarin.Android 7.1 это эквивалентно значению `legacy`.

  Это соответствует параметру **по умолчанию** на страницах свойств Visual Studio.

- `btls`: используется [Boring SSL](https://boringssl.googlesource.com/boringssl) для взаимодействия через TLS с [HttpWebRequest](xref:System.Net.HttpWebRequest).

  Это позволяет использовать TLS 1.2 во всех версиях Android.

  Это соответствует параметру **Собственный протокол TLS 1.2+** на страницах свойств Visual Studio.

- `legacy`. В Xamarin.Android 10.1 и более ранних версий используется историческая управляемая реализация протокола SSL для взаимодействия по сети. TLS 1.2 *не* поддерживается.

  Это соответствует параметру **Управляемый протокол TLS 1.0** на страницах свойств Visual Studio.

  В Xamarin.Android 10.2 и более поздних версий это значение игнорируется. Вместо него используется параметр `btls`.

- `default`. Это значение вряд ли будет использоваться в проектах Xamarin.Android. Вместо этого рекомендуется использовать пустую строку, соответствующую параметру **по умолчанию** на страницах свойств Visual Studio.

  Значение `default` не предлагается на страницах свойств Visual Studio.

  В настоящее время оно эквивалентно `legacy`.

Свойство добавлено в Xamarin.Android версии 7.1.

## <a name="androiduseaapt2"></a>AndroidUseAapt2

Логическое свойство, которое позволяет разработчику управлять использованием средства `aapt2` для упаковки.
По умолчанию имеет значение False и Xamarin.Android использует `aapt`.
Если разработчик хочет использовать новые функциональные возможности `aapt2`, следует добавить следующее:

```xml
<AndroidUseAapt2>True</AndroidUseAapt2>
```

в собственном файле `.csproj`. Вы также можете указать свойство в командной строке:

```shell
/p:AndroidUseAapt2=True
```

Это свойство было добавлено в Xamarin.Android 8.3. Установка для параметра `AndroidUseAapt2` значения `false` не рекомендуется в Xamarin.Android 11.2.

## <a name="androiduseapksigner"></a>AndroidUseApkSigner

Логическое свойство, которое позволяет разработчику использовать средство `apksigner` вместо `jarsigner`.

Свойство добавлено в Xamarin.Android версии 8.2.

## <a name="androidusedefaultaotprofile"></a>AndroidUseDefaultAotProfile

Логическое свойство, которое позволяет разработчику подавлять использование профилей AOT по умолчанию.

Чтобы подавить профиль AOT по умолчанию, присвойте этому свойству значение `false`.

Добавлено в Xamarin.Android версии 10.1.

## <a name="androiduselegacyversioncode"></a>AndroidUseLegacyVersionCode

Логическое свойство, которое позволяет разработчику восстановить поведение при расчете versionCode, существовавшее до версии Xamarin.Android 8.2. Это свойство могут использовать ТОЛЬКО разработчики, приложения которых находятся в Google Play. Настоятельно рекомендуется использовать новое свойство [`$(AndroidVersionCodePattern)`](#androidversioncodepattern).

Свойство добавлено в Xamarin.Android версии 8.2.

## <a name="androidusemanageddesigntimeresourcegenerator"></a>AndroidUseManagedDesignTimeResourceGenerator

Логическое свойство, которое переключает сборки времени разработки, чтобы использовать управляемое средство анализа ресурсов вместо `aapt`.

Свойство добавлено в Xamarin.Android версии 8.1.

## <a name="androidusesharedruntime"></a>AndroidUseSharedRuntime

Логическое свойство, определяющее, требуются ли *пакеты общей среды выполнения* для запуска приложения на целевом устройстве. Их использование позволяет уменьшить пакет приложения, что ускорит процесс создания и развертывания пакета, а также цикл разработки, развертывания и отладки.

До Xamarin.Android 11.2 это свойство должно иметь значение `True` для отладочных сборок и `False` для проектов выпуска.

Это свойство было *удалено* в Xamarin.Android 11.2.

## <a name="androidversioncodepattern"></a>AndroidVersionCodePattern

Строковое свойство, которое позволяет разработчикам настраивать `versionCode` в манифесте.
Дополнительные сведения об определении `versionCode` см. в разделе [Создание версии кода для APK](~/android/deploy-test/building-apps/abi-specific-apks.md).

Например, если `abi` имеет значение `armeabi`, а `versionCode` в манифесте — `123`, тогда `{abi}{versionCode}` выдает код версии `1123` при `$(AndroidCreatePackagePerAbi)` со значением True. В противном случае будет создано значение 123.
Если `abi` имеет значение `x86_64`, а `versionCode` в манифесте — `44`, будет получено значение `544` при `$(AndroidCreatePackagePerAbi)` со значением True. В противном случае значением будет `44`.

Если включить формат строки левого дополнения `{abi}{versionCode:0000}`, значением будет `50044`, так как слева к `versionCode` будет добавлен `0`. Кроме того, можно использовать десятичное заполнение, например `{abi}{versionCode:D4}`,
которое выполняет ту же функцию, что и в предыдущем примере.

Поддерживаются только строки формата дополнения "0" и "Dx", потому что значение ДОЛЖНО быть целым числом.

Предварительно определенные ключевые элементы

- **ABI**  &ndash; вставляет целевой ABI для приложения
  - 2 &ndash; `armeabi-v7a`;
  - 3 &ndash; `x86`
  - 4 &ndash; `arm64-v8a`
  - 5 &ndash; `x86_64`

- **minSDK** &ndash; вставляет минимальное поддерживаемое значение SDK из `AndroidManifest.xml` или `11`, если оно не определено.

- **versionCode** &ndash; использует код версии непосредственно из `Properties\AndroidManifest.xml`.

Можно определить настраиваемые элементы с помощью свойства `$(AndroidVersionCodeProperties)` (определение приводится ниже).

По умолчанию будет указано значение `{abi}{versionCode:D6}`. Если разработчик хочет сохранить предыдущее поведение, можно переопределить значение по умолчанию, задав для свойства `$(AndroidUseLegacyVersionCode)` значение `true`

Добавлено в Xamarin.Android версии 7.2.

## <a name="androidversioncodeproperties"></a>AndroidVersionCodeProperties

Строковое свойство, которое позволяет разработчику определить настраиваемые элементы для использования с [`$(AndroidVersionCodePattern)`](#androidversioncodepattern).
Они находятся в форме пары `key=value`. Все элементы в `value` должны быть целыми числами. Например: `screen=23;target=$(_AndroidApiLevel)`. Как видно, вы можете использовать существующие или пользовательские свойства MSBuild в строке.

Добавлено в Xamarin.Android версии 7.2.

## <a name="aotassemblies"></a>AotAssemblies

Логическое свойство, которое определяет, будут ли сборки скомпилированы в машинный код в режиме Ahead Of Time (AOT) и включены в `.apk`.

Поддержка этого свойства была добавлена в Xamarin.Android версии 5.1.

По умолчанию это свойство имеет значение `False`.

## <a name="aprofutilextraoptions"></a>AProfUtilExtraOptions

Дополнительные параметры, которые следует передать в `aprofutil`.

## <a name="beforegenerateandroidmanifest"></a>BeforeGenerateAndroidManifest

Целевые объекты MSBuild, перечисленные в этом свойстве, будут запускаться непосредственно перед `_GenerateJavaStubs`.

Добавлено в Xamarin.Android версии 9.4.

## <a name="configuration"></a>Конфигурация

Указывает конфигурацию сборки, например "отладка" или "выпуск". Свойство Configuration используется для определения значений по умолчанию для других свойств, которые определяют поведение целевого объекта. В вашей среде IDE можно создать дополнительные конфигурации.

*По умолчанию* конфигурация `Debug` приведет к созданию меньшего пакета Android целевыми объектами [`Install`](~/android/deploy-test/building-apps/build-targets.md#install)
и [`SignAndroidPackage`](~/android/deploy-test/building-apps/build-targets.md#signandroidpackage),
который требует наличия других файлов и пакетов для работы.

Выбор конфигурации по умолчанию `Release` приведет к тому, что целевые объекты [`Install`](~/android/deploy-test/building-apps/build-targets.md#install)
и [`SignAndroidPackage`](~/android/deploy-test/building-apps/build-targets.md#signandroidpackage)
создадут пакет Android, который является *автономным* и может использоваться без установки каких-либо других пакетов или файлов.

## <a name="debugsymbols"></a>DebugSymbols

Логическое значение, которое определяет, является ли пакет Android *отлаживаемым*, в сочетании со свойством [`$(DebugType)`](#debugtype).
Отлаживаемый пакет содержит отладочные символы, устанавливает для [атрибута `//application/@android:debuggable`](https://developer.android.com/guide/topics/manifest/application-element#debug) значение `true` и автоматически добавляет разрешение [`INTERNET`](https://developer.android.com/reference/android/Manifest.permission#INTERNET),
чтобы отладчик мог подключиться к процессу. Приложение отлаживается, если `DebugSymbols` имеет значение `True` *, а*`DebugType` является пустой строкой или имеет значение `Full`.

## <a name="debugtype"></a>DebugType

Определяет [тип отладочных символов](/visualstudio/msbuild/csc-task), которые следует создать как часть сборки, что также влияет на возможность отладки приложения. Возможные значения:

- **Full**: создаются все символы. Если свойство [`DebugSymbols`](#debugsymbols)
  MSBuild также имеет значение `True`, то пакет приложения является отлаживаемым.

- **PdbOnly**: создаются символы PDB. Пакет приложения не является отлаживаемым.

Если свойство `DebugType` не задано или является пустой строкой, тогда свойство `DebugSymbols` определяет, является ли это приложение отлаживаемым.

## <a name="embedassembliesintoapk"></a>EmbedAssembliesIntoApk

Логическое свойство, которое определяет, следует ли внедрять сборки приложения в пакет приложения.

Это свойство должно иметь значение `True` для сборок выпуска и `False` для сборок отладки. В сборках отладки значение `True`*может* понадобиться, если быстрое развертывание не поддерживается для целевого устройства.

Если этому свойству присвоено значение `False`, то свойство MSBuild [`$(AndroidFastDeploymentType)`](#androidfastdeploymenttype)
также определяет вложения в `.apk`, что может повлиять на время развертывания и повторной сборки.

## <a name="enablellvm"></a>EnableLLVM

Логическое свойство, которое определяет, будет ли использована низкоуровневая виртуальная машина (LLVM) при компиляции Ahead-of-Time сборок в машинный код.

Чтобы создать проект, для которого включено это свойство, необходимо установить Android NDK.

Поддержка этого свойства была добавлена в Xamarin.Android версии 5.1.

По умолчанию это свойство имеет значение `False`.

Это свойство игнорируется, если только свойство MSBuild [`$(AotAssemblies)`](#aotassemblies) не имеет значение `True`.

## <a name="enableproguard"></a>EnableProguard

Логическое свойство, которое определяет, запускается ли [ProGuard](https://developer.android.com/tools/help/proguard.html) в рамках процесса упаковки для связывания кода Java.

Поддержка этого свойства была добавлена в Xamarin.Android версии 5.1.

По умолчанию это свойство имеет значение `False`.

Если установлено значение `True`, файлы [@(ProguardConfiguration)](~/android/deploy-test/building-apps/build-items.md#proguardconfiguration) будут использоваться для управления выполнением `proguard`.

## <a name="javamaximumheapsize"></a>JavaMaximumHeapSize

Указывает значение параметра **java**
`-Xmx` для использования при сборке файла `.dex` в процессе упаковки. Если он не указан, то параметр `-Xmx` задает для **java** значение `1G`. Это часто будет требоваться в Windows по сравнению с другими платформами.

Это свойство требуется указывать, если целевой объект [`_CompileDex` вызывает ошибку `java.lang.OutOfMemoryError`](https://bugzilla.xamarin.com/show_bug.cgi?id=18327).

Настройка значения путем изменения:

```xml
<JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
```

## <a name="javaoptions"></a>JavaOptions

Указывает дополнительные параметры командной строки для передачи **java** при сборке файла `.dex`.

## <a name="jarsignertimestampauthoritycertificatealias"></a>JarsignerTimestampAuthorityCertificateAlias

Это свойство позволяет указать псевдоним в хранилище ключей в качестве источника для меток времени.
Дополнительные сведения см. в документации по Java, посвященной [поддержке меток времени для сигнатур](https://docs.oracle.com/javase/8/docs/technotes/guides/security/time-of-signing.html).

```xml
<PropertyGroup>
    <JarsignerTimestampAuthorityCertificateAlias>Alias</JarsignerTimestampAuthorityCertificateAlias>
</PropertyGroup>
```

## <a name="jarsignertimestampauthorityurl"></a>JarsignerTimestampAuthorityUrl

Это свойство позволяет указать URL-адрес службы в качестве источника для меток времени. Так вы сможете гарантировать, что сигнатура `.apk` содержит метку времени.
Дополнительные сведения см. в документации по Java, посвященной [поддержке меток времени для сигнатур](https://docs.oracle.com/javase/8/docs/technotes/guides/security/time-of-signing.html).

```xml
<PropertyGroup>
    <JarsignerTimestampAuthorityUrl>http://example.tsa.url</JarsignerTimestampAuthorityUrl>
</PropertyGroup>
```

## <a name="linkerdumpdependencies"></a>LinkerDumpDependencies

Логическое свойство, которое включает создание файла зависимостей компоновщика. Этот файл может использоваться в качестве входных данных для средства [illinkanalyzer](https://github.com/mono/linker/blob/master/src/analyzer/README.md).

Файл зависимостей с именем `linker-dependencies.xml.gz` записывается в каталог проекта. В .NET5/6 он записывается рядом со связанными сборками в каталоге `obj/<Configuration>/android<ABI>/linked`.

Значение по умолчанию равно False.

## <a name="mandroidi18n"></a>MandroidI18n

Указывает поддержку интернационализации в приложении, например параметры сортировки и таблицы сортировки. Значение представляет собой список, разделенный запятой или точкой с запятой, из одного или нескольких следующих значений, не учитывающих регистр:

- **None**: не включать дополнительные кодировки.

- **All**: включить все доступные кодировки.

- **CJK**: включить китайские, японские и корейские кодировки, такие как *Японская (EUC)* \[enc-jp, CP51932\], *Японская (Shift-JIS)* \[iso-2022-jp, shift\_jis, CP932\], *Японская (JIS)* \[CP50220\], *Китайская упрощенная (GB2312)* \[gb2312, CP936\], *Корейская (UHC)* \[ks\_c\_5601-1987, CP949\], *Корейская (EUC)* \[euc-kr, CP51949\], *Китайская традиционная (Big5)* \[big5, CP950\] и *Китайская упрощенная (GB18030)* \[GB18030, CP54936\].

- **MidEast**: включить кодировки языков стран Ближнего Востока, такие как *Турецкая (Windows)* \[iso-8859-9, CP1254\], *Иврит (Windows)* \[windows-1255, CP1255\], *Арабская (Windows)* \[windows-1256, CP1256\], *Арабская (ISO)* \[iso-8859-6, CP28596\], *Иврит (ISO)* \[iso-8859-8, CP28598\], *Латиница 5 (ISO)* \[iso-8859-9, CP28599\] и *Иврит (Iso альтернативный)* \[iso-8859-8, CP38598\].

- **Other**: включить другие кодировки, такие как *Кириллица (Windows)* \[CP1251\], *Балтийская (Windows)* \[iso-8859-4, CP1257\], *Вьетнамская (Windows)* \[CP1258\], *Кириллица (KOI8-R)* \[koi8-r, CP1251\], *Украинская (KOI8-U)* \[koi8-u, CP1251\], *Балтийская (ISO)* \[iso-8859-4, CP1257\], *Кириллица (ISO)* \[iso-8859-5, CP1251\], *ISCII - Деванагари* \[x-iscii-de, CP57002\], *ISCII - Бенгальская* \[x-iscii-be, CP57003 \], *ISCII - Тамильская* \[x-iscii-ta, CP57004\], *ISCII - Телугу* \[x-iscii-te, CP57005\], *ISCII - Ассамская* \[x-iscii-as, CP57006\], *ISCII - Ория* \[x-iscii-or, CP57007\], *ISCII - Каннада* \[x-iscii ка CP57008\], *ISCII - Малаялам* \[x-iscii-ka, CP57009\], *ISCII - Гуджарати* \[x-iscii-gu, CP57010\], *ISCII - Панджаби* \[x-iscii-pa, CP57011\] и *Тайская (Windows)* \[CP874\].

- **Rare**: включить редкие кодировки, такие как *IBM EBCDIC (турецкий)* \[CP1026\], *IBM EBCDIC (латиница 1 Open Systems)* \[CP1047\], *IBM EBCDIC (США/Канада с евро)* \[CP1140\], *IBM EBCDIC (немецкая с евро)* \[CP1141\], *IBM EBCDIC (датская/норвежская с евро)* \[CP1142\], *IBM EBCDIC (финская/шведская с евро)* \[CP1143\], *IBM EBCDIC (итальянская с евро)* \[CP1144\], *IBM EBCDIC (латиноамериканская/испанская с евро)* \[CP1145\], *IBM EBCDIC (британская с евро)* \[CP1146\], *IBM EBCDIC (французская с евро)* \[CP1147\], *IBM EBCDIC (международная с евро)* \[CP1148\], *IBM EBCDIC (исландская с евро)* \[CP1149\], *IBM EBCDIC (Германия)* \[ CP20273\], *IBM EBCDIC (Дания и Норвегия)* \[CP20277\], *IBM EBCDIC (Финляндия и Швеция)* \[CP20278\], *IBM EBCDIC (Италия)* \[CP20280\], *IBM EBCDIC (латиноамериканская/испанская)* \[CP20284\], *IBM EBCDIC (британская)* \[CP20285\], *IBM EBCDIC (японская расширенная катакана)* \[CP20290\], *IBM EBCDIC (Франция)* \[CP20297\], *IBM EBCDIC (арабская)* \[CP20420\], *IBM EBCDIC (иврит)* \[CP20424\], *IBM EBCDIC (исландская)* \[CP20871\], *IBM EBCDIC (Кириллица сербско-болгарская)* \[CP21025\], *IBM EBCDIC (США и Канада)* \[ CP37\], *IBM EBCDIC (международная)* \[CP500\], *Арабская (ASMO 708)* \[CP708\], *Центральноевропейская (DOS)* \[CP852\]*, Кириллица (DOS)* \[CP855\], *Турецкая (DOS)* \[CP857\], *Западноевропейская (DOS с евро)* \[CP858\], *Иврит (DOS)* \[CP862\], *Арабская (DOS)* \[CP864\], *Русская (DOS)* \[CP866\], *Греческая (DOS)* \[CP869\], *IBM EBCDIC (латиница 2)* \[CP870\] и *IBM EBCDIC (греческая)* \[CP875\].

- **West**: включить западные кодировки, такие как *Западноевропейская (Mac)* \[macintosh, CP10000\], *Исландская (Mac)* \[x-mac-icelandic, CP10079\], *Центральноевропейская (Windows)* \[iso-8859-2, CP1250\], *Западноевропейская (Windows)* \[iso-8859-1, CP1252\], *Греческая (Windows)* \[iso-8859-7, CP1253\], *Центральноевропейская (ISO)* \[iso-8859-2, CP28592\], *Латиница 3 (ISO)* \[iso-8859-3, CP28593\], *Греческая (ISO)* \[iso-8859-7, CP28597\], *Латиница 9 (ISO)* \[iso-8859-15, CP28605\], *OEM - США* \[CP437\], *Западноевропейская (DOS)* \[CP850\], *Португальская (DOS)* \[CP860\], *Исландская (DOS)* \[CP861\], *Французская канадская (DOS)* \[CP863\] и *Скандинавская (DOS)* \[CP865\].

```xml
<MandroidI18n>West</MandroidI18n>
```

## <a name="monoandroidresourceprefix"></a>MonoAndroidResourcePrefix

Указывает *префикс пути*, который удаляется в начале имен файлов с помощью действия сборки `AndroidResource`. Это позволяет изменять расположение ресурсов.

Значение по умолчанию — `Resources`. Установите значение `res` для структуры проекта Java.

## <a name="monosymbolarchive"></a>MonoSymbolArchive

Логическое свойство, которое определяет, следует ли создавать артефакты `.mSYM` для последующего использования с `mono-symbolicate`, чтобы извлечь &ldquo;"реальные"&rdquo; имя файла и номер строки из трассировки стека выпуска.

Для приложений &ldquo;выпуска&rdquo; значением по умолчанию является True с включенными отладочными символами: [`$(EmbedAssembliesIntoApk)`](#embedassembliesintoapk) — True, [`$(DebugSymbols)`](~/android/deploy-test/building-apps/build-properties.md#debugsymbols) —
True и [`$(Optimize)`](/visualstudio/msbuild/common-msbuild-project-properties) —
True.

Свойство добавлено в Xamarin.Android версии 7.1.
