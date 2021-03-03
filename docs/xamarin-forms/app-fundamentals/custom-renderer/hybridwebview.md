---
title: Настройка WebView
description: Xamarin.Forms WebView — это представление, позволяющее отображать веб-содержимое и содержимое HTML в приложении. В этой статье рассказывается, как создать пользовательский отрисовщик, расширяющий возможности WebView и позволяющий вызывать код C# из JavaScript.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/31/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: dcfb713da09a9dab1974110e161d8045132f6b2f
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375022"
---
# <a name="customizing-a-webview"></a>Настройка WebView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/customrenderers-hybridwebview)

_Xamarin.Forms `WebView` — это представление, позволяющее отображать веб-содержимое и содержимое HTML в приложении. В этой статье рассказывается, как создать пользовательский отрисовщик, расширяющий возможности `WebView` и позволяющий вызывать код C# из JavaScript._

Каждое представление Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления. Когда [`WebView`](xref:Xamarin.Forms.WebView) отображается в приложении Xamarin.Forms, на платформе iOS создается класс `WkWebViewRenderer`, который, в свою очередь, создает собственный элемент управления `WkWebView`. На платформе Android класс `WebViewRenderer` создает собственный элемент управления `WebView`. На универсальной платформе Windows (UWP) класс `WebViewRenderer` создает экземпляр собственного элемента управления `WebView`. Дополнительные сведения об отрисовщике и классах собственных элементов управления, с которыми сопоставляются элементы управления Xamarin.Forms, см. в статье [Базовые классы и собственные элементы управления отрисовщика](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между классом [`View`](xref:Xamarin.Forms.View) и соответствующими собственными элементами управления, которые его реализуют:

![Связь между классом WebView и его реализующими собственными классами](hybridwebview-images/webview-classes.png)

Процесс отрисовки можно использовать для реализации настроек для конкретных платформ путем создания пользовательского отрисовщика для [`WebView`](xref:Xamarin.Forms.WebView) на каждой платформе. Этот процесс выглядит следующим образом:

1. [Создание](#create-the-hybridwebview) пользовательского элемента управления `HybridWebView`.
1. [Использование](#consume-the-hybridwebview) `HybridWebView`из Xamarin.Forms.
1. [Создание](#create-the-custom-renderer-on-each-platform) настраиваемого отрисовщика для `HybridWebView` на каждой платформе.

Мы рассмотрим каждый элемент по очереди, чтобы реализовать отрисовщик `HybridWebView`, который расширяет возможности элемента Xamarin.Forms [`WebView`](xref:Xamarin.Forms.WebView) для вызова кода C# из JavaScript. Экземпляр `HybridWebView` будет использоваться для отображения HTML-страницы, которая предлагает пользователю ввести свое имя. Затем, когда пользователь нажимает кнопку HTML, функция JavaScript вызывает C# `Action`, который отображает всплывающее окно, содержащее имя пользователя.

Дополнительные сведения о процессе вызова C# из JavaScript см. в разделе [Вызов C# из JavaScript](#invoke-c-from-javascript). Дополнительные сведения о странице HTML см. в разделе [Создание веб-страницы](#create-the-web-page).

> [!NOTE]
> [`WebView`](xref:Xamarin.Forms.WebView) может вызывать функцию JavaScript из C# и возвращать любой результат в вызывающий код C#. Дополнительные сведения см. в разделе [Вызов JavaScript](~/xamarin-forms/user-interface/webview.md#invoking-javascript).

## <a name="create-the-hybridwebview"></a>Создание HybridWebView

Пользовательский элемент управления `HybridWebView` можно создать путем создания подкласса класса [`WebView`](xref:Xamarin.Forms.WebView):

```csharp
public class HybridWebView : WebView
{
    Action<string> action;

    public static readonly BindableProperty UriProperty = BindableProperty.Create(
        propertyName: "Uri",
        returnType: typeof(string),
        declaringType: typeof(HybridWebView),
        defaultValue: default(string));

    public string Uri
    {
        get { return (string)GetValue(UriProperty); }
        set { SetValue(UriProperty, value); }
    }

    public void RegisterAction(Action<string> callback)
    {
        action = callback;
    }

    public void Cleanup()
    {
        action = null;
    }

    public void InvokeAction(string data)
    {
        if (action == null || data == null)
        {
            return;
        }
        action.Invoke(data);
    }
}
```

Пользовательский элемент управления `HybridWebView` создается в проекте библиотеки .NET Standard и определяет следующий API для элемента управления:

- Свойство `Uri`, которое указывает адрес веб-страницы для загрузки.
- Метод `RegisterAction`, который регистрирует `Action` с элементом управления. Зарегистрированное действие будет вызываться из JavaScript, содержащегося в файле HTML, на который можно сослаться через свойство `Uri`.
- Метод `CleanUp`, который удаляет ссылку на зарегистрированный объект `Action`.
- Метод `InvokeAction`, который вызывает зарегистрированный объект `Action`. Этот метод будет вызываться из пользовательского отрисовщика в проекте для каждой платформы.

## <a name="consume-the-hybridwebview"></a>Использование HybridWebView

На пользовательский элемент управления `HybridWebView` можно ссылаться в XAML в проекте библиотеки .NET Standard, объявив пространство имен для его расположения и используя префикс пространства имен в пользовательском элементе управления. В следующем примере кода показано, как пользовательский элемент управления `HybridWebView` может использоваться страницей XAML.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,40,0,0">
    <local:HybridWebView x:Name="hybridWebView"
                         Uri="index.html" />
</ContentPage>

```

Префикс пространства имен `local` может иметь любое имя. Тем не менее значения `clr-namespace` и `assembly` должны соответствовать данным пользовательского элемента управления. После объявления пространства имен префикс используется для ссылки на пользовательский элемент управления.

В следующем примере кода показано, как пользовательский элемент управления `HybridWebView` может использоваться страницей C#.

```csharp
public HybridWebViewPageCS()
{
    var hybridWebView = new HybridWebView
    {
        Uri = "index.html"
    };
    // ...
    Padding = new Thickness(0, 40, 0, 0);
    Content = hybridWebView;
}
```

Экземпляр `HybridWebView` будет использоваться для отображения собственного веб-элемента управления на каждой платформе. Для его свойства `Uri` задан HTML-файл, который хранится в проекте для каждой платформы и будет отображаться собственным веб-элементом управления. Отрисованный HTML просит пользователя ввести свое имя с помощью JavaScript, вызывая C# `Action` в ответ на нажатие кнопки HTML.

`HybridWebViewPage` регистрирует действие, которое необходимо вызывать из JavaScript, как показано в следующем примере кода:

```csharp
public partial class HybridWebViewPage : ContentPage
{
    public HybridWebViewPage()
    {
        // ...
        hybridWebView.RegisterAction(data => DisplayAlert("Alert", "Hello " + data, "OK"));
    }
}
```

Это действие вызывает метод [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) для отображения модального всплывающего окна, которое представляет имя, введенное на странице HTML, отображаемой экземпляром `HybridWebView`.

Настраиваемый отрисовщик теперь можно добавить в каждый проект приложения для улучшения зависящих от платформы веб-элементов управления благодаря возможности вызова кода C# из JavaScript.

## <a name="create-the-custom-renderer-on-each-platform"></a>Создание пользовательского отрисовщика на каждой платформе

Процесс создания класса пользовательского отрисовщика выглядит следующим образом:

1. Создайте подкласс класса `WkWebViewRenderer` в iOS и класса `WebViewRenderer` в Android и UWP, который отрисовывает пользовательский элемент управления.
1. Переопределите метод `OnElementChanged`, который отрисовывает [`WebView`](xref:Xamarin.Forms.WebView), и напишите логику для его настройки. Этот метод вызывается при создании объекта `HybridWebView`.
1. Добавьте атрибут `ExportRenderer` в класс пользовательского отрисовщика или *AssemblyInfo.cs*, чтобы указать, что он будет использоваться для отрисовки пользовательского элемента управления Xamarin.Forms. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

> [!NOTE]
> Для большинства элементов Xamarin.Forms предоставлять пользовательский отрисовщик в проекте для каждой платформы необязательно. Если пользовательский отрисовщик не зарегистрирован, будет использоваться отрисовщик по умолчанию для базового класса элемента управления. Однако настраиваемые отрисовщики необходимы в каждом зависящем от платформы проекте при отрисовке элемента [View](xref:Xamarin.Forms.View).

На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![Обязанности проекта настраиваемого отрисовщика HybridWebView](hybridwebview-images/solution-structure.png)

Пользовательский элемент управления `HybridWebView` отрисовывается с помощью зависящих от платформы классов отрисовщика, которые являются производными от класса `WkWebViewRenderer` в iOS и класса `WebViewRenderer` в Android и UWP. Это приводит к тому, что каждый пользовательский элемент управления `HybridWebView` отрисовывается с помощью собственных веб-элементов управления, как показано на следующих снимках экрана:

![HybridWebView на каждой платформе](hybridwebview-images/screenshots.png)

Классы `WkWebViewRenderer` и `WebViewRenderer` предоставляют метод `OnElementChanged`, который вызывается при создании пользовательского элемента управления Xamarin.Forms для отрисовки соответствующего собственного веб-элемента управления. Этот метод принимает параметр `VisualElementChangedEventArgs`, содержащий свойства `OldElement` и `NewElement`. Эти свойства представляют элемент Xamarin.Forms, к которому *был* присоединен отрисовщик, и элемент Xamarin.Forms, к которому *присоединен* отрисовщик, соответственно. В примере приложения свойство `OldElement` будет иметь значение `null`, а свойство `NewElement` будет содержать ссылку на экземпляр `HybridWebView`.

Настройка собственного веб-элемента управления выполняется в переопределенной версии метода `OnElementChanged` в классе отрисовщика для каждой платформы. Ссылку на отрисовываемый элемент управления Xamarin.Forms можно получить с помощью свойства `Element`.

Каждый класс пользовательского отрисовщика дополняется атрибутом `ExportRenderer`, который регистрирует отрисовщик в Xamarin.Forms. Атрибут принимает два параметра — имя типа отрисовываемого пользовательского элемента управления Xamarin.Forms и имя типа пользовательского отрисовщика. Префикс `assembly` в атрибуте указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматриваются структура веб-страницы, загружаемой каждым собственным веб-элементом управления, процесс вызова C# из JavaScript и реализация этого в классах пользовательского отрисовщика для каждой платформы.

### <a name="create-the-web-page"></a>Создание веб-страницы

В следующем примере кода показана веб-страница, которая будет отображаться пользовательским элементом управления `HybridWebView`:

```html
<html>
<body>
    <script src="http://code.jquery.com/jquery-2.1.4.min.js"></script>
    <h1>HybridWebView Test</h1>
    <br />
    Enter name: <input type="text" id="name">
    <br />
    <br />
    <button type="button" onclick="javascript: invokeCSCode($('#name').val());">Invoke C# Code</button>
    <br />
    <p id="result">Result:</p>
    <script type="text/javascript">function log(str) {
            $('#result').text($('#result').text() + " " + str);
        }

        function invokeCSCode(data) {
            try {
                log("Sending Data:" + data);
                invokeCSharpAction(data);
            }
            catch (err) {
                log(err);
            }
        }</script>
</body>
</html>
```

Веб-страница позволяет пользователю ввести свое имя в элементе `input` и предоставляет элемент `button`, который будет вызывать код C# при нажатии. Этот процесс выглядит следующим образом:

- Когда пользователь нажимает на элемент `button`, вызывается функция JavaScript `invokeCSCode`, и значение элемента `input` передается в функцию.
- Функция `invokeCSCode` вызывает функцию `log` для отображения данных, которые она отправляет в C# `Action`. Затем она вызывает метод `invokeCSharpAction`, чтобы вызывать C# `Action`, передавая параметр, полученный от элемента `input`.

Функция JavaScript `invokeCSharpAction` не определена на веб-странице и внедряется в нее каждым настраиваемым отрисовщиком.

На iOS этот HTML-файл находится в папке Content проекта платформы с действием сборки **BundleResource**. На Android этот HTML-файл находится в папке Assets/Content проекта платформы с действием сборки **AndroidAsset**.

### <a name="invoke-c-from-javascript"></a>Вызов C# из JavaScript

Процесс вызова C# из JavaScript идентичен на каждой платформе:

- Настраиваемый отрисовщик создает собственный веб-элемент управления и загружает файл HTML, заданный свойством `HybridWebView.Uri`.
- После загрузки веб-страницы настраиваемый отрисовщик внедряет функцию JavaScript `invokeCSharpAction` в веб-страницу.
- Когда пользователь вводит свое имя и нажимает на элемент HTML `button`, вызывается функция `invokeCSCode`, которая, в свою очередь, вызывает функцию `invokeCSharpAction`.
- Функция `invokeCSharpAction` вызывает метод в настраиваемом отрисовщике, который, свою очередь, вызывает метод `HybridWebView.InvokeAction`.
- Метод `HybridWebView.InvokeAction` вызывает зарегистрированный `Action`.

В следующих разделах мы рассмотрим, как этот процесс реализуется на каждой платформе.

### <a name="create-the-custom-renderer-on-ios"></a>Создание пользовательского отрисовщика в iOS

В следующем примере кода показан пользовательский отрисовщик для платформы iOS:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : WkWebViewRenderer, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        public HybridWebViewRenderer() : this(new WKWebViewConfiguration())
        {
        }

        public HybridWebViewRenderer(WKWebViewConfiguration config) : base(config)
        {
            userController = config.UserContentController;
            var script = new WKUserScript(new NSString(JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
            userController.AddUserScript(script);
            userController.AddScriptMessageHandler(this, "invokeAction");
        }

        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                userController.RemoveAllUserScripts();
                userController.RemoveScriptMessageHandler("invokeAction");
                HybridWebView hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }

            if (e.NewElement != null)
            {
                string filename = Path.Combine(NSBundle.MainBundle.BundlePath, $"Content/{((HybridWebView)Element).Uri}");
                LoadRequest(new NSUrlRequest(new NSUrl(filename, false)));
            }
        }

        public void DidReceiveScriptMessage(WKUserContentController userContentController, WKScriptMessage message)
        {
            ((HybridWebView)Element).InvokeAction(message.Body.ToString());
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                ((HybridWebView)Element).Cleanup();
            }
            base.Dispose(disposing);
        }        
    }
}
```

Класс `HybridWebViewRenderer` загружает веб-страницу, указанную в свойстве `HybridWebView.Uri`, в собственный элемент управления [`WKWebView`](xref:WebKit.WKWebView), а функция JavaScript `invokeCSharpAction` внедряется в веб-страницы. Когда пользователь вводит свое имя и нажимает элемент HTML `button`, выполняется функция JavaScript `invokeCSharpAction`, и метод `DidReceiveScriptMessage` вызывается после получения сообщения от веб-страницы. В свою очередь, этот метод вызывает метод `HybridWebView.InvokeAction`, который будет вызывать зарегистрированное действие для отображения всплывающего окна.

Это достигается следующим образом:

- Конструктор отрисовщика создает объект `WkWebViewConfiguration` и получает его объект [`WKUserContentController`](xref:WebKit.WKUserContentController). Объект `WkUserContentController` позволяет размещать сообщения и внедрять скрипты пользователя в веб-страницу.
- Конструктор отрисовщика создает объект [`WKUserScript`](xref:WebKit.WKUserScript), который внедряет функцию JavaScript `invokeCSharpAction` в веб-страницу после загрузки веб-страницы.
- Конструктор отрисовщика вызывает метод [`WKUserContentController.AddUserScript`](xref:WebKit.WKUserContentController.AddUserScript(WebKit.WKUserScript)) для добавления объекта [`WKUserScript`](xref:WebKit.WKUserScript) в контроллер содержимого.
- Конструктор отрисовщика вызывает метод [`WKUserContentController.AddScriptMessageHandler`](xref:WebKit.WKUserContentController.AddScriptMessageHandler(WebKit.IWKScriptMessageHandler,System.String)) для добавления обработчика сообщений скрипта с именем `invokeAction` в объект [`WKUserContentController`](xref:WebKit.WKUserContentController), который приводит к определению функции JavaScript `window.webkit.messageHandlers.invokeAction.postMessage(data)` во всех фреймах в экземплярах `WebView`, которые используют объект `WKUserContentController`.
- Если настраиваемый отрисовщик подключен к новому элементу Xamarin.Forms:
  - Метод [`WKWebView.LoadRequest`](xref:WebKit.WKWebView.LoadRequest(Foundation.NSUrlRequest)) загружает HTML-файл, указанный свойством `HybridWebView.Uri`. Код указывает, что файл сохранен в папке `Content` проекта. Когда веб-страница отображается, функция JavaScript `invokeCSharpAction` внедряется в веб-страницу.
- Ресурсы освобождаются, когда элемент, к которому присоединен отрисовщик, меняется.
- При удалении отрисовщика элемент Xamarin.Forms очищается.

> [!NOTE]
> Класс `WKWebView` поддерживается только в iOS 8 и более поздних версий.

Кроме того, необходимо изменить файл **Info.plist**, включив в него следующие значения:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

### <a name="create-the-custom-renderer-on-android"></a>Создание пользовательского отрисовщика в Android

В следующем примере кода показан пользовательский отрисовщик для платформы Android:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : WebViewRenderer
    {
        const string JavascriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<WebView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                ((HybridWebView)Element).Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.SetWebViewClient(new JavascriptWebViewClient(this, $"javascript: {JavascriptFunction}"));
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl($"file:///android_asset/Content/{((HybridWebView)Element).Uri}");
            }
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                ((HybridWebView)Element).Cleanup();
            }
            base.Dispose(disposing);
        }        
    }
}
```

Класс `HybridWebViewRenderer` загружает веб-страницу, указанную в свойстве `HybridWebView.Uri`, в собственный элемент управления [`WebView`](xref:Android.Webkit.WebView), а функция JavaScript `invokeCSharpAction` внедряется в веб-страницы после окончания загрузки веб-страницы с переопределением `OnPageFinished` в классе `JavascriptWebViewClient`:

```csharp
public class JavascriptWebViewClient : FormsWebViewClient
{
    string _javascript;

