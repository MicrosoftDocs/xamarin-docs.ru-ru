---
title: Компоновка в Android
ms.prod: xamarin
ms.assetid: 3528E195-AA74-90AF-B5F3-3B65FB4F0BB8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/30/2018
ms.openlocfilehash: 3c42743a26ab4ec5388c1d3458d88ef4d161a1f3
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454849"
---
# <a name="linking-on-android"></a>Компоновка в Android

На платформе Xamarin.Android используется *компоновщик* для уменьшения размера приложений. Компоновщик применяет статический анализ приложения и определяет, какие сборки, типы и члены фактически используются приложением. Компоновщик применяет алгоритм *сборщика мусора*, проверяя все ссылки на сборки, типы и члены, пока не составит полный список используемых сборок, типов, и найти элементы. Затем он *удаляет* все, что осталось за пределом этого списка.

Давайте рассмотрим этот механизм на примере приложения [Привет, Android](/samples/xamarin/monodroid-samples/hellom4a).

|Конфигурация|Размер 1.2.0|Размер 4.0.1|
|---|---|---|
|Сборка выпуска без компоновщика|14.0 МБ|16.0 МБ|
|Сборка выпуска с компоновщиком|4.2 МБ|2.9 МБ|

Компоновка позволяет получить пакет, размер которого составляет около 30% от исходного размера пакета (без компоновщика) для версии 1.2.0 и всего 18% для версии 4.0.1.

## <a name="control"></a>Control

Работа компоновщика основана на *статического анализа*. Следовательно, он не сможет обнаружить зависимости, определяемые средой выполнения.

```csharp
// To play along at home, Example must be in a different assembly from MyActivity.
public class Example {
    // Compiler provides default constructor...
}

[Activity (Label="Linker Example", MainLauncher=true)]
public class MyActivity {
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Will this work?
        var o = Activator.CreateInstance (typeof (ExampleLibrary.Example));
    }
}
```

### <a name="linker-behavior"></a>Поведение компоновщика

Основным механизмом для управления компоновщиком является раскрывающийся список **Поведение компоновщика** (или *Компоновка* в Visual Studio), который размещен в диалоговом окне **Параметры проекта**. Доступны три параметра:

1. **Не компоновать** (или *Нет* в Visual Studio)
1. **Компоновать сборки пакета SDK** (*Только сборки пакета SDK*)
1. **Компоновать все сборки** (*Сборки пакета SDK и пользователя*)

Вариант **Не компоновать** отключает компоновщик. Сборка, размер которой приводится в графе "Сборка выпуска без компоновщика", была получена именно с этим значением параметра. Он может оказаться полезен при отладке ошибок среды выполнения, чтобы подтвердить или исключить проблемы компоновщика. Обычно для производственных сборок этот вариант применять не следует.

Вариант **Компоновать сборки пакета SDK** отбирает для компоновки только [сборки, входящие в состав Xamarin.Android](~/cross-platform/internals/available-assemblies.md).
Все остальные сборки (например, в вашем коде) компоновке не подвергаются.

Вариант **Компоновать все сборки** применяет компоновку ко всем сборкам, то есть может удалить даже фрагменты вашего кода, на которые нет статических ссылок.

Приведенный выше пример будет нормально работа в режимах *Не компоновать* и *Компоновать сборки пакета SDK*, но при выборе варианта *Компоновать все сборки* будет завершаться сбоем со следующей ошибкой:

```shell
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): UNHANDLED EXCEPTION: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type,bool) <0x00180>
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type) <0x00017>
I/MonoDroid(17755): at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle) <0x00027>
I/MonoDroid(17755): at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (intptr,intptr,intptr) <0x00057>
I/MonoDroid(17755): at (wrapper dynamic-method) object.95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr) <0x00033>
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):
E/mono    (17755): Unhandled Exception: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type, Boolean nonPublic) [0x00000] in <filename unknown>:0
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type) [0x00000] in <filename unknown>:0
E/mono    (17755):   at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle bundle) [0x00000] in <filename unknown>:0
E/mono    (17755):   at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (IntPtr jnienv, IntPtr native__this, IntPtr native_savedInstanceState) [0x00000] in <filename unknown>:0
E/mono    (17755):   at (wrapper dynamic-method) object:95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr)
```

### <a name="preserving-code"></a>Сохранение кода

Иногда компоновщик удаляет код, который нужно сохранить. Пример:

- Возможно, код вызывается динамически через `System.Reflection.MemberInfo.Invoke`.

- Если вы динамически создаете экземпляры типов, нужно сохранять в коде конструктор по умолчанию для этих типов.

- Если используется XML-сериализация, нужно сохранять свойства типов.

