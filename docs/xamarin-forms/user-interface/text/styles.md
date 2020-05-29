---
title: Xamarin.FormsСтили текста
description: В этой статье объясняется, как выполнять стилизацию текста в Xamarin.Forms приложениях. Стили можно определить один раз и использовать во многих представлениях, но стиль можно использовать только с представлениями одного типа.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 79a86fd7a2c0f5b82ca4b3e22b3ecedf42c5a0ba
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136153"
---
# <a name="xamarinforms-text-styles"></a>Xamarin.FormsСтили текста

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Стилизация текста в Xamarin. Forms_

Стили можно использовать для настройки внешнего вида меток, записей и редакторов. Стили можно определить один раз и использовать во многих представлениях, но стиль можно использовать только с представлениями одного типа.
Стили могут быть предоставлены `Key` и применены выборочно с помощью свойства конкретного элемента управления `Style` .

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Встроенные стили

Xamarin.Formsвключает несколько [встроенных](xref:Xamarin.Forms.Device.Styles) стилей для распространенных сценариев:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Чтобы применить один из встроенных стилей, используйте `DynamicResource` расширение разметки для указания стиля:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

В C# встроенные стили выбираются из `Device.Styles` :

```csharp
label.Style = Device.Styles.TitleStyle;
```

![Пример стилей устройств](styles-images/builtinstyles.png)

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Пользовательские стили

Стили, состоящие из методов задания и задания, состоят из свойств и значений, которые будут установлены для свойств.

В C# пользовательский стиль для метки с красным текстом размером 30 будет определяться следующим образом:

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

В XAML:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

Обратите внимание, что ресурсы (включая все стили) определены в `ContentPage.Resources` , что является элементом того же уровня, что и более привычный `ContentPage.Content` элемент.

![Пример пользовательских стилей](styles-images/customstyle.png)

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Применение стилей

После создания стиля его можно применить к любому представлению, соответствующему его свойству `TargetType` .

В XAML пользовательские стили применяются к представлениям путем предоставления своего `Style` свойства с `StaticResource` расширением разметки, ссылающимся на нужный стиль:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

В C# стили можно либо применять непосредственно к представлению, либо добавлять и получать из страницы `ResourceDictionary` . Добавление непосредственно:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Для добавления и извлечения из страницы `ResourceDictionary` :

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Встроенные стили применяются по-разному, так как они должны отвечать на параметры специальных возможностей. Для применения встроенных стилей в XAML `DynamicResource` используется расширение разметки:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

В C# встроенные стили выбираются из `Device.Styles` :

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Специальные возможности

Существуют встроенные стили, облегчающие соблюдение настроек специальных возможностей. При использовании любого из встроенных стилей размеры шрифтов автоматически увеличиваются, если пользователь настроит свои настройки специальных возможностей соответствующим образом.

Рассмотрим следующий пример той же страницы представлений, стиль которой включает встроенные стили с включенными и отключенными параметрами специальных возможностей:

Отключено.

![Стили устройств с отключенной поддержкой специальных возможностей](styles-images/pre-access.png)

Включено:

![Стили устройств с включенной поддержкой специальных возможностей](styles-images/post-access.png)

Чтобы обеспечить специальные возможности, убедитесь, что встроенные стили используются в качестве базового для любых текстовых стилей в приложении, и что вы используете единообразные стили. Дополнительные сведения о расширении и работе со стилями в целом см. в разделе [стили](~/xamarin-forms/user-interface/styles/index.md) .

## <a name="related-links"></a>Связанные ссылки

- [Создание мобильных приложений с помощью Xamarin.Forms , глава 12](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Стили](~/xamarin-forms/user-interface/styles/index.md)
- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Стиль](xref:Xamarin.Forms.Style)
