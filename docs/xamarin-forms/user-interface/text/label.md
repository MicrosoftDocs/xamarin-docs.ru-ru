---
title: Метка Xamarin. Forms
description: В этой статье объясняется, как использовать класс Label Xamarin. Forms для отображения одного и многострочного текста в приложениях.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: 6ce4e39986afb7444e7d126e9789760d9311ca45
ms.sourcegitcommit: 737c7fbbe8aed1f33110a5217d7e6d6ef3e5b785
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/05/2020
ms.locfileid: "82793226"
---
# <a name="xamarinforms-label"></a>Метка Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Отображение текста в Xamarin. Forms_

[`Label`](xref:Xamarin.Forms.Label) Представление используется для отображения текста как с одним, так и с несколькими строками. Метки могут иметь оформление текста, цветной текст и использовать пользовательские шрифты (семейства, размеры и параметры).

## <a name="text-decorations"></a>Оформление текста

Оформление текста подчеркиванием и зачеркиванием [`Label`](xref:Xamarin.Forms.Label) можно применить к экземплярам, присвоив `Label.TextDecorations` свойству одно или несколько `TextDecorations` членов перечисления:

- `None`
- `Underline`
- `Strikethrough`

В следующем примере XAML показано задание `Label.TextDecorations` свойства:

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

Эквивалентный код на C# выглядит так:

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

На следующих снимках экрана `TextDecorations` показаны члены перечисления, [`Label`](xref:Xamarin.Forms.Label) применяемые к экземплярам:

![Метки с оформлением текста](label-images/label-textdecorations.png)

