---
title: Размещение и цвет панели инструментов Таббедпаже в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, которая задает размещение и цвет панели инструментов в Таббедпаже.
ms.prod: xamarin
ms.assetid: A5C68D6A-9A5F-42EE-845D-1E5B0CB1544E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f5a2be4bd61056a42593fc45e1abdd3679795bc0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139988"
---
# <a name="tabbedpage-toolbar-placement-and-color-on-android"></a>Размещение и цвет панели инструментов Таббедпаже в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

> [!IMPORTANT]
> Особенности платформы, которые задают цвет панели инструментов в, [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) теперь устарели и заменены [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor) [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor) свойствами и. Дополнительные сведения см. [в разделе Создание таббедпаже](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md#create-a-tabbedpage).

Эти особенности платформы используются для задания расположения и цвета панели инструментов в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Они используются в XAML путем установки [`TabbedPage.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) присоединенного свойства в значение [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) перечисления, а [`TabbedPage.BarItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) [`TabbedPage.BarSelectedItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) вложенные свойства и присоединяются к [`Color`](xref:Xamarin.Forms.Color) :

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

Кроме того, их можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

`TabbedPage.On<Android>`Метод указывает, что эти особенности платформы будут выполняться только в Android. [ `TabbedPage.SetToolbarPlacement` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Сеттулбарплацемент ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Таббедпаже}, Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Тулбарплацемент)). в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для задания расположения панели инструментов в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) , а [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) перечисление предоставляет следующие значения:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default)— Указывает, что панель инструментов помещается в расположение по умолчанию на странице. Это верхняя часть страницы на телефонах, а нижняя часть страницы — на других идиомах устройств.
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top)— Указывает, что панель инструментов размещается в верхней части страницы.
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom)— Указывает, что панель инструментов размещается в нижней части страницы.

Кроме того, [ `TabbedPage.SetBarItemColor` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Сетбаритемколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Таббедпаже}, Xamarin.Forms . Color)) и [ `TabbedPage.SetBarSelectedItemColor` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Сетбарселектедитемколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Таббедпаже}, Xamarin.Forms . Color). методы используются для задания цвета элементов панели инструментов и выбранных элементов панели инструментов соответственно.

> [!NOTE]
> [ `GetToolbarPlacement` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Жеттулбарплацемент ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Таббедпаже})), [ `GetBarItemColor` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Жетбаритемколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Таббедпаже})) и [ `GetBarSelectedItemColor` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Таббедпаже. Жетбарселектедитемколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Таббедпаже})). методы можно использовать для получения расположения и цвета [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) панели инструментов.

Результат заключается в том, что размещение панели инструментов, цвет элементов панели инструментов и цвет выбранного элемента панели инструментов можно задать в [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) :

![](tabbedpage-toolbar-placement-color-images/tabbedpage-toolbar-placement.png)

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
