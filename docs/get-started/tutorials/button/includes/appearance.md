---
ms.openlocfilehash: 703d99446d3a43411b681677bf9d3c9ab01ea83f
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98689652"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Button`](xref:Xamarin.Forms.Button), чтобы изменить его внешний вид:

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked"
            TextColor="Blue"
            BackgroundColor="Aqua"
            BorderColor="Red"
            BorderWidth="5"
            CornerRadius="5"
            WidthRequest="150"
            HeightRequest="75" />
    ```

    Этот код задает свойства, которые изменяют внешний вид [`Button`](xref:Xamarin.Forms.Button). Свойство [`TextColor`](xref:Xamarin.Forms.Button.TextColor) задает цвет текста `Button`, а свойство [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) задает цвет фона для текста. Свойство [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) задает цвет области, окружающей `Button`, а свойство [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) задает ширину границы. По умолчанию `Button` представляет собой прямоугольник, но его можно представить со скругленными углами, установив для свойства [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) подходящее значение. Кроме того, размер `Button` можно изменить, настроив его свойства [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest).

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Обратите внимание, что внешний вид [`Button`](xref:Xamarin.Forms.Button) изменился:

    [![Снимок экрана: кнопка с измененным внешним видом в iOS и Android](../images/change-button-appearance.png "Кнопка с измененным видом")](../images/change-button-appearance-large.png#lightbox "Кнопка с измененным видом")

    В Visual Studio остановите приложение.

    Дополнительные сведения о настройке внешнего вида [`Button`](xref:Xamarin.Forms.Button) см. в разделе [Внешний вид кнопки](~/xamarin-forms/user-interface/button.md#button-appearance) в руководстве по [кнопкам в Xamarin.Forms](~/xamarin-forms/user-interface/button.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Button`](xref:Xamarin.Forms.Button), чтобы изменить его внешний вид:

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked"
            TextColor="Blue"
            BackgroundColor="Aqua"
            BorderColor="Red"
            BorderWidth="5"
            CornerRadius="5"
            WidthRequest="150"
            HeightRequest="75" />
    ```

    Этот код задает свойства, которые изменяют внешний вид [`Button`](xref:Xamarin.Forms.Button). Свойство [`TextColor`](xref:Xamarin.Forms.Button.TextColor) задает цвет текста `Button`, а свойство [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) задает цвет фона для текста. Свойство [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) задает цвет области, окружающей `Button`, а свойство [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) задает ширину границы. По умолчанию `Button` представляет собой прямоугольник, но его можно представить со скругленными углами, установив для свойства [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) подходящее значение. Кроме того, размер `Button` можно изменить, настроив его свойства [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest).

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio для Mac кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном симуляторе iOS или эмуляторе Android. Обратите внимание, что внешний вид [`Button`](xref:Xamarin.Forms.Button) изменился:

    [![Снимок экрана: кнопка с измененным внешним видом в iOS и Android](../images/change-button-appearance.png "Кнопка с измененным видом")](../images/change-button-appearance-large.png#lightbox "Кнопка с измененным видом")

    В Visual Studio для Mac остановите приложение.

    Дополнительные сведения о настройке внешнего вида [`Button`](xref:Xamarin.Forms.Button) см. в разделе [Внешний вид кнопки](~/xamarin-forms/user-interface/button.md#button-appearance) в руководстве по [кнопкам в Xamarin.Forms](~/xamarin-forms/user-interface/button.md).
