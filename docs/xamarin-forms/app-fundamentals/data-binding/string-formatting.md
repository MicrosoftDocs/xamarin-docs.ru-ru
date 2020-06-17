---
title: "Xamarin.Forms Форматирование строк" description: "Эта статья поясняет, как использовать привязки данных Xamarin.Forms для форматирования и отображения объектов в виде строк. Для этого StringFormat объекта Binding присваивается стандартная строка форматирования .NET с заполнителем".
ms.prod: xamarin ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 01/05/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-string-formatting"></a>Форматирование строк Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

Иногда привязки данных удобно использовать для отображения строкового представления объекта или значения. Например, может потребоваться использовать `Label` для отображения текущего значения `Slider`. В этой привязке данных `Slider` является источником, а целевым объектом является свойство `Text` объекта `Label`.

При отображении строк в коде наиболее эффективным средством является статический метод [`String.Format`](xref:System.String.Format(System.String,System.Object)). Строка форматирования содержит коды форматирования для различных типов объектов и наряду с форматируемыми значениями может содержать другой текст. Дополнительные сведения о форматировании строк см. в разделе [Типы форматирования в .NET](/dotnet/standard/base-types/formatting-types/).

## <a name="the-stringformat-property"></a>Свойство StringFormat

Эта функция переносится в привязки данных. Вы можете задать для свойства [`StringFormat`](xref:Xamarin.Forms.BindingBase.StringFormat) объекта `Binding` (или свойство [`StringFormat`](xref:Xamarin.Forms.Xaml.BindingExtension.StringFormat) расширения разметки `Binding`) стандартную строку форматирования .NET с одним заполнителем:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Обратите внимание, что эта строка форматирования отделена одинарными кавычками (апострофами), чтобы средство синтаксического анализа XAML не распознало фигурные скобки в качестве другого расширения разметки XAML. В противном случае эта строка без символа одинарной кавычки аналогична строке, используемой для отображения значения с плавающей запятой при вызове `String.Format`. Спецификация форматирования `F2` приводит к отображению значения с двумя десятичными разрядами.

Свойство `StringFormat` имеет смысл, только если целевое свойство имеет тип `string`, а режимом привязки является `OneWay` или `TwoWay`. Для двусторонних привязок `StringFormat` применимо только для значений, передаваемых из источника в целевой объект.

Как показано в следующей статье о [пути привязки](binding-path.md), привязки данных могут стать довольно сложными и запутанными. При отладке этих привязок данных можно добавить `Label` в файл XAML с помощью `StringFormat` для отображения некоторых промежуточных результатов. Это может пригодиться, даже если используется только для отображения типа объекта.

Страница **String Formatting** (Форматирование строк) иллюстрирует несколько способов применения свойства `StringFormat`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             x:Class="DataBindingDemos.StringFormattingPage"
             Title="String Formatting">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Blue" />
                <Setter Property="HeightRequest" Value="2" />
                <Setter Property="Margin" Value="0, 5" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <Slider x:Name="slider" />
        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The slider value is {0:F2}'}" />

        <BoxView />

        <TimePicker x:Name="timePicker" />
        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time,
                              StringFormat='The TimeSpan is {0:c}'}" />

        <BoxView />

        <Entry x:Name="entry" />
        <Label Text="{Binding Source={x:Reference entry},
                              Path=Text,
                              StringFormat='The Entry text is &quot;{0}&quot;'}" />

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:DateTime.Now}">
            <Label Text="{Binding}" />
            <Label Text="{Binding Path=Ticks,
                                  StringFormat='{0:N0} ticks since 1/1/1'}" />
            <Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
            <Label Text="{Binding StringFormat='The long date is {0:D}'}" />
        </StackLayout>

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:Math.PI}">
            <Label Text="{Binding}" />
            <Label Text="{Binding StringFormat='PI to 4 decimal points = {0:F4}'}" />
            <Label Text="{Binding StringFormat='PI in scientific notation = {0:E7}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Привязки для `Slider` и `TimePicker` демонстрируют использование спецификаций формата, характерных для типов данных `double` и `TimeSpan`. Свойство `StringFormat`, отображающее текст из представления `Entry`, показывает, как указать двойные кавычки в строке форматирования с использованием сущности HTML `&quot;`.

Следующий раздел в файле XAML называется `StackLayout`, и в нем для `BindingContext` задано расширение разметки `x:Static`, ссылающееся на статическое свойство `DateTime.Now`. Первая привязка не имеет свойств:

```xaml
<Label Text="{Binding}" />
```

Это просто отображает значение `DateTime` объекта `BindingContext` с форматированием по умолчанию. Вторая привязка отображает свойство `Ticks` объекта `DateTime`, тогда как две другие привязки отображают само значение `DateTime` с определенным форматированием. Обратите внимание на этот `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Если вам нужно отобразить левую или правую фигурные скобки в строке форматирования, просто используйте их в паре.

В последнем разделе `BindingContext` присваивается значение `Math.PI`, после чего оно отображается с форматированием по умолчанию и двумя разными типами числового форматирования.

Вот работающая программа:

[![Форматирование строк](string-formatting-images/stringformatting-small.png "Форматирование строки")](string-formatting-images/stringformatting-large.png#lightbox "Форматирование строки")

## <a name="viewmodels-and-string-formatting"></a>Модели представления и форматирование строк

При использовании `Label` и `StringFormat` для отображения значения представления, которое также является целевым объектом для модели представления, вы можете определить привязку от представления к `Label` или от модели представления к `Label`. В общем случае второй подход предпочтителен, так как он проверяет, что привязки между представлением и моделью представления работают.

Этот подход показан в примере **улучшенного выбора цвета**, где используется та же модель представления, что и в программе **простого выбора цвета**, представленной в статье [**Режим привязки**](binding-mode.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.BetterColorSelectorPage"
             Title="Better Color Selector">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Теперь существует три пары элементов `Slider` и `Label`, привязанных к одному исходному свойству в объекте `HslColorViewModel`. Единственное отличие заключается в том, что `Label` имеет свойство `StringFormat` для отображения каждого значения `Slider`.

[![Улучшенное средство выбора цвета](string-formatting-images/bettercolorselector-small.png "Улучшенное средство выбора цвета")](string-formatting-images/bettercolorselector-large.png#lightbox "Улучшенное средство выбора цвета")

Вы можете спросить, как отобразить значения RGB (красный, зеленый, синий) в традиционном двухзначном шестнадцатеричном формате. Эти целочисленные значения недоступны непосредственно из структуры `Color`. Одно решение заключается в вычислении целочисленных значений компонентов цветов в модели представления и предоставлении их в качестве свойств. Затем их можно отформатировать с помощью спецификации форматирования `X2`.

Другой подход является более общим. Можно написать *преобразователь значений привязки*, как описано [**здесь**](converters.md).

Тем не менее в следующей статье [**путь привязки**](binding-path.md) рассматривается более подробно, а также показано, как его можно использовать для ссылки на вложенные свойства и элементы в коллекциях.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Глава, посвященная привязкам данных, из книги о Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
