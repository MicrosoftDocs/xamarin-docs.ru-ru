---
title: Разметка C# Xamarin. Forms
description: Разметка C# — это набор вспомогательных методов и классов Fluent, позволяющий упростить процесс создания декларативных пользовательских интерфейсов Xamarin. Forms в C#.
ms.prod: xamarin
ms.assetid: D41B9DCD-5C34-4C2F-B177-FC082AB2E9E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/15/2020
ms.openlocfilehash: 4fa8397dafbbdd836f88193081720b4960f1ce5d
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/15/2020
ms.locfileid: "83425810"
---
# <a name="xamarinforms-c-markup"></a>Разметка C# Xamarin. Forms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)

Разметка C# — это набор вспомогательных методов и классов Fluent, позволяющий упростить процесс создания декларативных пользовательских интерфейсов Xamarin. Forms в C#. API-интерфейс Fluent, предоставляемый разметкой C#, доступен в `Xamarin.Forms.Markup` пространстве имен.

Как и в случае с XAML, разметка C# обеспечивает четкое разделение между разметкой пользовательского интерфейса и логикой пользовательского интерфейса. Это можно сделать, разделив разметку пользовательского интерфейса и логику пользовательского интерфейса на отдельные файлы разделяемого класса. Например, для страницы входа разметка пользовательского интерфейса будет находиться в файле с именем *LoginPage.CS*, а логика пользовательского интерфейса — в файле с именем *LoginPage.Logic.CS*.

Разметка C# доступна в Xamarin. Forms 4,6. Однако в настоящее время он экспериментальен и может использоваться только путем добавления следующей строки кода в файл *app.CS* :

```csharp
Device.SetFlags(new string[]{ "Markup_Experimental" });
```

> [!NOTE]
> Разметка C# доступна на всех платформах, поддерживаемых Xamarin. Forms.

## <a name="basic-example"></a>Простой пример

В следующем примере показано, как задать для содержимого страницы новое [`Grid`](xref:Xamarin.Forms.Grid) , содержащее [`Label`](xref:Xamarin.Forms.Label) и [`Entry`](xref:Xamarin.Forms.Entry) , в C#:

```csharp
Grid grid = new Grid();

Label label = new Label { Text = "Code: " };
grid.Children.Add(label, 0, 1);

Entry entry = new Entry
{
    Placeholder = "Enter number",
    Keyboard = Keyboard.Numeric,
    BackgroundColor = Color.AliceBlue,
    TextColor = Color.Black,
    FontSize = 15,
    HeightRequest = 44,
    Margin = fieldMargin
};
grid.Children.Add(entry, 0, 2);
Grid.SetColumnSpan(entry, 2);
entry.SetBinding(Entry.TextProperty, new Binding("RegistrationCode"));

Content = grid;
```

В этом примере создается [`Grid`](xref:Xamarin.Forms.Grid) объект с дочерними [`Label`](xref:Xamarin.Forms.Label) [`Entry`](xref:Xamarin.Forms.Entry) объектами и. `Label`Отображает текст и `Entry` привязывает данные к `RegistrationCode` свойству ViewModel. Каждое дочернее представление имеет значение, отображаемое в определенной строке в `Grid` , а также `Entry` охватывает все столбцы в `Grid` . Кроме того, высота `Entry` задается вместе с клавиатурой, цветами, размером шрифта его текста и его значением `Margin` . Наконец, свойству присваивается значение `Page.Content` `Grid` Object.

Разметка C# позволяет повторно написать этот код с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.Markup;
using static Xamarin.Forms.Markup.GridRowsColumns;

Content = new Grid
{
  Children =
  {
    new Label { Text = "Code:" }
               .Row (BodyRow.CodeHeader) .Column (BodyCol.Header),

    new Entry { Placeholder = "Enter number", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
               .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
               .Bind (nameof(vm.RegistrationCode))
  }
}};
```

Этот пример идентичен предыдущему примеру, но API-интерфейс Fluent разметки C# упрощает процесс создания пользовательского интерфейса в C#.

> [!NOTE]
> Разметка C# включает методы расширения, которые задают свойства конкретного представления. Эти методы расширения не предназначены для замены всех методов задания свойств. Вместо этого они предназначены для улучшения читаемости кода и могут использоваться в сочетании с методами задания свойств. Рекомендуется всегда использовать метод расширения, если он существует для свойства, но вы можете выбрать предпочтительный баланс.

## <a name="data-binding"></a>Привязка данных

Разметка C# включает `Bind` метод расширения вместе с перегрузками, который создает привязку данных между связанным свойством представления и указанным свойством. `Bind`Метод знает свойство, которое по умолчанию поддерживает привязку, для большинства элементов управления, которые включены в Xamarin. Forms. Поэтому при использовании этого метода обычно не требуется указывать целевое свойство. Однако вы также можете зарегистрировать свойство, которое можно привязать по умолчанию для дополнительных элементов управления:

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.Register(HoverButton.CommandProperty, RadialGauge.ValueProperty);
```

