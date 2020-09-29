---
title: Расширенные уведомления пользователей в Xamarin. iOS
description: В этой статье описывается платформа уведомлений пользователей, появившаяся в iOS 10. В нем обсуждаются локальные уведомления, удаленные уведомления, Управление уведомлениями, действия уведомлений и многое другое.
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: 207aac33101615a0a103176cd2bf5dd061e0d264
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430423"
---
# <a name="enhanced-user-notifications-in-xamarinios"></a>Расширенные уведомления пользователей в Xamarin. iOS

В iOS 10 платформа уведомлений пользователя обеспечивает доставку и обработку локальных и удаленных уведомлений. С помощью этой платформы расширение приложения или приложения может запланировать доставку локальных уведомлений, указав набор условий, например расположение или время суток.

## <a name="about-user-notifications"></a>Об уведомлениях пользователей

Как упоминалось выше, Новая платформа уведомлений пользователя обеспечивает доставку и обработку локальных и удаленных уведомлений. С помощью этой платформы расширение приложения или приложения может запланировать доставку локальных уведомлений, указав набор условий, например расположение или время суток.

Кроме того, приложение или расширение могут получать (и потенциально изменять) как локальные, так и удаленные уведомления по мере их доставки на устройство iOS пользователя.

Новая платформа пользовательского интерфейса уведомлений пользователя позволяет приложению или расширению приложения настраивать внешний вид как локальных, так и удаленных уведомлений, когда они представлены пользователю.

Эта платформа предоставляет следующие возможности, которые приложение может доставлять уведомления пользователю:

- **Визуальные оповещения** — где уведомление выводится из верхней части экрана в виде баннера.
- **Звук и вибрации** — могут быть связаны с уведомлением.
- **Значок приложения знакомство** — там, где значком приложения отображается значок, показывающий, что доступно новое содержимое, например число непрочитанных сообщений электронной почты.

Кроме того, в зависимости от текущего контекста пользователя существуют различные способы представления уведомления:

- Если устройство разблокировано, уведомление будет выдвигаться с верхней части экрана в виде баннера.
- Если устройство заблокировано, уведомление будет отображаться на экране блокировки пользователя.
- Если пользователь пропустил уведомление, он может открыть центр уведомлений и просмотреть все доступные, ожидающие уведомления.

Приложение Xamarin. iOS имеет два типа уведомлений о пользователях, которые могут быть отправлены:

- **Локальные уведомления** — они отправляются приложениями, установленными локально на устройстве пользователей.
- **Удаленные уведомления** — отправляются с удаленного сервера и либо представляются пользователю, либо вызывают фоновое обновление содержимого приложения.

### <a name="about-local-notifications"></a>Сведения о локальных уведомлениях

Локальные уведомления, которые может отправлять приложение iOS, имеют следующие функции и атрибуты:

- Они отправляются приложениями, которые являются локальными на устройстве пользователя. 
- Они могут быть настроены на использование времени или триггеров на основе расположения. 
- Приложение планирует уведомление с помощью устройства пользователя и отображается при выполнении условия триггера.
- Когда пользователь взаимодействует с уведомлением, приложение получит обратный вызов.

Ниже приведены некоторые примеры локальных уведомлений.

- Оповещения календаря
- Напоминания
- Триггеры с учетом расположения

