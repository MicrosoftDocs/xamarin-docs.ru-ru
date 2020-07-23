---
title: Параметры макета вXamarin.Forms
description: Каждое Xamarin.Forms представление имеет свойства хоризонталоптионс и вертикалоптионс типа LayoutOptions. В этой статье объясняется, как каждое значение LayoutOptions имеет выравнивание и расширение представления.
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1a3b9db435de49c438f458d1c4d85d3f81bbf749
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930693"
---
# <a name="layout-options-in-xamarinforms"></a>Параметры макета вXamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачайте пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)

_Каждое Xamarin.Forms представление имеет свойства хоризонталоптионс и вертикалоптионс типа LayoutOptions. В этой статье объясняется, как каждое значение LayoutOptions имеет выравнивание и расширение представления._

## <a name="overview"></a>Обзор

[`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)Структура инкапсулирует два предпочтения макета:

- **Выравнивание** — предпочтительное выравнивание представления, которое определяет его положение и размер в родительском макете.
- **Расширение** — используется только в [`StackLayout`](xref:Xamarin.Forms.StackLayout) и указывает, должно ли представление использовать дополнительное пространство, если оно доступно.

Эти параметры макета можно применить к [`View`](xref:Xamarin.Forms.View) объекту относительно его родителя, установив [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) свойство или объекта в `View` одно из открытых полей [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) структуры. Доступны следующие открытые поля:

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`Start`Поля, `Center` , `End` и `Fill` используются для определения выравнивания представления в родительском макете.

- Для горизонтального выравнивания [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) размещается в [`View`](xref:Xamarin.Forms.View) левой части родительского макета, а для вертикального выравнивания — в верхней части `View` родительского макета.
- Для горизонтального и вертикального выравнивания по [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) горизонтали или по вертикали разцентрированы [`View`](xref:Xamarin.Forms.View) .
- Для горизонтального выравнивания [`End`](xref:Xamarin.Forms.LayoutOptions.End) размещается в [`View`](xref:Xamarin.Forms.View) правой части родительского макета, а для вертикального выравнивания — в нижней части `View` родительского макета.
- Для выравнивания по горизонтали [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) обеспечивает [`View`](xref:Xamarin.Forms.View) Заполнение ширины родительского макета, а для вертикального выравнивания обеспечивает `View` Заполнение высоты родительского макета.

`StartAndExpand`Значения, `CenterAndExpand` , `EndAndExpand` и `FillAndExpand` используются для определения параметров выравнивания и того, будет ли представление занимать больше места, если оно доступно в пределах родителя [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

> [!NOTE]
> Значение по умолчанию свойств представления [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) — [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill).

## <a name="alignment"></a>Выравнивание

Выравнивание управляет порядком расположения представления в родительском макете, если родительский макет содержит неиспользуемое пространство (то есть размер родительского макета больше, чем общий размер всех его дочерних элементов).

Объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) учитывает только `Start` `Center` поля,, `End` и `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) дочерних представлений, которые находятся в противоположном направлении к `StackLayout` ориентации. Таким образом, дочерние представления в вертикально-ориентированном режиме `StackLayout` могут задавать [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) для их свойств одно из `Start` полей,, `Center` `End` или `Fill` . Аналогичным образом, дочерние представления в горизонтально-ориентированном режиме `StackLayout` могут задавать [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) для их свойств одно из `Start` полей,, `Center` `End` или `Fill` .

