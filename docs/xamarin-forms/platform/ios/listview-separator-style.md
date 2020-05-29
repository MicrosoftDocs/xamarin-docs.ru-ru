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
ms.openlocfilehash: c8d660896684283ba9b40cde168adbfe30ca0c51
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135997"
---
# <a name="listview-separator-style-on-ios"></a>Стиль разделителя ListView в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS определяет, использует ли разделитель ячеек в пределах [`ListView`](xref:Xamarin.Forms.ListView) всей ширины `ListView` . Он используется в XAML путем присвоения [`ListView.SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) свойству присоединенного свойства значения [`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

`ListView.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `ListView.SetSeparatorStyle` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. ListView. Сетсепараторстиле ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . ListView}, Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Сепараторстиле)). в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, используется ли в качестве разделителя ячеек [`ListView`](xref:Xamarin.Forms.ListView) полная ширина `ListView` , с [`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) перечислением, предоставляющим два возможных значения:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default)— Указывает поведение разделителя iOS по умолчанию. Это поведение по умолчанию в Xamarin.Forms .
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth)— Указывает, что разделители будут отображаться от одного края `ListView` до другого.

В результате заданное [`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) значение применяется к элементу [`ListView`](xref:Xamarin.Forms.ListView) , который управляет шириной разделителя ячеек.

![](listview-separator-style-images/listview-separatorstyle.png "ListView SeparatorStyle Platform-Specific")

> [!NOTE]
> После того как для стиля разделителя задано значение `FullWidth` , его нельзя изменить обратно `Default` во время выполнения.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
