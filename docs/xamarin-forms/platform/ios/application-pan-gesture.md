---
title: Одновременный распознавание жестов панорамирования в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая позволяет использовать одновременный распознавание жестов в приложении.
ms.prod: xamarin
ms.assetid: 883D89DA-F8FF-4B97-9C3F-2DD05C96A495
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f1baf5d71cc25a1f84ff683e9d0072f34715c0da
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373241"
---
# <a name="simultaneous-pan-gesture-recognition-on-ios"></a>Одновременный распознавание жестов панорамирования в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

При [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) присоединении к представлению в прокручиваемом представлении все жесты сдвига фиксируются `PanGestureRecognizer` и не передаются в представление прокрутки. Таким образом, представление прокрутки больше не будет прокручиваться.

Эта платформа iOS позволяет `PanGestureRecognizer` в представлении прокрутки захватывать и совместно использовать жест панорамирования с прокруткой. Он используется в XAML путем присвоения [`Application.PanGestureRecognizerShouldRecognizeSimultaneously`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) свойству присоединенного свойства значения `true` :

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

`Application.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Application. Сетпанжестуререкогнизершаулдрекогнизесимултанеаусли ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Приложение}, System. Boolean). в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, будет ли распознаватель жестов сдвига в прокручиваемом представлении захватывать жест панорамирования или захватывать и совместно использовать жест панорамирования с прокруткой. Кроме того, [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Application. Жетпанжестуререкогнизершаулдрекогнизесимултанеаусли ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Application})) можно использовать для возвращения того, является ли жест панорамирования общим для представления прокрутки, содержащего [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) .

Таким образом, если эта платформа включена, то когда объект [`ListView`](xref:Xamarin.Forms.ListView) содержит [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) , `ListView` и, и, `PanGestureRecognizer` получит жест панорамирования и обработает его. Однако если эта платформа отключена, то, когда `ListView` содержит, объект `PanGestureRecognizer` `PanGestureRecognizer` захватывает жест панорамирования и обрабатывает его, а элемент `ListView` не получает жест панорамирования.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)