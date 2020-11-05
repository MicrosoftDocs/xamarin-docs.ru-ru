---
title: Xamarin.Forms NOFRAMES
description: Xamarin.FormsКласс Frame — это макет, используемый для создания оболочки представления или макета с границей, которая может быть настроена с помощью цвета, тени и других параметров.
ms.prod: xamarin
ms.assetId: 4E074714-0928-41C8-A468-B60E23236A8C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/06/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ba5fd2e8488f1f28f6bdc02b85c8e41fa212be32
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373748"
---
# <a name="no-locxamarinforms-frame"></a>Xamarin.Forms NOFRAMES

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-frame/)

Xamarin.Forms [`Frame`](xref:Xamarin.Forms.Frame) Класс является макетом, используемым для создания оболочки представления с границей, которая может быть настроена с помощью цвета, тени и других параметров. Фреймы обычно используются для создания границ вокруг элементов управления, но их можно использовать для создания более сложного пользовательского интерфейса. Дополнительные сведения см. в разделе [Расширенное использование кадров](#advanced-frame-usage).

На следующем снимке экрана показаны `Frame` элементы управления iOS и Android.

[!["Примеры кадров в iOS и Android"](frame-images/frame-cropped.png)](frame-images/frame-full.png#lightbox "Примеры кадров в iOS и Android")

Класс `Frame` определяет следующие свойства:

* [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)`Color`значение, определяющее цвет `Frame` границы.
* [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)`float`значение, определяющее закругленный радиус угла.
* [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow)`bool`значение, определяющее, содержит ли кадр тень.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что `Frame` может быть целевым объектом привязок данных.

> [!NOTE]
> `HasShadow`Поведение свойства зависит от платформы. Значение по умолчанию — `true` на всех платформах. Однако на тенях тени UWP не отображаются. Тени отображаются в Android и iOS, но тени на iOS темнее и занимают больше места.

## <a name="create-a-frame"></a>Создание рамки

`Frame`Экземпляр можно создать в XAML. Объект по умолчанию `Frame` имеет белый фон, тень и без границы. `Frame`Объект обычно заключает в оболочку другой элемент управления. В следующем примере показан перенос объекта по умолчанию `Frame` `Label` .

```xaml
<Frame>
  <Label Text="Example" />
</Frame>
```

`Frame`Также можно создать в коде:

```csharp
Frame defaultFrame = new Frame
{
    Content = new Label { Text = "Example" }
};
```

`Frame` объекты можно настраивать с помощью скругленных углов, цветных границ и теней, задавая свойства в XAML. В следующем примере показан настраиваемый `Frame` объект:

```xaml
<Frame BorderColor="Orange"
       CornerRadius="10"
       HasShadow="True">
  <Label Text="Example" />
</Frame>
```

Эти свойства экземпляра также можно задать в коде:

```csharp
Frame frame = new Frame
{
    BorderColor = Color.Orange,
    CornerRadius = 10,
    HasShadow = true,
    Content = new Label { Text = "Example" }
};
```

## <a name="advanced-frame-usage"></a>Расширенное использование кадров

`Frame`Класс наследует от `ContentView` , что означает, что он может содержать любой тип `View` объекта, включая `Layout` объекты. Эта возможность позволяет `Frame` использовать для создания сложных объектов пользовательского интерфейса, таких как карты.

### <a name="create-a-card-with-a-frame"></a>Создание карточки с рамкой

Объединение `Frame` объекта с объектом, `Layout` таким как объект, `StackLayout` позволяет создать более сложный пользовательский интерфейс. На следующем снимке экрана показан пример карточки, созданной с помощью `Frame` объекта:

[![Снимок экрана Карты, созданной с помощью кадра](frame-images/frame-card-cropped.png)](frame-images/frame-full.png#lightbox "Снимок экрана Карты, созданной с помощью кадра")

В следующем коде XAML показано, как создать карточку с `Frame` классом:

```xaml
<Frame BorderColor="Gray"
       CornerRadius="5"
       Padding="8">
  <StackLayout>
    <Label Text="Card Example"
           FontSize="Medium"
           FontAttributes="Bold" />
    <BoxView Color="Gray"
             HeightRequest="2"
             HorizontalOptions="Fill" />
    <Label Text="Frames can wrap more complex layouts to create more complex UI components, such as this card!"/>
  </StackLayout>
</Frame>
```

Карту также можно создать в коде:

```csharp
Frame cardFrame = new Frame
{
    BorderColor = Color.Gray,
    CornerRadius = 5,
    Padding = 8,
    Content = new StackLayout
    {
        Children =
        {
            new Label
            {
                Text = "Card Example",
                FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label)),
                FontAttributes = FontAttributes.Bold
            },
            new BoxView
            {
                Color = Color.Gray,
                HeightRequest = 2,
                HorizontalOptions = LayoutOptions.Fill
            },
            new Label
            {
                Text = "Frames can wrap more complex layouts to create more complex UI components, such as this card!"
            }
        }
    }
};
```

### <a name="round-elements"></a>Элементы Round

`CornerRadius`Свойство `Frame` элемента управления можно использовать для создания кругового изображения. На следующем снимке экрана показан пример кругового изображения, созданного с помощью `Frame` объекта:

[![Снимок экрана с изображением круга, созданным с помощью кадра "](frame-images/circle-image-cropped.png)](frame-images/frame-full.png#lightbox "Снимок экрана с изображением круга, созданным с помощью рамки")

В следующем коде XAML показано, как создать изображение круга на языке XAML:

```xaml
<Frame Margin="10"
       BorderColor="Black"
       CornerRadius="50"
       HeightRequest="60"
       WidthRequest="60"
       IsClippedToBounds="True"
       HorizontalOptions="Center"
       VerticalOptions="Center">
  <Image Source="outdoors.jpg"
         Aspect="AspectFill"
         Margin="-20"
         HeightRequest="100"
         WidthRequest="100" />
</Frame>
```

Изображение в виде круга также можно создать в коде:

```csharp
Frame circleImageFrame = new Frame
{
    Margin = 10,
    BorderColor = Color.Black,
    CornerRadius = 50,
    HeightRequest = 60,
    WidthRequest = 60,
    IsClippedToBounds = true,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center,
    Content = new Image
    {
        Source = ImageSource.FromFile("outdoors.jpg"),
        Aspect = Aspect.AspectFill,
        Margin = -20,
        HeightRequest = 100,
        WidthRequest = 100
    }
};
```

Образ **outdoors.jpg** должен быть добавлен в каждый проект платформы, и как это достигается в зависимости от платформы. Дополнительные сведения см. [в разделе изображения Xamarin.Forms в ](~/xamarin-forms/user-interface/images.md).

> [!NOTE]
> Скругленные углы ведут себя немного иначе на разных платформах. `Image`Объект `Margin` должен быть половиной разницы между шириной изображения и шириной родительского фрейма и должен быть отрицательным для центрирования изображения равномерно в пределах `Frame` объекта. Однако запрошенные ширина и высота не гарантированы, поэтому `Margin` `HeightRequest` `WidthRequest` Свойства и могут потребоваться изменить в зависимости от размера изображения и других вариантов макета.

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации кадров](/samples/xamarin/xamarin-forms-samples/userinterface-frame/)
* [Изображения в Xamarin.Forms](~/xamarin-forms/user-interface/images.md)