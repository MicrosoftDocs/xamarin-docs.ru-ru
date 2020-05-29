---
title: Xamarin.FormsMenuItem
description: ''
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: de8c6bff2c9dc72821692708f5852cd874c31ede
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139208"
---
# <a name="xamarinforms-menuitem"></a>Xamarin.FormsMenuItem

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)

Xamarin.Forms [`MenuItem`](xref:Xamarin.Forms.MenuItem) Класс определяет пункты меню для меню, такие как `ListView` Контекстные меню элементов и всплывающие меню приложения оболочки.

На следующих снимках экрана показаны `MenuItem` объекты в `ListView` контекстном меню в iOS и Android:

[!["MenuItems в iOS и Android"](menuitem-images/menuitem-demo-cropped.png "MenuItems в iOS и Android")](menuitem-images/menuitem-demo-full.png#lightbox "Все MenuItems в iOS и Android Full Image")

`MenuItem`Класс определяет следующие свойства:

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)— Это `ICommand` , которое позволяет привязать пользовательские действия, такие как касания пальца или щелчки, к командам, определенным в ViewModel.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)Аргумент `object` , указывающий параметр, который должен быть передан в `Command` .
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)`ImageSource`значение, определяющее значок вывода.
* [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)`bool`значение, указывающее, `MenuItem` удаляет ли связанный элемент пользовательского интерфейса из списка.
* [`IsEnabled`](xref:Xamarin.Forms.MenuItem.IsEnabled)`bool`значение, указывающее, реагирует ли этот объект на вводимые пользователем данные.
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)`string`значение, указывающее отображаемый текст.

Эти свойства поддерживаются объектами, [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) поэтому `MenuItem` экземпляр может быть целевым объектом привязок данных.

## <a name="create-a-menuitem"></a>Создание элемента меню

`MenuItem`объекты можно использовать в контекстном меню `ListView` элементов объекта. Наиболее распространенным шаблоном является создание `MenuItem` объектов в `ViewCell` экземпляре, который используется в качестве `DataTemplate` объекта для `ListView` s `ItemTemplate` . Когда `ListView` объект заполняется, он создает каждый элемент с помощью `DataTemplate` , предоставляя `MenuItem` варианты при активации контекстного меню для элемента.

В следующем примере показано `MenuItem` Создание экземпляра в контексте `ListView` объекта:

```xaml
<ListView>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.ContextActions>
                    <MenuItem Text="Context Menu Option" />
                </ViewCell.ContextActions>
                <Label Text="{Binding .}" />
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

`MenuItem`Также можно создать в коде:

```csharp
// A function returns a ViewCell instance that
// is used as the template for each list item
DataTemplate dataTemplate = new DataTemplate(() =>
{
    // A Label displays the list item text
    Label label = new Label();
    label.SetBinding(Label.TextProperty, ".");

    // A ViewCell serves as the DataTemplate
    ViewCell viewCell = new ViewCell
    {
        View = label
    };

    // Add a MenuItem instance to the ContextActions
    MenuItem menuItem = new MenuItem
    {
        Text = "Context Menu Option"
    };
    viewCell.ContextActions.Add(menuItem);

    // The function returns the custom ViewCell
    // to the DataTemplate constructor
    return viewCell;
});

// Finally, the dataTemplate is provided to
// the ListView object
ListView listView = new ListView
{
    ...
    ItemTemplate = dataTemplate
};
```

## <a name="define-menuitem-behavior-with-events"></a>Определение поведения элемента MenuItem с помощью событий

Класс `MenuItem` представляет событие `Clicked`. Обработчик событий может быть присоединен к этому событию, чтобы реагировать на касания или на `MenuItem` экземпляр в XAML:

```xaml
<MenuItem ...
          Clicked="OnItemClicked" />
```

Обработчик событий можно также присоединить в коде:

```csharp
MenuItem item = new MenuItem { ... }
item.Clicked += OnItemClicked;
```

В предыдущих примерах указан `OnItemClicked` обработчик событий. В следующем коде показан пример реализации:

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    // The sender is the menuItem
    MenuItem menuItem = sender as MenuItem;

    // Access the list item through the BindingContext
    var contextItem = menuItem.BindingContext;

    // Do something with the contextItem here
}
```

## <a name="define-menuitem-behavior-with-mvvm"></a>Определение поведения MenuItem с помощью MVVM

