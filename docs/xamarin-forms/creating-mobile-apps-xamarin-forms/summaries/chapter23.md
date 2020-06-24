---
title: Сводка по главе 23. Триггеры и реакции на событие
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводка по главе 23. Триггеры и реакции на событие
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9a0206354254f79756e29f834c85837240736eca
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136660"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Сводка по главе 23. Триггеры и реакции на событие

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)

Триггеры и реакции на событие похожи тем, что используются в файлах XAML для упрощения взаимодействий элементов за пределами использования привязок данных и расширения функциональных возможностей элементов XAML. Как триггеры, так и реакции на событие почти всегда сочетаются с визуальными объектами пользовательского интерфейса.

Для использования триггеров и реакций на событие `VisualElement` и `Style` поддерживают два свойства коллекции:

- [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) и [`Style.Triggers`](xref:Xamarin.Forms.Style.Triggers) с типом `IList<TriggerBase>`;
- [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) и [`Style.Behaviors`](xref:Xamarin.Forms.Style.Behaviors) с типом `IList<Behavior>`.

## <a name="triggers"></a>Триггеры

Триггером называется условие (изменение свойства или срабатывание события), которое приводит к некоторому отклику (изменение другого свойства или выполнение некоторого кода). Свойство `Triggers` для `VisualElement` и `Style` имеет тип `IList<TriggersBase>`. [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) является абстрактным классом, от которого наследуются четыре запечатанных класса:

- [`Trigger`](xref:Xamarin.Forms.Trigger) для ответов на основе изменений свойств;
- [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) для ответов на основе выполнения событий;
- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) для ответов на основе привязки данных;
- [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) для ответов на основе нескольких триггеров.

Триггер всегда задается для элемента, свойство которого изменяется этим триггером.

### <a name="the-simplest-trigger"></a>Простейший триггер

Класс [`Trigger`](xref:Xamarin.Forms.Trigger) проверяет изменение значения свойства и отвечает, устанавливая другое свойство того же элемента.

`Trigger` определяет три свойства:

- [`Property`](xref:Xamarin.Forms.Trigger.Property) типа `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Trigger.Value) типа `Object`
- [`Setters`](xref:Xamarin.Forms.Trigger.Setters) с типом `IList<SetterBase>` — это свойство содержимого `Trigger`.

Кроме того, `Trigger` требует установить следующее свойство, наследуемое от `TriggerBase`:

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType), чтобы указывать тип элемента, к которому присоединен `Trigger`

Здесь `Property` и `Value` составляют условие, а коллекция `Setters` определяет ответ. Если `Property` имеет значение, указанное в `Value`, применяются объекты `Setter` из коллекции `Setters`. Если `Property` имеет другое значение, методы задания удаляются. `Setter` определяет два свойства, которые совпадают с двумя первыми свойствами `Trigger`:

- [`Property`](xref:Xamarin.Forms.Setter.Property) типа `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Setter.Value) типа `Object`

В примере [**EntryPop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) показано, как применение `Trigger` к `Entry` позволяет увеличивать размер `Entry` с помощью свойства `Scale`, когда свойство `IsFocused` объекта `Entry` имеет значение `true`.

Обычно так не делают, но `Trigger` можно задавать в коде, как показано в примере [**EntryPopCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode).

Пример [**StyledTriggers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) демонстрирует, как задать `Trigger` из `Style` для применения к нескольким элементам `Entry`.

### <a name="trigger-actions-and-animations"></a>Действия триггера и анимации

Вы также можете выполнять при активации триггера небольшой фрагмент кода. Этот код может быть анимацией, которая использует определенное свойство. Один из распространенных приемов — использовать [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) с определением двух свойств:

- [`Event`](xref:Xamarin.Forms.EventTrigger.Event) с типом `string`, где содержится имя события;
- [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions) с типом `IList<TriggerAction>`, где определен список выполняемых в ответ действий.

Чтобы использовать его, нужно написать производный от [`TriggerAction<T>`](xref:Xamarin.Forms.TriggerAction`1) класс, обычно это `TriggerAction<VisualElement>`. Определите свойства в этом классе. Это простые свойства CLR, а не привязываемые свойства, так как `TriggerAction` не наследует от `BindableObject`. Необходимо переопределить метод [`Invoke`](xref:Xamarin.Forms.TriggerAction`1.Invoke*), который вызывается при активации действия. В качестве аргумента он принимает целевой элемент.

Примером служит класс [`ScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit). Он вызывает свойство `ScaleTo` для анимации свойства `Scale` указанного элемента. Так как одно из его свойств имеет тип `Easing`, класс [`EasingConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) позволяет использовать в XAML стандартные статические поля `Easing`.

