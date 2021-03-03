---
title: Xamarin.Forms TimePicker
description: TimePicker — это Xamarin.Forms представление, позволяющее пользователю выбрать время. В этой статье объясняется, как использовать TimePicker в Xamarin.Forms приложении.
ms.prod: xamarin
ms.assetid: 2E99FB23-B82D-4EB4-AFB3-5002E736E7B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2f8452ea36d6fe376880b8882652c59fcb2ed23b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93371889"
---
# <a name="xamarinforms-timepicker"></a>Xamarin.Forms TimePicker

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)

_Xamarin.FormsПредставление, позволяющее пользователю выбрать время._

Xamarin.Forms [`TimePicker`](xref:Xamarin.Forms.TimePicker) Компонент вызывает элемент управления средства выбора времени платформы и позволяет пользователю выбрать время. `TimePicker` определяет следующие свойства:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) типа `TimeSpan` — выбранное время, которое по умолчанию равно `TimeSpan` 0. `TimeSpan`Тип обозначает длительность времени, начиная с полуночи.
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) типа `string` — [Стандартная](/dotnet/standard/base-types/standard-date-and-time-format-strings/) или [Пользовательская](/dotnet/standard/base-types/custom-date-and-time-format-strings/) строка форматирования .NET, которая по умолчанию имеет значение t, то есть короткий шаблон времени.
- [`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor) типа [`Color`](xref:Xamarin.Forms.Color) — цвет, используемый для показа выбранного времени, по умолчанию — [`Color.Default`](xref:Xamarin.Forms.Color.Default) .
- [`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes) типа [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) , значение по умолчанию — [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) .
- [`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily) типа `string` , значение по умолчанию — `null` .
- [`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize) типа `double` , значение по умолчанию — 1,0.
- `CharacterSpacing` с типом `double` представляет собой интервал между знаками текста `TimePicker`.

Все эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, то есть они могут быть стилями, а свойства могут быть целями привязок данных. [`Time`](xref:Xamarin.Forms.TimePicker.Time)Свойство имеет режим привязки по умолчанию [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) , то есть может быть целевым объектом привязки данных в приложении, использующем архитектуру [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) .

[`TimePicker`](xref:Xamarin.Forms.TimePicker)Не включает событие для указания нового выбранного [`Time`](xref:Xamarin.Forms.TimePicker.Time) значения. Если необходимо получать уведомления об этом, можно добавить обработчик для [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) события.

## <a name="initializing-the-time-property"></a>Инициализация свойства времени

В коде можно инициализировать [`Time`](xref:Xamarin.Forms.TimePicker.Time) свойство значением типа `TimeSpan` :

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

Если [`Time`](xref:Xamarin.Forms.TimePicker.Time) свойство указано в XAML, значение преобразуется в `TimeSpan` и проверяется, чтобы убедиться, что число миллисекунд больше или равно 0, а число часов меньше 24. Компоненты времени должны быть разделены двоеточиями:

```xaml
<TimePicker Time="4:15:26" />
```

Если [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) свойство объекта [`TimePicker`](xref:Xamarin.Forms.TimePicker) имеет значение экземпляра ViewModel, содержащего свойство типа `TimeSpan` с именем `SelectedTime` (например,), можно создать экземпляр `TimePicker` следующим образом:

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

В этом примере [`Time`](xref:Xamarin.Forms.TimePicker.Time) свойство инициализируется `SelectedTime` свойством в ViewModel. Так как `Time` свойство имеет режим привязки [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) , любое новое время, выбранное пользователем, автоматически распространяется на ViewModel.

Если [`TimePicker`](xref:Xamarin.Forms.TimePicker) свойство не содержит привязки к его [`Time`](xref:Xamarin.Forms.TimePicker.Time) свойству, приложение должно присоединить обработчик к [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) событию, чтобы получать уведомления о том, что пользователь выбрал новое время.

Дополнительные сведения о настройке свойств шрифта см. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="timepicker-and-layout"></a>TimePicker и макет

Можно использовать параметр неограниченного горизонтального макета `Center` , например,, `Start` или `End` с помощью [`TimePicker`](xref:Xamarin.Forms.TimePicker) :

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

Однако это не рекомендуется. В зависимости от значения [`Format`](xref:Xamarin.Forms.TimePicker.Format) свойства для выбранного времени может потребоваться другая ширина дисплея. Например, строка формата "T" заставляет [`TimePicker`](xref:Xamarin.Forms.TimePicker) представление отображать время в длинном формате, а "4:15:26 AM" требует больше ширины отображения, чем короткий формат времени ("T") "4:15 AM". В зависимости от платформы это различие может привести к `TimePicker` изменению ширины представления в макете или при усечении изображения.

> [!TIP]
> Лучше использовать параметр по умолчанию `HorizontalOptions` `Fill` со значением [`TimePicker`](xref:Xamarin.Forms.TimePicker) , а не использовать ширину `Auto` при помещении `TimePicker` `Grid` ячейки.

## <a name="timepicker-in-an-application"></a>TimePicker в приложении

