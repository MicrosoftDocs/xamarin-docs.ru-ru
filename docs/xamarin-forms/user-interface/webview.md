---
title: WebView Xamarin. Forms
description: В этой статье объясняется, как использовать класс Xamarin. Forms WebView для представления локальных или сетевых веб-содержимого и документов пользователям.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2020
ms.openlocfilehash: 31b705a51e405285cc5eaae391dd0794bfacfd9c
ms.sourcegitcommit: 443ecd9146fe2a7bbb9b5ab6d33c835876efcf1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2020
ms.locfileid: "82852489"
---
# <a name="xamarinforms-webview"></a>WebView Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)

[`WebView`](xref:Xamarin.Forms.WebView)— Это представление для отображения содержимого Web и HTML в приложении:

![В браузере приложений](webview-images/in-app-browser.png)

## <a name="content"></a>Content

`WebView`поддерживает следующие типы содержимого:

- Веб &ndash; -сайты HTML & CSS WebView полностью поддерживают веб-сайты, написанные с помощью HTML & CSS, включая поддержку JavaScript.
- Документы &ndash; , поскольку WebView реализуется с помощью собственных компонентов на каждой платформе, WebView может отображать документы, доступные для просмотра на каждой платформе. Это означает, что PDF-файлы работают в iOS и Android.
- Строки &ndash; HTML WebView могут показывать строки HTML из памяти.
- Локальные файлы &ndash; WebView могут представлять любой из типов содержимого, включенных выше, в приложение.

> [!NOTE]
> `WebView`в Windows не поддерживает Silverlight, Flash или любые элементы управления ActiveX, даже если они поддерживаются Internet Explorer на этой платформе.

### <a name="websites"></a>веб-сайты;

Чтобы отобразить веб-сайт из Интернета, задайте `WebView`для [`Source`](xref:Xamarin.Forms.WebViewSource) свойства значение строковый URL-адрес:

```csharp
var browser = new WebView
{
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URL-адреса должны быть полностью сформированы с указанным Протоколом (т. е. он должен иметь в начале префикса «http://» или «https://»).

#### <a name="ios-and-ats"></a>iOS и ATS

Начиная с версии 9, iOS позволяет приложению взаимодействовать только с серверами, которые по умолчанию реализуют оптимальную безопасность. Для подключения к небезопасным `Info.plist` серверам необходимо задать значения в параметре.

> [!NOTE]
> Если приложению требуется подключение к незащищенному веб-сайту, следует всегда вводить домен как исключение с помощью `NSExceptionDomains` вместо отключения ATS полностью с помощью. `NSAllowsArbitraryLoads` `NSAllowsArbitraryLoads`следует использовать только в чрезвычайных ситуациях.

Ниже показано, как включить конкретный домен (в данном случае xamarin.com) для обхода требований ATS:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
    ...
</key>
```

Рекомендуется только разрешить некоторым доменам обходить ATS, что позволяет использовать Доверенные сайты, одновременно выигрыша от дополнительной безопасности в недоверенных доменах. Ниже показан менее безопасный метод отключения ATS для приложения.

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
    ...
