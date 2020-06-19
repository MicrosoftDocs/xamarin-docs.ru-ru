---
title: Скроллвиевное содержимое касается iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая определяет, обрабатывает ли Скроллвиев жест касания или передает его в его содержимое.
ms.prod: xamarin
ms.assetid: 99F823DB-B379-40F0-A343-A9783C341120
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9b8f743b2c3d7f4b38feb4cfc5015b1113620562
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137102"
---
# <a name="scrollview-content-touches-on-ios"></a>Скроллвиевное содержимое касается iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Неявный таймер активируется, когда жест касания начинается в [`ScrollView`](xref:Xamarin.Forms.ScrollView) iOS, и `ScrollView` принимает решение на основе действия пользователя в диапазоне таймера, должно ли оно обрабатывать этот жест или передавать его содержимому. По умолчанию в iOS `ScrollView` содержимое задерживается, но это может вызвать проблемы в некоторых обстоятельствах, `ScrollView` когда содержимое не выигрывает жест, когда оно должно. Поэтому эта платформа управляет тем, обрабатывает ли объект `ScrollView` жест касания или передает его содержимому. Он используется в XAML путем присвоения `ScrollView.ShouldDelayContentTouches` свойству присоединенного свойства `boolean` значения:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `ScrollView.SetShouldDelayContentTouches`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, обрабатывает ли объект [`ScrollView`](xref:Xamarin.Forms.ScrollView) жест касания или передает его содержимому. Кроме того, `SetShouldDelayContentTouches` метод можно использовать для переключения задержки содержимого с помощью вызова `ShouldDelayContentTouches` метода, чтобы вернуть отложенные касания содержимого:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

В результате [`ScrollView`](xref:Xamarin.Forms.ScrollView) может быть отключена Задержка получения содержимого, поэтому в этом сценарии объект [`Slider`](xref:Xamarin.Forms.Slider) получает жест, а не [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) страницу из [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) :

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "ScrollView Delay Content Touches Platform-Specific")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
