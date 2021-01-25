---
ms.openlocfilehash: d32dd7d641861f1a95ff35dbbf89a7671594e873
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634843"
---
В Xamarin.Forms есть модальный всплывающий элемент (оповещение), позволяющий выводить предупреждения или задавать простые вопросы пользователю. В этом упражнении вы воспользуетесь методом [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) из класса [`Page`](xref:Xamarin.Forms.Page), чтобы выводить оповещения для пользователя, а также задать простой вопрос.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Для работы с этим руководством у вас должен быть последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**. Кроме того, вам потребуется компьютер Mac для сборки учебного приложения на iOS. Сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **PopupsTutorial**.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **PopupsTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** дважды щелкните файл **MainPage.xaml** в проекте **PopupsTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из двух объектов [`Button`](xref:Xamarin.Forms.Button) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойства [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывают текст, отображаемый в каждом `Button`, а события [`Clicked`](xref:Xamarin.Forms.Button.Clicked) связаны с обработчиками событий, которые будут созданы в следующем шаге.

1. В **обозревателе решений** в проекте **PopupsTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в файле **MainPage.xaml.cs** добавьте обработчики событий `OnDisplayAlertButtonClicked` и `OnDisplayAlertQuestionButtonClicked` в класс.

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод обработчика соответствующего события. Метод `OnDisplayAlertButtonClicked` вызывает метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) для отображения модального оповещения с одной кнопкой "Отмена". После закрытия оповещения пользователь может продолжить работу с приложением.

    Метод `OnDisplayAlertQuestionButtonClicked` вызывает перегрузку метода [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) для отображения модального оповещения с кнопками "Принять" и "Отмена". После того как пользователь нажмет одну из кнопок, выбор возвращается в виде `boolean`.

    > [!IMPORTANT]
    > Метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) является асинхронным и всегда должен ожидаться с ключевым словом `await`.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Выберите первый [`Button`](xref:Xamarin.Forms.Button):

    [![Снимок экрана: оповещение в iOS и Android](../images/alert.png "Предупреждение")](../images/alert-large.png#lightbox "Предупреждение")

    После отклонения оповещения коснитесь второго [`Button`](xref:Xamarin.Forms.Button):

    [![Снимок экрана: оповещение с вопросом в iOS и Android](../images/alert-question.png "Оповещение с вопросом")](../images/alert-question-large.png#lightbox "Оповещение с вопросом")

    Обратите внимание, что после выбора ответа на вопрос он поступает в окно **вывода** Visual Studio. Если это окно не отображается, его можно сделать видимым, выбрав пункт меню **Вид > Вывод**.

    В Visual Studio остановите приложение.

    Дополнительные сведения об отображении оповещений см. в разделе [о выводе оповещения](~/xamarin-forms/user-interface/pop-ups.md#display-an-alert) в руководстве [по отображению всплывающих окон](~/xamarin-forms/user-interface/pop-ups.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

Для работы с этим руководством вам нужно установить Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS. Кроме того, вам потребуется Xcode (последний выпуск). Дополнительные сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md).

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **PopupsTutorial**.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **PopupsTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решения** дважды щелкните файл **MainPage.xaml** в проекте **PopupsTutorial**, чтобы открыть его. В **MainPage.xaml** удалите весь код шаблона и замените его приведенным ниже кодом.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    Этот код декларативно определяет пользовательский интерфейс для страницы, который состоит из двух объектов [`Button`](xref:Xamarin.Forms.Button) в [`StackLayout`](xref:Xamarin.Forms.StackLayout). Свойства [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывают текст, отображаемый в каждом `Button`, а события [`Clicked`](xref:Xamarin.Forms.Button.Clicked) связаны с обработчиками событий, которые будут созданы в следующем шаге.

1. На **Панели решения** в проекте **PopupsTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчики событий `OnDisplayAlertButtonClicked` и `OnDisplayAlertQuestionButtonClicked` в класс.

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод обработчика соответствующего события. Метод `OnDisplayAlertButtonClicked` вызывает метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) для отображения модального оповещения с одной кнопкой "Отмена". После закрытия оповещения пользователь может продолжить работу с приложением.

    Метод `OnDisplayAlertQuestionButtonClicked` вызывает перегрузку метода [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) для отображения модального оповещения с кнопками "Принять" и "Отмена". После того как пользователь нажмет одну из кнопок, выбор возвращается в виде `boolean`.

    > [!IMPORTANT]
    > Метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) является асинхронным и всегда должен ожидаться с ключевым словом `await`.

1. Чтобы запустить приложения в выбранном симуляторе iOS или эмуляторе Android, нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) на панели инструментов Visual Studio для Mac. Выберите первый [`Button`](xref:Xamarin.Forms.Button):

    [![Снимок экрана: оповещение в iOS и Android](../images/alert.png "Предупреждение")](../images/alert-large.png#lightbox "Предупреждение")

    После отклонения оповещения коснитесь второго [`Button`](xref:Xamarin.Forms.Button):

    [![Снимок экрана: оповещение с вопросом в iOS и Android](../images/alert-question.png "Оповещение с вопросом")](../images/alert-question-large.png#lightbox "Оповещение с вопросом")

    Обратите внимание, что после выбора ответа на вопрос он поступает в окно **вывода** Visual Studio для Mac. Если это окно не отображается, его можно сделать видимым, выбрав пункт меню **Вид > Другие окна > Вывод приложения**.

    В Visual Studio для Mac остановите приложение.

    Дополнительные сведения об отображении оповещений см. в разделе [о выводе оповещения](~/xamarin-forms/user-interface/pop-ups.md#display-an-alert) в руководстве [по отображению всплывающих окон](~/xamarin-forms/user-interface/pop-ups.md).
