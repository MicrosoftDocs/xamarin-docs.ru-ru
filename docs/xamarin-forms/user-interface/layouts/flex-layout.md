---
title: Флекслайаут Xamarin. Forms
description: Используйте Флекслайаут для наложения или упаковки коллекции дочерних представлений.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 507f78bf887d8d11e93a5a6a1f7d074c55e69360
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2020
ms.locfileid: "83149972"
---
# <a name="the-xamarinforms-flexlayout"></a>Флекслайаут Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)

_Используйте Флекслайаут для наложения или упаковки коллекции дочерних представлений._

Xamarin. Forms [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) впервые помещается в Xamarin. Forms версии 3,0. Он основан на [модуле макета гибкой рамки](https://www.w3.org/TR/css-flexbox-1/)CSS, обычно известном как _Flex Layout_ или _Flex-Box_, поэтому вызывается, так как он включает множество гибких параметров для упорядочения дочерних элементов в макете.

`FlexLayout`функция схожа с Xamarin. Forms [`StackLayout`](~/xamarin-forms/user-interface/layouts/stacklayout.md) в том, что она может упорядочивать дочерние элементы по горизонтали и вертикали в стеке. Тем не менее `FlexLayout` компонент также может обернуть свои дочерние элементы, если слишком много подходит для одной строки или столбца, а также имеет множество параметров ориентации, выравнивания и адаптации к различным размерам экрана.

`FlexLayout`является производным от [`Layout<View>`](xref:Xamarin.Forms.Layout`1) и наследует [`Children`](xref:Xamarin.Forms.Layout`1.Children) свойство типа `IList<View>` .

`FlexLayout`определяет шесть общедоступных связываемых свойств и пять присоединенных связываемых свойств, которые влияют на размер, ориентацию и выравнивание дочерних элементов. (Если вы не знакомы с присоединенными связанными свойствами, см. статью **[присоединенные свойства](~/xamarin-forms/xaml/attached-properties.md)**.) Эти свойства подробно описаны в разделах ниже в разделе сведения о **[связываемых свойствах](#bindable-properties)** , а **[также подробно присоединенные привязываемые свойства](#attached-properties)**. Однако эта статья начинается с раздела о некоторых **[распространенных сценариях использования](#common-scenarios)** `FlexLayout` , которые более подробно описывают многие из этих свойств. В конце статьи вы увидите, как сочетаться `FlexLayout` с [таблицами стилей CSS](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Основные сценарии использования

Пример программы **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** содержит несколько страниц, демонстрирующих некоторые распространенные способы применения `FlexLayout` , и позволяет экспериментировать со своими свойствами.

### <a name="using-flexlayout-for-a-simple-stack"></a>Использование Флекслайаут для простого стека

На странице **простой стека** показано `FlexLayout` , как можно заменить, `StackLayout` но с помощью более простой разметки. Все данные в этом примере определены на странице XAML. `FlexLayout`Содержит четыре дочерних элемента:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.SimpleStackPage"
             Title="Simple Stack">

    <FlexLayout Direction="Column"
                AlignItems="Center"
                JustifyContent="SpaceEvenly">

        <Label Text="FlexLayout in Action"
               FontSize="Large" />

        <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />

        <Button Text="Do-Nothing Button" />

        <Label Text="Another Label" />
    </FlexLayout>
</ContentPage>
```

Ниже приведена страница, работающая на iOS, Android и универсальная платформа Windows:

[![Страница «простая стопка»](flex-layout-images/SimpleStack.png "Страница «простая стопка»")](flex-layout-images/SimpleStack-Large.png#lightbox)

`FlexLayout`В файле **симплестаккпаже. XAML** показаны три свойства:

- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)Свойству присваивается значение [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) перечисления. Значение по умолчанию — `Row`. Присвоение свойству значения `Column` приводит к тому, что дочерние элементы объекта `FlexLayout` будут упорядочены в один столбец элементов.

    Если элементы в `FlexLayout` упорядочены по столбцам, то говорят, что `FlexLayout` у элемента есть вертикальная _основная ось_ и горизонтальная _Перекрестная ось_.

- [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)Свойство имеет тип [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) и указывает, как выводятся элементы по перекрестной оси. `Center`Параметр приводит к горизонтальному центрированию каждого элемента.

    Если для этой задачи использовался объект, `StackLayout` а не, `FlexLayout` то все элементы будут центрированы путем присвоения `HorizontalOptions` свойству каждого элемента значения `Center` . `HorizontalOptions`Свойство не работает для дочерних элементов `FlexLayout` , но одно `AlignItems` свойство достигает той же цели. При необходимости можно использовать `AlignSelf` присоединенное свойство BIND, чтобы переопределить `AlignItems` свойство для отдельных элементов:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    С этим изменением оно `Label` размещается на левом крае, `FlexLayout` когда порядок чтения — слева направо.

- [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)Свойство имеет тип [`FlexJustify`](xref:Xamarin.Forms.FlexJustify) и указывает, как элементы упорядочиваются по главной оси. `SpaceEvenly`Параметр выделяет все оставшиеся вертикальные промежутки между всеми элементами, а также над первым элементом и под последним элементом.

    Если вы использовали, для `StackLayout` `VerticalOptions` достижения аналогичного результата необходимо присвоить свойству каждого элемента значение `CenterAndExpand` . Но `CenterAndExpand` параметр должен выделить в два раза больше пространства между каждым элементом, чем перед первым элементом и после последнего элемента. Можно имитировать `CenterAndExpand` параметр `VerticalOptions` , задав `JustifyContent` для свойства значение `FlexLayout` `SpaceAround` .

Эти `FlexLayout` свойства более подробно описаны в разделе ниже приводятся **[связываемые свойства](#bindable-properties)** .

### <a name="using-flexlayout-for-wrapping-items"></a>Использование Флекслайаут для упаковки элементов

На странице « **Перенос фотографий** » примера **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** показано, как `FlexLayout` можно переносить дочерние элементы в дополнительные строки или столбцы. Файл XAML создает экземпляр `FlexLayout` и присваивает ему два свойства:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.PhotoWrappingPage"
             Title="Photo Wrapping">
    <Grid>
        <ScrollView>
            <FlexLayout x:Name="flexLayout"
                        Wrap="Wrap"
                        JustifyContent="SpaceAround" />
        </ScrollView>

        <ActivityIndicator x:Name="activityIndicator"
                           IsRunning="True"
                           VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

`Direction`Свойство этого свойства `FlexLayout` не задано, поэтому имеет значение по умолчанию `Row` , означающее, что дочерние элементы упорядочены по строкам, а главная ось — горизонтальная.

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)Свойство имеет тип перечисления [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) . Если в строке слишком много элементов для размещения, то этот параметр свойства приводит к переносу элементов в следующую строку.

Обратите внимание, что `FlexLayout` является дочерним элементом объекта `ScrollView` . Если на странице слишком много строк, то `ScrollView` свойство имеет значение по умолчанию `Orientation` `Vertical` и разрешает вертикальную прокрутку.

`JustifyContent`Свойство выделяет остаточное пространство на основной оси (горизонтальной оси), чтобы каждый элемент был заключен в один и тот же объем пустого пространства.

Файл кода программной части обращается к коллекции образцов фотографий и добавляет их в `Children` коллекцию `FlexLayout` :

```csharp
public partial class PhotoWrappingPage : ContentPage
{
    // Class for deserializing JSON list of sample bitmaps
    [DataContract]
    class ImageList
    {
        [DataMember(Name = "photos")]
        public List<string> Photos = null;
    }

    public PhotoWrappingPage ()
    {
        InitializeComponent ();

        LoadBitmapCollection();
    }

    async void LoadBitmapCollection()
    {
        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("https://raw.githubusercontent.com/xamarin/docs-archive/master/Images/stock/small/stock.json");
                byte[] data = await webClient.DownloadDataTaskAsync(uri);

                // Convert to a Stream object
                using (Stream stream = new MemoryStream(data))
                {
                    // Deserialize the JSON into an ImageList object
                    var jsonSerializer = new DataContractJsonSerializer(typeof(ImageList));
                    ImageList imageList = (ImageList)jsonSerializer.ReadObject(stream);

                    // Create an Image object for each bitmap
                    foreach (string filepath in imageList.Photos)
                    {
                        Image image = new Image
                        {
                            Source = ImageSource.FromUri(new Uri(filepath))
                        };
                        flexLayout.Children.Add(image);
                    }
                }
            }
            catch
            {
                flexLayout.Children.Add(new Label
                {
                    Text = "Cannot access list of bitmap files"
                });
            }
        }

        activityIndicator.IsRunning = false;
        activityIndicator.IsVisible = false;
    }
}
```

Вот программа, которая выполняется, прокручивается сверху вниз:

[![Страница «Перенос фотографий»](flex-layout-images/PhotoWrapping.png "Страница «Перенос фотографий»")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Макет страницы с Флекслайаут

В веб-дизайне есть стандартный макет, именуемый « [_Великий граил_](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) », так как это очень желательный формат макета, но часто трудно реализовать с помощью совершенство. Макет состоит из заголовка в верхней части страницы и нижнего колонтитула в нижней части, что расширяется до полной ширины страницы. Занимаясь центром страницы, это основное содержимое, но часто с меню в столбцах слева от содержимого и дополнительными сведениями (иногда называемыми незанятой _областью_ ) справа. [Раздел 5.4.1 спецификации гибкой рамки CSS](https://www.w3.org/TR/css-flexbox-1/#order-accessibility) описывает, как можно реализовать макет "Святой граил" с помощью Flex Box.

На странице **макета «Святой граил** » примера **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** показана простая реализация этого макета с использованием одного `FlexLayout` вложенного в другой. Так как эта страница предназначена для телефона в книжной ориентации, области слева и справа от области содержимого выводятся только в 50 пикселей в ширину.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.HolyGrailLayoutPage"
             Title="Holy Grail Layout">

    <FlexLayout Direction="Column">

        <!-- Header -->
        <Label Text="HEADER"
               FontSize="Large"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center" />

        <!-- Body -->
        <FlexLayout FlexLayout.Grow="1">

            <!-- Content -->
            <Label Text="CONTENT"
                   FontSize="Large"
                   BackgroundColor="Gray"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center"
                   FlexLayout.Grow="1" />

            <!-- Navigation items-->
            <BoxView FlexLayout.Basis="50"
                     FlexLayout.Order="-1"
                     Color="Blue" />

            <!-- Aside items -->
            <BoxView FlexLayout.Basis="50"
                     Color="Green" />

        </FlexLayout>

        <!-- Footer -->
        <Label Text="FOOTER"
               FontSize="Large"
               BackgroundColor="Pink"
               HorizontalTextAlignment="Center" />
    </FlexLayout>
</ContentPage>
```

