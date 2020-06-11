---
Title: "WebViewные оповещения JavaScript в Windows" Описание: "особенности платформы позволяют использовать функции, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу Windows, которая позволяет WebView отображать оповещения JavaScript в диалоговом окне сообщения UWP.
MS. произв. Xamarin MS. AssetID: 95A153A1-72A0-4C0B-A452-ACE966BB12CB MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 10/24/2018 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="webview-javascript-alerts-on-windows"></a>WebView оповещений JavaScript в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа позволяет [`WebView`](xref:Xamarin.Forms.WebView) Отображать оповещения JavaScript в диалоговом окне сообщения UWP. Он используется в XAML путем присвоения [`WebView.IsJavaScriptAlertEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) свойству присоединенного свойства `boolean` значения:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

`WebView.On<Windows>`Метод указывает, что данная платформа будет запускаться только на универсальная платформа Windows. [ `WebView.SetIsJavaScriptAlertEnabled` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. WebView. Сетисжаваскрипталертенаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . WebView}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для управления включением предупреждений JavaScript. Кроме того, `WebView.SetIsJavaScriptAlertEnabled` метод можно использовать для переключения предупреждений JavaScript путем вызова [`IsJavaScriptAlertEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) метода для возврата того, включены ли они:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

В результате оповещения JavaScript могут отображаться в диалоговом окне сообщения UWP:

![WebView для платформы предупреждений JavaScript](webview-javascript-alert-images/webview-javascript-alert.png "WebView для платформы предупреждений JavaScript")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
