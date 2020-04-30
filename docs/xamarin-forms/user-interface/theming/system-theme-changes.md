---
title: Реагирование на изменения системных тем в приложениях Xamarin. Forms
description: Приложения Xamarin. Forms могут реагировать на изменения темы операционной системы с помощью типа Онаппсеме и расширения разметки DynamicResource.
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.prod: xamarin
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
ms.openlocfilehash: c524ac0809044e576a8d56561642f6c3bf2df4a4
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82533027"
---
# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>Реагирование на изменения системных тем в приложениях Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

Устройства обычно включают в себя светло-и темные темы, каждый из которых ссылается на широкий набор предпочтений внешнего вида, который можно задать на уровне операционной системы. Приложения должны учитывать эти системные темы и отвечать сразу после изменения темы системы.

Тема системы может измениться по ряду причин, в зависимости от конфигурации устройства. К ним относится пользовательская тема, измененная пользователем, она изменяется из-за времени суток и изменяется из-за таких факторов окружающей среды, как низкая интенсивность.

Приложения Xamarin. Forms могут реагировать на изменения темы системы путем определения ресурсов с `AppThemeColor` помощью класса, `OnAppTheme<T>` класса и расширения `OnAppTheme` разметки. Затем эти ресурсы следует использовать с расширением `DynamicResource` разметки.

> [!IMPORTANT]
> Реагирование на изменение системной темы в настоящее время экспериментально и может использоваться только путем установки `AppTheme_Experimental` флага. Дополнительные сведения см. в разделе [экспериментальные флаги](~/xamarin-forms/internals/experimental-flags.md).

Для реагирования на изменение системной темы в Xamarin. Forms должны выполняться следующие требования.

- Xamarin. Forms 4,6 или более поздней версии.
- iOS 13 или более поздняя.
- Android 10 (API 29) или более поздней версии.
- UWP сборки 14393 или более поздней версии.

На следующих снимках экрана показаны страницы с темами для светлых и темных системных тем в iOS и Android:

[![Снимок экрана: Главная страница приложения с темой, на снимке экрана iOS и Android](system-theme-changes-images/main-page-both-themes.png "Главная страница приложения с темой")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "Главная страница приложения с темой")
на[![странице сведений в приложении с темой, в iOS и Android](system-theme-changes-images/detail-page-both-themes.png "Страница сведений о приложении с темой")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "Страница сведений о приложении с темой")

## <a name="define-and-consume-theme-resources"></a>Определение и использование ресурсов тем

Ресурсы для светлой и темной темы можно определить с `AppThemeColor` помощью класса, `OnAppTheme<T>` класса и расширения `OnAppTheme` разметки. При каждом подходе эти ресурсы автоматически применяются в зависимости от значения текущей системной темы. Кроме того, объекты, использующие эти ресурсы, автоматически обновляются при изменении темы системы во время работы приложения.

### <a name="appthemecolor"></a>аппсемеколор

`AppThemeColor` Класс используется для определения [`Color`](xref:Xamarin.Forms.Color) ресурсов для светлых и темных системных тем. `AppThemeColor`ресурсы должны быть определены в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Application ...>
    <Application.Resources>
        <AppThemeColor x:Key="PageBackgroundColor"
                       Light="White"
                       Dark="Black" />
        <AppThemeColor x:Key="NavigationBarColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="PrimaryColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="SecondaryColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="PrimaryTextColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="SecondaryTextColor"
                       Light="White"
                       Dark="White" />
        <AppThemeColor x:Key="TertiaryTextColor"
                       Light="Gray"
                       Dark="WhiteSmoke" />
        <AppThemeColor x:Key="TransparentColor"
                       Light="Transparent"
                       Dark="Transparent" />
    </Application.Resources>
</Application>
```

Каждый `AppThemeColor` ресурс должен иметь `x:Key` атрибут, который предоставляет его описательный ключ в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Значения свойств `Light` и `Dark` должны быть [`Color`](xref:Xamarin.Forms.Color) объектами. Кроме того, `Default` свойству может быть присвоено `Color` значение, которое по умолчанию используется для использования объектом.

`AppThemeColor`ресурсы можно использовать в строке:

```xaml
<Label Text="This monkey reacts appropriately to ridiculous assertions and actions"
       TextColor="{DynamicResource PrimaryTextColor}" />
