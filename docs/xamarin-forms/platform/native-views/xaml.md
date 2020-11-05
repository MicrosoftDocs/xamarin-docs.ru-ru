---
title: Собственные представления в XAML
description: На собственные представления из iOS, Android и универсальная платформа Windows можно напрямую ссылаться из Xamarin.Forms файлов XAML. Свойства и обработчики событий могут быть установлены в собственных представлениях и могут взаимодействовать с Xamarin.Forms представлениями. В этой статье показано, как использовать собственные представления из Xamarin.Forms файлов XAML.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cd7c29f835b34b4c5ffb9a5af589815a09546a87
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93365912"
---
# <a name="native-views-in-xaml"></a>Собственные представления в XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_На собственные представления из iOS, Android и универсальная платформа Windows можно напрямую ссылаться из Xamarin.Forms файлов XAML. Свойства и обработчики событий могут быть установлены в собственных представлениях и могут взаимодействовать с Xamarin.Forms представлениями. В этой статье показано, как использовать собственные представления из Xamarin.Forms файлов XAML._

Внедрение собственного представления в Xamarin.Forms файл XAML:

1. Добавьте `xmlns` объявление пространства имен в файл XAML для пространства имен, содержащего собственное представление.
1. Создайте экземпляр собственного представления в файле XAML.

> [!IMPORTANT]
> Скомпилированный XAML должен быть отключен для всех страниц XAML, использующих собственные представления. Это можно сделать, добавив класс кода программной части для страницы XAML с помощью `[XamlCompilation(XamlCompilationOptions.Skip)]` атрибута. Дополнительные сведения о компиляции XAML см. [в разделе Компиляция XAML Xamarin.Forms в ](~/xamarin-forms/xaml/xamlc.md).

