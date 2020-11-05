---
title: Навигатионпаже линейчатая полупрозрачность на iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать особенности платформы iOS, которые изменяют прозрачность панели навигации в Навигатионпаже.
ms.prod: xamarin
ms.assetid: 1150941B-56DB-4781-BE36-A4C4F9F2C500
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c565467d3c42dbaa9f9d9fc40352efbc964503a9
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372318"
---
# <a name="navigationpage-bar-translucency-on-ios"></a>Навигатионпаже линейчатая полупрозрачность на iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS предназначена для изменения прозрачности панели навигации на [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и используется в XAML путем установки [`NavigationPage.IsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty) для присоединенного свойства `boolean` значения:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `NavigationPage.EnableTranslucentNavigationBar` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Навигатионпаже. Енаблетранслуцентнавигатионбар ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Навигатионпаже})). в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется метод, который делает панель навигации полупрозрачной. Кроме [`NavigationPage`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage) того, класс в `Xamarin.Forms.PlatformConfiguration.iOSSpecific` пространстве имен также имеет [ `DisableTranslucentNavigationBar` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Навигатионпаже. Дисаблетранслуцентнавигатионбар ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Навигатионпаже})) метод, который восстанавливает панель навигации до состояния по умолчанию и [ `SetIsNavigationBarTranslucent` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Навигатионпаже. Сетиснавигатионбартранслуцент ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Навигатионпаже}, System. Boolean)), который можно использовать для переключения прозрачности панели навигации путем вызова [ `IsNavigationBarTranslucent` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Навигатионпаже. Иснавигатионбартранслуцент ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Навигатионпаже})). метод:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

В результате прозрачность панели навигации может быть изменена:

![Полупрозрачная панель навигации Platform-Specific](navigation-bar-translucent-images/translucent-navigation-bar.png)

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)