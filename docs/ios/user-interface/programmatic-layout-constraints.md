---
title: Программные ограничения макета в Xamarin. iOS
description: В этом руководство рассматривается работа с ограничениями автоматического макета iOS в коде C# вместо создания их в конструкторе iOS.
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 3dd413d6d747c9bbec43e5a88f7e24b8e7868327
ms.sourcegitcommit: 4bbf54d2bc1df96af69814e2e5dae47be12e0474
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102602909"
---
# <a name="programmatic-layout-constraints-in-xamarinios"></a>Программные ограничения макета в Xamarin. iOS

_В этом руководство рассматривается работа с ограничениями автоматического макета iOS в коде C# вместо создания их в конструкторе iOS._

Автоматический макет (также называемый «адаптивным макетом») — это подход, реагирующий на быстроту разработки. В отличие от системы трехмерного макета, где расположение каждого элемента жестко запрограммировано на точку на экране, Автоматическая разметка связана с *связями* — позициями элементов относительно других элементов в области конструктора. В основе автоматического макета лежит идея ограничений или правил, определяющих размещение элемента или набора элементов в контексте других элементов на экране. Поскольку элементы не привязаны к определенной положению на экране, ограничения помогают создать адаптивный макет, хорошо выглядящий на различных размерах экрана и ориентациях устройств.

Обычно при работе с автоматической разметкой в iOS вы будете использовать Interface Builder Xcode для графического размещения ограничений макета для элементов пользовательского интерфейса. Однако иногда приходится создавать и применять ограничения в коде C#. Например, при использовании динамически создаваемых элементов пользовательского интерфейса, добавленных в `UIView` .

В этом руководство показано, как создавать ограничения и работать с ними с помощью кода C#, а не создавать их графически в Interface Builder Xcode.

<a name="Creating-Constraints-Programmatically"></a>

## <a name="creating-constraints-programmatically"></a>Создание ограничений программным способом

Как упоминалось выше, обычно вы будете работать с ограничениями автоматического макета в конструкторе iOS. В тех случаях, когда вам нужно создать ограничения программным способом, можно выбрать один из трех вариантов.

