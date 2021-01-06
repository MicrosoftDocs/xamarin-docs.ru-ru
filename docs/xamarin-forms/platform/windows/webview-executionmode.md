---
title: Режим выполнения WebView в Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая задает поток, в котором WebView размещает свое содержимое.
ms.prod: xamarin
ms.assetid: 4D3C4112-0FF6-47F8-BC22-579D92032807
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 314851c4697e95ef8662710c986d0b175249fad6
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940843"
---
# <a name="webview-execution-mode-on-windows"></a>Режим выполнения WebView в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа устанавливает поток, в котором [`WebView`](xref:Xamarin.Forms.WebView) размещается его содержимое. Он используется в XAML путем установки `WebView.ExecutionMode` Свойства BIND в `WebViewExecutionMode` значение перечисления:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.ExecutionMode="SeparateThread" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

WebView webView = new Xamarin.Forms.WebView();
webView.On<Windows>().SetExecutionMode(WebViewExecutionMode.SeparateThread);
```

`WebView.On<Windows>`Метод указывает, что данная платформа будет запускаться только на универсальная платформа Windows. `WebView.SetExecutionMode`Метод в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для задания потока, в котором `WebView` размещается его содержимое, с `WebViewExecutionMode` перечислением, предоставляющим три возможных значения:

- `SameThread` Указывает, что содержимое размещается в потоке пользовательского интерфейса. Это значение по умолчанию для в `WebView` Windows.
- `SeparateThread` Указывает, что содержимое размещается в фоновом потоке.
- `SeparateProcess` Указывает, что содержимое размещается в отдельном процессе в процессе приложения. Для каждого экземпляра WebView не существует отдельного процесса, поэтому все экземпляры WebView приложения используют один и тот же отдельный процесс.

Кроме того, `GetExecutionMode` метод можно использовать для возврата текущего `WebViewExecutionMode` значения для `WebView` .

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
