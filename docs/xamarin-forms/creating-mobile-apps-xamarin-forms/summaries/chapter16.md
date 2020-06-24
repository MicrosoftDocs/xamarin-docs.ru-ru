---
title: Сводная информация о главе 16. привязка данных,
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводная информация о главе 16. привязка данных,
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ece93730100001e8339a5f50cdb7ac437d96fa62
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136738"
---
# <a name="summary-of-chapter-16-data-binding"></a>Сводная информация о главе 16. привязка данных,

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)

> [!NOTE] 
> Примечания на этой странице указывают области, в которых Xamarin.Forms имеет расхождения с материалом, представленным в книге.

Программистам часто приходится писать обработчики событий, которые отслеживают изменение свойства одного объекта и используют его для изменения значения свойства в другом объекте. Этот процесс можно автоматизировать с помощью технологии *привязки данных*. Привязки данных обычно определяются в XAML и становятся частью определения пользовательского интерфейса.

Очень часто эти привязки данных соединяют объекты пользовательского интерфейса с базовыми данными. Эта технология подробнее рассматривается в главе 18 [ **MVVM**](chapter18.md). Но привязки данных могут также соединять два и более элементов пользовательского интерфейса. Этот способ демонстрируется в первых нескольких примерах привязки данных в этой главе.

## <a name="binding-basics"></a>Основы привязки данных

В привязке данных участвуют несколько свойств, методов и классов:

- класс [`Binding`](xref:Xamarin.Forms.Binding) наследуется от [`BindingBase`](xref:Xamarin.Forms.BindingBase) и инкапсулирует множество характеристик привязки данных;
- свойство [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) определено в классе [`BindableObject`](xref:Xamarin.Forms.BindableObject);
- Метод [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) также определяется классом [`BindableObject`](xref:Xamarin.Forms.BindableObject).
- класс [`BindableObjectExtensions`](xref:Xamarin.Forms.BindableObjectExtensions) определяет три дополнительных метода `SetBinding`.

Следующие два класса поддерживают расширения разметки XAML для привязок:

- [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) поддерживает расширение разметки `Binding`;
- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) поддерживает расширение разметки `x:Reference`.

В привязке данных участвуют два интерфейса:

- [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) в пространстве имен `System.ComponentModel` используется для реализации уведомлений об изменении свойства;
- [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) используется для определения небольших классов, которые в привязках данных преобразуют значения из одного типа в другой.

Привязка данных соединяет два свойства одного объекта или (чаще) двух разных объектов. Эти два свойства называются *исходным* и *целевым*. Как правило, изменение исходного свойства приводит к изменению целевого свойства, но иногда направление изменяется. Но, в любом случае:

- *целевое* свойство должно подкрепляться [`BindableProperty`](xref:Xamarin.Forms.BindableProperty);
- *исходное* свойство обычно является членом класса, который реализует [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged).

Класс, который реализует `INotifyPropertyChanged`, запускает событие [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) при изменении значения свойства. `BindableObject` реализует `INotifyPropertyChanged` и автоматически вызывает событие `PropertyChanged`, когда подкрепленное `BindableProperty` свойство изменяет значения. Но можно написать собственные классы с реализацией `INotifyPropertyChanged` без наследования от `BindableObject`.

## <a name="code-and-xaml"></a>Код и XAML

В примере [**OpacityBindingCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) показано, как задать привязку данных в коде.

- Исходным является свойство `Value` объекта `Slider`.
- Целевым является свойство `Opacity` объекта `Label`.

Эти два объекта связываются путем присвоения `BindingContext` из объекта `Label` в объект `Slider`. Эти два свойства связываются путем вызова метода расширения [`SetBinding`](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) для `Label` со ссылкой на привязываемое свойство `OpacityProperty` и свойство `Value` объекта `Slider`, выраженное строковым значением.

Теперь изменение `Slider` приводит к тому, что `Label` исчезает и появляется снова.

