---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 3. Deeper into text''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5423a9f716f384eca107003bdeca69615f8b459f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136907"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Сводная информация о главе 3. Более подробная информация

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)

В этой главе рассматриваются более подробные сведения о представлении [`Label`](xref:Xamarin.Forms.Label), в том числе о цветах, шрифтах и форматировании.

## <a name="wrapping-paragraphs"></a>Перенос абзацев

Если свойство [`Text`](xref:Xamarin.Forms.Label.Text) объекта `Label` содержит длинный текст, `Label` автоматически переносит его на несколько строк, как показано в примере [**Baskervilles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles). Вы можете использовать здесь символы Юникода, например длинное тире "\u2014", и (или) коды C#, например перенос строки "\r".

Если для свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) объекта `Label` задано значение `LayoutOptions.Fill`, общий размер `Label` определяется доступным пространством, которое предоставляет контейнер. Такое представление `Label` называется *ограниченным*. Размер `Label` равен размеру контейнера.

Если для свойств `HorizontalOptions` и `VerticalOptions` заданы значения, отличные от `LayoutOptions.Fill`, размер `Label` определяется необходимым пространством для отображения текста, но не более размера пространства, которое предоставляет контейнер для `Label`. Такое представление `Label` называется *неограниченным* и определяет свой размер самостоятельно.

(Примечание: Термины *ограниченный* и *неограниченный* могут противоречить здравому смыслу, так как неограниченное представление обычно занимает меньше места, чем ограниченное. Кроме того, эти термины не всегда одинаково применяются в первых главах книги.)

Некоторые представления, например `Label`, могут быть ограниченными по одному измерению и неограниченными по другому. Представление `Label` переносит текст на несколько строк только в том случае, если ограничен горизонтально.

Если `Label` ограничено, оно может занимать значительно больше пространства, чем требуется для текста. Текст можно поместить в общую область `Label`. Присвойте свойству [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) значение элемента перечисления [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [`Center`](xref:Xamarin.Forms.TextAlignment.Center) или [`End`](xref:Xamarin.Forms.TextAlignment.Center)), чтобы задать выравнивание для всех строк абзаца. По умолчанию используется значение `Start`, реализующее выравнивание текста по левому краю.

Присвойте свойству [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) значение элемента перечисления `TextAlignment`, чтобы поместить текст в верхней, центральной или нижней части зоны, занимаемой `Label`.

