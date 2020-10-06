---
title: Среда Xamarin.Android
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: da0e3775f400c965ee59a762884e638e3379c8df
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454862"
---
# <a name="xamarinandroid-environment"></a>Среда Xamarin.Android

## <a name="execution-environment"></a>Среда выполнения

*Среда выполнения* представляет собой набор переменных среды и системных свойств Android, которые влияют на выполнение программ. Чтобы задать свойства системы Android, используется команда `adb shell setprop`, а для переменных среды — системное свойство `debug.mono.env`.

```shell
## Enable GREF logging
adb shell setprop debug.mono.log gref

## Set the MONO_LOG_LEVEL and MONO_LOG_MASK environment variables
## so that additional Mono messages will be written to `adb logcat`.
adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
```

Свойства системы Android устанавливаются сразу для всех процессов на целевом устройстве.

Начиная с Xamarin.Android версии 4.6 системные свойства и переменные среды можно установить или переопределить отдельно для каждого приложения, добавив в проект *файл среды*. Файл среды имеет простой текстовый формат Unix и описывает [**действие сборки** для `AndroidEnvironment`](~/android/deploy-test/building-apps/build-process.md).
Строки в файле среды содержат пары *ключ-значение*.
Строки, которые начинаются с `#`, считаются комментариями. Пустые строки не учитываются.

Если *ключ* начинается с заглавной буквы, то этот *ключ* рассматривается как переменная среды. То есть при запуске процесса выполняется команда **setenv**(3) для создания переменной среды с указанным *значением*.

Если *ключ* начинается с прописной буквы, то этот *ключ* рассматривается как системное свойство Android, а указанное *значение* используется как *значение по умолчанию*. Системные свойства Android, которые управляют поведением Xamarin.Android, сначала проверяются в хранилище системных свойств Android. Значение из файла среды используется, только если такое свойство не существует на сервере. Такой механизм позволяет в диагностических целях переопределить значения, указанные в файле среды, с помощью `adb shell setprop`.

## <a name="xamarinandroid-environment-variables"></a>Переменные среды Xamarin.Android

Xamarin.Android поддерживает переменную `XA_HTTP_CLIENT_HANDLER_TYPE`, значение которой можно задать с помощью `adb shell setprop debug.mono.env` или действия сборки `$(AndroidEnvironment)`.

### `XA_HTTP_CLIENT_HANDLER_TYPE`

Тип с указанием сборки, который должен наследовать значение от [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler?view=xamarinandroid-7.1) и использовать [конструктор по умолчанию `HttpClient()`](/dotnet/api/system.net.http.httpclient.-ctor?view=xamarinandroid-7.1#System_Net_Http_HttpClient__ctor).

В версии Xamarin.Android 6.1 эта переменная среды не устанавливается по умолчанию. Вместо нее используется [HttpClientHandler](/dotnet/api/system.net.http.httpclienthandler?view=xamarinandroid-7.1).

Кроме того, может быть указано значение `Xamarin.Android.Net.AndroidClientHandler` для использования [`java.net.URLConnection`](xref:Java.Net.URLConnection)
для доступа к сети, который *может* разрешить использование TLS 1.2, если Android это поддерживает.

Свойство добавлено в Xamarin.Android версии 6.1.

## <a name="xamarinandroid-system-properties"></a>Системные свойства Xamarin.Android

Xamarin.Android поддерживает следующие системные свойства, которые можно задать с помощью `adb shell setprop` или действия сборки `$(AndroidEnvironment)`:

- `debug.mono.debug`
- `debug.mono.env`
- `debug.mono.gc`
- `debug.mono.log`
- `debug.mono.max_grefc`
- `debug.mono.profile`
- `debug.mono.runtime_args`
- `debug.mono.trace`
- `debug.mono.wref`
- `XA_HTTP_CLIENT_HANDLER_TYPE`

### `debug.mono.debug`

Значение системного свойства `debug.mono.debug` должно быть целым числом. Если это значение `1`, то все действия выполняются так, как если бы процесс был запущен с параметром `mono --debug`.
Это позволяет выводить сведения о файле и строке в трассировку стека и получать другую информацию, не открывая для приложения отладчик.

### `debug.mono.env`

Содержит список переменных среды, разделенных символом `|`.

### `debug.mono.gc`

Значение системного свойства `debug.mono.debug` должно быть целым числом.
Если это значение `1`, то сведения о сборке мусора будут вноситься в журнал.

Это эквивалентно наличию системного свойства `debug.mono.log` со значением `gc`.

### `debug.mono.log`

Устанавливает, какие дополнительные сведения Xamarin.Android будет записывать в `adb logcat`.
Представляет собой строку с запятыми (`,`) в качестве разделителей, составленную из комбинации следующих значений:

- `all`: вывод *всех* сообщений. Такой вариант нужен редко, так как он включает даже сообщения `lref`.
- `assembly`: вывод `.apk` и сообщений о синтаксическом анализе сборки.
- `gc`: вывод сообщений о сборке мусора.
- `gref`: вывод сообщений глобальной ссылки JNI.
- `lref`: вывод сообщений локальной ссылки JNI.
  > [!NOTE]
  > Это выводит *огромный* объем `adb logcat`.
  > В Xamarin.Android 5.1 оно дополнительно приводит к созданию файла `.__override__/lrefs.txt`, размер которого может *чрезмерно увеличиться*.
  > Не применяйте без крайней необходимости.
- `timing`: вывод некоторых сведений о времени выполнения методов. Также создаются файлы `.__override__/methods.txt` и `.__override__/counters.txt`.

### `debug.mono.max_grefc`

Значение системного свойства `debug.mono.max_grefc` должно быть целым числом.
Это значение *переопределяет* обнаруженное максимальное количество GREF, установленное по умолчанию для целевого устройства.

*Обратите внимание:* этот вариант можно указать только в `adb shell setprop
debug.mono.max_grefc`, так как информация из файла **environment.txt** считывается позже.

### `debug.mono.profile`

Системное свойство `debug.mono.profile` включает профилировщик.
Оно имеет такое же назначение и использует те же значения, что и параметр `mono --profile`. (Дополнительные сведения см. на странице со справочной информацией о команде [**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)).)

