---
title: Главная страница и страница сведений в Xamarin.Forms
description: Xamarin.Forms MasterDetailPage представляет собой страницу, управляющую двумя страницами связанных данных — главной страницей, которая представляет элементы, и страницей сведений, которая представляет сведения об элементах на главной странице. В этой статье описываются принципы работы со страницей MasterDetailPage и перехода между управляемыми страницами данных.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 89b1dedcb6219a32332641550018405abd4b6870
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137817"
---
# <a name="xamarinforms-master-detail-page"></a>Главная страница и страница сведений в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage)

_Xamarin.Forms MasterDetailPage представляет собой страницу, управляющую двумя страницами связанных данных — главной страницей, которая представляет элементы, и страницей сведений, которая представляет сведения об элементах на главной странице. В этой статье описываются принципы работы со страницей MasterDetailPage и перехода между управляемыми страницами данных._

## <a name="overview"></a>Обзор

На главной странице обычно отображается список элементов, как показано на следующем снимке экрана:

[![](master-detail-page-images/masterpage-components.png "Master Page Components")](master-detail-page-images/masterpage-components-large.png#lightbox "Master Page Components")

Расположение списка элементов одинаково для каждой платформы, а при выборе отдельного элемента осуществляется переход на соответствующую страницу сведений. Кроме того, на главной странице также представлена панель навигации с кнопкой, с помощью которой можно перейти к активной странице сведений:

- В iOS панель навигации располагается в верхней части страницы и содержит кнопку для перехода на страницу сведений. Кроме того, для перехода на активную страницу сведений можно провести пальцем по главной странице влево.
- В Android панель навигации располагается в верхней части страницы и содержит заголовок, значок, а также кнопку для перехода на страницу сведений. Значок определяется в атрибуте `[Activity]`, который оформляет класс `MainActivity` в проекте, зависящем от платформы Android. Кроме того, для перехода на активную страницу сведений можно провести пальцем по главной странице влево, коснуться страницы сведений в правой части экрана, а также коснуться кнопки *Назад* внизу экрана.
- На универсальной платформе Windows (UWP) панель навигации располагается в верхней части страницы и содержит кнопку для перехода на страницу сведений.

Страница сведений с данными, соответствующими выбранному на главной странице элементу, а также основные компоненты страницы сведений показаны на следующих снимках экрана:

![](master-detail-page-images/detailpage-components.png "Detail Page Components")

На странице сведений располагается панель навигации, содержимое которой зависит от платформы:

- В iOS панель навигации находится в верхней части экрана и содержит заголовок, а также кнопку для возврата на главную страницу при условии, что экземпляр страницы сведений упакован в экземпляр [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Кроме того, для возврата на главную страницу можно провести пальцем по странице сведений вправо.
- В Android панель навигации находится в верхней части страницы, где отображаются заголовок, значок и кнопка для возврата на главную страницу. Значок определяется в атрибуте `[Activity]`, который оформляет класс `MainActivity` в проекте, зависящем от платформы Android.
- На универсальной платформе Windows (UWP) панель навигации находится в верхней части страницы, где отображаются заголовок и кнопка для возврата на главную страницу.

### <a name="navigation-behavior"></a>Поведение при навигации

Реакция на события перехода между главной страницей и страницами сведений зависит от платформы:

- В iOS страница сведений *сдвигается* вправо, а главная страница выдвигается слева. При этом левая часть страницы сведений по-прежнему остается видимой.
- В Android главная страница и страницы сведений *накладываются* друг на друга.
- В UWP эталонная страница сдвигается из левой части страницы сведений с учетом того, что свойство [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) имеет значение `Popover`. Дополнительные сведения см. в разделе [Управление отображением страницы сведений](#Controlling_the_Detail_Page_Display_Behavior).

Аналогичное поведение реализуется в альбомном режиме. Исключение составляют платформы iOS и Android, на которых главная страница сохраняет ту же ширину, что и в книжной ориентации, благодаря чему отображается больше содержимого страницы сведений.

Дополнительные сведения об управлении поведением при навигации см. в статье [Управление поведением при отображении страницы сведений](#Controlling_the_Detail_Page_Display_Behavior).

## <a name="creating-a-masterdetailpage"></a>Создание страницы MasterDetailPage

Объект [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) содержит свойства [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) и [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) типа [`Page`](xref:Xamarin.Forms.Page), которые используются для получения и задания соответственно главной страницы и страниц сведений.

> [!IMPORTANT]
> Объект [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) должен представлять корневую страницу. Если использовать его в качестве дочерней или какой-либо другой страницы, это может привести к непредвиденному или несогласованному поведению. Кроме того, рекомендуется всегда использовать в качестве главной страницы объекта [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) экземпляр [`ContentPage`](xref:Xamarin.Forms.ContentPage), а для заполнения страницы сведений следует использовать только экземпляры [`TabbedPage`](xref:Xamarin.Forms.TabbedPage), [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и `ContentPage`. Это позволит обеспечить согласованность пользовательского интерфейса на всех платформах.

В следующем примере показан код XAML для объекта [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage), который задает свойства [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) и [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail):

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  xmlns:local="clr-namespace:MasterDetailPageNavigation;assembly=MasterDetailPageNavigation"
                  x:Class="MasterDetailPageNavigation.MainPage">
    <MasterDetailPage.Master>
        <local:MasterPage x:Name="masterPage" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

В следующем примере кода создается эквивалентный объект [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) на языке C#:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        masterPage = new MasterPageCS ();
        Master = masterPage;
        Detail = new NavigationPage (new ContactsPageCS ());
        ...
    }
    ...
}
```

Свойство [`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master) содержит экземпляр [`ContentPage`](xref:Xamarin.Forms.ContentPage). Свойство [`MasterDetailPage.Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) содержит объект [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), содержащий экземпляр `ContentPage`.

### <a name="creating-the-master-page"></a>Создание главной страницы

В следующем примере кода XAML показано объявление объекта `MasterPage`, на который задаются ссылки посредством свойства [`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:MasterDetailPageNavigation"
             x:Class="MasterDetailPageNavigation.MasterPage"
             Padding="0,40,0,0"
             IconImageSource="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
           <ListView.ItemsSource>
                <x:Array Type="{x:Type local:MasterPageItem}">
                    <local:MasterPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:MasterPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:MasterPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid Padding="5,10">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="30"/>
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding IconSource}" />
                            <Label Grid.Column="1" Text="{Binding Title}" />
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Эта страница содержит объект [`ListView`](xref:Xamarin.Forms.ListView), который заполняется данными в XAML. Для этого его свойству [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) присваивается массив экземпляров `MasterPageItem`. Каждый объект `MasterPageItem` определяет свойства `Title`, `IconSource` и `TargetType`.

Объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) присваивается свойству [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) для отображения каждого объекта `MasterPageItem`. Объект `DataTemplate` содержит объект [`ViewCell`](xref:Xamarin.Forms.ViewCell), который состоит из объектов [`Image`](xref:Xamarin.Forms.Image) и [`Label`](xref:Xamarin.Forms.Label). Объект [`Image`](xref:Xamarin.Forms.Image) отображает значение свойства `IconSource`, а объект [`Label`](xref:Xamarin.Forms.Label) отображает значение свойства `Title` для каждого объекта `MasterPageItem`.

Для этой страницы заданы свойства [`Title`](xref:Xamarin.Forms.Page.Title) и [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource). Если у страницы сведений есть заголовок, на ней появится значок. Чтобы реализовать это в iOS, необходимо упаковать экземпляр страницы сведений в экземпляр [`NavigationPage`](xref:Xamarin.Forms.NavigationPage).

> [!NOTE]
> Для страницы [`MasterDetailPage.Master`](xref:Xamarin.Forms.MasterDetailPage.Master) должно быть задано свойство [`Title`](xref:Xamarin.Forms.Page.Title), иначе возникнет исключение.

В следующем примере кода создается эквивалентная страница на языке C#:

```csharp
public class MasterPageCS : ContentPage
{
  public ListView ListView { get { return listView; } }

  ListView listView;

  public MasterPageCS ()
  {
    var masterPageItems = new List<MasterPageItem> ();
    masterPageItems.Add (new MasterPageItem {
      Title = "Contacts",
      IconSource = "contacts.png",
      TargetType = typeof(ContactsPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "TodoList",
      IconSource = "todo.png",
      TargetType = typeof(TodoListPageCS)
    });
    masterPageItems.Add (new MasterPageItem {
      Title = "Reminders",
      IconSource = "reminders.png",
      TargetType = typeof(ReminderPageCS)
    });

    listView = new ListView {
      ItemsSource = masterPageItems,
      ItemTemplate = new DataTemplate (() => {
        var grid = new Grid { Padding = new Thickness(5, 10) };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(30) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Star });

        var image = new Image();
        image.SetBinding(Image.SourceProperty, "IconSource");
        var label = new Label { VerticalOptions = LayoutOptions.FillAndExpand };
        label.SetBinding(Label.TextProperty, "Title");

        grid.Children.Add(image);
        grid.Children.Add(label, 1, 0);

        return new ViewCell { View = grid };
      }),
      SeparatorVisibility = SeparatorVisibility.None
    };

    IconImageSource = "hamburger.png";
    Title = "Personal Organiser";
    Content = new StackLayout
    {
      Children = { listView }
    };
  }
}
```

На следующих снимках экрана показаны главные страницы на каждой платформе:

![](master-detail-page-images/masterpage.png "Master Page Example")

### <a name="creating-and-displaying-the-detail-page"></a>Создание и отображение страницы сведений

Экземпляр `MasterPage` содержит свойство `ListView`, которое предоставляет экземпляр [`ListView`](xref:Xamarin.Forms.ListView), благодаря чему экземпляр `MainPage` [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) может зарегистрировать обработчик для события [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected). Это позволяет экземпляру `MainPage` присвоить свойству [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) страницу, которая представляет выбранный элемент `ListView`. В следующем примере кода показан обработчик событий:

```csharp
public partial class MainPage : MasterDetailPage
{
    public MainPage ()
    {
        ...
        masterPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as MasterPageItem;
        if (item != null) {
            Detail = new NavigationPage ((Page)Activator.CreateInstance (item.TargetType));
            masterPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

Метод `OnItemSelected` выполняет следующие действия:

- Извлекает объект [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) из экземпляра [`ListView`](xref:Xamarin.Forms.ListView) и, если он не имеет значение `null`, устанавливает в качестве страницы сведений новый экземпляр типа страницы, хранящегося в свойстве `TargetType` объекта `MasterPageItem`. Тип страницы упаковывается в экземпляр [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), чтобы обеспечить возможность ссылаться на значок посредством свойства [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) на объекте `MasterPage`, который отображается на главной странице в iOS.
- Элемент, выбранный в объекте [`ListView`](xref:Xamarin.Forms.ListView), получает значение `null`. Это позволяет гарантировать, что в следующий раз при отображении объекта `MasterPage` не будет выбран ни один из элементов объекта `ListView`.
- Для отображения страницы сведений пользователю свойства [`MasterDetailPage.IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) присваивается значение `false`. Это свойство определяет, отображается ли главная страница или страница сведений. Если оно имеет значение `true`, отображается главная страница. Значение `false` позволяет отобразить страницу сведений.

