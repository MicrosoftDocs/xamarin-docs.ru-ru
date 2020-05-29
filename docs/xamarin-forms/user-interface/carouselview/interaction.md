---
title: Xamarin.FormsВзаимодействие Карауселвиев
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
ms.openlocfilehash: 57c501c0f789ce448d8381cbbccb46666cf06305
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137414"
---
# <a name="xamarinforms-carouselview-interaction"></a>Xamarin.FormsВзаимодействие Карауселвиев

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)определяет следующие свойства, управляющие взаимодействием с пользователем:

- `CurrentItem`Тип `object` , текущий отображаемый элемент. Это свойство имеет режим привязки по умолчанию `TwoWay` и имеет значение, `null` Если нет данных для вывода.
- `CurrentItemChangedCommand`Тип `ICommand` , который выполняется при изменении текущего элемента.
- `CurrentItemChangedCommandParameter` с типом `object`, который передается как параметр в `CurrentItemChangedCommand`.
- `IsBounceEnabled`Тип `bool` , который указывает, `CarouselView` будет ли передается значение на границе содержимого. Значение по умолчанию — `true`.
- `IsSwipeEnabled`Тип `bool` , который определяет, будет ли жест прокрутки изменять отображаемый элемент. Значение по умолчанию — `true`.
- `Position`Тип `int` — индекс текущего элемента в базовой коллекции. Это свойство имеет режим привязки по умолчанию `TwoWay` и имеет значение 0, если нет данных для вывода.
- `PositionChangedCommand`Тип `ICommand` , который выполняется при изменении расположения.
- `PositionChangedCommandParameter` с типом `object`, который передается как параметр в `PositionChangedCommand`.
- `VisibleViews`Тип `ObservableCollection<View>` , который является свойством только для чтения и содержит объекты для элементов, видимых в данный момент.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)Определяет `CurrentItemChanged` событие, которое возникает при `CurrentItem` изменении свойства либо из-за прокрутки пользователем, либо когда приложение задает свойство. `CurrentItemChangedEventArgs`Объект, сопровождающий `CurrentItemChanged` событие, имеет два свойства типа `object` :

- `PreviousItem`— Предыдущий элемент после изменения свойства.
- `CurrentItem`— текущий элемент после изменения свойства.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)также определяет `PositionChanged` событие, которое возникает при `Position` изменении свойства либо из-за прокрутки пользователем, либо когда приложение задает свойство. `PositionChangedEventArgs`Объект, сопровождающий `PositionChanged` событие, имеет два свойства типа `int` :

- `PreviousPosition`— предыдущее расположение после изменения свойства.
- `CurrentPosition`— Текущая заданная позицией после изменения свойства.

## <a name="respond-to-the-current-item-changing"></a>Ответ на изменение текущего элемента

При изменении отображаемого в данный момент элемента `CurrentItem` свойству будет присвоено значение элемента. При изменении этого свойства `CurrentItemChangedCommand` выполняется со значением, `CurrentItemChangedCommandParameter` передаваемым в `ICommand` . `Position`Затем свойство обновляется и `CurrentItemChanged` вызывается событие.

> [!IMPORTANT]
> `Position`Свойство изменяется при `CurrentItem` изменении свойства. Это приведет к `PositionChangedCommand` выполнению и `PositionChanged` срабатыванию события.

### <a name="event"></a>Событие

В следующем примере XAML показан объект [`CarouselView`](xref:Xamarin.Forms.CarouselView) , использующий обработчик событий для реагирования на текущий изменяемый элемент:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChanged="OnCurrentItemChanged">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.CurrentItemChanged += OnCurrentItemChanged;
```

В этом примере `OnCurrentItemChanged` обработчик событий выполняется при `CurrentItemChanged` срабатывании события:

```csharp
void OnCurrentItemChanged(object sender, CurrentItemChangedEventArgs e)
{
    Monkey previousItem = e.PreviousItem as Monkey;
    Monkey currentItem = e.CurrentItem as Monkey;
}
```

В этом примере `OnCurrentItemChanged` обработчик событий предоставляет доступ к предыдущим и текущим элементам:

[![Снимок экрана Карауселвиев с предыдущими и текущими элементами в iOS и Android](interaction-images/current-item-events.png "Карауселвиев с текущими и предыдущими элементами")](interaction-images/current-item-events-large.png#lightbox "Карауселвиев с текущими и предыдущими элементами")

### <a name="command"></a>Get-Help

В следующем примере XAML показан объект [`CarouselView`](xref:Xamarin.Forms.CarouselView) , использующий команду для реагирования на текущий изменяемый элемент:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItemChangedCommand="{Binding ItemChangedCommand}"
              CurrentItemChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=CurrentItem}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandProperty, "ItemChangedCommand");
carouselView.SetBinding(CarouselView.CurrentItemChangedCommandParameterProperty, new Binding("CurrentItem", source: RelativeBindingSource.Self));
```

