---
title: Xamarin.Forms Операции
description: В этой статье объясняется, как использовать Xamarin.Forms класс entry для приема ввода текста или пароля в приложении.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 625dd57d1f84b95cef1c6513ae832af805bf565a
ms.sourcegitcommit: 10c7dd16fe78226053d1d036492b6c9102fc421b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "93375035"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms Операции

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-text)

Используется Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry) для однострочного ввода текста. `Entry`, Как и [`Editor`](xref:Xamarin.Forms.Editor) представление, поддерживает несколько типов клавиатуры. Кроме того, `Entry` можно использовать в качестве поля пароля.

## <a name="set-and-read-text"></a>Установка и чтение текста

`Entry`, Как и другие представления, представляемые текстом, предоставляет [`Text`](xref:Xamarin.Forms.InputView.Text) свойство. Это свойство можно использовать для установки и чтения текста, представленного `Entry` . В следующем примере показано задание `Text` свойства в XAML:

```xaml
<Entry Text="I am an Entry" />
```

В C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Чтобы прочитать текст, получите доступ к `Text` свойству в C#:

```csharp
var text = MyEntry.Text;
```

## <a name="set-placeholder-text"></a>Задать текст заполнителя

[`Entry`](xref:Xamarin.Forms.Entry)Может быть установлен для отображения текста заполнителя, если он не хранит данные, вводимые пользователем. Это достигается путем присвоения [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) свойству значения `string` , и часто используется для указания типа содержимого, подходящего для `Entry` . Кроме того, цвет текста заполнителя можно контролировать, присвоив [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) свойству значение [`Color`](xref:Xamarin.Forms.Color) .

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

> [!NOTE]
> Ширину `Entry` можно определить, задав ее `WidthRequest` свойство. Не зависят от ширины `Entry` определяемого значения, основанного на значении его `Text` Свойства.

## <a name="prevent-text-entry"></a>Запретить ввод текста

Пользователям может быть запрещено изменять текст в, [`Entry`](xref:Xamarin.Forms.Entry) задавая `IsReadOnly` свойство, которое по умолчанию имеет значение `false` , равным `true` :

```xaml
<Entry Text="This is a read-only Entry"
       IsReadOnly="true" />
```

```csharp
var entry = new Entry { Text = "This is a read-only Entry", IsReadOnly = true });
```

> [!NOTE]
> `IsReadonly`Свойство не изменяет внешний вид [`Entry`](xref:Xamarin.Forms.Entry) , в отличие от `IsEnabled` свойства, которое также изменяет внешний вид изображения `Entry` на серый.

## <a name="transform-text"></a>Преобразование текста

[`Entry`](xref:Xamarin.Forms.Entry)Может преобразовать регистр текста, хранящийся в `Text` свойстве, путем присвоения `TextTransform` свойству значения `TextTransform` перечисления. Это перечисление имеет четыре значения:

- `None` Указывает, что текст не будет преобразован.
- `Default` Указывает, что будет использоваться поведение по умолчанию для платформы. Это значение по умолчанию для свойства `TextTransform`.
- `Lowercase` Указывает, что текст будет преобразован в нижний регистр.
- `Uppercase` Указывает, что текст будет преобразован в верхний регистр.

В следующем примере показано преобразование текста в верхний регистр:

```xaml
<Entry Text="This text will be displayed in uppercase."
       TextTransform="Uppercase" />
```

Эквивалентный код на C# выглядит так:

```csharp
Entry entry = new Entry
{
    Text = "This text will be displayed in uppercase.",
    TextTransform = TextTransform.Uppercase
};
```

## <a name="limit-input-length"></a>Ограничить длину входных данных

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)Свойство можно использовать для ограничения длины ввода, разрешенной для [`Entry`](xref:Xamarin.Forms.Entry) . Для этого свойства необходимо задать положительное целое число:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)Значение свойства 0 указывает, что входные данные не допускаются, и значение `int.MaxValue` , которое является значением по умолчанию для [`Entry`](xref:Xamarin.Forms.Entry) , указывает на отсутствие действительного ограничения на количество вводимых символов.

## <a name="character-spacing"></a>Интервалы между символами

Интервал между символами можно применить к [`Entry`](xref:Xamarin.Forms.Entry) , присвоив `Entry.CharacterSpacing` свойству `double` значение:

```xaml
<Entry ...
       CharacterSpacing="10" />
```

Эквивалентный код на C# выглядит так:

```csharp
Entry entry = new Entry { CharacterSpacing = 10 };
```

Результат заключается в том, что символы в тексте, отображаемые в, [`Entry`](xref:Xamarin.Forms.Entry) `CharacterSpacing` разбиваются на устройства, независимые от устройств.