</key>
```

Дополнительные сведения об этой новой функции в iOS 9 см. в разделе [Безопасность транспорта приложений](~/ios/app-fundamentals/ats.md) .

### <a name="html-strings"></a>Строки HTML

Если требуется предоставить в коде строку HTML, определенную динамически, необходимо создать экземпляр [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![WebView отображение HTML-строки](webview-images/html-string.png)

В приведенном выше коде `@` используется для пометки HTML как [буквального строкового литерала](/dotnet/csharp/programming-guide/strings/#regular-and-verbatim-string-literals), что означает, что большинство escape-символов не учитываются.

> [!NOTE]
> Может потребоваться установить `WidthRequest` свойства и `HeightRequest` объекта [`WebView`](xref:Xamarin.Forms.WebView) для просмотра содержимого HTML в зависимости от макета, который `WebView` является дочерним для. Например, это необходимо в [`StackLayout`](xref:Xamarin.Forms.StackLayout).

### <a name="local-html-content"></a>Локальное содержимое HTML

WebView может отображать содержимое из HTML, CSS и JavaScript Embedded в приложении. Пример:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamarin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

Каскад

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Обратите внимание, что шрифты, указанные в приведенном выше CSS, должны быть настроены для каждой платформы, поскольку не все платформы имеют одинаковые шрифты.

Чтобы отобразить локальное содержимое с `WebView`помощью, необходимо открыть HTML-файл, как и любой другой, а затем загрузить содержимое в виде строки в `Html` свойство объекта. `HtmlWebViewSource` Дополнительные сведения об открытии файлов см. в разделе [Работа с файлами](~/xamarin-forms/data-cloud/data/files.md).

На следующих снимках экрана показан результат отображения локального содержимого на каждой платформе:

![WebView Отображение локального содержимого](webview-images/local-content.png)

Хотя первая страница загружена, не `WebView` имеет сведений о том, откуда поступил HTML-код. Это проблема при работе со страницами, которые ссылаются на локальные ресурсы. Например, если локальные страницы связаны друг с другом, страница использует отдельный файл JavaScript или ссылку на страницу таблицы стилей CSS.  

Чтобы решить эту проблему, необходимо указать, `WebView` где искать файлы в файловой системе. Это можно сделать, задав `BaseUrl` свойство в элементе `HtmlWebViewSource` , используемом `WebView`.

Так как файловая система в каждой операционной системе отличается, необходимо определить этот URL-адрес на каждой платформе. Xamarin. Forms предоставляет `DependencyService` для разрешения зависимостей во время выполнения на каждой платформе.

Чтобы использовать `DependencyService`, сначала определите интерфейс, который можно реализовать на каждой платформе:

```csharp
public interface IBaseUrl { string Get(); }
```

Обратите внимание, что пока интерфейс не будет реализован на каждой платформе, приложение не запустится. В общем проекте убедитесь в том, что вы не захотите задать `BaseUrl` с помощью `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

После этого необходимо предоставить реализации интерфейса для каждой платформы.

#### <a name="ios"></a>iOS

В iOS веб-содержимое должно находиться в корневом каталоге проекта или каталоге **ресурсов** с действием сборки *BundleResource*, как показано ниже:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Локальные файлы в iOS](webview-images/ios-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Локальные файлы в iOS](webview-images/ios-xs.png)

-----

