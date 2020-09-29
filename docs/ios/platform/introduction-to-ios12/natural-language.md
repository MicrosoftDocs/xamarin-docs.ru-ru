---
title: Использование платформы естественного языка с Xamarin. iOS
description: В этом документе описывается платформа естественного языка. Платформа естественного языка, появившаяся в iOS 12, является предпочтительным API iOS для использования при распознавании языка, части идентификации речи и распознавании именованных сущностей.
ms.prod: xamarin
ms.assetid: 126C8764-F873-4EB9-98A3-D82AB5689111
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/20/2018
ms.openlocfilehash: 3d559f197ffc1696c4fd9f7beff7b107103142d3
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434187"
---
# <a name="using-the-natural-language-framework-with-xamarinios"></a>Использование платформы естественного языка с Xamarin. iOS

Платформа естественного языка, появившаяся в iOS 12, обеспечивает обработку на естественном языке на устройстве. Он поддерживает распознавание языка, разметку и добавление тегов. Разметка разделяет текст на его составные слова, предложения или абзацы; Теги обозначают части речи, людей, мест и организаций.

Платформа естественного языка также может использовать пользовательские базовые модели машинного обучения для классификации и маркировки текста в специализированных контекстах.

Класс [нслингуистиктагжер](xref:Foundation.NSLinguisticTagger) по-прежнему доступен. Однако платформа естественного языка является предпочтительным механизмом для обработки на естественном языке.

## <a name="sample-app-xamarinnl"></a>Пример приложения: Ксамариннл

Чтобы узнать, как использовать платформу естественного языка с Xamarin. iOS, ознакомьтесь с [примером приложения ксамариннл](/samples/xamarin/ios-samples/ios12-xamarinnl).
В этом примере приложения демонстрируется использование платформы естественного языка для:

- [Распознавание языков](#recognizing-languages).
- [Разбиение текста на слова и предложения](#tokenizing-text-into-words-sentences-and-paragraphs).
- [Пометка именованных сущностей и частей речи](#tagging-named-entities-and-parts-of-speech).

## <a name="recognizing-languages"></a>Распознавание языков

На вкладке **распознавателя** примера приложения демонстрируется использование элемента [`NLLanguageRecognizer`](xref:NaturalLanguage.NLLanguageRecognizer)
для определения языка блока текста.

> [!NOTE]
> Распознавание языка — это конкретный тип классификации текста. Платформа естественного языка также поддерживает пользовательскую классификацию текста с помощью предоставляемых разработчиком основных моделей машинного обучения. Дополнительные сведения см. в статьях [Знакомство с языком естественной языковой инфраструктуры](https://developer.apple.com/videos/play/wwdc2018/713/) из ввдк 2018.

### <a name="dominant-language"></a>Главный язык

Нажмите кнопку **язык** , чтобы указать главный язык в введенных данных пользователя.

`HandleDetermineLanguageButtonTap`Метод объекта `LanguageRecognizerViewController` использует[`GetDominantLanguage`](xref:NaturalLanguage.NLLanguageRecognizer.GetDominantLanguage*)
метод `NLLanguageRecognizer` для выборки [`NLLanguage`](xref:NaturalLanguage.NLLanguage)
для основного языка, найденного в тексте:

```csharp
partial void HandleDetermineLanguageButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        NLLanguage lang = NLLanguageRecognizer.GetDominantLanguage(UserInput.Text);
        DominantLanguageLabel.Text = lang.ToString();
    }
}
```

### <a name="language-probabilities"></a>Вероятности языка

Коснитесь кнопки **вероятности языка** , чтобы получить список языковых значений для вводимых пользователем данных.

`HandleLanguageProbabilitiesButtonTap`Метод `LanguageRecognizerViewController` класса создает экземпляр `NLLanguageRecognizer` и запрашивает его[`Process`](xref:NaturalLanguage.NLLanguageRecognizer.Process*)
текст пользователя. Затем он вызывает распознаватель языка [`GetNativeLanguageHypotheses`](xref:NaturalLanguage.NLLanguageRecognizer.GetNativeLanguageHypotheses*)
метод, который извлекает словарь языков и связанных с ними вероятностей. `LanguageRecognizerTableViewController`Затем класс визуализирует эти языки и вероятности.

```csharp
partial void HandleLanguageProbabilitiesButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var recognizer = new NLLanguageRecognizer();
        recognizer.Process(UserInput.Text);
        NSDictionary<NSString, NSNumber> probabilities = recognizer.GetNativeLanguageHypotheses(10);
        PerformSegue(ShowLanguageProbabilitiesSegue, this);
    }
}
```

Возможные `NLLanguage` значения:

- `Amharic`
- `Arabic`
- `Armenian`
- `Bengali`
- `Bulgarian`
- `Burmese`
- `Catalan`
- `Cherokee`
- `Croatian`
- `Czech`
- `Danish`
- `Dutch`
- `English`
- `Finnish`
- `French`
- `Georgian`
- `German`
- `Greek`
- `Gujarati`
- `Hebrew`
- `Hindi`
- `Hungarian`
- `Icelandic`
- `Indonesian`
- `Italian`
- `Japanese`
- `Kannada`
- `Khmer`
- `Korean`
- `Lao`
- `Malay`
- `Malayalam`
- `Marathi`
- `Mongolian`
- `Norwegian`
- `Oriya`
- `Persian`
- `Polish`
- `Portuguese`
- `Punjabi`
- `Romanian`
- `Russian`
- `SimplifiedChinese`
- `Sinhalese`
- `Slovak`
- `Spanish`
- `Swedish`
- `Tamil`
- `Telugu`
- `Thai`
- `Tibetan`
- `TraditionalChinese`
- `Turkish`
- `Ukrainian`
- `Undetermined`
- `Urdu`
- `Vietnamese`

Полный список поддерживаемых языков доступен в составе [`NLLanguage`](xref:NaturalLanguage.NLLanguage)
Документация по API перечисления.

## <a name="tokenizing-text-into-words-sentences-and-paragraphs"></a>Разбиение текста на слова, предложения и абзацы

На вкладке " **лексема** " примера приложения показано, как разделить блок текста на его составные слова или предложения с помощью [`NLTokenizer`](xref:NaturalLanguage.NLTokenizer) .

Нажмите кнопку **слова** или **предложения** , чтобы получить список токенов. Каждый токен связан с словом или предложением в исходном тексте.

`ShowTokens` разделяет входные данные пользователя на маркеры, вызывая метод [`GetTokens`](xref:NaturalLanguage.NLTokenizer.GetTokens*)
метод класса `NLTokenizer` . Этот метод возвращает массив [`NSValue`](xref:Foundation.NSValue)
объекты, каждый из которых является оболочкой `NSRange` для значения, соответствующего маркеру в исходном тексте.

```csharp
void ShowTokens(NLTokenUnit unit)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tokenizer = new NLTokenizer(unit);
        tokenizer.String = UserInput.Text;
        var range = new NSRange(0, UserInput.Text.Length);
        NSValue[] tokens = tokenizer.GetTokens(range);
        PerformSegue(ShowTokensSegue, this);
    }
}
```

`LanguageTokenizerTableViewController` отображает один маркер в каждой ячейке таблицы. Он извлекает `NSRange` из маркера `NSValue` , находит соответствующую строку в исходном тексте и задает метку в ячейке представления таблицы:

```csharp
public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
    var cell = TableView.DequeueReusableCell(TokenCell);
    NSRange range = Tokens[indexPath.Row].RangeValue;
    cell.TextLabel.Text = Text.Substring((int)range.Location, (int)range.Length);
    return cell;
}
```

## <a name="tagging-named-entities-and-parts-of-speech"></a>Разметка именованных сущностей и частей речи

На вкладке Создание **тегов** примера приложения ксамариннл показано, как использовать [`NLTagger`](xref:NaturalLanguage.NLTagger)
класс, связывающий категории с токенами входной строки.
Платформа естественного языка включает встроенную поддержку распознавания людей, мест, организаций и частей речи.

> [!NOTE]
> Платформа естественного языка также поддерживает пользовательские схемы тегов с помощью предоставляемых разработчиком основных моделей машинного обучения. Дополнительные сведения см. в статьях [Знакомство с языком естественной языковой инфраструктуры](https://developer.apple.com/videos/play/wwdc2018/713/) из ввдк 2018.

Коснитесь **именованных сущностей** или **частей кнопки речи** , чтобы получить:

- Массив `NSValue` объектов, каждый из которых является оболочкой `NSRange` для маркера в исходном тексте.
- Массив [`NLTag`](xref:NaturalLanguage.NLTag) значений — категории для `NSValue` токенов по одному индексу массива.

В `LanguageTaggerViewController` `HandlePartsOfSpeechButtonTap` и `HandleNamedEntitiesButtonTap` каждый вызов `ShowTags` передается на [`NLTagScheme`](xref:NaturalLanguage.NLTagScheme) – либо `NLTagScheme.LexicalClass` (для частей речи), либо `NLTagScheme.NameType` (для именованных сущностей).

`ShowTags` создает экземпляр `NLTagger` класса с массивом `NLTagScheme` типов, для которых он будет запрашиваться (в данном случае только переданное `NLTagScheme` значение). Затем он использует [`GetTags`](xref:NaturalLanguage.NLTagger.GetTags*)
`NLTagger`для определения тегов, относящихся к тексту во входных данных пользователя.

```csharp
void ShowTags(NLTagScheme tagScheme)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tagger = new NLTagger(new NLTagScheme[] { tagScheme });
        var range = new NSRange(0, UserInput.Text.Length);
        tagger.String = UserInput.Text;

        NLTag[] tags = tagger.GetTags(range, NLTokenUnit.Word, tagScheme, NLTaggerOptions.OmitWhitespace, out NSValue[] ranges);
        NSValue[] tokenRanges = ranges;
        detailViewTitle = tagScheme == NLTagScheme.NameType ? "Named Entities" : "Parts of Speech";

        PerformSegue(ShowEntitiesSegue, this);
    }
}
```

Затем Теги отображаются в таблице с помощью `LanguageTaggerTableViewController` .

Возможные `NLTag` значения:

- `Adjective`
- `Adverb`
- `Classifier`
- `CloseParenthesis`
- `CloseQuote`
- `Conjunction`
- `Dash`
- `Determiner`
- `Idiom`
- `Interjection`
- `Noun`
- `Number`
- `OpenParenthesis`
- `OpenQuote`
- `OrganizationName`
- `Other`
- `OtherPunctuation`
- `OtherWhitespace`
- `OtherWord`
- `ParagraphBreak`
- `Particle`
- `PersonalName`
- `PlaceName`
- `Preposition`
- `Pronoun`
- `Punctuation`
- `SentenceTerminator`
- `Verb`
- `Whitespace`
- `Word`
- `WordJoiner`

Полный список поддерживаемых тегов доступен в составе [`NLTag`](xref:NaturalLanguage.NLTag)
Документация по API перечисления.

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения — Ксамариннл](/samples/xamarin/ios-samples/ios12-xamarinnl)
- [Знакомство с платформой естественного языка](https://developer.apple.com/videos/play/wwdc2018/713/)
- [Естественный язык (Apple)](https://developer.apple.com/documentation/naturallanguage?language=objc)