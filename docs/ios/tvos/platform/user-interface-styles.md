---
title: Стили пользовательского интерфейса tvOS в Xamarin
description: В этой статье рассматриваются светлая и темная темы пользовательского интерфейса, добавленные компанией Apple в tvOS 10, и способы их реализации в приложении Xamarin. tvOS.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 793a82b7bb88f3e0c0a233c8c18c2d90f741b137
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434767"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>Стили пользовательского интерфейса tvOS в Xamarin

_В этой статье рассматриваются светлая и темная темы пользовательского интерфейса, добавленные компанией Apple в tvOS 10, и способы их реализации в приложении Xamarin. tvOS._

tvOS 10 теперь поддерживает как темную, так и светлую тему пользовательского интерфейса, которая будет автоматически адаптирована для всех элементов управления UIKit, в зависимости от предпочтений пользователя. Кроме того, разработчик может вручную настраивать элементы пользовательского интерфейса на основе темы, выбранной пользователем, и может переопределять указанную тему.

<a name="About-the-New-User-Interface-Styles"></a>

## <a name="about-the-new-user-interface-styles"></a>О новых стилях пользовательского интерфейса

Как упоминалось выше, tvOS 10 теперь поддерживает как темную, так и светлую тему пользовательского интерфейса, которая будет автоматически адаптирована в соответствии с предпочтениями пользователя.

Пользователь может переключить эту тему, выбрав **Параметры**  >  **Общий**  >  **вид** и переключение между **светлым** и **темным**:

