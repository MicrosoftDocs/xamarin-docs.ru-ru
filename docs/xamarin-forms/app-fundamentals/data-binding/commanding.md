---
title: Командный интерфейс Xamarin.Forms
description: В этой статье объясняется, как реализовать свойство Command с привязками данных Xamarin.Forms. Командный интерфейс предоставляет альтернативный подход к реализации команд, который гораздо лучше подходит для архитектуры MVVM.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cd37166c461abe6b92a280dd52098d85c2393fbd
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93369835"
---
# <a name="the-xamarinforms-command-interface"></a>Командный интерфейс Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/databindingdemos)

В архитектуре "модель — представление — модель представления" (MVVM) привязки данных определяются между свойствами в модели представления, которое обычно является классом, производным от `INotifyPropertyChanged`, и свойствами в представлении, которое обычно является файлом XAML. Иногда приложение имеет потребности, которые не удовлетворяются этими привязками свойств, и требует, чтобы пользователь запускал команды, влияющие на элементы модели представления. Эти команды обычно обозначаются путем нажатия кнопки или касания пальцем и традиционно обрабатываются в файле с выделенным кодом в обработчике для события `Clicked` объекта `Button` или события `Tapped` объекта `TapGestureRecognizer`.

Командный интерфейс предоставляет альтернативный подход к реализации команд, который гораздо лучше подходит для архитектуры MVVM. Сама модель представления может содержать команды, которые представляют собой методы, выполняющиеся в ответ на определенное действие в представлении, например нажатие объекта `Button`. Привязки данных определяются между этими командами и объектом `Button`.

Чтобы разрешить привязку данных между объектом `Button` и моделью представления, `Button` определяет два свойства:

- [`Command`](xref:Xamarin.Forms.Button.Command) типа [`System.Windows.Input.ICommand`](xref:System.Windows.Input.ICommand).
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) типа `Object`

Чтобы использовать командный интерфейс, определите привязку данных, направленную на свойство `Command` объекта `Button`, где источником является свойство модели представления типа `ICommand`. Модель представления содержит код, связанный с этим свойством `ICommand`, которое выполняется при нажатии кнопки. Вы можете задать для `CommandParameter` произвольные данные, чтобы различать несколько кнопок, если все они привязаны к одному свойству `ICommand` в модели представления.

Свойства `Command` и `CommandParameter` также определяются следующими классами:

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) и, следовательно, [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem), производным от `MenuItem`;
- [`TextCell`](xref:Xamarin.Forms.TextCell) и, следовательно, [`ImageCell`](xref:Xamarin.Forms.ImageCell), производным от `TextCell`;
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) определяет свойство [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) типа `ICommand` и свойство [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter). Свойство [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) объекта [`ListView`](xref:Xamarin.Forms.ListView) также принадлежит типу `ICommand`.

Все эти команды могут обрабатываться в модели представления независимо от конкретного объекта пользовательского интерфейса в представлении.

## <a name="the-icommand-interface"></a>Интерфейс ICommand

