---
ms.openlocfilehash: 8b40b710b38a82c680f2da86f4d89b34b9ef00e4
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98690056"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Label`](xref:Xamarin.Forms.Label), чтобы изменить его внешний вид:

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    Этот код задает свойства, которые изменяют внешний вид [`Label`](xref:Xamarin.Forms.Label). Свойство [`TextColor`](xref:Xamarin.Forms.Label.TextColor) задает цвет текста `Label`. Свойство [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) используется для применения курсива, а свойство [`FontSize`](xref:Xamarin.Forms.Label.FontSize) задает размер шрифта. Кроме того, подчеркивание текста применяется к `Label`, при применении свойства [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations), а чтобы центрировать его по горизонтали, необходимо придать свойству [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) значение [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. Обратите внимание, что внешний вид [`Label`](xref:Xamarin.Forms.Label) изменился:

    [![Снимок экрана: метка с измененным внешним видом в iOS и Android](../images/change-label-appearance.png "Метка с измененным видом")](../images/change-label-appearance-large.png#lightbox "Метка с измененным видом")

    Дополнительные сведения о внешнем виде параметра [`Label`](xref:Xamarin.Forms.Label) см. в руководстве [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Label`](xref:Xamarin.Forms.Label), чтобы изменить его внешний вид:

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    Этот код задает свойства, которые изменяют внешний вид [`Label`](xref:Xamarin.Forms.Label). Свойство [`TextColor`](xref:Xamarin.Forms.Label.TextColor) задает цвет текста `Label`. Свойство [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) используется для применения курсива, а свойство [`FontSize`](xref:Xamarin.Forms.Label.FontSize) задает размер шрифта. Кроме того, подчеркивание текста применяется к `Label`, при применении свойства [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations), а чтобы центрировать его по горизонтали, необходимо придать свойству [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) значение [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio для Mac кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном симуляторе iOS или эмуляторе Android. Обратите внимание, что внешний вид [`Label`](xref:Xamarin.Forms.Label) изменился:

    [![Снимок экрана: метка с измененным внешним видом в iOS и Android](../images/change-label-appearance.png "Метка с измененным видом")](../images/change-label-appearance-large.png#lightbox "Метка с измененным видом")

    Дополнительные сведения о внешнем виде параметра [`Label`](xref:Xamarin.Forms.Label) см. в руководстве [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md).
