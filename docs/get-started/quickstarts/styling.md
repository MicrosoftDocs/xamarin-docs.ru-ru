---
title: Стилизация кроссплатформенного приложения Xamarin.Forms
description: В этой статье описывается изменение стиля кроссплатформенного приложения Xamarin.Forms с помощью стилей XAML.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/07/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9794f429744611654ce883b48aa828b997ec881f
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435441"
---
# <a name="style-a-cross-platform-no-locxamarinforms-application"></a>Стилизация кроссплатформенного приложения Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

В этом кратком руководстве рассматриваются следующие темы:

- Стилизация приложения Xamarin.Forms с использованием стилей XAML.

Из этого краткого руководства вы узнаете, как изменить стиль кроссплатформенного приложения Xamarin.Forms с помощью стилей XAML. Ниже показано итоговое приложение:

[![Страница заметок](styling-images/screenshots1-sml.png)](styling-images/screenshots1.png#lightbox "Страница заметок")
[![Страница ввода заметки](styling-images/screenshots2-sml.png)](styling-images/screenshots2.png#lightbox "Страница ввода заметки")

### <a name="prerequisites"></a>Предварительные требования

Прежде чем приступать к этому краткому руководству, необходимо успешно завершить [предыдущее](database.md). Также вы можете скачать [пример из предыдущего краткого руководства](/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/) и использовать его в качестве отправной точки для работы с этим руководством.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Обновление приложения с помощью Visual Studio

1. Запустите Visual Studio и откройте решение Notes.

2. В **обозревателе решений** дважды щелкните файл **App.xaml** в проекте **Notes**, чтобы открыть его. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="NavigationBarColor">#1976D2</Color>
            <Color x:Key="NavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{StaticResource NavigationBarColor}" />
                 <Setter Property="BarTextColor"
                        Value="{StaticResource NavigationBarTextColor}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    Этот код определяет значение [`Thickness`](xref:Xamarin.Forms.Thickness), ряд значений [`Color`](xref:Xamarin.Forms.Color), а также неявные стили для [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и [`ContentPage`](xref:Xamarin.Forms.ContentPage). Обратите внимание, что эти стили находятся на уровне приложения [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и могут использоваться по всему приложению. Дополнительные сведения об использовании стилей XAML см. в разделе [Задание стиля](deepdive.md#styling) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **App.xaml**, нажав клавиши **CTRL+S**, и закройте файл.

3. В **обозревателе решений** дважды щелкните файл **NotesPage.xaml** в проекте **Notes**, чтобы открыть его. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              TextColor="Black"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    Этот код добавляет неявный стиль для [`ListView`](xref:Xamarin.Forms.ListView) на уровне страницы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и присваивает свойству `ListView.Margin` значение, определенное на уровне приложения `ResourceDictionary`. Обратите внимание, что неявный стиль `ListView` был добавлен на уровне страницы `ResourceDictionary`, поскольку он используется только в `NotesPage`. Дополнительные сведения об использовании стилей XAML см. в разделе [Задание стиля](deepdive.md#styling) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NotesPage.xaml**, нажав клавиши **CTRL+S**, и закройте файл.

4. В **обозревателе решений** дважды щелкните файл **NoteEntryPage.xaml** в проекте **Notes**, чтобы открыть его. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    Этот код добавляет неявные стили для представлений [`Editor`](xref:Xamarin.Forms.Editor) и [`Button`](xref:Xamarin.Forms.Button) на уровне страницы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), а также присваивает свойству `StackLayout.Margin` значение, определенное на уровне приложения `ResourceDictionary`. Обратите внимание, что неявные стили `Editor` и `Button` были добавлены на уровне страницы `ResourceDictionary`, поскольку они используются только в `NoteEntryPage`. Дополнительные сведения об использовании стилей XAML см. в разделе [Задание стиля](deepdive.md#styling) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **NoteEntryPage.xaml**, нажав клавиши **CTRL+S**, и закройте файл.

5. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](single-page.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **+** , чтобы перейти к странице **NoteEntryPage** и ввести заметку. На каждой странице просмотрите изменения стиля по сравнению с предыдущим кратким руководством.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Обновление приложения с помощью Visual Studio для Mac

1. Запустите Visual Studio для Mac и откройте проект Notes.

2. В **панели решения** дважды щелкните файл **App.xaml** в проекте **Notes**, чтобы открыть его. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="NavigationBarColor">#1976D2</Color>
            <Color x:Key="NavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{StaticResource NavigationBarColor}" />
                 <Setter Property="BarTextColor"
                        Value="{StaticResource NavigationBarTextColor}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    Этот код определяет значение [`Thickness`](xref:Xamarin.Forms.Thickness), ряд значений [`Color`](xref:Xamarin.Forms.Color), а также неявные стили для [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и [`ContentPage`](xref:Xamarin.Forms.ContentPage). Обратите внимание, что эти стили находятся на уровне приложения [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и могут использоваться по всему приложению. Дополнительные сведения об использовании стилей XAML см. в разделе [Задание стиля](deepdive.md#styling) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в файле **App.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

3. В **панели решения** дважды щелкните файл **NotesPage.xaml** в проекте **Notes**, чтобы открыть его. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              TextColor="Black"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    Этот код добавляет неявный стиль для [`ListView`](xref:Xamarin.Forms.ListView) на уровне страницы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) и присваивает свойству `ListView.Margin` значение, определенное на уровне приложения `ResourceDictionary`. Обратите внимание, что неявный стиль `ListView` был добавлен на уровне страницы `ResourceDictionary`, поскольку он используется только в `NotesPage`. Дополнительные сведения об использовании стилей XAML см. в разделе [Задание стиля](deepdive.md#styling) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в **NotesPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

4. В **панели решения** дважды щелкните файл **NoteEntryPage.xaml** в проекте **Notes**, чтобы открыть его. Затем замените существующий код следующим:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    Этот код добавляет неявные стили для представлений [`Editor`](xref:Xamarin.Forms.Editor) и [`Button`](xref:Xamarin.Forms.Button) на уровне страницы [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), а также присваивает свойству `StackLayout.Margin` значение, определенное на уровне приложения `ResourceDictionary`. Обратите внимание, что неявные стили `Editor` и `Button` были добавлены на уровне страницы `ResourceDictionary`, поскольку они используются только в `NoteEntryPage`. Дополнительные сведения об использовании стилей XAML см. в разделе [Задание стиля](deepdive.md#styling) в статье [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md).

    Сохраните изменения в **NoteEntryPage.xaml**, выбрав **Файл > Сохранить** или нажав клавиши **&#8984;+S**, и закройте файл.

5. Создайте и запустите проект на каждой соответствующей платформе. Дополнительные сведения см. в разделе [Сборка примера из краткого руководства](single-page.md#building-the-quickstart).

    На странице **NotesPage** нажмите кнопку **+** , чтобы перейти к странице **NoteEntryPage** и ввести заметку. На каждой странице просмотрите изменения стиля по сравнению с предыдущим кратким руководством.

::: zone-end

## <a name="next-steps"></a>Следующие шаги

В этом кратком руководстве рассматривались следующие темы:

- Стилизация приложения Xamarin.Forms с использованием стилей XAML.

Чтобы узнать больше об основах разработки приложений с помощью Xamarin.Forms, продолжайте подробное знакомство с краткими руководствами.

> [!div class="nextstepaction"]
> [Вперед](deepdive.md)

## <a name="related-links"></a>Связанные ссылки

- [Заметки (пример)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Подробное изучение кратких руководств по Xamarin.Forms](deepdive.md)