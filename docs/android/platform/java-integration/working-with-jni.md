---
title: Работа с JNI и Xamarin.Android
description: Xamarin.Android позволяет создавать приложения Android на языке C# вместо Java. В Xamarin.Android доступно несколько сборок, которые предоставляют привязки для библиотек Java, включая Mono.Android.dll и Mono.Android.GoogleMaps.dll. Однако привязки предоставляются не для всех библиотек Java, а имеющиеся привязки могут не поддерживать все типы и члены Java. Для использования несвязанных типов и членов Java можно использовать собственный интерфейс Java (JNI). В этой статье показано, как использовать JNI для взаимодействия с типами и членами Java из приложений Xamarin.Android.
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 00c9c2e9f39943960d35c30602935ed109639cf4
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567740"
---
# <a name="working-with-jni-and-xamarinandroid"></a>Работа с JNI и Xamarin.Android

_Xamarin.Android позволяет создавать приложения Android на языке C# вместо Java. В Xamarin.Android доступно несколько сборок, которые предоставляют привязки для библиотек Java, включая Mono.Android.dll и Mono.Android.GoogleMaps.dll. Однако привязки предоставляются не для всех библиотек Java, а имеющиеся привязки могут не поддерживать все типы и члены Java. Для использования несвязанных типов и членов Java можно использовать собственный интерфейс Java (JNI). В этой статье показано, как использовать JNI для взаимодействия с типами и членами Java из приложений Xamarin.Android._

## <a name="overview"></a>Обзор

Создать управляемую вызываемую оболочку (MCW) для вызова кода Java не всегда необходимо или возможно. Во многих случаях вполне приемлемо и удобно использовать внутренний JNI для однократного использования несвязанных членов Java. Как правило, проще использовать JNI для вызова одного метода в классе Java, чем для создания всей привязки JAR.

В Xamarin.Android доступна сборка `Mono.Android.dll`, которая предоставляет привязку для библиотеки `android.jar` Android. Типы и члены, отсутствующие в `Mono.Android.dll`, и типы, отсутствующие в `android.jar`, можно привязывать вручную. Для привязки типов и членов Java можно использовать собственный интерфейс **Java** (**JNI**), позволяющий искать типы, читать и записывать значения полей, а также вызывать методы.

Принцип работы API JNI в Xamarin.Android напоминает API `System.Reflection` в .NET: он позволяет выполнять поиск типов и членов по имени, читать и записывать значения полей, вызывать методы и многое другое. Вы можете использовать JNI и настраиваемый атрибут `Android.Runtime.RegisterAttribute`, чтобы объявить виртуальные методы, которые можно привязать для поддержки переопределения. Можно привязать интерфейсы, чтобы их можно было реализовать в C#.

В этом документе объясняется:

- как JNI ссылается на типы;
- как искать, читать и записывать значения полей;
- как искать и вызывать методы;
- как предоставить виртуальные методы, чтобы обеспечить переопределение из управляемого кода;
- как предоставлять интерфейсы.

## <a name="requirements"></a>Требования

JNI, предоставляемый в [пространстве имен Android.Runtime.JNIEnv](xref:Android.Runtime.JNIEnv), доступен в каждой версии Xamarin.Android.
Для привязки типов и интерфейсов Java необходимо использовать Xamarin.Android 4.0 или более поздней версии.

## <a name="managed-callable-wrappers"></a>Управляемые вызываемые оболочки

**Управляемая вызываемая оболочка** (**MCW**) — это *привязка* для класса или интерфейса Java, в которую переносятся все механизмы JNI. Это позволяет скрыть от кода C# клиента всю сложность базовой структуры JNI. Большую часть `Mono.Android.dll` составляют управляемые вызываемые оболочки.

Управляемые вызываемые оболочки служат двум целям:

1. инкапсулировать использование JNI, чтобы код клиента не знал о сложности базовой инфраструктуры;
1. обеспечить возможность создания подкласса типов Java и реализовать интерфейсы Java.

Первая цель служит исключительно для удобства и инкапсуляции сложности, чтобы предоставить потребителям простой управляемый набор классов. Для этого необходимо использовать различные члены [JNIEnv](xref:Android.Runtime.JNIEnv), как описано далее в этой статье. Помните, что использовать управляемые вызываемые оболочки вовсе не обязательно. Для однократного использования несвязанных членов Java вполне достаточно встроенного JNI &ndash;. Управляемые вызываемые оболочки нужны для создания подклассов и реализации интерфейсов.

## <a name="android-callable-wrappers"></a>Вызываемые программы-оболочки Android