`Bind`Метод можно использовать для привязки к любому связанному свойству:

```csharp
using Xamarin.Forms.Markup;
// ...

new Label { Text = "No data available" }
           .Bind (Label.IsVisibleProperty, nameof(vm.Empty))
```

Кроме того, `BindCommand` метод расширения может быть привязан к свойствам и по умолчанию элемента управления `Command` `CommandParameter` в одном вызове метода:

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap me" }
              .BindCommand (nameof(vm.TapCommand))
```

По умолчанию привязка `CommandParameter` привязана к контексту привязки. Также можно указать путь привязки и источник для `Command` `CommandParameter` привязок и.

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap Me" }
              .BindCommand (nameof(vm.TapCommand), vm, nameof(Item.Id))
```

В этом примере контекст привязки является `Item` экземпляром, поэтому для привязки не нужно указывать источник `Id` `CommandParameter` .

Если требуется только привязка к `Command` , можно передать `null` в `parameterPath` аргумент `BindCommand` метода. Кроме того, можно использовать `Bind` метод.

Можно также зарегистрировать значения по умолчанию `Command` и `CommandParameter` свойства для дополнительных элементов управления:

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.RegisterCommand(
    (CustomViewA.CommandProperty, CustomViewA.CommandParameterProperty),
    (CustomViewB.CommandProperty, CustomViewB.CommandParameterProperty)
);
```

Код встроенного преобразователя можно передать в `Bind` метод с `convert` `convertBack` параметрами и:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth),
                  convert: (int depth) => new Thickness(depth * 20, 0, 0, 0))
```

Кроме того, поддерживаются параметры безопасного преобразователя:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { }
           .Bind (nameof(viewModel.Text),
                  convert: (string text, int repeat) => string.Concat(Enumerable.Repeat(text, repeat)))
```

Кроме того, код и экземпляры преобразователя можно повторно использовать с `FuncConverter` классом:

```csharp
using Xamarin.Forms.Markup;
//...

FuncConverter<int, Thickness> treeMarginConverter = new FuncConverter<int, Thickness>(depth => new Thickness(depth * 20, 0, 0, 0));
new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth), converter: treeMarginConverter),
```

`FuncConverter`Класс также поддерживает `CultureInfo` объекты:

```csharp
using Xamarin.Forms.Markup;
//...

cultureAwareConverter = new FuncConverter<DateTimeOffset, string, int>(
    (date, daysToAdd, culture) => date.AddDays(daysToAdd).ToString(culture)
);
```

Можно также выполнить привязку данных к `Span` объектам, указанным в `FormattedText` свойстве:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { } .FormattedText (
    new Span { Text = "Built with " },
    new Span { TextColor = Color.Blue, TextDecorations = TextDecorations.Underline }
              .BindTapGesture (nameof(vm.ContinueToCSharpForMarkupCommand))
              .Bind (nameof(vm.Title))
)
```

### <a name="gesture-recognizers"></a>Распознаватели жестов

`Command``CommandParameter`Свойства и могут быть привязаны к данным `GestureElement` и `View` типам с помощью `BindClickGesture` `BindSwipeGesture` `BindTapGesture` методов расширения, и.

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .BindTapGesture (nameof(vm.TapCommand))
```

В этом примере создается распознаватель жестов указанного типа, который добавляется в [`Label`](xref:Xamarin.Forms.Label) . `Bind*Gesture`Методы расширения предлагают те же параметры, что и `BindCommand` методы расширения. Однако по умолчанию `Bind*Gesture` не выполняет привязку `CommandParameter` , тогда как `BindCommand` .

Чтобы инициализировать распознаватель жестов с параметрами, используйте `ClickGesture` методы расширения,,, `PanGesture` `PinchGesture` `SwipeGesture` и `TapGesture` :

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .TapGesture (g => g.Bind(nameof(vm.DoubleTapCommand)).NumberOfTapsRequired = 2)
```

