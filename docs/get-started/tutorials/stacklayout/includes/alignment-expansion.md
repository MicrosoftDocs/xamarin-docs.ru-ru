---
ms.openlocfilehash: f9bacad9732a47a143aadb3efa9096b51a22f97a
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634803"
---
Размер и положение дочерних представлений в [`StackLayout`](xref:Xamarin.Forms.StackLayout) зависят от значений свойств [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) и [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) дочерних представлений и значений свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions).

Свойства [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) могут задаваться для полей из структуры [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions), которая инкапсулирует две настройки макета:

- **Выравнивание** — настройки выравнивания дочернего представления, которые определяют его положение и размер в рамках родительского макета.
- **Расширение** — указывает, должно ли дочернее представление использовать дополнительное пространство, если оно доступно (используется только [`StackLayout`](xref:Xamarin.Forms.StackLayout)).

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`StackLayout`](xref:Xamarin.Forms.StackLayout), чтобы задать параметры выравнивания и расширения для каждого [`Label`](xref:Xamarin.Forms.Label):

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    Этот код задает параметры выравнивания для первых четырех экземпляров [`Label`](xref:Xamarin.Forms.Label) и параметры расширения для последних четырех экземпляров `Label`. Поля [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), [`End`](xref:Xamarin.Forms.LayoutOptions.End) и [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) используются для определения выравнивания экземпляров [`Label`](xref:Xamarin.Forms.Label) в родительском элементе [`StackLayout`](xref:Xamarin.Forms.StackLayout). Поля [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand), [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand), [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) и [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) используются для определения параметров выравнивания и указывают, будет ли представление занимать больше места, если оно доступно в родительском элементе `StackLayout`.

    > [!NOTE]
    > Значение по умолчанию свойств представления [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) — [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill).

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана: дочерние представления в StackLayout с заданными параметрами выравнивания и расширения в iOS и Android](../images/alignment-expansion.png "StackLayout, содержащий экземпляры меток, с заданным выравниванием и расширением")](../images/alignment-expansion-large.png#lightbox "StackLayout, содержащий экземпляры меток, с заданным выравниванием и расширением")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) учитывает только параметры выравнивания дочерних представлениях, которые находятся в направлении, противоположном ориентации `StackLayout`. Поэтому дочерние представления [`Label`](xref:Xamarin.Forms.Label) в `StackLayout` с вертикальной ориентацией получают свойства [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) в соответствии с одним из следующих полей выравнивания:

    - [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) — располагает [`Label`](xref:Xamarin.Forms.Label) в левой части [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) — располагает [`Label`](xref:Xamarin.Forms.Label) в центре [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`End`](xref:Xamarin.Forms.LayoutOptions.End) — располагает [`Label`](xref:Xamarin.Forms.Label) в правой части [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) — гарантирует, что [`Label`](xref:Xamarin.Forms.Label) заполняет ширину [`StackLayout`](xref:Xamarin.Forms.StackLayout).

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) может развернуть дочерние представления только в направлении своей ориентации. Поэтому `StackLayout` в вертикальной ориентации может развернуть дочерние представления [`Label`](xref:Xamarin.Forms.Label), которые задают свои свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) одному из полей выравнивания. Это означает, что для вертикального выравнивания каждый `Label` занимает одинаковый объем пространства в `StackLayout`. Но только последний `Label` со значением свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions), равным [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand), имеет другой размер.

    > [!IMPORTANT]
    > Когда все пространство в [`StackLayout`](xref:Xamarin.Forms.StackLayout) занято, параметр расширения ни на что не влияет.

    В Visual Studio остановите приложение.

    Дополнительные сведения о выравнивании и расширении см. в разделе [Параметры макета в Xamarin.Forms](~/xamarin-forms/user-interface/layouts/layout-options.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`StackLayout`](xref:Xamarin.Forms.StackLayout), чтобы задать параметры выравнивания и расширения для каждого [`Label`](xref:Xamarin.Forms.Label):

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    Этот код задает параметры выравнивания для первых четырех экземпляров [`Label`](xref:Xamarin.Forms.Label) и параметры расширения для последних четырех экземпляров `Label`. Поля [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), [`End`](xref:Xamarin.Forms.LayoutOptions.End) и [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) используются для определения выравнивания экземпляров [`Label`](xref:Xamarin.Forms.Label) в родительском элементе [`StackLayout`](xref:Xamarin.Forms.StackLayout). Поля [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand), [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand), [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) и [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) используются для определения параметров выравнивания и указывают, будет ли представление занимать больше места, если оно доступно в родительском элементе `StackLayout`.

    > [!NOTE]
    > Значение по умолчанию свойств представления [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) — [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill).

1. Если приложение по-прежнему работает, сохраните изменения в файле, после чего пользовательский интерфейс приложения в симуляторе или эмуляторе будет автоматически обновлен. В противном случае нажмите на панели инструментов Visual Studio для Mac кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном симуляторе iOS или эмуляторе Android:

    [![Снимок экрана: дочерние представления в StackLayout с заданными параметрами выравнивания и расширения в iOS и Android](../images/alignment-expansion.png "StackLayout, содержащий экземпляры меток, с заданным выравниванием и расширением")](../images/alignment-expansion-large.png#lightbox "StackLayout, содержащий экземпляры меток, с заданным выравниванием и расширением")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) учитывает только параметры выравнивания дочерних представлениях, которые находятся в направлении, противоположном ориентации `StackLayout`. Поэтому дочерние представления [`Label`](xref:Xamarin.Forms.Label) в `StackLayout` с вертикальной ориентацией получают свойства [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) в соответствии с одним из следующих полей выравнивания:

    - [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) — располагает [`Label`](xref:Xamarin.Forms.Label) в левой части [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) — располагает [`Label`](xref:Xamarin.Forms.Label) в центре [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`End`](xref:Xamarin.Forms.LayoutOptions.End) — располагает [`Label`](xref:Xamarin.Forms.Label) в правой части [`StackLayout`](xref:Xamarin.Forms.StackLayout).
    - [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) — гарантирует, что [`Label`](xref:Xamarin.Forms.Label) заполняет ширину [`StackLayout`](xref:Xamarin.Forms.StackLayout).

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) может развернуть дочерние представления только в направлении своей ориентации. Поэтому `StackLayout` в вертикальной ориентации может развернуть дочерние представления [`Label`](xref:Xamarin.Forms.Label), которые задают свои свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) одному из полей выравнивания. Это означает, что для вертикального выравнивания каждый `Label` занимает одинаковый объем пространства в `StackLayout`. Но только последний `Label` со значением свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions), равным [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand), имеет другой размер.

    > [!IMPORTANT]
    > Когда все пространство в [`StackLayout`](xref:Xamarin.Forms.StackLayout) занято, параметр расширения ни на что не влияет.

    В Visual Studio для Mac остановите приложение.

    Дополнительные сведения о выравнивании и расширении см. в разделе [Параметры макета в Xamarin.Forms](~/xamarin-forms/user-interface/layouts/layout-options.md).
