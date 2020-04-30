---
title: Тема приложения Xamarin. Forms
description: Их можно реализовать в приложениях Xamarin. Forms, создав ResourceDictionary для каждой темы, а затем загрузив ресурсы с расширением разметки DynamicResource.
ms.prod: xamarin
ms.assetId: B7B17F66-4E37-4B50-9A57-351B62BE4FED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2019
ms.openlocfilehash: 43626c1f1581966a5b1ef65f97c177b83f19d0c2
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517602"
---
# <a name="theme-a-xamarinforms-application"></a>Тема приложения Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)

Приложения Xamarin. Forms могут динамически реагировать на изменения стиля во время выполнения с `DynamicResource` помощью расширения разметки. Это расширение разметки аналогично расширению `StaticResource` разметки в том, что оба используют ключ словаря для выборки значения из [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Однако, хотя расширение `StaticResource` разметки выполняет поиск по одному словарю, `DynamicResource` расширение разметки сохраняет ссылку на ключ словаря. Таким образом, если значение, связанное с ключом, заменяется, это изменение применяется [`VisualElement`](xref:Xamarin.Forms.VisualElement)к. Это позволяет реализовать среду выполнения в приложениях Xamarin. Forms.

Процесс реализации среды выполнения в приложении Xamarin. Forms выглядит следующим образом:

1. Определите ресурсы для каждой темы в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary).
1. Использовать ресурсы темы в приложении с помощью расширения `DynamicResource` разметки.
1. Задайте тему по умолчанию в файле **app. XAML** приложения.
1. Добавьте код для загрузки темы во время выполнения.

> [!NOTE]
> Изменение темы во время выполнения требует использования стилей XAML и в настоящее время не позволяет использовать CSS.

На следующих снимках экрана показаны страницы с темами и приложение iOS с использованием светлой темы и приложения Android с использованием темной темы:

