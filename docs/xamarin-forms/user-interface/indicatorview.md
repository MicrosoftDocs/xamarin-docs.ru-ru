---
title: Xamarin.Forms индикаторвиев
description: Индикаторвиев — это элемент управления, отображающий индикаторы, представляющие количество элементов и текущую позиции в Карауселвиев.
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/24/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 27ee2892a25a86210bc86ad4d329fd1f1ca2c788
ms.sourcegitcommit: 322e7bcf9fb8c1ad52ab8e929bea95d45e280834
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101751420"
---
# <a name="xamarinforms-indicatorview"></a>Xamarin.Forms индикаторвиев

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

[`IndicatorView`](xref:Xamarin.Forms.IndicatorView)— Это элемент управления, отображающий индикаторы, представляющие количество элементов и текущую позиции в [`CarouselView`](xref:Xamarin.Forms.CarouselView) :

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](indicatorview-images/circles.png "Индикаторвиев круги")](indicatorview-images/circles-large.png#lightbox "Индикаторвиев круги")

[`IndicatorView`](xref:Xamarin.Forms.IndicatorView) определяет следующие свойства:

- `Count`, типа `int` , количество индикаторов.
- `HideSingle`Тип `bool` — указывает, должен ли индикатор быть скрытым, если существует только один. Значение по умолчанию — `true`.
- `IndicatorColor`Тип `Color` — Цвет индикаторов.
- `IndicatorSize`Тип — `double` Размер индикаторов. Значение по умолчанию — 6,0.
- `IndicatorLayout`Тип `Layout<View>` определяет класс макета, используемый для визуализации [`IndicatorView`](xref:Xamarin.Forms.IndicatorView) . Это свойство задается параметром Xamarin.Forms и обычно не требуется задавать разработчиками.
- `IndicatorTemplate`Тип `DataTemplate` — шаблон, определяющий внешний вид каждого индикатора.
- `IndicatorsShape`Тип `IndicatorShape` — Форма каждого индикатора.
- `ItemsSource`Тип `IEnumerable` — коллекция, для которой будут отображаться индикаторы. Это свойство будет автоматически установлено при `CarouselView.IndicatorView` установке свойства.
- `MaximumVisible`, типа `int` , максимальное количество видимых индикаторов. Значение по умолчанию — `int.MaxValue`.
- `Position`Тип `int` — текущий выбранный индекс индикатора. Это свойство использует `TwoWay` привязку. Это свойство будет автоматически установлено при `CarouselView.IndicatorView` установке свойства.
- `SelectedIndicatorColor`Тип `Color` — Цвет индикатора, представляющий текущий элемент в [`CarouselView`](xref:Xamarin.Forms.CarouselView) .

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

## <a name="create-an-indicatorview"></a>Создание Индикаторвиев

В следующем примере показано, как создать экземпляр [`IndicatorView`](xref:Xamarin.Forms.IndicatorView) в XAML:

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

В этом примере объект отображается [`IndicatorView`](xref:Xamarin.Forms.IndicatorView) под элементом [`CarouselView`](xref:Xamarin.Forms.CarouselView) и с индикатором для каждого элемента в `CarouselView` . `IndicatorView`Заполняется данными путем присвоения `CarouselView.IndicatorView` свойству `IndicatorView` объекта значения. Каждый индикатор является светло-серым кругом, а индикатор, представляющий текущий элемент в, `CarouselView` темно-серый.

> [!IMPORTANT]
> Установка `CarouselView.IndicatorView` свойства приводит `IndicatorView.Position` к привязке свойства к `CarouselView.Position` свойству и `IndicatorView.ItemsSource` привязке свойства к `CarouselView.ItemsSource` свойству.

## <a name="change-indicator-shape"></a>Изменить форму индикатора

[`IndicatorView`](xref:Xamarin.Forms.IndicatorView)Класс имеет `IndicatorsShape` свойство, которое определяет форму индикаторов. Этому свойству может быть присвоено одно из `IndicatorShape` членов перечисления:

- `Circle` Указывает, что фигуры индикаторов будут циклическими. Это значение по умолчанию для свойства `IndicatorView.IndicatorsShape`.
- `Square` Указывает, что фигуры индикаторов будут квадратными.

В следующем примере показана [`IndicatorView`](xref:Xamarin.Forms.IndicatorView) Настройка для использования квадратных индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorsShape="Square"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="change-indicator-size"></a>Изменить размер индикатора

[`IndicatorView`](xref:Xamarin.Forms.IndicatorView)Класс имеет `IndicatorSize` свойство типа `double` , которое определяет размер индикаторов в аппаратно-независимых единицах. Значение этого свойства по умолчанию равно 6,0.

В следующем примере показана [`IndicatorView`](xref:Xamarin.Forms.IndicatorView) Настройка для отображения более крупных индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorSize="18" />
```

## <a name="limit-the-number-of-indicators-displayed"></a>Ограничение числа отображаемых индикаторов

[`IndicatorView`](xref:Xamarin.Forms.IndicatorView)Класс имеет `MaximumVisible` свойство типа `int` , которое определяет максимальное количество видимых индикаторов.

В следующем примере показана [`IndicatorView`](xref:Xamarin.Forms.IndicatorView) Настройка для отображения не более шести индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               MaximumVisible="6" />
```

## <a name="define-indicator-appearance"></a>Определение внешнего вида индикатора

Внешний вид каждого индикатора можно определить, задав `IndicatorView.IndicatorTemplate` для свойства значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   Margin="0,0,0,40"
                   IndicatorColor="Transparent"
                   SelectedIndicatorColor="Transparent"
                   HorizontalOptions="Center">
        <IndicatorView.IndicatorTemplate>
            <DataTemplate>
                <Label Text="&#xf30c;"
                       FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}" />
            </DataTemplate>
        </IndicatorView.IndicatorTemplate>
    </IndicatorView>
