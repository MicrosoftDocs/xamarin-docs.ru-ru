---
ms.openlocfilehash: 6118d4ee8d2fa56a3b14d3d280db4991a36874fc
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98689746"
---
Ранее мы заполнили [`CollectionView`](xref:Xamarin.Forms.CollectionView) данными с помощью привязки данных. Однако несмотря на привязку данных к коллекции, где каждый объект в коллекции определял несколько элементов данных, только один элемент данных отображался для каждого объекта (свойство `Name` объекта `Monkey`).

В этом упражнении вы измените проект **CollectionViewTutorial**, чтобы в представлении [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображалось несколько элементов данных в каждой строке.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`CollectionView`](xref:Xamarin.Forms.CollectionView), чтобы настроить внешний вид каждого элемента данных:

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10"
                      RowDefinitions="Auto, *"
                      ColumnDefinitions="Auto, *">
                    <Image Grid.RowSpan="2"
                           Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Name}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Location}"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
    ```

    Этот код задает значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для свойства [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate), которое определяет внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView). Дочерний элемент `DataTemplate` — [`Grid`](xref:Xamarin.Forms.Grid), содержащий объект [`Image`](xref:Xamarin.Forms.Image) и два объекта [`Label`](xref:Xamarin.Forms.Label). Объект `Image` привязывает свое свойство [`Source`](xref:Xamarin.Forms.Image.Source) к свойству `ImageUrl` каждого объекта `Monkey`, а объекты `Label` привязывают свои свойства [`Text`](xref:Xamarin.Forms.Label.Text) к свойствам `Name` и `Location` каждого объекта `Monkey`.

    Дополнительные сведения о внешнем виде элемента [`CollectionView`](xref:Xamarin.Forms.CollectionView) см. в разделе [Определение внешнего вида элемента](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance). Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана: представление CollectionView, элементы которого воспроизводятся по шаблону с помощью шаблона данных](../images/customize-item-appearance.png "Представление CollectionView, отображающее шаблонные данные")](../images/customize-item-appearance-large.png#lightbox "Представление CollectionView, отображающее шаблонные данные")

    В Visual Studio остановите приложение.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`CollectionView`](xref:Xamarin.Forms.CollectionView), чтобы настроить внешний вид каждого элемента данных:

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10"
                      RowDefinitions="Auto, *"
                      ColumnDefinitions="Auto, *">
                    <Image Grid.RowSpan="2"
                           Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Name}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Location}"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
    ```

    Этот код задает значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) для свойства [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate), которое определяет внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView). Дочерний элемент `DataTemplate` — [`Grid`](xref:Xamarin.Forms.Grid), содержащий объект [`Image`](xref:Xamarin.Forms.Image) и два объекта [`Label`](xref:Xamarin.Forms.Label). Объект `Image` привязывает свое свойство [`Source`](xref:Xamarin.Forms.Image.Source) к свойству `ImageUrl` каждого объекта `Monkey`, а объекты `Label` привязывают свои свойства [`Text`](xref:Xamarin.Forms.Label.Text) к свойствам `Name` и `Location` каждого объекта `Monkey`.

    Дополнительные сведения о внешнем виде элемента [`CollectionView`](xref:Xamarin.Forms.CollectionView) см. в разделе [Определение внешнего вида элемента](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance). Дополнительные сведения о шаблонах данных см. в разделе [Шаблоны данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана: представление CollectionView, элементы которого воспроизводятся по шаблону с помощью шаблона данных](../images/customize-item-appearance.png "Представление CollectionView, отображающее шаблонные данные")](../images/customize-item-appearance-large.png#lightbox "Представление CollectionView, отображающее шаблонные данные")

    В Visual Studio для Mac остановите приложение.
