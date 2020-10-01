---
title: Повторно используемая реакция на событие EffectBehavior
description: Реакции на события удобно использовать для добавления эффекта в элемент управления, удаления стереотипного эффекта, обработки кода из файлов кода программной части. В этой статье демонстрируется создание и использование реакции на событие Xamarin.Forms для добавления эффекта в элемент управления.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3a085226da3c63f4f151bc657976bba384c02170
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91561148"
---
# <a name="reusable-effectbehavior"></a>Повторно используемая реакция на событие EffectBehavior

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)

_Реакции на события удобно использовать для добавления эффекта в элемент управления, удаления стереотипного эффекта, обработки кода из файлов кода программной части. В этой статье демонстрируется создание и использование реакции на событие Xamarin.Forms для добавления эффекта в элемент управления._

## <a name="overview"></a>Обзор

Класс `EffectBehavior` представляет собой повторно используемую пользовательскую реакцию на событие Xamarin.Forms, которая добавляет экземпляр [`Effect`](xref:Xamarin.Forms.Effect) в элемент управления, когда реакция на событие присоединяется к элементу управления, и удаляет экземпляр `Effect`, когда она отсоединяется от элемента управления.

Для использования реакции на событие необходимо задать следующие ее свойства:

- **Group** — значение атрибута [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) для класса эффекта;
- **Name** — значение атрибута [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) для класса эффекта.

Дополнительные сведения об эффектах см. в статье [Эффекты](~/xamarin-forms/app-fundamentals/effects/index.md).

> [!NOTE]
> `EffectBehavior` — это пользовательский класс, который можно найти в [примере реакции на событие для эффекта](/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior). Он не входит в Xamarin.Forms.

## <a name="creating-the-behavior"></a>Создание реакции на событие

Класс `EffectBehavior` является производным от класса [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1), где `T` — это [`View`](xref:Xamarin.Forms.View). Это означает, что класс `EffectBehavior` можно присоединять к любому элементу управления Xamarin.Forms.

### <a name="implementing-bindable-properties"></a>Реализация привязываемых свойств

В классе `EffectBehavior` определены два экземпляра [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), которые служат для добавления [`Effect`](xref:Xamarin.Forms.Effect) в элемент управления, когда реакция на событие присоединяется к элементу управления. Эти свойства показаны в следующем примере кода.

```csharp
public class EffectBehavior : Behavior<View>
{
  public static readonly BindableProperty GroupProperty =
    BindableProperty.Create ("Group", typeof(string), typeof(EffectBehavior), null);
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(EffectBehavior), null);

  public string Group {
    get { return (string)GetValue (GroupProperty); }
    set { SetValue (GroupProperty, value); }
  }

  public string Name {
    get { return(string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }
  ...
}
```

При использовании `EffectBehavior` свойству `Group` должно присваиваться значение атрибута [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) для эффекта. Кроме того, свойству `Name` должно присваиваться значение атрибута [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) для класса эффекта.

### <a name="implementing-the-overrides"></a>Реализация переопределений

В классе `EffectBehavior` переопределяются методы [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) and [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) класса [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1), как показано в следующем примере кода:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  protected override void OnAttachedTo (BindableObject bindable)
  {
    base.OnAttachedTo (bindable);
    AddEffect (bindable as View);
  }

  protected override void OnDetachingFrom (BindableObject bindable)
  {
    RemoveEffect (bindable as View);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

Метод [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) method performs setup by calling the `AddEffect` method, passing in the attached control as a parameter. The [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) выполняет очистку, вызывая метод `RemoveEffect` и передавая присоединенный элемент управления в качестве параметра.

### <a name="implementing-the-behavior-functionality"></a>Реализация функциональности реакции на событие

Назначение реакции на событие заключается в добавлении эффекта [`Effect`](xref:Xamarin.Forms.Effect), определенного в свойствах `Group` и `Name`, в элемент управления, когда реакция на событие присоединяется к элементу управления, и удалении `Effect`, когда реакция на событие удаляется из элемента управления. Основная функциональность реакции на событие показана в следующем примере кода.

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  void AddEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Add (GetEffect ());
    }
  }

  void RemoveEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Remove (GetEffect ());
    }
  }

  Effect GetEffect ()
  {
    if (!string.IsNullOrWhiteSpace (Group) && !string.IsNullOrWhiteSpace (Name)) {
      return Effect.Resolve (string.Format ("{0}.{1}", Group, Name));
    }
    return null;
  }
}
```

Метод `AddEffect` выполняется в ответ на присоединение `EffectBehavior` к элементу управления и принимает присоединенный элемент управления в качестве параметра. Затем этот метод добавляет полученный эффект в коллекцию [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления. Метод `RemoveEffect` выполняется в ответ на отсоединение `EffectBehavior` от элемента управления и принимает присоединенный элемент управления в качестве параметра. Затем этот метод удаляет эффект из коллекции [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления.

Метод `GetEffect` извлекает [`Effect`](xref:Xamarin.Forms.Effect) с помощью метода [`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)). Эффект определяется путем объединения значений свойств `Group` и `Name`. Если платформа не предоставляет эффект, метод `Effect.Resolve` возвращает значение, отличное от `null`.

## <a name="consuming-the-behavior"></a>Использование реакции на событие

Класс `EffectBehavior` можно присоединить к коллекции [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) элемента управления, как показано в следующем примере кода XAML.

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};
label.Behaviors.Add (new EffectBehavior {
  Group = "Xamarin",
  Name = "LabelShadowEffect"
});
```

Свойствам `Group` и `Name` реакции на событие присваиваются значения атрибутов [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) и [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) для класса эффекта в каждом зависящем от платформы проекте.

Когда во время выполнения реакция на событие присоединяется к элементу управления [`Label`](xref:Xamarin.Forms.Label), `Xamarin.LabelShadowEffect` добавляется в коллекцию [`Effects`](xref:Xamarin.Forms.Element.Effects) элемента управления. В результате тень добавляется к тексту, отображаемому элементом управления `Label`, как показано на следующих снимках экрана.

![Пример приложения с EffectsBehavior](effect-behavior-images/screenshots.png)

Преимущество добавления эффектов в элементы управления и удаления эффектов из них с помощью реакций на события заключается в том, что код для обработки стереотипных эффектов можно удалить из файлов кода программной части.

## <a name="summary"></a>Сводка

В этой статье было продемонстрировано использование реакции на событие для добавления эффекта в элемент управления. Класс `EffectBehavior` представляет собой повторно используемую пользовательскую реакцию на событие Xamarin.Forms, которая добавляет экземпляр [`Effect`](xref:Xamarin.Forms.Effect) в элемент управления, когда реакция на событие присоединяется к элементу управления, и удаляет экземпляр `Effect`, когда она отсоединяется от элемента управления.

## <a name="related-links"></a>Связанные ссылки

- [Эффекты](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Реакция на событие для эффекта (пример)](/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)
- [Реакция на событие](xref:Xamarin.Forms.Behavior)
- [Реакция на событие&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)