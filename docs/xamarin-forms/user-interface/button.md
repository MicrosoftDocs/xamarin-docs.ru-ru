---
title: Xamarin.Forms Переключатель
description: Кнопка реагирует на касание или щелчок, которое направляет приложение для выполнения определенной задачи.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6534d25e46ecdd5fcdcd9c525aa49b8e2ded5f49
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374216"
---
# <a name="no-locxamarinforms-button"></a>Xamarin.Forms Переключатель

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_Кнопка реагирует на касание или щелчок, которое направляет приложение для выполнения определенной задачи._

[`Button`](xref:Xamarin.Forms.Button)Является самым фундаментальным интерактивным элементом управления во всех Xamarin.Forms . `Button`Обычно отображает короткую текстовую строку, указывающую команду, но может также отображать точечный рисунок или сочетание текста и изображения. Пользователь нажимает `Button` с помощью пальца или щелкает его с помощью мыши, чтобы запустить эту команду.

Большинство описанных ниже разделов соответствуют страницам образца [**буттондемос**](/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) .

## <a name="handling-button-clicks"></a>Обработка нажатий кнопок

`Button` Определяет [`Clicked`](xref:Xamarin.Forms.Button.Clicked) событие, возникающее при касании пользователем `Button` пальцем или указателем мыши. Событие возникает при отпускании кнопки с пальцем или кнопкой мыши с поверхности `Button` . Свойство должно иметь значение, равное, чтобы `Button` [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) `true` он отвечал на касания.

На странице **"основные"** в образце [**буттондемос**](/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) показано, как создать экземпляр `Button` в XAML и обработать его `Clicked` событие. Файл **басикбуттонкликкпаже. XAML** содержит и, `StackLayout` `Label` и, и `Button` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.BasicButtonClickPage"
             Title="Basic Button Click">
    <StackLayout>

        <Label x:Name="label"
               Text="Click the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Click to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

`Button`Как правило, он занимает все пространство, допустимое для него. Например, если не задать `HorizontalOptions` для свойства значение `Button` , отличное от `Fill` , то `Button` будет занимать полную ширину его родителя.

