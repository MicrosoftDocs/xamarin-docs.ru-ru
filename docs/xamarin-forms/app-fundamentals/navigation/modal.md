---
title: Модальные страницы Xamarin.Forms
description: Xamarin.Forms поддерживает модальные страницы. На модальной странице пользователь должен выполнить отдельную задачу, причем он не может уйти с этой страницы, пока задача не будет выполнена или отменена. В этой статье демонстрируется переход на модальные страницы.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 932f6d347afcc4957294036e2c76ef81635d958c
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368704"
---
# <a name="xamarinforms-modal-pages"></a>Модальные страницы Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/navigation-modal)

_Xamarin.Forms поддерживает модальные страницы. На модальной странице пользователь должен выполнить отдельную задачу, причем он не может уйти с этой страницы, пока задача не будет выполнена или отменена. В этой статье демонстрируется переход на модальные страницы._

Темы этой статьи:

- [Выполнение навигации](#performing-navigation) — помещение страниц в модальный стек, извлечение страниц из модального стека, отключение кнопки "Назад" и анимация перехода между страницами.
- [Передача данных при навигации](#passing-data-when-navigating) — передача данных через конструктор страницы и через `BindingContext`.

## <a name="overview"></a>Обзор

Модальная страница может быть любого из типов [Page](~/xamarin-forms/user-interface/controls/pages.md), поддерживаемых Xamarin.Forms. Для отображения модальной страницы приложение помещает ее в модальный стек, где она становится активной страницей, как показано на следующей схеме.

![Помещение страницы в модальный стек](modal-images/pushing.png)

Для возврата к предыдущей странице приложение извлекает текущую страницу из модального стека, после чего активной становится верхняя страница в стеке, как показано на следующей схеме.

![Извлечение страницы из модального стека](modal-images/popping.png)

## <a name="performing-navigation"></a>Выполнение навигации

Методы модальной навигации предоставляются свойством [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) любых типов, производных от класса [`Page`](xref:Xamarin.Forms.Page). Эти методы дают возможность [помещать модальные страницы](#pushing-pages-to-the-modal-stack) в модальный стек и [извлекать модальные страницы](#popping-pages-from-the-modal-stack) из него.

Свойство [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) также предоставляет свойство [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack), из которого можно получить модальные страницы в модальном стеке. Однако не существует средств для работы с модальным стеком или перехода к корневой странице модальной навигации. Причина в том, что такие операции поддерживаются не всеми базовыми платформами.

> [!NOTE]
> Экземпляр [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) не требуется для навигации по модальным страницам.

### <a name="pushing-pages-to-the-modal-stack"></a>Помещение страниц в модальный стек

Для перехода к странице `ModalPage` необходимо вызвать метод [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync*) свойства [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) текущей страницы, как показано в следующем примере кода.

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    ...
    await Navigation.PushModalAsync (detailPage);
  }
}
```

В результате в модальный стек помещается экземпляр `ModalPage`, где он становится активной страницей, при условии что элемент был выбран в представлении [`ListView`](xref:Xamarin.Forms.ListView) в экземпляре `MainPage`. Экземпляр `ModalPage` показан на следующих снимках экрана.

![Пример модальной страницы](modal-images/modalpage.png)

При вызове [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync*) происходят указанные ниже события:

- Вызывается переопределение [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) страницы, вызывающей `PushModalAsync`, при условии что базовой платформой не является Android.
- Вызывается переопределение [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) страницы, к которой осуществляется переход.
- Задача `PushAsync` завершается.

Однако точный порядок, в котором происходят эти события, зависит от платформы. Дополнительные сведения см. в [главе 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) книги о Xamarin.Forms Чарльза Петцольда (Charles Petzold).

> [!NOTE]
> Вызовы переопределений [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) и [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) не могут рассматриваться как гарантия перехода на страницу. Например, в iOS переопределение `OnDisappearing` вызывается на активной странице, когда приложение завершает работу.

### <a name="popping-pages-from-the-modal-stack"></a>Извлечение страниц из модального стека

Активная страница может быть извлечена из модального стека путем нажатия кнопки *Назад* на устройстве, причем это может быть как физическая кнопка, так и кнопка на экране.

Чтобы вернуться на исходную страницу программным образом, экземпляр `ModalPage` должен вызвать метод [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync), как показано в следующем примере кода:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

В результате экземпляр `ModalPage` удаляется из модального стека, а активной становится верхняя страница в нем. При вызове [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync) происходят указанные ниже события.

- У страницы, вызывающей `PopModalAsync`, вызывается переопределение [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing).
- Вызывается переопределение [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) страницы, на которую выполняется возврат, при условии что базовой платформой не является Android.
- Возвращается задача `PopModalAsync`.

Однако точный порядок, в котором происходят эти события, зависит от платформы. Дополнительные сведения см. в [главе 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) книги о Xamarin.Forms Чарльза Петцольда (Charles Petzold).

### <a name="disabling-the-back-button"></a>Отключение кнопки "Назад"

В Android пользователь всегда может вернуться на предыдущую страницу, нажав стандартную кнопку *Назад* на устройстве. Если пользователь должен выполнить автономную задачу, прежде чем покинуть модальную страницу, кнопка *Назад* в приложении должна быть отключена. Для этого можно переопределить метод [`Page.OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) модальной страницы. Дополнительные сведения см. в [главе 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) книги о Xamarin.Forms Чарльза Петцольда (Charles Petzold).

