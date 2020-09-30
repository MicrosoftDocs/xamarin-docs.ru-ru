---
title: Xamarin.Forms Диспетчер визуальных состояний
description: Используйте Диспетчер визуальных состояний для внесения изменений в элементы XAML на основе визуальных состояний, заданных из кода.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7e59cddbe9192f29ca1636c567131aad60157066
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556585"
---
# <a name="no-locxamarinforms-visual-state-manager"></a>Xamarin.Forms Диспетчер визуальных состояний

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Используйте Диспетчер визуальных состояний для внесения изменений в элементы XAML на основе визуальных состояний, заданных из кода._

Диспетчер визуальных состояний (VSM) предоставляет структурированный способ внесения визуальных изменений в пользовательский интерфейс из кода. В большинстве случаев пользовательский интерфейс приложения определен в XAML, а этот XAML включает в себя разметку, описывающую влияние диспетчера визуального состояния на визуальные элементы пользовательского интерфейса.

В VSM введена концепция _визуальных состояний_. Xamarin.FormsПредставление, например, `Button` может иметь несколько различных визуальных представлений в зависимости от его базового состояния: &mdash; отключено, нажато или имеет фокус ввода. Это состояния кнопки.

Визуальные состояния собираются в _группах визуальных состояний_. Все визуальные состояния в пределах визуальной группы состояний являются взаимоисключающими. Визуальные состояния и группы визуальных состояний идентифицируются простыми текстовыми строками.

Xamarin.FormsДиспетчер визуальных состояний определяет одну группу визуальных состояний с именем "коммонстатес" со следующими визуальными состояниями:

- "Normal"
- Доступ
- Указанной
- Выбрать

Эта группа визуальных состояний поддерживается для всех классов, производных от [`VisualElement`](xref:Xamarin.Forms.VisualElement) , которые являются базовым классом для [`View`](xref:Xamarin.Forms.View) и [`Page`](xref:Xamarin.Forms.Page) .

Можно также определить собственные визуальные группы состояний и визуальные состояния, как показано в этой статье.

> [!NOTE]
> Xamarin.Forms Разработчики, знакомые с [триггерами](~/xamarin-forms/app-fundamentals/triggers.md) , знают, что триггеры также могут вносить изменения в визуальные элементы в пользовательском интерфейсе на основе изменений в свойствах представления или при срабатывании событий. Однако использование триггеров для работы с различными сочетаниями этих изменений может стать весьма запутанным. Исторически, диспетчер визуального состояния появился в средах на основе XAML в Windows, чтобы сократить путаницу, полученную из-за сочетаний визуальных состояний. В VSM визуальные состояния в пределах визуальной группы состояний всегда являются взаимоисключающими. В любой момент только одно состояние в каждой группе — Текущее состояние.

## <a name="common-states"></a>Распространенные состояния

Диспетчер визуальных состояний позволяет включить в файл XAML разметку, которая может изменить внешний вид представления, если оно является нормальным или отключенным, или имеет фокус ввода. Они называются _распространенными состояниями_.

Например, предположим, что `Entry` на странице есть представление, и вы хотите, чтобы внешний вид элемента `Entry` изменялся следующим образом:

- `Entry`При отключении элемент должен иметь розовый фон `Entry` .
- Элемент `Entry` должен иметь травяной фон обычно.
- `Entry`При наличии фокуса ввода должен расширять его нормальную высоту в два раза.

Разметку VSM можно прикрепить к отдельному представлению или можно определить в стиле, если она применяется к нескольким представлениям. Эти подходы описаны в следующих двух разделах.

### <a name="vsm-markup-on-a-view"></a>Разметка VSM в представлении

Чтобы присоединить разметку VSM к `Entry` представлению, сначала разделяйте `Entry` открывающий и конечный теги.

```xaml
<Entry FontSize="18">

</Entry>
```

Он получает явный размер шрифта, так как одно из состояний использует `FontSize` свойство, чтобы удвоить размер текста в `Entry` .

