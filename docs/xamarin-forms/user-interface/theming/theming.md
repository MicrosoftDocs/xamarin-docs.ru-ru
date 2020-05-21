---
title: Тема приложения Xamarin. Forms
description: Их можно реализовать в приложениях Xamarin. Forms, создав ResourceDictionary для каждой темы, а затем загрузив ресурсы с расширением разметки DynamicResource.
ms.prod: xamarin
ms.assetId: B7B17F66-4E37-4B50-9A57-351B62BE4FED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2019
ms.openlocfilehash: 2f751549bdbd54b920a99b04d0068ab726600336
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/15/2020
ms.locfileid: "83425796"
---
# <a name="theme-a-xamarinforms-application"></a>Тема приложения Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)

Приложения Xamarin. Forms могут динамически реагировать на изменения стиля во время выполнения с помощью `DynamicResource` расширения разметки. Это расширение разметки аналогично `StaticResource` расширению разметки в том, что оба используют ключ словаря для выборки значения из [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Однако, хотя `StaticResource` расширение разметки выполняет поиск по одному словарю, `DynamicResource` расширение разметки сохраняет ссылку на ключ словаря. Таким образом, если значение, связанное с ключом, заменяется, это изменение применяется к [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Это позволяет реализовать среду выполнения в приложениях Xamarin. Forms.

Процесс реализации среды выполнения в приложении Xamarin. Forms выглядит следующим образом:

1. Определите ресурсы для каждой темы в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) .
1. Использовать ресурсы темы в приложении с помощью `DynamicResource` расширения разметки.
1. Задайте тему по умолчанию в файле **app. XAML** приложения.
1. Добавьте код для загрузки темы во время выполнения.

> [!IMPORTANT]
> Используйте `StaticResource` расширение разметки, если не нужно изменять тему приложения во время выполнения.

На следующих снимках экрана показаны страницы с темами и приложение iOS с использованием светлой темы и приложения Android с использованием темной темы:

[![Снимок экрана: Главная страница приложения с темой, в iOS и Android](theming-images/main-page-both-themes.png "Главная страница приложения с темой")](theming-images/main-page-both-themes-large.png#lightbox "Главная страница приложения с темой") 
 [ ![Снимок экрана со страницей сведений о приложении с темой в iOS и Android](theming-images/detail-page-both-themes.png "Страница сведений о приложении с темой")](theming-images/detail-page-both-themes-large.png#lightbox "Страница сведений о приложении с темой")

> [!NOTE]
> Изменение темы во время выполнения требует использования стилей XAML и в настоящее время не позволяет использовать CSS.

## <a name="define-themes"></a>Определение тем

Тема определяется как коллекция объектов ресурсов, хранящихся в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) .

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

Каждый [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) из них содержит [`Color`](xref:Xamarin.Forms.Color) ресурсы, определяющие соответствующие темы, при этом каждый `ResourceDictionary` из них использует идентичные значения ключа. Дополнительные сведения о словарях ресурсов см. в разделе [словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md).

> [!IMPORTANT]
> Для каждого из них требуется файл кода программной части `ResourceDictionary` , который вызывает `InitializeComponent` метод. Это необходимо, чтобы объект CLR, представляющий выбранную тему, можно было создать во время выполнения.

## <a name="set-a-default-theme"></a>Задать тему по умолчанию

Приложению требуется тема по умолчанию, чтобы элементы управления имели значения для используемых ресурсов. Тему по умолчанию можно задать путем объединения темы к [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) уровню приложения `ResourceDictionary` , определенному в **app. XAML**:

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

Когда приложению требуется использовать ресурс, хранящийся в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , который представляет тему, он должен сделать это с `DynamicResource` расширением разметки. Это гарантирует, что если во время выполнения выбрана другая тема, будут применены значения из новой темы.

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

Эти стили определяются в словаре ресурсов уровня приложения, чтобы их можно было использовать на нескольких страницах. Каждый стиль использует ресурсы темы с `DynamicResource` расширением разметки.

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

Когда ресурс темы используется напрямую, его следует использовать с `DynamicResource` расширением разметки. Однако при использовании стиля, использующего `DynamicResource` расширение разметки, его следует использовать с `StaticResource` расширением разметки.

Дополнительные сведения о стилях см. в разделе [стилизация приложений Xamarin. Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md). Дополнительные сведения о `DynamicResource` расширении разметки см. [в разделе динамические стили в Xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md).

## <a name="load-a-theme-at-runtime"></a>Загрузка темы во время выполнения

При выборе темы во время выполнения приложение должно:

1. Удалить текущую тему из приложения. Это достигается путем очистки [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Свойства уровня приложения [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) .
2. Загрузить выбранную тему. Это достигается путем добавления экземпляра выбранной темы к `MergedDictionaries` свойству уровня приложения `ResourceDictionary` .

Все [`VisualElement`](xref:Xamarin.Forms.VisualElement) объекты, устанавливающие свойства с `DynamicResource` расширением разметки, будут применять новые значения темы. Это происходит потому, что `DynamicResource` расширение разметки поддерживает ссылку на ключи словаря. Поэтому при замене значений, связанных с ключами, изменения применяются к `VisualElement` объектам.

В примере приложения тема выбирается с помощью модальной страницы, содержащей [`Picker`](xref:Xamarin.Forms.Picker) . В следующем коде показан `OnPickerSelectionChanged` метод, который выполняется при изменении выбранной темы:

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
