---
ms.openlocfilehash: bf46d8ab4ca124bc36dd971513a78147e71a0039
ms.sourcegitcommit: 4d260b655cb52b990dda79c239a9721f2e964625
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2021
ms.locfileid: "98570865"
---
В этом упражнении вы создадите пользовательский интерфейс для класса `RestService`, который получает данные репозитория .NET от веб-API GitHub. Извлеченные данные будут отображаться [`CollectionView`](xref:Xamarin.Forms.CollectionView).

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений** в проекте **WebServiceTutorial** дважды щелкните файл **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Get Repository Data"
                    Clicked="OnButtonClicked" />
            <CollectionView x:Name="collectionView">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <StackLayout>
                            <Label Text="{Binding Name}"
                                   FontSize="Medium" />
                            <Label Text="{Binding Description}"
                                   TextColor="Silver"
                                   FontSize="Small" />
                            <Label Text="{Binding GitHubHomeUrl}"
                                   TextColor="Gray"
                                   FontSize="Caption" />
                        </StackLayout>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    В этом коде декларативно определяется пользовательский интерфейс для страницы, который состоит из [`Button`](xref:Xamarin.Forms.Button) и [`CollectionView`](xref:Xamarin.Forms.CollectionView). `Button` устанавливает событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) для обработчика событий `OnButtonClicked`, который будет создан на следующем шаге. `CollectionView` задает значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для свойства [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate), которое определяет внешний вид каждого элемента в `CollectionView`. Дочерний элемент `DataTemplate` — [`StackLayout`](xref:Xamarin.Forms.StackLayout), содержащий три объекта [`Label`](xref:Xamarin.Forms.Label). Объекты `Label` привязывают свои свойства [`Text`](xref:Xamarin.Forms.Label.Text) к свойствам `Name`, `Description` и `GitHubHomeUrl` каждого из объектов `Repository`. Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    Кроме того, [`CollectionView`](xref:Xamarin.Forms.CollectionView) имеет имя, указанное с помощью атрибута `x:Name`. Это позволяет файлу с выделенным кодом получать доступ к объекту с помощью назначенного имени.

1. В **обозревателе решений** в проекте **WebServiceTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем удалите из **MainPage.xaml.cs** весь шаблонный код и замените его приведенным ниже:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                List<Repository> repositories = await _restService.GetRepositoriesAsync(Constants.GitHubReposEndpoint);
                collectionView.ItemsSource = repositories;
            }
        }
    }
    ```

    Метод `OnButtonClicked`, который выполняется при нажатии [`Button`](xref:Xamarin.Forms.Button), вызывает метод `RestService.GetRepositoriesAsync` для получения данных репозитория .NET от веб-API GitHub. Метод `GetRepositoriesAsync` требует аргумент `string`, представляющий URI вызываемого веб-API. Этот URI возвращается полем `Constants.GitHubReposEndpoint`.

    После получения запрошенных данных полученный набор данных присваивается свойству [`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Нажмите [`Button`](xref:Xamarin.Forms.Button), чтобы получить данные репозитория .NET из GitHub:

    [![Снимок экрана репозиториев .NET GitHub в iOS и Android](../images/consume-web-service.png)](../images/consume-web-service-large.png#lightbox)

    Дополнительные сведения об использовании веб-служб на базе REST в Xamarin.Forms см. в разделе [Использование веб-службы RESTful (руководство)](~/xamarin-forms/data-cloud/web-services/rest.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. На **панели решений** в проекте **WebServiceTutorial** дважды щелкните файл **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Get Repository Data"
                    Clicked="OnButtonClicked" />
            <CollectionView x:Name="collectionView">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <StackLayout>
                            <Label Text="{Binding Name}"
                                   FontSize="Medium" />
                            <Label Text="{Binding Description}"
                                   TextColor="Silver"
                                   FontSize="Small" />
                            <Label Text="{Binding GitHubHomeUrl}"
                                   TextColor="Gray"
                                   FontSize="Caption" />
                        </StackLayout>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    В этом коде декларативно определяется пользовательский интерфейс для страницы, который состоит из [`Button`](xref:Xamarin.Forms.Button) и [`CollectionView`](xref:Xamarin.Forms.CollectionView). `Button` устанавливает событие [`Clicked`](xref:Xamarin.Forms.Button.Clicked) для обработчика событий `OnButtonClicked`, который будет создан на следующем шаге. `CollectionView` задает значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для свойства [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate), которое определяет внешний вид каждого элемента в `CollectionView`. Дочерний элемент `DataTemplate` — [`StackLayout`](xref:Xamarin.Forms.StackLayout), содержащий три объекта [`Label`](xref:Xamarin.Forms.Label). Объекты `Label` привязывают свои свойства [`Text`](xref:Xamarin.Forms.Label.Text) к свойствам `Name`, `Description` и `GitHubHomeUrl` каждого из объектов `Repository`. Дополнительные сведения о привязке данных см. в разделе [Привязки данных в Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    Кроме того, [`CollectionView`](xref:Xamarin.Forms.CollectionView) имеет имя, указанное с помощью атрибута `x:Name`. Это позволяет файлу с выделенным кодом получать доступ к объекту с помощью назначенного имени.

1. На **панели решений** в проекте **WebServiceTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем удалите из **MainPage.xaml.cs** весь шаблонный код и замените его приведенным ниже:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                List<Repository> repositories = await _restService.GetRepositoriesAsync(Constants.GitHubReposEndpoint);
                collectionView.ItemsSource = repositories;
            }
        }
    }
    ```

    Метод `OnButtonClicked`, который выполняется при нажатии [`Button`](xref:Xamarin.Forms.Button), вызывает метод `RestService.GetRepositoriesAsync` для получения данных репозитория .NET от веб-API GitHub. Метод `GetRepositoriesAsync` требует аргумент `string`, представляющий URI вызываемого веб-API. Этот URI возвращается полем `Constants.GitHubReposEndpoint`.

    После получения запрошенных данных полученный набор данных присваивается свойству [`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource).

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Нажмите [`Button`](xref:Xamarin.Forms.Button), чтобы получить данные репозитория .NET из GitHub:

    [![Снимок экрана репозиториев .NET GitHub в iOS и Android](../images/consume-web-service.png)](../images/consume-web-service-large.png#lightbox)

    Дополнительные сведения об использовании веб-служб на базе REST в Xamarin.Forms см. в разделе [Использование веб-службы RESTful (руководство)](~/xamarin-forms/data-cloud/web-services/rest.md).
