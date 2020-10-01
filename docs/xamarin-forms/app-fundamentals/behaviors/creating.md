---
title: Создание реакций на события Xamarin.Forms
description: Реакции на событие Xamarin.Forms создаются путем наследования от класса Behavior или Behavior<T>. В этой статье содержатся сведения о создании и использовании реакций на события Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 53df681e79c4c233a8908764d31c8ee416c8a621
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91561174"
---
# <a name="create-no-locxamarinforms-behaviors"></a>Создание реакций на события Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-numericvalidationbehavior)

Реакции на событие _Xamarin.Forms создаются путем наследования от класса Behavior или Behavior&lt;T&gt;. В этой статье содержатся сведения о создании и использовании реакций на события Xamarin.Forms._

## <a name="overview"></a>Обзор

Процесс создания реакции на событие Xamarin.Forms выглядит следующим образом:

1. Создайте класс, наследующий от класса [`Behavior`](xref:Xamarin.Forms.Behavior) или [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1), где `T` — это тип элемента управления, к которому должна применяться реакция на событие.
1. Переопределите метод [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) для выполнения требуемой настройки.
1. Переопределите метод [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) для выполнения требуемой очистки.
1. Реализуйте основные функциональные возможности реакции на событие.

В результате вы получите структуру, приведенную в следующем примере кода:

```csharp
public class CustomBehavior : Behavior<View>
{
    protected override void OnAttachedTo (View bindable)
    {
        base.OnAttachedTo (bindable);
        // Perform setup
    }

    protected override void OnDetachingFrom (View bindable)
    {
        base.OnDetachingFrom (bindable);
        // Perform clean up
    }

    // Behavior implementation
}
```

Метод [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) запускается сразу после присоединения реакции на событие к элементу управления. Этот метод получает ссылку на элемент управления, к которому выполнено присоединение, и может использоваться для регистрации обработчиков событий или выполнения других настроек, необходимых для поддержки функциональных возможностей реакции на событие. Например, можно подписаться на событие в элементе управления. В этом случае функциональные возможности реакции на событие будут реализованы в обработчике событий для этого события.

Метод [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) активируется при удалении реакции на событие из элемента управления. Этот метод получает ссылку на элемент управления, к которому выполнено присоединение, и используется для выполнения любой требуемой очистки. Например, вы можете отменить подписку на событие в элементе управления, чтобы предотвратить утечки памяти.

После этого реакцию на событие можно использовать посредством ее присоединения к коллекции [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) соответствующего элемента управления.

## <a name="creating-a-no-locxamarinforms-behavior"></a>Создание реакции на событие Xamarin.Forms

