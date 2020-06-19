---
title: Индексирование приложения и создание прямых ссылок
description: В этой статье объясняется, как использовать индексирование приложения и создание прямых ссылок, чтобы предоставить доступ к доступному для поиска содержимому приложения Xamarin.Forms на устройствах iOS и Android.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/28/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c7f5979b93ff35b14ec7c13418a4565604f46bd7
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84570639"
---
# <a name="application-indexing-and-deep-linking"></a>Индексирование приложения и создание прямых ссылок

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/deeplinking)

_Индексирование приложения позволяет приложениям, которые в противном случае забываются после нескольких использований, оставаться релевантными благодаря отображению в результатах поиска. Создание прямых ссылок позволяет приложениям реагировать на результат поиска, который содержит данные приложения, как правило путем перехода на страницу, на которую ссылается прямая ссылка. В этой статье объясняется, как использовать индексирование приложения и создание прямых ссылок, чтобы предоставить доступ к доступному для поиска содержимому приложения Xamarin.Forms на устройствах iOS и Android._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Видеоролик о создании прямых ссылок с помощью Xamarin.Forms и Azure**

Функции индексирования приложения и создания прямых ссылок Xamarin.Forms предоставляют API для публикации метаданных для индексирования приложения при навигации пользователей по приложениям. Затем индексированное содержимое становится доступным для поиска в поиске Spotlight, Google или поиске в Интернете. При касании результата поиска, который содержит прямую ссылку, будет активировано событие, обрабатываемое приложением. Оно, как правило, используется для перехода на страницу, на которую ссылается прямая ссылка.

В этом примере представлено приложение списка задач, где данные сохраняются в локальной базе данных SQLite, как показано на следующих снимках экрана:

![](deep-linking-images/screenshots.png "TodoList Application")

Каждый экземпляр `TodoItem`, созданный пользователем, индексируется. Механизм поиска, определяемый платформой, может использоваться для поиска индексированных данных из приложения. Когда пользователь выбирает элемент в результатах поиска для приложения, приложение запускается, осуществляется переход к `TodoItemPage` и отображается элемент `TodoItem`, на который ссылается прямая ссылка.

Дополнительные сведения об использовании базы данных SQLite см. в разделе, посвященном [локальным базам данных Xamarin.Forms](~/xamarin-forms/data-cloud/data/databases.md).

> [!NOTE]
> Функции индексирования приложения и создания прямых ссылок Xamarin.Forms доступны только на платформах iOS и Android и требуют как минимум версии iOS 9 и API 23 соответственно.

## <a name="setup"></a>Установка

Следующие разделы содержат дополнительные инструкции по настройке для использования этой функции в платформах iOS и Android.

### <a name="ios"></a>iOS

Для платформы iOS убедитесь, что проект платформы iOS указывает файл **Entitlements.plist** как пользовательский файл прав для подписания пакета.

Использование универсальных ссылок iOS

1. С помощью ключа `applinks` добавьте в приложение право "Связанные домены", включающее все домены, которые будут поддерживаться вашим приложением.
1. Добавьте файл связывания с сайтом приложения Apple на свой веб-сайт.
1. Добавьте ключ `applinks` в файл связывания с сайтом приложения Apple.

