---
title: Триггеры Xamarin.Forms
description: В этой статье содержатся сведения об использовании триггеров Xamarin.Forms для реагирования на изменения пользовательского интерфейса с помощью XAML. Триггеры позволяют декларативно задавать действия в XAML, которые изменяют внешний вид элементов управления при изменении событий или свойств.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6f54ec3afd3d53d4a7bee40159bccff2d23f44f6
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98609055"
---
# <a name="no-locxamarinforms-triggers"></a>Триггеры Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/workingwithtriggers)

Триггеры позволяют декларативно задавать действия в XAML, которые изменяют внешний вид элементов управления при изменении событий или свойств. Кроме того, триггеры состояния, которые относятся к специализированной группе триггеров, определяют, когда следует применять [`VisualState`](xref:Xamarin.Forms.VisualState).

Триггер можно напрямую назначить элементу управления или добавить его в словарь ресурсов уровня страницы или приложения для применения к нескольким элементам управления.

## <a name="property-triggers"></a>Триггеры свойств

Простой триггер может быть выражен чисто в XAML — в коллекцию триггеров элемента управления добавляется элемент `Trigger`.
В этом примере показан триггер, который изменяет цвет фона элемента управления `Entry` при получении им фокуса:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
            <!-- multiple Setters elements are allowed -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Ниже приводятся важные части объявления триггера.

- **TargetType** — тип элемента управления, к которому применяется триггер.

- **Property** — отслеживаемое свойство элемента управления.

- **Value** — значение отслеживаемого свойства, при котором срабатывает триггер.

- **Setter** — возможность добавления коллекции элементов `Setter` при выполнении условия триггера. Необходимо указать `Property` и `Value`.

