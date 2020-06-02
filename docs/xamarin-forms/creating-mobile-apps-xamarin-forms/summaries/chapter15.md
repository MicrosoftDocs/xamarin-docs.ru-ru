---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 15. The interactive interface''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9bb930c2d0e4b6281b1aa48589ea5245eedc1a60
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136751"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Сводная информация о главе 15. Интерактивный интерфейс

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

В этой главе рассматриваются восемь производных `View`, которые обеспечивают взаимодействие с пользователем.

## <a name="view-overview"></a>Обзор представления

Xamarin.Forms содержит 20 классов с поддержкой создания экземпляров. Эти классы наследуются от `View`, но не от `Layout`. Шесть из них мы уже рассмотрели в предыдущих главах.

- `Label`. [**Глава 2. Анатомия приложения**](chapter02.md)
- `BoxView`. [**Глава 3. Прокрутка стека**](chapter03.md)
- `Button`. [**Глава 6. Нажатия кнопки**](chapter06.md)
- `Image`. [**Глава 13. Растровые изображения**](chapter13.md)
- `ActivityIndicator`. [**Глава 13. Растровые изображения**](chapter13.md)
- `ProgressBar`. [**Глава 14. AbsoluteLayout**](chapter14.md)

В этой главе мы обсудим еще восемь представлений, которые нужны для взаимодействия пользователей с базовыми типами данных .NET.

|Тип данных|Представления|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

Эти представления можно рассматривать как визуальные интерактивные представления для базовых типов данных. Эта концепция подробнее описана в следующей главе 16 [ **Привязка данных**](chapter16.md).

Остальные шесть представлений будут рассмотрены в следующих главах.

- `WebView`. [**Глава 16. Привязка данных**](chapter16.md)
- `Picker`. [**Глава 19. Представления коллекций**](chapter19.md)
- `ListView`. [**Глава 19. Представления коллекций**](chapter19.md)
- `TableView`. [**Глава 19. Представления коллекций**](chapter19.md)
- `Map`. [**Глава 28. Расположение и карты**](chapter28.md)
- `OpenGLView`. Не рассматривается в этой книге (и не поддерживается для платформ Windows)

## <a name="slider-and-stepper"></a>Ползунок и шаговый переключатель

Как с помощью [`Slider`](xref:Xamarin.Forms.Slider), так и [`Stepper`](xref:Xamarin.Forms.Stepper) пользователь может выбрать числовое значение из диапазона. `Slider` поддерживает непрерывный диапазон, а `Stepper` — дискретные значения.

### <a name="slider-basics"></a>Основные сведения о ползунках

[`Slider`](xref:Xamarin.Forms.Slider) представляет собой горизонтальную панель, которая отображает диапазон значений от некоторого минимума (крайнее левое положение) до максимума (крайнее правое). Он определяет три открытых свойства:

- [`Value`](xref:Xamarin.Forms.Slider.Value) с типом `double` и значением по умолчанию 0;
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) с типом `double` и значением по умолчанию 0;
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) с типом `double` и значением по умолчанию 1.

Привязываемые свойства, которые подкрепляют эти свойства, позволяют обеспечить их согласованность:

- Для всех трех свойств метод [`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate), определенный для привязываемого свойства, гарантирует, что значение `Value` всегда находится между `Minimum` и `Maximum`.
- Метод [`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) в `MinimumProperty` возвращает `false`, если для `Minimum` задано значение, большее или равное `Maximum`. То же применимо и к `MaximumProperty`. Если из метода `validateValue` возвращается значение `false`, возникает исключение `ArgumentException`.

`Slider` вызывает событие [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) с аргументом [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) при изменении свойства `Value` программными средствами или при взаимодействии пользователя с `Slider`.

В примере [**SliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) демонстрируется простой вариант использования `Slider`.

### <a name="common-pitfalls"></a>Типичные проблемы

Как в коде, так и в XAML свойства `Minimum` и `Maximum` задаются в том порядке, который вы укажете. При инициализации этих свойств следите за тем, чтобы значение `Maximum` всегда было больше `Minimum`. В противном случае возникнет исключение.

Инициализация свойств `Slider` может привести к изменению свойства `Value` и срабатыванию события `ValueChanged`. Следите за тем, чтобы обработчик событий `Slider` во время инициализации страницы не обращался к представлениям, которые еще не созданы.

Событие `ValueChanged` не срабатывает во время инициализации `Slider`, если не изменяется свойство `Value`. Вы можете вызвать обработчик `ValueChanged` напрямую из кода.

### <a name="slider-color-selection"></a>Выбор цвета с помощью ползунка