```

Кроме того `AppThemeColor` , ресурсы могут использоваться неявно или явными [`Style`](xref:Xamarin.Forms.Style) объектами:

```xaml
<Style TargetType="NavigationPage">
    <Setter Property="BarBackgroundColor"
            Value="{DynamicResource NavigationBarColor}" />
    <Setter Property="BarTextColor"
            Value="{DynamicResource SecondaryColor}" />
</Style>
```

> [!IMPORTANT]
> `AppThemeColor`ресурсы должны использоваться с расширением `DynamicResource` разметки. Это гарантирует, что внешний вид объекта будет обновляться при изменении темы системы.

### <a name="onappthemelttgt"></a>Онаппсеме&lt;T&gt;

`OnAppTheme<T>` Класс используется для определения ресурсов любого типа для светлых и темных системных тем. `OnAppTheme<T>`ресурсы должны быть определены в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), с `T` аргументом, указанным в качестве значения `x:TypeArguments` атрибута:

```xaml
<Application ...>
    <Application.Resources>
        <OnAppTheme x:Key="ImageLogo"
                    x:TypeArguments="FileImageSource"
                    Light="lightlogo.png"
                    Dark="darklogo.png" />
    </Application.Resources>
</Application>
```

Каждый `OnAppTheme<T>` ресурс должен иметь `x:Key` атрибут, который предоставляет его описательный ключ в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Значения свойств `Light` и `Dark` должны быть объектами типа, определенного в качестве `x:TypeArguments` атрибута. Кроме того `Default` , свойству может быть присвоено значение объекта типа `T` , используемого по умолчанию для использования объектом.

`OnAppTheme<T>`ресурсы можно использовать в строке:

```xaml
<Image Source="{DynamicResource ImageLogo}"
       Aspect="AspectFit"
       HeightRequest="200" /
```

Кроме того `OnAppTheme<T>` , ресурсы могут использоваться неявно или явными [`Style`](xref:Xamarin.Forms.Style) объектами:

```xaml
<Style x:Key="imageLogoStyle"
       TargetType="Image">
    <Setter Property="Source"
            Value="{DynamicResource ImageLogo}" />
    <Setter Property="Aspect"
            Value="AspectFit" />
</Style>
```

> [!IMPORTANT]
> `OnAppTheme<T>`ресурсы должны использоваться с расширением `DynamicResource` разметки. Это гарантирует, что внешний вид объекта будет обновляться при изменении темы системы.

### <a name="onapptheme-markup-extension"></a>Расширение разметки Онаппсеме

Расширение `OnAppTheme` разметки позволяет указать ресурс, который будет использоваться, например изображение или цвет, на основе текущей системной темы. Он предоставляет те же функциональные возможности, `OnAppTheme<T>` что и класс, но с более кратким представлением:

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Image Source="{OnAppTheme Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

В этом примере цвет текста первого [`Label`](xref:Xamarin.Forms.Label) объекта задается зеленым цветом, когда устройство использует его светлую тему, и задается красным, если устройство использует его темную тему. Аналогичным образом [`Image`](xref:Xamarin.Forms.Image) , отображает другой файл изображения на основе текущей системной темы.

Дополнительные сведения о расширении разметки см. в `OnAppTheme` разделе [расширение разметки онаппсеме](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension).

## <a name="detect-the-current-system-theme"></a>Обнаружение текущей системной темы

Текущая тема системы может быть обнаружена путем получения значения `Application.RequestedTheme` свойства:

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

`RequestedTheme` Свойство возвращает элемент `OSAppTheme` перечисления. Перечисление `OSAppTheme` определяет следующие члены:

- `Unspecified`, что означает, что устройство использует неуказанную тему.
- `Light`, что означает, что устройство использует светло-тему.
- `Dark`, что означает, что устройство использует его темную тему.

## <a name="react-to-theme-changes"></a>Реагирование на изменения темы

Тема системы на устройстве может измениться по ряду причин в зависимости от настройки устройства. Приложения Xamarin. Forms могут получать уведомления при изменении темы системы, обрабатывая `Application.RequestedThemeChanged` событие:

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

`AppThemeChangedEventArgs` Объект, который прилагается `RequestedThemeChanged` к событию, имеет одно свойство `RequestedTheme`с именем типа `OSAppTheme`. Это свойство можно проверить для обнаружения требуемой системной темы.

## <a name="related-links"></a>Связанные ссылки

- [Системсемес (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [Расширение разметки Онаппсеме](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили в Xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
