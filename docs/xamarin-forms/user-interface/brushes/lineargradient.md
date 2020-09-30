---
title: 'Xamarin.Forms Кисти: линейные градиенты'
description: Xamarin.FormsКласс LinearGradientBrush закрашивает область с линейным градиентом.
ms.prod: xamarin
ms.assetid: BEA2B3F5-96B0-4E39-88A6-0FAFE95C3DCD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: be4b868a2f38063f3a46d57ecb190b377eb89dce
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563592"
---
# <a name="no-locxamarinforms-brushes-linear-gradients"></a>Xamarin.Forms Кисти: линейные градиенты

![Предварительный просмотр API](~/media/shared/preview.png "Этот API-интерфейс сейчас доступен в предварительной версии.")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)

`LinearGradientBrush`Класс является производным от `GradientBrush` класса и закрашивает область линейным градиентом, который смешивает два или более цвета вдоль линии, известной как ось градиента. `GradientStop` объекты используются для указания цветов в градиенте и их позиций. Дополнительные сведения об `GradientStop` объектах см. в разделе [ Xamarin.Forms кисти: градиенты](gradient.md).

Класс `LinearGradientBrush` определяет следующие свойства:

- `StartPoint`Тип [`Point`](xref:Xamarin.Forms.Point) , который представляет начальные двухмерные координаты линейного градиента. Значение этого свойства по умолчанию равно (0, 0).
- `EndPoint`Тип [`Point`](xref:Xamarin.Forms.Point) , который представляет завершающие двухмерные координаты линейного градиента. Значение этого свойства по умолчанию равно (1, 1).

Эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки и стилизации данных.

`LinearGradientBrush`Класс также является `IsEmpty` методом, возвращающим объект `bool` , который представляет, назначена ли кисть какие-либо `GradientStop` объекты.

> [!NOTE]
> Линейные градиенты также можно создавать с помощью `linear-gradient()` функции CSS.

## <a name="create-a-lineargradientbrush"></a>Создание LinearGradientBrush

Ограничители градиента кисти линейного градиента располагаются вдоль оси градиента. Ориентацию и размер оси градиента можно изменить с помощью `StartPoint` свойств кисти и `EndPoint` . Управляя этими свойствами, можно создавать горизонтальные, вертикальные и диагональные градиенты, отменять направление градиента, уплотнение распределения градиента и т. д.

`StartPoint`Свойства и `EndPoint` задаются относительно закрашиваемой области. (0, 0) представляет верхний левый угол закрашиваемой области, а (1, 1) — нижний правый угол закрашиваемой области. На следующей диаграмме показана ось градиента для кисти по диагонали линейного градиента.

![Рамка с диагональной осью градиента](lineargradient-images/gradient-axis.png)

На этой схеме пунктирная линия показывает ось градиента, которая выделяет путь интерполяции градиента от начальной точки к конечной точке.

### <a name="create-a-horizontal-linear-gradient"></a>Создание горизонтального линейного градиента

Чтобы создать горизонтальный линейный градиент, создайте `LinearGradientBrush` объект и установите его `StartPoint` в значение (0, 0) и задайте значение `EndPoint` (1, 0). Затем добавьте в коллекцию два или более `GradientStop` объектов, `LinearGradientBrush.GradientStops` которые определяют цвета градиента и их позиции.

В следующем примере XAML показан горизонтальный `LinearGradientBrush` , заданный как элемент `Background` [`Frame`](xref:Xamarin.Forms.Frame) :

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- StartPoint defaults to (0,0) -->
        <LinearGradientBrush EndPoint="1,0">
            <GradientStop Color="Yellow"
                          Offset="0.1" />
            <GradientStop Color="Green"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Frame.Background>
</Frame>  
```

В этом примере фон [`Frame`](xref:Xamarin.Forms.Frame) закрашивается с `LinearGradientBrush` , который выполняет интерполяцию от желтой к зеленой по горизонтали:

![Кадр, закрашенный горизонтальным LinearGradientBrush](lineargradient-images/horizontal.png)

### <a name="create-a-vertical-linear-gradient"></a>Создание вертикального линейного градиента

Чтобы создать Вертикальный линейный градиент, создайте `LinearGradientBrush` объект и задайте для его свойства `StartPoint` значение (0, 0) и значение `EndPoint` (0, 1). Затем добавьте в коллекцию два или более `GradientStop` объектов, `LinearGradientBrush.GradientStops` которые определяют цвета градиента и их позиции.

В следующем примере XAML показан вертикальный `LinearGradientBrush` , заданный в качестве `Background` объекта [`Frame`](xref:Xamarin.Forms.Frame) :

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- StartPoint defaults to (0,0) -->    
        <LinearGradientBrush EndPoint="0,1">
            <GradientStop Color="Yellow"
                          Offset="0.1" />
            <GradientStop Color="Green"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Frame.Background>
</Frame>
```

В этом примере фон [`Frame`](xref:Xamarin.Forms.Frame) закрашивается с `LinearGradientBrush` , который выполняет интерполяцию от желтого к зеленому по вертикали:

![Кадр, закрашенный вертикальным LinearGradientBrush](lineargradient-images/vertical.png)

### <a name="create-a-diagonal-linear-gradient"></a>Создание диагонального линейного градиента

Чтобы создать диагональный линейный градиент, создайте `LinearGradientBrush` объект и задайте для него значение `StartPoint` (0, 0), а в качестве значения `EndPoint` (1, 1). Затем добавьте в коллекцию два или более `GradientStop` объектов, `LinearGradientBrush.GradientStops` которые определяют цвета градиента и их позиции.

В следующем примере XAML показана диагональная `LinearGradientBrush` , которая задается в качестве значения `Background` [`Frame`](xref:Xamarin.Forms.Frame) :

```xaml
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- StartPoint defaults to (0,0)      
             Endpoint defaults to (1,1) -->
        <LinearGradientBrush>
            <GradientStop Color="Yellow"
                          Offset="0.1" />
            <GradientStop Color="Green"
                          Offset="1.0" />
        </LinearGradientBrush>
    </Frame.Background>
</Frame>
```

В этом примере фон [`Frame`](xref:Xamarin.Forms.Frame) закрашивается с `LinearGradientBrush` , который выполняет интерполяцию от желтого к зеленому:

![Кадр, окрашенный в диагональный LinearGradientBrush](lineargradient-images/diagonal.png)

## <a name="related-links"></a>Связанные ссылки

- [Брушесдемос (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)
- [Xamarin.Forms Кисти: градиенты](gradient.md)