---
title: Отправка и получение push-уведомлений с помощью Центров уведомлений Azure и Xamarin.Forms
description: В этой статье объясняется, как использовать Центры уведомлений Azure для отправки push-уведомлений в приложения Xamarin.Forms на различных платформах.
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
- Firebase
ms.openlocfilehash: 5f7b83c1fc907de790b382aabde0c5a957e5a8bb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84565425"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Отправка и получение push-уведомлений с помощью Центров уведомлений Azure и Xamarin.Forms

[![Скачать пример](~/media/shared/download.png)Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurenotificationhub/)

Push-уведомления передают информацию из серверной системы в мобильное приложение. Платформы Apple, Google и других поставщиков имеют свои собственные службы push-уведомлений (Push Notification Services, PNS). Центры уведомлений Azure позволяют вашему серверному приложению взаимодействовать с единым центром, который возьмет на себя отправку уведомлений всем PNS-службам на различных платформах.

Принципы интеграции Центров уведомлений Azure в мобильные приложения описаны в следующих разделах:

1. [Настройка служб push-уведомлений и Центра уведомлений Azure](#set-up-push-notification-services-and-azure-notification-hub).
1. [Общие сведения об использовании шаблонов и тегов](#register-templates-and-tags-with-the-azure-notification-hub).
1. [Создание кроссплатформенного приложения Xamarin.Forms](#xamarinforms-application-functionality).
1. [Настройка собственного проекта Android для использования push-уведомлений](#configure-the-android-application-for-notifications).
1. [Настройка собственного проекта iOS для использования push-уведомлений](#configure-ios-for-notifications).
1. [Тестирование уведомлений с помощью Центра уведомлений Azure](#test-notifications-in-the-azure-portal).
1. [Создание внутреннего приложения для отправки уведомлений](#create-a-notification-dispatcher).

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>Настройка служб push-уведомлений и Центра уведомлений Azure

Интеграция Центров уведомлений Azure с мобильным приложением Xamarin.Forms осуществляется примерно так же, как и с собственным приложением Xamarin. Подготовьте приложение Firebase Cloud Messaging (FCM) согласно инструкциям для консоли Firebase в разделе [Отправка push-уведомлений в приложения Xamarin.Android с помощью Центров уведомлений Azure](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging). Следуя руководству по Xamarin.Android, выполните следующие действия:

1. Задайте имя для пакета Android, например `com.xamarin.notifysample`, которое используется в примере.
1. Загрузите `google-services.json` из консоли Firebase. Этот файл нужно будет добавить в приложение Android в одном из следующих этапов.
1. Создайте экземпляр Центра уведомлений Azure и присвойте ему имя. В этой статье и примере в качестве имени центра используется `xdocsnotificationhub`.
1. Скопируйте **ключ сервера** FCM и сохраните его как **ключ API** в разделе **Google (GCM/FCM)** Центра уведомлений Azure.

На следующем снимке экрана показана конфигурация платформы Google в Центре уведомлений Azure:

![Снимок экрана с конфигурацией Центра уведомлений Azure для Google](azure-notification-hub-images/fcm-notification-hub-config.png "Настройка Центра уведомлений Azure для Google")

Для установки на устройствах с iOS потребуется компьютер под управлением macOS. Настройте службы push-уведомлений Apple (APNS), следуя начальным инструкциям в разделе [Отправка push-уведомлений в приложения Xamarin.iOS с помощью Центров уведомлений Azure](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file). Следуя руководству по Xamarin.iOS, выполните следующие действия:

1. Задайте идентификатор пакета iOS. В этой статье и примере в качестве идентификатора пакета используется `com.xamarin.notifysample`.
1. Создайте файл запроса на подпись сертификата (CSR) и используйте его для создания сертификата push-уведомлений.
1. Загрузите сертификат push-уведомлений в разделе **Apple (APNS)** Центра уведомлений Azure.

На следующем снимке экрана показана конфигурация платформы Apple в Центре уведомлений Azure:

![Снимок экрана с конфигурацией Центра уведомлений Azure для Apple](azure-notification-hub-images/apns-notification-hub-config.png "Настройка Центра уведомлений Azure для Apple")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Регистрация шаблонов и тегов в Центре уведомлений Azure

Центр уведомлений Azure требует регистрации в центре, определения шаблонов и подписки на теги для мобильных приложений. Регистрация связывает дескриптор PNS для конкретной платформы с идентификатором в Центре уведомлений Azure. Дополнительные сведения о регистрациях см. в статье [Управление регистрацией](/azure/notification-hubs/notification-hubs-push-notification-registration-management).

Шаблоны позволяют устройствам задавать параметризованные шаблоны сообщений. Входящие сообщения можно настраивать для отдельных устройств и тегов. Дополнительные сведения о шаблонах см. в статье [Шаблоны](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).

Теги можно использовать для подписки на определенные категории сообщений, например на новости, спорт или погоду. Для простоты в примере приложения определяется шаблон по умолчанию с одним параметром `messageParam` и одним тегом `default`. В более сложных системах можно применять теги для отправки персонализированных уведомлений конкретным пользователям на различные устройства. Дополнительные сведения о тегах см. в статье [Маршрутизация и выражения тегов](/azure/notification-hubs/notification-hubs-tags-segment-push-message).

Для получения сообщений каждое платформенное приложение должно выполнить следующие действия:

1. Получить дескриптор PNS или токен от PNS-службы платформы.
1. Зарегистрировать дескриптор PNS в Центре уведомлений Azure.
1. Задать шаблон, содержащий те же параметры, что используются в исходящих сообщениях.
1. Подписаться на тег, который используется исходящими сообщениями.

Более подробное описание этих действий для каждой платформы см. в разделах [Настройка приложения Android для уведомлений](#configure-the-android-application-for-notifications) и [Настройка iOS для уведомлений](#configure-ios-for-notifications).

## <a name="xamarinforms-application-functionality"></a>Функциональные возможности приложения Xamarin.Forms

Пример приложения Xamarin.Forms отображает список сообщений в push-уведомлениях. Для этого используется метод `AddMessage`, который добавляет указанное сообщение push-уведомления в пользовательский интерфейс. Этот метод также предотвращает добавление в интерфейс дублируемых сообщений, и он выполняется в основном потоке, чтобы его можно было вызывать из любого потока. В следующем примере кода показан метод `AddMessage`:

```csharp
public void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (messageDisplay.Children.OfType<Label>().Where(c => c.Text == message).Any())
        {
            // Do nothing, an identical message already exists
        }
        else
        {
            Label label = new Label()
            {
                Text = message,
                HorizontalOptions = LayoutOptions.CenterAndExpand,
                VerticalOptions = LayoutOptions.Start
            };
            messageDisplay.Children.Add(label);
        }
    });
}
```

Пример приложения содержит файл `AppConstants.cs`, который определяет свойства, используемые платформенными проектами. В этом файле нужно указать значения из вашего Центра уведомлений Azure. В следующем коде демонстрируется файл `AppConstants.cs`:

```csharp
public static class AppConstants
{
    public static string NotificationChannelName { get; set; } = "XamarinNotifyChannel";
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string ListenConnectionString { get; set; } = "< Insert your DefaultListenSharedAccessSignature >";
    public static string DebugTag { get; set; } = "XamarinNotify";
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string FCMTemplateBody { get; set; } = "{\"data\":{\"message\":\"$(messageParam)\"}}";
    public static string APNTemplateBody { get; set; } = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";
}
```

Чтобы подключить приложение из примера к Центру уведомлений Azure, задайте в `AppConstants` следующие значения:

* `NotificationHubName`. Используйте имя Центра уведомлений Azure, созданного на портале Azure.
* `ListenConnectionString`. Это значение указано в Центре уведомлений Azure в разделе **Политики доступа**.

На следующем снимке экрана показано, где находятся эти значения на портале Azure:

![Снимок экрана с политикой доступа Центра уведомлений Azure](azure-notification-hub-images/notification-hub-access-policy.png "Политика доступа Центра уведомлений Azure")

## <a name="configure-the-android-application-for-notifications"></a>Настройка приложения Android для уведомлений

Чтобы настроить приложение Android для получения и обработки уведомлений, выполните следующие действия:

1. Укажите **Имя пакета** Android, совпадающее с именем пакета в консоли Firebase.
1. Установите следующие пакеты NuGet для взаимодействия с Google Play, Firebase и Центрами уведомлений Azure:
    1. `Xamarin.GooglePlayServices.Base`
    1. `Xamarin.Firebase.Messaging`
    1. `Xamarin.Azure.NotificationHubs.Android`
1. Скопируйте в проект файл `google-services.json`, который вы скачали в процессе установки FCM, и в качестве "Действия сборки" задайте `GoogleServicesJson`.
1. [Настройте](#configure-android-manifest) `AndroidManifest.xml` для обмена данными с Firebase.
1. [Переопределите](#override-firebasemessagingservice-to-handle-messages) `FirebaseMessagingService` для обработки сообщений.
1. [Добавьте](#add-incoming-notifications-to-the-xamarinforms-ui) входящие уведомления в пользовательский интерфейс Xamarin.Forms.

> [!NOTE]
> Действие сборки `GoogleServicesJson` является частью пакета NuGet `Xamarin.GooglePlayServices.Base`. Visual Studio 2019 задает доступные действия сборки во время установки. Если действие сборки `GoogleServicesJson` не отображается, после установки пакетов NuGet перезапустите Visual Studio 2019.

### <a name="configure-android-manifest"></a>Настройка манифеста Android

Элементы `receiver` в элементе `application` позволяют приложению взаимодействовать с Firebase. Элементы `uses-permission` позволяют приложению обрабатывать сообщения и регистрироваться в Центре уведомлений Azure. Полностью файл `AndroidManifest.xml` должен выглядеть как в приведенном ниже примере:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="YOUR_PACKAGE_NAME" android:installLocation="auto">
  <uses-sdk android:minSdkVersion="21" />
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
  <application android:label="Notification Hub Sample">
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
      <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
      </intent-filter>
    </receiver>
  </application>
</manifest>
```

### <a name="override-firebasemessagingservice-to-handle-messages"></a>Переопределите `FirebaseMessagingService` для обработки сообщений

Для регистрации в Firebase и обработки сообщений создайте подкласс `FirebaseMessagingService`. Приложение в примере определяет класс `FirebaseService`, который создает подкласс `FirebaseMessagingService`. Класс помечен атрибутом `IntentFilter`, включающим фильтр `com.google.firebase.MESSAGING_EVENT`. Этот фильтр позволяет Android передавать классу входящие сообщения для обработки:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
    // ...
}

```

При запуске приложения пакет SDK для Firebase будет автоматически запрашивать уникальный идентификатор токена с сервера Firebase. После выполнения запроса будет вызываться метод `OnNewToken` для класса `FirebaseService`. Проект в примере переопределяет этот метод и регистрирует токен в Центрах уведомлений Azure:

```csharp
public override void OnNewToken(string token)
{
    // NOTE: save token instance locally, or log if desired

    SendRegistrationToServer(token);
}

void SendRegistrationToServer(string token)
{
    try
    {
        NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

        // register device with Azure Notification Hub using the token from FCM
        Registration registration = hub.Register(token, AppConstants.SubscriptionTags);

        // subscribe to the SubscriptionTags list with a simple template.
        string pnsHandle = registration.PNSHandle;
        TemplateRegistration templateReg = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
    }
    catch (Exception e)
    {
        Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
    }
}
```

Метод `SendRegistrationToServer` регистрирует устройство в Центре уведомлений Azure и подписывается на теги с помощью шаблона. Для простоты в примере приложения определяется один тег с именем `default` и шаблон с единственным параметром `messageParam` в файле `AppConstants.cs`. Дополнительные сведения о регистрации, тегах и шаблонах см. в разделе [Регистрация шаблонов и тегов в Центре уведомлений Azure](#register-templates-and-tags-with-the-azure-notification-hub).

При получении сообщения будет вызываться метод `OnMessageReceived` для класса `FirebaseService`:

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    base.OnMessageReceived(message);
    string messageBody = string.Empty;

    if (message.GetNotification() != null)
    {
        messageBody = message.GetNotification().Body;
    }

    // NOTE: test messages sent via the Azure portal will be received here
    else
    {
        messageBody = message.Data.Values.First();
    }

    // convert the incoming message to a local notification
    SendLocalNotification(messageBody);

    // send the incoming message directly to the MainPage
    SendMessageToMainPage(messageBody);
}

void SendLocalNotification(string body)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    intent.PutExtra("message", body);

    //Unique request code to avoid PendingIntent collision.
    var requestCode = new Random().Next();
    var pendingIntent = PendingIntent.GetActivity(this, requestCode, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new NotificationCompat.Builder(this)
        .SetContentTitle("XamarinNotify Message")
        .SetSmallIcon(Resource.Drawable.ic_launcher)
        .SetContentText(body)
        .SetAutoCancel(true)
        .SetShowWhen(false)
        .SetContentIntent(pendingIntent);

    if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
    {
        notificationBuilder.SetChannelId(AppConstants.NotificationChannelName);
    }

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}

void SendMessageToMainPage(string body)
{
    (App.Current.MainPage as MainPage)?.AddMessage(body);
}
```

Входящие сообщения преобразуются в локальное уведомление с помощью метода `SendLocalNotification`. Этот метод создает объект `Intent` и помещает в `Intent` содержимое сообщения в качестве `string` `Extra`. Когда пользователь нажимает на локальное уведомление, независимо от того, работает ли приложение на переднем плане или в фоновом режиме, запускается объект `MainActivity`, который получает доступ к содержимому сообщения через объект `Intent`.

В примере с локальным уведомлением и объектом `Intent` необходимо, чтобы пользователь нажал на уведомление. Это оптимально, когда пользователь должен предпринять действие до изменения состояния приложения. Однако в некоторых случаях вам может быть нужно обратиться к данным сообщений без вмешательства пользователя. В предыдущем примере сообщение также отправляется непосредственно в текущий экземпляр `MainPage` с помощью метода `SendMessageToMainPage`. Если в рабочей среде для одного и того же типа сообщений реализованы оба этих метода, объект `MainPage` будет получать дублируемые сообщения, когда пользователь нажимает на уведомление.

> [!NOTE]
> Приложение Android будет получать push-уведомления, только если оно работает на переднем плане или в фоновом режиме. Чтобы push-уведомления доставлялись, когда основной процесс `Activity` не выполняется, необходимо реализовать службу, которая выходит за рамки этого примера. Дополнительные сведения см. в статье [Создание служб Android](/xamarin/android/app-fundamentals/services/).

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Добавление входящих уведомлений в пользовательский интерфейс Xamarin.Forms

Классу `MainActivity` необходимо получить разрешение на обработку уведомлений и управление данными входящих сообщений. В следующем коде показана полная реализация класса `MainActivity`:

```csharp
[Activity(Label = "NotificationHubSample", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, LaunchMode = LaunchMode.SingleTop)]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);

        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App());

        if (!IsPlayServiceAvailable())
        {
            throw new Exception("This device does not have Google Play Services and cannot receive push notifications.");
        }

        CreateNotificationChannel();
    }

    protected override void OnNewIntent(Intent intent)
    {
        if (intent.Extras != null)
        {
            var message = intent.GetStringExtra("message");
            (App.Current.MainPage as MainPage)?.AddMessage(message);
        }

        base.OnNewIntent(intent);
    }

    bool IsPlayServiceAvailable()
    {
        int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.Success)
        {
            if (GoogleApiAvailability.Instance.IsUserResolvableError(resultCode))
                Log.Debug(AppConstants.DebugTag, GoogleApiAvailability.Instance.GetErrorString(resultCode));
            else
            {
                Log.Debug(AppConstants.DebugTag, "This device is not supported");
            }
            return false;
        }
        return true;
    }

    void CreateNotificationChannel()
    {
        // Notification channels are new as of "Oreo".
        // There is no need to create a notification channel on older versions of Android.
        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            var channelName = AppConstants.NotificationChannelName;
            var channelDescription = String.Empty;
            var channel = new NotificationChannel(channelName, channelName, NotificationImportance.Default)
            {
                Description = channelDescription
            };

            var notificationManager = (NotificationManager)GetSystemService(NotificationService);
            notificationManager.CreateNotificationChannel(channel);
        }
    }
}
```

Атрибут `Activity` в качестве `LaunchMode` (режима запуска) приложения устанавливает `SingleTop`. Такой режим запуска сообщает ОС Android, что у данного действия может быть только один экземпляр. В этом режиме запуска входящие данные `Intent` направляются в метод `OnNewIntent`, который извлекает данные сообщений и отправляет их экземпляру `MainPage` через метод `AddMessage`. Если приложение использует другой режим запуска, оно должно выполнять обработку данных `Intent` иным образом.

Метод `OnCreate` использует вспомогательный метод `IsPlayServiceAvailable`, чтобы убедиться, что устройство поддерживает службу Google Play. Эмуляторы или устройства, которые не поддерживают службу Google Play, не могут получать push-уведомления от Firebase.

## <a name="configure-ios-for-notifications"></a>Настройка iOS для уведомлений

Процедура настройки приложения iOS для получения уведомлений следующая:

1. Настройте **Идентификатор пакета** в файле `Info.plist`, указав значение, используемое в профиле подготовки.
1. Добавьте параметр **Включить push-уведомления** в файл `Entitlements.plist`.
1. Добавьте в проект пакет NuGet `Xamarin.Azure.NotificationHubs.iOS`.
1. [Зарегистрируйтесь](#register-for-notifications-with-apns) для получения уведомлений в APNS.
1. [Зарегистрируйте](#register-with-azure-notification-hub-and-subscribe-to-tags) приложение в Центре уведомлений Azure и подпишитесь на теги.
1. [Добавьте](#add-apns-notifications-to-xamarinforms-ui) уведомления APNS в пользовательский интерфейс Xamarin.Forms.

На следующем снимке экрана показан выбор параметра **Включить push-уведомления** для файла `Entitlements.plist` в Visual Studio:

![Снимок экрана с правом на получение push-уведомлений](azure-notification-hub-images/push-notification-entitlement.png "Право на получение push-уведомлений")

### <a name="register-for-notifications-with-apns"></a>Регистрация для получения уведомлений в APNS

Чтобы зарегистрироваться для получения удаленных уведомлений, необходимо переопределить метод `FinishedLaunching` в файле `AppDelegate.cs`. Процесс регистрации зависит от того, какая версия iOS используется на устройстве. Проект iOS в примере приложения переопределяет метод `FinishedLaunching` таким образом, чтобы он вызывал `RegisterForRemoteNotifications`, как показано в следующем примере:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();
    LoadApplication(new App());

    base.FinishedLaunching(app, options);

    RegisterForRemoteNotifications();

    return true;
}

void RegisterForRemoteNotifications()
{
    // register for remote notifications based on system version
    if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
    {
        UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert |
            UNAuthorizationOptions.Sound |
            UNAuthorizationOptions.Sound,
            (granted, error) =>
            {
                if (granted)
                    InvokeOnMainThread(UIApplication.SharedApplication.RegisterForRemoteNotifications);
            });
    }
    else if (UIDevice.CurrentDevice.CheckSystemVersion(8, 0))
    {
        var pushSettings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
        new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(pushSettings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();
    }
    else
    {
        UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
        UIApplication.SharedApplication.RegisterForRemoteNotificationTypes(notificationTypes);
    }
}
```

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Регистрация в Центре уведомлений Azure и подписка на теги

Когда устройство зарегистрируется для получения удаленных уведомлений при выполнении метода `FinishedLaunching`, iOS вызовет метод `RegisteredForRemoteNotifications`. Этот метод необходимо переопределить для выполнения следующих действий:

1. Создание экземпляра `SBNotificationHub`.
1. Отмена существующих регистраций.
1. Регистрация устройства в центре уведомлений.
1. Подписка на определенные теги с помощью шаблона.

Дополнительные сведения о регистрации устройства, шаблонах и тегах см. в разделе [Регистрация шаблонов и тегов в Центре уведомлений Azure](#register-templates-and-tags-with-the-azure-notification-hub). В следующем коде показана регистрация устройства и шаблонов:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAll(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNative(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplate(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                if (errorCallback != null)
                {
                    Debug.WriteLine($"RegisterTemplateAsync error: {errorCallback}");
                }
            }
        });
    });
}
```

> [!NOTE]
> Регистрация для получения удаленных уведомлений в некоторых ситуациях может завершиться сбоем, например при отсутствии сетевого подключения. Для обработки случаев сбоя регистрации можно переопределить метод `FailedToRegisterForRemoveNotifications`.

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Добавление уведомлений APNS в пользовательский интерфейс Xamarin.Forms.

Когда устройство получает удаленное уведомление, iOS вызывает метод `ReceivedRemoteNotification`. Код JSON входящего сообщения преобразуется в объект `NSDictionary`, а метод `ProcessNotification` извлекает значения из словаря и отправляет их экземпляру Xamarin.Forms `MainPage`. Метод `ReceivedRemoteNotifications` переопределяется для вызова `ProcessNotification`, как показано в следующем примере кода:

```csharp
public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
{
    ProcessNotification(userInfo, false);
}