Интерфейс [`System.Windows.Input.ICommand`](xref:System.Windows.Input.ICommand) не является частью Xamarin.Forms. Вместо этого он определен в пространстве имен [System.Windows.Input](xref:System.Windows.Input) и состоит из двух методов и одного события:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Чтобы использовать командный интерфейс, ваша модель представления содержит свойства типа `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

Модель представления также должна ссылаться на класс, реализующий интерфейс `ICommand`. Этот класс будет описан ниже. В представлении свойство `Command` объекта `Button` привязано к этому свойству:

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Когда пользователь нажимает `Button`, `Button` вызывает метод `Execute` в объекте `ICommand`, привязанном к его свойству `Command`. Это самая простая частью командного интерфейса.

Метод `CanExecute` более сложен. При первом определении привязки для свойства `Command` объекта `Button` и при изменении привязки данных `Button` вызывает метод `CanExecute` в объекте `ICommand`. Если `CanExecute` возвращает `false`, `Button` отключается. Это означает, что определенная команда в данный момент недоступна или недопустима.

`Button` также подключает обработчик для события `CanExecuteChanged` объекта `ICommand`. Событие вызывается из модели представления. Когда событие возникает, `Button` снова вызывает `CanExecute`. `Button` включается, если `CanExecute` возвращает `true`, и отключается, если `CanExecute` возвращает `false`.

> [!IMPORTANT]
> Не используйте свойство `IsEnabled` объекта `Button` при использовании командного интерфейса.  

## <a name="the-command-class"></a>Класс Command

Если ваша модель представления определяет свойство типа `ICommand`, она также должна содержать класс или ссылаться на класс, реализующий интерфейс `ICommand`. Этот класс должен содержать или ссылаться на методы `Execute` и `CanExecute` и вызывать событие `CanExecuteChanged` каждый раз, когда метод `CanExecute` может возвращать другое значение.

Можно написать такой класс самостоятельно или использовать класс, написанный другими. Так как `ICommand` входит в Microsoft Windows, он много лет используется с приложениями Windows MVVM. С помощью класса Windows, который реализует `ICommand`, вы можете совместно использовать модели представления в приложениях Windows и приложениях Xamarin.Forms.

Если общий доступ к модели представления для Windows и Xamarin.Forms не имеет значения, используйте класс [`Command`](xref:Xamarin.Forms.Command) или [`Command<T>`](xref:Xamarin.Forms.Command`1), включенный в Xamarin.Forms, для реализации интерфейса `ICommand`. Эти классы позволяют указать текст методов `Execute` и `CanExecute` в конструкторах классов. Используйте `Command<T>` при использовании свойства `CommandParameter`, чтобы различать несколько представлений, привязанных к одному свойству `ICommand`, и более простой класс `Command`, если это не требуется.

## <a name="basic-commanding"></a>Базовые команды

Страница **Запись пользователя** в программе [**Демоверсии привязок данных**](/samples/xamarin/xamarin-forms-samples/databindingdemos) показывает несколько простых команд, реализованных в модели представления.

