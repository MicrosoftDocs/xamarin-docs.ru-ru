---
title: Вызываемые программы-оболочки Android для Xamarin.Android
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: ef2f8e0375786ba7b627fdf75545cbb48318c1aa
ms.sourcegitcommit: 854798de42566750d9c70b6d0539b7ee73ff6ddc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/20/2020
ms.locfileid: "81646605"
---
# <a name="android-callable-wrappers-for-xamarinandroid"></a>Вызываемые программы-оболочки Android для Xamarin.Android

Вызываемые оболочки Android (ACW) требуются при каждом вызове управляемого кода средой выполнения Android. Эти оболочки являются обязательными, так как в среде выполнения не существует способа регистрации классов с помощью ART (среда выполнения Android). (В частности, функция [JNI DefineClass()](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) не поддерживается средой выполнения Android.} Таким образом вызываемые оболочки Android компенсируют отсутствие поддержки регистрации типа среды выполнения. 

Код Android должен *каждый раз* выполнять `virtual` или метод интерфейса, который является `overridden` или реализован в управляемом коде, а Xamarin.Android — предоставить прокси-сервер Java, чтобы этот метод был отправлен в соответствующий управляемый тип. Эти типы прокси Java — это код Java, имеющий тот же базовый класс и список интерфейсов Java, что и управляемый тип, реализующий те же конструкторы и объявляющий любые переопределенные базовые классы и методы интерфейса. 

Вызываемые оболочки Android создаются программой **monodroid.exe** во время [процесса сборки](~/android/deploy-test/building-apps/build-process.md): они создаются для всех типов, которые (прямо или косвенно) наследуют [Java.Lang.Object](xref:Java.Lang.Object). 

## <a name="android-callable-wrapper-naming"></a>Имя вызываемой программы-оболочки

Имена пакетов для вызываемых оболочек Android основаны на MD5SUM имени экспортируемого типа с указанием сборки. Такая методика именования позволяет сделать то же полное имя типа доступным для разных сборок без введения ошибки упаковки. 

Из-за этой схемы именования MD5SUM вы не можете напрямую обращаться к типам по имени. Например, следующая команда `adb` не будет работать, так как имя типа `my.ActivityType` не создается по умолчанию: 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

Кроме того, если вы попытаетесь сослаться на тип по имени, вы можете заметить следующие ошибки:

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Если вы *требуете* доступ к типам по имени, имя для этого типа можно объявить в объявлении атрибута. Например, ниже приведен код, объявляющий действие с полным именем `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Для свойства `ActivityAttribute.Name` можно задать явное объявление имени этого действия: 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

После добавления этого параметра свойства `my.ActivityType` к нему можно получить доступ по имени из внешнего кода и из сценариев `adb`. Атрибут `Name` можно задать для множества различных типов, включая `Activity`, `Application`, `Service`, `BroadcastReceiver` и `ContentProvider`. 

- [ActivityAttribute.Name](xref:Android.App.ActivityAttribute.Name)
- [ApplicationAttribute.Name](xref:Android.App.ApplicationAttribute.Name)
- [ServiceAttribute.Name](xref:Android.App.ServiceAttribute.Name)
- [BroadcastReceiverAttribute.Name](xref:Android.Content.BroadcastReceiverAttribute.Name)
- [ContentProviderAttribute.Name](xref:Android.Content.ContentProviderAttribute.Name)

В Xamarin.Android 5.0 введено именование ACW на базе MD5SUM. Дополнительные сведения об именовании атрибутов см. в [RegisterAttribute](xref:Android.Runtime.RegisterAttribute). 

## <a name="implementing-interfaces"></a>Реализация интерфейсов

Иногда может потребоваться реализовать интерфейс Android, например [Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks). Поскольку все классы и интерфейсы Android расширяют интерфейс [Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject), возникает вопрос: как реализовать `IJavaObject`? 

Ответ на этот вопрос дан выше: причина, по которой все типы Android должны реализовать `IJavaObject`, заключается в том, что Xamarin.Android имеет вызываемую программу-оболочку Android для предоставления Android, т. е. прокси-сервер Java для данного типа. Поскольку **monodroid.exe** ищет только подклассы `Java.Lang.Object`, а `Java.Lang.Object` реализует `IJavaObject`, ответ очевиден: подкласс `Java.Lang.Object`: 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```

## <a name="implementation-details"></a>Сведения о реализации

*Оставшаяся часть этой страницы предоставляет сведения о реализации, которые могут быть изменены без предварительного уведомления* (и предоставляются здесь только потому, что разработчикам интересно, как это происходит). 

Например, при наличии следующего источника C#:

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

Программа **mandroid.exe** создаст следующую вызываемую оболочку Android: 

```java
package mono.samples.helloWorld;

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Обратите внимание, что базовый класс сохраняется, и для каждого метода, переопределяемого в управляемом коде, предоставляются объявления методов `native`. 
