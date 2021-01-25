---
ms.openlocfilehash: cd1e8aae7755953d58439211cf485dbb99ee76ea
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98635012"
---
Подкласс [`Application`](xref:Xamarin.Forms.Application) имеет статический словарь [`Properties`](xref:Xamarin.Forms.Application.Properties), который можно использовать для хранения данных в изменениях состояния жизненного цикла. Словарь использует ключ `string` и хранит значение `object`. Словарь сохраняется на устройстве автоматически и будет заполнен повторно при перезапуске приложения.

> [!IMPORTANT]
> Словарь [`Properties`](xref:Xamarin.Forms.Application.Properties) может только сериализовать примитивные типы для хранения.

В этом упражнении вы измените приложение для сохранения текста из [`Entry`](xref:Xamarin.Forms.Entry) после фоновой обработки и восстановите текст `Entry` при перезапуске приложения.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений** в проекте **AppLifecycleTutorial** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Удалите в **App.xaml.cs** весь код шаблона и замените его приведенным ниже кодом.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    Этот код определяет свойство `DisplayText` и константу `displayText`. Когда приложение находится в фоновом режиме или завершает работу, метод `OnSleep` переопределяет значение свойства `DisplayText` словарю [`Properties`](xref:Xamarin.Forms.Application.Properties) ключом `displayText`. Затем при запуске приложения, при условии, что словарь `Properties` содержит ключ `displayText`, значение ключа восстанавливается в свойство `DisplayText`.

    > [!IMPORTANT]
    > Всегда проверяйте словарь [`Properties`](xref:Xamarin.Forms.Application.Properties) на наличие ключа, прежде чем получить доступ к нему, чтобы предотвратить непредвиденные ошибки.

    Нет необходимости восстанавливать данные из словаря [`Properties`](xref:Xamarin.Forms.Application.Properties) в перегрузке метода `OnResume`. Это обусловлено тем, что когда приложение выполняется в фоновом режиме, оно и его состояние по-прежнему находятся в памяти.

1. В **обозревателе решений** в проекте **AppLifecycleTutorial** дважды щелкните файл **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Entry`](xref:Xamarin.Forms.Entry) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Entry.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) определяет текст заполнителя, который отображается, когда первым отображается `Entry`, а обработчик событий `OnEntryCompleted` зарегистрирован с помощью события [`Completed`](xref:Xamarin.Forms.Entry.Completed). Кроме того, `Entry` имеет имя, указанное с помощью атрибута `x:Name`. Это позволяет файлу с выделенным кодом получать доступ к объекту `Entry` с помощью назначенного имени.

1. В **обозревателе решений** в проекте **AppLifecycleTutorial** разверните узел **MainPage.xaml** и дважды щелкните **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте к классу переопределение для метода `OnAppearing` и обработчика событий `OnEntryCompleted`.

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    Метод `OnAppearing` возвращает значение свойства `App.DisplayText` и задает его как значение свойства [`Text`](xref:Xamarin.Forms.InputView.Text)[`Entry`](xref:Xamarin.Forms.Entry).

    > [!NOTE]
    > Переопределение метода `OnAppearing` выполняется после готовности [`ContentPage`](xref:Xamarin.Forms.ContentPage), но только прежде чем он станет видимым. Таким образом лучше всего задать содержимое представлений Xamarin.Forms.

    Когда текст в [`Entry`](xref:Xamarin.Forms.Entry) завершен с помощью ключа возврата, в свойстве `App.DisplayText`выполняется метод `OnEntryCompleted` и хранится текст `Entry`.

1. На панели инструментов Visual Studio нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android.

    Введите любой текст в [`Entry`](xref:Xamarin.Forms.Entry) и нажмите клавишу возврата. Затем выполните приложение в фоновом режиме, нажав кнопку "Домашняя страница", чтобы вызвать метод `OnSleep`.

    В Visual Studio остановите приложение и запустите его еще раз, после чего восстановится ранее введенный текст в [`Entry`](xref:Xamarin.Forms.Entry):

    [![Снимок экрана: запись, свойство текста которой сохраняется при изменении состояния жизненного цикла, в iOS и Android](../images/persist-data.png "Запись, свойство текста которой сохраняется при изменении состояния жизненного цикла")](../images/persist-data-large.png#lightbox "Запись, свойство текста которой сохраняется при изменении состояния жизненного цикла")

    В Visual Studio остановите приложение.

    Дополнительные сведения о сохранении данных в словарь Properties см. в разделе [Словарь Properties](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary) в руководстве [Класс App Xamarin.Forms](~/xamarin-forms/app-fundamentals/application-class.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. На **Панели решения** в проекте **AppLifecycleTutorial** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Удалите в **App.xaml.cs** весь код шаблона и замените его приведенным ниже кодом.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    Этот код определяет свойство `DisplayText` и константу `displayText`. Когда приложение находится в фоновом режиме или завершает работу, метод `OnSleep` переопределяет значение свойства `DisplayText` словарю [`Properties`](xref:Xamarin.Forms.Application.Properties) ключом `displayText`. Затем при запуске приложения, при условии, что словарь `Properties` содержит ключ `displayText`, значение ключа восстанавливается в свойство `DisplayText`.

    > [!IMPORTANT]
    > Всегда проверяйте словарь [`Properties`](xref:Xamarin.Forms.Application.Properties) на наличие ключа, прежде чем получить доступ к нему, чтобы предотвратить непредвиденные ошибки.

    Нет необходимости восстанавливать данные из словаря [`Properties`](xref:Xamarin.Forms.Application.Properties) в перегрузке метода `OnResume`. Это обусловлено тем, что когда приложение выполняется в фоновом режиме, оно и его состояние по-прежнему находятся в памяти.

1. На **Панели решения** в проекте **AppLifecycleTutorial** дважды щелкните **MainPage.xaml**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из [`Entry`](xref:Xamarin.Forms.Entry) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойство [`Entry.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) определяет текст заполнителя, который отображается, когда первым отображается `Entry`, а обработчик событий `OnEntryCompleted` зарегистрирован с помощью события [`Completed`](xref:Xamarin.Forms.Entry.Completed). Кроме того, `Entry` имеет имя, указанное с помощью атрибута `x:Name`. Это позволяет файлу с выделенным кодом получать доступ к объекту `Entry` с помощью назначенного имени.