По умолчанию `Button` является прямоугольным, но можно передать скругленные углы с помощью [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) свойства, как описано ниже в разделе « [**внешний вид кнопки**](#button-appearance)раздела».

[`Text`](xref:Xamarin.Forms.Button.Text)Свойство задает текст, отображаемый в `Button` . [`Clicked`](xref:Xamarin.Forms.Button.Clicked)Для события задается обработчик событий с именем `OnButtonClicked` . Этот обработчик находится в файле кода программной части **BasicButtonClickPage.XAML.CS** :

```csharp
public partial class BasicButtonClickPage : ContentPage
{
    public BasicButtonClickPage ()
    {
        InitializeComponent ();
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        await label.RelRotateTo(360, 1000);
    }
}
```

При касании `Button` выполняется метод `OnButtonClicked`. `sender`Аргумент — это `Button` объект, ответственный за это событие. Его можно использовать для доступа к `Button` объекту или для различения нескольких объектов, `Button` совместно использующих одно и то же `Clicked` событие.

Этот конкретный `Clicked` обработчик вызывает функцию анимации, которая поворачивает `Label` 360 градусов в 1000 миллисекунд. Вот программа, выполняемая на устройствах iOS и Android, а также как приложение универсальная платформа Windows (UWP) на рабочем столе Windows 10:

[![Нажатие кнопки "базовый"](button-images/BasicButtonClick.png "Нажатие кнопки "базовый"")](button-images/BasicButtonClick-Large.png#lightbox "Нажатие кнопки "базовый"")

Обратите внимание, что `OnButtonClicked` метод включает `async` модификатор, так как `await` используется в обработчике событий. `Clicked`Обработчик событий требует модификатор, `async` только если тело обработчика использует `await` .

Каждая платформа подготавливает к просмотру `Button` особым образом. В разделе [**вид кнопки**](#button-appearance) вы узнаете, как задать цвета и сделать `Button` границу видимой для более настраиваемых представлений. `Button` реализует [`IFontElement`](xref:Xamarin.Forms.Internals.IFontElement) интерфейс, поэтому он включает [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) свойства, [`FontSize`](xref:Xamarin.Forms.Button.FontSize) и [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) .

## <a name="creating-a-button-in-code"></a>Создание кнопки в коде

Часто создается экземпляр `Button` в XAML, но можно также создать `Button` в коде. Это удобно, когда приложению необходимо создать несколько кнопок на основе данных, которые перечислены с помощью `foreach` цикла.

На странице Нажатие **кнопки кода** показано, как создать страницу, которая функционально эквивалентна базовой странице нажатия **кнопки «обычная»** , но полностью на C#:

```csharp
public class CodeButtonClickPage : ContentPage
{
    public CodeButtonClickPage ()
    {
        Title = "Code Button Click";

        Label label = new Label
        {
            Text = "Click the Button below",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };

        Button button = new Button
        {
            Text = "Click to Rotate Text!",
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };
        button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);

        Content = new StackLayout
        {
            Children =
            {
                label,
                button
            }
        };
    }
}
```

Все выполняется в конструкторе класса. Так как `Clicked` обработчик является только одной инструкцией, он может быть присоединен к событию очень просто:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Разумеется, можно также определить обработчик событий как отдельный метод (точно так же, как `OnButtonClick` метод при **нажатии кнопки Basic** ) и присоединить этот метод к событию:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Отключение кнопки

Иногда приложение находится в определенном состоянии, в котором конкретный `Button` щелчок не является допустимой операцией. В таких случаях `Button` следует отключить, задав `IsEnabled` свойству значение `false` . Классический пример — это `Entry` элемент управления для имени файла, сопровождающего открытие файла `Button` : `Button` должен быть включен, только если какой-либо текст был введен в `Entry` .
`DataTrigger`Для этой задачи можно использовать, как показано в статье [**триггеры данных**](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) .

## <a name="using-the-command-interface"></a>Использование интерфейса команд

Приложение может реагировать на `Button` касания без обработки `Clicked` события. `Button`Класс реализует альтернативный механизм уведомления, называемый _командой_ или интерфейсом _командной строки_ . Это состоит из двух свойств:

- [`Command`](xref:Xamarin.Forms.Button.Command) типа [`ICommand`](xref:System.Windows.Input.ICommand) — интерфейс, определенный в [`System.Windows.Input`](xref:System.Windows.Input) пространстве имен.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) свойство типа [`Object`](xref:System.Object) .

Этот подход особенно удобен в связи с привязкой данных, особенно при реализации архитектуры Model-View-ViewModel (MVVM). Эти разделы обсуждаются в статье [Привязка](~/xamarin-forms/app-fundamentals/data-binding/index.md)данных, [из привязок данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)и [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md).

В приложении MVVM ViewModel определяет свойства типа `ICommand` , которые затем соединяются с `Button` элементами XAML с привязками данных. Xamarin.Forms также определяет [`Command`](xref:Xamarin.Forms.Command) [`Command<T>`](xref:Xamarin.Forms.Command`1) классы, реализующие `ICommand` интерфейс, и помогает использовать ViewModel при определении свойств типа `ICommand` .

Команда описывается более подробно в статье о [**интерфейсе команды**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) , но на странице **команды Basic** в образце [**буттондемос**](/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) показан базовый подход.

`CommandDemoViewModel`Класс является очень простым ViewModel, который определяет свойство типа `double` с именем `Number` , и два свойства типа `ICommand` с именем `MultiplyBy2Command` и `DivideBy2Command` :

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    double number = 1;

    public event PropertyChangedEventHandler PropertyChanged;

    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(() => Number *= 2);

        DivideBy2Command = new Command(() => Number /= 2);
    }

    public double Number
    {
        set
        {
            if (number != value)
            {
                number = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Number"));
            }
        }
        get
        {
            return number;
        }
    }

    public ICommand MultiplyBy2Command { private set; get; }

    public ICommand DivideBy2Command { private set; get; }
}
```

Эти два `ICommand` Свойства инициализируются в конструкторе класса с двумя объектами типа `Command` . `Command`Конструкторы содержат небольшую функцию (называемую `execute` аргументом конструктора), которая либо вдвое, либо вдвое является `Number` свойством.

Файл **басикбуттонкомманд. XAML** задает для него `BindingContext` экземпляр `CommandDemoViewModel` . `Label`Элемент и два элемента `Button` содержат привязки к трем свойствам в `CommandDemoViewModel` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.BasicButtonCommandPage"
             Title="Basic Button Command">

    <ContentPage.BindingContext>
        <local:CommandDemoViewModel />
    </ContentPage.BindingContext>

    <StackLayout>
        <Label Text="{Binding Number, StringFormat='Value is now {0}'}"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Multiply by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding MultiplyBy2Command}" />

        <Button Text="Divide by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding DivideBy2Command}" />
    </StackLayout>
</ContentPage>
```

По мере `Button` касания двух элементов выполняются команды, а число изменяется в значении:

[![Команда "основные"](button-images/BasicButtonCommand.png "Команда "основные"")](button-images/BasicButtonCommand-Large.png#lightbox)

Преимущество этого подхода к `Clicked` обработчикам заключается в том, что вся логика, включающая в себя функциональность этой страницы, расположена в ViewModel, а не в файле кода программной части, что обеспечивает более эффективное разделение пользовательского интерфейса от бизнес-логики.

Кроме того, `Command` объекты могут управлять включением и отключением `Button` элементов. Например, предположим, что требуется ограничить диапазон числовых значений от 2<sup>10</sup> до 2<sup> &ndash; 10</sup>. Можно добавить в конструктор другую функцию (называемую `canExecute` аргументом), которая возвращает `true` значение, если `Button` должно быть включено. Ниже приведено изменение в `CommandDemoViewModel` конструкторе.

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    ···
    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(
            execute: () =>
            {
                Number *= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number < Math.Pow(2, 10));

        DivideBy2Command = new Command(
            execute: () =>
            {
                Number /= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number > Math.Pow(2, -10));
    }
    ···
}
```

Вызовы к `ChangeCanExecute` методу необходимы для того `Command` , чтобы `Command` метод мог вызвать `canExecute` метод и определить, `Button` должен ли быть отключен. При изменении этого кода, так как число достигает предела, `Button` отключается:

[![Команда "основные" — изменено](button-images/BasicButtonCommandModified.png "Команда "основные" — изменено")](button-images/BasicButtonCommandModified-Large.png#lightbox)

Возможно, несколько `Button` элементов будут привязаны к одному и тому же `ICommand` свойству. `Button`Элементы можно отличать с помощью [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) свойства `Button` . В этом случае необходимо использовать универсальный [`Command<T>`](xref:Xamarin.Forms.Command`1) класс. `CommandParameter`Затем объект передается в качестве аргумента `execute` `canExecute` методам и. Этот метод подробно описан в разделе " [**базовый" команд**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) в статье о [**интерфейсе команд**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) .

В примере [**буттондемос**](/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) этот метод также используется в своем `MainPage` классе. Файл **MainPage. XAML** содержит `Button` для каждой страницы примера:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.MainPage"
             Title="Button Demos">
    <ScrollView>
        <FlexLayout Direction="Column"
                    JustifyContent="SpaceEvenly"
                    AlignItems="Center">

            <Button Text="Basic Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonClickPage}" />

            <Button Text="Code Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:CodeButtonClickPage}" />

            <Button Text="Basic Button Command"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonCommandPage}" />

            <Button Text="Press and Release Button"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:PressAndReleaseButtonPage}" />

            <Button Text="Button Appearance"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ButtonAppearancePage}" />

            <Button Text="Toggle Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ToggleButtonDemoPage}" />

            <Button Text="Image Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ImageButtonDemoPage}" />

        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Каждый `Button` из них имеет `Command` свойство, привязанное к свойству с именем `NavigateCommand` , а `CommandParameter` задается [`Type`](xref:System.Type) объект, соответствующий одному из классов страниц в проекте.

Это `NavigateCommand` свойство имеет тип `ICommand` и определено в файле кода программной части:

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

Конструктор инициализирует `NavigateCommand` свойство для `Command<Type>` объекта, так как `Type` является типом `CommandParameter` набора объектов в файле XAML. Это означает, что `execute` метод имеет аргумент типа `Type` , соответствующий этому `CommandParameter` объекту. Функция создает экземпляр страницы, а затем переходит к ней.

Обратите внимание, что конструктор завершает свою установку `BindingContext` . Это необходимо для привязки свойства к свойству в файле XAML `NavigateCommand` .

## <a name="pressing-and-releasing-the-button"></a>Нажатие и отпускание кнопки

Помимо `Clicked` события, `Button` также определяет [`Pressed`](xref:Xamarin.Forms.Button.Pressed) и [`Released`](xref:Xamarin.Forms.Button.Released) события. Это `Pressed` событие возникает, когда палец нажимает на `Button` , или кнопка мыши нажата с указателем, расположенным над `Button` . Это `Released` событие возникает при отпускании кнопки мыши или пальца. Как правило, `Clicked` событие также срабатывает в то же время, что и `Released` событие, но если палец или указатель мыши выходит за пределы области `Button` до выпуска, `Clicked` событие может не произойти.

`Pressed`События и `Released` часто не используются, но их можно использовать для специальных целей, как показано на странице нажатия **и выпуска** . XAML-файл содержит `Label` и `Button` обработчики, присоединенные к `Pressed` `Released` событиям и.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.PressAndReleaseButtonPage"
             Title="Press and Release Button">
    <StackLayout>

        <Label x:Name="label"
               Text="Press and hold the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Press to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Pressed="OnButtonPressed"
                Released="OnButtonReleased" />

    </StackLayout>
</ContentPage>
```

Файл кода программной части анимируется, `Label` когда `Pressed` происходит событие, но приостанавливает вращение при `Released` возникновении события:

```csharp
public partial class PressAndReleaseButtonPage : ContentPage
{
    bool animationInProgress = false;
    Stopwatch stopwatch = new Stopwatch();

    public PressAndReleaseButtonPage ()
    {
        InitializeComponent ();
    }

    void OnButtonPressed(object sender, EventArgs args)
    {
        stopwatch.Start();
        animationInProgress = true;

        Device.StartTimer(TimeSpan.FromMilliseconds(16), () =>
        {
            label.Rotation = 360 * (stopwatch.Elapsed.TotalSeconds % 1);

            return animationInProgress;
        });
    }

    void OnButtonReleased(object sender, EventArgs args)
    {
        animationInProgress = false;
        stopwatch.Stop();
    }
}
```

В результате происходит `Label` поворот только на то время, когда палец находится в контакте с `Button` и останавливается при отпускании пальца:

[![Нажатие кнопки "Пуск"](button-images/PressAndReleaseButton.png "Нажатие кнопки "Пуск"")](button-images/PressAndReleaseButton-Large.png)

Этот тип поведения имеет приложения для игр. палец, находящиеся на, `Button` может сделать объект на экране передвигаться в определенном направлении.

## <a name="button-appearance"></a>Внешний вид кнопки

Класс `Button` наследует или определяет несколько свойств, влияющих на его внешний вид:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) цвет `Button` текста
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) цвет фона для этого текста
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) цвет области, окружающей элемент `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) семейство шрифтов, используемое для текста
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) Размер текста
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) Указывает, выделен ли текст курсивом или полужирным шрифтом
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) Ширина границы
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) является радиусом угла `Button`
- [`CharacterSpacing`](xref:Xamarin.Forms.Button.CharacterSpacing) интервал между символами `Button` текста.
- `TextTransform` Определяет регистр `Button` текста.

> [!NOTE]
> `Button`Класс также имеет [`Margin`](xref:Xamarin.Forms.View.Margin) Свойства и [`Padding`](xref:Xamarin.Forms.Button.Padding) , управляющие поведением макета `Button` . Дополнительные сведения см. в статье [Поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

Влияние шести из этих свойств (за исключением `FontFamily` и `FontAttributes` ) демонстрируется на странице « **Оформление кнопки»** . Другое свойство, [`Image`](xref:Xamarin.Forms.Button.ImageSource) , рассматривается в разделе [**Использование растровых изображений с кнопкой**](#using-bitmaps-with-buttons).

Все представления и привязки данных на странице « **Оформление кнопки»** определены в файле XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ButtonAppearancePage"
             Title="Button Appearance">
    <StackLayout>
        <Button x:Name="button"
                Text="Button"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                TextColor="{Binding Source={x:Reference textColorPicker},
                                    Path=SelectedItem.Color}"
                BackgroundColor="{Binding Source={x:Reference backgroundColorPicker},
                                          Path=SelectedItem.Color}"
                BorderColor="{Binding Source={x:Reference borderColorPicker},
                                      Path=SelectedItem.Color}" />

        <StackLayout BindingContext="{x:Reference button}"
                     Padding="10">

            <Slider x:Name="fontSizeSlider"
                    Maximum="48"
                    Minimum="1"
                    Value="{Binding FontSize}" />

            <Label Text="{Binding Source={x:Reference fontSizeSlider},
                                  Path=Value,
                                  StringFormat='FontSize = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="borderWidthSlider"
                    Minimum="-1"
                    Maximum="12"
                    Value="{Binding BorderWidth}" />

            <Label Text="{Binding Source={x:Reference borderWidthSlider},
                                  Path=Value,
                                  StringFormat='BorderWidth = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="cornerRadiusSlider"
                    Minimum="-1"
                    Maximum="24"
                    Value="{Binding CornerRadius}" />

            <Label Text="{Binding Source={x:Reference cornerRadiusSlider},
                                  Path=Value,
                                  StringFormat='CornerRadius = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.Resources>
                    <Style TargetType="Label">
                        <Setter Property="VerticalOptions" Value="Center" />
                    </Style>
                </Grid.Resources>

                <Label Text="Text Color:"
                       Grid.Row="0" Grid.Column="0" />

                <Picker x:Name="textColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="0" Grid.Column="1" />

                <Label Text="Background Color:"
                       Grid.Row="1" Grid.Column="0" />

                <Picker x:Name="backgroundColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="1" Grid.Column="1" />

                <Label Text="Border Color:"
                       Grid.Row="2" Grid.Column="0" />

                <Picker x:Name="borderColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="2" Grid.Column="1" />
            </Grid>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

В `Button` верхней части страницы есть три `Color` свойства, привязанные к элементам в `Picker` нижней части страницы. Элементы в `Picker` элементах — это цвета из класса, который `NamedColor` входит в проект. Три `Slider` элемента содержат двусторонние привязки к `FontSize` `BorderWidth` `CornerRadius` свойствам, и объекта `Button` .

Эта программа позволяет экспериментировать с сочетаниями всех этих свойств:

[![Внешний вид кнопки](button-images/ButtonAppearance.png "Внешний вид кнопки")](button-images/ButtonAppearance-Large.png)

Чтобы увидеть `Button` границу, необходимо задать для параметра значение, отличное `BorderColor` от `Default` , и `BorderWidth` положительное значение.

В iOS вы заметите, что большая ширина границы будет `Button` помешать внутренней части и повлияет на отображение текста. Если вы решили использовать границу с iOS `Button` , то, вероятно, захотите начать и завершить `Text` свойство пробелами, чтобы сохранилась его видимость.

В UWP при выборе элемента `CornerRadius` , размер которого превышает половину высоты, `Button` возникает исключение.

## <a name="button-visual-states"></a>Визуальные состояния кнопки

[`Button`](xref:Xamarin.Forms.Button) имеет объект `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) , который можно использовать для инициации визуального изменения в `Button` при нажатии пользователем, при условии, что он включен.

