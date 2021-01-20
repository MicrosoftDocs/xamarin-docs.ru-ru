---
ms.openlocfilehash: 41c1c8ae97c62a3eb2a73681b215e7687d0e473c
ms.sourcegitcommit: b75c369adb8e02a429b6c0fed8ba4a855099bf01
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2021
ms.locfileid: "98558948"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`CollectionView`](xref:Xamarin.Forms.CollectionView) таким образом, чтобы в нем устанавливалось значение `Single` для свойства [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), а также устанавливался обработчик для события [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged):

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged" />
    ```

    Этот код разрешает выбор одного элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) и задает обработчик событий с именем `OnSelectionChanged` для события [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged). Обработчик событий будет создан на следующем шаге.

1. В **обозревателе решений** в проекте **CollectionViewTutorial** разверните **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчик событий `OnSelectionChanged` в класс:

    ```csharp
    void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        Monkey selectedItem = e.CurrentSelection[0] as Monkey;
    }
    ```

    При выборе элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) запускается событие [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged), которое выполняет метод `OnSelectionChanged`. Аргумент `sender` метода является объектом `CollectionView`, отвечающим за запуск события, и может использоваться для доступа к объекту `CollectionView`. Аргумент [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) метода `OnSelectionChanged` предоставляет выбранный элемент.

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана представления CollectionView, которое реагирует на выбор элементов, в iOS и Android](../images/item-selection.png "Выбор элементов для представления CollectionView")](../images/item-selection-large.png#lightbox "Выбор элементов для представления CollectionView")

    Установите точку останова в обработчике событий `OnSelectionChanged` и выберите элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView). Проверьте значение переменной `selectedItem`, чтобы убедиться, что оно содержит данные для выбранного элемента.

    Дополнительные сведения о выборе элементов см. в статье [Выбор CollectionView в Xamarin.Forms](~/xamarin-forms/user-interface/collectionview/selection.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`CollectionView`](xref:Xamarin.Forms.CollectionView) таким образом, чтобы в нем устанавливалось значение `Single` для свойства [`SelectionMode`](xref:Xamarin.Forms.SelectableItemsView.SelectionMode), а также устанавливался обработчик для события [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged):

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged" />
    ```

    Этот код разрешает выбор одного элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) и задает обработчик событий с именем `OnSelectionChanged` для события [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged). Обработчик событий будет создан на следующем шаге.

1. На **Панели решения** в проекте **CollectionViewTutorial** разверните **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в **MainPage.xaml.cs** добавьте обработчик событий `OnSelectionChanged` в класс:

    ```csharp
    void OnSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        Monkey selectedItem = e.CurrentSelection[0] as Monkey;
    }
    ```

    При выборе элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) запускается событие [`SelectionChanged`](xref:Xamarin.Forms.SelectableItemsView.SelectionChanged), которое выполняет метод `OnSelectionChanged`. Аргумент `sender` метода является объектом `CollectionView`, отвечающим за запуск события, и может использоваться для доступа к объекту `CollectionView`. Аргумент [`SelectionChangedEventArgs`](xref:Xamarin.Forms.SelectionChangedEventArgs) метода `OnSelectionChanged` предоставляет выбранный элемент.

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана представления CollectionView, которое реагирует на выбор элементов, в iOS и Android](../images/item-selection.png "Выбор элементов для представления CollectionView")](../images/item-selection-large.png#lightbox "Выбор элементов для представления CollectionView")

    Установите точку останова в обработчике событий `OnSelectionChanged` и выберите элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView). Проверьте значение переменной `selectedItem`, чтобы убедиться, что оно содержит данные для выбранного элемента.

    Дополнительные сведения о выборе элементов см. в статье [Выбор CollectionView в Xamarin.Forms](~/xamarin-forms/user-interface/collectionview/selection.md).