1. На **Панели решения** в проекте **AppLifecycleTutorial** разверните узел **MainPage.xaml** и дважды щелкните **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте к классу переопределение для метода `OnAppearing` и обработчика событий `OnEntryCompleted`.

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    Метод `OnAppearing` возвращает значение свойства `App.DisplayText` и задает его как значение свойства [`Text`](xref:Xamarin.Forms.InputView.Text)[`Entry`](xref:Xamarin.Forms.Entry).

    > [!NOTE]
    > Переопределение метода `OnAppearing` выполняется после готовности [`ContentPage`](xref:Xamarin.Forms.ContentPage), но только прежде чем он станет видимым. Таким образом лучше всего задать содержимое представлений Xamarin.Forms.

    Когда текст в [`Entry`](xref:Xamarin.Forms.Entry) завершен с помощью ключа возврата, в свойстве `App.DisplayText`выполняется метод `OnEntryCompleted` и хранится текст `Entry`.

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    Введите любой текст в [`Entry`](xref:Xamarin.Forms.Entry) и нажмите клавишу возврата. Затем выполните приложение в фоновом режиме, нажав кнопку "Домашняя страница", чтобы вызвать метод `OnSleep`.

    В Visual Studio для Mac остановите приложение и запустите его еще раз, после чего восстановится ранее введенный текст в [`Entry`](xref:Xamarin.Forms.Entry):

    [![Снимок экрана: запись, свойство текста которой сохраняется при изменении состояния жизненного цикла, в iOS и Android](../images/persist-data.png "Запись, свойство текста которой сохраняется при изменении состояния жизненного цикла")](../images/persist-data-large.png#lightbox "Запись, свойство текста которой сохраняется при изменении состояния жизненного цикла")

    В Visual Studio для Mac остановите приложение.

    Дополнительные сведения о сохранении данных в словарь Properties см. в разделе [Словарь Properties](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary) в руководстве [Класс App Xamarin.Forms](~/xamarin-forms/app-fundamentals/application-class.md).
