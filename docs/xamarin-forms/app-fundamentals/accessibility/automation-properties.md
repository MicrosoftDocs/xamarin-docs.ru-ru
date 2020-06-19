---
title: Свойства автоматизации
description: Эта статья поясняет, как использовать класс AutomationProperties в приложении Xamarin.Forms, чтобы средство чтения с экрана могло озвучивать элементы на странице.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 341e1454c37d1fd20423204c95337e580eba7ecb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573408"
---
# <a name="automation-properties-in-xamarinforms"></a>Свойства автоматизации в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)

_Xamarin.Forms позволяет задавать значения специальных возможностей в элементах пользовательского интерфейса с помощью присоединенных свойств из класса AutomationProperties, которые, в свою очередь, задают собственные значения специальных возможностей. Эта статья поясняет, как использовать класс AutomationProperties, чтобы средство чтения с экрана могло озвучивать элементы на странице._

Xamarin.Forms позволяет задавать свойства автоматизации в элементах пользовательского интерфейса с помощью следующих присоединенных свойств:

- `AutomationProperties.IsInAccessibleTree` — указывает, доступен ли элемент приложению специальных возможностей. Дополнительные сведения см. в разделе [AutomationProperties.IsInAccessibleTree](#automationpropertiesisinaccessibletree).
- `AutomationProperties.Name` — краткое описание элемента, выступающего в качестве произносимого идентификатора для элемента. Дополнительные сведения см. в разделе [AutomationProperties.Name](#automationpropertiesname).
- `AutomationProperties.HelpText` — более подробное описание элемента, которое можно рассматривать как текст подсказки, связанный с элементом. Дополнительные сведения см. в разделе [AutomationProperties.HelpText](#automationpropertieshelptext).
- `AutomationProperties.LabeledBy` — позволяет другому элементу определить сведения о специальных возможностях для текущего элемента. Дополнительные сведения см. в разделе [AutomationProperties.LabeledBy](#automationpropertieslabeledby).

Эти присоединенные свойства задают собственные значения специальных возможностей таким образом, чтобы средство чтения с экрана могло озвучить сведения об элементе. Дополнительные сведения о присоединенных свойствах см. в разделе [Присоединенные свойства](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Использование присоединенных свойств `AutomationProperties` может влиять на выполнение тестов пользовательского интерфейса в Android. Свойства [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId), `AutomationProperties.Name` и `AutomationProperties.HelpText` задают собственное свойство `ContentDescription`, при этом значения свойств `AutomationProperties.Name` и `AutomationProperties.HelpText` имеют приоритет над значением `AutomationId` (если задано как `AutomationProperties.Name`, так и `AutomationProperties.HelpText`, эти значения будут сцеплены). Это означает, что все тесты, ищущие `AutomationId`, завершатся ошибкой, если для элемента также задано `AutomationProperties.Name` или `AutomationProperties.HelpText`. В этом случае тесты пользовательского интерфейса следует изменить для поиска значения `AutomationProperties.Name` или `AutomationProperties.HelpText` либо объединения этих значений.

Каждая платформа имеет отдельное средство чтения с экрана для озвучивания значений специальных возможностей:

- В iOS есть VoiceOver. Дополнительные сведения см. в статье [Тестирование специальных возможностей на устройстве с использованием VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) на сайте developer.apple.com.
- В Android есть TalkBack. Дополнительные сведения см. в статье [Тестирование специальных возможностей приложения](https://developer.android.com/training/accessibility/testing.html#talkback) на сайте developer.android.com.
- В Windows есть экранный диктор. Дополнительные сведения см. в статье [Проверка основных сценариев приложения с использованием экранного диктора](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator).

Однако точное поведение средства чтения с экрана зависит от программного обеспечения и его настройки пользователем. Например, большинство средств чтения с экрана зачитывают текст с элемента управления, когда на него переключается фокус, что позволяет пользователям ориентироваться при переходе между элементами управления на странице. Некоторые средства чтения с экрана также считывают весь пользовательский интерфейс приложения при отображении страницы, что позволяет пользователю получить все доступные на странице сведения, прежде чем пытаться перемещаться по ней.

Кроме того, средства чтения с экрана зачитывают разные значения специальных возможностей. В рассматриваемом примере приложения:

- VoiceOver зачитает значение `Placeholder` объекта `Entry`, а затем инструкцию по использованию этого элемента управления;
- TalkBack зачитает значение `Placeholder` объекта `Entry`, затем значение `AutomationProperties.HelpText`, а затем инструкцию по использованию этого элемента управления;
- экранный диктор зачитает значение `AutomationProperties.LabeledBy` объекта `Entry`, а затем инструкцию по использованию этого элемента управления.

Кроме того, экранный диктор расставит приоритеты следующим образом: `AutomationProperties.Name`, `AutomationProperties.LabeledBy` и затем `AutomationProperties.HelpText`. TalkBack в Android может сочетать значения `AutomationProperties.Name` и `AutomationProperties.HelpText`. Поэтому для обеспечения оптимальной работы рекомендуется тщательно протестировать специальные возможности для каждой платформы.

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

Присоединенное свойство `AutomationProperties.IsInAccessibleTree` является `boolean`, определяющим, доступен ли элемент (и, соответственно, видят ли его средства чтения с экрана). Чтобы использовать другие присоединенные свойства специальных возможностей, для него нужно задать значение `true`. Это можно сделать в XAML следующим образом:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Кроме того, его можно задать в C# следующим образом:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Обратите внимание, что метод [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) также можно использовать для задания присоединенного свойства `AutomationProperties.IsInAccessibleTree` — `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`.

## <a name="automationpropertiesname"></a>AutomationProperties.Name

Значением присоединенного свойства `AutomationProperties.Name` должна быть короткая описательная текстовая строка, которую средство чтения с экрана использует для объявления элемента. Это свойство должно быть задано для элементов, значение которых важно для понимания содержимого или взаимодействия с пользовательским интерфейсом. Это можно сделать в XAML следующим образом:

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Кроме того, его можно задать в C# следующим образом:

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Обратите внимание, что метод [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) также можно использовать для задания присоединенного свойства `AutomationProperties.Name` — `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`.

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

Для присоединенного свойства `AutomationProperties.HelpText` нужно задать текст, который описывает данный элемент пользовательского интерфейса и может рассматриваться как текст подсказки, связанный с элементом. Это можно сделать в XAML следующим образом:

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Кроме того, его можно задать в C# следующим образом:

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Обратите внимание, что метод [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) также можно использовать для задания присоединенного свойства `AutomationProperties.HelpText` — `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`.

На некоторых платформах для редактирования элементов управления, таких как [`Entry`](xref:Xamarin.Forms.Entry), свойство `HelpText` иногда можно опустить и заменить текстом заполнителя. Например, элемент "Введите здесь свое имя" является хорошим кандидатом для использования в свойстве [`Entry.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder), которое помещает текст в элемент управления до фактического ввода данных пользователем.

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

Присоединенное свойство `AutomationProperties.LabeledBy` позволяет другому элементу определить сведения о специальных возможностях для текущего элемента. Например, расположение [`Label`](xref:Xamarin.Forms.Label) рядом с [`Entry`](xref:Xamarin.Forms.Entry) можно использовать для описания того, что представляет `Entry`. Это можно сделать в XAML следующим образом:

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Кроме того, его можно задать в C# следующим образом:

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Обратите внимание, что метод [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) также можно использовать для задания присоединенного свойства `AutomationProperties.IsInAccessibleTree` — `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`.

## <a name="accessibility-intricacies"></a>Особенности специальных возможностей

Следующие разделы описывают особенности настройки специальных возможностей у определенных элементов управления.

### <a name="navigationpage"></a>NavigationPage

На Android, чтобы задать текст средства чтения с экрана, который будет произноситься для стрелки "назад" в панели действий [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), используйте свойства `AutomationProperties.Name` и `AutomationProperties.HelpText` класса [`Page`](xref:Xamarin.Forms.Page). Однако учтите, что это не относится к кнопкам "назад" в операционной системе.

### <a name="masterdetailpage"></a>MasterDetailPage

На iOS и универсальной платформе Windows (UWP), чтобы задать текст средства чтения с экрана, который будет произноситься для выключателя в [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage), используйте либо свойства `AutomationProperties.Name` и `AutomationProperties.HelpText` класса `MasterDetailPage`, либо свойство `IconImageSource` страницы `Master`.

На Android, чтобы задать текст средства чтения с экрана, который будет произноситься для выключателя в [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage), добавьте в проект Android следующие строковые ресурсы:

```xml
<resources>
    <string name="app_name">Xamarin Forms Control Gallery</string>
    <string name="btnMDPAutomationID_open">Open Side Menu message</string>
    <string name="btnMDPAutomationID_close">Close Side Menu message</string>
</resources>
```

Затем назначьте нужную строку свойству `AutomationId` в свойстве `IconImageSource` страницы `Master`:

```csharp
var master = new ContentPage { ... };
master.IconImageSource.AutomationId = "btnMDPAutomationID";
```

### <a name="toolbaritem"></a>ToolbarItem

На iOS, Android и UWP средства чтения с экрана будут произносить значение свойства `Text` экземпляров [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) при условии, что не заданы значения `AutomationProperties.Name` или `AutomationProperties.HelpText`.

На iOS и UWP значение свойства `AutomationProperties.Name` заменит значение `Text`, произносимое средством чтения с экрана.

На Android значения свойств `AutomationProperties.Name` и `AutomationProperties.HelpText` полностью заменят значение `Text`, являющееся отображаемым и одновременно произносимым средством чтения с экрана. Это является ограничением API-интерфейсов вплоть до уровня 26.

## <a name="related-links"></a>Связанные ссылки

- [Вложенные свойства](~/xamarin-forms/xaml/attached-properties.md)
- [Специальные возможности (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)
