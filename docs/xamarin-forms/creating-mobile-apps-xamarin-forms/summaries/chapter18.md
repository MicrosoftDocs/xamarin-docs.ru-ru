---
title: Сводная информация о Главе 18. MVVM
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводная информация о Главе 18. MVVM
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b2309d6ec6712be32f6e972fab130a542fbec857
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91557430"
---
# <a name="summary-of-chapter-18-mvvm"></a>Сводная информация о Главе 18. MVVM

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)

Одним из лучших подходов к разработке приложения является отделение пользовательского интерфейса от базового кода, который иногда называют *бизнес-логикой*. Для этого существует несколько способов, но один из них специально разработан для сред на основе XAML и известен как "Модель — представление — модель представления" или MVVM.

## <a name="mvvm-interrelationships"></a>Взаимосвязи в MVVM

Приложение MVVM имеет три уровня.

- Модель предоставляет базовые данные, иногда через файлы или веб-доступ.
- Представление выполняет роль пользовательского интерфейса или уровня представления, и обычно реализуется в XAML.
- Модель представления (ViewModel) соединяет Модель с Представлением.

Модель ничего не знает о ViewModel, а ViewModel — о Представлении. Эти три уровня обычно взаимодействуют друг с другом по следующим механизмам?

![Представление, ViewModel, Представление](images/ch18fg03.png "MVVM")

Во многих небольших программах (а иногда даже в больших) Модель может отсутствовать или быть полностью встроенной во ViewModel.

## <a name="viewmodels-and-data-binding"></a>ViewModel и привязка данных

Чтобы участвовать в привязке данных, ViewModel должна уметь информировать Представление об изменении свойств ViewModel. Для этого ViewModel реализует интерфейс [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) в пространстве имен `System.ComponentModel`. Это относится скорее к .NET, чем к Xamarin.Forms. (Обычно ViewModel пытается остаться независимой от платформы.)

Интерфейс `INotifyPropertyChanged` объявляет одно событие с именем [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged), которое сигнализирует об изменении свойства.

### <a name="a-viewmodel-clock"></a>Часы ViewModel

[`DateTimeViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) определяет свойство типа `DateTime`, значение которого изменятся по таймеру. Этот класс реализует `INotifyPropertyChanged` и активирует событие `PropertyChanged` при каждом изменении свойства `DateTime`.

Пример [**MvvmClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) создает экземпляр этой ViewModel и использует привязку данных к ViewModel для отображения сведений о текущих дате и времени.

### <a name="interactive-properties-in-a-viewmodel"></a>Интерактивные свойства во ViewModel

Свойства ViewModel могут быть более интерактивными, как показано в классе [`SimpleMultiplierViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs), который входит в пример [**SimpleMultiplier**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier). Привязки данных предоставляют значения множителей из двух элементов `Slider` и отображают произведение с помощью `Label`. При этом вы можете вносить достаточно существенные изменения в этот интерфейс через XAML, не вызывая никаких изменений во ViewModel или файле кода программной части.

### <a name="a-color-viewmodel"></a>ViewModel для цвета

