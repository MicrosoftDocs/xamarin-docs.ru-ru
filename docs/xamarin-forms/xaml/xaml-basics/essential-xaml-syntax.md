---
title: Часть 2. Основной синтаксис XAML
description: В этой статье объясняются основные возможности синтаксиса XAML для элементов свойств и вложенных свойств.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 23d24ab7477bb7d9e95e4d78f25f334ae13a8ea2
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368522"
---
# <a name="part-2-essential-xaml-syntax"></a>Часть 2. Основной синтаксис XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML в основном предназначен для создания экземпляров и инициализации объектов. Но часто свойства должны быть заданы для сложных объектов, которые не могут быть представлены в виде XML-строк, а иногда свойства, определяемые одним классом, должны быть установлены в дочернем классе. Для этих двух нужд требуются ключевые функции синтаксиса XAML элементов свойств и вложенных свойств._

## <a name="property-elements"></a>Элементы свойств

В XAML свойства классов обычно устанавливаются в виде XML-атрибутов:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

Однако существует альтернативный способ задания свойства в XAML. Чтобы попробовать эту альтернативу с помощью `TextColor` , сначала удалите существующий `TextColor` параметр:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

Откройте тег пустого элемента `Label` , разделив его на начальный и конечный теги:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

В этих тегах добавьте открывающий и закрывающий теги, состоящие из имени класса и имени свойства, разделенных точкой:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

Задайте значение свойства в качестве содержимого этих новых тегов следующим образом:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Эти два способа указания `TextColor` Свойства функционально эквивалентны, но не используют два способа для одного и того же свойства, так как это значение эффективно настраивает свойство дважды и может быть неоднозначным.

С помощью этого нового синтаксиса можно использовать некоторые полезные термины:

- `Label` является  *элементом объекта*. Это объект, Xamarin.Forms выраженный как элемент XML.
- `Text`,  `VerticalOptions` `FontAttributes` и  `FontSize` являются  *атрибутами свойств*. Они представляют собой Xamarin.Forms свойства, выраженные в виде XML-атрибутов.
- В этом конечном фрагменте `TextColor` он стал  *элементом property*. Это свойство, Xamarin.Forms но теперь это элемент XML.

Определение элементов свойств может показаться нарушением синтаксиса XML, но это не так. Этот период не имеет особого смысла в XML. Для декодера XML `Label.TextColor` — это просто стандартный дочерний элемент.

Однако в XAML этот синтаксис очень особенный. Одно из правил для элементов свойств заключается в том, что в теге не может присутствовать ничего другого `Label.TextColor` . Значение свойства всегда определяется как содержимое между открывающим и закрывающим тегами элемента свойства.

Синтаксис элемента свойства можно использовать для нескольких свойств:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Или можно использовать синтаксис элемента свойства для всех свойств:

```xaml
<Label>
    <Label.Text>
        Hello, XAML!
    </Label.Text>
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
    <Label.VerticalOptions>
        Center
    </Label.VerticalOptions>
</Label>
```

Во-первых, синтаксис элемента свойства может показаться ненужной длительной заменой для чего-то сравнительно простого, и в этих примерах это, конечно же, не так.

Однако синтаксис элемента свойства становится очень важен, если значение свойства слишком сложное, чтобы оно было выражено как простая строка. В тегах элементов свойств можно создать экземпляр другого объекта и задать его свойства. Например, можно явно установить свойство, например, `VerticalOptions` в `LayoutOptions` значение с параметрами свойства:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

Еще один пример: `Grid` имеет два свойства с именами `RowDefinitions` и `ColumnDefinitions` . Эти два свойства имеют тип `RowDefinitionCollection` и `ColumnDefinitionCollection` , которые являются коллекциями `RowDefinition` объектов и `ColumnDefinition` . Для задания этих коллекций необходимо использовать синтаксис элемента свойства.

Вот начало XAML-файла для `GridDemoPage` класса, в котором показаны теги элемента свойства для `RowDefinitions` `ColumnDefinitions` коллекций и:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

Обратите внимание на сокращенный синтаксис для определения автоизменяемых ячеек, ячеек ширины и высоты в пикселях и параметров Star.

## <a name="attached-properties"></a>Присоединенные свойства

