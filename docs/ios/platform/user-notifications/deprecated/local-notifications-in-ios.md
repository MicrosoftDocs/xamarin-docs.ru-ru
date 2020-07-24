---
title: Уведомления в Xamarin.iOS
description: В этом разделе показано, как реализовать локальные уведомления в Xamarin. iOS. В нем объясняются различные элементы пользовательского интерфейса уведомления iOS и обсуждаются функции API, связанные с созданием и отображением уведомлений.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: 04631993d47a1d51858ab24948ab61170d37bb6e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939893"
---
# <a name="notifications-in-xamarinios"></a>Уведомления в Xamarin.iOS

> [!IMPORTANT]
> Сведения в этом разделе относятся к iOS 9 и более ранним версиям. Для iOS 10 и более поздних версий ознакомьтесь с [руководством по платформе уведомлений пользователей](~/ios/platform/user-notifications/index.md).

в iOS есть три способа указать пользователю, что было получено уведомление:

- **Звук или вибрация** — iOS может воспроизвести звук для уведомления пользователей. Если звук отключен, устройство можно настроить для вибрировало.
- **Оповещения** . на экране можно отобразить диалоговое окно со сведениями об уведомлении.
- **Эмблемы** . при публикации уведомления на значок приложения может отображаться число (с эмблемой).

iOS также предоставляет *Центр уведомлений* , в котором отображаются все уведомления, как локальные, так и удаленные, для пользователя. Пользователи могут получить доступ к этому пользователю, прожимая в верхней части экрана:

![Центр уведомлений](local-notifications-in-ios-images/image13.png "Центр уведомлений")

## <a name="creating-local-notifications-in-ios"></a>Создание локальных уведомлений в iOS

iOS делает создание и обработку локальных уведомлений довольно простым.
Во первых, iOS 8 требует, чтобы приложения запросили разрешение пользователя на отображение уведомлений. Добавьте следующий код в приложение перед попыткой отправить локальное уведомление — [присоединенный пример](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications) помещает его в метод **FinishedLaunching** **AppDelegate**.

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![Подтверждение возможности отправки локального уведомления](local-notifications-in-ios-images/image0-sml.png "Подтверждение возможности отправки локального уведомления")](local-notifications-in-ios-images/image0.png#lightbox)

Чтобы запланировать локальное уведомление, создайте `UILocalNotification` объект, задайте `FireDate` и запланируйте его с помощью `ScheduleLocalNotification` метода `UIApplication.SharedApplication` объекта. В следующем фрагменте кода показано, как запланировать уведомление, которое будет срабатывать в течение одной минуты в будущем, и отобразить предупреждение с сообщением:

```csharp
UILocalNotification notification = new UILocalNotification();
notification.FireDate = NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

На следующем снимке экрана показано, как выглядит это оповещение:

[![Пример оповещения](local-notifications-in-ios-images/image2-sml.png)](local-notifications-in-ios-images/image2.png#lightbox)

Обратите внимание, что если пользователь решил *запретить* уведомления, ничего не будет отображаться.

Если вы хотите применить эмблему к значку приложения с числом, его можно задать, как показано в следующем примере кода:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

Чтобы воспроизвести звук со значком, задайте для уведомления свойство Саунднаме, как показано в следующем фрагменте кода:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Если оповещение превышает 30 секунд, вместо него будет воспроизводиться звук по умолчанию.

> [!IMPORTANT]
> В симуляторе iOS есть ошибка, которая будет вызывать уведомление о делегировании дважды. Эта проблема не должна возникать при запуске приложения на устройстве.

## <a name="handling-notifications"></a>Обработка уведомлений

приложения iOS обрабатывали удаленные и локальные уведомления почти точно так же. При выполнении приложения `ReceivedLocalNotification` `ReceivedRemoteNotification` будет вызван метод или метод `AppDelegate` класса, а сведения об уведомлении передаются в качестве параметра.

Приложение может управлять уведомлением различными способами. Например, приложение может просто отобразить оповещение, чтобы напомнить пользователям о некоторых событиях. Или уведомление может использоваться для вывода предупреждения пользователю о завершении процесса, например синхронизации файлов на сервере.

В следующем коде показано, как управлять локальным уведомлением, отображать предупреждение и сбрасывать номер эмблемы в ноль:

```csharp
public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
{
    // show an alert
    UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
    okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

    Window.RootViewController.PresentViewController(okayAlertController, true, null);

    // reset our badge
    UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
}
```

Если приложение не выполняется, iOS воспроизводит звук и (или) обновляет эмблему значка как применимое. Когда пользователь запускает приложение, связанное с предупреждением, приложение запустится и `FinishedLaunching` будет вызван метод в делегате приложения, и данные уведомления будут передаваться через `launchOptions` параметр. Если словарь параметров содержит ключ `UIApplication.LaunchOptionsLocalNotificationKey` , то `AppDelegate` известно, что приложение было запущено из локального уведомления. Этот процесс показан в следующем фрагменте кода:

```csharp
// check for a local notification
if (launchOptions.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
{
    var localNotification = launchOptions[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
    if (localNotification != null)
    {
        UIAlertController okayAlertController = UIAlertController.Create(localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

        Window.RootViewController.PresentViewController(okayAlertController, true, null);

        // reset our badge
        UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
    }
}
```

Для удаленного уведомления `launchOptions` будет иметь объект с, `LaunchOptionsRemoteNotificationKey` связанный с `NSDictionary` удаленными полезными данными уведомления. Вы можете извлечь полезные данные уведомления с помощью `alert` `badge` ключей, и `sound` . В следующем фрагменте кода показано, как получить удаленные уведомления.

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Сводка

В этом разделе показано, как создать и опубликовать уведомление в Xamarin. iOS. В нем показано, как приложение может реагировать на уведомления путем переопределения `ReceivedLocalNotification` метода или `ReceivedRemoteNotification` метода в `AppDelegate` .

## <a name="related-links"></a>Связанные ссылки

- [Локальные уведомления (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications)
- [Локальные и Push-уведомления для разработчиков](https://developer.apple.com/notifications/)
- [руководстве по программированию локальных и push-уведомлений](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](https://docs.microsoft.com/dotnet/api/uikit.uiapplication)
- [уилокалнотификатион](https://docs.microsoft.com/dotnet/api/uikit.UILocalNotification)
