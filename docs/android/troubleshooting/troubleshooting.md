---
title: Советы по устранению неполадок
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 8855c80dd7478813408abaf2cfec68d48eced3bc
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571835"
---
# <a name="troubleshooting-tips"></a>Советы по устранению неполадок

## <a name="getting-diagnostic-information"></a>Получение диагностических сведений

В Xamarin.Android есть несколько областей, которые следует рассмотреть при отслеживании различных ошибок.
Сюда входит следующее.

1. Выходные данные диагностики MSBuild.
2. Журналы развертывания устройств.
3. Выходные данные журнала отладки Android.

<a name="Diagnostic_MSBuild_Output"></a>

## <a name="diagnostic-msbuild-output"></a>Выходные данные диагностики MSBuild

Диагностика MSBuild может содержать дополнительные сведения, относящиеся к сборке пакетов, а также некоторые сведения о развертывании пакета.

Чтобы включить вывод диагностики MSBuild в Visual Studio, выполните следующие действия:

1. Щелкните **Инструменты > Параметры...** .
2. В представлении дерева слева выберите **Проекты и решения > Сборка и запуск**.
3. На панели справа в раскрывающемся списке детализации выходных данных сборки MSBuild выберите вариант "Диагностика".
4. Нажмите кнопку **ОК**.
5. Выполните очистку и перестройку пакета.
6. Выходные данные диагностики появятся в области вывода.

Чтобы включить выходные данные диагностики MSBuild в Visual Studio для Mac/OS X, выполните следующие действия.

1. Щелкните **Visual Studio для Mac > Параметры...** .
2. В представлении дерева слева выберите **Проекты > Сборка**.
3. На панели справа откройте раскрывающийся список детализации журнала и выберите вариант "Диагностика".
4. Нажмите кнопку **ОК**.
5. Перезапустите Visual Studio для Mac.
6. Выполните очистку и перестройку пакета.
7. Просмотрите выходные данные диагностики на панели ошибок (**Представление > Панели > Ошибки**), нажав кнопку "Выходные данные сборки".

## <a name="device-deployment-logs"></a>Журналы развертывания устройств

Чтобы включить ведение журнала развертывания устройств в Visual Studio, выполните следующие действия.

1. **Сервис > Параметры…** >
2. В представлении дерева слева выберите **Xamarin > Параметры Android**.
3. На панели справа поставьте флажок [X] **ведение журнала отладки расширения (записывает monodroid.log на рабочий стол)** .
4. Сообщения журнала записываются в файл monodroid.log на рабочем столе.

Visual Studio для Mac всегда записывает журналы развертывания устройств. Найти их немного сложнее — файл журнала *AndroidUtils* создается на каждый день с указанием времени, когда происходит развертывание, например: **AndroidTools-2012-10-24_12-35-45.log**.

- В Windows файлы журнала записываются в `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`.
- В OS X файлы журнала записываются в `$HOME/Library/Logs/XamarinStudio-{VERSION}`.

## <a name="android-debug-log-output"></a>Выходные данные журнала отладки Android

