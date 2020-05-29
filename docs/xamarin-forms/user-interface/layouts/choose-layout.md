---
title: Выбор Xamarin.Forms макета
description: Xamarin.Formsклассы макетов позволяют упорядочивать и группировать элементы управления ИП в приложении.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 16a48423d05ce1cede75c0020bf18f4f398f5adc
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138506"
---
# <a name="choose-a-xamarinforms-layout"></a>Выбор Xamarin.Forms макета

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

Xamarin.Formsклассы макетов позволяют упорядочивать и группировать элементы управления ИП в приложении. Выбор класса макета требует знания того, как макет позиционирует свои дочерние элементы, и как макет изменяет свои дочерние элементы. Кроме того, может потребоваться вложение макетов для создания желаемого макета.

На следующем рисунке показаны типичные макеты, которые можно достичь с помощью основных Xamarin.Forms классов макета:

[![Основные классы макета вXamarin.Forms](images/layouts.png "[! Операцион. Классы макета NO-LOC (Xamarin. Forms)]")](images/layouts-large.png#lightbox "[! Операцион. Классы макета NO-LOC (Xamarin. Forms)]")

## <a name="stacklayout"></a>StackLayout

Объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) упорядочивает элементы в одномерном стеке: по горизонтали или по вертикали. [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)Свойство задает направление элементов, а ориентация по умолчанию — [`Vertical`](xref:Xamarin.Forms.StackOrientation) . `StackLayout`обычно используется для упорядочения подраздела пользовательского интерфейса на странице.

В следующем коде XAML показано, как создать вертикальный, [`StackLayout`](xref:Xamarin.Forms.StackLayout) содержащий три [`Label`](xref:Xamarin.Forms.Label) объекта:

```xaml
<StackLayout Margin="20,35,20,25">
    <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
    <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
    <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
</StackLayout>
```

В [`StackLayout`](xref:Xamarin.Forms.StackLayout) , если размер элемента не задан явно, он расширяется, чтобы заполнить доступную ширину или высоту, если [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) свойство имеет значение [`Horizontal`](xref:Xamarin.Forms.StackOrientation) .

Объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) часто используется в качестве родительского макета, который содержит другие дочерние макеты. Однако `StackLayout` не следует использовать для воспроизведения [`Grid`](xref:Xamarin.Forms.Grid) макета с помощью сочетания `StackLayout` объектов. В следующем коде показан пример этого неправильного практического занятия.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Name:" />
            <Entry Placeholder="Enter your name" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Age:" />
            <Entry Placeholder="Enter your age" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Occupation:" />
            <Entry Placeholder="Enter your occupation" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Address:" />
            <Entry Placeholder="Enter your address" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Такой метод неэффективен, так как выполняются ненужные вычисления макета. Вместо этого желаемый макет можно лучше добиться с помощью [`Grid`](xref:Xamarin.Forms.Grid) .

> [!TIP]
> При использовании метода [`StackLayout`](xref:Xamarin.Forms.StackLayout) Убедитесь, что только один дочерний элемент имеет значение [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) . В этом случае указанный дочерний элемент будет занимать максимальное пространство, предоставляемое ему макетом `StackLayout`. Выполнять эти вычисления несколько раз слишком затратно.

Дополнительные сведения см. в разделе [ Xamarin.Forms StackLayout](stacklayout.md).

## <a name="grid"></a>Макет Grid

Объект [`Grid`](xref:Xamarin.Forms.Grid) используется для отображения элементов в строках и столбцах, которые могут иметь пропорциональные или абсолютные размеры. Строки и столбцы сетки указываются с помощью [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) свойств и.

Чтобы разместить элементы в конкретных [`Grid`](xref:Xamarin.Forms.Grid) ячейках, используйте [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) вложенные свойства и. Чтобы сделать элементы охватывающими несколько строк и столбцов, используйте [`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) [`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) вложенные свойства и.

> [!NOTE]
> [`Grid`](xref:Xamarin.Forms.Grid)Макет не следует путать с таблицами и не предназначен для представления табличных данных. В отличие от таблиц HTML, `Grid` предназначено для размещения содержимого. Для отображения табличных данных рассмотрите возможность использования [ListView](~/xamarin-forms/user-interface/listview/index.md), [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)или [таблевиев](~/xamarin-forms/user-interface/tableview.md).