В примере [**EntrySwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) показано, как вызывать `ScaleAction` из `EventTrigger` объектов, отслеживающих события `Focused` и `Unfocused`.

В примере [**CustomEasingSwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) показано, как определить пользовательскую функцию для реалистичной анимации для `ScaleAction` в файле кода программной части.

Действия также можно вызывать с помощью `Trigger` (в отличие от `EventTrigger`). Для этого необходимо знать, что `TriggerBase` определяет две коллекции:

- [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) типа `IList<TriggerAction>`
- [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) типа `IList<TriggerAction>`

Пример [**EnterExitSwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) демонстрирует способ использования этих коллекций.

### <a name="more-event-triggers"></a>Дополнительные триггеры событий

Класс [`ScaleUpAndDownAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) дважды вызывает `ScaleTo` для увеличения и уменьшения размера. Пример [**ButtonGrowth**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) использует этот прием в `EventTrigger` с примененным стилем, чтобы обеспечить визуальное подтверждение нажатия `Button`. Эта двойная анимация также поддерживается двумя действиями в коллекции типа [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs).

Класс [`ShiverAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) из библиотеки **Xamarin.FormsBook.Toolkit** определяет действие дрожания с возможностью настройки. Эта техника демонстрируется в примере [**ShiverButtonDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo).

Класс [`NumericValidationAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) в библиотеке **Xamarin.FormsBook.Toolkit** действует только для элементов `Entry` и присваивает свойству `TextColor` значение red, если свойство `Text` не является `double`. Этот подход демонстрируется в примере [**TriggerEntryValidation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation).

### <a name="data-triggers"></a>Триггеры данных

[`DataTrigger`](xref:Xamarin.Forms.DataTrigger) похож на `Trigger`, за исключением того, что он отслеживает привязку данных, а не ожидает изменений значения свойства. Это позволяет свойству одного элемента влиять на свойство в другом элементе.

`DataTrigger` определяет три свойства:

- [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding) типа `BindingBase`
- [`Value`](xref:Xamarin.Forms.DataTrigger.Value) типа `Object`
- [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters) типа `IList<SetterBase>`

Для примера [**GenderColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) требуется библиотека [**SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt). Он задает цвета для имен учащихся: синий или розовый в зависимости от свойства `Sex`:

[![Снимок экрана с тремя изображениями цветов в зависимости от пола](images/ch23fg04-small.png "Цвета на основе пола")](images/ch23fg04-large.png#lightbox "Цвета на основе пола")

В примере [**ButtonEnabler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) для свойства `IsEnabled` элемента `Entry` устанавливается значение `False`, если свойство `Length` свойства `Text` элемента `Entry` равно 0. Обратите внимание, что свойство `Text` инициализируется пустой строкой. По умолчанию оно имеет значение `null`, при котором `DataTrigger` не будет работать правильно.

### <a name="combining-conditions-in-the-multitrigger"></a>Объединение условий в MultiTrigger

[`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) представляет собой коллекцию условий. Когда они все получают значение `true`, применяются методы задания. Этот класс определяет два свойства:

- [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions) типа `IList<Condition>`
- [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters) типа `IList<Setter>`

[`Condition`](xref:Xamarin.Forms.Condition) является абстрактным классом и имеет два дочерних класса:

- [`PropertyCondition`](xref:Xamarin.Forms.Condition), который имеет свойства [`Property`](xref:Xamarin.Forms.PropertyCondition.Property) и [`Value`](xref:Xamarin.Forms.PropertyCondition.Value), как `Trigger`
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition), который имеет свойства [`Binding`](xref:Xamarin.Forms.BindingCondition.Binding) и [`Value`](xref:Xamarin.Forms.BindingCondition.Value), как `DataTrigger`

В примере [**AndConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) элемент `BoxView` выделяется, только если включены все четыре элемента `Switch`.

В примере [**OrConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) показано, как можно изменять цвет `BoxView` при включении *любого* из четырех элементов `Switch`. Это требует применения закона де Моргана и обращения всей логики.

Сочетание логики И и ИЛИ реализовать сложнее, и обычно для этого нужны невидимые элементы `Switch` для промежуточных результатов. В примере [**XorConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) показано, как включать `Button`, если один и только один из двух элементов `Entry` содержит введенный текст.

## <a name="behaviors"></a>поведения

