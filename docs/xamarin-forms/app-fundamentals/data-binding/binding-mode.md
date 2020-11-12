---
title: Режим привязки Xamarin.Forms
description: В этой статье объясняется, как управлять потоком данных между источником и целевым объектом с помощью режима привязки, который указывается с помощью члена перечисления BindingMode. Каждое привязываемое свойство имеет режим привязки по умолчанию, который указывает режим, действующий, когда свойство является целевым объектом привязки данных.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/01/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1ab0603555c55d969e1bee8ae709df3cac23de73
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373644"
---
# <a name="no-locxamarinforms-binding-mode"></a>Режим привязки Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/databindingdemos)

В [предыдущей статье](basic-bindings.md), в разделах **Альтернативная привязка кода** и **Альтернативная привязка XAML** , объект `Label` со свойством `Scale` привязывался к свойству `Value` объекта `Slider`. Так как начальное значение `Slider` равно 0, свойство `Scale` объекта `Label` также было равно 0, а не 1, поэтому объект `Label` исчезал.

В примере [**DataBindingDemos**](/samples/xamarin/xamarin-forms-samples/databindingdemos) страница **Обратная привязка** аналогична программам в предыдущей статье, только привязка данных определяется для `Slider`, а не `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label"
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

Поначалу может показаться, что все наоборот: теперь `Label` является источником привязки данных, а `Slider` — целевым объектом. Привязка ссылается на свойство `Opacity` объекта `Label`, которое имеет значение по умолчанию 1.

Как можно догадаться, `Slider` инициализируется со значением 1 из начального значения `Opacity` объекта `Label`. Это показано на снимке экрана iOS слева.

[![Обратная привязка](binding-mode-images/reversebinding-small.png "Обратная привязка")](binding-mode-images/reversebinding-large.png#lightbox "Обратная привязка")

Вы удивитесь, но объект `Slider` продолжает работать, как показано на снимках экрана Android. Кажется, что привязка данных работает лучше, когда `Slider` является целевым объектом привязки вместо `Label`, так как инициализация работает, как и ожидалось.

Разница между примером **обратной привязки** и более ранними примерами заключается в *режиме привязки*.

## <a name="the-default-binding-mode"></a>Режим привязки по умолчанию

Режим привязки указывается с помощью члена перечисления [`BindingMode`](xref:Xamarin.Forms.BindingMode):

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) — данные передаются в обе стороны между целевым объектом и источником.
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) — данные передаются от источника к целевому объекту.
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) — данные передаются от целевого объекта к источнику.
- [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; данные передаются от источника к целевому объекту, но только если `BindingContext` меняется (новая возможность в Xamarin.Forms 3.0)

Каждое привязываемое свойство имеет режим привязки по умолчанию, который задается при создании привязываемого свойства и доступен из свойства [`DefaultBindingMode`](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) объекта `BindableProperty`. Этот режим привязки по умолчанию указывает режим, действующий, когда свойство является целевым объектом привязки данных.

Режим привязки по умолчанию для большинства свойств, таких как `Rotation`, `Scale` и `Opacity`, — `OneWay`. Если эти свойства являются целевыми объектами привязки данных, целевое свойство устанавливается из источника.

Но режим привязки по умолчанию для свойства `Value` объекта `Slider` — `TwoWay`. Это означает, что, когда свойство `Value` является целевым объектом привязки данных, целевой объект задается из источника (как обычно), но источник также задается из целевого объекта. Это позволяет задать `Slider` из первоначального значения `Opacity`.

Может показаться, что такая двусторонняя привязка создаст бесконечный цикл, но этого не происходит. Привязываемые свойства не сигнализируют об изменении свойства, пока свойство не меняется. Тем самым предотвращается бесконечный цикл.

### <a name="two-way-bindings"></a>Двусторонние привязки

У большинства привязываемых свойств режим привязки по умолчанию — `OneWay`, но следующие свойства имеют режим привязки по умолчанию `TwoWay`:

- свойство `Date` объекта `DatePicker`;
- свойство `Text` объектов `Editor`, `Entry`, `SearchBar`, и `EntryCell`;
- свойство `IsRefreshing` объекта `ListView`;
- свойство `SelectedItem` объекта `MultiPage`;
- свойства `SelectedIndex` и `SelectedItem` объектов `Picker`;
- свойство `Value` объектов `Slider` и `Stepper`;
- свойство `IsToggled` объекта `Switch`;
- свойство `On` объекта `SwitchCell`;
- свойство `Time` объекта `TimePicker`;

Эти конкретные свойства определяются как `TwoWay` не просто так.

При использовании привязки данных с архитектурой приложения "модель — представление — модель представления" (MVVM) класс ViewModel является источником привязки данных, а класс View, который состоит из таких представлений, как `Slider`, является целевым объектом привязки. Привязки MVVM напоминают пример **обратной привязки** больше, чем привязки в предыдущих примерах. Скорее всего, вы хотите, чтобы каждое представление на странице инициализировалось со значением соответствующего свойства в модели представления, но изменения в представлении также влияли на свойство модели представления.

Свойства с режимом привязки по умолчанию `TwoWay` чаще всего используются в сценариях MVVM.

### <a name="one-way-to-source-bindings"></a>Односторонние привязки к источнику

Привязываемые свойства только для чтения используют режим привязки по умолчанию `OneWayToSource`. Есть только одно привязываемое свойство для чтения и записи с режимом привязки по умолчанию `OneWayToSource`:

- свойство `SelectedItem` объекта `ListView`.

Дело в том, что привязка к свойству `SelectedItem` должна приводить к заданию источника привязки. Пример далее в этой статье переопределяет это поведение.

### <a name="one-time-bindings"></a>Одноразовые привязки

Несколько свойств используют режим привязки по умолчанию `OneTime`, включая свойство `IsTextPredictionEnabled` для `Entry`.

Целевые свойства с режимом привязки `OneTime` обновляются только при изменении контекста привязки. Для привязок к этим целевым свойствам это упрощает инфраструктуру привязки, так как не нужно отслеживать изменения в свойствах источника.

## <a name="viewmodels-and-property-change-notifications"></a>Уведомления об изменении свойства и моделей представления

Страница **Простой селектор цвета** демонстрирует использование простой модели представления. Привязки данных позволяют пользователю выбрать цвет с использованием трех элементов `Slider` для оттенка, насыщенности и яркости.

Модель представления является источником привязки данных. Модель представления *не* определяет привязываемые свойства, но реализует механизм уведомлений, который позволяет инфраструктуре узнавать об изменении значения свойства. Этим механизмом уведомлений является интерфейс [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged), который определяет одно событие с именем [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged). Класс, реализующий этот интерфейс, обычно запускает событие, когда одно из его открытых свойств меняет значение. Событие не инициируется, если свойство никогда не меняется. (Интерфейс `INotifyPropertyChanged` также реализуется объектом `BindableObject`, и событие `PropertyChanged` возникает при изменении значения привязываемого свойства.)

Класс `HslColorViewModel` определяет пять свойств: свойства `Hue`, `Saturation`, `Luminosity` и `Color` взаимосвязаны. Если один из трех компонентов цвета меняет значение, свойство `Color` пересчитывается и события `PropertyChanged` запускаются для всех четырех свойств:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

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

Когда свойство `Color` меняется, статический метод `GetNearestColorName` в классе `NamedColor` (также входит в решение **DataBindingDemos** ) получает ближайший именованный цвет и задает свойство `Name`. Это свойство `Name` имеет закрытый метод доступа `set`, поэтому его нельзя установить за пределами класса.

Если модель представления задана как источник привязки, инфраструктура привязки присоединяет обработчик к событию `PropertyChanged`. Таким образом, привязка получает уведомления об изменениях свойств и может задать целевые свойства на основе измененных значений.

Тем не менее, если целевое свойство (или определение `Binding` для целевого свойства) имеет режим привязки `BindingMode``OneTime`, инфраструктура привязки может не привязывать обработчик для события `PropertyChanged`. Целевое свойство обновляется только тогда, когда `BindingContext` меняется, а не когда меняется само свойство источника.

Файл XAML **Простой селектор цвета** создает экземпляр `HslColorViewModel` в словаре ресурсов страницы и инициализирует свойство `Color`. Свойство `BindingContext` объекта `Grid` устанавливается в расширение привязки `StaticResource` для ссылки на этот ресурс:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel"
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />

            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

`BoxView`, `Label` и три представления `Slider` наследуют контекст привязки из `Grid`. Эти представления являются адресатами (целевыми объектами) привязки, которые ссылаются на свойства источника в модели представления. Для свойства `Color` объекта `BoxView` и свойства `Text` объекта `Label` привязки данных имеют режим `OneWay`: свойства в представлении задаются по свойствам в модели представления.

Но свойство `Value` объекта `Slider` имеет режим `TwoWay`. Поэтому каждый объект `Slider` задается из модели представления, а модель представления задается из каждого объекта `Slider`.

При первом запуске программы `BoxView`, `Label` и три элемента `Slider` получают значения из модели представления в зависимости от первоначального свойства `Color`, заданного при создании экземпляра модели представления. Это показано на снимке экрана iOS слева.

[![Простое средство выбора цвета](binding-mode-images/simplecolorselector-small.png "Простое средство выбора цвета")](binding-mode-images/simplecolorselector-large.png#lightbox "Простое средство выбора цвета")

Когда вы перемещаете ползунки, объекты `BoxView` и `Label` обновляются соответствующим образом, как показано на снимках экрана Android.

Создание экземпляров модели представления в словаре ресурсов является распространенным подходом. Можно также создать экземпляр модели представления в тегах элемента свойства для свойства `BindingContext`. В файле XAML **Простой селектор цвета** попробуйте удалить `HslColorViewModel` из словаря ресурсов и задать его свойством `BindingContext` объекта `Grid` следующим образом.

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

Контекст привязки можно задать различными способами. В некоторых случаях файл с выделенным кодом создает экземпляр модели представления и задает его свойством `BindingContext` страницы. Это все допустимые подходы.

## <a name="overriding-the-binding-mode"></a>Переопределение режима привязки

Если режим привязки целевого свойства по умолчанию не подходит для конкретной привязки данных, можно переопределить его, задав свойство [`Mode`](xref:Xamarin.Forms.BindingBase.Mode) объекта `Binding` (или свойство [`Mode`](xref:Xamarin.Forms.Xaml.BindingExtension.Mode) расширения разметки `Binding`) одному из членов перечисления `BindingMode`.

Однако, если указать для свойства `Mode` значение `TwoWay`, оно не всегда будет работать так, как вы ожидаете. Например, попробуйте изменить файл XAML **Альтернативная привязка XAML** , чтобы включить `TwoWay` в определение привязки:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Можно ожидать, что объект `Slider` будет инициализироваться с начальным значением свойства `Scale`, равным 1, но этого не происходит. Когда привязка `TwoWay` инициализируется, целевой объект сначала получает значение из источника, то есть свойство `Scale` имеет значение объекта `Slider` по умолчанию, равное 0. Когда привязка `TwoWay` устанавливается для объекта `Slider`, объект `Slider` получает первоначальное значение из источника.

Можно задать режим привязки `OneWayToSource` в примере **альтернативной привязки XAML** :

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Теперь `Slider` инициализируется со значением 1 (значение по умолчанию свойства `Scale`), но изменение `Slider` не влияет на свойство `Scale`, так что это не очень полезно.

> [!NOTE]
> Класс [`VisualElement`](xref:Xamarin.Forms.VisualElement) также определяет свойства [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) и [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY), которые могут масштабировать `VisualElement` по-разному в горизонтальном и вертикальном направлениях.

Удобное переопределение режима привязки по умолчанию на `TwoWay` включает в себя свойство `SelectedItem` объекта `ListView`. Режим привязки по умолчанию — `OneWayToSource`. Если привязка данных задана для свойства `SelectedItem` и ссылается на свойство источника в модели представления, то это свойство источника задается из `ListView`. Но в некоторых случаях можно также инициализировать `ListView` из модели представления.

Страница **Примеры параметров** демонстрирует этот подход. Эта страница представляет простую реализацию параметров приложения, которые часто определены в модели представления, например файл `SampleSettingsViewModel`:

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Каждый параметр приложения — это свойство, которое сохраняется в словаре свойств Xamarin.Forms в методе с именем `SaveState` и загружается из этого словаря в конструкторе. В конце класса есть два метода, которые помогают упростить модели представлений и снизить вероятность ошибок в них. Метод `OnPropertyChanged` внизу имеет необязательный параметр, который задается как свойство вызова. Это позволяет избежать орфографических ошибок при указании имени свойства в виде строки.

Метод `SetProperty` в классе выполняет следующее. Он сравнивает значение, которое задается для свойства, со значением, хранящимся в виде поля, и вызывает `OnPropertyChanged`, только когда эти два значения не равны.

Класс `SampleSettingsViewModel` определяет два свойства для цвета фона: свойство `BackgroundNamedColor` имеет тип `NamedColor`, который как класс также входит в решение **DataBindingDemos**. Свойство `BackgroundColor` имеет тип `Color` и получается из свойства `Color` объекта `NamedColor`.

Класс `NamedColor` использует отражение .NET для перечисления всех открытых статических полей в структуре Xamarin.Forms `Color` и их хранения с именами в коллекции, доступной из статического свойства `All`.

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                                (int)(255 * color.R),
                                                (int)(255 * color.G),
                                                (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

Класс `App` в проекте **DataBindingDemos** определяет свойство с именем `Settings` типа `SampleSettingsViewModel`. Это свойство инициализируется, когда создается экземпляр класса `App`, а метод `SaveState` вызывается, когда вызывается метод `OnSleep`:

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

Дополнительные сведения о методах жизненного цикла приложения см. в статье [**Жизненный цикл приложения**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Практически все остальное обрабатывается в файле **SampleSettingsPage.xaml**. Параметр страницы `BindingContext` задается с помощью расширения разметки `Binding`: источник привязки является статическим свойством `Application.Current`, которое является экземпляром класса `App` в проекте, а `Path` задается по свойству `Settings`, которое является объектом `SampleSettingsViewModel`.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Все дочерние элементы страницы наследуют контекст привязки. Большинство других привязок на этой странице устанавливаются для свойств в `SampleSettingsViewModel`. Свойство `BackgroundColor` используется для задания свойства `BackgroundColor` объекта `StackLayout`, а свойства `Entry`, `DatePicker`, `Switch` и `Stepper` привязаны к другим свойствам в модели представления.

Свойство `ItemsSource` объекта `ListView` получает статическое свойство `NamedColor.All`. Это заполняет объект `ListView` всеми экземплярами `NamedColor`. Для каждого элемента в `ListView` контекст привязки для элемента устанавливается для объекта `NamedColor`. Объекты `BoxView` и `Label` в `ViewCell` привязаны к свойствам в `NamedColor`.

Свойство `SelectedItem` объекта `ListView` имеет тип `NamedColor` и привязывается к свойству `BackgroundNamedColor` объекта `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Режим привязки по умолчанию для `SelectedItem` — `OneWayToSource`, и он устанавливает свойство модели представления из выбранного элемента. Режим `TwoWay` позволяет инициализировать `SelectedItem` из модели представления.

Но если `SelectedItem` устанавливается таким образом, `ListView` не прокручивается автоматически для отображения выбранного элемента. Необходимо немного кода в файле с выделенным кодом:

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem,
                                   ScrollToPosition.MakeVisible,
                                   false);
        }
    }
}
```

На снимке экрана iOS слева показана программа при первом запуске. Конструктор в `SampleSettingsViewModel` инициализирует белый цвет фона, как это выбрано в `ListView`:

[![Примеры параметров](binding-mode-images/samplesettings-small.png "Примеры параметров")](binding-mode-images/samplesettings-large.png#lightbox "Примеры параметров")

На другом снимке экрана показаны измененные параметры. Экспериментируя с этой страницей, не забудьте перевести программу в спящий режим или завершить ее работу на устройстве или эмуляторе, где она выполняется. Завершение программы из отладчика Visual Studio не приведет к тому, что `OnSleep` переопределит класс `App` для вызова.

В следующей статье будет показано, как указать [**форматирование строки**](string-formatting.md) привязок данных, заданных для свойства `Text` объекта `Label`.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Глава, посвященная привязкам данных, из книги о Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)