---
title: Устаревший цветовой режим Висуалелемент в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая отключает Xamarin.Forms цветовой режим прежних версий.
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0e4300354588e5055e58251caa547a56c751fd94
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936075"
---
# <a name="visualelement-legacy-color-mode-on-android"></a>Устаревший цветовой режим Висуалелемент в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Некоторые Xamarin.Forms представления имеют устаревший цветовой режим. В этом режиме, если [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) свойство представления имеет значение `false` , представление переопределит цвета, заданные пользователем, с помощью собственных цветов по умолчанию для отключенного состояния. Для обеспечения обратной совместимости этот стандартный цветовой режим по умолчанию для поддерживаемых представлений остается прежним.

Эта платформа Android — отключается от этого устаревшего цветового режима, поэтому цвета, заданные пользователем в представлении, остаются даже в том случае, если представление отключено. Он используется в XAML путем присвоения [`VisualElement.IsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) свойству присоединенного свойства значения `false` :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. [ `VisualElement.SetIsLegacyColorModeEnabled` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Висуалелемент. Сетислегациколормодинаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Висуалелемент}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для управления тем, отключен ли устаревший цветовой режим. Кроме того, [ `VisualElement.GetIsLegacyColorModeEnabled` ] (xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Висуалелемент. Жетислегациколормодинаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . Висуалелемент})). можно использовать метод, чтобы возвращать, отключен ли устаревший цветовой режим.

В результате можно отключить устаревший цветовой режим, чтобы цвета, заданные для представления пользователем, оставались даже при отключенном представлении:

![Устаревший режим цвета отключен](legacy-color-mode-images/legacy-color-mode-disabled.png)

> [!NOTE]
> При задании [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) для представления устаревший цветовой режим не учитывается полностью. Дополнительные сведения о визуальных состояниях см. [в разделе Xamarin.Forms Диспетчер визуальных состояний](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
