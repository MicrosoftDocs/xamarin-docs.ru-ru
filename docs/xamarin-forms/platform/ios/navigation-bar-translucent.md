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
ms.openlocfilehash: be7e95df0ac247c5cd97c1715e4af67b48275180
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128119"
---
# <a name="navigationpage-bar-translucency-on-ios"></a>Навигатионпаже линейчатая полупрозрачность на iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

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

![](navigation-bar-translucent-images/translucent-navigation-bar.png "Translucent Navigation Bar Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
