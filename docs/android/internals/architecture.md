---
title: Архитектура
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 8cbcc0cf99dd82dc9a943c63d9e9544482581896
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571419"
---
# <a name="architecture"></a>Архитектура

Приложения Xamarin. Android выполняются в среде выполнения Mono.
Эта среда выполнения работает параллельно с виртуальной машиной среды выполнения Android (ART). Обе среды выполнения работают поверх ядра Linux и предоставляют различные API для пользовательского кода, который позволяет разработчикам получить доступ к базовой системе. Среда выполнения Mono написана на языке C.

Для доступа к основным средствам операционной системы Linux можно использовать [System](xref:System), [System.IO](xref:System.IO), [System.NET](xref:System.Net) и остальные библиотеки классов .NET.

В Android большинство систем, таких как аудио, графика, OpenGL и телефония, недоступны непосредственно в собственных приложениях, они доступны только через API-интерфейсы Java среды выполнения Android, находящиеся в одном из пространств имен [Java](xref:Java.Lang). * или [Android](xref:Android). *. Архитектура выглядит примерно так:

[![Схема Mono и ART над ядром и ниже привязок .NET/Java +](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Разработчики Xamarin. Android получают доступ к различным функциям в операционной системе либо путем вызова интерфейсов API .NET, которые они узнают (для низкоуровневого доступа), либо с помощью классов, предоставляемых в пространствах имен Android, которые предоставляют мост для API-интерфейсов Java, предоставляемых средой выполнения Android.

Дополнительные сведения о взаимодействии классов Android с классами среды выполнения Android см. в документе по [проектированию API](~/android/internals/api-design.md) .

## <a name="application-packages"></a>Пакеты приложений

Пакеты приложений Android — это ZIP-контейнеры с расширением *apk* . Пакеты приложений Xamarin. Android имеют ту же структуру и макет, что и обычные пакеты Android, со следующими дополнениями:

- Сборки приложения (содержащие IL) *хранятся* в папке *assemblies* без сжатия. Во время запуска процесса в выпуске сборка *. apk* — это *mmap ()* ED в процесс, а сборки загружаются из памяти. Это позволяет ускорить запуск приложения, так как сборки не нужно извлекать перед выполнением.  
- *Примечание.* Сведения о расположении сборки, такие как [Assembly. Location](xref:System.Reflection.Assembly.Location) и [Assembly. CodeBase](xref:System.Reflection.Assembly.CodeBase) , *не могут рассчитываться* в сборках выпуска. Они не существуют как отдельные записи файловой системы и не имеют пригодных для использования места.

- Собственные библиотеки, содержащие среду выполнения Mono, находятся в *. apk* . Приложение Xamarin. Android должно содержать собственные библиотеки для требуемой или целевой архитектуры Android, например *ARMEABI* , *ARMEABI-V7A* , *x86* . Приложения Xamarin. Android не могут работать на платформе, если она не содержит соответствующие библиотеки среды выполнения.

Приложения Xamarin. Android также содержат *вызываемые оболочки Android* , позволяющие Android вызывать управляемый код.

## <a name="android-callable-wrappers"></a>Вызываемые программы-оболочки Android

- **Вызываемые оболочки Android** — это мост [JNI](https://en.wikipedia.org/wiki/Java_Native_Interface) , который используется в любой момент, когда исполняющая среда Android должна вызывать управляемый код. Вызываемые оболочки Android — это способ переопределения виртуальных методов и возможности реализации интерфейсов Java. Дополнительные сведения см. в документации по [интеграции Java](~/android/platform/java-integration/index.md) .

<a name="Managed_Callable_Wrappers"></a>

## <a name="managed-callable-wrappers"></a>Управляемые вызываемые оболочки

Управляемые вызываемые оболочки — это мост JNI, который используется в любой момент, когда управляемый код должен вызывать код Android и обеспечивать поддержку переопределения виртуальных методов и реализации интерфейсов Java. Все устройства [Android](xref:Android). * и связанные пространства имен являются управляемыми вызываемыми оболочками, созданными с помощью [привязки JAR](~/android/platform/binding-java-library/index.md).
Управляемые вызываемые оболочки отвечают за преобразование между типами управляемых и Android и вызов базовых методов платформы Android через JNI.

Каждая созданная Управляемая обертка содержит глобальную ссылку на Java, доступную через свойство [Android. Runtime. ижаваобжект. Handle](xref:Android.Runtime.IJavaObject.Handle) . Глобальные ссылки используются для обеспечения сопоставления между экземплярами Java и управляемыми экземплярами. Глобальные ссылки — это ограниченный ресурс: Эмуляторы позволяют одновременно существовать только 2000 глобальных ссылок, в то время как большинство устройств поддерживает более 52 000 глобальных ссылок одновременно.

Чтобы определить, когда глобальные ссылки создаются и уничтожаются, можно задать системное свойство [Debug. моно. log](~/android/troubleshooting/index.md) , чтобы оно содержало [Греф](~/android/troubleshooting/index.md).

Глобальные ссылки можно явно освободить, вызвав [Java. lang. Object. Dispose ()](xref:Java.Lang.Object.Dispose) в управляемой вызываемой оболочке. Это приведет к удалению сопоставления между экземпляром Java и управляемым экземпляром и разрешающим сбор экземпляра Java. При повторном доступе к экземпляру Java из управляемого кода для него будет создана новая управляемая обертка.

Следует соблюдать осторожность при удалении управляемых вызываемых оболочек, если экземпляр может быть случайно совместно использоваться потоками, так как удаление экземпляра повлияет на ссылки из других потоков. Для обеспечения максимальной безопасности только `Dispose()` экземпляры, выделенные с помощью `new` *или* из методов, которым *известно* , всегда выделяют новые экземпляры, а не кэшированные экземпляры, которые могут вызвать случайное совместное использование экземпляров между потоками.

## <a name="managed-callable-wrapper-subclasses"></a>Управляемые подклассы вызываемой оболочки

Управляемые вызываемые подклассы оболочки — это то, где может находиться вся «интересная» логика приложения. К ним относятся пользовательские подклассы [Android. app. Activity](xref:Android.App.Activity) (например, тип [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) в шаблоне проекта по умолчанию). (В частности, это любые подклассы *Java. lang. Object* , которые *не* содержат настраиваемого атрибута [регистераттрибуте](xref:Android.Runtime.RegisterAttribute) или [регистераттрибуте. донотженератеакв](xref:Android.Runtime.RegisterAttribute.DoNotGenerateAcw) имеет *значение false*, что является значением по умолчанию.)

Как и управляемые вызываемые оболочки, подклассы управляемой обертки оболочки также содержат глобальную ссылку, доступную через свойство [Java. lang. Object. Handle](xref:Java.Lang.Object.Handle) . Точно так же, как и в случае с управляемыми вызываемыми оболочками, глобальные ссылки можно явно освободить, вызвав [Java. lang. Object. Dispose ()](xref:Java.Lang.Object.Dispose).
В отличие от управляемых вызываемых оболочек, необходимо принять во *внимание* перед удалением таких экземпляров, так как *метод Dispose ()*, применяющий экземпляр, нарушает сопоставление между экземпляром Java (экземпляром вызываемой оболочки Android) и управляемым экземпляром.

### <a name="java-activation"></a>Активация Java

При создании [вызываемой оболочки Android](~/android/platform/java-integration/android-callable-wrappers.md) (АКВ) из Java конструктор АКВ приведет к вызову соответствующего конструктора C#. Например, АКВ для *MainActivity* будет содержать конструктор по умолчанию, который вызовет конструктор по умолчанию *MainActivity*. (Это делается с помощью вызова *типеманажер. Activate ()* в конструкторах АКВ.)

Существует еще одна сигнатура конструктора с обследствием: *(IntPtr, жнихандлеовнершип)* . Конструктор *(IntPtr, жнихандлеовнершип)* вызывается всякий раз, когда объект Java предоставляется управляемому коду, а управляемая обертка должна быть создана для управления дескриптором JNI. Обычно это делается автоматически.

Существует два сценария, в которых конструктор *(IntPtr, жнихандлеовнершип)* должен быть предоставлен вручную в управляемом подклассе вызываемой обертки:

1. [Android. app. Application](xref:Android.App.Application) является подклассом. *Приложение* является специальным; Конструктор по *Applicaton* умолчанию приложения отменено *никогда не* будет вызываться, и [вместо него должен быть указан конструктор (IntPtr, жнихандлеовнершип)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Вызов виртуального метода из конструктора базового класса.

Обратите внимание, что (2) — это абстракция утечки. В Java, как и в C#, вызовы виртуальных методов из конструктора всегда вызывают реализацию самого производного метода. Например, [конструктор TextView (Context, InAttribute, int)](xref:Android.Widget.TextView#ctor*) вызывает виртуальный метод [TextView. жетдефаултмовементмесод ()](https://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), который привязан как [свойство TextView. дефаултмовементмесод](xref:Android.Widget.TextView.DefaultMovementMethod).
Таким образом, если тип [логтекстбокс](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) должен быть (1) [подклассом TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [переопределить TextView. дефаултмовементмесод](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)и (3) [активировать экземпляр этого класса через XML,](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) переопределенное свойство *ДЕФАУЛТМОВЕМЕНТМЕСОД* будет вызываться до того, как конструктор АКВ сможет выполниться, и это произойдет до того, как конструктор C# сможет выполнить.

Это поддерживается путем создания экземпляра Логтекстбокс с помощью конструктора [логтекствиев (IntPtr, жнихандлеовнершип)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) , когда экземпляр АКВ логтекстбокс сначала вводит управляемый код, а затем вызывает конструктор [логтекстбокс (Context, иаттрибутесет, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) *в том же экземпляре* при выполнении конструктора ACW.

Порядок событий:

1. XML-файл макета загружается в [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2. Android создает экземпляр графа объекта макета и создает экземпляр класса *monodroid. ApiDemo. логтекстбокс* , АКВ для *логтекстбокс* .

3. Конструктор *monodroid. ApiDemo. логтекстбокс* выполняет конструктор [Android. widget. TextView](https://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) .

4. Конструктор *TextView* вызывает *monodroid. ApiDemo. логтекстбокс. жетдефаултмовементмесод ()* .

5. *monodroid. ApiDemo. логтекстбокс. жетдефаултмовементмесод ()* вызывает *логтекстбокс. n_getDefaultMovementMethod ()* , который вызывает *TextView. n_GetDefaultMovementMethod ()* , который вызывает [Java. lang. Object. GetObject &lt; TextView &gt; (Handle, жнихандлеовнершип. доноттрансфер)](xref:Java.Lang.Object.GetObject*) .

6. *Java. lang. Object. &lt; GetObject TextView &gt; ()* проверяет, уже существует ли соответствующий экземпляр C# для *обработчика* . Если это так, возвращается значение. В этом сценарии нет, поэтому *Object. GetObject &lt; t &gt; ()* должен создать его.

7. *Object. GetObject &lt; T &gt; ()* ищет конструктор *логтекстбокс (IntPtr, жнихандлеовнешип)* , вызывает его, создает сопоставление между *дескриптором* и созданным экземпляром и возвращает созданный экземпляр.

8. *TextView. n_GetDefaultMovementMethod ()* вызывает метод получения свойства *логтекстбокс. дефаултмовементмесод* .

9. Элемент управления возвращает в конструктор *Android. widget. TextView* , который завершает выполнение.

10. Конструктор *monodroid. ApiDemo. логтекстбокс* выполняет, вызывая *типеманажер. Activate ()* .

11. Конструктор *логтекстбокс (Context, иаттрибутесет, int)* выполняется в *том же экземпляре, который создан в (7)* .

12. Если не удается найти конструктор (IntPtr, Жнихандлеовнершип), будет создано исключение System. MissingMethodException] (xref: System. MissingMethodException).

<a name="Premature_Dispose_Calls"></a>

### <a name="premature-dispose-calls"></a>Преждевременные вызовы Dispose ()

Существует сопоставление между обработчиком JNI и соответствующим экземпляром C#. Java. lang. Object. Dispose () прерывает это сопоставление. Если дескриптор JNI входит в управляемый код после разрыва сопоставления, он выглядит как активация Java, а конструктор *(IntPtr, жнихандлеовнершип)* будет проверяться и вызываться. Если конструктор не существует, будет создано исключение.

Например, при наличии следующего подкласса управляемого обертки обертки:

```csharp
class ManagedValue : Java.Lang.Object {

    public string Value {get; private set;}

    public ManagedValue (string value)
    {
        Value = value;
    }

    public override string ToString ()
    {
        return string.Format ("[Managed: Value={0}]", Value);
    }
}
```

Если мы создаем экземпляр, Dispose () и вызвавшего повторное создание управляемой вызываемой оболочки:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Программа будет выядре:

```shell
E/mono    ( 2906): Unhandled Exception: System.NotSupportedException: Unable to activate instance of type Scratch.PrematureDispose.ManagedValue from native handle 4051c8c8 --->
System.MissingMethodException: No constructor found for Scratch.PrematureDispose.ManagedValue::.ctor(System.IntPtr, Android.Runtime.JniHandleOwnership)
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateProxy (System.Type type, IntPtr handle, JniHandleOwnership transfer) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   --- End of inner exception stack trace ---
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object.GetObject (IntPtr handle, JniHandleOwnership transfer, System.Type type) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object._GetObject[IJavaObject] (IntPtr handle, JniHandleOwnership transfer) [0x00000
```

Если подкласс содержит конструктор *(IntPtr, жнихандлеовнершип)* , то будет создан *Новый* экземпляр типа. В результате экземпляр будет выглядеть как «потеря» всех данных экземпляра, так как это новый экземпляр. (Обратите внимание, что значение равно null.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Только *Dispose ()* управляемых вызываемых подклассов обертки, если известно, что объект Java больше не будет использоваться, или если подкласс не содержит данных экземпляра и предоставлен конструктор *(IntPtr, жнихандлеовнершип)* .

## <a name="application-startup"></a>Запуск приложения

Когда запускается действие, служба и т. д., Android сначала проверяет наличие процесса, выполняющегося для размещения действия или службы, а также т. д. Если такого процесса не существует, создается новый процесс, считывается [AndroidManifest. XML](https://developer.android.com/guide/topics/manifest/manifest-intro.html) и [/manifest/application/@android:name](https://developer.android.com/guide/topics/manifest/application-element.html#nm) загружается и создается экземпляр типа, указанный в атрибуте. Далее создается экземпляр всех типов, заданных [/manifest/application/provider/@android:name](https://developer.android.com/guide/topics/manifest/provider-element.html#nm) значениями атрибутов, и вызывается метод [ContentProvider. аттачинфо %28)](xref:Android.Content.ContentProvider.AttachInfo*) . Для этого в Xamarin. Android можно добавить *Mono. MonoRuntimeProvider* *ContentProvider* в AndroidManifest. XML во время процесса сборки. *Моно. Метод MonoRuntimeProvider. Аттачинфо ()* отвечает за загрузку среды выполнения Mono в процесс.
Все попытки использовать Mono до этого момента завершатся ошибкой. ( *Примечание*. Именно поэтому типы, которые подклассировать [Android. app. Application](xref:Android.App.Application) , должны предоставить [конструктор (IntPtr, жнихандлеовнершип)](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), так как экземпляр приложения создается до инициализации Mono.)

После завершения инициализации процесса `AndroidManifest.xml` осуществляется поиск имени класса действия или службы или т. д. для запуска. Например, [ /manifest/application/activity/@android:name атрибут](https://developer.android.com/guide/topics/manifest/activity-element.html#nm) используется для определения имени действия, которое необходимо загрузить. Для действий этот тип должен наследовать [Android. app. Activity](xref:Android.App.Activity).
Указанный тип загружается с помощью [класса. форнаме ()](https://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (который требует, чтобы тип был типом Java, поэтому вызываемые оболочки Android), а затем создается экземпляр. Создание экземпляра вызываемой оболочки Android вызовет создание экземпляра соответствующего типа C#. Затем Android вызовет [действие. OnCreate (набор)](https://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , которое приведет к вызову соответствующего [действия. OnCreate (набор)](xref:Android.App.Activity.OnCreate*) , и вы не будете состязания.