Дополнительные сведения см. в документации [по локальному и удаленному программированию уведомлений](https://developer.apple.com/documentation/usernotifications) Apple.

### <a name="about-remote-notifications"></a>Об удаленных уведомлениях

Удаленные уведомления, которые могут отправляться приложением iOS, имеют следующие функции и атрибуты:

- Приложение имеет серверный компонент, с которым он взаимодействует.
- Cлужба push-уведомлений Apple (APNs) используется для передачи на устройство пользователя оптимальной доставки удаленных уведомлений с облачных серверов разработчика.
- Когда приложение получает удаленное уведомление, оно будет отображаться пользователю.
- Когда пользователь взаимодействует с уведомлением, приложение получит обратный вызов.

Ниже приведены некоторые примеры удаленных уведомлений.

- Оповещения новостей
- Спортивные обновления
- Сообщения о мгновенных сообщениях

Для приложения iOS доступно два типа удаленных уведомлений:

- **Пользователь** недоступен — они отображаются для пользователя на устройстве.
- **Автоматическое обновление** — обеспечивает механизм обновления содержимого приложения iOS в фоновом режиме. При получении автоматического обновления приложение может подключиться к удаленным серверам, чтобы извлечь Последнее содержимое.

Дополнительные сведения см. в документации [по локальному и удаленному программированию уведомлений](https://developer.apple.com/documentation/usernotifications) Apple.

### <a name="about-the-existing-notifications-api"></a>О существующем API уведомлений

До версии iOS 10 приложение iOS будет использовать `UIApplication` для регистрации уведомления в системе и для планирования запуска этого уведомления (по времени или расположению).

Существует несколько проблем, с которыми может столкнуться разработчик при работе с существующим API уведомлений:

- Для локальных или удаленных уведомлений, которые могут привести к дублированию кода, требуются разные обратные вызовы.
- Приложение было ограничено контролем уведомлений после того, как оно было запланировано в системе.
- Существуют различные уровни поддержки для всех существующих платформ Apple.

### <a name="about-the-new-user-notification-framework"></a>О новой платформе уведомлений пользователей

В iOS 10 компания Apple предоставила новую платформу уведомлений пользователей, которая заменяет существующий `UIApplication` метод, указанный выше.

Платформа уведомлений пользователя предоставляет следующие сведения:

- Знакомый API, который включает в себя функции сравнения с предыдущими методами, что упрощает перенос кода из существующей платформы.
- Включает расширенный набор параметров содержимого, позволяющий отправлять пользователю более широкие уведомления.
- Как локальные, так и удаленные уведомления могут обрабатываться одним и тем же кодом и обратными вызовами.
- Упрощает процесс обработки обратных вызовов, которые отправляются в приложение, когда пользователь взаимодействует с уведомлением.
- Улучшенное управление как ожидающими, так и доставленными уведомлениями, включая возможность удаления или обновления уведомлений.
- Добавляет возможность выполнения в приложении представления уведомлений.
- Добавляет возможность планирования и обработки уведомлений в расширениях приложений.
- Добавляет новую точку расширения для самих уведомлений. 

Новая платформа уведомлений пользователя предоставляет единый API уведомлений на нескольких платформах, поддерживаемых Apple, включая: 

- **iOS** — полная поддержка для управления и планирования уведомлений.
- **tvOS** — добавляет возможность значков приложений с эмблемами для локальных и удаленных уведомлений.
- **watchOS** — обеспечивает возможность пересылки уведомлений с парного устройства iOS пользователя на свои Apple Watch и позволяет приложениям отслеживать возможность локальных уведомлений непосредственно в самих часах.

Дополнительные сведения см. в [справочнике по Усернотификатионс Framework](https://developer.apple.com/reference/usernotifications) Apple и документации по [усернотификатионсуи](https://developer.apple.com/reference/usernotificationsui) .

## <a name="preparing-for-notification-delivery"></a>Подготовка к доставке уведомлений

Прежде чем приложение iOS сможет отправлять уведомления пользователю, приложение должно быть зарегистрировано в системе, и, поскольку уведомление является пределом пользователя, приложение должно явно запросить разрешение перед его отправкой.

Существует три разных уровня запросов на уведомления, которые пользователь может утверждать для приложения:

- Отображается баннер.
- Звуковые оповещения.
- Знакомство значок приложения.

Кроме того, эти уровни утверждения должны быть запрошены и заданы как для локальных, так и для удаленных уведомлений.

Разрешение на уведомление следует запрашивать сразу после запуска приложения, добавив следующий код в `FinishedLaunching` метод `AppDelegate` и указав требуемый тип уведомления ( `UNAuthorizationOptions` ):

```csharp
using UserNotifications;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    return true;
}
```

Кроме того, пользователь всегда может изменить права уведомления для приложения в любое время с помощью приложения " **Параметры** " на устройстве. Приложение должно проверить наличие запрашиваемых уведомлений пользователя перед представлением уведомления, используя следующий код:

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
});    
``` 

### <a name="configuring-the-remote-notifications-environment"></a>Настройка среды удаленного уведомления

Начиная с iOS 10, разработчик должен информировать ОС о том, как работает push-уведомление среды в среде разработки или эксплуатации. Невозможность предоставить эту информацию может привести к тому, что приложение будет отклонено при отправке в хранилище приложений iTunes с уведомлением следующего вида:

> Отсутствует право на отправку push-уведомлений — приложение включает API для службы push-уведомлений Apple, но право `aps-environment` отсутствует в подписи приложения.

Чтобы предоставить необходимый объем обслуживания, выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Дважды щелкните `Entitlements.plist` файл в **панель решения** , чтобы открыть его для редактирования.
2. Переключитесь в представление **исходного кода** : 

    [![Представление источника](enhanced-user-notifications-images/setup01.png)](enhanced-user-notifications-images/setup01.png#lightbox)
3. Нажмите кнопку, **+** чтобы добавить новый ключ.
4. Введите `aps-environment` для **Свойства**, оставьте **Тип** `String` и введите `development` или `production` для **значения**: 

    [![Свойство среды APS](enhanced-user-notifications-images/setup02.png)](enhanced-user-notifications-images/setup02.png#lightbox)
5. Сохраните изменения в файле.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Дважды щелкните `Entitlements.plist` файл в **Обозреватель решений** , чтобы открыть его для редактирования.
2. Нажмите кнопку, **+** чтобы добавить новый ключ.
3. Введите `aps-environment` для **Свойства**, оставьте **Тип** `String` и введите `development` или `production` для **значения**: 

    [![Свойство среды APS](enhanced-user-notifications-images/setup02w.png)](enhanced-user-notifications-images/setup02.png#lightbox)
4. Сохраните изменения в файле.

-----

### <a name="registering-for-remote-notifications"></a>Регистрация для удаленных уведомлений

Если приложение будет отправлять и получать удаленные уведомления, ему все равно потребуется _зарегистрировать токен_ с помощью существующего `UIApplication` API. Для этой регистрации устройство должно иметь действующее сетевое подключение APNs, что приведет к формированию необходимого маркера, который будет отправлен в приложение. Приложению необходимо затем перенаправить этот маркер в приложение на стороне сервера разработчика для регистрации удаленных уведомлений:

[![Обзор регистрации маркеров](enhanced-user-notifications-images/token01.png)](enhanced-user-notifications-images/token01.png#lightbox)

Используйте следующий код для инициализации необходимой регистрации:

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

Токен, который отправляется на серверное приложение разработчика, должен быть включен в состав полезных данных уведомления, которые передаются с сервера в APNs при отправке удаленного уведомления:

[![Маркер, входящий в состав полезных данных уведомления](enhanced-user-notifications-images/token02.png)](enhanced-user-notifications-images/token02.png#lightbox)

Маркер выступает в качестве ключа, который связывает вместе уведомление и приложение, используемое для открытия уведомления или ответа на него.

Дополнительные сведения см. в документации [по локальному и удаленному программированию уведомлений](https://developer.apple.com/documentation/usernotifications) Apple.

## <a name="notification-delivery"></a>Доставка уведомлений

После полной регистрации приложения и необходимых разрешений, запрошенных пользователем и предоставленных ему, приложение теперь готово к отправке и получению уведомлений. 

### <a name="providing-notification-content"></a>Предоставление содержимого уведомлений

В iOS 10 все уведомления содержат как **заголовок** , так и **подзаголовок** , которые всегда будут отображаться вместе с **текстом** уведомления. Кроме того, это возможность добавлять в содержимое уведомлений **мультимедийные вложения** .

Чтобы создать содержимое локального уведомления, используйте следующий код:

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

Для удаленных уведомлений процесс аналогичен:

```csharp
{
    "aps":{
        "alert":{
            "title":"Notification Title",
            "subtitle":"Notification Subtitle",
            "body":"This is the message body of the notification."
        },
        "badge":1
    }
}
```

### <a name="scheduling-when-a-notification-is-sent"></a>Планирование при отправке уведомления

После создания содержимого уведомления приложение должно запланировать, когда уведомление будет представлено пользователю, установив *триггер*. iOS 10 поддерживает четыре разных типа триггеров:

- **Push-уведомление** — используется исключительно с удаленными уведомлениями и активируется, когда APNs отправляет пакет уведомлений приложению, выполняющемуся на устройстве.
- **Интервал времени** — позволяет планировать локальное уведомление с интервала времени, начиная с текущего периода, и заканчивая какой-то будущей точкой. Например `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`.
- **Календарная дата** . позволяет планировать локальные уведомления на определенные дату и время.
- **На основе расположения** . разрешает планировать локальные уведомления, когда устройство iOS вводит или выходит из определенного географического расположения или находится в определенном расположении с любыми маяками Bluetooth.

Когда локальное уведомление будет готово, приложению необходимо вызвать `Add` метод `UNUserNotificationCenter` объекта, чтобы запланировать его отображение пользователю. Для удаленных уведомлений приложение на стороне сервера отправляет полезные данные уведомления в APNs, которые затем отправляют пакет на устройство пользователя.

Выведя все части вместе, пример локального уведомления может выглядеть следующим образом:

```csharp
using UserNotifications;
...

var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);