    public JavascriptWebViewClient(HybridWebViewRenderer renderer, string javascript) : base(renderer)
    {
        _javascript = javascript;
    }

    public override void OnPageFinished(WebView view, string url)
    {
        base.OnPageFinished(view, url);
        view.EvaluateJavascript(_javascript, null);
    }
}
```

Когда пользователь вводит свое имя и нажимает элемент HTML `button`, выполняется функция JavaScript `invokeCSharpAction`. Это достигается следующим образом:

- Если настраиваемый отрисовщик подключен к новому элементу Xamarin.Forms:
  - Метод `SetWebViewClient` задает новый объект `JavascriptWebViewClient` в качестве реализации `WebViewClient`.
  - Метод [`WebView.AddJavascriptInterface`](xref:Android.Webkit.WebView.AddJavascriptInterface*) вставляет новый экземпляр `JSBridge` в главный фрейм контекста JavaScript веб-преставления, называя его `jsBridge`. Это открывает доступ к методам в классе `JSBridge` из JavaScript.
  - Метод [`WebView.LoadUrl`](xref:Android.Webkit.WebView.LoadUrl*) загружает HTML-файл, указанный свойством `HybridWebView.Uri`. Код указывает, что файл сохранен в папке `Content` проекта.
  - В классе `JavascriptWebViewClient` функция JavaScript `invokeCSharpAction` внедряется в веб-страницу после завершения загрузки страницы.
- Ресурсы освобождаются, когда элемент, к которому присоединен отрисовщик, меняется.
- При удалении отрисовщика элемент Xamarin.Forms очищается.

Когда выполняется функция JavaScript `invokeCSharpAction`, она, в свою очередь, вызывает метод `JSBridge.InvokeAction`, который показан в следующем примере кода:

```csharp
public class JSBridge : Java.Lang.Object
{
    readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

