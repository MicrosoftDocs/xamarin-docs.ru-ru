---
title: Присоединенные реакции на события
description: Присоединенные реакции на события — это статические классы с одним или несколькими присоединенными свойствами. В этой статье содержатся сведения о создании и использовании присоединенных реакций на события.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d6c8154891f6ec295b7082dc6eebe46916d4e4e0
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368078"
---
# <a name="attached-behaviors"></a>Присоединенные реакции на события

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/behaviors-attachednumericvalidationbehavior)

_Присоединенные реакции на события — это статические классы с одним или несколькими присоединенными свойствами. В этой статье содержатся сведения о создании и использовании присоединенных реакций на события._

## <a name="overview"></a>Обзор

Присоединенное свойство — это привязываемое свойство особого типа. Оно определяется в одном классе, но присоединяется к другим объектам и распознается в XAML как атрибут, который содержит имя класса и имя свойства, разделенные точкой.

Присоединенное свойство может определять делегат `propertyChanged`, который будет выполняться при изменении значения свойства, например при задании свойства для элемента управления. Когда делегат `propertyChanged` выполняется, ему передается ссылка на элемент управления, к которому он присоединяется, а также параметры, содержащие старое и новое значения свойства. С помощью этого делегата можно расширять функциональность элемента управления, к которому присоединяется свойство, путем манипуляций с передаваемой ссылкой.

1. Делегат `propertyChanged` приводит ссылку на элемент управления, полученную как [`BindableObject`](xref:Xamarin.Forms.BindableObject), к типу элемента управления, который должна расширять реакция на событие.
1. Делегат `propertyChanged` изменяет свойства элемента управления, вызывает его методы или регистрирует обработчики событий, предоставляемых элементом управления, для реализации базовой функциональности реакции на событие.

Проблема при использовании присоединенных реакций на события — в том, что они определяются в классе `static` и имеют свойства и методы `static`. Это затрудняет создание присоединенных реакций на события с состоянием. Кроме того, при создании реакций на события предпочтительнее использовать реакции на события Xamarin.Forms, а не присоединенные реакции на события. Дополнительные сведения о расширениях функциональности Xamarin.Forms см. в разделе [Расширения функциональности Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md).

## <a name="creating-an-attached-behavior"></a>Создание присоединенной реакции на событие

В примере приложения демонстрируется реакция на событие `NumericValidationBehavior`, которая выделяет значение, введенное пользователем в элементе управления [`Entry`](xref:Xamarin.Forms.Entry), красным цветом, если оно не имеет тип `double`. Эта реакция на событие показана в следующем примере кода:

```csharp
public static class NumericValidationBehavior
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached (
            "AttachBehavior",
            typeof(bool),
            typeof(NumericValidationBehavior),
            false,
            propertyChanged:OnAttachBehaviorChanged);

    public static bool GetAttachBehavior (BindableObject view)
    {
        return (bool)view.GetValue (AttachBehaviorProperty);
    }

    public static void SetAttachBehavior (BindableObject view, bool value)
    {
        view.SetValue (AttachBehaviorProperty, value);
    }

    static void OnAttachBehaviorChanged (BindableObject view, object oldValue, object newValue)
    {
        var entry = view as Entry;
        if (entry == null) {
            return;
        }

        bool attachBehavior = (bool)newValue;
        if (attachBehavior) {
            entry.TextChanged += OnEntryTextChanged;
        } else {
            entry.TextChanged -= OnEntryTextChanged;
        }
    }

    static void OnEntryTextChanged (object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Класс `NumericValidationBehavior` содержит присоединенное свойство `AttachBehavior` с методами задания и получения `static`, управляющее добавлением и удалением реакции на событие в элементе управления, к которому она будет присоединена. Это присоединенное свойство регистрирует метод `OnAttachBehaviorChanged`, который будет выполняться при изменении значения свойства. В зависимости от значения присоединенного свойства `AttachBehavior` этот метод регистрирует обработчик событий [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) или отменяет его регистрацию. Базовая функциональность реакции на событие обеспечивается методом `OnEntryTextChanged`, который анализирует значение, введенное пользователем в элементе управления [`Entry`](xref:Xamarin.Forms.Entry), и присваивает свойству `TextColor` значение red (красный), если введенное значение не имеет тип `double`.

## <a name="consuming-an-attached-behavior"></a>Использование присоединенной реакции на событие

Класс `NumericValidationBehavior` можно использовать путем добавления присоединенного свойства `AttachBehavior` к элементу управления [`Entry`](xref:Xamarin.Forms.Entry), как показано в следующем примере кода XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

В следующем примере кода показан эквивалентный элемент управления [`Entry`](xref:Xamarin.Forms.Entry) на языке C#:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Во время выполнения реакция на событие будет реагировать на взаимодействие с элементом управления в соответствии с ее реализацией. На следующих снимках экрана показана присоединенная реакция на событие ввода недопустимого значения:

[![Пример приложения с присоединенной реакцией на событие](attached-images/screenshots-sml.png)](attached-images/screenshots.png#lightbox "Пример приложения с присоединенной реакцией на событие")

> [!NOTE]
> Присоединенная реакция на событие создается для элементов управления определенного типа (или суперкласса, включающего множество разных элементов управления) и должна добавляться только к совместимым элементам управления. Попытка присоединить реакцию на событие к несовместимому элементу управления может привести к неожиданному результату, который зависит от реализации реакции на событие.

### <a name="removing-an-attached-behavior-from-a-control"></a>Удаление присоединенной реакции на событие из элемента управления

Класс `NumericValidationBehavior` можно удалить из элемента управления путем присвоения присоединенному свойству `AttachBehavior` значения `false`, как показано в следующем примере кода XAML:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

В следующем примере кода показан эквивалентный элемент управления [`Entry`](xref:Xamarin.Forms.Entry) на языке C#:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

Во время выполнения метод `OnAttachBehaviorChanged` выполняется, когда присоединенному свойству `AttachBehavior` присваивается значение `false`. Метод `OnAttachBehaviorChanged` отменяет регистрацию обработчика событий [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged), чтобы реакция на событие не выполнялась, когда пользователь взаимодействует с элементом управления.

## <a name="summary"></a>Сводка

В этой статье было показано, как создавать и использовать присоединенные реакции на события. Присоединенные реакции на события — это классы `static` с одним или несколькими присоединенными свойствами.

## <a name="related-links"></a>Связанные ссылки

- [Присоединенные реакции на события (пример)](/samples/xamarin/xamarin-forms-samples/behaviors-attachednumericvalidationbehavior)