Присвойте свойству [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) значение элемента перечисления [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [`CharacterWrap`](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap), [`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation) или [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode.TailTruncation)), чтобы задать правила усечения и разрыва строк для абзаца с несколькими строками.

## <a name="text-and-background-colors"></a>Цвет текста и фона

Задайте для свойств [`TextColor`](xref:Xamarin.Forms.Label.TextColor) и [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) объекта `Label` значения [`Color`](xref:Xamarin.Forms.Color), чтобы управлять цветом текста и фона.

`BackgroundColor` применяется к фону всей области, занимаемой `Label`. В зависимости от свойств `HorizontalOptions` и `VerticalOptions` этот размер может значительно превышать размер области, необходимой для отображения текста. Вы можете с помощью цвета испытать разные значения `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment` и `VerticalTextAlignment`, чтобы оценить их влияние на размер и расположение `Label`, а также размер и расположение текста в `Label`.

## <a name="the-color-structure"></a>Структура Color

Структура [`Color`](xref:Xamarin.Forms.Color) позволяет указывать цвета в виде значений RGB (красный, зеленый, синий) или HSL (оттенок, насыщенность, яркость), а также по имени цвета. Для обозначения прозрачности также доступен альфа-канал.

Используйте конструктор `Color`, чтобы задать следующие значения:

- [оттенок серого](xref:Xamarin.Forms.Color.%23ctor(System.Double));
- [RGB-значение](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double));
- [RGB-значение с прозрачностью](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)).

В качестве аргументов принимаются значения `double` в диапазоне от 0 до 1.

Для создания значений `Color` можно также использовать несколько статических методов:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) для RGB-значений `double` в диапазоне от 0 до 1;
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) для целочисленных RGB-значений в диапазоне от 0 до 255;
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) для RGB-значений `double` с прозрачностью;
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) для целочисленных RGB-значений с прозрачностью;
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) для HSL-значений `double` с прозрачностью;
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) для значений `uint`, которые вычисляются как (B+256\*(G+256\*(R+256\*A)));
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) для значений `string` в формате шестнадцатеричных цифр ("#AARRGGBB", "#RRGGBB", "#ARGB" или "#RGB"), где каждая буква соответствует шестнадцатеричной цифре для каналов альфа, красного, зеленого и синего. Этот метод используется в основном для преобразования цветов XAML, как описано в главе 7 [XAML и код](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Созданное значение `Color` невозможно изменить. Характеристики цвета можно получить из следующих свойств:

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

Все они имеют значения `double` в диапазоне от 0 до 1.

`Color` также определяет 240 открытых статических полей только для чтения, обозначающих стандартные цвета. На момент написания книги поддерживались лишь 17 распространенных цветов.

Еще одно общее статическое поле только для чтения определяет цвет, у которого все цветовые каналы имеют нулевое значение.

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

Несколько методов экземпляра позволяют изменять существующий цвет, чтобы получить новый цвет.

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

И наконец, два статических свойства только для чтения определяют особое значение цвета.

- [`Color.Default`](xref:Xamarin.Forms.Color.Default), где все каналы имеют значение &ndash;1
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` предназначено для применения цветовой схемы платформы, а значит имеет разное значение в разных контекстах на разных платформах. По умолчанию на платформах применяются следующие цветовые схемы:

- iOS: темный текст на светлом фоне.
- Android: светлый текст на темном фоне (как в книге) или темный текст на светлом фоне (для материального дизайна, реализуемого через AppCompat в **главной ветви** репозитория для примеров кода).
- UWP: темный текст на светлом фоне.

Значение `Color.Accent` использует цвет, определяемый платформой (либо пользователем), который отображается на темном или светлом фоне.

## <a name="changing-the-application-color-scheme"></a>Изменение цветовой схемы приложения

Для нескольких платформ используется цветовая схема по умолчанию, как показано в списке выше.

При нацеливании на Android вы можете включить схему темного шрифта на светлом фоне, выбрав светлую тему в файле Android.Manifest.xml или [добавив AppCompat и элементы материального дизайна](~/xamarin-forms/platform/android/appcompat-material-design.md).

Для платформ Windows цветовую тему обычно выбирает пользователь, но вы можете добавить в файл App.xaml для этой платформы атрибут `RequestedTheme`, которому присвоено значение `Light` или `Dark`. По умолчанию файл App.xaml в проекте UWP содержит атрибут `RequestedTheme` со значением `Light`.

## <a name="font-sizes-and-attributes"></a>Размеры и атрибуты шрифта

Задайте для свойства [`FontFamily`](xref:Xamarin.Forms.Label.FontFamily) объекта `Label` строковое значение, например "Times Roman", чтобы выбрать семейство шрифтов. Но таким способом можно указать лишь такое семейство шрифтов, которое поддерживается на конкретной платформе, а платформы в этом отношении не соответствуют друг другу.

Задайте для свойства [`FontSize`](xref:Xamarin.Forms.Label.FontSize) объекта `Label` значение `double`, чтобы указать приблизительную высоту шрифта. Дополнительные сведения о правильном выборе размера шрифта см. в главе 5 [Работа с размерами](chapter05.md).

Вы также можете получить один из нескольких предварительно заданных размеров шрифтов, которые зависят от платформы. Статический метод [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) и [перегрузка](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) возвращают значение размера шрифта `double` для используемой платформы, которое является элементом перечисления [`NamedSize`](xref:Xamarin.Forms.NamedSize) ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [`Micro`](xref:Xamarin.Forms.NamedSize.Micro), [`Small`](xref:Xamarin.Forms.NamedSize.Small), [`Medium`](xref:Xamarin.Forms.NamedSize.Medium) и [`Large`](xref:Xamarin.Forms.NamedSize.Large)). Значение, возвращаемое элементом `Medium`, не обязательно совпадает с `Default`. В примере [**NamedFontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) текст отображается с использованием имен размеров.

Задайте свойству [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) объекта `Label` значение одного из элементов этого перечисления [`FontAttributes`](xref:Xamarin.Forms.FontAttributes), то есть [`Bold`](xref:Xamarin.Forms.FontAttributes.Bold), [`Italic`](xref:Xamarin.Forms.FontAttributes.Italic) или [`None`](xref:Xamarin.Forms.FontAttributes.None). Элементы `Bold` и `Italic` можно объединять с помощью оператора побитового или из C#.

## <a name="formatted-text"></a>Форматированный текст

Пока во всех примерах весь текст, отображаемый в `Label`, был отформатирован единообразно. Чтобы изменить форматирование в пределах одной строки текста, не устанавливайте свойство `Text` для `Label`. Вместо этого присвойте свойству [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) значение объекта с типом [`FormattedString`](xref:Xamarin.Forms.FormattedString).

У `FormattedString` есть свойство [`Spans`](xref:Xamarin.Forms.FormattedString.Spans), которое указывает на коллекцию объектов [`Span`](xref:Xamarin.Forms.Span). Каждый объект `Span` имеет собственные свойства [`Text`](xref:Xamarin.Forms.Span.Text), [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily), [`FontSize`](xref:Xamarin.Forms.Span.FontSize), [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes), [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) и [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor).

Пример [**VariableFormattedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) демонстрирует использование свойства `FormattedText` для одной строки текста, а [**VariableFormattedParagraph**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) — ту же методику для всего абзаца, как показано ниже:

[![Тройной снимок экрана с переменным форматированием абзаца](images/ch03fg06-small.png "Текст метки с отформатированной переменной")](images/ch03fg06-large.png#lightbox "Текст метки с отформатированной переменной")

Программа [**NamedFontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) использует один `Label` и объект `FormattedString`, чтобы отобразить все размеры именованных шрифтов для каждой платформы.

## <a name="related-links"></a>Связанные ссылки

- [Глава 3, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Примеры для главы 3](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Примеры F# для главы 3](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Label](~/xamarin-forms/user-interface/text/label.md)
- [Работа с цветами](~/xamarin-forms/user-interface/colors.md)
