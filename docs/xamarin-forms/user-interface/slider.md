---
title: Xamarin.Forms Группу
description: Xamarin.FormsПолзунок — это горизонтальная линия, которую пользователь может обрабатывать, чтобы выбрать значение типа Double из непрерывного диапазона. В этой статье объясняется, как использовать класс Slider для выбора значения из диапазона непрерывных значений.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 364cda6372986113e8a782a061783e0ca5455f3b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368548"
---
# <a name="no-locxamarinforms-slider"></a>Xamarin.Forms Группу

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)

_Используйте ползунок для выбора из диапазона непрерывных значений._

Xamarin.Forms [`Slider`](xref:Xamarin.Forms.Slider) — Это горизонтальная панель, которой пользователь может управлять, чтобы выбрать `double` значение из непрерывного диапазона.

`Slider`Определяет три свойства типа `double` :

- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) является минимумом диапазона и значением по умолчанию 0.
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) Максимальное значение диапазона со значением по умолчанию 1.
- [`Value`](xref:Xamarin.Forms.Slider.Value) значение ползунка, которое может варьироваться от `Minimum` до и `Maximum` имеет значение по умолчанию 0.

Все три свойства поддерживаются `BindableProperty` объектами. `Value`Свойство имеет режим привязки по умолчанию `BindingMode.TwoWay` , что означает, что он подходит как источник привязки в приложении, использующем архитектуру [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) .

> [!WARNING]
> На внутреннем уровне `Slider` обеспечивает `Minimum` меньшее значение `Maximum` . Если `Minimum` или `Maximum` когда-либо задаются так, что `Minimum` не меньше `Maximum` , возникает исключение. Дополнительные сведения о задании свойств и см. в разделе [**меры предосторожности**](#precautions) ниже `Minimum` `Maximum` .

Объект `Slider` приводит `Value` свойство таким образом, чтобы оно было между `Minimum` и `Maximum` включительно. Если `Minimum` свойству присвоено значение, большее, чем `Value` свойство, `Slider` `Value` свойство устанавливает для значения `Minimum` . Аналогично, если для параметра задано `Maximum` значение меньше `Value` , то `Slider` для свойства задается `Value` `Maximum` .

`Slider` Определяет [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) событие, возникающее при изменении с `Value` помощью пользовательской манипуляции `Slider` или, когда программа задает `Value` свойство напрямую. `ValueChanged`Событие также срабатывает при `Value` приведении свойства, как описано в предыдущем абзаце.

[`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs)Объект, сопровождающий `ValueChanged` событие, имеет два свойства типа `double` : [`OldValue`](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) и [`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue) . Во время срабатывания события значение совпадает `NewValue` со `Value` свойством `Slider` объекта.

`Slider` также определяет `DragStarted` и `DragCompleted` события, которые срабатывают в начале и в конце действия перетаскивания. В отличие от [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) события, `DragStarted` события и `DragCompleted` срабатывают только при манипуляции пользователя с `Slider` . При `DragStarted` срабатывании события выполняется объект `DragStartedCommand` типа `ICommand` . Аналогично, при `DragCompleted` срабатывании события `DragCompletedCommand` выполняется тип, типа `ICommand` .

> [!WARNING]
> Не используйте неограниченные горизонтальные параметры макета `Center` , `Start` или `End` с `Slider` . Как в Android, так и в UWP, элемент `Slider` сворачивается в полоску нулевой длины, а в iOS это очень короткий отрезок. Установите значение по умолчанию `HorizontalOptions` `Fill` и не используйте ширину `Auto` При размещении `Slider` в `Grid` макете.

`Slider`Также определяет несколько свойств, влияющих на его внешний вид:

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty) Цвет столбца в левой части бегунка.
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty) Цвет столбца в правой части бегунка.
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty) цвет ползунка.
- [`ThumbImageSource`](xref:Xamarin.Forms.Slider.ThumbImageSourceProperty) изображение, используемое для Thumb типа [`ImageSource`](xref:Xamarin.Forms.ImageSource) .

> [!NOTE]
> `ThumbColor`Свойства и `ThumbImageSource` являются взаимоисключающими. Если заданы оба свойства, `ThumbImageSource` свойство будет иметь приоритет.

