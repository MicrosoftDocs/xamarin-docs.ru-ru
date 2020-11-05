---
title: Размер шрифта записи в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS для масштабирования размера шрифта записи.
ms.prod: xamarin
ms.assetid: E8881D4E-902B-4397-A43E-916B2885EC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a3f5b920a3717bb70454eb16a5f830e6472413eb
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373189"
---
# <a name="entry-font-size-on-ios"></a>Размер шрифта записи в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS предназначена для масштабирования размера шрифта, [`Entry`](xref:Xamarin.Forms.Entry) чтобы гарантировать, что выводимый текст умещается в элемент управления. Он используется в XAML путем присвоения [`Entry.AdjustsFontSizeToFitWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty) свойству присоединенного свойства `boolean` значения:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `Entry.EnableAdjustsFontSizeToFitWidth` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Entry. Енаблеаджустсфонтсизетофитвидс ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Entry})). в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для масштабирования размера шрифта выведенного текста, чтобы убедиться, что он умещается в [`Entry`](xref:Xamarin.Forms.Entry) . Кроме [`Entry`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry) того, класс в `Xamarin.Forms.PlatformConfiguration.iOSSpecific` пространстве имен также имеет [ `DisableAdjustsFontSizeToFitWidth` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Entry. Дисаблеаджустсфонтсизетофитвидс ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Entry})), который отключает данную платформу, и [ `SetAdjustsFontSizeToFitWidth` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Entry. Сетаджустсфонтсизетофитвидс ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Элемент}, System. Boolean)), который можно использовать для переключения масштабирования размера шрифта путем вызова [ `AdjustsFontSizeToFitWidth` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Entry. Аджустсфонтсизетофитвидс ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Entry})), метод:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

В результате размер шрифта для масштабируется так, чтобы выводимый [`Entry`](xref:Xamarin.Forms.Entry) текст поместился в элемент управления:

![Изменить размер вводимого шрифта Platform-Specific](entry-font-size-images/entry-font-size.png)

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)