> [!NOTE]
> Оформление текста также может применяться к [`Span`](xref:Xamarin.Forms.Span) экземплярам. Дополнительные сведения о классе см `Span` . в разделе [форматированный текст](#Formatted_Text).

## <a name="character-spacing"></a>Интервалы между символами

Интервал между символами можно применять [`Label`](xref:Xamarin.Forms.Label) к экземплярам, `Label.CharacterSpacing` присвоив свойству `double` значение.

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

Эквивалентный код на C# выглядит так:

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

Результат заключается в том, что символы в тексте, [`Label`](xref:Xamarin.Forms.Label) отображаемые в `CharacterSpacing` , разбиваются на устройства, независимые от устройств.

## <a name="new-lines"></a>Символы перевода строки

Существует два основных способа принудительного ввода текста в [`Label`](xref:Xamarin.Forms.Label) новую строку из XAML:

1. Используйте символ перевода строки в Юникоде, который имеет значение&amp;"#10;".
1. Укажите текст с помощью синтаксиса *элемента свойства* .

В следующем коде показан пример обоих методов.

```xaml
<!-- Unicode line feed character -->
<Label Text="First line &#10; Second line" />

<!-- Property element syntax -->
<Label>
    <Label.Text>
        First line
        Second line
    </Label.Text>
</Label>
```

В C# текст может быть принудительно помещен в новую строку с символом "\n":

```csharp
Label label = new Label { Text = "First line\nSecond line" };
```

## <a name="colors"></a>Цвета

Метки могут быть настроены для использования пользовательского цвета текста через [`TextColor`](xref:Xamarin.Forms.Label.TextColor) свойство, доступное для привязки.

Чтобы обеспечить возможность использования цветов на каждой платформе, необходимо особое внимание. Так как каждая платформа имеет разные значения по умолчанию для цветов текста и фона, необходимо соблюдать осторожность при выборе значения по умолчанию, которое работает для каждого из них.

В следующем примере XAML задается цвет текста `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a green label." />
    </StackLayout>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a green label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

На следующих снимках экрана показан результат установки `TextColor` свойства:

![Пример метки TextColor](label-images/textcolor.png)

Дополнительные сведения о цветах см. в разделе [цвета](~/xamarin-forms/user-interface/colors.md).

## <a name="fonts"></a>Шрифты

Дополнительные сведения об указании шрифтов в см `Label`. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Усечение и перенос

Метки могут быть настроены на обработку текста, который не может поместиться в одну строку одним из нескольких способов, `LineBreakMode` предоставляемых свойством. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)— Это перечисление со следующими значениями:

- **Хеадтрункатион** &ndash; усекает заголовок текста, отображая конец.
- **Чарактерврап** &ndash; заключает текст на новую строку с границей символа.
- **Миддлетрункатион** &ndash; отображает начало и конец текста, а середина заменяется многоточием.
- Не переносит текст, отображая только столько текста, сколько может уместиться в одной строке. **NoWrap** &ndash;
- **Таилтрункатион** &ndash; показывает начало текста с усечением конца.
- Перенос текста на границе слова. **WordWrap** &ndash;

## <a name="display-a-specific-number-of-lines"></a>Отображение определенного числа строк

Количество строк, отображаемых, [`Label`](xref:Xamarin.Forms.Label) можно задать, задав для `Label.MaxLines` свойства `int` значение:

- Если `MaxLines` равно-1, то есть значение по умолчанию, `Label` то [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) свойство учитывает значение свойства, чтобы отобразить только одну строку, возможно усечение или все строки со всем текстом.
- Если `MaxLines` значение равно 0, `Label` элемент не отображается.
- Если `MaxLines` имеет значение 1, результат идентичен присвоению [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) свойству значения [`NoWrap`](xref:Xamarin.Forms.LineBreakMode), [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode), [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)или. [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode) Однако [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) свойство будет `Label` учитывать значение свойства в отношении размещения многоточия, если это применимо.
- Если `MaxLines` значение больше 1, то `Label` будет отображаться до указанного числа строк, при этом в зависимости от значения [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) свойства учитывается положение многоточия (если применимо). Однако установка `MaxLines` свойства в значение больше 1 не оказывает никакого влияния, если [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) свойство имеет значение. [`NoWrap`](xref:Xamarin.Forms.LineBreakMode)

В следующем примере XAML показано задание `MaxLines` свойства для: [`Label`](xref:Xamarin.Forms.Label)

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

Эквивалентный код на C# выглядит так:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

На следующих снимках экрана показан результат установки `MaxLines` свойства равным 2, если текст достаточно длинный, чтобы занимать более 2 строк:

![Пример метки Макслинес](label-images/label-maxlines.png)

## <a name="display-html"></a>Отображение HTML

[`Label`](xref:Xamarin.Forms.Label) Класс имеет `TextType` свойство, которое определяет, должен ли `Label` экземпляр отображать обычный текст или HTML-текст. Этому свойству должно быть присвоено значение одного из членов `TextType` перечисления:

- `Text`Указывает, что `Label` будет отображаться обычный текст, а — значение `Label.TextType` свойства по умолчанию.
- `Html`Указывает, что `Label` будет отображать HTML-текст.

Таким образом [`Label`](xref:Xamarin.Forms.Label) , экземпляры могут отображать HTML, присвоив `Label.TextType` свойству значение `Html`, `Label.Text` а свойству — строке HTML:

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

В приведенном выше примере символы двойной кавычки в HTML должны быть экранированы с помощью `\` символа.

В XAML строки HTML могут стать нечитаемыми из-за дополнительного экранирования символов `<` и `>` .

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

Кроме того, для повышения удобочитаемости HTML может быть встроен в `CDATA` раздел:

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

В этом примере `Label.Text` свойству присваивается строка HTML, встроенная в `CDATA` раздел. Это работает потому, `Text` что свойство имеет `ContentProperty` значение для `Label` класса.

На следующих снимках экрана [`Label`](xref:Xamarin.Forms.Label) показан отображаемый HTML-код:

![Снимки экрана метки, отображающей HTML, в iOS и Android](label-images/label-html.png)

> [!IMPORTANT]
> Отображение HTML в [`Label`](xref:Xamarin.Forms.Label) ограничено тегами HTML, которые поддерживаются базовой платформой.

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Форматированный текст

Метки предоставляют [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) свойство, которое позволяет просматривать текст с несколькими шрифтами и цветами в одном и том же представлении.

`FormattedText` Свойство имеет тип [`FormattedString`](xref:Xamarin.Forms.FormattedString), который состоит из одного или нескольких [`Span`](xref:Xamarin.Forms.Span) экземпляров, задается через [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) свойство. Для настройки `Span` внешнего вида можно использовать следующие свойства:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)— цвет фона диапазона.
- `CharacterSpacing` с типом `double` представляет собой интервал между знаками текста `Span`.
- [`Font`](xref:Xamarin.Forms.Span.Font)— шрифт текста в диапазоне.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)— атрибуты шрифта для текста в диапазоне.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)— семейство шрифтов, которому принадлежит шрифт текста в диапазоне.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)— Размер шрифта для текста в диапазоне.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)— цвет текста в диапазоне. Это свойство устарело и было заменено `TextColor` свойством.
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)— коэффициент, применяемый к высоте строки диапазона по умолчанию. Дополнительные сведения см. в разделе [Height строки](#line-height).
- [`Style`](xref:Xamarin.Forms.Span.Style)— стиль, применяемый к диапазону.
- [`Text`](xref:Xamarin.Forms.Span.Text)— текст диапазона.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)— цвет текста в диапазоне.
- `TextDecorations`— Оформление, применяемое к тексту в диапазоне. Дополнительные сведения см. в разделе [Оформление текста](#text-decorations).

Свойства [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor), [`Text`](xref:Xamarin.Forms.Span.Text)и [`Text`](xref:Xamarin.Forms.Span.Text) , допускающие привязку, имеют режим привязки по [`OneWay`](xref:Xamarin.Forms.BindingMode)умолчанию. Дополнительные сведения об этом режиме привязки см. в описании [режима привязки по умолчанию](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode) в разделе " [режим привязки](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md) ".

Кроме того, [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) свойство можно использовать для определения коллекции распознавателей жестов, которые будут реагировать на жесты на [`Span`](xref:Xamarin.Forms.Span).

> [!NOTE]
> Невозможно отобразить HTML в [`Span`](xref:Xamarin.Forms.Span).

В следующем примере XAML демонстрируется `FormattedText` свойство, состоящее из [`Span`](xref:Xamarin.Forms.Span) трех экземпляров:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label LineBreakMode="WordWrap">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}">
                        <Span.GestureRecognizers>
                            <TapGestureRecognizer Command="{Binding TapCommand}" />
                        </Span.GestureRecognizers>
                    </Span>
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });

        var span = new Span { Text = "default, " };
        span.GestureRecognizers.Add(new TapGestureRecognizer { Command = new Command(async () => await DisplayAlert("Tapped", "This is a tapped Span.", "OK")) });
        formattedString.Spans.Add(span);
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!IMPORTANT]
> [`Text`](xref:Xamarin.Forms.Span.Text) Свойство объекта `Span` можно задать с помощью привязки данных. Более подробную информацию см. в разделе [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Обратите внимание [`Span`](xref:Xamarin.Forms.Span) , что может также реагировать на любые жесты, добавленные в [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) коллекцию диапазона. Например, [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) к второму `Span` из приведенных выше примеров кода добавляется. Таким образом, при `Span` нажатии этой кнопки `TapGestureRecognizer` будет получен ответ, выполняя `ICommand` объект [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) , определенный свойством. Дополнительные сведения о распознавателях жестов см. в разделе [жесты Xamarin. Forms](~/xamarin-forms/app-fundamentals/gestures/index.md).

На следующих снимках экрана показан результат установки `FormattedString` свойства в три `Span` экземпляра:

![Пример метки FormattedText](label-images/formattedtext.png)

## <a name="line-height"></a>Высота линии

[`Label`](xref:Xamarin.Forms.Label) Вертикальная высота и [`Span`](xref:Xamarin.Forms.Span) может быть настроена путем задания [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) свойства или [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) `double` значения. В iOS и Android эти значения являются множителями исходной высоты строки, а на универсальная платформа Windows (UWP) значение `Label.LineHeight` свойства является множителем размера шрифта метки.

> [!NOTE]
>
> - В iOS свойства [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) и [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) изменяют высоту строки текста, находящуюся в одной строке, и текст, который переносится на несколько строк.
> - В Android свойства [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) и [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) изменяют только высоту строки текста, которая переносится на несколько строк.
> - В UWP [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) свойство изменяет высоту строки текста, которая переносится на несколько строк, и [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) свойство не оказывает никакого влияния.

В следующем примере XAML показано задание [`LineHeight`](xref:Xamarin.Forms.Label.LineHeight) свойства для: [`Label`](xref:Xamarin.Forms.Label)

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

Эквивалентный код на C# выглядит так:

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

На следующих снимках экрана показан результат установки [`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight) свойства в значение 1,8:

![Пример метки LineHeight](label-images/label-lineheight.png)

В следующем примере XAML показано задание [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight) свойства для: [`Span`](xref:Xamarin.Forms.Span)

```xaml
<Label LineBreakMode="WordWrap">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. "
                  LineHeight="1.8"/>
            <Span Text="Nullam feugiat sodales elit, et maximus nibh vulputate id."
                  LineHeight="1.8" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Эквивалентный код на C# выглядит так:

```csharp
var formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. ",
  LineHeight = 1.8
});
formattedString.Spans.Add(new Span
{
  Text = "Nullam feugiat sodales elit, et maximus nibh vulputate id.",
  LineHeight = 1.8
});
var label = new Label
{
  FormattedText = formattedString,
  LineBreakMode = LineBreakMode.WordWrap
};
```

На следующих снимках экрана показан результат установки [`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight) свойства в значение 1,8:

![Пример span LineHeight](label-images/span-lineheight.png)

## <a name="padding"></a>Заполнение

Заполнение представляет пространство между элементом и его дочерними элементами и используется для отделения элемента от его собственного содержимого. Заполнение можно применять к [`Label`](xref:Xamarin.Forms.Label) экземплярам, присвоив `Label.Padding` свойству [`Thickness`](xref:Xamarin.Forms.Thickness) значение.

