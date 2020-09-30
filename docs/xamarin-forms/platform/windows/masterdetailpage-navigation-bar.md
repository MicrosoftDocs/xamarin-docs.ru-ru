---
title: Панель навигации Мастердетаилпаже в Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая сворачивает панель навигации в Мастердетаилпаже.
ms.prod: xamarin
ms.assetid: 0E7436C9-FA3E-40CD-801C-3F7ED95C412D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e6fac17f28703ec7254958e19bff7132bee6b78a
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91555727"
---
# <a name="masterdetailpage-navigation-bar-on-windows"></a>Панель навигации Мастердетаилпаже в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот универсальная платформа Windows для конкретной платформы используется для свертывания панели навигации в [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) и используется в XAML путем установки [`MasterDetailPage.CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) [`MasterDetailPage.CollapsedPaneWidth`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty) свойств и присоединенные свойства:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>`Метод указывает, что эта платформа будет запускаться только в Windows. [ `Page.SetCollapseStyle` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Мастердетаилпаже. Сетколлапсестиле ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Мастердетаилпаже}, Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Коллапсестиле)). в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для указания стиля свертывания с [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) перечислением, предоставляющим два значения: [`Full`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) и [`Partial`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial) . [ `MasterDetailPage.CollapsedPaneWidth` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Мастердетаилпаже. Коллапседпаневидс ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Мастердетаилпаже}, System. Double)) используется для указания ширины частично свернутой панели навигации.

Результат заключается в том, что к [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) экземпляру применяется указанный атрибут, ширина которого также определяется:

[![Свернутая панель навигации, относящаяся к платформе](masterdetailpage-navigation-bar-images/collapsed-navigation-bar.png)](masterdetailpage-navigation-bar-images/collapsed-navigation-bar-large.png#lightbox "Свернутая панель навигации, относящаяся к платформе")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)