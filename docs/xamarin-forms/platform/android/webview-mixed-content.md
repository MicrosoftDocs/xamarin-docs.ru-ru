---
title: WebView смешанное содержимое в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать зависящую от платформы Android платформу, которая отображает смешанное содержимое в WebView в приложениях, предназначенных для API 21 или более поздней версии.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 27d19773d86c125cd5574f7a281c37c26ef643fd
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563202"
---
# <a name="webview-mixed-content-on-android"></a>WebView смешанное содержимое в Android

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления для платформы Android определяет, [`WebView`](xref:Xamarin.Forms.WebView) может ли объект отображать смешанное содержимое в приложениях, предназначенных для API 21 или более поздней версии. Смешанное содержимое — это содержимое, которое изначально загружается через HTTPS-соединение, но при этом ресурсы (например, изображения, аудио, видео, таблицы стилей и сценарии) загружаются по протоколу HTTP. Он используется в XAML путем присвоения [`WebView.MixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) свойству присоединенного свойства значения [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) перечисления:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>`Метод указывает, что эта платформа будет запускаться только в Android. [ `WebView.SetMixedContentMode` ] (Xref: Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. WebView. Сетмикседконтентмоде ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Android, Xamarin.Forms . WebView}, Xamarin.Forms . Платформконфигуратион. АндроидспеЦифик. Микседконтенсандлинг)). в [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространстве имен используется для управления отображением смешанного содержимого, при этом [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) перечисление предоставляет три возможных значения:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) — Указывает, что [`WebView`](xref:Xamarin.Forms.WebView) разрешает источнику HTTPS загружать содержимое из источника HTTP.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) — Указывает, что [`WebView`](xref:Xamarin.Forms.WebView) не будет разрешать источнику HTTPS загружать содержимое из источника HTTP.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) — Указывает, что [`WebView`](xref:Xamarin.Forms.WebView) будет пытаться обеспечить совместимость с подходом к последнему веб-браузеру устройства. Может быть разрешено загружать часть содержимого HTTP с помощью HTTPS-источника, а другие типы содержимого будут заблокированы. Блокируемые или разрешенные типы содержимого могут изменяться при каждом выпуске операционной системы.

В результате заданное [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) значение применяется к элементу [`WebView`](xref:Xamarin.Forms.WebView) , который определяет, можно ли отобразить смешанное содержимое:

[![WebView смешанное содержимое, зависящее от платформы](webview-mixed-content-images/webview-mixedcontent.png "WebView смешанное содержимое, зависящее от платформы")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView смешанное содержимое, зависящее от платформы")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API АндроидспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [АндроидспеЦифик. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)