Затем вставьте `VisualStateManager.VisualStateGroups` теги между этими тегами:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) — Это присоединяемое свойство с возможностью привязки, определенное [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) классом. (Дополнительные сведения о присоединенных свойствах привязки см. в статье [присоединенные свойства](~/xamarin-forms/xaml/attached-properties.md).) Таким способом `VisualStateGroups` свойство прикрепляется к `Entry` объекту.

`VisualStateGroups`Свойство имеет тип [`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList) , который представляет собой коллекцию [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) объектов. В `VisualStateManager.VisualStateGroups` тегах вставьте пару `VisualStateGroup` тегов для каждой группы визуальных состояний, которые вы хотите включить:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Обратите внимание, что `VisualStateGroup` тег имеет `x:Name` атрибут, указывающий имя группы. `VisualStateGroup`Класс определяет `Name` свойство, которое можно использовать вместо него:

```xaml
<VisualStateGroup Name="CommonStates">
```

`x:Name`В одном элементе можно использовать один или, `Name` но не оба.

`VisualStateGroup`Класс определяет свойство с именем [`States`](xref:Xamarin.Forms.VisualStateGroup.States) , которое представляет собой коллекцию [`VisualState`](xref:Xamarin.Forms.VisualState) объектов. `States` — Это _свойство Content_ , которое позволяет `VisualStateGroups` включать `VisualState` теги непосредственно между `VisualStateGroup` тегами. (Свойства содержимого обсуждаются в статье [важный синтаксис XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties).)

Следующим шагом является включение пары тегов для каждого визуального состояния в этой группе. Их также можно определить с помощью `x:Name` или `Name` :

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState` Определяет свойство с именем [`Setters`](xref:Xamarin.Forms.VisualState.Setters) , которое является коллекцией [`Setter`](xref:Xamarin.Forms.Setter) объектов. Это те же `Setter` объекты, которые используются в [`Style`](xref:Xamarin.Forms.Style) объекте.

`Setters`_не_ является свойством Content объекта `VisualState` , поэтому необходимо включить теги элементов свойства для `Setters` Свойства:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Теперь можно вставить один или несколько `Setter` объектов между каждой парой `Setters` тегов. Ниже приведены `Setter` объекты, определяющие визуальные состояния, описанные выше.

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Каждый `Setter` тег указывает значение конкретного свойства, когда это состояние является текущим. Любое свойство, на которое ссылается `Setter` объект, должно поддерживаться связываемым свойством.

Разметка, аналогичная этой, является основанием страницы **VSM on View** в образце программы **[всмдемос](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** . Страница содержит три `Entry` представления, но к ней присоединена разметка VSM только во второй.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />
        <Entry />
        <Label Text="Entry with VSM: " />
        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>
        <Label Text="Entry to enable 2nd Entry:" />
        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Обратите внимание, что вторая `Entry` также содержит `DataTrigger` как часть своей `Trigger` коллекции. Это приводит к `Entry` отключению, пока что что-либо не будет введено в третий `Entry` . Ниже приведена страница при запуске в iOS, Android и универсальная платформа Windows (UWP):

[![VSM в представлении: отключено](vsm-images/VsmOnViewDisabled.png "VSM при просмотре — отключено")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Текущее визуальное состояние — "отключено", а фон второго `Entry` — розовый на экранах iOS и Android. Реализация UWP не `Entry` позволяет задать цвет фона при `Entry` отключении.

При вводе какого-либо текста в третьем `Entry` , второй `Entry` переключается в нормальное состояние, а фон теперь заменяется на:

[![VSM в представлении: обычная](vsm-images/VsmOnViewNormal.png "VSM в представлении — обычная")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Когда вы намерены коснуться второго `Entry` , он получает фокус ввода. Он переключается в состояние "назначено" и увеличивает его высоту вдвое:

[![VSM в представлении: с упором](vsm-images/VsmOnViewFocused.png "VSM для просмотра — ориентированный на представление")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Обратите внимание, что объект не `Entry` удерживает травяной фон при получении фокуса ввода. Когда диспетчер визуального состояния переключается между визуальными состояниями, свойства, заданные предыдущим состоянием, задаются неопределенными. Помните, что визуальные состояния являются взаимоисключающими. "Нормальное" состояние не означает, что `Entry` включено. Это означает, что параметр `Entry` включен и не имеет фокуса ввода.

Если вы хотите, `Entry` чтобы элемент имел травяное изображение в состоянии "назначено", добавьте другое `Setter` в это визуальное состояние:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

Чтобы эти `Setter` объекты работали правильно, объект `VisualStateGroup` должен содержать `VisualState` объекты для всех состояний в этой группе. Если имеется визуальное состояние, в котором нет `Setter` объектов, включите его в все равно как пустой тег:

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>Разметка диспетчера визуального состояния в стиле

Часто возникает необходимость совместно использовать ту же разметку диспетчера визуального состояния для двух или более представлений. В этом случае необходимо поместить разметку в `Style` Определение.

Вот существующая неявная `Style` для `Entry` элементов на странице **представления VSM on** :

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

Добавьте `Setter` теги для `VisualStateManager.VisualStateGroups` присоединенного свойства BIND:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

Свойство Content для `Setter` равно `Value` , поэтому значение `Value` свойства можно указать непосредственно в этих тегах. Это свойство имеет тип `VisualStateGroupList` :

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style>
```

Внутри этих тегов можно включить один или несколько `VisualStateGroup` объектов:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

Оставшаяся часть разметки VSM та же, что и раньше.

Ниже приведена страница **VSM в стиле** , на которой показана полная разметка VSM:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />
        <Entry />
        <Label Text="Entry with VSM: " />
        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>
        <Label Text="Entry to enable 2nd Entry:" />
        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Теперь все `Entry` представления на этой странице реагируют так же, как и их визуальные состояния. Кроме того, обратите внимание, что состояние "Фокус" теперь включает секунду `Setter` , которая дает каждому `Entry` травяному фону также, когда он имеет фокус ввода:

[![VSM в стиле](vsm-images/VsmInStyle.png "VSM в стиле")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="visual-states-in-no-locxamarinforms"></a>Визуальные состояния в Xamarin.Forms

В следующей таблице перечислены визуальные состояния, определенные в Xamarin.Forms .

| Class | Состояния | Дополнительные сведения |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [Визуальные состояния кнопки](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CheckBox` | `IsChecked` | [Визуальные состояния флажка](~/xamarin-forms/user-interface/checkbox.md#checkbox-visual-states) |
| `CarouselView` | `DefaultItem`, `CurrentItem`, `PreviousItem`, `NextItem` | [Визуальные состояния Карауселвиев](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `ImageButton` | `Pressed` | [Визуальные состояния ImageButton](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `RadioButton` | `IsChecked` | [Визуальные состояния RadioButton](~/xamarin-forms/user-interface/radiobutton.md#radiobutton-visual-states) |
| `Switch` | `On`, `Off` | [Переключение визуальных состояний](~/xamarin-forms/user-interface/switch.md#switch-visual-states) |
| `VisualElement` | `Normal`, `Disabled`, `Focused`, `Selected` | [Распространенные состояния](#common-states) |

Доступ к каждому из этих состояний можно получить с помощью группы визуального состояния с именем `CommonStates` .

Кроме того, `CollectionView` класс реализует `Selected` состояние. Дополнительные сведения см. в разделе [изменение цвета выбранного элемента](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color).

## <a name="set-state-on-multiple-elements"></a>Задание состояния для нескольких элементов

В предыдущих примерах визуальные состояния были присоединены к отдельным элементам и работали с ними. Однако можно также создать визуальные состояния, присоединенные к одному элементу, но задавайте свойства других элементов в той же области. Это позволяет избежать повторения визуальных состояний для каждого элемента, над которым работают состояния.

[`Setter`](xref:Xamarin.Forms.Setter)Тип имеет `TargetName` свойство типа, `string` представляющее целевой элемент, который `Setter` будет управлять визуальным состоянием. Если `TargetName` свойство определено, задает для `Setter` элемента, `Property` определенного в `TargetName` `Value` :

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

В этом примере `Label` свойство с именем `label` будет иметь `TextColor` значение `Red` . При задании `TargetName` свойства необходимо указать полный путь к свойству в `Property` . Таким образом, чтобы задать `TextColor` свойство для `Label` , `Property` указывается как `Label.TextColor` .

> [!NOTE]
> Любое свойство, на которое ссылается `Setter` объект, должно поддерживаться связываемым свойством.

Страница **VSM with Setter (TargetName** ) в образце **[всмдемос](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** показывает, как задать состояние для нескольких элементов из одной группы состояний визуализации. XAML-файл состоит из элемента `StackLayout` , содержащего `Label` элемент, `Entry` и `Button` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmSetterTargetNamePage"
             Title="VSM with Setter TargetName">
    <StackLayout Margin="10">
        <Label Text="What is the capital of France?" />
        <Entry x:Name="entry"
               Placeholder="Enter answer" />
        <Button Text="Reveal answer">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal" />
                    <VisualState x:Name="Pressed">
                        <VisualState.Setters>
                            <Setter Property="Scale"
                                    Value="0.8" />
                            <Setter TargetName="entry"
                                    Property="Entry.Text"
                                    Value="Paris" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

Разметка VSM прикрепляется к `StackLayout` . Существует два взаимоисключающих состояния с именами "нормально" и "нажато", с каждым состоянием, содержащим `VisualState` теги.

"Нормальное" состояние активно `Button` , если не нажата кнопка, и можно указать ответ на вопрос:

[![Метод задания VSM TargetName: нормальное состояние](vsm-images/VsmSetterTargetNameNormal.png "Метод задания VSM, TargetName — нормальный")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

"Нажатое" состояние становится активным при `Button` нажатии кнопки:

[![Метод задания VSM, TargetName: нажатое состояние](vsm-images/VsmSetterTargetNamePressed.png "Метод задания VSM, нажатие TargetName")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

"Нажато" `VisualState` указывает, что при `Button` нажатии элемента его `Scale` свойство будет изменено со значения по умолчанию от 1 до 0,8. Кроме того, `Entry` свойство с именем `entry` будет иметь `Text` значение Париж. Таким образом, при нажатии на эту `Button` клавишу происходит масштабирование, которое немного меньше, а `Entry` отображает Париж. Затем, когда объект `Button` будет освобожден, он восмасштабируется до значения по умолчанию 1, а на `Entry` экран будет выведен любой введенный ранее текст.

> [!IMPORTANT]
> Пути к свойствам в настоящее время не поддерживаются в `Setter` элементах, которые задают `TargetName` свойство.

## <a name="define-your-own-visual-states"></a>Определение собственных визуальных состояний

Каждый класс, производный от, `VisualElement` поддерживает общие состояния "обычный", "Focused" и "Disabled". Кроме того, `CollectionView` класс поддерживает состояние "выбрано". На внутреннем уровне [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) класс обнаруживает, когда он включается или отключается, или фокус, и вызывает статический [ `VisualStateManager.GoToState` ] (xref: Xamarin.Forms . VisualStateManager. статический GoToState ( Xamarin.Forms . Висуалелемент, System. String)):

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Это единственный код диспетчера визуального состояния, который можно найти в `VisualElement` классе. Поскольку `GoToState` вызывается для каждого объекта, основанного на каждом классе, производном от `VisualElement` , можно использовать Диспетчер визуальных состояний с любым `VisualElement` объектом для реагирования на эти изменения.

Интересно, что имя группы визуальных состояний «Коммонстатес» не упоминается явно в `VisualElement` . Имя группы не входит в API для диспетчера визуальных состояний. В одном из двух примеров программы, показанных на данный момент, можно изменить имя группы с «Коммонстатес» на любое другое, и программа по-прежнему будет работать. Имя группы — это просто общее описание состояний в этой группе. Неявное понимание того, что визуальные состояния в любой группе являются взаимоисключающими: одно состояние и только одно состояние является текущим в любое время.

Если вы хотите реализовать собственные визуальные состояния, необходимо вызвать `VisualStateManager.GoToState` из кода. Чаще всего этот вызов будет осуществляться из файла кода программной части класса Page.

На странице **проверки VSM** в образце **[всмдемос](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** показано, как использовать Диспетчер визуальных состояний в связи с проверкой входных данных. XAML-файл состоит из, `StackLayout` содержащего два `Label` элемента `Entry` :, и `Button` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout x:Name="stackLayout"
                 Padding="10, 10">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter TargetName="helpLabel"
                                    Property="Label.TextColor"
                                    Value="Transparent" />
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Pink" />
                            <Setter TargetName="submitButton"
                                    Property="Button.IsEnabled"
                                    Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />
        <Entry x:Name="entry"
               Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />
        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1" />
        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center" />
    </StackLayout>
</ContentPage>
```

Разметка VSM прикрепляется к `StackLayout` (с именем `stackLayout` ). Существует два взаимоисключающих состояния с именами "Valid" и "Invalid" с каждым состоянием, содержащим `VisualState` теги.

Если параметр не `Entry` содержит допустимый номер телефона, то текущее состояние является "недопустимым" и, следовательно, `Entry` имеет розовый фон, второй `Label` видим и `Button` отключен:

[![Проверка VSM: недопустимое состояние](vsm-images/VsmValidationInvalid.png "Проверка VSM-недопустимо")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

При указании допустимого номера телефона текущее состояние становится допустимым. `Entry`Получает травяной фон, второй `Label` исчезает и `Button` теперь включается:

[![Проверка VSM: допустимое состояние](vsm-images/VsmValidationValid.png "Проверка VSM — допустимая")](vsm-images/VsmValidationValid-Large.png#lightbox)

Файл кода программной части отвечает за обработку `TextChanged` события из `Entry` . Обработчик использует регулярное выражение для определения допустимости входной строки. Метод в файле кода программной части называется `GoToState` вызов статического `VisualStateManager.GoToState` метода для `stackLayout` :

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage()
    {
        InitializeComponent();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(stackLayout, visualState);
    }
}
```

Кроме того, обратите внимание, что `GoToState` метод вызывается из конструктора для инициализации состояния. Всегда должно быть текущее состояние. Но нигде в коде нет какой-либо ссылки на имя группы визуальных состояний, хотя для ясности на него имеется ссылка в XAML как «Валидатионстатес».

Обратите внимание, что файл кода программной части должен иметь только учетную запись объекта на странице, определяющей визуальные состояния, и вызывать `VisualStateManager.GoToState` для этого объекта. Это обусловлено тем, что оба визуальных состояния нацелены на несколько объектов на странице.

Может возникнуть вопрос: Если файл кода программной части должен ссылаться на объект на странице, где определены визуальные состояния, то почему файл кода программной части просто напрямую обращается к этому и другим объектам? Конечно, это может быть. Однако преимущество использования VSM заключается в том, что вы можете контролировать, как визуальные элементы реагируют на разные состояния в XAML, что позволяет хранить всю структуру пользовательского интерфейса в одном расположении. Это позволяет избежать настройки визуального отображения путем доступа к визуальным элементам непосредственно из кода программной части.

## <a name="visual-state-triggers"></a>Триггеры визуального состояния

Визуальные состояния поддерживают триггеры состояния, которые являются специализированной группой триггеров, определяющих условия, при которых [`VisualState`](xref:Xamarin.Forms.VisualState) следует применять.

Триггеры состояния добавляются в коллекцию [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) [`VisualState`](xref:Xamarin.Forms.VisualState). Эта коллекция может содержать один или несколько триггеров состояния. При наличии активных триггеров состояния в коллекции будет применяться [`VisualState`](xref:Xamarin.Forms.VisualState).

При использовании триггеров состояния для управления визуальными состояниями Xamarin.Forms применяет следующие правила приоритета, чтобы определить, какой триггер (и соответственно [`VisualState`](xref:Xamarin.Forms.VisualState)) будет активен:

1. Любой триггер, производный от [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase).
1. [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) активируется из-за выполнения условия [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth).
1. [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) активируется из-за выполнения условия [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight).

Если одновременно активны несколько триггеров (например, два пользовательских триггера), то у первого триггера, объявленного в разметке, будет приоритет.

Дополнительные сведения о триггерах состояния см. в разделе [Триггеры состояния](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers).

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>Использование диспетчера визуальных состояний для адаптивного макета

Xamarin.FormsПриложение, работающее на телефоне, обычно может быть просмотрено в книжной или альбомной пропорции, и Xamarin.Forms для программы, выполняющейся на рабочем столе, можно изменить размер, чтобы иметь много различных размеров и пропорций. Хорошо спроектированное приложение может отображать свое содержимое по-разному для различных форм и конструктивных параметров формы.

Этот метод иногда называют _адаптивным макетом_. Поскольку адаптивный макет включает только визуальные элементы программы, это идеальное приложение диспетчера визуального состояния.

Простой пример — это приложение, которое отображает небольшую коллекцию кнопок, влияющих на содержимое приложения. В портретном режиме эти кнопки могут отображаться в горизонтальной строке в верхней части страницы:

[![Адаптивный макет VSM: Книжная](vsm-images/VsmAdaptiveLayoutPortrait.png "Адаптивный макет VSM — книжная ориентация")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

В альбомном режиме массив кнопок может быть перемещен на одну сторону и отображаться в столбце:

[![Адаптивный макет VSM: альбомная ориентация](vsm-images/VsmAdaptiveLayoutLandscape.png "Адаптивный макет VSM — альбомная ориентация")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

В верхней части окна программа работает на универсальная платформа Windows, Android и iOS.

На странице **адаптивного макета VSM** в образце [всмдемос](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) определена группа с именем "ориентатионстатес" с двумя визуальными состояниями "Книжная" и "Альбомная". (Более сложный подход может основываться на нескольких разных ширинах страницы или окна.)

Разметка VSM происходит в четырех местах файла XAML. `StackLayout`Именованный объект `mainStack` содержит как меню, так и содержимое, которое является `Image` элементом. Эта `StackLayout` ориентация должна иметь вертикальную ориентацию в книжной и горизонтальной ориентации в альбомном режиме:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">
                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">
                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">
                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>
                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

Внутренние `ScrollView` именованные `menuScroll` и `StackLayout` именованные `menuStack` реализуют меню кнопок. Ориентация этих макетов противоположна `mainStack` . Меню должно располагаться горизонтально в книжной ориентации и вертикально в альбомном режиме.

Четвертый раздел разметки VSM имеет неявный стиль для самих кнопок. Эти наборы разметки `VerticalOptions` , `HorizontalOptions` и `Margin` свойства, связанные с книжной и альбомной ориентацией.

Файл кода программной части задает `BindingContext` свойство класса `menuStack` для реализации `Button` команд, а также присоединяет обработчик к `SizeChanged` событию страницы:

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

`SizeChanged`Обработчик вызывает `VisualStateManager.GoToState` для двух `StackLayout` `ScrollView` элементов и, а затем просматривает дочерние элементы `menuStack` для вызова `VisualStateManager.GoToState` `Button` элементов.

Может показаться, что файл кода программной части может более точно управлять изменением ориентации, устанавливая свойства элементов в файле XAML, но диспетчер визуального состояния является определенно более структурированным подходом. Все визуальные элементы хранятся в файле XAML, где они становятся проще исследовать, обслуживать и изменять.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Диспетчер визуальных состояний с Xamarin. Университет

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms видео о диспетчере визуальных состояний 3,0**

## <a name="related-links"></a>Связанные ссылки

- [всмдемос](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [Триггеры состояния](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)