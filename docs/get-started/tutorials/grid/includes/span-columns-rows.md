---
ms.openlocfilehash: 89b56e3733d1c75ed616f79508d555ccb0c51f00
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634911"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Grid`](xref:Xamarin.Forms.Grid) для определения столбцов и строк и разместите содержимое в столбцах и строках:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    Этот код определяет столбцы и строки для [`Grid`](xref:Xamarin.Forms.Grid) и располагает экземпляры [`Label`](xref:Xamarin.Forms.Label) в конкретных столбцах и строках. Первый `Label` задает вложенное свойство [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) так, чтобы его текст охватывал несколько столбцов. Свойство `ColumnSpan` имеет значение 2; оно задает количество столбцов, которые `Label` будет охватывать. Второй `Label` задает вложенное свойство [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) так, чтобы его текст охватывал несколько строк. Свойство `RowSpan` имеет значение 2; оно задает количество строк, которые `Label` будет охватывать.

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном удаленном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана: сетка с содержимым, занимающим несколько столбцов и строк, в iOS и Android](../images/span-columns-rows.png "Сетка с содержимым, занимающим несколько столбцов и строк")](../images/span-columns-rows-large.png#lightbox "Сетка с содержимым, занимающим несколько столбцов и строк")

    В Visual Studio остановите приложение.

    Дополнительные сведения об охвате нескольких столбцов и строк см. в разделе [Строки и столбцы](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns) в руководстве [Сетка Xamarin.Forms](~/xamarin-forms/user-interface/layouts/grid.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Grid`](xref:Xamarin.Forms.Grid) для определения столбцов и строк и разместите содержимое в столбцах и строках:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    Этот код определяет столбцы и строки для [`Grid`](xref:Xamarin.Forms.Grid) и располагает экземпляры [`Label`](xref:Xamarin.Forms.Label) в конкретных столбцах и строках. Первый `Label` задает вложенное свойство [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) так, чтобы его текст охватывал несколько столбцов. Свойство `ColumnSpan` имеет значение 2; оно задает количество столбцов, которые `Label` будет охватывать. Второй `Label` задает вложенное свойство [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) так, чтобы его текст охватывал несколько строк. Свойство `RowSpan` имеет значение 2; оно задает количество строк, которые `Label` будет охватывать.

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio для Mac кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана: сетка с содержимым, занимающим несколько столбцов и строк, в iOS и Android](../images/span-columns-rows.png "Сетка с содержимым, занимающим несколько столбцов и строк")](../images/span-columns-rows-large.png#lightbox "Сетка с содержимым, занимающим несколько столбцов и строк")

    В Visual Studio для Mac остановите приложение.

    Дополнительные сведения об охвате нескольких столбцов и строк см. в разделе [Строки и столбцы](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns) в руководстве [Сетка Xamarin.Forms](~/xamarin-forms/user-interface/layouts/grid.md).
