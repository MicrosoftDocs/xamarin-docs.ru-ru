---
title: Xamarin.Formsбоксвиев
description: В этой статье объясняется, как использовать цветной прямоугольник для оформления, графики и взаимодействия в Xamarin.Forms приложении.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3f4788c0201d2d286ff4de9b29ba6385d323a3b0
ms.sourcegitcommit: c3329ab25d377907d8804cdd5e26dc84a274f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/12/2020
ms.locfileid: "88130946"
---
# <a name="no-locxamarinforms-boxview"></a>Xamarin.Formsбоксвиев

[![Скачать пример](~/media/shared/download.png) Скачайте пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)

[`BoxView`](xref:Xamarin.Forms.BoxView)визуализирует простой прямоугольник с заданной шириной, высотой и цветом. Можно использовать `BoxView` для украшения, элементарной графики и взаимодействия с пользователем с помощью сенсорного ввода.

Поскольку не Xamarin.Forms имеет встроенной системы векторной графики, она `BoxView` помогает компенсировать. Некоторые из примеров программ, описанных в этой статье, используются `BoxView` для отрисовки графики. `BoxView`Можно выбрать размер, напоминающий строку определенной ширины и толщины, а затем поворачивать с любым углом с помощью `Rotation` Свойства.

Несмотря на `BoxView` то, что может имитировать простую графику, может потребоваться исследовать [Использование SkiaSharp в Xamarin.Forms ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) для более сложных требований к графике.

## <a name="setting-boxview-color-and-size"></a>Настройка цвета и размера Боксвиев

Обычно устанавливаются следующие свойства `BoxView` :

- [`Color`](xref:Xamarin.Forms.BoxView.Color)для установки его цвета.
- [`CornerRadius`](xref:Xamarin.Forms.BoxView.CornerRadius)для задания радиуса угла.
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)для задания ширины `BoxView` в единицах, не зависящих от устройства.
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)Задание высоты объекта `BoxView` .

`Color`Свойство имеет тип `Color` ; для свойства может быть задано любое `Color` значение, включая 141 статические поля только для чтения именованных цветов в алфавите от `AliceBlue` до `YellowGreen` .

`CornerRadius`Свойство имеет тип [`CornerRadius`](xref:Xamarin.Forms.CornerRadius) ; для свойства можно задать одно `double` однородное значение радиуса или `CornerRadius` структуру, определяемую четырьмя `double` значениями, которые применяются к верхнему левому краю, верхнему правому краю, нижнему левому краю и правому нижнему краю `BoxView` .

`WidthRequest`Свойства и `HeightRequest` играют роль только в том случае, если `BoxView` в макете нет *ограничений* . Это происходит, когда контейнеру макета необходимо получить сведения о размере дочернего элемента, например, если объект `BoxView` является дочерним по отношению к ячейке автоподбора в `Grid` макете. `BoxView`Кроме того, не ограничено, если `HorizontalOptions` `VerticalOptions` Свойства и установлены в значения, отличные от `LayoutOptions.Fill` . Если свойство `BoxView` не ограничено, но `WidthRequest` `HeightRequest` Свойства и не заданы, то для ширины или высоты устанавливаются значения по умолчанию 40 единиц или около 1/4 дюйма на мобильных устройствах.

`WidthRequest`Свойства и не `HeightRequest` учитываются `BoxView` , если *ограничен* в макете. в этом случае контейнер макета накладывает собственный размер в `BoxView` .

`BoxView` может быть ограниченным по одному измерению и неограниченным по другому. Например, если `BoxView` является дочерним элементом вертикального `StackLayout` , вертикальное измерение элемента `BoxView` не ограничено, а его горизонтальное измерение обычно ограничено. Но существуют исключения для этого горизонтального измерения: Если `BoxView` `HorizontalOptions` свойство имеет значение, отличное от, то `LayoutOptions.Fill` горизонтальное измерение также остается неограниченным. Также возможно `StackLayout` , что у самого себя есть неограниченное горизонтальное измерение, в этом случае `BoxView` также будет отображаться горизонтально неограниченный размер.

В примере [**басикбоксвиев**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview) в центре страницы отображается один 2,5-квадратный квадрат, который не ограничен `BoxView` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             CornerRadius="10"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center"
             HorizontalOptions="Center" />

