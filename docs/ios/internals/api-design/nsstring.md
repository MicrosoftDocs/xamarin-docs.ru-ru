---
title: NSString в Xamarin.iOS и Xamarin.Mac
description: В этом документе описывается, как Xamarin.iOS прозрачно преобразует объекты NSString в строковые объекты C#, когда это не происходит.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 314c94fc9208a63e2f9305511df262327df921a5
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84565074"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>NSString в Xamarin.iOS и Xamarin.Mac

Структура Xamarin.iOS и Xamarin.Mac требует использования API для отображения собственного типа строки .NET, `string`, для использования строк на C# и других языках программирования .NET, и для отображения строки в качестве типа данных, предоставляемого API, а не типом данных `NSString` .

Это означает, что разработчикам не нужно хранить строки, предназначенные для вызова Xamarin.iOS и API Xamarin.Mac (унифицированный) в специальном типе (`Foundation.NSString`), они могут использовать `System.String` Mono для всех операций, и каждый раз, когда для API в Xamarin.iOS или Xamarin.Mac требуется строка, поскольку наша привязка API выполняет упаковку информации.

Например, свойство Objective-C "text" в `UILabel` типа `NSString` объявляется следующим образом:

```objc
@property(nonatomic, copy) NSString *text
```

Оно предоставляется в Xamarin.iOS как:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

В фоновом режиме реализация этого свойства маршалирует строку C# в `NSString` и вызывает метод `objc_msgSend` так же, как это делал бы метод Objective-C.

Существует несколько сторонних API Objective-C, которые используют не `NSString`, а строку C ("*char*"). В таких случаях вы по-прежнему сможете использовать строковый тип данных C#, однако также понадобиться атрибут [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md), чтобы сообщить генератору привязки, что эта строка должна упаковываться не как `NSString`, а как строка C.

 <a name="Exceptions_to_the_Rule"></a>

## <a name="exceptions-to-the-rule"></a>Исключения из правил

В Xamarin.iOS и Xamarin.Mac создано исключение из этого правила. Решение в случае, когда мы предоставляем `string`, и когда мы делаем исключение и предоставляем `NSString`, выполняется, если метод  `NSString` может выполнить сравнение указателей вместо сравнения содержимого.

Это может произойти, когда API-интерфейсы Objective-C используют общедоступную `NSString` константу в качестве маркера, представляющего некоторое действие, вместо сравнения фактического содержимого строки.

В этих случаях `NSString` API подвержены воздействию, и таких API, которые имеют это воздействие, мало. Также обратите внимание, что свойства NSString представлены в некоторых классах. Эти свойства `NSString` предоставляются для таких элементов, как уведомления. Эти свойства обычно выглядят следующим образом:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

Уведомления — это ключи, которые используются для класса `NSNotification`, если необходимо выполнить регистрацию для конкретного события, транслируемого средой выполнения.

Ключи обычно выглядят примерно так:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Другое место, где `NSString` представлены в API, — это маркеры, используемые в качестве параметров для некоторых API в iOS или OS X, принимающие объекты `NSDictionary` в качестве параметров. Словарь обычно содержит ключи `NSString`. Xamarin.iOS, по соглашению, именует статические свойства `NSString`, добавляя имя "Ключ".
