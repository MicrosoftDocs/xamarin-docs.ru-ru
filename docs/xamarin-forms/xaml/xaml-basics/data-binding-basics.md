---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 08be571d3ba69891a56c08efd556a999e51431c8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139858"
---
# <a name="part-4-data-binding-basics"></a>Часть 4. Основы привязки данных

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_Привязки данных позволяют связывать свойства двух объектов, чтобы изменение одного из них привело к изменению другого. Это очень ценное средство, и хотя привязки данных могут быть полностью определены в коде, XAML предоставляет ярлыки и удобство. Следовательно, одним из наиболее важных расширений разметки в Xamarin.Forms является привязка._

## <a name="data-bindings"></a>Привязки данных

Привязки данных соединяют свойства двух объектов, называемых *источником* и *целевым объектом*. В коде требуются два шага: `BindingContext` для свойства целевого объекта должен быть задан исходный объект, а `SetBinding` метод (часто используемый в сочетании с `Binding` классом) должен вызываться для целевого объекта, чтобы привязать свойство этого объекта к свойству исходного объекта.

Целевое свойство должно быть связываемым свойством, что означает, что целевой объект должен быть производным от `BindableObject` . В интерактивной Xamarin.Forms документации указываются свойства, которые являются связываемыми. Свойство, `Label` например `Text` , связано с связываемым свойством `TextProperty` .

В разметке также необходимо выполнить те же два шага, которые необходимы в коде, за исключением того, что `Binding` расширение разметки принимает место `SetBinding` вызова и `Binding` класс.

Однако при определении привязок данных в XAML существует несколько способов задать `BindingContext` целевой объект. Иногда он задается из файла кода программной части, иногда с `StaticResource` помощью `x:Static` расширения разметки или, иногда в качестве содержимого `BindingContext` тегов элементов свойств.

Чаще всего привязки используются для соединения визуальных элементов программы с базовой моделью данных, как правило, в реализации архитектуры приложения MVVM (модель-представление-ViewModel), как описано в [части 5. Из привязок данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), но возможны и другие сценарии.

## <a name="view-to-view-bindings"></a>Привязки представлений и представлений

Можно определить привязки данных, чтобы связать свойства двух представлений на одной странице. В этом случае необходимо задать `BindingContext` целевой объект с помощью `x:Reference` расширения разметки.

Ниже приведен XAML-файл, который содержит `Slider` и два `Label` представления, одно из которых поворачивается на `Slider` значение, а другое — для отображения `Slider` значения:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderBindingsPage"
             Title="Slider Bindings Page">

    <StackLayout>
        <Label Text="ROTATION"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider`Содержит `x:Name` атрибут, на который ссылаются два `Label` представления с помощью `x:Reference` расширения разметки.

`x:Reference`Расширение привязки определяет свойство с именем `Name` , которое присваивается имени упоминаемого элемента, в данном случае `slider` . Однако `ReferenceExtension` класс, определяющий `x:Reference` расширение разметки, также определяет `ContentProperty` атрибут для `Name` , что означает, что он не требуется явно. Только для различных, первая `x:Reference` включает "Name =", а вторая — нет:

```xaml
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding`Само расширение разметки может иметь несколько свойств, как `BindingBase` и класс и `Binding` . `ContentProperty`Для `Binding` имеет значение `Path` , но часть "Path =" расширения разметки можно опустить, если путь является первым элементом `Binding` расширения разметки. Первый пример содержит "Path =", но во втором примере он опускается:

```xaml
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Все свойства могут находиться в одной строке или быть разделены на несколько строк:

```xaml
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

Сделайте что-нибудь удобно.

Обратите внимание на `StringFormat` свойство во втором `Binding` расширении разметки. В Xamarin.Forms привязках не выполняет никаких неявных преобразований типов, и если необходимо отобразить нестроковый объект в виде строки, необходимо предоставить преобразователь типов или использовать `StringFormat` . В фоновом режиме `String.Format` для реализации используется статический метод `StringFormat` . Это может быть проблемой, так как спецификации форматирования .NET содержат фигурные скобки, которые также используются для разделения расширений разметки. Это создает риск путаницы с синтаксическим анализатором XAML. Чтобы избежать этого, заключите всю строку форматирования в одинарные кавычки:

```xaml
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Вот работающая программа:

[![Привязки представлений и представлений](data-binding-basics-images/sliderbinding.png)](data-binding-basics-images/sliderbinding-large.png#lightbox)

## <a name="the-binding-mode"></a>Режим привязки

Одно представление может иметь привязки к данным в нескольких свойствах. Однако каждое представление может иметь только одно `BindingContext` , поэтому несколько привязок данных в этом представлении должны иметь все ссылочные свойства одного и того же объекта.

Решением этой и других проблем `Mode` является свойство, для которого задается член `BindingMode` перечисления:

- `Default`
- `OneWay`— значения передаются из источника в целевой объект
- `OneWayToSource`— значения передаются из целевого объекта в источник
- `TwoWay`— значения передаются обоими способами между исходным и целевым элементами.
- `OneTime`— данные переходят от источника к целевому объекту, но только когда `BindingContext` изменения

В следующей программе показано одно частое использование `OneWayToSource` `TwoWay` режимов привязки и. Четыре `Slider` представления предназначены для управления `Scale` `Rotate` свойствами,, `RotateX` и `RotateY` объекта `Label` . Сначала кажется, что эти четыре свойства `Label` должны быть целями привязки данных, так как каждый из них задается `Slider` . Однако в `BindingContext` `Label` может быть только один объект, и существует четыре разных ползунка.

По этой причине все привязки задаются в обратном направлении: для `BindingContext` каждого из четырех ползунков устанавливается значение `Label` , а привязки устанавливаются для `Value` свойств ползунков. С помощью `OneWayToSource` режимов and `TwoWay` эти `Value` свойства могут задавать исходные свойства, которые являются `Scale` свойствами,, `Rotate` `RotateX` и `RotateY` объекта `Label` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderTransformsPage"
             Padding="5"
             Title="Slider Transforms Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <!-- Scaled and rotated Label -->
        <Label x:Name="label"
               Text="TEXT"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <!-- Slider and identifying Label for Scale -->
        <Slider x:Name="scaleSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="1" Grid.Column="0"
                Maximum="10"
                Value="{Binding Scale, Mode=TwoWay}" />

        <Label BindingContext="{x:Reference scaleSlider}"
               Text="{Binding Value, StringFormat='Scale = {0:F1}'}"
               Grid.Row="1" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for Rotation -->
        <Slider x:Name="rotationSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="2" Grid.Column="0"
                Maximum="360"
                Value="{Binding Rotation, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationSlider}"
               Text="{Binding Value, StringFormat='Rotation = {0:F0}'}"
               Grid.Row="2" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationX -->
        <Slider x:Name="rotationXSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="3" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationX, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationXSlider}"
               Text="{Binding Value, StringFormat='RotationX = {0:F0}'}"
               Grid.Row="3" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationY -->
        <Slider x:Name="rotationYSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="4" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationY, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationYSlider}"
               Text="{Binding Value, StringFormat='RotationY = {0:F0}'}"
               Grid.Row="4" Grid.Column="1"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Привязки в трех `Slider` представлениях имеют `OneWayToSource` значение, что означает, что оно `Slider` приводит к изменению свойства объекта `BindingContext` , который является `Label` именованным `label` . Эти три `Slider` представления вызывают изменения `Rotate` `RotateX` свойств, и `RotateY` объекта `Label` .

Однако привязка для `Scale` свойства — `TwoWay` . Это связано с тем, что `Scale` свойство имеет значение по умолчанию 1, а использование `TwoWay` привязки приводит к тому, что `Slider` начальное значение задается как 1, а не 0. Если эта привязка была `OneWayToSource` , `Scale` свойство изначально было установлено в значение 0 из `Slider` значения по умолчанию. `Label`Не будет отображаться, и это может вызвать некоторую путаницу с пользователем.

 [![Обратные привязки](data-binding-basics-images/slidertransforms.png)](data-binding-basics-images/slidertransforms-large.png#lightbox)

 > [!NOTE]
 > [`VisualElement`](xref:Xamarin.Forms.VisualElement)Класс также имеет [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) Свойства и [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) , которые масштабируются `VisualElement` на оси x и оси y соответственно.

## <a name="bindings-and-collections"></a>Привязки и коллекции

Ничто не демонстрирует возможности использования XAML и привязок данных лучше, чем шаблон `ListView` .

`ListView`Определяет `ItemsSource` свойство типа `IEnumerable` и отображает элементы в этой коллекции. Эти элементы могут быть объектами любого типа. По умолчанию `ListView` использует `ToString` метод каждого элемента для вывода этого элемента. Иногда это именно то, что вам нужно, но во многих случаях `ToString` возвращает только полное имя класса объекта.

Однако элементы в `ListView` коллекции могут отображаться любым способом с помощью *шаблона*, который включает класс, производный от `Cell` . Шаблон клонируется для каждого элемента в `ListView` , а привязки данных, заданные для шаблона, передаются в отдельные клоны.

Очень часто требуется создать пользовательскую ячейку для этих элементов с помощью `ViewCell` класса. Этот процесс несколько запутанен в коде, но XAML становится очень простым.

В проект Ксамлсамплес входит класс с именем `NamedColor` . Каждый `NamedColor` объект имеет `Name` `FriendlyName` Свойства и типа `string` , а `Color` свойство типа `Color` . Кроме того, `NamedColor` имеет 141 статические поля типа, относящиеся только `Color` для чтения, соответствующие цветам, определенным в Xamarin.Forms `Color` классе. Статический конструктор создает `IEnumerable<NamedColor>` коллекцию, содержащую `NamedColor` объекты, соответствующие этим статическим полям, и присваивает их общедоступному статическому `All` свойству.

Задание для свойства static значения, равное, `NamedColor.All` позволяет `ItemsSource` `ListView` легко использовать `x:Static` расширение разметки:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

Результирующий экран определяет, что элементы действительно имеют тип `XamlSamples.NamedColor` :

[![Привязка к коллекции](data-binding-basics-images/listview1.png)](data-binding-basics-images/listview1-large.png#lightbox)

Это не очень много информации, но `ListView` доступно для прокрутки и выбора.

Чтобы определить шаблон для элементов, необходимо разорвать `ItemTemplate` свойство как элемент свойства и присвоить ему значение `DataTemplate` , которое затем ссылается на `ViewCell` . К `View` свойству можно `ViewCell` определить макет одного или нескольких представлений для отображения каждого элемента. Вот простой пример:

```xaml
<ListView ItemsSource="{x:Static local:NamedColor.All}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.View>
                    <Label Text="{Binding FriendlyName}" />
                </ViewCell.View>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

> [!NOTE]
> Источником привязки для ячеек и потомками ячеек является `ListView.ItemsSource` коллекция.

`Label`Элементу задается `View` свойство объекта `ViewCell` . ( `ViewCell.View` Теги не нужны, поскольку `View` свойство является свойством Content объекта `ViewCell` .) Эта разметка отображает `FriendlyName` свойство каждого `NamedColor` объекта:

[![Привязка к коллекции с помощью DataTemplate](data-binding-basics-images/listview2.png)](data-binding-basics-images/listview2-large.png#lightbox)

Гораздо лучше. Теперь все, что нужно, — изящности шаблон элемента с дополнительными сведениями и фактическим цветом. Для поддержки этого шаблона некоторые значения и объекты были определены в словаре ресурсов страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <OnPlatform x:Key="boxSize"
                        x:TypeArguments="x:Double">
                <On Platform="iOS, Android, UWP" Value="50" />
            </OnPlatform>

            <OnPlatform x:Key="rowHeight"
                        x:TypeArguments="x:Int32">
                <On Platform="iOS, Android, UWP" Value="60" />
            </OnPlatform>

            <local:DoubleToIntConverter x:Key="intConverter" />

        </ResourceDictionary>
    </ContentPage.Resources>

    <ListView ItemsSource="{x:Static local:NamedColor.All}"
              RowHeight="{StaticResource rowHeight}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <StackLayout Padding="5, 5, 0, 5"
                                 Orientation="Horizontal"
                                 Spacing="15">

                        <BoxView WidthRequest="{StaticResource boxSize}"
                                 HeightRequest="{StaticResource boxSize}"
                                 Color="{Binding Color}" />

                        <StackLayout Padding="5, 0, 0, 0"
                                     VerticalOptions="Center">

                            <Label Text="{Binding FriendlyName}"
                                   FontAttributes="Bold"
                                   FontSize="Medium" />

                            <StackLayout Orientation="Horizontal"
                                         Spacing="0">
                                <Label Text="{Binding Color.R,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat='R={0:X2}'}" />

                                <Label Text="{Binding Color.G,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', G={0:X2}'}" />

                                <Label Text="{Binding Color.B,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', B={0:X2}'}" />
                            </StackLayout>
                        </StackLayout>
                    </StackLayout>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Обратите внимание на использование `OnPlatform` для определения размера `BoxView` и высоты `ListView` строк. Хотя значения для всех платформ одинаковы, разметку можно легко адаптировать для других значений, чтобы настроить отображение.

## <a name="binding-value-converters"></a>Преобразователи значений привязки

Предыдущий файл XAML **демонстрации ListView** отображает отдельные `R` `G` свойства, и `B` Xamarin.Forms `Color` структуры. Эти свойства имеют тип `double` и диапазон от 0 до 1. Если нужно отобразить шестнадцатеричные значения, вы не можете просто использовать `StringFormat` с указанием форматирования "x2". Это работает только для целых чисел, а не для `double` значений, а значения должны быть умножены на 255.

Эта небольшая проблема была решена с помощью *преобразователя значений*, также называемого *преобразовательом привязок*. Это класс, реализующий `IValueConverter` интерфейс, то есть у него есть два метода с именами `Convert` и `ConvertBack` . `Convert`Метод вызывается при передаче значения из источника в целевой объект; `ConvertBack` метод вызывается для передачи из целевого объекта в источник `OneWayToSource` или `TwoWay` привязки.

```csharp
using System;
using System.Globalization;
using Xamarin.Forms;

namespace XamlSamples
{
    class DoubleToIntConverter : IValueConverter
    {
        public object Convert(object value, Type targetType,
                              object parameter, CultureInfo culture)
        {
            double multiplier;

            if (!Double.TryParse(parameter as string, out multiplier))
                multiplier = 1;

            return (int)Math.Round(multiplier * (double)value);
        }

        public object ConvertBack(object value, Type targetType,
                                  object parameter, CultureInfo culture)
        {
            double divider;

            if (!Double.TryParse(parameter as string, out divider))
                divider = 1;

            return ((double)(int)value) / divider;
        }
    }
}
```

`ConvertBack`Метод не воспроизводит роль в этой программе, так как привязки являются только одним способом из источника в целевой объект.

Привязка ссылается на преобразователь привязки со `Converter` свойством. Преобразователь привязки может также принимать параметр, заданный `ConverterParameter` свойством. Для некоторой универсальности это то, как указан множитель. Преобразователь привязки проверяет допустимость значения параметра преобразователя `double` .

Экземпляр преобразователя создается в словаре ресурсов, поэтому он может совместно использоваться несколькими привязками:

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

Три привязки к данным ссылаются на этот единственный экземпляр. Обратите внимание, что `Binding` расширение разметки содержит внедренное `StaticResource` расширение разметки:

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

Вот результат:

[![Привязка к коллекции с помощью шаблона DataTemplate и преобразователей](data-binding-basics-images/listview3.png)](data-binding-basics-images/listview3-large.png#lightbox)

Он `ListView` довольно сложен в обработке изменений, которые могут динамически происходить в базовых данных, но только в том случае, если выполняются определенные действия. Если коллекция элементов, назначенных `ItemsSource` свойству `ListView` изменений во время выполнения, т. е. Если элементы можно добавлять в коллекцию или удалять из нее, используйте `ObservableCollection` для этих элементов класс. `ObservableCollection`реализует `INotifyCollectionChanged` интерфейс и установит `ListView` обработчик для `CollectionChanged` события.

Если свойства самих элементов изменяются во время выполнения, элементы в коллекции должны реализовать `INotifyPropertyChanged` интерфейс и изменить сигналы для значений свойств с помощью `PropertyChanged` события. Это продемонстрировано в следующей части этой серии, [часть 5. Из привязки данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Сводка

Привязки данных предоставляют мощный механизм для связывания свойств между двумя объектами на странице или между визуальными объектами и базовыми данными. Но когда приложение начинает работать с источниками данных, популярное архитектурное шаблон приложения начнет быть полезной парадигмой. Это рассматривается в [части 5. Из привязок данных в MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="related-links"></a>Связанные ссылки

- [ксамлсамплес](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Часть 1. Начало работы с XAML (пример)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Часть 2. Важный синтаксис XAML (пример)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Часть 3. Расширения разметки XAML (пример)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Часть 5. Из привязки данных к MVVM (пример)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
