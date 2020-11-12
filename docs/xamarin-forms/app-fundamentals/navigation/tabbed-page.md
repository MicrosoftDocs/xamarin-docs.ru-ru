---
title: Xamarin.Forms TabbedPage
description: Xamarin.Forms TabbedPage состоит из списка вкладок и большой области сведений, где каждая вкладка загружает содержимое в область сведений. В этой статье демонстрируется использование TabbedPage для перехода по коллекции страниц.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d5c380a5ce6e76b0f9275b09d2943be479ef09e4
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370901"
---
# <a name="no-locxamarinforms-tabbedpage"></a>Xamarin.Forms TabbedPage

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)

Xamarin.Forms [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) состоит из списка вкладок и большой области сведений, где каждая вкладка загружает содержимое в область сведений. На снимках экрана ниже показана страница `TabbedPage` в iOS и Android.

[![Снимок экрана TabbedPage с тремя вкладками в iOS и Android](tabbed-page-images/tabbedpage-today.png "TabbedPage с тремя вкладками")](tabbed-page-images/tabbedpage-today-large.png#lightbox "TabbedPage с тремя вкладками")

В iOS список вкладок отображается в нижней части экрана, а также в области данных сверху. У каждой вкладки есть заголовок и значок, который должен быть PNG-файлом с альфа-каналом. В книжной ориентации значки панели вкладок отображаются над заголовками вкладок. В альбомной ориентации значки и заголовки отображаются рядом. Кроме того, в зависимости от устройства и ориентации может отображаться обычная или компактная панель вкладок. При наличии более пяти вкладок появляется вкладка **Дополнительно** , используемая для доступа к дополнительным вкладкам. Сведения о требованиях к значкам см. в разделе о [размере значков на панели вкладок](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons#tab-bar-icon-size) на сайте developer.apple.com.

> [!TIP]
> Для `TabbedRenderer` в iOS есть переопределяемый метод `GetIcon`, который можно использовать для загрузки значков вкладок из указанного источника. Это переопределение позволяет использовать изображения SVG в виде значков в `TabbedPage`. Кроме того, можно предоставить варианты значка в выбранном и невыбранном состоянии.

В Android список вкладок отображается в верхней части экрана, а также в области данных ниже. У каждой вкладки есть заголовок и значок, который должен быть PNG-файлом с альфа-каналом. Но эти вкладки можно переместить в нижнюю часть экрана в зависимости от конкретной платформы. При наличии более пяти вкладок, список которых отображается внизу экрана, появляется вкладка *Дополнительно* , которую можно использовать для доступа к дополнительным вкладкам. Дополнительные сведения о требованиях к значкам см. в разделах о [вкладках](https://material.io/components/tabs/#) на сайте material.io и [поддержке разных значений плотности пикселей](https://developer.android.com/training/multiscreen/screendensities) на сайте developer.android.com. Дополнительные сведения о перемещении вкладок в нижнюю часть экрана см. в статье [TabbedPage Toolbar Placement and Color on Android](~/xamarin-forms/platform/android/tabbedpage-toolbar-placement-color.md) (Цвет и размещение значков на панели TabbedPage в Android).

> [!TIP]
> Кроме того, у `TabbedPageRenderer` для Android AppCompat есть переопределяемый метод `SetTabIconImageSource`, который может использоваться для загрузки значков вкладок из настраиваемого объекта `Drawable`. Это переопределение позволяет использовать изображения SVG в виде значков в `TabbedPage` и работает как для верхних, так и для нижних панелей вкладок.

На универсальной платформе Windows (UWP) список вкладок отображается в верхней части экрана, а также в области данных ниже. У каждой вкладки есть заголовок. Тем не менее на каждую вкладку можно добавлять значки в зависимости от платформы. Дополнительные сведения см. в статье о [значках TabbedPage в Windows](~/xamarin-forms/platform/windows/tabbedpage-icons.md).

## <a name="create-a-tabbedpage"></a>Создание TabbedPage

Создать [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) можно двумя способами:

- Заполните [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) коллекцией дочерних объектов [`Page`](xref:Xamarin.Forms.Page), например [`ContentPage`](xref:Xamarin.Forms.ContentPage). Дополнительные сведения см. в разделе [Заполнение TabbedPage коллекцией Page](#populate-a-tabbedpage-with-a-page-collection).
- Назначьте коллекции свойство [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource), а затем присвойте [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) свойству [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) для возврата станицы объектов в коллекции. Дополнительные сведения см. в разделе [Заполнение TabbedPage с помощью шаблона](#populate-a-tabbedpage-with-a-template).

При обоих подходах [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) отображает каждую страницу при выборе пользователем каждой из вкладок.

> [!IMPORTANT]
> Рекомендуется заполнять [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) только экземплярами [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и [`ContentPage`](xref:Xamarin.Forms.ContentPage). Это позволит обеспечить согласованность пользовательского интерфейса на всех платформах.

Кроме того, [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) определяет следующие свойства:

- [`BarBackgroundColor`](xref:Xamarin.Forms.TabbedPage.BarBackgroundColor) с типом [`Color`](xref:Xamarin.Forms.Color) — цвет фона панели вкладок;
- [`BarTextColor`](xref:Xamarin.Forms.TabbedPage.BarTextColor) с типом [`Color`](xref:Xamarin.Forms.Color) — цвет текста на панели вкладок;
- [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor) с типом [`Color`](xref:Xamarin.Forms.Color) — цвет выбранной вкладки.
- [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor) с типом [`Color`](xref:Xamarin.Forms.Color) — цвет вкладки, если она не выбрана.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть к ним можно применить стиль и их можно указывать в качестве целевых для привязки данных.

> [!WARNING]
> В [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) каждый объект [`Page`](xref:Xamarin.Forms.Page) создается при формировании `TabbedPage`. Это может повлечь проблемы с произвольностью, особенно если `TabbedPage` является корневой страницей приложения. Тем не менее оболочка Xamarin.Forms позволяет по запросу создавать страницы, доступ к которым осуществляется через панель вкладок, в ответ на навигацию. Дополнительные сведения см. в разделе [Оболочка Xamarin.Forms](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="populate-a-tabbedpage-with-a-page-collection"></a>Заполнение TabbedPage коллекцией Page

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) можно заполнить коллекцией дочерних объектов [`Page`](xref:Xamarin.Forms.Page), например [`ContentPage`](xref:Xamarin.Forms.ContentPage). Для этого необходимо добавить объекты `Page` в коллекцию [`TabbedPage.Children`](xref:Xamarin.Forms.MultiPage`1.Children*). Это можно сделать в XAML следующим образом:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" IconImageSource="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

> [!NOTE]
> Свойство [`Children`](xref:Xamarin.Forms.MultiPage`1.Children*) класса [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1), из которого происходит [`TabbedPage`](xref:Xamarin.Forms.TabbedPage), – это свойство `ContentProperty``MultiPage<T>`. Таким образом, в XAML нет необходимости явно назначать объекты [`Page`](xref:Xamarin.Forms.Page) свойству `Children`.

Эквивалентный код на C# выглядит так:

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    NavigationPage navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.IconImageSource = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

В этом примере [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) заполнена двумя объектами [`Page`](xref:Xamarin.Forms.ContentPage). Первый дочерний объект — это объект [`ContentPage`](xref:Xamarin.Forms.ContentPage), а второй дочерний объект — [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), содержащий объект `ContentPage`.

На снимках экрана ниже показан объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage):

[![Снимок экрана TabbedPage с тремя вкладками в iOS и Android](tabbed-page-images/tabbedpage-today.png "TabbedPage с тремя вкладками")](tabbed-page-images/tabbedpage-today-large.png#lightbox "TabbedPage с тремя вкладками")

При выборе другой вкладки отображается объект [`ContentPage`](xref:Xamarin.Forms.ContentPage), представляющий вкладку:

[![Снимок экрана TabbedPage со вкладками в iOS и Android](tabbed-page-images/tabbedpage-week.png "TabbedPage со вкладками")](tabbed-page-images/tabbedpage-week-large.png#lightbox "TabbedPage со вкладками")

На вкладке **Расписание** объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) упаковывается в объект [`NavigationPage`](xref:Xamarin.Forms.NavigationPage).

> [!WARNING]
> Хотя [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) можно поместить в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage), не рекомендуется размещать `TabbedPage` в `NavigationPage`. Это обусловлено тем, что в iOS `UITabBarController` всегда служит оболочкой для `UINavigationController`. Дополнительные сведения см. в разделе [Комбинированные интерфейсы контроллера представления](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) библиотеки разработчика iOS.

## <a name="navigate-within-a-tab"></a>Перемещение в пределах вкладки

Навигацию можно осуществлять в пределах одной вкладки при условии, что объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) упакован в объект [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Для этого необходимо вызвать метод [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) для свойства [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) объекта [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```csharp
await Navigation.PushAsync (new UpcomingAppointmentsPage ());
```

Страница, к которой осуществляется переход, указывается в качестве аргумента метода [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*). В этом примере страница `UpcomingAppointmentsPage` принудительно отправляется в стек навигации, где становится активной:

[![Снимок экрана: перемещение в пределах вкладки в iOS и Android](tabbed-page-images/tabbedpage-upcoming.png "Навигация в TabbedPage в пределах вкладки")](tabbed-page-images/tabbedpage-upcoming-large.png#lightbox "Навигация в TabbedPage в пределах вкладки")

Дополнительные сведения об осуществлении навигации с помощью класса [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) см. в статье [Иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## <a name="populate-a-tabbedpage-with-a-template"></a>Заполнение TabbedPage с помощью шаблона

[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) можно заполнить страницами, назначив коллекцию данных свойству [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource), а также назначив [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) свойству [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate), которое записывает данные в шаблон в виде объектов [`Page`](xref:Xamarin.Forms.Page). Это можно сделать в XAML следующим образом:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage"
            ItemsSource="{x:Static local:MonkeyDataModel.All}">            
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" IconImageSource="monkeyicon.png">
        <StackLayout Padding="5, 25">
          <Label Text="{Binding Name}" Font="Bold,Large" HorizontalOptions="Center" />
          <Image Source="{Binding PhotoUrl}" WidthRequest="200" HeightRequest="200" />
          <StackLayout Padding="50, 10">
            <StackLayout Orientation="Horizontal">
              <Label Text="Family:" HorizontalOptions="FillAndExpand" />
              <Label Text="{Binding Family}" Font="Bold,Medium" />
            </StackLayout>
            ...
          </StackLayout>
        </StackLayout>
      </ContentPage>
    </DataTemplate>
  </TabbedPage.ItemTemplate>
</TabbedPage>
```

Эквивалентный код на C# выглядит так:

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() =>
    {
      var nameLabel = new Label
      {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label
      {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage
      {
        IconImageSource = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children =
          {
            nameLabel,
            image,
            new StackLayout
            {
              Padding = new Thickness (50, 10),
              Children =
              {
                new StackLayout
                {
                  Orientation = StackOrientation.Horizontal,
                  Children =
                  {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                // ...
              }
            }
          }
        }
      };
      contentPage.SetBinding (TitleProperty, "Name");
      return contentPage;
    });
    ItemsSource = MonkeyDataModel.All;
  }
}
```

В этом примере каждая вкладка состоит из объекта [`ContentPage`](xref:Xamarin.Forms.ContentPage), который использует объекты [`Image`](xref:Xamarin.Forms.Image) и [`Label`](xref:Xamarin.Forms.Label) для вывода данных на вкладку:

[![Снимок экрана шаблона TabbedPage в iOS и Android](tabbed-page-images/tabbedpage-template.png "Шаблон TabbedPage")](tabbed-page-images/tabbedpage-template-large.png#lightbox "Шаблон TabbedPage")

При выборе другой вкладки отображается объект [`ContentPage`](xref:Xamarin.Forms.ContentPage), представляющий вкладку.

## <a name="related-links"></a>Связанные ссылки

- [TabbedPageWithNavigationPage (пример)](/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)
- [TabbedPage (пример)](/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage)
- [Иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Виды страниц](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [API TabbedPage](xref:Xamarin.Forms.TabbedPage)