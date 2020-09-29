---
title: Элемент управления средства выбора в Xamarin. iOS
description: В этом документе описывается разработка элементов управления средства выбора в приложении Xamarin. iOS и работа с ними. В нем обсуждается реализация средства выбора в коде и в конструкторе iOS.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/14/2018
ms.openlocfilehash: 108d7752c51a6dffd28107b87d29198443827ed2
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436856"
---
# <a name="picker-control-in-xamarinios"></a>Элемент управления средства выбора в Xamarin. iOS

[`UIPickerView`](xref:UIKit.UIPickerView)Позволяет выбрать значение из списка путем прокрутки отдельных компонентов интерфейса, похожего на круг.

Выборки часто используются для выбора даты и времени. Apple предоставляет [`UIDatePicker`](xref:UIKit.UIDatePicker)
класс для этой цели.

В этой статье описывается, как реализовать и использовать `UIPickerView` `UIDatePicker` элементы управления и.

## <a name="uipickerview"></a>UIPickerView

### <a name="implementing-a-picker"></a>Реализация средства выбора

Реализация средства выбора путем создания нового экземпляра `UIPickerView` :

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width, 
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
    )
);
```

### <a name="pickers-and-storyboards"></a>Выборки и раскадровки

Чтобы создать средство выбора в **конструкторе iOS**, перетащите **представление средства выбора** из **области элементов** в область конструктора.

![Перетащите представление средства выбора в область конструктора](picker-images/image1.png "Перетащите представление средства выбора в область конструктора")

### <a name="working-with-a-picker-control"></a>Работа с элементом управления "выборка"

Средство выбора использует _модель_ для взаимодействия с данными:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

[`UIPickerViewModel`](xref:UIKit.UIPickerViewModel)Базовый класс реализует два интерфейса,[`IUIPickerDataSource`](xref:UIKit.IUIPickerViewDataSource)
и [`IUIPickerViewDelegate`](xref:UIKit.IUIPickerViewDelegate) , объявляющие различные методы, определяющие данные средства выбора и способ обработки взаимодействия:

```csharp
public class PeopleModel : UIPickerViewModel
{
    public string[] names = new string[] {
            "Amy Burns",
            "Kevin Mullins",
            "Craig Dunn",
            "Joel Martinez",
            "Charles Petzold",
            "David Britch",
            "Mark McLemore",
            "Tom Opegenorth",
            "Joseph Hill",
            "Miguel De Icaza"
        };

    private UILabel personLabel;

    public PeopleModel(UILabel personLabel)
    {
        this.personLabel = personLabel;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 2;
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return names.Length;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        if (component == 0)
            return names[row];
        else
            return row.ToString();
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        personLabel.Text = $"This person is: {names[pickerView.SelectedRowInComponent(0)]},\n they are number {pickerView.SelectedRowInComponent(1)}";
    }

    public override nfloat GetComponentWidth(UIPickerView picker, nint component)
    {
        if (component == 0)
            return 240f;
        else
            return 40f;
    }