Дополнительные сведения см. в статье о [разрешении ссылок на содержимое от приложений и веб-сайтов](https://developer.apple.com/documentation/uikit/core_app/allowing_apps_and_websites_to_link_to_your_content) на сайте developer.apple.com.

### <a name="android"></a>Android

Для платформы Android существует ряд необходимых условий, которые должны быть выполнены для использования индексирования приложения и создания прямых ссылок.

1. Версия вашего приложения должна быть актуальна в Google Play.
1. Дополнительный веб-сайт должен быть зарегистрирован для приложения в консоли разработчика Google. После связывания приложения с веб-сайтом можно выполнить индексирование URL-адресов для веб-сайта и приложения, которые затем могут обрабатываться в результатах поиска. Дополнительные сведения см. в разделе, посвященном [индексированию приложений для поиска Google](https://support.google.com/googleplay/android-developer/answer/6041489), на веб-сайте Google.
1. Приложение должно поддерживать намерения URL-адреса HTTP в классе `MainActivity`, который уведомляет компонент индексирования приложения о типах схем данных URL-адресов, на которые может отвечать приложение. Дополнительные сведения см. в разделе [Настройка фильтра намерений](~/android/platform/app-linking.md#configure-intent-filter).

После выполнения этих условий необходимо выполнить следующую дополнительную настройку для использования индексирования приложения и создания прямых ссылок Xamarin.Forms в платформе Android:

1. Установите пакет NuGet [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) в проект приложения Android.
1. В файле **MainActivity.cs** добавьте объявление для использования пространства имен `Xamarin.Forms.Platform.Android.AppLinks`.
1. В файле **MainActivity.cs** добавьте объявление для использования пространства имен `Firebase`.
1. В веб-браузере создайте новый проект с помощью [консоли Firebase](https://console.firebase.google.com/).
1. В консоли Firebase добавьте Firebase в приложение Android и введите необходимые данные.
1. Скачайте итоговый файл **google-services.json**.
1. Добавьте файл **google-services.json** в корневой каталог проекта Android и задайте для параметра **Действие при сборке** значение **GoogleServicesJson**.
1. В переопределении `MainActivity.OnCreate` добавьте следующую строку кода под `Forms.Init(this, bundle)`:

```csharp
FirebaseApp.InitializeApp(this);
AndroidAppLinks.Init(this);
```

При добавлении в проект **google-services.json** (и задании действия при сборке *GoogleServicesJson*\*) процесс сборки извлекает ключ API и идентификатор клиента, а затем добавляет эти учетные данные в созданный файл манифеста.

> [!NOTE]
> В этой статье термины "ссылки на приложения" и "прямые ссылки" часто взаимозаменяемы. Но в Android значения этих терминов разные. В Android прямая ссылка — это фильтр намерений, который позволяет пользователям непосредственно переходить к определенному действию в приложении. Щелкнув прямую ссылку, можно открыть диалоговое окно для устранения неоднозначности. В таком окне пользователь может выбрать одно из нескольких приложений, которые могут работать с этим URL-адресом. Ссылка на приложение Android — это прямая ссылка на основе URL-адреса, проверенного на предмет на принадлежности вашему веб-сайту. Если щелкнуть ссылку на приложение, откроется ваше приложение (при условии, что оно установлено). Диалоговое окно для устранения неоднозначности отображаться не будет.

Дополнительные сведения см. в статье, посвященной [созданию прямых ссылок на содержимое с помощью навигации по URL-адресам Xamarin.Forms](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/), в блоге Xamarin.

## <a name="indexing-a-page"></a>Индексации страницы

Процесс индексации страницы и предоставления ее системам поиска Google и Spotlight выглядит следующим образом:

1. Создайте [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry), содержащий метаданные, необходимые для индексирования страницы, а также прямую ссылку для возврата на страницу при выборе пользователем индексированного содержимого в результатах поиска.
1. Зарегистрируйте экземпляр [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) для его индексирования для поиска.

В следующем примере кода демонстрируется создание экземпляра [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry):

```csharp
AppLinkEntry GetAppLink(TodoItem item)
{
    var pageType = GetType().ToString();
    var pageLink = new AppLinkEntry
    {
        Title = item.Name,
        Description = item.Notes,
        AppLinkUri = new Uri($"http://{App.AppName}/{pageType}?id={item.ID}", UriKind.RelativeOrAbsolute),
        IsLinkActive = true,
        Thumbnail = ImageSource.FromFile("monkey.png")
    };

    pageLink.KeyValues.Add("contentType", "TodoItemPage");
    pageLink.KeyValues.Add("appName", App.AppName);
    pageLink.KeyValues.Add("companyName", "Xamarin");

    return pageLink;
}
```

Экземпляр [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) содержит ряд свойств, значения которых необходимы для индексирования страницы и создания прямой ссылки. Свойства [`Title`](xref:Xamarin.Forms.IAppLinkEntry.Title), [`Description`](xref:Xamarin.Forms.IAppLinkEntry.Description) и [`Thumbnail`](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) используются для идентификации индексированного содержимого, когда оно отображается в результатах поиска. Свойству [`IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) присваивается значение `true`, указывающее, что индексированное содержимое сейчас просматривается. Свойство [`AppLinkUri`](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) является `Uri`, содержащим сведения, необходимые для возврата к текущей странице и отображения текущего элемента `TodoItem`. В следующем примере показан пример `Uri` для примера приложения:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=2
```

Этот `Uri` содержит все сведения, необходимые для запуска приложения `deeplinking`, перехода к `DeepLinking.TodoItemPage` и отображения элемента `TodoItem` с `ID`, равным 2.

## <a name="registering-content-for-indexing"></a>Регистрация содержимого для индексирования

После создания экземпляра [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) для отображения в результатах поиска его необходимо зарегистрировать для индексирования. Это осуществляется с помощью метода [`RegisterLink`](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)), как показано в следующем примере кода:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

При этом экземпляр [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) добавляется в коллекцию [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks) приложения.

> [!NOTE]
> Метод `RegisterLink` может также использоваться для обновления содержимого, которое индексируется для страницы.

После регистрации экземпляра [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) для индексирования он может отображаться в результатах поиска. На следующем снимке экрана показано индексированное содержимое, отображаемое в результатах поиска на платформе iOS:

![](deep-linking-images/ios-search.png "Indexed Content in Search Results on iOS")

## <a name="de-registering-indexed-content"></a>Отмена регистрации индексированного содержимого

Метод [`DeregisterLink`](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) используется для удаления индексированного содержимого из результатов поиска, как показано в следующем примере кода:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

При этом экземпляр [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) удаляется из коллекции [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks) приложения.

> [!NOTE]
> В Android нельзя удалить индексированное содержимое из результатов поиска.

## <a name="responding-to-a-deep-link"></a>Ответ на прямую ссылку

Когда пользователь выбирает индексированное содержимое, отображаемое в результатах поиска, класс `App` приложения получает запрос на обработку `Uri`, содержащегося в индексированном содержимом. Этот запрос может обрабатываться в переопределении [`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)), как показано в следующем примере кода:

```csharp
public class App : Application
{
    ...
    protected override async void OnAppLinkRequestReceived(Uri uri)
    {
        string appDomain = "http://" + App.AppName.ToLowerInvariant() + "/";
        if (!uri.ToString().ToLowerInvariant().StartsWith(appDomain, StringComparison.Ordinal))
            return;

        string pageUrl = uri.ToString().Replace(appDomain, string.Empty).Trim();
        var parts = pageUrl.Split('?');
        string page = parts[0];
        string pageParameter = parts[1].Replace("id=", string.Empty);

        var formsPage = Activator.CreateInstance(Type.GetType(page));
        var todoItemPage = formsPage as TodoItemPage;
        if (todoItemPage != null)
        {
            var todoItem = await App.Database.GetItemAsync(int.Parse(pageParameter));
            todoItemPage.BindingContext = todoItem;
            await MainPage.Navigation.PushAsync(formsPage as Page);
        }
        base.OnAppLinkRequestReceived(uri);
    }
}
```

Метод [`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) проверяет, предназначен ли `Uri` для приложения перед преобразованием `Uri` в страницу, на которую будет выполнен переход, и параметр для передачи на страницу. Создается экземпляр страницы для перехода и извлекается `TodoItem`, представленный параметром страницы. Затем для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы, на которую будет выполнен переход, задается значение `TodoItem`. Это гарантирует, что при отображении `TodoItemPage` методом [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)) он будет отображать `TodoItem`, `ID` которого содержится в прямой ссылке.

## <a name="making-content-available-for-search-indexing"></a>Предоставление доступа к содержимому для индексирования поиска

При каждом отображении страницы, представленной прямой ссылкой, для свойства [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) может задаваться значение `true`. В iOS и Android это делает экземпляр [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) доступным для индексирования поиска, и только в iOS экземпляр `AppLinkEntry` также становится доступным для функции Handoff. Дополнительные сведения о функции Handoff см. в этой [статье](~/ios/platform/handoff.md).

В следующем примере кода демонстрируется задание для свойства [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) значения `true` в переопределении [`Page.OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing):

```csharp
protected override void OnAppearing()
{
    appLink = GetAppLink(BindingContext as TodoItem);
    if (appLink != null)
    {
        appLink.IsLinkActive = true;
    }
}
```

Аналогичным образом, при каждом переходе со страницы, представленной прямой ссылкой, для свойства [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) может задаваться значение `false`. В iOS и Android это прекращает объявление экземпляра [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) для индексирования поиска, и только в iOS — объявление экземпляра `AppLinkEntry` для функции Handoff. Это поведение может осуществляться переопределением [`Page.OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing), как показано в следующем примере кода:

```csharp
protected override void OnDisappearing()
{
    if (appLink != null)
    {
        appLink.IsLinkActive = false;
    }
}
```

## <a name="providing-data-to-handoff"></a>Предоставление данных в функцию Handoff

В iOS относящиеся к приложению данные могут сохраняться при индексировании страницы. Это достигается путем добавления данных в коллекцию [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues), являющуюся `Dictionary<string, string>` для хранения пар "ключ-значение", которые используются функцией Handoff. Функция Handoff позволяет пользователю начать действие на одном из своих устройств и продолжить его на других устройствах (это зависит от учетной записи iCloud). Ниже приведен пример кода для сохранения пар "ключ-значение", определяемых приложением:

```csharp
var pageLink = new AppLinkEntry
{
    ...
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Значения, хранящиеся в коллекции [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues), будут сохраняться в метаданных для индексированной страницы и восстанавливаться, когда пользователь коснется результата поиска, содержащего прямую ссылку (или при использовании функции Handoff для просмотра содержимого на другом устройстве, где пользователь выполнил вход).

Кроме того, можно указать значения для следующих ключей:

- `contentType` — тип `string`, задающий идентификатор универсального типа для индексированного содержимого. Рекомендуемым соглашением для этого значения является имя типа страницы, содержащей индексированное содержимое.
- `associatedWebPage` — тип `string`, представляющий веб-страницу для посещения, если индексированное содержимое также можно просмотреть в Интернете или если приложение поддерживает прямые ссылки Safari.
- `shouldAddToPublicIndex` — тип `string` со значением `true` или `false`, который определяет, следует ли добавлять индексированное содержимое в индекс общедоступного облака Apple, которое затем будет представлено пользователям, еще не установившим приложение на своем устройстве iOS. Учтите, что задание содержимого для общедоступного индексирования не означает, что оно будет автоматически добавляться в индекс общедоступного облака Apple. Дополнительные сведения см. в статье, посвященной [общедоступному индексированию поиска](~/ios/platform/search/nsuseractivity.md). Обратите внимание, что для этого ключа следует задать значение `false` при добавлении персональных данных в коллекцию [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues).

> [!NOTE]
> Коллекция `KeyValues` не используется для платформы Android.

Дополнительные сведения о функции Handoff см. в этой [статье](~/ios/platform/handoff.md).

## <a name="summary"></a>Сводка

В этой статье было описано, как использовать индексирование приложения и создание прямых ссылок, чтобы предоставить доступ к доступному для поиска содержимому приложения Xamarin.Forms на устройствах iOS и Android. Индексирование приложения позволяет приложениям, которые в противном случае забываются после нескольких использований, оставаться релевантными благодаря отображению в результатах поиска.

## <a name="related-links"></a>Связанные ссылки

- [Создание прямых ссылок (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/deeplinking)
- [API поиска iOS](~/ios/platform/search/index.md)
- [Связывание приложений в Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
