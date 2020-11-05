---
title: Быстрая прокрутка ListView в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая обеспечивает быструю прокрутку данных в ListView.
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 38a59c2e468873d0beb632b601ee59d8c94bf716
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367118"
---
# <a name="listview-fast-scrolling-on-android"></a>Быстрая прокрутка ListView в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа для Android используется для быстрой прокрутки данных в [`ListView`](xref:Xamarin.Forms.ListView) . Он используется в XAML путем присвоения `ListView.IsFastScrollEnabled` свойству присоединенного свойства `boolean` значения:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. `ListView.SetIsFastScrollEnabled`Метод в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется, чтобы обеспечить быструю прокрутку данных в [`ListView`](xref:Xamarin.Forms.ListView) . Кроме того, `SetIsFastScrollEnabled` метод можно использовать для переключения быстрой прокрутки путем вызова `IsFastScrollEnabled` метода для возврата, включена ли Быстрая прокрутка.

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

В результате можно включить быструю прокрутку данных в [`ListView`](xref:Xamarin.Forms.ListView) , что изменяет размер бегунка прокрутки.

[![ListView Фастскролл для конкретной платформы](listview-fast-scrolling-images/fastscroll.png)](listview-fast-scrolling-images/fastscroll-large.png#lightbox "Platform-Specific ListView Фастскролл")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)