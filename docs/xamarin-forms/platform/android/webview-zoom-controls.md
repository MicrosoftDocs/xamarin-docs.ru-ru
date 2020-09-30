---
title: WebView Zoom в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Android, которая позволяет увеличить масштаб WebView.
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1e41efd4bf100205f20b8f4e02a74fad193134a4
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556429"
---
# <a name="webview-zoom-on-android"></a>WebView Zoom в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа Android включает контроль сжатия и масштабирования для [`WebView`](xref:Xamarin.Forms.WebView) . Он используется в XAML путем задания `WebView.EnableZoomControls` `WebView.DisplayZoomControls` свойству и связываемым свойствам `boolean` значений:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

`WebView.EnableZoomControls`Свойство, доступное для привязки, определяет, включено ли сжатие сжатия в, а свойство, доступное для [`WebView`](xref:Xamarin.Forms.WebView) `WebView.DisplayZoomControls` привязки, определяет, будут ли элементы управления масштабом наложены на `WebView` .

В качестве альтернативы для платформы можно использовать C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

`WebView.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. `WebView.EnableZoomControls`Метод в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для управления включением сжатия в [`WebView`](xref:Xamarin.Forms.WebView) . `WebView.DisplayZoomControls`Метод в том же пространстве имен используется для управления тем, наложены ли элементы управления Zoom на `WebView` . Кроме того, `WebView.ZoomControlsEnabled` `WebView.ZoomControlsDisplayed` можно использовать методы и для возврата к включению элементов управления сжатием в масштабе и масштабом соответственно.

В результате можно включить сжатие сжатия в [`WebView`](xref:Xamarin.Forms.WebView) , и элементы управления масштабом можно заменять на `WebView` :

[![Снимок экрана с масштабным WebView в Android](webview-zoom-controls-images/webview-zoom.png "Масштабное WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "Масштабное WebView")

> [!IMPORTANT]
> Элементы управления масштабом должны быть включены и отображены с помощью соответствующих связываемых свойств или методов, которые будут наложены на [`WebView`](xref:Xamarin.Forms.WebView) .

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)