В следующем примере XAML показано, как определить визуальное состояние для `Pressed` состояния:

```xaml
<Button Text="Click me!"
        ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Button>
```

`Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) Указывает, что при [`Button`](xref:Xamarin.Forms.Button) нажатии элемента его [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) свойство будет заменено значением по умолчанию от 1 до 0,8. `Normal` `VisualState` Указывает, что если `Button` находится в нормальном состоянии, его `Scale` свойство будет установлено в значение 1. Таким образом, общий результат заключается в том, что при `Button` нажатии кнопки она масштабируется немного меньше, а при `Button` освобождении она масштабируется до размера по умолчанию.

Дополнительные сведения о визуальных состояниях см. [в разделе Xamarin.Forms Диспетчер визуальных состояний](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="creating-a-toggle-button"></a>Создание выключателя

Можно создать подкласс, чтобы `Button` он работал как параметр "вкл.". Нажмите кнопку один раз, чтобы включить кнопку, и коснитесь кнопки еще раз, чтобы отключить ее.

Следующий `ToggleButton` класс является производным от класса `Button` и определяет новое событие с именем `Toggled` и логическое свойство с именем `IsToggled` . Это те же два свойства, которые определены в Xamarin.Forms [`Switch`](xref:Xamarin.Forms.Switch) :

```csharp
class ToggleButton : Button
{
    public event EventHandler<ToggledEventArgs> Toggled;

