---
title: Флажок Xamarin.Forms
description: Флажок Xamarin. Forms является типом кнопки, которая может быть либо установлена, либо пустой. Если флажок установлен, он считается включенным. Если флажок пуст, он считается отключенным.
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 10b7c4c3478545863ef49a23ef0f1be777e7eda9
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517115"
---
# <a name="xamarinforms-checkbox"></a>Флажок Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Xamarin. Forms `CheckBox` — это тип кнопки, которая может быть либо установлена, либо пустой. Если флажок установлен, он считается включенным. Если флажок пуст, он считается отключенным.

`CheckBox`Определяет `bool` свойство с именем `IsChecked`, которое указывает, установлен `CheckBox` ли флажок. Это свойство также поддерживается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом, то есть его можно присвоить стилю и быть целевым объектом привязок данных.

> [!NOTE]
> Свойство `IsChecked` BIND имеет режим привязки по умолчанию [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay).

`CheckBox`Определяет `CheckedChanged` событие, которое возникает при изменении `IsChecked` свойства с помощью манипуляции пользователя или когда приложение задает `IsChecked` свойство. `CheckedChangedEventArgs` Объект, сопровождающий `CheckedChanged` событие, имеет одно свойство с именем `Value`типа `bool`. При срабатывании события в качестве значения `Value` свойства задается новое значение `IsChecked` свойства.

## <a name="create-a-checkbox"></a>Создание флажка

В следующем примере показано, `CheckBox` как создать экземпляр в XAML:

```xaml
<CheckBox />
```

Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана пустого флажка в iOS и Android](checkbox-images/checkbox-empty.png "Пустой флажок")

По умолчанию `CheckBox` параметр пуст. `CheckBox` Можно проверить с помощью манипуляции с пользователем или задав для `IsChecked` `true`свойства значение:

```xaml
<CheckBox IsChecked="true" />
```

Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана флажка "отмечено" в iOS и Android](checkbox-images/checkbox-checked.png "Флажок флажка")

Кроме того, `CheckBox` можно создать в коде:

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>Реагирование на изменение состояния флажка

Когда `IsChecked` свойство изменяется либо с помощью пользовательской манипуляции, либо когда приложение задает `IsChecked` свойство, возникает `CheckedChanged` событие. Для реагирования на изменение можно зарегистрировать обработчик событий для этого события:

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

Файл кода программной части содержит обработчик `CheckedChanged` события:

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

`sender` Аргумент является `CheckBox` ответственным за это событие. Его можно использовать для доступа к `CheckBox` объекту или для различения нескольких `CheckBox` объектов, совместно использующих `CheckedChanged` один и тот же обработчик событий.

Кроме того, обработчик событий для `CheckedChanged` события можно зарегистрировать в коде:

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>Флажок привязки данных

Обработчик `CheckedChanged` событий можно исключить с помощью привязки данных и триггеров, чтобы реагировать на `CheckBox` проверяемый или пустой:

```xaml
<CheckBox x:Name="checkBox" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

В этом примере [`Label`](xref:Xamarin.Forms.Label) компонент использует выражение привязки в триггере данных для отслеживания `IsChecked` свойства объекта. `CheckBox` Когда это свойство примет `true`значение, `FontAttributes` свойства `FontSize` и для `Label` изменения. Когда `IsChecked` `false`свойство возвращает значение, свойства `FontAttributes` и `FontSize` объекта `Label` сбрасываются в исходное состояние.

На приведенных ниже снимках экрана в iOS отображается [`Label`](xref:Xamarin.Forms.Label) форматирование, когда `CheckBox` пусто, а на снимке экрана Android `Label` отображается форматирование при `CheckBox` проверке.

[![Снимок экрана: флажок привязки данных в iOS и Android](checkbox-images/checkbox-databinding.png "Флажок привязки данных")](checkbox-images/checkbox-databinding-large.png#lightbox "Флажок привязки данных")

Дополнительные сведения о триггерах см. в разделе [триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-checkbox"></a>Отключение флажка

Иногда приложение переходит в состояние, в котором `CheckBox` проверяемое значение не является допустимой операцией. В таких случаях `CheckBox` можно отключить, задав `IsEnabled` свойству значение. `false`

## <a name="checkbox-appearance"></a>Вид CheckBox

В дополнение к свойствам, `CheckBox` унаследованным от [`View`](xref:Xamarin.Forms.View) класса, `CheckBox` также определяет `Color` свойство, которое задает для его цвета значение. [`Color`](xref:Xamarin.Forms.Color)

```xaml
<CheckBox Color="Red" />
```

На следующих снимках экрана показан ряд проверенных `CheckBox` объектов, для каждого из которых `Color` задано другое [`Color`](xref:Xamarin.Forms.Color)свойство:

![Снимок экрана с цветными флажками в iOS и Android](checkbox-images/checkbox-colors.png "Цветовой флажок")

## <a name="checkbox-visual-states"></a>Визуальные состояния флажка

`CheckBox`имеет объект `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения в `CheckBox` при проверке.

В следующем примере XAML показано, как определить визуальное состояние для `IsChecked` состояния:

```xaml
<CheckBox ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Red" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="IsChecked">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Green" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</CheckBox>
```

В `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) этом примере параметр указывает, что если `CheckBox` флажок установлен, его `Color` свойство будет иметь значение Green. Указывает, что `CheckBox` когда находится в нормальном состоянии, его `Color` свойство будет установлено в значение Red. `Normal` `VisualState` Таким образом, общий результат заключается в `CheckBox` том, что красный, когда он пуст, и зеленый при проверке.

Дополнительные сведения о визуальных состояниях см. в разделе [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации CheckBox (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Триггеры Xamarin.Forms](~/xamarin-forms/app-fundamentals/triggers.md)
- [Диспетчер визуальных состояний Xamarin. Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
