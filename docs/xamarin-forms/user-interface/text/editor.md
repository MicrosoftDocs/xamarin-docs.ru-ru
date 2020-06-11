---
Title: " Xamarin.Forms редактор" Description: "в этой статье объясняется, как использовать Xamarin.Forms элемент управления редактора для приема многострочного ввода текста в приложении".
MS. произв. Xamarin MS. AssetID: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 09/26/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-editor"></a>Xamarin.FormsРедактор

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Многострочный ввод текста_

[`Editor`](xref:Xamarin.Forms.Editor)Элемент управления используется для приема многострочного ввода. В этой статье рассматриваются следующие вопросы:

- **[Настройка](#customization)** &ndash; Параметры клавиатуры и цвета.
- **[Интерактивность](#interactivity)** &ndash; события, которые можно прослушивать для предоставления интерактивности.

## <a name="customization"></a>Настройка

### <a name="setting-and-reading-text"></a>Установка и чтение текста

[`Editor`](xref:Xamarin.Forms.Editor), Как и другие представления, представляемые текстом, предоставляет `Text` свойство. Это свойство можно использовать для установки и чтения текста, представленного `Editor` . В следующем примере показано задание `Text` свойства в XAML:

```xaml
<Editor Text="I am an Editor" />
```

В C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Чтобы прочитать текст, получите доступ к `Text` свойству в C#:

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>Задание текста заполнителя

[`Editor`](xref:Xamarin.Forms.Editor)Может быть установлен для отображения текста заполнителя, если он не хранит данные, вводимые пользователем. Это достигается путем присвоения [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) свойству значения `string` , и часто используется для указания типа содержимого, подходящего для `Editor` . Кроме того, цвет текста заполнителя можно контролировать, присвоив [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) свойству значение [`Color`](xref:Xamarin.Forms.Color) .

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>Предотвращение ввода текста

Пользователям может быть запрещено изменять текст в, [`Editor`](xref:Xamarin.Forms.Editor) задавая `IsReadOnly` свойство, которое по умолчанию имеет значение `false` , равным `true` :

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> `IsReadonly`Свойство не изменяет внешний вид [`Editor`](xref:Xamarin.Forms.Editor) , в отличие от `IsEnabled` свойства, которое также изменяет внешний вид изображения `Editor` на серый.

### <a name="limiting-input-length"></a>Ограничение длины входных данных

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)Свойство можно использовать для ограничения длины ввода, разрешенной для [`Editor`](xref:Xamarin.Forms.Editor) . Для этого свойства необходимо задать положительное целое число:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)Значение свойства 0 указывает, что входные данные не допускаются, и значение `int.MaxValue` , которое является значением по умолчанию для [`Editor`](xref:Xamarin.Forms.Editor) , указывает на отсутствие действительного ограничения на количество вводимых символов.

### <a name="character-spacing"></a>Интервалы между символами

Интервал между символами можно применить к [`Editor`](xref:Xamarin.Forms.Editor) , присвоив `Editor.CharacterSpacing` свойству `double` значение:

```xaml
<Editor ...
        CharacterSpacing="10" />
```

Эквивалентный код на C# выглядит так:

```csharp
Editor editor = new editor { CharacterSpacing = 10 };
```

Результат заключается в том, что символы в тексте, отображаемые в, [`Editor`](xref:Xamarin.Forms.Editor) `CharacterSpacing` разбиваются на устройства, независимые от устройств.

> [!NOTE]
> `CharacterSpacing`Значение свойства применяется к тексту, отображаемому `Text` `Placeholder` свойствами и.

### <a name="auto-sizing-an-editor"></a>Автоматическое изменение размера редактора