## <a name="basic-slider-code-and-markup"></a>Базовый код и разметка ползунка

Пример [**слидердемос**](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) начинается с трех страниц, которые функционально идентичны, но реализуются различными способами. На первой странице используется только код C#, а во втором используется XAML с обработчиком событий в коде, а третья — это возможность избежать обработчика событий с помощью привязки данных в файле XAML.

### <a name="creating-a-slider-in-code"></a>Создание ползунка в коде

**Базовая кодовая страница Slider** в образце [**слидердемос**](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) показывает, как создать `Slider` и два `Label` объекта в коде:

```csharp
public class BasicSliderCodePage : ContentPage
{
    public BasicSliderCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Slider slider = new Slider
        {
            Maximum = 360
        };
        slider.ValueChanged += (sender, args) =>
        {
            rotationLabel.Rotation = slider.Value;
            displayLabel.Text = String.Format("The Slider value is {0}", args.NewValue);
        };

        Title = "Basic Slider Code";
        Padding = new Thickness(10, 0);
        Content = new StackLayout
        {
            Children =
            {
                rotationLabel,
                slider,
                displayLabel
            }
        };
    }
}
```

`Slider`Инициализируется как свойство со значением `Maximum` 360. `ValueChanged`Обработчик `Slider` использует `Value` свойство `slider` объекта, чтобы установить `Rotation` свойство первого элемента `Label` , и использует `String.Format` метод со `NewValue` свойством аргументов события, чтобы установить `Text` свойство второго `Label` . Эти два подхода к получению текущего значения `Slider` являются взаимозаменяемыми.

Вот программа, выполняемая на устройствах iOS и Android:

[![Базовый код Slider](slider-images/BasicSliderCode.png "Базовый код Slider")](slider-images/BasicSliderCode-Large.png#lightbox)

Во втором `Label` выводится текст "(не инициализировано)" до тех пор `Slider` , пока не будет выполнен манипуляций, что приводит к `ValueChanged` срабатыванию первого события. Обратите внимание, что число отображаемых десятичных разрядов различается для каждой платформы. Эти различия связаны с реализациями платформы `Slider` и обсуждаются далее в этой статье в разделе [различия в реализации платформы](#platform-implementation-differences).

### <a name="creating-a-slider-in-xaml"></a>Создание ползунка в XAML

**Базовая страница XAML с ползунком** функционально аналогична основному **коду Slider** , но реализована в основном в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderXamlPage"
             Title="Basic Slider XAML"
             Padding="10, 0">
    <StackLayout>
        <Label x:Name="rotatingLabel"
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider Maximum="360"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части содержит обработчик `ValueChanged` события:

```csharp
public partial class BasicSliderXamlPage : ContentPage
{
    public BasicSliderXamlPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double value = args.NewValue;
        rotatingLabel.Rotation = value;
        displayLabel.Text = String.Format("The Slider value is {0}", value);
    }
}
```

Кроме того, обработчик событий может получить объект `Slider` , который заактивирует событие с помощью `sender` аргумента. `Value`Свойство содержит текущее значение:

```csharp
double value = ((Slider)sender).Value;
```

Если `Slider` объекту было присвоено имя в XAML-файле с `x:Name` атрибутом (например, "Slider"), то обработчик событий может ссылаться на этот объект напрямую:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Привязка данных ползунок

На странице **привязки на базовых ползунках** показано, как написать почти эквивалентную программу, которая удаляет `Value` обработчик событий с помощью [привязки данных](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderBindingsPage"
             Title="Basic Slider Bindings"
             Padding="10, 0">
    <StackLayout>
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference slider},
                                  Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360" />

        <Label x:Name="displayLabel"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The Slider value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Rotation`Свойство первого объекта `Label` привязано к `Value` свойству `Slider` , а — к свойству `Text` секунды `Label` со `StringFormat` спецификацией. **Основная страница привязок Slider** немного отличается от двух предыдущих страниц: при первом отображении страницы вторая `Label` отображает текстовую строку со значением. Это преимущество использования привязки данных. Чтобы отобразить текст без привязки данных, необходимо специально инициализировать `Text` свойство объекта `Label` или имитировать срабатывание `ValueChanged` события, вызвав обработчик события из конструктора класса.

## <a name="precautions"></a>Меры предосторожности

Значение `Minimum` свойства должно всегда быть меньше значения `Maximum` Свойства. В следующем фрагменте кода вызывается `Slider` исключение:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Компилятор C# создает код, который устанавливает эти два свойства последовательно, а если `Minimum` свойство имеет значение 10, оно больше, чем значение по умолчанию, `Maximum` равное 1. Исключение в этом случае можно избежать, задав `Maximum` свойство в первую очередь:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Значение `Maximum` 20 не является проблемой, так как оно больше, чем значение по умолчанию `Minimum` 0. Если `Minimum` задано, значение меньше, чем `Maximum` значение 20.

Та же проблема существует в XAML. Задайте свойства в порядке, который гарантирует, что `Maximum` всегда будет больше `Minimum` :

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Можно задать `Minimum` `Maximum` значения и для отрицательных чисел, но только в том порядке, где `Minimum` всегда меньше `Maximum` :

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value`Свойство всегда больше или равно `Minimum` значению и меньше или равно `Maximum` . Если для параметра задано `Value` значение за пределами этого диапазона, значение будет преобразовано в диапазон, но исключение не будет создано. Например, этот код *не* вызовет исключение:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Вместо этого `Value` свойство приводится к `Maximum` значению 1.

Ниже приведен фрагмент кода, показанный выше.

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Если параметр `Minimum` имеет значение 10, то `Value` также устанавливается значение 10.

Если `ValueChanged` обработчик событий был присоединен к моменту, когда `Value` свойство приводится к значению, отличному от значения по умолчанию 0, то `ValueChanged` возникает событие. Вот фрагмент кода XAML:

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Если `Minimum` для параметра задано значение 10, `Value` также устанавливается значение 10, и `ValueChanged` возникает событие. Это может произойти до того, как будет создана оставшаяся часть страницы, и обработчик может попытаться сослаться на другие элементы на странице, которые еще не были созданы. Может потребоваться добавить в обработчик некоторый код `ValueChanged` , который проверяет `null` значения других элементов на странице. Или можно задать `ValueChanged` обработчик событий после `Slider` инициализации значений.

## <a name="platform-implementation-differences"></a>Различия в реализации платформы

Отображаемые снимки экрана ранее отображают значение `Slider` с другим числом десятичных точек. Это связано с тем, как `Slider` реализуется на платформах Android и UWP.

### <a name="the-android-implementation"></a>Реализация Android

Реализация Android `Slider` основана на Android [`SeekBar`](xref:Android.Widget.SeekBar) и всегда устанавливает [`Max`](xref:Android.Widget.ProgressBar.Max) свойство в значение 1000. Это означает, что в `Slider` Android есть только 1 001 дискретных значения. Если присвоить параметру значение `Slider` `Minimum` 0 и значение, равное `Maximum` 5000, то при `Slider` управлении этим `Value` свойством значения будут равны 0, 5, 10, 15 и т. д.

### <a name="the-uwp-implementation"></a>Реализация UWP

Реализация UWP `Slider` основана на [`Slider`](/uwp/api/windows.ui.xaml.controls.slider) ЭЛЕМЕНТЕ управления UWP. `StepFrequency`Свойству UWP `Slider` присваивается разность `Maximum` свойств и, `Minimum` деленную на 10, но не больше 1.

Например, для диапазона по умолчанию от 0 до 1 свойство устанавливается `StepFrequency` в значение 0,1. Как и управляется `Slider` , `Value` свойство ограничено 0, 0,1, 0,2, 0,3, 0,4, 0,5, 0,6, 0,7, 0,8, 0,9 и 1,0. (Это очевидно на последней странице примера [**слидердемос**](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) .) Если разница между `Maximum` `Minimum` свойствами и равна 10 или больше, то `StepFrequency` устанавливается в 1, а `Value` свойство имеет целочисленные значения.

### <a name="the-stepslider-solution"></a>Решение Степслидер

Более гибкая часть `StepSlider` обсуждается в [главе 27. Пользовательские модули подготовки](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) книги, *создающие мобильные приложения с Xamarin.Forms помощью*. Объект `StepSlider` аналогичен, `Slider` но добавляет `Steps` свойство, чтобы указать количество значений в диапазоне от `Minimum` до `Maximum` .

## <a name="sliders-for-color-selection"></a>Ползунки для выбора цвета

Последние две страницы в образце [**слидердемос**](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) используют три `Slider` экземпляра для выбора цвета. Первая страница обрабатывает все взаимодействия в файле кода программной части, а вторая страница показывает, как использовать привязку данных с ViewModel.

### <a name="handling-sliders-in-the-code-behind-file"></a>Обработка ползунков в файле кода программной части

На странице **ползунки цвета RGB** создается экземпляр `BoxView` для отображения цвета, трех `Slider` экземпляров для выбора красного, зеленого и синего компонентов цвета, а также три `Label` элемента для отображения этих значений цвета:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.RgbColorSlidersPage"
             Title="RGB Color Sliders">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="Maximum" Value="255" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView x:Name="boxView"
                 Color="Black"
                 VerticalOptions="FillAndExpand" />

        <Slider x:Name="redSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="redLabel" />

        <Slider x:Name="greenSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="greenLabel" />

        <Slider x:Name="blueSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="blueLabel" />
    </StackLayout>
</ContentPage>
```

`Style`Предоставляет все три `Slider` элемента в диапазоне от 0 до 255. `Slider`Элементы используют один и тот же `ValueChanged` обработчик, который реализуется в файле кода программной части:

```csharp
public partial class RgbColorSlidersPage : ContentPage
{
    public RgbColorSlidersPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (sender == redSlider)
        {
            redLabel.Text = String.Format("Red = {0:X2}", (int)args.NewValue);
        }
        else if (sender == greenSlider)
        {
            greenLabel.Text = String.Format("Green = {0:X2}", (int)args.NewValue);
        }
        else if (sender == blueSlider)
        {
            blueLabel.Text = String.Format("Blue = {0:X2}", (int)args.NewValue);
        }

        boxView.Color = Color.FromRgb((int)redSlider.Value,
                                      (int)greenSlider.Value,
                                      (int)blueSlider.Value);
    }
}
```

В первом разделе `Text` свойству одного из экземпляров присваивается `Label` короткая текстовая строка, указывающая значение `Slider` в шестнадцатеричном формате. Затем осуществляется доступ ко всем трем `Slider` экземплярам для создания `Color` значения из компонентов RGB:

[![Ползунки цвета RGB](slider-images/RgbColorSliders.png "Ползунки цвета RGB")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Привязка ползунка к ViewModel

На странице **ползунки цвета HSL** показано, как использовать ViewModel для выполнения вычислений, используемых для создания `Color` значений из оттенков, насыщенности и значений яркости. Как и для всех ViewModels, `HSLColorViewModel` класс реализует `INotifyPropertyChanged` интерфейс и запускает `PropertyChanged` событие при каждом изменении одного из свойств:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));
            }
        }
        get
        {
            return color;
        }
    }
}
```

ViewModels и `INotifyPropertyChanged` Interface обсуждаются в [привязке данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)статьи.

Файл **хслколорслидерспаже. XAML** создает экземпляр `HslColorViewModel` и задает его `BindingContext` свойству страницы. Это позволяет выполнять привязку всех элементов в файле XAML к свойствам в ViewModel:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SliderDemos"
             x:Class="SliderDemos.HslColorSlidersPage"
             Title="HSL Color Sliders">

    <ContentPage.BindingContext>
        <local:HslColorViewModel Color="Chocolate" />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</ContentPage>
```

По мере `Slider` манипуляции с элементами `BoxView` `Label` элементы и обновляются из ViewModel:

[![Ползунки цвета HSL](slider-images/HslColorSliders.png "Ползунки цвета HSL")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat`Компонент `Binding` расширения разметки задается в формате "F2" для вывода двух десятичных разрядов. (Форматирование строк в привязках данных рассматривается в статье [Форматирование строк](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Однако версия программы UWP ограничена значениями 0, 0,1, 0,2,... 0,9 и 1,0. Это прямой результат реализации UWP, `Slider` как описано выше в разделе [различия в реализации платформы](#platform-implementation-differences).

## <a name="related-links"></a>Связанные ссылки

- [Пример демонстрации Slider](/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)
- [API Slider](xref:Xamarin.Forms.Slider)