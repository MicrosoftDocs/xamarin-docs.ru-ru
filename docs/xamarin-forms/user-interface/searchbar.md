---
title: Xamarin.Forms сеарчбар
description: Xamarin.FormsСеарчбар — это элемент управления вводом пользователя, который используется для запуска поиска. Элемент управления Сеарчбар поддерживает текст заполнителя, ввод запроса, выполнение и отмену. В этой статье объясняется, как использовать Сеарчбар в XAML и коде.
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 66ffbe0f45754517610a2fc2858a00a6185e1d45
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93369133"
---
# <a name="xamarinforms-searchbar"></a>Xamarin.Forms сеарчбар

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

Xamarin.Forms [`SearchBar`](xref:Xamarin.Forms.SearchBar) — Это элемент управления вводом пользователя, используемый для запуска поиска. `SearchBar`Элемент управления поддерживает текст заполнителя, ввод запроса, выполнение поиска и отмену. На следующем снимке экрана показан `SearchBar` запрос с результатами, отображаемыми в `ListView` :

[![Снимок экрана Сеарчбар в iOS и Android](searchbar-images/device-searchbars-cropped.png "Сеарчбар в iOS и Android")](searchbar-images/device-searchbars.png#lightbox "Сеарчбар в iOS и Android")

Класс `SearchBar` определяет следующие свойства:

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) значение типа `Color` , определяющее цвет кнопки отмены.
* `CharacterSpacing` с типом `double` представляет собой интервал между знаками текста `SearchBar`.
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes)`FontAttributes`значение перечисления, которое определяет, является ли `SearchBar` шрифт полужирным, курсивом или ни тем ни другого.
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily) значение типа `string` , определяющее семейство шрифтов, используемое `SearchBar` .
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize) может быть `NamedSize` значением перечисления или `double` значением, представляющим определенные размеры шрифтов на разных платформах.
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment)`TextAlignment`значение перечисления, определяющее выравнивание текста запроса по горизонтали.
* `VerticalTextAlignment``TextAlignment`значение перечисления, определяющее вертикальное выравнивание текста запроса.
* [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) значение `string` , определяющее текст заполнителя, например "Поиск...".
* [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) значение типа `Color` , определяющее цвет текста заполнителя.
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) — Это `ICommand` , которое позволяет привязать пользовательские действия, такие как касания пальца или щелчки, к командам, определенным в ViewModel.
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) Аргумент `object` , указывающий параметр, который должен быть передан в `SearchCommand` .
* [`Text`](xref:Xamarin.Forms.InputView.Text) Объект, `string` содержащий текст запроса в `SearchBar` .
* [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) значение типа `Color` , определяющее цвет текста запроса.
* `TextTransform``TextTransform`значение, определяющее регистр `SearchBar` текста.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает `SearchBar` возможность настройки и назначения привязок данных. Указание свойств шрифта в `SearchBar` согласуется с настройкой текста в других [ Xamarin.Forms текстовых элементах управления](~/xamarin-forms/user-interface/text/index.md). Дополнительные сведения см. [в разделе Шрифты Xamarin.Forms в ](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="create-a-searchbar"></a>Создание Сеарчбар

`SearchBar`Экземпляр можно создать в XAML. Его необязательное `Placeholder` свойство можно задать для определения текста подсказки в поле ввода запроса. Значение по умолчанию для параметра `Placeholder` — пустая строка, поэтому не отображается заполнитель, если он не задан. В следующем примере показано, как создать экземпляр `SearchBar` в XAML с помощью необязательного `Placeholder` набора свойств:

```xaml
<SearchBar Placeholder="Search items..." />
```

`SearchBar`Также можно создать в коде:

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>Свойства внешнего вида Сеарчбар

`SearchBar`Элемент управления определяет множество свойств, которые настраивают внешний вид элемента управления. В следующем примере показано, как создать экземпляр `SearchBar` в XAML с несколькими указанными свойствами:

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           TextTransform="Lowercase"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

Эти свойства также можно указать при создании `SearchBar` объекта в коде:

```csharp
SearchBar searchBar = new SearchBar
{
    Placeholder = "Search items...",
    PlaceholderColor = Color.Orange,
    TextColor = Color.Orange,
    TextTransform = TextTransform.Lowercase,
    HorizontalTextAlignment = TextAlignment.Center,
    FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(SearchBar)),
    FontAttributes = FontAttributes.Italic
};
```

На следующем снимке экрана показан итоговый `SearchBar` элемент управления:

[![Снимок экрана настраиваемого Сеарчбар в iOS и Android](searchbar-images/device-searchbars-styled-cropped.png "Настраиваемые Сеарчбар в iOS и Android")](searchbar-images/device-searchbars-styled.png#lightbox "Настраиваемые Сеарчбар в iOS и Android")

> [!NOTE]
> В iOS `SearchBarRenderer` класс содержит переопределяемый `UpdateCancelButton` метод. Этот метод управляет тем, когда появляется кнопка Отмена, и может быть переопределена в пользовательском модуле подготовки отчетов. Дополнительные сведения о пользовательских модулях подготовки отчетов см. в разделе [ Xamarin.Forms пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов.

## <a name="perform-a-search-with-event-handlers"></a>Выполнение поиска с помощью обработчиков событий

Поиск можно выполнить с помощью `SearchBar` элемента управления, присоединив обработчик событий к одному из следующих событий:

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) вызывается, когда пользователь либо нажимает кнопку поиска, либо нажимает клавишу ВВОД.
* [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) вызывается всякий раз, когда изменяется текст в поле запроса.

В следующем примере показан обработчик событий, присоединенный к `TextChanged` событию в XAML и использующий `ListView` для отображения результатов поиска:

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

Обработчик событий можно также присоединить к `SearchBar` созданному в коде:

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

`TextChanged`Обработчик событий в файле кода программной части одинаков, создается ли объект с `SearchBar` помощью XAML или кода:

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

В предыдущем примере подразумевается существование `DataService` класса с `GetSearchResults` методом, способным возвращать элементы, соответствующие запросу. `SearchBar` `Text` Значение свойства элемента управления передается в `GetSearchResults` метод, а результат используется для обновления `ListView` свойства элемента управления `ItemsSource` . Общий результат заключается в том, что результаты поиска отображаются в `ListView` элементе управления.

Пример приложения предоставляет `DataService` реализацию класса, которую можно использовать для тестирования функций поиска.

## <a name="perform-a-search-using-a-viewmodel"></a>Выполнение поиска с помощью ViewModel

Поиск может выполняться без обработчиков событий путем привязки `SearchCommand` `SearchCommandParameter` свойств и к `ICommand` реализациям. Пример проекта демонстрирует эти реализации с помощью шаблона Model-View-ViewModel (MVVM). Дополнительные сведения о привязках данных с помощью MVVM см. [в разделе привязки данных с помощью MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

В примере приложения ViewModel содержит следующий код:

```csharp
public class SearchViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void NotifyPropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public ICommand PerformSearch => new Command<string>((string query) =>
    {
        SearchResults = DataService.GetSearchResults(query);
    });

    private List<string> searchResults = DataService.Fruits;
    public List<string> SearchResults
    {
        get
        {
            return searchResults;
        }
        set
        {
            searchResults = value;
            NotifyPropertyChanged();
        }
    }
}
```

> [!NOTE]
> ViewModel предполагает наличие `DataService` класса, способного выполнять поиск. `DataService`Класс, включая пример данных, доступен в примере приложения.

В следующем коде XAML показано, как привязать `SearchBar` к примеру ViewModel, с `ListView` элементом управления, отображающим результаты поиска:

```xaml
<ContentPage ...>
    <ContentPage.BindingContext>
        <viewmodels:SearchViewModel />
    </ContentPage.BindingContext>
    <StackLayout ...>
        <SearchBar x:Name="searchBar"
                   ...
                   SearchCommand="{Binding PerformSearch}"
                   SearchCommandParameter="{Binding Text, Source={x:Reference searchBar}}"/>
        <ListView x:Name="searchResults"
                  ...
                  ItemsSource="{Binding SearchResults}" />
    </StackLayout>
</ContentPage>
```

В этом примере задается как `BindingContext` экземпляр `SearchViewModel` класса. Он привязывает `SearchCommand` свойство к в `PerformSearch` `ICommand` ViewModel и привязывает `SearchBar` `Text` свойство к `SearchCommandParameter` свойству. `ListView.ItemsSource`Свойство привязано к `SearchResults` свойству ViewModel.

Дополнительные сведения о `ICommand` интерфейсе и привязках см. в разделе [ Xamarin.Forms Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md) и [интерфейс ICommand](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации Сеарчбар](/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Xamarin.Forms Текстовые элементы управления](~/xamarin-forms/user-interface/text/index.md)
* [Шрифты в Xamarin.Forms](~/xamarin-forms/user-interface/text/fonts.md)
* [Xamarin.Forms Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)