[![Приложение параметров](user-interface-styles-images/theme01.png)](user-interface-styles-images/theme01.png#lightbox)

Если выбрана **темная** тема, все элементы пользовательского интерфейса будут переключаться на светлый текст на темном фоне:

[![Темная тема](user-interface-styles-images/theme02.png)](user-interface-styles-images/theme02.png#lightbox)

Пользователь имеет возможность переключить тему в любое время и может сделать это на основе текущего действия, в котором находится Apple TV, или время суток.

Тема «светлый пользовательский интерфейс» является темой по умолчанию, и все существующие приложения tvOS по-прежнему будут использовать нестандартную тему независимо от настроек пользователя, если только они не изменялись для tvOS 10, чтобы воспользоваться преимуществами темной темы. Приложение tvOS 10 также может переопределять текущую тему и всегда использовать светло-или темную тему для некоторых или всех элементов пользовательского интерфейса.

<a name="Adopting-the-Light-and-Dark-Themes"></a>

## <a name="adopting-the-light-and-dark-themes"></a>Внедрение светлых и темных тем

Для поддержки этой функции компания Apple добавила в класс новый API, `UITraitCollection` а приложение tvOS должно поддерживать темный внешний вид (с помощью параметра в своем `Info.plist` файле).

Чтобы принять участие в поддержке светлых и темных тем, выполните следующие действия.

1. В **обозревателе решений** дважды щелкните файл `Info.plist`, чтобы открыть его для редактирования.
2. Выберите представление **исходного кода** (в нижней части редактора).
3. Добавьте новый ключ и вызовите его `UIUserInterfaceStyle` :

    [![Ключ Уиусеринтерфацестиле](user-interface-styles-images/theme03.png)](user-interface-styles-images/theme03.png#lightbox)
4. Оставьте для типа значение `String` и введите `Automatic` :

    [![Введите автоматический](user-interface-styles-images/theme04.png)](user-interface-styles-images/theme04.png#lightbox)
5. Сохраните изменения в файле.

Существует три возможных значения `UIUserInterfaceStyle` ключа:

- **Легкая** — заставляет пользовательский интерфейс приложения tvOS всегда использовать тему освещения.
- **Темный** — заставляет пользовательский интерфейс приложения tvOS всегда использовать темную тему.
- **Автоматическое** переключение между светлой и темной темами в зависимости от настроек пользователя в разделе Параметры. Это предпочтительный параметр.

<a name="UIKit-Theme-Support"></a>

### <a name="uikit-theme-support"></a>Поддержка тем UIKit

Если приложение tvOS использует стандартные встроенные `UIView` элементы управления, они будут автоматически реагировать на тему пользовательского интерфейса без вмешательства разработчика.

Кроме того `UILabel` , `UITextView` он автоматически изменит свой цвет в соответствии с темой выбора пользовательского интерфейса:

- Текст будет черным в светлой теме.
- В темной теме текст будет белым.

Если разработчик когда-либо изменяет цвет текста вручную (в раскадровке или коде), они отвечают за обработку изменений цвета на основе темы пользовательского интерфейса.

<a name="New-Blur-Effects"></a>

### <a name="new-blur-effects"></a>Новые эффекты размытия

Для поддержки светлых и темных тем в приложении tvOS 10 компания Apple добавила два новых эффекта размытия. Эти новые эффекты будут автоматически настраивать размытие на основе темы пользовательского интерфейса, выбранной пользователем, следующим образом.

- `UIBlurEffectStyleRegular` — Использует светло размытие в светлой теме и темное размытие в темной теме.
- `UIBlurEffectStyleProminent` — Использует очень светлое размытие в светлой теме и очень темное размытие в темной теме.

<a name="Working-with-Trait-Collections"></a>

## <a name="working-with-trait-collections"></a>Работа с коллекциями признаков

Новое `UserInterfaceStyle` свойство `UITraitCollection` класса можно использовать для получения текущей выбранной темы пользовательского интерфейса и будет `UIUserInterfaceStyle` перечислением одного из следующих значений:

- **Light** — выбрана тема СВЕТЛОГО пользовательского интерфейса.
- **Темно** -выбрана темная тема пользовательского интерфейса.
- Не **указано** — представление еще не отображается на экране, поэтому текущая тема пользовательского интерфейса неизвестна.

Кроме того, коллекции признаков имеют следующие функции в tvOS 10:

- Прокси-сервер внешнего вида можно настроить в зависимости от того, что `UserInterfaceStyle` задается `UITraitCollection` для изменения таких объектов, как изображения или цвета элементов на основе темы.
- Приложение tvOS может управлять изменениями коллекции признаков путем переопределения `TraitCollectionDidChange` метода `UIView` `UIViewController` класса или.

> [!IMPORTANT]
> Ранний предварительный просмотр Xamarin. tvOS для tvOS 10 не полностью поддерживается `UIUserInterfaceStyle` `UITraitCollection` . Полная поддержка будет добавлена в следующем выпуске.

<a name="Customizing-Appearance-Based-on-Theme"></a>

### <a name="customizing-appearance-based-on-theme"></a>Настройка внешнего вида на основе темы

Для элементов пользовательского интерфейса, поддерживающих прокси-сервер внешнего вида, их внешний вид можно изменить на основе темы пользовательского интерфейса их коллекции признаков. Таким образом, для данного элемента пользовательского интерфейса разработчик может указать один цвет для светлой темы и другой цвет для темной темы.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> К сожалению, предварительная версия Xamarin. tvOS для tvOS 10 не полностью поддерживает `UIUserInterfaceStyle` `UITraitCollection` , поэтому этот тип настройки пока недоступен. Полная поддержка будет добавлена в следующем выпуске.

<a name="Responding-to-Theme-Changes-Directly"></a>

### <a name="responding-to-theme-changes-directly"></a>Непосредственная реакция на изменения темы

Разработчику требуется более глубокий контроль над внешним видом элемента пользовательского интерфейса на основе выбранной темы пользовательского интерфейса, но они могут переопределить `TraitCollectionDidChange` метод `UIView` `UIViewController` класса или.

Пример:

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);

    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly"></a>

### <a name="overriding-a-trait-collection"></a>Переопределение коллекции признаков

В зависимости от структуры приложения tvOS могут возникнуть ситуации, когда разработчику нужно переопределить коллекцию признаков данного элемента пользовательского интерфейса и всегда использовать определенную тему ПОЛЬЗОВАТЕЛЬСКОГО интерфейса.

Это можно сделать с помощью `SetOverrideTraitCollection` метода для `UIViewController` класса. Пример:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

Дополнительные сведения см. в разделах признаков и [Переопределение](~/ios/user-interface/storyboards/unified-storyboards.md) признаков статьи [Введение в унифицированные раскадровки](~/ios/user-interface/storyboards/unified-storyboards.md) документации. [Traits](~/ios/user-interface/storyboards/unified-storyboards.md)

<a name="Trait-Collections-and-Storyboards"></a>

### <a name="trait-collections-and-storyboards"></a>Коллекции признаков и раскадровки

В tvOS 10 раскадровку приложения можно настроить для реагирования на коллекции признаков, а многие элементы пользовательского интерфейса можно сделать неяркий и темной темой. В настоящее время ранний предварительный просмотр Xamarin. tvOS для tvOS 10 не поддерживает эту функцию в конструкторе интерфейсов, поэтому раскадровка должна быть изменена в Interface Builderе Xcode в качестве обходного пути.

Чтобы включить поддержку коллекции признаков, выполните следующие действия.

1. Щелкните правой кнопкой мыши файл раскадровки в **Обозреватель решений** и выберите **Открыть с помощью**  >  **Xcode Interface Builder**:

    [![Открыть с помощью Xcode Interface Builder](user-interface-styles-images/theme05.png)](user-interface-styles-images/theme05.png#lightbox)
2. Чтобы включить поддержку коллекции признаков, переключитесь в **инспекторе файлов** и установите флажок **использовать** признаки в **Interface Builder разделе документа** :

    [![Включить поддержку коллекции признаков](user-interface-styles-images/theme06.png)](user-interface-styles-images/theme06.png#lightbox)
3. Подтвердите изменение, чтобы использовать варианты характеристик:

    [![Предупреждение о вариантах использования](user-interface-styles-images/theme07.png)](user-interface-styles-images/theme07.png#lightbox)
4. Сохраните изменения в файле раскадровки.

Компания Apple добавила следующие возможности при редактировании раскадровок tvOS в Interface Builder:

- Разработчик может указать различные варианты элементов пользовательского интерфейса на основе темы пользовательского интерфейса в **инспекторе атрибутов**:

  - У нескольких свойств теперь есть несколько **+** значков, которые можно щелкнуть, чтобы добавить конкретную версию темы пользовательского интерфейса:

    [![Добавление определенной версии темы пользовательского интерфейса](user-interface-styles-images/theme08.png)](user-interface-styles-images/theme08.png#lightbox)

  - Разработчик может указать новое свойство или нажать кнопку **x** , чтобы удалить его:

    [![Укажите новое свойство или нажмите кнопку x, чтобы удалить его](user-interface-styles-images/theme09.png)](user-interface-styles-images/theme09.png#lightbox)
- Разработчик может выполнить предварительный просмотр дизайна пользовательского интерфейса в светлой или темной теме в Interface Builder:

  - Нижняя часть область конструктора позволяет разработчику переключать текущую тему пользовательского интерфейса:

    [![В нижней части область конструктора](user-interface-styles-images/theme10.png)](user-interface-styles-images/theme10.png#lightbox)

  - Новая тема отобразится в Interface Builder и будут отображены определенные корректировки коллекции признаков:

    [![Тема, отображаемая в Interface Builder](user-interface-styles-images/theme11.png)](user-interface-styles-images/theme11.png#lightbox)

Кроме того, симулятор tvOS теперь имеет сочетание клавиш, которое позволяет разработчику быстро переключаться между светлой и темной темами при отладке приложения tvOS. Используйте сочетание клавиш **Command-Shift-D** для переключения между светлым и темным.

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье были рассмотрены светло-и темные темы пользовательского интерфейса, добавленные компанией Apple в tvOS 10, а также способы их реализации в приложении Xamarin. tvOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [Новые возможности в tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)