В этом примере `CurrentItemChangedCommand` свойство привязывается к `ItemChangedCommand` свойству, передавая `CurrentItem` ему значение свойства в качестве аргумента. `ItemChangedCommand`Затем может реагировать на изменение текущего элемента по мере необходимости:

```csharp
public ICommand ItemChangedCommand => new Command<Monkey>((item) =>
{
    PreviousMonkey = CurrentMonkey;
    CurrentMonkey = item;
});
```

В этом примере `ItemChangedCommand` объекты Updates хранят предыдущие и текущие элементы.

## <a name="respond-to-the-position-changing"></a>Реагирование на изменение расположения

Когда отображаемый в данный момент элемент изменяется, `Position` для свойства будет задан индекс текущего элемента в базовой коллекции. При изменении этого свойства `PositionChangedCommand` выполняется со значением, `PositionChangedCommandParameter` передаваемым в `ICommand` . `PositionChanged`Затем вызывается событие. Если `Position` свойство было изменено программно, [`CarouselView`](xref:Xamarin.Forms.CarouselView) будет выполнена прокрутка до элемента, соответствующего `Position` значению.

> [!NOTE]
> Установка значения `Position` 0 для свойства приведет к отображению первого элемента в базовой коллекции.

### <a name="event"></a>Событие

В следующем примере XAML показан объект [`CarouselView`](xref:Xamarin.Forms.CarouselView) , использующий обработчик событий для реагирования на `Position` изменение свойства:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"              
              PositionChanged="OnPositionChanged">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.PositionChanged += OnPositionChanged;
```

В этом примере `OnPositionChanged` обработчик событий выполняется при `PositionChanged` срабатывании события:

```csharp
void OnPositionChanged(object sender, PositionChangedEventArgs e)
{
    int previousItemPosition = e.PreviousPosition;
    int currentItemPosition = e.CurrentPosition;
}
```

В этом примере `OnCurrentItemChanged` обработчик событий предоставляет предыдущую и текущую позиции:

[![Снимок экрана Карауселвиев с предыдущими и текущими позициями в iOS и Android](interaction-images/current-position-events.png "Карауселвиев с текущими и предыдущими позициями")](interaction-images/current-position-events-large.png#lightbox "Карауселвиев с текущими и предыдущими позициями")

### <a name="command"></a>Get-Help

В следующем примере XAML показано [`CarouselView`](xref:Xamarin.Forms.CarouselView) , в котором используется команда для реагирования на `Position` изменение свойства:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PositionChangedCommand="{Binding PositionChangedCommand}"
              PositionChangedCommandParameter="{Binding Source={RelativeSource Self}, Path=Position}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionChangedCommandProperty, "PositionChangedCommand");
carouselView.SetBinding(CarouselView.PositionChangedCommandParameterProperty, new Binding("Position", source: RelativeBindingSource.Self));
```

В этом примере `PositionChangedCommand` свойство привязывается к `PositionChangedCommand` свойству, передавая `Position` ему значение свойства в качестве аргумента. `PositionChangedCommand`Затем объект может ответить на изменение расположения по мере необходимости:

```csharp
public ICommand PositionChangedCommand => new Command<int>((position) =>
{
    PreviousPosition = CurrentPosition;
    CurrentPosition = position;
});
```

В этом примере `PositionChangedCommand` объекты Updates сохраняют предыдущую и текущую позиции.

## <a name="preset-the-current-item"></a>Предустановка текущего элемента

Текущий элемент в [`CarouselView`](xref:Xamarin.Forms.CarouselView) может быть задан программным путем задания `CurrentItem` свойства для элемента. В следующем примере XAML показано `CarouselView` , что предварительно выбирает текущий элемент:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              CurrentItem="{Binding CurrentItem}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.CurrentItemProperty, "CurrentItem");
```

> [!NOTE]
> `CurrentItem`Свойство имеет режим привязки по умолчанию `TwoWay` .

`CarouselView.CurrentItem`Данные свойства привязываются к `CurrentItem` свойству подключенной модели представления, имеющей тип `Monkey` . По умолчанию `TwoWay` используется привязка, так что если пользователь изменяет текущий элемент, значение `CurrentItem` свойства будет установлено в текущий `Monkey` объект. `CurrentItem`Свойство определено в `MonkeysViewModel` классе:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    public Monkey CurrentItem { get; set; }

    public MonkeysViewModel()
    {
        // ...
        CurrentItem = Monkeys.Skip(3).FirstOrDefault();
        OnPropertyChanged("CurrentItem");
    }
}
```

В этом примере `CurrentItem` свойство задано для четвертого элемента в `Monkeys` коллекции:

[![Снимок экрана Карауселвиев с элементом предустановки в iOS и Android](interaction-images/preset-item.png "Карауселвиев с предварительно заданным элементом")](interaction-images/preset-item-large.png#lightbox "Карауселвиев с предварительно заданным элементом")

## <a name="preset-the-position"></a>Предустановка расположения

Отображаемый элемент [`CarouselView`](xref:Xamarin.Forms.CarouselView) может быть задан программным путем присвоения `Position` свойству индекса элемента в базовой коллекции. В следующем примере XAML показан объект `CarouselView` , который задает отображаемый элемент:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              Position="{Binding Position}">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
carouselView.SetBinding(CarouselView.PositionProperty, "Position");
```

> [!NOTE]
> `Position`Свойство имеет режим привязки по умолчанию `TwoWay` .

`CarouselView.Position`Данные свойства привязываются к `Position` свойству подключенной модели представления, имеющей тип `int` . По умолчанию `TwoWay` используется привязка, так что если пользователь выполняет прокрутку по [`CarouselView`](xref:Xamarin.Forms.CarouselView) , то значение `Position` свойства будет равно индексу отображаемого элемента. `Position`Свойство определено в `MonkeysViewModel` классе:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    // ...
    public int Position { get; set; }

    public MonkeysViewModel()
    {
        // ...
        Position = 3;
        OnPropertyChanged("Position");
    }
}
```

В этом примере `Position` свойство задано для четвертого элемента в `Monkeys` коллекции:

[![Снимок экрана Карауселвиев с заданной позицией в iOS и Android](interaction-images/preset-position.png "Карауселвиев с заданной позицией")](interaction-images/preset-position-large.png#lightbox "Карауселвиев с заданной позицией")

## <a name="define-visual-states"></a>Определение визуальных состояний

[`CarouselView`](xref:Xamarin.Forms.CarouselView)определяет четыре визуальных состояния:

- `CurrentItem`представляет визуальное состояние для отображаемого в данный момент элемента.
- `PreviousItem`представляет визуальное состояние для ранее отображаемого элемента.
- `NextItem`представляет визуальное состояние для следующего элемента.
- `DefaultItem`представляет визуальное состояние для оставшейся части элементов.

Эти визуальные состояния можно использовать для инициации визуальных изменений элементов, отображаемых в [`CarouselView`](xref:Xamarin.Forms.CarouselView) .

В следующем примере XAML показано, как определить `CurrentItem` `PreviousItem` `NextItem` `DefaultItem` визуальные состояния,, и.

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="CurrentItem">
                            <VisualState.Setters>
                                <Setter Property="Scale"
                                        Value="1.1" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="PreviousItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="NextItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="DefaultItem">
                            <VisualState.Setters>
                                <Setter Property="Opacity"
                                        Value="0.25" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <!-- Item template content -->
                <Frame HasShadow="true">
                    ...
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

В этом примере `CurrentItem` визуальное состояние указывает, что для текущего элемента, отображаемого [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойством, будет изменено значение по умолчанию от 1 до 1,1. `PreviousItem`Состояние и `NextItem` визуальные состояния указывают, что элементы, окружающие текущий элемент, будут отображаться со [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) значением 0,5. `DefaultItem`Визуальное состояние указывает, что оставшаяся часть элементов, отображаемых, `CarouselView` будет отображаться со `Opacity` значением 0,25.

> [!NOTE]
> Кроме того, визуальные состояния могут быть определены в [`Style`](xref:Xamarin.Forms.Style) , имеющем [`TargetType`](xref:Xamarin.Forms.Style.TargetType) значение свойства, которое является типом корневого элемента объекта [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , который задается как `ItemTemplate` значение свойства.

На следующих снимках экрана `CurrentItem` показаны `PreviousItem` состояния отображения, и `NextItem` .

[![Снимок экрана Карауселвиев с использованием визуальных состояний в iOS и Android](interaction-images/visual-states.png "Визуальные состояния Карауселвиев")](interaction-images/visual-states-large.png#lightbox "Визуальные состояния Карауселвиев")

Дополнительные сведения о визуальных состояниях см. в разделе [ Xamarin.Forms Диспетчер визуальных состояний](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="clear-the-current-item"></a>Очистить текущий элемент

`CurrentItem`Свойство можно очистить, задавая его или объект, к которому он привязан, к `null` .

## <a name="disable-bounce"></a>Отключить Bounce

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) посылает элементы на границах содержимого. Это можно отключить, задав `IsBounceEnabled` для свойства значение `false` .

## <a name="disable-swipe-interaction"></a>Отключить взаимодействие при прокрутке

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) разрешает пользователям перемещаться по элементам с помощью жеста прокрутки. Это взаимодействие по прокрутке можно отключить, задав `IsSwipeEnabled` для свойства значение `false` .

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.FormsДиспетчер визуальных состояний](~/xamarin-forms/user-interface/visual-state-manager.md)
