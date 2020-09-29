---
title: Службы переднего плана
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: 0736e427c5c75b0eebe9b9353322b146e6359603
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455304"
---
# <a name="foreground-services"></a>Службы переднего плана

Служба переднего плана — это особый тип привязанной службы или запущенной службы. Службы иногда выполняют задачи, с которыми пользователи должны иметь активное осведомленность, эти службы называются _основными службами_. Примером службы переднего плана является приложение, которое предоставляет пользователю направления и движения. Даже если приложение находится в фоновом режиме, важно, чтобы служба соработала достаточные ресурсы для правильной работы, а у пользователя есть быстрый и удобный способ доступа к приложению. Для приложения Android это означает, что служба переднего плана должна получить более высокий приоритет, чем обычная служба, а служба переднего плана должна предоставлять `Notification` , что Android будет отображаться при условии, что служба запущена.

Чтобы запустить службу переднего плана, приложение должно отправить цель, которая указывает Android запустить службу. Затем служба должна зарегистрироваться в качестве службы переднего плана с Android. Приложения, работающие на Android 8,0 (или более поздней версии) `Context.StartForegroundService` , должны использовать метод для запуска службы, а приложения, работающие на устройствах с более старой версией Android, должны использовать `Context.StartService`

Этот метод расширения C# является примером запуска службы переднего плана. В Android 8,0 и более поздних версиях будет использоваться `StartForegroundService` метод, в противном случае `StartService` будет использован более старый метод.

```csharp
public static void StartForegroundServiceCompat<T>(this Context context, Bundle args = null) where T : Service
{
    var intent = new Intent(context, typeof(T));
    if (args != null) 
    {
        intent.PutExtras(args);
    }

    if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.O)
    {
        context.StartForegroundService(intent);
    }
    else
    {
        context.StartService(intent);
    }
}
```

## <a name="registering-as-a-foreground-service"></a>Регистрация в качестве службы переднего плана

После запуска службы переднего плана она должна зарегистрироваться в Android, вызвав [`StartForeground`](xref:Android.App.Service.StartForeground*) . Если служба запускается с помощью `Service.StartForegroundService` метода, но не регистрируется, Android останавливает службу и помечает приложение как неотвечающее.

`StartForeground` принимает два параметра, оба из которых являются обязательными.

- Целочисленное значение, уникальное в пределах приложения для обнаружения службы.
- `Notification`Объект, который Android будет отображать в строке состояния, пока служба запущена.

При условии, что служба запущена, Android отобразит уведомление в строке состояния. Уведомление, как минимум, предоставит пользователю визуальную подсказку о том, что служба запущена. В идеале уведомление должно предоставлять пользователю ярлык приложения или, возможно, некоторые кнопки действий для управления приложением. Примером этого является музыкальный проигрыватель &ndash; . отображаемое уведомление может содержать кнопки для приостановки или воспроизведения музыки, перемотки до предыдущей песни или перехода к следующей песне. 

Этот фрагмент кода является примером регистрации службы в качестве службы переднего плана.   

```csharp
// This is any integer value unique to the application.
public const int SERVICE_RUNNING_NOTIFICATION_ID = 10000;

public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
{
    // Code not directly related to publishing the notification has been omitted for clarity.
    // Normally, this method would hold the code to be run when the service is started.

    var notification = new Notification.Builder(this)
        .SetContentTitle(Resources.GetString(Resource.String.app_name))
        .SetContentText(Resources.GetString(Resource.String.notification_text))
        .SetSmallIcon(Resource.Drawable.ic_stat_name)
        .SetContentIntent(BuildIntentToShowMainActivity())
        .SetOngoing(true)
        .AddAction(BuildRestartTimerAction())
        .AddAction(BuildStopServiceAction())
        .Build();

    // Enlist this instance of the service as a foreground service
    StartForeground(SERVICE_RUNNING_NOTIFICATION_ID, notification);
}
```

В предыдущем уведомлении отобразится уведомление о строке состояния, похожее на следующее:

![Изображение, показывающее уведомление в строке состояния](foreground-services-images/foreground-services-01.png "Изображение, показывающее уведомление в строке состояния")

На этом снимке экрана показано расширенное уведомление в области уведомлений с двумя действиями, которые позволяют пользователю управлять службой:

![Изображение, показывающее развернутое уведомление](foreground-services-images/foreground-services-02.png "Изображение, показывающее развернутое уведомление.")

Дополнительные сведения об уведомлениях см. в разделе [локальные уведомления](~/android/app-fundamentals/notifications/local-notifications.md) в статье [об уведомлениях Android](~/android/app-fundamentals/notifications/index.md) .

## <a name="unregistering-as-a-foreground-service"></a>Отмена регистрации в качестве службы переднего плана

Служба может отменить список в качестве службы переднего плана, вызвав метод `StopForeground` . `StopForeground` не останавливает службу, но удаляет значок уведомления и оповещает Android о возможности отключения службы при необходимости.

Отображаемое уведомление о строке состояния также можно удалить, передав `true` методу: 

```csharp
StopForeground(true);
```

Если служба остановлена с помощью вызова `StopSelf` или `StopService` , уведомление в строке состояния будет удалено.

## <a name="related-links"></a>Связанные ссылки

- [Android. app. Service](xref:Android.App.Service)
- [Android. app. Service. Стартфореграунд](xref:Android.App.Service.StartForeground*)
- [Локальные уведомления](~/android/app-fundamentals/notifications/local-notifications.md)
- [Фореграундсервицедемо (пример)](/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-foregroundservicedemo)