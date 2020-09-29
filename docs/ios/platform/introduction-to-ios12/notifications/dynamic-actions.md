---
title: Кнопки динамических действий с уведомлениями в Xamarin. iOS
description: В iOS 12 расширение содержимого уведомлений может добавлять, удалять и обновлять кнопки действий, отображаемые вместе с уведомлением. В этом документе описывается использование кнопок действий динамического уведомления с помощью Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 6B34AD78-5117-42D0-B6E7-C8B4B453EAFF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/04/2018
ms.openlocfilehash: 88eb3dec0c72acd6984b226eae5acee4feb3eaac
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433818"
---
# <a name="dynamic-notification-action-buttons-in-xamarinios"></a>Кнопки динамических действий с уведомлениями в Xamarin. iOS

В iOS 12 уведомления могут динамически добавлять, удалять и обновлять связанные кнопки действий. Такая настройка позволяет предоставлять пользователям действия, которые непосредственно важны для содержимого уведомления, и взаимодействия пользователя с ним.

## <a name="sample-app-redgreennotifications"></a>Пример приложения: Редгриннотификатионс

Фрагменты кода в этом руководстве взяты из примера приложения [редгриннотификатионс](/samples/xamarin/ios-samples/ios12-redgreennotifications) , в котором демонстрируется использование Xamarin. iOS для работы с кнопками действий с уведомлениями в iOS 12.

Этот пример приложения отправляет два типа локальных уведомлений: красный и зеленый.
После того как приложение отправит уведомление, используйте трехмерное касание для просмотра его настраиваемого пользовательского интерфейса. Затем используйте кнопки действия уведомления для поворота отображаемого изображения. При повороте изображения кнопка **сброса вращения** появляется и исчезает при необходимости.

Фрагменты кода в этом пошаговом окне поступают из этого примера приложения.

## <a name="default-action-buttons"></a>Кнопки действий по умолчанию

Категория уведомлений определяет кнопки действий по умолчанию.