Во всех таких случаях вам пригодится атрибут [Android.Runtime.Preserve](xref:Android.Runtime.PreserveAttribute). Все члены, на которые нет статических ссылок из приложения, подлежат удалению. Этот атрибут позволяет отметить те члены, которые должны сохраняться в приложении даже при отсутствии статических ссылок. Этот атрибут можно применить к любому члену типа или к самому типу.

В следующем примере мы используем этот атрибут, чтобы сохранить конструктор класса `Example`:

```csharp
public class Example
{
    [Android.Runtime.Preserve]
    public Example ()
    {
    }
}
```

Если вы хотите сохранить весь тип, используйте следующий синтаксис:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
```

Например, в следующем фрагменте года сохраняется весь класс `Example`, чтобы использовать его для сериализации XML:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
class Example
{
    // Compiler provides default constructor...
}
```

Иногда нужно сохранять определенные члены при условии, что содержащий их тип уже сохранен. В таких случаях используйте следующий синтаксис:

```csharp
[Android.Runtime.Preserve (Conditional = true)]
```

Если вы хотите избавиться от зависимостей от библиотек Xamarin &ndash; например, при разработке межплатформенной переносимой библиотеки классов (PCL) &ndash; вы также можете использовать атрибут `Android.Runtime.Preserve`. Для этого объявите класс `PreserveAttribute` в пространстве имен `Android.Runtime` следующим образом:

```csharp
namespace Android.Runtime
{
    public sealed class PreserveAttribute : System.Attribute
    {
        public bool AllMembers;
        public bool Conditional;
    }
}
```

В примерах выше атрибут `Preserve` объявляется в пространстве имен `Android.Runtime`, однако вы можете использовать атрибут `Preserve` в любом пространстве, так как компоновщик распознает этот атрибут по имени типа.

### <a name="falseflag"></a>falseflag

Если атрибут [Preserve] использовать невозможно, можно создать специальный неисполняемый блок кода, благодаря которому компоновщик посчитает, что нужный вам тип используется. Например, такой подход можно реализовать так:

```csharp
[Activity (Label="Linker Example", MainLauncher=true)]
class MyActivity {

#pragma warning disable 0219, 0649
    static bool falseflag = false;
    static MyActivity ()
    {
        if (falseflag) {
            var ignore = new Example ();
        }
    }
#pragma warning restore 0219, 0649

    // ...
}
```

### <a name="linkskip"></a>linkskip

Также можно указать, что к некоторому набору пользовательских сборок компоновка не применяется вовсе, сохраняя возможность исключать другие пользовательские сборки. Для этого выберите вариант поведения *Компоновать сборки пакета SDK* и установите свойство [AndroidLinkSkip MSBuild](~/android/deploy-test/building-apps/build-process.md) следующим образом:

```xml
<PropertyGroup>
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
</PropertyGroup>
```

### <a name="linkdescription"></a>LinkDescription

[`@(LinkDescription)`](~/android/deploy-test/building-apps/build-process.md)
**Действие сборки** можно применять с файлами, разместив в них [файл пользовательской конфигурации компоновщика](~/cross-platform/deploy-test/linker.md).
RDL-файл. Файлы пользовательской конфигурации компоновщика позволяют сохранить нужные элементы `internal` и (или) `private`.

### <a name="custom-attributes"></a>Настраиваемые атрибуты

При компоновке сборки из всех элементов удаляются пользовательские атрибуты следующих типов:

- System.ObsoleteAttribute
- System.MonoDocumentationNoteAttribute
- System.MonoExtensionAttribute
- System.MonoInternalNoteAttribute
- System.MonoLimitationAttribute
- System.MonoNotSupportedAttribute
- System.MonoTODOAttribute
- System.Xml.MonoFIXAttribute

При компоновке сборки для сборки выпуска из всех элементов удаляются пользовательские атрибуты следующих типов:

- System.Diagnostics.DebuggableAttribute
- System.Diagnostics.DebuggerBrowsableAttribute
- System.Diagnostics.DebuggerDisplayAttribute
- System.Diagnostics.DebuggerHiddenAttribute
- System.Diagnostics.DebuggerNonUserCodeAttribute
- System.Diagnostics.DebuggerStepperBoundaryAttribute
- System.Diagnostics.DebuggerStepThroughAttribute
- System.Diagnostics.DebuggerTypeProxyAttribute
- System.Diagnostics.DebuggerVisualizerAttribute

## <a name="related-links"></a>Связанные ссылки

- [Пользовательская конфигурация компоновщика](~/cross-platform/deploy-test/linker.md)
- [Компоновка в iOS](~/ios/deploy-test/linker.md)