---
title: Xamarin.Forms индикаторвиев
description: Индикаторвиев — это элемент управления, отображающий индикаторы, представляющие количество элементов и текущую позиции в Карауселвиев.
ms.prod: xamarin
ms.assetId: BBCC223B-4B02-46B7-80BB-EE0E86A67CE2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 95c4e247ca0be6e5f0f39a7bc95c41a73b3f590e
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556793"
---
# <a name="no-locxamarinforms-indicatorview"></a>Xamarin.Forms индикаторвиев

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

`IndicatorView`— Это элемент управления, отображающий индикаторы, представляющие количество элементов и текущую позиции в `CarouselView` :

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](indicatorview-images/circles.png "Индикаторвиев круги")](indicatorview-images/circles-large.png#lightbox "Индикаторвиев круги")

`IndicatorView` определяет следующие свойства:

- `Count`, типа `int` , количество индикаторов.
- `HideSingle`Тип `bool` — указывает, должен ли индикатор быть скрытым, если существует только один. Значение по умолчанию — `true`.
- `IndicatorColor`Тип `Color` — Цвет индикаторов.
- `IndicatorSize`Тип — `double` Размер индикаторов. Значение по умолчанию — 6,0.
- `IndicatorLayout`Тип `Layout<View>` определяет класс макета, используемый для визуализации `IndicatorView` . Это свойство задается параметром Xamarin.Forms и обычно не требуется задавать разработчиками.
- `IndicatorTemplate`Тип `DataTemplate` — шаблон, определяющий внешний вид каждого индикатора.
- `IndicatorsShape`Тип `IndicatorShape` — Форма каждого индикатора.
- `ItemsSource`Тип `IEnumerable` — коллекция, для которой будут отображаться индикаторы. Это свойство будет автоматически установлено при `CarouselView.IndicatorView` установке свойства.
- `MaximumVisible`, типа `int` , максимальное количество видимых индикаторов. Значение по умолчанию — `int.MaxValue`.
- `Position`Тип `int` — текущий выбранный индекс индикатора. Это свойство использует `TwoWay` привязку. Это свойство будет автоматически установлено при `CarouselView.IndicatorView` установке свойства.
- `SelectedIndicatorColor`Тип `Color` — Цвет индикатора, представляющий текущий элемент в `CarouselView` .

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

## <a name="create-an-indicatorview"></a>Создание Индикаторвиев

В следующем примере показано, как создать экземпляр `IndicatorView` в XAML:

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

В этом примере объект отображается `IndicatorView` под элементом `CarouselView` и с индикатором для каждого элемента в `CarouselView` . `IndicatorView`Заполняется данными путем присвоения `CarouselView.IndicatorView` свойству `IndicatorView` объекта значения. Каждый индикатор является светло-серым кругом, а индикатор, представляющий текущий элемент в, `CarouselView` темно-серый.

> [!IMPORTANT]
> Установка `CarouselView.IndicatorView` свойства приводит `IndicatorView.Position` к привязке свойства к `CarouselView.Position` свойству и `IndicatorView.ItemsSource` привязке свойства к `CarouselView.ItemsSource` свойству.

## <a name="change-indicator-shape"></a>Изменить форму индикатора

`IndicatorView`Класс имеет `IndicatorsShape` свойство, которое определяет форму индикаторов. Этому свойству может быть присвоено одно из `IndicatorShape` членов перечисления:

- `Circle` Указывает, что фигуры индикаторов будут циклическими. Это значение по умолчанию для свойства `IndicatorView.IndicatorsShape`.
- `Square` Указывает, что фигуры индикаторов будут квадратными.

В следующем примере показана `IndicatorView` Настройка для использования квадратных индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorsShape="Square"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="change-indicator-size"></a>Изменить размер индикатора

`IndicatorView`Класс имеет `IndicatorSize` свойство типа `double` , которое определяет размер индикаторов в аппаратно-независимых единицах. Значение этого свойства по умолчанию равно 6,0.

В следующем примере показана `IndicatorView` Настройка для отображения более крупных индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorSize="18" />
```

## <a name="limit-the-number-of-indicators-displayed"></a>Ограничение числа отображаемых индикаторов

`IndicatorView`Класс имеет `MaximumVisible` свойство типа `int` , которое определяет максимальное количество видимых индикаторов.

В следующем примере показана `IndicatorView` Настройка для отображения не более шести индикаторов:

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
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="Black"
                   HorizontalOptions="Center">
        <IndicatorView.IndicatorTemplate>
            <DataTemplate>
                <Image Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}" />
            </DataTemplate>
        </IndicatorView.IndicatorTemplate>
    </IndicatorView>
</StackLayout>
```

Элементы, указанные в параметре, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяют внешний вид каждого индикатора. В этом примере каждый индикатор имеет значение [`Image`](xref:Xamarin.Forms.Image) , которое отображает значок шрифта с помощью `FontImage` расширения разметки.

На следующих снимках экрана показаны индикаторы, отображаемые с помощью значка шрифта:

[![Снимок экрана с шаблоном Индикаторвиев в iOS и Android](indicatorview-images/templated.png "Шаблонные Индикаторвиев")](indicatorview-images/templated-large.png#lightbox "Шаблонные Индикаторвиев")

Дополнительные сведения о `FontImage` расширении разметки см. в разделе [расширение разметки фонтимаже](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension).

## <a name="related-links"></a>Связанные ссылки

- [Индикаторвиев (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [Расширение разметки FontImage](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)