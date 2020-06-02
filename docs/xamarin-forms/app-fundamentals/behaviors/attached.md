---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 08b1c738dc87bb9373436a3fd96486eb15341112
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139429"
---
# <a name="attached-behaviors"></a>Присоединенные реакции на события

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-attachednumericvalidationbehavior)

_Присоединенные реакции на события — это статические классы с одним или несколькими присоединенными свойствами. В этой статье содержатся сведения о создании и использовании присоединенных реакций на события._

## <a name="overview"></a>Обзор

Присоединенное свойство — это привязываемое свойство особого типа. Оно определяется в одном классе, но присоединяется к другим объектам и распознается в XAML как атрибут, который содержит имя класса и имя свойства, разделенные точкой.

Присоединенное свойство может определять делегат `propertyChanged`, который будет выполняться при изменении значения свойства, например при задании свойства для элемента управления. Когда делегат `propertyChanged` выполняется, ему передается ссылка на элемент управления, к которому он присоединяется, а также параметры, содержащие старое и новое значения свойства. С помощью этого делегата можно расширять функциональность элемента управления, к которому присоединяется свойство, путем манипуляций с передаваемой ссылкой.

1. Делегат `propertyChanged` приводит ссылку на элемент управления, полученную как [`BindableObject`](xref:Xamarin.Forms.BindableObject), к типу элемента управления, который должна расширять реакция на событие.
1. Делегат `propertyChanged` изменяет свойства элемента управления, вызывает его методы или регистрирует обработчики событий, предоставляемых элементом управления, для реализации базовой функциональности реакции на событие.

Проблема при использовании присоединенных реакций на события — в том, что они определяются в классе `static` и имеют свойства и методы `static`. Это затрудняет создание присоединенных реакций на события с состоянием. Кроме того, при создании реакций на события предпочтительнее использовать реакции на события Xamarin.Forms, а не присоединенные реакции на события. Дополнительные сведения о реакциях на события Xamarin.Forms см. в статьях [Реакции на события Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) и [Повторно используемые реакции на события](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

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

[![](attached-images/screenshots-sml.png "Sample Application with Attached Behavior")](attached-images/screenshots.png#lightbox "Sample Application with Attached Behavior")

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

- [Присоединенные реакции на события (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-attachednumericvalidationbehavior)
