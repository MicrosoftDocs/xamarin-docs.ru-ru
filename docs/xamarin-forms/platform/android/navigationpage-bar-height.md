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
ms.openlocfilehash: 2dcabe3c0067734250834c2927fd4cbb83906943
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128795"
---
# <a name="navigationpage-bar-height-on-android"></a>Высота Навигатионпаже Bar на Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа Android определяет высоту панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . Он используется в XAML путем установки [`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) Свойства BIND в целочисленное значение:

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

`NavigationPage.On<Android>`Метод указывает, что эта платформа будет запускаться только на платформе с поддержкой совместимости с приложением Android. [ `NavigationPage.SetBarHeight` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. AppCompat. Навигатионпаже. Сетбархеигхт ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Навигатионпаже}, System. Int32). в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) пространстве имен используется для задания высоты панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . Кроме того, [ `NavigationPage.GetBarHeight` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. AppCompat. Навигатионпаже. Жетбархеигхт ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Навигатионпаже})). можно использовать метод, чтобы вернуть высоту панели навигации в `NavigationPage` .

В результате высота панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) может быть задана следующим образом:

![](navigationpage-bar-height-images/navigationpage-barheight.png "NavigationPage navigation bar height")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
