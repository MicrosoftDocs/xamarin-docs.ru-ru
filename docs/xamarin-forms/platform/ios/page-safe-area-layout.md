---
title: Структура безопасного макета области в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая обеспечивает расположение содержимого страницы в области экрана, которая является надежной для всех устройств, использующих iOS 11 и более поздние версии.
ms.prod: xamarin
ms.assetid: 2B6789C1-39B4-4C16-ADE1-3ED3378EAC63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 920246e9cbbe85c606969333ccb05d3c87dcef66
ms.sourcegitcommit: 14d67a2db82e67471584b1749e0d5b9ec0c0c09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2020
ms.locfileid: "88228590"
---
# <a name="safe-area-layout-guide-on-ios"></a>Структура безопасного макета области в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS предназначена для того, чтобы обеспечить расположение содержимого страницы в области экрана, которая является надежной для всех устройств, использующих iOS 11 и более поздних версий. В частности, это поможет убедиться, что содержимое не обрезается углами скругленных устройств, индикатором дома или корпусом датчика на iPhone X. Он используется в XAML путем присвоения `Page.UseSafeArea` свойству присоединенного свойства `boolean` значения:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `Page.SetUseSafeArea`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен определяет, включена ли структура защищенной области.

В результате содержимое страницы может быть размещено в области экрана, которая является надежной для всех iPhone:

[![Направляющая макета безопасной области](page-safe-area-images/safe-area-layout.png)](page-safe-area-images/safe-area-layout-large.png#lightbox "Направляющая макета безопасной области")

> [!NOTE]
> Защищенная область, определенная Apple, используется в Xamarin.Forms для задания [`Page.Padding`](xref:Xamarin.Forms.Page.Padding) Свойства и переопределяет все предыдущие значения этого свойства, которые были заданы.

Защищенную область можно настроить, извлекая ее [`Thickness`](xref:Xamarin.Forms.Thickness) значение с помощью `Page.SafeAreaInsets` метода из [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен. Затем его можно изменить по мере необходимости и повторно назначить `Padding` свойству в [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) переопределении:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