[`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) включает в себя цветовые модели RGB и HSL. Его использование представлено в примере [**HslSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders):

[![Три снимка экрана с набором инструментов](images/ch18fg08-small.png "Цветовая модель HSL")](images/ch18fg08-large.png#lightbox "Цветовая модель HSL")

### <a name="streamlining-the-viewmodel"></a>Упрощение ViewModel

Вы можете упростить код ViewModel, определив метод `OnPropertyChanged` с атрибутом [`CallerMemberName`](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute), который автоматически получает имя вызывающего свойства. Класс [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) выполняет именно эту функцию и служит базовым классом для ViewModel.

## <a name="the-command-interface"></a>Командный интерфейс

MVVM работает с привязками данных, а они в свою очередь — со свойствами, поэтому возможности MVVM довольно ограничены при обработке события `Clicked` из `Button` или события `Tapped` из `TapGestureRecognizer`. Чтобы ViewModel могла обрабатывать такие события, Xamarin.Forms поддерживает *командный интерфейс*.

Командный интерфейс объявляет себя в `Button` с двумя открытыми свойствами:

- [`Command`](xref:Xamarin.Forms.Button.Command) с типом [`ICommand`](xref:System.Windows.Input.ICommand) (определено в пространстве имен `System.Windows.Input`);
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) типа `Object`

Чтобы поддерживать командный интерфейс, во ViewModel нужно определить свойство с типом `ICommand` и выполнить для него привязку данных к свойству `Command` из `Button`. Интерфейс `ICommand` объявляет два метода и одно событие:

- метод [`Execute`](xref:System.Windows.Input.ICommand.Execute(System.Object)) с аргументом типа `object`;
- метод [`CanExecute`](xref:System.Windows.Input.ICommand.CanExecute(System.Object)) с аргументом типа `object`, который возвращает `bool`;
- событие [`CanExecuteChanged`](xref:System.Windows.Input.ICommand.CanExecuteChanged).

Внутри ViewModel присваивает каждому свойству с типом `ICommand` значение экземпляра класса, который реализует интерфейс `ICommand`. Через привязку данных `Button` изначально вызывает метод `CanExecute` и отключается, если этот метод возвращает `false`. Также она назначает обработчик для события `CanExecuteChanged` и вызывает `CanExecute` при активации этого события. Если включен `Button`, он вызывает метод `Execute` при каждом нажатии `Button`.

Возможно, у вас уже есть существующие ViewModels, созданные раньше Xamarin.Forms, и они даже могут поддерживать командный интерфейс. Для новых ViewModels, которые будут использоваться только с Xamarin.Forms, Xamarin.Forms предоставляет класс [`Command`](xref:Xamarin.Forms.Command) и класс [`Command<T>`](xref:Xamarin.Forms.Command`1), который реализует интерфейс `ICommand`. Для аргументов методов `Execute` и `CanExecute` используется универсальный тип.

### <a name="simple-method-executions"></a>Простое выполнение метода

Пример [**PowersOfThree**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) демонстрирует применение командного интерфейса из ViewModel. Класс [`PowersViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) определяет два свойства с типом `ICommand`, а также два частных свойства, которые он передает в простейший [конструктор `Command`](xref:Xamarin.Forms.Command.%23ctor(System.Action)). Эта программа содержит привязки данных из ViewModel к свойствам `Command` двух элементов `Button`.

Элементы `Button` можно свободно заменить в XAML объектами `TapGestureRecognizer`, не меняя код.

### <a name="a-calculator-almost"></a>Калькулятор, ну почти

Пример [**AddingMachine**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) использует оба метода класса `ICommand`: `Execute` и `CanExecute`. Он использует также класс [`AdderViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) из библиотеки [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs). ViewModel содержит шесть свойств с типом `ICommand`. Они инициализируются из [конструктора `Command`](xref:Xamarin.Forms.Command.%23ctor(System.Action)) и [конструктора `Command`](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) класса `Command`, а также [конструктора `Command<T>`](/dotnet/api/xamarin.forms.command.-ctor?view=xamarin-forms#Xamarin_Forms_Command__ctor_System_Action_System_Object__System_Func_System_Object_System_Boolean__) класса `Command<T>`. Все числовые кнопки на сумматоре привязаны к свойству, которое инициализируется значением `Command<T>`, а аргумент `string` — к `Execute`, где `CanExecute` обозначает конкретную кнопку.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModel и жизненный цикл приложения

`AdderViewModel` из примера **AddingMachine** определяет также два метода с именами `SaveState` и `RestoreState`. Эти методы вызываются из приложения, когда оно переходит в спящий режим и пробуждается из спящего режима.

## <a name="related-links"></a>Связанные ссылки

- [Глава 18, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Примеры для Главы 18](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
- [Электронная книга "Шаблоны корпоративного приложения с использованием Xamarin.Forms"](~/xamarin-forms/enterprise-application-patterns/index.md)