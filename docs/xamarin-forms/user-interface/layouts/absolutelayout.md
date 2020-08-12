---
title: Xamarin.Formsабсолутелайаут
description: Xamarin.FormsАбсолутелайаут используется для размещения и изменения размеров элементов с помощью явных значений или значений, пропорциональных размеру макета.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 696429e04775640d46add77ec6a4bbf6e69f675b
ms.sourcegitcommit: c3329ab25d377907d8804cdd5e26dc84a274f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/12/2020
ms.locfileid: "88134159"
---
# <a name="no-locxamarinforms-absolutelayout"></a>Xamarin.Formsабсолутелайаут

[![Скачать пример](~/media/shared/download.png) Скачайте пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-absolutelayoutdemos)

[![::: No-Loc (Xamarin. Forms)::: Абсолутелайаут](absolutelayout-images/layouts.png)](absolutelayout-images/layouts-large.png#lightbox)

Объект [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) используется для размещения и изменения размеров дочерних элементов с помощью явных значений. Расположение задается в левом верхнем углу дочернего угла относительно левого верхнего угла `AbsoluteLayout` , в единицах, не зависящих от устройства. `AbsoluteLayout` также реализует функции пропорционального размещения и изменения размера. Кроме того, в отличие от других классов макета, может `AbsoluteLayout` располагать дочерние элементы, чтобы они были перекрываться.

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)Следует рассматривать как макет специального назначения, который будет использоваться только в том случае, если можно наложить размер дочерних элементов или если размер элемента не влияет на расположение других дочерних элементов.

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)Класс определяет следующие свойства:

- [`LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty)Тип — [`Rectangle`](xref:Xamarin.Forms.Rectangle) это присоединенное свойство, представляющее расположение и размер дочернего элемента. Значение этого свойства по умолчанию равно (0, 0, AutoSize, AutoSize).
- [`LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)Тип — [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) это присоединенное свойство, которое указывает, обрабатываются ли свойства границ макета, используемые для позиционирования и размера дочернего элемента, пропорционально. Значение по умолчанию этого свойства равно `AbsoluteLayoutFlags.None`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами для привязок данных и стилей. Дополнительные сведения о вложенных свойствах см. в разделе [ Xamarin.Forms вложенные свойства](~/xamarin-forms/xaml/attached-properties.md).

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)Класс является производным от `Layout<T>` класса, который определяет `Children` свойство типа `IList<T>` . `Children`Свойство является свойством `ContentProperty` `Layout<T>` класса, поэтому его не нужно явно задавать из XAML.

> [!TIP]
> Чтобы получить максимально возможную производительность макета, следуйте рекомендациям по [оптимизации производительности макета](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance).

## <a name="position-and-size-children"></a>Дочерние элементы позиционирования и размера

