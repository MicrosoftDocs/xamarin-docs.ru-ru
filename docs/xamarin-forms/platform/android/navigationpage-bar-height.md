---
title: Высота Навигатионпаже Bar на Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, устанавливающую высоту панели навигации в Навигатионпаже.
ms.prod: xamarin
ms.assetid: C8A73B64-FE70-408A-A72E-8AF147F0C52C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5f5e6311c79a88a6018526a2e1c0c06065eefb32
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929744"
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

![Высота панели навигации Навигатионпаже](navigationpage-bar-height-images/navigationpage-barheight.png)

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