void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
{
    // make sure we have a payload
    if (options != null && options.ContainsKey(new NSString("aps")))
    {
        // get the APS dictionary and extract message payload. Message JSON will be converted
        // into a NSDictionary so more complex payloads may require more processing
        NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
        string payload = string.Empty;
        NSString payloadKey = new NSString("alert");
        if (aps.ContainsKey(payloadKey))
        {
            payload = aps[payloadKey].ToString();
        }

        if (!string.IsNullOrWhiteSpace(payload))
        {
            (App.Current.MainPage as MainPage)?.AddMessage(payload);
        }

    }
    else
    {
        Debug.WriteLine($"Received request to process notification but there was no payload.");
    }
}
```

## <a name="test-notifications-in-the-azure-portal"></a>Тестирование уведомлений на портале Azure

Центры уведомлений Azure позволяют проверить, может ли ваше приложение получать тестовые сообщения. Вы можете выбрать целевую платформу и отправить сообщение из раздела **Тестовая отправка** в центре уведомлений. Если задать для параметра **Отправить в выражение тега** значение `default`, сообщения будут отправляться в приложения, которые зарегистрировали шаблон для тега `default`. При нажатии на кнопку **Отправить** создается отчет с указанием количества устройств, получивших это сообщение. На следующем снимке экрана показан тест уведомлений Android на портале Azure:

![Снимок экрана с тестовым сообщением Центра уведомлений Azure](azure-notification-hub-images/azure-notification-hub-test-send.png "Тестовое сообщение Центра уведомлений Azure")

### <a name="testing-tips"></a>Советы по тестированию

1. Для проверки приложения на получение push-уведомлений необходимо использовать физическое устройство. Виртуальные устройства с Android и iOS могут не получать push-уведомления из-за неверных настроек.
1. Приложение Android в примере регистрирует свой токен и шаблоны один раз при выдаче токена Firebase. Во время тестирования может потребоваться запросить новый токен и повторить регистрацию в Центре уведомлений Azure. Для этого лучше всего очистить проект, удалить папки `bin` и `obj`, а затем удалить приложение с устройства и выполнить сборку и развертывание заново.
1. Многие элементы потока push-уведомлений выполняются асинхронно. Это может привести к пропуску точек останова или нарушению порядка их прохождения. Используйте журналы отладки или устройств для трассировки выполнения без прерывания потока приложения. Для фильтрации журнала устройств Android используйте тег `DebugTag`, указанный в `Constants`.
1. При остановке отладки в Visual Studio приложение принудительно закрывается. Все получатели сообщений или другие службы, запущенные в процессе отладки, будут закрыты и не ответят на события сообщений.

## <a name="create-a-notification-dispatcher"></a>Создание диспетчера уведомлений

Центры уведомлений Azure позволяют серверным приложениям отправлять уведомления устройствам на базе различных платформ. В примере показана отправка уведомлений с использованием консольного приложения. Приложение включает файл `DispatcherConstants.cs`, который определяет следующие свойства:

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

Файл `DispatcherConstants.cs` необходимо настроить в соответствии с конфигурацией Центра уведомлений Azure. Значение свойства `SubscriptionTags` должно соответствовать значениям, используемым в клиентских приложениях. Свойство `NotificationHubName` — это имя вашего экземпляра Центра уведомлений Azure. Свойство `FullAccessConnectionString` — это ключ доступа, указанный в **Политиках доступа** вашего центра уведомлений. На следующем снимке экрана показано местонахождение свойств `NotificationHubName` и `FullAccessConnectionString` на портале Azure:

![Снимок экрана с именем Центра уведомлений Azure и свойством FullAccessConnectionString](azure-notification-hub-images/notification-hub-full-access-policy.png "Имя Центра уведомлений Azure и свойство FullAccessConnectionString")

Консольное приложение циклически обрабатывает каждое значение `SubscriptionTags` и отправляет подписчикам уведомления, используя экземпляр класса `NotificationHubClient`. Следующий код показывает класс консольного приложения `Program`:

``` csharp
class Program
{
    static int messageCount = 0;

