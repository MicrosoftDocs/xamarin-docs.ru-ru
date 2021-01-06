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
ms.openlocfilehash: 072f5db9115069fad547bb363865a609e5167ce8
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97939944"
---
# <a name="scrollview-content-touches-on-ios"></a>Скроллвиевное содержимое касается iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Неявный таймер активируется, когда жест касания начинается в [`ScrollView`](xref:Xamarin.Forms.ScrollView) iOS, и `ScrollView` принимает решение на основе действия пользователя в диапазоне таймера, должно ли оно обрабатывать этот жест или передавать его содержимому. По умолчанию в iOS `ScrollView` содержимое задерживается, но это может вызвать проблемы в некоторых обстоятельствах, `ScrollView` когда содержимое не выигрывает жест, когда оно должно. Поэтому эта платформа управляет тем, обрабатывает ли объект `ScrollView` жест касания или передает его содержимому. Он используется в XAML путем присвоения `ScrollView.ShouldDelayContentTouches` свойству присоединенного свойства `boolean` значения:

```xaml
<FlyoutPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <FlyoutPage.Flyout>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </FlyoutPage.Flyout>
    <FlyoutPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </FlyoutPage.Detail>
</FlyoutPage>
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

В результате [`ScrollView`](xref:Xamarin.Forms.ScrollView) может быть отключена Задержка получения содержимого, поэтому в этом сценарии объект [`Slider`](xref:Xamarin.Forms.Slider) получает жест, а не [`Detail`](xref:Xamarin.Forms.FlyoutPage.Detail) страницу из [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) :

[![Скроллвиев задержка содержимого касается конкретной платформы](scrollview-content-touches-images/scrollview-delay-content-touches.png)](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "Скроллвиев с задержкой содержимого Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)