В примере приложения демонстрируется реакция на событие `NumericValidationBehavior`, которая выделяет значение, введенное пользователем в элементе управления [`Entry`](xref:Xamarin.Forms.Entry), красным цветом, если оно не имеет тип `double`. Эта реакция на событие показана в следующем примере кода:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    protected override void OnAttachedTo(Entry entry)
    {
        entry.TextChanged += OnEntryTextChanged;
        base.OnAttachedTo(entry);
    }

    protected override void OnDetachingFrom(Entry entry)
    {
        entry.TextChanged -= OnEntryTextChanged;
        base.OnDetachingFrom(entry);
    }

    void OnEntryTextChanged(object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` является производным от класса [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1), где `T` — это [`Entry`](xref:Xamarin.Forms.Entry). Метод [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) method registers an event handler for the [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) event, with the [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) отменяет регистрацию события `TextChanged` во избежание утечек памяти. Базовая функциональность реакции на событие обеспечивается методом `OnEntryTextChanged`, который анализирует значение, введенное пользователем в `Entry`, и присваивает свойству [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) значение red (красный), если введенное значение не имеет тип `double`.

> [!NOTE]
> Xamarin.Forms не задает `BindingContext` для реакции на событие, так как реакции на событие можно использовать совместно и применять к нескольким элементам управления через стили.

## <a name="consuming-a-no-locxamarinforms-behavior"></a>Использование реакции на событие Xamarin.Forms

Каждый элемент управления Xamarin.Forms имеет коллекцию [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors), в которую можно добавить одну или несколько реакций на событие, как показано в следующем примере кода XAML:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

В следующем примере кода показан эквивалентный элемент управления [`Entry`](xref:Xamarin.Forms.Entry) на языке C#:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Во время выполнения реакция на событие будет реагировать на взаимодействие с элементом управления в соответствии с ее реализацией. На следующих снимках экрана показана реакция на событие ввода недопустимого значения:

[![Примеры приложений с Xamarin.Forms Поведение](creating-images/screenshots-sml.png)](creating-images/screenshots.png#lightbox "Примеры приложений с Xamarin.Forms Поведение")

> [!NOTE]
> Реакция на событие создается для элементов управления определенного типа (или суперкласса, включающего множество разных элементов управления) и должна добавляться только к совместимым элементам управления. Попытка присоединить реакцию на событие к несовместимому элементу управления может привести к возникновению исключения.

### <a name="consuming-a-no-locxamarinforms-behavior-with-a-style"></a>Использование реакции на событие Xamarin.Forms с помощью стиля

Реакции на событие также могут использоваться явным или неявным стилем. Однако создание стиля, который задает свойство [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) элемента управления, не поддерживается, так как это свойство доступно только для чтения. Решение заключается в том, чтобы добавить присоединенное свойство в класс реакции на событие, управляющий добавлением и удалением реакции на событие. Процесс выглядит следующим образом:

1. Добавьте присоединенное свойство в класс реакции на событие, который будет использоваться для управления добавлением или удалением реакции на событие, в элемент управления, к которому будет присоединена реакция на событие. Это присоединенное свойство должно регистрировать делегат `propertyChanged`, который будет выполняться при изменении значения свойства.
1. Создайте методы получения и задания `static` для присоединенного свойства.
1. Реализуйте логику в делегате `propertyChanged` для добавления и удаления реакции на событие.

В следующем примере кода показано присоединенное свойство, управляющее добавлением и удалением `NumericValidationBehavior`:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached ("AttachBehavior", typeof(bool), typeof(NumericValidationBehavior), false, propertyChanged: OnAttachBehaviorChanged);

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
            entry.Behaviors.Add (new NumericValidationBehavior ());
        } else {
            var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
            if (toRemove != null) {
                entry.Behaviors.Remove (toRemove);
            }
        }
    }
    ...
}
```

Класс `NumericValidationBehavior` содержит присоединенное свойство `AttachBehavior` с методами задания и получения `static`, управляющее добавлением и удалением реакции на событие в элементе управления, к которому она будет присоединена. Это присоединенное свойство регистрирует метод `OnAttachBehaviorChanged`, который будет выполняться при изменении значения свойства. В зависимости от значения присоединенного свойства `AttachBehavior`, этот метод добавляет или удаляет реакцию на событие в элементе управления.

В следующем примере кода показан *явный* стиль для `NumericValidationBehavior`, который использует присоединенное свойство `AttachBehavior` и может применяться к элементам управления [`Entry`](xref:Xamarin.Forms.Entry):

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

Чтобы применить [`Style`](xref:Xamarin.Forms.Style) к элементу управления [`Entry`](xref:Xamarin.Forms.Entry), для его свойства [`Style`](xref:Xamarin.Forms.NavigableElement.Style) следует задать значение экземпляра `Style` с помощью расширения разметки `StaticResource`, как показано в следующем примере кода:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Дополнительные сведения о стилях см. в статье [Стили](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Хотя вы можете добавить привязываемые свойства для реакции на событие, которая задается или запрашивается в XAML, в этом случае нужно создать реакции на событие с состоянием, которое не должно совместно использоваться между элементами управления в `Style` в `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Удаление реакции на событие из элемента управления

Коллекция [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) method is fired when a behavior is removed from a control, and is used to perform any required cleanup such as unsubscribing from an event to prevent a memory leak. However, behaviors are not implicitly removed from controls unless the control's [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) collection is modified by a `Remove` or `Clear` method. The following code example demonstrates removing a specific behavior from a control's `Behaviors`:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Кроме того, коллекцию [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) элемента управления можно очистить, как показано в следующем примере кода:

```csharp
entry.Behaviors.Clear();
```

Обратите внимание, что реакции на событие не удаляются из элементов управления неявным образом при извлечении страниц из стека навигации. Вместо этого их требуется явно удалить до того, как страницы окажутся вне области действия.

## <a name="summary"></a>Сводка

В этой статье было показано создание и использование реакций на события Xamarin.Forms. Реакции на событие Xamarin.Forms создаются путем наследования от классов [`Behavior`](xref:Xamarin.Forms.Behavior) или [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1).

## <a name="related-links"></a>Связанные ссылки

- [Реакция на событие Xamarin.Forms (пример)](/samples/xamarin/xamarin-forms-samples/behaviors-numericvalidationbehavior)
- [Применение реакции на событие Xamarin.Forms с помощью стиля (пример)](/samples/xamarin/xamarin-forms-samples/behaviors-numericvalidationbehaviorstyle)
- [Реакция на событие](xref:Xamarin.Forms.Behavior)
- [Реакция на событие\<T>](xref:Xamarin.Forms.Behavior`1)