Вы только что видели, что для `Grid` `RowDefinitions` `ColumnDefinitions` определения строк и столбцов требуются элементы свойств для коллекций и. Однако для программиста также должен быть определен способ указания строки и столбца, где размещается каждый дочерний элемент `Grid` .

В теге для каждого дочернего элемента `Grid` нужно указать строку и столбец этого дочернего объекта, используя следующие атрибуты:

- `Grid.Row`
- `Grid.Column`

Значения этих атрибутов по умолчанию равны 0. Можно также указать, что дочерний элемент охватывает более одной строки или столбца с этими атрибутами:

- `Grid.RowSpan`
- `Grid.ColumnSpan`

Эти два атрибута имеют значения по умолчанию 1.

Вот полный файл Гриддемопаже. XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               Grid.Row="0" Grid.Column="0"
               TextColor="White"
               BackgroundColor="Blue" />

        <BoxView Color="Silver"
                 HeightRequest="0"
                 Grid.Row="0" Grid.Column="1" />

        <BoxView Color="Teal"
                 Grid.Row="1" Grid.Column="0" />

        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two rows (or more if you want)"
               Grid.Row="0" Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two columns"
               Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

`Grid.Row`Параметры и `Grid.Column` 0 не являются обязательными, но обычно включаются в целях ясности.

Вот как это выглядит:

[![Макет сетки](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

Судя только из синтаксиса, эти `Grid.Row` атрибуты,, `Grid.Column` `Grid.RowSpan` и `Grid.ColumnSpan` выглядят как статические поля или свойства `Grid` , но любопытно достаточно, не `Grid` определяет ничего с именем,, `Row` `Column` `RowSpan` или `ColumnSpan` .

Вместо этого `Grid` определяет четыре связываемых свойства с именами `RowProperty` , `ColumnProperty` , `RowSpanProperty` и `ColumnSpanProperty` . Это специальные типы связываемых свойств, называемые *присоединенными свойствами*. Они определяются `Grid` классом, но задаются для дочерних элементов объекта `Grid` .

Если вы хотите использовать эти вложенные свойства в коде, `Grid` класс предоставляет статические методы с именами `SetRow` , `GetColumn` и т. д. Но в XAML эти вложенные свойства задаются как атрибуты в дочерних элементах `Grid` с использованием имен простых свойств.

Вложенные свойства всегда распознаются в XAML-файлах как атрибуты, содержащие класс и имя свойства, разделенные точкой. Они называются *присоединенными свойствами* , поскольку они определены одним классом (в данном случае `Grid` ), но присоединены к другим объектам (в данном случае дочерними объектами `Grid` ). Во время компоновки `Grid` может опрашивать значения этих вложенных свойств, чтобы понять, где разместить каждый дочерний элемент.

`AbsoluteLayout`Класс определяет два присоединенных свойства с именами `LayoutBounds` и `LayoutFlags` . Ниже приведен шаблон шахматной доски, реализованный с использованием пропорциональных функций позиционирования и размеров `AbsoluteLayout` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.AbsoluteDemoPage"
             Title="Absolute Demo Page">

    <AbsoluteLayout BackgroundColor="#FF8080">
        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

  </AbsoluteLayout>
</ContentPage>
```

И вот так:

[![Абсолютный макет](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

Что-то вроде этого, вы можете столкнуться с знанияом XAML. Конечно, повтор и регулярный интервал `LayoutBounds` прямоугольника предполагает, что он может быть лучше реализован в коде.

Это, безусловно, подлинное внимание, и нет проблем с балансировкой использования кода и разметки при определении пользовательских интерфейсов. Можно легко определить некоторые визуальные элементы в XAML, а затем использовать конструктор файла кода программной части для добавления дополнительных визуальных элементов, которые лучше создавать в циклах.

## <a name="content-properties"></a>Свойства содержимого

В предыдущих примерах `StackLayout` `Grid` объекты, и `AbsoluteLayout` задаются для `Content` свойства `ContentPage` , а дочерние элементы этих макетов фактически являются элементами в `Children` коллекции. Однако эти `Content` `Children` Свойства и не находятся в файле XAML.

Безусловно, можно включить `Content` Свойства и в `Children` качестве элементов свойств, например в примере **ксамлплускоде** :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout.Children>
                <Slider VerticalOptions="CenterAndExpand"
                        ValueChanged="OnSliderValueChanged" />

                <Label x:Name="valueLabel"
                       Text="A simple Label"
                       FontSize="Large"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand" />

                <Button Text="Click Me!"
                      HorizontalOptions="Center"
                      VerticalOptions="CenterAndExpand"
                      Clicked="OnButtonClicked" />
            </StackLayout.Children>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Реальный вопрос: почему эти элементы свойств *не* требуются в файле XAML?

Элементы, определенные в Xamarin.Forms для использования в XAML, могут иметь одно свойство, помеченное `ContentProperty` атрибутом в классе. При поиске `ContentPage` класса в интерактивной Xamarin.Forms документации вы увидите следующий атрибут:

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

Это означает, что `Content` теги элементов свойств не являются обязательными. Предполагается, что любое XML-содержимое, отображаемое между открывающим и закрывающим `ContentPage` тегами, присвоено `Content` свойству.

 `StackLayout`, `Grid` , `AbsoluteLayout` и `RelativeLayout` все являются производными от `Layout<View>` , и если вы ищете `Layout<T>` в  Xamarin.Forms документации, вы увидите еще один `ContentProperty` атрибут:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

Это позволяет автоматически добавлять содержимое макета в `Children` коллекцию без явных `Children` тегов элементов свойств.

Другие классы также имеют `ContentProperty` определения атрибутов. Например, свойство Content объекта имеет значение `Label` `Text` . Ознакомьтесь с документацией по API для других пользователей.

## <a name="platform-differences-with-onplatform"></a>Различия между платформами и onplatform

В одностраничных приложениях обычно устанавливается `Padding` свойство на странице, чтобы избежать перезаписи строки состояния iOS. В коде `Device.RuntimePlatform` для этой цели можно использовать свойство:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

Вы также можете сделать что-то подобное в XAML, используя [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) [`On`](xref:Xamarin.Forms.On) классы и. Сначала включите элементы свойств для `Padding` свойства в верхней части страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

В эти теги включает `OnPlatform` тег. `OnPlatform` является универсальным классом. Необходимо указать аргумент универсального типа, в данном случае — `Thickness` тип `Padding` Свойства. К счастью, существует атрибут XAML, предназначенный специально для определения универсальных аргументов с именем `x:TypeArguments` . Он должен соответствовать типу свойства, которое вы задаете:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">

        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`OnPlatform` имеет свойство с именем `Platforms` , которое является классом `IList` `On` объектов. Используйте теги элемента свойства для этого свойства:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>

            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Теперь добавьте `On` элементы. Каждый из них устанавливает `Platform` свойство и `Value` свойство в разметку для `Thickness` Свойства:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>
                <On Platform="iOS" Value="0, 20, 0, 0" />
                <On Platform="Android" Value="0, 0, 0, 0" />
                <On Platform="UWP" Value="0, 0, 0, 0" />
            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Эту разметку можно упростить. Свойство Content элемента `OnPlatform` имеет значение `Platforms` , поэтому теги элементов свойств можно удалить:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android" Value="0, 0, 0, 0" />
            <On Platform="UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`Platform`Свойство объекта `On` имеет тип `IList<string>` , поэтому можно включить несколько платформ, если значения одинаковы.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android, UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Так как для Android и UWP задано значение по умолчанию `Padding` , этот тег можно удалить:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Это стандартный способ задания свойства, зависящего от платформы, `Padding` в XAML. Если `Value` параметр не может быть представлен одной строкой, можно определить для него элементы свойств:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS">
                <On.Value>
                    0, 20, 0, 0
                </On.Value>
            </On>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

> [!NOTE]
> `OnPlatform`Расширение разметки также можно использовать в XAML для настройки внешнего вида пользовательского интерфейса на уровне платформы. Он предоставляет те же функциональные возможности `OnPlatform` , что и `On` классы и, но с более кратким представлением. Дополнительные сведения см. в разделе [расширение разметки для платформы](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="summary"></a>Сводка

При использовании элементов свойств и вложенных свойств большая часть базового синтаксиса XAML была установлена. Однако иногда необходимо задать свойства для объектов косвенным способом, например из словаря ресурсов. Этот подход рассматривается в следующей части, часть [3. Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="related-links"></a>Связанные ссылки

- [ксамлсамплес](/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Часть 1. начало работы с XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Часть 3. Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Часть 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Часть 5. Из привязки данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)