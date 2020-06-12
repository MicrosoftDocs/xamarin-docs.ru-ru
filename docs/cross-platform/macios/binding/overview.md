---
title: Общие сведения о привязках Objective-C
description: В этом документе представлен обзор различных способов создания привязок C# для кода Objective-C, включая привязки командной строки, привязки проектов и Objective Sharpie. Здесь также рассматривается принцип работы привязок.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: ca83f6ced2e9c2f5380d3bf760e00d613cb0acb0
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570977"
---
# <a name="overview-of-objective-c-bindings"></a>Общие сведения о привязках Objective-C

_Сведения о принципах работы привязок_

Процесс привязки библиотеки Objective-C для использования с Xamarin состоит из трех шагов:

1. Создайте определение API на C#, чтобы описать, как осуществляется доступ к нативному API в .NET и как этот API сопоставляется с базовым API Objective-C. Для этого используются стандартные конструкции C#, такие как `interface` и различные **атрибуты** привязки (см. этот [простой пример](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Создав определение API на C#, скомпилируйте его для создания сборки привязки. Это можно сделать в [**командной строке**](#command-line-bindings) или с помощью [**проекта привязки**](#bindingproject) в Visual Studio либо Visual Studio для Mac.

3. После этого сборка привязки добавляется в проект приложения Xamarin, чтобы вы смогли получить доступ к нативным функциям с помощью определенного вами API.
   Проект привязки отличается от проектов приложений.

   > [!NOTE]
   > Первый шаг можно автоматизировать с помощью [**Objective Sharpie**](#objectivesharpie). Это средство проверяет API Objective-C и создает предложенное определение API на C#. Вы можете изменить файлы, созданные Objective Sharpie, и использовать их в проекте привязки (или в командной строке) для создания сборки привязки. Objective Sharpie не создает привязки. По сути это необязательная процедура, которая является частью большего процесса.

Можете также ознакомиться с дополнительными техническими сведениями [о работе этого средства](#howitworks), которое поможет при создании привязок.

## <a name="command-line-bindings"></a>Создание привязок с помощью командной строки

Вы можете использовать `btouch-native` для Xamarin.iOS (или `bmac-native` для Xamarin.Mac), чтобы создать привязки напрямую. Для этого нужно передать определения API на C#, которые вы создали вручную (или с помощью Objective Sharpie), в средство командной строки (`btouch-native` для iOS или `bmac-native` для Mac).

Общий синтаксис для вызова этих средств выглядит так:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Приведенная выше команда создаст файл `cocos2d.dll` в текущем каталоге и будет содержать библиотеку с полной привязкой, которую можно использовать в проекте. Это средство применяется в Visual Studio для Mac для создания привязок, если вы используете проект привязки (описанный [ниже](#bindingproject)).

<a name="bindingproject"></a>

## <a name="binding-project"></a>Проект привязки

Проект привязки можно создать в Visual Studio для Mac или Visual Studio (Visual Studio поддерживает только привязки iOS). Это упростит изменение и сборку определений API для привязки по сравнению с использованием командной строки.

Сведения о том, как создать и использовать привязку проекта для реализации привязки, см. в [этом руководстве по началу работы](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started).

<a name="objectivesharpie"></a>

## <a name="objective-sharpie"></a>Objective Sharpie

Objective Sharpie — это еще одно средство командной строки, которое будет полезно на начальных этапах создания привязки. Это средство не создает привязку, но позволяет автоматизировать начальный шаг создания определения API для целевой нативной библиотеки.

Сведения о том, как анализировать нативные библиотеки, нативные платформы и CocoaPods для создания определений API, которые можно встроить в привязки, см. в [документации по Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md).

<a name="howitworks"></a>

## <a name="how-binding-works"></a>Принцип работы привязки

Можно использовать атрибут [[Register]](xref:Foundation.RegisterAttribute), [[Export]](xref:Foundation.ExportAttribute) атрибут и [вызвать селектор Objective-C вручную](~/ios/internals/objective-c-selectors.md), чтобы вручную создать привязку новых типов Objective-C (ранее несвязанные).

1\. Найдите тип, для которого нужно создать привязку. В качестве примера и для простоты мы привяжем тип [NSEnumerator](https://developer.apple.com/documentation/foundation/nsenumerator), для которого уже есть привязка в [Foundation.NSEnumerator](xref:Foundation.NSEnumerator). Приведенная ниже реализация используется лишь для примера.

2\. Нам нужно создать тип C#. Лучшего всего поместить его в пространство имен. Но так как Objective-C не поддерживает пространства имен, мы воспользуемся атрибутом `[Register]`, чтобы изменить имя типа, которое Xamarin.iOS зарегистрирует в среде выполнения Objective-C. Кроме того, тип C# должен быть производным от [Foundation.NSObject](xref:Foundation.NSObject):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

3\. Ознакомьтесь с документацией по Objective-C и создайте экземпляры [ObjCRuntime.Selector](xref:ObjCRuntime.Selector) для каждого селектора, который вы будете использовать. Поместите их в текст класса:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

4\. Для вашего типа нужно предоставить конструкторы. *Нужно* привязать вызов конструктора к конструктору базового класса. Атрибуты `[Export]` позволяют коду Objective-C вызывать конструкторы с указанным именем селектора:

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

5\. Предоставьте методы для каждого селектора, объявленного на шаге 3. В этом случае будет использоваться метод `objc_msgSend()` для вызова селектора в нативном объекте. Обратите внимание на использование [Runtime.GetNSObject()](xref:ObjCRuntime.Runtime.GetNSObject*) для преобразования `IntPtr` в правильно типизированный объект `NSObject` (подтип). Для вызова метода из кода Objective-C элемент *должен* быть **виртуальным**.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

Сборка

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```