var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

## <a name="handling-foreground-app-notifications"></a>Обработка уведомлений приложения переднего плана

В iOS 10 приложение может выполнять уведомления по-разному, когда оно находится на переднем плане, и запускается уведомление. Предоставляя `UNUserNotificationCenterDelegate` и реализуя `WillPresentNotification` метод, приложение может принять за собой ответственность за отображение уведомления. Пример:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        #region Constructors
        public UserNotificationCenterDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void WillPresentNotification (UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
        {
            // Do something with the notification
            Console.WriteLine ("Active Notification: {0}", notification);

            // Tell system to display the notification anyway or use
            // `None` to say we have handled the display locally.
            completionHandler (UNNotificationPresentationOptions.Alert);
        }
        #endregion
    }
}
```

Этот код просто записывает содержимое в `UNNotification` Выход приложения и запрашивает у системы отображение стандартного оповещения для уведомления. 

Если приложение отображало бы само уведомление, когда оно находилось на переднем плане, и не использовало системные значения по умолчанию, передайте `None` обработчику завершения. Пример

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

Выполнив этот код, откройте `AppDelegate.cs` файл для редактирования и измените `FinishedLaunching` метод так, чтобы он выглядел следующим образом:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    // Watch for notifications while the app is active
    UNUserNotificationCenter.Current.Delegate = new UserNotificationCenterDelegate ();

    return true;
}
```

