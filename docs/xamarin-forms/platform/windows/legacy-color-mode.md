---
title: Устаревший цветовой режим Висуалелемент в Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая отключает Xamarin.Forms цветовой режим прежних версий.
ms.prod: xamarin
ms.assetid: B8759309-07C7-4DCA-A18A-C1A198A7951B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3bcbee04edbeebf3949673a53edde4200856f5c6
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84132115"
---
# <a name="visualelement-legacy-color-mode-on-windows"></a>Устаревший цветовой режим Висуалелемент в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Некоторые Xamarin.Forms представления имеют устаревший цветовой режим. В этом режиме, если [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) свойство представления имеет значение `false` , представление переопределит цвета, заданные пользователем, с помощью собственных цветов по умолчанию для отключенного состояния. Для обеспечения обратной совместимости этот стандартный цветовой режим по умолчанию для поддерживаемых представлений остается прежним.

Этот универсальная платформа Windows, зависящий от платформы, отключает этот устаревший цветовой режим, чтобы цвета, заданные пользователем в представлении, оставались даже при отключенном представлении. Он используется в XAML путем присвоения [`VisualElement.IsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) свойству присоединенного свойства значения `false` :

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>`Метод указывает, что эта платформа будет запускаться только в Windows. [ `VisualElement.SetIsLegacyColorModeEnabled` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Сетислегациколормодинаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для управления тем, отключен ли устаревший цветовой режим. Кроме того, [ `VisualElement.GetIsLegacyColorModeEnabled` ] (xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Висуалелемент. Жетислегациколормодинаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Висуалелемент})). можно использовать метод, чтобы возвращать, отключен ли устаревший цветовой режим.

В результате можно отключить устаревший цветовой режим, чтобы цвета, заданные для представления пользователем, оставались даже при отключенном представлении:

![](legacy-color-mode-images/legacy-color-mode-disabled.png "Legacy color mode disabled")

> [!NOTE]
> При задании [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) для представления устаревший цветовой режим не учитывается полностью. Дополнительные сведения о визуальных состояниях см. [в разделе Xamarin.Forms Диспетчер визуальных состояний](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
