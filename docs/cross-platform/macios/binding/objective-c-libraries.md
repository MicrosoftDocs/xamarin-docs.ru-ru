---
title: Цель привязки-библиотеки C
description: В этом документе представлен общий обзор создания привязок C# к коду цели-C, описание способов привязки событий, методов, пользовательских элементов управления и многого другого.
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: davidortinau
ms.author: daortin
ms.date: 03/06/2018
ms.openlocfilehash: 6508f7ec48d10196b47e0b51fc30e54e3c3668a6
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930563"
---
# <a name="binding-objective-c-libraries"></a>Цель привязки-библиотеки C

При работе с Xamarin. iOS или Xamarin. Mac могут возникнуть ситуации, когда требуется использовать сторонние библиотеки цели-C. В таких ситуациях можно использовать проекты привязки Xamarin, чтобы создать привязку C# к библиотекам машинного задания на языке C. В проекте используются те же средства, которые используются для переноса API-интерфейсов iOS и Mac в C#.

В этом документе описывается, как привязать интерфейсы API цели-C. при привязке только интерфейсов API C следует использовать стандартный механизм .NET для этого, а [также платформу P/Invoke](https://www.mono-project.com/docs/advanced/pinvoke/).
Сведения о статической компоновке библиотеки C доступны на странице « [связывание машинных библиотек](~/ios/platform/native-interop.md) ».

См. [справочное руководство по сопутствующим типам привязки](~/cross-platform/macios/binding/binding-types-reference.md).
Кроме того, если вы хотите узнать больше о том, что происходит, см. страницу [обзора привязок](~/cross-platform/macios/binding/overview.md) .

Привязки можно создавать для библиотек iOS и Mac.
На этой странице описывается работа с привязкой iOS, однако привязки для Mac очень похожи.

**Пример кода для iOS**

Для экспериментов с привязками можно использовать [образец проекта привязки iOS](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) .

<a name="Getting_Started"></a>

## <a name="getting-started"></a>Начало работы

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Самый простой способ создать привязку — создать проект привязки Xamarin. iOS.
Это можно сделать из Visual Studio для Mac, выбрав тип проекта, Библиотека **привязок > библиотеки iOS >**.

[![Сделайте это из Visual Studio для Mac, выбрав тип проекта "Библиотека привязок библиотеки iOS"](objective-c-libraries-images/00-sml.png)](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Самый простой способ создать привязку — создать проект привязки Xamarin. iOS.
Это можно сделать в Visual Studio в Windows, выбрав тип проекта, **Visual C# > библиотека привязок > iOS (IOS)**:

[![Библиотека привязок iOS, iOS](objective-c-libraries-images/00vs-sml.png)](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Примечание. проекты привязки для **Xamarin. Mac** поддерживаются только в Visual Studio для Mac.

-----

Созданный проект содержит небольшой шаблон, который можно изменить, он содержит два файла: `ApiDefinition.cs` и `StructsAndEnums.cs` .

Именно `ApiDefinition.cs` здесь вы определяете контракт API, это файл, который описывает, как базовый API цели-C проецируется на C#. Синтаксис и содержимое этого файла являются основной темой обсуждения этого документа, и его содержимое ограничено интерфейсами C# и объявлениями делегатов C#. `StructsAndEnums.cs`Файл — это файл, в который будут входить все определения, необходимые для интерфейсов и делегатов. Сюда входят значения перечисления и структуры, которые может использовать ваш код.

<a name="Binding_an_API"></a>

## <a name="binding-an-api"></a>Привязка API

Чтобы выполнить комплексную привязку, необходимо ознакомиться с определением API цели-C и ознакомиться с рекомендациями по проектированию .NET Framework.

Чтобы привязать библиотеку, как правило, вы начинаете с файла определения API. Файл определения API — это просто исходный файл C#, содержащий интерфейсы C#, которые снабжены несколькими атрибутами, помогающими управлять привязкой.  Этот файл определяет, что такое контракт между C# и цели-C.

Например, это тривиальный файл API для библиотеки:

```csharp
using Foundation;

namespace Cocos2D {
  [BaseType (typeof (NSObject))]
  interface Camera {
    [Static, Export ("getZEye")]
    nfloat ZEye { get; }

    [Export ("restore")]
    void Restore ();

    [Export ("locate")]
    void Locate ();

    [Export ("setEyeX:eyeY:eyeZ:")]
    void SetEyeXYZ (nfloat x, nfloat y, nfloat z);

    [Export ("setMode:")]
    void SetMode (CameraMode mode);
  }
}
```

В приведенном выше примере определяется класс `Cocos2D.Camera` , производный от `NSObject` базового типа (от которого берется этот тип `Foundation.NSObject` ), который определяет статическое свойство ( `ZEye` ), два метода, которые не принимают аргументы и метод, принимающий три аргумента.

Подробное описание формата файла API и атрибутов, которые можно использовать, описано в разделе [файл определения API](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) ниже.

Чтобы создать полную привязку, вы обычно имеете дело с четырьмя компонентами:

- Файл определения API ( `ApiDefinition.cs` в шаблоне).
- Необязательно: любые перечисления, типы, структуры, необходимые для файла определения API ( `StructsAndEnums.cs` в шаблоне).
- Необязательно. Дополнительные источники, которые могут расширить созданную привязку или предоставить более удобный API-интерфейс C# (все файлы C#, добавленные в проект).
- Собственная библиотека, которая привязывается.

На этой диаграмме показана связь между файлами:

 [![На этой диаграмме показана связь между файлами](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png)](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Файл определения API будет содержать только пространства имен и определения интерфейсов (с любыми членами, которые может содержать интерфейс) и не должны содержать классы, перечисления, делегаты или структуры. Файл определения API — это просто контракт, который будет использоваться для создания API.

Любой дополнительный код, который требуется, как перечисления или вспомогательные классы, должен размещаться в отдельном файле, в примере выше "Камерамоде" — это значение перечисления, которое не существует в файле CS и должно размещаться в отдельном файле, например `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs`Файл объединяется с `StructsAndEnum` классом и используется для создания базовой привязки библиотеки. Вы можете использовать итоговую библиотеку "как есть", но как правило, вам потребуется настроить итоговую библиотеку, чтобы добавить некоторые функции C# для преимуществ пользователей. Некоторые примеры включают реализацию `ToString()` метода, предоставление индексаторов C#, добавление неявных преобразований в некоторые машинные типы и из некоторых собственных типов или предоставление строго типизированных версий некоторых методов. Эти улучшения хранятся в дополнительных файлах C#. Просто добавьте файлы C# в проект, и они будут включаться в этот процесс сборки.

В этом примере показано, как реализовать код в `Extra.cs` файле. Обратите внимание, что вы будете использовать разделяемые классы, так как они дополняют разделяемые классы, создаваемые из сочетания `ApiDefinition.cs` `StructsAndEnums.cs` базовой привязки и.

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Сборка библиотеки приведет к созданию собственной привязки.

Чтобы выполнить эту привязку, необходимо добавить в проект собственную библиотеку.  Это можно сделать, добавив собственную библиотеку в проект, перетащив собственную библиотеку из поиска в проект в обозревателе решений или щелкнув правой кнопкой мыши проект и выбрав **Добавить**  >  **Добавить файлы** , чтобы выбрать собственную библиотеку.
Собственные библиотеки по соглашению начинаются с слова «lib» и заканчиваются расширением «. a». После этого Visual Studio для Mac добавит два файла:. a и автоматически заполняемый файл C#, содержащий сведения о том, что содержит собственная библиотека:

 [![Собственные библиотеки по соглашению начинаются с слова lib и заканчиваются расширением. a](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png)](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Содержимое этого `libMagicChord.linkwith.cs` файла содержит сведения о том, как можно использовать эту библиотеку, и указывает интегрированной среде разработки упаковать этот двоичный файл в итоговый DLL:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Полные сведения об использовании[`[LinkWith]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) 
атрибуты описаны в [справочном руководстве по типам привязки](~/cross-platform/macios/binding/binding-types-reference.md).

Теперь при построении проекта вы получите `MagicChords.dll` файл, содержащий как привязку, так и собственную библиотеку. Вы можете распространить этот проект или итоговую библиотеку DLL другим разработчикам для их собственного использования.

Иногда может оказаться, что вам потребуется несколько значений перечисления, определений делегатов или других типов. Не размещайте их в файле определений API, так как это просто контракт

<a name="The_API_definition_file"></a>

## <a name="the-api-definition-file"></a>Файл определения API

Файл определения API состоит из нескольких интерфейсов. Интерфейсы в определении API будут преобразованы в объявление класса и должны быть снабжены [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) атрибутом, чтобы указать базовый класс для класса.

Возможно, вас интересует, почему мы не использовали классы вместо интерфейсов для определения контракта. Мы выбрали интерфейсы, так как позволяли написать контракт для метода без необходимости указания тела метода в файле определения API или при необходимости указать текст, который должен был вызвать исключение или вернуть осмысленное значение.

Но поскольку мы используем интерфейс в качестве каркаса для создания класса, нам пришлось бы попытаться заменять различные части контракта атрибутами для привязки.

<a name="Binding_Methods"></a>

### <a name="binding-methods"></a>Методы привязки

Простейший способ привязки — привязать метод. Просто объявите метод в интерфейсе с соглашениями об именовании C# и дополните метод атрибутом[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
. [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)Атрибут — это ссылка на имя C# с именем цели-C в среде выполнения Xamarin. iOS. Параметр[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
атрибут — это имя селектора цели-C. Некоторые примеры.

```csharp
// A method, that takes no arguments
[Export ("refresh")]
void Refresh ();

// A method that takes two arguments and return the result
[Export ("add:and:")]
nint Add (nint a, nint b);

// A method that takes a string
[Export ("draw:atColumn:andRow:")]
void Draw (string text, nint column, nint row);
```

В приведенных выше примерах показано, как можно привязать методы экземпляра. Чтобы привязать статические методы, необходимо использовать [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) атрибут следующим образом:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Это необходимо потому, что контракт является частью интерфейса, и интерфейсы не имеют представления об объявлениях статических экземпляров VS, поэтому необходимо еще раз прибегнуть к атрибутам. Если требуется скрыть определенный метод из привязки, можно снабдить метод [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) атрибутом.

`btouch-native`Команда предложит проверить, чтобы ссылочные параметры не были пустыми. Если вы хотите разрешить значения NULL для определенного параметра, используйте[`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
в параметре следующим образом:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

При экспорте ссылочного типа с [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) ключевым словом можно также указать семантику выделения. Это необходимо, чтобы избежать утечки данных.

<a name="Binding_Properties"></a>

### <a name="binding-properties"></a>Свойства привязки

Как и методы, свойства цели-C привязываются с помощью[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)
и сопоставляются непосредственно со свойствами C#. Как и методы, свойства можно снабдить атрибутом[`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute)
и[`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
.

При использовании [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) атрибута в свойстве внутри btouch-Native фактически привязывает два метода: метод получения и метод задания. Имя, которое вы задаете для экспорта, является **базовым** именем, а метод задания выдается при помощи слова «Set», который преобразует первую букву **basename** в верхний регистр и делает селектор занимать аргумент. Это означает, что `[Export ("label")]` при применении к свойству фактически привязываются методы цели-C "Label" и "setLabel:".

Иногда свойства цели-C не следуют описанному выше шаблону и имя перезаписывается вручную. В этих случаях можно управлять способом создания привязки с помощью[`[Bind]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) 
атрибут для метода получения или задания, например:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Затем привязывает "Исменувисибле" и "Сетменувисибле:". При необходимости свойство можно привязать с помощью следующего синтаксиса:

```csharp
[Category, BaseType(typeof(UIView))]
interface UIView_MyIn
{
  [Export ("name")]
  string Name();

  [Export("setName:")]
  void SetName(string name);
}
```

Где методы получения и задания явно определены в `name` `setName` привязках и выше.

Помимо поддержки статических свойств с помощью [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) , можно снабдить статические свойства потока [`[IsThreadStatic]`](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute) , например:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Так же, как и методы, можно помечать некоторые параметры с помощью [`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) , вы можете применить[`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)
для свойства, чтобы указать, что значение null является допустимым значением для свойства, например:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[`[NullAllowed]`](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute)Параметр также может быть указан непосредственно в методе задания:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Предостережения о привязке пользовательских элементов управления

При настройке привязки для пользовательского элемента управления следует учитывать следующие предостережения.

1. **Свойства привязки должны быть статическими** . при определении привязки свойств [`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) необходимо использовать атрибут.
2. **Имена свойств должны точно совпадать** — имя, используемое для привязки свойства, должно точно совпадать с именем свойства в пользовательском элементе управления.
3. **Типы свойств должны точно соответствовать** . тип переменной, используемой для привязки свойства, должен точно совпадать с типом свойства в пользовательском элементе управления.
4. **Точки останова и метод считывания и задания** — точки останова, помещаемые в методы получения или задания свойства, никогда не будут достигнуты.
5. **Обратите внимание на обратные вызовы** — необходимо использовать обратные вызовы наблюдения, чтобы получать уведомления об изменениях в значениях свойств пользовательских элементов управления.

Несоблюдение приведенных выше предупреждений может привести к тому, что привязка автоматически завершится сбоем во время выполнения.

<a name="MutablePattern"></a>

#### <a name="objective-c-mutable-pattern-and-properties"></a>Изменяемый шаблон и свойства цели-C

Платформы цели-C используют идиому, в которой некоторые классы являются неизменяемыми с изменяемым подклассом. Например `NSString` , неизменяемая версия, а `NSMutableString` является подклассом, допускающим изменение.

В этих классах обычно видно, что неизменяемый базовый класс содержит свойства со свойством Getter, но не имеет метода задания. И для изменяемой версии, чтобы ввести метод задания. Поскольку это не является возможным в C#, нам пришлось сопоставлять эту идиому с идиомом, который будет работать с C#.

Способ сопоставления этого кода с C# заключается в добавлении методов getter и Setter в базовый класс, но при этом метод задания помечается свойством[`[NotImplemented]`](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute)
.

Затем в изменяемом подклассе используется[`[Override]`](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) 
для свойства, чтобы убедиться, что свойство фактически переопределяет поведение родителя.

Пример.

```csharp
[BaseType (typeof (NSObject))]
interface MyTree {
    string Name { get; [NotImplemented] set; }
}

[BaseType (typeof (MyTree))]
interface MyMutableTree {
    [Override]
    string Name { get; set; }
}
```

<a name="Binding_Constructors"></a>

### <a name="binding-constructors"></a>Конструкторы привязки

`btouch-native`Средство автоматически создает четыре конструктора в классе для данного класса `Foo` , который создает:

- `Foo ()`: конструктор по умолчанию (сопоставляется с конструктором "Init" цели-C)
- `Foo (NSCoder)`: конструктор, используемый при десериализации файлов NIB (сопоставляется с конструктором "Инитвискодер:" цели-C).
- `Foo (IntPtr handle)`: конструктор для создания на основе маркеров, который вызывается средой выполнения, когда среде выполнения необходимо предоставить управляемый объект из неуправляемого объекта.
- `Foo (NSEmptyFlag)`: используется производными классами для предотвращения двойной инициализации.

Для конструкторов, определяемых пользователем, их необходимо объявлять с помощью следующей сигнатуры внутри определения интерфейса: они должны возвращать `IntPtr` значение, а имя метода должно быть конструктором. Например, чтобы привязать `initWithFrame:` конструктор, нужно использовать следующее:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols"></a>

### <a name="binding-protocols"></a>Протоколы привязки

Как описано в документе по проектированию API, в разделе, [посвященном моделям и протоколам](~/ios/internals/api-design/index.md#models), Xamarin. iOS сопоставляет протоколы цели-C с классами, помеченными[`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute)
. Обычно это используется при реализации классов делегата цели-C.

Большая разница между обычным привязанным классом и классом делегата заключается в том, что класс делегата может иметь один или несколько необязательных методов.

Например `UIKit` , рассмотрим класс `UIAccelerometerDelegate` , так как он привязан в Xamarin. iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Так как это необязательный метод определения, `UIAccelerometerDelegate` никаких других действий не требуется. Но при наличии необходимого метода для протокола следует добавить[`[Abstract]`](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute)
атрибут для метода. При этом пользователь реализации будет вынужден предоставить текст для метода.

Как правило, протоколы используются в классах, которые реагируют на сообщения. Обычно это делается в цели-C путем назначения свойству "Delegate" экземпляра объекта, отвечающего на методы протокола.

Соглашение в Xamarin. iOS заключается в поддержке слабо связанного стиля цели-C, где любой экземпляр `NSObject` может быть назначен делегату, а также для предоставления строго типизированной версии. По этой причине мы обычно предоставляем как `Delegate` свойство со строгой типизацией, так и `WeakDelegate` слабо типизированный. Как правило, привязывает слабо типизированную версию с [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) , и мы используем [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) атрибут для предоставления строго типизированной версии.

Это показывает, как мы привязаны к `UIAccelerometer` классу:

```csharp
[BaseType (typeof (NSObject))]
interface UIAccelerometer {
        [Static] [Export ("sharedAccelerometer")]
        UIAccelerometer SharedAccelerometer { get; }

        [Export ("updateInterval")]
        double UpdateInterval { get; set; }

        [Wrap ("WeakDelegate")]
        UIAccelerometerDelegate Delegate { get; set; }

        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }
}
```

<a name="iOS7ProtocolSupport"></a>

**Новое в коtouch 7,0**

Начиная с коtouch 7,0 была включена новая и улучшенная функциональность привязки протокола.  Эта новая поддержка упрощает использование идиом объектив-C для принятия одного или нескольких протоколов в заданном классе.

Для каждого определения протокола `MyProtocol` в цели-C теперь существует `IMyProtocol` интерфейс, который перечисляет все необходимые методы из протокола, а также класс расширения, который предоставляет все необязательные методы.  Приведенная выше поддержка в сочетании с новой поддержкой в редакторе Xamarin Studio позволяет разработчикам реализовывать методы протокола, не прибегая к использованию отдельных подклассов предыдущих классов абстрактной модели.

Любое определение, содержащее [`[Protocol]`](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) атрибут, на самом деле создает три поддерживающих класса, что значительно улучшает способ использования протоколов:

```csharp
// Full method implementation, contains all methods
class MyProtocol : IMyProtocol {
    public void Say (string msg);
    public void Listen (string msg);
}

// Interface that contains only the required methods
interface IMyProtocol: INativeObject, IDisposable {
    [Export ("say:")]
    void Say (string msg);
}

// Extension methods
static class IMyProtocol_Extensions {
    public static void Optional (this IMyProtocol this, string msg);
    }
}
```

**Реализация класса** предоставляет полный абстрактный класс, который позволяет переопределять отдельные методы и получать полную безопасность типов.  Но из-за того, что в C# не поддерживалось множественное наследование, существуют сценарии, в которых может потребоваться другой базовый класс, но вы все равно хотите реализовать интерфейс, в котором

Созданное **Определение интерфейса** поступает в.  Это интерфейс, который имеет все необходимые методы из протокола.  Это позволяет разработчикам, которые хотят реализовать ваш протокол, просто реализовать интерфейс.  Среда выполнения автоматически зарегистрирует тип в качестве внедрения протокола.

Обратите внимание, что интерфейс содержит только необходимые методы и предоставляет необязательные методы.  Это означает, что классы, которые принимают протокол, получат полную проверку типов для требуемых методов, но придется прибегнуть к нестрогой типизации (вручную с помощью[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
атрибуты и соответствующие сигнатуры) для необязательных методов протокола.

Для удобства использования API, использующего протоколы, средство привязки также создаст класс метода Extensions, который предоставляет все необязательные методы.  Это означает, что пока вы используете API, вы сможете рассматривать протоколы как все методы.

Если вы хотите использовать определения протоколов в API, вам потребуется написать скелет пустых интерфейсов в определении API.  Если вы хотите использовать Мипротокол в API, необходимо сделать следующее:

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

Выше требуется, так как во время привязки `IMyProtocol` не существовало, поэтому необходимо предоставить пустой интерфейс.

#### <a name="adopting-protocol-generated-interfaces"></a>Внедрение интерфейсов, созданных протоколом

Каждый раз при реализации одного из интерфейсов, созданных для протоколов, например:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Реализация методов интерфейса автоматически экспортируется с соответствующим именем, поэтому она эквивалентна следующей:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Не имеет значения, реализован ли интерфейс неявно или явно.

<a name="Binding_Class_Extensions"></a>

### <a name="binding-class-extensions"></a>Расширения классов привязки

В цели-C можно расширять классы с помощью новых методов, аналогично методам расширения C#. При наличии одного из этих методов можно использовать[`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
атрибут, помечающий метод как получатель сообщения цели-C.

Например, в Xamarin. iOS мы привязываете методы расширения, определенные в `NSString` , когда, `UIKit` импортируются как методы в `NSStringDrawingExtensions` , как показано ниже:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists"></a>

### <a name="binding-objective-c-argument-lists"></a>Цели привязки — списки аргументов C

Цель-C поддерживает аргументы Variadic. Пример:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Чтобы вызвать этот метод из C#, нужно создать сигнатуру следующим образом:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

Он объявляет метод внутренним, скрывая приведенный выше API от пользователей, но предоставляя его библиотеке. Затем можно написать метод следующим образом:

```csharp
public void AppendWorkers(params Worker[] workers)
{
    if (workers == null)
         throw new ArgumentNullException ("workers");

    var pNativeArr = Marshal.AllocHGlobal(workers.Length * IntPtr.Size);
    for (int i = 1; i < workers.Length; ++i)
        Marshal.WriteIntPtr (pNativeArr, (i - 1) * IntPtr.Size, workers[i].Handle);

    // Null termination
    Marshal.WriteIntPtr (pNativeArr, (workers.Length - 1) * IntPtr.Size, IntPtr.Zero);

    // the signature for this method has gone from (IntPtr, IntPtr) to (Worker, IntPtr)
    WorkerManager.AppendWorkers(workers[0], pNativeArr);
    Marshal.FreeHGlobal(pNativeArr);
}
```

<a name="Binding_Fields"></a>

### <a name="binding-fields"></a>Привязка полей

Иногда требуется доступ к открытым полям, объявленным в библиотеке.

Обычно эти поля содержат значения строк или целочисленных значений, на которые необходимо ссылаться. Они обычно используются в качестве строки, представляющей конкретное уведомление, и в качестве ключей в словарях.

Чтобы привязать поле, добавьте свойство в файл определения интерфейса и снабдите свойство [`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) атрибутом. Этот атрибут принимает один параметр: имя C символа для поиска. Пример:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Если необходимо заключить различные поля в статический класс, который не является производным от `NSObject` , можно использовать[`[Static]`](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) 
в классе следующим образом:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Приведенный выше параметр создаст объект, `LonelyClass` который не является производным от `NSObject` и будет содержать привязку к `NSSomeEventNotification` 
 `NSString` представлению в виде `NSString` .

[`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute)Атрибут может применяться к следующим типам данных:

- `NSString`ссылки (только для свойств только для чтения)
- `NSArray`ссылки (только для свойств только для чтения)
- 32-разрядный ints ( `System.Int32` )
- 64-разрядный ints ( `System.Int64` )
- 32-разрядное число с плавающей запятой ( `System.Single` )
- 64-разрядное число с плавающей запятой ( `System.Double` )
- `System.Drawing.SizeF`
- `CGSize`

В дополнение к имени собственного поля можно указать имя библиотеки, в которой находится поле, передав имя библиотеки:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

При статической компоновке нет библиотеки для привязки, поэтому необходимо использовать `__Internal` Имя:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums"></a>

### <a name="binding-enums"></a>Привязка перечислений

Можно добавить `enum` непосредственно в файлы привязки, чтобы упростить их использование в определениях API — без использования другого исходного файла (который должен быть скомпилирован как в привязке, так и в окончательном проекте).

Пример.

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Также можно создать собственные перечисления для замены `NSString` констант. В этом случае генератор **автоматически** создает методы для преобразования значений enum и констант NSString.

Пример.

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}

interface MyType {
    [Export ("performForMode:")]
    void Perform (NSString mode);

    [Wrap ("Perform (mode.GetConstant ())")]
    void Perform (NSRunLoopMode mode);
}
```

В приведенном выше примере можно было бы выбрать Оформление `void Perform (NSString mode);` с помощью [`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) атрибута. Это приведет к **скрытию** API, основанного на константе, от потребителей привязки.

Однако это ограничит подкласс типа, так как в качестве альтернативы API лучше используется [`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) атрибут. Эти созданные методы не имеют `virtual` , т. е. Вы не сможете переопределить их, что может быть хорошим выбором.

Альтернативой является пометка исходного, `NSString` основанного на, определения как `[Protected]` . Это позволит подклассам работать, если это необходимо, а версия врап'ед будет работать и вызывать переопределенный метод.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Привязка `NSValue` , `NSNumber` и `NSString` к лучшему типу

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)Атрибут допускает привязку `NSNumber` `NSValue` и `NSString` (перечисления) к более точным типам C#. Атрибут можно использовать для создания лучшего, более точного, API .NET через собственный API.

Можно оформлять методы (в возвращаемом значении), параметры и свойства с помощью [`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) . Единственное ограничение заключается в том, что член **не должен** находиться внутри элемента[`[Protocol]`](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) 
[`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute)интерфейс или.

Пример:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Выводит:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

Внутри мы будем выполнять `bool?`  <->  `NSNumber` `CGRect`  <->  `NSValue` преобразования и.

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute)также поддерживает массивы `NSNumber` `NSValue` и `NSString` (перечисления).

Пример:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

Выводит:

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll`является `NSString` обратным перечислением, мы выбирали правильное `NSString` значение и обработаем преобразование типа.

Дополнительные сведения о [`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) поддерживаемых типах конвертации см. в документации.

<a name="Binding_Notifications"></a>

### <a name="binding-notifications"></a>Уведомления о привязке

Уведомления — это сообщения, которые публикуются в `NSNotificationCenter.DefaultCenter` и используются в качестве механизма для передачи сообщений из одной части приложения в другую. Разработчики подписываются на уведомления, как правило, с помощью метода [AddObserver](xref:Foundation.NSNotificationCenter.AddObserver(Foundation.NSString,System.Action{Foundation.NSNotification})) [нснотификатионцентер](xref:Foundation.NSNotificationCenter). Когда приложение отправляет сообщение в центр уведомлений, оно обычно содержит полезные данные, хранящиеся в словаре [нснотификатион. UserInfo](xref:Foundation.NSNotification.UserInfo) . Этот словарь слабо типизирован, и получение информации из него может быть подвержено ошибкам, так как пользователям обычно требуется прочитать в документации, какие ключи доступны в словаре, и типы значений, которые могут храниться в словаре. Наличие ключей иногда также используется в качестве логического.

Генератор привязки Xamarin. iOS предоставляет разработчикам поддержку для привязки уведомлений. Для этого необходимо установить[`[Notification]`](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute)
для свойства, помеченного атрибутом[`[Field]`](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute)
(оно может быть общедоступным или частным).

Этот атрибут может использоваться без аргументов для уведомлений, не содержащих полезных данных, или можно указать объект `System.Type` , ссылающийся на другой интерфейс в определении API, обычно с именем EventArgs. Генератор преобразует интерфейс в класс, который подклассы `EventArgs` и будет включать все свойства, перечисленные здесь. [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute)Атрибут должен использоваться в классе EventArgs для перечисления имени ключа, используемого для поиска словаря цели-C для выборки значения.

Пример:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Приведенный выше код создаст вложенный класс `MyClass.Notifications` со следующими методами:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Пользователи кода могут легко подписываться на уведомления, опубликованные в [нсдефаултцентер](xref:Foundation.NSNotificationCenter.DefaultCenter) , используя следующий код:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Возвращаемое значение `ObserveDidStart` можно использовать для простой отмены получения уведомлений, например:

```csharp
token.Dispose ();
```

Также можно вызвать [нснотификатион. дефаултцентер. RemoveObserver](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject)) и передать токен. Если уведомление содержит параметры, следует указать вспомогательный `EventArgs` интерфейс следующим образом:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

Приведенный выше компонент создаст `MyScreenChangedEventArgs` класс со `ScreenX` свойствами и `ScreenY` , которые будут получать данные из словаря [нснотификатион. UserInfo](xref:Foundation.NSNotification.UserInfo) , используя ключи "скринкскэй" и "скриникэй" соответственно, и применить соответствующие преобразования. `[ProbePresence]`Атрибут используется в генераторе для проверки того, задан ли ключ в `UserInfo` , вместо того, чтобы пытаться извлечь значение. Используется в тех случаях, когда присутствие ключа является значением (обычно для логических значений).

Это позволяет написать следующий код:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories"></a>

### <a name="binding-categories"></a>Категории привязки

Категории — это механизм цели-C, используемый для расширения набора методов и свойств, доступных в классе.   На практике они используются для расширения функциональных возможностей базового класса (например `NSObject` ,), когда определена связь между определенной платформой (например `UIKit` ), делая их методы доступными, но только если новая платформа связана.   В некоторых других случаях они используются для упорядочения функций в классе по функциональным возможностям.   Они похожи на методы расширения C#. Вот как будет выглядеть Категория в цели-C:

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Приведенный выше пример, если он найден в библиотеке, будет расширять экземпляры `UIView` с помощью метода `makeBackgroundRed` .

Чтобы привязать их, можно использовать [`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) атрибут в определении интерфейса.  При использовании[`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
, значение атрибута[`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) 
изменения атрибутов используются для указания базового класса, который должен быть расширен, в качестве типа для расширения.

Ниже показано, как `UIView` расширения привязываются и включаются в методы расширения C#:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Приведенный выше объект создаст `MyUIViewExtension` класс, содержащий `MakeBackgroundRed` метод расширения.  Это означает, что теперь можно вызывать "Макебаккграундред" для любого `UIView` подкласса, обеспечивая те же функциональные возможности, что и на уровне цели-C. В некоторых других случаях категории используются не для расширения класса System, а для Организации функциональных возможностей исключительно в целях оформления.  Пример:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Хотя можно использовать[`[Category]`](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute)
атрибут также для этого стиля оформления объявлений можно просто добавить их все в определение класса.  Оба они будут иметь одно и то же:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Twitter {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Facebook {
    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

В этих случаях для слияния категорий это просто короче.

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

<a name="Binding_Blocks"></a>

### <a name="binding-blocks"></a>Блоки привязки

Блоки представляют собой новую конструкцию, появившуюся компанией Apple для приведения функционального эквивалента анонимных методов C# к цели-C. Например, `NSSet` класс теперь предоставляет этот метод:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Приведенное выше описание объявляет метод с именем `enumerateObjectsUsingBlock:` , который принимает один аргумент с именем `block` . Этот блок похож на анонимный метод C# в том, что он поддерживает запись текущей среды (указатель "this", доступ к локальным переменным и параметрам). Приведенный выше метод в `NSSet` вызывает блок с двумя параметрами `NSObject` ( `id obj` часть) и указателем на логическую `BOOL *stop` часть ().

Чтобы привязать этот тип API к btouch, необходимо сначала объявить сигнатуру типа блока как делегат C#, а затем сослаться на него из точки входа API следующим образом:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

Теперь код может вызывать функцию из C#:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Лямбда-выражения также можно использовать, если вы предпочитаете:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync"></a>

### <a name="asynchronous-methods"></a>Асинхронные методы

Генератор привязок может превратить определенный класс методов в методы с асинхронной обрезкой (методы, возвращающие задачу или задачу &lt; T &gt; ).

Можно использовать[`[Async]`](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) 
атрибут в методах, возвращающий значение void и последний аргумент которого является обратным вызовом.  При применении этого метода к методу генератор привязок создаст версию этого метода с суффиксом `Async` .  Если обратный вызов не принимает параметров, возвращаемое значение будет равно `Task` , если обратный вызов принимает параметр, результатом будет `Task<T>` .  Если обратный вызов принимает несколько параметров, следует задать или, `ResultType` `ResultTypeName` чтобы указать нужное имя созданного типа, который будет содержать все свойства.

Пример.

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Приведенный выше код создаст как метод LoadFile, так и:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types"></a>

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Отображая строгие типы для слабых параметров NSDictionary

Во многих местах в API объектив-C параметры передаются как слабо типизированные `NSDictionary` API с определенными ключами и значениями, но они подвержены ошибкам (можно передать недопустимые ключи и не получать предупреждения; можно передать недопустимые значения и не получать предупреждений) и неприятное использование, так как им требуется несколько обращений к документации для поиска возможных имен и значений ключей.

Решением является предоставление строго типизированной версии, предоставляющей строго типизированную версию API, и в фоновом режиме сопоставляет различные базовые ключи и значения.

Например, если API цели-C принял, `NSDictionary` и он задокументирован как принимающий ключ, `XyzVolumeKey` который принимает в качестве ключа `NSNumber` значение от 0,0 до 1,0, и `XyzCaptionKey` , который принимает строку, вам нужно, чтобы ваши пользователи имели удобный API, который выглядит следующим образом:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume`Свойство определено как допускающее значение NULL float, так как в соглашении в цели-C не требуются значения этих словарей, поэтому существуют сценарии, в которых значение может не быть задано.

Для этого необходимо выполнить несколько действий:

- Создайте класс со строгой типизацией, который подклассировать [диктионариконтаинер](xref:Foundation.DictionaryContainer) и предоставляет различные методы получения и задания для каждого свойства.
- Объявите перегрузки для методов, принимающих `NSDictionary` новую строго типизированную версию.

Вы можете создать строго типизированный класс либо вручную, либо использовать генератор для выполнения работы.  Сначала мы рассмотрим, как это сделать вручную, чтобы понять, что происходит, а затем — автоматический подход.

Вам нужно создать вспомогательный файл для этого, он не перейдет в API контракта.  Это то, что вам пришлось бы писать для создания класса Ксизоптионс:

```csharp
public class XyzOptions : DictionaryContainer {
# if !COREBUILD
    public XyzOptions () : base (new NSMutableDictionary ()) {}
    public XyzOptions (NSDictionary dictionary) : base (dictionary){}

    public nfloat? Volume {
       get { return GetFloatValue (XyzOptionsKeys.VolumeKey); }
       set { SetNumberValue (XyzOptionsKeys.VolumeKey, value); }
    }
    public string Caption {
       get { return GetStringValue (XyzOptionsKeys.CaptionKey); }
       set { SetStringValue (XyzOptionsKeys.CaptionKey, value); }
    }
# endif
}
```

Затем необходимо предоставить метод-оболочку, который охватывает интерфейс API верхнего уровня, поверх низкоуровневого API.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Если API не требуется перезаписывать, можно безопасно скрыть API на основе NSDictionary с помощью[`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
.

Как видите, мы используем[`[Wrap]`](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute)
атрибут для размещения новой точки входа API, и мы поможем ей воспользоваться нашим строго типизированным `XyzOptions` классом.  Метод-оболочка также позволяет передавать значение null.

В настоящий момент мы не упоминали, `XyzOptionsKeys` из чего поступили значения.  Обычно вы группируюте ключи, которые API отображает в статическом классе `XyzOptionsKeys` , например:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Рассмотрим автоматическую поддержку создания этих словарей со строгой типизацией.  Это позволяет избежать недостаточного использования шаблона, и вы можете определить словарь непосредственно в контракте API, а не использовать внешний файл.

Для создания строго типизированного словаря необходимо ввести интерфейс в API и снабдить его атрибутом [стронгдиктионари](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) .  Это говорит генератору, что ему следует создать класс с тем же именем, что и у интерфейса, который будет производным от `DictionaryContainer` и будет предоставлять для него строго типизированные методы доступа.

[`[StrongDictionary]`](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary)Атрибут принимает один параметр, который представляет собой имя статического класса, содержащего ключи словаря.  Затем каждое свойство интерфейса станет строго типизированным методом доступа.  По умолчанию код будет использовать имя свойства с суффиксом "Key" в статическом классе для создания метода доступа.

Это означает, что для создания строго типизированного метода доступа больше не требуется внешний файл, и не нужно вручную создавать методы получения и задания для каждого свойства, а также не искать ключи вручную.

Вот как будет выглядеть вся привязка:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
[StrongDictionary ("XyzOptionKeys")]
interface XyzOptions {
    nfloat Volume { get; set; }
    string Caption { get; set; }
}

[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Если вам нужно ссылаться в своих `XyzOption` элементах на другое поле (не имя свойства с суффиксом `Key` ), можно снабдить свойство атрибутом[`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) 
атрибут с именем, которое вы хотите использовать.

<a name="Type_mappings"></a>

## <a name="type-mappings"></a>Сопоставления типов

В этом разделе описывается, как типы цели-C сопоставляются с типами C#.

<a name="Simple_Types"></a>

### <a name="simple-types"></a>Простые типы

В следующей таблице показано, как следует сопоставлять типы из мира цели-C и Кокоатауч в мире Xamarin. iOS:

|Имя типа цели-C|Тип Unified API Xamarin. iOS|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString`([Подробнее о привязке NSString](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string`(см. также: [`[PlainString]`](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) )|
|`CGRect`|`CGRect`|
|`CGPoint`|`CGPoint`|
|`CGSize`|`CGSize`|
|`CGFloat`, `GLfloat`|`nfloat`|
|Типы Корефаундатион ( `CF*` )|`CoreFoundation.CF*`|
|`GLint`|`nint`|
|`GLfloat`|`nfloat`|
|Типы фундамента ( `NS*` )|`Foundation.NS*`|
|`id`|`Foundation`.`NSObject`|
|`NSGlyph`|`nint`|
|`NSSize`|`CGSize`|
|`NSTextAlignment`|`UITextAlignment`|
|`SEL`|`ObjCRuntime.Selector`|
|`dispatch_queue_t`|`CoreFoundation.DispatchQueue`|
|`CFTimeInterval`|`double`|
|`CFIndex`|`nint`|
|`NSGlyph`|`nuint`|

<a name="Arrays"></a>

### <a name="arrays"></a>Массивы

Среда выполнения Xamarin. iOS автоматически выполняет преобразование массивов C# в `NSArrays` и обратно, поэтому, например, метод мнимой цели-C, возвращающий значение `NSArray` из `UIViews` :

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

Имеет следующий привязку:

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

Идея заключается в использовании строго типизированного массива C#, так как это позволит интегрированной среде разработки обеспечить правильное завершение кода с фактическим типом, не вынуждая пользователя угадать, или найти документацию, чтобы узнать фактический тип объектов, содержащихся в массиве.

В случаях, когда нельзя отключать фактический наиболее производный тип, содержащийся в массиве, можно использовать в `NSObject []` качестве возвращаемого значения.

<a name="Selectors"></a>

### <a name="selectors"></a>Селекторы

Селекторы отображаются в API цели-C в качестве специального типа `SEL` . При привязке селектора следует сопоставлять тип с `ObjCRuntime.Selector` .  Обычно селекторы предоставляются в API с объектом, целевым объектом и селектором, который вызывается в целевом объекте. Оба эти значения по сути соответствуют делегату C#: что инкапсулирует как метод для вызова, так и объект для вызова метода в.

Вот как выглядит привязка:

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

И вот как метод обычно используется в приложении:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        b.SetTarget (this, new Selector ("print"));
    }

    [Export ("print")]
    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

Чтобы привязать лучше к разработчикам C#, обычно будет предоставлен метод, принимающий `NSAction` параметр, который позволяет использовать делегаты C# и лямбда-выражения вместо `Target+Selector` . Для этого вы обычно скрываете `SetTarget` метод, помечая его на[`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
, а затем вы предоставите новый вспомогательный метод следующим образом:

```csharp
// API.cs
interface Button {
   [Export ("setTarget:selector:"), Internal]
   void SetTarget (NSObject target, Selector sel);
}

// Extensions.cs
public partial class Button {
     public void SetTarget (NSAction callback)
     {
         SetTarget (new NSActionDispatcher (callback), NSActionDispatcher.Selector);
     }
}
```

Теперь ваш код пользователя можно записать следующим образом:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        // First Style
        b.SetTarget (ThePrintMethod);

        // Lambda style
        b.SetTarget (() => {  /* print here */ });
    }

    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

<a name="Strings"></a>

### <a name="strings"></a>Строки

При привязке метода, который принимает `NSString` , можно заменить его строковым типом C# как для возвращаемых типов, так и для параметров.

Единственный случай, когда может потребоваться использовать `NSString` напрямую, — это то, что строка используется в качестве маркера. Дополнительные сведения о строках и `NSString` см. в статье [Разработка API в NSString](~/ios/internals/api-design/nsstring.md) документе.

В некоторых редких случаях API может представлять собой строку (), похожую на C, `char *` вместо строки задания-C ( `NSString *` ). В этих случаях параметр можно снабдить заметками[`[PlainString]`](~/cross-platform/macios/binding/binding-types-reference.md#plainstring)
.

<a name="outref_parameters"></a>

### <a name="outref-parameters"></a>параметры out и ref

Некоторые API возвращают значения в своих параметрах или передают параметры по ссылке.

Как правило, сигнатура выглядит следующим образом:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

В первом примере показана общая идиома цели-C для возврата кодов ошибок, `NSError` передается указатель на указатель и при возврате значения задается.   Второй метод показывает, как метод цели-C может принимать объект и изменять его содержимое.   Это будет передаваться по ссылке, а не чистому выходному значению.

Привязка будет выглядеть следующим образом:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes"></a>

### <a name="memory-management-attributes"></a>Атрибуты управления памятью

При использовании [`[Export]`](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) атрибута и передаче данных, которые будут храниться в вызываемом методе, можно указать семантику аргумента, передав ее в качестве второго параметра, например:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

В приведенном выше примере значение помечается как имеющее семантику "удержания". Семантика доступна:

- Присвоение
- Копировать
- Сохранять

<a name="Style_Guidelines"></a>

### <a name="style-guidelines"></a>Рекомендации по стилю

<a name="Using_[Internal]"></a>

#### <a name="using-internal"></a>Using [внутренний]

Можно использовать[`[Internal]`](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute)
атрибут для скрытия метода из общедоступного API. Это может потребоваться в тех случаях, когда предоставленный API слишком низкий уровень и необходимо предоставить высокоуровневая реализация в отдельном файле на основе этого метода.

Это также можно использовать при работе с ограничениями в генераторе привязок, например в некоторых расширенных сценариях могут предоставляться непривязанные типы, которые нужно привязать самостоятельно, и вам нужно самостоятельно заключить эти типы в собственный подход.

<a name="Event_Handlers_and_Callbacks"></a>

## <a name="event-handlers-and-callbacks"></a>Обработчики событий и обратные вызовы

Классы цели-C обычно рассылают уведомления или запрашивают информацию, отправляя сообщение в класс делегата (делегат цели-C).

Эта модель, хотя и полностью поддерживаемая Xamarin. iOS, иногда может быть громоздкими. Xamarin. iOS предоставляет шаблон события C# и систему обратного вызова методов в классе, который можно использовать в таких ситуациях. Это позволяет выполнять код следующим образом:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Генератор привязок способен уменьшить объем ввода, необходимый для привязки шаблона цели-C к шаблону C#.

Начиная с Xamarin. iOS 1,4, можно также указать генератору создавать привязки для конкретных делегатов цели-C и предоставлять делегата как события и свойства C# в типе узла.

В этом процессе есть два класса: класс Host, который будет создавать события и отправлять их в `Delegate` класс или, `WeakDelegate` а также фактический делегат.

Учитывая следующие настройки:

```csharp
[BaseType (typeof (NSObject))]
interface MyClass {
    [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
    NSObject WeakDelegate { get; set; }

    [Wrap ("WeakDelegate")][NullAllowed]
    MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
    [Export ("loaded:bytes:")]
    void Loaded (MyClass sender, int bytes);
}
```

Чтобы заключить класс в оболочку, необходимо:

- В классе Host добавьте в[`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   Объявите тип, который выступает в качестве своего делегата, и предоставленное вами имя C#. В нашем примере выше они являются `typeof (MyClassDelegate)` и `WeakDelegate` соответственно.
- В классе делегата для каждого метода, имеющего более двух параметров, необходимо указать тип, который будет использоваться для автоматически создаваемого класса EventArgs.

Генератор привязки не ограничивается только одним назначением события, но некоторые классы цели-C могут создавать сообщения более чем в одном делегате, поэтому для поддержки этой настройки необходимо предоставить массивы. Для большинства настроек это не потребуется, но генератор готов к поддержке этих вариантов.

Результирующий код будет следующим:

```csharp
[BaseType (typeof (NSObject),
    Delegates=new string [] {"WeakDelegate"},
    Events=new Type [] { typeof (MyClassDelegate) })]
interface MyClass {
        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }

        [Wrap ("WeakDelegate")][NullAllowed]
        MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
        [Export ("loaded:bytes:"), EventArgs ("MyClassLoaded")]
        void Loaded (MyClass sender, int bytes);
}
```

`EventArgs`Используется для указания имени `EventArgs` создаваемого класса. Следует использовать одну для каждой сигнатуры (в этом примере объект `EventArgs` будет содержать `With` свойство типа НИНТ).

Используя приведенные выше определения, генератор создаст следующее событие в созданном MyClass:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Теперь можно использовать следующий код:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Обратные вызовы так же, как и вызовы событий, разница заключается в том, что вместо нескольких потенциальных подписчиков (например, несколько методов могут присоединиться к `Clicked` событиям или `DownloadFinished` событиям) обратные вызовы могут иметь только один подписчик.

Процесс идентичен, единственное отличие заключается в том, что вместо предоставления имени `EventArgs` создаваемого класса, EventArgs фактически используется для именования результирующего имени делегата C#.

Если метод в классе делегата возвращает значение, генератор привязки сопоставляет его с методом делегата в родительском классе, а не в событии. В таких случаях необходимо указать значение по умолчанию, которое должно возвращаться методом, если пользователь не присоединяется к делегату. Для этого используется[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute)
[`[DefaultValueFromArgument]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute)атрибуты или.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute)будет жестко кодировать возвращаемое значение, в то время как[`[DefaultValueFromArgument]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute)
используется для указания того, какой входной аргумент будет возвращен.

<a name="Enumerations_and_Base_Types"></a>

## <a name="enumerations-and-base-types"></a>Перечисления и базовые типы

Также можно ссылаться на перечисления или базовые типы, которые не поддерживаются системой определения интерфейса btouch напрямую. Для этого поместите перечисления и основные типы в отдельный файл и включите его как часть одного из дополнительных файлов, предоставляемых для btouch.

<a name="Linking_the_Dependencies"></a>

## <a name="linking-the-dependencies"></a>Связывание зависимостей

При привязке интерфейсов API, которые не являются частью приложения, необходимо убедиться, что исполняемый файл связан с этими библиотеками.

Необходимо сообщить Xamarin. iOS о том, как связать библиотеки. это можно сделать, изменив конфигурацию сборки, чтобы вызвать `mtouch` команду с некоторыми дополнительными аргументами сборки, которые определяют способ связи с новыми библиотеками с помощью параметра "-gcc_flags", за которым следует строка в кавычках, которая содержит все дополнительные библиотеки, необходимые для программы, например:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Приведенный выше пример свяжет `libMyLibrary.a` `libSystemLibrary.dylib` и `CFNetwork` библиотеку Framework с окончательным исполняемым файлом.

Также можно воспользоваться преимуществами уровня сборки [`[LinkWithAttribute]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) , которые можно внедрить в файлы контрактов (например, `AssemblyInfo.cs` ).
При использовании необходимо, [`[LinkWithAttribute]`](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) чтобы собственная библиотека была доступна во время создания привязки, так как в этом случае собственная библиотека будет внедрена в приложение. Пример:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Возможно, вы заинтересовались, зачем вам нужна `-force_load` команда, и причина заключается в том, что флаг-ObjC хотя компилирует код в, он не сохраняет метаданные, необходимые для поддержки категорий (компоновщик или компилятор без исключения), которые необходимы во время выполнения для Xamarin. iOS.

<a name="Assisted_References"></a>

## <a name="assisted-references"></a>Справочные материалы

Некоторые временные объекты, такие как листы действий и окна предупреждений, несложно отследить для разработчиков, а генератор привязок может немного помочь в этом.

Например, если у вас есть класс, который показал сообщение, а затем создал `Done` событие, традиционный способ обработки этого:

```csharp
class Demo {
    MessageBox box;

    void ShowError (string msg)
    {
        box = new MessageBox (msg);
        box.Done += { box = null; ... };
    }
}
```

В приведенном выше сценарии разработчику необходимо постоянно ссылаться на объект и либо на утечку, либо на активное удаление ссылки на поле.  Во время привязки кода генератор поддерживает отслеживание ссылки и очищает его при вызове специального метода, приведенный выше код будет выглядеть следующим образом:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Обратите внимание, что больше не требуется поддерживать переменную в экземпляре, что она работает с локальной переменной и нет необходимости очищать ссылку при удалении объекта.

Чтобы воспользоваться этим преимуществом, класс должен иметь свойство Events, установленное в [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) объявлении, а также `KeepUntilRef` переменную, для которой задано имя метода, вызываемого при завершении работы объекта, например:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols"></a>

## <a name="inheriting-protocols"></a>Наследование протоколов

Начиная с Xamarin. iOS версии 3.2, мы поддерживаем наследование от протоколов, которые были помечены [`[Model]`](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) свойством. Это полезно в некоторых шаблонах API, например в `MapKit` случаях, когда `MKOverlay` протокол наследует от `MKAnnotation` протокола и используется несколькими классами, наследующими от `NSObject` .

Исторически нам требовалось копировать протокол в каждую реализацию, но в таких случаях теперь можно `MKShape` создать класс, наследующий от протокола, `MKOverlay` и он автоматически создаст все необходимые методы.

## <a name="related-links"></a>Связанные ссылки

- [Пример привязки](https://docs.microsoft.com/samples/xamarin/ios-samples/bindingsample/)
