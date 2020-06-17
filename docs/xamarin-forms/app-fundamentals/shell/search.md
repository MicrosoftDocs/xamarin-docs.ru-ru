---
title: "Xamarin.Forms — поиск по оболочке" description: "Xamarin.Forms — приложения оболочки могут использовать интегрированную функцию поиска, которая реализована в виде поля поиска в верхней части каждой страницы".
ms.prod: xamarin ms.assetid: F8F9471D-6771-4D23-96C0-2B79473A06D4 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 12/18/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-shell-search"></a>Поиск по оболочке Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Оболочка Xamarin.Forms включает встроенные функции поиска, предоставляемые классом `SearchHandler`. Возможность поиска можно добавить на страницу, указав для присоединенного свойства `Shell.SearchHandler` в качестве значения объект подкласса `SearchHandler`. После этого в верхней части страницы появится поле поиска:

[![Снимок экрана: объект SearchHandler оболочки на iOS и Android](search-images/searchhandler.png "Объект SearchHandler оболочки")](search-images/searchhandler-large.png#lightbox "Объект SearchHandler оболочки")

При вводе запроса в поле поиска обновляется свойство `Query`, а при каждом обновлении выполняется метод `OnQueryChanged`. Этот метод может быть переопределен для заполнения данными области поисковых подсказок:

[![Снимок экрана: результаты поиска в объекте SearchHandler оболочки на iOS и Android](search-images/search-suggestions.png "Результаты поиска в объекте SearchHandler оболочки")](search-images/search-suggestions-large.png#lightbox "Результаты поиска в объекте SearchHandler оболочки")

При выборе результата из области поисковых подсказок выполняется метод `OnItemSelected`. Этот метод может быть переопределен, чтобы вызвать соответствующую реакцию, например переход на страницу подробных сведений.

## <a name="create-a-searchhandler"></a>Создание SearchHandler

Чтобы добавить функцию поиска в приложение оболочки, создайте класс, производный от класса `SearchHandler`, и переопределите методы `OnQueryChanged` и `OnItemSelected`.

```csharp
public class MonkeySearchHandler : SearchHandler
{
    protected override void OnQueryChanged(string oldValue, string newValue)
    {
        base.OnQueryChanged(oldValue, newValue);

        if (string.IsNullOrWhiteSpace(newValue))
        {
            ItemsSource = null;
        }
        else
        {
            ItemsSource = MonkeyData.Monkeys
                .Where(monkey => monkey.Name.ToLower().Contains(newValue.ToLower()))
                .ToList<Animal>();
        }
    }

    protected override async void OnItemSelected(object item)
    {
        base.OnItemSelected(item);

        // Note: strings will be URL encoded for navigation (e.g. "Blue Monkey" becomes "Blue%20Monkey"). Therefore, decode at the receiver.
        await (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"monkeydetails?name={((Animal)item).Name}");
    }
}
```

Переопределение `OnQueryChanged` имеет два аргумента: `oldValue` содержит предыдущий поисковый запрос, а `newValue` — текущий поисковый запрос. Область поисковых подсказок можно обновлять, задавая свойству `SearchHandler.ItemsSource` значение коллекции `IEnumerable` с элементами, которые соответствуют текущему поисковому запросу.

Когда пользователь выбирает результат поиска, выполняется переопределение метода `OnItemSelected` и присваивается значение свойству `SelectedItem`. В нашем примере этот метод переходит на другую страницу, которая отображает данные о выбранных `Animal`. Дополнительные сведения о навигации см. в разделе [Навигация по оболочке Xamarin.Forms](navigation.md).

> [!NOTE]
> Дополнительные свойства `SearchHandler` позволяют управлять внешним видом поля поиска.

## <a name="consume-a-searchhandler"></a>Использование SearchHandler

Подкласс `SearchHandler` можно использовать, присваивая присоединенному свойству `Shell.SearchHandler` значение объекта соответствующего типа:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true"
                                      DisplayMemberName="Name" />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name"
});
```

Метод `MonkeySearchHandler.OnQueryChanged` возвращает `List` объектов `Animal`. Свойству `DisplayMemberName` присваивается свойство `Name` каждого объекта `Animal`, а значит отображаемые в области поисковых подсказок данные будут содержать названия каждого животного.

Свойству `ShowsResults` присваивается значение `true`, чтобы поисковые подсказки отображались по мере ввода поискового запроса пользователем:

[![Снимок экрана: результаты поиска в объекте SearchHandler оболочки на iOS и Android](search-images/search-results.png "Результаты поиска в объекте SearchHandler оболочки")](search-images/search-results-large.png#lightbox "Результаты поиска в объекте SearchHandler оболочки")

По мере изменения поискового запроса данные в области поисковых подсказок обновляются:

[![Снимок экрана: результаты поиска в объекте SearchHandler оболочки на iOS и Android](search-images/search-results-change.png "Результаты поиска в объекте SearchHandler оболочки")](search-images/search-results-change-large.png#lightbox "Результаты поиска в объекте SearchHandler оболочки")

При выборе результата поиска происходит переход к `MonkeyDetailPage` и отображаются данные о выбранной обезьяне:

[![Снимок экрана: сведения об обезьяне для iOS и Android](search-images/detailpage.png "Сведения об обезьяне")](search-images/detailpage-large.png#lightbox "Сведения об обезьяне")

## <a name="define-search-results-item-appearance"></a>Определения внешнего вида элементов в результатах поиска

Помимо отображения данных `string` в результатах поиска, вы можете изменить внешний вид каждого элемента результатов поиска, присвоив свойству `SearchHandler.ItemTemplate` значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">    
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true">
            <controls:MonkeySearchHandler.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="0.15*" />
                            <ColumnDefinition Width="0.85*" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="40"
                               WidthRequest="40" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                    </Grid>
                </DataTemplate>
            </controls:MonkeySearchHandler.ItemTemplate>
       </controls:MonkeySearchHandler>
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name",
    ItemTemplate = new DataTemplate(() =>
    {
        Grid grid = new Grid { Padding = 10 };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.15, GridUnitType.Star) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.85, GridUnitType.Star) });

        Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 40, WidthRequest = 40 };
        image.SetBinding(Image.SourceProperty, "ImageUrl");
        Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        nameLabel.SetBinding(Label.TextProperty, "Name");

        grid.Children.Add(image);
        grid.Children.Add(nameLabel, 1, 0);
        return grid;
    })
});
```