На следующих снимках экрана показана страница сведений `ContactPage`, которая выводится на экран после выбора соответствующего элемента на главной странице:

![](master-detail-page-images/detailpage.png "Detail Page Example")

<a name="Controlling_the_Detail_Page_Display_Behavior" />

### <a name="controlling-the-detail-page-display-behavior"></a>Управление поведением при отображении страницы сведений

Принципы управления главными страницами и страницами сведений для объекта [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) зависят от того, выполняется ли приложение на телефоне или планшете, от ориентации устройства, а также от значения свойства [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior). Это свойство определяет, каким образом будет отображаться страница сведений. Возможные значения:

- **Default** — страницы отображаются с использованием платформы по умолчанию.
- **Popover** — страница сведений частично перекрывает главную страницу.
- **Split** — главная страница отображается слева, а страница сведений выводится справа.
- **SplitOnLandscape** — экран разделяется в том случае, если устройство имеет альбомную ориентацию.
- **SplitOnPortrait** — экран разделяется в том случае, если устройство имеет книжную ориентацию.

В следующем примере кода XAML показано, как задать свойство [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) на объекте [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage):

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
                  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                  x:Class="MasterDetailPageNavigation.MainPage"
                  MasterBehavior="Popover">
  ...
</MasterDetailPage>
```

В следующем примере кода создается эквивалентный объект [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) на языке C#:

```csharp
public class MainPageCS : MasterDetailPage
{
    MasterPageCS masterPage;

    public MainPageCS ()
    {
        MasterBehavior = MasterBehavior.Popover;
        ...
    }
}
```

Тем не менее значение свойства [`MasterBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) влияет только на приложения, работающие на планшетах или настольных ПК. Для приложений, работающих на телефонах, всегда применяется поведение *Popover*.

## <a name="summary"></a>Сводка

В этой статье описываются принципы работы со страницей [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) и перехода между управляемыми страницами данных. Xamarin.Forms `MasterDetailPage` представляет собой страницу, управляющую двумя страницами связанных данных — главной страницей, которая представляет элементы, и страницей сведений, которая представляет сведения об элементах на главной странице.

## <a name="related-links"></a>Связанные ссылки

- [Виды страниц](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [MasterDetailPage (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage)
- [MasterDetailPage](xref:Xamarin.Forms.MasterDetailPage)
