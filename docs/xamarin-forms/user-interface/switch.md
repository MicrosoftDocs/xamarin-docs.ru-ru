---
title: Xamarin.Forms Ключом
description: Xamarin.FormsПараметр — это тип кнопки, которая может управляться пользователем для переключения между состояниями. В этой статье объясняется, как использовать класс Switch для отображения переключаемого элемента пользовательского интерфейса.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 94f77fd70fee595efd341ff7372828b12661442d
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91561733"
---
# <a name="no-locxamarinforms-switch"></a>Xamarin.Forms Ключом

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Xamarin.Forms [`Switch`](xref:Xamarin.Forms.Switch) Элемент управления является горизонтальной выключателью, с помощью которого пользователь может переключаться между состояниями включения и выключения, которые представлены `boolean` значением. `Switch`Класс наследует от [`View`](xref:Xamarin.Forms.View) .

На следующих снимках экрана показан `Switch` элемент управления **on** в состояниях переключения и **отключения** в iOS и Android:

![Снимок экрана параметров включения и отключения состояний, в iOS и Android](switch-images/switch-states-default.png "Параметры в iOS и Android")

`Switch`Элемент управления определяет следующие свойства:

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)`boolean`значение, указывающее, включено ли в `Switch` . **on**
- [`OnColor`](xref:Xamarin.Forms.Switch.OnColor) параметр `Color` , который влияет на то, как объект отображается `Switch` в переключенном или **включенном**состоянии.
- `ThumbColor` значение параметра `Color` бегунка Switched.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом. Это означает, что `Switch` можно использовать стиль и цель привязок данных.

`Switch`Элемент управления определяет `Toggled` событие, которое возникает при `IsToggled` изменении свойства с помощью манипуляции пользователя или когда приложение задает `IsToggled` свойство. `ToggledEventArgs`Объект, сопровождающий `Toggled` событие, имеет одно свойство с именем `Value` типа `bool` . При срабатывании события значение `Value` Свойства отражает новое значение `IsToggled` Свойства.

## <a name="create-a-switch"></a>Создание переключателя

`Switch`Экземпляр можно создать в XAML. Его `IsToggled` свойство можно задать для переключения `Switch` . По умолчанию `IsToggled` свойство имеет значение `false` . В следующем примере показано, как создать экземпляр `Switch` в XAML с помощью необязательного `IsToggled` набора свойств:

```xaml
<Switch IsToggled="true"/>
```

`Switch`Также можно создать в коде:

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>Переключить внешний вид

В дополнение к свойствам, [`Switch`](xref:Xamarin.Forms.Switch) унаследованным от [`View`](xref:Xamarin.Forms.View) класса, `Switch` также определяет `OnColor` и `ThumbColor` Свойства. `OnColor`Свойство может быть задано для определения `Switch` цвета, когда он переключается в состояние **On** , а `ThumbColor` свойство может быть задано для определения `Color` типа бегунка переключателя. В следующем примере показано, как создать экземпляр `Switch` в XAML со следующими наборами свойств:

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

Свойства также можно задать при создании `Switch` в коде:

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

На следующем снимке экрана показаны `Switch` состояния переключателя **On** и **Off** , для которых `OnColor` `ThumbColor` заданы свойства и.

![Снимок экрана параметров включения и отключения состояний, в iOS и Android](switch-images/switch-states-colors.png "Параметры в iOS и Android")

## <a name="respond-to-a-switch-state-change"></a>Ответ на изменение состояния переключения

Когда `IsToggled` свойство изменяется либо с помощью пользовательской манипуляции, либо когда приложение задает `IsToggled` свойство, `Toggled` возникает событие. Для реагирования на изменение можно зарегистрировать обработчик событий для этого события:

```xaml
<Switch Toggled="OnToggled" />
```

Файл кода программной части содержит обработчик `Toggled` события:

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

`sender`Аргумент в обработчике событий является `Switch` ответственным за срабатывание этого события. Свойство можно использовать `sender` для доступа к `Switch` объекту или для различения нескольких `Switch` объектов, совместно использующих один и тот же `Toggled` обработчик событий.

`Toggled`Обработчик событий можно также назначить в коде:

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>Привязка данных коммутатора

`Toggled`Обработчик событий можно исключить с помощью привязки данных и триггеров, чтобы реагировать на `Switch` изменяющиеся состояния переключения.

```xaml
<Switch x:Name="styleSwitch" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference styleSwitch}, Path=IsToggled}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

В этом примере [`Label`](xref:Xamarin.Forms.Label) компонент использует выражение привязки в `DataTrigger` для отслеживания `IsToggled` свойства `Switch` с именем `styleSwitch` . Когда это свойство примет значение `true` , `FontAttributes` `FontSize` `Label` изменяются свойства и объекта. Когда `IsToggled` свойство возвращает значение `false` , `FontAttributes` `FontSize` Свойства и объекта `Label` сбрасываются в исходное состояние.

Дополнительные сведения о триггерах см. в разделе [ Xamarin.Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="switch-visual-states"></a>Переключение визуальных состояний

[`Switch`](xref:Xamarin.Forms.Switch) имеет `On` и `Off` визуальные состояния, которые можно использовать для инициации визуального изменения при [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) изменении свойства.

В следующем примере XAML показано, как определить визуальные состояния для `On` `Off` состояний и.

```xaml
<Switch IsToggled="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="On">
                <VisualState.Setters>
                    <Setter Property="ThumbColor"
                            Value="MediumSpringGreen" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Off">
                <VisualState.Setters>
                    <Setter Property="ThumbColor"
                            Value="Red" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Switch>
```

В этом примере `On` [`VisualState`](xref:Xamarin.Forms.VisualState) указывает, что если [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) свойство имеет значение `true` , `ThumbColor` для свойства будет задано значение Средний пружинный зеленый цвет. `Off` `VisualState` Указывает, что если `IsToggled` свойство имеет значение `false` , `ThumbColor` свойству будет присвоено значение Red. Таким образом, общий результат заключается в том, что когда `Switch` находится в положении «ВЫКЛ.», бегунок имеет красный цвет, а бегунок — светло-зеленый, когда `Switch` находится в положении ON:

![Снимок экрана Switch on VisualState on IOS and Android](switch-images/on-visualstate.png "Переключение на VisualState") 
 ![Снимок экрана: выключение VisualState в iOS и Android](switch-images/off-visualstate.png "Отключить VisualState")

Дополнительные сведения о визуальных состояниях см. в статье [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-a-switch"></a>Отключение переключателя

Приложение может перейти в состояние, в котором `Switch` переключаемый объект не является допустимой операцией. В таких случаях `Switch` можно отключить, задав `IsEnabled` свойству значение `false` . Это предотвратит возможность пользователей управлять `Switch` .

## <a name="related-links"></a>Связанные ссылки

- [Переключить демонстрации](/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
- [Триггеры Xamarin.Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)