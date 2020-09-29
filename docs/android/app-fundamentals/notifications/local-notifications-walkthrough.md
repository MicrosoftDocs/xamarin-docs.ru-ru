---
title: Пошаговое руководство. Использование локальных уведомлений в Xamarin. Android
description: В этом пошаговом руководстве показано, как использовать локальные уведомления в приложениях Xamarin. Android. В нем демонстрируются основы создания и публикации локального уведомления. Когда пользователь щелкает уведомление в области уведомлений, он запускает второе действие.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/16/2018
ms.openlocfilehash: 09603fa86bce18c7cfdc141938267ee54710842c
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455749"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>Пошаговое руководство. Использование локальных уведомлений в Xamarin. Android

_В этом пошаговом руководстве показано, как использовать локальные уведомления в приложениях Xamarin. Android. В нем демонстрируются основы создания и публикации локального уведомления. Когда пользователь щелкает уведомление в области уведомлений, он запускает второе действие._

## <a name="overview"></a>Обзор

В этом пошаговом руководстве мы создадим приложение Android, которое создает уведомление, когда пользователь нажимает кнопку в действии. Когда пользователь щелкает уведомление, он запускает второе действие, которое показывает, сколько раз пользователь нажал кнопку в первом действии.

На следующих снимках экрана показаны некоторые примеры этого приложения:

[![Пример снимков экрана с уведомлением](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> В этом руководством основное внимание уделяется [интерфейсам API нотификатионкомпат](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) из [библиотеки поддержки Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Эти API обеспечивают максимальную обратную совместимость с Android 4,0 (API уровня 14).

## <a name="creating-the-project"></a>Создание проекта

Для начала создадим новый проект Android с помощью шаблона **приложения Android** . Давайте назовем этот проект **локалнотификатионс**. (Если вы не знакомы с созданием проектов Xamarin. Android, см. раздел [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)

Измените значения в файле ресурсов **или Strings.xml** так, чтобы они содержали два дополнительных строковых ресурса, которые будут использоваться при создании канала уведомления:

```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Добавление пакета NuGet для Android. support. v4

В этом пошаговом руководстве мы используем `NotificationCompat.Builder` для создания локального уведомления. Как описано в разделах [локальные уведомления](~/android/app-fundamentals/notifications/local-notifications.md), мы должны включить в наш проект [библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) (NuGet) `NotificationCompat.Builder` .

Теперь изменим **MainActivity.CS** и добавим следующую `using` инструкцию, чтобы типы в `Android.Support.V4.App` были доступны для нашего кода:

```csharp
using Android.Support.V4.App;
```

Кроме того, необходимо сделать так, чтобы компилятор использовал `Android.Support.V4.App` версию, `TaskStackBuilder` а не `Android.App` версию. Добавьте следующую `using` инструкцию для устранения неоднозначности:

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>Создание канала уведомления

Затем добавьте метод для `MainActivity` , который создаст канал уведомления (при необходимости):

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var name = Resources.GetString(Resource.String.channel_name);
    var description = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, name, NotificationImportance.Default)
                  {
                      Description = description
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Обновите `OnCreate` метод, чтобы вызвать этот новый метод:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```

### <a name="define-the-notification-id"></a>Определение идентификатора уведомления

Для наших уведомлений и каналов уведомлений потребуется уникальный идентификатор. Давайте изменим **MainActivity.CS** и добавим в класс следующую переменную статического экземпляра `MainActivity` :

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>Добавление кода для создания уведомления

Далее необходимо создать новый обработчик событий для `Click` события кнопки. Добавьте следующий метод к `MainActivity`:

```csharp
void ButtonOnClick(object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    var valuesForActivity = new Bundle();
    valuesForActivity.PutInt(COUNT_KEY, count);

    // When the user clicks the notification, SecondActivity will start up.
    var resultIntent = new Intent(this, typeof(SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras(valuesForActivity);

    // Construct a back stack for cross-task navigation:
    var stackBuilder = TaskStackBuilder.Create(this);
    stackBuilder.AddParentStack(Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent(resultIntent);

    // Create the PendingIntent with the back stack:
    var resultPendingIntent = stackBuilder.GetPendingIntent(0, (int) PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    var builder = new NotificationCompat.Builder(this, CHANNEL_ID)
                  .SetAutoCancel(true) // Dismiss the notification from the notification area when the user clicks on it
                  .SetContentIntent(resultPendingIntent) // Start up this activity when the user clicks the intent.
                  .SetContentTitle("Button Clicked") // Set the title
                  .SetNumber(count) // Display the count in the Content Info
                  .SetSmallIcon(Resource.Drawable.ic_stat_button_click) // This is the icon to display
                  .SetContentText($"The button has been clicked {count} times."); // the message to display.

    // Finally, publish the notification:
    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(NOTIFICATION_ID, builder.Build());

    // Increment the button press count:
    count++;
}
```

`OnCreate`Метод MainActivity должен сделать вызов для создания канала уведомления и назначить `ButtonOnClick` метод для `Click` события кнопки (Замените обработчик событий делегата, предоставленный шаблоном):

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();

    // Display the "Hello World, Click Me!" button and register its event handler:
    var button = FindViewById<Button>(Resource.Id.MyButton);
    button.Click += ButtonOnClick;
}
```

### <a name="create-a-second-activity"></a>Создание второго действия

Теперь нужно создать еще одно действие, которое Android будет отображать, когда пользователь щелкнет уведомление. Добавьте еще одно действие Android в проект с именем **секондактивити**. Откройте **SecondActivity.CS** и замените его содержимое следующим кодом:

```csharp
using System;
using Android.App;
using Android.OS;
using Android.Widget;

namespace LocalNotifications
{
    [Activity(Label = "Second Activity")]
    public class SecondActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            // Get the count value passed to us from MainActivity:
            var count = Intent.Extras.GetInt(MainActivity.COUNT_KEY, -1);

            // No count was passed? Then just return.
            if (count <= 0)
            {
                return;
            }

            // Display the count sent from the first activity:
            SetContentView(Resource.Layout.Second);
            var txtView = FindViewById<TextView>(Resource.Id.textView1);
            txtView.Text = $"You clicked the button {count} times in the previous activity.";
        }
    }
}
```

Также необходимо создать макет ресурса для **секондактивити**. Добавьте новый файл **макета Android** в проект с именем **Second. axml**. Измените **второй. axml** и вставьте следующий код макета:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView1" />
</LinearLayout>
```

### <a name="add-a-notification-icon"></a>Добавить значок уведомления

Наконец, добавьте маленький значок, который будет отображаться в области уведомлений при запуске уведомления. [Этот значок](local-notifications-walkthrough-images/ic-stat-button-click.png) можно скопировать в свой проект или создать собственный пользовательский значок. Присвойте **кнопке имя файл МФ \_ stat, \_ \_click.png** и скопируйте ее в папку **Resources/Draw** . Не забудьте использовать **добавление > существующий элемент...** , чтобы включить этот файл значка в проект.

### <a name="run-the-application"></a>Запуск приложения

Выполните сборку и запустите приложение. Должно быть представлено первое действие, аналогичное следующему снимку экрана:

[![Снимок экрана первого действия](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

При нажатии кнопки следует заметить, что в области уведомлений отображается маленький значок уведомления:

[![Отображается значок уведомления](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

При прокрутке и предоставлении этого ящика уведомлений должно отобразиться следующее уведомление:

[![Сообщение уведомления](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

Если щелкнуть уведомление, оно должно исчезнуть, а наше другое действие должно быть запущено, &ndash; как на следующем снимке экрана:

[![Снимок экрана второго действия](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

Поздравляем! На этом этапе вы завершили пошаговое руководство по локальному уведомлению Android, и у вас есть рабочий пример, на который можно ссылаться. Здесь больше больше уведомлений, чем показано здесь, поэтому если вы хотите получить дополнительные сведения, ознакомьтесь с [документацией Google по уведомлениям](https://developer.android.com/guide/topics/ui/notifiers/notifications.html).

## <a name="summary"></a>Сводка

Это пошаговое руководство используется `NotificationCompat.Builder` для создания и вывода уведомлений. Он показал простой пример того, как запустить второе действие как способ реагирования на взаимодействие пользователя с уведомлением и продемонстрировать перенос данных из первого действия во второе действие.

## <a name="related-links"></a>Связанные ссылки

- [Локалнотификатионс (пример)](/samples/xamarin/monodroid-samples/localnotifications)
- [Каналы уведомлений Android Oreo](https://blog.xamarin.com/android-oreo-notification-channels/)
- [Уведомление](xref:Android.App.Notification)
- [нотификатионманажер](xref:Android.App.NotificationManager)
- [Нотификатионкомпат. Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](xref:Android.App.PendingIntent)