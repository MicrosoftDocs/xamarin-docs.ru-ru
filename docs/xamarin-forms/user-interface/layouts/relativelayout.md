---
title: Xamarin.Forms RelativeLayout
description: Xamarin.FormsRelativeLayout используется для позиционирования и размера потомков относительно свойств макета или элементов того же уровня.
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/13/2020
ms.custom: contperfq1
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d350ceee778c9f9ba9f25555a89a925558c6d38b
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556845"
---
# <a name="no-locxamarinforms-relativelayout"></a>Xamarin.Forms RelativeLayout

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-relativelayoutdemos)

[![::: No-Loc (Xamarin. Forms)::: RelativeLayout](relativelayout-images/layouts.png)](relativelayout-images/layouts-large.png#lightbox)

Объект [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) используется для размещения и изменения размеров дочерних элементов относительно свойств макета или элементов того же уровня. Это позволяет создавать пользовательские интерфейсы, которые масштабируются пропорционально между размерами устройств. Кроме того, в отличие от других классов макетов, может `RelativeLayout` располагать дочерние элементы, чтобы перекрывать.

[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)Класс определяет следующие свойства:

- [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty)Тип [`Constraint`](xref:Xamarin.Forms.Constraint) , который является присоединенным свойством, представляющим ограничение по оси X дочернего элемента.
- [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty)Тип [`Constraint`](xref:Xamarin.Forms.Constraint) , который является присоединенным свойством, представляющим ограничение по оси Y дочернего элемента.
- [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty)Тип [`Constraint`](xref:Xamarin.Forms.Constraint) , который является присоединенным свойством, представляющим ограничение ширины дочернего элемента.
- [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty)Тип [`Constraint`](xref:Xamarin.Forms.Constraint) , который является присоединенным свойством, представляющим ограничение высоты дочернего элемента.
- [`BoundsConstraint`](xref:Xamarin.Forms.RelativeLayout.BoundsConstraintProperty)Тип — [`BoundsConstraint`](xref:Xamarin.Forms.BoundsConstraint) это присоединенное свойство, представляющее ограничение на расположение и размер дочернего элемента. Это свойство нельзя легко использовать из XAML.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами для привязок данных и стилей. Дополнительные сведения о вложенных свойствах см. в разделе [ Xamarin.Forms вложенные свойства](~/xamarin-forms/xaml/attached-properties.md).

> [!NOTE]
> Ширину и высоту дочернего элемента в [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) можно также указать через свойства дочернего элемента `WidthRequest` и `HeightRequest` , а не [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) присоединенные свойства и.

[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)Класс является производным от `Layout<T>` класса, который определяет `Children` свойство типа `IList<T>` . `Children`Свойство является свойством `ContentProperty` `Layout<T>` класса, поэтому его не нужно явно задавать из XAML.

> [!TIP]
> По возможности избегайте использования [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout). В противном случае ЦП будет испытывать значительно большую нагрузку.

## <a name="constraints"></a>Ограничения

В [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) , положение и размер дочерних элементов задаются в виде ограничений с использованием абсолютных значений или относительных значений. Если ограничения не указаны, дочерний элемент будет расположен в левом верхнем углу макета.

В следующей таблице показано, как указать ограничения в XAML и C#:

|     | XAML | C# |
| --- | ---- | -- |
| **Абсолютные значения** | Абсолютные ограничения задаются путем присвоения [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) присоединяемым свойствам `double` значений. | Абсолютные ограничения задаются [`Constraint.Constant`](xref:Xamarin.Forms.Constraint.Constant*) методом или с помощью `Children.Add` перегрузки, для которой требуется `Func<Rectangle>` аргумент. |
| **Относительные значения** | Относительные ограничения задаются путем установки [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) присоединенных свойств к [`Constraint`](xref:Xamarin.Forms.Constraint) объектам, возвращаемым `ConstraintExpression` расширением разметки. | Относительные ограничения задаются [`Constraint`](xref:Xamarin.Forms.Constraint) объектами, возвращаемыми методами `Constraint` класса. |

Дополнительные сведения об указании ограничений с помощью абсолютных значений см. в разделе [абсолютное позиционирование и изменение размера](#absolute-positioning-and-sizing). Дополнительные сведения об указании ограничений с помощью относительных значений см. в разделе [относительное размещение и изменение размера](#relative-positioning-and-sizing).

В C# дочерние элементы можно добавлять в [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) с помощью трех `Add` перегрузок. Первая перегрузка требует, `Expression<Func<Rectangle>>` чтобы указать расположение и размер дочернего элемента. Вторая перегрузка требует наличия дополнительных `Expression<Func<double>>` объектов для `x` аргументов, `y` , `width` и `height` . Третья перегрузка требует наличия дополнительных `Constraint` объектов для `x` аргументов, `y` , `width` и `height` .

Можно изменить расположение и размер дочернего элемента в [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) с помощью [`SetXConstraint`](xref:Xamarin.Forms.RelativeLayout.SetXConstraint*) методов,, [`SetYConstraint`](xref:Xamarin.Forms.RelativeLayout.SetYConstraint*) [`SetWidthConstraint`](xref:Xamarin.Forms.RelativeLayout.SetWidthConstraint*) и [`SetHeightConstraint`](xref:Xamarin.Forms.RelativeLayout.SetHeightConstraint*) . Первый аргумент для каждого из этих методов является дочерним, а второй — [`Constraint`](xref:Xamarin.Forms.Constraint) объектом. Кроме [`SetBoundsConstraint`](xref:Xamarin.Forms.RelativeLayout.SetBoundsConstraint*) того, метод можно использовать для изменения расположения и размера дочернего элемента. Первым аргументом этого метода является дочерний, а второй — [`BoundsConstraint`](xref:Xamarin.Forms.BoundsConstraint) объект.

## <a name="absolute-positioning-and-sizing"></a>Абсолютное позиционирование и изменение размера

[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)Может располагать и масштабировать дочерние элементы с помощью абсолютных значений, заданных в аппаратно-независимых единицах, которые явно определяют, где дочерние элементы должны размещаться в макете. Это достигается путем добавления дочерних элементов в `Children` коллекцию `RelativeLayout` и установки [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) присоединенных свойств,, и для [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) каждого дочернего элемента в абсолютную положение и (или) размер значения.

> [!WARNING]
> Использование абсолютных значений для позиционирования и изменения размера дочернего элемента может быть проблематичным, так как разные устройства имеют разные размеры и разрешения экрана. Поэтому Координаты центра экрана на одном устройстве могут быть смещены на других устройствах.

В следующем коде XAML показано, [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) чьи потомки расположены с использованием абсолютных значений:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="RelativeLayoutDemos.Views.StylishHeaderDemoPage"
             Title="Stylish header demo">
    <RelativeLayout Margin="20">
        <BoxView Color="Silver"
                 RelativeLayout.XConstraint="0"
                 RelativeLayout.YConstraint="10"
                 RelativeLayout.WidthConstraint="200"
                 RelativeLayout.HeightConstraint="5" />
        <BoxView Color="Silver"
                 RelativeLayout.XConstraint="0"
                 RelativeLayout.YConstraint="20"
                 RelativeLayout.WidthConstraint="200"
                 RelativeLayout.HeightConstraint="5" />
        <BoxView Color="Silver"
                 RelativeLayout.XConstraint="10"
                 RelativeLayout.YConstraint="0"
                 RelativeLayout.WidthConstraint="5"
                 RelativeLayout.HeightConstraint="65" />
        <BoxView Color="Silver"
                 RelativeLayout.XConstraint="20"
                 RelativeLayout.YConstraint="0"
                 RelativeLayout.WidthConstraint="5"
                 RelativeLayout.HeightConstraint="65" />
        <Label Text="Stylish header"
               FontSize="24"
               RelativeLayout.XConstraint="30"
               RelativeLayout.YConstraint="25" />
    </RelativeLayout>
</ContentPage>
```

В этом примере расположение каждого [`BoxView`](xref:Xamarin.Forms.BoxView) объекта определяется с использованием значений, указанных в [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) свойствах и. Размер каждого из них `BoxView` определяется с помощью значений, указанных в [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) свойствах и. Расположение [`Label`](xref:Xamarin.Forms.Label) объекта также определяется с помощью значений, указанных в `XConstraint` `YConstraint` свойствах и. Однако значения размера не задаются для `Label` , поэтому они не ограничены и сами размеры. Во всех случаях абсолютные значения представляют аппаратно-независимые единицы.

На следующих снимках экрана показан получившийся макет:

![Дочерние элементы, размещенные в RelativeLayout с использованием абсолютных значений](relativelayout-images/absolute-values.png)

Эквивалентный код C# показан ниже:

```csharp
public class StylishHeaderDemoPageCS : ContentPage
{
    public StylishHeaderDemoPageCS()
    {
        RelativeLayout relativeLayout = new RelativeLayout
        {
            Margin = new Thickness(20)
        };

        relativeLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, () => new Rectangle(0, 10, 200, 5));

        relativeLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, () => new Rectangle(0, 20, 200, 5));

        relativeLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, () => new Rectangle(10, 0, 5, 65));

        relativeLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, () => new Rectangle(20, 0, 5, 65));

        relativeLayout.Children.Add(new Label
        {
            Text = "Stylish Header",
            FontSize = 24
        }, Constraint.Constant(30), Constraint.Constant(25));

        Title = "Stylish header demo";
        Content = relativeLayout;
    }
}
```

В этом примере [`BoxView`](xref:Xamarin.Forms.BoxView) объекты добавляются в, [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) используя `Add` перегрузку, для которой необходимо `Expression<Func<Rectangle>>` указать расположение и размер каждого дочернего элемента. Расположение [`Label`](xref:Xamarin.Forms.Label) определяется с помощью `Add` перегрузки, для которой требуются необязательные [`Constraint`](xref:Xamarin.Forms.Constraint) объекты, в данном случае созданного [`Constraint.Constant`](xref:Xamarin.Forms.Constraint.Constant*) методом.

> [!NOTE]
> Объект [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) , который использует абсолютные значения, может располагать и масштабировать дочерние элементы, чтобы они не походили в границах макета.

## <a name="relative-positioning-and-sizing"></a>Относительное размещение и изменение размера

[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)Может располагать и масштабировать дочерние элементы с помощью значений, относительных относительно свойств макета или родственных элементов. Это достигается путем добавления дочерних элементов в `Children` коллекцию `RelativeLayout` и установки [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) вложенных свойств,, и для [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) каждого дочернего объекта в относительные значения с помощью [`Constraint`](xref:Xamarin.Forms.Constraint) объектов.

Ограничения могут быть константой, относительно родителя или относительного одноуровневого элемента. Тип ограничения представлен [`ConstraintType`](xref:Xamarin.Forms.ConstraintType) перечислением, которое определяет следующие члены:

- `RelativeToParent`, который указывает ограничение, относящееся к родительскому элементу.
- `RelativeToView`, который указывает ограничение, относящееся к представлению (или родственному элементу).
- `Constant`, который указывает на постоянное ограничение.

### <a name="constraint-markup-extension"></a>Расширение разметки Constraint

В XAML [`Constraint`](xref:Xamarin.Forms.Constraint) объект может быть создан [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) расширением разметки. Это расширение разметки обычно используется для связи расположения и размера дочернего элемента в [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) с родительским элементом или с элементом того же уровня.

[`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)Класс определяет следующие свойства:

- [`Constant`](xref:Xamarin.Forms.ConstraintExpression.Constant)Тип `double` , который представляет постоянное значение ограничения.
- [`ElementName`](xref:Xamarin.Forms.ConstraintExpression.ElementName)Тип `string` , представляющий имя исходного элемента, для которого вычисляется ограничение.
- [`Factor`](xref:Xamarin.Forms.ConstraintExpression.Factor)Тип `double` , который представляет коэффициент, на который масштабируется ограниченное измерение относительно исходного элемента. По умолчанию это свойство имеет значение 1.
- [`Property`](xref:Xamarin.Forms.ConstraintExpression.Property)Тип `string` , который представляет имя свойства в элементе Source, используемое в вычислении ограничения.
- [`Type`](xref:Xamarin.Forms.ConstraintExpression.Type)Тип [`ConstraintType`](xref:Xamarin.Forms.ConstraintType) , который представляет тип ограничения.

Дополнительные сведения о расширениях разметки Xamarin.Forms см. в статье [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md).

В следующем коде XAML показан объект, [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) дочерние элементы которого ограничены [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) расширением разметки:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="RelativeLayoutDemos.Views.RelativePositioningAndSizingDemoPage"
             Title="RelativeLayout demo">
    <RelativeLayout>
        <BoxView Color="Red"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=Constant, Constant=0}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=Constant, Constant=0}" />
        <BoxView Color="Green"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Constant=-40}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=Constant, Constant=0}" />
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=Constant, Constant=0}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Constant=-40}" />
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Constant=-40}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Silver"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.33}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0.33}"
                 RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.33}"
                 RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0.33}" />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Black"
                 RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=oneThird, Property=X}"
                 RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=oneThird, Property=Y}"
                 RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToView, ElementName=oneThird, Property=Width, Factor=0.33}"
                 RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToView, ElementName=oneThird, Property=Height, Factor=0.33}" />
    </RelativeLayout>
