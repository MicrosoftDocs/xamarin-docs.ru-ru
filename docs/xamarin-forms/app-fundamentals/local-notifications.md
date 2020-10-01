---
title: Локальные уведомления в Xamarin.Forms
description: В этой статье вы узнаете, как отправлять и получать локальные уведомления в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/10/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c62d03e785e02160d9c0f83c3375383d890d62b
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91558275"
---
# <a name="local-notifications-in-no-locxamarinforms"></a>Локальные уведомления в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/local-notifications)

Локальные уведомления — это оповещения, отправляемые приложениями, установленными на мобильном устройстве. Локальные уведомления часто используются для таких функций, как:

- События календаря
- Напоминания
- Триггеры на основе расположения

Каждая платформа обрабатывает создание, отображение и использование локальных уведомлений по-разному. В этой статье вы узнаете, как создать кроссплатформенную абстракцию для отправки и получения локальных уведомлений с помощью Xamarin.Forms.

[![Приложение для локальных уведомлений в iOS и Android](local-notifications-images/local-notifications-msg-cropped.png)](local-notifications-images/local-notifications-msg.png#lightbox)

## <a name="create-a-cross-platform-interface"></a>Создание кросс-платформенного интерфейса

Приложение Xamarin.Forms должно создавать и использовать уведомления, не заботясь о реализации базовой платформы. Следующий интерфейс `INotificationManager` реализуется в общей библиотеке кода и определяет кросс-платформенный API, который приложение может использовать для взаимодействия с уведомлениями:

```csharp
public interface INotificationManager
{
    event EventHandler NotificationReceived;

    void Initialize();

    int ScheduleNotification(string title, string message);

    void ReceiveNotification(string title, string message);
}
```

Этот интерфейс будет реализован в каждом проекте платформы. Событие `NotificationReceived` позволяет приложению обрабатывать входящие уведомления. Метод `Initialize` должен выполнять любую собственную логику платформы, необходимую для подготовки системы уведомлений. Метод `ScheduleNotification` должен отправить уведомление. Метод `ReceiveNotification` должен вызываться базовой платформой при получении сообщения.

## <a name="consume-the-interface-in-no-locxamarinforms"></a>Использование интерфейса в Xamarin.Forms

После создания интерфейса его можно использовать в общем проекте Xamarin.Forms, даже если реализация платформы еще не создана. Пример приложения содержит `ContentPage` с именем **MainPage.xaml** со следующим содержимым:

```xaml
<StackLayout Margin="0,35,0,0"
             x:Name="stackLayout">
    <Label Text="Click the button to create a local notification."
           TextColor="Red"
           HorizontalOptions="Center"
           VerticalOptions="Start" />
    <Button Text="Create Notification"
            HorizontalOptions="Center"
            VerticalOptions="Start"
            Clicked="OnScheduleClick"/>
</StackLayout>
```

Макет содержит элемент `Label` с инструкциями для пользователя и `Button`, который должен запланировать уведомление при нажатии.

Код программной части класса `MainPage` обрабатывает отправку и получение уведомлений:

```csharp
public partial class MainPage : ContentPage
{
    INotificationManager notificationManager;
    int notificationNumber = 0;

    public MainPage()
    {
        InitializeComponent();

        notificationManager = DependencyService.Get<INotificationManager>();
        notificationManager.NotificationReceived += (sender, eventArgs) =>
        {
            var evtData = (NotificationEventArgs)eventArgs;
            ShowNotification(evtData.Title, evtData.Message);
        };
    }

    void OnScheduleClick(object sender, EventArgs e)
    {
        notificationNumber++;
        string title = $"Local Notification #{notificationNumber}";
        string message = $"You have now received {notificationNumber} notifications!";
        notificationManager.ScheduleNotification(title, message);
    }

    void ShowNotification(string title, string message)
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            var msg = new Label()
            {
                Text = $"Notification Received:\nTitle: {title}\nMessage: {message}"
            };
            stackLayout.Children.Add(msg);
        });
    }
}
```

Конструктор класса `MainPage` использует Xamarin.Forms `DependencyService` для получения экземпляра `INotificationManager`, зависящего от платформы. Метод `OnScheduleClicked` использует экземпляр `INotificationManager` для планирования нового уведомления. Метод `ShowNotification` вызывается из обработчика событий, присоединенного к событию `NotificationReceived`, и вставляет новый `Label` в страницу при вызове события.

Обработчик событий `NotificationReceived` приводит свои аргументы событий к `NotificationEventArgs`. Этот тип определен в общем проекте Xamarin.Forms:

```csharp
public class NotificationEventArgs : EventArgs
{
    public string Title { get; set; }
    public string Message { get; set; }
}
```

Дополнительные сведения о Xamarin.Forms `DependencyService` см. в разделе [Xamarin.Forms DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/introduction.md).

## <a name="create-the-android-interface-implementation"></a>Создание реализации интерфейса Android

Чтобы приложение Xamarin.Forms отправляло и получало уведомления на Android, оно должно предоставить реализацию интерфейса `INotificationManager`.

### <a name="create-the-androidnotificationmanager-class"></a>Создание класса AndroidNotificationManager

Класс `AndroidNotificationManager` реализует интерфейс `INotificationManager`:

```csharp
using Android.Support.V4.App;
using Xamarin.Forms;
using AndroidApp = Android.App.Application;

[assembly: Dependency(typeof(LocalNotifications.Droid.AndroidNotificationManager))]
namespace LocalNotifications.Droid
{
    public class AndroidNotificationManager : INotificationManager
    {
        const string channelId = "default";
        const string channelName = "Default";
        const string channelDescription = "The default channel for notifications.";
        const int pendingIntentId = 0;

        public const string TitleKey = "title";
        public const string MessageKey = "message";

        bool channelInitialized = false;
        int messageId = -1;
        NotificationManager manager;

        public event EventHandler NotificationReceived;

        public void Initialize()
        {
            CreateNotificationChannel();
        }

        public int ScheduleNotification(string title, string message)
        {
            if (!channelInitialized)
            {
                CreateNotificationChannel();
            }

            messageId++;

            Intent intent = new Intent(AndroidApp.Context, typeof(MainActivity));
            intent.PutExtra(TitleKey, title);
            intent.PutExtra(MessageKey, message);

            PendingIntent pendingIntent = PendingIntent.GetActivity(AndroidApp.Context, pendingIntentId, intent, PendingIntentFlags.OneShot);

            NotificationCompat.Builder builder = new NotificationCompat.Builder(AndroidApp.Context, channelId)
                .SetContentIntent(pendingIntent)
                .SetContentTitle(title)
                .SetContentText(message)
                .SetLargeIcon(BitmapFactory.DecodeResource(AndroidApp.Context.Resources, Resource.Drawable.xamagonBlue))
                .SetSmallIcon(Resource.Drawable.xamagonBlue)
                .SetDefaults((int)NotificationDefaults.Sound | (int)NotificationDefaults.Vibrate);

            var notification = builder.Build();
            manager.Notify(messageId, notification);

            return messageId;
        }

        public void ReceiveNotification(string title, string message)
        {
            var args = new NotificationEventArgs()
            {
                Title = title,
                Message = message,
            };
            NotificationReceived?.Invoke(null, args);
        }

        void CreateNotificationChannel()
        {
            manager = (NotificationManager)AndroidApp.Context.GetSystemService(AndroidApp.NotificationService);

            if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
            {
                var channelNameJava = new Java.Lang.String(channelName);
                var channel = new NotificationChannel(channelId, channelNameJava, NotificationImportance.Default)
                {
                    Description = channelDescription
                };
                manager.CreateNotificationChannel(channel);
            }

            channelInitialized = true;
        }
    }
}
```

Атрибут `assembly` над пространством имен регистрирует реализацию `INotificationManager` интерфейса с `DependencyService`.

Android позволяет приложениям определять несколько каналов для уведомлений. Метод `Initialize` создает базовый канал, который образец приложения использует для отправки уведомлений. Метод `ScheduleNotification` определяет логику конкретной платформы, необходимую для создания и отправки уведомления. Наконец, при получении сообщения, операционная система Android вызывает метод `ReceiveNotification` и обработчик событий.

> [!NOTE]
> Класс `Application` определен в пространствах имен `Xamarin.Forms` и `Android.App`, поэтому псевдоним `AndroidApp` определяется в инструкциях `using` для различия этих двух пространств.

### <a name="handle-incoming-notifications-on-android"></a>Обработка входящих уведомлений в Android

Класс `MainActivity` должен обнаруживать входящие уведомления и уведомлять экземпляр `AndroidNotificationManager`. Атрибут `Activity` в классе `MainActivity` должен указывать значение `LaunchMode` для `LaunchMode.SingleTop`:

```csharp
[Activity(
        //...
        LaunchMode = LaunchMode.SingleTop]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        // ...
    }
```

Режим `SingleTop` предотвращает запуск нескольких экземпляров `Activity`, пока приложение находится на переднем плане. `LaunchMode` не подходит для приложений, которые запускают несколько действий в более сложных сценариях уведомления. Дополнительные сведения о значениях перечисления `LaunchMode` см. в разделе [Режим запуска действий Android](https://developer.android.com/guide/topics/manifest/activity-element#lmode).

В `MainActivity` класс изменен для получения входящих уведомлений:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    // ...

    global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
    LoadApplication(new App());
    CreateNotificationFromIntent(Intent);
}

protected override void OnNewIntent(Intent intent)
{
    CreateNotificationFromIntent(intent);
}

void CreateNotificationFromIntent(Intent intent)
{
    if (intent?.Extras != null)
    {
        string title = intent.Extras.GetString(AndroidNotificationManager.TitleKey);
        string message = intent.Extras.GetString(AndroidNotificationManager.MessageKey);
        DependencyService.Get<INotificationManager>().ReceiveNotification(title, message);
    }
}
```

Метод `CreateNotificationFromIntent` извлекает данные уведомления из аргумента `intent` и предоставляет их `AndroidNotificationManager` с помощью метода `ReceiveNotification`. Метод `CreateNotificationFromIntent` вызывается как из метода `OnCreate`, так и с помощью метода `OnNewIntent`:

- При запуске приложения с помощью данных уведомления, данные `Intent` передаются в метод `OnCreate`.
- Если приложение уже находится на переднем плане, данные `Intent` будут переданы методу `OnNewIntent`.

Android предлагает множество дополнительных параметров для уведомлений. Дополнительные сведения см. в статье [Уведомления в Xamarin.Android](~/android/app-fundamentals/notifications/index.md).

## <a name="create-the-ios-interface-implementation"></a>Создание реализации интерфейса iOS

Чтобы приложение Xamarin.Forms отправляло и получало уведомления на iOS, оно должно предоставить реализацию `INotificationManager`.

### <a name="create-the-iosnotificationmanager-class"></a>Создание класса iOSNotificationManager

Класс `iOSNotificationManager` реализует интерфейс `INotificationManager`:

```csharp
[assembly: Dependency(typeof(LocalNotifications.iOS.iOSNotificationManager))]
namespace LocalNotifications.iOS
{
    public class iOSNotificationManager : INotificationManager
    {
        int messageId = -1;

        bool hasNotificationsPermission;

        public event EventHandler NotificationReceived;

        public void Initialize()
        {
            // request the permission to use local notifications
            UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert, (approved, err) =>
            {
                hasNotificationsPermission = approved;
            });
        }

        public int ScheduleNotification(string title, string message)
        {
            // EARLY OUT: app doesn't have permissions
            if(!hasNotificationsPermission)
            {
                return -1;
            }

            messageId++;

            var content = new UNMutableNotificationContent()
            {
                Title = title,
                Subtitle = "",
                Body = message,
                Badge = 1
            };

            // Local notifications can be time or location based
            // Create a time-based trigger, interval is in seconds and must be greater than 0
            var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger(0.25, false);

            var request = UNNotificationRequest.FromIdentifier(messageId.ToString(), content, trigger);
            UNUserNotificationCenter.Current.AddNotificationRequest(request, (err) =>
            {
                if (err != null)
                {
                    throw new Exception($"Failed to schedule notification: {err}");
                }
            });

            return messageId;
        }

        public void ReceiveNotification(string title, string message)
        {
            var args = new NotificationEventArgs()
            {
                Title = title,
                Message = message
            };
            NotificationReceived?.Invoke(null, args);
        }
    }
}
```

Атрибут `assembly` над пространством имен регистрирует реализацию `INotificationManager` интерфейса с `DependencyService`.

В iOS перед попыткой планирования уведомления необходимо запросить разрешение на использование уведомлений. Метод `Initialize` запрашивает авторизацию для использования локальных уведомлений. Метод `ScheduleNotification` определяет логику, необходимую для создания и отправки уведомления. Наконец, при получении сообщения, iOS вызывает метод `ReceiveNotification` и обработчик событий.

### <a name="handle-incoming-notifications-on-ios"></a>Обработка входящих уведомлений в iOS

В iOS необходимо создать делегат, который делит `UNUserNotificationCenterDelegate` на подклассы для обработки входящих сообщений. Пример приложения определяет класс `iOSNotificationReceiver`:

```csharp
public class iOSNotificationReceiver : UNUserNotificationCenterDelegate
{
    public override void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
    {
        DependencyService.Get<INotificationManager>().ReceiveNotification(notification.Request.Content.Title, notification.Request.Content.Body);

        // alerts are always shown for demonstration but this can be set to "None"
        // to avoid showing alerts if the app is in the foreground
        completionHandler(UNNotificationPresentationOptions.Alert);
    }
}
```

Этот класс использует `DependencyService` для получения экземпляра класса `iOSNotificationManager` и предоставляет входные данные уведомления методу `ReceiveNotification`.

Во время запуска приложения класс `AppDelegate` должен указать пользовательский делегат. Во время запуска приложения класс `AppDelegate` должен указывать объект `iOSNotificationReceiver` в качестве делегата `UNUserNotificationCenter`. Это происходит в методе `FinishedLaunching`:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();

    UNUserNotificationCenter.Current.Delegate = new iOSNotificationReceiver();

    LoadApplication(new App());
    return base.FinishedLaunching(app, options);
}
```