Создание и регистрация категорий уведомлений во время запуска приложения.
Например, в [примере приложения](#sample-app-redgreennotifications) `FinishedLaunching` метод `AppDelegate` выполняет следующие действия:

- Определяет одну категорию для красных уведомлений, а другая — для экологичных уведомлений.
- Регистрирует эти категории, вызывая метод [`SetNotificationCategories`](xref:UserNotifications.UNUserNotificationCenter.SetNotificationCategories*)
метода `UNUserNotificationCenter`
- Присоединяет один элемент [`UNNotificationAction`](xref:UserNotifications.UNNotificationAction)
к каждой категории

В следующем примере кода показано, как это работает:

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Provisional | UNAuthorizationOptions.ProvidesAppNotificationSettings;
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        // ...
        var rotateTwentyDegreesAction = UNNotificationAction.FromIdentifier("rotate-twenty-degrees-action", "Rotate 20°", UNNotificationActionOptions.None);

        var redCategory = UNNotificationCategory.FromIdentifier(
            "red-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var greenCategory = UNNotificationCategory.FromIdentifier(
            "green-category",
            new UNNotificationAction[] { rotateTwentyDegreesAction },
            new string[] { },
            UNNotificationCategoryOptions.CustomDismissAction
        );

        var set = new NSSet<UNNotificationCategory>(redCategory, greenCategory);
        center.SetNotificationCategories(set);
    });
    // ...
}
```

На основе этого кода все уведомления, для которых [`Content.CategoryIdentifier`](xref:UserNotifications.UNNotificationContent.CategoryIdentifier)
"Красная-Category" или "Зеленая-Category", по умолчанию отображается кнопка действия " **повернуть 20 °** ".

## <a name="in-app-handling-of-notification-action-buttons"></a>Обработка в приложении кнопок действий уведомления

`UNUserNotificationCenter` имеет `Delegate` свойство типа [`IUNUserNotificationCenterDelegate`](xref:UserNotifications.IUNUserNotificationCenterDelegate) .

В примере приложения `AppDelegate` задается как делегат центра уведомлений пользователя в `FinishedLaunching` :

```csharp
public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
{
    // Request authorization to send notifications
    UNUserNotificationCenter center = UNUserNotificationCenter.Current;
    var options = // ...
    center.RequestAuthorization(options, (bool success, NSError error) =>
    {
        center.Delegate = this;
        // ...
```

Затем `AppDelegate` реализует [`DidReceiveNotificationResponse`](xref:UserNotifications.UNUserNotificationCenterDelegate_Extensions.DidReceiveNotificationResponse*)
для управления отводами кнопок действий:

```csharp
[Export("userNotificationCenter:didReceiveNotificationResponse:withCompletionHandler:")]
public void DidReceiveNotificationResponse(UNUserNotificationCenter center, UNNotificationResponse response, System.Action completionHandler)
{
    if (response.IsDefaultAction)
    {
        Console.WriteLine("ACTION: Default");
    }
    if (response.IsDismissAction)
    {
        Console.WriteLine("ACTION: Dismiss");
    }
    else
    {
        Console.WriteLine($"ACTION: {response.ActionIdentifier}");
    }

    completionHandler();
        }
```

Эта реализация `DidReceiveNotificationResponse` не обрабатывает кнопку действия " **повернуть 20 °** " уведомления. Вместо этого в этой кнопке обрабатываются расширения содержимого уведомления. В следующем разделе далее обсуждается обработка кнопок действий с уведомлениями.

## <a name="action-buttons-in-the-notification-content-extension"></a>Кнопки действий в расширении содержимого уведомлений

Расширение содержимого уведомлений содержит контроллер представления, который определяет пользовательский интерфейс для уведомления.

Этот контроллер представления может использовать `GetNotificationActions` методы и `SetNotificationActions` для его [`ExtensionContext`](xref:UIKit.UIViewController.ExtensionContext)
свойство для доступа и изменения кнопок действия уведомления.

В примере приложения контроллер представления расширения содержимого уведомления изменяет кнопки действий только при ответе на нажатие уже существующего действия.

> [!NOTE]
> Расширение содержимого уведомления может реагировать на нажатие кнопки действия в [`DidReceiveNotificationResponse`](xref:UserNotificationsUI.UNNotificationContentExtension_Extensions.DidReceiveNotificationResponse*) методе контроллера представления, объявленном как часть [иуннотификатионконтентекстенсион](xref:UserNotificationsUI.IUNNotificationContentExtension).
>
> Хотя он использует имя с `DidReceiveNotificationResponse` [описанным выше](#in-app-handling-of-notification-action-buttons)методом, это другой метод.
>
> После того как расширение содержимого уведомлений заканчивает обработку нажатия кнопки, оно может выбрать, следует ли сообщить основному приложению об обработке этой же кнопки. Для этого необходимо передать соответствующее значение [уннотификатионконтентекстенсионреспонсеоптион](xref:UserNotificationsUI.UNNotificationContentExtensionResponseOption) в обработчик завершения:
>
> - `Dismiss` Указывает, что интерфейс уведомления должен быть закрыт, и что основному приложению не нужно обрабатывать нажатие кнопки.
> - `DismissAndForwardAction` Указывает, что интерфейс уведомления должен быть закрыт, и что основное приложение также должно обрабатывать касание кнопки.
> - `DoNotDismiss` Указывает, что интерфейс уведомления не должен быть закрыт, и что основному приложению не нужно обрабатывать нажатие кнопки.

Метод расширения содержимого `DidReceiveNotificationResponse` определяет, какая кнопка действия была нажата, поворачивает изображение в интерфейсе уведомления и отображает или скрывает кнопку **сброса** действия:

```csharp
[Export("didReceiveNotificationResponse:completionHandler:")]
public void DidReceiveNotificationResponse(UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
{
    var rotationAction = ExtensionContext.GetNotificationActions()[0];

    if (response.ActionIdentifier == "rotate-twenty-degrees-action")
    {
        rotationButtonTaps += 1;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        // 9 rotations * 20 degrees = 180 degrees. No reason to
        // show the reset rotation button when the image is half
        // or fully rotated.
        if (rotationButtonTaps % 9 == 0)
        {
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
        }
        else if (rotationButtonTaps % 9 == 1)
        {
            var resetRotationAction = UNNotificationAction.FromIdentifier("reset-rotation-action", "Reset rotation", UNNotificationActionOptions.None);
            ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction, resetRotationAction });
        }
    }

    if (response.ActionIdentifier == "reset-rotation-action")
    {
        rotationButtonTaps = 0;

        double radians = (20 * rotationButtonTaps) * (2 * Math.PI / 360.0);
        Xamagon.Transform = CGAffineTransform.MakeRotation((float)radians);

        ExtensionContext.SetNotificationActions(new UNNotificationAction[] { rotationAction });
    }

    completionHandler(UNNotificationContentExtensionResponseOption.DoNotDismiss);
}
```

В этом случае метод передает `UNNotificationContentExtensionResponseOption.DoNotDismiss` его обработчику завершения. Это означает, что интерфейс уведомления останется открытым.

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения — Редгриннотификатионс](/samples/xamarin/ios-samples/ios12-redgreennotifications)
- [Платформа уведомлений пользователей в Xamarin. iOS](~/ios/platform/user-notifications/index.md)
- [Объявление типов уведомлений с действиями пользователя](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types?language=objc)
- [Усернотификатионс (Apple)](https://developer.apple.com/documentation/usernotifications?language=objc)
- [Новые возможности уведомлений пользователей (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/710/)
- [Рекомендации и новые возможности уведомлений пользователей (ВВДК 2017)](https://developer.apple.com/videos/play/wwdc2017/708/)
- [Создание удаленного уведомления (Apple)](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)