---
title: Таббедпаже прокрутка страниц на Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая позволяет выполнять прокрутку с помощью жеста горизонтального пальца между страницами в Таббедпаже.
ms.prod: xamarin
ms.assetid: D1C09CCB-7246-41A4-8BD2-FA6FABCF1C72
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 61c0137788303363769fdec80a16542e2d8bea5e
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128613"
---
# <a name="tabbedpage-page-swiping-on-android"></a>Таббедпаже прокрутка страниц на Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android используется для включения прокрутки с помощью жеста горизонтального пальца между страницами в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Он используется в XAML путем присвоения [`TabbedPage.IsSwipePagingEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) свойству присоединенного свойства `boolean` значения:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. [ `TabbedPage.SetIsSwipePagingEnabled` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Сетиссвипепагинженаблед ( Xamarin.Forms . BindableObject, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для включения прокрутки между страницами в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Кроме `TabbedPage` того, класс в `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` пространстве имен также имеет [ `EnableSwipePaging` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Енаблесвипепагинг ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Таббедпаже})), который включает этот метод для конкретной платформы и [ `DisableSwipePaging` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Дисаблесвипепагинг ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Таббедпаже})). Этот метод отключает данную платформу. [`TabbedPage.OffscreenPageLimit`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty)Присоединенное свойство и [ `SetOffscreenPageLimit` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Сетоффскринпажелимит ( Xamarin.Forms . BindableObject, System. Int32)) используются для задания числа страниц, которые должны храниться в состоянии простоя с любой стороны текущей страницы.

Результат заключается в том, что проведите разбиение по страницам, отображаемым объектом, [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) включенным.

![](tabbedpage-page-swiping-images/tabbedpage-swipe.png)

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
