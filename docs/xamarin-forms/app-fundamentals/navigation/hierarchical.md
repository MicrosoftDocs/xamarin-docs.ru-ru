---
title: 'title: "Иерархическая навигация" description: "В этой статье показано, как использовать класс NavigationPage для навигации в стеке страниц по методу ЛИФО".'
description: 'ms.prod: xamarin ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 03/10/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ec35b03e7e96f0730813918bdd96e1408cfabde7
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571497"
---
# <a name="hierarchical-navigation"></a>Иерархическая навигация

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)

_Класс NavigationPage обеспечивает иерархическую навигацию, при которой пользователь может переходить по страницам вперед и назад по своему желанию. Этот класс реализует навигацию на основе стека объектов Page по методу ЛИФО (последним поступил — первым обслужен). В этой статье показано, как использовать класс NavigationPage для навигации в стеке страниц._

Для перехода с одной страницы на другую приложение помещает новую страницу в стек навигации, где она становится активной страницей, как показано на следующем рисунке.

![](hierarchical-images/pushing.png "Pushing a Page to the Navigation Stack")

Для возврата к предыдущей странице приложение выбирает текущую страницу из стека навигации, после чего активной становится верхняя страница в стеке, как показано на следующем рисунке.

![](hierarchical-images/popping.png "Popping a Page from the Navigation Stack")

Методы навигации предоставляются свойством [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) любых типов, производных от класса [`Page`](xref:Xamarin.Forms.Page). Эти методы предоставляют возможность для отправки страниц в стек навигации, извлечения страниц из стека навигации, а также для выполнения операций со стеком.

## <a name="performing-navigation"></a>Выполнение навигации

При иерархической навигации класс [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) используется для перехода по стеку объектов [`ContentPage`](xref:Xamarin.Forms.ContentPage). На следующих снимках экрана показаны основные компоненты `NavigationPage` на каждой платформе.

![](hierarchical-images/navigationpage-components.png "NavigationPage Components")

Макет [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) зависит от платформы:

- В iOS панель навигации находится в верхней части страницы, где отображается заголовок и кнопка *Назад*, которая возвращает на предыдущую страницу.
- В Android панель навигации находится в верхней части страницы, где отображается заголовок, значок и кнопка *Назад*, которая возвращает на предыдущую страницу. Значок определяется в атрибуте `[Activity]`, который оформляет класс `MainActivity` в проекте, зависящем от платформы Android.
- На универсальной платформе Windows панель навигации расположена в верхней части страницы, где отображается заголовок.

На всех платформах значение свойства [`Page.Title`](xref:Xamarin.Forms.Page.Title) отображается как заголовок страницы. Кроме того, свойству `IconColor` можно задать значение [`Color`](xref:Xamarin.Forms.Color), которое применяется к значку на панели навигации.

> [!NOTE]
> Рекомендуется заполнять `NavigationPage` только экземплярами `ContentPage`.

### <a name="creating-the-root-page"></a>Создание корневой страницы

Первая страница, добавленная в стек навигации, называется *корневой* страницей приложения, что демонстрируется в следующем примере кода:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

В результате в стек навигации помещается экземпляр `Page1Xaml` [`ContentPage`](xref:Xamarin.Forms.ContentPage), где он становится активной страницей и корневой страницей приложения. Эти действия показаны на следующих снимках экрана.

![](hierarchical-images/mainpage.png "Root Page of Navigation Stack")

> [!NOTE]
> Свойство [`RootPage`](xref:Xamarin.Forms.NavigationPage.RootPage) экземпляра [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) предоставляет доступ к первой странице в стеке навигации.

### <a name="pushing-pages-to-the-navigation-stack"></a>Помещение страниц в стек навигации

Для перехода к странице `Page2Xaml` необходимо вызвать метод [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) свойства [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) текущей страницы, как показано в следующем примере кода.

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

В результате в стек навигации помещается экземпляр `Page2Xaml`, где он становится активной страницей. Эти действия показаны на следующих снимках экрана.

