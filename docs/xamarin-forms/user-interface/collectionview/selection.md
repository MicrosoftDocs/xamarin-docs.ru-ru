---
title: Xamarin.Forms CollectionView выбор
description: По умолчанию выбор CollectionView отключен. Однако можно включить один и несколько элементов.
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c432c1981ba057f61bba780b7997c8b78f8cef3f
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91557352"
---
# <a name="no-locxamarinforms-collectionview-selection"></a>Xamarin.Forms CollectionView выбор

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет следующие свойства, управляющие выбором элемента:

- [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), типа [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) , режим выбора.
- [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)Тип `object` , выбранный элемент в списке. Это свойство имеет режим привязки по умолчанию `TwoWay` и имеет значение, `null` Если ни один элемент не выбран.
- [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems), типа `IList<object>` , выбранные элементы в списке. Это свойство имеет режим привязки по умолчанию `OneWay` и имеет значение, `null` Если ни один элемент не выбран.
- [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand)Тип `ICommand` , который выполняется при изменении выбранного элемента.
- [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter)Тип `object` , который является параметром, который передается в `SelectionChangedCommand` .

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) выделение отключено. Однако это поведение можно изменить, задав [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) для свойства значение одного из [`SelectionMode`](xref:Xamarin.Forms.SelectionMode) членов перечисления:

- `None` — Указывает, что элементы не могут быть выбраны. Это значение по умолчанию.
- `Single` — Указывает, что можно выбрать один элемент, выделив выделенный элемент.
- `Multiple` — Указывает, что можно выбрать несколько элементов с выделенными элементами.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) Определяет [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) событие, которое возникает при [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) изменении свойства либо из-за выбора пользователем элемента из списка, либо когда приложение задает свойство. Кроме того, это событие возникает при [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) изменении свойства. [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs)Объект, сопровождающий `SelectionChanged` событие, имеет два свойства типа `IReadOnlyList<object>` :

- `PreviousSelection` — список элементов, которые были выбраны до изменения выбора.
- `CurrentSelection` — список элементов, выбранных после изменения выбора.

Кроме того, [`CollectionView`](xref:Xamarin.Forms.CollectionView) содержит `UpdateSelectedItems` метод, который обновляет [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) свойство списком выбранных элементов, в то время как обрабатывалось только одно уведомление об изменении.

## <a name="single-selection"></a>Выбор одного элемента