Вызываемые оболочки Android (ACW) требуются, когда среде выполнения Android (ART) нужно вызвать управляемый код. Эти оболочки являются обязательными, так как невозможно зарегистрировать классы во время выполнения.
(В частности, функция JNI [DefineClass](https://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) не поддерживается средой выполнения Android. Таким образом вызываемые оболочки Android компенсируют отсутствие поддержки регистрации типа во время выполнения.)

Когда код Android должен выполнить виртуальный метод или метод интерфейса, который переопределен или реализован в управляемом коде, среда Xamarin.Android должна предоставить замещающий тип Java, чтобы этот метод был отправлен в соответствующий управляемый тип. Данные замещающие типы Java — это код Java, имеющий тот же базовый класс и список интерфейсов Java, что и управляемый тип, реализующий те же конструкторы и объявляющий любые переопределенные базовые классы и методы интерфейса.

Вызываемые оболочки Android создаются программой **monodroid.exe** во время [процесса сборки](~/android/deploy-test/building-apps/build-process.md): они создаются для всех типов, которые (прямо или косвенно) наследуют [Java.Lang.Object](xref:Java.Lang.Object).

### <a name="implementing-interfaces"></a>Реализация интерфейсов

Иногда может потребоваться реализовать интерфейс Android, например [Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks).

Все классы и интерфейсы Android расширяют интерфейс [Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject), поэтому все типы Android должны реализовать `IJavaObject`.
Платформа Xamarin.Android использует этот факт &ndash; в ней для предоставления Android замещающего типа Java (вызываемая оболочка Android) для данного управляемого типа используется `IJavaObject`. Так как **monodroid.exe** ищет только подклассы `Java.Lang.Object` (которые должны реализовывать `IJavaObject`), вывод подкласса `Java.Lang.Object` позволяет реализовать интерфейсы в управляемом коде. Пример:

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {
    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig) {
        // implementation goes here...
    }
    public void OnLowMemory () {
        // implementation goes here...
    }
}
```

### <a name="implementation-details"></a>Сведения о реализации

*Оставшаяся часть этой страницы содержит сведения о реализации, которые могут быть изменены без предварительного уведомления* (и предоставляются здесь только потому, что разработчикам может быть интересно, что происходит "внутри").

Например, рассмотрим приведенный ниже исходный код C#.

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

Программа **mandroid.exe** создаст следующую вызываемую оболочку Android.

```java
package mono.samples.helloWorld;

public class HelloAndroid extends android.app.Activity {
    static final String __md_methods;
    static {
        __md_methods =
            "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" +
            "";
        mono.android.Runtime.register (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                HelloAndroid.class,
                __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                "", this, new java.lang.Object[] { });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Обратите внимание на то, что базовый класс сохраняется, и для каждого метода, переопределяемого в управляемом коде, предоставляются объявления собственных методов.

### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute и ExportFieldAttribute

Как правило, Xamarin.Android автоматически создает код Java, который содержит ACW. Для этого используются имена классов и методов, если класс является производным от класса Java и переопределяет существующие методы Java. Однако в некоторых сценариях создания кода не достаточно, как описано ниже.

- Android поддерживает имена действий в атрибутах XML макета, например атрибут XML [Android:OnClick](xref:Android.Views.View.IOnClickListener.OnClick*). Если оно указано, экземпляр неструктурированного элемента View пытается найти метод Java.

- Для интерфейса [java.io.Serializable](https://developer.android.com/reference/java/io/Serializable.html) требуются методы `readObject` и `writeObject`. Так как они не являются членами этого интерфейса, соответствующая управляемая реализация не предоставляет эти методы коду Java.

- Интерфейс [android.os.Parcelable](xref:Android.OS.Parcelable) ожидает, что у класса реализации должно быть статическое поле `CREATOR` типа `Parcelable.Creator`. Для созданного кода Java требуется явно заданное поле. В нашем стандартном сценарии невозможно вывести значение поля в коде Java из управляемого кода.

Так как создание кода не обеспечивает решения для создания произвольных методов Java с произвольными именами, начиная с Xamarin.Android 4.2 были введены атрибуты [ExportAttribute](xref:Java.Interop.ExportAttribute) и [ExportFieldAttribute](xref:Java.Interop.ExportFieldAttribute), которые являются решением для приведенных выше сценариев. Оба атрибута находятся в пространстве имен `Java.Interop`.

- `ExportAttribute` &ndash; указывает имя метода и ожидаемые типы исключений (для явного порождения исключений в Java). При использовании в методе экспортируется метод Java, который создает код подготовки к отправке соответствующего вызова JNI в управляемом методе. Его можно использовать с `android:onClick` и `java.io.Serializable`.

- `ExportFieldAttribute` &ndash; определяет имя поля. Он находится в методе, который работает как инициализатор поля. Его можно использовать с `android.os.Parcelable`.

#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>Устранение неполадок ExportAttribute и ExportFieldAttribute

- Сбой упаковки из-за отсутствия **Mono.Android.Export.dll** &ndash; если вы использовали `ExportAttribute` или `ExportFieldAttribute` в каких-либо методах в коде или зависимых библиотеках, необходимо добавить **Mono.Android.Export.dll**. Эта сборка изолирована для поддержки кода обратного вызова из Java. Она не связана со сборкой **Mono.Android.dll**, так как увеличивает размер приложения.

- В сборке с конфигурацией "Выпуск" `MissingMethodException` порождается для методов экспорта &ndash; В сборке с конфигурацией "Выпуск" `MissingMethodException` порождается для методов экспорта. (Эта проблема исправлена в последней версии Xamarin.Android.)

### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` и `ExportFieldAttribute` предоставляют функциональные возможности, которые может использовать код времени выполнения Java. Этот код обращается к управляемому коду через созданные методы JNI, управляемые этими атрибутами. В результате управляемый метод не привязывается к какому-либо существующему методу Java. Следовательно, метод Java создается на основе сигнатуры управляемого метода.

Однако этот вариант не является решающим фактором. Более важно то, что это справедливо для некоторых расширенных сопоставлений управляемых типов и типов Java, в том числе:

- InputStream
- OutputStream
- XmlPullParser
- XmlResourceParser

Если таким типам необходимы экспортированные методы, нужно использовать `ExportParameterAttribute`, чтобы явно задать тип соответствующего параметра или возвращаемого значения.

### <a name="annotation-attribute"></a>Атрибут заметки

В Xamarin.Android 4.2 мы преобразовали типы реализации `IAnnotation` в атрибуты (System.Attribute) и добавили поддержку создания заметок в оболочках Java.

Это означает следующие изменения направления обработки.

- Генератор привязок создает `Java.Lang.DeprecatedAttribute` из `java.Lang.Deprecated` (в то время как он должен быть `[Obsolete]` в управляемом коде).

- Это не означает, что существующий класс `Java.Lang.Deprecated` исчезнет. Эти объекты на основе Java можно по-прежнему использовать как обычные объекты Java (если это имеет место). Будут доступны классы `Deprecated` и `DeprecatedAttribute`.

- Класс `Java.Lang.DeprecatedAttribute` помечен как `[Annotation]`. Если имеется настраиваемый атрибут, унаследованный от атрибута `[Annotation]`, задача msbuild создаст заметку Java для этого настраиваемого атрибута (@Deprecated) в вызываемой оболочке Android (ACW).

- Заметки можно создавать для классов, методов и экспортируемых полей (которые являются методом в управляемом коде).

Если содержащий заметку класс (сам класс с заметкой или класс, содержащий члены с заметками) не зарегистрирован, то вообще весь источник класса Java не создается, включая заметки. Для методов можно указать `ExportAttribute`, чтобы явно создать метод и добавить к нему заметку. Кроме того, эта функция не предназначена для создания определения класса заметки Java. Иными словами, если нужно определить настраиваемый управляемый атрибут для определенной заметки, то следует добавить еще одну библиотеку в формате JAR, содержащую соответствующий класс заметки Java. Недостаточно добавить только исходный файл Java, определяющий тип заметки. Компилятор Java работает не так, как **apt**.

В связи с этим применяются дополнительные ограничения.

- В этом процессе преобразования пока еще не учитывается заметка `@Target` в типе заметки.

- Атрибуты для свойства не работают. Вместо этого используйте атрибуты для метода получения или задания свойства.

## <a name="class-binding"></a>Привязка класса

Привязка класса означает написание управляемой вызываемой оболочки для упрощения вызова базового типа Java.

Привязка виртуальных и абстрактных методов для поддержки переопределения из C# требует наличия Xamarin.Android 4.0. Однако любая версия Xamarin.Android позволяет привязывать невиртуальные методы, статические методы или виртуальные методы без поддержки переопределения.

Обычно привязка содержит следующие элементы:

- [обработчик JNI для привязываемого типа Java](#_Looking_up_Java_Types);

- [идентификаторы полей и свойства JNI для каждого привязанного поля](#_Instance_Fields);

- [идентификаторы методов и методы JNI для каждого привязанного метода](#_Instance_Methods).

- Если требуется создать подкласс, объявление типа должно содержать настраиваемый атрибут [RegisterAttribute](xref:Android.Runtime.RegisterAttribute) и [RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) со значением `true`.

### <a name="declaring-type-handle"></a>Дескриптор объявляющего типа

Для методов поиска полей и методов требуется ссылка на объект, указывающая их объявляющий тип. По соглашению это значение хранится в поле `class_ref`.

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Дополнительные сведения о маркере `CLASS` см. в разделе [Ссылки на типы JNI](#_JNI_Type_References).

### <a name="binding-fields"></a>Привязка полей

Поля Java предоставляются как свойства C#. Например, поле Java [Java.lang.System.in](https://developer.android.com/reference/java/lang/System.html#in) привязано к свойству C# [Java.lang.JavaSystem.in](xref:Java.Lang.JavaSystem.In).
Более того, так как JNI различает статические поля и поля экземпляров, для реализации свойств используются различные методы.

Для привязки полей используются три набора методов.

1. Метод *получения идентификатора поля*. Метод *получения идентификатора поля* отвечает за возвращение дескриптора поля, который будет использоваться методами *получения значения поля* и *задания значения поля*. Для получения идентификатора поля необходимо знать объявляющий тип, имя поля и [сигнатуру типа JNI](#JNI_Type_Signatures).

1. Методы *получения значения поля*. Эти методы используют дескриптор поля и отвечают за чтение значения поля из Java.
    Используемый метод зависит от типа поля.

1. Методы *задания значения поля*. Эти методы используют дескриптор поля и отвечают за запись значения поля в Java. Используемый метод зависит от типа поля.

Для [статических полей](#_Static_Fields) используются методы [JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), `JNIEnv.GetStatic*Field` и [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*).

 Для [полей экземпляров](#_Instance_Fields) используются методы [JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*), `JNIEnv.Get*Field` и [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*).

Например, статическое свойство `JavaSystem.In` можно реализовать следующим образом.

```csharp
static IntPtr in_jfieldID;
public static System.IO.Stream In
{
    get {
        if (in_jfieldId == IntPtr.Zero)
            in_jfieldId = JNIEnv.GetStaticFieldID (class_ref, "in", "Ljava/io/InputStream;");
        IntPtr __ret = JNIEnv.GetStaticObjectField (class_ref, in_jfieldId);
        return InputStreamInvoker.FromJniHandle (__ret, JniHandleOwnership.TransferLocalRef);
    }
}
```

Примечание. Мы используем [InputStreamInvoker.FromJniHandle](xref:Android.Runtime.InputStreamInvoker.FromJniHandle*), чтобы преобразовать ссылку JNI в экземпляр `System.IO.Stream`, и мы используем `JniHandleOwnership.TransferLocalRef`, так как [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) возвращает локальную ссылку.

Многие типы [Android.Runtime](xref:Android.Runtime) имеют методы `FromJniHandle`, которые преобразовывают ссылку JNI в нужный тип.

### <a name="method-binding"></a>Привязка метода

Методы Java предоставляются как методы и свойства C#. Например, метод Java [java.lang.Runtime.runFinalizersOnExit](https://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) привязывается к методу [Java.Lang.Runtime.RunFinalizersOnExit](xref:Java.Lang.Runtime.RunFinalizersOnExit*), а метод [java.lang.Object.getClass](https://developer.android.com/reference/java/lang/Object.html#getClass) — к свойству [Java.Lang.Object.Class](xref:Java.Lang.Object.Class).

Вызов метода происходит в два этапа.

1. Вызывается метод *получения идентификатора метода* для вызываемого метода. Метод *получения идентификатора метода* отвечает за возвращение дескриптора метода, который будет использоваться методами вызова этого метода. Для получения идентификатора метода необходимо знать объявляющий тип, имя метода и [сигнатуру типа JNI](#JNI_Type_Signatures) метода.

1. Вызовите метод.

Как и в случае с полями, методы, используемые для получения идентификатора метода и вызова метода, отличаются для статических методов и методов экземпляра.

Для поиска идентификаторов [статических методов](#_Static_Methods_1) используется [JNIEnv.GetStaticMethodID()](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), а для их вызова используется семейство методов `JNIEnv.CallStatic*Method`.

Для поиска идентификаторов [методов экземпляров](#_Instance_Methods) используется [JNIEnv.GetMethodID()](xref:Android.Runtime.JNIEnv.GetMethodID*), а для их вызова используются семейства методов `JNIEnv.Call*Method` и `JNIEnv.CallNonvirtual*Method`.

Потенциально привязка метода — это не просто его вызов. Привязка метода также дает возможность переопределить метод (для абстрактных и неконечных методов) или реализовать его (для методов интерфейса). В разделе [Поддержка наследования и интерфейсы](#_Supporting_Inheritance,_Interfaces_1) описываются сложности поддержки виртуальных методов и методов интерфейса.

<a name="_Static_Methods_1"></a>

#### <a name="static-methods"></a>Статические методы

Привязка статического метода включает в себя использование `JNIEnv.GetStaticMethodID` для получения дескриптора метода и выполнение соответствующего метода `JNIEnv.CallStatic*Method` в зависимости от типа возвращаемого значения метода. Ниже приведен пример привязки для метода [Runtime.getRuntime](https://developer.android.com/reference/java/lang/Runtime.html#getRuntime()).

```csharp
static IntPtr id_getRuntime;

[Register ("getRuntime", "()Ljava/lang/Runtime;", "")]
public static Java.Lang.Runtime GetRuntime ()
{
    if (id_getRuntime == IntPtr.Zero)
        id_getRuntime = JNIEnv.GetStaticMethodID (class_ref,
                "getRuntime", "()Ljava/lang/Runtime;");

    return Java.Lang.Object.GetObject<Java.Lang.Runtime> (
            JNIEnv.CallStaticObjectMethod  (class_ref, id_getRuntime),
            JniHandleOwnership.TransferLocalRef);
}
```

Обратите внимание на то, что дескриптор метода сохраняется в статическом поле `id_getRuntime`. Это сделано в целях оптимизации производительность, чтобы при каждом вызове не требовалось искать дескриптор метода. Не обязательно кэшировать дескриптор метода таким образом. После получения дескриптора метода для вызова метода используется [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*). `JNIEnv.CallStaticObjectMethod` возвращает `IntPtr`, который содержит дескриптор возвращенного экземпляра Java.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*) используется для преобразования дескриптора Java в экземпляр объекта со строгой типизацией.

#### <a name="non-virtual-instance-method-binding"></a>Привязка невиртуального метода экземпляра

Привязка метода экземпляра `final` или метода экземпляра, не требующего переопределения, включает в себя использование `JNIEnv.GetMethodID` для получения дескриптора метода и выполнение соответствующего метода `JNIEnv.Call*Method` в зависимости от типа возвращаемого значения метода. Ниже приведен пример привязки для свойства `Object.Class`.

```csharp
static IntPtr id_getClass;
public Java.Lang.Class Class {
    get {
        if (id_getClass == IntPtr.Zero)
            id_getClass = JNIEnv.GetMethodID (class_ref, "getClass", "()Ljava/lang/Class;");
        return Java.Lang.Object.GetObject<Java.Lang.Class> (
                JNIEnv.CallObjectMethod (Handle, id_getClass),
                JniHandleOwnership.TransferLocalRef);
    }
}
```

Обратите внимание на то, что дескриптор метода сохраняется в статическом поле `id_getClass`.
Это сделано в целях оптимизации производительность, чтобы при каждом вызове не требовалось искать дескриптор метода. Не обязательно кэшировать дескриптор метода таким образом. После получения дескриптора метода для вызова метода используется [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*). `JNIEnv.CallStaticObjectMethod` возвращает `IntPtr`, который содержит дескриптор возвращенного экземпляра Java.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object.GetObject*) используется для преобразования дескриптора Java в экземпляр объекта со строгой типизацией.

### <a name="binding-constructors"></a>Привязка конструкторов

Конструкторы — это методы Java с именем `"<init>"`. Как и в случае с методами экземпляров Java, для поиска дескриптора конструктора используется `JNIEnv.GetMethodID`. В отличие от методов Java, методы [JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*) используются для вызова дескриптора метода конструктора. Возвращаемое значение `JNIEnv.NewObject` является локальной ссылкой на JNI.

```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Как правило, привязка класса выводит подкласс [Java.Lang.Object](xref:Java.Lang.Object).
При выводе подкласса `Java.Lang.Object` задействуется дополнительная семантика: экземпляр `Java.Lang.Object` поддерживает глобальную ссылку на экземпляр Java с помощью свойства `Java.Lang.Object.Handle`.

1. Конструктор `Java.Lang.Object` по умолчанию выделит экземпляр Java.

1. Если тип имеет значение `RegisterAttribute`, а `RegisterAttribute.DoNotGenerateAcw` — `true`, то создается экземпляр типа `RegisterAttribute.Name` с помощью конструктора по умолчанию.

1. В противном случае с помощью конструктора по умолчанию создается экземпляр [вызываемой оболочки Android](~/android/platform/java-integration/android-callable-wrappers.md) (ACW), соответствующий `this.GetType`. Вызываемые оболочки Android создаются во время создания пакета для каждого подкласса `Java.Lang.Object`, для которого атрибуту `RegisterAttribute.DoNotGenerateAcw` не присвоено значение `true`.

Это ожидаемая семантика для типов, которые не являются привязками класса: при создании экземпляра `Mono.Samples.HelloWorld.HelloAndroid` C# должен создаваться экземпляр `mono.samples.helloworld.HelloAndroid` Java, который является сформированной вызываемой оболочкой Android.

Для привязок классов это поведение может быть корректным, если тип Java содержит конструктор по умолчанию и (или) не требуется вызывать другие конструкторы. В противном случае необходимо предоставить конструктор, который выполняет следующие действия.

1. Вызывает [Java.Lang.Object(IntPtr, JniHandleOwnership)](xref:Java.Lang.Object#ctor*) вместо конструктора по умолчанию `Java.Lang.Object`. Это необходимо, чтобы не создавать новый экземпляр Java.

1. Проверяет значение [Java.Lang.Object.Handle](xref:Java.Lang.Object.Handle) перед созданием экземпляров Java. Свойство `Object.Handle` будет иметь значение, отличное от `IntPtr.Zero`, если вызываемая оболочка Android была создана в коде Java, а создаваемая привязка класса должна содержать сформированный экземпляр вызываемой оболочки Android. Например, когда Android создает экземпляр `mono.samples.helloworld.HelloAndroid`, сначала создается вызываемая оболочка Android, а затем конструктор Java `HelloAndroid` создает экземпляр соответствующего типа `Mono.Samples.HelloWorld.HelloAndroid`, предварительно указав в свойстве `Object.Handle` экземпляр Java.

1. Если текущий тип среды выполнения отличается от объявляющего типа, необходимо создать экземпляр соответствующей вызываемой оболочки Android и использовать [Object.SetHandle](xref:Java.Lang.Object.SetHandle*) для хранения дескриптора, возвращаемого [JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*).

1. Если текущий тип среды выполнения совпадает с объявляющим типом, следует вызвать конструктор Java и использовать [Object.SetHandle](xref:Java.Lang.Object.SetHandle*) для хранения дескриптора, возвращаемого `JNIEnv.NewInstance`.

Например, рассмотрим конструктор [java.lang.Integer(int)](https://developer.android.com/reference/java/lang/Integer.html#Integer(int)). Вот как он привязан.

```csharp
// Cache the constructor's method handle for later use
static IntPtr id_ctor_I;

// Need [Register] for subclassing
// RegisterAttribute.Name is always ".ctor"
// RegisterAttribute.Signature is tye JNI type signature of constructor
// RegisterAttribute.Connector is ignored; use ""
[Register (".ctor", "(I)V", "")]
public Integer (int value)
    // 1. Prevent Object default constructor execution
    : base (IntPtr.Zero, JniHandleOwnership.DoNotTransfer)
{
    // 2. Don't allocate Java instance if already allocated
    if (Handle != IntPtr.Zero)
        return;

    // 3. Derived type? Create Android Callable Wrapper
    if (GetType () != typeof (Integer)) {
        SetHandle (
                Android.Runtime.JNIEnv.CreateInstance (GetType (), "(I)V", new JValue (value)),
                JniHandleOwnership.TransferLocalRef);
        return;
    }

    // 4. Declaring type: lookup &amp; cache method id...
    if (id_ctor_I == IntPtr.Zero)
        id_ctor_I = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
    // ...then create the Java instance and store
    SetHandle (
            JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value)),
            JniHandleOwnership.TransferLocalRef);
}
```

Методы [JNIEnv.CreateInstance](xref:Android.Runtime.JNIEnv.CreateInstance*) являются вспомогательными для выполнения `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject` и `JNIEnv.DeleteGlobalReference` для значения, возвращаемого из `JNIEnv.FindClass`. См. подробные сведения в следующем подразделе.

<a name="_Supporting_Inheritance,_Interfaces_1"></a>

### <a name="supporting-inheritance-interfaces"></a>Поддержка наследования и интерфейсы

Для вывода подкласса типа Java или реализации интерфейса Java требуется создавать [вызываемые оболочки Android](~/android/platform/java-integration/android-callable-wrappers.md) (ACW), которые формируются для каждого подкласса `Java.Lang.Object` в процессе упаковки. Созданием ACW можно управлять с помощью настраиваемого атрибута [Android.Runtime.RegisterAttribute](xref:Android.Runtime.RegisterAttribute).

Для типов C# `[Register]` конструктору настраиваемого атрибута требуется один аргумент: [упрощенная ссылка на тип JNI](#_Simplified_Type_References_1) для соответствующего типа Java. Это позволяет предоставлять разные имена для Java и C#.

До выпуска Xamarin.Android 4.0 настраиваемый атрибут `[Register]` не позволял создавать псевдонимы для существующих типов Java. Это обусловлено тем, что процесс создания ACW формирует вызываемые оболочки для каждого обнаруженного подкласса `Java.Lang.Object`.

В Xamarin.Android 4.0 введено свойство [RegisterAttribute.DoNotGenerateAcw](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw). Это свойство указывает процессу создания ACW *пропустить* тип с заметкой, что позволяет объявить новые управляемые вызываемые оболочки, не запуская создание ACW при создании пакета. Это дает возможность привязывать существующие типы Java. Например, рассмотрим следующий простой класс Java, `Adder`, содержащий один метод, `add`, который складывает целые числа и возвращает результат.

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

Тип `Adder` можно привязать следующим образом.

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public partial class Adder : Java.Lang.Object {
    static IntPtr class_ref = JNIEnv.FindClass ( "mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }
}
partial class ManagedAdder : Adder {
}
```

Здесь тип C# `Adder` использует *псевдоним* типа Java `Adder`. Атрибут `[Register]` используется для указания имени JNI типа Java `mono.android.test.Adder`, а свойство `DoNotGenerateAcw` — для запрета создания ACW. Это приведет к созданию ACW для типа `ManagedAdder`, который правильно выводит подкласс типа `mono.android.test.Adder`. Если бы свойство `RegisterAttribute.DoNotGenerateAcw` не использовалось, процесс сборки Xamarin.Android создал бы новый тип Java `mono.android.test.Adder`. Это привело бы к ошибкам компиляции, так как тип `mono.android.test.Adder` был бы представлен дважды в двух отдельных файлах.

### <a name="binding-virtual-methods"></a>Привязка виртуальных методов

`ManagedAdder` выводит подкласс типа Java `Adder`, но это не особенно интересно: тип C# `Adder` не определяет виртуальные методы, поэтому `ManagedAdder` не может переопределить что-либо.

Привязка методов `virtual` для переопределения с помощью подклассов требует выполнить несколько действий, которые можно разделить на две категории:

1. **привязка метода**;

1. **регистрация метода**.

#### <a name="method-binding"></a>Привязка метода

Для привязки метода требуется добавить два элемента поддержки в определение C# `Adder`: `ThresholdType` и `ThresholdClass`.

##### <a name="thresholdtype"></a>ThresholdType

Свойство `ThresholdType` возвращает текущий тип привязки.

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` используется в привязке метода, чтобы определить, когда нужно подготавливать к отправке виртуальный, а когда — невиртуальный метод. Оно всегда должно возвращать экземпляр `System.Type`, соответствующий объявляемому типу C#.

##### <a name="thresholdclass"></a>ThresholdClass

Свойство `ThresholdClass` возвращает ссылку на класс JNI для типа привязки.

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` используется в привязке метода при вызове невиртуальных методов.

#### <a name="binding-implementation"></a>Реализация привязки

Реализация привязки метода отвечает за вызов метода Java во время выполнения. Она также содержит объявление настраиваемого атрибута `[Register]`, используемое в регистрации метода. Это будет рассмотрено в разделе "Регистрация метода".

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }
}
```

Поле `id_add` содержит идентификатор вызываемого метода Java. Значение `id_add` извлекается из `JNIEnv.GetMethodID`. Для этого нужен объявляющий класс (`class_ref`), имя метода Java (`"add"`) и сигнатура JNI метода (`"(II)I"`).

После получения идентификатора метода `GetType` сравнивается с `ThresholdType`, чтобы определить, требуется подготовка к доставке виртуального или невиртуального метода. Подготовка к доставке виртуального метода необходима, если `GetType` соответствует `ThresholdType`, так как `Handle` может ссылаться на подкласс, выделенный Java, который переопределяет метод.

Если `GetType` не соответствует `ThresholdType`, для `Adder` был выведен подкласс (например, `ManagedAdder`), а реализация `Adder.Add` будет вызываться только в том случае, если был вызван подкласс `base.Add`. Это случай подготовки к доставке невиртуального метода, в котором задействуется `ThresholdClass`. `ThresholdClass` указывает, какой класс Java предоставит реализацию вызываемого метода.

#### <a name="method-registration"></a>Регистрация метода

Предположим, что у нас есть обновленное определение `ManagedAdder`, которое переопределяет метод `Adder.Add`.

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Как вы помните, у `Adder.Add` был настраиваемый атрибут `[Register]`.

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

Конструктор настраиваемого атрибута `[Register]` принимает три значения:

1. имя метода Java, в данном случае — `"add"`;

1. сигнатуру типа JNI метода, в данном случае — `"(II)I"`;

1. *метод соединителя*, в данном случае — `GetAddHandler`.
    Методы соединителя будут рассмотрены позже.

Первые два параметра позволяют процессу создания ACW сформировать объявление метода для переопределения метода. Полученная оболочка ACW будет содержать приведенный ниже код.

```csharp
public class ManagedAdder extends mono.android.test.Adder {
    static final String __md_methods;
    static {
        __md_methods = "n_add:(II)I:GetAddHandler\n" +
            "";
        mono.android.Runtime.register (...);
    }
    @Override
    public int add (int p0, int p1) {
        return n_add (p0, p1);
    }
    private native int n_add (int p0, int p1);
    // ...
}
```

Обратите внимание на то, что объявляется метод `@Override`, который замещает метод с тем же именем и префиксом `n_`. Это гарантирует, что при вызове `ManagedAdder.add` из кода Java будет вызван `ManagedAdder.n_add`, что позволит переопределить метод C# `ManagedAdder.Add`.

Следовательно, остается самый важный вопрос: как `ManagedAdder.n_add` присоединяется к `ManagedAdder.Add`?

Методы Java `native` регистрируются в среде выполнения Java (среде выполнения Android) с помощью [функции JNI RegisterNatives](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734).
`RegisterNatives` принимает массив структур, содержащий имя метода Java, сигнатуру типа JNI и указатель функции для вызова, соответствующий [соглашению о вызове JNI](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715).
Указатель функции должен указывать на функцию, принимающую два аргумента указателя, после которых задаются параметры метода. Метод Java `ManagedAdder.n_add` должен быть реализован с помощью функции, имеющей приведенный ниже прототип C.

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android не предоставляет метод `RegisterNatives`. Вместо этого ACW и MCW предоставляют сведения, необходимые для вызова `RegisterNatives`: ACW содержит имя метода и сигнатуру типа JNI. Единственное, что отсутствует, — это указатель функции для присоединения.

Именно для этого используется *метод соединителя*. Третий параметр настраиваемого атрибута `[Register]` — это имя метода, определенного в зарегистрированном типе или базовом классе зарегистрированного типа, который не принимает параметры и возвращает `System.Delegate`. Возвращаемый `System.Delegate`, в свою очередь, ссылается на метод, имеющий правильную сигнатуру функции JNI. Наконец, делегат, возвращаемый методом соединителя, *должен* быть корневым, чтобы сборщик мусора не удалил его, так как делегат предоставляется Java.

```csharp
#pragma warning disable 0169
static Delegate cb_add;
// This method must match the third parameter of the [Register]
// custom attribute, must be static, must return System.Delegate,
// and must accept no parameters.
static Delegate GetAddHandler ()
{
    if (cb_add == null)
        cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
    return cb_add;
}
// This method is registered with JNI.
static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
{
    Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
    return __this.Add (a, b);
}
#pragma warning restore 0169
```

Метод `GetAddHandler` создает делегат `Func<IntPtr, IntPtr, int, int,
int>`, который ссылается на метод `n_Add`, а затем вызывает [JNINativeWrapper.CreateDelegate](xref:Android.Runtime.JNINativeWrapper.CreateDelegate*).
`JNINativeWrapper.CreateDelegate` заключает предоставленный метод в блок try/catch, чтобы все необработанные исключения обрабатывались и приводили к порождению события [AndroidEvent.UnhandledExceptionRaiser](xref:Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser). Полученный делегат хранится в статической переменной `cb_add`, поэтому сборщик мусора его не удаляет.

Наконец, метод `n_Add` отвечает за маршалинг параметров JNI в соответствующие управляемые типы, он делегирует вызов метода.

Примечание. Всегда используйте `JniHandleOwnership.DoNotTransfer` при получении MCW с использованием экземпляра Java. Если обрабатывать их как локальные ссылки (и вызывать `JNIEnv.DeleteLocalRef`), это приведет к нарушению перехода управляемой вызываемой оболочки в Java, а затем — в управляемый стек.

### <a name="complete-adder-binding"></a>Полная привязка Adder

Ниже приведена полная управляемая привязка для типа `mono.android.tests.Adder`.

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public class Adder : Java.Lang.Object {

    static IntPtr class_ref = JNIEnv.FindClass ("mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    protected override Type ThresholdType {
        get {return typeof (Adder);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

#region Add
    static IntPtr id_add;

    [Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }

#pragma warning disable 0169
    static Delegate cb_add;
    static Delegate GetAddHandler ()
    {
        if (cb_add == null)
            cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
        return cb_add;
    }

    static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
    {
        Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
        return __this.Add (a, b);
    }
#pragma warning restore 0169
#endregion
}
```

### <a name="restrictions"></a>Ограничения

Ограничения действуют при записи типа, который соответствует следующим критериям:

1. используются подклассы `Java.Lang.Object`;

1. имеется настраиваемый атрибут `[Register]`;

1. `RegisterAttribute.DoNotGenerateAcw` равно `true`

В этом случае для интеграции сборки мусора тип *не должен* содержать какие-либо поля, которые могут ссылаться на подкласс `Java.Lang.Object` или `Java.Lang.Object` во время выполнения. Например, не допускаются поля типа `System.Object` и любого типа интерфейса. Разрешены типы, которые не могут ссылаться на экземпляры `Java.Lang.Object`, такие как `System.String` и `List<int>`. Это ограничение призвано предотвратить преждевременное удаление объектов сборщиком мусора.

Если тип должен содержать поле экземпляра, которое может ссылаться на экземпляр `Java.Lang.Object`, то типом поля должен быть `System.WeakReference` или `GCHandle`.

## <a name="binding-abstract-methods"></a>Привязка абстрактных методов

Привязка методов `abstract` в основном идентична привязке виртуальных методов. Существуют только два отличия.

1. Абстрактный метод является абстрактным. Он по-прежнему содержит `[Register]` атрибут и связанную с ним регистрацию метода. Привязка метода просто перемещается на тип `Invoker`.

1. Создается тип `Invoker`, не являющийся `abstract`, который выводит подкласс из абстрактного типа. Тип `Invoker` должен переопределять все абстрактные методы, объявленные в базовом классе, и переопределенная реализация — это реализация метода привязки, хотя случаем подготовки к отправке невиртуального метода можно пренебречь.

Например, предположим, что приведенный выше метод `mono.android.test.Adder.add` относится к типу `abstract`. Привязка C# изменится, чтобы `Adder.Add` был абстрактными, и будет определен новый тип `AdderInvoker`, который реализует `Adder.Add`.

```csharp
partial class Adder {
    [Register ("add", "(II)I", "GetAddHandler")]
    public abstract int Add (int a, int b);

    // The Method Registration machinery is identical to the
    // virtual method case...
}

partial class AdderInvoker : Adder {
    public AdderInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    static IntPtr id_add;
    public override int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
    }
}
```

Тип `Invoker` требуется только при получении ссылок JNI на экземпляры, созданные в Java.

## <a name="binding-interfaces"></a>Привязка интерфейсов

Привязка интерфейсов принципиально схожа с привязкой классов, содержащих виртуальные методы, но между ними существует множество как небольших, так и заметных отличий. Рассмотрим следующее [объявление интерфейса Java](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14).

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Привязки интерфейсов состоят из двух частей: определения интерфейса C# и определения вызывающего объекта для интерфейса.

### <a name="interface-definition"></a>Определение интерфейса

Определение интерфейса C# должно удовлетворять следующим требованиям.

- Определение интерфейса должно иметь настраиваемый атрибут `[Register]`.

- Определение интерфейса должно расширять `IJavaObject interface`.
    Несоблюдение этих условий помешает наследованию оболочек ACW от интерфейса Java.

- Каждый метод интерфейса должен содержать атрибут `[Register]`, указывающий соответствующее имя метода Java, сигнатуру JNI и метод соединителя.

- В методе соединителя также должен быть указан тип для поиска метода соединителя.

При привязке методов `abstract` и `virtual` будет выполнен поиск метода соединителя в иерархии наследования регистрируемого типа. Интерфейсы не должны иметь методов, содержащих текст, так как привязка не будет работать, поэтому требуется указать тип, в котором расположен метод соединителя. Тип указывается в строке метода соединителя после двоеточия `':'`. Нужно указать полное имя сборки типа, содержащего вызывающий объект.

Объявления методов интерфейса — это преобразование соответствующего метода Java с использованием *совместимых* типов. Для встроенных типов Java совместимыми являются соответствующие типы C#, например `int` в Java соответствует `int` в C#. Для ссылочных типов совместимым является тип, который может предоставить дескриптор JNI соответствующего типа Java.

Члены интерфейса не будут напрямую вызываться в Java &ndash; вызов будет осуществлен с помощью типа вызывающего объекта &ndash;. Это увеличивает гибкость метода.

Интерфейс хода выполнения Java можно [объявить в C# как ](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83).

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Обратите внимание на то, что в приведенном выше примере параметр Java `int[]` сопоставляется с [JavaArray&lt;int&gt;](xref:Android.Runtime.JavaArray`1).
Это не обязательно: его можно привязать в C# к `int[]`, `IList<int>` или чему-то абсолютно иному. Какой бы тип ни был выбран, `Invoker` должен иметь возможность преобразовать его в тип Java `int[]` для вызова.

### <a name="invoker-definition"></a>Определение вызывающего объекта

Определение типа `Invoker` должно наследовать `Java.Lang.Object`, реализовывать соответствующий интерфейс и предоставлять все методы подключения, на которые имеются ссылки в определении интерфейса. Существует еще одна рекомендация, отличающая привязку интерфейса от привязки класса: поле `class_ref` и идентификаторы методов должны быть членами экземпляра, а не статическими элементами.

Использование членов экземпляра предпочтительно ввиду поведения `JNIEnv.GetMethodID` в среде выполнения Android. (Это может относиться и к Java, хотя это и не было проверено.) `JNIEnv.GetMethodID` возвращает значение NULL при поиске метода, который поступает из реализованного интерфейса, а не объявленного интерфейса. Рассмотрим интерфейс Java [java.util.SortedMap&lt;K, V&gt;](https://developer.android.com/reference/java/util/SortedMap.html), реализующий интерфейс [java.util.Map&lt;K, V&gt;](https://developer.android.com/reference/java/util/Map.html). Map предоставляет метод [clear](https://developer.android.com/reference/java/util/Map.html#clear()), поэтому очевидно, что разумным определением `Invoker` для SortedMap будет следующее.

```csharp
// Fails at runtime. DO NOT FOLLOW
partial class ISortedMapInvoker : Java.Lang.Object, ISortedMap {
    static IntPtr class_ref = JNIEnv.FindClass ("java/util/SortedMap");
    static IntPtr id_clear;
    public void Clear()
    {
        if (id_clear == IntPtr.Zero)
            id_clear = JNIEnv.GetMethodID(class_ref, "clear", "()V");
        JNIEnv.CallVoidMethod(Handle, id_clear);
    }
     // ...
}
```

Приведенный выше код завершится ошибкой, так как `JNIEnv.GetMethodID` будет возвращать `null` при поиске метода `Map.clear` посредством экземпляра класса `SortedMap`.

Существуют два решения: отслеживание интерфейса, из которого поступают все методы, и применение `class_ref` для каждого интерфейса, либо представление всех элементов в качестве членов экземпляра и выполнение поиска методов по ближайшему производному типу класса, а не типу интерфейса. Второй способ применяется в **Mono.Android.dll**.

Определение вызывающего объекта состоит из шести разделов: конструктора, метода `Dispose`, членов `ThresholdType` и `ThresholdClass`, метода `GetObject`, реализации метода интерфейса и реализации метода соединителя.

#### <a name="constructor"></a>Конструктор

Конструктору необходимо найти класс среды выполнения для вызываемого экземпляра и сохранить этот класс в поле `class_ref` экземпляра.

```csharp
partial class IAdderProgressInvoker {
    IntPtr class_ref;
    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref   = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }
}
```

Примечание. Свойство `Handle` должно быть указано в тексте конструктора, а не в параметре `handle`, так как в Android 4.0 параметр `handle` может быть недопустимым после завершения выполнения базового конструктора.

#### <a name="dispose-method"></a>Метод Dispose

Методу `Dispose` необходимо освободить глобальную ссылку, выделенную в конструкторе.

```csharp
partial class IAdderProgressInvoker {
    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }
}
```

#### <a name="thresholdtype-and-thresholdclass"></a>ThresholdType и ThresholdClass

Члены `ThresholdType` и `ThresholdClass` идентичны элементам в привязке класса.

```csharp
partial class IAdderProgressInvoker {
    protected override Type ThresholdType {
        get {
            return typeof (IAdderProgressInvoker);
        }
    }
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

#### <a name="getobject-method"></a>Метод GetObject

Статический метод `GetObject` требуется для поддержки [Extensions.JavaCast&lt;T&gt;()](xref:Android.Runtime.Extensions.JavaCast*).

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```

#### <a name="interface-methods"></a>Методы интерфейса

Каждый метод интерфейса должен иметь реализацию, которая вызывает соответствующий метод Java с помощью JNI.

```csharp
partial class IAdderProgressInvoker {
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd", "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd, new JValue (JNIEnv.ToJniHandle (values)), new JValue (currentIndex), new JValue (currentSum));
    }
}
```

#### <a name="connector-methods"></a>Методы соединителя

Методы соединителя и вспомогательная инфраструктура отвечают за маршалинг параметров JNI в соответствующие типы C#. Параметр Java `int[]` будет передан как JNI `jintArray`, который является `IntPtr` в C#. `IntPtr` нужно маршалировать в `JavaArray<int>` для поддержки вызова интерфейса C#.

```csharp
partial class IAdderProgressInvoker {
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
}
```

Если предпочтительнее использовать `int[]`, а не `JavaList<int>`, то можно использовать [JNIEnv.GetArray()](xref:Android.Runtime.JNIEnv.GetArray*).

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Однако обратите внимание на то, что `JNIEnv.GetArray` копирует весь массив между виртуальными машинами, поэтому при обработке больших массивов это может сильно нагрузить сборщик мусора.

### <a name="complete-invoker-definition"></a>Полное определение вызывающего объекта

Ниже приведено [полное определение IAdderProgressInvoker](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88).

```csharp
class IAdderProgressInvoker : Java.Lang.Object, IAdderProgress {

    IntPtr class_ref;

    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }

    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }

    protected override Type ThresholdType {
        get {return typeof (IAdderProgressInvoker);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }

#region OnAdd
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd",
                    "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd,
                new JValue (JNIEnv.ToJniHandle (values)),
                new JValue (currentIndex),
new JValue (currentSum));
    }

#pragma warning disable 0169
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
#pragma warning restore 0169
#endregion
}
```

## <a name="jni-object-references"></a>Ссылки на объекты JNI

Многие методы JNIEnv возвращают *ссылки на объекты* *JNI*, которые похожи на элементы `GCHandle`. JNI предоставляет три разных типа ссылок на объекты: локальные ссылки, глобальные ссылки и слабые глобальные ссылки. Все три типа представляются как `System.IntPtr`, *но* (в соответствии с разделом типов функций JNI) не все объекты `IntPtr`, возвращенные из методов `JNIEnv`, являются ссылками. Например, [JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*) возвращает `IntPtr`, но не возвращает ссылку на объект. Он возвращает `jmethodID`. Дополнительные сведения см. в [документации по функциям JNI](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html).

Локальные ссылки создаются *большинством* методов создания ссылок.
Android допускает существование только ограниченного числа локальных ссылок. Обычно это 512 локальных ссылок. Локальные ссылки можно удалить с помощью [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*).
В отличие от JNI, не все ссылочные методы JNIEnv, возвращающие ссылки на объекты, возвращают локальные ссылки. [JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*) возвращает *глобальную* ссылку. Настоятельно рекомендуется удалять локальные ссылки как можно быстрее, возможно, путем создания оболочки [Java.Lang.Object](xref:Java.Lang.Object) для объекта и указания `JniHandleOwnership.TransferLocalRef` для конструктора [Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*).

Глобальные ссылки создаются с помощью [JNIEnv.NewGlobalRef](xref:Android.Runtime.JNIEnv.NewGlobalRef*) и [JNIEnv.FindClass](xref:Android.Runtime.JNIEnv.FindClass*).
Их можно уничтожить с помощью [JNIEnv.DeleteGlobalRef](xref:Android.Runtime.JNIEnv.DeleteGlobalRef*).
Эмуляторы имеют ограничение в 2000 ожидающих выполнения глобальных ссылок, в то время как ограничение устройств составляет порядка 52 000 глобальных ссылок.

Слабые глобальные ссылки доступны только в Android 2.2 (Froyo) и более поздних версиях. Слабые глобальные ссылки можно удалить с помощью [JNIEnv.DeleteWeakGlobalRef](xref:Android.Runtime.JNIEnv.DeleteWeakGlobalRef*).

### <a name="dealing-with-jni-local-references"></a>Работа с локальными ссылками JNI

Методы [JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*), [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*), [JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*), [JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) и [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) возвращают `IntPtr` с локальной ссылкой JNI на объект Java либо `IntPtr.Zero`, если платформа Java вернула `null`. Ввиду ограниченного количества локальных ссылок, которые могут ожидать обработки (512), желательно обеспечить своевременное удаление ссылок. Существуют три способа обработки локальных ссылок: их можно явно удалить, создать экземпляр `Java.Lang.Object` для их хранения и использовать `Java.Lang.Object.GetObject<T>()`, чтобы создать для них управляемую вызываемую оболочку.

### <a name="explicitly-deleting-local-references"></a>Явное удаление локальных ссылок

Для удаления локальных ссылок используется [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*). После удаления локальной ссылки она больше не может использоваться, поэтому применять `JNIEnv.DeleteLocalRef` к локальным ссылкам следует весьма осторожно и в самую последнюю очередь.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```

### <a name="wrapping-with-javalangobject"></a>Перенос в оболочку с помощью Java.Lang.Object

`Java.Lang.Object` предоставляет конструктор [Java.Lang.Object(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object#ctor*), который можно использовать для переноса в оболочку завершаемой ссылки JNI. Параметр [JniHandleOwnership](xref:Android.Runtime.JniHandleOwnership) определяет, как следует обрабатывать параметр `IntPtr`.

- [JniHandleOwnership.DoNotTransfer](xref:Android.Runtime.JniHandleOwnership.DoNotTransfer) &ndash; созданный экземпляр `Java.Lang.Object` сформирует новую глобальную ссылку на основе параметра `handle`, а `handle` не изменится.
    Вызывающий объект отвечает за освобождение `handle`, если это необходимо.

- [JniHandleOwnership.TransferLocalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; созданный экземпляр `Java.Lang.Object` сформирует новую глобальную ссылку на основе параметра `handle`, а `handle` будет удален посредством [JNIEnv.DeleteLocalRef](xref:Android.Runtime.JNIEnv.DeleteLocalRef*). Вызывающий объект не должен освобождать `handle` и не должен использовать `handle` после завершения выполнения конструктора.

- [JniHandleOwnership.TransferGlobalRef](xref:Android.Runtime.JniHandleOwnership.TransferLocalRef) &ndash; созданный экземпляр `Java.Lang.Object` станет владельцем параметра `handle`. Вызывающий объект не должен освобождать `handle`.

Так как методы вызова метода JNI возвращают локальные ссылки, как правило, используется `JniHandleOwnership.TransferLocalRef`.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Созданная глобальная ссылка не будет освобождена, пока экземпляр `Java.Lang.Object` не будет удален сборщиком мусора. Рекомендуется удалять экземпляр, чтобы освободить глобальную ссылку, что ускоряет сборку мусора.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```

### <a name="using-javalangobjectgetobjectlttgt"></a>Использование Java.Lang.Object.GetObject&lt;T&gt;()

`Java.Lang.Object` предоставляет метод [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr handle, JniHandleOwnership transfer)](xref:Java.Lang.Object.GetObject*), который можно использовать для создания управляемой вызываемой оболочки указанного типа.

Тип `T` должен соответствовать следующим требованиям:

1. Тип `T` должен быть ссылочным.

1. `T` должен реализовывать интерфейс `IJavaObject`.

1. Если `T` не является абстрактным классом или интерфейсом, `T` должен предоставлять конструктор с типами параметров `(IntPtr,
    JniHandleOwnership)`.

1. Если `T` является абстрактным классом или интерфейсом, то для `T` *должен быть* предоставлен *вызывающий объект*. Вызывающий объект — это неабстрактный тип, который наследует `T` или реализует `T` и использует то же имя, что и `T`, с добавлением суффикса вызывающего объекта. Например, если T является интерфейсом `Java.Lang.IRunnable`, то тип `Java.Lang.IRunnableInvoker` должен существовать и должен содержать обязательный конструктор `(IntPtr,
    JniHandleOwnership)`.

Так как методы вызова метода JNI возвращают локальные ссылки, как правило, используется `JniHandleOwnership.TransferLocalRef`.

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types"></a>

## <a name="looking-up-java-types"></a>Поиск типов Java

Для поиска поля или метода в JNI необходимо сначала найти объявляющий тип для этого поля или метода. Для поиска типов Java используется [Android.Runtime.JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*). Параметр string — это *упрощенная ссылка на тип* или *полная ссылка на тип* для типа Java. Дополнительные сведения об упрощенных и полных ссылках на тип см. в разделе [Ссылки на типы JNI](#_JNI_Type_References).

Примечание. В отличие от любого другого метода `JNIEnv`, который возвращает экземпляры объекта, `FindClass` возвращает глобальную ссылку, а не локальную ссылку.

<a name="_Instance_Fields"></a>

## <a name="instance-fields"></a>Поля экземпляра

Для управления полями используются *идентификаторы полей*. Для получения идентификаторов полей используется [JNIEnv.GetFieldID](xref:Android.Runtime.JNIEnv.GetFieldID*), для которого требуется указать класс, в котором определено поле, имя поля и [сигнатуру типа JNI](#JNI_Type_Signatures) поля.

Идентификаторы полей не обязательно освобождаются и являются допустимыми, пока загружен соответствующий тип Java. (В настоящее время Android не поддерживает выгрузку классов.)

Существуют два набора методов для управления полями экземпляра: один — для чтения полей экземпляра, а другой — для записи полей экземпляра. Для всех наборов методов требуется идентификатор поля для чтения или записи значения поля.

### <a name="reading-instance-field-values"></a>Чтение значений полей экземпляра

Набор методов для чтения значений полей экземпляра соответствует шаблону именования:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```

где `*` — это тип поля.

- [JNIEnv.GetObjectField](xref:Android.Runtime.JNIEnv.GetObjectField*) &ndash; чтение значения любого поля экземпляра, которое не является встроенным типом (например `java.lang.Object`, массивы и типы интерфейсов). Возвращаемое значение является локальной ссылкой JNI.

- [JNIEnv.GetBooleanField](xref:Android.Runtime.JNIEnv.GetBooleanField*) &ndash; чтение значения полей экземпляра типа `bool`.

- [JNIEnv.GetByteField](xref:Android.Runtime.JNIEnv.GetByteField*) &ndash; чтение значения полей экземпляра типа `sbyte`.

- [JNIEnv.GetCharField](xref:Android.Runtime.JNIEnv.GetCharField*) &ndash; чтение значения полей экземпляра типа `char`.

- [JNIEnv.GetShortField](xref:Android.Runtime.JNIEnv.GetShortField*) &ndash; чтение значения полей экземпляра типа `short`.

- [JNIEnv.GetIntField](xref:Android.Runtime.JNIEnv.GetIntField*) &ndash; чтение значения полей экземпляра типа `int`.

- [JNIEnv.GetLongField](xref:Android.Runtime.JNIEnv.GetLongField*) &ndash; чтение значения полей экземпляра типа `long`.

- [JNIEnv.GetFloatField](xref:Android.Runtime.JNIEnv.GetFloatField*) &ndash; чтение значения полей экземпляра типа `float`.

- [JNIEnv.GetDoubleField](xref:Android.Runtime.JNIEnv.GetDoubleField*) &ndash; чтение значения полей экземпляра типа `double`.

### <a name="writing-instance-field-values"></a>Запись значений полей экземпляра

Набор методов для записи значений полей экземпляра соответствует шаблону именования:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

где *Type* — это тип поля.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; запись значения любого поля экземпляра, которое не является встроенным типом (например `java.lang.Object`, массивы и типы интерфейсов). Значением `IntPtr` может быть локальная ссылка JNI, глобальная ссылка JNI, слабая глобальная ссылка JNI или `IntPtr.Zero` (для `null`).

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*) &ndash; запись значения полей экземпляра типа `bool`.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; запись значения полей экземпляра типа `sbyte`.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; запись значения полей экземпляра типа `char`.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; запись значения полей экземпляра типа `short`.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; запись значения полей экземпляра типа `int`.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; запись значения полей экземпляра типа `long`.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; запись значения полей экземпляра типа `float`.

- [JNIEnv.SetField](xref:Android.Runtime.JNIEnv.SetField*)) &ndash; запись значения полей экземпляра типа `double`.

<a name="_Static_Fields"></a>

## <a name="static-fields"></a>Статические поля

Для управления статическими полями используются *идентификаторы полей*. Для получения идентификаторов полей используется [JNIEnv.GetStaticFieldID](xref:Android.Runtime.JNIEnv.GetStaticFieldID*), для которого требуется указать класс, в котором определено поле, имя поля и [сигнатуру типа JNI](#JNI_Type_Signatures) поля.

Идентификаторы полей не обязательно освобождаются и являются допустимыми, пока загружен соответствующий тип Java. (В настоящее время Android не поддерживает выгрузку классов.)

Существуют два набора методов для управления статическими полями: один — для чтения полей, а другой — для записи полей. Для всех наборов методов требуется идентификатор поля для чтения или записи значения поля.

### <a name="reading-static-field-values"></a>Чтение значений статических полей

Набор методов для чтения значений статических полей соответствует шаблону именования:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

где `*` — это тип поля.

- [JNIEnv.GetStaticObjectField](xref:Android.Runtime.JNIEnv.GetStaticObjectField*) &ndash; чтение значения любого статического поля, которое не является встроенным типом (например `java.lang.Object`, массивы и типы интерфейсов). Возвращаемое значение является локальной ссылкой JNI.

- [JNIEnv.GetStaticBooleanField](xref:Android.Runtime.JNIEnv.GetStaticBooleanField*) &ndash; чтение значения статических полей типа `bool`.

- [JNIEnv.GetStaticByteField](xref:Android.Runtime.JNIEnv.GetStaticByteField*) &ndash; чтение значения статических полей типа `sbyte`.

- [JNIEnv.GetStaticCharField](xref:Android.Runtime.JNIEnv.GetStaticCharField*) &ndash; чтение значения статических полей типа `char`.

- [JNIEnv.GetStaticShortField](xref:Android.Runtime.JNIEnv.GetStaticShortField*) &ndash; чтение значения статических полей типа `short`.

- [JNIEnv.GetStaticLongField](xref:Android.Runtime.JNIEnv.GetStaticLongField*) &ndash; чтение значения статических полей типа `long`.

- [JNIEnv.GetStaticFloatField](xref:Android.Runtime.JNIEnv.GetStaticFloatField*) &ndash; чтение значения статических полей типа `float`.

- [JNIEnv.GetStaticDoubleField](xref:Android.Runtime.JNIEnv.GetStaticDoubleField*) &ndash; чтение значения статических полей типа `double`.

### <a name="writing-static-field-values"></a>Запись значений статических полей

Набор методов для записи значений статических полей соответствует шаблону именования:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

где *Type* — это тип поля.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*) &ndash; запись значения любого статического поля, которое не является встроенным типом (например `java.lang.Object`, массивы и типы интерфейсов). Значением `IntPtr` может быть локальная ссылка JNI, глобальная ссылка JNI, слабая глобальная ссылка JNI или `IntPtr.Zero` (для `null`).

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; запись значения статических полей типа `bool`.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; запись значения статических полей типа `sbyte`.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; запись значения статических полей типа `char`.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; запись значения статических полей типа `short`.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; запись значения статических полей типа `int`.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; запись значения статических полей типа `long`.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; запись значения статических полей типа `float`.

- [JNIEnv.SetStaticField](xref:Android.Runtime.JNIEnv.SetStaticField*)) &ndash; запись значения статических полей типа `double`.

<a name="_Instance_Methods"></a>

## <a name="instance-methods"></a>Методы экземпляра

Для вызова методов экземпляра используются *идентификаторы методов*. Для получения идентификаторов методов используется [JNIEnv.GetMethodID](xref:Android.Runtime.JNIEnv.GetMethodID*), для которого требуется указать тип, в котором определен метод, имя метода и [сигнатуру типа JNI](#JNI_Type_Signatures) метода.

Идентификаторы методов не обязательно освобождаются и являются допустимыми, пока загружен соответствующий тип Java. (В настоящее время Android не поддерживает выгрузку классов.)

Существуют два набора методов для вызова методов: один — для виртуального вызова методов, и другой — для невиртуального вызова методов. Оба набора методов для вызова метода используют идентификатор метода, а для невиртуального вызова также требуется указать, какая реализация класса должна быть вызвана.

Методы интерфейса можно искать только внутри объявляющего типа. Методы, поступающие из расширенных или наследуемых интерфейсов, искать невозможно. Дополнительные сведения см. в разделе о привязке интерфейсов и реализация вызывающего объекта.

Возможен поиск любого метода, объявленного в классе, любом базовом классе или реализованном интерфейсе.

### <a name="virtual-method-invocation"></a>Виртуальный вызов метода

Набор методов для виртуального вызова методов соответствует шаблону именования:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

где `*` — тип возвращаемого значения метода.

- [JNIEnv.CallObjectMethod](xref:Android.Runtime.JNIEnv.CallObjectMethod*) &ndash; вызов метода, который возвращает не встроенный тип (например, `java.lang.Object`, массивы и интерфейсы). Возвращаемое значение является локальной ссылкой JNI.

- [JNIEnv.CallBooleanMethod](xref:Android.Runtime.JNIEnv.CallBooleanMethod*) &ndash; вызов метода, который возвращает значение типа `bool`.

- [JNIEnv.CallByteMethod](xref:Android.Runtime.JNIEnv.CallByteMethod*) &ndash; вызов метода, который возвращает значение типа `sbyte`.

- [JNIEnv.CallCharMethod](xref:Android.Runtime.JNIEnv.CallCharMethod*) &ndash; вызов метода, который возвращает значение типа `char`.

- [JNIEnv.CallShortMethod](xref:Android.Runtime.JNIEnv.CallShortMethod*) &ndash; вызов метода, который возвращает значение типа `short`.

- [JNIEnv.CallLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; вызов метода, который возвращает значение типа `long`.

- [JNIEnv.CallFloatMethod](xref:Android.Runtime.JNIEnv.CallFloatMethod*) &ndash; вызов метода, который возвращает значение типа `float`.

- [JNIEnv.CallDoubleMethod](xref:Android.Runtime.JNIEnv.CallDoubleMethod*) &ndash; вызов метода, который возвращает значение типа `double`.

### <a name="non-virtual-method-invocation"></a>Невиртуальный вызов метода

Набор методов для невиртуального вызова методов соответствует шаблону именования:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

где `*` — тип возвращаемого значения метода. Невиртуальный вызов метода обычно используется для вызова базового метода виртуального метода.

- [JNIEnv.CallNonvirtualObjectMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualObjectMethod*) &ndash; невиртуальный вызов метода, который возвращает не встроенный тип (например, `java.lang.Object`, массивы и интерфейсы). Возвращаемое значение является локальной ссылкой JNI.

- [JNIEnv.CallNonvirtualBooleanMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod*) &ndash; невиртуальный вызов метода, который возвращает значение `bool`.

- [JNIEnv.CallNonvirtualByteMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualByteMethod*) &ndash; невиртуальный вызов метода, который возвращает значение типа `sbyte`.

- [JNIEnv.CallNonvirtualCharMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualCharMethod*) &ndash; невиртуальный вызов метода, который возвращает значение типа `char`.

- [JNIEnv.CallNonvirtualShortMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualShortMethod*) &ndash; невиртуальный вызов метода, который возвращает значение типа `short`.

- [JNIEnv.CallNonvirtualLongMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualLongMethod*) &ndash; невиртуальный вызов метода, который возвращает значение типа `long`.

- [JNIEnv.CallNonvirtualFloatMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualFloatMethod*) &ndash; невиртуальный вызов метода, который возвращает значение типа `float`.

- [JNIEnv.CallNonvirtualDoubleMethod](xref:Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod*) &ndash; невиртуальный вызов метода, который возвращает значение типа `double`.

<a name="_Static_Methods"></a>

## <a name="static-methods"></a>Статические методы

Для вызова статических методов используются *идентификаторы методов*. Для получения идентификаторов методов используется [JNIEnv.GetStaticMethodID](xref:Android.Runtime.JNIEnv.GetStaticMethodID*), для которого требуется указать тип, в котором определен метод, имя метода и [сигнатуру типа JNI](#JNI_Type_Signatures) метода.

Идентификаторы методов не обязательно освобождаются и являются допустимыми, пока загружен соответствующий тип Java. (В настоящее время Android не поддерживает выгрузку классов.)

### <a name="static-method-invocation"></a>Вызов статического метода

Набор методов для виртуального вызова методов соответствует шаблону именования:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

где `*` — тип возвращаемого значения метода.

- [JNIEnv.CallStaticObjectMethod](xref:Android.Runtime.JNIEnv.CallStaticObjectMethod*) &ndash; вызов статического метода, который возвращает не встроенный тип (например, `java.lang.Object`, массивы и интерфейсы). Возвращаемое значение является локальной ссылкой JNI.

- [JNIEnv.CallStaticBooleanMethod](xref:Android.Runtime.JNIEnv.CallStaticBooleanMethod*) &ndash; вызов статического метода, который возвращает значение типа `bool`.

- [JNIEnv.CallStaticByteMethod](xref:Android.Runtime.JNIEnv.CallStaticByteMethod*) &ndash; вызов статического метода, который возвращает значение типа `sbyte`.

- [JNIEnv.CallStaticCharMethod](xref:Android.Runtime.JNIEnv.CallStaticCharMethod*) &ndash; вызов статического метода, который возвращает значение типа `char`.

- [JNIEnv.CallStaticShortMethod](xref:Android.Runtime.JNIEnv.CallStaticShortMethod*) &ndash; вызов статического метода, который возвращает значение типа `short`.

- [JNIEnv.CallStaticLongMethod](xref:Android.Runtime.JNIEnv.CallLongMethod*) &ndash; вызов статического метода, который возвращает значение типа `long`.

- [JNIEnv.CallStaticFloatMethod](xref:Android.Runtime.JNIEnv.CallStaticFloatMethod*) &ndash; вызов статического метода, который возвращает значение типа `float`.

- [JNIEnv.CallStaticDoubleMethod](xref:Android.Runtime.JNIEnv.CallStaticDoubleMethod*) &ndash; вызов статического метода, который возвращает значение типа `double`.

<a name="JNI_Type_Signatures"></a>

## <a name="jni-type-signatures"></a>Сигнатуры типа JNI

[Сигнатуры типа JNI](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) — это [ссылки на типы JNI](#_JNI_Type_References) (но не упрощенные ссылки на типы), за исключением методов. Для методов сигнатура типа JNI представляет собой следующую конструкцию: открывающая круглая скобка `'('`, за ней следуют ссылки на типы всех параметров, сцепленные вместе (без разделителей), затем следует закрывающая круглая скобка `')'`, за которой следует ссылка на тип JNI для возвращаемого типа метода.

Рассмотрим приведенный ниже метод Java.

```java
long f(int n, String s, int[] array);
```

Сигнатура типа JNI будет выглядеть следующим образом.

```csharp
(ILjava/lang/String;[I)J
```

Как правило, *настоятельно* рекомендуется использовать команду `javap` для определения сигнатур JNI. Например, сигнатура типа JNI для метода [java.lang.Thread.State.valueOf(String)](https://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) — "(Ljava/lang/String;)Ljava/lang/Thread$State;", а сигнатура типа JNI для метода [java.lang.Thread.State.values](https://developer.android.com/reference/java/lang/Thread.State.html#values) — "()[Ljava/lang/Thread$State;". Следите за конечными точками с запятой. Они *входят* в сигнатуру типа JNI.

<a name="_JNI_Type_References"></a>

## <a name="jni-type-references"></a>Ссылки на типы JNI

Ссылки на типы JNI отличаются от ссылок на типы Java. Для JNI невозможно использовать полные имена типов Java, такие как `java.lang.String`. Вместо них необходимо использовать варианты JNI `"java/lang/String"` или `"Ljava/lang/String;"`, в зависимости от контекста. Дополнительные сведения приведены ниже.
Существуют четыре типа ссылок на типы JNI:

- **ссылки на встроенные типы**;
- **упрощенные ссылки**;
- **type**
- **array**

### <a name="built-in-type-references"></a>Ссылки на встроенные типы

Ссылки на встроенные типы представляют собой отдельный знак, используемый в качестве ссылки на встроенные типы значений. Сопоставление выглядит следующим образом:

- `"B"` используется для `sbyte`.
- `"S"` используется для `short`.
- `"I"` используется для `int`.
- `"J"` используется для `long`.
- `"F"` используется для `float`.
- `"D"` используется для `double`.
- `"C"` используется для `char`.
- `"Z"` используется для `bool`.
- `"V"` используется для типов возвращаемого значения метода `void`.

<a name="_Simplified_Type_References_1"></a>

### <a name="simplified-type-references"></a>Упрощенные ссылки на типы

Упрощенные ссылки на типы могут использоваться только в [JNIEnv.FindClass(string)](xref:Android.Runtime.JNIEnv.FindClass*).
Существуют два способа получить упрощенную ссылку на тип.

1. В полном имени Java замените каждый знак `'.'` в имени пакета, перед именем типа `'/'` и в имени типа `'.'` знаком `'$'`.

1. Считайте выходные данные `'unzip -l android.jar | grep JavaName'`.

Оба метода позволяют сопоставить [Java.Lang.Thread.State](https://developer.android.com/reference/java/lang/Thread.State.html) типа Java с упрощенной ссылкой на тип `java/lang/Thread$State`.

### <a name="type-references"></a>Ссылки на типы

Ссылка на тип — это ссылка на встроенный тип или упрощенная ссылка на тип с префиксом `'L'` и суффиксом `';'`. Для [Java.Lang.String](https://developer.android.com/reference/java/lang/String.html) типа Java упрощенная ссылка на тип — `"java/lang/String"`, а ссылка на тип — `"Ljava/lang/String;"`.

Ссылки на типы используются наряду со ссылками на типы массива и сигнатурами JNI.

Чтобы получить ссылку на тип, можно считать выходные данные `'javap -s -classpath android.jar fully.qualified.Java.Name'`.
В зависимости от применяемого типа можно использовать объявление конструктора или тип возвращаемого значения метода, чтобы определить имя JNI. Пример:

```shell
$ javap -classpath android.jar -s java.lang.Thread.State
Compiled from "Thread.java"
```

```java
public final class java.lang.Thread$State extends java.lang.Enum{
public static final java.lang.Thread$State NEW;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State RUNNABLE;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State BLOCKED;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TIMED_WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TERMINATED;
  Signature: Ljava/lang/Thread$State;
public static java.lang.Thread$State[] values();
  Signature: ()[Ljava/lang/Thread$State;
public static java.lang.Thread$State valueOf(java.lang.String);
  Signature: (Ljava/lang/String;)Ljava/lang/Thread$State;
static {};
  Signature: ()V
}
```

`Thread.State` является типом перечисления Java, поэтому мы можем использовать сигнатуру метода `valueOf`, чтобы определить, что ссылка на тип — Ljava/lang/Thread$State;.

### <a name="array-type-references"></a>Ссылки на типы массива

Ссылки на типы массивов представляют собой ссылки на типы JNI с префиксом `'['`.
Упрощенные ссылки на типы невозможно использовать для указания массивов.

Например, `int[]` соответствует `"[I"`, `int[][]` — `"[[I"`, а `java.lang.Object[]` — `"[Ljava/lang/Object;"`.

## <a name="java-generics-and-type-erasure"></a>Универсальные шаблоны Java и очистка типов

*Большую часть* времени, как видно в JNI, универсальные шаблоны Java *не существуют*.
Существуют некоторые детали, которые проявляются в том, как Java взаимодействует с универсальными шаблонами, но не в том, как JNI ищет и вызывает универсальные члены.

При взаимодействии посредством JNI нет разницы между универсальным типом или членом и неуниверсальным типом или членом. Например, универсальный тип [Java.Lang.Class&lt;T&gt;](https://developer.android.com/reference/java/lang/Class.html) является также необработанным универсальным типом `java.lang.Class`, и для обоих этих типов используется одна и та же упрощенная ссылка на тип, `"java/lang/Class"`.

## <a name="java-native-interface-support"></a>Поддержка Java Native Interface

[Android.Runtime.JNIEnv](xref:Android.Runtime.JNIEnv) — это управляемая оболочка для Java Native Interface (JNI). Функции JNI объявляются в [спецификации Java Native Interface](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html), хотя методы были изменены: был удален явный параметр `JNIEnv*`, а вместо `jobject`, `jclass`, `jmethodID` и т. д. используется `IntPtr`. Например, рассмотрим [функцию JNI NewObject](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517).

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

Она предоставляется как метод [JNIEnv.NewObject](xref:Android.Runtime.JNIEnv.NewObject*).

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

Взаимное преобразование двух вызовов достаточно простое. В C нужно сделать следующее.

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

Эквивалент в C# показан ниже.

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

Если экземпляр объекта Java хранится в IntPtr, вероятно, с ним нужно будет что-то сделать. Для этого можно использовать такие методы JNIEnv, как [JNIEnv.CallVoidMethod()](xref:Android.Runtime.JNIEnv.CallVoidMethod*), но если уже имеется аналогичная оболочка C#, необходимо будет создать оболочку для ссылки JNI. Это можно сделать с помощью метода расширения [Extensions.JavaCast\<T>](xref:Android.Runtime.Extensions.JavaCast*).

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

Можно также использовать метод [Java.Lang.Object.GetObject\<T>](xref:Java.Lang.Object.GetObject*).

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Более того, все функции JNI были изменены: удален параметр `JNIEnv*`, присутствовавший в каждой функции JNI.

## <a name="summary"></a>Сводка

Непосредственно работать с JNI ужасно неудобно, и этого следует избегать любой ценой. К сожалению, избежать этого удается не всегда. Надеюсь, это руководство поможет вам, если вы столкнетесь с использованием несвязанных типов Java и Mono для Android.

## <a name="related-links"></a>Связанные ссылки

- [Спецификация Java Native Interface](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Функции Java Native Interface](https://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
