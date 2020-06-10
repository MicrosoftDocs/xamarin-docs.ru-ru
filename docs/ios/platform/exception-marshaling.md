---
title: Маршалирование исключений в Xamarin. iOS
description: В этом документе описывается, как работать с собственными и управляемыми исключениями в приложении Xamarin. iOS. Обсуждаются проблемы, которые могут возникнуть, и решение этих проблем.
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/05/2017
ms.openlocfilehash: 07b39f87b6eeb0fc24486be83573a721abc07966
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572407"
---
# <a name="exception-marshaling-in-xamarinios"></a>Маршалирование исключений в Xamarin. iOS

_Xamarin. iOS содержит новые события, которые помогают реагировать на исключения, особенно в машинном коде._

Как управляемый код, так и цель-C имеют поддержку исключений среды выполнения (предложения try/catch/finally).

Однако их реализации отличаются. Это означает, что библиотеки времени выполнения (среда выполнения Mono и библиотеки времени выполнения "цель-C") имеют проблемы, когда им приходится управлять исключениями, а затем запускать код, написанный на других языках.

В этом документе объясняются проблемы, которые могут возникнуть, и возможные решения.

Он также включает пример проекта, [маршалирование исключений](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling), который можно использовать для тестирования различных сценариев и их решений.

## <a name="problem"></a>Проблема

Проблема возникает при возникновении исключения и при обнаружении стека кадра, который не соответствует типу выданного исключения.

Типичный пример этого для Xamarin. iOS или Xamarin. Mac заключается в том, что машинный API создает исключение цели-C, а затем это исключение цели-C должно быть обработано, когда процесс очистки стека достигает управляемого фрейма.

Действие по умолчанию — не выполнять никаких действий. В приведенном выше примере это означает, что Вы раздаете управляемым кадрам стека целевой среды выполнения C. Это проблематично, поскольку среда выполнения цели-C не знает, как очищать управляемые фреймы. Например, он не будет выполнять `catch` никаких `finally` предложений OR в этом кадре.

### <a name="broken-code"></a>Неработающий код

Рассмотрим следующий пример кода:

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

Это приведет к вызову цели-C Нсинвалидаргументексцептион в машинном коде:

```
NSInvalidArgumentException *** setObjectForKey: key cannot be nil
```

И трассировка стека будет выглядеть примерно так:

```
0   CoreFoundation          __exceptionPreprocess + 194
1   libobjc.A.dylib         objc_exception_throw + 52
2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
3   libobjc.A.dylib         objc_msgSend + 102
4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()
```

Фреймы 0-3 — это машинные кадры, а в среде выполнения цели-C — _для_ очистки этих кадров. В частности, он будет выполнять любые предложения цели-C `@catch` или `@finally` .

Однако в стеке "цель-C" _не_ предусмотрена правильная очистка управляемых кадров (кадров 4-6), в которых кадры будут развернуты, но управляемая логика исключений не будет выполнена.

Это означает, что обычно невозможно перехватить эти исключения следующим образом:

```csharp
try {
    var dict = new NSMutableDictionary ();
    dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero);
} catch (Exception ex) {
    Console.WriteLine (ex);
} finally {
    Console.WriteLine ("finally");
}
```

Это связано с тем, что в процессе выполнения задания-C стека не всзнается управляемое `catch` предложение, и ни одно из них не будет `finally` выполнено.

Если приведенный выше пример _кода эффективен,_ это обусловлено тем, что цель-c имеет метод уведомления о необработанных исключениях цели-c, [`NSSetUncaughtExceptionHandler`][2] , используемых Xamarin. iOS и Xamarin. Mac, и в этот момент пытается преобразовать любые исключения цели-C в управляемые исключения.

## <a name="scenarios"></a>Сценарии

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>Сценарий 1. перехват исключений цели-C с помощью управляемого обработчика catch

В следующем сценарии можно перехватывать исключения цели-C с помощью управляемых `catch` обработчиков:

1. Выдается исключение цели-C.
2. Среда выполнения цели-C просматривает стек (но не выполняет его очистку), ищут собственный `@catch` обработчик, который может справиться с этим исключением.
3. Среда выполнения цели-C не находит `@catch` обработчики, вызывает `NSGetUncaughtExceptionHandler` и вызывает обработчик, установленный Xamarin. iOS/Xamarin. Mac.
4. Обработчик Xamarin. iOS/Xamarin. Mac преобразует исключение цели-C в управляемое исключение и выдает его. Так как среда выполнения цели-C не вывела стек (просмотрела его), текущий кадр будет таким же, как и исключение цели-C.