В следующем коде XAML показано, как создать [`Grid`](xref:Xamarin.Forms.Grid) с двумя строками и двумя столбцами:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>    
    <Label Text="Column 0, Row 0"
           WidthRequest="200" />
    <Label Grid.Column="1"
           Text="Column 1, Row 0" />
    <Label Grid.Row="1"
           Text="Column 0, Row 1" />
    <Label Grid.Column="1"
           Grid.Row="1"
           Text="Column 1, Row 1" />
</Grid>
```

В этом примере размер работает следующим образом:

- Каждая строка имеет явную высоту в 50 единиц, независимых от устройства.
- Ширина первого столбца устанавливается равным [`Auto`](xref:Xamarin.Forms.GridLength.Auto) , и поэтому она имеет ширину, необходимую для дочерних элементов. В данном случае это 200 единиц, не зависящих от устройства, для размещения ширины первой [`Label`](xref:Xamarin.Forms.Label) .

Пространство может распространяться в пределах столбца или строки с помощью автоматического изменения размера, что позволяет изменять размер столбцов и строк в соответствии с содержимым. Это достигается путем установки высоты [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) или ширины типа в [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) [`Auto`](xref:Xamarin.Forms.GridLength.Auto) . Пропорциональное изменение размера также можно использовать для распределения доступного пространства между строками и столбцами сетки по взвешенным пропорциям. Это достигается путем установки высоты `RowDefinition` или ширины объекта в `ColumnDefinition` значение, которое использует `*` оператор.

> [!CAUTION]
> Старайтесь сделать так, чтобы максимально допустимое число строк и столбцов было равно [`Auto`](xref:Xamarin.Forms.GridLength.Auto) размеру. Из-за каждой строки или столбца с автоматическим размером обработчик макета будет выполнять дополнительные вычисления макета. Если возможно, используйте строки и столбцы фиксированного размера. Кроме того, можно задать для строк и столбцов занимать пропорциональный объем пространства со [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) значением перечисления.

Дополнительные сведения см. в разделе [ Xamarin.Forms Grid](grid.md).

## <a name="flexlayout"></a>FlexLayout

[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)Аналогично тому [`StackLayout`](xref:Xamarin.Forms.StackLayout) , что отображает дочерние элементы в стеке по горизонтали или по вертикали. Однако `FlexLayout` может также заключить свои дочерние элементы, если слишком много подходит для одной строки или столбца, а также обеспечивает более детализированный контроль над размером, ориентацией и выравниванием дочерних элементов.

В следующем коде XAML показано, как создать объект [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) , отображающий его представления в одном столбце:

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">
    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

В этом примере макет работает следующим образом:

- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)Свойство имеет значение `Column` , что приводит к тому, что дочерние элементы объекта `FlexLayout` упорядочиваются в один столбец элементов.
- [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)Свойство имеет значение `Center` , что приводит к горизонтальному центрированию каждого элемента.
- [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)Свойство имеет значение `SpaceEvenly` , которое выделяет все оставшиеся вертикальные промежутки между всеми элементами, а также над первым элементом и под последним элементом.

Дополнительные сведения см. в разделе [ Xamarin.Forms флекслайаут](flex-layout.md).

## <a name="relativelayout"></a>RelativeLayout

Объект [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) используется для размещения и изменения размеров элементов относительно свойств макета или элементов того же уровня. По умолчанию элемент размещается в левом верхнем углу макета. `RelativeLayout`Можно использовать для создания пользовательских интерфейсов, которые масштабируются пропорционально по размеру устройств.

В [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) , позиции и размеры указываются как ограничения. Ограничения имеют [`Factor`](xref:Xamarin.Forms.ConstraintExpression.Factor) [`Constant`](xref:Xamarin.Forms.ConstraintExpression.Constant) Свойства и, которые можно использовать для определения позиций и размеров в виде нескольких (или дробей) свойств других объектов, а также константы. Кроме того, константы могут быть отрицательными.

> [!NOTE]
> [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)Поддерживает элементы позиционирования за пределами собственных границ.

В следующем коде XAML показано, как упорядочивать элементы в [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) :

```xaml
<RelativeLayout>
    <BoxView Color="Blue"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView Color="Red"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.85}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView x:Name="pole"
             Color="Gray"
             WidthRequest="15"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.45}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.25}" />
    <BoxView Color="Green"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
             RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.2, Constant=20}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

В этом примере макет работает следующим образом:

- Синим [`BoxView`](xref:Xamarin.Forms.BoxView) образом присваивается явный размер 50x50 аппаратно-независимых единиц. Он размещается в левом верхнем углу макета, который является положением по умолчанию.
- Красный получает [`BoxView`](xref:Xamarin.Forms.BoxView) явный размер 50x50 аппаратно-независимых единиц. Он размещается в правом верхнем углу макета.
- Серый цвет [`BoxView`](xref:Xamarin.Forms.BoxView) имеет явную ширину 15 независимых от устройства единиц, а высота равна 75% от высоты родительского элемента.
- Зеленый цвет [`BoxView`](xref:Xamarin.Forms.BoxView) не имеет явно заданного размера. Его расположение задается относительно `BoxView` именованного `pole` .

> [!WARNING]
> По возможности избегайте использования `RelativeLayout`. В противном случае ЦП будет испытывать значительно большую нагрузку.

Дополнительные сведения см. в разделе [ Xamarin.Forms RelativeLayout](relative-layout.md).

## <a name="absolutelayout"></a>AbsoluteLayout

Объект [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) используется для размещения и изменения размеров элементов с помощью явных значений или значений относительно размера макета. Значение положения указывается в левом верхнем углу дочернего угла относительно левого верхнего угла `AbsoluteLayout` .

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)Следует рассматривать как макет специального назначения, который будет использоваться только в том случае, если можно наложить размер дочерних элементов или если размер элемента не влияет на расположение других дочерних элементов. Стандартным применением этого макета является создание наложения, которая охватывает страницу с другими элементами управления, что может защитить пользователя от взаимодействия с обычными элементами управления на странице.

> [!IMPORTANT]
> `HorizontalOptions`Свойства и `VerticalOptions` не влияют на дочерние элементы `AbsoluteLayout` .

В [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) присоединяемое свойство используется для указания горизонтальной и вертикальной позиций, ширины и высоты элемента. Кроме того, [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) присоединенное свойство указывает, как будут интерпретироваться границы макета.

В следующем коде XAML показано, как упорядочивать элементы в [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) :

```xaml
<AbsoluteLayout Margin="40">
    <BoxView Color="Red"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="30" />
    <BoxView Color="Green"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="60" />
    <BoxView Color="Blue"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" />
</AbsoluteLayout>
```

В этом примере макет работает следующим образом:

- Каждому [`BoxView`](xref:Xamarin.Forms.BoxView) параметру присваивается явный размер 100x100, и он отображается в той же положении горизонтально по центру.
- Красный угол [`BoxView`](xref:Xamarin.Forms.BoxView) поворачивается на 30 градусов, а зеленый `BoxView` — на 60 градусов.
- Для каждого [`BoxView`](xref:Xamarin.Forms.BoxView) [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) Свойства присоединенное свойство имеет значение `PositionProportional` , указывающее, что расположение пропорционально остальному пространству после ширины и высоты.

> [!CAUTION]
> Старайтесь не использовать [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) свойство, когда это возможно, так как оно приведет к тому, что обработчик макетов будет выполнять дополнительные вычисления макета.

Дополнительные сведения см. в разделе [ Xamarin.Forms абсолутелайаут](absolute-layout.md).

## <a name="input-transparency"></a>Прозрачность ввода

Каждый визуальный элемент имеет [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent) свойство, которое используется для определения того, получает ли элемент входные данные. Значение по умолчанию — `false` , гарантируя, что элемент получает входные данные.

Если это свойство задано для класса макета, его значение передается в дочерние элементы. Поэтому присвоение [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent) свойству значения `true` класса макета приведет к тому, что все элементы в макете не будут получать входные данные.

## <a name="layout-performance"></a>Производительность макета

Чтобы получить максимально возможную производительность макета, следуйте рекомендациям по [оптимизации производительности макета](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance).

Кроме того, производительность отрисовки страницы также можно улучшить с помощью сжатия макета, который удаляет указанные макеты из визуального дерева. Дополнительные сведения см. в разделе [Сжатие макета](layout-compression.md).

## <a name="related-links"></a>Связанные ссылки

- [Макет (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Xamarin.FormsМакеты (видео)](https://youtu.be/4HlLjTZQzjM)
- [Xamarin.FormsStackLayout](stacklayout.md)
- [Xamarin.FormsСетки](grid.md)
- [Xamarin.Formsфлекслайаут](flex-layout.md)
- [Xamarin.Formsабсолутелайаут](absolute-layout.md)
- [Xamarin.FormsRelativeLayout](relative-layout.md)
- [Оптимизация производительности макета](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)
- [Сжатие макета](layout-compression.md)