### `debug.mono.runtime_args`

Системное свойство `debug.mono.runtime_args` содержит дополнительные параметры, которые должны анализироваться командой **mono**.

### `debug.mono.trace`

Системное свойство `debug.mono.trace` включает трассировку.
Оно имеет такое же назначение и использует те же значения, что и параметр `mono --trace`. (Дополнительные сведения см. на странице со справочной информацией о команде [**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)).)

Обычного его *не следует использовать*. Трассировка заполнит сообщениями выходные данные `adb logcat`, значительно замедлит работу программы и может изменить ее поведение (вплоть до появления новых ошибок).

Но *иногда* трассировка позволяет получить некоторые важные результаты.

### `debug.mono.wref`

Системное свойство `debug.mono.wref` позволяет переопределить значения по умолчанию, обнаруженные для механизма слабых ссылок JNI. Здесь поддерживаются два значения:

- `jni`: использовать слабые ссылки JNI, которые создаются `JNIEnv::NewWeakGlobalRef()` и уничтожаются `JNIEnv::DeleteWeakGlobalREf()`.
- `java`: использовать глобальные ссылки JNI на экземпляры `java.lang.WeakReference`.

По умолчанию `java` применяется в версиях до API-7 и в API 19 (Kit Kat) с включенным режимом ART. (В API-8 добавлены ссылки `jni`, а режим ART *нарушал работу ссылок * `jni`.)

Это системное свойство удобно для тестирования и определенных исследований.
*Обычно* его не нужно изменять.

### <a name="xa_http_client_handler_type"></a>XA\_HTTP\_CLIENT\_HANDLER\_TYPE

Эта переменная среды, которая появилась в версии Xamarin.Android 6.1, объявляет реализацию `HttpMessageHandler` по умолчанию, которая будет использоваться для `HttpClient`. По умолчанию эта переменная не задана и Xamarin.Android использует `HttpClientHandler`.

```shell
XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
```

> [!NOTE]
> Базовое устройство Android должно поддерживать протокол TLS 1.2.
TLS 1.2 поддерживается во всех версиях Android начиная с 5.0.

## <a name="example"></a>Пример

```shell
## Comments are lines which start with '#'
## Blank lines are ignored.

## Enable GREF messages to `adb logcat`
debug.mono.log=gref

## Clear out a Mono environment variable to decrease logging
MONO_LOG_LEVEL=
```

## <a name="related-links"></a>Связанные ссылки

- [Протокол TLS](~/cross-platform/app-fundamentals/transport-layer-security.md)