`PersonViewModel` определяет три свойства с именами `Name`, `Age` и `Skills`, которые определяют пользователя. Этот класс *не* содержит свойств `ICommand`:

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    string name;
    double age;
    string skills;

    public event PropertyChangedEventHandler PropertyChanged;

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public double Age
    {
        set { SetProperty(ref age, value); }
        get { return age; }
    }

    public string Skills
    {
        set { SetProperty(ref skills, value); }
        get { return skills; }
    }

    public override string ToString()
    {
        return Name + ", age " + Age;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
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

Модель `PersonCollectionViewModel`, показанная ниже, создает объекты типа `PersonViewModel` и позволяет пользователю вводить данные. Для этой цели класс определяет свойства `IsEditing` типа `bool` и `PersonEdit` типа `PersonViewModel`. Кроме того, класс определяет три свойства типа `ICommand` и свойство с именем `Persons` типа `IList<PersonViewModel>`:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    PersonViewModel personEdit;
    bool isEditing;

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public bool IsEditing
    {
        private set { SetProperty(ref isEditing, value); }
        get { return isEditing; }
    }

    public PersonViewModel PersonEdit
    {
        set { SetProperty(ref personEdit, value); }
        get { return personEdit; }
    }

    public ICommand NewCommand { private set; get; }

    public ICommand SubmitCommand { private set; get; }

    public ICommand CancelCommand { private set; get; }

    public IList<PersonViewModel> Persons { get; } = new ObservableCollection<PersonViewModel>();

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
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

Этот сокращенный список не включает конструктор класса, где определяются три свойства типа `ICommand`, о чем мы поговорим позже. Изменения трех свойств типа `ICommand` и свойства `Persons` не приводят к возникновению событий `PropertyChanged`. Эти свойства задаются при создании класса и не меняются позже.

Перед изучением конструктора класса `PersonCollectionViewModel` взглянем на файл XAML для программы **Запись пользователя**. Он содержит `Grid` со свойствами `BindingContext`, заданными для `PersonCollectionViewModel`. `Grid` содержит `Button` с текстом **Создать** в свойстве `Command`, привязанном к свойству `NewCommand` в модели представления, форму записи со свойствами, привязанными к свойству `IsEditing`, а также свойствам модели `PersonViewModel`, и еще две кнопки, привязанные к свойствам `SubmitCommand` и `CancelCommand` модели представления. Конечный объект `ListView` отображает коллекцию уже введенных пользователей:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.PersonEntryPage"
             Title="Person Entry">
    <Grid Margin="10">
        <Grid.BindingContext>
            <local:PersonCollectionViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <!-- New Button -->
        <Button Text="New"
                Grid.Row="0"
                Command="{Binding NewCommand}"
                HorizontalOptions="Start" />

        <!-- Entry Form -->
        <Grid Grid.Row="1"
              IsEnabled="{Binding IsEditing}">

            <Grid BindingContext="{Binding PersonEdit}">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Label Text="Name: " Grid.Row="0" Grid.Column="0" />
                <Entry Text="{Binding Name}"
                       Grid.Row="0" Grid.Column="1" />

                <Label Text="Age: " Grid.Row="1" Grid.Column="0" />
                <StackLayout Orientation="Horizontal"
                             Grid.Row="1" Grid.Column="1">
                    <Stepper Value="{Binding Age}"
                             Maximum="100" />
                    <Label Text="{Binding Age, StringFormat='{0} years old'}"
                           VerticalOptions="Center" />
                </StackLayout>

                <Label Text="Skills: " Grid.Row="2" Grid.Column="0" />
                <Entry Text="{Binding Skills}"
                       Grid.Row="2" Grid.Column="1" />

            </Grid>
        </Grid>

        <!-- Submit and Cancel Buttons -->
        <Grid Grid.Row="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <Button Text="Submit"
                    Grid.Column="0"
                    Command="{Binding SubmitCommand}"
                    VerticalOptions="CenterAndExpand" />

            <Button Text="Cancel"
                    Grid.Column="1"
                    Command="{Binding CancelCommand}"
                    VerticalOptions="CenterAndExpand" />
        </Grid>

        <!-- List of Persons -->
        <ListView Grid.Row="3"
                  ItemsSource="{Binding Persons}" />
    </Grid>
</ContentPage>
```

Вот как это работает. Первый пользователь нажимает кнопку **Создать**. Это включает форму ввода, но отключает кнопку **Создать**. Пользователь вводит имя, возраст и навыки. В любое время во время редактирования пользователь может нажать кнопку **Отмена**, чтобы начать заново. Кнопка **Отправить** будет доступна только после того, как пользователь введет допустимое имя и возраст. При нажатии на кнопку **Отправить** пользователь переходит в коллекцию, отображаемую объектом `ListView`. После нажатия кнопки **Отмена** или **Отправить** форма ввода очищается и снова включается кнопка **Создать**.

На экране iOS слева показан макет до ввода допустимого возраста. На экранах Android кнопка **Отправить** включается после указания возраста:

[![Запись пользователя](commanding-images/personentry-small.png "Запись пользователя")](commanding-images/personentry-large.png#lightbox "Запись пользователя")

У программы нет возможностей для редактирования существующих записей и сохранения записей при уходе со страницы.

Вся логика для кнопок **Создать**, **Отправить** и **Отмена** обрабатывается в `PersonCollectionViewModel` посредством определения свойств `NewCommand`, `SubmitCommand` и `CancelCommand`. Конструктор `PersonCollectionViewModel` задает эти три свойства объектам типа `Command`.  

[Конструктор](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) класса `Command` позволяет передавать аргументы типа `Action` и `Func<bool>`, соответствующие методам `Execute` и `CanExecute`. Проще всего определить эти действия и функции как лямбда-функции прямо в конструкторе `Command`. Вот определение объекта `Command` для свойства `NewCommand`:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {
        NewCommand = new Command(
            execute: () =>
            {
                PersonEdit = new PersonViewModel();
                PersonEdit.PropertyChanged += OnPersonEditPropertyChanged;
                IsEditing = true;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return !IsEditing;
            });

        ···

    }

    void OnPersonEditPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        (SubmitCommand as Command).ChangeCanExecute();
    }

    void RefreshCanExecutes()
    {
        (NewCommand as Command).ChangeCanExecute();
        (SubmitCommand as Command).ChangeCanExecute();
        (CancelCommand as Command).ChangeCanExecute();
    }

    ···

}
```

Когда пользователь щелкает кнопку **Создать**, выполняется функция `execute`, переданная конструктору `Command`. При этом создается новый объект `PersonViewModel`, задается обработчик для события `PropertyChanged` этого объекта, для `IsEditing` устанавливается значение `true` и вызывается метод `RefreshCanExecutes`, определенный после конструктора.

Помимо реализации интерфейса `ICommand`, класс `Command` определяет метод с именем `ChangeCanExecute`. Ваша модель представления должна вызывать `ChangeCanExecute` для свойства `ICommand` всякий раз, когда происходит то, что может изменить возвращаемое значение метода `CanExecute`. Вызов `ChangeCanExecute` заставляет класс `Command` активировать метод `CanExecuteChanged`. Объект `Button` присоединяет обработчик для этого события и реагирует повторным вызовом `CanExecute`, а затем включается в зависимости от возвращаемого значения этого метода.

Когда метод `execute``NewCommand` вызывает `RefreshCanExecutes`, свойство `NewCommand` получает вызов `ChangeCanExecute`, а `Button` вызывает метод `canExecute`, который теперь возвращает `false`, поскольку свойство `IsEditing` теперь имеет значение `true`.

Обработчик `PropertyChanged` для нового объекта `PersonViewModel` вызывает метод `ChangeCanExecute` объекта `SubmitCommand`. Вот как реализуется свойство Command:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        SubmitCommand = new Command(
            execute: () =>
            {
                Persons.Add(PersonEdit);
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return PersonEdit != null &&
                       PersonEdit.Name != null &&
                       PersonEdit.Name.Length > 1 &&
                       PersonEdit.Age > 0;
            });

        ···
    }

    ···

}
```

Функция `canExecute` для `SubmitCommand` вызывается каждый раз при изменении свойства в редактируемом объекте `PersonViewModel`. Она возвращает `true`, только когда свойство `Name` имеет по крайней мере один символ, а `Age` больше 0. В этот момент кнопка **Отправить** становится доступной.

Функция `execute` для кнопки **Отправить** удаляет обработчик, измененный свойством, из `PersonViewModel`, добавляет объект в коллекцию `Persons` и возвращает все в исходное состояние.

Функция `execute` для кнопки **Отмена** делает все то же самое, что и кнопка **Отправить**, но не добавляет объект в коллекцию:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        CancelCommand = new Command(
            execute: () =>
            {
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return IsEditing;
            });
    }

    ···

}
```

