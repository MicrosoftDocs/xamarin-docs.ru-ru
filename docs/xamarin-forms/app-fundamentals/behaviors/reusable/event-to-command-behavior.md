---
title: Повторно используемый EventToCommandBehavior
description: Реакции на события можно использовать для сопоставления команд с элементами управления, которые не были предназначены для взаимодействия с командами. В этой статье демонстрируется создание и использование реакции на событие Xamarin.Forms для выполнения команды при возникновении события.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/09/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0967845ac61ddf5f8e1cc76664a50877d041f011
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939676"
---
# <a name="reusable-eventtocommandbehavior"></a>Повторно используемый EventToCommandBehavior

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

_Реакции на события можно использовать для сопоставления команд с элементами управления, которые не были предназначены для взаимодействия с командами. В этой статье демонстрируется создание и использование реакции на событие Xamarin.Forms для выполнения команды при возникновении события._

## <a name="overview"></a>Обзор

Класс `EventToCommandBehavior` является повторно используемой пользовательской реакцией на событие Xamarin.Forms, которая выполняет команду в ответ на *любое* вызываемое событие. По умолчанию аргументы для события передаются в команду, а при необходимости могут быть преобразованы реализацией [`IValueConverter`](xref:Xamarin.Forms.IValueConverter).

Для использования реакции на событие необходимо задать следующие ее свойства:

- **EventName** — имя события, которое отслеживает реакция на события.
- **Команда** — `ICommand` для выполнения. Реакция на событие ожидает найти экземпляр `ICommand` в [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) из присоединенного элемента управления, который может быть унаследован от родительского элемента.

Кроме того, можно задать следующие необязательные свойства реакции на событие:

- **CommandParameter** — `object`, который будет передан в команду.
- **Converter** — реализация [`IValueConverter`](xref:Xamarin.Forms.IValueConverter), которая изменит формат аргумента события при его передаче между *источником* и *целевым объектом* модулем привязки.

> [!NOTE]
> `EventToCommandBehavior` — это пользовательский класс, который можно найти в [примере реакции на событие EventToCommand](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior). Он не входит в Xamarin.Forms.

## <a name="creating-the-behavior"></a>Создание реакции на событие

Класс `EventToCommandBehavior` является производным от класса `BehaviorBase<T>`, который, в свою очередь, является производным от класса [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1). Класс `BehaviorBase<T>` служит для предоставления базового класса для любых реакций на событие Xamarin.Forms, требующих установить присоединенный элемент управления для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) реакции на событие. Это гарантирует, что при использовании реакция на событие может осуществить привязку к `ICommand`, определяемому свойством `Command`, и выполнить его.

Класс `BehaviorBase<T>` предоставляет переопределяемый метод [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) method that sets the [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) of the behavior and an overridable [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)), очищающий `BindingContext`. Кроме того, класс хранит ссылку на присоединенный элемент управления в свойстве `AssociatedObject`.

### <a name="implementing-bindable-properties"></a>Реализация привязываемых свойств

Класс `EventToCommandBehavior` определяет четыре экземпляра [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), выполняющих определенную пользователем команду при возникновении события. Эти свойства показаны в следующем примере кода:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

При использовании класса `EventToCommandBehavior` свойство `Command` должно иметь привязку данных к `ICommand`, чтобы выполняться в ответ на возникшее событие, которое определено в свойстве `EventName`. Реакция на событие ожидает найти `ICommand` в [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) присоединенного элемента управления.

По умолчанию аргументы для события передаются в команду. При необходимости при передаче между *источником* и *целевым объектом* модулем привязки эти данные можно преобразовать, указав реализацию [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) в качестве значения свойства `Converter`. Кроме того, параметр можно передать в команду, указав значение свойства `CommandParameter`.

### <a name="implementing-the-overrides"></a>Реализация переопределений

В классе `EventToCommandBehavior` переопределяются методы [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) and [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) класса `BehaviorBase<T>`, как показано в следующем примере кода:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

Метод [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) method performs setup by calling the `RegisterEvent` method, passing in the value of the `EventName` property as a parameter. The [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) выполняет очистку, вызывая метод `DeregisterEvent` и передавая значение свойства `EventName` в качестве параметра.

### <a name="implementing-the-behavior-functionality"></a>Реализация функциональности реакции на событие

