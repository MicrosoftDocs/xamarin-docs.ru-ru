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
ms.openlocfilehash: 9264bdf4ab5644b1cdfa0c37f1c7cacd3ae4ed0a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128340"
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

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