Этот код присоединяет настраиваемый объект `UNUserNotificationCenterDelegate` выше к текущему, `UNUserNotificationCenter` чтобы приложение могла управлять уведомлением, когда оно активно и находится на переднем плане.

## <a name="notification-management"></a>Управление уведомлениями

В iOS 10 Управление уведомлениями предоставляет доступ к ожидающим и доставленным уведомлениям и добавляет возможность удаления, обновления или продвижения этих уведомлений.

Важной частью управления уведомлениями является _идентификатор запроса_ , который был назначен уведомлению при его создании и планировании в системе. Для удаленных уведомлений это значение назначается с помощью нового `apps-collapse-id` поля в заголовке HTTP-запроса.

Идентификатор запроса используется для выбора уведомления о том, что приложение желает выполнять Управление уведомлениями.

### <a name="removing-notifications"></a>Удаление уведомлений

Чтобы удалить ожидающее уведомление из системы, используйте следующий код:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

Чтобы удалить уже доставленное уведомление, используйте следующий код:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>Обновление существующего уведомления

Чтобы обновить существующее уведомление, просто создайте новое уведомление с измененными параметрами (например, новым временем активации) и добавьте его в систему с тем же идентификатором запроса, что и у уведомления, которое необходимо изменить. Пример

```csharp
using UserNotifications;
...

// Rebuild notification
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

// New trigger time
var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger (10, false);

// ID of Notification to be updated
var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

// Add to system to modify existing Notification
UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

Для уже доставленных уведомлений существующее уведомление будет обновлено и перенесено в верхнюю часть списка на экранах дома и блокировки и в центре уведомлений, если он уже прочитан пользователем.

## <a name="working-with-notification-actions"></a>Работа с действиями с уведомлениями

В iOS 10 уведомления, которые доставляются пользователю, не являются статическими и предоставляют несколько способов взаимодействия пользователя с ними (из встроенных в пользовательские действия).

Существует три типа действий, на которые приложение iOS может отвечать:

- **Действие по умолчанию** . это происходит, когда пользователь отменяет уведомление для открытия приложения и отображает подробные сведения об этом уведомлении.
- **Настраиваемые действия** . они были добавлены в iOS 8 и позволяют пользователю быстро выполнить пользовательскую задачу непосредственно из уведомления без необходимости запуска приложения. Их можно представить в виде списка кнопок с настраиваемыми заголовками или поля ввода текста, которое может выполняться либо в фоновом режиме (где приложение получает небольшое количество времени на выполнение запроса), либо на переднем плане (где приложение запускается на переднем плане для выполнения запроса). Настраиваемые действия доступны как в iOS, так и в watchOS.
- **Действие "отклонить** " — это действие отправляется в приложение, когда пользователь закрывает данное уведомление.

### <a name="creating-custom-actions"></a>Создание настраиваемых действий

Чтобы создать и зарегистрировать настраиваемое действие в системе, используйте следующий код:

```csharp
// Create action
var actionID = "reply";
var title = "Reply";
var action = UNNotificationAction.FromIdentifier (actionID, title, UNNotificationActionOptions.None);

