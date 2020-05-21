---
title: Сетка Xamarin. Forms
description: Сетка Xamarin. Forms представляет собой макет, который упорядочивает свои дочерние элементы по строкам и столбцам ячеек.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/15/2020
ms.openlocfilehash: 4f1d9d0f2d597018b9832d918bbec3f0b2594773
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/15/2020
ms.locfileid: "83425986"
---
# <a name="xamarinforms-grid"></a>Сетка Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-griddemos)

[![Сетка Xamarin. Forms](grid-images/layouts.png "Сетка Xamarin. Forms")](grid-images/layouts-large.png#lightbox "Сетка Xamarin. Forms")

[`Grid`](xref:Xamarin.Forms.Grid)— Это макет, который упорядочивает свои дочерние элементы по строкам и столбцам, которые могут иметь пропорциональные или абсолютные размеры. По умолчанию `Grid` содержит одну строку и один столбец. Кроме того, `Grid` можно использовать в качестве родительского макета, содержащего другие дочерние макеты.

[`Grid`](xref:Xamarin.Forms.Grid)Макет не следует путать с таблицами и не предназначен для представления табличных данных. В отличие от таблиц HTML, `Grid` предназначено для размещения содержимого. Для отображения табличных данных рассмотрите возможность использования [ListView](~/xamarin-forms/user-interface/listview/index.md), [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)или [таблевиев](~/xamarin-forms/user-interface/tableview.md).

[`Grid`](xref:Xamarin.Forms.Grid)Класс определяет следующие свойства:

- [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty)Тип — `int` это присоединенное свойство, которое указывает выравнивание столбцов в родительском представлении `Grid` . Значение по умолчанию для этого свойства равно 0. Обратный вызов проверки гарантирует, что если свойство задано, его значение больше или равно 0.
- [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions)тип — это [`ColumnDefinitionCollection`](xref:Xamarin.Forms.ColumnDefinitionCollection) список [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) объектов, определяющих ширину столбцов сетки.
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing)Тип — `double` указывает расстояние между столбцами сетки. Значение этого свойства по умолчанию равно 6 единицам, не зависящим от устройства.
- [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)Тип `int` , который является присоединенным свойством, указывающим общее число столбцов, охватываемое представлением в пределах родителя `Grid` . Значение этого свойства по умолчанию равно 1. Обратный вызов проверки гарантирует, что если свойство задано, его значение больше или равно 1.
- [`Row`](xref:Xamarin.Forms.Grid.RowProperty)Тип — `int` это присоединенное свойство, которое указывает выравнивание строки для представления в родительском элементе `Grid` . Значение по умолчанию для этого свойства равно 0. Обратный вызов проверки гарантирует, что если свойство задано, его значение больше или равно 0.
- [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions)тип — это [`RowDefinitionCollection`](xref:Xamarin.Forms.RowDefinitionCollection) список [`RowDefintion`](xref:Xamarin.Forms.RowDefinition) объектов, определяющих высоту строк сетки.
- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing)Тип — `double` указывает расстояние между строками сетки. Значение этого свойства по умолчанию равно 6 единицам, не зависящим от устройства.
- [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)Тип — `int` это присоединенное свойство, указывающее общее количество строк, которые представление охватывает в родительском представлении `Grid` . Значение этого свойства по умолчанию равно 1. Обратный вызов проверки гарантирует, что если свойство задано, его значение больше или равно 1.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами для привязок данных и стилей.

[`Grid`](xref:Xamarin.Forms.Grid)Класс является производным от `Layout<T>` класса, который определяет `Children` свойство типа `IList<T>` . `Children`Свойство является свойством `ContentProperty` `Layout<T>` класса, поэтому его не нужно явно задавать из XAML.

> [!TIP]
> Чтобы получить максимально возможную производительность макета, следуйте рекомендациям по [оптимизации производительности макета](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance).

## <a name="rows-and-columns"></a>Строки и столбцы

По умолчанию [`Grid`](xref:Xamarin.Forms.Grid) содержит одну строку и один столбец:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridTutorial.MainPage">
    <Grid Margin="20,35,20,20">
        <Label Text="By default, a Grid contains one row and one column." />
    </Grid>
