---
ms.openlocfilehash: f205e97c11c6ae1c75b101ada57c10ef49a9c146
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634783"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`StackLayout`](xref:Xamarin.Forms.StackLayout), чтобы выровнять его дочерние элементы по горизонтали, а не по вертикали:

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    В этом коде свойству [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) присваивается значение [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal).

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана: дочерние представления с горизонтальной ориентацией в StackLayout в iOS и Android](../images/orientation.png "StackLayout, содержащий горизонтально ориентированные экземпляры меток")](../images/orientation-large.png#lightbox "StackLayout, содержащий горизонтально ориентированные экземпляры меток")

    Обратите внимание, что экземпляры [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout) теперь выровнены по горизонтали, а не по вертикали.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`StackLayout`](xref:Xamarin.Forms.StackLayout), чтобы выровнять его дочерние элементы по горизонтали, а не по вертикали:

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    В этом коде свойству [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) присваивается значение [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal).

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio для Mac кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана: дочерние представления с горизонтальной ориентацией в StackLayout в iOS и Android](../images/orientation.png "StackLayout, содержащий горизонтально ориентированные экземпляры меток")](../images/orientation-large.png#lightbox "StackLayout, содержащий горизонтально ориентированные экземпляры меток")

    Обратите внимание, что экземпляры [`Label`](xref:Xamarin.Forms.Label) в [`StackLayout`](xref:Xamarin.Forms.StackLayout) теперь выровнены по горизонтали, а не по вертикали.