// Create category
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);
    
// Register category
var categories = new UNNotificationCategory [] { category };
UNUserNotificationCenter.Current.SetNotificationCategories (new NSSet<UNNotificationCategory>(categories)); 
```

При создании нового `UNNotificationAction` ему присваивается уникальный идентификатор и заголовок, который будет отображаться на кнопке. По умолчанию действие будет создано как фоновое действие, однако можно указать параметры для настройки поведения действия (например, чтобы сделать его основным действием).

Каждое созданное действие должно быть связано с категорией. При создании нового `UNNotificationCategory` ему присваивается уникальный идентификатор, список действий, которые он может выполнить, список идентификаторов намерений для предоставления дополнительных сведений о намерениях действий в категории и некоторые параметры для управления поведением категории.

Наконец, все категории регистрируются в системе с помощью `SetNotificationCategories` метода.

### <a name="presenting-custom-actions"></a>Представление настраиваемых действий

После того как набор настраиваемых действий и категорий был создан и зарегистрирован в системе, они могут быть представлены как из локальных, так и из удаленных уведомлений.

Для удаленного уведомления задайте `category` в полезных данных удаленного уведомления, которые соответствуют одной из категорий, созданных выше. Пример:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

Для локальных уведомлений задайте `CategoryIdentifier` свойство `UNMutableNotificationContent` объекта. Пример:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

Опять же, этот идентификатор должен соответствовать одной из ранее созданных категорий.

### <a name="handling-dismiss-actions"></a>Обработка действий закрытия

Как упоминалось выше, действие закрытия может быть отправлено в приложение, когда пользователь откроет уведомление. Поскольку это не стандартное действие, при создании категории необходимо задать параметр. Пример:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>Обработка ответов действий

Когда пользователь взаимодействует с настраиваемыми действиями и категориями, созданными ранее, приложение должно выполнить запрошенную задачу. Это делается путем предоставления `UNUserNotificationCenterDelegate` и реализации `UserNotificationCenter` метода. Пример:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        ...

        #region Override Methods
        public override void DidReceiveNotificationResponse (UNUserNotificationCenter center, UNNotificationResponse response, Action completionHandler)
        {
            // Take action based on Action ID
            switch (response.ActionIdentifier) {
            case "reply":
                // Do something
                break;
            default:
                // Take action based on identifier
                if (response.IsDefaultAction) {
                    // Handle default action...
                } else if (response.IsDismissAction) {
                    // Handle dismiss action
                }
                break;
            }

            // Inform caller it has been handled
            completionHandler();
        }
        #endregion
    }
}
```

