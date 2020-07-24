---
title: Социальные платформы в Xamarin. iOS
description: Социальные платформы предоставляют унифицированный API для взаимодействия с социальными сетями, включая Twitter и Facebook, а также Синавеибо для пользователей в Китае.
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: a77b5cd33710a7a8755441efc8b7134d82855c2a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937973"
---
# <a name="social-framework-in-xamarinios"></a>Социальные платформы в Xamarin. iOS

_Социальные платформы предоставляют унифицированный API для взаимодействия с социальными сетями, включая Twitter и Facebook, а также Синавеибо для пользователей в Китае._

Использование социальных сетей позволяет приложениям взаимодействовать с социальными сетями из одного API без необходимости управлять проверкой подлинности. Он включает в себя контроллер представления, предоставляемый системой, а также абстракцию, которая позволяет использовать API-интерфейсы социальных сетей по протоколу HTTP.

## <a name="connecting-to-twitter"></a>Подключение к Twitter

### <a name="twitter-account-settings"></a>Параметры учетной записи Twitter

Чтобы подключиться к Twitter с помощью социальной платформы, необходимо настроить учетную запись в параметрах устройства, как показано ниже.

 [![Параметры учетной записи Twitter](social-framework-images/twitter01.png)](social-framework-images/twitter01.png#lightbox)

После того как учетная запись введена и проверена с помощью Twitter, любое приложение на устройстве, которое использует классы социальных сетей для доступа к Twitter, будет использовать эту учетную запись.

### <a name="sending-tweets"></a>Отправка твитов

В состав платформы социальных сетей входит контроллер `SLComposeViewController` с именем, который представляет собой системное представление для редактирования и отправки твитов. На следующем снимке экрана показан пример этого представления:

 [![На этом снимке экрана показан пример Слкомпосевиевконтроллер](social-framework-images/twitter02.png)](social-framework-images/twitter02.png#lightbox)

Чтобы использовать `SLComposeViewController` с Twitter, необходимо создать экземпляр контроллера, вызвав `FromService` метод с помощью метода, `SLServiceType.Twitter` как показано ниже:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

После `SLComposeViewController` возвращения экземпляра его можно использовать для предоставления пользовательского интерфейса для публикации в Twitter. Тем не менее, первое, что нужно сделать, — это проверить доступность социальных сетей, Twitter в данном случае, вызвав `IsAvailable` :

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController`никогда не отправляет твит непосредственно без взаимодействия с пользователем. Однако его можно инициализировать с помощью следующих методов:

- `SetInitialText`— Добавляет начальный текст для отображения в твите.
- `AddUrl`— Добавляет URL-адрес твита.
- `AddImage`— Добавляет изображение в твит.

После инициализации вызов `PresentVIewController` отображает представление, созданное `SLComposeViewController` . Пользователь может при необходимости изменить и отправить твит или отменить его отправку. В любом случае контроллер должен быть закрыт в `CompletionHandler` , где результат также можно проверить, чтобы увидеть, был ли твит отправлен или отменен, как показано ниже:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>Пример твитов

В следующем примере кода демонстрируется использование `SLComposeViewController` для представления, используемого для отправки твита:

```csharp
using System;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _twitterComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
        #endregion

        #region Computed Properties
        public bool isTwitterAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Twitter); }
        }

        public SLComposeViewController TwitterComposer {
            get { return _twitterComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {

        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            SendTweet.Enabled = isTwitterAvailable;
        }
        #endregion

        #region Actions
        partial void SendTweet_TouchUpInside (UIButton sender)
        {
            // Set initial message
            TwitterComposer.SetInitialText ("Hello Twitter!");
            TwitterComposer.AddImage (UIImage.FromFile ("Icon.png"));
            TwitterComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (TwitterComposer, true, null);
        }
        #endregion
    }
}
```

### <a name="calling-twitter-api"></a>Вызов API Twitter

В составе социальных сетей также включена поддержка выполнения HTTP-запросов к социальным сетям. Он инкапсулирует запрос в `SLRequest` класс, который используется для работы с API-интерфейсом конкретной социальной сети.

Например, следующий код выполняет запрос к Twitter, чтобы получить открытую временную шкалу (путем расширения кода, приведенного выше):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _twitterAccount;
#endregion

#region Computed Properties
public ACAccount TwitterAccount {
    get { return _twitterAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    SendTweet.Enabled = isTwitterAvailable;
    RequestTwitterTimeline.Enabled = false;

    // Initialize Twitter Account access
    var accountStore = new ACAccountStore ();
    var accountType = accountStore.FindAccountType (ACAccountType.Twitter);

    // Request access to Twitter account
    accountStore.RequestAccess (accountType, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestTwitterTimeline.Enabled = true;
            });
        }
    });
}
#endregion

#region Actions
partial void RequestTwitterTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
    var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = TwitterAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Рассмотрим этот код подробно. Во-первых, он получает доступ к хранилищу учетных записей и возвращает тип учетной записи Twitter:

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

Затем он запрашивает у пользователя, может ли приложение получить доступ к своей учетной записи Twitter, и, если предоставлен доступ, учетная запись загружается в память и обновляется ИП.

```csharp
// Request access to Twitter account
accountStore.RequestAccess (accountType, (granted, error) => {
    // Allowed by user?
    if (granted) {
        // Get account
        _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
        InvokeOnMainThread (() => {
            // Update UI
            RequestTwitterTimeline.Enabled = true;
        });
    }
});
```

Когда пользователь запрашивает данные временной шкалы (путем нажатия кнопки в пользовательском интерфейсе), приложение сначала формирует запрос на доступ к данным из Twitter:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```

