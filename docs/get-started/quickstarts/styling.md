---
title: Стилизация кроссплатформенного приложения Xamarin.Forms
description: В этой статье объясняется, как применить стили XAML к многоплатформенному приложению Оболочки в Xamarin.Forms и использовать Горячую перезагрузку XAML для просмотра этих изменений.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 1C6ED8AF-41B4-4D6C-9BD8-C72D3F05E541
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.custom: contperf-fy21q3
ms.date: 01/26/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0424f3c9cdcde98cef4c414ada33046cd8ce7ee6
ms.sourcegitcommit: 1f391667869a4541dd9b42d78862dc01d69ed160
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99818140"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>Стилизация кроссплатформенного приложения Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

В этом кратком руководстве рассматриваются следующие темы:

- Стилизация приложения Оболочки в Xamarin.Forms с использованием стилей XAML.
- Использование Горячей перезагрузки XAML для просмотра изменений пользовательского интерфейса без перестроения приложения.

Из этого краткого руководства вы узнаете, как изменить стиль кроссплатформенного приложения Xamarin.Forms с помощью стилей XAML. Кроме того, в рамках краткого руководства выполняется обновление пользовательского интерфейса работающего приложения с использованием Горячей перезагрузки XAML без перестраивания приложения. Дополнительные сведения о Горячей перезагрузке XAML см. в статье [Горячая перезагрузка XAML для Xamarin.Forms](~/xamarin-forms/xaml/hot-reload.md).

Ниже показано итоговое приложение:

[![Страница заметок](styling-images/screenshots1-sml.png)](styling-images/screenshots1.png#lightbox)
[![Страница ввода заметки](styling-images/screenshots2-sml.png)](styling-images/screenshots2.png#lightbox)
[![Страница "О программе"](styling-images/screenshots3-sml.png)](styling-images/screenshots3.png#lightbox)

### <a name="prerequisites"></a>Предварительные требования

Прежде чем приступать к этому краткому руководству, необходимо успешно завершить [предыдущее](database.md). Также вы можете скачать [пример из предыдущего краткого руководства](/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/) и использовать его в качестве отправной точки для работы с этим руководством.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Обновление приложения с помощью Visual Studio

1. Запустите Visual Studio и откройте решение Notes.

2. Создайте и запустите проект на выбранной платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](app.md#building-the-quickstart).

    Оставьте приложение работающим и вернитесь в Visual Studio.

3. В **обозревателе решений** откройте файл **App.xaml** в проекте **Notes**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

        <!-- Resources used by multiple pages in the application -->         
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppPrimaryColor">#1976D2</Color>
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="PrimaryColor">Black</Color>
            <Color x:Key="SecondaryColor">White</Color>
            <Color x:Key="TertiaryColor">Silver</Color>

            <!-- Implicit styles -->
            <Style TargetType="ContentPage"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button">
                <Setter Property="FontSize"
                        Value="Medium" />
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="TextColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="CornerRadius"
                        Value="5" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    Этот код определяет значение [`Thickness`](xref:Xamarin.Forms.Thickness), ряд значений [`Color`](xref:Xamarin.Forms.Color), а также неявные стили для типов [`ContentPage`](xref:Xamarin.Forms.ContentPage) и [`Button`](xref:Xamarin.Forms.Button). Обратите внимание, что эти стили находятся на уровне приложения [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и могут использоваться по всему приложению. Дополнительные сведения об использовании стилей XAML см. в разделе [Задание стиля](deepdive.md#styling) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **App.xaml**, нажав клавиши **CTRL+S**. При Горячей загрузке XAML будет обновлен пользовательский интерфейс работающего приложения без перестроения приложения. В частности, изменится цвет фона каждой страницы.

4. В **обозревателе решений** откройте файл **AppShell.xaml** в проекте **Notes**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">

        <Shell.Resources>
            <!-- Style Shell elements -->
            <Style x:Key="BaseStyle"
                   TargetType="Element">
                <Setter Property="Shell.BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="Shell.ForegroundColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TitleColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TabBarUnselectedColor"
                        Value="#95FFFFFF"/>
            </Style>
            <Style TargetType="TabBar"
                   BasedOn="{StaticResource BaseStyle}" />
        </Shell.Resources>

        <!-- Display a bottom tab bar containing two tabs -->   
        <TabBar>
            <ShellContent Title="Notes"
                          Icon="icon_feed.png"
                          ContentTemplate="{DataTemplate views:NotesPage}" />
            <ShellContent Title="About"
                          Icon="icon_about.png"
                          ContentTemplate="{DataTemplate views:AboutPage}" />
        </TabBar>
    </Shell>
    ```

    Этот код добавляет два стиля в словарь ресурсов `Shell`, который определяет ряд значений [`Color`](xref:Xamarin.Forms.Color), используемых приложением.

    Сохраните изменения в файле **AppShell.xaml**, нажав клавиши **CTRL+S**. При Горячей загрузке XAML будет обновлен пользовательский интерфейс работающего приложения без перестроения приложения. В частности, изменится цвет фона для хрома Оболочки.

5. В **обозревателе решений** в проекте **Notes** откройте **NotesPage.xaml** в папке **Views**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">

        <ContentPage.Resources>
            <!-- Define a visual state for the Selected state of the CollectionView -->
            <Style TargetType="StackLayout">
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal" />
                            <VisualState x:Name="Selected">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor"
                                            Value="{StaticResource AppPrimaryColor}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>
        </ContentPage.Resources>

        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="{StaticResource PageMargin}"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium" />
                        <Label Text="{Binding Date}"
                               TextColor="{StaticResource TertiaryColor}"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    Этот код добавляет неявный стиль для объекта [`StackLayout`](xref:Xamarin.Forms.StackLayout), который определяет внешний вид каждого выбранного элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView), на уровне страницы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и задает для `CollectionView.Margin` и свойства `Label.TextColor` значения, определенные в `ResourceDictionary` на уровне приложения. Обратите внимание, что неявный стиль `StackLayout` был добавлен на уровне страницы `ResourceDictionary`, поскольку он используется только в `NotesPage`.

    Сохраните изменения в файле **NotesPage.xaml**, нажав клавиши **CTRL+S**. При Горячей загрузке XAML будет обновлен пользовательский интерфейс работающего приложения без перестроения приложения. В частности, изменится цвет выбранных элементов в [`CollectionView`](xref:Xamarin.Forms.CollectionView).

6. В **обозревателе решений** в проекте **Notes** откройте **NoteEntryPage.xaml** в папке **Views**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>         
        </ContentPage.Resources>

        <!-- Layout children vertically -->
        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <Grid ColumnDefinitions="*,*">
                <!-- Layout children in two columns -->
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Этот код добавляет неявный стиль для [`Editor`](xref:Xamarin.Forms.Editor) на уровне страницы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и присваивает свойству `StackLayout.Margin` значение, определенное на уровне приложения `ResourceDictionary`. Обратите внимание, что неявные стили `Editor` добавлены в `ResourceDictionary` на уровне страницы, так как он используется только в объекте `NoteEntryPage`.

7. В работающем приложении перейдите к объекту `NoteEntryPage`.

8. В Visual Studio сохраните изменения в **NoteEntryPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

    При Горячей загрузке XAML будет обновлен пользовательский интерфейс приложения без его перестроения. В частности, цвет фона [`Editor`](xref:Xamarin.Forms.Editor) в работающем приложении изменится, как и внешний вид объектов [`Button`](xref:Xamarin.Forms.Button).

9. В **обозревателе решений** в проекте **Notes** откройте **AboutPage.xaml** в папке **Views**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{StaticResource AppPrimaryColor}"
                   Opacity="0.85"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->       
            <StackLayout Grid.Row="1"
                         Margin="{StaticResource PageMargin}"
                         Spacing="20">
                <Label FontSize="22">
                    <Label.FormattedText>
                        <FormattedString>
                            <FormattedString.Spans>
                                <Span Text="Notes"
                                      FontAttributes="Bold"
                                      FontSize="22" />
                                <Span Text=" v1.0" />
                            </FormattedString.Spans>
                        </FormattedString>
                    </Label.FormattedText>
                </Label>
                <Label Text="This app is written in XAML and C# with the Xamarin Platform." />
                <Button Text="Learn more"
                        Clicked="OnButtonClicked" />
            </StackLayout>
        </Grid>
    </ContentPage>
    ```

    Этот код задает для свойств `Image.BackgroundColor` и `StackLayout.Margin` значения, определенные в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) на уровне приложения.

10. В работающем приложении перейдите к объекту `AboutPage`.

11. В Visual Studio сохраните изменения в файле **AboutPage.xaml**, нажав клавиши **CTRL+S**.

    При Горячей загрузке XAML будет обновлен пользовательский интерфейс приложения без его перестроения. В частности, изменится цвет фона объекта [`Image`](xref:Xamarin.Forms.Editor) в работающем приложении.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Обновление приложения с помощью Visual Studio для Mac

1. Запустите Visual Studio для Mac и откройте проект Notes.

2. Создайте и запустите проект на выбранной платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](app.md#building-the-quickstart).

    Оставьте приложение работающим и вернитесь в Visual Studio для Mac.

3. На **Панели решения** откройте файл **App.xaml** в проекте **Notes**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

        <!-- Resources used by multiple pages in the application -->                 
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppPrimaryColor">#1976D2</Color>
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="PrimaryColor">Black</Color>
            <Color x:Key="SecondaryColor">White</Color>
            <Color x:Key="TertiaryColor">Silver</Color>

            <!-- Implicit styles -->
            <Style TargetType="ContentPage"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button">
                <Setter Property="FontSize"
                        Value="Medium" />
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="TextColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="CornerRadius"
                        Value="5" />
            </Style>  
        </Application.Resources>
    </Application>
    ```

    Этот код определяет значение [`Thickness`](xref:Xamarin.Forms.Thickness), ряд значений [`Color`](xref:Xamarin.Forms.Color), а также неявные стили для типов [`ContentPage`](xref:Xamarin.Forms.ContentPage) и [`Button`](xref:Xamarin.Forms.Button). Обратите внимание, что эти стили находятся на уровне приложения [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и могут использоваться по всему приложению. Дополнительные сведения об использовании стилей XAML см. в разделе [Задание стиля](deepdive.md#styling) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **App.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**. При Горячей загрузке XAML будет обновлен пользовательский интерфейс работающего приложения без перестроения приложения. В частности, изменится цвет фона каждой страницы.

4. На **Панели решения** откройте файл **AppShell.xaml** в проекте **Notes**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">

        <Shell.Resources>
            <!-- Style Shell elements -->
            <Style x:Key="BaseStyle"
                   TargetType="Element">
                <Setter Property="Shell.BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="Shell.ForegroundColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TitleColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TabBarUnselectedColor"
                        Value="#95FFFFFF"/>
            </Style>
            <Style TargetType="TabBar"
                   BasedOn="{StaticResource BaseStyle}" />
        </Shell.Resources>

        <!-- Display a bottom tab bar containing two tabs -->
        <TabBar>
            <ShellContent Title="Notes"
                          Icon="icon_feed.png"
                          ContentTemplate="{DataTemplate views:NotesPage}" />
            <ShellContent Title="About"
                          Icon="icon_about.png"
                          ContentTemplate="{DataTemplate views:AboutPage}" />
        </TabBar>
    </Shell>
    ```

    Этот код добавляет два стиля в словарь ресурсов `Shell`, который определяет ряд значений [`Color`](xref:Xamarin.Forms.Color), используемых приложением.

    Сохраните изменения в файле **AppShell.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**. При Горячей загрузке XAML будет обновлен пользовательский интерфейс работающего приложения без перестроения приложения. В частности, изменится цвет фона для хрома Оболочки.

5. На **Панели решения** в проекте **Notes** откройте **NotesPage.xaml** в папке **Views**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">

        <ContentPage.Resources>
            <!-- Define a visual state for the Selected state of the CollectionView -->
            <Style TargetType="StackLayout">
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal" />
                            <VisualState x:Name="Selected">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor"
                                            Value="{StaticResource AppPrimaryColor}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>
        </ContentPage.Resources>

        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="{StaticResource PageMargin}"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium" />
                        <Label Text="{Binding Date}"
                               TextColor="{StaticResource TertiaryColor}"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    Этот код добавляет неявный стиль для объекта [`StackLayout`](xref:Xamarin.Forms.StackLayout), который определяет внешний вид каждого выбранного элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView), на уровне страницы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и задает для `CollectionView.Margin` и свойства `Label.TextColor` значения, определенные в `ResourceDictionary` на уровне приложения. Обратите внимание, что неявный стиль `StackLayout` был добавлен на уровне страницы `ResourceDictionary`, поскольку он используется только в `NotesPage`.

    Сохраните изменения в файле **NotesPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**. При Горячей загрузке XAML будет обновлен пользовательский интерфейс работающего приложения без перестроения приложения. В частности, изменится цвет выбранных элементов в [`CollectionView`](xref:Xamarin.Forms.CollectionView).

6. На **Панели решения** в проекте **Notes** откройте **NoteEntryPage.xaml** в папке **Views**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>       
        </ContentPage.Resources>

        <!-- Layout children vertically -->
        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <!-- Layout children in two columns -->
            <Grid ColumnDefinitions="*,*">
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Этот код добавляет неявные стили для [`Editor`](xref:Xamarin.Forms.Editor) в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и присваивает свойству `StackLayout.Margin` значение, определенное в `ResourceDictionary` на уровне приложения. Обратите внимание, что неявный стиль `Editor` добавлен в `ResourceDictionary` на уровне страницы, так как он используется только в объекте `NoteEntryPage`.

7. В работающем приложении перейдите к объекту `NoteEntryPage`.

8. В Visual Studio для Mac сохраните изменения в **NoteEntryPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

    При Горячей загрузке XAML будет обновлен пользовательский интерфейс приложения без его перестроения. В частности, цвет фона [`Editor`](xref:Xamarin.Forms.Editor) в работающем приложении изменится, как и внешний вид объектов [`Button`](xref:Xamarin.Forms.Button).

9. На **Панели решения** в проекте **Notes** откройте **AboutPage.xaml** в папке **Views**. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{StaticResource AppPrimaryColor}"
                   Opacity="0.85"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->
            <StackLayout Grid.Row="1"
                         Margin="{StaticResource PageMargin}"
                         Spacing="20">
                <Label FontSize="22">
                    <Label.FormattedText>
                        <FormattedString>
                            <FormattedString.Spans>
                                <Span Text="Notes"
                                      FontAttributes="Bold"
                                      FontSize="22" />
                                <Span Text=" v1.0" />
                            </FormattedString.Spans>
                        </FormattedString>
                    </Label.FormattedText>
                </Label>
                <Label Text="This app is written in XAML and C# with the Xamarin Platform." />
                <Button Text="Learn more"
                        Clicked="OnButtonClicked" />
            </StackLayout>
        </Grid>
    </ContentPage>
    ```

    Этот код задает для свойств `Image.BackgroundColor` и `StackLayout.Margin` значения, определенные в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) на уровне приложения.

10. В работающем приложении перейдите к объекту `AboutPage`.

11. В Visual Studio для Mac сохраните изменения в **AboutPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**.

    При Горячей загрузке XAML будет обновлен пользовательский интерфейс приложения без его перестроения. В частности, изменится цвет фона объекта [`Image`](xref:Xamarin.Forms.Editor) в работающем приложении.

::: zone-end

## <a name="next-steps"></a>Дальнейшие действия

В этом кратком руководстве рассматривались следующие темы:

- Стилизация приложения Оболочки в Xamarin.Forms с использованием стилей XAML.
- Использование Горячей перезагрузки XAML для просмотра изменений пользовательского интерфейса без перестроения приложения.

Чтобы узнать больше об основах разработки приложений с помощью Оболочки в Xamarin.Forms, продолжайте подробное знакомство с краткими руководствами.

> [!div class="nextstepaction"]
> [Вперед](deepdive.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Горячая перезагрузка XAML для Xamarin.Forms](~/xamarin-forms/xaml/hot-reload.md)
- [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md)