Все, что можно сделать с помощью триггера, можно реализовать и с помощью реакции на событие, но для этого всегда требуется производный от [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) класс с переопределением следующих двух методов:

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

В качестве аргумента принимается элемент, к которому присоединяется реакция на событие. Как правило, метод `OnAttachedTo` присоединяет некоторые обработчики событий, а `OnDetachingFrom` отсоединяет их. Так как этот класс обычно сохраняет некоторые сведения о состоянии, его нельзя использовать совместно в `Style`.

Пример [**BehaviorEntryValidation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) похож на **TriggerEntryValidation** с той разницей, что он использует реакцию на событие &mdash; в классе [`NumericValidationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

### <a name="behaviors-with-properties"></a>Реакция на событие с поддержкой свойств

`Behavior<T>` наследует от `Behavior`, который наследует от `BindableObject`, а значит для реакции на событие можно определить привязываемые свойства. Эти свойства можно активно использовать в привязке данных.

Этот подход демонстрируется в программе [**EmailValidationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) на основе класса [`ValidEmailBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit). `ValidEmailBehavior` имеет привязываемое свойство только для чтения и служит источником в привязках данных.

В примере [**EmailValidationConv**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) эта же реакция на событие используется для отображения индикатора другого типа, который информирует о допустимом формате адреса электронной почты.

Пример [**EmailValidationTrigger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) является модификацией предыдущего примера. ButtonGlide использует `DataTrigger` в сочетании с той же реакцией на событие.

### <a name="toggles-and-check-boxes"></a>Переключатели и флажки

Поведение выключателя можно инкапсулировать в классе, например [`ToggleBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit), а затем определить все визуальные элементы для переключателя полностью в XAML.

В примере [**ToggleLabel**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) используется `ToggleBehavior` с `DataTrigger`, чтобы применить `Label` с двумя текстовыми строками для переключения.

Пример [**FormattedTextToggle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) расширяет эту схему, переключаясь между двумя объектами `FormattedString`.

Класс [`ToggleBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) в библиотеке **Xamarin.FormsBook.Toolkit** является производным от `ContentView`, определяет свойство `IsToggled` и содержит `ToggleBehavior` для логики переключения. Это упрощает определение выключателя в XAML, как показано в примере [**TraditionalCheckBox**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox).

Пример [**SwitchCloneDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) содержит класс [`SwitchClone`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs), производный от `ToggleBase`, и использует класс [`TranslateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs) для создания переключателя, похожего на стандартный Xamarin.Forms `Switch`.

Класс [`RotateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) в **Xamarin.FormsBook.Toolkit** предоставляет анимацию, на которой основан анимированный рычаг в примере [**LeverToggle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle).

### <a name="responding-to-taps"></a>Реагирование на касания

Недостаток `EventTrigger` заключается в том, что вы не можете присоединить его к `TapGestureRecognizer`, чтобы реагировать на касания. Чтобы обойти эту проблему, в библиотеку [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) включен [`TapBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs).

В примере [**BoxViewTapShiver**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) используется `TapBehavior`, чтобы применить описанный выше `ShiverAction` для нажатых элементов `BoxView`.

В примере [**ShiverViews**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) показано, как сократить объем разметки с помощью инкапсуляции класса `ShiverView`.

### <a name="radio-buttons"></a>Переключатели

Библиотека [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) также имеет класс [`RadioBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) для создания переключателей, сгруппированных по имени группы `string`.

Программа [**RadioLabels**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) использует текстовые строки в качестве переключателя. В примере [**RadioStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) используется `Style`, чтобы различать нажатые и не нажатые кнопки. Пример [**RadioImages**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) использует текстовые строки в качестве переключателей.

[![Снимок экрана с тремя изображениями переключателей](images/ch23fg17-small.png "Изображения переключателей")](images/ch23fg17-large.png#lightbox "Изображения переключателей")

В примере [**TraditionalRadios**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) отображаются переключатели традиционного вида с точкой внутри окружности.

### <a name="fades-and-orientation"></a>Исчезновение и ориентация

В последнем примере [**MultiColorSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) вы с помощью переключателей можете выбирать любое из трех разных представлений для выбора цвета. Эти три представления плавно появляются и исчезают с использованием [`FadeEnableAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

Эта программа также реагирует на изменения ориентации (книжная или альбомная) с помощью [`GridOrientationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) из библиотеки **Xamarin.FormsBook.Toolkit**.

## <a name="related-links"></a>Связанные ссылки

- [Глава 23 — полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Глава 23 — примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Работа с триггерами](~/xamarin-forms/app-fundamentals/triggers.md)
- [Реакции на события Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
