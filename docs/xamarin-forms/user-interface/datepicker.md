---
title: Xamarin.Forms DatePicker
description: DatePicker — это Xamarin.Forms представление, позволяющее пользователю выбрать дату. В этой статье объясняется, как использовать DatePicker в Xamarin.Forms приложении.
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f7cb79b5b90d586980f2e74cd6d2f04b82a8fdf3
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373696"
---
# <a name="no-locxamarinforms-datepicker"></a>Xamarin.Forms DatePicker

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)

_Xamarin.FormsПредставление, позволяющее пользователю выбрать дату._

Xamarin.Forms [`DatePicker`](xref:Xamarin.Forms.DatePicker) Вызывает элемент управления "Выбор даты" платформы и позволяет пользователю выбрать дату. `DatePicker` Определяет восемь свойств:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) типа [`DateTime`](xref:System.DateTime) , значение которого по умолчанию равно первому дню года 1900.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) типа `DateTime` , значение которого по умолчанию равно последнему дню года 2100.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) типа `DateTime` — Выбранная дата, которая по умолчанию имеет значение [`DateTime.Today`](xref:System.DateTime.Today) .
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) типа `string` — [Стандартная](/dotnet/standard/base-types/standard-date-and-time-format-strings/) или [Настраиваемая](/dotnet/standard/base-types/custom-date-and-time-format-strings/) строка форматирования .NET, которая по умолчанию имеет значение D, шаблон длинной даты.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) для типа [`Color`](xref:Xamarin.Forms.Color) — цвет, используемый для показа выбранной даты, по умолчанию — [`Color.Default`](xref:Xamarin.Forms.Color.Default) .
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) типа [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) , значение по умолчанию — [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) .
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) типа `string` , значение по умолчанию — `null` .
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) типа `double` , значение по умолчанию — 1,0.
- `CharacterSpacing` с типом `double` представляет собой интервал между знаками текста `DatePicker`.

`DatePicker`Запускает событие, [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) когда пользователь выбирает дату.

> [!WARNING]
> При установке `MinimumDate` и `MaximumDate` Убедитесь, что `MinimumDate` всегда меньше или равно `MaximumDate` . В противном случае `DatePicker` вызовет исключение.

На внутреннем уровне `DatePicker` гарантирует, что `Date` находится в диапазоне от `MinimumDate` до `MaximumDate` включительно. Если параметр `MinimumDate` или `MaximumDate` задан так, что `Date` не находится между ними, `DatePicker` будет корректировать значение `Date` .

Все восемь свойств поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, то есть они могут быть стилями, а свойства могут быть целями привязок данных. `Date`Свойство имеет режим привязки по умолчанию [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) , то есть может быть целевым объектом привязки данных в приложении, использующем архитектуру [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) .

## <a name="initializing-the-datetime-properties"></a>Инициализация свойств DateTime

В коде можно инициализировать `MinimumDate` `MaximumDate` свойства, и `Date` для значений типа `DateTime` :

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Если `DateTime` значение указано в XAML, средство синтаксического анализа XAML использует `DateTime.Parse` метод с `CultureInfo.InvariantCulture` аргументом для преобразования строки в `DateTime` значение. Даты должны быть указаны в точном формате: двузначное число месяцев, две цифры и годы, состоящие из четырех цифр, разделенных косыми чертами:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Если `BindingContext` свойство объекта `DatePicker` задано для экземпляра ViewModel, содержащего свойства типа `DateTime` с именем `MinDate` , `MaxDate` и `SelectedDate` (например,), можно создать экземпляр `DatePicker` следующим образом:

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

В этом примере все три свойства инициализируются с соответствующими свойствами в ViewModel. Поскольку `Date` свойство имеет режим привязки `TwoWay` , любая новая дата, выбираемая пользователем, автоматически отражается в ViewModel.

Если `DatePicker` свойство не содержит привязки к его `Date` свойству, приложение должно присоединить обработчик к `DateSelected` событию, чтобы получать уведомления о том, что пользователь выбирает новую дату.

Дополнительные сведения о настройке свойств шрифта см. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>DatePicker и макет

Можно использовать параметр неограниченного горизонтального макета `Center` , например,, `Start` или `End` с помощью `DatePicker` :

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Однако это не рекомендуется. В зависимости от настройки `Format` свойства для выбранных дат могут потребоваться разные значения ширины экрана. Например, строка формата "D" приводит `DateTime` к отображению дат в длинном формате, а "Среда, 12 сентября, 2018" требует больше ширины экрана, чем "Пятница, Май 4, 2018". В зависимости от платформы это различие может привести к `DateTime` изменению ширины представления в макете или при усечении изображения.

> [!TIP]
> Лучше использовать параметр по умолчанию `HorizontalOptions` `Fill` со значением `DatePicker` , а не использовать ширину `Auto` при помещении `DatePicker` `Grid` ячейки.

## <a name="datepicker-in-an-application"></a>DatePicker в приложении

Образец [**дайсбетвиндатес**](/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) включает два `DatePicker` представления на странице. Их можно использовать для выбора двух дат, и программа вычисляет количество дней между этими датами. Программа не изменяет параметры `MinimumDate` `MaximumDate` свойств и, поэтому две даты должны находиться в диапазоне от 1900 до 2100.

Вот файл XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Каждому `DatePicker` `Format` свойству присваивается свойство «D» для длинного формата даты. Обратите внимание, что `endDatePicker` объект имеет привязку, предназначенную для его `MinimumDate` Свойства. Источником привязки является выбранное `Date` свойство `startDatePicker` объекта. Это гарантирует, что Дата окончания всегда будет позже или равна дате начала. Помимо двух объектов, помечена как `DatePicker` `Switch` «включить оба дня в итог».

Эти два `DatePicker` представления имеют обработчики, прикрепленные к `DateSelected` событию, а к `Switch` его `Toggled` событию привязан обработчик. Эти обработчики событий находятся в файле кода программной части и активируют новое вычисление дней между двумя датами:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

При первом запуске образца оба `DatePicker` представления инициализируются до сегодняшней даты. На следующем снимке экрана показана программа, выполняемая в iOS и Android:

[![Начальные дни между датами](datepicker-images/DaysBetweenDatesStart.png "Начальные дни между датами")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "Начальные дни между датами")

При касании любого из них `DatePicker` вызывается средство выбора даты платформы. Эти платформы реализуют этот элемент выбора даты различными способами, но каждый подход знаком пользователям этой платформы:

[![Число дней между датами SELECT](datepicker-images/DaysBetweenDatesSelect.png "Число дней между датами SELECT")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "Число дней между датами SELECT")

> [!TIP]
> В Android `DatePicker` диалоговое окно можно настроить, переопределив `CreateDatePickerDialog` метод в пользовательском модуле подготовки отчетов. Это позволяет, например, добавить дополнительные кнопки в диалоговое окно.

После выбора двух дат в приложении отображается число дней между этими датами:

[![Результат в днях между датами](datepicker-images/DaysBetweenDatesResult.png "Результат в днях между датами")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "Результат в днях между датами")

## <a name="related-links"></a>Связанные ссылки

- [Пример Дайсбетвиндатес](/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)
- [API DatePicker](xref:Xamarin.Forms.DatePicker)