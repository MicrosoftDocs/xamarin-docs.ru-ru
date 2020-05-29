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
ms.openlocfilehash: dcbc20139b989ced11f2d1d890ca7dd99a780e96
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137063"
---
# <a name="navigationpage-bar-text-color-mode-on-ios"></a>Цветовой режим текста на панели Навигатионпаже в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа определяет, изменяется ли цвет текста в строке состояния в с [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) учетом яркости панели навигации. Он используется в XAML путем присвоения [`NavigationPage.StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty) свойству присоединенного свойства значения [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) перечисления:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

`NavigationPage.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `NavigationPage.SetStatusBarTextColorMode` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Навигатионпаже. Сетстатусбартекстколормоде ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Навигатионпаже}, Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Статусбартекстколормоде)) в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен определяет, корректируется ли цвет текста в строке состояния на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) соответствие яркости панели навигации, при этом [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) перечисление предоставляет два возможных значения:

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust)— Указывает, что цвет текста в строке состояния не должен быть скорректирован.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity)— Указывает, что цвет текста строки состояния должен соответствовать яркости панели навигации.

Кроме того, [ `GetStatusBarTextColorMode` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Навигатионпаже. Жетстатусбартекстколормоде ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Навигатионпаже})). метод можно использовать для получения текущего значения [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) перечисления, применяемого к [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) .

В результате цвет текста строки состояния на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) можно изменить в соответствии с яркостью панели навигации. В этом примере цвет текста строки состояния изменяется при переключении пользователя между [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) страницами и [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) :

![](status-bar-text-color-images/status-bar-text-color-mode.png "Status Bar Text Color Mode Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