</StackLayout>
```

Элементы, указанные в параметре, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяют внешний вид каждого индикатора. В этом примере каждый индикатор — это [`Label`](xref:Xamarin.Forms.Label) , отображающий значок шрифта.

На следующих снимках экрана показаны индикаторы, отображаемые с помощью значка шрифта:

[![Снимок экрана с шаблоном Индикаторвиев в iOS и Android](indicatorview-images/templated.png "Шаблонные Индикаторвиев")](indicatorview-images/templated-large.png#lightbox "Шаблонные Индикаторвиев")

## <a name="set-visual-states"></a>Задание визуальных состояний

[`IndicatorView`](xref:Xamarin.Forms.IndicatorView) имеет `Selected` визуальное состояние, которое можно использовать для инициации визуального изменения индикатора для текущей должности в `IndicatorView` . Распространенным вариантом использования этого [`VisualState`](xref:Xamarin.Forms.VisualState) метода является изменение цвета индикатора, представляющего текущую точку.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style x:Key="IndicatorLabelStyle"
               TargetType="Label">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="LightGray" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="Selected">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Black" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>

    <StackLayout>
        ...
        <IndicatorView x:Name="indicatorView"
                       Margin="0,0,0,40"
                       IndicatorColor="Transparent"
                       SelectedIndicatorColor="Transparent"
                       HorizontalOptions="Center">
            <IndicatorView.IndicatorTemplate>
                <DataTemplate>
                    <Label Text="&#xf30c;"
                           FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}"
                           Style="{StaticResource IndicatorLabelStyle}" />
                </DataTemplate>
            </IndicatorView.IndicatorTemplate>
        </IndicatorView>
    </StackLayout>
</ContentPage>
```

В этом примере `Selected` визуальное состояние указывает, что индикатор, представляющий текущую точку, будет иметь [`TextColor`](xref:Xamarin.Forms.Label.TextColor) черный цвет. В противном случае значение `TextColor` индикатора будет светло-серым:

![Снимок экрана: состояние выбранного визуального элемента Индикаторвиев](indicatorview-images/visual-state.png)

Дополнительные сведения о визуальных состояниях см. в статье [Диспетчер визуального представления состояний Xamarin.Forms](visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [Индикаторвиев (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [Xamarin.Forms карауселвиев](~/xamarin-forms/user-interface/carouselview/index.md)
- [Диспетчер визуального представления состояний Xamarin.Forms](visual-state-manager.md)
