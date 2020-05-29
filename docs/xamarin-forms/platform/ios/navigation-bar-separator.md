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
ms.openlocfilehash: 39d203cf0fb7fff026106d98cfb512aad42f83d2
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128249"
---
# <a name="navigationpage-bar-separator-on-ios"></a>Разделитель Навигатионпаже Bar в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

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

![](navigation-bar-separator-images/navigationpage-hideseparatorbar.png "NavigationPage navigation bar hidden")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
