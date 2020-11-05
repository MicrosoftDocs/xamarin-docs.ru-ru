---
title: Xamarin.Forms таблевиев
description: В этой статье объясняется, как использовать Xamarin.Forms класс таблевиев для представления прокручиваемых меню, настроек и форм ввода в приложениях.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: afb2c7c5c82a7ce530846c266d231b893bbbf39d
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93371239"
---
# <a name="no-locxamarinforms-tableview"></a>Xamarin.Forms таблевиев

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-tableview)

[`TableView`](xref:Xamarin.Forms.TableView) — Это представление для отображения прокручиваемых списков данных или вариантов выбора, где имеются строки, которые не используют один и тот же шаблон. В отличие от [ListView](~/xamarin-forms/user-interface/listview/index.md), не `TableView` имеет концепции `ItemsSource` , поэтому элементы должны добавляться вручную в качестве дочерних элементов.

![Пример Таблевиев](tableview-images/tableview-all-sml.png)

## <a name="use-cases"></a>Варианты использования

[`TableView`](xref:Xamarin.Forms.TableView) полезно в следующих случаях:

- представление списка параметров
- сбор данных в форме или
- Отображение данных, которые представлены по-разному из строки в строку (например, числа, проценты и изображения).

[`TableView`](xref:Xamarin.Forms.TableView) обрабатывает прокрутку и размещение строк в привлекательных разделах, что часто требуется для приведенных выше сценариев. `TableView`Элемент управления использует собственное эквивалентное представление каждой платформы, если оно доступно, создавая собственный внешний вид для каждой платформы.

## <a name="structure"></a>Структура

Элементы в разделе [`TableView`](xref:Xamarin.Forms.TableView) упорядочены по разделам. В корне объекта `TableView` — [`TableRoot`](xref:Xamarin.Forms.TableRoot) , который является родительским для одного или нескольких [`TableSection`](xref:Xamarin.Forms.TableSection) экземпляров. Каждый [`TableSection`](xref:Xamarin.Forms.TableSection) из них состоит из заголовка и одного или нескольких [`ViewCell`](xref:Xamarin.Forms.ViewCell) экземпляров:

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
            <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

Эквивалентный код на C# выглядит так:

```csharp
Content = new TableView
{
    Root = new TableRoot
    {
        new TableSection("Ring")
        {
          // TableSection constructor takes title as an optional parameter
          new SwitchCell { Text = "New Voice Mail" },
          new SwitchCell { Text = "New Mail", On = true }
        }
    },
    Intent = TableIntent.Settings
};
```

## <a name="appearance"></a>Внешний вид

[`TableView`](xref:Xamarin.Forms.TableView) предоставляет [`Intent`](xref:Xamarin.Forms.TableView.Intent) свойство, которое можно задать любому из [`TableIntent`](xref:Xamarin.Forms.TableIntent) членов перечисления:

- `Data` — для использования при отображении записей данных. Обратите внимание, что [ListView](~/xamarin-forms/user-interface/listview/index.md) может быть лучшим вариантом для прокрутки списков данных.
- `Form` — используется, если Таблевиев выступает в качестве формы.
- `Menu` — для использования при показе меню выбора.
- `Settings` — для использования при отображении списка параметров конфигурации.

[`TableIntent`](xref:Xamarin.Forms.TableIntent)Выбранное значение может повлиять на то, как [`TableView`](xref:Xamarin.Forms.TableView) отображается на каждой платформе. Даже если различий нет, рекомендуется выбрать тот `TableIntent` , который наиболее полно соответствует тому, как предполагается использовать таблицу.

Кроме того, цвет текста, отображаемого для каждого из них, [`TableSection`](xref:Xamarin.Forms.TableSection) можно изменить, задав `TextColor` для свойства значение [`Color`](xref:Xamarin.Forms.Color) .

## <a name="built-in-cells"></a>Встроенные ячейки

Xamarin.Forms поставляется со встроенными ячейками для сбора и отображения информации. Хотя [`ListView`](xref:Xamarin.Forms.ListView) и [`TableView`](xref:Xamarin.Forms.TableView) могут использовать все одни и те же ячейки [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) и [`EntryCell`](xref:Xamarin.Forms.EntryCell) наиболее актуальны для `TableView` сценария.

Подробное описание [текстцелл](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) и [имажецелл](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell)см. в разделе [представление ячеек ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) .

### <a name="switchcell"></a>свитчцелл

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)элемент управления, используемый для представления и записи состояния On/Off или `true` / `false` . Он определяет следующие свойства:

- `Text` — текст, отображаемый рядом с параметром.
- `On` — Указывает, отображается ли переключатель как on или OFF.
- `OnColor` — параметр параметра, [`Color`](xref:Xamarin.Forms.Color) если он находится в положении ON.

Все эти свойства могут быть связаны.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) также предоставляет `OnChanged` событие, позволяющее реагировать на изменения в состоянии ячейки.

![Пример Свитчцелл](tableview-images/switch-cell.png)

### <a name="entrycell"></a>ентрицелл

[`EntryCell`](xref:Xamarin.Forms.EntryCell) полезен, если требуется отображать текстовые данные, которые пользователь может изменять. Он определяет следующие свойства:

- `Keyboard` — Клавиатура, отображаемая во время редактирования. Существуют такие параметры, как числовые значения, электронная почта, Номера телефонов и т. д. [см. документацию по API](xref:Xamarin.Forms.Keyboard).
- `Label` — Текст метки, отображаемый слева от поля ввода текста.
- `LabelColor` — Цвет текста метки.
- `Placeholder` — Текст, отображаемый в поле ввода, если он имеет значение null или пуст. Этот текст исчезает при начале ввода текста.
- `Text` — Текст в поле ввода.
- `HorizontalTextAlignment` — Горизонтальное выравнивание текста. Значения выравниваются по центру, по левому или по правому краю. [См. документацию по API](xref:Xamarin.Forms.TextAlignment).
- `VerticalTextAlignment` — Вертикальное выравнивание текста. Значения: `Start` , `Center` или `End` .

[`EntryCell`](xref:Xamarin.Forms.EntryCell) также предоставляет `Completed` событие, которое срабатывает, когда пользователь нажимает кнопку "Готово" на клавиатуре при редактировании текста.

![Пример Ентрицелл](tableview-images/entry-cell.png)

## <a name="custom-cells"></a>Пользовательские ячейки

Если встроенные ячейки недостаточно, можно использовать пользовательские ячейки для представления и записи данных способом, который имеет смысл для вашего приложения. Например, может потребоваться Показать ползунок, чтобы позволить пользователю выбрать непрозрачность изображения.

Все пользовательские ячейки должны быть производными от того [`ViewCell`](xref:Xamarin.Forms.ViewCell) же базового класса, который используется всеми встроенными типами ячеек.

Пример пользовательской ячейки:

![Пример пользовательской ячейки](tableview-images/custom-cell.png)

В следующем примере показан XAML, используемый для создания на [`TableView`](xref:Xamarin.Forms.TableView) снимках экрана выше:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoTableView.TablePage"
             Title="TableView">
      <TableView Intent="Settings">
          <TableRoot>
              <TableSection Title="Getting Started">
                  <ViewCell>
                      <StackLayout Orientation="Horizontal">
                          <Image Source="bulb.png" />
                          <Label Text="left"
                                 TextColor="#f35e20" />
                          <Label Text="right"
                                 HorizontalOptions="EndAndExpand"
                                 TextColor="#503026" />
                      </StackLayout>
                  </ViewCell>
              </TableSection>
          </TableRoot>
      </TableView>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() { Source = "bulb.png"});
layout.Children.Add (new Label()
{
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label ()
{
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot ()
{
    new TableSection("Getting Started")
    {
        new ViewCell() {View = layout}
    }
};
Content = table;
```

Корневым элементом в [`TableView`](xref:Xamarin.Forms.TableView) является [`TableRoot`](xref:Xamarin.Forms.TableRoot) , а [`TableSection`](xref:Xamarin.Forms.TableSection) сразу под ним находится `TableRoot` . Определяется [`ViewCell`](xref:Xamarin.Forms.ViewCell) непосредственно в `TableSection` , а [`StackLayout`](xref:Xamarin.Forms.StackLayout) используется для управления макетом пользовательской ячейки, хотя здесь можно использовать любой макет.

> [!NOTE]
> В отличие от [`ListView`](xref:Xamarin.Forms.ListView) , не [`TableView`](xref:Xamarin.Forms.TableView) требует, чтобы пользовательские (или любые) ячейки были определены в `ItemTemplate` .

## <a name="row-height"></a>Высота строки

[`TableView`](xref:Xamarin.Forms.TableView)Класс имеет два свойства, которые можно использовать для изменения высоты строки ячеек:

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) — Задает высоту каждой строки в `int` .
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) — строки имеют различные значения высоты, если задано значение `true` . Обратите внимание, что при присвоении этому свойству значения `true` высоты строк будут автоматически вычисляться и применяться Xamarin.Forms .

Когда изменяется высота содержимого ячейки в [`TableView`](xref:Xamarin.Forms.TableView) , ее высота неявным образом обновляется в Android и универсальная платформа Windows (UWP). Однако в iOS необходимо принудительно обновить, задав [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) для свойства значение `true` и, вызвав [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) метод.

В следующем примере XAML показан объект [`TableView`](xref:Xamarin.Forms.TableView) , содержащий [`ViewCell`](xref:Xamarin.Forms.ViewCell) :

```xaml
<ContentPage ...>
    <TableView ...
               HasUnevenRows="true">
        <TableRoot>
            ...
            <TableSection ...>
                ...
                <ViewCell x:Name="_viewCell"
                          Tapped="OnViewCellTapped">
                    <Grid Margin="15,0">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Label Text="Tap this cell." />
                        <Label x:Name="_target"
                               Grid.Row="1"
                               Text="The cell has changed size."
                               IsVisible="false" />
                    </Grid>
                </ViewCell>
            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

При [`ViewCell`](xref:Xamarin.Forms.ViewCell) нажатии кнопки `OnViewCellTapped` выполняется обработчик событий:

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

`OnViewCellTapped`Обработчик событий показывает или скрывает вторую секунду [`Label`](xref:Xamarin.Forms.Label) в [`ViewCell`](xref:Xamarin.Forms.ViewCell) и явно обновляет размер ячейки путем вызова [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) метода.

На следующих снимках экрана показана ячейка перед тем, как коснуться:

![ViewCell до изменения размера](tableview-images/cell-beforeresize.png)

На следующих снимках экрана показана ячейка после нажатия на:

![ViewCell после изменения размера](tableview-images/cell-afterresize.png)

> [!IMPORTANT]
> Если эта функция используется слишком сильно, снижение производительности может снизить производительность.

## <a name="related-links"></a>Связанные ссылки

- [Таблевиев (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-tableview)