    public static BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled", typeof(bool), typeof(ToggleButton), false,
                                propertyChanged: OnIsToggledChanged);

    public ToggleButton()
    {
        Clicked += (sender, args) => IsToggled ^= true;
    }

    public bool IsToggled
    {
        set { SetValue(IsToggledProperty, value); }
        get { return (bool)GetValue(IsToggledProperty); }
    }

    protected override void OnParentSet()
    {
        base.OnParentSet();
        VisualStateManager.GoToState(this, "ToggledOff");
    }

    static void OnIsToggledChanged(BindableObject bindable, object oldValue, object newValue)
    {
        ToggleButton toggleButton = (ToggleButton)bindable;
        bool isToggled = (bool)newValue;

        // Fire event
        toggleButton.Toggled?.Invoke(toggleButton, new ToggledEventArgs(isToggled));

        // Set the visual state
        VisualStateManager.GoToState(toggleButton, isToggled ? "ToggledOn" : "ToggledOff");
    }
}
```

`ToggleButton`Конструктор присоединяет обработчик к `Clicked` событию, чтобы он мог изменить значение `IsToggled` Свойства. `OnIsToggledChanged`Метод запускает `Toggled` событие.

Последняя строка `OnIsToggledChanged` метода вызывает статический `VisualStateManager.GoToState` метод с двумя текстовыми строками «тоггледон» и «тоггледофф». Вы можете прочитать об этом методе и о том, как приложение может реагировать на визуальные состояния в статье [**Xamarin.Forms Диспетчер визуальных состояний**](~/xamarin-forms/user-interface/visual-state-manager.md).

Поскольку вызывает `ToggleButton` `VisualStateManager.GoToState` класс, ему не нужно включать дополнительные средства для изменения внешнего вида кнопки в зависимости от ее `IsToggled` состояния. Это несет ответственность за XAML, в котором размещается `ToggleButton` .

**Демонстрационная Страница переключателя** содержит два экземпляра `ToggleButton` , включая разметку диспетчера визуального состояния, которая задает `Text` , `BackgroundColor` и, а также `TextColor` кнопку на основе визуального состояния:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ToggleButtonDemoPage"
             Title="Toggle Button Demo">

    <ContentPage.Resources>
        <Style TargetType="local:ToggleButton">
            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            <Setter Property="HorizontalOptions" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <local:ToggleButton Toggled="OnItalicButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Italic Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Italic On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <local:ToggleButton Toggled="OnBoldButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Bold Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Bold On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <Label x:Name="label"
               Text="Just a little passage of some sample text that can be formatted in italic or boldface by toggling the two buttons."
               FontSize="Large"
               HorizontalTextAlignment="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

`Toggled`Обработчики событий находятся в файле кода программной части. Они отвечают за задание `FontAttributes` свойства объекта в `Label` зависимости от состояния кнопок:

```csharp
public partial class ToggleButtonDemoPage : ContentPage
{
    public ToggleButtonDemoPage ()
    {
        InitializeComponent ();
    }