    public JSBridge(HybridWebViewRenderer hybridRenderer)
    {
        hybridWebViewRenderer = new WeakReference<HybridWebViewRenderer>(hybridRenderer);
    }

    [JavascriptInterface]
    [Export("invokeAction")]
    public void InvokeAction(string data)
    {
        HybridWebViewRenderer hybridRenderer;

        if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget(out hybridRenderer))
        {
            ((HybridWebView)hybridRenderer.Element).InvokeAction(data);
        }
    }
}
```

Класс должен быть производным от `Java.Lang.Object`, и методы, предоставляемые для JavaScript, должны быть снабжены атрибутами `[JavascriptInterface]` и `[Export]`. Таким образом, когда функция JavaScript `invokeCSharpAction` внедряется в веб-страницу и выполняется, она вызывает метод `JSBridge.InvokeAction`, поскольку имеет атрибуты `[JavascriptInterface]` и `[Export("invokeAction")]`. В свою очередь, метод `InvokeAction` вызывает метод `HybridWebView.InvokeAction`, который будет вызывать зарегистрированное действие для отображения всплывающего окна.

> [!IMPORTANT]
> Проекты Android, в которых используется атрибут `[Export]`, должны содержать ссылку на `Mono.Android.Export`, иначе возникнет ошибка компилятора.

Обратите внимание, что класс `JSBridge` поддерживает `WeakReference` для класса `HybridWebViewRenderer`. Это позволяет избежать создания циклической ссылки между двумя классами. Дополнительные сведения см. в разделе [Слабые ссылки](/en-us/dotnet/standard/garbage-collection/weak-references).

### <a name="create-the-custom-renderer-on-uwp"></a>Создание пользовательского отрисовщика в UWP

В следующем примере кода показан пользовательский отрисовщик для платформы UWP:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : WebViewRenderer
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.WebView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri($"ms-appx-web:///Content//{((HybridWebView)Element).Uri}");
            }
        }

        async void OnWebViewNavigationCompleted(Windows.UI.Xaml.Controls.WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            ((HybridWebView)Element).InvokeAction(e.Value);
        }

        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                ((HybridWebView)Element).Cleanup();
            }
            base.Dispose(disposing);
        }        
    }
}
```

