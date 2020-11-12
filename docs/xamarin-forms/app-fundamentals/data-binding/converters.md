---
title: Преобразователи значений привязки Xamarin.Forms
description: В этой статье объясняется, как выполнить приведение или преобразование значений в пределах привязки данных Xamarin.Forms путем реализации преобразователя величин (который также называется преобразователем привязки или преобразователем значений привязки).
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7a2d7251f3896994ab75aba07c2fb03af6558c4e
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370927"
---
# <a name="no-locxamarinforms-binding-value-converters"></a>Преобразователи значений привязки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/databindingdemos)

Привязки данных обычно передают данные из исходного свойства в целевое свойство (а иногда из целевого свойства в исходное). Эта передача проста в том случае, когда исходные и целевые свойства относятся к одному типу или когда один тип может быть преобразован в другой тип путем неявного преобразования. Если это не так, должно выполняться преобразование типов.

В статье, посвященной [**форматированию строк**](string-formatting.md), было показано, как можно использовать свойство `StringFormat` привязки данных для преобразования любого типа в строку. Для других типов преобразований необходимо написать специальный код в классе, реализующем интерфейс [`IValueConverter`](xref:Xamarin.Forms.IValueConverter). (Универсальная платформа Windows содержит аналогичный класс с именем [`IValueConverter`](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) в пространстве имен `Windows.UI.Xaml.Data`, однако этот `IValueConverter` находится в пространстве имен `Xamarin.Forms`.) Классы, реализующие `IValueConverter`, называются *преобразователями величин* , а также *преобразователями привязки* или *преобразователями значений привязки*.

## <a name="the-ivalueconverter-interface"></a>Интерфейс IValueConverter

Предположим, что вы хотите определить привязку данных, в которой исходное свойство имеет тип `int`, а целевым свойством является `bool`. Требуется, чтобы эта привязка данных создавала значение `false`, если целочисленный источник равняется нулю, и значение `true` в противном случае.  

Это можно сделать с помощью класса, реализующего интерфейс `IValueConverter`.

```csharp
public class IntToBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value != 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? 1 : 0;
    }
}
```

Вы можете задать экземпляр этого класса для свойства [`Converter`](xref:Xamarin.Forms.Binding.Converter) класса `Binding` или свойства [`Converter`](xref:Xamarin.Forms.Xaml.BindingExtension.Converter) расширения разметки `Binding`. Этот класс становится частью привязки данных.

Метод `Convert` вызывается при перемещении данных из источника в целевое свойство в привязках `OneWay` или `TwoWay`. Параметр `value` — это объект или значение из источника привязки данных. Метод должен возвращать значение типа целевого свойства привязки данных. Показанный здесь метод приводит параметр `value` к `int`, а затем сравнивает их с нулем для получения логического (`bool`) возвращаемого значения.

Метод `ConvertBack` вызывается, когда данные перемещаются из целевого в исходное свойство в привязках `TwoWay` или `OneWayToSource`. `ConvertBack` выполняет обратное преобразование. Предполагается, что параметр `value` является `bool` из целевого свойства, и выполняется его преобразование в возвращаемое значение `int` для исходного свойства.

Если привязка данных также содержит параметр `StringFormat`, вызывается преобразователь величин, прежде чем результат форматируется как строка.

Страница **Включение кнопок** в примере [**Демонстрации привязки данных**](/samples/xamarin/xamarin-forms-samples/databindingdemos) демонстрирует использование этого преобразователя в привязке данных. Экземпляр `IntToBoolConverter` создается в словаре ресурсов страницы. Затем он указывается с помощью расширения разметки `StaticResource`, чтобы задать свойство в двух привязках данных `Converter`. Совместное использование преобразователей данных несколькими привязками данных на странице широко распространено:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.EnableButtonsPage"
             Title="Enable Buttons">
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:IntToBoolConverter x:Key="intToBool" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <Entry x:Name="entry1"
               Text=""
               Placeholder="enter search term"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Search"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry1},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />

        <Entry x:Name="entry2"
               Text=""
               Placeholder="enter destination"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Submit"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry2},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />
    </StackLayout>