Этот пример ограничивает возвращаемые результаты до десяти последних записей, включая `?count=10` в URL-адрес. Наконец, он присоединяет запрос к учетной записи Twitter (которая была загружена ранее) и выполняет вызов Twitter для получения данных:

```csharp
// Request data
request.Account = TwitterAccount;
request.PerformRequest ((data, response, error) => {
    // Was there an error?
    if (error == null) {
        // Was the request successful?
        if (response.StatusCode == 200) {
            // Yes, display it
            InvokeOnMainThread (() => {
                Results.Text = data.ToString ();
            });
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", response.StatusCode);
            });
        }
    } else {
        // No, display error
        InvokeOnMainThread (() => {
            Results.Text = string.Format ("Error: {0}", error);
        });
    }
});
```

Если данные были успешно загружены, отобразятся необработанные данные JSON (как показано в примере ниже).

[![Пример вывода необработанных данных JSON](social-framework-images/twitter03.png)](social-framework-images/twitter03.png#lightbox)

В реальном приложении результаты JSON можно затем проанализировать как обычные и результаты, представленные пользователю. Сведения о том, как анализировать JSON, см. в статье [Введение в веб-службы](~/cross-platform/data-cloud/web-services/index.md) .

## <a name="connecting-to-facebook"></a>Подключение к Facebook

### <a name="facebook-account-settings"></a>Параметры учетной записи Facebook

Подключение к Facebook с помощью социальной инфраструктуры практически идентично процессу, используемому для Twitter, показанного выше. Учетная запись пользователя Facebook должна быть настроена в параметрах устройства, как показано ниже.

[![Параметры учетной записи Facebook](social-framework-images/facebook01.png)](social-framework-images/facebook01.png#lightbox)

После настройки любое приложение на устройстве, использующем социальные платформы, будет использовать эту учетную запись для подключения к Facebook.

### <a name="posting-to-facebook"></a>Публикация в Facebook

Так как социальные платформы — единый API, предназначенный для доступа к нескольким социальным сетям, код остается практически одинаковым независимо от используемой социальной сети.

Например, `SLComposeViewController` можно использовать точно так же, как в приведенном выше примере Twitter, но единственным отличием является переключение на параметры и параметры, относящиеся к Facebook. Например.

```csharp
using System;
using Foundation;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _facebookComposer = SLComposeViewController.FromService (SLServiceType.Facebook);
        #endregion

        #region Computed Properties
        public bool isFacebookAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Facebook); }
        }

        public SLComposeViewController FacebookComposer {
            get { return _facebookComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {

        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            PostToFacebook.Enabled = isFacebookAvailable;
        }
        #endregion

        #region Actions
        partial void PostToFacebook_TouchUpInside (UIButton sender)
        {
            // Set initial message
            FacebookComposer.SetInitialText ("Hello Facebook!");
            FacebookComposer.AddImage (UIImage.FromFile ("Icon.png"));
            FacebookComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (FacebookComposer, true, null);
        }
        #endregion
    }
}
```

При использовании с Facebook `SLComposeViewController` отображает представление, которое выглядит почти так же, как и пример Twitter, в этом случае в качестве заголовка отображается **Facebook** :

[![Слкомпосевиевконтроллер дисплей](social-framework-images/facebook02.png)](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Вызов API Graph Facebook

Как и в примере с Twitter, объект социальной инфраструктуры `SLRequest` можно использовать с API Graph в Facebook. Например, следующий код возвращает сведения из API Graph по учетной записи Xamarin (путем расширения кода, приведенного выше):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _facebookAccount;
#endregion

#region Computed Properties
public ACAccount FacebookAccount {
    get { return _facebookAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    PostToFacebook.Enabled = isFacebookAvailable;
    RequestFacebookTimeline.Enabled = false;

    // Initialize Facebook Account access
    var accountStore = new ACAccountStore ();
    var options = new AccountStoreOptions ();
    var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
    accountType = accountStore.FindAccountType (ACAccountType.Facebook);

    // Request access to Facebook account
    accountStore.RequestAccess (accountType, options, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _facebookAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestFacebookTimeline.Enabled = true;
            });
        }
    });

}
#endregion

#region Actions
partial void RequestFacebookTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl ("https://graph.facebook.com/283148898401104");
    var request = SLRequest.Create (SLServiceKind.Facebook, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = FacebookAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Единственное вещественное различие между этим кодом и представленной выше версией Twitter — требование Facebook для получения идентификатора разработчика или приложения (который можно создать на портале разработчика Facebook), который необходимо задать в качестве варианта при выполнении запроса:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

Если не установить этот параметр (или использовать недопустимый ключ), возникнет либо ошибка, либо данные не будут возвращены.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать социальные платформы для взаимодействия с Twitter и Facebook. В нем показано, где настроить учетные записи для каждой социальной сети в параметрах устройства. Также рассматривается использование `SLComposeViewController` для представления единого представления для публикации в социальных сетях. Кроме того, он рассматривал `SLRequest` класс, который используется для вызова API-интерфейса социальных сетей.

## <a name="related-links"></a>Связанные ссылки

- [СоЦиалфрамеворкдемо (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/socialframeworkdemo)
- [Введение в веб-службы](~/cross-platform/data-cloud/web-services/index.md)