Программа [**RgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) содержит три элемента `Slider`, которые позволяют в интерактивном режиме выбирать цвет через RGB-значения:

[![Снимок экрана, на котором показаны три слайдера R, G, B](images/ch15fg03-small.png "Ползунки RGB-значений")](images/ch15fg03-large.png#lightbox "Ползунки RGB-значений")

В примере [**TextFade**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) два элемента `Slider` используются для перемещения двух элементов `Label` в области `AbsoluteLayout` и плавного перехода одного из этих элементов в другой.

### <a name="the-stepper-difference"></a>Отличия шагового переключателя

[`Stepper`](xref:Xamarin.Forms.Stepper) определяет те же свойства и события, что и `Slider`, но свойство `Maximum` здесь инициализируется значением 100, а `Stepper` определяет четвертое свойство:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) с типом `double` и начальным значением 1.

Визуально `Stepper` состоит из двух кнопок с метками **&ndash;** и **+** . Нажатие **&ndash;** уменьшает `Value` на значение `Increment` до минимума `Minimum`. Нажатие **+** увеличивает `Value` на значение `Increment` до максимума `Maximum`.

Это демонстрируется в примере [**StepperDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo).

## <a name="switch-and-checkbox"></a>Переключатель и флажок

С помощью [`Switch`](xref:Xamarin.Forms.Switch) пользователь может указать логическое значение.

### <a name="switch-basics"></a>Основные сведения о переключателях

Визуально `Switch` состоит из переключателя, который может быть выключен и включен. Этот класс определяет одно свойство.

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) типа `bool`

`Switch` определяет одно событие.

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) дополняется объектом [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs) и срабатывает при изменении свойства `IsToggled`.

Использование `Switch` демонстрируется в программе [**SwitchDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo).

### <a name="a-traditional-checkbox"></a>Традиционный флажок

Некоторые разработчики предпочитают более традиционную реализацию `CheckBox` вместо `Switch`. Библиотека [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) содержит класс `CheckBox`, производный от `ContentView`. `CheckBox` реализуется через файлы [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) и [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs). `CheckBox` определяет три свойства (`Text`, `FontSize` и `IsChecked`) и одно событие `CheckedChanged`.

Эта реализация `CheckBox` демонстрируется в примере [**CheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo).

## <a name="typing-text"></a>Набор текста

Xamarin.Forms определяет три представления, которые позволяют вводить и редактировать текст.

- [`Entry`](xref:Xamarin.Forms.Entry) для одной строки текста;
- [`Editor`](xref:Xamarin.Forms.Editor) для нескольких строк текста;
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) для одной строки текста, предназначенной для поиска.

`Entry` и `Editor` являются производными от [`InputView`](xref:Xamarin.Forms.InputView), который в свою очередь наследуется от `View`. `SearchBar` наследуется напрямую от `View`.

### <a name="keyboard-and-focus"></a>Клавиатура и фокус ввода

На телефонах и планшетах без физических клавиатур все элементы `Entry`, `Editor` и `SearchBar` вызывают появление виртуальной клавиатуры. Наличие клавиатуры на экране зависит от фокуса ввода. Для получения фокуса ввода у свойства [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) и [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) представления должны иметь значение `true`.

С фокусом ввода связаны два метода, одно свойство только для чтения и два события. Все они определяются в `VisualElement`.

- Метод [`Focus`](xref:Xamarin.Forms.VisualElement.Focus) пытается установить фокус ввода на элемент и возвращает `true` в случае успеха.
- Метод [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus) убирает фокус ввода из элемента.
- Свойство только для чтения [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) указывает, имеет ли элемент фокус ввода.
- Событие [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) сообщает, когда элемент получает фокус ввода.
- Событие [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) сообщает, когда элемент теряет фокус ввода.

### <a name="choosing-the-keyboard"></a>Выбор клавиатуры

Класс [`InputView`](xref:Xamarin.Forms.InputView), от которого наследуются `Entry` и `Editor`, определяет только одно свойство:

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) типа [`Keyboard`](xref:Xamarin.Forms.Keyboard).

Оно обозначает тип отображаемой клавиатуры. Некоторые клавиатуры имеют оптимизацию для ввода URI и (или) чисел.