Переданный `UNNotificationResponse` класс имеет `ActionIdentifier` свойство, которое может быть либо действием по умолчанию, либо действием закрытия. Используйте `response.Notification.Request.Identifier` для проверки каких-либо настраиваемых действий.

`UserText`Свойство содержит значение любого вводимого пользователем текста. `Notification`Свойство содержит исходное уведомление, включающее запрос с триггером и содержимым уведомления. Приложение может решить, было ли оно локальным или удаленным уведомлением в зависимости от типа триггера.

> [!NOTE]
> iOS 12 дает возможность настраиваемому пользовательскому интерфейсу уведомлений изменять свои кнопки действий во время выполнения. Дополнительные сведения см. в документации по [кнопкам действия динамического уведомления](~/ios/platform/introduction-to-ios12/notifications/dynamic-actions.md) .

## <a name="working-with-service-extensions"></a>Работа с расширениями служб

При работе с удаленными уведомлениями _расширения службы_ предоставляют способ включения сквозного шифрования в полезных данных уведомления. Расширения службы — это расширение пользовательского интерфейса (доступное в iOS 10), которое выполняется в фоновом режиме с основной целью расширения или замены видимого содержимого уведомления до того, как оно будет представлено пользователю. 

[![Обзор расширения службы](enhanced-user-notifications-images/extension01.png)](enhanced-user-notifications-images/extension01.png#lightbox)

Расширения службы предназначены для быстрого выполнения, и они задаются лишь за короткий промежуток времени, выполняемый системой. В случае, если расширение службы не может завершить свою задачу за отведенное время, будет вызван метод отката. В случае сбоя резервного перехода пользователю будет отображено исходное содержимое уведомления.

К некоторым потенциальным вариантам использования расширений служб относятся:

- Обеспечение сквозного шифрования содержимого удаленного уведомления.
- Добавление вложений к удаленным уведомлениям, чтобы расширить их.

### <a name="implementing-a-service-extension"></a>Реализация расширения службы

Чтобы реализовать расширение службы в приложении Xamarin. iOS, выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Откройте решение приложения в Visual Studio для Mac.
2. Щелкните правой кнопкой мыши имя решения в **панель решения** и выберите **Добавить**  >  **Добавить новый проект**.
3. Выберите **iOS**  >  **модули iOS расширения**  >  **службы уведомлений** и нажмите кнопку **Далее** : 

    [![Выбор расширений службы уведомлений](enhanced-user-notifications-images/extension02.png)](enhanced-user-notifications-images/extension02.png#lightbox)
4. Введите **имя** для расширения и нажмите кнопку **Далее** : 

    [![Введите имя для расширения](enhanced-user-notifications-images/extension03.png)](enhanced-user-notifications-images/extension03.png#lightbox)
5. При необходимости измените **имя проекта** или **имя решения** , а затем нажмите кнопку **создать** : 

    [![Изменение имени проекта и/или имени решения](enhanced-user-notifications-images/extension04.png)](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Откройте решение приложения в Visual Studio.
2. Щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите **Добавить > новый проект...**.
3. Выберите **Visual C# > расширения iOS > расширение службы уведомлений**:

    [![Выбор расширений службы уведомлений](enhanced-user-notifications-images/extension01.w157-sml.png)](enhanced-user-notifications-images/extension01.w157.png#lightbox)
4. Введите **имя** расширения и нажмите кнопку **ОК** .

-----

> [!IMPORTANT]
> Идентификатор пакета для расширения службы должен соответствовать идентификатору пакета основного приложения с `.appnameserviceextension` добавлением к концу. Например, если основное приложение имело идентификатор пакета  `com.xamarin.monkeynotify` , расширение службы должно иметь идентификатор пакета `com.xamarin.monkeynotify.monkeynotifyserviceextension` . Это значение должно автоматически устанавливаться при добавлении расширения в решение. 

В расширении службы уведомлений имеется один основной класс, который необходимо изменить, чтобы обеспечить требуемую функциональность. Пример:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyChatServiceExtension
{
    [Register ("NotificationService")]
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Computed Properties
        public Action<UNNotificationContent> ContentHandler { get; set; }
        public UNMutableNotificationContent BestAttemptContent { get; set; }
        #endregion

        #region Constructors
        protected NotificationService (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            ContentHandler = contentHandler;
            BestAttemptContent = (UNMutableNotificationContent)request.Content.MutableCopy ();

            // Modify the notification content here...
            BestAttemptContent.Title = $"{BestAttemptContent.Title}[modified]";

            ContentHandler (BestAttemptContent);
        }

        public override void TimeWillExpire ()
        {
            // Called just before the extension will be terminated by the system.
            // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.

            ContentHandler (BestAttemptContent);
        }
        #endregion
    }
}
```

Первому методу, `DidReceiveNotificationRequest` ,, будет передан идентификатор уведомления, а также содержимое уведомления через `request` объект. Переданный метод должен `contentHandler` вызываться для предоставления пользователю уведомления.

Второй метод, `TimeWillExpire` , вызывается непосредственно перед тем, как будет запущено расширение службы для обработки запроса. Если расширение службы не может вызвать в `contentHandler` течение отведенного времени, исходное содержимое будет отображаться для пользователя.

### <a name="triggering-a-service-extension"></a>Активация расширения службы

Если расширение службы создано и доставляется вместе с приложением, оно может быть запущено путем изменения полезных данных удаленного уведомления, отправляемых на устройство. Пример:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

Новый `mutable-content` ключ указывает, что для обновления удаленного содержимого уведомлений необходимо запустить расширение службы. `encrypted-content`Ключ содержит зашифрованные данные, которые расширение службы может расшифровать перед представлением пользователю.

Взгляните на следующий пример расширения службы:

```csharp
using UserNotification;

namespace myApp {
    public class NotificationService : UNNotificationServiceExtension {
    
        public override void DidReceiveNotificationRequest(UNNotificationRequest request, contentHandler) {
            // Decrypt payload
            var decryptedBody = Decrypt(Request.Content.UserInfo["encrypted-content"]);
            
            // Modify Notification body
            var newContent = new UNMutableNotificationContent();
            newContent.Body = decryptedBody;
            
            // Present to user
            contentHandler(newContent);
        }
        
        public override void TimeWillExpire() {
            // Handle out-of-time fallback event
            ...
        }
        
    }
}
```

Этот код расшифровывает зашифрованное содержимое из `encrypted-content` ключа, создает новый `UNMutableNotificationContent` , устанавливает `Body` свойство в расшифрованное содержимое и использует `contentHandler` для представления уведомления пользователю.

## <a name="summary"></a>Сводка

В этой статье описаны все способы улучшения уведомлений пользователей в iOS 10. В нем представлена новая платформа уведомлений пользователей и способы ее использования в приложении Xamarin. iOS или расширении приложения.

## <a name="related-links"></a>Связанные ссылки

- [Примеры iOS 10](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS10)
- [Справочник по Усернотификатионс Framework](https://developer.apple.com/reference/usernotifications)
- [усернотификатионсуи](https://developer.apple.com/reference/usernotificationsui)
- [Локальное и удаленное уведомление по программированию](https://developer.apple.com/documentation/usernotifications)