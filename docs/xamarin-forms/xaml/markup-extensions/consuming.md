---
title: Использование расширений разметки XAML
description: В этой статье объясняется, как использовать Xamarin.Forms расширения разметки XAML для расширения возможностей и гибкости XAML, разрешая установку атрибутов элементов из различных источников.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ee834bf7bae945536facb576ae26b9d13f5b7bcd
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91558951"
---
# <a name="consuming-xaml-markup-extensions"></a>Использование расширений разметки XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

Расширения разметки XAML помогают повысить степень гибкости и гибкость XAML, разрешая установку атрибутов элементов из различных источников. Несколько расширений разметки XAML являются частью спецификации XAML 2009. Они отображаются в файлах XAML с `x` префиксом пользовательского пространства имен и обычно называются этим префиксом. В этой статье рассматриваются следующие расширения разметки:

- [`x:Static`](#xstatic-markup-extension) — Ссылка на статические свойства, поля или члены перечисления.
- [`x:Reference`](#xreference-markup-extension) — ссылки на именованные элементы на странице.
- [`x:Type`](#xtype-markup-extension) — Задайте атрибут для `System.Type` объекта.
- [`x:Array`](#xarray-markup-extension) — Создание массива объектов определенного типа.
- [`x:Null`](#xnull-markup-extension) — Присвойте атрибуту `null` значение.
- [`OnPlatform`](#onplatform-markup-extension) — Настройка внешнего вида пользовательского интерфейса для отдельных платформ.
- [`OnIdiom`](#onidiom-markup-extension) — Настройка внешнего вида пользовательского интерфейса на основе идиомы устройства, на котором работает приложение.
- [`DataTemplate`](#datatemplate-markup-extension) — Преобразует тип в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .
- [`FontImage`](#fontimage-markup-extension) — отображать значок шрифта в любом представлении, которое может отображать `ImageSource` .
- [`AppThemeBinding`](#appthemebinding-markup-extension) — использование ресурса на основе текущей системной темы.

Дополнительные расширения разметки XAML исторически поддерживаются другими реализациями XAML, а также поддерживаются Xamarin.Forms . Они описаны более полно в других статьях:

- `StaticResource` — ссылки на объекты из словаря ресурсов, как описано в статье  [**словари ресурсов**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` — реагирование на изменения объектов в словаре ресурсов, как описано в статье [**динамические стили**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` — Установите связь между свойствами двух объектов, как описано в статье [**Привязка данных**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding`— выполняет привязку данных из шаблона элемента управления, как описано в статье [** Xamarin.Forms шаблоны элементов управления**](~/xamarin-forms/app-fundamentals/templates/control-template.md).
- `RelativeSource` — Задает источник привязки относительно положения целевого объекта привязки, как описано в статье [относительные привязки](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md).

[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)Макет использует пользовательское расширение разметки [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) . Это расширение разметки описано в статье [**RelativeLayout**](~/xamarin-forms/user-interface/layouts/relativelayout.md).

## <a name="xstatic-markup-extension"></a>расширение разметки x:Static

`x:Static`Расширение разметки поддерживается [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) классом. Класс имеет одно свойство с именем [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) типа `string` , для которого задано имя общей константы, статического свойства, статического поля или члена перечисления.

Один из распространенных способов использования `x:Static` — сначала определить класс с некоторыми константами или статическими переменными, такими как этот маленький `AppConstants` класс в программе [**расширений MarkupExtension**](/samples/xamarin/xamarin-forms-samples/xaml-markupextensions) :

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

На **демонстрационной странице x:Static** показано несколько способов использования `x:Static` расширения разметки. Самый подробный подход создает экземпляр `StaticExtension` класса между `Label.FontSize` тегами элементов свойства:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

Средство синтаксического анализа XAML также позволяет `StaticExtension` сократить класс следующим образом `x:Static` :

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Это можно сделать еще более простым, но в результате изменения появился новый синтаксис: он состоит в размещении `StaticExtension` класса и параметра элемента в фигурных скобках. Результирующее выражение задается непосредственно в `FontSize` атрибуте:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Обратите внимание, *no* что в фигурных скобках отсутствуют кавычки. `Member`Свойство объекта `StaticExtension` больше не является XML-атрибутом. Вместо этого он является частью выражения для расширения разметки.

Точно так же, как `x:StaticExtension` и `x:Static` при использовании в качестве объектного элемента, можно также сократить его в выражении внутри фигурных скобок:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension`Класс имеет `ContentProperty` атрибут, ссылающийся на свойство `Member` , которое помечает это свойство как свойство содержимого класса по умолчанию. Для расширений разметки XAML, выраженных фигурными скобками, можно исключить `Member=` часть выражения:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Это наиболее распространенная форма `x:Static` расширения разметки.

**Статическая демонстрационная** страница содержит два других примера. Корневой тег файла XAML содержит объявление пространства имен XML для `System` пространства имен .NET:

```xaml
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

Это позволяет `Label` задать для размера шрифта статическое поле `Math.PI` . Это приводит к простому тексту, поэтому `Scale` свойство имеет значение `Math.E` :

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

В последнем примере отображается `Device.RuntimePlatform` значение. `Environment.NewLine`Свойство static используется для вставки символа новой строки между двумя `Span` объектами:

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Вот пример, в котором работает:

[![Демонстрация в виде x:Static](consuming-images/staticdemo-small.png "Демонстрация в виде x:Static")](consuming-images/staticdemo-large.png#lightbox "Демонстрация в виде x:Static")

## <a name="xreference-markup-extension"></a>расширение разметки x:Reference

`x:Reference`Расширение разметки поддерживается [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) классом. Класс имеет одно свойство с именем [`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) типа `string` , которому задается имя элемента на странице, которому было присвоено имя `x:Name` . Это `Name` свойство является свойством Content объекта `ReferenceExtension` , поэтому `Name=` оно не является обязательным при `x:Reference` отображении в фигурных скобках.

`x:Reference`Расширение разметки используется исключительно с привязками данных, которые более подробно описаны в статье [**Привязка данных**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

На **демонстрационной странице x:Reference** показано два способа использования `x:Reference` с привязками данных, первая из которых используется для задания `Source` свойства `Binding` объекта, а вторая — для установки `BindingContext` свойства для двух привязок данных:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Оба `x:Reference` выражения используют сокращенную версию `ReferenceExtension` имени класса и удаляют `Name=` часть выражения. В первом примере `x:Reference` расширение разметки внедряется в `Binding` расширение разметки. Обратите внимание, что `Source` `StringFormat` Параметры и разделяются запятыми. Вот работающая программа:

[![Демонстрация x:Reference](consuming-images/referencedemo-small.png "Демонстрация x:Reference")](consuming-images/referencedemo-large.png#lightbox "Демонстрация x:Reference")

## <a name="xtype-markup-extension"></a>x:Type - расширение разметки

`x:Type`Расширение разметки является эквивалентом XAML [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/) ключевого слова C#. Он поддерживается [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) классом, который определяет одно свойство с именем [`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) типа `string` , для которого задано имя класса или структуры. `x:Type`Расширение разметки возвращает [`System.Type`](xref:System.Type) объект этого класса или структуры. `TypeName` свойство Content объекта `TypeExtension` , поэтому `TypeName=` не требуется, если `x:Type` отображается с фигурными скобками.

В Xamarin.Forms существует несколько свойств с аргументами типа `Type` . Примеры включают [`TargetType`](xref:Xamarin.Forms.Style.TargetType) свойство класса `Style` и атрибут [x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#specifying-a-generic-type-argument) , используемый для указания аргументов в универсальных классах. Однако средство синтаксического анализа XAML автоматически выполняет `typeof` операцию, и `x:Type` в таких случаях расширение разметки не используется.

В одном месте `x:Type` *is* требуется `x:Array` расширение разметки, которое описано в [следующем разделе](#xarray-markup-extension).

`x:Type`Расширение разметки также полезно при создании меню, в котором каждый элемент меню соответствует объекту определенного типа. Можно связать `Type` объект с каждым пунктом меню, а затем создать экземпляр объекта при выборе пункта меню.

Таким способом работает меню навигации в `MainPage` в программе **расширения разметки** . Файл **MainPage. XAML** содержит объект, `TableView` `TextCell` соответствующий определенной странице в программе:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

Вот как открыть главную страницу в **расширениях разметки**:

[![Главная страница](consuming-images/mainpage-small.png "Главная страница")](consuming-images/mainpage-large.png#lightbox "Главная страница")

`CommandParameter`Для каждого свойства задается `x:Type` расширение разметки, ссылающееся на одну из других страниц. `Command`Свойство привязано к свойству с именем `NavigateCommand` . Это свойство определено в `MainPage` файле кода программной части:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

`NavigateCommand`Свойство — это `Command` объект, реализующий команду Execute с аргументом типа `Type` &mdash; value `CommandParameter` . Метод использует `Activator.CreateInstance` для создания экземпляра страницы, а затем переходит к ней. Конструктор завершается заданием для `BindingContext` страницы самого себя, что позволяет `Binding` работать в `Command` . Дополнительные сведения об этом типе кода см. в статье [**Привязка данных**](~/xamarin-forms/app-fundamentals/data-binding/index.md) и в частности в статье о [**командном**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) коде.

На **демонстрационной странице x:Type** используется аналогичная методика для создания экземпляров Xamarin.Forms элементов и их добавления в `StackLayout` . XAML-файл изначально состоит из трех `Button` элементов с их `Command` свойствами, для которых задано значение, `Binding` а `CommandParameter` свойства — типы трех Xamarin.Forms представлений:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

Файл кода программной части определяет и инициализирует `CreateCommand` свойство:

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

Метод, выполняемый при `Button` нажатии кнопки, создает новый экземпляр аргумента, устанавливает его `VerticalOptions` свойство и добавляет его в `StackLayout` . Затем три `Button` элемента совместно используют страницу с динамически созданными представлениями:

[![x:Type — демонстрация](consuming-images/typedemo-small.png "x:Type — демонстрация")](consuming-images/typedemo-large.png#lightbox "x:Type — демонстрация")

## <a name="xarray-markup-extension"></a>x:Array - расширение разметки

`x:Array`Расширение разметки позволяет определить массив в разметке. Он поддерживается [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) классом, который определяет два свойства:

- `Type` типа `Type` , который указывает тип элементов в массиве.
- `Items` типа `IList` , который представляет собой коллекцию самих элементов. Это свойство содержимого объекта `ArrayExtension` .

`x:Array`Само расширение разметки никогда не отображается в фигурных скобках. Вместо этого `x:Array` открывающий и закрывающий теги разделяют список элементов. Задайте `Type` для свойства `x:Type` расширение разметки.

На странице **демонстрации** продолжений показано, как использовать `x:Array` для добавления элементов в `ListView` , задав `ItemsSource` для свойства массив.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

`ViewCell`Создает простую `BoxView` для каждой записи цвета:

[![Демонстрация расx:Array](consuming-images/arraydemo-small.png "Демонстрация расx:Array")](consuming-images/arraydemo-large.png#lightbox "Демонстрация расx:Array")

Существует несколько способов указать отдельные `Color` элементы в этом массиве. Вы можете использовать `x:Static` расширение разметки:

```xaml
<x:Static Member="Color.Blue" />
```

Или можно использовать `StaticResource` для получения цвета из словаря ресурсов:

```xaml
<StaticResource Key="myColor" />
```

В конце этой статьи вы увидите собственное расширение разметки XAML, которое также создает новое значение цвета:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

При определении массивов общих типов, таких как строки или числа, используйте теги, перечисленные в статье [**Передача аргументов конструктора**](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments) , чтобы разделить значения.

## <a name="xnull-markup-extension"></a>x:Null - расширение разметки

`x:Null`Расширение разметки поддерживается [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) классом. Он не имеет свойств и является просто эквивалентом XAML [`null`](/dotnet/csharp/language-reference/keywords/null/) ключевого слова C#.

`x:Null`Расширение разметки редко требуется и редко используется, но если вы нашли нужную необходимость, вы будете рады, что он существует.

В **демонстрационной странице x:NULL** показан один сценарий, `x:Null` который может быть удобным. Предположим, что вы определяете неявные `Style` для `Label` , включающие, `Setter` которое задает `FontFamily` для свойства имя семейства, зависящее от платформы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

Затем вы обнаружите, что для одного из `Label` элементов необходимо, чтобы все параметры свойств в неявном `Style` значении, за исключением параметра `FontFamily` , который вы хотите использовать по умолчанию. Для этой цели можно определить другой, `Style` но более простой подход заключается в установке `FontFamily` свойства конкретного объекта `Label` в `x:Null` , как показано в центре `Label` .

Вот работающая программа:

[![Демонстрация x:Null](consuming-images/nulldemo-small.png "Демонстрация x:Null")](consuming-images/nulldemo-large.png#lightbox "Демонстрация x:Null")

Обратите внимание, что четыре элемента `Label` имеют шрифт с засечками, но у центра `Label` есть шрифт Sans-Serif по умолчанию.

## <a name="onplatform-markup-extension"></a>Расширение разметки для платформы

Расширение разметки `OnPlatform` позволяет настраивать внешний вид пользовательского интерфейса для каждой платформы. Он предоставляет те же функциональные возможности [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) , что и [`On`](xref:Xamarin.Forms.On) классы и, но с более кратким представлением.

`OnPlatform`Расширение разметки поддерживается [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) классом, который определяет следующие свойства:

- `Default` типа, для которого `object` задано значение по умолчанию, применяемое к свойствам, представляющим платформы.
- `Android` типа `object` , для которого задано значение, применяемое к Android.
- `GTK` типа `object` , для которого задано значение, применяемое к платформам GTK.
- `iOS` типа `object` , для которого задано значение, применяемое к iOS.
- `macOS` типа `object` , для которого задано значение, применяемое к macOS.
- `Tizen` типа `object` , для которого задано значение, применяемое к платформе Tizen.
- `UWP` типа, для которого `object` задано значение, применяемое к универсальная платформа Windows.
- `WPF` типа, для которого `object` задано значение, применяемое к Windows Presentation Foundationной платформе.
- `Converter` типа `IValueConverter` , для которого можно задать `IValueConverter` реализацию.
- `ConverterParameter` типа `object` , для которого можно задать значение, передаваемое в `IValueConverter` реализацию.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) сократить класс до `OnPlatform` .

`Default`Свойство является свойством Content объекта `OnPlatformExtension` . Поэтому для выражений разметки XAML, выраженных с фигурными скобками, можно исключить `Default=` часть выражения, если она является первым аргументом. Если `Default` свойство не задано, по умолчанию будет установлено [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) значение свойства при условии, что расширение разметки предназначено для [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) .

> [!IMPORTANT]
> Средство синтаксического анализа XAML требует, чтобы значения правильного типа были предоставлены свойствам, которые занимают `OnPlatform` расширение разметки. Если требуется преобразование типа, `OnPlatform` расширение разметки будет пытаться выполнить его с помощью преобразователей по умолчанию, предоставляемых Xamarin.Forms . Однако существуют преобразования типов, которые не могут быть выполнены преобразователями по умолчанию, и в таких случаях `Converter` свойству следует присвоить `IValueConverter` реализацию.

На странице **демонстрационная страница для демонстрации архитектуры** показано, как использовать `OnPlatform` расширение разметки:

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

В этом примере все три `OnPlatform` выражения используют сокращенную версию `OnPlatformExtension` имени класса. Три `OnPlatform` расширения разметки устанавливают [`Color`](xref:Xamarin.Forms.BoxView.Color) свойства, [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) для в [`BoxView`](xref:Xamarin.Forms.BoxView) различные значения в iOS, Android и UWP. Расширения разметки также предоставляют значения по умолчанию для этих свойств на неопределенных платформах, исключая `Default=` часть выражения. Обратите внимание, что заданные свойства расширения разметки разделяются запятыми.

Вот работающая программа:

[![Демонстрационная демонстрация](consuming-images/onplatformdemo-small.png "Демонстрационная демонстрация")](consuming-images/onplatformdemo-large.png#lightbox "Демонстрационная демонстрация")

## <a name="onidiom-markup-extension"></a>Расширение разметки onidiomа

`OnIdiom`Расширение разметки позволяет настраивать внешний вид пользовательского интерфейса на основе идиомы устройства, на котором выполняется приложение. Он поддерживается [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) классом, который определяет следующие свойства:

- `Default` типа, для которого `object` задано значение по умолчанию, применяемое к свойствам, представляющим идиомы устройств.
- `Phone` типа `object` , для которого задано значение, применяемое к телефонам.
- `Tablet` типа `object` , для которого задано значение, применяемое к планшетам.
- `Desktop` типа `object` , для которого задано значение, применяемое к настольным платформам.
- `TV` типа `object` , для которого задано значение, применяемое на телевизионных платформах.
- `Watch` типа `object` , для которого задано значение, применяемое к платформам наблюдения.
- `Converter` типа `IValueConverter` , для которого можно задать `IValueConverter` реализацию.
- `ConverterParameter` типа `object` , для которого можно задать значение, передаваемое в `IValueConverter` реализацию.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) сократить класс до `OnIdiom` .

`Default`Свойство является свойством Content объекта `OnIdiomExtension` . Поэтому для выражений разметки XAML, выраженных с фигурными скобками, можно исключить `Default=` часть выражения, если она является первым аргументом.

> [!IMPORTANT]
> Средство синтаксического анализа XAML требует, чтобы значения правильного типа были предоставлены свойствам, которые занимают `OnIdiom` расширение разметки. Если требуется преобразование типа, `OnIdiom` расширение разметки будет пытаться выполнить его с помощью преобразователей по умолчанию, предоставляемых Xamarin.Forms . Однако существуют преобразования типов, которые не могут быть выполнены преобразователями по умолчанию, и в таких случаях `Converter` свойству следует присвоить `IValueConverter` реализацию.

На **демонстрационной странице Onidiomа** показано, как использовать `OnIdiom` расширение разметки:

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

В этом примере все три `OnIdiom` выражения используют сокращенную версию `OnIdiomExtension` имени класса. Три `OnIdiom` расширения разметки устанавливают [`Color`](xref:Xamarin.Forms.BoxView.Color) свойства, [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) для в различные значения в заметках [`BoxView`](xref:Xamarin.Forms.BoxView) телефона, планшета и рабочего стола. Расширения разметки также предоставляют значения по умолчанию для этих свойств в неуказанных идиомах, исключая `Default=` часть выражения. Обратите внимание, что заданные свойства расширения разметки разделяются запятыми.

Вот работающая программа:

[![Демонстрация onidiomы](consuming-images/onidiomdemo-small.png "Демонстрация onidiomы")](consuming-images/onidiomdemo-large.png#lightbox "Демонстрация onidiomы")

## <a name="datatemplate-markup-extension"></a>Расширение разметки DataTemplate

`DataTemplate`Расширение разметки позволяет преобразовать тип в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Он поддерживается `DataTemplateExtension` классом, который определяет `TypeName` свойство типа, которому присваивается `string` имя типа, преобразуемого в `DataTemplate` . `TypeName`Свойство является свойством Content объекта `DataTemplateExtension` . Таким образом, для выражений разметки XAML с фигурными скобками можно исключить часть `TypeName=` выражения.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет сократить класс `DataTemplateExtension` как `DataTemplate`.

Типичное использование этого расширения разметки — в приложении оболочки, как показано в следующем примере:

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

В этом примере `MonkeysPage` преобразуется из в [`ContentPage`](xref:Xamarin.Forms.ContentPage) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , который задается как значение `ShellContent.ContentTemplate` Свойства. Это гарантирует, что `MonkeysPage` она будет создана только при переходе на страницу, а не при запуске приложения.

Дополнительные сведения о приложениях оболочки см. в разделе [ Xamarin.Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="fontimage-markup-extension"></a>Расширение разметки FontImage

`FontImage`Расширение разметки позволяет отображать значок шрифта в любом представлении, которое может отображать `ImageSource` . Он предоставляет те же функциональные возможности, что и `FontImageSource` класс, но с более кратким представлением.

Расширение разметки `FontImage` поддерживается классом `FontImageExtension`, определяющим следующие свойства:

- `FontFamily` типа `string` — семейство шрифтов, которому принадлежит значок шрифта.
- `Glyph` типа `string` , значение символа Юникода со значком Font.
- `Color` Тип [`Color`](xref:Xamarin.Forms.Color) — цвет, используемый при отображении значка шрифта.
- `Size` Тип `double` — Размер в единицах, не зависящих от устройства, значка отображаемого шрифта. Значение по умолчанию — 30. Кроме того, для этого свойства можно задать именованный размер шрифта.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет сократить класс `FontImageExtension` как `FontImage`.

`Glyph`Свойство является свойством Content объекта `FontImageExtension` . Поэтому для выражений разметки XAML, выраженных с фигурными скобками, можно исключить `Glyph=` часть выражения, если она является первым аргументом.

На **демонстрационной странице фонтимаже** показано, как использовать `FontImage` расширение разметки:

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

В этом примере сокращенная версия `FontImageExtension` имени класса используется для вывода значка Xbox из семейства шрифтов иониконс в [`Image`](xref:Xamarin.Forms.Image) . Выражение также использует `OnPlatform` расширение разметки для указания различных `FontFamily` значений свойств в iOS и Android. Кроме того, `Glyph=` часть выражения исключена, а заданные свойства расширения разметки разделяются запятыми. Обратите внимание, что хотя символ Юникода для значка — `\uf30c` , он должен быть экранирован в XAML и, таким образом, стал `&#xf30c;` .

Вот работающая программа:

[![Снимок экрана расширения разметки Фонтимаже](consuming-images/fontimagedemo.png "Демонстрация Фонтимаже")](consuming-images/fontimagedemo-large.png#lightbox "Демонстрация Фонтимаже")

Сведения о отображении значков шрифтов путем указания данных значка шрифта в `FontImageSource` объекте см. в разделе [Отображение значков шрифтов](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons).

## <a name="appthemebinding-markup-extension"></a>Расширение разметки AppThemeBinding

`AppThemeBinding`Расширение разметки позволяет указать ресурс, который будет использоваться, например изображение или цвет, на основе текущей системной темы.

> [!IMPORTANT]
> `AppThemeBinding`Расширение разметки имеет минимальные требования к операционной системе. Дополнительные сведения см. [в разделе реагирование на изменения системных тем в Xamarin.Forms приложениях](~/xamarin-forms/user-interface/theming/system-theme-changes.md).

Расширение разметки `AppThemeBinding` поддерживается классом `AppThemeBindingExtension`, определяющим следующие свойства:

- `Default`, тип, заданный `object` для ресурса, который будет использоваться по умолчанию.
- `Light`, тип, заданный `object` для ресурса, который будет использоваться, когда устройство использует светло-тему.
- `Dark`, тип, заданный `object` для ресурса, который будет использоваться, когда устройство использует его темную тему.
- `Value`Тип `object` , который возвращает ресурс, который в настоящее время используется расширением разметки.

> [!NOTE]
> Средство синтаксического анализа XAML позволяет сократить класс `AppThemeBindingExtension` как `AppBindingTheme`.

`Default`Свойство является свойством Content объекта `AppThemeBindingExtension` . Поэтому для выражений разметки XAML, выраженных с фигурными скобками, можно исключить `Default=` часть выражения, если она является первым аргументом.

На **демонстрационной странице аппсемебиндинг** показано, как использовать `AppThemeBinding` расширение разметки:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.AppThemeBindingDemoPage"
             Title="AppThemeBinding Demo">
    <ContentPage.Resources>

        <Style x:Key="labelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{AppThemeBinding Black, Light=Blue, Dark=Teal}" />
        </Style>

    </ContentPage.Resources>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{AppThemeBinding Light=Green, Dark=Red}" />
        <Label Text="This text is black by default, blue in light mode, and teal in dark mode."
               Style="{StaticResource labelStyle}" />
    </StackLayout>
</ContentPage>
```

В этом примере цвет текста первого объекта задается [`Label`](xref:Xamarin.Forms.Label) зеленым цветом, когда устройство использует его светлую тему, и задается красным, если устройство использует его темную тему. Во втором `Label` [`TextColor`](xref:Xamarin.Forms.Label.TextColor) свойство задается с помощью [`Style`](xref:Xamarin.Forms.Style) . При этом `Style` Цвет текста `Label` для черного по умолчанию устанавливается синим цветом, если устройство использует светло-тему, и синего цвета, когда устройство использует его темную тему.

Вот работающая программа:

![Демонстрация Аппсемебиндинг](consuming-images/appthemebindingdemo.png "Демонстрация Аппсемебиндинг")

## <a name="define-markup-extensions"></a>Определение расширений разметки

Если вы столкнулись с потребностью в расширении разметки XAML, которое недоступно в Xamarin.Forms , можно [создать собственное](creating.md).

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки (пример)](/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Глава о расширениях разметки XAML из Xamarin.Forms книги](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Оболочка Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Реагирование на изменения системных тем в Xamarin.Forms приложениях](~/xamarin-forms/user-interface/theming/system-theme-changes.md)