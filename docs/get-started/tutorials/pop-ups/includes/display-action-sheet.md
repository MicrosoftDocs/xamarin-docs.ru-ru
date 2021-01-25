---
ms.openlocfilehash: 9ff398b53e88f38589733b1caac623e564e1ed39
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634842"
---

В Xamarin.Forms есть модальное всплывающее окно, известное как таблица действий, которое помогает пользователям выполнять различные задачи. В этом упражнении вы воспользуетесь методом [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) из класса [`Page`](xref:Xamarin.Forms.Page), чтобы отобразить таблицу действий, которая проводит пользователей через задачу.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** добавьте новое объявление [`Button`](xref:Xamarin.Forms.Button), которое будет отображать таблицу действий:

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

     Свойство [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывает текст, отображаемый в `Button`. Кроме того, код задает для события [`Clicked`](xref:Xamarin.Forms.Button.Clicked) обработчик событий с именем `OnDisplayActionSheetButtonClicked`, который будет создан на следующем шаге.

1. В **обозревателе решений** в проекте **PopupsTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчик событий `OnDisplayActionSheetButtonClicked` в класс:

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод `OnDisplayActionSheetButtonClicked`. Этот метод вызывает метод [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*), чтобы предоставить пользователю набор вариантов дальнейших действий в рамках задачи. После того как пользователь выберет один из вариантов, выбор возвращается в виде `string`.

    > [!IMPORTANT]
    > Метод [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) является асинхронным и всегда должен ожидаться с ключевым словом `await`.

1. На панели инструментов Visual Studio нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Затем коснитесь пункта [`Button`](xref:Xamarin.Forms.Button), добавленного в [`ContentPage`](xref:Xamarin.Forms.ContentPage):

    [![Снимок экрана: лист действий в iOS и Android](../images/actionsheet.png "Лист действий с инструкциями по задаче для пользователя")](../images/actionsheet-large.png#lightbox "Лист действий с инструкциями по задаче для пользователя")

    Обратите внимание, что после выбора альтернативы в диалоговом окне таблицы действий выбор выводится в окне **вывода** Visual Studio. Если это окно не отображается, его можно сделать видимым, выбрав пункт меню **Вид > Вывод**.

    В Visual Studio остановите приложение.

    Дополнительные сведения об отображении таблицы действий см. в разделе [с инструкциями по задачам](~/xamarin-forms/user-interface/pop-ups.md#guide-users-through-tasks) в руководстве [по отображению всплывающих окон](~/xamarin-forms/user-interface/pop-ups.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** добавьте новое объявление [`Button`](xref:Xamarin.Forms.Button), которое будет отображать таблицу действий:

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

    Свойство [`Button.Text`](xref:Xamarin.Forms.Button.Text) указывает текст, отображаемый в `Button`. Кроме того, код задает для события [`Clicked`](xref:Xamarin.Forms.Button.Clicked) обработчик событий с именем `OnDisplayActionSheetButtonClicked`, который будет создан на следующем шаге.

1. На **Панели решения** в проекте **PopupsTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчик событий `OnDisplayActionSheetButtonClicked` в класс:

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    При касании [`Button`](xref:Xamarin.Forms.Button) выполняется метод `OnDisplayActionSheetButtonClicked`. Этот метод вызывает метод [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*), чтобы предоставить пользователю набор вариантов дальнейших действий в рамках задачи. После того как пользователь выберет один из вариантов, выбор возвращается в виде `string`.

    > [!IMPORTANT]
    > Метод [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) является асинхронным и всегда должен ожидаться с ключевым словом `await`.

1. На панели инструментов Visual Studio для Mac нажмите клавишу **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android. Затем коснитесь пункта [`Button`](xref:Xamarin.Forms.Button), добавленного в [`ContentPage`](xref:Xamarin.Forms.ContentPage):

    [![Снимок экрана: лист действий в iOS и Android](../images/actionsheet.png "Лист действий с инструкциями по задаче для пользователя")](../images/actionsheet-large.png#lightbox "Лист действий с инструкциями по задаче для пользователя")

    Обратите внимание, что после выбора альтернативы в диалоговом окне таблицы действий выбор выводится в окне **вывода** Visual Studio для Mac. Если это окно не отображается, его можно сделать видимым, выбрав пункт меню **Вид > Другие окна > Вывод приложения**.

    В Visual Studio для Mac остановите приложение.

    Дополнительные сведения об отображении таблицы действий см. в разделе [с инструкциями по задачам](~/xamarin-forms/user-interface/pop-ups.md#guide-users-through-tasks) в руководстве [по отображению всплывающих окон](~/xamarin-forms/user-interface/pop-ups.md).
