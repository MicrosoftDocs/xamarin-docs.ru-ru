---
title: Путь привязки Xamarin.Forms
description: В этой статье описывается, как использовать привязки данных Xamarin.Forms для доступа к вложенным свойствам и элементам коллекций с помощью свойства Path класса Binding.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4fd8c91ccf18e72c4e5881261637b7f41b2f3c79
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373618"
---
# <a name="no-locxamarinforms-binding-path"></a>Путь привязки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/databindingdemos)

Во всех предыдущих примерах привязки данных в свойстве [`Path`](xref:Xamarin.Forms.Binding.Path) класса `Binding` (или свойстве [`Path`](xref:Xamarin.Forms.Xaml.BindingExtension.Path) расширения разметки `Binding`) указывалось одно свойство. В действительности `Path` может указывать на *вложенное свойство* (свойство свойства) или элемент коллекции.

Например, предположим, что страница содержит объект `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

Свойство `Time` класса `TimePicker` имеет тип `TimeSpan`, но, возможно, вам нужно создать привязку данных, которая ссылается на свойство `TotalSeconds` этого значения `TimeSpan`. Вот эта привязка данных:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

Свойство `Time` имеет тип `TimeSpan`, который имеет свойство `TotalSeconds`. Свойства `Time` и `TotalSeconds` соединяются точкой. Элементы в строке `Path` всегда ссылаются на свойства, а не на их типы.

Этот и еще ряд примеров представлены на странице **Path Variations** (Варианты пути):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=netstandard"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />

        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The second Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

Во втором элементе `Label` источником привязки является сама страница. Свойство `Content` имеет тип `StackLayout`, который имеет свойство `Children` типа `IList<View>`. Этот тип, в свою очередь, имеет свойство `Count`, в котором указывается число дочерних объектов.

## <a name="paths-with-indexers"></a>Пути с индексаторами

Привязка в третьем элементе `Label` на странице **Path Variations** ссылается на класс [`CultureInfo`](xref:System.Globalization.CultureInfo) в пространстве имен `System.Globalization`:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

В качестве источника задано статическое свойство `CultureInfo.CurrentCulture`, представляющее собой объект типа `CultureInfo`. В этом классе определено свойство `DateTimeFormat` типа [`DateTimeFormatInfo`](xref:System.Globalization.DateTimeFormatInfo), содержащее коллекцию `DayNames`. Индекс выбирает четвертый элемент.

Четвертый элемент `Label` выполняет схожие действия, но для французского языка и региональных параметров. Свойству `Source` привязки присваивается объект `CultureInfo` с конструктором:

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

Дополнительные сведения об указании аргументов конструктора в XAML см. в разделе [Передача аргументов конструктора](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments).

Наконец, последний пример аналогичен второму, за тем исключением, что он ссылается на один из дочерних элементов `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Этот дочерний объект представляет собой элемент `Label`, который имеет свойство `Text` типа `String`, имеющего свойство `Length`. Первый элемент `Label` сообщает значение `TimeSpan`, заданное в `TimePicker`, поэтому при изменении текста итоговый элемент `Label` также меняется.

Вот работающая программа:

[![Варианты путей](binding-path-images/pathvariations-small.png "Варианты путей")](binding-path-images/pathvariations-large.png#lightbox "Варианты путей")

## <a name="debugging-complex-paths"></a>Отладка сложных путей

Составлять определения сложных путей может быть непросто. Вам нужно знать тип каждого вложенного свойства или элементов в коллекции, чтобы правильно добавить следующее вложенное свойство. Но сами типы в пути отсутствуют. Один из приемов — формировать путь поэтапно, проверяя промежуточные результаты. В последнем примере можно было начать без определения `Path`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Этот код отображает тип источника привязки (`DataBindingDemos.PathVariationsPage`). Известно, что страница `PathVariationsPage` является производной `ContentPage`, поэтому она имеет свойство `Content`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Теперь видно, что свойство `Content` имеет тип `Xamarin.Forms.StackLayout`. Добавьте свойство `Children` в `Path`, и будет ясно, что его тип — `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`. Это внутренний класс Xamarin.Forms, однако очевидно, что он представляет собой коллекцию. Добавьте индекс. Тип — `Xamarin.Forms.Label`. Продолжайте тем же образом.

Когда Xamarin.Forms обрабатывает путь привязки, для каждого объекта, реализующего интерфейс `PropertyChanged`, в пути устанавливается обработчик `INotifyPropertyChanged`. Например, последняя привязка реагирует на изменение в первом элементе `Label`, так как меняется свойство `Text`.

Если свойство в пути привязки не реализует интерфейс `INotifyPropertyChanged`, изменения этого свойства игнорируются. Некоторые изменения могут сделать путь привязки полностью недействительным, поэтому этот прием следует использовать, только если строка из свойств и вложенных свойств не может стать недействительной.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Глава, посвященная привязкам данных, из книги о Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)