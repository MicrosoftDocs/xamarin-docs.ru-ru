---
title: Xamarin.FormsКлючом
description: Xamarin.FormsПараметр — это тип кнопки, которая может управляться пользователем для переключения между состояниями. В этой статье объясняется, как использовать класс Switch для отображения переключаемого элемента пользовательского интерфейса.
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a5c2583b7632acdfa7d8439dc96b3964fa3cfcab
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136244"
---
# <a name="xamarinforms-switch"></a>Xamarin.FormsКлючом

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Xamarin.Forms [`Switch`](xref:Xamarin.Forms.Switch) Элемент управления является горизонтальной выключателью, с помощью которого пользователь может переключаться между состояниями включения и выключения, которые представлены `boolean` значением. `Switch`Класс наследует от [`View`](xref:Xamarin.Forms.View) .

На следующих снимках экрана показан `Switch` элемент управления **on** в состояниях переключения и **отключения** в iOS и Android:

![Снимок экрана параметров включения и отключения состояний, в iOS и Android](switch-images/switch-states-default.png "Параметры в iOS и Android")

`Switch`Элемент управления определяет следующие свойства:

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)`boolean`значение, указывающее, включено ли в `Switch` . **on**
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor)параметр `Color` , который влияет на то, как объект отображается `Switch` в переключенном или **включенном**состоянии.
* `ThumbColor`значение параметра `Color` бегунка Switched.

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

## <a name="disable-a-switch"></a>Отключение переключателя

Приложение может перейти в состояние, в котором `Switch` переключаемый объект не является допустимой операцией. В таких случаях `Switch` можно отключить, задав `IsEnabled` свойству значение `false` . Это предотвратит возможность пользователей управлять `Switch` .

## <a name="related-links"></a>Связанные ссылки

* [Переключить демонстрации](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Xamarin.FormsПлан](~/xamarin-forms/app-fundamentals/triggers.md)