Класс `HybridWebViewRenderer` загружает веб-страницу, указанную в свойстве `HybridWebView.Uri` в собственном элементе управления `WebView`, а функция JavaScript `invokeCSharpAction` внедряется в веб-страницы после загрузки веб-страницы с методом `WebView.InvokeScriptAsync`. Когда пользователь вводит свое имя и нажимает элемент HTML `button`, выполняется функция JavaScript `invokeCSharpAction`, и метод `OnWebViewScriptNotify` вызывается после получения уведомления от веб-страницы. В свою очередь, этот метод вызывает метод `HybridWebView.InvokeAction`, который будет вызывать зарегистрированное действие для отображения всплывающего окна.

Это достигается следующим образом:

- Если настраиваемый отрисовщик подключен к новому элементу Xamarin.Forms:
  - Обработчики событий для событий `NavigationCompleted` и `ScriptNotify` регистрируются. Событие `NavigationCompleted` возникает, когда собственный элемент управления `WebView` завершил загрузку текущего содержимого или произошел сбой навигации. Событие `ScriptNotify` возникает, когда содержимое в собственном элементе управления `WebView` использует JavaScript для передачи строки в приложение. Веб-страница активирует событие `ScriptNotify` путем вызова `window.external.notify` с передачей параметра `string`.
  - Свойство `WebView.Source` получает значение URI HTML-файла, который задается свойством `HybridWebView.Uri`. Код предполагает, что файл сохранен в папке `Content` проекта. Когда веб-страница отобразится, событие `NavigationCompleted` сработает и метод `OnWebViewNavigationCompleted` будет вызван. Функция JavaScript `invokeCSharpAction` будет внедрена в веб-страницу с помощью метода `WebView.InvokeScriptAsync`, если навигация успешно завершена.
- Подписка на это событие отменяется, если элемент Xamarin.Forms, к которому присоединен отрисовщик, изменяется.
- При удалении отрисовщика элемент Xamarin.Forms очищается.

## <a name="related-links"></a>Связанные ссылки

- [HybridWebView (пример)](/samples/xamarin/xamarin-forms-samples/customrenderers-hybridwebview)