[**OPacityBindingXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) — это такая же программа, но в ней привязка данных настроена в XAML. `BindingContext` в `Label` содержит расширение разметки `x:Reference`, которое ссылается на `Slider`, а свойство `Opacity` в `Label` содержит расширение разметки `Binding`, у которого свойство [`Path`](xref:Xamarin.Forms.Binding.Path) ссылается на свойство `Value` объекта `Slider`.

## <a name="source-and-bindingcontext"></a>Источник и BindingContext

В примере [**BindingSourceCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) показан другой подход, реализуемый в коде. Для создания объекта `Binding` свойству [`Source`](xref:Xamarin.Forms.Binding.Source) присваивается объект `Slider`, а свойству [`Path`](xref:Xamarin.Forms.Binding.Path) задается значение "Value". Затем для объекта `Label` вызывается метод [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) из `BindableObject`.

[`Binding` Конструктор](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) также можно использовать для определения объекта `Binding`.

В примере [**BindingSourceXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) показан аналогичный подход с использованием XAML. Свойству `Opacity` объекта `Label` присвоено расширение разметки `Binding`, где [`Path`](xref:Xamarin.Forms.Binding.Path) получает значение свойства `Value`, и [`Source`](xref:Xamarin.Forms.Binding.Source) содержит внедренное расширение разметки `x:Reference`.

Итак, у нас есть несколько способов указать исходный объект для привязки:

- через свойство `BindingContext` в целевом объекте;
- через свойство `Source` в самом объекте `Binding`.

Если указаны оба варианта, второй имеет более высокий приоритет. Преимуществом `BindingContext` является то, что оно распространяется через визуальное дерево. Это *очень* удобно, если несколько целевых свойств привязаны к одному и тому же исходному объекту.

Программа [**WebViewDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) демонстрирует этот способ на примере элемента [`WebView`](xref:Xamarin.Forms.WebView). Два элемента `Button` для перемещения вперед и назад наследуют `BindingContext` от своего родительского элемента, который ссылается на `WebView`. Затем свойства `IsEnabled` двух кнопок получают простые расширения разметки `Binding`, нацеленные на свойства `IsEnabled` кнопки, как определяется свойствами [`CanGoBack`](xref:Xamarin.Forms.WebView.CanGoBack) и [`CanGoForward`](xref:Xamarin.Forms.WebView.CanGoForward) только для чтения в объекте `WebView`.

## <a name="the-binding-mode"></a>Режим привязки

Присвойте свойству [`Mode`](xref:Xamarin.Forms.BindingBase.Mode) объекта `Binding` значение одного из элементов перечисления [`BindingMode`](xref:Xamarin.Forms.BindingMode):

- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) означает, что изменение исходного свойства влияет на целевое;
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) означает, что изменение целевого свойства влияет на исходное;
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) означает, что исходное и целевое свойства влияют друг на друга;
- [`Default`](xref:Xamarin.Forms.BindingMode.Default) означает, что используется [`DefaultBindingMode`](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode), указанное при создании целевого `BindableProperty`. Если не указано ни одно значение, по умолчанию используется `OneWay` для обычных привязываемых свойств и `OneWayToSource` для привязываемых свойств только для чтения.

> [!NOTE]
> Перечисление `BindingMode` теперь также включает `OnTime`, при выборе которого привязка применяется только в случае изменения контекста привязки, а не изменения исходного свойства.

Свойства, которые могут стать целевыми для привязки данных в сценариях MVVM, обычно имеют для `DefaultBindingMode` значение `TwoWay`. Эти особые значения приведены ниже.

- свойство `Value` объектов `Slider` и `Stepper`;
- свойство `IsToggled` объекта `Switch`;
- свойство `Text` объектов `Entry`, `Editor` и `SearchBar`;
- свойство `Date` объекта `DatePicker`;
- свойство `Time` объекта `TimePicker`;

В примере [**BindingModes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) демонстрируются четыре режима привязки данных, где целевым является свойство `FontSize` объекта `Label`, а исходным — свойство `Value` объекта `Slider`. Это позволяет каждому `Slider` управлять размером шрифта соответствующего `Label`. Однако элементы `Slider` не инициализируются, так как `DefaultBindingMode` для свойства `FontSize` имеет значение `OneWay`.