</ContentPage>
```

В этом примере элемент [`Grid`](xref:Xamarin.Forms.Grid) содержит один дочерний элемент [`Label`](xref:Xamarin.Forms.Label) , который автоматически размещается в одном расположении:

[![Снимок экрана с макетом сетки по умолчанию](grid-images/default.png "Макет сетки по умолчанию")](grid-images/default-large.png#lightbox "Макет сетки по умолчанию")

Поведение макета [`Grid`](xref:Xamarin.Forms.Grid) может быть определено с помощью [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) свойств и, которые являются коллекциями [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) объектов и соответственно. Эти коллекции определяют характеристики строк и столбцов `Grid` , а также должны содержать один [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) объект для каждой строки в `Grid` и один [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) объект для каждого столбца в `Grid` .

[`RowDefinition`](xref:Xamarin.Forms.RowDefinition)Класс определяет [`Height`](xref:Xamarin.Forms.RowDefinition.Height) свойство типа [`GridLength`](xref:Xamarin.Forms.GridLength) , а [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) класс определяет [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) свойство типа [`GridLength`](xref:Xamarin.Forms.GridLength) . [`GridLength`](xref:Xamarin.Forms.GridLength)Структура задает высоту строки или ширину столбца в терминах [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) перечисления, которая имеет три члена:

- `Absolute`— Высота строки или ширина столбца — это значение в единицах, не зависящих от устройства (число в XAML).
- `Auto`— Высота строки или ширина столбца определяется авторазмерами на основе содержимого ячейки ( `Auto` в XAML).
- `Star`— Высота неразнесенной строки или ширина столбца выделяется пропорционально (число, за которым следует `*` код XAML).

[`Grid`](xref:Xamarin.Forms.Grid)Строка со `Height` свойством `Auto` ограничивает высоту представлений в этой строке так же, как и по вертикали [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Аналогичным образом столбец со `Width` свойством `Auto` работает примерно так же, как по горизонтали `StackLayout` .

> [!CAUTION]
> Старайтесь сделать так, чтобы максимально допустимое число строк и столбцов было равно [`Auto`](xref:Xamarin.Forms.GridLength.Auto) размеру. Из-за каждой строки или столбца с автоматическим размером обработчик макета будет выполнять дополнительные вычисления макета. Если возможно, используйте строки и столбцы фиксированного размера. Кроме того, можно задать для строк и столбцов занимать пропорциональный объем пространства со [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) значением перечисления.

В следующем коде XAML показано, как создать объект [`Grid`](xref:Xamarin.Forms.Grid) с тремя строками и двумя столбцами:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.BasicGridPage"
             Title="Basic Grid demo">
   <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

В этом примере объект [`Grid`](xref:Xamarin.Forms.Grid) имеет общую высоту, которая является высотой страницы. `Grid`Известно, что высота третьей строки составляет 100 единиц, независимых от устройства. Она вычитает эту высоту из своей собственной высоты и выделяет оставшуюся высоту пропорционально между первой и второй строками, исходя из числа до звезды. В этом примере высота первой строки находится в два раза больше второй строки.

[`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition)Оба объекта устанавливают для значение, то есть то же, что и [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) `*` , что `1*` означает, что ширина экрана делится под двумя столбцами.

> [!IMPORTANT]
> Значение свойства по умолчанию [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) — `*` . Аналогичным образом значение свойства по умолчанию [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) — `*` . Поэтому нет необходимости задавать эти свойства в тех случаях, когда допустимы эти значения по умолчанию.

Дочерние представления можно располагать в конкретных [`Grid`](xref:Xamarin.Forms.Grid) ячейках с [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) вложенными свойствами и. Кроме того, чтобы сделать дочерние представления охватывающими несколько строк и столбцов, [`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) Используйте [`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) вложенные свойства и.

В следующем коде XAML показано то же [`Grid`](xref:Xamarin.Forms.Grid) Определение, а также размещается дочерние представления в конкретных `Grid` ячейках:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.BasicGridPage"
             Title="Basic Grid demo">
   <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <BoxView Color="Green" />
        <Label Text="Row 0, Column 0"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Column="1"
                 Color="Blue" />
        <Label Grid.Column="1"
               Text="Row 0, Column 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Color="Teal" />
        <Label Grid.Row="1"
               Text="Row 1, Column 0"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="1"
                 Color="Purple" />
        <Label Grid.Row="1"
               Grid.Column="1"
               Text="Row1, Column 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="2"
                 Grid.ColumnSpan="2"
                 Color="Red" />
        <Label Grid.Row="2"
               Grid.ColumnSpan="2"
               Text="Row 2, Columns 0 and 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

