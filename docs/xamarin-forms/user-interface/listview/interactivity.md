---
title: Интерактивное взаимодействие с ListView
description: В этой статье объясняется, как добавить интерактивность в Xamarin.Forms ListView путем реализации выбора, контекстных действий и получения обновления.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4a922a841452e5e934cc7dcb88a9f84373ae3ded
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375503"
---
# <a name="listview-interactivity"></a>Интерактивность ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)

Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) Класс поддерживает взаимодействие пользователя с данными, которые он представляет.

## <a name="selection-and-taps"></a>Выделение и касания

[`ListView`](xref:Xamarin.Forms.ListView)Режим выбора контролируется путем присвоения [`ListView.SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) свойству значения [`ListViewSelectionMode`](xref:Xamarin.Forms.ListViewSelectionMode) перечисления.

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) Указывает, что можно выбрать один элемент, при этом выделенный элемент выделяется. Это значение по умолчанию.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) Указывает, что элементы не могут быть выбраны.

Когда пользователь касается элемента, запускаются два события:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) активируется при выборе нового элемента.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) активируется при касании элемента.

Если коснуться одного и того же элемента дважды, будет срабатывать два [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) события, но будет запущено только одно [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) событие.

> [!NOTE]
> [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs)Класс, который содержит аргументы события [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) , имеет [`Group`](xref:Xamarin.Forms.ItemTappedEventArgs.Group) [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) Свойства и, и `ItemIndex` свойство, значение которого представляет индекс в элементе [`ListView`](xref:Xamarin.Forms.ListView) нажатого элемента. Аналогично, [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) класс, который содержит аргументы события [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) , имеет [`SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) свойство и `SelectedItemIndex` свойство, значение которого представляет индекс в `ListView` выбранном элементе.

Если [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) для свойства задано значение [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) , то элементы в [`ListView`](xref:Xamarin.Forms.ListView) могут быть выбраны, [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) события и будут [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) срабатывать, а [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) свойству будет присвоено значение выбранного элемента.

Если [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) свойство имеет значение [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) , элементы в [`ListView`](xref:Xamarin.Forms.ListView) не могут быть выбраны, [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) событие не будет запущено и [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) свойство останется `null` . Однако [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) события по-прежнему будут срабатывать, и при касании элемент будет кратковременно выделен.

При выборе элемента и [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) изменении свойства с [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) на [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) свойство будет установлено на, `null` а [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) событие будет запущено с `null` элементом.

На следующих снимках экрана показан [`ListView`](xref:Xamarin.Forms.ListView) режим выбора по умолчанию:

![ListView с включенным выделением](interactivity-images/selection-default.png)

### <a name="disable-selection"></a>Отключить выделение

Чтобы отключить [`ListView`](xref:Xamarin.Forms.ListView) Выбор, задайте [`SelectionMode`](xref:Xamarin.Forms.ListView.SelectionMode) для свойства значение [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) :

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

## <a name="context-actions"></a>Контекстные действия

Часто пользователи хотят предпринять действия с элементом в `ListView` . Например, рассмотрим список сообщений электронной почты в почтовом приложении. В iOS можно прокрутить, чтобы удалить сообщение:

![ListView с действиями контекста](interactivity-images/context-default.png)

Действия контекста могут быть реализованы в C# и XAML. Ниже вы найдете конкретные руководства для обоих, но сначала рассмотрим некоторые основные сведения о реализации для обоих.

Контекстные действия создаются с помощью `MenuItem` элементов. События TAP для `MenuItems` объектов вызываются `MenuItem` самим собой, а не `ListView` . Это отличается от того, как события касания обрабатываются для ячеек, где объект `ListView` создает событие, а не ячейку. Поскольку `ListView` вызывает событие, его обработчик событий получает ключевые сведения, такие как выбор или касание элемента.

По умолчанию `MenuItem` не имеет способа узнать, к какой ячейке она принадлежит. `CommandParameter`Свойство доступно `MenuItem` для хранения объектов, таких как объект, лежащий в основе `MenuItem` `ViewCell` . `CommandParameter`Свойство может быть задано как в XAML, так и в C#.

### <a name="xaml"></a>XAML

`MenuItem` элементы можно создавать в коллекции XAML. В XAML-коде ниже показана пользовательская ячейка с двумя реализованными контекстными действиями:

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore"
                      CommandParameter="{Binding .}"
                      Text="More" />
            <MenuItem Clicked="OnDelete"
                      CommandParameter="{Binding .}"
                      Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
              <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

