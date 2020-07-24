---
title: Xamarin.Formsабсолутелайаут
description: В этой статье объясняется, как использовать Xamarin.Forms класс абсолутелайаут для создания интерфейсов пользователя с неполными пикселями. Этот класс размещает и изменяет размеры дочерних элементов пропорционально его размеру и положению или абсолютным значениям.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 923f7643bd1e137192bfb80dbbc7c5d2c25b5471
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938428"
---
# <a name="xamarinforms-absolutelayout"></a>Xamarin.Formsабсолутелайаут

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)позиции и размеры дочерних элементов пропорционально его размеру и положению или абсолютным значениям. Дочерние представления можно располагать и изменять их размер с помощью пропорциональных или статических значений, а пропорциональные и статические значения могут быть смешанными.

[![Xamarin.FormsМетка](absolute-layout-images/layouts-sml.png)](absolute-layout-images/layouts.png#lightbox "[! Операцион. Макеты NO-LOC (Xamarin. Forms)]")

В этой статье рассматриваются следующие темы:

- **[Назначение](#purpose)** &ndash; распространенные способы применения `AbsoluteLayout` .
- **[Использование](#usage)** &ndash; использование `AbsoluteLayout` для достижения желаемого дизайна.
  - **[Пропорциональные макеты](#proportional-layouts)** &ndash; Узнайте, как пропорциональные значения работают в `AbsoluteLayout` .
  - **[Указание значений](#specifying-values)** &ndash; Узнайте, как указаны пропорциональные и абсолютные значения.
  - **[Пропорциональные значения](#proportional-values)** &ndash; Узнайте, как работают пропорциональные значения.
    - **[Абсолютные значения](#absolute-values)** &ndash; Узнайте, как работают абсолютные значения.

## <a name="purpose"></a>Назначение

Из-за модели позиционирования `AbsoluteLayout` макета макет позволяет относительно легко позиционировать элементы, чтобы они были сброшены с любой стороны макета или центрированы. С пропорциональными размерами и позициями элементы в `AbsoluteLayout` могут автоматически масштабироваться до любого размера представления. Для элементов, где только позиция, но не размер, должны масштабироваться, абсолютные и пропорциональные значения могут быть смешанными.

`AbsoluteLayout`может использоваться в любом месте внутри представления и особенно полезно при выровняйте элементов по краям.

## <a name="usage"></a>Использование

### <a name="proportional-layouts"></a>Пропорциональные макеты

`AbsoluteLayout`имеет уникальную модель привязки, в которой привязка элемента располагается относительно его элемента, так как элемент располагается относительно макета при использовании пропорционального позиционирования. При использовании абсолютного позиционирования привязка находится в пределах представления (0, 0). Это имеет два важных последствия.

- Элементы не могут быть размещены на экране с использованием пропорциональных значений.
- Элементы могут быть надежно расположены вдоль любой стороны макета или в центре независимо от размера макета или устройства.

`AbsoluteLayout`, например `RelativeLayout` , может располагать элементы так, чтобы они были перекрываться.

Обратите внимание, что на следующем снимке экрана привязка бокса является белой точкой. Обратите внимание на связь между точкой привязки и полем при перемещении по макету:

![Закрепить при запуске ](absolute-layout-images/anchor-start.png)
 ![ привязки в центре ](absolute-layout-images/anchor-center.png)
 ![ Привязка в конце](absolute-layout-images/anchor-end.png)

### <a name="specifying-values"></a>Указание значений

Представления в коллекции располагаются `AbsoluteLayout` с помощью четырех значений:

- **X** Координата по &ndash; оси x (по горизонтали) для привязки представления
- **Y** &ndash; (по вертикали) для привязки представления
- **Ширина** &ndash; Ширина представления
- **Высота** &ndash; Высота представления

Каждое из этих значений может быть задано как [пропорциональное](#proportional-values) значение или [абсолютное](#absolute-values) значение.

Значения задаются в виде сочетания границ и флага. `LayoutBounds`состоит [`Rectangle`](xref:Xamarin.Forms.Rectangle) из четырех значений: `x` , `y` , `width` , `height` .

### <a name="absolutelayoutflags"></a>абсолутелайаутфлагс

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags)Указывает, как будут интерпретироваться значения, и содержит следующие предопределенные параметры:

- **Нет** &ndash; интерпретирует все значения как абсолютные. Это значение по умолчанию, если флаги макета не указаны.
- **Все** &ndash; интерпретирует все значения как пропорциональные.
- **Видспропортионал** &ndash; интерпретирует `Width` значение как пропорциональное и все остальные значения как абсолютные.
- **Хеигхтпропортионал** &ndash; интерпретирует только значение высоты, пропорциональное абсолютно всем остальным значениям.
- **Кспропортионал** &ndash; интерпретирует `X` значение как пропорциональное, принимая все остальные значения как абсолютные.
- **Ипропортионал** &ndash; интерпретирует `Y` значение как пропорциональное, принимая все остальные значения как абсолютные.
- **Поситионпропортионал** &ndash; интерпретирует `X` `Y` значения и как пропорциональные, в то время как значения размера интерпретируется как абсолютные.
- **Сизепропортионал** &ndash; интерпретирует `Width` `Height` значения и как пропорциональные, в то время как значения позиций являются абсолютными.

В XAML границы и флаги задаются как часть определения представлений в макете с помощью `AbsoluteLayout.LayoutBounds` Свойства. Границы задаются в виде разделенного запятыми списка значений,,, `X` `Y` `Width` и `Height` в указанном порядке. Флаги также указываются в объявлении представлений в макете с помощью `AbsoluteLayout.LayoutFlags` Свойства. Обратите внимание, что флаги можно комбинировать в XAML с помощью списка с разделителями-запятыми. Рассмотрим следующий пример.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![Примеры Абсолутелайаут](absolute-layout-images/exploration.png)

Следует отметить следующее.

- Метка в центре располагается с использованием абсолютных значений размера и расположения. По этой причине он отображается по центру iPhone 4S и ниже, но не располагается на более крупных устройствах.
- Текст в нижней части макета размещается с использованием пропорциональных значений размера и позиции. Он всегда будет отображаться в нижней центральной области макета, но его размер будет увеличиваться с большим размером макета.
- Три цветные панели расположены `BoxView` в верхней, левой и правой границах экрана с использованием пропорциональных позиций и абсолютного размера.

Следующий пример достигает того же макета в C#:

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```

### <a name="proportional-values"></a>Пропорциональные значения

Пропорциональные значения определяют связь между макетом и представлением. Эта связь определяет положение или масштаб дочернего представления как часть соответствующего значения родительского макета. Эти значения выражаются в виде `double` s со значениями от 0 до 1.

Пропорциональные значения используются для размещения и изменения размеров представлений в макете. Таким образом, если ширина представления задана как пропорция, то результирующее значение ширины будет равно пропорции, умноженной на `AbsoluteLayout` ширину. Например, если для `AbsoluteLayout` ширины `500` и представления задана пропорциональная ширина в 0,5, то отображаемая ширина представления будет 250 (500 x. 5).

Чтобы использовать пропорциональные значения, задайте `LayoutBounds` для параметра использование (x, y) пропорций и пропорциональных размеров, затем установите `LayoutFlags` для значение `All` .

В XAML:

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

В C#:

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

### <a name="absolute-values"></a>Абсолютные значения

Абсолютные значения явно определяют, где должны располагаться представления в макете. В отличие от пропорциональных значений, абсолютные значения могут располагать и изменять размер представления, которое не умещается в границах макета.

Использование абсолютных значений для позиционирования может быть опасно, если размер макета неизвестен. При использовании абсолютного положения элемент в центре экрана с одним размером может быть смещен на любой другой размер. Важно протестировать приложение на различных размерах экрана поддерживаемых устройств.

Чтобы использовать абсолютные значения макета, установите `LayoutBounds` с помощью координат (x, y) и явных размеров, а затем задайте для параметра значение `LayoutFlags` `None` .

В XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

В C#:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Изучение сложного макета

Каждый из макетов имеет свои сильные стороны и недостатки для создания определенных макетов. В этой серии макетных статей был создан пример приложения с тем же макетом страницы, который был реализован с использованием трех различных макетов.

Рассмотрим следующий код XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

Приведенный выше код приводит к следующему макету:

![Сложные Абсолутелайаут](absolute-layout-images/abs.png)

Обратите внимание, что `AbsoluteLayout` s вложены, поскольку в некоторых случаях вложение макетов может быть проще, чем представление всех элементов в одном макете.

## <a name="related-links"></a>Связанные ссылки

- [Создание мобильных приложений с помощью Xamarin.Forms , глава 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [Макет (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Пример Бусинесстумбле (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
