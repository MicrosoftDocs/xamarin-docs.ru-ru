---
title: "Xamarin.Forms — страница карусели" description: "CarouselPage в Xamarin.Forms — это страница, по которой пользователи могут проводить из стороны в сторону, чтобы переходить по страницам содержимого, например по страницам коллекции. В этой статье демонстрируется использование CarouselPage для перехода по коллекции страниц".
ms.prod: xamarin ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 12/01/2017 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-carousel-page"></a>Страница карусели в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)

_CarouselPage в Xamarin.Forms — это страница, по которой пользователи могут проводить из стороны в сторону, чтобы переходить по страницам содержимого, например по страницам коллекции. В этой статье демонстрируется использование CarouselPage для перехода по коллекции страниц._

> [!IMPORTANT]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) был заменен на [`CarouselView`](xref:Xamarin.Forms.CarouselView), предоставляющий прокручиваемый макет, в котором пользователи могут перемещаться по коллекции элементов, проводя пальцем. Дополнительные сведения о `CarouselView` см. в разделе [Xamarin.Forms CarouselView](~/xamarin-forms/user-interface/carouselview/index.md).

На следующих снимках экрана показана страница [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) на каждой платформе.

![](carousel-page-images/thirdpage.png "CarouselPage Third Item")

Макет [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) одинаков на каждой платформе. По страницам можно переходить, проводя по экрану вправо для перехода вперед по коллекции или влево для перехода назад. На следующих снимках экрана показана первая страница в экземпляре [`CarouselPage`](xref:Xamarin.Forms.CarouselPage).

![](carousel-page-images/firstpage.png "CarouselPage First Item")

Если провести по экрану пальцем справа налево, будет выполнен переход ко второй странице, как показано на следующих снимках экрана.

![](carousel-page-images/secondpage.png "CarouselPage Second Item")

Если провести по экрану пальцем справа налево еще раз, будет выполнен переход к третьей странице, а если провести слева направо, то вы вернетесь к предыдущей странице.

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) не поддерживает виртуализацию пользовательского интерфейса. Поэтому, если `CarouselPage` содержит слишком много дочерних элементов, производительность может снижаться.

Если объект [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) внедрен на страницу [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) страницы [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage), свойству [`MasterDetailPage.IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) необходимо присвоить значение `false`, чтобы предотвратить конфликты жестов между `CarouselPage` и `MasterDetailPage`.

Дополнительные сведения о [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) см. в [главе 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) книги о Xamarin.Forms Чарльза Петцольда (Charles Petzold).

## <a name="create-a-carouselpage"></a>Создание CarouselPage

Создать [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) можно двумя способами:

- [Заполните](#populate-a-carouselpage-with-a-page-collection)`CarouselPage` коллекцией дочерних экземпляров [`ContentPage`](xref:Xamarin.Forms.ContentPage).
- [Присвойте](#populate-a-carouselpage-with-a-template) коллекцию свойству [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource), а затем присвойте [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) свойству [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate), чтобы вернуть экземпляры [`ContentPage`](xref:Xamarin.Forms.ContentPage) для объектов в коллекции.

Оба способа приводят к тому, что в `CarouselPage` страницы будут отображаться по очереди. При проведении пальцем по экрану будет выполняться переход к следующей странице.

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) можно заполнять только экземплярами [`ContentPage`](xref:Xamarin.Forms.ContentPage) или производными объектами `ContentPage`.

### <a name="populate-a-carouselpage-with-a-page-collection"></a>Заполнение CarouselPage коллекцией страниц

В следующем примере кода XAML показан объект [`CarouselPage`](xref:Xamarin.Forms.CarouselPage), отображающий три экземпляра [`ContentPage`](xref:Xamarin.Forms.ContentPage).

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

В следующем примере кода показан эквивалентный пользовательский интерфейс на языке C#:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

На каждой странице [`ContentPage`](xref:Xamarin.Forms.ContentPage) отображается элемент [`Label`](xref:Xamarin.Forms.Label) для соответствующего цвета и элемент [`BoxView`](xref:Xamarin.Forms.BoxView) этого цвета.

### <a name="populate-a-carouselpage-with-a-template"></a>Заполнение CarouselPage с помощью шаблона

В следующем примере кода XAML показан объект [`CarouselPage`](xref:Xamarin.Forms.CarouselPage), сформированный путем присвоения [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) свойству [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) для возврата страниц для объектов в коллекции.

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

Объект [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) заполняется данными путем задания свойства [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) в конструкторе для файла кода программной части:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

В следующем примере кода создается эквивалентный объект [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) на языке C#:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

На каждой странице [`ContentPage`](xref:Xamarin.Forms.ContentPage) отображается элемент [`Label`](xref:Xamarin.Forms.Label) для соответствующего цвета и элемент [`BoxView`](xref:Xamarin.Forms.BoxView) этого цвета.

## <a name="related-links"></a>Связанные ссылки

- [Виды страниц](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)
- [CarouselPageTemplate (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