```xaml
<Label Padding="10">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum" />
            <Span Text="dolor sit amet." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Эквивалентный код на C# выглядит так:

```csharp
FormattedString formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum"
});
formattedString.Spans.Add(new Span
{
  Text = "dolor sit amet."
});
Label label = new Label
{
    FormattedText = formattedString,
    Padding = new Thickness(20)
};
```

> [!IMPORTANT]
> В iOS при [`Label`](xref:Xamarin.Forms.Label) создании, который задает `Padding` свойство, будет применено заполнение, а значение заполнения можно будет обновить позже. Однако если создается объект `Label` , который не задает `Padding` свойство, попытка его установки будет невозможным.
>
> В Android и универсальная платформа Windows значение `Padding` свойства может быть указано при `Label` создании или более поздней версии.

Дополнительные сведения о заполнении см. в разделе [поля и заполнение](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## <a name="hyperlinks"></a>Гиперссылки

Текст, [`Label`](xref:Xamarin.Forms.Label) отображаемый экземплярами и [`Span`](xref:Xamarin.Forms.Span) , можно преобразовать в гиперссылки с помощью следующего подхода:

1. Задайте свойства `TextColor` и `TextDecoration` объекта [`Label`](xref:Xamarin.Forms.Label) или [`Span`](xref:Xamarin.Forms.Span).
1. [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) Добавьте [`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers) в коллекцию объекта [`Label`](xref:Xamarin.Forms.Label) или [`Span`](xref:Xamarin.Forms.Span), свойство которого [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) привязано к `ICommand`, свойство которого [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) содержит URL-адрес для открытия.
1. Определите объект `ICommand` , который будет выполняться [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer).
1. Напишите код, который будет выполняться `ICommand`.

В следующем примере кода, взятом из примера [демонстрации гиперссылок](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) , показано, [`Label`](xref:Xamarin.Forms.Label) содержимое которого задается из [`Span`](xref:Xamarin.Forms.Span) нескольких экземпляров:

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Alternatively, click " />
            <Span Text="here"
                  TextColor="Blue"
                  TextDecorations="Underline">
                <Span.GestureRecognizers>
                    <TapGestureRecognizer Command="{Binding TapCommand}"
                                          CommandParameter="https://docs.microsoft.com/xamarin/" />
                </Span.GestureRecognizers>
            </Span>
            <Span Text=" to view Xamarin documentation." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