![](hierarchical-images/secondpage.png "Page Pushed onto Navigation Stack")

Когда вызывается метод [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*), происходят следующие события.

- У страницы, вызывающей `PushAsync`, вызывается переопределение [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing).
- Вызывается переопределение [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) страницы, к которой осуществляется переход.
- Задача `PushAsync` завершается.

Однако точный порядок, в котором происходят эти события, зависит от платформы. Дополнительные сведения см. в [главе 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) книги о Xamarin.Forms Чарльза Петцольда (Charles Petzold).

> [!NOTE]
> Вызовы переопределений [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) и [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) не могут рассматриваться как гарантия перехода на страницу. Например, в iOS переопределение `OnDisappearing` вызывается на активной странице, когда приложение завершает работу.

### <a name="popping-pages-from-the-navigation-stack"></a>Извлечение страниц из стека навигации

Активная страница может быть извлечена из стека навигации путем нажатия кнопки *Назад* на устройстве, причем это может быть как физическая кнопка, так и кнопка на экране.

Чтобы вернуться на исходную страницу программным образом, экземпляр `Page2Xaml` должен вызвать метод [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync), как показано в следующем примере кода:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

В результате экземпляр `Page2Xaml` удаляется из стека навигации, а активной становится верхняя страница в нем. Когда вызывается метод [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync), происходят следующие события.

- У страницы, вызывающей `PopAsync`, вызывается переопределение [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing).
- У страницы, к которой вы возвращаетесь, вызывается переопределение [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing).
- Возвращается задача `PopAsync`.

Однако точный порядок, в котором происходят эти события, зависит от платформы. Дополнительные сведения см. в [главе 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) книги о Xamarin.Forms Чарльза Петцольда (Charles Petzold).

Как и методы [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) и [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync), свойство [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) каждой страницы также предоставляет метод [`PopToRootAsync`](xref:Xamarin.Forms.NavigationPage.PopToRootAsync), который показан в следующем примере кода.

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Этот метод извлекает все, кроме корневого объекта [`Page`](xref:Xamarin.Forms.Page), из стека навигации; таким образом, корневая страница приложения становится активной страницей.

### <a name="animating-page-transitions"></a>Анимация переходов по страницам

Свойство [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) каждой страницы также предоставляет переопределенные методы отправки и извлечения, которые включают параметр `boolean`, указывающий, нужно ли отображать анимацию страниц во время перехода, как показано в следующем примере кода.

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushAsync (new Page2Xaml (), false);
}

async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopAsync (false);
}