[![Снимок экрана: Главная страница приложения с темой, на снимке экрана iOS и Android](theming-images/main-page-both-themes.png "Главная страница приложения с темой")](theming-images/main-page-both-themes-large.png#lightbox "Главная страница приложения с темой")
на[![странице сведений в приложении с темой, в iOS и Android](theming-images/detail-page-both-themes.png "Страница сведений о приложении с темой")](theming-images/detail-page-both-themes-large.png#lightbox "Страница сведений о приложении с темой")

## <a name="define-themes"></a>Определение тем

Тема определяется как коллекция объектов ресурсов, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)хранящихся в.

В следующем примере показано `LightTheme` из примера приложения.

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.LightTheme">
    <Color x:Key="PageBackgroundColor">White</Color>
    <Color x:Key="NavigationBarColor">WhiteSmoke</Color>
    <Color x:Key="PrimaryColor">WhiteSmoke</Color>
    <Color x:Key="SecondaryColor">Black</Color>
    <Color x:Key="PrimaryTextColor">Black</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">Gray</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

В следующем примере показано `DarkTheme` из примера приложения.

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ThemingDemo.DarkTheme">
    <Color x:Key="PageBackgroundColor">Black</Color>
    <Color x:Key="NavigationBarColor">Teal</Color>
    <Color x:Key="PrimaryColor">Teal</Color>
    <Color x:Key="SecondaryColor">White</Color>
    <Color x:Key="PrimaryTextColor">White</Color>
    <Color x:Key="SecondaryTextColor">White</Color>
    <Color x:Key="TertiaryTextColor">WhiteSmoke</Color>
    <Color x:Key="TransparentColor">Transparent</Color>
</ResourceDictionary>
```

Каждый [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) из [`Color`](xref:Xamarin.Forms.Color) них содержит ресурсы, определяющие соответствующие темы, `ResourceDictionary` при этом каждый из них использует идентичные значения ключа. Дополнительные сведения о словарях ресурсов см. в разделе [словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md).

> [!IMPORTANT]
> Для каждого из `ResourceDictionary` `InitializeComponent` них требуется файл кода программной части, который вызывает метод. Это необходимо, чтобы объект CLR, представляющий выбранную тему, можно было создать во время выполнения.

## <a name="set-a-default-theme"></a>Задать тему по умолчанию

Приложению требуется тема по умолчанию, чтобы элементы управления имели значения для используемых ресурсов. Тему по умолчанию можно задать путем объединения темы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) к уровню `ResourceDictionary` приложения, определенному в **app. XAML**:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>
        <ResourceDictionary Source="Themes/LightTheme.xaml" />
    </Application.Resources>
</Application>
```

Дополнительные сведения о слиянии словарей ресурсов см. в разделе [Объединенные словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md#merged-resource-dictionaries).

## <a name="consume-theme-resources"></a>Использование ресурсов темы

Когда приложению требуется использовать ресурс, хранящийся в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , который представляет тему, он должен сделать это с расширением `DynamicResource` разметки. Это гарантирует, что если во время выполнения выбрана другая тема, будут применены значения из новой темы.

В следующем примере показаны три стиля из примера приложения, которые можно применить к [`Label`](xref:Xamarin.Forms.Label) объектам:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ThemingDemo.App">
    <Application.Resources>

        <Style x:Key="LargeLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource SecondaryTextColor}" />
            <Setter Property="FontSize"
                    Value="30" />
        </Style>

        <Style x:Key="MediumLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource PrimaryTextColor}" />
            <Setter Property="FontSize"
                    Value="25" />
        </Style>

        <Style x:Key="SmallLabelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{DynamicResource TertiaryTextColor}" />
            <Setter Property="FontSize"
                    Value="15" />
        </Style>

    </Application.Resources>
</Application>
```

Эти стили определяются в словаре ресурсов уровня приложения, чтобы их можно было использовать на нескольких страницах. Каждый стиль использует ресурсы темы с расширением `DynamicResource` разметки.

Затем эти стили используются страницами:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ThemingDemo"
             x:Class="ThemingDemo.UserSummaryPage"
             Title="User Summary"
             BackgroundColor="{DynamicResource PageBackgroundColor}">
    ...
    <ScrollView>
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="200" />
                <RowDefinition Height="120" />
                <RowDefinition Height="70" />
            </Grid.RowDefinitions>
            <Grid BackgroundColor="{DynamicResource PrimaryColor}">
                <Label Text="Face-Palm Monkey"
                       VerticalOptions="Center"
                       Margin="15"
                       Style="{StaticResource MediumLabelStyle}" />
                ...
            </Grid>
            <StackLayout Grid.Row="1"
                         Margin="10">
                <Label Text="This monkey reacts appropriately to ridiculous assertions and actions."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Cynical but not unfriendly."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Seven varieties of grimaces."
                       Style="{StaticResource SmallLabelStyle}" />
                <Label Text="  &#x2022; Doesn't laugh at your jokes."
                       Style="{StaticResource SmallLabelStyle}" />
            </StackLayout>
            ...
        </Grid>
    </ScrollView>
</ContentPage>
```

Когда ресурс темы используется напрямую, его следует использовать с расширением `DynamicResource` разметки. Однако при использовании стиля, использующего расширение `DynamicResource` разметки, его следует использовать с расширением `StaticResource` разметки.

Дополнительные сведения о стилях см. в разделе [стилизация приложений Xamarin. Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md). Дополнительные сведения о расширении `DynamicResource` разметки см. в разделе [динамические стили в Xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md).

## <a name="load-a-theme-at-runtime"></a>Загрузка темы во время выполнения

При выборе темы во время выполнения приложение должно:

1. Удалить текущую тему из приложения. Это достигается путем очистки [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) свойства уровня [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)приложения.
2. Загрузить выбранную тему. Это достигается путем добавления экземпляра выбранной темы к `MergedDictionaries` свойству уровня `ResourceDictionary`приложения.

Все [`VisualElement`](xref:Xamarin.Forms.VisualElement) объекты, устанавливающие свойства с `DynamicResource` расширением разметки, будут применять новые значения темы. Это происходит потому, `DynamicResource` что расширение разметки поддерживает ссылку на ключи словаря. Поэтому при замене значений, связанных с ключами, изменения применяются к `VisualElement` объектам.

В примере приложения тема выбирается с помощью модальной страницы, содержащей [`Picker`](xref:Xamarin.Forms.Picker). В следующем коде показан `OnPickerSelectionChanged` метод, который выполняется при изменении выбранной темы:

```csharp
void OnPickerSelectionChanged(object sender, EventArgs e)
{
    Picker picker = sender as Picker;
    Theme theme = (Theme)picker.SelectedItem;

    ICollection<ResourceDictionary> mergedDictionaries = Application.Current.Resources.MergedDictionaries;
    if (mergedDictionaries != null)
    {
        mergedDictionaries.Clear();

        switch (theme)
        {
            case Theme.Dark:
                mergedDictionaries.Add(new DarkTheme());
                break;
            case Theme.Light:
            default:
                mergedDictionaries.Add(new LightTheme());
                break;
        }
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Их (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [Реагирование на изменения темы системы](system-theme-changes.md)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили в Xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
