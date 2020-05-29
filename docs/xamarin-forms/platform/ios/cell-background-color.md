---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 90282262926fef663183be247e37d64dd1be9124
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138573"
---
# <a name="cell-background-color-on-ios"></a>Цвет фона ячейки в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS задает цвет фона по умолчанию для [`Cell`](xref:Xamarin.Forms.Cell) экземпляров. Он используется в XAML путем установки `Cell.DefaultBackgroundColor` Свойства BIND в значение [`Color`](xref:Xamarin.Forms.Color) :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  IsGroupingEnabled="true">
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <ViewCell ios:Cell.DefaultBackgroundColor="Teal">
                        <Label Margin="10,10"
                               Text="{Binding Key}"
                               FontAttributes="Bold" />
                    </ViewCell>
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

`ListView.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `Cell.SetDefaultBackgroundColor`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен задает для цвета фона ячейки указанный объект [`Color`](xref:Xamarin.Forms.Color) . Кроме того, `Cell.DefaultBackgroundColor` метод можно использовать для получения текущего цвета фона ячейки.

В результате в качестве цвета фона в [`Cell`](xref:Xamarin.Forms.Cell) можно задать определенное значение [`Color`](xref:Xamarin.Forms.Color) :

[![Снимок экрана: ячейки заголовка синего группы в iOS](cell-background-color-images/group-header-cell-color.png "ListView с синей ячейкой заголовка группы")](cell-background-color-images/group-header-cell-color-large.png#lightbox "ListView с синей ячейкой заголовка группы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