В примере [**ReverseBinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) настраивается привязка свойства `Value` объекта `Slider` к свойству `FontSize` каждого объекта `Label`. Кажется, что должно быть наоборот, но так лучше работает инициализация элементов `Slider`, так как в свойстве `Value` объекта `Slider` параметр `DefaultBindingMode` имеет значение `TwoWay`.

[![Снимок экрана с тремя изображениями обратной привязки](images/ch16fg06-small.png "Обратная привязка")](images/ch16fg06-large.png#lightbox "Обратная привязка")

Похожим способом привязки определяются в MVVM, и вы будете часто использовать этот тип привязки.

## <a name="string-formatting"></a>Форматирование строк

Если целевое свойство имеет тип `string`, вы можете применить свойство [`StringFormat`](xref:Xamarin.Forms.BindingBase.StringFormat), определенное в `BindingBase`, для преобразования исходного свойства в `string`. Задайте для свойства `StringFormat` строку форматирования .NET, которая будет использоваться совместно со статическим форматом [`String.Format`](xref:System.String.Format(System.String,System.Object)) для отображения объекта. При использовании этой строки форматирования в расширении разметки ее следует заключить в одинарные кавычки, чтобы фигурные скобки не были ошибочно приняты за внедренное расширение разметки.

В примере [**ShowViewValues**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) показано использование `StringFormat` в XAML.

В примере [**WhatSizeBindings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) демонстрируется отображение размера страницы с привязками к свойствам `Width` и `Height` объекта `ContentPage`.

## <a name="why-is-it-called-path"></a>Почему "путь" так называется?

Свойство [`Path`](xref:Xamarin.Forms.Binding.Path) ("Путь") объекта `Binding` получило такое название, так как оно может содержать серию свойств и индексаторов, разделенных точками. В [**BindingPathDemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) представлено несколько примеров.

## <a name="binding-value-converters"></a>Преобразователи значений привязки

Если исходное и целевое свойства привязки имеют разные типы, преобразование между этими типами можно выполнить с помощью преобразователя привязок. Этот класс реализует интерфейс [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) и содержит два метода: [`Convert`](xref:Xamarin.Forms.IValueConverter.Convert(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) для преобразования исходного значения в целевое и [`ConvertBack`](xref:Xamarin.Forms.IValueConverter.ConvertBack(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) для преобразования целевого значения в исходное.

Класс [`IntToBoolConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) служит примером преобразования `int` в `bool`. Это демонстрируется в примере [**ButtonEnabler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler), который включает `Button` только в том случае, если в `Entry` введен хотя бы один символ.

Класс [`BoolToStringConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) преобразует `bool` в `string` и определяет два свойства, чтобы указать текст для возвращаемых значений `false` и `true`.
[`BoolToColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) действует аналогично. В примере [**SwitchText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) показано использование этих двух преобразователей для отображения разных текстов разными цветами на основе значения параметра `Switch`.

Универсальный [`BoolToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) может заменить `BoolToStringConverter` и `BoolToColorConverter`, выполняя роль обобщенного преобразователя `bool` в объект любого типа.

## <a name="bindings-and-custom-views"></a>Привязки и пользовательские представления

Вы можете упростить пользовательские элементы управления, используя привязки данных. В файле кода [`NewCheckBox.cs`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) определяются свойства `Text`, `TextColor`, `FontSize`, `FontAttributes` и `IsChecked`, но он не содержит логику для визуальных элементов управления.
Вместо этого в файле [`NewCheckBox.cs.xaml`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) определена вся разметку для визуальных элементов управления через привязки данных в элементах `Label`, основанные на свойствах в файле кода программной части.

В примере [**NewCheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) демонстрируется пользовательский элемент управления `NewCheckBox`.

## <a name="related-links"></a>Связанные ссылки

- [Глава 16, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Примеры для главы 16](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
- [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)