iOS предлагает множество дополнительных параметров для уведомлений. Дополнительные сведения см. в статье [Уведомления в Xamarin.iOS](~/ios/platform/user-notifications/index.md).

## <a name="test-the-application"></a>Тестирование приложения

Приложение можно тестировать на обеих платформах после того, как проекты платформы будут содержать зарегистрированную реализацию интерфейса `INotificationManager`. Запустите приложение и нажмите кнопку **Запланировать уведомление**, чтобы создать уведомление.

В Android уведомление появится в области уведомлений. При нажатии уведомления, приложение получает уведомление и отображает сообщение под кнопкой **Запланировать уведомление**.

![Локальные уведомления в Android](local-notifications-images/local-notifications-android.png)

В iOS приложение автоматически получает входящие уведомления без необходимости ввода данных пользователем. Приложение получает уведомление и отображает сообщение под кнопкой **Запланировать уведомление**.

![Локальные уведомления в iOS](local-notifications-images/local-notifications-ios.png)

## <a name="related-links"></a>Связанные ссылки

- [Пример проекта](/samples/xamarin/xamarin-forms-samples/local-notifications)
- [Уведомления в Xamarin.Android](~/android/app-fundamentals/notifications/index.md)
- [Уведомления в Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [Xamarin.Forms Dependency.Service](~/xamarin-forms/app-fundamentals/dependency-service/introduction.md)