В файле кода программной части убедитесь, что `Clicked` методы реализованы:

```csharp
public void OnMore (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e)
{
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> `NavigationPageRenderer`Для Android доступен переопределяемый `UpdateMenuItemIcon` метод, который можно использовать для загрузки значков из пользовательского `Drawable` . Это переопределение позволяет использовать изображения SVG в качестве значков на `MenuItem` экземплярах в Android.

### <a name="code"></a>Код

Контекстные действия могут быть реализованы в любом `Cell` подклассе (если он не используется как заголовок группы) путем создания `MenuItem` экземпляров и их добавления в `ContextActions` коллекцию для ячейки. Для контекстного действия можно настроить следующие свойства:

- **Текстовая надпись** &ndash; Строка, которая отображается в пункте меню.
- **Щелчок** &ndash; событие при щелчке элемента.
- **Разрушающая** &ndash; (необязательно.) Если значение равно true, элемент отрисовывается в iOS по-разному.

В ячейку можно добавить несколько контекстных действий, но только одна из них должна иметь `IsDestructive` значение `true` . В следующем коде показано, как можно добавить контекстные действия в `ViewCell` :

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) =>
{
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

## <a name="pull-to-refresh"></a>Обновление путем оттягивания

Пользователи будут ждать, что список данных обновится. [`ListView`](xref:Xamarin.Forms.ListView)Элемент управления поддерживает это готовые элементы. Чтобы включить функцию "принудительное обновление", задайте [`IsPullToRefreshEnabled`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) для параметра значение `true` :

```xaml
<ListView ...
          IsPullToRefreshEnabled="true" />
```

Эквивалентный код на C# выглядит так:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Счетчик отображается во время обновления, который по умолчанию является черным. Однако цвет счетчика можно изменить в iOS и Android, задав `RefreshControlColor` для свойства значение [`Color`](xref:Xamarin.Forms.Color) .

```xaml
<ListView ...
          IsPullToRefreshEnabled="true"
          RefreshControlColor="Red" />
```

Эквивалентный код на C# выглядит так:

```csharp
listView.RefreshControlColor = Color.Red;
```

На следующих снимках экрана показан запрос на обновление по мере извлечения пользователя:

![Извлечение ListView для обновления In-Progress](interactivity-images/refresh-start.png)

На следующих снимках экрана показан запрос на обновление после того, как пользователь освободил запрос на вытягивание, и при обновлении отображается счетчик [`ListView`](xref:Xamarin.Forms.ListView) :

![Извлечение ListView для обновления завершено](interactivity-images/refresh-in-progress.png)

[`ListView`](xref:Xamarin.Forms.ListView) запускает [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) событие для запуска обновления, и [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) свойство будет установлено в значение `true` . Любой код, необходимый для обновления содержимого, `ListView` должен затем выполняться обработчиком событий для `Refreshing` события или методом, выполняемым [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) . После `ListView` обновления `IsRefreshing` свойство должно иметь значение `false` , или [`EndRefresh`](xref:Xamarin.Forms.ListView.EndRefresh) метод должен быть вызван, чтобы указать, что обновление завершено.

> [!NOTE]
> При определении [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) `CanExecute` метода можно указать метод команды, чтобы включить или отключить команду.

## <a name="detect-scrolling"></a>Обнаружение прокрутки

[`ListView`](xref:Xamarin.Forms.ListView) Определяет `Scrolled` событие, которое срабатывает для указания на то, что произошла прокрутка. В следующем примере XAML показан объект `ListView` , который задает обработчик событий для `Scrolled` события:

```xaml
<ListView Scrolled="OnListViewScrolled">
    ...
</ListView>
```

Эквивалентный код на C# выглядит так:

```csharp
ListView listView = new ListView();
listView.Scrolled += OnListViewScrolled;
```

В этом примере кода `OnListViewScrolled` обработчик событий выполняется при `Scrolled` срабатывании события:

```csharp
void OnListViewScrolled(object sender, ScrolledEventArgs e)
{
    Debug.WriteLine("ScrollX: " + e.ScrollX);
    Debug.WriteLine("ScrollY: " + e.ScrollY);  
}
```

`OnListViewScrolled`Обработчик событий выводит значения `ScrolledEventArgs` объекта, сопровождающего событие.

## <a name="related-links"></a>Связанные ссылки

- [Взаимодействие с ListView (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)