Указанные в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) элементы определяют внешний вид каждого элемента в области поисковых подсказок. В нашем примере компоновка внутри `DataTemplate` определяется значением [`Grid`](xref:Xamarin.Forms.Grid). `Grid` содержит объект [`Image`](xref:Xamarin.Forms.Image) и объект [`Label`](xref:Xamarin.Forms.Label), оба из которых содержат привязки к свойствам каждого объекта `Monkey`.

На следующих снимках экрана представлены результаты применения шаблона для всех элементов в области поисковых подсказок:

[![Снимок экрана: шаблонные результаты поиска в объекте SearchHandler оболочки на iOS и Android](search-images/search-results-template.png "Шаблонные результаты поиска в объекте SearchHandler оболочки")](search-images/search-results-template-large.png#lightbox "Шаблонные результаты поиска в объекте SearchHandler оболочки")

Дополнительные сведения о шаблонах данных см. в разделе [Общие сведения о шаблонах данныхXamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="search-box-visibility"></a>Видимость поля поиска

Когда в верхней части страницы добавляется `SearchHandler`, поле поиска по умолчанию является видимым и полностью развернутым. Тем не менее, это поведение можно изменить, задав в свойстве `SearchHandler.SearchBoxVisibility` один из членов перечисления `SearchBoxVisibility`:

- `Hidden` — поле поиска не отображается и недоступно;
- `Collapsible` — поле поиска является скрытым, пока пользователь не выполнит действие, открывающее его; Для отображения поля поиска в iOS нужно инициировать "подпрыгивание" содержимого страницы в вертикальном направлении, а в Android — коснуться значка вопросительного знака.
- `Expanded` — поле поиска является видимым и полностью развернутым. Это значение по умолчанию для свойства `SearchHandler.SearchBoxVisibility`.

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

При касании поля поиска отображается экранная клавиатура, а в поле поиска появляется фокус ввода. Это можно также реализовать программным образом, вызвав метод `Focus`, который пытается задать фокус ввода в поле поиска и возвращает `true` при успешном выполнении. Когда поле поиска получает фокус, активируется событие `Focus` и вызывается переопределяемый метод `OnFocused`.

Когда в поле поиска есть фокус ввода, касание экрана в любом другом месте скрывает экранную клавиатуру и поле теряет фокус ввода. Это можно также реализовать программным образом, вызвав метод `Unfocus`. Когда поле поиска теряет фокус, активируется событие `Unfocused` и вызывается переопределяемый метод `OnUnfocus`.

Состояние фокуса в поле поиска можно получить с помощью свойства `IsFocused`, которое возвращает `true`, если `SearchHandler` в этот момент имеет фокус ввода.

## <a name="searchhandler-appearance"></a>Внешний вид SearchHandler

Класс `SearchHandler` предоставляет следующие свойства, которые определяют его внешний вид:

- `BackgroundColor` с типом `Color` определяет цвет фона для текста поля поиска.
- `CancelButtonColor` с типом `Color` определяет цвет кнопки "Отмена".
- `CharacterSpacing` с типом `double` представляет собой интервал между знаками текста `SearchHandler`.
- `FontAttributes` с типом `FontAttributes` указывает, является ли шрифт поля поиска наклонным или жирным.
- `FontFamily` с типом `string` определяет семейство шрифтов, используемое для текста поля поиска.
- `FontSize` с типом `double` определяет размер шрифта для текста поля поиска.
- `HorizontalTextAlignment` с типом `TextAlignment` определяет горизонтальное выравнивание для текста поля поиска.
- `PlaceholderColor` с типом `Color` определяет цвет текста заполнителя в поле поиска.
- `TextColor` с типом `Color` определяет цвет текста в поле поиска.
- `VerticalTextAlignment` с типом `TextAlignment` определяет вертикальное выравнивание для текста поля поиска.

## <a name="searchhandler-keyboard"></a>Клавиатура SearchHandler

Клавиатуру, отображаемую при взаимодействии пользователя с `SearchHandler`, можно задать программно с помощью свойства `Keyboard`, используя следующие свойства класса [`Keyboard`](xref:Xamarin.Forms.Keyboard):

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

## <a name="searchhandler-reference"></a>Справочные данные по SearchHandler

Класс `SearchHandler` предоставляет следующие свойства, которые определяют его внешний вид и поведение.

- `BackgroundColor` с типом `Color` определяет цвет фона для текста поля поиска.
- `CancelButtonColor` с типом `Color` определяет цвет кнопки "Отмена".
- `ClearIcon` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource) обозначает отображаемый значок очистки поля поиска.
- `ClearIconHelpText` с типом `string` — доступный текст справки для значка очистки.
- `ClearIconName` с типом `string` — имя значка очистки, которое используется со средствами чтения с экрана.
- `ClearPlaceholderCommand` с типом `ICommand`, который выполняется при касании `ClearPlaceholderIcon`.
- `ClearPlaceholderCommandParameter` с типом `object`, который передается как параметр в `ClearPlaceholderCommand`.
- `ClearPlaceholderEnabled` с типом `bool`, который определяет возможность выполнять `ClearPlaceholderCommand`. Значение по умолчанию — `true`.
- `ClearPlaceholderHelpText` с типом `string` — доступный текст справки возле значка заполнителя очистки.
- `ClearPlaceholderIcon` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource) — значок заполнителя очистки, отображаемый при пустом поле поиска.
- `ClearPlaceholderName` с типом `string` — имя значка заполнителя очистки для использования со средствами чтения с экрана.
- `Command` с типом `ICommand`, который выполняется при подтверждении поискового запроса.
- `CommandParameter` с типом `object`, который передается как параметр в `Command`.
- `DisplayMemberName` с типом `string` обозначает имя или путь к свойству, которое отображается для каждого элемента данных в коллекции `ItemsSource`.
- `FontAttributes` с типом `FontAttributes` указывает, является ли шрифт поля поиска наклонным или жирным.
- `FontFamily` с типом `string` определяет семейство шрифтов, используемое для текста поля поиска.
- `FontSize` с типом `double` определяет размер шрифта для текста поля поиска.
- `HorizontalTextAlignment` с типом `TextAlignment` определяет горизонтальное выравнивание для текста поля поиска.
- `IsFocused` с типом `bool` указывает, имеет ли сейчас `SearchHandler` фокус ввода.
- `IsSearchEnabled` с типом `bool`, который представляет состояние активности поля поиска. Значение по умолчанию — `true`.
- `ItemsSource` с типом `IEnumerable` определяет коллекцию элементов, которые отображаются в области поисковых подсказок. По умолчанию имеет значение `null`.
- `ItemTemplate` с типом [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяет шаблон, применяемый к каждому элементу в коллекции элементов, которые отображаются в области поисковых подсказок.
- `Keyboard` с типом `Keyboard` является клавиатурой для `SearchHandler`.
- `Placeholder` с типом `string` — текст для отображения при пустом поле поиска.
- `PlaceholderColor` с типом `Color` определяет цвет текста заполнителя в поле поиска.
- `Query` с типом `string` — введенный пользователем текст в поле поиска.
- `QueryIcon` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource) — значок, напоминающий пользователю о том, что доступна функция поиска.
- `QueryIconHelpText` с типом `string` — доступный текст справки для значка запроса.
- `QueryIconName` с типом `string` — имя значка запроса, которое используется со средствами чтения с экрана.
- `SearchBoxVisibility` с типом `SearchBoxVisibility` определяет видимость поля поиска. По умолчанию поле поиска является видимым и полностью развернутым.
- `SelectedItem` с типом `object` — выбранный элемент в результатах поиска. Это свойство доступно только для чтения и по умолчанию имеет значение `null`.
- `ShowsResults` с типом `bool` обозначает, нужно ли ожидать результаты поиска в области поисковых подсказок при вводе текста. Значение по умолчанию — `false`.
- `TextColor` с типом `Color` определяет цвет текста в поле поиска.

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

Кроме того, класс `SearchHandler` также предоставляет следующие переопределяемые методы:

- `OnClearPlaceholderClicked` вызывается каждый раз при касании `ClearPlaceholderIcon`;
- `OnItemSelected` вызывается каждый раз, когда пользователь выбирает результат поиска;
- `OnFocused` вызывается, когда `SearchHandler` получает фокус ввода.
- `OnQueryChanged` вызывается при изменении свойства `Query`;
- `OnQueryConfirmed` вызывается каждый раз, когда пользователь нажимает клавишу ВВОД или подтверждает запрос в поле поиска.
- `OnUnfocus` вызывается, когда `SearchHandler` теряет фокус ввода.

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Навигация по оболочке Xamarin.Forms](navigation.md)
