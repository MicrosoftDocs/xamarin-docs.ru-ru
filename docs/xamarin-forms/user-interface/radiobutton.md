---
title: Xamarin.FormsRadioButton
description: Xamarin.FormsRadioButton — это тип кнопки, который позволяет пользователям выбрать один вариант из набора. Каждый параметр представлен одним переключателем, и вы можете выбрать только один переключатель в группе.
ms.prod: xamarin
ms.assetid: E2AA40E0-69A5-41DF-BFC4-C151CA657451
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/13/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f7cbd11f98127cb73514112dae785102ff9c51c0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127630"
---
# <a name="xamarinforms-radiobutton"></a>Xamarin.FormsRadioButton

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)

Xamarin.Forms `RadioButton` — Это тип кнопки, который позволяет пользователям выбрать один вариант из набора. Каждый параметр представлен одним переключателем, и вы можете выбрать только один переключатель в группе. `RadioButton`Класс наследует от [`Button`](xref:Xamarin.Forms.Button) класса.

На следующих снимках экрана показаны `RadioButton` объекты в состоянии очистки и выбора в iOS и Android:

![Снимок экрана с переключателями в выбранных и снятых состояниях в iOS и Android](radiobutton-images/radiobutton-states.png "Переключатели в iOS и Android")

> [!IMPORTANT]
> `RadioButton`в настоящее время экспериментально и может использоваться только путем установки `RadioButton_Experimental` флага. Дополнительные сведения см. в разделе [экспериментальные флаги](~/xamarin-forms/internals/experimental-flags.md).

`RadioButton`Элемент управления определяет следующие свойства:

- `IsChecked`Тип `bool` , который определяет, выбран ли объект `RadioButton` . Это свойство использует `TwoWay` привязку и имеет значение по умолчанию `false` .
- `GroupName`Тип `string` , который определяет имя, определяющее, какие `RadioButton` элементы управления являются взаимоисключающими. Это свойство имеет значение по умолчанию `null` .

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

`RadioButton`Элемент управления определяет `CheckedChanged` событие, которое возникает при `IsChecked` изменении свойства с помощью пользователя или программной манипуляции. `CheckedChangedEventArgs`Объект, сопровождающий `CheckedChanged` событие, имеет одно свойство с именем `Value` типа `bool` . При срабатывании события в качестве значения `Value` свойства задается новое значение `IsChecked` Свойства.

Кроме того, `RadioButton` класс наследует следующие свойства, обычно используемые из [`Button`](xref:Xamarin.Forms.Button) класса:

- [`Command`](xref:Xamarin.Forms.Button.Command)Тип `ICommand` , который выполняется при `RadioButton` выборе.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)Тип `object` , который является параметром, который передается в `Command` .
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)Тип [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) , который определяет стиль текста.
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)Тип `string` , который определяет семейство шрифтов.
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)Тип `double` , который определяет размер шрифта.
- [`Text`](xref:Xamarin.Forms.Button.Text)Тип `string` , который определяет отображаемый текст.
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)Тип [`Color`](xref:Xamarin.Forms.Color) , который определяет цвет отображаемого текста.

Дополнительные сведения об [`Button`](xref:Xamarin.Forms.Button) элементе управления см. в разделе [ Xamarin.Forms Button](~/xamarin-forms/user-interface/button.md).

## <a name="create-radiobuttons"></a>Создание переключателей

В следующем примере показано, как создать экземпляры `RadioButton` объектов в XAML:

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton Text="Cat" />
    <RadioButton Text="Dog" />
    <RadioButton Text="Elephant" />
    <RadioButton Text="Monkey"
                 IsChecked="true" />
</StackLayout>
```

В этом примере `RadioButton` объекты неявно группируются в один и тот же родительский контейнер. Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана с неявно сгруппированными переключателями в iOS и Android](radiobutton-images/radiobuttons.png "Неявно сгруппированные переключатели RadioButton в iOS и Android")

Кроме того, `RadioButton` объекты можно создавать в коде:

```csharp
StackLayout stackLayout = new StackLayout
{
    Children =
    {
        new Label { Text = "What's your favorite animal?" },
        new RadioButton { Text = "Cat" },
        new RadioButton { Text = "Dog" },
        new RadioButton { Text = "Elephant" },
        new RadioButton { Text = "Monkey", IsChecked = true }
    }
};
```

## <a name="group-radiobuttons"></a>Переключатели группы

Переключатели работают в группах, и существует два подхода к группированию переключателей:

- Помещение их в один и тот же родительский контейнер. Это называется неявной группировкой.
- Задав `GroupName` для свойства каждого переключателя одно и то же значение. Это называется явной группировкой.

В следующем примере XAML показано явное группирование `RadioButton` объектов путем задания их `GroupName` свойств:

```xaml
<Label Text="What's your favorite color?" />
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors" />
<RadioButton Text="Green"
             TextColor="Green"
             GroupName="colors" />
