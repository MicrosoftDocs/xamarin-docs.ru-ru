---
title: Xamarin.Forms RadioButton
description: Xamarin.FormsRadioButton — это тип кнопки, который позволяет пользователям выбрать один вариант из набора. Каждый параметр представлен одним переключателем, и вы можете выбрать только один переключатель в группе.
ms.prod: xamarin
ms.assetid: E2AA40E0-69A5-41DF-BFC4-C151CA657451
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f1f36329c36dfae3f2ca0754ecbb867520d4c530
ms.sourcegitcommit: bd8ce0ae64698246db92a6c4396983290dc54a22
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99427750"
---
# <a name="xamarinforms-radiobutton"></a>Xamarin.Forms RadioButton

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)

Xamarin.Forms [`RadioButton`](xref:Xamarin.Forms.RadioButton) — Это тип кнопки, который позволяет пользователям выбрать один вариант из набора. Каждый параметр представлен одним переключателем, и вы можете выбрать только один переключатель в группе. По умолчанию каждый из них [`RadioButton`](xref:Xamarin.Forms.RadioButton) отображает текст:

![Снимок экрана с переключателями по умолчанию](radiobutton-images/radiobuttons-default.png "Переключатели по умолчанию")

Однако на некоторых платформах a [`RadioButton`](xref:Xamarin.Forms.RadioButton) может отображать [`View`](xref:Xamarin.Forms.View) , а на всех платформах их внешний вид `RadioButton` можно переопределить с помощью [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) :

![Снимок экрана с переопределенными переключателями](radiobutton-images/radiobuttons-controltemplate.png "Переопределенные переключатели")

[`RadioButton`](xref:Xamarin.Forms.RadioButton)Элемент управления определяет следующие свойства:

- `Content`Тип `object` , который определяет `string` или [`View`](xref:Xamarin.Forms.View) для отображения `RadioButton` .
- `IsChecked`Тип `bool` , который определяет, `RadioButton` установлен ли флажок. Это свойство использует `TwoWay` привязку и имеет значение по умолчанию `false` .
- `GroupName`Тип `string` , который определяет имя, определяющее, какие `RadioButton` элементы управления являются взаимоисключающими. Это свойство имеет значение по умолчанию `null` .
- `Value`Тип `object` , который определяет необязательное уникальное значение, связанное с `RadioButton` .
- `BorderColor`Тип [`Color`](xref:Xamarin.Forms.Color) , который определяет цвет обводки границы.
- `BorderWidth`Тип `double` , который определяет ширину `RadioButton` границы.
- `CharacterSpacing`Тип `double` , который определяет интервал между символами отображаемого текста.
- `CornerRadius`Тип `int` , который определяет радиус угла `RadioButton` .
- `FontAttributes`Тип [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) , который определяет стиль текста.
- `FontFamily`Тип `string` , который определяет семейство шрифтов.
- `FontSize`Тип `double` , который определяет размер шрифта.
- `TextColor`Тип [`Color`](xref:Xamarin.Forms.Color) , который определяет цвет отображаемого текста.
- `TextTransform`Тип `TextTransform` , который определяет регистр любого отображаемого текста.

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

[`RadioButton`](xref:Xamarin.Forms.RadioButton)Элемент управления также определяет `CheckedChanged` событие, которое возникает при `IsChecked` изменении свойства с помощью пользователя или программной манипуляции. `CheckedChangedEventArgs`Объект, сопровождающий `CheckedChanged` событие, имеет одно свойство с именем `Value` типа `bool` . При срабатывании события в качестве значения `CheckedChangedEventArgs.Value` свойства задается новое значение `IsChecked` Свойства.

[`RadioButton`](xref:Xamarin.Forms.RadioButton) Группирование может управляться `RadioButtonGroup` классом, который определяет следующие вложенные свойства:

-   `GroupName`Тип `string` , который определяет имя группы для `RadioButton` объектов в `Layout<View>` .
- `SelectedValue`Тип `object` , который представляет значение проверяемого `RadioButton` объекта в `Layout<View>` группе. Это присоединенное свойство использует `TwoWay` привязку по умолчанию.