Метод `canExecute` возвращает `true` каждый раз при редактировании `PersonViewModel`.

Эти методы можно адаптировать и для более сложных сценариев. Свойство в `PersonCollectionViewModel` можно привязать к свойству `SelectedItem` объекта `ListView` для редактирования существующих элементов. Также можно добавить кнопку **Удалить** для удаления этих элементов.

Не обязательно определять методы `execute` и `canExecute` как лямбда-функции. Можно написать их как обычные закрытые методы в модели представления и ссылаться на них в конструкторах `Command`. Но этот подход обычно приводит к появлению множества методов, на которые ссылаются только один раз в модели представления.

## <a name="using-command-parameters"></a>Использование параметров команды

Иногда бывает удобно для одной или нескольких кнопок (или других объектов пользовательского интерфейса) использовать одно и то же свойство `ICommand` в модели представления. В этом случае вы используете свойство `CommandParameter` для различия между кнопками.

Вы можете продолжать использовать класс `Command` для таких общих свойств `ICommand`. Этот класс определяет [альтернативный конструктор](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean})), принимающий методы `execute` и `canExecute` с параметрами типа `Object`. Вот как `CommandParameter` передается этим методам.

Тем не менее при использовании `CommandParameter` проще всего использовать универсальный класс [`Command<T>`](xref:Xamarin.Forms.Command`1), чтобы указать тип объекта, установленный в `CommandParameter`. Указываемые вами методы `execute` и `canExecute` имеют параметры этого типа.

Страница **Десятичная клавиатура** иллюстрирует этот способ, показывая, как реализовать клавиатуру для ввода десятичных чисел. `BindingContext` для объекта `Grid` — `DecimalKeypadViewModel`. Свойство `Entry` этой модели представления привязано к свойству `Text` объекта `Label`. Все объекты `Button` привязаны к различным командам в модели представления: `ClearCommand`, `BackspaceCommand`, и `DigitCommand`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.DecimalKeypadPage"
             Title="Decimal Keyboard">

    <Grid WidthRequest="240"
          HeightRequest="480"
          ColumnSpacing="2"
          RowSpacing="2"
          HorizontalOptions="Center"
          VerticalOptions="Center">

        <Grid.BindingContext>
            <local:DecimalKeypadViewModel />
        </Grid.BindingContext>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Button">
                    <Setter Property="FontSize" Value="32" />
                    <Setter Property="BorderWidth" Value="1" />
                    <Setter Property="BorderColor" Value="Black" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Label Text="{Binding Entry}"
               Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3"
               FontSize="32"
               LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center"
               HorizontalTextAlignment="End" />

        <Button Text="CLEAR"
                Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding ClearCommand}" />

        <Button Text="&#x21E6;"
                Grid.Row="1" Grid.Column="2"
                Command="{Binding BackspaceCommand}" />

        <Button Text="7"
                Grid.Row="2" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="7" />

        <Button Text="8"
                Grid.Row="2" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="8" />

        <Button Text="9"
                Grid.Row="2" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="9" />

        <Button Text="4"
                Grid.Row="3" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="4" />

        <Button Text="5"
                Grid.Row="3" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="5" />

        <Button Text="6"
                Grid.Row="3" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="6" />

        <Button Text="1"
                Grid.Row="4" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="1" />

        <Button Text="2"
                Grid.Row="4" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="2" />

        <Button Text="3"
                Grid.Row="4" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="3" />

        <Button Text="0"
                Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding DigitCommand}"
                CommandParameter="0" />

        <Button Text="&#x00B7;"
                Grid.Row="5" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="." />
    </Grid>
</ContentPage>
```

11 кнопок для 10 цифр и десятичного разделителя имеют общую привязку к `DigitCommand`. `CommandParameter` различает эти кнопки. Значение, заданное в `CommandParameter`, обычно совпадает с текстом на кнопке, за исключением десятичного разделителя, который для большей ясности отображается как точка посередине.

Вот программа в действии:

[![Десятичная клавиатура](commanding-images/decimalkeyboard-small.png "Десятичная клавиатура")](commanding-images/decimalkeyboard-large.png#lightbox "Десятичная клавиатура")

Обратите внимание, что кнопки для десятичного разделителя на всех трех снимках экрана отключены, поскольку введенное число уже содержит разделитель.

`DecimalKeypadViewModel` определяет свойство `Entry` типа `string` (которое является единственным свойством, вызывающим событие `PropertyChanged`) и три свойства типа `ICommand`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{
    string entry = "0";

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public string Entry
    {
        private set
        {
            if (entry != value)
            {
                entry = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Entry"));
            }
        }
        get
        {
            return entry;
        }
    }

    public ICommand ClearCommand { private set; get; }

    public ICommand BackspaceCommand { private set; get; }

    public ICommand DigitCommand { private set; get; }
}
```

Кнопка, соответствующая `ClearCommand`, всегда активна и просто возвращает запись обратно в значение 0:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {
        ClearCommand = new Command(
            execute: () =>
            {
                Entry = "0";
                RefreshCanExecutes();
            });

        ···

    }

    void RefreshCanExecutes()
    {
        ((Command)BackspaceCommand).ChangeCanExecute();
        ((Command)DigitCommand).ChangeCanExecute();
    }

    ···

}
```

Поскольку кнопка всегда активна, не нужно указывать аргумент `canExecute` в конструкторе `Command`.

Логика для ввода и удаления чисел немного сложна, поскольку, если числа не введены, свойство `Entry` представляет собой строку "0". Если пользователь вводит дополнительные нули, `Entry` по-прежнему содержит только один ноль. Если пользователь вводит любую другую цифру, эта цифра заменяет ноль. Но если пользователь вводит десятичный разделитель до другой цифры, `Entry` становится строкой "0.".

Кнопка **Backspace** доступна только в том случае, если длина записи больше 1 или если `Entry` не равно строке "0":

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        BackspaceCommand = new Command(
            execute: () =>
            {
                Entry = Entry.Substring(0, Entry.Length - 1);
                if (Entry == "")
                {
                    Entry = "0";
                }
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return Entry.Length > 1 || Entry != "0";
            });

        ···

    }

    ···

}
```

Логика для функции `execute` для кнопки **Backspace** гарантирует, что `Entry` — это по крайней мере строка "0".

Свойство `DigitCommand` привязано к 11 кнопкам, каждая из которых определяет себя с помощью свойства `CommandParameter`. `DigitCommand` может быть присвоено экземпляру стандартного класса `Command`, но проще использовать универсальный класс `Command<T>`. При использовании командного интерфейса с XAML свойства `CommandParameter` обычно являются строками, и это тип универсального аргумента. Функции `execute` и `canExecute` получают аргументы типа `string`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        DigitCommand = new Command<string>(
            execute: (string arg) =>
            {
                Entry += arg;
                if (Entry.StartsWith("0") && !Entry.StartsWith("0."))
                {
                    Entry = Entry.Substring(1);
                }
                RefreshCanExecutes();
            },
            canExecute: (string arg) =>
            {
                return !(arg == "." && Entry.Contains("."));
            });
    }

    ···

}
```

