---
title: Панель навигации Флйоутпаже в Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая сворачивает панель навигации в Флйоутпаже.
ms.prod: xamarin
ms.assetid: 0E7436C9-FA3E-40CD-801C-3F7ED95C412D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e31a13daf5ff4b3e2c9fc185ddb3325f99406160
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940830"
---
# <a name="flyoutpage-navigation-bar-on-windows"></a>Панель навигации Флйоутпаже в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот универсальная платформа Windows для конкретной платформы используется для свертывания панели навигации в [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) и используется в XAML путем установки [`FlyoutPage.CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.FlyoutPage.CollapseStyleProperty) [`FlyoutPage.CollapsedPaneWidth`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.FlyoutPage.CollapsedPaneWidthProperty) свойств и присоединенные свойства:

```xaml
<FlyoutPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:FlyoutPage.CollapseStyle="Partial"
                  windows:FlyoutPage.CollapsedPaneWidth="48">
  ...
</FlyoutPage>

```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`FlyoutPage.On<Windows>`Метод указывает, что эта платформа будет запускаться только в Windows. [ `Page.SetCollapseStyle` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Флйоутпаже. Сетколлапсестиле ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Флйоутпаже}, Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Коллапсестиле)). в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для указания стиля свертывания с [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) перечислением, предоставляющим два значения: [`Full`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) и [`Partial`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial) . [ `FlyoutPage.CollapsedPaneWidth` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Флйоутпаже. Коллапседпаневидс ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Флйоутпаже}, System. Double)) используется для указания ширины частично свернутой панели навигации.

Результат заключается в том, что к [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) экземпляру применяется указанный атрибут, ширина которого также определяется:

[![Свернутая панель навигации, относящаяся к платформе](flyoutpage-navigation-bar-images/collapsed-navigation-bar.png)](flyoutpage-navigation-bar-images/collapsed-navigation-bar-large.png#lightbox "Свернутая панель навигации Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