</ContentPage>
```

Если преобразователь величин используется на нескольких страницах приложения, можно создать его экземпляр в словаре ресурсов в файле **App.xaml**.

Страница **Включение кнопок** демонстрирует распространенную потребность, когда `Button` выполняет операцию на основе текста, который пользователь вводит в представлении `Entry`. Если никакие данные не были введены в `Entry`, `Button` должен быть отключен. Каждый элемент `Button` содержит привязку данных для его свойства `IsEnabled`. Источником привязки данных является свойство `Length` свойства `Text` соответствующего элемента `Entry`. Если это свойство `Length` не возвращает нуль, преобразователь величин возвращает `true` и включается элемент `Button`:

[![Включение кнопок](converters-images/enablebuttons-small.png "Включение кнопок")](converters-images/enablebuttons-large.png#lightbox "Включение кнопок")

Обратите внимание, что свойство `Text` в каждом `Entry` инициализируется в пустую строку. В этом случаесвойство `Text` имеет значение `null` по умолчанию, а данные привязки не будут работать.

Некоторые преобразователи величин созданы специально для конкретных приложений, тогда как другие являются общими. Если вы знаете, что преобразователь величин будет использоваться только в привязках `OneWay`, то метод `ConvertBack` может просто возвращать `null`.

Метод `Convert` в приведенном выше примере неявно предполагает, что аргумент `value` имеет тип `int`, а возвращаемое значение должно быть типа `bool`. Аналогичным образом метод `ConvertBack` предполагает, что аргумент `value` имеет тип `bool`, и возвращает значение `int`. Если это не так, возникнет исключение времени выполнения.

Вы можете написать более общие преобразователи величин, которые принимают несколько разных типов данных. Методы `Convert` и `ConvertBack` могут использовать операторы `as` или `is` с параметром `value` либо вызывать `GetType` для этого параметра, чтобы определить его тип, а затем выполнять соответствующие действия. Ожидаемый тип возвращаемого значения метода задается параметром `targetType`. В некоторых случаях преобразователи величин используются с привязками данных разных целевых типов: преобразователь величин может использовать аргумент `targetType` для выполнения преобразования для правильного типа.

Если выполняемое преобразование отличается для разных языков и региональных параметров, используйте для этой цели параметр `culture`. Аргумент `parameter` для `Convert` и `ConvertBack` рассматривается далее в этой статье.

## <a name="binding-converter-properties"></a>Свойства преобразователя привязки

Классы преобразователей величин могут иметь свойства и универсальные параметры. Этот конкретный преобразователь величин преобразует `bool` из источника в объект типа `T` для целевого свойства:

```csharp
public class BoolToObjectConverter<T> : IValueConverter
{
    public T TrueObject { set; get; }

