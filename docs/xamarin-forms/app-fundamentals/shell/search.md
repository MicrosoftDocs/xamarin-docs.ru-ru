---
title: Поиск по оболочке Xamarin.Forms
description: Приложения оболочки Xamarin.Forms могут использовать интегрированную функцию поиска, которая реализована в виде поля поиска в верхней части каждой страницы.
ms.prod: xamarin
ms.assetid: F8F9471D-6771-4D23-96C0-2B79473A06D4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cae7b1e86db6dfceb04c8298c116b6e7de32e5a1
ms.sourcegitcommit: 1b542afc0f6f2f6adbced527ae47b9ac90eaa1de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101758008"
---
# <a name="xamarinforms-shell-search"></a>Поиск по оболочке Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Оболочка Xamarin.Forms включает встроенные функции поиска, предоставляемые классом [`SearchHandler`](xref:Xamarin.Forms.SearchHandler). Возможность поиска можно добавить на страницу, указав для присоединенного свойства [`Shell.SearchHandler`](xref:Xamarin.Forms.SearchHandler) в качестве значения объект подкласса `SearchHandler`. После этого в верхней части страницы появится поле поиска:

[![Снимок экрана: объект SearchHandler оболочки на iOS и Android](search-images/searchhandler.png)](search-images/searchhandler-large.png#lightbox)

При вводе запроса в поле поиска обновляется свойство [`Query`](xref:Xamarin.Forms.SearchHandler.Query), а при каждом обновлении выполняется метод [`OnQueryChanged`](xref:Xamarin.Forms.SearchHandler.OnQueryChanged*). Этот метод может быть переопределен для заполнения данными области поисковых подсказок:

[![Снимок экрана: результаты поиска в объекте SearchHandler оболочки на iOS и Android](search-images/search-suggestions.png)](search-images/search-suggestions-large.png#lightbox)

При выборе результата из области поисковых подсказок выполняется метод [`OnItemSelected`](xref:Xamarin.Forms.SearchHandler.OnItemSelected*). Этот метод может быть переопределен, чтобы вызвать соответствующую реакцию, например переход на страницу подробных сведений.

## <a name="create-a-searchhandler"></a>Создание SearchHandler

Чтобы добавить функцию поиска в приложение оболочки, создайте класс, производный от класса [`SearchHandler`](xref:Xamarin.Forms.SearchHandler), и переопределите методы [`OnQueryChanged`](xref:Xamarin.Forms.SearchHandler.OnQueryChanged*) и [`OnItemSelected`](xref:Xamarin.Forms.SearchHandler.OnItemSelected*):

```csharp
public class AnimalSearchHandler : SearchHandler
{
    public IList<Animal> Animals { get; set; }
    public Type SelectedItemNavigationTarget { get; set; }

    protected override void OnQueryChanged(string oldValue, string newValue)
    {
        base.OnQueryChanged(oldValue, newValue);

        if (string.IsNullOrWhiteSpace(newValue))
        {
            ItemsSource = null;
        }
        else
        {
            ItemsSource = Animals
                .Where(animal => animal.Name.ToLower().Contains(newValue.ToLower()))
                .ToList<Animal>();
        }
    }

    protected override async void OnItemSelected(object item)
    {
        base.OnItemSelected(item);

        // Let the animation complete
        await Task.Delay(1000);

        ShellNavigationState state = (App.Current.MainPage as Shell).CurrentState;
        // The following route works because route names are unique in this application.
        await Shell.Current.GoToAsync($"{GetNavigationTarget()}?name={((Animal)item).Name}");
    }

    string GetNavigationTarget()
    {
        return (Shell.Current as AppShell).Routes.FirstOrDefault(route => route.Value.Equals(SelectedItemNavigationTarget)).Key;
    }
}
```

Переопределение [`OnQueryChanged`](xref:Xamarin.Forms.SearchHandler.OnQueryChanged*) имеет два аргумента: `oldValue` содержит предыдущий поисковый запрос, а `newValue` — текущий поисковый запрос. Область поисковых подсказок можно обновлять, задавая свойству [`SearchHandler.ItemsSource`](xref:Xamarin.Forms.SearchHandler.ItemsSource) значение коллекции `IEnumerable` с элементами, которые соответствуют текущему поисковому запросу.

Когда пользователь выбирает результат поиска, выполняется переопределение метода [`OnItemSelected`](xref:Xamarin.Forms.SearchHandler.OnItemSelected*) и присваивается значение свойству [`SelectedItem`](xref:Xamarin.Forms.SearchHandler.SelectedItem). В нашем примере этот метод переходит на другую страницу, которая отображает данные о выбранных `Animal`. Дополнительные сведения о навигации см. в разделе [Навигация по оболочке Xamarin.Forms](navigation.md).

> [!NOTE]
> Дополнительные свойства [`SearchHandler`](xref:Xamarin.Forms.SearchHandler) позволяют управлять внешним видом поля поиска.

## <a name="consume-a-searchhandler"></a>Использование SearchHandler

Подкласс [`SearchHandler`](xref:Xamarin.Forms.SearchHandler) можно использовать, присваивая присоединенному свойству [`Shell.SearchHandler`](xref:Xamarin.Forms.SearchHandler) значение объекта соответствующего типа на странице использования:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:AnimalSearchHandler Placeholder="Enter search term"
                                      ShowsResults="true"
                                      DisplayMemberName="Name" />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
Shell.SetSearchHandler(this, new AnimalSearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name"
});
```

Метод `AnimalSearchHandler.OnQueryChanged` возвращает `List` объектов `Animal`. Свойству [`DisplayMemberName`](xref:Xamarin.Forms.SearchHandler.DisplayMemberName) присваивается свойство `Name` каждого объекта `Animal`, а значит, отображаемые в области поисковых подсказок данные будут содержать названия каждого животного.

Свойству [`ShowsResults`](xref:Xamarin.Forms.SearchHandler.ShowsResults) присваивается значение `true`, чтобы поисковые подсказки отображались по мере ввода поискового запроса пользователем:

[![Снимок экрана с результатами поиска в объекте SearchHandler оболочки в iOS и Android; результаты для частичной строки M.](search-images/search-results.png)](search-images/search-results-large.png#lightbox)

По мере изменения поискового запроса данные в области поисковых подсказок обновляются:

[![Снимок экрана с результатами поиска в объекте SearchHandler оболочки в iOS и Android; показаны результаты для частичной строки M o n.](search-images/search-results-change.png)](search-images/search-results-change-large.png#lightbox)

При выборе результата поиска происходит переход к `MonkeyDetailPage` и отображается страница сведений о выбранной обезьяне:

[![Снимок экрана: сведения об обезьяне для iOS и Android](search-images/detailpage.png)](search-images/detailpage-large.png#lightbox)

## <a name="define-search-results-item-appearance"></a>Определения внешнего вида элементов в результатах поиска

Помимо отображения данных `string` в результатах поиска, вы можете изменить внешний вид каждого элемента результатов поиска, присвоив свойству [`SearchHandler.ItemTemplate`](xref:Xamarin.Forms.SearchHandler.ItemTemplate) значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">    
    <Shell.SearchHandler>
        <controls:AnimalSearchHandler Placeholder="Enter search term"
                                      ShowsResults="true">
            <controls:AnimalSearchHandler.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10"
                          ColumnDefinitions="0.15*,0.85*">
                        <Image Source="{Binding ImageUrl}"
                               HeightRequest="40"
                               WidthRequest="40" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold"
                               VerticalOptions="Center" />
                    </Grid>
                </DataTemplate>
            </controls:AnimalSearchHandler.ItemTemplate>
       </controls:AnimalSearchHandler>
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
Shell.SetSearchHandler(this, new AnimalSearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    ItemTemplate = new DataTemplate(() =>
    {
        Grid grid = new Grid { Padding = 10 };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.15, GridUnitType.Star) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.85, GridUnitType.Star) });

        Image image = new Image { HeightRequest = 40, WidthRequest = 40 };
        image.SetBinding(Image.SourceProperty, "ImageUrl");
        Label nameLabel = new Label { FontAttributes = FontAttributes.Bold, VerticalOptions = LayoutOptions.Center };
        nameLabel.SetBinding(Label.TextProperty, "Name");

        grid.Children.Add(image);
        grid.Children.Add(nameLabel, 1, 0);
        return grid;
    })
});
```

Указанные в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) элементы определяют внешний вид каждого элемента в области поисковых подсказок. В нашем примере компоновка внутри `DataTemplate` определяется значением [`Grid`](xref:Xamarin.Forms.Grid). `Grid` содержит объект [`Image`](xref:Xamarin.Forms.Image) и объект [`Label`](xref:Xamarin.Forms.Label), оба из которых содержат привязки к свойствам каждого объекта `Monkey`.