> [!NOTE]
> `Grid.Row`Свойства и `Grid.Column` индексируются из 0, поэтому `Grid.Row="2"` ссылается на третью строку, а `Grid.Column="1"` ссылается на второй столбец. Кроме того, оба этих свойства имеют значение по умолчанию 0, поэтому не нужно задавать их для дочерних представлений, которые занимают первую строку или первый столбец в [`Grid`](xref:Xamarin.Forms.Grid) .

В этом примере все три [`Grid`](xref:Xamarin.Forms.Grid) строки заняты [`BoxView`](xref:Xamarin.Forms.BoxView) [`Label`](xref:Xamarin.Forms.Label) представлениями и. Третья строка имеет значение 100 единиц, не зависящих от устройства, и первые две строки занимают оставшееся пространство (первая строка будет вдвое выше, чем вторая строка). Два столбца имеют одинаковую ширину и делятся на `Grid` половину. `BoxView`В третьей строке охватывает оба столбца.

[![Снимок экрана: базовый макет сетки](grid-images/basic.png "Базовый макет сетки")](grid-images/basic-large.png#lightbox "Базовый макет сетки")

Кроме того, дочерние представления в [`Grid`](xref:Xamarin.Forms.Grid) могут совместно использовать ячейки. Порядок, в котором дочерние элементы отображаются в XAML, — это порядок, в котором дочерние элементы помещаются в `Grid` . В предыдущем примере [`Label`](xref:Xamarin.Forms.Label) объекты видимы только в том случае, если они отображаются поверх [`BoxView`](xref:Xamarin.Forms.BoxView) объектов. `Label`Объекты не будут видны, если `BoxView` объекты были визуализированы поверх них.

Эквивалентный код на C# выглядит так:

```csharp
public class BasicGridPageCS : ContentPage
{
    public BasicGridPageCS()
    {
        Grid grid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition { Height = new GridLength(2, GridUnitType.Star) },
                new RowDefinition(),
                new RowDefinition { Height = new GridLength(100) }
            },
            ColumnDefinitions =
            {
                new ColumnDefinition(),
                new ColumnDefinition()
            }
        };

        // Row 0
        // The BoxView and Label are in row 0 and column 0, and so only needs to be added to the
        // Grid.Children collection to get default row and column settings.
        grid.Children.Add(new BoxView
        {
            Color = Color.Green
        });
        grid.Children.Add(new Label
        {
            Text = "Row 0, Column 0",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        });

        // This BoxView and Label are in row 0 and column 1, which are specified as arguments
        // to the Add method.
        grid.Children.Add(new BoxView
        {
            Color = Color.Blue
        }, 1, 0);
        grid.Children.Add(new Label
        {
            Text = "Row 0, Column 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 0);

        // Row 1
        // This BoxView and Label are in row 1 and column 0, which are specified as arguments
        // to the Add method overload.
        grid.Children.Add(new BoxView
        {
            Color = Color.Teal
        }, 0, 1, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Row 1, Column 0",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 0, 1, 1, 2); // These arguments indicate that that the child element goes in the column starting at 0 but ending before 1.
                        // They also indicate that the child element goes in the row starting at 1 but ending before 2.

        grid.Children.Add(new BoxView
        {
            Color = Color.Purple
        }, 1, 2, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Row1, Column 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 2, 1, 2);

        // Row 2
        // Alternatively, the BoxView and Label can be positioned in cells with the Grid.SetRow
        // and Grid.SetColumn methods.
        BoxView boxView = new BoxView { Color = Color.Red };
        Grid.SetRow(boxView, 2);
        Grid.SetColumnSpan(boxView, 2);
        Label label = new Label
        {
            Text = "Row 2, Column 0 and 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        };
        Grid.SetRow(label, 2);
        Grid.SetColumnSpan(label, 2);

        grid.Children.Add(boxView);
        grid.Children.Add(label);

        Title = "Basic Grid demo";
        Content = grid;
    }
}
```

В коде, чтобы указать высоту [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) объекта и ширину [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) объекта, используются значения [`GridLength`](xref:Xamarin.Forms.GridLength) структуры, часто в сочетании с [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) перечислением.

В приведенном выше примере кода также показано несколько различных подходов к добавлению дочерних элементов в [`Grid`](xref:Xamarin.Forms.Grid) и указания ячеек, в которых они находятся. При использовании `Add` перегрузки, указывающей *левые*, *правые*, *верхние*и *нижние* аргументы, а аргументы *Left* и *Top* всегда ссылаются на ячейки внутри `Grid` , аргументы *right* и *Bottom* будут ссылаться на ячейки, находящиеся за пределами `Grid` . Это происходит потому, что *правый* аргумент всегда должен быть больше *левого* аргумента, а *Нижний* аргумент всегда должен быть больше, чем *верхний* аргумент. В следующем примере, который предполагает использование 2x2 `Grid` , показан эквивалентный код, использующий обе `Add` перегрузки:

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);           // first column, first row
grid.Children.Add(topRight, 1, 0);          // second column, first tow
grid.Children.Add(bottomLeft, 0, 1);        // first column, second row
grid.Children.Add(bottomRight, 1, 1);       // second column, second row

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);     // first column, first row
grid.Children.Add(topRight, 1, 2, 0, 1);    // second column, first tow
grid.Children.Add(bottomLeft, 0, 1, 1, 2);  // first column, second row
grid.Children.Add(bottomRight, 1, 2, 1, 2); // second column, second row
```

> [!NOTE]
> Кроме того, дочерние представления можно добавлять в [`Grid`](xref:Xamarin.Forms.Grid) с помощью [`AddHorizontal`](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) [`AddVertical`](xref:Xamarin.Forms.Grid.IGridList`1.AddVertical*) методов и, которые добавляют дочерние элементы в одну строку или один столбец `Grid` . `Grid`Затем разворачивает строки или столбцы по мере совершения этих вызовов, а также автоматически размещая потомки в нужных ячейках.