Не [`StackLayout`](xref:Xamarin.Forms.StackLayout) учитывает `Start` `Center` поля,, `End` и `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) в дочерних представлениях, которые находятся в том же направлении, что и `StackLayout` ориентация. Таким образом, вертикально `StackLayout` пропускает `Start` поля, `Center` , и, `End` `Fill` если они заданы для [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) свойств дочерних представлений. Аналогично, горизонтально ориентированный `StackLayout` игнорирует `Start` `Center` поля,, `End` или, `Fill` если они заданы для [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) свойств дочерних представлений.

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)обычно переопределяет запросы размера, заданные с помощью [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) свойств и.

В следующем примере кода XAML демонстрируется вертикальная ориентация, в [`StackLayout`](xref:Xamarin.Forms.StackLayout) которой каждый дочерний элемент [`Label`](xref:Xamarin.Forms.Label) устанавливает свое [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) свойство в одно из четырех полей выравнивания из [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) структуры:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Эквивалентный код C# показан ниже:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

Код приводит к отображению макета на следующих снимках экрана:

[![Параметры макета выравнивания](layout-options-images/alignment.png)](layout-options-images/alignment-large.png#lightbox "Параметры макета выравнивания")

## <a name="expansion"></a>Расширение

Расширение определяет, будет ли представление занимать больше места, если доступно, в [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Если объект `StackLayout` содержит неиспользуемое пространство (то есть `StackLayout` больше, чем общий размер всех его дочерних элементов), неиспользуемое пространство совместно используется всеми дочерними представлениями, которые запрашивают расширение, путем задания [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) свойств или для [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) поля, использующего `AndExpand` суффикс. Обратите внимание, что если используется все пространство в `StackLayout` , параметры расширения не действуют.

[`StackLayout`](xref:Xamarin.Forms.StackLayout) может развернуть дочерние представления только в направлении своей ориентации. Таким образом, вертикально `StackLayout` может расширять дочерние представления, устанавливающие их [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) свойства в одно из `StartAndExpand` `CenterAndExpand` полей,, `EndAndExpand` или `FillAndExpand` , если `StackLayout` содержит неиспользуемое пространство. Аналогично, горизонтально ориентированный `StackLayout` может развернуть дочерние представления, устанавливающие их [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) свойства, в одно из `StartAndExpand` полей,, `CenterAndExpand` `EndAndExpand` или `FillAndExpand` , если `StackLayout` содержит неиспользуемое пространство.

[`StackLayout`](xref:Xamarin.Forms.StackLayout)Не может развернуть дочерние представления в направлении, противоположном его ориентации. Таким образом, по вертикали `StackLayout` Установка свойства в [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) дочернем представлении [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) имеет тот же результат, что и присвоение свойству значения [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) .

> [!NOTE]
> Обратите внимание, что включение расширения не изменяет размер представления, если оно не используется [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) .

В следующем примере кода XAML демонстрируется вертикальная ориентация, в [`StackLayout`](xref:Xamarin.Forms.StackLayout) которой каждый дочерний элемент [`Label`](xref:Xamarin.Forms.Label) устанавливает свое [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) свойство в одно из четырех полей расширения из [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) структуры:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

Эквивалентный код C# показан ниже:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

Код приводит к отображению макета на следующих снимках экрана:

[![Параметры макета расширения](layout-options-images/expansion.png)](layout-options-images/expansion-large.png#lightbox "Параметры макета расширения")

Каждый из них [`Label`](xref:Xamarin.Forms.Label) занимает одинаковый объем пространства внутри [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Но только последний `Label` со значением свойства [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions), равным [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand), имеет другой размер. Кроме того, каждая из них `Label` отделяется небольшим красным [`BoxView`](xref:Xamarin.Forms.BoxView) , что позволяет легко просматривать пространство, занимаемое запятыми `Label` .

## <a name="summary"></a>Итоги

В этой статье объясняется, [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) как каждое значение структуры имеет выравнивание и расширение представления относительно его родителя. `Start`Поля, `Center` , `End` и используются `Fill` для определения выравнивания представления в родительском макете, а `StartAndExpand` `CenterAndExpand` поля,, `EndAndExpand` и `FillAndExpand` используются для определения параметров выравнивания, а также для определения того, займет ли представление больше места, если оно доступно, в [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

## <a name="related-links"></a>Связанные ссылки

- [LayoutOptions (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