На следующих снимках экрана представлены результаты применения шаблона для всех элементов в области поисковых подсказок:

[![Снимок экрана: шаблонные результаты поиска в объекте SearchHandler оболочки на iOS и Android](search-images/search-results-template.png)](search-images/search-results-template-large.png#lightbox)

Дополнительные сведения о шаблонах данных см. в разделе [Общие сведения о шаблонах данныхXamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="search-box-visibility"></a>Видимость поля поиска

Когда в верхней части страницы добавляется [`SearchHandler`](xref:Xamarin.Forms.SearchHandler), поле поиска по умолчанию является видимым и полностью развернутым. Тем не менее это поведение можно изменить, задав в свойстве [`SearchHandler.SearchBoxVisibility`](xref:Xamarin.Forms.SearchHandler.SearchBoxVisibility) один из членов перечисления [`SearchBoxVisibility`](xref:Xamarin.Forms.SearchBoxVisibility):

- `Hidden` — поле поиска не отображается и недоступно;
- `Collapsible` — поле поиска является скрытым, пока пользователь не выполнит действие, открывающее его; Для отображения поля поиска в iOS нужно инициировать "подпрыгивание" содержимого страницы в вертикальном направлении, а в Android — коснуться значка вопросительного знака.
- `Expanded` — поле поиска является видимым и полностью развернутым. Это значение по умолчанию для свойства [`SearchBoxVisibility`](xref:Xamarin.Forms.SearchHandler.SearchBoxVisibility).

> [!IMPORTANT]
> В iOS для свертываемого поля поиска требуется iOS 11 или более поздней версии.

В следующем примере показано, как скрыть поле поиска:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:AnimalSearchHandler SearchBoxVisibility="Hidden"
                                      ... />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

## <a name="search-box-focus"></a>Фокус поля поиска

При касании поля поиска отображается экранная клавиатура, а в поле поиска появляется фокус ввода. Это можно также реализовать программным образом, вызвав метод [`Focus`](xref:Xamarin.Forms.SearchHandler.Focus), который пытается задать фокус ввода в поле поиска и возвращает `true` при успешном выполнении. Когда поле поиска получает фокус, активируется событие [`Focused`](xref:Xamarin.Forms.SearchHandler.Focused) и вызывается переопределяемый метод `OnFocused`.

Когда в поле поиска есть фокус ввода, касание экрана в любом другом месте скрывает экранную клавиатуру и поле теряет фокус ввода. Это можно также реализовать программным образом, вызвав метод [`Unfocus`](xref:Xamarin.Forms.SearchHandler.Unfocus). Когда поле поиска теряет фокус, активируется событие [`Unfocused`](xref:Xamarin.Forms.SearchHandler.Unfocused) и вызывается переопределяемый метод `OnUnfocus`.

Состояние фокуса в поле поиска можно получить с помощью свойства [`IsFocused`](xref:Xamarin.Forms.SearchHandler.IsFocused), которое возвращает `true`, если [`SearchHandler`](xref:Xamarin.Forms.SearchHandler) в этот момент имеет фокус ввода.

## <a name="searchhandler-keyboard"></a>Клавиатура SearchHandler

Клавиатуру, отображаемую при взаимодействии пользователя с [`SearchHandler`](xref:Xamarin.Forms.SearchHandler), можно задать программно с помощью свойства [`Keyboard`](xref:Xamarin.Forms.SearchHandler.Keyboard), используя следующие свойства класса [`Keyboard`](xref:Xamarin.Forms.Keyboard):

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) — это текстовая клавиатура с эмодзи.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) — это клавиатура по умолчанию.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) используется для ввода адресов электронной почты.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) — цифровая клавиатура.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) предназначена для ввода текста, если нет заданных [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags).
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) — клавиатура для ввода телефонных номеров.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) — текстовая клавиатура.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) — клавиатура для ввода путей к файлам и веб-адресов.