Чтобы сослаться на собственное представление из файла кода программной части, необходимо использовать проект общего ресурса (SAP) и заключить код, зависящий от платформы, с директивами условной компиляции. Дополнительные сведения см. [в статье ссылки на собственные представления из кода](#refer-to-native-views-from-code).

## <a name="consume-native-views"></a>Использование собственных представлений

В следующем примере кода показано использование собственных представлений для каждой платформы в Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

Также `clr-namespace` `assembly` необходимо указать и для пространства имен собственного представления `targetPlatform` . Для этого свойства должно быть задано значение `iOS` , `Android` , `UWP` , `Windows` (что эквивалентно `UWP` ), `macOS` ,, `GTK` `Tizen` или `WPF` . Во время выполнения средство синтаксического анализа XAML будет игнорировать все префиксы пространства имен XML, имеющие `targetPlatform` , что не соответствует платформе, на которой выполняется приложение.

Каждое объявление пространства имен может использоваться для ссылки на любой класс или структуру из указанного пространства имен. Например, `ios` объявление пространства имен может использоваться для ссылки на любой класс или структуру из `UIKit` пространства имен iOS. Свойства собственного представления можно задать с помощью XAML, но типы свойств и объектов должны совпадать. Например, для `UILabel.TextColor` свойства задано `UIColor.Red` использование `x:Static` расширения разметки и `ios` пространства имен.

Связываемые свойства и присоединенные привязываемые свойства также могут быть установлены для собственных представлений с помощью `Class.BindableProperty="value"` синтаксиса. Каждое собственное представление упаковывается в экземпляр, зависящий `NativeViewWrapper` от платформы, который является производным от [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) класса. Задание привязываемого свойства или присоединенного привязываемого свойства в собственном представлении передает значение свойства в оболочку. Например, можно указать горизонтальный макет с выравниванием по центру, задав `View.HorizontalOptions="Center"` для собственного представления.

> [!NOTE]
> Обратите внимание, что стили нельзя использовать с собственными представлениями, так как стили могут указывать только свойства, которые поддерживаются `BindableProperty` объектами.

Конструкторам мини-приложений Android обычно требуется `Context` объект Android в качестве аргумента, и это можно сделать доступным через статическое свойство в `MainActivity` классе. Поэтому при создании мини-приложения Android в XAML `Context` объект должен быть обычно передан в конструктор мини-приложения с помощью `x:Arguments` атрибута с `x:Static` расширением разметки. Дополнительные сведения см. [в разделе Передача аргументов в собственные представления](#pass-arguments-to-native-views).

> [!NOTE]
> Обратите внимание, что именование собственного представления невозможно `x:Name` в проекте библиотеки .NET Standard или в общем проекте ресурса (SAP). Это приведет к формированию переменной собственного типа, которая вызовет ошибку компиляции. Однако собственные представления можно упаковывать в `ContentView` экземпляры и извлекать в файле кода программной части при условии, что используется SAP. Дополнительные сведения см. [в статье о встроенном представлении из кода](#refer-to-native-views-from-code).

## <a name="native-bindings"></a>Собственные привязки

Привязка данных используется для синхронизации пользовательского интерфейса с источником данных и упрощает Xamarin.Forms Отображение и взаимодействие приложения с его данными. При условии, что исходный объект реализует `INotifyPropertyChanged` интерфейс, изменения в *исходном* объекте автоматически отправляются в *целевой* объект платформой привязки, а изменения в *целевом* объекте могут быть переданы в *Исходный* объект.

Свойства собственных представлений также могут использовать привязку данных. В следующем примере кода демонстрируется привязка данных с помощью свойств собственных представлений:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

Страница содержит объект [`Entry`](xref:Xamarin.Forms.Entry) , [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) свойство которого привязано к `NativeSwitchPageViewModel.IsSwitchOn` свойству. Для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы задается новый экземпляр `NativeSwitchPageViewModel` класса в файле кода программной части с классом ViewModel, реализующим `INotifyPropertyChanged` интерфейс.

Страница также содержит собственный коммутатор для каждой платформы. Каждый собственный коммутатор использует [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) привязку для обновления значения `NativeSwitchPageViewModel.IsSwitchOn` Свойства. Таким образом, если параметр выключен, параметр `Entry` отключен, а при включенном параметре `Entry` включается. На следующих снимках экрана показана эта функция на каждой платформе:

![Встроенный коммутатор с отключенным собственным коммутатором ](xaml-images/native-switch-disabled.png)
 ![ включен](xaml-images/native-switch-enabled.png)

Двусторонние привязки поддерживаются автоматически при условии, что собственное свойство реализует `INotifyPropertyChanged` или поддерживает Key-Value наблюдение (кво) в iOS или является классом `DependencyProperty` UWP. Однако многие собственные представления не поддерживают уведомление об изменении свойства. Для этих представлений можно указать [`UpdateSourceEventName`](xref:Xamarin.Forms.Binding.UpdateSourceEventName) значение свойства как часть выражения привязки. Этому свойству должно быть присвоено имя события в собственном представлении, которое сигнализирует об изменении целевого свойства. Затем, когда значение собственного коммутатора меняется, `Binding` класс уведомляется о том, что пользователь изменил значение переключателя, а `NativeSwitchPageViewModel.IsSwitchOn` значение свойства — Обновлено.

## <a name="pass-arguments-to-native-views"></a>Передача аргументов в собственные представления

Аргументы конструктора можно передать в собственные представления с помощью `x:Arguments` атрибута с `x:Static` расширением разметки. Кроме того, собственные методы фабрики представлений ( `public static` методы, возвращающие объекты или значения того же типа, что и класс или структура, определяющие методы) могут вызываться путем указания имени метода с помощью `x:FactoryMethod` атрибута и его аргументов с помощью `x:Arguments` атрибута.

В следующем примере кода показаны обе методики.

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

[`UIFont.FromName`](xref:UIKit.UIFont.FromName*)Метод Factory используется для присвоения [`UILabel.Font`](xref:UIKit.UILabel.Font) свойству нового элемента [`UIFont`](xref:UIKit.UIFont) в iOS. `UIFont`Имя и размер указываются аргументами метода, которые являются дочерними элементами `x:Arguments` атрибута.

[`Typeface.Create`](xref:Android.Graphics.Typeface.Create*)Метод Factory используется для присвоения [`TextView.Typeface`](xref:Android.Widget.TextView.Typeface) свойству нового [`Typeface`](xref:Android.Graphics.Typeface) в Android. `Typeface`Имя и стиль семейства задаются аргументами метода, которые являются дочерними по отношению к `x:Arguments` атрибуту.

[`FontFamily`](/uwp/api/Windows.UI.Xaml.Media.FontFamily)Конструктор используется для присвоения [`TextBlock.FontFamily`](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) свойству нового значения `FontFamily` в универсальная платформа Windows (UWP). `FontFamily`Имя задается аргументом метода, который является дочерним по отношению к `x:Arguments` атрибуту.

> [!NOTE]
> Аргументы должны соответствовать типам, необходимым конструктору или фабричному методу.

На следующих снимках экрана показан результат указания фабричного метода и аргументов конструктора для задания шрифта в различных собственных представлениях:

![Задание шрифтов в собственных представлениях](xaml-images/passing-arguments.png)

Дополнительные сведения о передаче аргументов в XAML см. в разделе [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md).

## <a name="refer-to-native-views-from-code"></a>Ссылки на собственные представления из кода

Хотя невозможно присвоить собственное представление `x:Name` атрибуту, можно получить экземпляр собственного представления, объявленный в файле XAML, из файла кода программной части в проекте общего доступа при условии, что собственное представление является дочерним элементом объекта [`ContentView`](xref:Xamarin.Forms.ContentView) , который задает `x:Name` значение атрибута. Затем внутри директив условной компиляции в файле кода программной части следует:

1. Получите [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) значение свойства и приведите его к типу, зависящему от платформы `NativeViewWrapper` .
1. Извлеките `NativeViewWrapper.NativeElement` свойство и приведите его к типу собственного представления.

Затем собственный API может быть вызван в собственном представлении для выполнения требуемых операций. Этот подход также дает преимущество, так как несколько собственных представлений XAML для различных платформ могут быть дочерними элементами одного и того же [`ContentView`](xref:Xamarin.Forms.ContentView) . Этот метод показан в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
              <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

В приведенном выше примере собственные представления для каждой платформы являются дочерними [`ContentView`](xref:Xamarin.Forms.ContentView) элементами элементов управления со `x:Name` значением атрибута, используемым для получения `ContentView` в коде программной части:

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

[`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content)Доступ к свойству осуществляется для получения встроенного представления с оболочкой в виде экземпляра, зависящего от платформы `NativeViewWrapper` . `NativeViewWrapper.NativeElement`Затем осуществляется доступ к свойству для получения собственного представления в виде собственного типа. Затем для выполнения требуемых операций вызывается API собственного представления.

Собственные кнопки iOS и Android используют один и тот же `OnButtonTap` обработчик событий, так как каждая собственная кнопка принимает `EventHandler` делегат в ответ на событие касания. Однако в универсальная платформа Windows (UWP) используется отдельный `RoutedEventHandler` , который, в свою очередь, использует `OnButtonTap` обработчик событий в этом примере. Поэтому при нажатии на встроенную кнопку `OnButtonTap` выполняется обработчик событий, который масштабирует и поворачивает собственный элемент управления, содержащийся в [`ContentView`](xref:Xamarin.Forms.ContentView) названии `contentViewTextParent` . На следующих снимках экрана показано, что происходит на каждой платформе:

![ContentView, содержащий собственный элемент управления](xaml-images/contentview.png)

## <a name="subclass-native-views"></a>Собственные представления подклассов

Многие собственные представления iOS и Android не подходят для создания экземпляров в XAML, поскольку для настройки элемента управления используются методы, а не свойства. Решением этой проблемы является подкласс собственных представлений в оболочках, которые определяют более удобный для XAML интерфейс API, использующий свойства для настройки элемента управления и использующего независимые от платформы события. Встроенные собственные представления могут быть помещены в общий проект ресурсов (SAP) и окружены директивами условной компиляции или помещены в проекты для конкретных платформ и на которые имеются ссылки из XAML в проекте библиотеки .NET Standard.

В следующем примере кода показана Xamarin.Forms страница, использующая собственные представления подкласса:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

Страница содержит объект [`Label`](xref:Xamarin.Forms.Label) , отображающий фруктов, выбранный пользователем из собственного элемента управления. `Label`Привязка к `SubclassedNativeControlsPageViewModel.SelectedFruit` свойству. Для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы задается новый экземпляр `SubclassedNativeControlsPageViewModel` класса в файле кода программной части с классом ViewModel, реализующим `INotifyPropertyChanged` интерфейс.

Страница также содержит собственное представление средства выбора для каждой платформы. Каждое собственное представление отображает коллекцию фруктам, привязывая ее `ItemSource` свойство к `SubclassedNativeControlsPageViewModel.Fruits` коллекции. Это позволяет пользователю выбрать фруктов, как показано на следующих снимках экрана:

![Собственные представления с подклассами](xaml-images/sub-classed.png)

В iOS и Android собственные средства выбора используют методы для настройки элементов управления. Таким образом, эти подборы должны быть подклассами для предоставления свойств, чтобы сделать их понятными для XAML. На универсальная платформа Windows (UWP) `ComboBox` компонент уже понятен XAML и поэтому не требует подклассов.

### <a name="ios"></a>iOS

Реализация iOS подклассует [`UIPickerView`](xref:UIKit.UIPickerView) представление и предоставляет свойства и событие, которое можно легко использовать из XAML:

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

`MyUIPickerView`Класс предоставляет доступ `ItemsSource` к `SelectedItem` свойствам и и `SelectedItemChanged` событию. Для [`UIPickerView`](xref:UIKit.UIPickerView) требуется базовая [`UIPickerViewModel`](xref:UIKit.UIPickerViewModel) модель данных, доступ к которой осуществляется с помощью `MyUIPickerView` свойств и события. `UIPickerViewModel`Модель данных обеспечивается `PickerModel` классом:

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

`PickerModel`Класс предоставляет базовое хранилище для `MyUIPickerView` класса через `Items` свойство. При каждом изменении выбранного элемента `MyUIPickerView` [`Selected`](xref:UIKit.UIPickerViewModel.Selected*) выполняется метод, который обновляет выбранный индекс и запускает `ItemChanged` событие. Это гарантирует, что `SelectedItem` свойство всегда будет возвращать последний элемент, выбранный пользователем. Кроме того, `PickerModel` класс переопределяет методы, используемые для настройки `MyUIPickerView` экземпляра.

### <a name="android"></a>Android

Реализация Android подклассировать [`Spinner`](xref:Android.Widget.Spinner) представление и предоставляет свойства и событие, которое можно легко использовать из XAML:

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

`MySpinner`Класс предоставляет доступ `ItemsSource` к `SelectedObject` свойствам и и `ItemSelected` событию. Элементы, отображаемые `MySpinner` классом, предоставляются [`Adapter`](xref:Android.Widget.Adapter) связанным с представлением, а элементы заполняются `Adapter` при `ItemsSource` первом задании свойства. При изменении выбранного элемента в `MySpinner` классе `OnBindableSpinnerItemSelected` обработчик событий обновляет `SelectedObject` свойство.

## <a name="related-links"></a>Связанные ссылки

- [Нативесвитч (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native (пример)](/samples/xamarin/xamarin-forms-samples/forms2native)
- [Нативевиевинсидеконтентвиев (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [Субкласседнативеконтролс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [Собственные формы](~/xamarin-forms/platform/native-forms.md)
- [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md)