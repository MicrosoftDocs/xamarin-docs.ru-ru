---
title: Тень Флйоутпаже на iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая определяет, применена ли к странице сведений Флйоутпаже теневая копия при раскрытии всплывающей страницы.
ms.prod: xamarin
ms.assetid: FB907EA2-C00A-43A5-84B1-A43584C867A5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0b05cb37a2399f438b9003e39341feb138bce490
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940820"
---
# <a name="flyoutpage-shadow-on-ios"></a>Тень Флйоутпаже на iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа управляет тем [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) , применяется ли к ней теневая страница сведений при раскрытии всплывающей страницы. Он используется в XAML путем установки `FlyoutPage.ApplyShadow` Свойства BIND в значение `true` :

```xaml
<FlyoutPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                  ios:FlyoutPage.ApplyShadow="true">
    ...
</FlyoutPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSFlyoutPageCS : FlyoutPage
{
    public iOSFlyoutPageCS(ICommand restore)
    {
        On<iOS>().SetApplyShadow(true);
        // ...
    }
}
```

`FlyoutPage.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `FlyoutPage.SetApplyShadow`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, применена ли к странице детализации к [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) ней теневая страница при раскрытии всплывающей страницы. Кроме того, `GetApplyShadow` метод можно использовать для определения того, применяется ли тень к странице сведений `FlyoutPage` .

В результате на страницу детализации [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) может быть применена теневая копия при раскрытии всплывающей страницы:

[![Снимок экрана Флйоутпаже с тенью и без нее](flyoutpage-shadow-images/shadow.png "Флйоутпаже с тенью и без нее")](flyoutpage-shadow-images/shadow-large.png#lightbox "Флйоутпаже с тенью и без нее")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
