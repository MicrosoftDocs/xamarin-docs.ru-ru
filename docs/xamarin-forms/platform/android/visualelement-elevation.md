---
title: Повышение прав Висуалелемент в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, которая управляет повышением уровня Висуалелементс в приложениях, предназначенных для API 21 или более поздней версии.
ms.prod: xamarin
ms.assetid: 5BFD6175-2BBD-41CD-B8F9-521B4750B708
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: a721f51d3f59bc166a48f5cc3a3eec9712ace837
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996686"
---
# <a name="visualelement-elevation-on-android"></a>Повышение прав Висуалелемент в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android используется для управления повышением или Z-порядком визуальных элементов в приложениях, предназначенных для API 21 или более поздней версии. Повышение уровня визуального элемента определяет его порядок отображения, при этом визуальные элементы с более высоким значением Z окклудинг визуальные элементы с более низкими значениями Z. Он используется в XAML путем присвоения `VisualElement.Elevation` свойству присоединенного свойства `boolean` значения:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;assembly=:::no-loc(Xamarin.Forms):::.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration;
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. `VisualElement.SetElevation`Метод в [`:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific) пространстве имен используется для установки повышения уровня визуального элемента до значения NULL `float` . Кроме того, `VisualElement.GetElevation` метод можно использовать для получения значения повышения прав визуального элемента.

В результате повышение уровня визуальных элементов может быть управляемым, чтобы визуальные элементы с более высоким значением Z окклуде визуальные элементы с более низкими значениями Z. Таким образом, в этом примере вторая отображается [`Button`](xref::::no-loc(Xamarin.Forms):::.Button) над, [`BoxView`](xref::::no-loc(Xamarin.Forms):::.BoxView) так как имеет более высокое значение повышения прав:

![Снимок экрана Висуалелемент повышения прав](visualelement-elevation-images/elevation.png)

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.AppCompat)
