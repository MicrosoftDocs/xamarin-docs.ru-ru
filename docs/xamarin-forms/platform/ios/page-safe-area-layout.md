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
ms.openlocfilehash: 5ca30481fbc0e5631ff75000c688dd805793e670
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128067"
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

[![](page-safe-area-images/safe-area-layout.png "Safe Area Layout Guide")](page-safe-area-images/safe-area-layout-large.png#lightbox "Safe Area Layout Guide")

> [!NOTE]
> Защищенная область, определенная Apple, используется в Xamarin.Forms для задания [`Page.Padding`](xref:Xamarin.Forms.Page.Padding) Свойства и переопределяет все предыдущие значения этого свойства, которые были заданы.

Защищенную область можно настроить, извлекая ее [`Thickness`](xref:Xamarin.Forms.Thickness) значение с помощью `Page.SafeAreaInsets` метода из [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен. Затем его можно изменить по мере необходимости и повторно назначить `Padding` свойству в конструкторе страницы или [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) переопределить:

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