Класс `Keyboard` позволяет определить клавиатуру, используя статический метод [`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) с аргументом типа [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags), который является перечислением со следующими битовыми флагами:

- `None` имеет значение 0;
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) имеет значение 1;
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) имеет значение 2;
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) имеет значение 4;
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) имеет значение \xFFFFFFFF.

При использовании многострочного элемента [`Editor`](xref:Xamarin.Forms.Editor), если ожидается ввод одного или нескольких абзацев текста, для выбора клавиатуры удобно вызывать `Keyboard.Create`. Для однострочного элемента [`Entry`](xref:Xamarin.Forms.Entry) наиболее удобны следующие статические свойства класса `Keyboard`, доступные только для чтения.

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) для положительных чисел с десятичной запятой или без нее.

[`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter) позволяет указать эти свойства в XAML, как показано в программе [**EntryKeyboards**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards).

### <a name="entry-properties-and-events"></a>Свойства и события ввода данных

Однострочный элемент [`Entry`](xref:Xamarin.Forms.Entry) определяет следующие свойства:

- [`Text`](xref:Xamarin.Forms.InputView.Text) с типом `string` (это отображаемый в `Entry` текст);
- [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) типа `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) типа `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) типа `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) типа `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) с типом `bool`, который применяет маскирование символов;
- [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) с типом `string` для отображения в `Entry` текста с "приглушенным" цветом до начала ввода;
- [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) типа `Color`

`Entry` также определяет два события:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) с объектом [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs), которое вызывается при каждом изменении свойства `Text`;
- [`Completed`](xref:Xamarin.Forms.Entry.Completed), которое вызывается после завершения ввода и закрытии клавиатуры. Сигнал о завершении ввода подается пользователем по-разному в зависимости от платформы.

В примере [**QuadraticEquations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) демонстрируются эти два события.

### <a name="the-editor-difference"></a>Отличия редактора

Многострочный [`Editor`](xref:Xamarin.Forms.Editor) определяет те же свойства `Text` и `Font`, что и `Entry`, но не имеет других свойств. `Editor` также определяет те же два свойства, что и `Entry`.

[**JustNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) — программа для ввода заметок в свободной форме, которая сохраняет и восстанавливает содержимое `Editor`.

### <a name="the-searchbar"></a>Панель поиска

[`SearchBar`](xref:Xamarin.Forms.SearchBar) не является производным от `InputView`, поэтому у него нет свойства `Keyboard`. Но у него есть все свойства `Text`, `Font` и `Placeholder`, которые определяет `Entry`. Кроме того, `SearchBar` определяет три дополнительных свойства:

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) типа `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) с типом [`ICommand`](xref:System.Windows.Input.ICommand) для использования с привязками данных и MVVM;
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) с типом `Object` для использования с `SearchCommand`.

Кнопка отмены, которая у каждой платформы своя, стирает текст. Также `SearchBar` содержит кнопку поиска, которая зависит от платформы. Нажатие любой из этих кнопок вызывает одно из двух событий, которые определены в `SearchBar`:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) дополняется объектом [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs).
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

Эта реализация `SearchBar` демонстрируется в примере [**SearchBarDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo).

## <a name="date-and-time-selection"></a>Выбор даты и времени

Представления [`DatePicker`](xref:Xamarin.Forms.DatePicker) и [`TimePicker`](xref:Xamarin.Forms.TimePicker) реализуют зависящие от платформы элементы управления, с помощью которых пользователь может указать дату или время.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker) определяет четыре свойства:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) с типом `DateTime` и начальным значением "1 января 1900 г.";
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) с типом `DateTime` и начальным значением "31 декабря 2100 г.";
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) с типом `DateTime` и начальным значением `DateTime.Today`;
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) с типом `string`, это строка в формате .NET с начальным значением "d", которая обозначает краткий формат даты (для США дата будет отображаться в формате "7/20/1969").

Свойства `DateTime` можно задать в XAML, выразив их в виде элементов свойств и используя краткий формат даты ("7/20/1969"), не зависящий от выбора языка и региональных параметров.   

Пример [**DaysBetweenDates**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) вычисляет количество дней между двумя датами, выбранными пользователем.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>Элемент TimePicker (или TimeSpanPicker?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) не имеет событий и определяет два свойства:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) с типом `TimeSpan` вместо `DateTime`, которое указывает время, прошедшее после полуночи;
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) с типом `string`, это строка в формате .NET с начальным значением "t", которая обозначает краткий формат времени (для США время будет отображаться в формате "1:45 PM").

Программа [**SetTimer**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) демонстрирует использование `TimePicker` для задания времени таймера. Эта программа работает только в том случае, если находится на переднем плане.

**SetTimer** также демонстрирует применение метода [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) из `Page` для отображения окна с оповещением.

## <a name="related-links"></a>Связанные ссылки

- [Глава 15, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Примеры для главы 15](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Slider](~/xamarin-forms/user-interface/slider.md)
- [Ввод](~/xamarin-forms/user-interface/text/entry.md)
- [Редактор](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