- **EnterActions и ExitActions** (не показаны) пишутся в коде и могут использоваться наряду с элементами `Setter` (или вместо них). Они [описаны ниже](#enteractions-and-exitactions).

### <a name="applying-a-trigger-using-a-style"></a>Применение триггера при использовании стиля

Триггеры также можно добавлять в объявление `Style` в элементе управления в `ResourceDictionary` на странице или в приложении. В этом примере объявляется неявный стиль (т. е. `Key` не задан). Это значит, что он будет применяться ко всем элементам управления `Entry` на странице.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                    <!-- multiple Setters elements are allowed -->
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

## <a name="data-triggers"></a>Триггеры данных

Триггеры данных используют привязку данных для отслеживания другого элемента управления в целях вызова `Setter`. Для отслеживания указанного значения задайте в триггере свойства атрибут `Binding` вместо атрибута `Property`.

В примере ниже используется синтаксис привязки данных `{Binding Source={x:Reference entry}, Path=Text.Length}`,
который показывает, как осуществляется ссылка на свойства другого элемента управления. Если длина `entry` равна нулю, сработает триггер. В этом примере триггер отключает кнопку, если входные данные отсутствуют.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
            <!-- multiple Setters elements are allowed -->
        </DataTrigger>
    </Button.Triggers>
</Button>
```

> [!TIP]
> При определении `Path=Text.Length` всегда следует указывать значение по умолчанию для целевого свойства (например, `Text=""`), так как в противном случае будет использоваться значение `null` и триггер не будет работать надлежащим образом.

Помимо указания `Setter`, можно задать коллекции [`EnterActions` и `ExitActions`](#enteractions-and-exitactions).

## <a name="event-triggers"></a>Триггеры событий

Элементу `EventTrigger` требуется только свойство `Event`, такое как `"Clicked"` в примере ниже.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Обратите внимание, что здесь нет элементов `Setter`, а есть ссылка на класс, определяемый элементом `local:NumericValidationTriggerAction`, для которого требуется объявить пространство имен `xmlns:local` в XAML страницы.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Сам класс реализует класс `TriggerAction`, то есть он должен предоставлять переопределение для метода `Invoke`, вызываемого при возникновении события триггера.

В ходе реализации действия триггера должны выполняться следующие задачи.

- Реализация универсального класса `TriggerAction<T>` с универсальным параметром, соответствующим типу элемента управления, к которому будет применяться триггер. Можно использовать суперклассы, например `VisualElement`, для записи действий триггера, которые работают с различными элементами управления, или указать тип элемента управления, например `Entry`.

- Переопределение метода `Invoke`, который вызывается при каждом выполнении условий триггера.

- Необязательное предоставление свойств, которые можно задать в XAML. Пример см. в описании `VisualElementPopTriggerAction` класса в прилагаемом примере приложения.

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Затем триггер события можно использовать из XAML:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Следует соблюдать осторожность при совместном использовании триггеров в классе `ResourceDictionary`: один экземпляр будет общим для элементов управления, поэтому любое однократно настроенное состояние будет применяться ко всем элементам.

Обратите внимание, что триггеры событий не поддерживают коллекции `EnterActions` и `ExitActions`, [описанные ниже](#enteractions-and-exitactions).

## <a name="multi-triggers"></a>Мультитриггеры

`MultiTrigger` аналогичен `Trigger` или `DataTrigger`, за исключением того, что для него может существовать несколько условий. `Setter` срабатывают при выполнении всех условий.

Ниже приведен пример триггера для кнопки, которая привязывается к двум различным записям (`email` и `phone`).

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>
    <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

Коллекция `Conditions` может также содержать элементы `PropertyCondition`, аналогичные приведенным ниже.

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Создание мультитриггера, требующего выполнения всех условий

Мультитриггер обновляет свой элемент управления только в том случае, если выполняются все условия. Тестировать условие "Все значения длины поля равны нулю" (например, для страницы входа, где должны быть указаны все входные данные) достаточно сложно, поскольку требуется условие "где Text.Length > 0", которое невозможно выразить в XAML.

Это можно сделать с помощью интерфейса `IValueConverter`. Приведенный ниже код преобразует привязку `Text.Length` в значение типа `bool`, указывающее, является поле пустым или нет.

```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

Для использования этого конвертера в мультитриггере сначала добавьте его в словарь ресурсов страницы (вместе с пользовательским определением пространства имен `xmlns:local`).

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML приведен ниже. Обратите внимание на указанные далее отличия от первого примера мультитриггера.

- Для кнопки задан параметр `IsEnabled="false"` по умолчанию.
- Условия мультитриггера используют преобразователь для преобразования значения `Text.Length` в тип `boolean`.
- Если все условия имеют значение `true`, метод задания устанавливает для свойства `IsEnabled` кнопки значение `true`.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

На этих снимках экрана показаны различия между двумя приведенными выше примерами мультитриггеров. В верхней части экранов достаточно ввести текст только в один элемент управления `Entry`, чтобы включить кнопку **Сохранить**.
В нижней части экранов кнопка **Вход** остается неактивной до тех пор, пока не будут заполнены оба поля.

![Примеры мультитриггеров](triggers-images/multi-requireall.png)

## <a name="enteractions-and-exitactions"></a>EnterActions и ExitActions

Другим способом реализации изменений при срабатывании триггера является добавление коллекций `EnterActions` и `ExitActions` и указание реализаций класса `TriggerAction<T>`.

Коллекция [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) служит для определения списка `IList` объектов [`TriggerAction`](xref:Xamarin.Forms.TriggerAction), которые будут вызываться при соблюдении условия триггера. Коллекция [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) служит для определения списка `IList` объектов `TriggerAction`, которые будут вызываться, когда условие триггера больше не соблюдается.

> [!NOTE]
> Объекты [`TriggerAction`](xref:Xamarin.Forms.TriggerAction), определенные в коллекциях `EnterActions` и `ExitActions`, игнорируются классом [`EventTrigger`](xref:Xamarin.Forms.EventTrigger).    

В триггер можно добавить *обе* коллекции `EnterActions` и `ExitActions`, а также `Setter`, но учтите, что `Setter` вызываются мгновенно (они не ожидают завершения `EnterAction` или `ExitAction`). Кроме того, можно выполнить все действия в коде и вообще не использовать `Setter`.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
            <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Как обычно, если класс указывается в XAML, необходимо объявить пространство имен, например `xmlns:local`, следующим образом.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Ниже приведен код для класса `FadeTriggerAction`.

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public int StartsFrom { set; get; }

    protected override void Invoke(VisualElement sender)
    {
        sender.Animate("FadeTriggerAction", new Animation((d) =>
        {
            var val = StartsFrom == 1 ? d : 1 - d;
            // so i was aiming for a different color, but then i liked the pink :)
            sender.BackgroundColor = Color.FromRgb(1, val, 1);
        }),
        length: 1000, // milliseconds
        easing: Easing.Linear);
    }
}
```

## <a name="state-triggers"></a>Триггеры состояния

Триггеры состояния — это специализированная группа триггеров, определяющих условия применения [`VisualState`](xref:Xamarin.Forms.VisualState).

Триггеры состояния добавляются в коллекцию [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) [`VisualState`](xref:Xamarin.Forms.VisualState). Эта коллекция может содержать один или несколько триггеров состояния. При наличии активных триггеров состояния в коллекции будет применяться [`VisualState`](xref:Xamarin.Forms.VisualState).

При использовании триггеров состояния для управления визуальными состояниями Xamarin.Forms применяет следующие правила приоритета, чтобы определить, какой триггер (и соответственно [`VisualState`](xref:Xamarin.Forms.VisualState)) будет активен:

1. Любой триггер, производный от [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase).
1. [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) активируется из-за выполнения условия [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth).
1. [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) активируется из-за выполнения условия [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight).

Если одновременно активны несколько триггеров (например, два пользовательских триггера), то у первого триггера, объявленного в разметке, будет приоритет.

> [!NOTE]
> Триггеры состояния можно задать в [`Style`](xref:Xamarin.Forms.Style) или напрямую в элементах.

Дополнительные сведения о визуальных состояниях см. в статье [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

### <a name="state-trigger"></a>Триггер состояния

Класс [`StateTrigger`](xref:Xamarin.Forms.StateTrigger), производный от класса [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase), имеет привязываемое свойство [`IsActive`](xref:Xamarin.Forms.StateTrigger.IsActive). `StateTrigger` выполняет изменение [`VisualState`](xref:Xamarin.Forms.VisualState), когда свойство `IsActive` изменяет значение.

Класс [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase), который является базовым классом для всех триггеров состояния, имеет свойство [`IsActive`](xref:Xamarin.Forms.StateTriggerBase.IsActive) и событие [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged). Это событие возникает при каждом изменении [`VisualState`](xref:Xamarin.Forms.VisualState). Кроме того, класс `StateTriggerBase` имеет переопределяемые методы `OnAttached` и `OnDetached`.

> [!IMPORTANT]
> Привязываемое свойство [`StateTrigger.IsActive`](xref:Xamarin.Forms.StateTrigger.IsActive) скрывает унаследованное свойство [`StateTriggerBase.IsActive`](xref:Xamarin.Forms.StateTriggerBase.IsActive).

В следующем примере XAML показан элемент [`Style`](xref:Xamarin.Forms.Style), предусматривающий объекты [`StateTrigger`](xref:Xamarin.Forms.StateTrigger):

```xaml
<Style TargetType="Grid">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Checked">
                    <VisualState.StateTriggers>
                        <StateTrigger IsActive="{Binding IsToggled}"
                                      IsActiveChanged="OnCheckedStateIsActiveChanged" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Black" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Unchecked">
                    <VisualState.StateTriggers>
                        <StateTrigger IsActive="{Binding IsToggled, Converter={StaticResource inverseBooleanConverter}}"
                                      IsActiveChanged="OnUncheckedStateIsActiveChanged" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="White" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