## <a name="space-between-rows-and-columns"></a>Расстояние между строками и столбцами

По умолчанию [`Grid`](xref:Xamarin.Forms.Grid) строки разделяются 6 независимыми от устройства единицами пространства. Аналогичным образом `Grid` Столбцы разделяются 6 независимыми от устройства единицами пространства. Эти значения по умолчанию можно изменить, задав [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) Свойства и соответственно.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.GridSpacingPage"
             Title="Grid spacing demo">
    <Grid RowSpacing="0"
          ColumnSpacing="0">
        ..
    </Grid>
</ContentPage>
```

В этом примере создается [`Grid`](xref:Xamarin.Forms.Grid) , у которого нет интервала между строками и столбцами:

[![Снимок экрана: сетка без интервала между ячейками](grid-images/spacing.png "Сетка без интервала между ячейками")](grid-images/spacing-large.png#lightbox "Сетка без интервала между ячейками")

> [!TIP]
> [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) Для свойств и можно задать отрицательные значения, чтобы содержимое ячеек было перекрываться.

Эквивалентный код на C# выглядит так:

```csharp
public GridSpacingPageCS()
{
    Grid grid = new Grid
    {
        RowSpacing = 0,
        ColumnSpacing = 0,
        // ...
    };
    // ...

    Content = grid;
}
```

## <a name="alignment"></a>Выравнивание

Дочерние представления в [`Grid`](xref:Xamarin.Forms.Grid) можно располагать в своих ячейках по [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) свойствам и. Для этих свойств можно задать следующие поля из [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) структуры:

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)

> [!IMPORTANT]
> `AndExpands`Поля в [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) структуре применимы только к [`StackLayout`](xref:Xamarin.Forms.StackLayout) объектам.

Следующий код XAML создает [`Grid`](xref:Xamarin.Forms.Grid) с девятью ячеек одинакового размера и помещает [`Label`](xref:Xamarin.Forms.Label) в каждую ячейку с другим выравниванием:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.GridAlignmentPage"
             Title="Grid alignment demo">
    <Grid RowSpacing="0"
          ColumnSpacing="0">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>

        <BoxView Color="AliceBlue" />
        <Label Text="Upper left"
               HorizontalOptions="Start"
               VerticalOptions="Start" />
        <BoxView Grid.Column="1"
                 Color="LightSkyBlue" />
        <Label Grid.Column="1"
               Text="Upper center"
               HorizontalOptions="Center"
               VerticalOptions="Start"/>
        <BoxView Grid.Column="2"
                 Color="CadetBlue" />
        <Label Grid.Column="2"
               Text="Upper right"
               HorizontalOptions="End"
               VerticalOptions="Start" />
        <BoxView Grid.Row="1"
                 Color="CornflowerBlue" />
        <Label Grid.Row="1"
               Text="Center left"
               HorizontalOptions="Start"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="1"
                 Color="DodgerBlue" />
        <Label Grid.Row="1"
               Grid.Column="1"
               Text="Center center"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="2"
                 Color="DarkSlateBlue" />
        <Label Grid.Row="1"
               Grid.Column="2"
               Text="Center right"
               HorizontalOptions="End"
               VerticalOptions="Center" />
        <BoxView Grid.Row="2"
                 Color="SteelBlue" />
        <Label Grid.Row="2"
               Text="Lower left"
               HorizontalOptions="Start"
               VerticalOptions="End" />
        <BoxView Grid.Row="2"
                 Grid.Column="1"
                 Color="LightBlue" />
        <Label Grid.Row="2"
               Grid.Column="1"
               Text="Lower center"
               HorizontalOptions="Center"
               VerticalOptions="End" />
        <BoxView Grid.Row="2"
                 Grid.Column="2"
                 Color="BlueViolet" />
        <Label Grid.Row="2"
               Grid.Column="2"
               Text="Lower right"
               HorizontalOptions="End"
               VerticalOptions="End" />
    </Grid>
</ContentPage>
```

