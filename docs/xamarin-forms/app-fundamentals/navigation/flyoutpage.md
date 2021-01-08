---
title: FlyoutPage в Xamarin.Forms
description: 'FlyoutPage в Xamarin.Forms представляет собой страницу, которая обеспечивает доступ к двум страницам со связанными данными: всплывающей странице, где представлены элементы, и странице со сведениями об этих элементах. Статья описывает использование такой всплывающей страницы и переход между связанными с ней страницами сведений.'
ms.prod: xamarin
ms.assetid: 119945E3-58B8-4630-A3D2-8B561529D53B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ad3b5d76816710d45cd1d8e738771e39555401fa
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940630"
---
# <a name="no-locxamarinforms-flyoutpage"></a>FlyoutPage в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-flyoutpage)

На всплывающей странице обычно отображается список элементов, как показано на следующих снимках экрана:

[![Компоненты всплывающей страницы](flyoutpage-images/flyoutpage-components.png)](flyoutpage-images/flyoutpage-components-large.png#lightbox "Компоненты всплывающей страницы")

Расположение списка элементов одинаково для каждой платформы, а при выборе отдельного элемента осуществляется переход на соответствующую страницу сведений. Кроме того, на всплывающей странице также представлена панель навигации с кнопкой, позволяющей перейти к активной странице сведений:

- В iOS панель навигации располагается в верхней части страницы и содержит кнопку для перехода на страницу сведений. Кроме того, для перехода на активную страницу сведений можно провести пальцем по главной странице влево.
- В Android панель навигации располагается в верхней части страницы и содержит заголовок, значок, а также кнопку для перехода на страницу сведений. Значок определяется в атрибуте `[Activity]`, который оформляет класс `MainActivity` в проекте, зависящем от платформы Android. Кроме того, для перехода на активную страницу сведений можно провести по всплывающей странице влево, коснуться страницы сведений в крайней правой части экрана и коснуться кнопки *Назад* внизу экрана.
- На универсальной платформе Windows (UWP) панель навигации располагается в верхней части страницы и содержит кнопку для перехода на страницу сведений.

Страница сведений с данными, соответствующими выбранному на всплывающей странице элементу, а также основные компоненты страницы сведений показаны на следующих снимках экрана:

![Компоненты страницы сведений](flyoutpage-images/detailpage-components.png)

На странице сведений располагается панель навигации, содержимое которой зависит от платформы:

- На iOS панель навигации находится в верхней части страницы и содержит заголовок, а также кнопку для возврата на всплывающую страницу при условии, что экземпляр страницы сведений находится внутри экземпляра [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Кроме того, для возврата на всплывающую страницу можно провести по странице сведений вправо.
- На Android панель навигации находится в верхней части страницы, где отображаются заголовок, значок и кнопка для возврата на всплывающую страницу. Значок определяется в атрибуте `[Activity]`, который оформляет класс `MainActivity` в проекте, зависящем от платформы Android.
- На универсальной платформе Windows (UWP) панель навигации находится в верхней части страницы, где отображаются заголовок и кнопка для возврата на всплывающую страницу.

## <a name="navigation-behavior"></a>Выполнение переходов

Реакция на события перехода между всплывающей страницей и страницами сведений зависит от платформы:

- На iOS страница сведений *сдвигается* вправо, а всплывающая страница выдвигается слева. При этом левая часть страницы сведений по-прежнему остается видимой.
- На Android всплывающая страница и страницы сведений *накладываются* друг на друга.
- На UWP всплывающая страница выдвигается слева и частично накладывается на страницу сведений при условии, что свойство [`FlyoutLayoutBehavior`](xref:Xamarin.Forms.FlyoutPage.FlyoutLayoutBehavior) имеет значение `Popover`.

То же самое будет происходить в альбомной ориентации, только на iOS и Android всплывающая страница будет иметь примерно ту же ширину, что и в книжной, позволяя увидеть больше содержимого страницы сведений.

Дополнительные сведения: [Управление разметкой страницы сведений](#control-the-detail-page-layout-behavior).

## <a name="create-a-flyoutpage"></a>Создание FlyoutPage

Страница [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) содержит свойства [`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) и [`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail) типа [`Page`](xref:Xamarin.Forms.Page), которые используются для получения и задания соответственно всплывающей страницы и страниц сведений.

> [!IMPORTANT]
> Объект [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) должен представлять корневую страницу. Если использовать его в качестве дочерней или какой-либо другой страницы, это может привести к непредвиденному или несогласованному поведению. Кроме того, рекомендуется всегда использовать в качестве всплывающей страницы `FlyoutPage` экземпляр [`ContentPage`](xref:Xamarin.Forms.ContentPage), а для заполнения страницы сведений использовать только экземпляры [`TabbedPage`](xref:Xamarin.Forms.TabbedPage), [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и `ContentPage`. Это позволит обеспечить согласованность пользовательского интерфейса на всех платформах.

В следующем примере показан код XAML для объекта [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage), который задает свойства [`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) и [`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail):

```xaml
<FlyoutPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:FlyoutPageNavigation;assembly=FlyoutPageNavigation"
            x:Class="FlyoutPageNavigation.MainPage">
    <FlyoutPage.Flyout>
        <local:FlyoutMenuPage x:Name="flyoutPage" />
    </FlyoutPage.Flyout>
    <FlyoutPage.Detail>
        <NavigationPage>
            <x:Arguments>
                <local:ContactsPage />
            </x:Arguments>
        </NavigationPage>
    </FlyoutPage.Detail>
</FlyoutPage>
```

В следующем примере кода создается эквивалентный объект [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) на языке C#:

```csharp
public class MainPageCS : FlyoutPage
{
    FlyoutMenuPageCS flyoutPage;

    public MainPageCS()
    {
        flyoutPage = new FlyoutMenuPageCS();
        Flyout = flyoutPage;
        Detail = new NavigationPage(new ContactsPageCS());
        ...
    }
    ...
}    
```

Свойство [`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) содержит экземпляр [`ContentPage`](xref:Xamarin.Forms.ContentPage). Свойство [`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail) содержит объект [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), содержащий экземпляр `ContentPage`.

### <a name="create-the-flyout-page"></a>Создание всплывающей страницы

В следующем примере кода XAML показано объявление объекта `FlyoutMenuPage`, на который задаются ссылки посредством свойства [`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="using:FlyoutPageNavigation"
             x:Class="FlyoutPageNavigation.FlyoutMenuPage"
             Padding="0,40,0,0"
             IconImageSource="hamburger.png"
             Title="Personal Organiser">
    <StackLayout>
        <ListView x:Name="listView" x:FieldModifier="public">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:FlyoutPageItem}">
                    <local:FlyoutPageItem Title="Contacts" IconSource="contacts.png" TargetType="{x:Type local:ContactsPage}" />
                    <local:FlyoutPageItem Title="TodoList" IconSource="todo.png" TargetType="{x:Type local:TodoListPage}" />
                    <local:FlyoutPageItem Title="Reminders" IconSource="reminders.png" TargetType="{x:Type local:ReminderPage}" />
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

Эта страница содержит объект [`ListView`](xref:Xamarin.Forms.ListView), который заполняется данными в XAML. Для этого его свойству [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) присваивается массив объектов `FlyoutPageItem`. Каждый объект `FlyoutPageItem` определяет свойства `Title`, `IconSource` и `TargetType`.

Объект [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) присваивается свойству [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) для отображения каждого объекта `FlyoutPageItem`. Объект `DataTemplate` содержит объект [`ViewCell`](xref:Xamarin.Forms.ViewCell), который состоит из объектов [`Image`](xref:Xamarin.Forms.Image) и [`Label`](xref:Xamarin.Forms.Label). Объект [`Image`](xref:Xamarin.Forms.Image) отображает значение свойства `IconSource`, а объект [`Label`](xref:Xamarin.Forms.Label) отображает значение свойства `Title` для каждого объекта `FlyoutPageItem`.

Для этой страницы заданы свойства [`Title`](xref:Xamarin.Forms.Page.Title) и [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource). Если у страницы сведений есть заголовок, на ней появится значок. Чтобы реализовать это в iOS, необходимо упаковать экземпляр страницы сведений в экземпляр [`NavigationPage`](xref:Xamarin.Forms.NavigationPage).

> [!NOTE]
> Для страницы [`Flyout`](xref:Xamarin.Forms.FlyoutPage.Flyout) должно быть задано свойство [`Title`](xref:Xamarin.Forms.Page.Title), иначе возникнет исключение.

В следующем примере кода создается эквивалентная страница на языке C#:

```csharp
public class FlyoutMenuPageCS : ContentPage
{
    ListView listView;
    public ListView ListView { get { return listView; } }

    public FlyoutMenuPageCS()
    {
        var flyoutPageItems = new List<FlyoutPageItem>();
        flyoutPageItems.Add(new FlyoutPageItem
        {
            Title = "Contacts",
            IconSource = "contacts.png",
            TargetType = typeof(ContactsPageCS)
        });
        flyoutPageItems.Add(new FlyoutPageItem
        {
            Title = "TodoList",
            IconSource = "todo.png",
            TargetType = typeof(TodoListPageCS)
        });
        flyoutPageItems.Add(new FlyoutPageItem
        {
            Title = "Reminders",
            IconSource = "reminders.png",
            TargetType = typeof(ReminderPageCS)
        });

        listView = new ListView
        {
            ItemsSource = flyoutPageItems,
            ItemTemplate = new DataTemplate(() =>
            {
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
        Padding = new Thickness(0, 40, 0, 0);
        Content = new StackLayout
        {
            Children = { listView }
        };
    }
}
```

На следующих снимках экрана показаны всплывающие страницы на каждой платформе:

![Пример эталонной страницы](flyoutpage-images/flyoutpage.png)

### <a name="create-and-display-the-detail-page"></a>Создание и отображение страницы сведений

Экземпляр `FlyoutMenuPage` содержит свойство `ListView`, которое предоставляет экземпляр [`ListView`](xref:Xamarin.Forms.ListView), благодаря чему экземпляр `MainPage` [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) может зарегистрировать обработчик для события [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected). Это позволяет экземпляру `MainPage` присвоить свойству [`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail) страницу, которая представляет выбранный элемент `ListView`. В следующем примере кода показан обработчик событий:

```csharp
public partial class MainPage : FlyoutPage
{
    public MainPage()
    {
        ...
        flyoutPage.listView.ItemSelected += OnItemSelected;
    }

    void OnItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var item = e.SelectedItem as FlyoutPageItem;
        if (item != null)
        {
            Detail = new NavigationPage((Page)Activator.CreateInstance(item.TargetType));
            flyoutPage.listView.SelectedItem = null;
            IsPresented = false;
        }
    }
}
```

Метод `OnItemSelected` выполняет следующие действия:

- Извлекает объект [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) из экземпляра [`ListView`](xref:Xamarin.Forms.ListView) и, если он не имеет значение `null`, устанавливает в качестве страницы сведений новый экземпляр типа страницы, хранящегося в свойстве `TargetType` объекта `FlyoutPageItem`. Тип страницы упаковывается в экземпляр [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), чтобы обеспечить возможность ссылаться на значок посредством свойства [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) на объекте `FlyoutMenuPage`, который отображается на главной странице в iOS.
- Элемент, выбранный в объекте [`ListView`](xref:Xamarin.Forms.ListView), получает значение `null`. Это позволяет гарантировать, что в следующий раз при отображении объекта `FlyoutMenuPage` не будет выбран ни один из элементов объекта `ListView`.
- Для отображения страницы сведений пользователю свойства [`FlyoutPage.IsPresented`](xref:Xamarin.Forms.FlyoutPage.IsPresented) присваивается значение `false`. Это свойство определяет, отображается ли всплывающая страница или страница сведений. Если оно имеет значение `true`, то отображается всплывающая страница, а значение `false` позволяет отобразить страницу сведений.

На следующих снимках экрана показана страница сведений `ContactPage`, которая выводится на экран после выбора соответствующего элемента на всплывающей странице:

![Пример страницы сведений](flyoutpage-images/detailpage.png)

### <a name="control-the-detail-page-layout-behavior"></a>Управление разметкой страницы сведений

Принципы управления всплывающей страницей и страницами сведений для [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) зависят от того, работает ли приложение на телефоне или планшете, от ориентации устройства, а также от значения свойства [`FlyoutLayoutBehavior`](xref:Xamarin.Forms.FlyoutPage.FlyoutLayoutBehavior). Это свойство определяет, каким образом будет отображаться страница сведений. Возможные значения:

- `Default` — страницы отображаются с использованием платформы по умолчанию.
- `Popover` — страница сведений полностью или частично перекрывает всплывающую страницу.
- `Split` — всплывающая страница отображается слева, а страница сведений находится справа.
- `SplitOnLandscape` — экран разделяется в том случае, если устройство имеет альбомную ориентацию.
- `SplitOnPortrait` — экран разделяется в том случае, если устройство имеет книжную ориентацию.

В следующем примере кода XAML показано, как задать свойство [`FlyoutLayoutBehavior`](xref:Xamarin.Forms.FlyoutPage.FlyoutLayoutBehavior) на объекте [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage):

```xaml
<FlyoutPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            x:Class="FlyoutPageNavigation.MainPage"
            FlyoutLayoutBehavior="Popover">
  ...
</FlyoutPage>
```

В следующем примере кода создается эквивалентный объект [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) на языке C#:

```csharp
public class MainPageCS : FlyoutPage
{
    FlyoutMenuPageCS flyoutPage;

    public MainPageCS()
    {
        ...
        FlyoutLayoutBehavior = FlyoutLayoutBehavior.Popover;
    }
}
```

> [!IMPORTANT]
> Значение свойства [`FlyoutLayoutBehavior`](xref:Xamarin.Forms.FlyoutPage.FlyoutLayoutBehavior) влияет только на приложения, работающие на компьютерах или планшетах. Для приложений, работающих на телефонах, всегда применяется поведение `Popover`.

## <a name="related-links"></a>Связанные ссылки

- [Виды страниц](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [FlyoutPage (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-flyoutpage)
- [API FlyoutPage](xref:Xamarin.Forms.FlyoutPage)