</ContentPage>
```

Ниже приведен результат:

[![Базовый Боксвиев](boxview-images/basicboxview-small.png "Базовый Боксвиев")](boxview-images/basicboxview-large.png#lightbox "басикбоксвиев")

Если `VerticalOptions` Свойства и `HorizontalOptions` были удалены из `BoxView` тега или заданы как `Fill` , то `BoxView` преобразуются в ограничение по размеру страницы и разворачивается для заполнения страницы.

`BoxView`Может также быть дочерним элементом `AbsoluteLayout` . В этом случае расположение и размер `BoxView` задаются с помощью `LayoutBounds` присоединенного свойства BIND. `AbsoluteLayout`Описывается в статье [**абсолутелайаут**](~/xamarin-forms/user-interface/layouts/absolutelayout.md).

Вы увидите примеры всех этих вариантов в приведенных ниже примерах программ.

## <a name="rendering-text-decorations"></a>Отрисовка украшений текста

С помощью можно `BoxView` добавить некоторые простые украшения на страницы в виде горизонтальных и вертикальных линий. Это показано в образце [**TextDecoration**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration) . Все визуальные элементы программы определены в файле **MainPage. XAML** , который содержит несколько `Label` элементов и, `BoxView` `StackLayout` показанных здесь:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TextDecoration"
             x:Class="TextDecoration.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Black" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ScrollView Margin="15">
        <StackLayout>

            ···

        </StackLayout>
    </ScrollView>
</ContentPage>
```

Вся следующая разметка является дочерними для `StackLayout` . Эта разметка состоит из нескольких типов декоративных `BoxView` элементов, используемых с `Label` элементом.

