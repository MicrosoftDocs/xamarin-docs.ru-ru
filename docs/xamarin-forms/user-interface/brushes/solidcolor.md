---
title: 'Xamarin.FormsКисти: сплошные цвета'
description: Xamarin.FormsКласс SolidColorBrush закрашивает область сплошным цветом.
ms.prod: xamarin
ms.assetid: 4225D40A-16C1-40E1-ACBE-23E321E7FDE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 04e882f9b64d0e1f56e1170d353af4e13b079933
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919141"
---
# <a name="no-locxamarinforms-brushes-solid-colors"></a>Xamarin.FormsКисти: сплошные цвета

![Предварительный просмотр API](~/media/shared/preview.png "Этот API-интерфейс сейчас доступен в предварительной версии.")

[![Скачать пример](~/media/shared/download.png) Скачайте пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)

`SolidColorBrush`Класс является производным от `Brush` класса и используется для заливки области сплошным цветом. Существует множество подходов к указанию цвета `SolidColorBrush` . Например, можно указать его цвет со [`Color`](xref:Xamarin.Forms.Color) значением или с помощью одного из предопределенных `SolidColorBrush` объектов, предоставляемых `Brush` классом.

`SolidColorBrush`Класс определяет `Color` свойство типа [`Color`](xref:Xamarin.Forms.Color) , представляющее цвет кисти. Это свойство поддерживается [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом, что означает, что он может быть целевым объектом привязок данных и имеет стиль.

`SolidColorBrush`Класс также содержит `IsEmpty` метод, который возвращает объект `bool` , который представляет, назначена ли кисть цвет.

## <a name="create-a-solidcolorbrush"></a>Создание SolidColorBrush

Существует три основных метода создания `SolidColorBrush` . Можно создать `SolidColorBrush` из [`Color`](xref:Xamarin.Forms.Color) , использовать предопределенную кисть или создать `SolidColorBrush` с помощью шестнадцатеричной нотации.

### <a name="use-a-predefined-color"></a>Использовать стандартный цвет

Xamarin.Formsвключает преобразователь типов, который создает `SolidColorBrush` из [`Color`](xref:Xamarin.Forms.Color) значения. В XAML это позволяет `SolidColorBrush` создать из предопределенного `Color` значения:

```xaml
<Frame Background="DarkBlue"
       BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120" />
```

В этом примере фон [`Frame`](xref:Xamarin.Forms.Frame) закрашивается темно-синим `SolidColorBrush` :

![Рамка окрашена в стандартный цвет](solidcolor-images/predefined-color.png)

Кроме того, [`Color`](xref:Xamarin.Forms.Color) значение можно указать с помощью синтаксиса тега свойства:

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
       <Frame.Background>
           <SolidColorBrush Color="DarkBlue" />
       </Frame.Background>
</Frame>
```

В этом примере фон [`Frame`](xref:Xamarin.Forms.Frame) зарисовывается `SolidColorBrush` цветом, цвет которого задается путем установки `SolidColorBrush.Color` Свойства.

### <a name="use-a-predefined-brush"></a>Использование предопределенной кисти

`Brush`Класс определяет набор часто используемых `SolidColorBrush` объектов. В следующем примере используется один из следующих предопределенных `SolidColorBrush` объектов:

```xaml
<Frame Background="{x:Static Brush.Indigo}"
       BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120" />       
```

Эквивалентный код на C# выглядит так:

```csharp
Frame frame = new Frame
{
    Background = Brush.Indigo,
    BorderColor = Color.LightGray,
    // ...
};
```

В этом примере фон [`Frame`](xref:Xamarin.Forms.Frame) закрашивается с помощью Indigo `SolidColorBrush` :

![Кадр закрашен с предопределенным SolidColorBrush](solidcolor-images/predefined-brush.png)

Список предопределенных `SolidColorBrush` объектов, предоставляемых `Brush` классом, см. в разделе [сплошные цветные кисти](#solid-color-brushes).

### <a name="use-hexadecimal-notation"></a>Использовать шестнадцатеричную нотацию

`SolidColorBrush`объекты также можно создавать с помощью шестнадцатеричного формата. При таком подходе цвет задается с точки зрения красного, зеленого и синего цветов для объединения в один цвет. Основной формат для указания цвета в шестнадцатеричной нотации — `#rrggbb` , где:

- `rr`представляет собой двузначное шестнадцатеричное число, указывающее относительный объем красного.
- `gg`представляет собой двузначное шестнадцатеричное число, определяющее относительный объем зеленого цвета.
- `bb`представляет собой двузначное шестнадцатеричное число, указывающее относительный объем синего.

Кроме того, цвет можно указать как, `#aarrggbb` где `aa` указывает альфа-значение или прозрачность цвета. Этот подход позволяет создавать частично прозрачные цвета.

В следующем примере задается значение цвета `SolidColorBrush` с использованием шестнадцатеричной нотации:

```xaml
<Frame Background="#FF9988"
       BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120" />
```

В этом примере фон изображения [`Frame`](xref:Xamarin.Forms.Frame) закрашивается оранжево цветом `SolidColorBrush` :

![Кадр, закрашенный SolidColorBrush, созданной с помощью шестнадцатеричной нотации](solidcolor-images/hex.png)

Другие способы описания цвета см. в разделе [цвета в Xamarin.Forms ](~/xamarin-forms/user-interface/colors.md).

## <a name="solid-color-brushes"></a>Кисти сплошной заливки

Для удобства `Brush` класс предоставляет набор часто используемых `SolidColorBrush` объектов, таких как `AliceBlue` и `YellowGreen` . На следующем рисунке показан цвет каждой предопределенной кисти, ее имя и шестнадцатеричное значение:

[![Таблица цветов, включая цветовую палитру, имя цвета и шестнадцатеричное значение](solidcolor-images/solidcolorbrushes.png)](solidcolor-images/solidcolorbrushes-large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Брушесдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)
- [Цвета вXamarin.Forms](~/xamarin-forms/user-interface/colors.md)