В этом примере неявный элемент [`Style`](xref:Xamarin.Forms.Style) направлен на объекты [`Grid`](xref:Xamarin.Forms.Grid). Если у свойства `IsToggled` привязанного объекта значение `true`, цвет фона `Grid` задается черным. Когда у свойства `IsToggled` привязанного объекта значение `false`, активируется изменение [`VisualState`](xref:Xamarin.Forms.VisualState), а цвет фона `Grid` становится белым.

Кроме того, каждый раз при изменении [`VisualState`](xref:Xamarin.Forms.VisualState) возникает событие [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) для `VisualState`. Каждый элемент `VisualState` регистрирует обработчик событий для этого события:

```csharp
void OnCheckedStateIsActiveChanged(object sender, EventArgs e)
{
    StateTriggerBase stateTrigger = sender as StateTriggerBase;
    Console.WriteLine($"Checked state active: {stateTrigger.IsActive}");
}

void OnUncheckedStateIsActiveChanged(object sender, EventArgs e)
{
    StateTriggerBase stateTrigger = sender as StateTriggerBase;
    Console.WriteLine($"Unchecked state active: {stateTrigger.IsActive}");
}
```

В этом примере при срабатывании обработчика для события [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) обработчик выводит сведения о том, активен ли [`VisualState`](xref:Xamarin.Forms.VisualState). Например, следующие сообщения выводятся в окно консоли при переходе от визуального состояния `Checked` к визуальному состоянию `Unchecked`:

```
Checked state active: False
Unchecked state active: True
```

> [!NOTE]
> Триггеры настраиваемого состояния могут быть созданы путем наследования от класса [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) и переопределения методов `OnAttached` и `OnDetached` для выполнения необходимых операций регистраций и очистки.

### <a name="adaptive-trigger"></a>Адаптивный триггер