Еще одна проблема возникает, так как среда выполнения Mono не знает, как правильно выочищать кадры цели-C.

При вызове неперехваченного метода обратного вызова исключения C в Xamarin. iOS стек выглядит следующим образом:

```
 0 libxamarin-debug.dylib   exception_handler(exc=name: "NSInvalidArgumentException" - reason: "*** setObjectForKey: key cannot be nil")
 1 CoreFoundation           __handleUncaughtException + 809
 2 libobjc.A.dylib          _objc_terminate() + 100
 3 libc++abi.dylib          std::__terminate(void (*)()) + 14
 4 libc++abi.dylib          __cxa_throw + 122
 5 libobjc.A.dylib          objc_exception_throw + 337
 6 CoreFoundation           -[__NSDictionaryM setObject:forKey:] + 1015
 7 libxamarin-debug.dylib   xamarin_dyn_objc_msgSend + 102
 8 TestApp                  ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
 9 TestApp                  Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr) [0x00000]
10 TestApp                  ExceptionMarshaling.Exceptions.ThrowObjectiveCException () [0x00013]
```

Здесь единственными управляемыми кадрами являются фреймы 8-10, но управляемое исключение создается в кадре 0. Это означает, что среда выполнения Mono должна очищать машинные кадры 0-7, что вызывает проблему, эквивалентную описанной выше проблеме: Хотя среда выполнения Mono будет очищать машинные кадры, она не выполнит никаких `@catch` `@finally` предложений.

Пример кода:

```objc
-(id) setObject: (id) object forKey: (id) key
{
    @try {
        if (key == nil)
            [NSException raise: @"NSInvalidArgumentException"];
    } @finally {
        NSLog (@"This will not be executed");
    }
}
```

И `@finally` предложение не будет выполнено, так как среда выполнения Mono, которая расматывает этот кадр, не знает о ней.

Разновидность этого параметра заключается в создании управляемого исключения в управляемом коде, а затем при очистке в машинных кадрах для получения первого управляемого `catch` предложения:

```csharp
class AppDelegate : UIApplicationDelegate {
    public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
    {
        throw new Exception ("An exception");
    }
    static void Main (string [] args)
    {
        try {
            UIApplication.Main (args, null, typeof (AppDelegate));
        } catch (Exception ex) {
            Console.WriteLine ("Managed exception caught.");
        }
    }
}
```

Управляемый `UIApplication:Main` метод будет вызывать собственный `UIApplicationMain` метод, а затем iOS будет выполнять большое количество машинного кода перед вызовом управляемого `AppDelegate:FinishedLaunching` метода, при этом при возникновении управляемого исключения остается множество собственных кадров в стеке:

```
 0: TestApp                 ExceptionMarshaling.IOS.AppDelegate:FinishedLaunching (UIKit.UIApplication,Foundation.NSDictionary)
 1: TestApp                 (wrapper runtime-invoke) <Module>:runtime_invoke_bool__this___object_object (object,intptr,intptr,intptr) 
 2: libmonosgen-2.0.dylib   mono_jit_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 3: libmonosgen-2.0.dylib   do_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 4: libmonosgen-2.0.dylib   mono_runtime_invoke [inlined] mono_runtime_invoke_checked(method=<unavailable>, obj=<unavailable>, params=<unavailable>, error=0xbff45758)
 5: libmonosgen-2.0.dylib   mono_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>)
 6: libxamarin-debug.dylib  xamarin_invoke_trampoline(type=<unavailable>, self=<unavailable>, sel="application:didFinishLaunchingWithOptions:", iterator=<unavailable>), context=<unavailable>)
 7: libxamarin-debug.dylib  xamarin_arch_trampoline(state=0xbff45ad4)
 8: libxamarin-debug.dylib  xamarin_i386_common_trampoline
 9: UIKit                   -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:]
10: UIKit                   -[UIApplication _callInitializationDelegatesForMainScene:transitionContext:]
11: UIKit                   -[UIApplication _runWithMainScene:transitionContext:completion:]
12: UIKit                   __84-[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]_block_invoke.3124
13: UIKit                   -[UIApplication workspaceDidEndTransaction:]
14: FrontBoardServices      __37-[FBSWorkspace clientEndTransaction:]_block_invoke_2
15: FrontBoardServices      __40-[FBSWorkspace _performDelegateCallOut:]_block_invoke
16: FrontBoardServices      __FBSSERIALQUEUE_IS_CALLING_OUT_TO_A_BLOCK__
17: FrontBoardServices      -[FBSSerialQueue _performNext]
18: FrontBoardServices      -[FBSSerialQueue _performNextFromRunLoopSource]
19: FrontBoardServices      FBSSerialQueueRunLoopSourceHandler
20: CoreFoundation          __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
21: CoreFoundation          __CFRunLoopDoSources0
22: CoreFoundation          __CFRunLoopRun
23: CoreFoundation          CFRunLoopRunSpecific
24: CoreFoundation          CFRunLoopRunInMode
25: UIKit                   -[UIApplication _run]
26: UIKit                   UIApplicationMain
27: TestApp                 (wrapper managed-to-native) UIKit.UIApplication:UIApplicationMain (int,string[],intptr,intptr)
28: TestApp                 UIKit.UIApplication:Main (string[],intptr,intptr)
29: TestApp                 UIKit.UIApplication:Main (string[],string,string)
30: TestApp                 ExceptionMarshaling.IOS.Application:Main (string[])
```