[`Editor`](xref:Xamarin.Forms.Editor)Можно установить для автоматического изменения размера содержимого, задав [`Editor.AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) свойству [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) значение, которое является значением [`EditoAutoSizeOption`](xref:Xamarin.Forms.EditorAutoSizeOption) перечисления. Это перечисление имеет два значения:

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled)Указывает, что автоматическое изменение размера отключено и является значением по умолчанию.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)Указывает, что автоматическое изменение размера включено.

Это можно сделать в коде следующим образом:

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

Если автоматическое изменение размера включено, то [`Editor`](xref:Xamarin.Forms.Editor) при заполнении пользователем текста высота будет увеличиваться, а высота будет уменьшаться по мере удаления текста пользователем.

> [!NOTE]
> [`Editor`](xref:Xamarin.Forms.Editor)Если свойство задано, автоматическое масштабирование не выполняется [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) .

### <a name="customizing-the-keyboard"></a>Настройка клавиатуры

Клавиатура, представленная, когда пользователи взаимодействуют с [`Editor`](xref:Xamarin.Forms.Editor) компонентом, может быть задана программно с помощью [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) свойства, к одному из следующих свойств [`Keyboard`](xref:Xamarin.Forms.Keyboard) класса:

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
<Editor Keyboard="Chat" />
```

Эквивалентный код на C# выглядит так:

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
```

Примеры каждой клавиатуры можно найти в нашем репозитории [рецептов](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) .

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
<Editor>
    <Editor.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Editor.Keyboard>
</Editor>
```

Эквивалентный код на C# выглядит так:

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="enabling-and-disabling-spell-checking"></a>Включение и отключение проверки орфографии

[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)Свойство определяет, включена ли проверка орфографии. По умолчанию свойство имеет значение `true` . При вводе пользователем текста указывается опечатка.

Однако для некоторых сценариев ввода текста, таких как ввод имени пользователя, проверка орфографии обеспечивает негативную работу, поэтому ее следует отключить, задав [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) для свойства значение `false` :

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Если [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) свойство имеет значение `false` , а пользовательская клавиатура не используется, встроенная проверка орфографии будет отключена. Однако если [`Keyboard`](xref:Xamarin.Forms.Keyboard) задано значение, которое отключает проверку орфографии, например [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat) , `IsSpellCheckEnabled` свойство игнорируется. Поэтому свойство не может быть использовано для включения проверки орфографии для того `Keyboard` , что явно отключает его.

### <a name="enabling-and-disabling-text-prediction"></a>Включение и отключение прогнозирования текста

`IsTextPredictionEnabled`Свойство определяет, включена ли функция прогнозирования текста и автоматического исправления текста. По умолчанию свойство имеет значение `true` . При вводе пользователем текста отображаются прогнозы Word.

Однако для некоторых сценариев ввода текста, таких как ввод имени пользователя, прогнозирование текста и автоматическое исправление текста обеспечивают негативную работу и должны быть отключены путем присвоения `IsTextPredictionEnabled` свойству значения `false` :

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Если `IsTextPredictionEnabled` свойство имеет значение `false` , а пользовательская клавиатура не используется, прогнозирование текста и автоматическое исправление текста отключаются. Однако если было [`Keyboard`](xref:Xamarin.Forms.Keyboard) задано, что отключает прогнозирование текста, `IsTextPredictionEnabled` свойство игнорируется. Таким образом, свойство нельзя использовать для включения прогнозирования текста для `Keyboard` , который явно отключает его.

### <a name="colors"></a>Цвета

`Editor`можно настроить для использования пользовательского цвета фона с помощью `BackgroundColor` Свойства. Чтобы обеспечить возможность использования цветов на каждой платформе, необходимо особое внимание. Так как каждая платформа имеет разные значения по умолчанию для цвета текста, может потребоваться задать пользовательский цвет фона для каждой платформы. Дополнительные сведения об оптимизации пользовательского интерфейса для каждой платформы см. [в разделе Работа с](~/xamarin-forms/platform/device.md) оптимизациями платформы.

В C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "Editor with BackgroundColor Example")

Убедитесь, что выбранный цвет фона и текста можно использовать на каждой платформе и не закрывает текст заполнителя.

## <a name="interactivity"></a>Интерактивность

`Editor`предоставляет два события:

- [TextChanged](xref:Xamarin.Forms.InputView.TextChanged) &ndash; возникает при изменении текста в редакторе. Предоставляет текст до и после изменения.
- [Завершено](xref:Xamarin.Forms.Editor.Completed) &ndash; возникает, когда пользователь завершает ввод, нажав клавишу Return на клавиатуре.

> [!NOTE]
> [`VisualElement`](xref:Xamarin.Forms.VisualElement)Класс, от которого [`Entry`](xref:Xamarin.Forms.Entry) наследует, также имеет [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) события и [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) .

### <a name="completed"></a>Завершено

`Completed`Событие используется для реагирования на завершение взаимодействия с `Editor` . `Completed`вызывается, когда пользователь заканчивает ввод с полем, вводя ключ возврата на клавиатуре (или нажав клавишу TAB в UWP). Обработчик события является универсальным обработчиком событий, принимающим отправителя и `EventArgs` :

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

На событие Completed можно подписываться в коде и XAML:

В C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

`TextChanged`Событие используется для реагирования на изменение содержимого поля.

`TextChanged`вызывается при каждом `Text` `Editor` изменении. Обработчик события принимает экземпляр `TextChangedEventArgs` . `TextChangedEventArgs`предоставляет доступ к старым и новым значениям с `Editor` `Text` помощью `OldTextValue` `NewTextValue` свойств и:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

На событие Completed можно подписываться в коде и XAML:

В коде:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [API редактора](xref:Xamarin.Forms.Editor)
