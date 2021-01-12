---
title: Xamarin.Forms свипевиев
description: Xamarin.FormsСвипевиев — это контейнерный элемент управления, который служит оболочкой для элемента содержимого и предоставляет элементы контекстного меню, которые выводятся с помощью жеста прокрутки.
ms.prod: xamarin
ms.assetId: 602456B5-701B-4948-B454-B1F31283F1CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d0ebae93405cb115a0f1e87453ab9b438202ef30
ms.sourcegitcommit: 1decf2c65dc4c36513f7dd459a5df01e170a036f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98115253"
---
# <a name="no-locxamarinforms-swipeview"></a>Xamarin.Forms свипевиев

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)

`SwipeView`— Это контейнерный элемент управления, который служит оболочкой для элемента содержимого и предоставляет пункты контекстного меню, которые выводятся с помощью жеста прокрутки:

[![Снимок экрана Свипевиев: считывание элементов в CollectionView на iOS и Android](swipeview-images/swipeview-collectionview.png "Свипевиев считывание элементов")](swipeview-images/swipeview-collectionview-large.png#lightbox "Свипевиев считывание элементов")

`SwipeView` определяет следующие свойства:

- `LeftItems`Тип `SwipeItems` , который представляет элементы считывания, которые могут быть вызваны при считывании элемента управления с левой стороны.
- `RightItems`Тип `SwipeItems` , который представляет элементы считывания, которые могут быть вызваны при считывании элемента управления с правой стороны.
- `TopItems`Тип `SwipeItems` , который представляет элементы считывания, которые могут быть вызваны при прокрутке элемента управления сверху вниз.
- `BottomItems`Тип `SwipeItems` , который представляет элементы считывания, которые могут быть вызваны при прокрутке элемента управления снизу вверх.
- `Threshold`Тип `double` , который представляет число независимых от устройства единиц, которые активируют жест прокрутки для полного отображения элементов считывания.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

Кроме того, `SwipeView` компонент наследует [`Content`](xref:Xamarin.Forms.ContentView.Content) свойство от [`ContentView`](xref:Xamarin.Forms.ContentView) класса. `Content`Свойство является свойством Content `SwipeView` класса, поэтому его не нужно задавать явно.

`SwipeView`Класс также определяет три события:

- `SwipeStarted` активируется при начале считывания. `SwipeStartedEventArgs`Объект, сопровождающий это событие, имеет `SwipeDirection` свойство типа `SwipeDirection` .
- `SwipeChanging` происходит при перемещении прокрутки. `SwipeChangingEventArgs`Объект, сопровождающий это событие, имеет `SwipeDirection` свойство типа `SwipeDirection` и `Offset` свойство типа `double` .
- `SwipeEnded` возникает, когда прокрутка заканчивается. `SwipeEndedEventArgs`Объект, сопровождающий это событие, имеет `SwipeDirection` свойство типа `SwipeDirection` и `IsOpen` свойство типа `bool` .

Кроме того, `SwipeView` включает `Open` `Close` методы и, которые программно открывают и закрывают элементы для прокрутки соответственно.

> [!NOTE]
> `SwipeView` имеет зависящую от платформы платформу iOS и Android, которая управляет переходом, используемым при открытии `SwipeView` . Дополнительные сведения см. в разделе [Свипевиев считывание режима переходов в iOS](~/xamarin-forms/platform/ios/swipeview-swipetransitionmode.md) и [Свипевиев, проведите переход на Android](~/xamarin-forms/platform/android/swipeview-swipetransitionmode.md).

## <a name="create-a-swipeview"></a>Создание Свипевиев

Объект `SwipeView` должен определять содержимое, вокруг которого `SwipeView` выполняется оболочка, и прокрутки элементов, которые выводятся с помощью жеста прокрутки. Элементы считывания — это один или несколько `SwipeItem` объектов, помещенных в одну из четырех `SwipeView` направленных коллекций — `LeftItems` , `RightItems` , `TopItems` или `BottomItems` .

В следующем примере показано, как создать экземпляр `SwipeView` в XAML:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
    <Grid HeightRequest="60"
          WidthRequest="300"
          BackgroundColor="LightGray">
        <Label Text="Swipe right"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Grid>
</SwipeView>
```

Эквивалентный код на C# выглядит так:

```csharp
// SwipeItems
SwipeItem favoriteSwipeItem = new SwipeItem
{
    Text = "Favorite",
    IconImageSource = "favorite.png",
    BackgroundColor = Color.LightGreen
};
favoriteSwipeItem.Invoked += OnFavoriteSwipeItemInvoked;

SwipeItem deleteSwipeItem = new SwipeItem
{
    Text = "Delete",
    IconImageSource = "delete.png",
    BackgroundColor = Color.LightPink
};
deleteSwipeItem.Invoked += OnDeleteSwipeItemInvoked;

List<SwipeItem> swipeItems = new List<SwipeItem>() { favoriteSwipeItem, deleteSwipeItem };

// SwipeView content
Grid grid = new Grid
{
    HeightRequest = 60,
    WidthRequest = 300,
    BackgroundColor = Color.LightGray
};
grid.Children.Add(new Label
{
    Text = "Swipe right",
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center
});

SwipeView swipeView = new SwipeView
{
    LeftItems = new SwipeItems(swipeItems),
    Content = grid
};
```

В этом примере `SwipeView` содержимое — это [`Grid`](xref:Xamarin.Forms.Grid) , содержащее [`Label`](xref:Xamarin.Forms.Label) :

[![Снимок экрана с содержимым Свипевиев в iOS и Android](swipeview-images/swipeview-content.png "Свипевиев содержимое")](swipeview-images/swipeview-content-large.png#lightbox "Свипевиев содержимое")

Элементы считывания используются для выполнения действий с `SwipeView` содержимым и отображаются при считывании элемента управления с левой стороны:

[![Снимок экрана с Свипевиев считывания элементов в iOS и Android](swipeview-images/swipeview-swipeitems.png "Свипевиев считывание элементов")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Свипевиев считывание элементов")

По умолчанию элемент прокрутки выполняется при касании пользователем. Хотя это можно изменить. Дополнительные сведения см. в разделе [режим прокрутки](#swipe-mode).

После выполнения элемента считывания элементы прокрутки скрываются и `SwipeView` содержимое отображается повторно. Хотя это можно изменить. Дополнительные сведения см. в разделе [поведение при прокрутке](#swipe-behavior).

> [!NOTE]
> Прокрутка содержимого и считывание элементов можно разместить в строке или определить как ресурсы.

## <a name="swipe-items"></a>Прокрутка элементов

`LeftItems`Коллекции, `RightItems` , `TopItems` и `BottomItems` имеют тип `SwipeItems` . Класс `SwipeItems` определяет следующие свойства:

- `Mode`Тип `SwipeMode` , который указывает на результат взаимодействия при считывании. Дополнительные сведения о режиме прокрутки см. в разделе [режим прокрутки](#swipe-mode).
- `SwipeBehaviorOnInvoked`Тип `SwipeBehaviorOnInvoked` , который указывает, как работает `SwipeView` после вызова элемента считывания. Дополнительные сведения о поведении при прокрутке см. в разделе [поведение при прокрутке](#swipe-behavior).

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

Каждый элемент прокрутки определяется как `SwipeItem` объект, помещенный в одну из четырех `SwipeItems` направленных коллекций. `SwipeItem`Класс является производным от [`MenuItem`](xref:Xamarin.Forms.MenuItem) класса и добавляет следующие члены:

- `BackgroundColor`Свойство типа, `Color` определяющее цвет фона для элемента прокрутки. Это свойство поддерживается связываемым свойством.
- `Invoked`Событие, которое срабатывает при выполнении элемента считывания.

> [!IMPORTANT]
> [`MenuItem`](xref:Xamarin.Forms.MenuItem)Класс определяет несколько свойств, включая `Command` ,, `CommandParameter` `IconImageSource` и `Text` . Эти свойства можно задать `SwipeItem` для объекта, чтобы определить его внешний вид, и определить `ICommand` , которое выполняется при вызове элемента считывания. Дополнительные сведения см. в разделе [ Xamarin.Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md).

В следующем примере показаны два `SwipeItem` объекта в `LeftItems` коллекции `SwipeView` :

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

Внешний вид каждого из них `SwipeItem` определяется сочетанием `Text` `IconImageSource` свойств, и `BackgroundColor` :

[![Снимок экрана с Свипевиев считывания элементов в iOS и Android](swipeview-images/swipeview-swipeitems.png "Свипевиев считывание элементов")](swipeview-images/swipeview-swipeitems-large.png#lightbox "Свипевиев считывание элементов")

При `SwipeItem` касании его `Invoked` событие срабатывает и обрабатывается зарегистрированным обработчиком событий. Кроме того, `MenuItem.Clicked` событие срабатывает. Кроме того, `Command` для свойства можно задать `ICommand` реализацию, которая будет выполняться при `SwipeItem` вызове метода.

> [!NOTE]
> Если внешний вид `SwipeItem` определяется только с помощью `Text` `IconImageSource` свойств или, содержимое всегда выравнивается по центру.

Помимо определения элементов считывания как `SwipeItem` объектов, можно также определить пользовательские представления элементов считывания. Дополнительные сведения см. в разделе [настраиваемое считывание элементов](#custom-swipe-items).

## <a name="swipe-direction"></a>Направление прокрутки

`SwipeView` поддерживает четыре разных направления прокрутки с направлением считывания, определяемым направленной `SwipeItems` коллекцией, `SwipeItem` в которую добавляются объекты. Каждое направление прокрутки может содержать собственные элементы для считывания. Например, в следующем примере показано, для `SwipeView` которого элементы прокрутки зависят от направления прокрутки:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <SwipeView.RightItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Command="{Binding FavoriteCommand}" />
            <SwipeItem Text="Share"
                       IconImageSource="share.png"
                       BackgroundColor="LightYellow"
                       Command="{Binding ShareCommand}" />
        </SwipeItems>
    </SwipeView.RightItems>
    <!-- Content -->
</SwipeView>
```

В этом примере `SwipeView` содержимое можно прокрутить вправо или влево. В результате прокрутки вправо отображается элемент **Удалить** прокрутка, а при прокрутке влево — элементы " **Избранное** " и " **Общий** Просмотр".

> [!WARNING]
> Только один экземпляр направленной `SwipeItems` коллекции может быть установлен в каждый момент времени `SwipeView` . Поэтому в нельзя использовать два `LeftItems` определения `SwipeView` .

`SwipeStarted`События, `SwipeChanging` и `SwipeEnded` сообщают направление прокрутки через `SwipeDirection` свойство в аргументах события. Это свойство имеет тип `SwipeDirection` , который представляет собой перечисление, состоящее из четырех элементов:

- `Right` Указывает, что произошло право прокрутки.
- `Left` Указывает, что произошла левая прокрутка.
- `Up` Указывает, что произошло предыдущее считывание.
- `Down` Указывает, что произошло прокрутка вниз.

## <a name="swipe-threshold"></a>Порог прокрутки

`SwipeView` включает `Threshold` свойство типа `double` , представляющее число независимых от устройства единиц, которые активируют жест прокрутки для полного отображения элементов считывания.

В следующем примере показан объект `SwipeView` , который задает `Threshold` свойство:

```xaml
<SwipeView Threshold="200">
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

В этом примере `SwipeView` необходимо прокрутить для единиц, не зависящих от устройства 200, прежде чем `SwipeItem` будет полностью отображен.

> [!NOTE]
> В настоящее время `Threshold` свойство реализовано только в iOS и Android.

## <a name="swipe-mode"></a>Режим прокрутки

`SwipeItems`Класс имеет `Mode` свойство, которое указывает на результат взаимодействия при считывании. Этому свойству должно быть присвоено значение одного из `SwipeMode` членов перечисления:

- `Reveal` Указывает, что прокрутка показывает элементы считывания. Это значение по умолчанию для свойства `SwipeItems.Mode`.
- `Execute` Указывает, что прокрутка выполняет прокрутку элементов.

В режиме открытия пользователь выполняет `SwipeView` команду, чтобы открыть меню, состоящее из одного или нескольких элементов считывания, и для его выполнения необходимо явно коснуться элемента для прокрутки. После выполнения элемента прокрутки элементы прокрутки закрываются, а `SwipeView` содержимое снова отображается. В режиме выполнения пользователь проведите объект, `SwipeView` чтобы открыть меню, состоящее из одного большего количества элементов, которые затем автоматически выполняются. После выполнения прокрутка элементов закрывается и `SwipeView` содержимое снова отображается.

В следующем примере показана `SwipeView` Настройка для использования режима выполнения.

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems Mode="Execute">
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Command="{Binding DeleteCommand}" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

В этом примере `SwipeView` содержимое можно прокрутить вправо, чтобы отобразить элемент считывания, который выполняется немедленно. После выполнения `SwipeView` повторно отображается содержимое.

## <a name="swipe-behavior"></a>Поведение при прокрутке

`SwipeItems`Класс имеет `SwipeBehaviorOnInvoked` свойство, которое указывает, как работает `SwipeView` после вызова элемента считывания. Этому свойству должно быть присвоено значение одного из `SwipeBehaviorOnInvoked` членов перечисления:

- `Auto` Указывает, что в режиме отображения `SwipeView` закрывается после вызова элемента считывания и в режиме выполнения `SwipeView` после вызова элемента считывания остается открытым. Это значение по умолчанию для свойства `SwipeItems.SwipeBehaviorOnInvoked`.
- `Close` Указывает, что `SwipeView` закрывается после вызова элемента считывания.
- `RemainOpen` Указывает, что `SwipeView` компонент остается открытым после вызова элемента считывания.

В следующем примере показана `SwipeView` Настройка, которая остается открытой после вызова элемента считывания:

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems SwipeBehaviorOnInvoked="RemainOpen">
            <SwipeItem Text="Favorite"
                       IconImageSource="favorite.png"
                       BackgroundColor="LightGreen"
                       Invoked="OnFavoriteSwipeItemInvoked" />
            <SwipeItem Text="Delete"
                       IconImageSource="delete.png"
                       BackgroundColor="LightPink"
                       Invoked="OnDeleteSwipeItemInvoked" />
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

## <a name="custom-swipe-items"></a>Пользовательские элементы прокрутки

Пользовательские элементы прокрутки можно определить с помощью `SwipeItemView` типа. `SwipeItemView`Класс является производным от [`ContentView`](xref:Xamarin.Forms.ContentView) класса и добавляет следующие свойства:

- `Command`Тип `ICommand` , который выполняется при касании элемента прокрутки.
- `CommandParameter` с типом `object`, который передается как параметр в `Command`.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

`SwipeItemView`Класс также определяет `Invoked` событие, которое возникает при касании элемента, после `Command` выполнения.

В следующем примере показан `SwipeItemView` объект в `LeftItems` коллекции `SwipeView` :

```xaml
<SwipeView>
    <SwipeView.LeftItems>
        <SwipeItems>
            <SwipeItemView Command="{Binding CheckAnswerCommand}"
                           CommandParameter="{Binding Source={x:Reference resultEntry}, Path=Text}">
                <StackLayout Margin="10"
                             WidthRequest="300">
                    <Entry x:Name="resultEntry"
                           Placeholder="Enter answer"
                           HorizontalOptions="CenterAndExpand" />
                    <Label Text="Check"
                           FontAttributes="Bold"
                           HorizontalOptions="Center" />
                </StackLayout>
            </SwipeItemView>
        </SwipeItems>
    </SwipeView.LeftItems>
    <!-- Content -->
</SwipeView>
```

В этом примере объект состоит из, `SwipeItemView` [`StackLayout`](xref:Xamarin.Forms.StackLayout) содержащего, [`Entry`](xref:Xamarin.Forms.Entry) и [`Label`](xref:Xamarin.Forms.Label) . После того, как пользователь введет входные данные в `Entry` , можно коснуться остальной части объекта, `SwipeViewItem` который выполняет объект, `ICommand` определяемый `SwipeItemView.Command` свойством.

## <a name="open-and-close-a-swipeview-programmatically"></a>Программное открытие и закрытие SwipeView

`SwipeView` включает `Open` `Close` методы и, которые программно открывают и закрывают элементы прокрутки соответственно. По умолчанию эти методы будут анимировать объект `SwipeView` при открытии или закрытии.

`Open`Метод требует `OpenSwipeItem` аргумент, чтобы указать направление, `SwipeView` из которого будет открываться. `OpenSwipeItem`Перечисление состоит из четырех элементов:

- `LeftItems`, который указывает, что `SwipeView` будет открыт слева для отображения элементов, прокрутхся в `LeftItems` коллекции.
- `TopItems`, который указывает, что `SwipeView` будет открываться из верхней части, чтобы отобразить элементы считывания в `TopItems` коллекции.
- `RightItems`, который указывает, что объект `SwipeView` будет открываться справа, чтобы показать элементы считывания в `RightItems` коллекции.
- `BottomItems`, который указывает, что объект `SwipeView` будет открыт из нижней части для отображения элементов, прокрутхся в `BottomItems` коллекции.

Кроме `Open` того, метод также принимает необязательный `bool` аргумент, который определяет, `SwipeView` будет ли анимирован при открытии.

В `SwipeView` `swipeView` следующем примере показано, как открыть `SwipeView` для отображения элементов, прокрутхся в коллекции, с указанным именем `LeftItems` .

```csharp
swipeView.Open(OpenSwipeItem.LeftItems);
```

`swipeView`Затем объект можно закрыть с помощью `Close` метода:

```csharp
swipeView.Close();
```

> [!NOTE]
> `Close`Метод также принимает необязательный `bool` аргумент, который определяет, `SwipeView` будет ли анимирован при закрытии.

## <a name="disable-a-swipeview"></a>Отключение Свипевиев

Приложение может ввести состояние, при котором считывание элемента содержимого не является допустимой операцией. В таких случаях `SwipeView` можно отключить, задав `IsEnabled` свойству значение `false` . Это не позволит пользователям выполнять считывание содержимого для отображения элементов считывания.

Кроме того, при определении `Command` свойства `SwipeItem` или `SwipeItemView` `CanExecute` делегата `ICommand` можно указать, чтобы включить или отключить элемент прокрутки.

## <a name="related-links"></a>Связанные ссылки

- [Свипевиев (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-swipeviewdemos/)
- [Xamarin.Forms MenuItem](~/xamarin-forms/user-interface/menuitem.md)