    public override nfloat GetRowHeight(UIPickerView picker, nint component)
    {
        return 40f;
    }
```

В средстве выбора может быть несколько столбцов или _компонентов_. Компоненты разделяют средство выбора на несколько разделов, позволяя упростить и выбрать конкретные данные:

![Средство выбора с двумя компонентами](picker-images/image3.png "Средство выбора с двумя компонентами")

Чтобы указать количество компонентов в средстве выбора, используйте [`GetComponentCount`](xref:UIKit.UIPickerViewModel.GetComponentCount(UIKit.UIPickerView)) 
метод.

### <a name="customizing-a-pickers-appearance"></a>Настройка внешнего вида средства выбора

Чтобы настроить внешний вид средства выбора, используйте [`UIPickerView.UIPickerViewAppearance`](xref:UIKit.UIPickerView.UIPickerViewAppearance)
класс или переопределить [`GetView`](xref:UIKit.UIPickerViewModel.GetView(UIKit.UIPickerView,System.nint,System.nint,UIKit.UIView)) методы и [`GetRowHeight`](xref:UIKit.UIPickerViewModel.GetRowHeight(UIKit.UIPickerView,System.nint)) в `UIPickerViewModel` .

## <a name="uidatepicker"></a>уидатепиккер

### <a name="implementing-a-date-picker"></a>Реализация элемента выбора даты

Реализация средства выбора даты путем создания экземпляра `UIDatePicker` :

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width,
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
     )
);
```

### <a name="date-pickers-and-storyboards"></a>Выбор даты и раскадровки

Чтобы создать элемент выбора даты в **конструкторе iOS**, перетащите элемент **выбора даты** с **панели элементов** в область конструктора.

![Перетащите элемент выбора даты в область конструктора](picker-images/image2.png "Перетащите элемент выбора даты в область конструктора")

### <a name="date-picker-properties"></a>Свойства выбора даты

#### <a name="minimum-and-maximum-date"></a>Минимальная и максимальная дата

[`MinimumDate`](xref:UIKit.UIDatePicker.MinimumDate) и [`MaximumDate`](xref:UIKit.UIDatePicker.MaximumDate) Ограничьте диапазон дат, доступных в средстве выбора даты. Например, следующий код ограничивает выбор даты до 60 лет, ведущих до текущего момента:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();
components.Year = -60;
NSDate minDate = calendar.DateByAddingComponents(components, currentDate, NSCalendarOptions.None);
datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = currentDate;
```

> [!TIP]
> Можно явно привести `DateTime` к типу `NSDate` :
>
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>Интервал в минутах

[`MinuteInterval`](xref:UIKit.UIDatePicker.MinuteInterval)Свойство задает интервал времени, в течение которого средство выбора будет отображать минуты:

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>Режим

Выбор даты поддерживает четыре [режима](xref:UIKit.UIDatePickerMode), описанные ниже.

##### <a name="uidatepickermodetime"></a>Уидатепиккермоде. время

`UIDatePickerMode.Time` Отображает время с селектором часов и минут, а также необязательное обозначение AM или PM.

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![Уидатепиккермоде. время](picker-images/image8.png "Уидатепиккермоде. время")

##### <a name="uidatepickermodedate"></a>Уидатепиккермоде. Дата

`UIDatePickerMode.Date` Отображает дату с селектором "месяц, день" и "год":

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![Уидатепиккермоде. Дата](picker-images/image7.png "Уидатепиккермоде. Дата")

Порядок селекторов зависит от языкового стандарта выбора даты, который по умолчанию использует язык системы. На приведенном выше рисунке показана структура селекторов в `en_US` языковом стандарте, но следующий порядок меняется на день | Месяц | Год

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![День | Месяц | Год](picker-images/image9.png "День | Месяц | Год")

##### <a name="uidatepickermodedateandtime"></a>Уидатепиккермоде. Датеандтиме

`UIDatePickerMode.DateAndTime` Отображает сокращенное представление даты, время в часах и минутах, а также необязательное обозначение AM или PM (в зависимости от того, используется ли 12-или 24-часовой формат):

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![Уидатепиккермоде. Датеандтиме](picker-images/image6.png "Уидатепиккермоде. Датеандтиме")

Как [`UIDatePickerMode.Date`](#uidatepickermodedate) и в случае, порядок селекторов и использование 12 или 24 часового времени зависят от языкового стандарта выбора даты.

> [!TIP]
> Используйте `Date` свойство для записи значения элемента выбора даты в режиме `UIDatePickerMode.Time` , `UIDatePickerMode.Date` или `UIDatePickerMode.DateAndTime` . Это значение хранится в виде `NSDate` .

##### <a name="uidatepickermodecountdowntimer"></a>Уидатепиккермоде. Каунтдовнтимер

`UIDatePickerMode.CountDownTimer` Отображает значения часов и минут:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

!["Уидатепиккермоде. Каунтдовнтимер"](picker-images/image5.png "Уидатепиккермоде. Каунтдовнтимер")

`CountDownDuration`Свойство захватывает значение элемента выбора даты в `UIDatePickerMode.CountDownTimer` режиме. Например, чтобы добавить значение обратного отсчета к текущей дате:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>нсдатеформаттер

Чтобы отформатировать `NSDate` , используйте [`NSDateFormatter`](xref:Foundation.NSDateFormatter) .

Чтобы использовать `NSDateFormatter` , вызовите его [`ToString`](xref:Foundation.NSDateFormatter.ToString(Foundation.NSDate)) метод. Пример:

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

[`DateFormat`](xref:Foundation.NSDateFormatter.DateFormat)Свойство (строка) `NSDateFormatter` позволяет задать настраиваемую спецификацию формата даты:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>тиместиле

[`TimeStyle`](xref:Foundation.NSDateFormatter.TimeStyle)Свойство ( [`NSDateFormatterStyle`](xref:Foundation.NSDateFormatterStyle) `NSDateFormatter` определяет форматирование времени на основе предварительно заданных стилей:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Различные `NSDateFormatterStyle` значения отображают время следующим образом:

- `NSDateFormatterStyle.Full`: 7:46:00 по восточному времени (лето)
- `NSDateFormatterStyle.Long`: 7:47:00. EDT
- `NSDateFormatterStyle.Medium`: 7:47:00 РМ
- `NSDateFormatterSytle.Short`: 7:47 РМ

##### <a name="datestyle"></a>датестиле

[`DateStyle`](xref:Foundation.NSDateFormatter.DateStyle)Свойство ( `NSDateFormatterStyle` ) `NSDateFormatter` задает форматирование даты на основе предварительно заданных стилей:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Различные `NSDateFormatterStyle` значения отображают даты следующим образом:

- `NSDateFormatterStyle.Full`: Среда, 2 августа, 2017 в 7:48 PM
- `NSDateFormatterStyle.Long`: 2 августа 2017 at 7:49 PM
- `NSDateFormatterStyle.Medium`: Август 2, 2017, 7:49 PM
- `NSDateFormatterStyle.Short`: 8/2/17, 7:50 PM

> [!NOTE]
> `DateFormat`и `DateStyle` / `TimeStyle` предоставляют различные способы указания форматирования даты и времени. Последние свойства Set определяют выходные данные модуля форматирования даты.

## <a name="related-links"></a>Связанные ссылки

- [Пиккерконтрол (пример)](/samples/xamarin/ios-samples/pickercontrol)