Android записывает много сообщений в [журнал отладки Android](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin.Android использует свойства системы Android для управления созданием дополнительных сообщений в журнале отладки Android. Свойства системы Android можно задать с помощью команды *setprop* в [Android Debug Bridge (adb)](https://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Свойства системы считываются во время запуска процесса и поэтому должны быть установлены перед запуском приложения, или приложение должно быть перезапущено после изменения системных свойств.

### <a name="xamarinandroid-system-properties"></a>Системные свойства Xamarin.Android

Xamarin.Android поддерживает следующие системные свойства.

- *debug.mono.debug*. Если не пустая строка, эквивалентно `*mono-debug*`.

- *debug.mono.env*. Разделенный вертикальной чертой список переменных среды ( *|* ), которые необходимо экспортировать во время запуска приложения *перед* инициализацией Mono. Это позволяет задать переменные среды, управляющие ведением журнала Mono.

  > [!NOTE]
  > Так как значение имеет разделитель *|* , у него должен быть дополнительный уровень кавычек, поскольку команда \`*adb shell*\` удаляет набор кавычек.

  > [!NOTE]
  > Длина значений системных свойств Android не может превышать 92 символа.

  Пример.

  ```
  adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
  ```

- *debug.mono.log*. Список компонентов, разделенных запятыми (" *,* "), которые должны заносить дополнительные сообщения в журнал отладки Android. По умолчанию не задано ничего. Возможные компоненты:

  - *all*: занесение всех сообщений.
  - *gc*: занесение сообщений о сборке мусора.
  - *gref*: занесение сообщений о распределении и освобождении (слабых, глобальных) ссылок.
  - *lref*. занесение сообщений о распределении и освобождении локальных ссылок.

  > [!NOTE]
  > Это *чрезвычайно* подробный уровень. Не включайте этот параметр, если он вам не требуется.

- *debug.mono.trace*. Позволяет задать параметр [mono --trace](http://docs.go-mono.com/?link=man%3amono(1))`=PROPERTY_VALUE`.

## <a name="deleting-bin-and-obj"></a>Удаление `bin` и `obj`

В Xamarin.Android раньше возникала подобная проблема:

- Возникает странная ошибка сборки или времени выполнения.
- Вы выполняете операции `Clean`, `Rebuild` или вручную удаляете каталоги `bin` и `obj`.
- Проблема исчезает.

Мы много работали над устранением таких проблем, поскольку они влияют на производительность разработчиков.

Если у вас возникает следующая проблема:

1. Обратите на нее внимание. Какое действие было выполнено последним и вызвало это состояние проекта?
1. Сохраните текущий журнал сборки. Попробуйте выполнить сборку еще раз и запишите [диагностический журнал сборки](#diagnostic-msbuild-output).
1. Отправьте [отчет об ошибке][bug].

Прежде чем удалять каталоги `bin` и `obj`, заархивируйте их и сохраните для последующей диагностики при необходимости. Возможно, достаточно просто выполнить операцию `Clean` для проекта приложения Xamarin.Android, чтобы все снова заработало.

[bug]: https://github.com/xamarin/xamarin-android/wiki/Submitting-Bugs,-Feature-Requests,-and-Pull-Requests

## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android не может разрешить System.ValueTuple

Эта ошибка возникает из-за несовместимости с Visual Studio.

- **Обновление 1 для Visual Studio 2017** (версия 15.1 или более ранняя) совместимо только с **пакетом NuGet System.ValueTuple 4.3.0** (или более ранней версии).

- **Обновление 2 для Visual Studio 2017** (версия 15.2 или более поздняя) совместимо только с **пакетом NuGet System.ValueTuple 4.3.1** (или более поздней версии).

Выберите правильный пакет NuGet System.ValueTuple, соответствующий установке Visual Studio 2017.

## <a name="gc-messages"></a>Сообщения сборки мусора

Сообщения компонента сборки мусора можно просмотреть, задав для системного свойства debug.mono.log значение, содержащее gc.

Сообщения сборки мусора создаются каждый раз, когда выполняется сборка мусора, и предоставляют сведения о том, сколько работы выполнено в ходе сборки мусора:

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Дополнительные сведения о сборке мусора, например о времени, можно создать, задав для переменной среды `MONO_LOG_LEVEL` значение `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Это приведет к появлению (множества) дополнительных сообщений Mono, включая следующие три последствия.

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

В сообщении `GC_BRIDGE` `num-objects` — количество объектов моста, рассматриваемых в ходе этого прохода, а `num_hash_entries` — количество объектов, обработанных в ходе этого вызова кода моста.

В сообщениях `GC_MINOR` и `GC_MAJOR` `total` — время приостановки (потоки не выполняются), а `bridge` — время, затрачиваемое на код обработки моста (который работает с виртуальной машиной Java). Во время обработки моста среда *не* приостанавливается.

 *В целом* чем больше значение `num_hash_entries`, тем больше времени займут сборки `bridge` и тем больше будет время `total`, потраченное на сбор.

## <a name="global-reference-messages"></a>Сообщения по глобальным ссылкам

Чтобы включить ведение журнала глобальных ссылок, системное свойство *debug.mono.log* должно содержать *gref*, например:

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android использует глобальные ссылки Android для предоставления сопоставлений между экземплярами Java и связанными управляемыми экземплярами, так как при вызове метода Java необходимо предоставить экземпляр Java.

К сожалению, эмуляторы Android допускают наличие только 2000 глобальных ссылок за раз. У оборудования существует более высокий предел в 52 000 глобальных ссылок. Меньшее ограничение может представлять проблему при запуске приложений в эмуляторе, поэтому полезно знать,  *откуда* поступает экземпляр.

> [!NOTE]
> Количество глобальных ссылок считается внутри Xamarin.Android и не может содержать глобальные ссылки, извлеченные другими собственными библиотеками, загруженными в процесс. Используйте количество глобальных ссылок для расчета.

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

Существует четыре сообщения о последствиях.

- Создание глобальной ссылки: это строки, которые начинаются с *+g+* и предоставляют трассировку стека для создания пути кода.
- Уничтожение глобальной ссылки: это строки, которые начинаются с *-g-* и могут предоставлять трассировку стека для удаления пути кода глобальной ссылки. Если сборщик мусора удаляет глобальную ссылку, трассировка стека не предоставляется.
- Создание слабой глобальной ссылки: это строки, начинающиеся с *+w+* .
- Уничтожение слабой глобальной ссылки: это строки, начинающиеся с *-w-* .

Во всех сообщениях значение *grefc* — это число глобальных ссылок, созданных Xamarin.Android, а значение *grefwc* — это число слабых глобальных ссылок, созданных Xamarin.Android. Значение *handle* или *obj-handle* является значением обработчика JNI, а символ после " */* " является типом значения обработчика: */L* для локальной ссылки, */G* для глобальных ссылок и */W* для слабых глобальных ссылок.

В рамках процесса сборки мусора глобальные ссылки (+g+) преобразуются в слабые глобальные ссылки (+w+ и -g-), выполняется сборка мусора на стороне Java, а затем проверяется наличие слабой глобальной ссылки, чтобы определить, была ли она собрана. Если она все еще активна, то для слабой ссылки создается новая глобальная ссылка (+g+, -w-), в противном случае слабая ссылка уничтожается (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Экземпляр Java создается и упаковывается с помощью MCW

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>Выполняется сборка мусора...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>Объект по-прежнему активен, так как handle! = null
## <a name="wref-turned-back-into-a-gref"></a>Слабая ссылка снова становится глобальной ссылкой

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>Объект дезактивируется, так как handle == null
## <a name="wref-is-freed-no-new-gref-created"></a>Слабая ссылка удалена, новая глобальная ссылка не создается

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

Здесь есть один интересный момент: на целевых объектах под управлением Android до версии 4.0 значение глобальной ссылки равно адресу объекта Java в памяти среды выполнения Android. (Т. е. сборщик мусора является неподвижным консервативным сборщиком и передает прямые ссылки на эти объекты.) Таким образом, после последовательности +g+, +w+, -g-, +g+, -w- получившаяся глобальная ссылка будет иметь то же значение, что и исходная. Это упрощает обработку журналов.

Но в Android 4.0 есть перемещаемый сборщик, который больше не передает прямые ссылки на объекты виртуальных машин среды выполнения Android. Поэтому после последовательности +g+, +w+, -g-, +g+, -w- значение глобальной ссылки *будет другим*. Если объект остается активным после нескольких сборок мусора, у него образуется несколько значений глобальной ссылки, что усложняет определение места, из которого фактически был выделен экземпляр.

### <a name="querying-programmatically"></a>Программное выполнение запросов

Вы можете узнать количество глобальных и слабых ссылок, выполнив запрос к объекту `JniRuntime`.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` — количество глобальных ссылок

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` — количество слабых ссылок

## <a name="android-debug-logs"></a>Журналы отладки Android

[Журналы отладки Android](~/android/deploy-test/debugging/android-debug-log.md) могут предоставить дополнительный контекст для любых ошибок во время выполнения.

## <a name="floating-point-performance-is-terrible"></a>Производительность операций с плавающей запятой очень низкая!

Или: "Мое приложение работает 10 раз быстрее с отладочной сборкой, чем со сборкой выпуска!"

Xamarin.Android поддерживает несколько ABI устройств: *armeabi*, *armeabi-v7a* и *x86*. ABI устройств можно указать в разделе **Свойства проекта > вкладка "Приложение" > Поддерживаемые архитектуры**.

В отладочных сборках используется пакет Android, который предоставляет все ABI, поэтому для целевого устройства будет использоваться самый быстрый ABI.

Сборки выпуска будут включать только ABI, выбранные на вкладке "Свойства проекта". Можно выбрать несколько ABI.

*armeabi* является интерфейсом ABI по умолчанию и обладает самыми широкими возможностями поддержки устройств. *Однако* armeabi не поддерживает многопроцессорные устройства и аппаратные операции с плавающей запятой, а также другие возможности. Следовательно, приложения, использующие среду выполнения armeabi, будут привязаны к одному ядру и будут использовать реализацию soft-float. Оба ограничения могут значительно снизить производительность приложения.

Если приложению требуется высокая производительность для операций с плавающей запятой (например, игры), следует включить ABI *armeabi-v7a*. Вы можете выбрать поддержку только среды выполнения *armeabi-v7a*, хотя это означает, что старые устройства, которые поддерживают только *armeabi*, не смогут запустить приложение.

## <a name="could-not-locate-android-sdk"></a>Не удалось выполнить обнаружение пакета SDK для Android

Для пакета SDK для Android в Windows Google предоставляет 2 загрузки.
При выборе установщика exe будут записаны разделы реестра, указывающие Xamarin.Android на место установки. Если вы выберете ZIP-файл и распакуете его самостоятельно, Xamarin.Android не узнает, где искать пакет SDK. Вы можете сообщить Xamarin.Android, где находится пакет SDK, в Visual Studio, в разделе **Средства > Параметры > Xamarin > Параметры Android**:

[![Расположение пакета SDK для Android в параметрах Xamarin.Android](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)

## <a name="ide-does-not-display-target-device"></a>В интегрированной среде разработки не отображается целевое устройство

Иногда вы пытаетесь развернуть приложение на устройстве, но устройство, которое вы хотите развернуть, не отображается в диалоговом окне "Выбор устройства". Это может произойти, если Android Debug Bridge неактивен.

Чтобы диагностировать эту проблему, найдите [программу adb](~/android/deploy-test/debugging/android-debug-log.md), а затем выполните следующую команду:

```shell
adb devices
```

Если устройство отсутствует, необходимо перезапустить сервер Android Debug Bridge, чтобы можно было найти его:

```shell
adb kill-server
adb start-server
```

Программное обеспечение HTC Sync может препятствовать правильной работе **adb start-server**. Если команда **adb start-server** не выводит порт для запуска, выйдите из программы HTC Sync и попробуйте перезапустить сервер adb.

## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>Не удается запустить указанный исполняемый файл задачи keytool

Это означает, что путь не содержит каталог, в котором находится каталог bin пакета SDK для Java. Проверьте, выполнены ли эти действия из руководства по [установке](~/android/get-started/installation/index.md).

## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>Файл monodroid.exe или aresgen.exe завершил работу с кодом 1

Чтобы решить эту проблему, перейдите в Visual Studio и измените уровень детализации MSBuild. Для этого выберите: **Сервис > Параметры > Проект** и **Решения > Сборка**, **Запуск > Уровень детализации выходных данных о сборке проекта MSBuild** и присвойте этому параметру значение **Обычный**.

Выполните сборку повторно и проверьте область вывода Visual Studio, которая должна содержать полную ошибку.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>На устройстве недостаточно дискового пространства для развертывания пакета

Это происходит, когда вы не запускаете эмулятор из среды Visual Studio. При запуске эмулятора вне среды Visual Studio необходимо передать параметры `-partition-size 512`, например

```shell
emulator -partition-size 512 -avd MonoDroid
```

Убедитесь, что используете правильное имя симулятора, т. е. [имя, которое использовалось при настройке симулятора](~/android/get-started/installation/windows.md#device).

## <a name="install_failed_invalid_apk-when-installing-a-package"></a>INSTALL\_FAILED\_INVALID\_APK при установке пакета

Имена пакетов Android *должны* содержать точку (" *.* "). Измените имя пакета, чтобы оно содержало точку.

- В среде Visual Studio
  - Щелкните правой кнопкой мыши проект и выберите "Свойства".
  - Перейдите на вкладку "Манифест Android" слева.
  - Измените значение в поле "Имя пакета".
    - Если отображается сообщение: &ldquo;Не найден файл AndroidManifest.xml. Щелкните, чтобы добавить его.&rdquo;, — щелкните ссылку, а затем измените имя пакета.
- В Visual Studio для Mac:
  - Щелкните правой кнопкой мыши проект и выберите "Параметры".
  - Перейдите к разделу "Сборка/Приложение Android".
  - Измените значение в поле "Имя пакета", чтобы оно содержало ".".

## <a name="install_failed_missing_shared_library-when-installing-a-package"></a>INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY при установке пакета

"Общая библиотека" в этом контексте *не* является файлом собственной общей библиотеки (*libfoo.so*). Это библиотека, которая должна быть отдельно установлена на целевом устройстве, например в Google Maps.

Пакет Android определяет, какие общие библиотеки требуются для элемента `<uses-library/>`. Если на целевом устройстве отсутствует *необходимая* библиотека (например, `//uses-library/@android:required` имеет значение *true*, что является значением по умолчанию), установка пакета завершится сбоем с ошибкой *INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY*.

Чтобы определить, какие общие библиотеки требуются, просмотрите *созданный файл*
**AndroidManifest.xml** (например, **obj\\Debug\\android\\AndroidManifest.xml**) и найдите элементы `<uses-library/>`. Элементы `<uses-library/>` можно добавлять вручную в файл **Properties\\AndroidManifest.xml** и через [настраиваемый атрибут UsesLibraryAttribute](xref:Android.App.UsesLibraryAttribute).

Например, при добавлении ссылки на сборку в *Mono.Android.GoogleMaps.dll* будет неявно добавляться `<uses-library/>` для общей библиотеки Google Maps.

## <a name="install_failed_update_incompatible-when-installing-a-package"></a>INSTALL\_FAILED\_UPDATE\_INCOMPATIBLE при установке пакета

У пакетов Android есть три требования.

- Они должны содержать "." (см. предыдущий пункт).
- Они должны иметь уникальное имя строки пакета (поэтому в именах приложений Android, например com.android.chrome для приложения Chrome, рассматривается соглашение об обратных доменах).
- При обновлении пакетов пакет должен иметь один и тот же ключ подписывания.

Представьте себе следующий сценарий.

1. Вы создаете и развертываете приложение для отладки.
2. Вы изменяете ключ подписывания, например для использования в качестве приложения выпуска (или если вы не хотите использовать ключ подписывания по умолчанию).
3. Вы устанавливаете приложение, не удалив его сначала, например: Отладка > Запуск без отладки в Visual Studio.

В этом случае установка пакета завершится ошибкой INSTALL\_FAILED\_UPDATE\_INCOMPATIBLE, так как имя пакета не изменилось во время выполнения ключа подписывания. [Журнал отладки Android](~/android/deploy-test/debugging/android-debug-log.md) также будет содержать примерно следующее сообщение:

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Чтобы устранить эту ошибку, полностью удалите приложение с устройства, прежде чем выполнять повторную установку.

## <a name="install_failed_uid_changed-when-installing-a-package"></a>INSTALL\_FAILED\_UID\_CHANGED при установке пакета

При установке пакета Android ему назначается *идентификатор пользователя* (UID).
*Иногда* по неизвестным причинам при установке поверх уже установленного приложения происходит сбой установки с ошибкой `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Чтобы обойти эту ошибку, *полностью удалите* пакет Android, установив приложение из графического пользовательского интерфейса целевого устройства Android или используя `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**НЕ ИСПОЛЬЗУЙТЕ** `adb uninstall -k`, так как это *сохранит* данные приложения и, таким образом, конфликтующий UID останется на целевом устройстве.

## <a name="release-apps-fail-to-launch-on-device"></a>Не удается запустить приложения выпуска на устройстве

В выходных данных журнала отладки Android может содержаться примерно следующее сообщение:

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

У этого есть две возможные причины:

1. APK не предоставляет ABI, поддерживаемый целевым устройством.
    Например, APK содержит только двоичные файлы armeabi-v7a, а целевое устройство поддерживает только armeabi.

2. [Ошибка Android](https://code.google.com/p/android/issues/detail?id=21670). В этом случае удалите приложение, скрестите пальцы и переустановите приложение.

Чтобы исправить (1), измените параметры/свойства проекта и [добавьте поддержку требуемого интерфейса ABI в список поддерживаемых ABI](~/android/app-fundamentals/cpu-architectures.md). Чтобы определить, какой ABI необходимо добавить, выполните следующую команду ADB на целевом устройстве:

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

Выходные данные будут содержать основной (и необязательный дополнительный) ABI.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Свойство OutPath не задано для проекта &ldquo;MyApp.csproj&rdquo;

Обычно это означает, что у вас есть компьютер HP, а для переменной среды &ldquo;Platform&rdquo; было установлено MCD или HPD. Это противоречит свойству платформы MSBuild, которое обычно имеет значение &ldquo;Любой ЦП&rdquo; или &ldquo;x86&rdquo;. Необходимо удалить эту переменную среды с компьютера, чтобы MSBuild мог работать:

- Панель управления > Система > Дополнительно > Переменные среды

Перезапустите Visual Studio или Visual Studio для Mac и попытайтесь выполнить перестроение. Теперь все должно работать правильно.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: mono.android.runtime.JavaObject невозможно привести к...

Xamarin.Android 4.x неправильно маршалирует вложенные универсальные типы. Например, рассмотрим следующий код C\#, который использует [SimpleExpandableListAdapter](xref:Android.Widget.SimpleExpandableListAdapter):

```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```

Проблема заключается в том, что Xamarin.Android неправильно маршалирует вложенные универсальные типы. `List<IDictionary<string, object>>` маршалируется в [java.lang.ArrrayList](xref:Java.Util.ArrayList), но `ArrayList` содержит экземпляры `mono.android.runtime.JavaObject` (которые ссылаются на экземпляры `Dictionary<string, object>`) вместо элемента, который реализует [java.util.Map](xref:Java.Util.IMap), что приводит к следующему исключению:

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

Чтобы устранить эту проблему, используйте предоставленные [типы коллекций Java](~/android/internals/api-design.md), а не типы `System.Collections.Generic` для &ldquo;внутренних&rdquo; типов. Это приведет к появлению соответствующих типов Java при маршалинге экземпляров. (Следующий код является более сложным, чем требуется для сокращения времени жизни глобальной ссылки. Его можно упростить путем изменения исходного кода с помощью `s/List/JavaList/g` и `s/Dictionary/JavaDictionary/g`, если время существования глобальной ссылки не имеет значения.)

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[Эта проблема будет исправлена в будущем выпуске](https://bugzilla.xamarin.com/show_bug.cgi?id=5401).

## <a name="unexpected-nullreferenceexceptions"></a>Неожиданные исключения NullReferenceException

Иногда в [журнале отладки Android](~/android/deploy-test/debugging/android-debug-log.md) упоминаются NullReferenceException, которые &ldquo;не могут произойти&rdquo; или поступают из Mono для кода среды выполнения Android незадолго перед отключением приложения:

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

or

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

Это может произойти, когда среда выполнения Android решает прервать процесс по ряду причин, включая ограничение глобальной ссылки целевого объекта или &ldquo;проблемы&rdquo; с JNI.

Чтобы узнать, в этом ли причина, проверьте в журнале отладки Android сообщение от процесса, аналогичное следующему:

```shell
E/dalvikvm(  123): VM aborting
```

## <a name="abort-due-to-global-reference-exhaustion"></a>Прерывание из-за исчерпания глобальной ссылки

Слой JNI среды выполнения Android поддерживает только ограниченное число действительных ссылок на объекты JNI в любой момент времени. При превышении этого ограничения происходит прерывание операции.

Ограничение *глобальных ссылок* составляет 2000 ссылок для эмулятора и примерно 52 000 ссылок для оборудования.

Вы понимаете, что глобальных ссылок становится слишком много, когда в журнале отладки Android появляются такие сообщения:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

При достижении предельного значения глобальных ссылок выводится следующее сообщение:

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```

В приведенном выше примере (который, кстати, является результатом [ошибки 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)) проблема заключается в том, что создается слишком много экземпляров Android.Graphics.Point. Список исправлений для этой ошибки см. в [комментарии \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2).

Как правило, следует узнать, у какого типа слишком много выделенных экземпляров — Android.Graphics.Point в приведенном выше дампе, — а затем найти, где они создаются в исходном коде, и избавиться от них соответствующим образом (чтобы сократить время существования их Java-объекта). Это не всегда подходящее решение (\#685215 является многопоточным, поэтому стандартное решение позволяет избежать вызова Dispose), но это первое, о чем следует подумать.

Вы можете включить [Ведения журнала глобальных ссылок](~/android/troubleshooting/index.md), чтобы увидеть, когда глобальные ссылки создаются и сколько их.

## <a name="abort-due-to-jni-type-mismatch"></a>Прерывание из-за несоответствия типов JNI

Если вы вручную развертываете код JNI, типы могут сопоставляться неправильно, например при попытке вызвать `java.lang.Runnable.run` для типа, который не реализует `java.lang.Runnable`. В этом случае в журнале отладки Android будет отображаться примерно следующее сообщение:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Поддержка динамического кода

### <a name="dynamic-code-does-not-compile"></a>Динамический код не компилируется

Чтобы использовать в приложении или библиотеке динамический C\#, необходимо добавить в проект файлы System.Core.dll, Microsoft.CSharp.dll и Mono.CSharp.dll.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>В сборке выпуска исключение MissingMethodException возникает для динамического кода во время выполнения.

- Скорее всего, проект приложения не имеет ссылок на System.Core.dll, Microsoft.CSharp.dll или Mono.CSharp.dll. Убедитесь, что ссылки на эти сборки существуют.

  - Помните, что динамический код всегда связан с затратами. Если вам важна эффективность, рекомендуется не использовать динамический код.

- В первой предварительной версии эти сборки были исключены, если в коде приложения явно не использовались типы в каждой сборке. Обходное решение см. на этой странице: [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)

## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Проекты, созданные с помощью AOT + LLVM, завершаются сбоем на устройствах x86

При развертывании приложения, созданного с помощью [AOT + LLVM](~/android/deploy-test/release-prep/index.md) на устройствах на базе x86, может появиться сообщение об ошибке, похожее на следующее:

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (Xamarin.bug56111)
```

Это известная проблема — решение заключается в отключении LLVM.