Положение и размер дочерних элементов в определяется [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) путем установки [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) присоединенного свойства каждого дочернего элемента, используя абсолютные значения или пропорциональные значения. Абсолютные и пропорциональные значения могут быть смешанными для детей, когда положение должно масштабироваться, но размер должен оставаться фиксированным или наоборот. Дополнительные сведения о абсолютных значениях см. в разделе [абсолютное позиционирование и изменение размера](#absolute-positioning-and-sizing). Дополнительные сведения о пропорциональных значениях см. в разделе [пропорциональное позиционирование и изменение размера](#proportional-positioning-and-sizing).

[`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty)Присоединенное свойство можно задать с помощью двух форматов, независимо от того, используются ли абсолютные или пропорциональные значения:

- `x, y`. В этом формате `x` `y` значения и указывают на расположение верхнего левого угла дочернего элемента относительно его родителя. Дочерний элемент не ограничен и имеет размеры.
- `x, y, width, height`. В этом формате `x` `y` значения и указывают на расположение верхнего левого угла дочернего элемента относительно его родителя, а `width` `height` значения и обозначают размер дочернего элемента.

Чтобы указать, что размеры дочернего элемента располагаются горизонтально или вертикально или оба, установите `width` значения и (или) `height` для [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) Свойства. Однако чрезмерное применение этого свойства может нанести вред производительности приложения, так как оно приводит к тому, что обработчик макетов выполняет дополнительные вычисления макета.

> [!IMPORTANT]
> [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)Свойства и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) не влияют на дочерние элементы [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) .

## <a name="absolute-positioning-and-sizing"></a>Абсолютное позиционирование и изменение размера

По умолчанию [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) позиции и размеры дочерние элементы, использующие абсолютные значения, заданные в аппаратно-независимых единицах, которые явно определяют, где должны размещаться представления в макете. Это достигается путем добавления дочерних элементов в `Children` коллекцию `AbsoluteLayout` и присвоения [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) свойству присоединенного свойства для каждого дочернего элемента значения абсолютного расположения и (или) размера.

> [!WARNING]
> Использование абсолютных значений для позиционирования и изменения размера дочернего элемента может быть проблематичным, так как разные устройства имеют разные размеры и разрешения экрана. Поэтому Координаты центра экрана на одном устройстве могут быть смещены на других устройствах.

В следующем коде XAML показан объект, [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) дочерние элементы которого располагаются с помощью абсолютных значений:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="AbsoluteLayoutDemos.Views.StylishHeaderDemoPage"
             Title="Stylish header demo">
    <AbsoluteLayout Margin="20">
        <BoxView Color="Silver"
                 AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
        <BoxView Color="Silver"
                 AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
        <BoxView Color="Silver"
                 AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
        <BoxView Color="Silver"
                 AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
        <Label Text="Stylish Header"
               FontSize="24"
               AbsoluteLayout.LayoutBounds="30, 25" />
    </AbsoluteLayout>
</ContentPage>
```

В этом примере положение каждого [`BoxView`](xref:Xamarin.Forms.BoxView) объекта определяется с помощью первых двух абсолютных значений, указанных в [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) присоединенном свойстве. Размер каждого из них `BoxView` определяется с помощью третьего и приведенного значений. Положение [`Label`](xref:Xamarin.Forms.Label) объекта определяется с помощью двух абсолютных значений, указанных в `AbsoluteLayout.LayoutBounds` присоединенном свойстве. Значения размера не указаны для `Label` , поэтому они не ограничены и сами размеры. Во всех случаях абсолютные значения представляют аппаратно-независимые единицы.

На следующем снимке экрана показан получившийся макет:

![Дочерние элементы, размещенные в Абсолутелайаут с использованием абсолютных значений](absolutelayout-images/absolute-values.png)

Эквивалентный код C# показан ниже:

```csharp
public class StylishHeaderDemoPageCS : ContentPage
{
    public StylishHeaderDemoPageCS()
    {
        AbsoluteLayout absoluteLayout = new AbsoluteLayout
        {
            Margin = new Thickness(20)
        };

        absoluteLayout.Children.Add(new BoxView
        {
            Color = Color.Silver,
        }, new Rectangle(0, 10, 200, 5));
        absoluteLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, new Rectangle(0, 20, 200, 5));
        absoluteLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, new Rectangle(10, 0, 5, 65));
        absoluteLayout.Children.Add(new BoxView
        {
            Color = Color.Silver
        }, new Rectangle(20, 0, 5, 65));

        absoluteLayout.Children.Add(new Label
        {
            Text = "Stylish Header",
            FontSize = 24
        }, new Point(30,25));                    

        Title = "Stylish header demo";
        Content = absoluteLayout;
    }
}
```

В этом примере расположение и размер каждой из них [`BoxView`](xref:Xamarin.Forms.BoxView) определяются с помощью [`Rectangle`](xref:Xamarin.Forms.Rectangle) объекта. Расположение определяется [`Label`](xref:Xamarin.Forms.Label) с помощью [`Point`](xref:Xamarin.Forms.Point) объекта.

В C# можно также задать расположение и размер дочернего элемента [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) после добавления в `Children` коллекцию с помощью [`AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds*) метода. Первым аргументом этого метода является дочерний, а второй — [`Rectangle`](xref:Xamarin.Forms.Rectangle) объект.

> [!NOTE]
> Объект [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) , который использует абсолютные значения, может располагать и масштабировать дочерние элементы, чтобы они не походили в границах макета.

## <a name="proportional-positioning-and-sizing"></a>Пропорциональное позиционирование и изменение размера

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)Может располагать и масштабировать дочерние элементы с помощью пропорциональных значений. Это достигается путем добавления дочерних элементов в `Children` коллекцию `AbsoluteLayout` , а также путем установки [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) присоединенного свойства для каждого дочернего элемента в пропорциональное расположение и (или) размера в диапазоне 0-1. Значения расположения и размера делаются пропорциональными путем установки [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) присоединенного свойства для каждого дочернего элемента.

[`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)Присоединенное свойство типа [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) позволяет установить флаг, указывающий, что макет, ограничивающий положение и значения размера для дочернего элемента, пропорционален размеру `AbsoluteLayout` . При разметке дочернего элемента размеры `AbsoluteLayout` значения расположения и размера должны масштабироваться на любой размер устройства.

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags)Перечисление определяет следующие члены:

- `None`Указывает, что значения будут интерпретироваться как абсолютные. Это значение [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) присоединенного свойства по умолчанию.
- `XProportional`Указывает, что `x` значение будет интерпретироваться как пропорциональное, при этом все остальные значения будут рассматриваться как абсолютные.
- `YProportional`Указывает, что `y` значение будет интерпретироваться как пропорциональное, при этом все остальные значения будут рассматриваться как абсолютные.
- `WidthProportional`Указывает, что `width` значение будет интерпретироваться как пропорциональное, при этом все остальные значения будут рассматриваться как абсолютные.
- `HeightProportional`Указывает, что `height` значение будет интерпретироваться как пропорциональное, при этом все остальные значения будут рассматриваться как абсолютные.
- `PositionProportional`Указывает, что `x` значения и `y` будут интерпретироваться как пропорциональные, тогда как значения размера будут интерпретироваться как абсолютные.
- `SizeProportional`Указывает, что `width` значения и `height` будут интерпретироваться как пропорциональные, тогда как значения позиций считаются абсолютными.
- `All`Указывает, что все значения будут интерпретироваться как пропорциональные.

> [!TIP]
> [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags)Перечисление — это `Flags` перечисление, которое означает, что члены перечисления могут быть объединены. Это выполняется в XAML с помощью списка с разделителями-запятыми, а в C# — с помощью побитового оператора или.

Например, если вы используете `SizeProportional` флаг и устанавливаете ширину дочернего элемента в 0,25, а высота равна 0,1, то дочерний элемент будет состоять из одной четверти ширины [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) и одной десятой высоты. Этот `PositionProportional` флаг аналогичен. Положение (0,0) помещает дочерний элемент в левый верхний угол, а положение (1, 1) помещает дочерний элемент в правый нижний угол, а положение (0,5, 0,5) выравнивает его дочернему элементу в `AbsoluteLayout` .

В следующем коде XAML показан объект, [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) дочерние элементы которого располагаются с использованием пропорциональных значений:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="AbsoluteLayoutDemos.Views.ProportionalDemoPage"
             Title="Proportional demo">
    <AbsoluteLayout>
        <BoxView Color="Blue"
                 AbsoluteLayout.LayoutBounds="0.5,0,100,25"
                 AbsoluteLayout.LayoutFlags="PositionProportional" />
        <BoxView Color="Green"
                 AbsoluteLayout.LayoutBounds="0,0.5,25,100"
                 AbsoluteLayout.LayoutFlags="PositionProportional" />
        <BoxView Color="Red"
                 AbsoluteLayout.LayoutBounds="1,0.5,25,100"
                 AbsoluteLayout.LayoutFlags="PositionProportional" />
        <BoxView Color="Black"
                 AbsoluteLayout.LayoutBounds="0.5,1,100,25"
                 AbsoluteLayout.LayoutFlags="PositionProportional" />
        <Label Text="Centered text"
               AbsoluteLayout.LayoutBounds="0.5,0.5,110,25"
               AbsoluteLayout.LayoutFlags="PositionProportional" />
    </AbsoluteLayout>
</ContentPage>
```

В этом примере каждый дочерний элемент размещается с использованием пропорциональных значений, но имеет размер, используя абсолютные значения. Это достигается путем задания [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) для присоединенного свойства каждого дочернего элемента значения `PositionProportional` . Первые два значения, указанные в [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) присоединенном свойстве для каждого дочернего элемента, определяют расположение с использованием пропорциональных значений. Размер каждого дочернего элемента определяется третьим и более абсолютными значениями с использованием аппаратно-независимых единиц.

На следующем снимке экрана показан получившийся макет:

![Дочерние элементы, помещенные в Абсолутелайаут с использованием пропорциональных значений поситино](absolutelayout-images/proportional-position.png)

Эквивалентный код C# показан ниже:

```csharp
public class ProportionalDemoPageCS : ContentPage
{
    public ProportionalDemoPageCS()
    {
        BoxView blue = new BoxView { Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds(blue, new Rectangle(0.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags(blue, AbsoluteLayoutFlags.PositionProportional);

        BoxView green = new BoxView { Color = Color.Green };
        AbsoluteLayout.SetLayoutBounds(green, new Rectangle(0, 0.5, 25, 100));
        AbsoluteLayout.SetLayoutFlags(green, AbsoluteLayoutFlags.PositionProportional);

        BoxView red = new BoxView { Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds(red, new Rectangle(1, 0.5, 25, 100));
        AbsoluteLayout.SetLayoutFlags(red, AbsoluteLayoutFlags.PositionProportional);

        BoxView black = new BoxView { Color = Color.Black };
        AbsoluteLayout.SetLayoutBounds(black, new Rectangle(0.5, 1, 100, 25));
        AbsoluteLayout.SetLayoutFlags(black, AbsoluteLayoutFlags.PositionProportional);

        Label label = new Label { Text = "Centered text" };
        AbsoluteLayout.SetLayoutBounds(label, new Rectangle(0.5, 0.5, 110, 25));
        AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.PositionProportional);

        Title = "Proportional demo";
        Content = new AbsoluteLayout
        {
            Children = { blue, green, red, black, label }
        };
    }
}
```

В этом примере расположение и размер каждого дочернего элемента задаются с помощью [`AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds*) метода. Первым аргументом метода является дочерний, а второй — [`Rectangle`](xref:Xamarin.Forms.Rectangle) объект. Положение каждого дочернего элемента устанавливается с пропорциональными значениями, а размер каждого дочернего элемента задается абсолютными значениями с использованием устройств, не зависящих от устройства.

> [!NOTE]
> Объект [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) , использующий пропорциональные значения, может располагать и масштабировать дочерние элементы, чтобы они не поместились в границах макета, используя значения за пределами диапазона 0-1.

## <a name="related-links"></a>Связанные ссылки

- [Демонстрации Абсолутелайаут (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-absolutelayoutdemos)
- [Xamarin.FormsВложенные свойства](~/xamarin-forms/xaml/attached-properties.md)
- [Выбор Xamarin.Forms макета](choose-layout.md)
- [Улучшение Xamarin.Forms производительности приложения](~/xamarin-forms/deploy-test/performance.md)