async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopToRootAsync (false);
}
```

Установка для параметра `boolean` значения `false` отключает анимацию перехода страницы, а установка для параметра значения `true` включает анимацию, при условии что она поддерживается используемой платформой. Однако методы отправки и извлечения без этого параметра включают анимацию по умолчанию.

## <a name="passing-data-when-navigating"></a>Передача данных при переходе

Иногда странице необходимо передать данные другой странице во время навигации. Существует два способа: передача данных с помощью конструктора страниц и указание данных для объекта [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) новой страницы. Мы обсудим оба способа.

### <a name="passing-data-through-a-page-constructor"></a>Передача данных через конструктор страниц

Самый простой способ передачи данных на другую страницу во время навигации — в качестве параметра конструктора страниц, как показано в следующем примере кода.

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Этот код создает экземпляр `MainPage`, передавая текущую дату и время в формате ISO8601, который упаковывается в экземпляр [`NavigationPage`](xref:Xamarin.Forms.NavigationPage).

Экземпляр `MainPage` получает данные с помощью параметра конструктора, как показано в следующем примере кода.

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Данные отображаются на странице путем установки свойства [`Label.Text`](xref:Xamarin.Forms.Label.Text), как показано на следующих снимках экрана.

![](hierarchical-images/passing-data-constructor.png "Data Passed Through a Page Constructor")

### <a name="passing-data-through-a-bindingcontext"></a>Передача данных через объект BindingContext

Альтернативный способ передачи данных на другую страницу во время навигации —указание данных для объекта [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) новой страницы, как показано в следующем примере кода:

```csharp
async void OnNavigateButtonClicked (object sender, EventArgs e)
{
  var contact = new Contact {
    Name = "Jane Doe",
    Age = 30,
    Occupation = "Developer",
    Country = "USA"
  };

  var secondPage = new SecondPage ();
  secondPage.BindingContext = contact;
  await Navigation.PushAsync (secondPage);
}
```

Этот код задает объекту [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) экземпляра `SecondPage` экземпляр `Contact`, а затем переходит к `SecondPage`.

Затем `SecondPage` использует привязку данных для отображения данных экземпляра `Contact`, как показано в следующем примере кода XAML.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="PassingData.SecondPage"
             Title="Second Page">
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
            ...
            <Button x:Name="navigateButton" Text="Previous Page" Clicked="OnNavigateButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода показано, как можно выполнить привязку данных в C#.

```csharp
public class SecondPageCS : ContentPage
{
  public SecondPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var navigateButton = new Button { Text = "Previous Page" };
    navigateButton.Clicked += OnNavigateButtonClicked;

    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        navigateButton
      }
    };
  }

  async void OnNavigateButtonClicked (object sender, EventArgs e)
  {
    await Navigation.PopAsync ();
  }
}
```

Данные отображаются на странице с помощью нескольких элементов управления [`Label`](xref:Xamarin.Forms.Label), как показано на следующих снимках экрана.

![](hierarchical-images/passing-data-bindingcontext.png "Data Passed Through a BindingContext")

Дополнительные сведения о привязке данных см. в статье [Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="manipulating-the-navigation-stack"></a>Управление стеком навигации

Свойство [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) предоставляет свойство [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack), из которого могут быть получены страницы в стеке навигации. Хотя Xamarin.Forms поддерживает доступ к стеку навигации, свойство `Navigation` предоставляет методы [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore*) и [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage*) для управления стеком путем вставки страниц или их удаления.

Метод [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore*) вставляет указанную страницу в стек навигации перед указанной существующей страницей, как показано на следующей схеме.

![](hierarchical-images/insert-page-before.png "Inserting a Page in the Navigation Stack")

Метод [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage*) удаляет указанную страницу из стека навигации, как показано на следующей схеме.

![](hierarchical-images/remove-page.png "Removing a Page from the Navigation Stack")

Эти методы обеспечивают возможность настраиваемой навигации, например замены страницы входа на новую страницу после успешного входа. Следующий пример кода демонстрирует этот сценарий.

```csharp
async void OnLoginButtonClicked (object sender, EventArgs e)
{
  ...
  var isValid = AreCredentialsCorrect (user);
  if (isValid) {
    App.IsUserLoggedIn = true;
    Navigation.InsertPageBefore (new MainPage (), this);
    await Navigation.PopAsync ();
  } else {
    // Login failed
  }
}

```

При условии что учетные данные пользователя верны, экземпляр `MainPage` вставляется в стек навигации перед текущей страницей. Метод [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) удаляет текущую страницу из стека навигации, а активной страницей становится экземпляр `MainPage`.

## <a name="displaying-views-in-the-navigation-bar"></a>Отображение представлений на панели навигации

Любое представление Xamarin.Forms [`View`](xref:Xamarin.Forms.View) может отображаться на панели навигации [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Для этого нужно установить присоединенное свойство [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) в `View`. Это присоединенное свойство может быть задано для любого объекта [`Page`](xref:Xamarin.Forms.Page), и, когда `Page` помещается в `NavigationPage`, `NavigationPage` будет учитывать значение этого свойства.

В следующем примере, взятом из [примера представления заголовка](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-titleview), показано, как задать присоединенное свойство [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) в XAML.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="NavigationPageTitleView.TitleViewPage">
    <NavigationPage.TitleView>
        <Slider HeightRequest="44" WidthRequest="300" />
    </NavigationPage.TitleView>
    ...
</ContentPage>
```

