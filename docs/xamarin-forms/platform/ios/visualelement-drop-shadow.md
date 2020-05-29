---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7410386e10f605fdeed452fe37755c1e48e6b9b9
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136998"
---
# <a name="visualelement-drop-shadows-on-ios"></a>Висуалелемент тени в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS используется для включения тени на [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Он используется в XAML путем присвоения [`VisualElement.IsShadowEnabled`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsShadowEnabledProperty) присоединенному свойству значения `true` , а также ряда дополнительных вложенных свойств, управляющих тенью.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <BoxView ...
                 ios:VisualElement.IsShadowEnabled="true"
                 ios:VisualElement.ShadowColor="Purple"
                 ios:VisualElement.ShadowOpacity="0.7"
                 ios:VisualElement.ShadowRadius="12">
            <ios:VisualElement.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </ios:VisualElement.ShadowOffset>
         </BoxView>
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var boxView = new BoxView { Color = Color.Aqua, WidthRequest = 100, HeightRequest = 100 };
boxView.On<iOS>()
       .SetIsShadowEnabled(true)
       .SetShadowColor(Color.Purple)
       .SetShadowOffset(new Size(10,10))
       .SetShadowOpacity(0.7)
       .SetShadowRadius(12);
```

`VisualElement.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `VisualElement.SetIsShadowEnabled` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Сетисшадовенаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, включена ли в. тень тени `VisualElement` . Кроме того, можно вызвать следующие методы для управления тенью тени:

- [ `SetShadowColor` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Сетшадовколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент}, Xamarin.Forms . Цвет) — задает цвет тени. Цвет по умолчанию — [`Color.Default`](xref:Xamarin.Forms.Color.Default*) .
- [ `SetShadowOffset` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Сетшадовоффсет ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент}, Xamarin.Forms . Size)) — задает смещение тени. Смещение изменяет направление, в котором происходит приведение тени, и указывается как [`Size`](xref:Xamarin.Forms.Size) значение. `Size`Значения структуры выражаются в единицах, не зависящих от устройства, где первое значение равно расстоянию слева (отрицательное значение) или правому (положительное значение), а второе значение равно расстоянию выше (отрицательное значение) или ниже (положительное значение). Значение этого свойства по умолчанию равно (0,0, 0,0), что приводит к приведению тени вокруг каждой стороны `VisualElement` .
- [ `SetShadowOpacity` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. СетшадовопаЦити ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент}, System. Double)) — задает прозрачность тени со значением в диапазоне 0,0 (прозрачное) до 1,0 (непрозрачный). Значение прозрачности по умолчанию — 0,5.
- [ `SetShadowRadius` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Сетшадоврадиус ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент}, System. Double)) — задает радиус размытия, используемый для визуализации тени. Значение радиуса по умолчанию — 10,0.

> [!NOTE]
> Состояние тени можно запросить, вызвав [ `GetIsShadowEnabled` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Жетисшадовенаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент})), [ `GetShadowColor` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Жетшадовколор ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент})), [ `GetShadowOffset` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Жетшадовоффсет ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент})), [ `GetShadowOpacity` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. ЖетшадовопаЦити ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент})) и [ `GetShadowRadius` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Жетшадоврадиус ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент})).

В результате тень может быть включена в [`VisualElement`](xref:Xamarin.Forms.VisualElement) :

![](drop-shadow-images/drop-shadow.png "Drop shadow enabled")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