Дополнительные сведения о `GroupName` присоединенном свойстве см. в разделе [переключатели группы](#group-radiobuttons). Дополнительные сведения о `SelectedValue` присоединенном свойстве см. [в разделе реагирование на изменения состояния RadioButton](#respond-to-radiobutton-state-changes).

## <a name="create-radiobuttons"></a>Создание переключателей

Внешний вид определяется [`RadioButton`](xref:Xamarin.Forms.RadioButton) типом данных, назначенных `RadioButton.Content` свойству:

- Когда `RadioButton.Content` свойству присвоено значение `string` , оно будет отображаться на каждой платформе, горизонтально выравниваемая рядом с переключателем.
- Когда `RadioButton.Content` назначается [`View`](xref:Xamarin.Forms.View) , оно будет отображаться на поддерживаемых платформах (iOS, UWP), а Неподдерживаемые платформы будут переноситься в строковое представление `View` объекта (Android). В обоих случаях содержимое отображается горизонтально по вертикали рядом с переключателем Circle.
- При [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) применении к [`RadioButton`](xref:Xamarin.Forms.RadioButton) [`View`](xref:Xamarin.Forms.View) можно присвоить `RadioButton.Content` свойству на всех платформах. Дополнительные сведения см. в разделе [Переопределение внешнего вида RadioButton](#redefine-radiobutton-appearance).

### <a name="display-string-based-content"></a>Отображение содержимого на основе строк

[`RadioButton`](xref:Xamarin.Forms.RadioButton)Отображает текст, когда `Content` свойству присваивается значение `string` .

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton Content="Cat" />
    <RadioButton Content="Dog" />
    <RadioButton Content="Elephant" />
    <RadioButton Content="Monkey"
                 IsChecked="true" />
</StackLayout>
```

В этом примере [`RadioButton`](xref:Xamarin.Forms.RadioButton) объекты неявно группируются в один и тот же родительский контейнер. Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана с текстовыми переключателями](radiobutton-images/radiobuttons-text.png "Текстовые переключатели")

### <a name="display-arbitrary-content"></a>Отображение произвольного содержимого

В iOS и UWP [`RadioButton`](xref:Xamarin.Forms.RadioButton) может отображать произвольное содержимое, когда `Content` свойству назначается [`View`](xref:Xamarin.Forms.View) :

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton>
        <RadioButton.Content>
            <Image Source="cat.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton>
        <RadioButton.Content>
            <Image Source="dog.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton>
        <RadioButton.Content>
            <Image Source="elephant.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton>
        <RadioButton.Content>
            <Image Source="monkey.png" />
        </RadioButton.Content>
    </RadioButton>
</StackLayout>
```

В этом примере [`RadioButton`](xref:Xamarin.Forms.RadioButton) объекты неявно группируются в один и тот же родительский контейнер. Этот XAML приводит к отображению на следующих снимках экрана:

![Снимок экрана с переключателями на основе представлений](radiobutton-images/radiobuttons-view.png "Переключатели на основе представления")

В Android [`RadioButton`](xref:Xamarin.Forms.RadioButton) объекты будут отображать строковое представление [`View`](xref:Xamarin.Forms.View) объекта, установленного как содержимое:

![Снимок экрана с переключателем на основе представлений в Android](radiobutton-images/radiobutton-view-android.png "Переключатель на основе представления на Android")

> [!NOTE]
> При [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) применении к [`RadioButton`](xref:Xamarin.Forms.RadioButton) [`View`](xref:Xamarin.Forms.View) можно присвоить `RadioButton.Content` свойству на всех платформах. Дополнительные сведения см. в разделе [Переопределение внешнего вида RadioButton](#redefine-radiobutton-appearance).

## <a name="associate-values-with-radiobuttons"></a>Связывание значений с RadioButton

Каждый [`RadioButton`](xref:Xamarin.Forms.RadioButton) объект имеет `Value` свойство типа `object` , которое определяет необязательное уникальное значение для связи с переключателем. Это позволяет использовать значение, которое `RadioButton` будет отличаться для его содержимого, и особенно полезно при `RadioButton` отображении объектов объектами [`View`](xref:Xamarin.Forms.View) .

В следующем коде XAML показано, как задать `Content` `Value` Свойства и для каждого [`RadioButton`](xref:Xamarin.Forms.RadioButton) объекта:

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton Value="Cat">
        <RadioButton.Content>
            <Image Source="cat.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton Value="Dog">
        <RadioButton.Content>
            <Image Source="dog.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton Value="Elephant">
        <RadioButton.Content>
            <Image Source="elephant.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton Value="Monkey">
        <RadioButton.Content>
            <Image Source="monkey.png" />
        </RadioButton.Content>
    </RadioButton>
</StackLayout>
```

В этом примере каждый [`RadioButton`](xref:Xamarin.Forms.RadioButton) имеет в [`Image`](xref:Xamarin.Forms.Image) качестве своего содержимого, а также определяет строковое значение. Это позволяет легко идентифицировать значение проверяемого переключателя.

## <a name="group-radiobuttons"></a>Переключатели группы

Переключатели работают в группах, и существует три подхода к группированию переключателей:

- Поместите их в один и тот же родительский контейнер. Это называется *неявной* группировкой.
- `GroupName`Для каждого переключателя в группе установите для свойства одно и то же значение. Это называется *явной* группировкой.
- Установите `RadioButtonGroup.GroupName` присоединенное свойство для родительского контейнера, который, в свою очередь, устанавливает `GroupName` свойство любых [`RadioButton`](xref:Xamarin.Forms.RadioButton) объектов в контейнере. Это также называется *явной* группировкой.

> [!IMPORTANT]
> [`RadioButton`](xref:Xamarin.Forms.RadioButton) объекты не обязательно должны принадлежать к одному и тому же родительскому элементу, который должен быть сгруппирован. Они являются взаимоисключающими при условии, что они совместно используют имя группы.

### <a name="explicit-grouping-with-the-groupname-property"></a>Явная группировка со свойством GroupName

В следующем примере XAML показано явное группирование [`RadioButton`](xref:Xamarin.Forms.RadioButton) объектов путем задания их `GroupName` свойств:

```xaml
<Label Text="What's your favorite color?" />
<RadioButton Content="Red"
             GroupName="colors" />
<RadioButton Content="Green"
             GroupName="colors" />
<RadioButton Content="Blue"
             GroupName="colors" />
<RadioButton Content="Other"
             GroupName="colors" />
```

В этом примере каждый из них [`RadioButton`](xref:Xamarin.Forms.RadioButton) является взаимоисключающим, так как он использует одно и то же `GroupName` значение.

### <a name="explicit-grouping-with-the-radiobuttongroupgroupname-attached-property"></a>Явная группировка с присоединенным свойством Радиобуттонграуп. GroupName

`RadioButtonGroup`Класс определяет `GroupName` присоединенное свойство типа `string` , которое может быть задано для `Layout<View>` объекта. Это позволяет превратить любой макет в группу переключателей:

```xaml
<StackLayout RadioButtonGroup.GroupName="colors">
    <Label Text="What's your favorite color?" />
    <RadioButton Content="Red" />
    <RadioButton Content="Green" />
    <RadioButton Content="Blue" />
    <RadioButton Content="Other" />
</StackLayout>
```

В этом примере для каждого [`RadioButton`](xref:Xamarin.Forms.RadioButton) свойства в [`StackLayout`](xref:Xamarin.Forms.StackLayout) `GroupName` свойстве будет задано значение `fruits` , и оно будет взаимно исключающим.

> [!NOTE]
> Если `Layout<View` объект>, задающий `RadioButtonGroup.GroupName` присоединенное свойство, содержит объект [`RadioButton`](xref:Xamarin.Forms.RadioButton) , который задает его `GroupName` свойство, значение `RadioButton.GroupName` свойства будет иметь приоритет.

## <a name="respond-to-radiobutton-state-changes"></a>Реагирование на изменения состояния RadioButton

Возможных состояния у переключателя два: установлен или не установлен. Если переключатель установлен, его `IsChecked` свойство имеет значение `true` . Если переключатель не установлен, его `IsChecked` свойство имеет значение `false` . Переключатель можно очистить, коснувшись другого переключателя в той же группе, но нельзя снять его с помощью повторного нажатия. При этом выбор переключателя можно отменить и программными средствами, установив для свойства `IsChecked` значение `false`.

### <a name="respond-to-an-event-firing"></a>Реагирование на срабатывание события

Когда `IsChecked` свойство изменяется с помощью пользователя или программной манипуляции, `CheckedChanged` вызывается событие. Для реагирования на изменение можно зарегистрировать обработчик событий для этого события:

```xaml
<RadioButton Content="Red"
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

`sender`Аргумент является [`RadioButton`](xref:Xamarin.Forms.RadioButton) ответственным за это событие. Его можно использовать для доступа к [`RadioButton`](xref:Xamarin.Forms.RadioButton) объекту или для различения нескольких объектов, `RadioButton` совместно использующих один и тот же `CheckedChanged` обработчик событий.

### <a name="respond-to-a-property-change"></a>Реагирование на изменение свойства

`RadioButtonGroup`Класс определяет `SelectedValue` присоединенное свойство типа `object` , которое может быть задано для `Layout<View>` объекта. Это присоединенное свойство представляет значение проверки в [`RadioButton`](xref:Xamarin.Forms.RadioButton) группе, определенной в макете.

При `IsChecked` изменении свойства с помощью пользователя или программной манипуляции `RadioButtonGroup.SelectedValue` присоединенное свойство также изменяется. Поэтому `RadioButtonGroup.SelectedValue` присоединенное свойство может быть привязано к данным свойства, в котором хранится выбор пользователя:

```xaml
<StackLayout RadioButtonGroup.GroupName="{Binding GroupName}"
             RadioButtonGroup.SelectedValue="{Binding Selection}">
    <Label Text="What's your favorite animal?" />
    <RadioButton Content="Cat"
                 Value="Cat" />
    <RadioButton Content="Dog"
                 Value="Dog" />
    <RadioButton Content="Elephant"
                 Value="Elephant" />
    <RadioButton Content="Monkey"
                 Value="Monkey"/>
    <Label x:Name="animalLabel">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="You have chosen:" />
                <Span Text="{Binding Selection}" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
</StackLayout>
```

В этом примере значение `RadioButtonGroup.GroupName` присоединенного свойства задается `GroupName` свойством в контексте привязки. Аналогичным образом значение `RadioButtonGroup.SelectedValue` присоединенного свойства задается `Selection` свойством в контексте привязки. Кроме того, `Selection` свойство обновляется до `Value` свойства установленного [`RadioButton`](xref:Xamarin.Forms.RadioButton) .

## <a name="radiobutton-visual-states"></a>Визуальные состояния RadioButton

[`RadioButton`](xref:Xamarin.Forms.RadioButton) объекты имеют `Checked` и `Unchecked` визуальные состояния, которые можно использовать для инициации визуального изменения при установке `RadioButton` или снятии флажка.

В следующем примере XAML показано, как определить визуальное состояние для `Checked` состояний и `Unchecked` .

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CheckedStates">
                        <VisualState x:Name="Checked">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Green" />
                                <Setter Property="Opacity"
                                        Value="1" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="Unchecked">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Red" />
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout>
        <Label Text="What's your favorite mode of transport?" />
        <RadioButton Content="Car" />
        <RadioButton Content="Bike" />
        <RadioButton Content="Train" />
        <RadioButton Content="Walking" />
    </StackLayout>
</ContentPage>
```

В этом примере неявный элемент [`Style`](xref:Xamarin.Forms.Style) направлен на объекты [`RadioButton`](xref:Xamarin.Forms.RadioButton). `Checked` [`VisualState`](xref:Xamarin.Forms.VisualState) Указывает, что если `RadioButton` флажок установлен, его `TextColor` свойство будет установлено в значение Green со `Opacity` значением 1. `Unchecked` `VisualState` Указывает, что если `RadioButton` параметр находится в непроверенном состоянии, его `TextColor` свойство будет установлено в значение Red со `Opacity` значением 0,5. Таким образом, общий эффект заключается в том, что при снятии `RadioButton` флажка он выделяется красным и частично прозрачным и выделяется зеленым цветом без прозрачности при проверке:

![Снимок экрана визуальных состояний RadioButton](radiobutton-images/radiobuttons-visualstates.png "Визуальные состояния RadioButton")

Дополнительные сведения о визуальных состояниях см. в статье [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="redefine-radiobutton-appearance"></a>Изменение внешнего вида RadioButton

По умолчанию [`RadioButton`](xref:Xamarin.Forms.RadioButton) объекты используют средства визуализации платформы для использования собственных элементов управления на поддерживаемых платформах. Однако `RadioButton` визуальную структуру можно переопределить с помощью [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) , чтобы `RadioButton` объекты имели одинаковый внешний вид на всех платформах. Это возможно, поскольку [`RadioButton`](xref:Xamarin.Forms.RadioButton) класс наследуется от [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) класса.

В следующем коде XAML показан объект [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) , который можно использовать для переопределения визуальной структуры [`RadioButton`](xref:Xamarin.Forms.RadioButton) объектов:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="RadioButtonTemplate">
            <Frame BorderColor="#F3F2F1"
                   BackgroundColor="#F3F2F1"
                   HasShadow="False"
                   HeightRequest="100"
                   WidthRequest="100"
                   HorizontalOptions="Start"
                   VerticalOptions="Start"
                   Padding="0">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CheckedStates">
                            <VisualState x:Name="Checked">
                                <VisualState.Setters>
                                    <Setter Property="BorderColor"
                                            Value="#FF3300" />
                                    <Setter TargetName="check"
                                            Property="Opacity"
                                            Value="1" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Unchecked">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor"
                                            Value="#F3F2F1" />
                                    <Setter Property="BorderColor"
                                            Value="#F3F2F1" />
                                    <Setter TargetName="check"
                                            Property="Opacity"
                                            Value="0" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </VisualStateManager.VisualStateGroups>
                <Grid Margin="4"
                      WidthRequest="100">
                    <Grid WidthRequest="18"
                          HeightRequest="18"
                          HorizontalOptions="End"
                          VerticalOptions="Start">
                        <Ellipse Stroke="Blue"
                                 Fill="White"
                                 WidthRequest="16"
                                 HeightRequest="16"
                                 HorizontalOptions="Center"
                                 VerticalOptions="Center" />
                        <Ellipse x:Name="check"
                                 Fill="Blue"
                                 WidthRequest="8"
                                 HeightRequest="8"
                                 HorizontalOptions="Center"
                                 VerticalOptions="Center" />
                    </Grid>
                    <ContentPresenter />
                </Grid>
            </Frame>
        </ControlTemplate>

        <Style TargetType="RadioButton">
            <Setter Property="ControlTemplate"
                    Value="{StaticResource RadioButtonTemplate}" />
        </Style>
    </ContentPage.Resources>
    <!-- Page content -->
</ContentPage>
```

В этом примере корневой элемент объекта — это [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) [`Frame`](xref:Xamarin.Forms.Frame) объект, который определяет `Checked` и `Unchecked` визуальные состояния. `Frame`Объект использует сочетание [`Grid`](xref:Xamarin.Forms.Grid) [`Ellipse`](xref:Xamarin.Forms.Shapes.Ellipse) объектов, и [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) для определения визуальной структуры [`RadioButton`](xref:Xamarin.Forms.RadioButton) . Пример также включает *неявный* стиль, который присвоит `RadioButtonTemplate` `ControlTemplate` свойству любого `RadioButton` объекта на странице.

> [!NOTE]
> [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)Объект отмечает расположение в визуальной структуре, в которой [`RadioButton`](xref:Xamarin.Forms.RadioButton) будет отображаться содержимое.

В следующем коде XAML показаны [`RadioButton`](xref:Xamarin.Forms.RadioButton) объекты, использующие с [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) помощью *неявного* стиля:

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <StackLayout RadioButtonGroup.GroupName="animals"
                 Orientation="Horizontal">
        <RadioButton Value="Cat">
            <RadioButton.Content>
                <StackLayout>
                    <Image Source="cat.png"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand" />
                    <Label Text="Cat"
                           HorizontalOptions="Center"
                           VerticalOptions="End" />
                </StackLayout>
            </RadioButton.Content>
        </RadioButton>
        <RadioButton Value="Dog">
            <RadioButton.Content>
                <StackLayout>
                    <Image Source="dog.png"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand" />
                    <Label Text="Dog"
                           HorizontalOptions="Center"
                           VerticalOptions="End" />
                </StackLayout>
            </RadioButton.Content>
        </RadioButton>
        <RadioButton Value="Elephant">
            <RadioButton.Content>
                <StackLayout>
                    <Image Source="elephant.png"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand" />
                    <Label Text="Elephant"
                           HorizontalOptions="Center"
                           VerticalOptions="End" />
                </StackLayout>
            </RadioButton.Content>
        </RadioButton>
        <RadioButton Value="Monkey">
            <RadioButton.Content>
                <StackLayout>
                    <Image Source="monkey.png"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand" />
                    <Label Text="Monkey"
                           HorizontalOptions="Center"
                           VerticalOptions="End" />
                </StackLayout>
            </RadioButton.Content>
        </RadioButton>
    </StackLayout>
</StackLayout>
```

В этом примере визуальная структура, определенная для каждой из них, [`RadioButton`](xref:Xamarin.Forms.RadioButton) заменяется визуальной структурой, определенной в [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) , и поэтому во время выполнения объекты `ControlTemplate` становятся частью визуального дерева для каждого из них `RadioButton` . Кроме того, содержимое для каждого из них `RadioButton` подставляется в элемент, [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) определенный в шаблоне элемента управления. Это приводит к следующему `RadioButton` виду:

![Снимок экрана с шаблонными переключателями](radiobutton-images/radiobuttons-templated.png "Шаблонные переключатели")

Дополнительные сведения о шаблонах элементов управления см. в разделе [ Xamarin.Forms шаблоны элементов управления](~/xamarin-forms/app-fundamentals/templates/control-template.md).

## <a name="disable-a-radiobutton"></a>Отключение RadioButton

Иногда приложение переходит в состояние, в котором проверяемое значение `RadioButton` не является допустимой операцией. В таких случаях `RadioButton` можно отключить, задав `IsEnabled` свойству значение `false` .

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации RadioButton (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)
- [Кнопка Xamarin.Forms](~/xamarin-forms/user-interface/button.md)
- [Диспетчер визуального представления состояний Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md)