Фреймы 0-1 и 27-30 управляются, тогда как все они находятся в машинном виде. Если моно выполняет очистку этих кадров, ни одно из них не `@catch` `@finally` будет выполняться.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>Сценарий 2. не удается перехватить исключения цели-C

В следующем _сценарии невозможно_ перехватить исключения цели-c с помощью управляемых `catch` обработчиков, так как исключение цели-c было обработано другим способом:

1. Выдается исключение цели-C.
2. Среда выполнения цели-C просматривает стек (но не выполняет его очистку), ищут собственный `@catch` обработчик, который может справиться с этим исключением.
3. Среда выполнения цели-C находит `@catch` обработчик, очищает стек и начинает выполнять `@catch` обработчик.

Этот сценарий обычно находится в приложениях Xamarin. iOS, поскольку в основном потоке обычно используется следующий код:

``` objective-c
void UIApplicationMain ()
{
    @try {
        while (true) {
            ExecuteRunLoop ();
        }
    } @catch (NSException *ex) {
        NSLog (@"An unhandled exception occured: %@", exc);
        abort ();
    }
}

```

Это означает, что в основном потоке никогда не будет действительно необработанного исключения цели-C, поэтому наш обратный вызов, который преобразует исключения цели-C в управляемые исключения, никогда не вызывается.

Это также весьма распространено при отладке приложений Xamarin. Mac в более ранней версии macOS, чем в Xamarin. Mac, так как проверка большинства объектов пользовательского интерфейса в отладчике попытается получить свойства, соответствующие селекторам, которые не существуют на выполняющейся платформе (так как Xamarin. Mac включает поддержку более поздней версии macOS). При вызове таких селекторов создается исключение `NSInvalidArgumentException` ("Нераспознанный селектор, отправленный в..."), что в итоге приводит к сбою процесса.

В итоге, наличие кадров среды выполнения цели или среды выполнения Mono, которые не запрограммированы для обработки, может привести к неопределенному поведению, например к сбоям, утечкам памяти и другим типам непредсказуемых (MIS) поведений.

## <a name="solution"></a>Решение

В Xamarin. iOS 10 и Xamarin. Mac 2,10 мы добавили поддержку перехвата управляемых и целевых исключений на любой управляемой границе, а также для преобразования этого исключения в другой тип.

В псевдо-коде он выглядит примерно так:

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

Метод P/Invoke для objc_msgSend перехватывается, а вместо него вызывается:

``` objective-c
void
xamarin_dyn_objc_msgSend (id obj, SEL sel)
{
    @try {
        objc_msgSend (obj, sel);
    } @catch (NSException *ex) {
        convert_to_and_throw_managed_exception (ex);
    }
}
```

И что-то похожее для обращения к обратным случаям (маршалирование управляемых исключений в исключения цели-C).

Перехват исключений в управляемой границах не всегда является экономичным, поэтому по умолчанию он не поддерживается.

- Xamarin. iOS/tvOS: перехват исключений цели-C включен в симуляторе.
- Xamarin. watchOS: перехват применяется во всех случаях, так как предоставление управляемым кадрам для очистки среды выполнения, выполняемой задачей-C, приводит к путанице в сборщике мусора и либо в случае зависания, либо при сбое.
- Xamarin. Mac: перехват исключений цели-C включен для отладочных сборок.

В разделе " [Флаги времени сборки](#build_time_flags) " объясняется, как включить перехват, если он не включен по умолчанию.

## <a name="events"></a>События

После перехвата исключения возникают два новых события: `Runtime.MarshalManagedException` и `Runtime.MarshalObjectiveCException` .