Так как распознаватель жестов — это `BindableObject` , вы можете использовать `Bind` `BindCommand` методы расширения и при его инициализации. Можно также инициализировать пользовательские типы распознавателя жестов с помощью `Gesture<TGestureElement, TGestureRecognizer>` метода расширения.

## <a name="layout"></a>Layout

Разметка C# включает ряд методов расширения макета, которые поддерживают представления позиционирования в макетах и содержимое в представлениях:

| Type | Методы расширения |
|---|---|
| `FlexLayout` | `AlignSelf`, `Basis`, `Grow`, `Menu`, `Order`, `Shrink` |
| `Grid` | `Row`, `Column`, `RowSpan`, `ColumnSpan` |
| `Label` | `TextLeft`, `TextCenterHorizontal`, `TextRight` <br/> `TextTop`, `TextCenterVertical`, `TextBottom` <br/> `TextCenter` |
| `Layout` | `Padding`, `Paddings` |
| `LayoutOptions` | `Left`, `CenterHorizontal`, `FillHorizontal`, `Right` <br/> `LeftExpand`, `CenterExpandHorizontal`, `FillExpandHorizontal`, `RightExpand` <br /> `Top`, `Bottom`, `CenterVertical`, `FillVertical` <br /> `TopExpand`, `BottomExpand`, `CenterExpandVertical`, `FillExpandVertical` <br /> `Center`, `Fill`, `CenterExpand`, `FillExpand` |
| `View` | `Margin`, `Margins` |
| `VisualElement` | `Height`, `Width`, `MinHeight`, `MinWidth`, `Size`, `MinSize` |

### <a name="left-to-right-and-right-to-left-support"></a>Поддержка слева направо и справа налево

Для разметки C#, предназначенной для поддержки направления потока слева направо (LTR) или справа налево, перечисленные выше методы расширения предлагают наиболее интуитивно понятный набор имен: `Left` , `Right` `Top` и `Bottom` .

Чтобы сделать правильный набор доступных методов расширения Left и Right, и в процессе необходимо явно определить направление потока, для которого предназначена разметка, включите одну из следующих двух `using` директив: `using Xamarin.Forms.Markup.LeftToRight;` или `using Xamarin.Forms.Markup.RightToLeft;` .

Для разметки C#, предназначенной для поддержки направления потока слева направо и справа налево, рекомендуется использовать методы расширения, приведенные в следующей таблице, а не одно из указанных выше пространств имен.

| Type | Методы расширения |
|---|---|
| `Label` | `TextStart`, `TextEnd` |
| `LayoutOptions` | `Start`, `End` <br/> `StartExpand`, `EndExpand` |

### <a name="layout-line-convention"></a>Соглашение о строках макета

Рекомендуемое соглашение — разместить все методы расширения макета для представления в одной строке в следующем порядке:

1. Строка и столбец, содержащие представление.
1. Выравнивание в строке и столбце.
1. Поля вокруг представления.
1. Размер представления.
1. Заполнение внутри представления.
1. Выравнивание содержимого в пределах заполнения.

В следующем коде показан пример этого соглашения:

```csharp
new Label { }
           .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal () // Layout line
```

Согласованное соглашение позволяет быстро прочитать разметку C# и создать карту, в которой содержимое представления находится в пользовательском интерфейсе.

## <a name="grid-rows-and-columns"></a>Строки и столбцы сетки

Перечисления можно использовать для определения [`Grid`](xref:Xamarin.Forms.Grid) строк и столбцов вместо чисел. Это обеспечивает преимущество, которое не требуется для перенумерации при добавлении или удалении строк или столбцов.

> [!IMPORTANT]
> [`Grid`](xref:Xamarin.Forms.Grid)Для определения строк и столбцов с помощью перечислений требуется следующая `using` директива:`using static Xamarin.Forms.Markup.GridRowsColumns;`

В следующем коде показан пример определения и использования [`Grid`](xref:Xamarin.Forms.Grid) строк и столбцов с помощью перечислений.

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms.Markup.LeftToRight;
using static Xamarin.Forms.Markup.GridRowsColumns;
// ...

enum BodyRow
{
    Prompt,
    CodeHeader,
    CodeEntry,
    Button
}

enum BodyCol
{
    FieldLabel,
    FieldValidation
}