Задачей этой реакции на событие является выполнение команды, определенной свойством `Command`, в ответ на возникшее событие, которое определено свойством `EventName`. Основная функциональность реакции на событие показана в следующем примере кода:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }        

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

Метод `RegisterEvent` выполняется в ответ на присоединение `EventToCommandBehavior` к элементу управления и принимает значение свойства `EventName` в качестве параметра. Затем метод пытается найти событие, определенное в свойстве `EventName`, для присоединенного элемента управления. При условии, что событие может быть найдено, метод `OnEvent` регистрируется в качестве метода обработчика для события.

Метод `OnEvent` выполняется в ответ на возникшее событие, которое определено в свойстве `EventName`. При условии, что свойство `Command` ссылается на допустимый `ICommand`, метод пытается получить параметр для передачи в `ICommand` следующим образом:

- Если свойство `CommandParameter` определяет параметр, он извлекается.
- В противном случае, если свойство `Converter` определяет реализацию [`IValueConverter`](xref:Xamarin.Forms.IValueConverter), выполняется конвертер, преобразующий данные аргументов события при их передаче между *источником* и *целевым объектом* модулем привязки.
- В противном случае аргументы события считаются параметром.

Затем выполняется `ICommand` с привязкой данных, передавая параметр в команду, при условии, что метод [`CanExecute`](xref:Xamarin.Forms.Command.CanExecute(System.Object)) возвращает `true`.

Хотя это здесь и не показано, `EventToCommandBehavior` также включает в себя метод `DeregisterEvent`, который выполняется свойством [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) method. The `DeregisterEvent` method is used to locate and deregister the event defined in the `EventName` для очистки потенциальных утечек памяти.

## <a name="consuming-the-behavior"></a>Использование реакции на событие

Класс `EventToCommandBehavior` можно присоединить к коллекции [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) элемента управления, как показано в следующем примере кода XAML.

```xaml
<ListView ItemsSource="{Binding People}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding Name}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.Behaviors>
        <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}" Converter="{StaticResource SelectedItemConverter}" />
    </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var listView = new ListView();
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "People");
listView.ItemTemplate = new DataTemplate(() =>
{
    var textCell = new TextCell();
    textCell.SetBinding(TextCell.TextProperty, "Name");
    return textCell;
});
listView.Behaviors.Add(new EventToCommandBehavior
{
    EventName = "ItemSelected",
    Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
    Converter = new SelectedItemEventArgsToSelectedItemConverter()
});

var selectedItemLabel = new Label();
selectedItemLabel.SetBinding(Label.TextProperty, "SelectedItemText");
```

Свойство `Command` реакции на событие имеет привязку данных к свойству `OutputAgeCommand` соответствующей модели представления, хотя для свойства `Converter` задан экземпляр `SelectedItemConverter`, в результате чего возвращается [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) объекта [`ListView`](xref:Xamarin.Forms.ListView) из [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs).

Во время выполнения реакция на событие будет реагировать на взаимодействие с элементом управления. При выборе элемента в [`ListView`](xref:Xamarin.Forms.ListView) активируется событие [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected), из-за чего выполняется `OutputAgeCommand` в модели представления. В свою очередь, это вызывает обновление свойства `SelectedItemText` модели представления, к которому привязан [`Label`](xref:Xamarin.Forms.Label), как показано на следующем снимке экрана:

[![Пример приложения с EventToCommandBehavior](event-to-command-behavior-images/screenshots-sml.png)](event-to-command-behavior-images/screenshots.png#lightbox "Пример приложения с EventToCommandBehavior")

Преимуществом использования этой реакции на событие для выполнения команды при возникновении события является то, что команды можно сопоставить с элементами управления, которые не были предназначены для взаимодействия с командами. Кроме того, это удаляет стандартный код обработки событий из файлов кода программной части.

## <a name="summary"></a>Сводка

В этой статье демонстрируется использование реакции на событие Xamarin.Forms для выполнения команды при возникновении события. Реакции на события можно использовать для сопоставления команд с элементами управления, которые не были предназначены для взаимодействия с командами.

## <a name="related-links"></a>Связанные ссылки

- [Реакция на событие EventToCommand (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [Реакция на событие](xref:Xamarin.Forms.Behavior)
- [Реакция на событие&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