Метод `execute` добавляет строковый аргумент в свойство `Entry`. Тем не менее, если результат начинается с нуля (но не с нуля с десятичным разделителем), начальный ноль необходимо удалить с помощью функции `Substring`.

Метод `canExecute` возвращает `false` только в том случае, если аргумент является десятичным разделителем (указывая, что десятичный разделитель нажат) и `Entry` уже содержит десятичный разделитель.

Все методы `execute` вызывают `RefreshCanExecutes`, который затем вызывает `ChangeCanExecute` для `DigitCommand` и `ClearCommand`. Это гарантирует, что кнопки десятичного разделителя и удаления включаются и отключаются в соответствии с текущей последовательностью введенных цифр.

## <a name="asynchronous-commanding-for-navigation-menus"></a>Асинхронные команды для меню навигации

Команды удобны для реализации меню навигации, как, например, в самой программе [**Демоверсии привязок данных**](/samples/xamarin/xamarin-forms-samples/databindingdemos). Вот часть **MainPage.xaml**:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MainPage"
             Title="Data Binding Demos"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection Title="Basic Bindings">

                <TextCell Text="Basic Code Binding"
                          Detail="Define a data-binding in code"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicCodeBindingPage}" />

                <TextCell Text="Basic XAML Binding"
                          Detail="Define a data-binding in XAML"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicXamlBindingPage}" />

                <TextCell Text="Alternative Code Binding"
                          Detail="Define a data-binding in code without a BindingContext"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:AlternativeCodeBindingPage}" />

                ···

            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