    static void Main(string[] args)
    {
        Console.WriteLine($"Press the spacebar to send a message to each tag in {string.Join(", ", DispatcherConstants.SubscriptionTags)}");
        WriteSeparator();
        while (Console.ReadKey().Key == ConsoleKey.Spacebar)
        {
            SendTemplateNotificationsAsync().GetAwaiter().GetResult();
        }
    }

    private static async Task SendTemplateNotificationsAsync()
    {
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(DispatcherConstants.FullAccessConnectionString, DispatcherConstants.NotificationHubName);
        Dictionary<string, string> templateParameters = new Dictionary<string, string>();

        messageCount++;

        // Send a template notification to each tag. This will go to any devices that
        // have subscribed to this tag with a template that includes "messageParam"
        // as a parameter
        foreach (var tag in DispatcherConstants.SubscriptionTags)
        {
            templateParameters["messageParam"] = $"Notification #{messageCount} to {tag} category subscribers!";
            try
            {
                await hub.SendTemplateNotificationAsync(templateParameters, tag);
                Console.WriteLine($"Sent message to {tag} subscribers.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Failed to send template notification: {ex.Message}");
            }
        }

        Console.WriteLine($"Sent messages to {DispatcherConstants.SubscriptionTags.Length} tags.");
        WriteSeparator();
    }

    private static void WriteSeparator()
    {
        Console.WriteLine("==========================================================================");
    }
}
```

При запуске примера консольного приложения для отправки сообщений можно использовать клавишу ПРОБЕЛ. Устройства с клиентскими приложениями должны получать нумерованные уведомления, если они настроены должным образом.

## <a name="related-links"></a>Связанные ссылки

* [Шаблоны push-уведомлений](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).
* [Управление регистрацией устройств](/azure/notification-hubs/notification-hubs-push-notification-registration-management).
* [Маршрутизация и выражения тегов](/azure/notification-hubs/notification-hubs-tags-segment-push-message).
* [Руководство по Центрам уведомлений Azure для Xamarin.Android](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm).
* [Руководство по Центрам уведомлений Azure для Xamarin.iOS](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started).