В этом примере первый и третий [`Span`](xref:Xamarin.Forms.Span) экземпляры состоят из текста, а вторая `Span` — для касания. Цвет текста установлен синим цветом и имеет подчеркивание текста. При этом создается внешний вид гиперссылки, как показано на следующих снимках экрана:

[![Гиперссылки](label-images/hyperlinks-small.png "Гиперссылки")](label-images/hyperlinks-large.png#lightbox)

При нажатии этой гиперссылки объект [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) будет отвечать, выполняя `ICommand` объект, определенный [`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command) ее свойством. Кроме того, URL-адрес, заданный [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) свойством, будет передан `ICommand` в качестве параметра.

Код программной части для страницы XAML содержит `TapCommand` реализацию:

```csharp
public partial class MainPage : ContentPage
{
    // Launcher.OpenAsync is provided by Xamarin.Essentials.
    public ICommand TapCommand => new Command<string>(async (url) => await Launcher.OpenAsync(url));

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }
}
```

`TapCommand` Выполняет `Launcher.OpenAsync` метод, передавая значение [`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) свойства в качестве параметра. `Launcher.OpenAsync` Метод предоставляется Xamarin. Essentials и открывает URL-адрес в веб-браузере. Таким образом, общий результат заключается в том, что при касании гиперссылки на странице появляется веб-браузер, и открывается URL-адрес, связанный с гиперссылкой.

### <a name="creating-a-reusable-hyperlink-class"></a>Создание повторно используемого класса гиперссылки

Предыдущий подход к созданию гиперссылки требует написания повторяющегося кода каждый раз, когда требуется гиперссылка в приложении. Однако классы [`Label`](xref:Xamarin.Forms.Label) и [`Span`](xref:Xamarin.Forms.Span) могут быть созданы в виде подклассов для создания `HyperlinkLabel` классов `HyperlinkSpan` и с добавлением в него кода для распознавания и форматирования текста.

В следующем примере кода, взятом из примера [демонстрации гиперссылок](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/) , показан `HyperlinkSpan` класс:

```csharp
public class HyperlinkSpan : Span
{
    public static readonly BindableProperty UrlProperty =
        BindableProperty.Create(nameof(Url), typeof(string), typeof(HyperlinkSpan), null);

    public string Url
    {
        get { return (string)GetValue(UrlProperty); }
        set { SetValue(UrlProperty, value); }
    }

    public HyperlinkSpan()
    {
        TextDecorations = TextDecorations.Underline;
        TextColor = Color.Blue;
        GestureRecognizers.Add(new TapGestureRecognizer
        {
            // Launcher.OpenAsync is provided by Xamarin.Essentials.
            Command = new Command(async () => await Launcher.OpenAsync(Url))
        });
    }
}
```

`HyperlinkSpan` Класс `Url` определяет свойство и связывается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), и конструктор задает внешний вид гиперссылки и объект [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) , который будет реагировать при касании гиперссылки. При `HyperlinkSpan` нажатии на `TapGestureRecognizer` будет получен ответ, выполнив `Launcher.OpenAsync` метод, чтобы открыть URL-адрес, заданный `Url` свойством, в веб-браузере.

`HyperlinkSpan` Класс можно использовать, добавив в XAML экземпляр класса:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:HyperlinkDemo"
             x:Class="HyperlinkDemo.MainPage">
    <StackLayout>
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Alternatively, click " />
                    <local:HyperlinkSpan Text="here"
                                         Url="https://docs.microsoft.com/appcenter/" />
                    <Span Text=" to view AppCenter documentation." />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

## <a name="styling-labels"></a>Метки стилей

В предыдущих разделах рассматривались [`Label`](xref:Xamarin.Forms.Label) параметры [`Span`](xref:Xamarin.Forms.Span) и свойства для каждого экземпляра. Однако наборы свойств можно сгруппировать в один стиль, который постоянно применяется к одному или нескольким представлениям. Это может повысить удобочитаемость кода и упростить реализацию изменений в проекте. Дополнительные сведения см. в разделе [стили](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Гиперссылки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [Создание мобильных приложений с помощью Xamarin. Forms, глава 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [API Label](xref:Xamarin.Forms.Label)
- [API span](xref:Xamarin.Forms.Span)
