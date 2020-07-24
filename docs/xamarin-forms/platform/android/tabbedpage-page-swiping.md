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
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 0043d90e631c19a55b766a877a9cd30316f14650
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997388"
---
# <a name="tabbedpage-page-swiping-on-android"></a>Таббедпаже прокрутка страниц на Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android используется для включения прокрутки с помощью жеста горизонтального пальца между страницами в [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) . Он используется в XAML путем присвоения [`TabbedPage.IsSwipePagingEnabled`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) свойству присоединенного свойства `boolean` значения:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;assembly=:::no-loc(Xamarin.Forms):::.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration;
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. [ `TabbedPage.SetIsSwipePagingEnabled` ] (Xref: :::no-loc(Xamarin.Forms)::: . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Сетиссвипепагинженаблед ( :::no-loc(Xamarin.Forms)::: . BindableObject, System. Boolean)) в [`:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific) пространстве имен используется для включения прокрутки между страницами в [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) . Кроме `TabbedPage` того, класс в `:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific` пространстве имен также имеет [ `EnableSwipePaging` ] (xref: :::no-loc(Xamarin.Forms)::: . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Енаблесвипепагинг ( :::no-loc(Xamarin.Forms)::: . Иплатформелементконфигуратион { :::no-loc(Xamarin.Forms)::: . Платформконфигуратион. Android, :::no-loc(Xamarin.Forms)::: . Таббедпаже})), который включает этот метод для конкретной платформы и [ `DisableSwipePaging` ] (xref: :::no-loc(Xamarin.Forms)::: . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Дисаблесвипепагинг ( :::no-loc(Xamarin.Forms)::: . Иплатформелементконфигуратион { :::no-loc(Xamarin.Forms)::: . Платформконфигуратион. Android, :::no-loc(Xamarin.Forms)::: . Таббедпаже})). Этот метод отключает данную платформу. [`TabbedPage.OffscreenPageLimit`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty)Присоединенное свойство и [ `SetOffscreenPageLimit` ] (xref: :::no-loc(Xamarin.Forms)::: . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Сетоффскринпажелимит ( :::no-loc(Xamarin.Forms)::: . BindableObject, System. Int32)) используются для задания числа страниц, которые должны храниться в состоянии простоя с любой стороны текущей страницы.

Результат заключается в том, что проведите разбиение по страницам, отображаемым объектом, [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) включенным.

![Проведите разбиение по страницам Таббедпаже](tabbedpage-page-swiping-images/tabbedpage-swipe.png)

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.AppCompat)
