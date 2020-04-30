---
title: Задание стиля приложений Xamarin.Forms с помощью каскадных таблиц стилей (CSS)
description: Xamarin. Forms поддерживает визуальные элементы стиля с помощью каскадные таблицы стилей (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
ms.openlocfilehash: 182bd088ba169dad6bdbbb4315a033925964f64e
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517565"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Стилизация приложений Xamarin. Forms с помощью каскадные таблицы стилей (CSS)

[![Скачать пример](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)

_Xamarin. Forms поддерживает визуальные элементы стиля с помощью каскадные таблицы стилей (CSS)._

Приложения Xamarin. Forms можно использовать в стиле CSS. Таблица стилей состоит из списка правил с каждым правилом, состоящим из одного или нескольких селекторов и блока объявления. Блок объявления состоит из списка объявлений в фигурных скобках с каждым объявлением, состоящим из свойства, двоеточия и значения. При наличии нескольких объявлений в блоке в качестве разделителя вставляется точка с запятой. В следующем примере кода показаны некоторые CSS, совместимые с Xamarin. Forms:

```css
navigationpage {
    -xf-bar-background-color: lightgray;
}

^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

В Xamarin. Forms таблицы стилей CSS анализируются и оцениваются во время выполнения, а не во время компиляции, а таблицы стилей повторно анализируются при использовании.

> [!NOTE]
> В настоящее время все стили, которые можно реализовать с помощью стиля XAML, не могут быть выполнены с помощью CSS. Однако стили XAML можно использовать для дополнения CSS для свойств, которые в настоящее время не поддерживаются Xamarin. Forms. Дополнительные сведения о стилях XAML см. в руководстве по [оформлению приложений Xamarin.Forms с использованием стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

В примере [монкэйаппксс](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss) показано использование CSS для стиля простого приложения и показано на следующих снимках экрана:

[![Главная страница Монкэйапп с стилизацией CSS](css-images/MonkeyAppMainPage.png "Главная страница Монкэйапп с стилизацией CSS")](css-images/MonkeyAppMainPage-Large.png#lightbox "Главная страница Монкэйапп с стилизацией CSS")

[![Страница сведений о Монкэйапп с стилизацией CSS](css-images/MonkeyAppDetailPage.png "Страница сведений о Монкэйапп с стилизацией CSS")](css-images/MonkeyAppDetailPage-Large.png#lightbox "Страница сведений о Монкэйапп с стилизацией CSS")

## <a name="consuming-a-style-sheet"></a>Использование таблицы стилей

Процесс добавления таблицы стилей в решение выглядит следующим образом:

1. Добавьте пустой CSS файл в проект библиотеки .NET Standard.
1. Задайте для действия сборки файла CSS значение **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Загрузка таблицы стилей

Существует ряд подходов, которые можно использовать для загрузки таблицы стилей.

> [!NOTE]
> Сейчас невозможно изменить таблицу стилей во время выполнения и применить новую таблицу стилей.

### <a name="xaml"></a>XAML

Таблицу стилей можно загрузить и проанализировать с помощью [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) класса перед добавлением в: [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

[`StyleSheet.Source`](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) Свойство определяет таблицу стилей как URI относительно расположения включающего XAML- `/`файла или относительно корневого каталога проекта, если URI начинается с.

> [!WARNING]
> Если для действия сборки не задано значение **EmbeddedResource**, Загрузка CSS-файла будет невозможна.

Кроме того, таблицу стилей можно загрузить и проанализировать с помощью [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) класса перед добавлением в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), выполнив встраивание в `CDATA` раздел.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Дополнительные сведения о словарях ресурсов см. в разделе [словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md).

### <a name="c"></a>C\#

В C# таблица стилей может быть загружена из `StringReader` и добавлена к: [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

Аргумент `StyleSheet.FromReader` метода — это объект `TextReader` , который прочитал таблицу стилей.

## <a name="selecting-elements-and-applying-properties"></a>Выбор элементов и применение свойств

CSS использует селекторы для определения целевых элементов. Стили с соответствующими селекторами применяются последовательно в порядке определения. Стили, определенные для конкретного элемента, всегда применяются последними. Дополнительные сведения о поддерживаемых селекторах см. в разделе [Справочник по селектору](#selector-reference).

В CSS для стиля выбранного элемента используются свойства. Каждое свойство имеет набор возможных значений, а некоторые свойства могут влиять на любой тип элемента, а другие — на группы элементов. Дополнительные сведения о поддерживаемых свойствах см. в разделе [Справочник по свойствам](#property-reference).

> [!IMPORTANT]
> Переменные CSS не поддерживаются.

### <a name="selecting-elements-by-type"></a>Выбор элементов по типу

Элементы в визуальном дереве можно выбрать по типу с помощью селектора без учета `element` регистра:

```css
stacklayout {
    margin: 20;
}
```

Этот селектор определяет все [`StackLayout`](xref:Xamarin.Forms.StackLayout) элементы на страницах, использующих таблицу стилей, и устанавливает для их полей однородную толщину в 20 символов.

> [!NOTE]
> `element` Селектор не определяет подклассы указанного типа.

### <a name="selecting-elements-by-base-class"></a>Выбор элементов по базовому классу

Элементы в визуальном дереве могут быть выбраны базовым классом с помощью селектора без учета `^base` регистра:

```css
^contentpage {
    background-color: lightgray;
}
```

Этот селектор определяет все [`ContentPage`](xref:Xamarin.Forms.ContentPage) элементы, использующие таблицу стилей, и задает для `lightgray`них цвет фона.

> [!NOTE]
> `^base` Селектор предназначен только для Xamarin. Forms и не является частью спецификации CSS.

### <a name="selecting-an-element-by-name"></a>Выбор элемента по имени

Отдельные элементы в визуальном дереве можно выбрать с помощью селектора с `#id` учетом регистра:

```css
#listView {
    background-color: lightgray;
}
```

Этот селектор определяет элемент, свойство [`StyleId`](xref:Xamarin.Forms.Element.StyleId) которого имеет значение `listView`. Однако если `StyleId` свойство не задано, селектор будет возвращаться к использованию `x:Name` элемента. Поэтому в следующем примере кода `#listView` XAML селектор определит, [`ListView`](xref:Xamarin.Forms.ListView) чей `x:Name` атрибут имеет значение `listView`, и установит для него цвет фона. `lightgray`

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>Выбор элементов с указанным атрибутом класса

С помощью селектора с учетом `.class` регистра можно выбрать элементы с указанным атрибутом класса:

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

Класс CSS может быть назначен элементу XAML путем присвоения [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) свойству элемента имени класса CSS. Поэтому в следующем примере XAML стили, `.detailPageTitle` определенные классом, присваиваются первому [`Label`](xref:Xamarin.Forms.Label), а стили, определенные `.detailPageSubtitle` классом, присваиваются второму. `Label`

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>Выбор дочерних элементов

Дочерние элементы в визуальном дереве можно выбрать с помощью селектора `element element` без учета регистра:

```css
listview image {
    height: 60;
    width: 60;
}
```

Этот селектор определяет все [`Image`](xref:Xamarin.Forms.Image) элементы, являющиеся дочерними [`ListView`](xref:Xamarin.Forms.ListView) элементами элементов, и устанавливает их высоту и ширину в 60. Поэтому в следующем примере кода XAML `listview image` селектор определит [`Image`](xref:Xamarin.Forms.Image) , что это дочерний элемент [`ListView`](xref:Xamarin.Forms.ListView), и установит для его высоты и ширины значение 60.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> `element element` Селектор не требует, чтобы дочерний элемент был _прямым_ потомком родительского элемента — дочерний элемент может иметь другой родитель. Выполняется выбор, если предком является указанный первый элемент.

### <a name="selecting-direct-child-elements"></a>Выбор прямых дочерних элементов

Прямые дочерние элементы в визуальном дереве можно выбрать с помощью селектора `element>element` без учета регистра:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Этот селектор определяет все [`Image`](xref:Xamarin.Forms.Image) элементы, которые являются прямыми [`StackLayout`](xref:Xamarin.Forms.StackLayout) дочерними элементами элементов, и устанавливает их высоту и ширину в 200. Таким образом, в следующем примере кода XAML `stacklayout>image` селектор определит [`Image`](xref:Xamarin.Forms.Image) , что является прямым дочерним элементом [`StackLayout`](xref:Xamarin.Forms.StackLayout), и установит для его высоты и ширины значение 200.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> `element>element` Селектор требует, чтобы дочерний элемент был _прямым_ дочерним элементом родительского элемента.

## <a name="selector-reference"></a>Справочник по селектору

В Xamarin. Forms поддерживаются следующие селекторы CSS:

|Выбор|Пример|Описание|
|---|---|---|
|`.class`|`.header`|Выбирает все элементы со свойством, `StyleClass` содержащим "заголовок". Обратите внимание, что в этом селекторе учитывается регистр.|
|`#id`|`#email`|Выбирает все элементы с `StyleId` параметром `email`, имеющим значение. Если `StyleId` параметр не установлен, для параметра `x:Name`Fallback на значение. При использовании XAML `x:Name` предпочтительнее `StyleId`. Обратите внимание, что в этом селекторе учитывается регистр.|
|`*`|`*`|Выбирает все элементы.|
|`element`|`label`|Выбирает все элементы типа `Label`, но не подклассы. Обратите внимание, что этот селектор не учитывает регистр.|
|`^base`|`^contentpage`|Выбирает все элементы в `ContentPage` качестве базового класса, включая `ContentPage` самого себя. Обратите внимание, что этот селектор не учитывает регистр и не является частью спецификации CSS.|
|`element,element`|`label,button`|Выбирает все `Button` элементы и все `Label` элементы. Обратите внимание, что этот селектор не учитывает регистр.|
|`element element`|`stacklayout label`|Выбирает все `Label` элементы внутри `StackLayout`. Обратите внимание, что этот селектор не учитывает регистр.|
|`element>element`|`stacklayout>label`|Выбирает все `Label` элементы в `StackLayout` качестве прямого родителя. Обратите внимание, что этот селектор не учитывает регистр.|
|`element+element`|`label+entry`|Выбирает все `Entry` элементы непосредственно после `Label`. Обратите внимание, что этот селектор не учитывает регистр.|
|`element~element`|`label~entry`|Выбирает все `Entry` элементы с префиксом `Label`. Обратите внимание, что этот селектор не учитывает регистр.|

Стили с соответствующими селекторами применяются последовательно в порядке определения. Стили, определенные для конкретного элемента, всегда применяются последними.

> [!TIP]
> Селекторы можно сочетать без ограничений, таких как `StackLayout>ContentView>label.email`.

Следующие селекторы в настоящее время не поддерживаются:

- `[attribute]`
- `@media` и `@supports`
- `:` и `::`

> [!NOTE]
> Определение приоритетности и переопределений с определенными параметрами не поддерживаются.

## <a name="property-reference"></a>Справочные данные по свойствам

Xamarin. Forms поддерживают следующие свойства CSS (в столбце **значения** типы имеют _курсив_, а строковые литералы `gray`):

|Свойство|Область применения|Значения|Пример|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial` |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial`|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_цветовая палитра_ \|`initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_строка_ \|`initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`, `ImageButton`|_цветовая палитра_ \|`initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_double_ \| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_double_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `CheckBox`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_цветовая палитра_ \|`initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_double_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| `column-reverse` \| `initial`|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_float_ \| float `auto` . \| `initial` Кроме того, с помощью `%` знака можно указать процент в диапазоне от 0% до 100%.|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_FLOAT_ \| `initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_FLOAT_ \| `initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_строка_ \|`initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_двойной_ \| _namedsize_ намедсизе \|  `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_double_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`letter-spacing`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `SearchHandler`, `Span`, `TimePicker`|_double_ \| `initial`|`letter-spacing: 2.5;`|
|`line-height`|`Label`, `Span`|_double_ \| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_толщина_ \|`initial` |`margin: 6 12;`|
|`margin-left`|`View`|_толщина_ \|`initial` |`margin-left: 3;`|
|`margin-top`|`View`|_толщина_ \|`initial` |`margin-top: 2;`|
|`margin-right`|`View`|_толщина_ \|`initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_толщина_ \|`initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_INT_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_double_ \| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|_INT_ \| `initial`|`order: -1;`|
|`padding`|`Button`, `ImageButton`, `Layout`, `Page`|_толщина_ \|`initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`, `ImageButton`, `Layout`, `Page`|_double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _double_ \| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`. `left`и `right` следует избегать в средах с письмом справа налево.| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _Double_, _Double_ \|`initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial`|`visibility: hidden;`|
|`width`|`VisualElement`|_double_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial`является допустимым значением для всех свойств. Он очищает значение (сбрасывается до значения по умолчанию), установленное из другого стиля.

Следующие свойства в настоящее время не поддерживаются:

- `all: initial`.
- Свойства макета (Box или Grid).
- Сокращенные свойства, такие `font`как, `border`и.

Кроме того, нет `inherit` значения, поэтому наследование не поддерживается. Поэтому вы не можете, например, задать `font-size` свойство для макета и ожидает, что все [`Label`](xref:Xamarin.Forms.Label) экземпляры в макете наследуют это значение. Единственным исключением является `direction` свойство, значение которого по умолчанию равно `inherit`.

Целевые `Span` элементы имеют известную ошибку, препятствующую нацеливанию стилей CSS на элемент и имя (с помощью `#` символа). `Span` Элемент является производным от `GestureElement`, который не имеет `StyleClass` свойства, поэтому диапазоны не поддерживают целевые классы CSS. Дополнительные сведения см. в разделе [не удается применить стилизацию CSS для управления охватом](https://github.com/xamarin/Xamarin.Forms/issues/5979).

### <a name="xamarinforms-specific-properties"></a>Свойства, относящиеся к Xamarin. Forms

Также поддерживаются следующие свойства CSS, относящиеся к Xamarin. Forms (в столбце **значения** типы имеют _курсив_, а строковые литералы `gray`):

|Свойство|Область применения|Значения|Пример|
|---|---|---|---|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_цветовая палитра_ \|`initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_цветовая палитра_ \|`initial` |`-xf-bar-text-color: gray`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-max-length`|`Entry`, `Editor`, `SearchBar`|_INT_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-max-track-color`|`Slider`|_цветовая палитра_ \|`initial` |`-xf-max-track-color: red;`|
|`-xf-min-track-color`|`Slider`|_цветовая палитра_ \|`initial` |`-xf-min-track-color: yellow;`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both`поддерживается только в `ScrollView`. |`-xf-orientation: horizontal;`|
|`-xf-placeholder`|`Entry`, `Editor`, `SearchBar`|\| _текст в кавычках_`initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`, `Editor`, `SearchBar`|_цветовая палитра_ \|`initial` |`-xf-placeholder-color: green;`|
|`-xf-spacing`|`StackLayout`|_double_ \| `initial` |`-xf-spacing: 8;`|
|`-xf-thumb-color`|`Slider`, `Switch`|_цветовая палитра_ \|`initial` |`-xf-thumb-color: limegreen;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-vertical-text-alignment`|`Label`| `start` \| `center` \| `end` \| `initial`|`-xf-vertical-text-alignment: end;`|
|`-xf-visual`|`VisualElement`|_строка_ \|`initial` |`-xf-visual: material;`|

### <a name="xamarinforms-shell-specific-properties"></a>Особые свойства оболочки Xamarin.Forms

Также поддерживаются следующие свойства CSS, относящиеся к оболочке Xamarin. Forms (в столбце **значения** типы имеют _курсив_, а строковые литералы `gray`):

|Свойство|Область применения|Значения|Пример|
|---|---|---|---|
|`-xf-flyout-background`|`Shell`|_цветовая палитра_ \|`initial` |`-xf-flyout-background: red;`|
|`-xf-shell-background`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-background: green;`|
|`-xf-shell-disabled`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-disabled: blue;`|
|`-xf-shell-foreground`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-foreground: yellow;`|
|`-xf-shell-tabbar-background`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-tabbar-background: white;`|
|`-xf-shell-tabbar-disabled`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-tabbar-disabled: black;`|
|`-xf-shell-tabbar-foreground`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-tabbar-foreground: gray;`|
|`-xf-shell-tabbar-title`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-tabbar-title: lightgray;`|
|`-xf-shell-tabbar-unselected`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-tabbar-unselected: cyan;`|
|`-xf-shell-title`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-title: teal;`|
|`-xf-shell-unselected`|`Element`|_цветовая палитра_ \|`initial` |`-xf-shell-unselected: limegreen;`|

### <a name="color"></a>Цвет

Поддерживаются `color` следующие значения:

- `X11`[цвета](https://en.wikipedia.org/wiki/X11_color_names), соответствующие цветам CSS, предварительно определенным цветам UWP и цветам Xamarin. Forms. Обратите внимание, что в этих цветовых значениях регистр не учитывается.
- Шестнадцатеричные цвета `#rgb`: `#argb`, `#rrggbb`,,`#aarrggbb`
- цвета RGB: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Значения находятся в диапазоне 0-255 или 0%-100%.
- цвета RGBA: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. Значение непрозрачности находится в диапазоне от 0,0 до 1,0.
- цвета HSL: `hsl(120, 100%, 50%)`. Значение h находится в диапазоне 0-360, а s и l — в диапазоне 0%-100%.
- цвета хсла: `hsla(120, 100%, 50%, .8)`. Значение непрозрачности находится в диапазоне от 0,0 до 1,0.

### <a name="thickness"></a>Thickness

Поддерживаются одно, два, три или `thickness` четыре значения, разделенные пробелами:

- Одно значение обозначает однородную толщину.
- Два значения указывают вертикальную толщину по горизонтали.
- Три значения указываются сверху, затем горизонтально (слева и справа), затем толщина нижней части.
- Четыре значения указываются сверху, справа, снизу, затем слева.

> [!NOTE]
> Значения `thickness` CSS отличаются от значений [`Thickness`](xref:Xamarin.Forms.Thickness) XAML. Например, в XAML два значения `Thickness` обозначают горизонтальную и вертикальную толщину, а четыре значения `Thickness` — влево, затем сверху, справа и снизу. Кроме того, значения `Thickness` XAML разделяются запятыми.

### <a name="namedsize"></a>намедсизе

Поддерживаются следующие значения без `namedsize` учета регистра:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Точное значение каждого `namedsize` значения зависит от платформы и от представления.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS в Xamarin. Forms с Xamarin. Университет

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Видео о CSS Xamarin. Forms 3,0**

## <a name="related-links"></a>Связанные ссылки

- [Монкэйаппксс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