Здесь он работает:

[![Страница макета «Святой Граил»](flex-layout-images/HolyGrailLayout.png "Страница макета «Святой Граил»")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Области навигации и перемещения в сторону отображаются с помощью `BoxView` слева и справа.

Первый `FlexLayout` в XAML-файле имеет вертикальную основную ось и содержит три дочерних элемента, расположенных в столбце. Это заголовок, текст страницы и нижний колонтитул. Вложенный объект `FlexLayout` имеет горизонтальную основную ось с тремя дочерними элементами, расположенными в строке.

В этой программе демонстрируются три присоединенных связываемых свойств:

- `Order`Присоединенное свойство привязки устанавливается в первом `BoxView` . Это свойство является целым числом со значением по умолчанию 0. Это свойство можно использовать для изменения порядка разметки. Обычно разработчики предпочитают, чтобы содержимое страницы отображалось в разметке до элементов навигации и элементов. Присвоение `Order` свойству первого `BoxView` элемента значения, меньшего, чем его другие элементы того же уровня, приводит к тому, что оно будет отображаться как первый элемент в строке. Аналогичным образом можно убедиться, что элемент отображается последним, задав `Order` свойству значение, большее, чем его элементы того же уровня.

- `Basis`Присоединенное свойство привязки устанавливается для двух `BoxView` элементов, чтобы присвоить им ширину 50 пикселей. Это свойство имеет тип `FlexBasis` , структура, определяющая статическое свойство типа `FlexBasis` с именем `Auto` , которое является значением по умолчанию. Можно использовать `Basis` для указания размера пикселя или процента, определяющего объем пространства, занимаемого элементом на основной оси. Он называется _базисом_ , поскольку он задает размер элемента, который является основанием для всех последующих макетов.

- `Grow`Свойство задается во вложенном `Layout` и в `Label` дочернем элементе, представляющем содержимое. Это свойство имеет тип `float` и имеет значение по умолчанию 0. Если задано положительное значение, все оставшееся пространство на главной оси выделяется этому элементу и к родственным элементам с положительными значениями `Grow` . Пространство выделяется пропорционально значениям, что напоминает спецификацию Star в `Grid` .

    Первое `Grow` присоединенное свойство задается во вложенном `FlexLayout` объекте, указывая, что это `FlexLayout` займет все неиспользуемое вертикальное пространство внутри внешнего `FlexLayout` . Второе `Grow` присоединенное свойство устанавливается на объекте `Label` , представляющем содержимое, что означает, что это содержимое займет все неиспользуемое по горизонтали пространство внутри внутреннего `FlexLayout` .

    Существует также аналогичное `Shrink` вложенное свойство, которое можно использовать, когда размер дочерних элементов превышает размер, `FlexLayout` но не требуется перенос.

### <a name="catalog-items-with-flexlayout"></a>Элементы каталога с Флекслайаут

Страница « **элементы каталога** » в образце **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** похожа на [Пример 1 в разделе 1,1 спецификации CSS Flex Layout Box](https://www.w3.org//TR/css-flexbox-1/#overview) , за исключением того, что он отображает последовательности изображений и описания трех монкэйс по горизонтали.

[![Страница «элементы каталога»](flex-layout-images/CatalogItems.png "Страница «элементы каталога»")](flex-layout-images/CatalogItems-Large.png#lightbox)

Каждый из трех монкэйсов `FlexLayout` содержится в `Frame` , который имеет явную высоту и ширину, а также является дочерним элементом большего `FlexLayout` . В этом XAML-файле большинство свойств дочерних элементов задаются `FlexLayout` в стилях, кроме одного неявного стиля:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CatalogItemsPage"
             Title="Catalog Items">
    <ContentPage.Resources>
        <Style TargetType="Frame">
            <Setter Property="BackgroundColor" Value="LightYellow" />
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="Margin" Value="10" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="Margin" Value="0, 4" />
        </Style>

        <Style x:Key="headerLabel" TargetType="Label">
            <Setter Property="Margin" Value="0, 8" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="Blue" />
        </Style>

        <Style TargetType="Image">
            <Setter Property="FlexLayout.Order" Value="-1" />
            <Setter Property="FlexLayout.AlignSelf" Value="Center" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="White" />
            <Setter Property="BackgroundColor" Value="Green" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}"
                           WidthRequest="240"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Неявный стиль для `Image` включает параметры двух присоединенных связываемых свойств `Flexlayout` :

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order`Значение &ndash; 1 приводит к тому, что `Image` элемент будет отображаться первым в каждом вложенном `FlexLayout` представлении независимо от его позиции в коллекции дочерних элементов. `AlignSelf`Свойство объекта `Center` вызывает `Image` Центрирование объекта в `FlexLayout` . Это переопределяет значение `AlignItems` свойства, которое имеет значение по умолчанию `Stretch` , означающее, что `Label` `Button` дочерние элементы и растягиваются до полной ширины `FlexLayout` .

В каждом из трех `FlexLayout` представлений пустое `Label` поле предшествует `Button` , но имеет `Grow` значение 1. Это означает, что все дополнительное вертикальное пространство выделено для этого пустого поля `Label` , которое фактически отправляет в `Button` нижнюю часть.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Подробные сведения о связываемых свойствах

Теперь, когда вы видели некоторые распространенные приложения `FlexLayout` , свойства `FlexLayout` можно изучить более подробно.
`FlexLayout`определяет шесть связываемых свойств, заданных в `FlexLayout` коде или коде XAML для управления ориентацией и выравниванием. (Одно из этих свойств, [`Position`](xref:Xamarin.Forms.FlexLayout.Position) не рассматривается в этой статье.)

Вы можете поэкспериментировать с пятью оставшимися связываемыми свойствами, используя страницу **эксперимента** образца **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** . На этой странице можно добавлять или удалять дочерние элементы из `FlexLayout` и для установки сочетаний пяти связываемых свойств. Все дочерние элементы объекта `FlexLayout` — это `Label` представления различных цветов и размеров, для `Text` свойства которого задано число, соответствующее его положению в `Children` коллекции.

При запуске программы в пяти `Picker` представлениях отображаются значения по умолчанию для этих пяти `FlexLayout` свойств. В `FlexLayout` нижней части экрана содержатся три дочерних элемента:

[![Страница эксперимента: по умолчанию](flex-layout-images/ExperimentDefault.png "Страница эксперимента — по умолчанию")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Каждое из `Label` представлений имеет серый фон, который показывает пространство, выделенное для него `Label` в `FlexLayout` . Фон `FlexLayout` самого себя — Алиса синего цвета. Он занимает всю нижнюю область страницы, за исключением небольшого поля слева и справа.

<a name="direction" />

### <a name="the-direction-property"></a>Свойство Direction

[`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)Свойство имеет тип [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) , перечисление с четырьмя элементами:

- `Column`
- `ColumnReverse`(или "Column-Reverse" в XAML)
- `Row`, по умолчанию
- `RowReverse`(или "строка-обратная" в XAML)

В XAML можно указать значение этого свойства, используя имена членов перечисления в нижнем или верхнем регистре, или можно использовать две дополнительные строки, отображаемые в круглых скобках, которые совпадают с индикаторами CSS. (Строки "Column-Reverse" и "Row-Reverse" определены в [`FlexDirectionTypeConverter`](xref:Xamarin.Forms.FlexDirectionTypeConverter) классе, используемом средством синтаксического анализа XAML.)

Ниже показана страница **эксперимента** (слева направо), `Row` направление, `Column` направление и `ColumnReverse` направление:

[![Страница эксперимента: направление](flex-layout-images/ExperimentDirection.png "Направление страницы эксперимента")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Обратите внимание, что для `Reverse` параметров элементы начинаются справа или снизу.

<a name="wrap" />

### <a name="the-wrap-property"></a>Свойство Wrap

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)Свойство имеет тип [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) , перечисление с тремя элементами:

- `NoWrap`, по умолчанию
- `Wrap`
- `Reverse`(или «обернуть-Reverse» в XAML)

На этих экранах слева направо отображаются `NoWrap` Параметры, `Wrap` и `Reverse` для 12 дочерних элементов:

[![Страница эксперимента: перенос](flex-layout-images/ExperimentWrap.png "Страница эксперимента — перенос")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Если `Wrap` свойство имеет значение `NoWrap` , а главная ось ограничена (как в этой программе), а главная ось не является широкой или высотой достаточного размера для размещения всех дочерних элементов, то `FlexLayout` предпринимается попытка уменьшить размер элементов, как показано на снимке экрана iOS. Можно управлять сжатием элементов с помощью [`Shrink`](#shrink) присоединенного свойства BIND.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Свойство Жустификонтент

[`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)Свойство имеет тип [`FlexJustify`](xref:Xamarin.Forms.FlexJustify) , перечисление с шестью членами:

- `Start`(или "Flex-Start" в XAML), по умолчанию
- `Center`
- `End`(или "Flex-End" в XAML)
- `SpaceBetween`(или "пробел между" в XAML)
- `SpaceAround`(или «пробел вокруг» в XAML)
- `SpaceEvenly`

Это свойство указывает, как расположены элементы на главной оси, которая является горизонтальной осью в этом примере:

[![Страница эксперимента: выравнивание содержимого](flex-layout-images/ExperimentJustifyContent.png "Содержимое по ширине страницы эксперимента")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

Во всех трех снимках экрана `Wrap` свойство имеет значение `Wrap` . Значение `Start` по умолчанию показано на предыдущем снимке экрана Android. На снимке экрана iOS здесь показан `Center` параметр: все элементы перемещаются в центр. Три других параметра, начинающиеся с слова, `Space` выделяют дополнительное пространство, не занятое элементами. `SpaceBetween`равномерно распределяет интервал между элементами; `SpaceAround`устанавливает равные пробелы вокруг каждого элемента, в то же время `SpaceEvenly` устанавливает равное пространство между каждым элементом и перед первым и последним элементом в строке.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Свойство Алигнитемс

[`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)Свойство имеет тип [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) , перечисление с четырьмя элементами:

- `Stretch`, по умолчанию
- `Center`
- `Start`(или "Flex-Start" в XAML)
- `End`(или "Flex-End" в XAML)

Это одно из двух свойств (другое — [`AlignContent`](#align-content) ), которое указывает, как дочерние элементы выводятся по перекрестной оси. В каждой строке дочерние элементы растягиваются (как показано на предыдущем снимке экрана) или выравниваются по началу, центру или концу каждого элемента, как показано на следующих трех снимках экрана:

[![Страница эксперимента: выровняйте элементы](flex-layout-images/ExperimentAlignItems.png "Элементы, выровняйте по страницам эксперимента")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

На снимке экрана iOS верхние края всех дочерних элементов согласовываются. На снимках экрана Android элементы располагаются вертикально по центру в зависимости от самого высокого элемента. На снимке экрана UWP нижние части всех элементов вычисляются.

Для любого отдельного элемента `AlignItems` параметр можно переопределить с помощью [`AlignSelf`](#align-self) присоединенного свойства BIND.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Свойство Алигнконтент

[`AlignContent`](xref:Xamarin.Forms.FlexLayout.AlignContent)Свойство имеет тип [`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent) , перечисление с семью членами:

- `Stretch`, по умолчанию
- `Center`
- `Start`(или "Flex-Start" в XAML)
- `End`(или "Flex-End" в XAML)
- `SpaceBetween`(или "пробел между" в XAML)
- `SpaceAround`(или «пробел вокруг» в XAML)
- `SpaceEvenly`

Как и `AlignItems` свойство, оно `AlignContent` также совмещает дочерние элементы на перекрестной оси, но влияет на целые строки или столбцы:

[![Страница эксперимента: Выровняйте содержимое](flex-layout-images/ExperimentAlignContent.png "Содержимое, выровняйте страницу эксперимента")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

На снимке экрана iOS обе строки находятся вверху. на снимке экрана Android они находятся в центре; и на снимке экрана UWP они находятся внизу. Строки также можно разставлять по разным путям:

[![Страница эксперимента: Выровняйте содержимое 2](flex-layout-images/ExperimentAlignContent2.png "Страница эксперимента — выровняйте содержимое 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

Параметр не `AlignContent` действует, если имеется только одна строка или столбец.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Подробно присоединенные связываемые свойства

`FlexLayout`определяет пять присоединенных привязанных свойств. Эти свойства задаются для дочерних элементов `FlexLayout` и относятся только к этому конкретному дочернему элементу.

<a name="align-self" />

### <a name="the-alignself-property"></a>Свойство Алигнселф

[`AlignSelf`](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty)Присоединенное свойство привязки имеет тип [`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent) , перечисление с пятью элементами:

- `Auto`, по умолчанию
- `Stretch`
- `Center`
- `Start`(или "Flex-Start" в XAML)
- `End`(или "Flex-End" в XAML)

Для любого отдельного дочернего элемента этого `FlexLayout` параметра свойства переопределяет свойство, заданное [`AlignItems`](#align-items) для `FlexLayout` самого себя. Значение по умолчанию `Auto` означает использование `AlignItems` параметра.

Для `Label` элемента с именем `label` (или example) можно задать `AlignSelf` свойство в коде следующим образом:

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

Обратите внимание, что отсутствует ссылка на `FlexLayout` родительский элемент объекта `Label` . В XAML задается свойство следующим образом:

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Свойство Order

[`Order`](xref:Xamarin.Forms.FlexLayout.OrderProperty)Свойство имеет тип `int` . Значение по умолчанию — 0.

`Order`Свойство позволяет изменить порядок, в котором упорядочиваются дочерние элементы объекта `FlexLayout` . Как правило, дочерние элементы `FlexLayout` располагаются в том же порядке, в котором они отображаются в `Children` коллекции. Этот порядок можно переопределить, задав `Order` присоединяемому свойству BIND ненулевое целочисленное значение для одного или нескольких дочерних элементов. `FlexLayout`Затем упорядочивает свои дочерние элементы на основе значения `Order` свойства для каждого дочернего элемента, но дочерние элементы с тем же `Order` параметром упорядочиваются в том порядке, в котором они отображаются в `Children` коллекции.

### <a name="the-basis-property"></a>Свойство "Базис"

[`Basis`](xref:Xamarin.Forms.FlexLayout.BasisProperty)Присоединенное свойство BIND указывает объем пространства, выделенного дочернему элементу `FlexLayout` на главной оси. Размер, заданный `Basis` свойством, является размером вдоль основной оси родителя `FlexLayout` . Таким образом, `Basis` указывает ширину дочернего элемента, когда дочерние элементы упорядочиваются по строкам, или высоту, когда дочерние элементы упорядочиваются по столбцам.

`Basis`Свойство имеет тип [`FlexBasis`](xref:Xamarin.Forms.FlexBasis) , структура. Размер может быть указан либо в единицах, независимых от устройства, либо в процентах от размера `FlexLayout` . Значением свойства по умолчанию `Basis` является статическое свойство `FlexBasis.Auto` , которое означает, что используется запрошенная ширина или высота дочернего элемента.

В коде можно задать `Basis` свойство для `Label` именованных устройств с именем, `label` равным 40, следующим образом:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Второй аргумент `FlexBasis` конструктора имеет имя `isRelative` и указывает, является ли размер относительным ( `true` ) или абсолютным ( `false` ). Аргумент имеет значение по умолчанию `false` , поэтому можно также использовать следующий код:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Определено неявное преобразование из `float` в `FlexBasis` , поэтому его можно упростить еще больше:

```csharp
FlexLayout.SetBasis(label, 40);
```

Можно задать размер 25% `FlexLayout` родительского элемента следующим образом:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Это дробное значение должно находиться в диапазоне от 0 до 1.

В XAML можно использовать число в качестве размера в единицах, не зависящих от устройства:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Также можно указать процент в диапазоне от 0% до 100%:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

На странице «основной **эксперимент** » образца **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** можно поэкспериментировать со `Basis` свойством. На странице отображается столбец с текстом из пяти `Label` элементов с чередованием цветов фона и переднего плана. Два `Slider` элемента позволяют указать `Basis` значения для второго и четвертого `Label` :

[![Страница «эксперимент»](flex-layout-images/BasisExperiment.png "Страница «эксперимент»")](flex-layout-images/BasisExperiment-Large.png#lightbox)

На снимке экрана iOS слева показаны два `Label` элемента, высота которых задается в единицах, не зависящих от устройства. Экран Android показывает, что высота задается в виде доли от общей высоты `FlexLayout` . Если `Basis` задано значение 100%, то дочерний элемент является высотой `FlexLayout` , и будет переноситься к следующему столбцу и занимать всю высоту этого столбца, как показано на снимке экрана UWP: он выглядит так, как если бы пять потомков были расположены в строке, но фактически расположены в пяти столбцах.

### <a name="the-grow-property"></a>Свойство "рост"

[`Grow`](xref:Xamarin.Forms.FlexLayout.GrowProperty)Присоединенное свойство привязки имеет тип `int` . Значение по умолчанию равно 0, а значение должно быть больше или равно 0.

`Grow`Свойство играет роль, если `Wrap` свойство имеет значение, `NoWrap` а строка дочерних элементов имеет общую ширину меньше ширины `FlexLayout` , или столбец потомков имеет меньшую высоту, чем `FlexLayout` . `Grow`Свойство указывает, как распределять остаточное пространство между дочерними элементами.

На странице **увеличение эксперимента** пять `Label` элементов чередующихся цветов упорядочены по столбцам, а два `Slider` элемента позволяют настроить `Grow` свойство второго и четвертого `Label` . На снимке экрана iOS в левом углу отображаются свойства по умолчанию `Grow` 0:

[![Страница "увеличение эксперимента"](flex-layout-images/GrowExperiment.png "Страница "увеличение эксперимента"")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Если одному дочернему элементу присваивается положительное `Grow` значение, то этот дочерний элемент занимает все оставшееся пространство, как показано на снимке экрана Android. Это пространство также может быть распределено между двумя и более дочерними элементами. На снимке экрана UWP `Grow` свойству второго свойства `Label` присвоено значение 0,5, а `Grow` свойство четвертого `Label` — 1,5, что дает четвертое значение в `Label` три раза больше, чем во втором `Label` .

Как дочернее представление использует это пространство, зависит от конкретного типа дочернего элемента. Для `Label` текст можно разместить в общем пространстве `Label` с помощью свойств `HorizontalTextAlignment` и `VerticalTextAlignment` .

<a name="shrink" />

### <a name="the-shrink-property"></a>Свойство Shrink

[`Shrink`](xref:Xamarin.Forms.FlexLayout.ShrinkProperty)Присоединенное свойство привязки имеет тип `int` . Значение по умолчанию — 1, а значение должно быть больше или равно 0.

`Shrink`Свойство играет роль, если `Wrap` свойство имеет значение, `NoWrap` а суммарная ширина строки дочернего элемента больше, чем ширина `FlexLayout` , или агрегатная высота одного столбца дочерних элементов больше, чем высота `FlexLayout` . Обычно `FlexLayout` эти дочерние элементы отображаются путем ограниченного их размера. `Shrink`Свойство может указывать, какие дочерние элементы получают приоритет при отображении их полных размеров.

Страница **Сжатие эксперимента** создает объект `FlexLayout` с одной строкой из пяти `Label` дочерних элементов, для которых требуется больше пространства, чем `FlexLayout` Ширина. Снимок экрана iOS в левой части отображает все `Label` элементы со значениями по умолчанию 1:

[![Страница сжатия эксперимента](flex-layout-images/ShrinkExperiment.png "Страница сжатия эксперимента")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

На снимке экрана Android `Shrink` для второго параметра `Label` задается значение 0, которое `Label` отображается в полной ширине. Кроме того, четвертый `Label` имеет `Shrink` значение больше единицы и сжимается. На снимке экрана UWP показаны оба `Label` элемента, для которых задано `Shrink` значение 0, что позволяет отображать их в полном размере, если это возможно.

Можно задать оба `Grow` значения и, `Shrink` чтобы они соответствовали ситуациям, когда суммарные размеры дочернего элемента могут иногда быть меньше или больше, чем размер `FlexLayout` .

## <a name="css-styling-with-flexlayout"></a>Стилизация CSS с помощью Флекслайаут

Можно использовать функцию [стилизации CSS](~/xamarin-forms/user-interface/styles/css/index.md) , представленную в Xamarin. forms 3,0 в связи с `FlexLayout` . Страница « **элементы каталога CSS** » примера **[флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** дублирует макет страницы « **элементы каталога** », но с таблицей стилей CSS для многих стилей.

[![Страница «элементы каталога CSS»](flex-layout-images/CssCatalogItems.png "Страница «элементы каталога CSS»")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Исходный файл **каталогитемспаже. XAML** содержит пять `Style` определений в `Resources` разделе с 15 `Setter` объектами. В файле **ксскаталогитемспаже. XAML** , который был сокращен до двух `Style` определений с четырьмя `Setter` объектами. Эти стили дополняют таблицу стилей CSS для свойств, которые сейчас не поддерживаются в функции стилизации CSS в Xamarin. Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CssCatalogItemsPage"
             Title="CSS Catalog Items">
    <ContentPage.Resources>
        <StyleSheet Source="CatalogItemsStyles.css" />

        <Style TargetType="Frame">
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey" StyleClass="header" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey" StyleClass="header" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey" StyleClass="header" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

На таблицу стилей CSS имеется ссылка в первой строке `Resources` раздела:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Кроме того, обратите внимание, что два элемента в каждом из трех элементов включают в себя `StyleClass` Параметры:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Они ссылаются на селекторы в таблице стилей **каталогитемсстилес. CSS** :

```css
frame {
    width: 300;
    height: 480;
    background-color: lightyellow;
    margin: 10;
}

label {
    margin: 4 0;
}

label.header {
    margin: 8 0;
    font-size: large;
    color: blue;
}

label.empty {
    flex-grow: 1;
}

image {
    height: 180;
    order: -1;
    align-self: center;
}

button {
    font-size: large;
    color: white;
    background-color: green;
}
```

`FlexLayout`Здесь приведены ссылки на несколько присоединенных свойств привязки. В `label.empty` селекторе вы увидите `flex-grow` атрибут, в котором стили пусты, `Label` чтобы предоставить пустое пространство над `Button` . `image`Селектор содержит `order` атрибут и `align-self` атрибут, которые соответствуют `FlexLayout` присоединенным связанным свойствам.

Вы видели, что вы можете задать свойства непосредственно в, `FlexLayout` а также задать присоединенные привязываемые свойства к дочерним элементам `FlexLayout` . Можно также задать эти свойства косвенно с помощью традиционных стилей на основе XAML или стилей CSS. Важно знать и понять эти свойства. Эти свойства становятся `FlexLayout` действительно гибкими.

## <a name="flexlayout-with-xamarinuniversity"></a>Флекслайаут с Xamarin. Университет

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Видео о макете Xamarin. Forms 3,0 Flex**

## <a name="related-links"></a>Связанные ссылки

- [флекслайаутдемос](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)
