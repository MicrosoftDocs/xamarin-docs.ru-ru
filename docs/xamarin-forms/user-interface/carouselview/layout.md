---
title: Xamarin.FormsМакет Карауселвиев
description: По умолчанию Карауселвиев отображает элементы по горизонтали. Однако также возможна вертикальная ориентация.
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b45b1375ab7676e96976951e10b903d25c88bc14
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918522"
---
# <a name="no-locxamarinforms-carouselview-layout"></a>Xamarin.FormsМакет Карауселвиев

![Предварительный выпуск API](~/media/shared/preview.png)

[![Скачать пример](~/media/shared/download.png) Скачайте пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)определяет следующие свойства, управляющие макетом:

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)Тип `LinearItemsLayout` — указывает макет для использования.
- `PeekAreaInsets`, типа [`Thickness`](xref:Xamarin.Forms.Thickness) , указывает, сколько элементов можно сделать смежными, частично видимыми для.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных.

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) элементы отображаются на горизонтальной ориентации. На экране будет отображаться один элемент с жестами прокрутки, приводящими к пересылке и обратной навигации по коллекции элементов. Однако также возможна вертикальная ориентация. Это обусловлено тем [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) , что свойство имеет тип `LinearItemsLayout` , который наследуется от [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) класса. Класс `ItemsLayout` определяет следующие свойства:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)Тип [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) задает направление, в котором [`CarouselView`](xref:Xamarin.Forms.CarouselView) добавляются элементы расширения по мере добавления.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment), типа [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) , задает способ выравнивания точек привязки по элементам.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)Тип [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) — задает поведение точек привязки при прокрутке.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что свойства могут быть целевыми объектами привязок данных. Дополнительные сведения о точках привязки см. в разделе [точки привязки](scrolling.md#snap-points) в [ Xamarin.Forms CollectionView прокрутки](scrolling.md) .

[`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)Перечисление определяет следующие члены:

- `Vertical`Указывает, что [`CarouselView`](xref:Xamarin.Forms.CarouselView) будет расширяться по вертикали по мере добавления элементов.
- `Horizontal`Указывает, что [`CarouselView`](xref:Xamarin.Forms.CarouselView) будет разворачиваться горизонтально по мере добавления элементов.

`LinearItemsLayout`Класс наследует от [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) класса и определяет `ItemSpacing` свойство типа `double` , представляющее пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0. `LinearItemsLayout`Класс также определяет статические `Vertical` `Horizontal` члены и. Эти члены можно использовать для создания вертикальных или горизонтальных списков соответственно. Кроме того, `LinearItemsLayout` можно создать объект, указав в [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) качестве аргумента член перечисления.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)использует собственные обработчики макетов для выполнения макета.

## <a name="horizontal-layout"></a>Горизонтальное расположение

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) отображает элементы по горизонтали. Поэтому нет необходимости задавать [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) свойство для использования этого макета:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Кроме того, этот макет можно также выполнить, задав [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) для свойства `LinearItemsLayout` объект, указав в `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) качестве `Orientation` значения свойства элемент перечисления:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

Это приводит к увеличению макета по горизонтали по мере добавления новых элементов:

[![Снимок экрана Карауселвиев горизонтального макета на iOS и Android](layout-images/horizontal.png "Горизонтальный макет Карауселвиев")](layout-images/horizontal-large.png#lightbox "Горизонтальный макет Карауселвиев")

## <a name="vertical-layout"></a>Вертикальное расположение

[`CarouselView`](xref:Xamarin.Forms.CarouselView)может отображать элементы по вертикали, присвоив [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) свойству `LinearItemsLayout` значение Object, указав в `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) качестве `Orientation` значения свойства элемент перечисления:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CarouselView.ItemsLayout>
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

Это приводит к увеличению макета по вертикали по мере добавления новых элементов:

[![Снимок экрана с вертикальным макетом Карауселвиев на iOS и Android](layout-images/vertical.png "Вертикальный макет Карауселвиев")](layout-images/vertical-large.png#lightbox "Вертикальный макет Карауселвиев")

## <a name="partially-visible-adjacent-items"></a>Частично видимые смежные элементы

По умолчанию [`CarouselView`](xref:Xamarin.Forms.CarouselView) отображает все элементы одновременно. Однако это поведение можно изменить, задав `PeekAreaInsets` свойству `Thickness` значение, которое указывает, какой объем смежных элементов должен быть частично видимым. Это может быть полезно для того, чтобы пользователи могли просматривать дополнительные элементы. В следующем коде XAML показан пример задания этого свойства:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    ...
</CarouselView>
```

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    PeekAreaInsets = new Thickness(100)
};
```

В результате смежные элементы частично открываются на экране:

[![Снимок экрана CollectionView с частично видимыми смежными элементами в iOS и Android](layout-images/peek-items.png "Карауселвиев область просмотра инсетс")](layout-images/peek-items-large.png#lightbox "Карауселвиев Пиковая область инсетс")

## <a name="item-spacing"></a>Промежуток между элементами

По умолчанию между каждым элементом в не существует пробела [`CarouselView`](xref:Xamarin.Forms.CarouselView) . Это поведение можно изменить, задав `ItemSpacing` свойство в макете элементов, используемом `CarouselView` .

Когда [`CarouselView`](xref:Xamarin.Forms.CarouselView) [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) свойство присваивает свойству `LinearItemsLayout` объект, `LinearItemsLayout.ItemSpacing` свойству может быть присвоено `double` значение, представляющее пробел между элементами:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

> [!NOTE]
> `LinearItemsLayout.ItemSpacing`Свойство имеет набор обратного вызова проверки, который гарантирует, что значение свойства всегда больше или равно 0.

Эквивалентный код на C# выглядит так:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

Этот код приводит к вертикальному макету с интервалом, равным 20 между элементами.

## <a name="dynamic-resizing-of-items"></a>Динамическое изменение размера элементов

В [`CarouselView`](xref:Xamarin.Forms.CarouselView) среде выполнения элементы в можно динамически изменять размер, изменяя связанные с макетом свойства элементов в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Например, в следующем примере кода изменяются [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) Свойства и объекта и [`Image`](xref:Xamarin.Forms.Image) `HeightRequest` свойство его родителя [`Frame`](xref:Xamarin.Forms.Frame) :

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(150) ? 200 : 150;
    Frame frame = ((Frame)image.Parent.Parent);
    frame.HeightRequest = frame.HeightRequest.Equals(300) ? 350 : 300;
}
```

`OnImageTapped`Обработчик событий выполняется в ответ на [`Image`](xref:Xamarin.Forms.Image) касание объекта и изменяет размеры изображения (и его родителя `Frame` ), чтобы упростить его просмотр:

[![Снимок экрана Карауселвиев с динамическим изменением размера элемента в iOS и Android](layout-images/runtime-resizing.png "Изменение размера динамического элемента Карауселвиев")](layout-images/runtime-resizing-large.png#lightbox "Изменение размера динамического элемента Карауселвиев")

## <a name="right-to-left-layout"></a>Макет с письмом справа налево

[`CarouselView`](xref:Xamarin.Forms.CarouselView)может Разметка содержимого в направлении справа налево путем присвоения [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) свойству значения [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) . Однако в `FlowDirection` идеале это свойство должно быть установлено на странице или в корневом макете, что приводит к тому, что все элементы на странице или в корневом макете реагируют на направление потока:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CarouselViewDemos.Views.HorizontalTemplateLayoutRTLPage"
             Title="Horizontal layout (RTL FlowDirection)"
             FlowDirection="RightToLeft">    
    <CarouselView ItemsSource="{Binding Monkeys}">
        ...
    </CarouselView>
</ContentPage>
```

По умолчанию [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) для элемента с родительским элементом имеет значение [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) . Таким образом, объект [`CarouselView`](xref:Xamarin.Forms.CarouselView) наследует `FlowDirection` значение свойства из [`ContentPage`](xref:Xamarin.Forms.ContentPage) .

Дополнительные сведения о направлении потока см. [в разделе локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Связанные ссылки

- [Карауселвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin.FormsПрокрутка Карауселвиев](scrolling.md)