Образец [**сеттимер**](/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) включает [`TimePicker`](xref:Xamarin.Forms.TimePicker) представления, [`Entry`](xref:Xamarin.Forms.Entry) и [`Switch`](xref:Xamarin.Forms.Switch) на странице. `TimePicker`Можно использовать для выбора времени, а при возникновении этого времени отображается диалоговое окно предупреждения, напоминающее пользователю текст в `Entry` , при условии, что параметр `Switch` включен. Вот файл XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SetTimer"
             x:Class="SetTimer.MainPage">
    <StackLayout>
        ...
        <Entry x:Name="_entry"
               Placeholder="Enter event to be reminded of" />
        <Label Text="Select the time below to be reminded at." />
        <TimePicker x:Name="_timePicker"
                    Time="11:00:00"
                    Format="T"
                    PropertyChanged="OnTimePickerPropertyChanged" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Enable timer:" />
            <Switch x:Name="_switch"
                    HorizontalOptions="EndAndExpand"
                    Toggled="OnSwitchToggled" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

[`Entry`](xref:Xamarin.Forms.Entry)Позволяет ввести текст напоминания, который будет отображаться при наступлении выбранного времени. [`TimePicker`](xref:Xamarin.Forms.TimePicker)Свойству присваивается [`Format`](xref:Xamarin.Forms.TimePicker.Format) значение T для длинного формата времени. Он имеет обработчик событий, присоединенный к событию [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) , а [`Switch`](xref:Xamarin.Forms.Switch) имеет обработчик, присоединенный к его [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) событию. Эти обработчики событий находятся в файле кода программной части и вызывают `SetTriggerTime` метод:

```csharp
public partial class MainPage : ContentPage
{
    DateTime _triggerTime;

    public MainPage()
    {
        InitializeComponent();

        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimerTick);
    }

    bool OnTimerTick()
    {
        if (_switch.IsToggled && DateTime.Now >= _triggerTime)
        {
            _switch.IsToggled = false;
            DisplayAlert("Timer Alert", "The '" + _entry.Text + "' timer has elapsed", "OK");
        }
        return true;
    }

    void OnTimePickerPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        if (args.PropertyName == "Time")
        {
            SetTriggerTime();
        }
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        SetTriggerTime();
    }

    void SetTriggerTime()
    {
        if (_switch.IsToggled)
        {
            _triggerTime = DateTime.Today + _timePicker.Time;
            if (_triggerTime < DateTime.Now)
            {
                _triggerTime += TimeSpan.FromDays(1);
            }
        }
    }
}
```

`SetTriggerTime`Метод вычисляет время таймера на основе `DateTime.Today` значения свойства и `TimeSpan` значения, возвращаемого из [`TimePicker`](xref:Xamarin.Forms.TimePicker) . Это необходимо `DateTime.Today` , поскольку свойство возвращает значение типа, `DateTime` указывающее текущую дату, но с временем полуночи. Если время таймера уже прошло сегодня, предполагается, что он будет завтра.

Таймер записывает каждую секунду, выполняя `OnTimerTick` метод, который проверяет, включен ли параметр, [`Switch`](xref:Xamarin.Forms.Switch) и является ли текущее время большим или равным времени таймера. При наступлении времени таймера [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) метод отображает диалоговое окно предупреждения пользователю в виде напоминания.

При первом запуске образца [`TimePicker`](xref:Xamarin.Forms.TimePicker) представление инициализируется значением 11:00. При нажатии кнопки `TimePicker` вызывается средство выбора времени платформы. Платформы реализуют средство выбора времени различными способами, но каждый подход знаком пользователям этой платформы:

[![Выбрать время](timepicker-images/timepicker-open.png "Выбрать время")](timepicker-images/timepicker-open-large.png#lightbox "Выбрать время")

> [!TIP]
> В Android [`TimePicker`](xref:Xamarin.Forms.TimePicker) диалоговое окно можно настроить, переопределив `CreateTimePickerDialog` метод в пользовательском модуле подготовки отчетов. Это позволяет, например, добавить дополнительные кнопки в диалоговое окно.

После выбора времени в поле отображается выбранное время [`TimePicker`](xref:Xamarin.Forms.TimePicker) .

[![Выбранное время](timepicker-images/timepicker-selected.png "Выбранное время")](timepicker-images/timepicker-selected-large.png#lightbox "Выбранное время")

При условии, что параметр [`Switch`](xref:Xamarin.Forms.Switch) переключается на положение ON, приложение отображает диалоговое окно предупреждения с напоминанием пользователя о тексте в, [`Entry`](xref:Xamarin.Forms.Entry) когда происходит выбранное время:

[![Всплывающее окно таймера](timepicker-images/timer-test.png "Всплывающее окно таймера")](timepicker-images/timer-test-large.png#lightbox "Всплывающее окно таймера")

Как только откроется диалоговое окно оповещения, параметр переключается [`Switch`](xref:Xamarin.Forms.Switch) в положение Выкл.

## <a name="related-links"></a>Связанные ссылки

- [Пример Сеттимер](/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)
- [API TimePicker](xref:Xamarin.Forms.TimePicker)