[`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) активирует изменение [`VisualState`](xref:Xamarin.Forms.VisualState), когда окно имеет заданную высоту или ширину. Этот триггер имеет два привязываемых свойства:

- [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) типа `double`, которое указывает минимальную высоту окна, при которой следует применять [`VisualState`](xref:Xamarin.Forms.VisualState).
- [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) типа `double`, которое указывает минимальную ширину окна, при которой следует применять [`VisualState`](xref:Xamarin.Forms.VisualState).

> [!NOTE]
> [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) является производным от класса [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) и может присоединить обработчик событий к событию [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged).

В следующем примере XAML показан элемент [`Style`](xref:Xamarin.Forms.Style), предусматривающий объекты [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger):

```xaml
<Style TargetType="StackLayout">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Vertical">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="0" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="Orientation"
                                Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Horizontal">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="800" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="Orientation"
                                Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

В этом примере неявный элемент [`Style`](xref:Xamarin.Forms.Style) направлен на объекты [`StackLayout`](xref:Xamarin.Forms.StackLayout). Если ширина окна — 0–800 аппаратно-независимых единиц, у объектов `StackLayout`, к которым применяется `Style`, будет вертикальная ориентация. Если ширина окна больше или равна 800 аппаратно-независимым единицам, активируется изменение [`VisualState`](xref:Xamarin.Forms.VisualState), а ориентация `StackLayout` меняется на горизонтальную:

![Вертикальная ориентация StackLayout VisualState](triggers-images/adaptivetrigger-vertical.png "Пример AdaptiveTrigger")
![Горизонтальная ориентация StackLayout VisualState](triggers-images/adaptivetrigger-horizontal.png "Пример AdaptiveTrigger")

Свойства [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) и [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) можно использовать независимо друг от друга или совместно. Ниже представлен простой пример XAML для установки обоих свойств:

```xaml
<AdaptiveTrigger MinWindowWidth="800"
                 MinWindowHeight="1200"/>
```

В этом примере [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) указывает, что соответствующий элемент [`VisualState`](xref:Xamarin.Forms.VisualState) будет применен, если текущая ширина окна больше или равна 800 аппаратно-независимым единицам, а текущая высота окна больше или равна 1200 аппаратно-независимым единицам.

### <a name="compare-state-trigger"></a>Сравнение триггера состояния

[`CompareStateTrigger`](xref:Xamarin.Forms.CompareStateTrigger) активирует изменение [`VisualState`](xref:Xamarin.Forms.VisualState), если свойство равно определенному значению. Этот триггер имеет два привязываемых свойства:

- [`Property`](xref:Xamarin.Forms.CompareStateTrigger.Property) типа `object` указывает свойство, сравниваемое триггером.
- [`Value`](xref:Xamarin.Forms.CompareStateTrigger.Value) типа `object` указывает значение, при котором следует применить [`VisualState`](xref:Xamarin.Forms.VisualState).

> [!NOTE]
> [`CompareStateTrigger`](xref:Xamarin.Forms.CompareStateTrigger) является производным от класса [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) и может присоединить обработчик событий к событию [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged).

В следующем примере XAML показан элемент [`Style`](xref:Xamarin.Forms.Style), предусматривающий объекты [`CompareStateTrigger`](xref:Xamarin.Forms.CompareStateTrigger):

```xaml
<Style TargetType="Grid">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Checked">
                    <VisualState.StateTriggers>
                        <CompareStateTrigger Property="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                                             Value="True" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Black" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Unchecked">
                    <VisualState.StateTriggers>
                        <CompareStateTrigger Property="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                                             Value="False" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="White" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
...
<Grid>
    <Frame BackgroundColor="White"
           CornerRadius="12"
           Margin="24"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <StackLayout Orientation="Horizontal">
            <CheckBox x:Name="checkBox"
                      VerticalOptions="Center" />
            <Label Text="Check the CheckBox to modify the Grid background color."
                   VerticalOptions="Center" />
        </StackLayout>
    </Frame>
</Grid>
```

В этом примере неявный элемент [`Style`](xref:Xamarin.Forms.Style) направлен на объекты [`Grid`](xref:Xamarin.Forms.Grid). Если у свойства [`IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) [`CheckBox`](xref:Xamarin.Forms.CheckBox) значение `false`, устанавливается белый цвет фона `Grid`. Когда у свойства `CheckBox.IsChecked` значение `true`, активируется изменение [`VisualState`](xref:Xamarin.Forms.VisualState), а цвет фона `Grid` становится черным:

[![Снимок экрана с активированным изменением состояния визуализации в iOS и Android с невыбранным триггером.](triggers-images/comparestatetrigger-unchecked.png "Пример CompareStateTrigger")](triggers-images/comparestatetrigger-unchecked-large.png#lightbox "Пример CompareStateTrigger")
[![Снимок экрана с активированным изменением состояния визуализации в iOS и Android с выбранным триггером.](triggers-images/comparestatetrigger-checked.png "Пример CompareStateTrigger")](triggers-images/comparestatetrigger-checked-large.png#lightbox "Пример CompareStateTrigger")

### <a name="device-state-trigger"></a>Триггер состояния устройства

[`DeviceStateTrigger`](xref:Xamarin.Forms.DeviceStateTrigger) активирует изменение [`VisualState`](xref:Xamarin.Forms.VisualState) в зависимости от платформы устройства, на которой работает приложение. У этого триггера одно привязываемое свойство:

- [`Device`](xref:Xamarin.Forms.DeviceStateTrigger.Device) типа `string` указывает платформу устройства, на которой следует применить [`VisualState`](xref:Xamarin.Forms.VisualState).

> [!NOTE]
> [`DeviceStateTrigger`](xref:Xamarin.Forms.DeviceStateTrigger) является производным от класса [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) и может присоединить обработчик событий к событию [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged).

В следующем примере XAML показан элемент [`Style`](xref:Xamarin.Forms.Style), предусматривающий объекты `DeviceStateTrigger`:

```xaml
<Style x:Key="DeviceStateTriggerPageStyle"
       TargetType="ContentPage">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="iOS">
                    <VisualState.StateTriggers>
                        <DeviceStateTrigger Device="iOS" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Silver" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Android">
                    <VisualState.StateTriggers>
                        <DeviceStateTrigger Device="Android" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="#2196F3" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="UWP">
                    <VisualState.StateTriggers>
                        <DeviceStateTrigger Device="UWP" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Aquamarine" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

В этом примере явный элемент [`Style`](xref:Xamarin.Forms.Style) направлен на объекты [`ContentPage`](xref:Xamarin.Forms.ContentPage). Объекты `ContentPage`, использующие этот стиль, устанавливают серебристый цвет фона в iOS, бледно-голубой — в Android и аквамариновый — на универсальной платформе Windows. На следующих снимках экрана показаны полученные страницы в iOS и Android:

[![Снимок экрана с активированным изменением визуального состояния в iOS и Android](triggers-images/devicestatetrigger.png "Пример DeviceStateTrigger")](triggers-images/devicestatetrigger-large.png#lightbox "Пример DeviceStateTrigger")

### <a name="orientation-state-trigger"></a>Триггер состояния ориентации

[`OrientationStateTrigger`](xref:Xamarin.Forms.OrientationStateTrigger) активирует изменение [`VisualState`](xref:Xamarin.Forms.VisualState) при изменении ориентации устройства. У этого триггера одно привязываемое свойство:

- [`Orientation`](xref:Xamarin.Forms.OrientationStateTrigger.Orientation) типа [`DeviceOrientation`](xref:Xamarin.Forms.Internals.DeviceOrientation) указывает ориентацию, к которой следует применять [`VisualState`](xref:Xamarin.Forms.VisualState).

> [!NOTE]
> [`OrientationStateTrigger`](xref:Xamarin.Forms.OrientationStateTrigger) является производным от класса [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) и может присоединить обработчик событий к событию [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged).

В следующем примере XAML показан элемент [`Style`](xref:Xamarin.Forms.Style), предусматривающий объекты `OrientationStateTrigger`:

```xaml
<Style x:Key="OrientationStateTriggerPageStyle"
       TargetType="ContentPage">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Portrait">
                    <VisualState.StateTriggers>
                        <OrientationStateTrigger Orientation="Portrait" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Silver" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Landscape">
                    <VisualState.StateTriggers>
                        <OrientationStateTrigger Orientation="Landscape" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="White" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

В этом примере явный элемент [`Style`](xref:Xamarin.Forms.Style) направлен на объекты [`ContentPage`](xref:Xamarin.Forms.ContentPage). Объекты `ContentPage`, использующие этот стиль, устанавливают серебристый цвет фона, если ориентация книжная, и белый — если ориентация альбомная.

## <a name="related-links"></a>Связанные ссылки

- [Пример триггеров](/samples/xamarin/xamarin-forms-samples/workingwithtriggers)
- [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
- [API триггера Xamarin.Forms](xref:Xamarin.Forms.TriggerAction`1)