Для `BaseUrl` параметра должен быть задан путь к основному пакету:

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS
{
  public class BaseUrl_iOS : IBaseUrl
  {
    public string Get()
    {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

В Android поместите HTML, CSS и изображения в папку Assets с действием Build *AndroidAsset* , как показано ниже:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Локальные файлы на Android](webview-images/android-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Локальные файлы на Android](webview-images/android-xs.png)

-----

В Android для параметра `BaseUrl` должно быть задано `"file:///android_asset/"`значение:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android
{
  public class BaseUrl_Android : IBaseUrl
  {
    public string Get()
    {
      return "file:///android_asset/";
    }
  }
}
```

В Android файлы в папке **Assets** также могут быть доступны через текущий контекст Android, который предоставляется `MainActivity.Instance` свойством:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html")))
{
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В проектах универсальная платформа Windows (UWP) размещайте HTML, CSS и изображения в корне проекта с действием Build, имеющим значение *Content*.

Для `BaseUrl` `"ms-appx-web:///"`свойства должно быть задано значение:

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>Навигация

WebView поддерживает навигацию по нескольким методам и свойствам, которые он делает доступными:

- **GoForward ()** &ndash; если `CanGoForward` имеет значение true, `GoForward` вызов переходит вперед к следующей посещенной странице.
- **GoBack ()** &ndash; если `CanGoBack` имеет значение true, `GoBack` при вызове осуществляется переход к последней просмотренной странице.
- **CanGoBack** &ndash; CanGoBack `true` , если есть страницы для обратного перехода, `false` если браузер находится на начальном URL-адресе.
- **CanGoForward** &ndash; Кангофорвард `true` , если пользователь перешел назад и может перейти вперед к уже посещенной странице.

На страницах `WebView` не поддерживает жесты с несколькими касаниями. Важно убедиться, что содержимое является оптимизированным для мобильных устройств и отображается без необходимости масштабирования.

Обычно для приложений отображается ссылка в `WebView`, а не в браузере устройства. В таких случаях полезно разрешить нормальную навигацию, но когда пользователь обращается к начальной ссылке, приложение должно вернуться к обычному представлению приложения.

Чтобы включить этот сценарий, используйте встроенные методы навигации и свойства.

Начните с создания страницы для представления браузера:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.InAppBrowserXaml"
             Title="Browser">
    <StackLayout Margin="20">
        <StackLayout Orientation="Horizontal">
            <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="OnBackButtonClicked" />
            <Button Text="Forward" HorizontalOptions="EndAndExpand" Clicked="OnForwardButtonClicked" />
        </StackLayout>
        <!-- WebView needs to be given height and width request within layouts to render. -->
        <WebView x:Name="webView" WidthRequest="1000" HeightRequest="1000" />
    </StackLayout>
</ContentPage>
```

В коде программной части:

```csharp
public partial class InAppBrowserXaml : ContentPage
{
    public InAppBrowserXaml(string URL)
    {
        InitializeComponent();
        webView.Source = URL;
    }

    async void OnBackButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoBack)
        {
            webView.GoBack();
        }
        else
        {
            await Navigation.PopAsync();
        }
    }

    void OnForwardButtonClicked(object sender, EventArgs e)
    {
        if (webView.CanGoForward)
        {
            webView.GoForward();
        }
    }
}
```

Вот и все!

![Кнопки навигации WebView](webview-images/in-app-browser.png)

## <a name="events"></a>События

WebView создает следующие события, помогающие реагировать на изменения в состоянии:

- [`Navigating`](xref:Xamarin.Forms.WebView.Navigating)— событие, создаваемое, когда WebView начинает загрузку новой страницы.
- [`Navigated`](xref:Xamarin.Forms.WebView.Navigated)— событие, возникающее при загрузке страницы и остановке навигации.
- [`ReloadRequested`](xref:Xamarin.Forms.WebView.ReloadRequested)— событие, создаваемое при запросе на перезагрузку текущего содержимого.

[`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs) Объект, сопровождающий [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) событие, имеет четыре свойства:

- `Cancel`— Указывает, следует ли отменить навигацию.
- `NavigationEvent`— событие навигации, которое было вызвано.
- `Source`— элемент, который выполнил навигацию.
- `Url`— Назначение навигации.

[`WebNavigatedEventArgs`](xref:Xamarin.Forms.WebNavigatedEventArgs) Объект, сопровождающий [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) событие, имеет четыре свойства:

- `NavigationEvent`— событие навигации, которое было вызвано.
- `Result`— Описывает результат навигации с помощью элемента [`WebNavigationResult`](xref:Xamarin.Forms.WebNavigationResult) перечисления. Допустимые значения: `Cancel`, `Failure`, `Success` и `Timeout`.
- `Source`— элемент, который выполнил навигацию.
- `Url`— Назначение навигации.

Если предполагается использование веб-страниц, загрузка которых занимает много времени, рассмотрите возможность использования событий [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) и [`Navigated`](xref:Xamarin.Forms.WebView.Navigated) для реализации индикатора состояния. Пример:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="WebViewSample.LoadingLabelXaml"
             Title="Loading Demo">
    <StackLayout>
        <!--Loading label should not render by default.-->
        <Label x:Name="labelLoading" Text="Loading..." IsVisible="false" />
        <WebView HeightRequest="1000" WidthRequest="1000" Source="http://www.xamarin.com" Navigated="webviewNavigated" Navigating="webviewNavigating" />
    </StackLayout>
</ContentPage>
```

Два обработчика событий:

```csharp
void webviewNavigating(object sender, WebNavigatingEventArgs e)
{
    labelLoading.IsVisible = true;
}

void webviewNavigated(object sender, WebNavigatedEventArgs e)
{
    labelLoading.IsVisible = false;
}
```

В результате получается следующий результат (Загрузка):

![Пример события навигации WebView](webview-images/loading-start.png)

Загрузка завершена:

![Пример события навигации WebView](webview-images/loading-end.png)

## <a name="reloading-content"></a>Перезагрузка содержимого

[`WebView`](xref:Xamarin.Forms.WebView)содержит `Reload` метод, который можно использовать для повторной загрузки текущего содержимого:

```csharp
var webView = new WebView();
...
webView.Reload();
```

При вызове `Reload` метода запускается `ReloadRequested` событие, указывающее, что был выполнен запрос на перезагрузку текущего содержимого.

## <a name="performance"></a>Производительность

Популярные веб-браузеры принимают такие технологии, как визуализация с аппаратным ускорением и компиляция JavaScript. До Xamarin. Forms 4,4 в iOS была реализована `WebView` Xamarin. Forms `UIWebView` классом. Однако многие из этих технологий были недоступны в этой реализации. Таким образом, поскольку Xamarin. Forms 4,4, Xamarin. `WebView` Forms реализуется `WkWebView` классом для iOS, который поддерживает более быстрый просмотр.

> [!NOTE]
> В iOS `WkWebViewRenderer` есть перегрузка конструктора, принимающая `WkWebViewConfiguration` аргумент. Это позволяет настроить модуль подготовки отчетов при создании.

Приложение может вернуться к использованию класса iOS `UIWebView` для реализации Xamarin. Forms `WebView`по соображениям совместимости. Это можно сделать, добавив следующий код в файл **AssemblyInfo.CS** в проекте платформы iOS для приложения:

```csharp
// Opt-in to using UIWebView instead of WkWebView.
[assembly: ExportRenderer(typeof(Xamarin.Forms.WebView), typeof(Xamarin.Forms.Platform.iOS.WebViewRenderer))]
```

`WebView`по умолчанию в Android выполняется примерно так же быстро, как и встроенный браузер.

[WebView UWP](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) использует механизм визуализации Microsoft ребра. Для настольных и планшетных устройств должна отображаться такая же производительность, как и при использовании браузера Microsoft ребра.

## <a name="permissions"></a>Разрешения

`WebView` Для работы необходимо убедиться, что для каждой платформы заданы разрешения. Обратите внимание, что на `WebView` некоторых платформах будет работать в режиме отладки, но не при построении для выпуска. Это связано с тем, что некоторые разрешения, например для доступа к Интернету на Android, по умолчанию устанавливаются по Visual Studio для Mac в режиме отладки.

- Для отображения сетевого содержимого **UWP** &ndash; требуется возможность Интернета (клиент & сервер).
- **Android** &ndash; требуется `INTERNET` только при отображении содержимого из сети. Для локального содержимого не требуются специальные разрешения.
- для **iOS** &ndash; не требуются специальные разрешения.

## <a name="layout"></a>Макет

В отличие от большинства других представлений Xamarin. Forms `WebView` , `HeightRequest` требует `WidthRequest` , чтобы и были указаны, если они содержатся в StackLayout или RelativeLayout. Если не указать эти свойства, объект `WebView` не будет отображен.

В следующих примерах демонстрируются макеты, которые приводят к работе, отрисовка `WebView`s:

StackLayout с Видсрекуест & Хеигхтрекуест:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout с Видсрекуест & Хеигхтрекуест:

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

Абсолутелайаут *без* Видсрекуест & хеигхтрекуест:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Сетка *без* Видсрекуест & хеигхтрекуест. Grid является одной из нескольких макетов, не требующих указания требуемых значений высоты и ширины.

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```

## <a name="invoking-javascript"></a>Вызов JavaScript

[`WebView`](xref:Xamarin.Forms.WebView)включает возможность вызывать функцию JavaScript из C# и возвращать любой результат в вызывающий код C#. Это осуществляется с помощью [`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) метода, который показан в следующем примере из примера [WebView](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) :

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

[`WebView.EvaluateJavaScriptAsync`](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) Метод вычисляет код JavaScript, указанный в качестве аргумента, и возвращает любой результат в виде `string`. В этом примере вызывается `factorial` функция JavaScript, которая возвращает факториал в `number` виде результата. Эта функция JavaScript определена в локальном HTML-файле, который [`WebView`](xref:Xamarin.Forms.WebView) загружается, и показана в следующем примере:

```html
<html>
<body>
<script type="text/javascript">
function factorial(num) {
        if (num === 0 || num === 1)
            return 1;
        for (var i = num - 1; i >= 1; i--) {
            num *= i;
        }
        return num;
}
</script>
</body>
</html>
```

## <a name="cookies"></a>Файлы cookie

Файлы cookie можно задать для [`WebView`](xref:Xamarin.Forms.WebView), который затем отправляется с веб-запросом по указанному URL. Это достигается путем добавления `Cookie` объектов в объект `CookieContainer`, который затем задается как значение `WebView.Cookies` привязываемого свойства. В приведенном ниже коде показан соответствующий пример:

```csharp
using System.Net;
using Xamarin.Forms;
// ...

CookieContainer cookieContainer = new CookieContainer();
Uri uri = new Uri("https://dotnet.microsoft.com/apps/xamarin", UriKind.RelativeOrAbsolute);

Cookie cookie = new Cookie
{
    Name = "XamarinCookie",
    Expires = DateTime.Now.AddDays(1),
    Value = "My cookie",
    Domain = uri.Host,
    Path = "/"
};
cookieContainer.Add(uri, cookie);
webView.Cookies = cookieContainer;
webView.Source = new UrlWebViewSource { Url = uri.ToString() };
```

В этом примере к `Cookie` `CookieContainer` объекту добавляется один объект, который затем задается как значение `WebView.Cookies` свойства. Когда [`WebView`](xref:Xamarin.Forms.WebView) отправляет веб-запрос по указанному URL-адресу, файл cookie отправляется вместе с запросом.

## <a name="uiwebview-deprecation-and-app-store-rejection-itms-90809"></a>Уивебвиев устаревания и отклонение магазина приложений (ИТМС-90809)

Начиная с 2020 апреля [компания Apple будет отклонять приложения](https://developer.apple.com/news/?id=12232019b) , которые по-прежнему `UIWebView` используют устаревший API. Хотя Xamarin. Forms перешли в `WKWebView` качестве по умолчанию, по-прежнему существует ссылка на старый пакет SDK в двоичных файлах Xamarin. Forms. Текущее поведение [компоновщика iOS](~/ios/deploy-test/linker.md) не удаляет это, поэтому в результате нерекомендуемый `UIWebView` API будет по-прежнему отображаться в приложении при отправке в App Store.

Для устранения этой проблемы доступна предварительная версия компоновщика. Чтобы включить предварительную версию, необходимо предоставить компоновщику дополнительный аргумент `--optimize=experimental-xforms-product-type` .

Ниже перечислены необходимые условия для работы.

- **Xamarin. forms 4,5 или более поздней версии**. Xamarin. Forms 4,6 или более поздней версии требуется, если приложение использует визуальный элемент Material.
- **Xamarin. iOS 13.10.0.17 или более поздней версии**. Проверьте версию Xamarin. iOS [в Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#version-information). Эта версия Xamarin. iOS входит в состав Visual Studio для Mac 8.4.1 и Visual Studio 16.4.3.
- **Удалите ссылки на `UIWebView` **. Код не должен содержать ссылки на `UIWebView` классы, использующие. `UIWebView`

Дополнительные сведения об обнаружении и удалении `UIWebView` ссылок см. в разделе [уивебвиев устарел](~/ios/user-interface/controls/webview.md#uiwebview-deprecation).

### <a name="configure-the-linker"></a>Настройка компоновщика

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Выполните следующие действия, чтобы компоновщик удалил `UIWebView` ссылки:

1. **Откройте свойства** &ndash; проекта iOS. Щелкните правой кнопкой мыши проект iOS и выберите пункт **Свойства**.
1. **Перейдите к разделу** &ndash; сборка iOS выберите раздел **сборка iOS** .
1. **Обновите дополнительные аргументы** &ndash; mtouch в **дополнительных аргументах mtouch** . Добавьте этот `--optimize=experimental-xforms-product-type` флаг (в дополнение к любому значению, которое уже возможно в нем). Примечание. Этот флаг работает совместно с **поведением компоновщика** , установленным в **пакет SDK** , или **свяжите все**. Если по какой-либо причине возникают ошибки при задании режима "поведение компоновщика", это, скорее всего, проблема в коде приложения или библиотеке стороннего разработчика, не защищенной компоновщиком. Дополнительные сведения о компоновщике см. в разделе [связывание приложений Xamarin. iOS](~/ios/deploy-test/linker.md).
1. **Обновление всех конфигураций** &ndash; сборки используйте списки **Конфигурация** и **платформа** в верхней части окна для обновления всех конфигураций сборки. Наиболее важной конфигурацией для обновления является конфигурация **Release/iPhone** , так как она обычно используется для создания сборок для отправки в App Store.

Можно увидеть окно с новым флагом на этом снимке экрана:

[![Установка флага в разделе "сборка iOS"](webview-images/iosbuildblade-vs-sml.png)](webview-images/iosbuildblade-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Выполните следующие действия, чтобы компоновщик удалил `UIWebView` ссылки:

1. **Откройте параметры** &ndash; проекта iOS щелкните правой кнопкой мыши проект iOS и выберите пункт **Параметры**.
1. **Перейдите к разделу** &ndash; сборка iOS выберите раздел **сборка iOS** .
1. **Обновите дополнительные _mtouch_ аргументы** &ndash; mtouch в **дополнительных аргументах _mtouch_ ** . Добавьте этот `--optimize=experimental-xforms-product-type` флаг (в дополнение к любому значению, которое уже возможно в нем). Примечание. Этот флаг работает совместно с **поведением компоновщика** , установленным в **пакет SDK** , или **свяжите все**. Если по какой-либо причине возникают ошибки при задании режима "поведение компоновщика", это, скорее всего, проблема в коде приложения или библиотеке стороннего разработчика, не защищенной компоновщиком. Дополнительные сведения о компоновщике см. в разделе [связывание приложений Xamarin. iOS](~/ios/deploy-test/linker.md).
1. **Обновление всех конфигураций** &ndash; сборки используйте списки **Конфигурация** и **платформа** в верхней части окна для обновления всех конфигураций сборки. Наиболее важной конфигурацией для обновления является конфигурация **Release/iPhone** , так как она обычно используется для создания сборок для отправки в App Store.

Можно увидеть окно с новым флагом на этом снимке экрана:

[![Установка флага в разделе "сборка iOS"](webview-images/iosbuildblade-xs-sml.png)](webview-images/iosbuildblade-xs.png#lightbox)

-----

Теперь при создании новой сборки (выпуска) и ее отправке в App Store не должно быть предупреждений об устаревшем API.

## <a name="related-links"></a>Связанные ссылки

- [Работа с WebView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)
- [WebView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview)
- [Устаревшие Уивебвиев](~/ios/user-interface/controls/webview.md#uiwebview-deprecation)