Вот эквивалент в коде C#:

```csharp
public class TitleViewPage : ContentPage
{
    public TitleViewPage()
    {
        var titleView = new Slider { HeightRequest = 44, WidthRequest = 300 };
        NavigationPage.SetTitleView(this, titleView);
        ...
    }
}
```

В результате объект [`Slider`](xref:Xamarin.Forms.Slider) отображается на панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage):

[![TitleView ползунка](hierarchical-images/titleview-small.png "TitleView ползунка")](hierarchical-images/titleview-large.png#lightbox "TitleView ползунка")

> [!IMPORTANT]
> Многие представления не будут отображаться на панели навигации, если не указан размер представления с помощью свойств [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest). Кроме того, представление может быть заключено в [`StackLayout`](xref:Xamarin.Forms.StackLayout) со свойствами [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions), для которых установлены соответствующие значения.

Поскольку класс [`Layout`](xref:Xamarin.Forms.Layout) является производным от класса [`View`](xref:Xamarin.Forms.View), присоединенное свойство [`TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) можно настроить для отображения класса макета, содержащего несколько представлений. В iOS и на универсальной платформе Windows (UWP) высоту панели навигации нельзя изменить, поэтому она будет обрезана, если представление, отображаемое на панели навигации, больше, чем размер панели навигации по умолчанию. В Android высоту панели навигации можно изменить, задав для привязываемого свойства [`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) значение `double`, представляющее новую высоту. Дополнительные сведения см. в разделе [Установка высоты панели навигации в объекте NavigationPage](~/xamarin-forms/platform/android/navigationpage-bar-height.md).

Также панель навигации можно расширить, если поместить некоторое содержимое на панели навигации, а некоторое — в представление в верхней части страницы таким образом, чтобы цвет совпадал с панелью навигации. Кроме того, в iOS разделительную линию и тень в нижней части панели навигации можно удалить, установив для привязываемого свойства [`NavigationPage.HideNavigationBarSeparator`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) значение `true`. Дополнительные сведения см. в разделе [Сокрытие разделителя панели навигации в объекте NavigationPage](~/xamarin-forms/platform/ios/navigation-bar-separator.md).

> [!NOTE]
> Свойства [`BackButtonTitle`](xref:Xamarin.Forms.NavigationPage.BackButtonTitleProperty), [`Title`](xref:Xamarin.Forms.Page.Title), [`TitleIcon`](xref:Xamarin.Forms.NavigationPage.TitleIconProperty) и [`TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) позволяют определять значения, которые занимают место на панели навигации. Хотя размер панели навигации зависит от платформы и размера экрана, установка всех этих свойств приведет к конфликтам из-за ограничений свободного пространства. Вместо комбинации этих свойств лучше задать желаемый дизайн панели навигации только с помощью свойства `TitleView`.

### <a name="limitations"></a>Ограничения

Существует ряд ограничений, которые следует учитывать при отображении объекта [`View`](xref:Xamarin.Forms.View) на панели навигации [`NavigationPage`](xref:Xamarin.Forms.NavigationPage).

- В iOS представления, размещенные на панели навигации `NavigationPage`, отображаются по-разному в зависимости от того, включены ли крупные заголовки. Дополнительные сведения о включении крупных заголовков см. в разделе [Отображение крупных заголовков](~/xamarin-forms/platform/ios/page-large-title.md).
- В Android можно поместить представления на панели навигации `NavigationPage` только в приложениях, использующих совместимость приложений.
- Не рекомендуется помещать большие и сложные представления, например [`ListView`](xref:Xamarin.Forms.ListView) и [`TableView`](xref:Xamarin.Forms.TableView), в строке навигации `NavigationPage`.

## <a name="related-links"></a>Связанные ссылки

- [Переход по страницам](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Иерархическое (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)
- [PassingData (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)
- [LoginFlow (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)
- [TitleView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-titleview)
- [Видеоролик о создании страницы для входа в Xamarin.Forms](https://www.youtube.com/watch?v=qKQ7pyyG1fo)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