Если [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойство имеет значение `Single` , в поле [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно выбрать один элемент. Если выбран элемент, [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) свойству будет присвоено значение выбранного элемента. При изменении этого свойства [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) выполняется (со значением, [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) передаваемым в `ICommand` ), и [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) вызывается событие.

В следующем примере XAML показано [`CollectionView`](xref:Xamarin.Forms.CollectionView) , что может отвечать на выбор одного элемента:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

В этом примере кода `OnCollectionViewSelectionChanged` обработчик событий выполняется при [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) срабатывании события, когда обработчик событий получает выбранный ранее элемент и текущий выбранный элемент:

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (e.PreviousSelection.FirstOrDefault() as Monkey)?.Name;
    string current = (e.CurrentSelection.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)Событие может быть вызвано изменениями, происходящими в результате изменения [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Свойства.

На следующих снимках экрана показан выбор одного элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) :

[![Снимок экрана с одним выделенным вертикальным списком CollectionView в iOS и Android](selection-images/single-selection.png "Вертикальный список CollectionView с одним выделением")](selection-images/single-selection-large.png#lightbox "Вертикальный список CollectionView с одним выделением")

## <a name="multiple-selection"></a>Выбор нескольких элементов

Если [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойство имеет значение `Multiple` , можно выбрать несколько элементов в поле [`CollectionView`](xref:Xamarin.Forms.CollectionView) . При выборе элементов [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) свойству будут присвоены выбранные элементы. При изменении этого свойства [`SelectionChangedCommand`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommand) выполняется (со значением, [`SelectionChangedCommandParameter`](xref:Xamarin.Forms.SelectableItemsView.SelectionChangedCommandParameter) передаваемым в `ICommand` ), и [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) вызывается событие.

В следующем примере XAML показано [`CollectionView`](xref:Xamarin.Forms.CollectionView) , что может отвечать на выбор нескольких элементов:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

В этом примере кода `OnCollectionViewSelectionChanged` обработчик событий выполняется при [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) срабатывании события, когда обработчик событий извлекает ранее выбранные элементы и выделенные элементы.

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var previous = e.PreviousSelection;
    var current = e.CurrentSelection;
    ...
}
```

> [!IMPORTANT]
> [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged)Событие может быть вызвано изменениями, происходящими в результате изменения [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) Свойства.

На следующих снимках экрана показано выделение нескольких элементов в [`CollectionView`](xref:Xamarin.Forms.CollectionView) :

[![Снимок экрана: вертикальный список CollectionView с множественным выделением в iOS и Android](selection-images/multiple-selection.png "Вертикальный список CollectionView с множественным выделением")](selection-images/multiple-selection-large.png#lightbox "Вертикальный список CollectionView с множественным выделением")

## <a name="single-pre-selection"></a>Один предварительный выбор

Если [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойство имеет значение `Single` , один элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно предварительно выбрать, установив [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) для свойства элемент. В следующем примере кода XAML показано `CollectionView` , что предварительно выбирает один элемент:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey}">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey");
```

> [!NOTE]
> [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)Свойство имеет режим привязки по умолчанию `TwoWay` .

[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)Данные свойства привязываются к `SelectedMonkey` свойству подключенной модели представления, имеющей тип `Monkey` . По умолчанию `TwoWay` используется привязка, так что если пользователь изменяет выбранный элемент, значение `SelectedMonkey` свойства будет установлено для выбранного `Monkey` объекта. `SelectedMonkey`Свойство определено в `MonkeysViewModel` классе и устанавливается в четвертый элемент `Monkeys` коллекции:

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    Monkey selectedMonkey;
    public Monkey SelectedMonkey
    {
        get
        {
            return selectedMonkey;
        }
        set
        {
            if (selectedMonkey != value)
            {
                selectedMonkey = value;
            }
        }
    }

    public MonkeysViewModel()
    {
        ...
        selectedMonkey = Monkeys.Skip(3).FirstOrDefault();
    }
    ...
}
```

Таким образом, когда [`CollectionView`](xref:Xamarin.Forms.CollectionView) появится четвертый элемент в списке, он будет предварительно выбран:

[![Снимок экрана CollectionViewого вертикального списка с одним предварительным выбором, в iOS и Android](selection-images/single-pre-selection.png "Вертикальный список CollectionView с одним предварительным выбором")](selection-images/single-pre-selection-large.png#lightbox "Вертикальный список CollectionView с одним предварительным выбором")

## <a name="multiple-pre-selection"></a>Множественный предварительный выбор

Если [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойство имеет значение `Multiple` , можно заранее выбрать несколько элементов в поле [`CollectionView`](xref:Xamarin.Forms.CollectionView) . В следующем примере XAML показано `CollectionView` , в котором будет включен предварительный выбор нескольких элементов:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}"
                SelectionMode="Multiple"
                SelectedItems="{Binding SelectedMonkeys}">
    ...
</CollectionView>
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Multiple
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemsProperty, "SelectedMonkeys");
```

> [!NOTE]
> [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)Свойство имеет режим привязки по умолчанию `OneWay` .

[`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems)Данные свойства привязываются к `SelectedMonkeys` свойству подключенной модели представления, имеющей тип `ObservableCollection<object>` . `SelectedMonkeys`Свойство определено в `MonkeysViewModel` классе и устанавливается для второго, четвертого и пятого элементов в `Monkeys` коллекции:

```csharp
namespace CollectionViewDemos.ViewModels
{
    public class MonkeysViewModel : INotifyPropertyChanged
    {
        ...
        ObservableCollection<object> selectedMonkeys;
        public ObservableCollection<object> SelectedMonkeys
        {
            get
            {
                return selectedMonkeys;
            }
            set
            {
                if (selectedMonkeys != value)
                {
                    selectedMonkeys = value;
                }
            }
        }

        public MonkeysViewModel()
        {
            ...
            SelectedMonkeys = new ObservableCollection<object>()
            {
                Monkeys[1], Monkeys[3], Monkeys[4]
            };
        }
        ...
    }
}
```

Таким образом, при [`CollectionView`](xref:Xamarin.Forms.CollectionView) появлении второго, четвертого и пятого элементов в списке будут предварительно выбраны:

[![Снимок экрана: вертикальный список CollectionView с несколькими предварительными выбранными элементами в iOS и Android](selection-images/multiple-pre-selection.png "Вертикальный список CollectionView с несколькими предварительными выбранными элементами")](selection-images/multiple-pre-selection-large.png#lightbox "Вертикальный список CollectionView с несколькими предварительными выбранными элементами")

## <a name="clear-selections"></a>Очистить выделенные элементы

[`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem)Свойства и [`SelectedItems`](xref:Xamarin.Forms.SelectableItemsView.SelectedItems) можно очистить, установив их или объекты, к которым они привязаны, к `null` .

## <a name="change-selected-item-color"></a>Изменить цвет выбранного элемента

[`CollectionView`](xref:Xamarin.Forms.CollectionView) имеет объект `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения выбранного элемента в `CollectionView` . Распространенным вариантом использования этого варианта `VisualState` является изменение цвета фона выбранного элемента, который показан в следующем примере XAML:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="Grid">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal" />
                        <VisualState x:Name="Selected">
                            <VisualState.Setters>
                                <Setter Property="BackgroundColor"
                                        Value="LightSkyBlue" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}"
                        SelectionMode="Single">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        ...
                    </Grid>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

> [!IMPORTANT]
> Объект [`Style`](xref:Xamarin.Forms.Style) , содержащий объект, `Selected` `VisualState` должен иметь [`TargetType`](xref:Xamarin.Forms.Style.TargetType) значение свойства, которое является типом корневого элемента объекта [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , который задается как `ItemTemplate` значение свойства.

В этом примере [`Style.TargetType`](xref:Xamarin.Forms.Style.TargetType) свойству присваивается значение, `Grid` поскольку корневым элементом объекта [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) является [`Grid`](xref:Xamarin.Forms.Grid) . `Selected` [`VisualState`](xref:Xamarin.Forms.VisualState) Указывает, что при выборе элемента в параметре для [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) элемента будет установлено значение `LightSkyBlue` :

[![Снимок экрана CollectionViewого вертикального списка с пользовательским цветом одного выделения в iOS и Android](selection-images/single-selection-color.png "Вертикальный список CollectionView с пользовательским одним цветом выделения")](selection-images/single-selection-color-large.png#lightbox "Вертикальный список CollectionView с пользовательским одним цветом выделения")

Дополнительные сведения о визуальных состояниях см. в статье [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-selection"></a>Отключить выделение

[`CollectionView`](xref:Xamarin.Forms.CollectionView) по умолчанию выделение отключено. Однако если `CollectionView` включен выбор, его можно отключить, задав [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) для свойства значение `None` :

```xaml
<CollectionView ...
                SelectionMode="None" />
```

Эквивалентный код на C# выглядит так:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

Если [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) свойство имеет значение `None` , элементы в [`CollectionView`](xref:Xamarin.Forms.CollectionView) не могут быть выбраны, [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) свойство останется `null` , а [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) событие не будет запущено.

> [!NOTE]
> При выборе элемента и [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode) изменении свойства с `Single` на `None` [`SelectedItem`](xref:Xamarin.Forms.SelectableItemsView.SelectedItem) свойство будет установлено на, `null` а [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged) событие будет запущено с пустым `CurrentSelection` свойством.

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)