    void OnItalicButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Italic;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Italic;
        }
    }

    void OnBoldButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Bold;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Bold;
        }
    }
}
```

Вот программа, выполняемая в iOS, Android и UWP:

[![Демонстрация выключателя](button-images/ToggleButtonDemo.png "Демонстрация выключателя")](button-images/ToggleButtonDemo-Large.png#lightbox)

## <a name="using-bitmaps-with-buttons"></a>Использование растровых изображений в кнопках

`Button`Класс определяет [`ImageSource`](xref:Xamarin.Forms.Button.Image) свойство, которое позволяет отображать точечный рисунок `Button` либо отдельно, либо в сочетании с текстом. Можно также указать способ упорядочения текста и изображения.

`ImageSource`Свойство имеет тип, что [`ImageSource`](xref:Xamarin.Forms.ImageSource) означает, что точечные рисунки можно загрузить из файла, внедренного ресурса, URI или потока.

> [!NOTE]
> Хотя `Button` может загрузить анимированный GIF-файл, он будет отображать только первый кадр GIF-файла.

Каждая платформа, поддерживаемая, Xamarin.Forms позволяет хранить образы в разных размерах для различных разрешающих точек различных устройств, на которых может работать приложение. Эти несколько точечных рисунков называются или хранятся таким образом, что операционная система может выбрать лучшее соответствие для разрешения экрана устройства.

Для точечного рисунка `Button` наиболее оптимальный размер обычно составляет от 32 до 64 единиц, независимых от устройств, в зависимости от того, насколько оно должно быть. Изображения, используемые в этом примере, основаны на размере 48 единиц, не зависящих от устройства.

В проекте iOS папка **Resources** содержит три размера этого образа:

- Точечный рисунок размером 48 пикселей, хранящийся как **/ресаурцес/MonkeyFace.png**
- Точечный рисунок размером 96 пикселей, хранящийся как **/Resource/MonkeyFace@2x.png**
- Точечный рисунок размером 144 пикселей, хранящийся как **/Resource/MonkeyFace@3x.png**

Всем трем точечным рисункам было предоставлено **действие сборки** **BundleResource**.

Для проекта Android все точечные рисунки имеют одинаковое имя, но хранятся в разных вложенных папках папки **Resources** :

- Точечный рисунок размером 72 пикселей, хранящийся как **/ресаурцес/дравабле-хдпи/MonkeyFace.png**
- Точечный рисунок размером 96 пикселей, хранящийся как **/ресаурцес/дравабле-ксхдпи/MonkeyFace.png**
- Точечный рисунок размером 144 пикселей, хранящийся как **/ресаурцес/дравабле-ксксхдпи/MonkeyFace.png**
- Точечный рисунок размером 192 пикселей, хранящийся как **/ресаурцес/дравабле-ксксксхдпи/MonkeyFace.png**

Им было предоставлено **действие сборки** **AndroidResource**.

В проекте UWP точечные рисунки можно хранить в любом месте проекта, но они обычно хранятся в пользовательской папке или в существующей папке **Assets** . Проект UWP содержит следующие растровые изображения:

- Точечный рисунок размером 48 пикселей, хранящийся как **/ассетс/MonkeyFace.scale-100.png**
- Точечный рисунок размером 96 пикселей, хранящийся как **/ассетс/MonkeyFace.scale-200.png**
- Точечный рисунок размером 192 пикселей, хранящийся как **/ассетс/MonkeyFace.scale-400.png**

Всем им было предоставлено **действие сборки** **содержимого**.

Можно указать способ `Text` `ImageSource` упорядочения свойств и `Button` с помощью [`ContentLayout`](xref:Xamarin.Forms.Button.ContentLayout) свойства `Button` . Это свойство имеет тип [`ButtonContentLayout`](xref:Xamarin.Forms.Button.ButtonContentLayout) , который является внедренным классом в `Button` . [Constructor] (xref: Xamarin.Forms . Button. Буттонконтентлайаут .% 23ctor ( Xamarin.Forms . Кнопка. Буттонконтентлайаут. Имажепоситион, система. Double) имеет два аргумента:

- Член [`ImagePosition`](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) перечисления: `Left` , `Top` , `Right` или, `Bottom` указывающий, как точечный рисунок отображается относительно текста.
- `double`Величина интервала между точечным рисунком и текстом.

Значения по умолчанию — `Left` и 10 единиц. Два свойства только для чтения `ButtonContentLayout` с именами [`Position`](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) и [`Spacing`](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) предоставляют значения этих свойств.

В коде можно создать `Button` и задать `ContentLayout` свойство следующим образом:

```csharp
Button button = new Button
{
    Text = "button text",
    ImageSource = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

В XAML необходимо указать только член перечисления, пробелы или оба значения в любом порядке, разделенном запятыми:

```xaml
<Button Text="button text"
        ImageSource="image filename"
        ContentLayout="Right, 20" />
```

На странице **Демонстрационная Страница с изображением** используется `OnPlatform` для указания различных имен файлов точечных рисунков iOS, Android и UWP. Если вы хотите использовать одно и то же имя файла для каждой платформы и избежать использования `OnPlatform` , необходимо сохранить точечные рисунки UWP в корневом каталоге проекта.

Первая `Button` Страница на **демонстрационной странице с изображением** задает `Image` свойство, но не `Text` свойство:

```xaml
<Button>
    <Button.ImageSource>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.ImageSource>
</Button>
```

Если точечные рисунки UWP хранятся в корневом каталоге проекта, эта разметка может значительно упроститься:

```xaml
<Button ImageSource="MonkeyFace.png" />
```

Чтобы избежать большого объема разметки какой в файле **имажебуттондемо. XAML** , `Style` также определяется неявная Настройка `ImageSource` Свойства. Это `Style` автоматически применяется к пяти другим `Button` элементам. Вот полный XAML файл:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">

        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="ImageSource">
                    <OnPlatform x:TypeArguments="ImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.ImageSource>
                <OnPlatform x:TypeArguments="ImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.ImageSource>
        </Button>

        <Button Text="Default" />

        <Button Text="Left - 10"
                ContentLayout="Left, 10" />

        <Button Text="Top - 10"
                ContentLayout="Top, 10" />

        <Button Text="Right - 20"
                ContentLayout="Right, 20" />

        <Button Text="Bottom - 20"
                ContentLayout="Bottom, 20" />
    </FlexLayout>
</ContentPage>
```

Последние четыре `Button` элемента используют `ContentLayout` свойство для указания расположения и расстояния текста и точечного рисунка:

[![Демонстрация кнопки изображения](button-images/ImageButtonDemo.png "Демонстрация кнопки изображения")](button-images/ImageButtonDemo-Large.png#lightbox)

Теперь вы видели различные способы, которыми можно управлять `Button` событиями и изменять `Button` внешний вид.

## <a name="related-links"></a>Связанные ссылки

- [Пример Буттондемос](/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [API Button](xref:Xamarin.Forms.Button)