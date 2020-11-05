---
title: Разделитель Навигатионпаже Bar в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая скрывает разделительную линию и тень, расположенную в нижней части панели навигации в Навигатионпаже.
ms.prod: xamarin
ms.assetid: 5A45748A-6779-4441-82F2-415BD68473B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 44bd1ca75220d0aefe2f26bf0e3ba1afcde93150
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372357"
---
# <a name="navigationpage-bar-separator-on-ios"></a>Разделитель Навигатионпаже Bar в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS скрывает линию разделения и тень, расположенную в нижней части панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . Он используется в XAML путем установки [`NavigationPage.HideNavigationBarSeparator`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) Свойства BIND в значение `false` :

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ios:NavigationPage.HideNavigationBarSeparator="true">

</NavigationPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;

public class iOSTitleViewNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public iOSTitleViewNavigationPageCS()
    {
        On<iOS>().SetHideNavigationBarSeparator(true);
    }
}
```

`NavigationPage.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `NavigationPage.SetHideNavigationBarSeparator` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Навигатионпаже. Сесиденавигатионбарсепаратор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Навигатионпаже}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, является ли разделитель панели навигации скрытым. Кроме того, [ `NavigationPage.HideNavigationBarSeparator` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Навигатионпаже. Хиденавигатионбарсепаратор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Навигатионпаже})). можно использовать метод, чтобы вернуть скрытый разделитель панели навигации.

В результате разделитель панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) может быть скрытым:

![Панель навигации Навигатионпаже скрыта](navigation-bar-separator-images/navigationpage-hideseparatorbar.png)

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)