Обоим событиям передается `EventArgs` объект, содержащий исходное исключение, которое было вызвано ( `Exception` свойство), и `ExceptionMode` свойство для определения способа маршалирования исключения.

`ExceptionMode`Свойство можно изменить в обработчике событий, чтобы изменить поведение в соответствии с любой пользовательской обработкой, выполняемой в обработчике. Одним из примеров может быть прерывание процесса при возникновении определенного исключения.

Изменение `ExceptionMode` свойства применяется к отдельному событию, оно не влияет на исключения, перехваченные в будущем.

Доступны следующие режимы.

- `Default`. Значение по умолчанию зависит от платформы. Это происходит `ThrowObjectiveCException` , если сборщик мусора находится в режиме совместной работой (watchOS) и `UnwindNativeCode` в противном случае (iOS/WatchOS/macOS). Значение по умолчанию может измениться в будущем.
- `UnwindNativeCode`: Это предыдущее (неопределенное) поведение. Это недоступно при использовании GC в режиме совместной работы (который является единственным параметром в watchOS; таким образом, этот параметр не является допустимым для watchOS), но является параметром по умолчанию для всех других платформ.
- `ThrowObjectiveCException`: Преобразуйте управляемое исключение в исключение цели-C и вызовите исключение цели-C. Это значение по умолчанию для watchOS.
- `Abort`: Прервать процесс.
- `Disable`: Отключает перехват исключений, поэтому не имеет смысла задавать это значение в обработчике событий, но после возникновения события оно слишком поздно для отключения. В любом случае, если задано, оно будет вести себя как `UnwindNativeCode` .

Для маршалирования исключений цели-C в управляемый код доступны следующие режимы.

- `Default`. Значение по умолчанию зависит от платформы. Это происходит `ThrowManagedException` , если сборщик мусора находится в режиме совместной работой (watchOS) и `UnwindManagedCode` в противном случае (iOS/TvOS/macOS). Значение по умолчанию может измениться в будущем.
- `UnwindManagedCode`: Это предыдущее (неопределенное) поведение. Это недоступно при использовании GC в режиме совместной работы (который является единственным допустимым режимом GC в watchOS, поэтому этот параметр не является допустимым для watchOS), но используется по умолчанию для всех остальных платформ.
- `ThrowManagedException`: Преобразуйте исключение цели-C в управляемое исключение и вызовите управляемое исключение. Это значение по умолчанию для watchOS.
- `Abort`: Прервать процесс.
- `Disable`:D поддерживает перехват исключений, поэтому не имеет смысла задавать это значение в обработчике событий, но после возникновения события оно слишком поздно для его отключения. В любом случае, если оно задано, процесс будет прерван.

Таким образом, чтобы увидеть каждый раз при маршалировании исключения, можно сделать следующее:

``` csharp
Runtime.MarshalManagedException += (object sender, MarshalManagedExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling managed exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
    
};
Runtime.MarshalObjectiveCException += (object sender, MarshalObjectiveCExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling Objective-C exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
};
```

<a name="build_time_flags"></a>

## <a name="build-time-flags"></a>Флаги времени сборки

Можно передать следующие параметры в **mtouch** (для приложений Xamarin. IOS) и **MMP** (для приложений Xamarin. Mac), которые будут определять, включено ли перехват исключений, и задать выполняемое по умолчанию действие:

- `--marshal-managed-exceptions=`
  - `default`
  - `unwindnativecode`
  - `throwobjectivecexception`
  - `abort`
  - `disable`

- `--marshal-objectivec-exceptions=`
  - `default`
  - `unwindmanagedcode`
  - `throwmanagedexception`
  - `abort`
  - `disable`

За исключением `disable` , эти значения идентичны `ExceptionMode` значениям, передаваемым `MarshalManagedException` `MarshalObjectiveCException` событиям и.

`disable`Параметр в _основном_ отключает перехват, за исключением того, что мы по-прежнему перехватывают исключения, когда не добавляется никаких дополнительных затрат на выполнение. События маршалирования по-прежнему вызываются для этих исключений, а режим по умолчанию — режим по умолчанию для выполняемой платформы.

## <a name="limitations"></a>Ограничения

Мы перехватывать вызовы P/Invoke только для `objc_msgSend` семейства функций при попытке перехвата исключений цели-C. Это означает, что P/Invoke для другой функции C, которая затем создает исключения цели-C, по-прежнему будет работать со старым и неопределенным поведением (это может быть повышено в будущем).

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc

## <a name="related-links"></a>Связанные ссылки

- [Маршалирование исключений (пример)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