</ContentPage>
```

В этом примере расположение каждого [`BoxView`](xref:Xamarin.Forms.BoxView) объекта определяется с помощью установки [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) присоединенных свойств и. Первый `BoxView` имеет `XConstraint` `YConstraint` вложенные свойства и присвоены к константам, которые являются абсолютными значениями. Все остальные `BoxView` объекты имеют установленную позиции, используя по меньшей мере одно относительное значение. Например, желтый `BoxView` объект устанавливает `XConstraint` присоединенное свойство в ширину его родителя (значение [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) ) минус 40. Аналогичным образом для `BoxView` `YConstraint` присоединенного свойства устанавливается высота родителя минус 40. Это гарантирует, что желтый цвет `BoxView` появится в правом нижнем углу экрана.

> [!NOTE]
> [`BoxView`](xref:Xamarin.Forms.BoxView) объекты, для которых не задан размер, автоматически изменяются в соответствии с размером 40x40 Xamarin.Forms .

Серебристое [`BoxView`](xref:Xamarin.Forms.BoxView) название располагается `oneThird` централизованно относительно его родителя. Он также имеет размер относительно родительского элемента, равный третьему из его ширины и высоты. Это достигается путем задания [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) свойству и вложенным свойствам ширины родителя ( [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) ), умноженной на 0,33. Аналогично, [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) вложенным свойствам и присваивается высота родителя, умноженная на 0,33.

Черный [`BoxView`](xref:Xamarin.Forms.BoxView) расположен и имеет размер относительно `oneThird` `BoxView` . Это достигается путем установки [`XConstraint`](xref:Xamarin.Forms.RelativeLayout.XConstraintProperty) свойств и [`YConstraint`](xref:Xamarin.Forms.RelativeLayout.YConstraintProperty) присоединенных к `X` `Y` значениям и соответственно для родственного элемента. Аналогично, его размер устанавливается равным одной трети от ширины и высоты элемента того же уровня. Это достигается путем присвоения [`WidthConstraint`](xref:Xamarin.Forms.RelativeLayout.WidthConstraintProperty) [`HeightConstraint`](xref:Xamarin.Forms.RelativeLayout.HeightConstraintProperty) свойству и вложенным свойствам `Width` `Height` значений и родственного элемента, которые затем умножаются на 0,33.

На следующем снимке экрана показан получившийся макет:

![Дочерние элементы, помещенные в RelativeLayout с использованием относительных значений](relativelayout-images/relative-values.png)

### <a name="constraint-objects"></a>Объекты ограничений

[`Constraint`](xref:Xamarin.Forms.Constraint)Класс определяет следующие открытые статические методы, которые возвращают `Constraint` объекты:

- [`Constant`](xref:Xamarin.Forms.Constraint.Constant*), который ограничивает дочерний объект до размера, указанного с помощью `double` .
- [`FromExpression`](xref:Xamarin.Forms.Constraint.FromExpression*), который ограничивает дочерний объект с помощью лямбда-выражения.
- [`RelativeToParent`](xref:Xamarin.Forms.Constraint.RelativeToParent*), который ограничивает дочерний объект относительно его размера родителя.
- [`RelativeToView`](xref:Xamarin.Forms.Constraint.RelativeToView*), который ограничивает дочерний объект относительно размера представления.

Кроме того, [`BoundsConstraint`](xref:Xamarin.Forms.BoundsConstraint) класс определяет единственный метод, [`FromExpression`](xref:Xamarin.Forms.BoundsConstraint.FromExpression*) который возвращает объект `BoundsConstraint` , ограничивающий расположение и размер дочернего элемента с помощью `Expression<Func<Rectangle>>` . Этот метод можно использовать для установки [`BoundsConstraint`](xref:Xamarin.Forms.RelativeLayout.BoundsConstraintProperty) присоединенного свойства.

В следующем коде C# показан объект, [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) дочерние элементы которого ограничены [`Constraint`](xref:Xamarin.Forms.Constraint) объектами:

```csharp
public class RelativePositioningAndSizingDemoPageCS : ContentPage
{
    public RelativePositioningAndSizingDemoPageCS()
    {
        RelativeLayout relativeLayout = new RelativeLayout();

        // Four BoxView's
        relativeLayout.Children.Add(
            new BoxView { Color = Color.Red },
            Constraint.Constant(0),
            Constraint.Constant(0));

        relativeLayout.Children.Add(
            new BoxView { Color = Color.Green },
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Width - 40;
            }), Constraint.Constant(0));

        relativeLayout.Children.Add(
            new BoxView { Color = Color.Blue },
            Constraint.Constant(0),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Height - 40;
            }));

        relativeLayout.Children.Add(
            new BoxView { Color = Color.Yellow },
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Width - 40;
            }),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Height - 40;
            }));

        // Centered and 1/3 width and height of parent
        BoxView silverBoxView = new BoxView { Color = Color.Silver };
        relativeLayout.Children.Add(
            silverBoxView,
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Width * 0.33;
            }),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Height * 0.33;
            }),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Width * 0.33;
            }),
            Constraint.RelativeToParent((parent) =>
            {
                return parent.Height * 0.33;
            }));

        // 1/3 width and height of previous
        relativeLayout.Children.Add(
            new BoxView { Color = Color.Black },
            Constraint.RelativeToView(silverBoxView, (parent, sibling) =>
            {
                return sibling.X;
            }),
            Constraint.RelativeToView(silverBoxView, (parent, sibling) =>
            {
                return sibling.Y;
            }),
            Constraint.RelativeToView(silverBoxView, (parent, sibling) =>
            {
                return sibling.Width * 0.33;
            }),
            Constraint.RelativeToView(silverBoxView, (parent, sibling) =>
            {
                return sibling.Height * 0.33;
            }));

        Title = "RelativeLayout demo";
        Content = relativeLayout;
    }
}
```

В этом примере дочерние элементы добавляются в, [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) используя `Add` перегрузку, для которой требуется необязательный `Constraint` объект для `x` аргументов,, `y` `width` и `height` .

> [!NOTE]
> Объект [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) , использующий относительные значения, может располагать и масштабировать дочерние элементы, чтобы они не походили в границах макета.

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации RelativeLayout (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-relativelayoutdemos)
- [Xamarin.Forms Вложенные свойства](~/xamarin-forms/xaml/attached-properties.md)
- [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Выбор Xamarin.Forms макета](choose-layout.md)
- [Улучшение Xamarin.Forms производительности приложения](~/xamarin-forms/deploy-test/performance.md)