[![Оформление текста](boxview-images/textdecoration-small.png "Оформление текста")](boxview-images/textdecoration-large.png#lightbox "Оформление текста")

Стиль верхнего колонтитула в верхней части страницы достигается тем, `AbsoluteLayout` чьи дочерние элементы являются четырьмя `BoxView` элементами `Label` , и, все из которых назначены определенные расположения и размеры:

```xaml
<AbsoluteLayout>
    <BoxView AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
    <BoxView AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
    <Label Text="Stylish Header"
           FontSize="24"
           AbsoluteLayout.LayoutBounds="30, 25, AutoSize, AutoSize"/>
</AbsoluteLayout>
```

В файле XAML `AbsoluteLayout` за ним следует `Label` отформатированный текст, описывающий `AbsoluteLayout` .

Текстовую строку можно подчеркнуть, заключив в нее `Label` и `BoxView` в `StackLayout` , `HorizontalOptions` для которой задано значение, отличное от `Fill` . Затем ширина элемента `StackLayout` регулируется шириной `Label` , которая затем накладывается на эту ширину `BoxView` . `BoxView`Присваивается только явная высота:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Этот метод нельзя использовать для подчеркивания отдельных слов в длинных текстовых строках или параграфах.

Также можно использовать, `BoxView` чтобы напоминать `hr` элемент HTML (горизонтальное правило). Просто позвольте, чтобы ширина `BoxView` определялась родительским контейнером, в данном случае — `StackLayout` :

```xaml
<BoxView HeightRequest="3" />
```

Наконец, можно нарисовать вертикальную линию на одной стороне абзаца текста, заключив `BoxView` и в `Label` горизонтальную `StackLayout` . В этом случае высота элемента совпадает с высотой `BoxView` `StackLayout` , которая регулируется высотой `Label` :

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
</StackLayout>
```

## <a name="listing-colors-with-boxview"></a>Перечисление цветов с помощью Боксвиев

Объект `BoxView` удобен для отображения цветов. Эта программа использует `ListView` для перечисления всех открытых статических полей структуры, которые доступны только для чтения Xamarin.Forms `Color` .

[![Цвета ListView](boxview-images/listviewcolors-small.png "Цвета ListView")](boxview-images/listviewcolors-large.png#lightbox "Цвета ListView")

Программа [**листвиевколорс**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors) включает класс с именем `NamedColor` . Статический конструктор использует отражение для доступа ко всем полям `Color` структуры и создает `NamedColor` объект для каждого из них. Они хранятся в `All` свойстве static:

```csharp
public class NamedColor
{
    // Instance members.
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    // Static members.
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields ())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof (Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                               (int)(255 * color.R),
                                               (int)(255 * color.G),
                                               (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }
}
```

Визуальные элементы программы описаны в файле XAML. `ItemsSource`Свойство объекта `ListView` задается в статическом `NamedColor.All` свойстве, что означает `ListView` Отображение всех отдельных `NamedColor` объектов:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewColors"
             x:Class="ListViewColors.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="10, 20, 10, 0" />
            <On Platform="Android, UWP" Value="10, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ListView SeparatorVisibility="None"
              ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.RowHeight>
            <OnPlatform x:TypeArguments="x:Int32">
                <On Platform="iOS, Android" Value="80" />
                <On Platform="UWP" Value="90" />
            </OnPlatform>
        </ListView.RowHeight>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ContentView Padding="5">
                        <Frame OutlineColor="Accent"
                               Padding="10">
                            <StackLayout Orientation="Horizontal">
                                <BoxView Color="{Binding Color}"
                                         WidthRequest="50"
                                         HeightRequest="50" />
                                <StackLayout>
                                    <Label Text="{Binding FriendlyName}"
                                           FontSize="22"
                                           VerticalOptions="StartAndExpand" />
                                    <Label Text="{Binding RgbDisplay, StringFormat='RGB = {0}'}"
                                           FontSize="16"
                                           VerticalOptions="CenterAndExpand" />
                                </StackLayout>
                            </StackLayout>
                        </Frame>
                    </ContentView>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

`NamedColor`Объекты форматируются с помощью `ViewCell` объекта, заданного в качестве шаблона данных `ListView` . Этот шаблон включает объект `BoxView` , `Color` свойство которого привязано к `Color` свойству `NamedColor` объекта.

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Воспроизведение игры с помощью подклассов Боксвиев

Игра в жизнь — это сотовый автоматизации, масематиЦиан Джон Конвея и популярный на страницах *научных Америки* в 1970-х. Хорошее введение представлено в игре в статье Википедии [Конвея](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Программа Xamarin.Forms [**гамеофлифе**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife) определяет класс с именем `LifeCell` , производным от `BoxView` . Этот класс инкапсулирует логику отдельной ячейки в игре жизнь:

```csharp
class LifeCell : BoxView
{
    bool isAlive;

    public event EventHandler Tapped;

    public LifeCell()
    {
        BackgroundColor = Color.White;

        TapGestureRecognizer tapGesture = new TapGestureRecognizer();
        tapGesture.Tapped += (sender, args) =>
        {
            Tapped?.Invoke(this, EventArgs.Empty);
        };
        GestureRecognizers.Add(tapGesture);
    }

    public int Col { set; get; }

    public int Row { set; get; }

    public bool IsAlive
    {
        set
        {
            if (isAlive != value)
            {
                isAlive = value;
                BackgroundColor = isAlive ? Color.Black : Color.White;
            }
        }
        get
        {
            return isAlive;
        }
    }
}
```

`LifeCell`добавляет еще три свойства в `BoxView` : `Col` Свойства и `Row` хранят расположение ячейки в сетке, а `IsAlive` свойство указывает его состояние. `IsAlive`Свойство также задает значение `Color` черного свойства, `BoxView` Если ячейка является активной, и белым, если ячейка не является активной.

`LifeCell`также устанавливает, `TapGestureRecognizer` чтобы разрешить пользователю переключать состояние ячеек, коснувшись их. Класс преобразует `Tapped` событие из распознавателя жестов в собственное `Tapped` событие.

Программа **гамеофлифе** также включает `LifeGrid` класс, который инкапсулирует большую часть логики игры, и `MainPage` класс, который обрабатывает визуальные элементы программы. К ним относятся наложение, описывающее правила игры. Ниже приведена программа в действии, показывающая пару сотен `LifeCell` объектов на странице:

[![Игра](boxview-images/gameoflife-small.png "Игра")](boxview-images/gameoflife-large.png#lightbox "Игра")

## <a name="creating-a-digital-clock"></a>Создание цифровых часов

Программа [**дотматриксклокк**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock) создает элементы 210 `BoxView` для имитации точек старого и постороннего матрицы с 5 по 7. Время можно прочитать в книжном или альбомном режиме, но оно имеет больший размер в альбомной ориентации:

[![Часовая точка-матрица](boxview-images/dotmatrixclock-small.png "Часовая точка-матрица")](boxview-images/dotmatrixclock-large.png#lightbox "Часовая точка-матрица")

Файл XAML выполняет немного больше, чем создание экземпляра, `AbsoluteLayout` используемого для часов:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DotMatrixClock"
             x:Class="DotMatrixClock.MainPage"
             Padding="10"
             SizeChanged="OnPageSizeChanged">

    <AbsoluteLayout x:Name="absoluteLayout"
                    VerticalOptions="Center" />
</ContentPage>
```

Все остальное происходит в файле кода программной части. Логика интерфейса для матричной матрицы значительно упрощена определением нескольких массивов, описывающих точки, соответствующие каждой из 10 цифр и двоеточия:

```csharp
public partial class MainPage : ContentPage
{
    // Total dots horizontally and vertically.
    const int horzDots = 41;
    const int vertDots = 7;

    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[, ,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 1, 1}, { 1, 0, 1, 0, 1},
            { 1, 1, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 0, 0}, { 0, 1, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0},
            { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0},
            { 0, 0, 1, 0, 0}, { 0, 1, 0, 0, 0}, { 1, 1, 1, 1, 1}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 0, 1, 0},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 0, 1, 0}, { 0, 0, 1, 1, 0}, { 0, 1, 0, 1, 0}, { 1, 0, 0, 1, 0},
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 0, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, { 0, 0, 0, 0, 1},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 1, 0}, { 0, 1, 0, 0, 0}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0},
            { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 1},
            { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 1, 1, 0, 0}
        },
    };

    // Dot matrix pattern for a colon.
    static readonly int[,] colonPattern = new int[7, 2]
    {
        { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }
    };

    // BoxView colors for on and off.
    static readonly Color colorOn = Color.Red;
    static readonly Color colorOff = new Color(0.5, 0.5, 0.5, 0.25);

    // Box views for 6 digits, 7 rows, 5 columns.
    BoxView[, ,] digitBoxViews = new BoxView[6, 7, 5];

    ···

}
```

Эти поля заканчиваются трехмерным массивом `BoxView` элементов для хранения шаблонов точек для шести цифр.

Конструктор создает все `BoxView` элементы для цифр и двоеточия, а также инициализирует `Color` свойство `BoxView` элементов для двоеточия:

```csharp
public partial class MainPage : ContentPage
{

    ···

    public MainPage()
    {
        InitializeComponent();

        // BoxView dot dimensions.
        double height = 0.85 / vertDots;
        double width = 0.85 / horzDots;

        // Create and assemble the BoxViews.
        double xIncrement = 1.0 / (horzDots - 1);
        double yIncrement = 1.0 / (vertDots - 1);
        double x = 0;

        for (int digit = 0; digit < 6; digit++)
        {
            for (int col = 0; col < 5; col++)
            {
                double y = 0;

                for (int row = 0; row < 7; row++)
                {
                    // Create the digit BoxView and add to layout.
                    BoxView boxView = new BoxView();
                    digitBoxViews[digit, row, col] = boxView;
                    absoluteLayout.Children.Add(boxView,
                                                new Rectangle(x, y, width, height),
                                                AbsoluteLayoutFlags.All);
                    y += yIncrement;
                }
                x += xIncrement;
            }
            x += xIncrement;

            // Colons between the hours, minutes, and seconds.
            if (digit == 1 || digit == 3)
            {
                int colon = digit / 2;

                for (int col = 0; col < 2; col++)
                {
                    double y = 0;

                    for (int row = 0; row < 7; row++)
                    {
                        // Create the BoxView and set the color.
                        BoxView boxView = new BoxView
                            {
                                Color = colonPattern[row, col] == 1 ?
                                            colorOn : colorOff
                            };
                        absoluteLayout.Children.Add(boxView,
                                                    new Rectangle(x, y, width, height),
                                                    AbsoluteLayoutFlags.All);
                        y += yIncrement;
                    }
                    x += xIncrement;
                }
                x += xIncrement;
            }
        }

        // Set the timer and initialize with a manual call.
        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimer);
        OnTimer();
    }

    ···

}
```

Эта программа использует функцию относительного позиционирования и изменения размера `AbsoluteLayout` . Ширина и высота каждого `BoxView` задаются для дробных значений, в частности 85% от 1, деленного на число горизонтальных и вертикальных точек. Позиции также устанавливаются в дробные значения.

Так как все позиции и размеры зависят от общего размера `AbsoluteLayout` , `SizeChanged` обработчику для этой страницы требуется только задать для `HeightRequest` `AbsoluteLayout` :

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnPageSizeChanged(object sender, EventArgs args)
    {
        // No chance a display will have an aspect ratio > 41:7
        absoluteLayout.HeightRequest = vertDots * Width / horzDots;
    }

    ···

}
```

Ширина элемента `AbsoluteLayout` автоматически задается, так как она растягивается до полной ширины страницы.

Окончательный код в `MainPage` классе обрабатывает обратный вызов таймера и цвета точек каждой цифры. Определение многомерных массивов в начале файла кода программной части позволяет сделать эту логику самой простой частью программы:

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimer()
    {
        DateTime dateTime = DateTime.Now;

        // Convert 24-hour clock to 12-hour clock.
        int hour = (dateTime.Hour + 11) % 12 + 1;

        // Set the dot colors for each digit separately.
        SetDotMatrix(0, hour / 10);
        SetDotMatrix(1, hour % 10);
        SetDotMatrix(2, dateTime.Minute / 10);
        SetDotMatrix(3, dateTime.Minute % 10);
        SetDotMatrix(4, dateTime.Second / 10);
        SetDotMatrix(5, dateTime.Second % 10);
        return true;
    }

    void SetDotMatrix(int index, int digit)
    {
        for (int row = 0; row < 7; row++)
            for (int col = 0; col < 5; col++)
            {
                bool isOn = numberPatterns[digit, row, col] == 1;
                Color color = isOn ? colorOn : colorOff;
                digitBoxViews[index, row, col].Color = color;
            }
    }
}
```

