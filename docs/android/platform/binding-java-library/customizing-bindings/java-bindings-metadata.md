---
title: Метаданные привязок Java
description: Код C# в Xamarin.Android вызывает библиотеки Java с помощью привязок, которые являются механизмом для абстрагирования низкоуровневых сведений, указанных в собственном интерфейсе Java (JNI). Xamarin.Android предоставляет средство, создающее эти привязки. С помощью этого средства разработчик может управлять способами создания привязок с использованием метаданных, что позволяет выполнять такие процедуры, как изменение пространств имен и переименование членов. В этом документе описывается, как работают метаданные, перечислены атрибуты, поддерживаемые метаданными, и объясняется, как устранить проблемы привязки путем изменения этих метаданных.
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 5439e213c0016ea01935d617f5f6b5a3edf3eee8
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853014"
---
# <a name="java-bindings-metadata"></a>Метаданные привязок Java

> [!IMPORTANT]
> В настоящее время рассматривается возможность использования настраиваемых привязок на платформе Xamarin. Примите участие в [**этом опросе**](https://www.surveymonkey.com/r/KKBHNLT), чтобы помочь определить дальнейшие направления разработки.

_Код C# в Xamarin.Android вызывает библиотеки Java с помощью привязок, которые являются механизмом для абстрагирования низкоуровневых сведений, указанных в собственном интерфейсе Java (JNI). Xamarin.Android предоставляет средство, создающее эти привязки. С помощью этого средства разработчик может управлять способами создания привязок с использованием метаданных, что позволяет выполнять такие процедуры, как изменение пространств имен и переименование членов. В этом документе описывается, как работают метаданные, перечислены атрибуты, поддерживаемые метаданными, и объясняется, как устранить проблемы привязки путем изменения этих метаданных._

## <a name="overview"></a>Обзор

**Библиотека привязки Java** Xamarin.Android предназначена автоматизировать большую часть работы по привязке существующей библиотеки Android с помощью средства, которое иногда называют _генератором привязок_. Если выполняется привязка библиотеки Java, Xamarin.Android проверяет классы Java и создает список всех пакетов, типов и элементов, которые должны быть привязаны. Этот список API хранится в файле XML, который можно найти по адресу **\{project directory}\obj\Release\api.xml** для сборки **RELEASE** и **\{project directory}\obj\Debug\api.xml** для сборки **DEBUG**.

![Расположение файла api.xml в папке obj/Debug](java-bindings-metadata-images/java-bindings-metadata-01.png)

Генератор привязок будет использовать файл **api.xml** как основу для создания необходимых классов-оболочек C#. Содержимое этого файла XML является разновидностью формата _Android Open Source Project_ Google.
Ниже приведен пример содержимого файла **api.xml**:

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

В этом примере файл **api.xml** объявляет класс в пакете `android` с именем `Manifest`, который расширяет `java.lang.Object`.

Во многих случаях требуется участие человека, чтобы сделать API Java похожим на .NET или устранить проблемы, препятствующие компиляции сборки привязки. Например, может потребоваться изменить имена пакетов Java на пространства имен .NET, переименовать класс или изменить тип возвращаемого значения метода.

Эти изменения не внедряются путем прямого изменения файла **api.xml**.
Вместо этого изменения записываются в специальные файлы XML, предоставляемые шаблоном библиотеки привязки Java. На генератор привязок будут влиять файлы сопоставления при создании и компиляции сборки привязки Xamarin.Android.

Эти файлы сопоставления XML можно найти в папке проекта **Transforms**:

- **Metadata.xml** — позволяет вносить изменения в окончательный API, например изменять пространство имен созданной привязки. 

- **EnumFields.xml** — содержит сопоставление констант `int` Java и `enums` C#. 

- **EnumMethods.xml** — разрешает изменение параметров метода и типов возвращаемых значений из констант `int` Java на `enums` C#. 

Файл **MetaData.xml** — самый важный, так как он позволяет вносить изменения общего назначения в привязку, например:

- переименовывать пространства имен, классы, методы или поля, чтобы они соответствовали соглашениям .NET; 

- удалять пространства имен, классы, методы или поля, которые больше не требуются; 

- перемещать классы в разные пространства имен; 

- добавлять дополнительные классы поддержки для обеспечения соответствия структуры привязки шаблонам .NET Framework. 

Рассмотрим **Metadata.xml** более детально.

## <a name="metadataxml-transform-file"></a>Файл преобразования Metadata.xml

Как уже указано, файл **Metadata.xml** использует генератор привязок, чтобы повлиять на создание сборки привязки.
В формате метаданных используется синтаксис [XPath](https://www.w3.org/TR/xpath/) и он практически идентичен *метаданным GAPI*, которые описаны в [этом разделе](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) руководства. Эта реализация является почти полной реализацией XPath 1.0 с такой же поддержкой элементов в стандарте 1.0. Этот файл является мощным механизмом на основе XPath для изменения, добавления, скрытия или перемещения любого элемента или атрибута в файле API. Все элементы правил в спецификации метаданных включают в себя атрибут path (пути) для указания узла, к которому должно применяться правило. Правила применяются в следующем порядке:

- **add-node** — добавляет дочерний узел к узлу, заданному атрибутом path.
- **attr** — устанавливает значение атрибута элемента, заданного атрибутом path.
- **remove-node** — удаляет узлы, соответствующие заданному XPath.

Далее приведен пример файла **Metadata.xml**:

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

Ниже перечислены некоторые из наиболее часто используемых элементов XPath для API Java.

- `interface` — используется для определения расположения интерфейса Java. Например, `/interface[@name='AuthListener']`.

- `class` — используется для определения расположения класса. Например, `/class[@name='MapView']`.

- `method` — используется для определения расположения метода в классе или интерфейсе Java. Например, `/class[@name='MapView']/method[@name='setTitleSource']`.

- `parameter` — определяет параметр для метода. Например, `/parameter[@name='p0']`

### <a name="adding-types"></a>Добавление типов

Элемент `add-node` сообщает проекту привязки Xamarin.Android о добавлении нового класса-оболочки в **api.xml**. Например, следующий фрагмент кода будет направлять генератор привязки для создания класса с конструктором и одним полем:

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```

### <a name="removing-types"></a>Удаление типов

Можно указать генератору привязок Xamarin.Android игнорировать тип Java, а не привязывать его. Это можно сделать, добавив XML-элемент `remove-node` в файл **metadata.xml**:

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Переименование членов

Нельзя переименовать члены, напрямую отредактировав файл **api.xml**, так как для Xamarin.Android требуются исходные имена собственного интерфейса Java (JNI). Поэтому атрибут `//class/@name` изменить нельзя. Если сделать это, привязка не будет работать.

Рассмотрим случай, когда нужно переименовать тип `android.Manifest`.
Для этого мы можем попытаться напрямую изменить **api.xml** и переименовать класс следующим образом:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Это приведет к созданию в генераторе привязок следующего кода C# для класса-оболочки:

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Обратите внимание, что класс-оболочка переименован в `NewName`, а исходный тип Java — все еще `Manifest`. Класс привязки Xamarin.Android больше не может получить доступ к каким-либо методам `android.Manifest`. Класс-оболочка привязан к несуществующему типу Java.

Чтобы правильно изменить управляемое имя упакованного типа (или метода), необходимо задать атрибут `managedName`, как показано в этом примере:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes"></a>

#### <a name="renaming-eventarg-wrapper-classes"></a>Переименование классов-оболочек `EventArg`

Если генератор привязки Xamarin.Android определяет метод задания `onXXX` для _типа прослушивателя_, то создается событие и подкласс C# `EventArgs` для поддержки в API с чертами .NET прослушивателей на основе Java. В качестве примера рассмотрим следующий класс и метод Java:

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android удалит префикс `on` из метода задания, а вместо этого использует `2DSignNextManuever` в качестве основы для имени подкласса `EventArgs`. Подкласс будет называться примерно так:

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Это имя класса C# недопустимо. Чтобы устранить эту проблему, автор привязки должен использовать атрибут `argsType` и указать допустимое имя C# для подкласса `EventArgs`:

```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

## <a name="supported-attributes"></a>Поддерживаемые атрибуты

В следующих разделах описаны некоторые атрибуты для преобразования API Java.

### <a name="argstype"></a>argsType

Этот атрибут помещается в методы задания, чтобы присвоить имя подклассу `EventArg`, который будет создан для поддержки прослушивателей Java. Более подробно это описано в разделе [Переименование классов-оболочек EventArg](#Renaming_EventArg_Wrapper_Classes) этого руководства.

### <a name="eventname"></a>eventName

Задает имя для события. Если значение пустое, то событие не создается.
Более подробно это описано в разделе [Переименование классов-оболочек EventArg](#Renaming_EventArg_Wrapper_Classes).

### <a name="managedname"></a>managedName

Используется для изменения имени пакета, класса, метода или параметра. Например, чтобы изменить имя класса Java `MyClass` на `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

В следующем примере показано выражение XPath для переименования метода `java.lang.object.toString` на `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>ManagedType

`managedType` используется для изменения типа возвращаемого значения метода. В некоторых ситуациях генератор привязок неправильно определяет тип возвращаемого значения метода Java, что приводит к ошибке времени компиляции. Одним из возможных решений в этой ситуации является изменение типа возвращаемого значения метода.

Например, генератор привязок считает, что метод Java `de.neom.neoreadersdk.resolution.compareTo()` должен возвращать `int` и принимать `Object` в качестве параметров, в результате чего появляется сообщение об ошибке **Ошибка CS0535: "DE.Neom.Neoreadersdk.Resolution" не реализует член интерфейса "Java.Lang.IComparable.CompareTo(Java.Lang.Object)".** В следующем фрагменте кода показано, как изменить тип первого параметра, созданного методом C#, с `DE.Neom.Neoreadersdk.Resolution` на `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]" name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

Изменяет тип возвращаемого значения метода. Это не изменит атрибут возвращаемого значения (так как изменения таких атрибутов могут привести к несовместимости изменений в сигнатуре JNI). В следующем примере тип возвращаемого значения метода `append` изменяется с `SpannableStringBuilder` на `IAppendable` (повторный вызов C# не поддерживает ковариантные типы возвращаемых данных):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>obfuscated

Средства обфускации кода в библиотеках Java могут конфликтовать с генератором привязок Xamarin.Android и помешать ему создать классы-оболочки C#. Характеристики классов obfuscated включают в себя: 

- имя класса содержит символ **$** , например **a$.class**;
- имя класса написано полностью строчными буквами, например **a.class**.

Ниже приведен пример того, как создать "немаскированный" тип C#:

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Этот атрибут можно использовать для изменения имени управляемого свойства.

Особым случаем использования `propertyName` является ситуация, когда у класса Java есть только метод получения для поля. В этом случае генератору привязки нужно создать свойство доступное только для записи, что не рекомендуется в .NET. В следующем фрагменте кода показано, как "удалить" свойства .NET, установив для `propertyName` пустую строку:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Обратите внимание, что генератор привязок по-прежнему будет создавать методы задания и получения.

### <a name="sender"></a>отправитель

Указывает, какой параметр метода должен быть параметром `sender`, если метод сопоставлен с событием. Значением может быть `true` или `false`. Пример:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>видимость

Этот атрибут используется для изменения видимости класса, метода или свойства. Например, может потребоваться повысить уровень метода Java `protected` таким образом, чтобы у него была соответствующая программа-оболочка C# `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>Файлы EnumFields.xml и EnumMethods.xml

Бывают случаи, когда библиотеки Android используют целочисленные константы для представления состояний, передаваемых в свойства или методы библиотек. Во многих случаях полезно привязать такие константы к перечислениям в C#. Чтобы упростить это сопоставление, используйте файлы **EnumFields.xml** и **EnumMethods.xml** в проекте привязки. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Определение перечисления с помощью файла EnumFields.xml

Файл **EnumFields.xml** содержит сопоставление констант `int` Java и `enums` C#. Рассмотрим следующий пример перечисления C#, создаваемого для набора констант `int`: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Здесь мы воспользовались классом Java `SKRealReachSettings` и определили перечисление C# с именем `SKMeasurementUnit` в пространстве имен `Skobbler.Ngx.Map.RealReach`. Записи `field` определяют имя константы Java (например, `UNIT_SECOND`), имя записи перечисления (например, `Second`) и целочисленное значение, представленное обеими сущностями (например, `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Определение методов получения и задания с помощью файла EnumMethods.xml

Файл **EnumMethods.xml** позволяет изменять параметры метода и типы возвращаемых значений из констант `int` Java на `enums` C#. Другими словами, он сопоставляет чтение и запись перечислений C# (определенных в файле **EnumFields.xml**) с методами `get` и `set` константы Java `int`.

Учитывая выше заданное перечисление `SKRealReachSettings`, файл **EnumMethods.xml** определяет метод получения или задания для этого перечисления:

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

Первая строка `method` сопоставляет возвращаемое значение метода Java `getMeasurementUnit` с перечислением `SKMeasurementUnit`. Вторая строка `method` сопоставляет первый параметр `setMeasurementUnit` с тем же перечислением.

После внесения всех этих изменений можно использовать следующий код в Xamarin.Android, чтобы задать `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели, как Xamarin.Android использует метаданные для преобразования определения API из *формата AOSP* *Google*. После внесения изменений, которые можно выполнить с помощью файла *Metadata.xml*, рассмотрены ограничения, обнаруженные при переименовании элементов и представлен список поддерживаемых атрибутов XML с описанием случаев, когда следует использовать каждый из них.

## <a name="related-links"></a>Связанные ссылки

- [Работа с JNI](~/android/platform/java-integration/working-with-jni.md)
- [Привязка библиотеки Java](~/android/platform/binding-java-library/index.md)
- [Метаданные GAPI](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