`MenuItem`Класс поддерживает шаблон Model-View-ViewModel (MVVM) через [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объекты и `ICommand` интерфейс. В следующем коде XAML показаны `MenuItem` экземпляры, привязанные к командам, определенным в ViewModel:

```xaml
<ContentPage.BindingContext>
    <viewmodels:ListPageViewModel />
</ContentPage.BindingContext>

<StackLayout>
    <Label Text="{Binding Message}" ... />
    <ListView ItemsSource="{Binding Items}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ViewCell.ContextActions>
                        <MenuItem Text="Edit"
                                    IconImageSource="icon.png"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.EditCommand}"
                                    CommandParameter="{Binding .}"/>
                        <MenuItem Text="Delete"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.DeleteCommand}"
                                    CommandParameter="{Binding .}"/>
                    </ViewCell.ContextActions>
                    <Label Text="{Binding .}" />
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

В предыдущем примере два `MenuItem` объекта определяются с их `Command` свойствами и, `CommandParameter` привязанными к командам в ViewModel. ViewModel содержит команды, упоминаемые в XAML:

```csharp
public class ListPageViewModel : INotifyPropertyChanged
{
    ...

    public ICommand EditCommand => new Command<string>((string item) =>
    {
        Message = $"Edit command was called on: {item}";
    });

    public ICommand DeleteCommand => new Command<string>((string item) =>
    {
        Message = $"Delete command was called on: {item}";
    });
}
```

Пример приложения включает класс, `DataService` используемый для получения списка элементов для заполнения `ListView` объектов. Создается экземпляр ViewModel, элементы `DataService` класса и задаются как `BindingContext` в коде программной части:

```csharp
public MenuItemXamlMvvmPage()
{
    InitializeComponent();
    BindingContext = new ListPageViewModel(DataService.GetListItems());
}
```

## <a name="menuitem-icons"></a>Значки MenuItem

> [!WARNING]
> `MenuItem`на устройствах Android отображаются только значки. На других платформах будет отображаться только текст, заданный `Text` свойством.

 Значки задаются с помощью `IconImageSource` Свойства. Если указан значок, то текст, заданный `Text` свойством, не будет отображаться. На следующем снимке экрана показан `MenuItem` значок со значком на Android:

![Снимок экрана значка MenuItem в Android](menuitem-images/menuitem-android-icon.png "Снимок экрана значка MenuItem в Android")

Дополнительные сведения об использовании образов в см Xamarin.Forms . в разделе [изображения Xamarin.Forms в ](~/xamarin-forms/user-interface/images.md).

## <a name="enable-or-disable-a-menuitem-at-runtime"></a>Включение или отключение элемента MenuItem во время выполнения

Чтобы включить отключение `MenuItem` во время выполнения, привяжите его `Command` свойство к `ICommand` реализации и убедитесь, что `canExecute` делегат включает и отключается `ICommand` соответствующим образом.

> [!IMPORTANT]
> Не привязывать `IsEnabled` свойство к другому свойству при использовании `Command` свойства для включения или отключения `MenuItem` .

В следующем примере показано, `MenuItem` `Command` свойство которого привязано к `ICommand` именованной `MyCommand` :

```xaml
<MenuItem Text="My menu item"
          Command="{Binding MyCommand}" />
```

`ICommand`Для реализации требуется `canExecute` делегат, возвращающий значение свойства, `bool` чтобы включить и отключить `MenuItem` :

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    bool isMenuItemEnabled = false;
    public bool IsMenuItemEnabled
    {
        get { return isMenuItemEnabled; }
        set
        {
            isMenuItemEnabled = value;
            MyCommand.ChangeCanExecute();
        }
    }

    public Command MyCommand { get; private set; }

    public ToolbarItemViewModel()
    {
        MyCommand = new Command(() =>
        {
            // Execute logic here
        },
        () => IsToolbarItemEnabled);
    }
}
```

В этом примере элемент `MenuItem` отключен до тех пор, пока `IsMenuItemEnabled` не будет задано свойство. В этом случае `Command.ChangeCanExecute` вызывается метод, который вызывает `canExecute` `MyCommand` повторное вычисление делегата.

## <a name="cross-platform-context-menu-behavior"></a>Поведение контекстного меню на разных платформах

К контекстным меню обращаются и отображаются по-разному на каждой платформе.

В Android контекстное меню активируется длительным нажатием элемента списка. Контекстное меню заменяет заголовок и область панели навигации, а `MenuItem` Параметры отображаются горизонтальными кнопками.

!["Снимок экрана контекстного меню в Android"](menuitem-images/menuitem-android-icon.png "Снимок экрана: контекстное меню в Android")

В iOS контекстное меню активируется функцией прокрутки на элементе списка. Контекстное меню отображается в элементе списка и `MenuItems` отображается в виде горизонтальных кнопок.

!["Снимок экрана контекстного меню в iOS"](menuitem-images/menuitem-ios-contextmenu.png "Снимок экрана: контекстное меню в iOS")

В UWP контекстное меню активируется щелчком правой кнопкой мыши по элементу списка. Контекстное меню отображается рядом с курсором в виде вертикального списка.

!["Снимок экрана контекстного меню в UWP"](menuitem-images/menuitem-uwp.png "Снимок экрана: контекстное меню в UWP")

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации MenuItem](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitemdemos/)
* [Изображения вXamarin.Forms](~/xamarin-forms/user-interface/images.md)