> [!NOTE]
> `CharacterSpacing`Значение свойства применяется к тексту, отображаемому `Text` `Placeholder` свойствами и.

## <a name="password-fields"></a>Поля пароля

`Entry` предоставляет `IsPassword` свойство. Если `IsPassword` имеет значение `true` , содержимое поля будет представлено черными кружками:

В XAML:

```xaml
<Entry IsPassword="true" />
```

В C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![Пример ввода пароля](entry-images/password.png)

Заполнители можно использовать с экземплярами `Entry` , которые настроены в качестве полей паролей.

В XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

В C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![Пример ввода пароля и заполнителя](entry-images/passwordplaceholder.png)

## <a name="set-the-cursor-position-and-text-selection-length"></a>Установка положения курсора и длины выделения текста

[`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition)Свойство можно использовать для возвращения или установки позиции, в которую следующий символ будет вставлен в строку, хранящуюся в [`Text`](xref:Xamarin.Forms.InputView.Text) свойстве:

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

Значение свойства по умолчанию [`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) равно 0, что означает, что текст будет вставлен в начало `Entry` .

Кроме того, [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) свойство можно использовать для возвращения или установки длины выделения текста в `Entry` :

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

Значение свойства по умолчанию [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) равно 0, что означает, что текст не выбран.

## <a name="display-a-clear-button"></a>Отображение кнопки "очистить"

`ClearButtonVisibility`Свойство можно использовать для управления [`Entry`](xref:Xamarin.Forms.Entry) отображением кнопки Clear, которая позволяет пользователю очистить текст. Этому свойству должно быть присвоено значение `ClearButtonVisibility` члена перечисления:

- `Never` Указывает, что кнопка Clear никогда не будет отображаться. Это значение по умолчанию для свойства `Entry.ClearButtonVisibility`;
- `WhileEditing` Указывает, что кнопка Clear будет отображаться в [`Entry`](xref:Xamarin.Forms.Entry) , тогда как у нее есть фокус и текст.

В следующем примере показано задание свойства в XAML:

```xaml
<Entry Text="Xamarin.Forms"
       ClearButtonVisibility="WhileEditing" />
```

Эквивалентный код на C# выглядит так:

```csharp
var entry = new Entry { Text = "Xamarin.Forms", ClearButtonVisibility = ClearButtonVisibility.WhileEditing };
```

На следующих снимках экрана показано, [`Entry`](xref:Xamarin.Forms.Entry) что включена кнопка Clear:

![Снимок экрана записи с кнопкой "очистить" в iOS и Android](entry-images/entry-clear-button.png)

## <a name="customize-the-keyboard"></a>Настройка клавиатуры

Клавиатура, представленная, когда пользователи взаимодействуют с [`Entry`](xref:Xamarin.Forms.Entry) компонентом, может быть задана программно с помощью [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) свойства, к одному из следующих свойств [`Keyboard`](xref:Xamarin.Forms.Keyboard) класса:

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
<Entry Keyboard="Chat" />
```

Эквивалентный код на C# выглядит так:

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
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
<Entry Placeholder="Enter text here">
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

Эквивалентный код на C# выглядит так:

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="customize-the-return-key"></a>Настройка ключа возврата

Внешний вид ключа возврата на экранной клавиатуре, который отображается при [`Entry`](xref:Xamarin.Forms.Entry) наличии фокуса, может быть настроен путем присвоения [`ReturnType`](xref:Xamarin.Forms.Entry.ReturnType) свойству значения [`ReturnType`](xref:Xamarin.Forms.ReturnType) перечисления:

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) — Указывает, что какой-либо конкретный ключ возврата не требуется и будет использоваться платформа по умолчанию.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) — Указывает ключ возврата "Done".
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) — Указывает ключ возврата "Go".
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) — Указывает ключ возврата "Next".
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) — Указывает ключ возврата поиска.
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) — Указывает ключ возврата "Send".

В следующем примере XAML показано, как задать ключ возврата:

```xaml
<Entry ReturnType="Send" />
```

Эквивалентный код на C# выглядит так:

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Точный вид ключа возврата зависит от платформы. В iOS ключ возврата — это текстовая кнопка. Однако на платформах Android и универсальных ОС Windows ключ возврата является кнопкой на основе значков.

При нажатии клавиши Return [`Completed`](xref:Xamarin.Forms.Entry.Completed) вызывается событие и выполняется любое значение, `ICommand` заданное [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) свойством. Кроме того, любое значение, `object` заданное [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) свойством, будет передано в `ICommand` качестве параметра. Дополнительные сведения о командах см. в разделе [Командный интерфейс](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="enable-and-disable-spell-checking"></a>Включение и отключение проверки орфографии

[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)Свойство определяет, включена ли проверка орфографии. По умолчанию свойство имеет значение `true` . При вводе пользователем текста указывается опечатка.

Однако для некоторых сценариев ввода текста, таких как ввод имени пользователя, проверка орфографии обеспечивает негативную работу и должна быть отключена путем присвоения [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) свойству значения `false` :

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Если [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) свойство имеет значение `false` , а пользовательская клавиатура не используется, встроенная проверка орфографии будет отключена. Однако если [`Keyboard`](xref:Xamarin.Forms.Keyboard) задано значение, которое отключает проверку орфографии, например [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat) , `IsSpellCheckEnabled` свойство игнорируется. Поэтому свойство не может быть использовано для включения проверки орфографии для того `Keyboard` , что явно отключает его.

## <a name="enable-and-disable-text-prediction"></a>Включение и отключение прогнозирования текста

[`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)Свойство определяет, включена ли функция прогнозирования текста и автоматического исправления текста. По умолчанию свойство имеет значение `true` . При вводе пользователем текста отображаются прогнозы Word.

