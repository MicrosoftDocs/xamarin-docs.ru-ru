---
title: Сгруппированные уведомления в Xamarin. iOS
description: IOS 12 позволяет группировать уведомления в центре уведомлений или на экране блокировки по приложениям или потокам. В этом документе описывается, как отправлять потоковые и несвязанные уведомления с помощью Xamarin. iOS.
ms.prod: xamarin
ms.assetid: C6FA7C25-061B-4FD7-8E55-88597D512F3C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: 8c440d61a41fc26aa7537dc169be08c2bf413c68
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435218"
---
# <a name="grouped-notifications-in-xamarinios"></a>Сгруппированные уведомления в Xamarin. iOS

По умолчанию iOS 12 размещает все уведомления приложения в группе. На экране блокировки и в центре уведомлений эта группа отображается в виде стека с самым последним уведомлением в верхней части. Пользователи могут развернуть группу, чтобы просмотреть все уведомления, которые она содержит, и закрыть группу в целом.

Приложения также могут группировать уведомления по потокам, что упрощает пользователям поиск и взаимодействие с конкретными сведениями, в которых они интересуются.

## <a name="sample-app-groupednotifications"></a>Пример приложения: Граупеднотификатионс

Чтобы узнать, как использовать сгруппированные уведомления с Xamarin. iOS, ознакомьтесь с примером приложения [граупеднотификатионс](/samples/xamarin/ios-samples/ios12-groupednotifications) .

Этот пример приложения имитирует беседы с различными друзьями, отправляя уведомление для каждого сообщения и группируя их по потоку. Здесь также показано, как непотоковые уведомления помещаются в группу уровня приложения.

Фрагменты кода в этом пошаговом окне поступают из этого примера приложения.

## <a name="request-authorization-and-allow-foreground-notifications"></a>Запросить авторизацию и разрешить уведомления переднего плана

Прежде чем приложение сможет отправлять локальные уведомления, оно должно запросить разрешение для этого. В примере приложения [`AppDelegate`](xref:UIKit.UIApplicationDelegate) [`FinishedLaunching`](xref:UIKit.UIApplicationDelegate.FinishedLaunching(UIKit.UIApplication,Foundation.NSDictionary)) метод запрашивает это разрешение:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    center.RequestAuthorization(UNAuthorizationOptions.Alert, (bool success, NSError error) =>
    {
        // Set the Delegate regardless of success; users can modify their notification
        // preferences at any time in the Settings app.
        center.Delegate = this;
    });
    return true;
}
```

Значение [`Delegate`](xref:UserNotifications.UNUserNotificationCenter.Delegate) (приведенное выше) [`UNUserNotificationCenter`](xref:UserNotifications.UNUserNotificationCenter) определяет, должно ли приложение переднего плана отображать входящее уведомление путем вызова обработчика завершения, переданного в [`WillPresentNotification`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.WillPresentNotification(UserNotifications.IUNUserNotificationCenterDelegate,UserNotifications.UNUserNotificationCenter,UserNotifications.UNNotification,System.Action{UserNotifications.UNNotificationPresentationOptions})) :

```csharp
[Export("userNotificationCenter:willPresentNotification:withCompletionHandler:")]
public void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, System.Action<UNNotificationPresentationOptions> completionHandler)
{
    completionHandler(UNNotificationPresentationOptions.Alert);
}
```

[`UNNotificationPresentationOptions.Alert`](xref:UserNotifications.UNNotificationPresentationOptions)Параметр указывает, что приложение должно отображать предупреждение, но не воспроизводить звук или обновлять эмблему.

## <a name="threaded-notifications"></a>Потоковые уведомления

Повторно Коснитесь сообщения примера приложения **с помощью кнопки Алисы** , чтобы отправить уведомление для диалога с дружественным именем Alice.
Так как уведомления этого диалога являются частью одного потока, на экране блокировки и в центре уведомлений группируются все эти сообщения.

Чтобы начать беседу с другим дружественным, коснитесь кнопки **выбрать новую дружественную команду** . Уведомления для этого диалога отображаются в отдельной группе.

### <a name="threadidentifier"></a>среадидентифиер

Каждый раз, когда пример приложения запускает новый поток, он создает уникальный идентификатор потока:

```csharp
void StartNewThread()
{
    threadId = $"message-{friend}";
    // ...
}
```

Для отправки потокового уведомления пример приложения:

- Проверяет, имеет ли приложение разрешение на отправку уведомления.
- Создает объект [`UNMutableNotificationContent`](xref:UserNotifications.UNMutableNotificationContent)
объект для содержимого уведомления и задает его свойство [`ThreadIdentifier`](xref:UserNotifications.UNMutableNotificationContent.ThreadIdentifier)
в созданный выше идентификатор потока.
- Создает запрос и планирует уведомление:

```csharp
async partial void ScheduleThreadedNotification(UIButton sender)
{
    var center = UNUserNotificationCenter.Current;

    UNNotificationSettings settings = await center.GetNotificationSettingsAsync();
    if (settings.AuthorizationStatus != UNAuthorizationStatus.Authorized)
    {
        return;
    }

    string author =  // ...
    string message = // ...

    var content = new UNMutableNotificationContent()
    {
        ThreadIdentifier = threadId,
        Title = author,
        Body = message,
        SummaryArgument = author
    };

    var request = UNNotificationRequest.FromIdentifier(
        Guid.NewGuid().ToString(),
        content,
        UNTimeIntervalNotificationTrigger.CreateTrigger(1, false)
    );

    center.AddNotificationRequest(request, null);

    // ...
}
```

Все уведомления из одного приложения с одним идентификатором потока будут отображаться в одной группе уведомлений.

> [!NOTE]
> Чтобы задать идентификатор потока для удаленного уведомления, добавьте `thread-id` ключ в полезные данные JSON уведомления. Дополнительные сведения см. [в статье Создание удаленного документа уведомления](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification) Apple.

### <a name="summaryargument"></a>суммаряргумент

`SummaryArgument` Указывает, как уведомление повлияет на текст сводки, отображаемый в левом нижнем углу группы уведомлений, к которой относится уведомление. iOS объединяет сводный текст из уведомлений в той же группе, чтобы создать общее сводное описание.

В примере приложения в качестве аргумента сводки используется автор сообщения. При таком подходе текст сводки для группы из шести уведомлений с Алисой может представлять собой **5 дополнительных уведомлений от Алисы и Me**.

## <a name="unthreaded-notifications"></a>Несвязанные уведомления

Каждое касание кнопки **напоминания о встрече** примера приложения отправляет одно из различных уведомлений о встречах. Так как эти напоминания не являются потоками, они отображаются в группе уведомлений уровня приложения на экране блокировки и в центре уведомлений.

Для отправки несвязанного уведомления метод примера приложения `ScheduleUnthreadedNotification` использует аналогичный код, как показано выше.
Однако он не задает `ThreadIdentifier` `UNMutableNotificationContent` объект для объекта.

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения — Граупеднотификатионс](/samples/xamarin/ios-samples/ios12-groupednotifications)
- [Платформа уведомлений пользователей в Xamarin. iOS](~/ios/platform/user-notifications/index.md)
- [Новые возможности уведомлений пользователей (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Использование сгруппированных уведомлений (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/711/)
- [Рекомендации и новые возможности уведомлений пользователей (ВВДК 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Создание удаленного уведомления (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)