View Build() => new Grid
{
    RowDefinitions = Rows.Define(
        (BodyRow.Prompt    , 170 ),
        (BodyRow.CodeHeader, 75  ),
        (BodyRow.CodeEntry , Auto),
        (BodyRow.Button    , Auto)
    ),

    ColumnDefinitions = Columns.Define(
        (BodyCol.FieldLabel     , 160 ),
        (BodyCol.FieldValidation, Star)
    ),

    Children =
    {
        new Label { LineBreakMode = LineBreakMode.WordWrap } .Font (15) .Bold ()
                   .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal ()
                   .Bind (nameof(vm.RegistrationPrompt)),

        new Label { Text = "Registration code" } .Bold ()
                   .Row (BodyRow.CodeHeader) .Column(BodyCol.FieldLabel) .Bottom () .Margin (fieldNameMargin),

        new Label { } .Italic ()
                   .Row (BodyRow.CodeHeader) .Column (BodyCol.FieldValidation) .Right () .Bottom () .Margin (fieldNameMargin)
                   .Bind (nameof(vm.RegistrationCodeValidationMessage)),

        new Entry { Placeholder = "E.g. 123456", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
                   .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
                   .Bind (nameof(vm.RegistrationCode), BindingMode.TwoWay),

        new Button { Text = "Verify" } .Style (FilledButton)
                    .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (PageMarginSize)
                    .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode))
                    .Bind (nameof(vm.VerifyRegistrationCodeCommand)),
    }
};
```

Кроме того, можно кратко определять строки и столбцы без перечислений:

```csharp
new Grid
{
    RowDefinitions = Rows.Define (Auto, Star, 20),
    ColumnDefinitions = Columns.Define (Auto, Star, 20, 40)
    // ...
}
```

## <a name="fonts"></a>Шрифты

Элементы управления в следующем списке могут вызывать `FontSize` `Bold` методы расширения,, `Italic` и, `Font` чтобы задать внешний вид текста, отображаемого элементом управления:

- `Button`
- `DatePicker`
- `Editor`
- `Entry`
- `Label`
- `Picker`
- `SearchBar`
- `Span`
- `TimePicker`

## <a name="effects"></a>Произведенный эффект

Эффекты можно прикреплять к элементам управления с помощью `Effect` метода расширения:

```csharp
using Xamarin.Forms.Markup;
// ...

new Button { Text = "Tap Me" }
            .Effects (new ButtonMixedCaps())
```

## <a name="logic-integration"></a>Интеграция логики

`Invoke`Метод расширения можно использовать для выполнения кода, встроенного в разметку C#:

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Invoke (l => l.ItemTapped += OnListViewItemTapped)
```

Кроме того, можно использовать `Assign` метод расширения для доступа к элементу управления вне разметки ПОЛЬЗОВАТЕЛЬСКОГО интерфейса (в файле логики пользовательского интерфейса):

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Assign (out MyListView)
```

## <a name="styles"></a>Стили

В следующем примере показано, как создать явные и неявные стили с помощью разметки C#:

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms;

namespace CSharpForMarkupDemos
{
    public static class Styles
    {
        static Style<Button> buttons, filledButton;
        static Style<Label> labels;
        static Style<Span> link;

        #region Implicit styles

        public static ResourceDictionary Implicit => new ResourceDictionary { Buttons, Labels };

        public static Style<Button> Buttons => buttons ?? (buttons = new Style<Button>(
            (Button.HeightRequestProperty, 44),
            (Button.FontSizeProperty, 13),
            (Button.HorizontalOptionsProperty, LayoutOptions.Center),
            (Button.VerticalOptionsProperty, LayoutOptions.Center)
        ));

        public static Style<Label> Labels => labels ?? (labels = new Style<Label>(
            (Label.FontSizeProperty, 13),
            (Label.TextColorProperty, Color.Black)
        ));

        #endregion Implicit styles

        #region Explicit styles

        public static Style<Button> FilledButton => filledButton ?? (filledButton = new Style<Button>(
            (Button.TextColorProperty, Color.White),
            (Button.BackgroundColorProperty, Color.FromHex("#1976D2")),
            (Button.CornerRadiusProperty, 5)
        )).BasedOn(Buttons);

        public static Style<Span> Link => link ?? (link = new Style<Span>(
            (Span.TextColorProperty, Color.Blue),
            (Span.TextDecorationsProperty, TextDecorations.Underline)
        ));

        #endregion Explicit styles
    }
}
```