    public T FalseObject { set; get; }

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? TrueObject : FalseObject;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ((T)value).Equals(TrueObject);
    }
}
```

Страница **Переключение индикаторов** демонстрирует, как он может использоваться для отображения значения представления `Switch`. Хотя, как правило, экземпляры преобразователей величин создаются как ресурсы в словаре ресурсов, на этой странице показан альтернативный вариант: экземпляр каждого преобразователя величин создается между тегами элемента и свойства `Binding.Converter`. `x:TypeArguments` указывает универсальный аргумент, а для `TrueObject` и `FalseObject` задаются объекты этого типа:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SwitchIndicatorsPage"
             Title="Switch Indicators">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>

            <Style TargetType="Switch">
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Subscribe?" />
            <Switch x:Name="switch1" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch1}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Of course!"
                                                         FalseObject="No way!" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Allow popups?" />
            <Switch x:Name="switch2" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Yes"
                                                         FalseObject="No" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
                <Label.TextColor>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Color"
                                                         TrueObject="Green"
                                                         FalseObject="Red" />
                        </Binding.Converter>
                    </Binding>
                </Label.TextColor>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Learn more?" />
            <Switch x:Name="switch3" />
            <Label FontSize="18"
                   VerticalOptions="Center">
                <Label.Style>
                    <Binding Source="{x:Reference switch3}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Style">
                                <local:BoolToObjectConverter.TrueObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Indubitably!" />
                                        <Setter Property="FontAttributes" Value="Italic, Bold" />
                                        <Setter Property="TextColor" Value="Green" />
                                    </Style>                                    
                                </local:BoolToObjectConverter.TrueObject>

                                <local:BoolToObjectConverter.FalseObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Maybe later" />
                                        <Setter Property="FontAttributes" Value="None" />
                                        <Setter Property="TextColor" Value="Red" />
                                    </Style>
                                </local:BoolToObjectConverter.FalseObject>
                            </local:BoolToObjectConverter>
                        </Binding.Converter>
                    </Binding>
                </Label.Style>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

В последних трех парах `Switch` и `Label` универсальный аргумент имеет значение `Style` и все объекты `Style` предоставляются для значений `TrueObject` и `FalseObject`. Они переопределяют неявный стиль для `Label`, заданный в словаре ресурсов, поэтому свойства в этом стиле явно назначаются `Label`. Включение и выключение `Switch` приводит к тому, что изменения отражаются в соответствующем `Label`:

[![Переключение индикаторов](converters-images/switchindicators-small.png "Переключение индикаторов")](converters-images/switchindicators-large.png#lightbox "Переключение индикаторов")

[`Triggers`](~/xamarin-forms/app-fundamentals/triggers.md) можно также использовать для реализации аналогичных изменений в интерфейсе пользователя на основе других представлений.

## <a name="binding-converter-parameters"></a>Параметры преобразователя привязки

Класс `Binding` определяет свойство [`ConverterParameter`](xref:Xamarin.Forms.Binding.ConverterParameter), и расширение разметки `Binding` также определяет свойство [`ConverterParameter`](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter). Если это свойство задано, то значение передается в методы `Convert` и `ConvertBack` как аргумент `parameter`. Даже если экземпляр преобразователя величин совместно используется несколькими привязками данных, параметр `ConverterParameter` может отличаться для выполнения немного отличающихся преобразований.

Использование `ConverterParameter` демонстрируется на примере программы выбора цветов. В этом случае `RgbColorViewModel` имеет три свойства типа `double` с именем `Red`, `Green` и `Blue`, которые используются для создания значения `Color`:

```csharp
public class RgbColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Red
    {
        set
        {
            if (color.R != value)
            {
                Color = new Color(value, color.G, color.B);
            }
        }
        get
        {
            return color.R;
        }
    }

    public double Green
    {
        set
        {
            if (color.G != value)
            {
                Color = new Color(color.R, value, color.B);
            }
        }
        get
        {
            return color.G;
        }
    }

    public double Blue
    {
        set
        {
            if (color.B != value)
            {
                Color = new Color(color.R, color.G, value);
            }
        }
        get
        {
            return color.B;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Red"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Green"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Blue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

Значения свойств `Red`, `Green` и `Blue` задаются в диапазоне от 0 до 1. При этом требуется, чтобы компоненты отображались как шестнадцатеричные значения из двух цифр.

Чтобы отобразить их в виде шестнадцатеричных значений в XAML, они должны быть умножены на 255, преобразованы в целое число, а затем отформатированы со спецификацией X2 в свойстве `StringFormat`. Преобразователь величин может обработать первые две задачи (умножение на 255 и преобразование в целое число). Чтобы сделать преобразователь величин максимально универсальным, можно указать множитель с помощью свойства `ConverterParameter`, которое означает, что он вводит методы `Convert` и `ConvertBack` как аргумент `parameter`:

```csharp
public class DoubleToIntConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)Math.Round((double)value * GetParameter(parameter));
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value / GetParameter(parameter);
    }

    double GetParameter(object parameter)
    {
        if (parameter is double)
            return (double)parameter;

        else if (parameter is int)
            return (int)parameter;

        else if (parameter is string)
            return double.Parse((string)parameter);

        return 1;
    }
}
```

`Convert` преобразует из `double` в `int` с умножением на значение `parameter`; `ConvertBack` делит аргумент `value` целого числа на `parameter` и возвращает результат `double`. (В программе, показанной ниже, преобразователь величин используется только в связи с форматированием строк, поэтому `ConvertBack` не используется.)

Тип аргумента `parameter` может отличаться в зависимости от того, определена ли привязка данных в коде или в XAML. Если свойство `ConverterParameter` объекта `Binding` задается в коде, вполне вероятно, ему будет присвоено числовое значение:

```csharp
binding.ConverterParameter = 255;
```

Свойство `ConverterParameter` имеет тип `Object`, поэтому компилятор C# интерпретирует литерал 255 как целое число и присваивает это значение свойству.

При этом в XAML `ConverterParameter`, вероятнее всего, будет задано следующим образом:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255 выглядит как число, но, поскольку `ConverterParameter` имеет тип `Object`, средство синтаксического анализа XAML обрабатывает 255 как строку.

По этой причине преобразователь величин, показанный выше, включает отдельный метод `GetParameter`, который обрабатывает ситуации, когда `parameter` принадлежит к типу `double`, `int` или `string`.  

Страница **Выбор цвета RGB** создает экземпляр `DoubleToIntConverter` в словаре ресурсов, за которым следует определение двух неявные стилей:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.RgbColorSelectorPage"
             Title="RGB Color Selector">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <local:DoubleToIntConverter x:Key="doubleToInt" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:RgbColorViewModel Color="Gray" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Red}" />
            <Label Text="{Binding Red,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0:X2}'}" />

            <Slider Value="{Binding Green}" />
            <Label Text="{Binding Green,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0:X2}'}" />

            <Slider Value="{Binding Blue}" />
            <Label>
                <Label.Text>
                    <Binding Path="Blue"
                             StringFormat="Blue = {0:X2}"
                             Converter="{StaticResource doubleToInt}">
                        <Binding.ConverterParameter>
                            <x:Double>255</x:Double>
                        </Binding.ConverterParameter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Значения свойств `Red` и `Green` отображаются с помощью расширения разметки `Binding`. Свойство `Blue`, тем не менее, создает экземпляр класса `Binding` для демонстрации способа задания явного значения `double` для свойства `ConverterParameter`.

Ниже приведен результат.

[![Выбор цвета в системе RGB](converters-images/rgbcolorselector-small.png "Выбор цвета в системе RGB")](converters-images/rgbcolorselector-large.png#lightbox "Выбор цвета в системе RGB")

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Глава, посвященная привязкам данных, из книги о Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)