В этом примере [`Label`](xref:Xamarin.Forms.Label) объекты в каждой строке одинаково выровнены по вертикали, но используют разные горизонтальные выравнивания. Кроме того, это можно рассматривать как `Label` объекты в каждом столбце одинаково выровнены по горизонтали, но при этом используются разные вертикальные выравнивания:

[![Снимок экрана: выравнивание ячейки в сетке](grid-images/alignment.png "Выравнивание ячеек в сетке")](grid-images/alignment-large.png#lightbox "Выравнивание ячеек в сетке")

Эквивалентный код на C# выглядит так:

```csharp
public class GridAlignmentPageCS : ContentPage
{
    public GridAlignmentPageCS()
    {
        Grid grid = new Grid
        {
            RowSpacing = 0,
            ColumnSpacing = 0,
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition()
            },
            ColumnDefinitions =
            {
                new ColumnDefinition(),
                new ColumnDefinition(),
                new ColumnDefinition()
            }
        };

        // Row 0
        grid.Children.Add(new BoxView
        {
            Color = Color.AliceBlue
        });
        grid.Children.Add(new Label
        {
            Text = "Upper left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Start
        });

        grid.Children.Add(new BoxView
        {
            Color = Color.LightSkyBlue
        }, 1, 0);
        grid.Children.Add(new Label
        {
            Text = "Upper center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Start
        }, 1, 0);

        grid.Children.Add(new BoxView
        {
            Color = Color.CadetBlue
        }, 2, 0);
        grid.Children.Add(new Label
        {
            Text = "Upper right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.Start
        }, 2, 0);

        // Row 1
        grid.Children.Add(new BoxView
        {
            Color = Color.CornflowerBlue
        }, 0, 1);
        grid.Children.Add(new Label
        {
            Text = "Center left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Center
        }, 0, 1);

        grid.Children.Add(new BoxView
        {
            Color = Color.DodgerBlue
        }, 1, 1);
        grid.Children.Add(new Label
        {
            Text = "Center center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 1);

        grid.Children.Add(new BoxView
        {
            Color = Color.DarkSlateBlue
        }, 2, 1);
        grid.Children.Add(new Label
        {
            Text = "Center right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.Center
        }, 2, 1);

        // Row 2
        grid.Children.Add(new BoxView
        {
            Color = Color.SteelBlue
        }, 0, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.End
        }, 0, 2);

        grid.Children.Add(new BoxView
        {
            Color = Color.LightBlue
        }, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.End
        }, 1, 2);

        grid.Children.Add(new BoxView
        {
            Color = Color.BlueViolet
        }, 2, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.End
        }, 2, 2);

        Title = "Grid alignment demo";
        Content = grid;
    }
}
```

## <a name="nested-grid-objects"></a>Вложенные объекты Grid

[`Grid`](xref:Xamarin.Forms.Grid)Можно использовать в качестве родительского макета, содержащего вложенные дочерние `Grid` объекты или другие дочерние макеты. При вложении `Grid` объектов вложенные `Grid.Row` свойства,, `Grid.Column` `Grid.RowSpan` и `Grid.ColumnSpan` всегда ссылаются на расположение представлений в их родительском объекте `Grid` .

В следующем коде XAML показан пример вложенных [`Grid`](xref:Xamarin.Forms.Grid) объектов:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:converters="clr-namespace:GridDemos.Converters"
             x:Class="GridDemos.Views.ColorSlidersGridPage"
             Title="Nested Grids demo">

    <ContentPage.Resources>
        <converters:DoubleToIntConverter x:Key="doubleToInt" />

        <Style TargetType="Label">
            <Setter Property="HorizontalTextAlignment"
                    Value="Center" />
        </Style>
    </ContentPage.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <BoxView x:Name="boxView"
                 Color="Black" />
        <Grid Grid.Row="1"
              Margin="20">
            <Grid.RowDefinitions>
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
            </Grid.RowDefinitions>
            <Slider x:Name="redSlider"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="1"
                   Text="{Binding Source={x:Reference redSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0}'}" />
            <Slider x:Name="greenSlider"
                    Grid.Row="2"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="3"
                   Text="{Binding Source={x:Reference greenSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0}'}" />
            <Slider x:Name="blueSlider"
                    Grid.Row="4"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="5"
                   Text="{Binding Source={x:Reference blueSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Blue = {0}'}" />
        </Grid>
    </Grid>
</ContentPage>
```

В этом примере корневой [`Grid`](xref:Xamarin.Forms.Grid) Макет содержит [`BoxView`](xref:Xamarin.Forms.BoxView) в первой строке и дочерний элемент `Grid` во второй строке. Дочерний объект `Grid` содержит [`Slider`](xref:Xamarin.Forms.Slider) объекты, управляющие цветом, который отображается в `BoxView` , и [`Label`](xref:Xamarin.Forms.Label) объекты, отображающие значение каждого из них `Slider` :

[![Снимок экрана вложенных сеток](grid-images/nesting.png "Вложенные объекты Grid")](grid-images/nesting-large.png#lightbox "Вложенные объекты Grid")

> [!IMPORTANT]
> Чем глубже вы вкладывает [`Grid`](xref:Xamarin.Forms.Grid) объекты и другие макеты, тем больше вложенных макетов влияет на производительность. Дополнительные сведения см. [в разделе Выбор правильного макета](~/xamarin-forms/deploy-test/performance.md#choose-the-correct-layout).

Эквивалентный код на C# выглядит так:

```csharp
public class ColorSlidersGridPageCS : ContentPage
{
    BoxView boxView;
    Slider redSlider;
    Slider greenSlider;
    Slider blueSlider;

    public ColorSlidersGridPageCS()
    {
        // Create an implicit style for the Labels
        Style labelStyle = new Style(typeof(Label))
        {
            Setters =
            {
                new Setter { Property = Label.HorizontalTextAlignmentProperty, Value = TextAlignment.Center }
            }
        };
        Resources.Add(labelStyle);

        // Root page layout
        Grid rootGrid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition()
            }
        };

        boxView = new BoxView { Color = Color.Black };
        rootGrid.Children.Add(boxView);

        // Child page layout
        Grid childGrid = new Grid
        {
            Margin = new Thickness(20),
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition()
            }
        };

        DoubleToIntConverter doubleToInt = new DoubleToIntConverter();

        redSlider = new Slider();
        redSlider.ValueChanged += OnSliderValueChanged;
        childGrid.Children.Add(redSlider);

        Label redLabel = new Label();
        redLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Red = {0}", source: redSlider));
        Grid.SetRow(redLabel, 1);
        childGrid.Children.Add(redLabel);

        greenSlider = new Slider();
        greenSlider.ValueChanged += OnSliderValueChanged;
        Grid.SetRow(greenSlider, 2);
        childGrid.Children.Add(greenSlider);

        Label greenLabel = new Label();
        greenLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Green = {0}", source: greenSlider));
        Grid.SetRow(greenLabel, 3);
        childGrid.Children.Add(greenLabel);

        blueSlider = new Slider();
        blueSlider.ValueChanged += OnSliderValueChanged;
        Grid.SetRow(blueSlider, 4);
        childGrid.Children.Add(blueSlider);

        Label blueLabel = new Label();
        blueLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Blue = {0}", source: blueSlider));
        Grid.SetRow(blueLabel, 5);
        childGrid.Children.Add(blueLabel);

        // Place the child Grid in the root Grid
        rootGrid.Children.Add(childGrid, 0, 1);

        Title = "Nested Grids demo";
        Content = rootGrid;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        boxView.Color = new Color(redSlider.Value, greenSlider.Value, blueSlider.Value);
    }
}
```

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации сетки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-griddemos)
- [Параметры макета в Xamarin. Forms](layout-options.md)
- [Выбор макета Xamarin. Forms](choose-layout.md)
- [Повышение производительности приложений Xamarin.Forms](~/xamarin-forms/deploy-test/performance.md)