### <a name="animating-page-transitions"></a>Анимация переходов по страницам

Свойство [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) каждой страницы также предоставляет переопределенные методы отправки и извлечения, которые включают параметр `boolean`, указывающий, нужно ли отображать анимацию страниц во время перехода, как показано в следующем примере кода.

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushModalAsync (new DetailPage (), false);
}

async void OnDismissButtonClicked (object sender, EventArgs args)
{
  // Page appearance not animated
  await Navigation.PopModalAsync (false);
}
```

Установка для параметра `boolean` значения `false` отключает анимацию перехода страницы, а установка для параметра значения `true` включает анимацию, при условии что она поддерживается используемой платформой. Однако методы отправки и извлечения без этого параметра включают анимацию по умолчанию.

## <a name="passing-data-when-navigating"></a>Передача данных при переходе

Иногда странице необходимо передать данные другой странице во время навигации. Существуют два способа: передача данных с помощью конструктора страниц и указание данных для объекта [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) новой страницы. Мы обсудим оба способа.

### <a name="passing-data-through-a-page-constructor"></a>Передача данных через конструктор страниц

Самый простой способ передачи данных на другую страницу во время навигации — в качестве параметра конструктора страниц, как показано в следующем примере кода:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

В этом коде создается экземпляр `MainPage` для передачи текущих даты и времени в формате ISO8601.

Экземпляр `MainPage` получает данные с помощью параметра конструктора, как показано в следующем примере кода:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Данные отображаются на странице путем установки свойства [`Label.Text`](xref:Xamarin.Forms.Label.Text).

### <a name="passing-data-through-a-bindingcontext"></a>Передача данных через объект BindingContext

Альтернативный способ передачи данных на другую страницу во время навигации —указание данных для объекта [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) новой страницы, как показано в следующем примере кода:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    detailPage.BindingContext = e.SelectedItem as Contact;
    listView.SelectedItem = null;
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Этот код задает объекту [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) экземпляра `DetailPage` экземпляр `Contact`, а затем переходит к `DetailPage`.

Затем `DetailPage` использует привязку данных для отображения данных экземпляра `Contact`, как показано в следующем примере кода XAML.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ModalNavigation.DetailPage">
    <ContentPage.Padding>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,40,0,0" />
      </OnPlatform>
    </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" FontSize="Medium" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
              ...
            <Button x:Name="dismissButton" Text="Dismiss" Clicked="OnDismissButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода показано, как можно выполнить привязку данных в C#:

```csharp
public class DetailPageCS : ContentPage
{
  public DetailPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var dismissButton = new Button { Text = "Dismiss" };
    dismissButton.Clicked += OnDismissButtonClicked;

    Thickness padding;
    switch (Device.RuntimePlatform)
    {
        case Device.iOS:
            padding = new Thickness(0, 40, 0, 0);
            break;
        default:
            padding = new Thickness();
            break;
    }

    Padding = padding;
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
        dismissButton
      }
    };
  }

  async void OnDismissButtonClicked (object sender, EventArgs args)
  {
    await Navigation.PopModalAsync ();
  }
}
```

Данные отображаются на странице с помощью нескольких элементов управления [`Label`](xref:Xamarin.Forms.Label).

Дополнительные сведения о привязке данных см. в статье [Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/index.md).

## <a name="summary"></a>Сводка

В этой статье был продемонстрирован переход на модальные страницы. На модальной странице пользователь должен выполнить отдельную задачу, причем он не может уйти с этой страницы, пока задача не будет выполнена или отменена.

## <a name="related-links"></a>Связанные ссылки

- [Переход по страницам](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Модальная страница (пример)](/samples/xamarin/xamarin-forms-samples/navigation-modal)
- [PassingData (пример)](/samples/xamarin/xamarin-forms-samples/navigation-passingdata)