<RadioButton Text="Blue"
             TextColor="Blue"
             GroupName="colors" />
<RadioButton Text="Other"
             GroupName="colors" />
```

В этом примере каждый из них `RadioButton` является взаимоисключающим, так как он использует одно и то же `GroupName` значение. Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана явно сгруппированных переключателей в iOS и Android](radiobutton-images/grouped-radiobuttons.png "Явно сгруппированные переключатели в iOS и Android")

## <a name="respond-to-a-radiobutton-state-change"></a>Ответ на изменение состояния RadioButton

Переключатель может иметь два состояния: selected или cleared. Если переключатель выбран, его `IsChecked` свойство имеет значение `true` . При очистке переключателя его `IsChecked` свойство имеет значение `false` . Выбор переключателя можно отменить, выбрав другой переключатель в той же группе, но нельзя отменить выбор, щелкнув переключатель еще раз. Однако можно удалить переключатель программным путем, задав `IsChecked` для его свойства значение `false` .

Когда `IsChecked` свойство изменяется с помощью пользователя или программной манипуляции, `CheckedChanged` вызывается событие. Для реагирования на изменение можно зарегистрировать обработчик событий для этого события:

```xaml
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors"
             CheckedChanged="OnColorsRadioButtonCheckedChanged" />
```

Код программной части содержит обработчик `CheckedChanged` события:

```csharp
void OnColorsRadioButtonCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation
}
```

`sender`Аргумент является `RadioButton` ответственным за это событие. Его можно использовать для доступа к `RadioButton` объекту или для различения нескольких объектов, `RadioButton` совместно использующих один и тот же `CheckedChanged` обработчик событий.

Кроме того, обработчик событий для `CheckedChanged` события можно зарегистрировать в коде:

```csharp
RadioButton radioButton = new RadioButton { ... };
radioButton.CheckedChanged += (sender, e) =>
{
    // Perform required operation
};
```

> [!NOTE]
> Альтернативный подход к реагированию на `RadioButton` изменение состояния заключается в определении `ICommand` и присвоении его `RadioButton.Command` свойству. Дополнительные сведения см. [в разделе кнопка: использование интерфейса команды](~/xamarin-forms/user-interface/button.md#using-the-command-interface).

## <a name="radiobutton-visual-states"></a>Визуальные состояния RadioButton

`RadioButton`имеет объект `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения при `RadioButton` выборе.

В следующем примере XAML показано, как определить визуальное состояние для `IsChecked` состояния:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Red" />
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="IsChecked">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Green" />
                                <Setter Property="Opacity"
                                        Value="1" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout>
        <Label Text="What's your favorite mode of transport?" />
        <RadioButton Text="Car"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Bike"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Train"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Walking"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
    </StackLayout>
</ContentPage>
```

В этом примере неявный элемент [`Style`](xref:Xamarin.Forms.Style) направлен на объекты `RadioButton`. `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) Указывает, что если `RadioButton` выбран параметр, его `TextColor` свойство будет иметь значение Green, равное `Opacity` 1. `Normal` `VisualState` Указывает, что когда элемент `RadioButton` находится в состоянии очистки, его `TextColor` свойство будет установлено в значение Red со `Opacity` значением 0,5. Таким образом, общий эффект заключается в том, что при `RadioButton` очистке он выделяется красным и частично прозрачным и выделяется зеленым цветом без прозрачности, если он выбран:

![Снимок экрана: набор элементов RadioButton, установленный визуальным состоянием, в iOS и Android](radiobutton-images/ischecked-visualstate.png "Визуальные состояния RadioButton в iOS и Android")

Дополнительные сведения о визуальных состояниях см. в статье [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-a-radiobutton"></a>Отключение RadioButton

Иногда приложение переходит в состояние, в котором проверяемое значение `RadioButton` не является допустимой операцией. В таких случаях `RadioButton` можно отключить, задав `IsEnabled` свойству значение `false` .

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации RadioButton (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)
- [Кнопка Xamarin.Forms](~/xamarin-forms/user-interface/button.md)
- [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
