---
title: Бегунок ползунка в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая позволяет задать свойство Slider. Value, коснувшись ползунка.
ms.prod: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 66fd7fcd5accbc1b6b1595fcb8ee7b1f8eb8d6d4
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370745"
---
# <a name="slider-thumb-tap-on-ios"></a>Бегунок ползунка в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS позволяет [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) задать свойство, коснувшись расположения на [`Slider`](xref:Xamarin.Forms.Slider) панели, а не путем перетаскивания `Slider` бегунка. Он используется в XAML путем установки [`Slider.UpdateOnTap`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) Свойства BIND в значение `true` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

`Slider.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `Slider.SetUpdateOnTap` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Slider. Сетупдатеонтап ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Ползунок}, System. Boolean). в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, будет ли значение свойства откасанием на `Slider` панели задается [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) . Кроме того, [ `Slider.GetUpdateOnTap` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Slider. Жетупдатеонтап ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . ). Можно использовать метод, чтобы возвращать значение, указывающее, задаст ли касание на `Slider` панели `Slider.Value` свойство.

В результате касание на [`Slider`](xref:Xamarin.Forms.Slider) полосе может переместить `Slider` бегунок и установить [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) свойство:

![Ползунок обновляется при нажатии кнопки "включено"](slider-thumb-images/slider-updateontap.png)

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)