Неявные стили можно использовать, загрузив их в словарь ресурсов приложения:

```csharp
public App()
{
    Resources = Styles.Implicit;
    // ...
}
```

С помощью метода расширения можно использовать явные стили `Style` .

```csharp
using static CSharpForMarkupExample.Styles;
// ...

new Button { Text = "Tap Me" } .Style (FilledButton),
```

> [!NOTE]
> Помимо `Style` метода расширения, существуют `ApplyToDerivedTypes` `BasedOn` `Add` методы расширения,, и `CanCascade` .

Кроме того, можно создавать собственные методы расширения стиля:

```csharp
public static TButton Filled<TButton>(this TButton button) where TButton : Button
{
    button.Buttons(); // Equivalent to Style .BasedOn (Buttons)
    button.TextColor = Color.White;
    button.BackgroundColor = Color.Red;
    return button;
}
```

`Filled`Затем можно использовать метод расширения следующим образом:

```csharp
new Button { Text = "Tap Me" } .Filled ()
```

## <a name="platform-specifics"></a>Особенности платформы

`Invoke`Метод расширения можно использовать для применения зависящих от платформы. Однако, чтобы избежать ошибок неоднозначности, не включайте `using` непосредственно директивы для `Xamarin.Forms.PlatformConfiguration.*Specific` пространств имен. Вместо этого создайте псевдоним пространства имен и используйте зависящую от платформы через псевдоним:

```csharp
using Xamarin.Forms.Markup;
using PciOS = Xamarin.Forms.PlatformConfiguration.iOSSpecific;
// ...

new ListView { } .Invoke (l => PciOS.ListView.SetGroupHeaderStyle(l, PciOS.GroupHeaderStyle.Grouped))
```

Кроме того, если вы часто потребляете определенные платформы, вы можете создать для них методы расширения Fluent в своем собственном классе Extensions:

```csharp
public static T iOSGroupHeaderStyle<T>(this T listView, PciOS.GroupHeaderStyle style) where T : Forms.ListView
{
  PciOS.ListView.SetGroupHeaderStyle(listView, style);
  return listView;
}
```

Затем можно использовать метод расширения следующим образом:

```csharp
new ListView { } .iOSGroupHeaderStyle(PciOS.GroupHeaderStyle.Grouped)
```

Дополнительные сведения о конкретных платформах см. в разделе [функции платформы Android](~/xamarin-forms/platform/android/index.md), [функции платформы iOS](~/xamarin-forms/platform/ios/index.md)и [функции платформы Windows](~/xamarin-forms/platform/windows/index.md).

## <a name="recommended-convention"></a>Рекомендуемое соглашение

Рекомендуемый порядок и группировка свойств и вспомогательных методов:

- **Назначение**. любые свойства или вспомогательные методы, значение которых определяет назначение элемента управления (например `Text` ,, `Placeholder` . `Assign` ).
- **Другие**: все свойства или вспомогательные методы, не являющиеся макетом или привязкой, в одной строке или нескольких строках.
- **Макет**: макет упорядочивается в сторону: строки и столбцы, параметры макета, поля, размер, заполнение и выравнивание содержимого.
- **BIND**: привязка данных выполняется в конце цепочки методов с одним привязанным свойством в строке. Если привязанное *по умолчанию* свойство имеет привязку, оно должно находиться в конце цепочки методов.

В следующем коде показан пример следующего соглашения:

```csharp
new Button { Text = "Verify" /* purpose */ } .Style (FilledButton) // other
            .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (10) // layout
            .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode)) // bind
            .Bind (nameof(vm.VerifyRegistrationCodeCommand)), // bind default

new Label { }
           .Assign (out animatedMessageLabel) // purpose
           .Invoke (label => label.SizeChanged += MessageLabel_SizeChanged) // other
           .Row (BodyRow.Message) .ColumnSpan (All<BodyCol>()) // layout
           .Bind (nameof(vm.Message)), // bind default
```

Согласованное применение этого соглашения позволяет быстро сканировать разметку C# и создавать схему макета пользовательского интерфейса.

## <a name="related-links"></a>Связанные ссылки

- [Кшарпформаркупдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)
- [Возможности платформы Android](~/xamarin-forms/platform/android/index.md)
- [возможности платформы iOS](~/xamarin-forms/platform/ios/index.md)
- [Возможности платформы Windows](~/xamarin-forms/platform/windows/index.md)