Это можно сделать в XAML следующим образом:

```xaml
<SearchHandler Keyboard="Email" />
```

Эквивалентный код на C# выглядит так:

```csharp
SearchHandler searchHandler = new SearchHandler { Keyboard = Keyboard.Email };
```

Класс [`Keyboard`](xref:Xamarin.Forms.Keyboard) также имеет фабричный метод [`Create`](xref:Xamarin.Forms.Keyboard.Create*), который может использоваться для настройки клавиатуры, задавая регистр букв, проверку орфографии и режим подсказок. Значения перечисления [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) задаются как аргументы метода, при этом возвращается настроенное свойство `Keyboard`. Перечисление `KeyboardFlags` имеет такие значения:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) указывает, что клавиатура не имеет никаких дополнительных функций.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) указывает, что первые слова во всех вводимых предложениях автоматически начинаются с прописных букв.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) указывает, что для вводимого текста выполняется проверка орфографии.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) указывает, что для вводимых слов предлагается завершение.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) указывает, что все слова автоматически начинаются с прописных букв.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) указывает, что все символы автоматически пишутся прописными буквами.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) указывает, что автоматическая подстановка прописных букв не выполняется.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) указывает, что для вводимого текста выполняется проверка орфографии, завершение слов и автоматическое написание предложений с прописной буквы.

В следующем примере кода XAML показано, как настроить значение по умолчанию [`Keyboard`](xref:Xamarin.Forms.Keyboard), чтобы включить предложение завершения слов и написание всех символов прописными буквами:

```xaml
<SearchHandler Placeholder="Enter search terms">
    <SearchHandler.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </SearchHandler.Keyboard>
</SearchHandler>
```

Эквивалентный код на C# выглядит так:

```csharp
SearchHandler searchHandler = new SearchHandler { Placeholder = "Enter search terms" };
searchHandler.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Навигация по оболочке Xamarin.Forms](navigation.md)