- [Привязки макета](#Layout-Anchors) . Этот API предоставляет доступ к свойствам привязки (таким как `TopAnchor` , `BottomAnchor` или `HeightAnchor` ) элементов пользовательского интерфейса, которые ограничены.
- [Ограничения макета](#Layout-Constraints) . ограничения можно создавать непосредственно с помощью `NSLayoutConstraint` класса.
- [Язык форматирования визуальных элементов](#Visual-Format-Language) — предоставляет набор ASCII, например метод для определения ограничений.

В следующих разделах подробно рассматривается каждый из этих параметров.

<a name="Layout-Anchors"></a>

### <a name="layout-anchors"></a>Привязки макета

С помощью `NSLayoutAnchor` класса у вас есть интерфейс Fluent для создания ограничений на основе свойств привязки элементов пользовательского интерфейса, которые ограничены. Например, верхние и нижние направляющие для контроллера представления предоставляют `TopAnchor` свойства, `BottomAnchor` и `HeightAnchor` привязки, тогда как представление предоставляет свойства ребра, центрирования, размера и базового плана.

> [!IMPORTANT]
> В дополнение к стандартному набору свойств привязки, в представлениях iOS также `LayoutMarginsGuides` есть `ReadableContentGuide` Свойства и. Эти свойства предоставляют `UILayoutGuide` объекты для работы с полями представления и руководствами по содержимому для чтения соответственно.

Привязки макета предоставляют несколько методов для создания ограничений в удобном для чтения компактном формате:

- **Констраинтекуалто** — определяет связь, где `first attribute = second attribute + [constant]` с необязательно указанным `constant` значением смещения.
- **Констраинтгреатерсанорекуалто** — определяет связь, где `first attribute >= second attribute + [constant]` с необязательно указанным `constant` значением смещения.
- **Констраинтлесссанорекуалто** — определяет связь, где `first attribute <= second attribute + [constant]` с необязательно указанным `constant` значением смещения.

Пример:

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

Типичное ограничение макета может быть выражено просто в виде линейного выражения. Возьмем следующий пример:

[![Ограничение макета, выраженное в виде линейного выражения](programmatic-layout-constraints-images/graph01.png)](programmatic-layout-constraints-images/graph01.png#lightbox)

Который преобразуется в следующую строку кода C# с использованием привязок макета:

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

Где части кода C# соответствуют заданным частям уравнения следующим образом:

|Дробь|Код|
|---|---|
|Элемент 1|пурплевиев|
|Атрибут 1|леадинганчор|
|Relationship|констраинтекуалто|
|Множитель|Значение по умолчанию — 1,0, поэтому не указано|
|Элемент 2|оранжевиев|
|Атрибут 2|траилинганчор|
|Константа|10.0|

Помимо предоставления только параметров, необходимых для решения определенного уравнения ограничения макета, каждый из методов привязки макета применяет безопасность типа передаваемых им параметров. Таким образом, горизонтальные привязки ограничений, такие как `LeadingAnchor` или, `TrailingAnchor` могут использоваться только с другими типами горизонтальных привязок, а множители — только для ограничений размера.

<a name="Layout-Constraints"></a>

### <a name="layout-constraints"></a>Ограничения макета

Можно добавить ограничения автоматического макета вручную, напрямую создав `NSLayoutConstraint` в коде C#. В отличие от привязок макета, необходимо указать значение для каждого параметра, даже если оно не будет влиять на определяемое ограничение. В результате вы получите значительный объем сложного для чтения и создания стандартного кода. Пример:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

Где `NSLayoutAttribute` перечисление определяет значение для полей представления и соответствует `LayoutMarginsGuide` свойствам, таким как `Left` , `Right` и, `Top` `Bottom` а `NSLayoutRelation` перечисление определяет связь, которая будет создана между заданными атрибутами как `Equal` `LessThanOrEqual` или `GreaterThanOrEqual` .

В отличие от API привязки макета, `NSLayoutConstraint` методы создания не выделяют важные аспекты определенного ограничения, и для ограничения не выполняются проверки времени компиляции. В результате легко создать недопустимое ограничение, которое вызовет исключение во время выполнения.

<a name="Visual-Format-Language"></a>

### <a name="visual-format-language"></a>Язык визуального формата

Язык визуального формата позволяет определять ограничения с помощью таких рисунков ASCII, как строки, которые предоставляют визуальное представление создаваемого ограничения. Это имеет следующие преимущества и недостатки.

- Язык визуального формата обеспечивает только создание допустимых ограничений.
- Автоматическая разметка выводит ограничения на консоль, используя язык визуального формата, поэтому сообщения отладки будут похожи на код, используемый для создания ограничения.
- Язык визуального формата позволяет создавать несколько ограничений одновременно с очень компактным выражением.
- Поскольку проверка строк языка визуального формата отсутствует, проблемы могут обнаруживаться только во время выполнения.
- Так как язык визуального формата подчеркивает визуализацию на полноту, некоторые типы ограничений не могут быть созданы с ним (например, соотношение).

При использовании языка визуального формата для создания ограничения необходимо выполнить следующие шаги.

1. Создайте объект `NSDictionary` , содержащий объекты представления и направляющие макета, а также строковый ключ, который будет использоваться при определении форматов.
2. При необходимости создайте объект `NSDictionary` , определяющий набор ключей и значений ( `NSNumber` ), используемых в качестве постоянного значения для ограничения.
3. Создайте строку формата для разметки одного столбца или строки элементов.
4. Вызовите `FromVisualFormat` метод класса, `NSLayoutConstraint` чтобы создать ограничения.
5. Вызовите `ActivateConstraints` метод класса, `NSLayoutConstraint` чтобы активировать и применить ограничения.

Например, для создания начального и конечного ограничений в языке визуального формата можно использовать следующее:

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Поскольку язык визуального формата всегда создает ограничения нулевых точек, присоединенные к полям родительского представления при использовании расстояния по умолчанию, этот код создает идентичные результаты в приведенных выше примерах.

Для более сложных дизайнов пользовательского интерфейса, таких как несколько дочерних представлений в одной строке, язык визуального формата задает как горизонтальный, так и вертикальный выравнивание. Как и в приведенном выше примере, он указывает, что `AlignAllTop` `NSLayoutFormatOptions` все представления в строке или столбце выдаются по верхним краям.

Некоторые примеры указания общих ограничений и грамматики строк визуального формата см. в разделе [приложение по языку визуального формата](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) Apple.

<a name="Summary"></a>

## <a name="summary"></a>Итоги

В этом руководство было представлено создание и работа с ограничениями автоматического макета в C#, а не их графическим созданием в конструкторе iOS. Во-первых, он рассматривал использование привязок макета ( `NSLayoutAnchor` ) для автоматической разметки. Далее было показано, как работать с ограничениями макета ( `NSLayoutConstraint` ). Наконец, он был представлен с использованием языка визуального формата для автоматического макета.

## <a name="related-links"></a>Связанные ссылки

- [Введение в раскадровку](~/ios/user-interface/storyboards/index.md)
- [Пошаговое руководство по разработке элементов управления для iOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Автоматический макет с Xamarin Designer для iOS](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Программное создание ограничений Apple](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Приложение для языка Apple-Visual Format](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