Однако для некоторых сценариев ввода текста, таких как ввод имени пользователя, прогнозирование текста и автоматическое исправление текста обеспечивают негативную работу и должны быть отключены путем присвоения [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) свойству значения `false` :

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Если [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) свойство имеет значение `false` , а пользовательская клавиатура не используется, прогнозирование текста и автоматическое исправление текста отключаются. Однако если было [`Keyboard`](xref:Xamarin.Forms.Keyboard) задано, что отключает прогнозирование текста, `IsTextPredictionEnabled` свойство игнорируется. Таким образом, свойство нельзя использовать для включения прогнозирования текста для `Keyboard` , который явно отключает его.

## <a name="colors"></a>Цвета

Запись может быть настроена для использования пользовательского фона и цвета текста с помощью следующих связываемых свойств:

- **TextColor** &ndash; Задает цвет текста.
- **BackgroundColor** &ndash; Задает цвет, отображаемый за текстом.

Чтобы обеспечить возможность использования цветов на каждой платформе, необходимо особое внимание. Так как каждая платформа имеет разные значения по умолчанию для цветов текста и фона, часто приходится устанавливать и то, и другое, если задать их.

Чтобы задать цвет текста записи, используйте следующий код:

В XAML:

```xaml
<Entry TextColor="Green" />
```

В C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![Пример записи TextColor](entry-images/textcolor.png)

Обратите внимание, что заполнитель не зависит от указанного `TextColor` .

Чтобы задать цвет фона в XAML, сделайте следующее:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

В C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![Пример записи BackgroundColor](entry-images/textbackgroundcolor.png)

Будьте внимательны, чтобы цвет фона и текста, который вы выбрали, можно было использовать на каждой платформе, и не скрывать текст заполнителя.

## <a name="events-and-interactivity"></a>События и интерактивность

Запись предоставляет два события:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)&ndash;возникает, когда текст изменяется в записи. Предоставляет текст до и после изменения.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed)&ndash;возникает, когда пользователь завершает ввод, нажав клавишу Return на клавиатуре.

> [!NOTE]
> [`VisualElement`](xref:Xamarin.Forms.VisualElement)Класс, от которого [`Entry`](xref:Xamarin.Forms.Entry) наследует, также имеет [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) события и [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) .

### <a name="completed"></a>Завершено

`Completed`Событие используется для реагирования на завершение взаимодействия с записью. `Completed` вызывается, когда пользователь заканчивает ввод с полем, нажав клавишу Return на клавиатуре (или нажав клавишу TAB в UWP). Обработчик события является универсальным обработчиком событий, принимающим отправителя и `EventArgs` :

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

На языке XAML можно подписываться на событие Completed:

```xaml
<Entry Completed="Entry_Completed" />
```

и C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

После [`Completed`](xref:Xamarin.Forms.Entry.Completed) срабатывания события выполняется любое значение, заданное `ICommand` [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) свойством, с `object` заданным [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) свойством, передаваемым в `ICommand` .

### <a name="textchanged"></a>TextChanged

`TextChanged`Событие используется для реагирования на изменение содержимого поля.

`TextChanged` вызывается при каждом `Text` `Entry` изменении. Обработчик события принимает экземпляр `TextChangedEventArgs` . `TextChangedEventArgs` предоставляет доступ к старым и новым значениям с `Entry` `Text` помощью `OldTextValue` `NewTextValue` свойств и:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

На `TextChanged` событие можно подписываться в XAML:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

и C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```

## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [API Entry](xref:Xamarin.Forms.Entry)