## <a name="creating-an-analog-clock"></a>Создание аналоговых часов

Часовая матрица может показаться очевидным приложением `BoxView` , но элементы также могут использовать `BoxView` аналоговые часы:

[![Боксвиев часы](boxview-images/boxviewclock-small.png "Боксвиев часы")](boxview-images/boxviewclock-large.png#lightbox "Боксвиев часы")

Все визуальные элементы в программе [**боксвиевклокк**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) являются дочерними элементами `AbsoluteLayout` . Размер этих элементов определяется с помощью `LayoutBounds` присоединенного свойства и поворачивается с помощью `Rotation` Свойства.

Три `BoxView` элемента для работы с часами создаются в файле XAML, но не расположены в расположении или размере:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BoxViewClock"
             x:Class="BoxViewClock.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <AbsoluteLayout x:Name="absoluteLayout"
                    SizeChanged="OnAbsoluteLayoutSizeChanged">

        <BoxView x:Name="hourHand"
                 Color="Black" />

        <BoxView x:Name="minuteHand"
                 Color="Black" />

        <BoxView x:Name="secondHand"
                 Color="Black" />
    </AbsoluteLayout>
</ContentPage>
```

Конструктор файла кода программной части создает экземпляры `BoxView` элементов 60 для делений, обозначающих длину окружности часов:

```csharp
public partial class MainPage : ContentPage
{

    ···

    BoxView[] tickMarks = new BoxView[60];

    public MainPage()
    {
        InitializeComponent();

        // Create the tick marks (to be sized and positioned later).
        for (int i = 0; i < tickMarks.Length; i++)
        {
            tickMarks[i] = new BoxView { Color = Color.Black };
            absoluteLayout.Children.Add(tickMarks[i]);
        }

        Device.StartTimer(TimeSpan.FromSeconds(1.0 / 60), OnTimerTick);
    }

    ···

}
```

Изменение размера и положения всех `BoxView` элементов происходит в `SizeChanged` обработчике для `AbsoluteLayout` . Небольшая структура, которая является внутренней для класса с именем, `HandParams` описывает размер каждой из трех стрелок относительно общего размера часов:

```csharp
public partial class MainPage : ContentPage
{
    // Structure for storing information about the three hands.
    struct HandParams
    {
        public HandParams(double width, double height, double offset) : this()
        {
            Width = width;
            Height = height;
            Offset = offset;
        }

        public double Width { private set; get; }   // fraction of radius
        public double Height { private set; get; }  // ditto
        public double Offset { private set; get; }  // relative to center pivot
    }

    static readonly HandParams secondParams = new HandParams(0.02, 1.1, 0.85);
    static readonly HandParams minuteParams = new HandParams(0.05, 0.8, 0.9);
    static readonly HandParams hourParams = new HandParams(0.125, 0.65, 0.9);

    ···

 }
```

`SizeChanged`Обработчик определяет центр и радиус `AbsoluteLayout` , а затем изменяет размеры и положение элементов 60, `BoxView` используемых в качестве делений. `for`Цикл завершается установкой `Rotation` свойства каждого из этих `BoxView` элементов. В конце `SizeChanged` обработчика `LayoutHand` вызывается метод для изменения размера и позиционирования трех рук часов:

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnAbsoluteLayoutSizeChanged(object sender, EventArgs args)
    {
        // Get the center and radius of the AbsoluteLayout.
        Point center = new Point(absoluteLayout.Width / 2, absoluteLayout.Height / 2);
        double radius = 0.45 * Math.Min(absoluteLayout.Width, absoluteLayout.Height);

        // Position, size, and rotate the 60 tick marks.
        for (int index = 0; index < tickMarks.Length; index++)
        {
            double size = radius / (index % 5 == 0 ? 15 : 30);
            double radians = index * 2 * Math.PI / tickMarks.Length;
            double x = center.X + radius * Math.Sin(radians) - size / 2;
            double y = center.Y - radius * Math.Cos(radians) - size / 2;
            AbsoluteLayout.SetLayoutBounds(tickMarks[index], new Rectangle(x, y, size, size));
            tickMarks[index].Rotation = 180 * radians / Math.PI;
        }

        // Position and size the three hands.
        LayoutHand(secondHand, secondParams, center, radius);
        LayoutHand(minuteHand, minuteParams, center, radius);
        LayoutHand(hourHand, hourParams, center, radius);
    }

    void LayoutHand(BoxView boxView, HandParams handParams, Point center, double radius)
    {
        double width = handParams.Width * radius;
        double height = handParams.Height * radius;
        double offset = handParams.Offset;

        AbsoluteLayout.SetLayoutBounds(boxView,
            new Rectangle(center.X - 0.5 * width,
                          center.Y - offset * height,
                          width, height));

        // Set the AnchorY property for rotations.
        boxView.AnchorY = handParams.Offset;
    }

    ···

}
```

`LayoutHand`Размеры и положения каждого метода указываются прямо в позиции 12:00. В конце метода `AnchorY` для свойства задается расположение, соответствующее центру часов. Указывает центр вращения.

Стрелки поворачиваются в функции обратного вызова таймера:

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimerTick()
    {
        // Set rotation angles for hour and minute hands.
        DateTime dateTime = DateTime.Now;
        hourHand.Rotation = 30 * (dateTime.Hour % 12) + 0.5 * dateTime.Minute;
        minuteHand.Rotation = 6 * dateTime.Minute + 0.1 * dateTime.Second;

        // Do an animation for the second hand.
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        secondHand.Rotation = 6 * (dateTime.Second + t);
        return true;
    }
}
```

Вторая рука несколько отличается: функция плавности анимации применяется для того, чтобы движение считалось механическим, а не гладким. На каждом такте вторая рука запрашивается немного, а затем перемещает свое назначение. Этот небольшой фрагмент кода значительно увеличивает реальную часть движения.

## <a name="related-links"></a>Связанные ссылки

- [Базовый Боксвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)
- [Оформление текста (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)
- [Цвета ListView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)
- [Игра (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)
- [Матричные часы (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)
- [Боксвиев часов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)
- [BoxView](xref:Xamarin.Forms.BoxView)