При использовании команд с XAML свойства `CommandParameter` обычно задаются как строки. В этом случае, однако, используется расширение разметки XAML, чтобы `CommandParameter` имел тип `System.Type`.

Каждое свойство `Command` привязано к свойству с именем `NavigateCommand`. Это свойство определено в файле с выделенным кодом **MainPage.xaml.cs**:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(
            async (Type pageType) =>
            {
                Page page = (Page)Activator.CreateInstance(pageType);
                await Navigation.PushAsync(page);
            });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Конструктор присваивает свойство `NavigateCommand` методу `execute`, который создает экземпляр параметра `System.Type` и переходит к нему. Так как вызов `PushAsync` требует оператор `await`, метод `execute` должен быть помечен как асинхронный. Это осуществляется с помощью ключевого слова `async` перед списком параметров.

Конструктор также задает `BindingContext` страницы на себя, чтобы привязки ссылались на `NavigateCommand` в этом классе.

Порядок кода в этом конструкторе имеет значение: вызов `InitializeComponent` приводит к анализу XAML, но в этот момент привязка к свойству с именем `NavigateCommand` не может быть разрешена, так как для `BindingContext` установлено `null`. Если `BindingContext` задается в конструкторе *перед* тем, как задается `NavigateCommand`, привязка может быть разрешена, когда задается `BindingContext`, но в этот момент `NavigateCommand` по-прежнему имеет значение `null`. Если задать `NavigateCommand` после `BindingContext`, это не повлияет на привязку, так как изменение `NavigateCommand` не вызывает событие `PropertyChanged` и привязка не знает, что `NavigateCommand` теперь считается допустимым.

Если задать `NavigateCommand` и `BindingContext` (в любом порядке) до вызова `InitializeComponent`, это сработает, так как оба компонента привязки задаются, когда средство синтаксического анализа XAML обнаруживает определение привязки.

Иногда в привязках данных сложно разобраться, но, как вы убедились в этой серии статей, это удобный и универсальный инструмент, который очень помогает упорядочить код путем отделения базовой логики от пользовательского интерфейса.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Глава, посвященная привязкам данных, из книги о Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)