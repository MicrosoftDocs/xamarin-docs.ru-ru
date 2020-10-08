---
title: Удаленные уведомления с помощью Firebase Cloud Messaging
description: В этом пошаговом руководстве представлено пошаговое описание использования Firebase Cloud Messaging для реализации удаленных уведомлений (также называемых Push-уведомлениями) в приложении Xamarin. Android. В нем показано, как реализовать различные классы, необходимые для связи с Firebase Cloud Messaging (FCM), а также приводятся примеры настройки манифеста Android для доступа к FCM и демонстрации подчиненного обмена сообщениями с помощью консоли Firebase.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/31/2018
ms.openlocfilehash: 75edc03706908df365bfda05229a8081ef50a4ac
ms.sourcegitcommit: db5173f26edbb94b0ec60ab9a708fa04478480f6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/08/2020
ms.locfileid: "91853294"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Удаленные уведомления с помощью Firebase Cloud Messaging

_В этом пошаговом руководстве представлено пошаговое описание использования Firebase Cloud Messaging для реализации удаленных уведомлений (также называемых Push-уведомлениями) в приложении Xamarin. Android. В нем показано, как реализовать различные классы, необходимые для связи с Firebase Cloud Messaging (FCM), а также приводятся примеры настройки манифеста Android для доступа к FCM и демонстрации подчиненного обмена сообщениями с помощью консоли Firebase._

## <a name="fcm-notifications-overview"></a>Общие сведения об уведомлениях FCM

В этом пошаговом руководстве будет создано базовое приложение с именем **фкмклиент** для демонстрации основы обмена сообщениями FCM. **Фкмклиент** проверяет наличие сервисы Google Play, получает маркеры регистрации от FCM, отображает удаленные уведомления, отправляемые из консоли Firebase, и подписывается на сообщения раздела:

[![Пример снимка экрана приложения](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

Будут рассмотрены следующие темы:

1. Фоновые уведомления

2. Сообщения раздела

3. Уведомления переднего плана

В ходе этого пошагового руководства вы будете постепенно добавлять функции в **фкмклиент** и запускать их на устройстве или в эмуляторе, чтобы понять, как она взаимодействует с FCM. Вы будете использовать ведение журнала для транзакций с активными приложениями-свидетелями с FCM серверами, и вы будете видеть, как создаются уведомления из сообщений FCM, которые вводятся в графический интерфейс уведомлений консоли Firebase.

## <a name="requirements"></a>Requirements (Требования)

Будет полезно ознакомиться с [различными типами сообщений](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) , которые могут быть отправлены Firebase облачными сообщениями. Полезная нагрузка сообщения определит, как клиентское приложение будет принимать и обрабатывать сообщение.

Прежде чем продолжить работу с этим пошаговым руководством, необходимо получить необходимые учетные данные для использования серверов FCM Google. Этот процесс описан в [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm).
В частности, необходимо загрузить **google-services.jsв** файл для использования с примером кода, представленного в этом пошаговом руководстве. Если вы еще не создали проект в консоли Firebase (или если вы еще не скачали **google-services.jsв** файле), см. статью [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

Для запуска примера приложения потребуется тестовое устройство Android или эмулятор, совместимы с Firebase. Firebase Cloud Messaging поддерживает клиенты, работающие под управлением Android 4,0 или более поздней версии. Кроме того, на этих устройствах должно быть установлено приложение Google Play Маркет (Сервисы Google Play 9.2.1 или более поздней версии). Если на устройстве еще не установлено приложение Google Play Маркет, посетите веб-сайт [Google Play](https://support.google.com/googleplay) , чтобы скачать и установить его. Кроме того, можно использовать эмулятор пакет SDK для Android с установленным Сервисы Google Play, а не тестовым устройством (не нужно устанавливать Google Play Маркет при использовании эмулятора пакет SDK для Android).

## <a name="start-an-app-project"></a>Запуск проекта приложения

Чтобы начать, создайте пустой проект Xamarin. Android с именем **фкмклиент**. Если вы не знакомы с созданием проектов Xamarin. Android, см. раздел [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).
После создания нового приложения следующим шагом является задание имени пакета и установка нескольких пакетов NuGet, которые будут использоваться для связи с FCM.

### <a name="set-the-package-name"></a>Задание имени пакета

В [Firebase Cloud Messaging (облачная система обмена сообщениями](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)) вы указали имя пакета для приложения с поддержкой FCM. Это имя пакета также служит в качестве [*идентификатора приложения*](./firebase-cloud-messaging.md#fcm-in-action-app-id) , связанного с [ключом API](firebase-cloud-messaging.md#fcm-in-action-api-key). Настройте приложение для использования этого имени пакета:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Откройте свойства проекта **фкмклиент** .

2. На странице **манифеста Android** задайте имя пакета.

В следующем примере имя пакета имеет значение `com.xamarin.fcmexample` :

[![Задание имени пакета](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

При обновлении **манифеста Android**также убедитесь, что `Internet` разрешение включено.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Откройте свойства проекта **фкмклиент** .

2. На странице **приложение Android** задайте имя пакета.

В следующем примере имя пакета имеет значение `com.xamarin.fcmexample` :

[![Задание имени пакета](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

При обновлении **манифеста Android**также убедитесь, что `INTERNET` разрешение включено (в разделе **необходимые разрешения**).

-----

> [!IMPORTANT]
> Клиентское приложение не сможет получить маркер регистрации от FCM, если это имя пакета не *полностью* совпадает с именем пакета, которое было указано в консоли Firebase.

### <a name="add-the-xamarin-google-play-services-base-package"></a>Добавление базового пакета Xamarin Сервисы Google Play

Так как Firebase облачные службы обмена сообщениями зависят от Сервисы Google Play, пакет NuGet [сервисы Google Play-Base](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) должен быть добавлен в проект Xamarin. Android. Вам потребуется версия 29.0.0.2 или более поздняя.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. В Visual Studio щелкните правой кнопкой мыши **ссылки > Управление пакетами NuGet..**..

2. Перейдите на вкладку **Обзор** и выполните поиск по фразе **Xamarin. гуглеплайсервицес. base**.

3. Установите этот пакет в проект **фкмклиент** :

    [![Базовая установка Сервисы Google Play](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. В Visual Studio для Mac щелкните правой кнопкой мыши **пакеты > добавить пакеты...**.

2. Найдите **Xamarin.GooglePlayServices.Base**.

3. Установите этот пакет в проект **фкмклиент** :

    [![Базовая установка Сервисы Google Play](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

Если во время установки NuGet возникает ошибка, закройте проект **фкмклиент** , откройте его еще раз и повторите установку NuGet.

При установке **Xamarin. гуглеплайсервицес. base**также устанавливаются все необходимые зависимости. Измените **MainActivity.CS** и добавьте следующий `using` оператор:

```csharp
using Android.Gms.Common;
```

Эта инструкция делает `GoogleApiAvailability` класс в **Xamarin. Гуглеплайсервицес. base** доступным для кода **фкмклиент** .
`GoogleApiAvailability` используется для проверки наличия Сервисы Google Play.

### <a name="add-the-xamarin-firebase-messaging-package"></a>Добавление пакета обмена сообщениями Xamarin Firebase

Чтобы получать сообщения от FCM, пакет NuGet [Xamarin Firebase-Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) должен быть добавлен в проект приложения. Без этого пакета приложение Android не сможет принимать сообщения от серверов FCM.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. В Visual Studio щелкните правой кнопкой мыши **ссылки > Управление пакетами NuGet..**..

2. Найдите **Xamarin. Firebase. Messaging**.

3. Установите этот пакет в проект **фкмклиент** :

    [![Установка службы обмена сообщениями Xamarin Firebase](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. В Visual Studio для Mac щелкните правой кнопкой мыши **пакеты > добавить пакеты...**.

2. Найдите **Xamarin. Firebase. Messaging**.

3. Установите этот пакет в проект **фкмклиент** :

    [![Установка службы обмена сообщениями Xamarin Firebase](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

При установке **Xamarin. Firebase. Messaging**также устанавливаются все необходимые зависимости.

Затем измените **MainActivity.CS** и добавьте следующие `using` инструкции:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

Первые два оператора делают типы в пакете NuGet **Xamarin. Firebase. Messaging** доступными для кода **фкмклиент** . **Android. util** добавляет функции ведения журнала, которые будут использоваться для отслеживания транзакций с помощью FMS.

### <a name="add-the-google-services-json-file"></a><a name="add-googleplayservices-json"></a>Добавление JSON-файла сервисов Google

Следующим шагом является добавление **google-services.js** файла в корневой каталог проекта:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Скопируйте **google-services.js** в папку проекта.

2. Добавьте **google-services.js** в проект приложения (щелкните **Показывать все файлы** в **Обозреватель решений**, щелкните правой кнопкой мыши **google-services.jsв**, а затем выберите **включить в проект**).

3. В окне **обозревателя решений** выберите файл **google-services.json**.

4. На панели **Свойства** задайте для **действия сборки** значение  **гуглесервицесжсон**:

    [![Установка для действия сборки значения Гуглесервицесжсон](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

    > [!NOTE] 
    > Если действие сборка **гуглесервицесжсон** не отображается, сохраните и закройте решение, а затем снова откройте его.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Скопируйте **google-services.js** в папку проекта.

2. Добавьте **google-services.js** в проект приложения.

3. Щелкните правой кнопкой мыши **google-services.jsв**.

4. Задайте для **действия сборки** значение **гуглесервицесжсон**:

    [![Установка для действия сборки значения Гуглесервицесжсон](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

При добавлении **google-services.js** к проекту (и задании действия сборки **гуглесервицесжсон** ) процесс сборки извлекает идентификатор клиента и [ключ API](./firebase-cloud-messaging.md#fcm-in-action-api-key) , а затем добавляет эти учетные данные в Объединенные или созданные **AndroidManifest.xml** , которые находятся в **obj/Debug/Android/AndroidManifest.xml**. Этот процесс слияния автоматически добавляет любые разрешения и другие элементы FCM, необходимые для подключения к серверам FCM.

## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Проверка Сервисы Google Play и создание канала уведомления

Google рекомендует, чтобы приложения Android проверяют наличие Сервисы Google Play APK перед доступом к Сервисы Google Playным функциям (Дополнительные сведения см. в разделе [Проверка наличия служб Google Play](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)).

Сначала будет создан начальный макет для пользовательского интерфейса приложения. Измените **Resources/Layout/Main. axml** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

Он `TextView` будет использоваться для отображения сообщений, указывающих, установлен ли сервисы Google Play. Сохраните изменения в **Main. axml**.

Измените **MainActivity.CS** и добавьте следующие переменные экземпляра в `MainActivity` класс:

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

Переменные `CHANNEL_ID` и `NOTIFICATION_ID` будут использоваться в методе [`CreateNotificationChannel`](#create-notification-channel-code) , который будет добавлен в `MainActivity` дальнейшем в этом пошаговом руководстве.

В следующем примере метод проверит `OnCreate` , доступен ли сервисы Google Play, прежде чем приложение попытается использовать FCM Services.
Добавьте в класс `MainActivity` следующий метод:

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "This device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

Этот код проверяет устройство на наличие установленного Сервисы Google Play APK. Если он не установлен, в появится сообщение `TextBox` , указывающее пользователю загрузить APK из Google Play Маркет (или включить его в параметрах системы устройства).

<a name="create-notification-channel-code"></a>Приложения, работающие на Android 8,0 (уровень API 26) или более поздней версии, должны создавать [_канал уведомления_](~/android/app-fundamentals/notifications/local-notifications.md) для публикации своих уведомлений.  Добавьте в класс следующий метод `MainActivity` , который создаст канал уведомления (при необходимости):

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

    var channel = new NotificationChannel(CHANNEL_ID,
                                          "FCM Notifications",
                                          NotificationImportance.Default)
                  {

                      Description = "Firebase Cloud Messages appear in this channel"
                  };

    var notificationManager = (NotificationManager)GetSystemService(Android.Content.Context.NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Замените метод `OnCreate` следующим кодом:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();

    CreateNotificationChannel();
}
```

`IsPlayServicesAvailable` вызывается в конце, `OnCreate` чтобы проверка сервисы Google Play выполнялась каждый раз при запуске приложения. Метод `CreateNotificationChannel` вызывается, чтобы убедиться, что канал уведомления существует для устройств под управлением Android 8 или более поздней версии. Если у приложения есть `OnResume` метод, он также должен вызывать `IsPlayServicesAvailable` из `OnResume` . Полностью перестройте и запустите приложение. Если все настроено должным образом, появится экран, который выглядит как на следующем снимке экрана:

[![Приложение указывает, что Сервисы Google Play доступен](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

Если вы не получаете этот результат, убедитесь, что на устройстве установлен Сервисы Google Play APK (Дополнительные сведения см. в разделе [настройка сервисы Google Play](https://developers.google.com/android/guides/setup)).
Также убедитесь, что вы добавили пакет **Xamarin. Google. Play. Services. base** в проект **фкмклиент** , как описано выше.

## <a name="add-the-instance-id-receiver"></a>Добавление приемника ИДЕНТИФИКАТОРов экземпляров

Следующим шагом является добавление службы, которая расширяется `FirebaseInstanceIdService` для управления созданием, поворотом и обновлением [маркеров регистрации Firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token). `FirebaseInstanceIdService`Служба необходима для того, чтобы FCM мог отсылать сообщения на устройство. Когда `FirebaseInstanceIdService` служба добавляется в клиентское приложение, приложение автоматически получает сообщения FCM и отображает их в виде уведомлений, когда приложение находится в фоновом режиме.

### <a name="declare-the-receiver-in-the-android-manifest"></a>Объявление получателя в манифесте Android

Измените **AndroidManifest.xml** и вставьте в `<receiver>` раздел следующие элементы `<application>` :

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

Этот XML-код выполняет следующие действия:

- Объявляет `FirebaseInstanceIdReceiver` реализацию, которая предоставляет [уникальный идентификатор](https://developers.google.com/instance-id/) для каждого экземпляра приложения. Этот получатель также проверяет подлинность и авторизует действия.

- Объявляет внутреннюю `FirebaseInstanceIdInternalReceiver` реализацию, которая используется для безопасного запуска служб.

- [Идентификатор приложения](./firebase-cloud-messaging.md#fcm-in-action-app-id) хранится в **google-services.js** в файле, который был [добавлен в проект](#add-googleplayservices-json). Привязки Xamarin. Android Firebase заменяют маркер `${applicationId}` идентификатором приложения. в клиентском приложении не требуется дополнительный код для предоставления идентификатора приложения.

`FirebaseInstanceIdReceiver`— Это объект `WakefulBroadcastReceiver` , который `FirebaseInstanceId` получает `FirebaseMessaging` события и доставляет их в класс, производный от `FirebaseInstanceIdService` .

### <a name="implement-the-firebase-instance-id-service"></a>Реализация службы ИДЕНТИФИКАТОРов экземпляров Firebase

Работа по регистрации приложения с помощью FCM обрабатывается настраиваемой `FirebaseInstanceIdService` службой, которую вы предоставляете.
`FirebaseInstanceIdService` выполняет следующие действия.

1. Использует [API идентификатора экземпляра](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) для создания маркеров безопасности, которые разрешают клиентскому приложению доступ к FCM и серверу приложений. После возврата приложение возвращает [маркер регистрации](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token) из FCM.

2. Перенаправляет маркер регистрации на сервер приложений, если это требуется для сервера приложений.

Добавьте новый файл с именем **MyFirebaseIIDService.CS** и замените его код шаблона следующим:

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

Эта служба реализует `OnTokenRefresh` метод, который вызывается при первоначальном создании или изменении маркера регистрации. При `OnTokenRefresh` запуске извлекается последний токен из `FirebaseInstanceId.Instance.Token` Свойства (которое обновляется асинхронно с помощью FCM). В этом примере обновленный маркер заносится в журнал, чтобы его можно было просмотреть в окне вывода:

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` вызывается редко: он используется для обновления маркера в следующих случаях:

- При установке или удалении приложения.

- Когда пользователь удаляет данные приложения.

- Когда приложение удаляет идентификатор экземпляра.

- При компрометации безопасности маркера.

В соответствии с документацией по [идентификатору экземпляра](https://developers.google.com/instance-id/guides/android-implementation) Google служба идентификаторов экземпляров FCM будет запрашивать, что приложение периодически обновляет свой маркер (обычно каждые 6 месяцев).

`OnTokenRefresh` также вызывает метод `SendRegistrationToAppServer` , чтобы связать маркер регистрации пользователя с учетной записью на стороне сервера (если таковая имеется), которая поддерживается приложением:

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Поскольку эта реализация зависит от структуры сервера приложений, в этом примере предоставляется пустой текст метода. Если серверу приложений требуются сведения о регистрации FCM, измените, `SendRegistrationToAppServer` чтобы связать маркер идентификатора экземпляра FCM пользователя с любой учетной записью на стороне сервера, поддерживаемой приложением. (Обратите внимание, что маркер непрозрачен для клиентского приложения.)

При отправке маркера на сервер приложений `SendRegistrationToAppServer` должен поддерживать логическое значение, указывающее, был ли маркер отправлен на сервер. Если это логическое значение false, `SendRegistrationToAppServer` отправляет маркер на сервер приложений &ndash; , в противном случае маркер уже был отправлен на сервер приложений в предыдущем вызове. В некоторых случаях (например, в этом `FCMClient` примере) серверу приложений не требуется маркер, поэтому этот метод не требуется в этом примере.

## <a name="implement-client-app-code"></a>Реализация кода клиентского приложения

Теперь, когда службы приемника настроены, можно написать код клиентского приложения, чтобы воспользоваться преимуществами этих служб. В следующих разделах в пользовательский интерфейс добавляется кнопка для регистрации маркера регистрации (также называемого *маркером идентификатора экземпляра*), а для `MainActivity` просмотра `Intent` сведений о запуске приложения из уведомления добавляется дополнительный код.

[![Кнопка "токен журнала" добавлена в экран приложения](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>Токены журнала

Код, добавленный на этом шаге, предназначен только для демонстрационных целей &ndash; . в рабочем клиентском приложении не нужно регистрировать маркеры регистрации. Измените **Resources/Layout/Main. axml** и добавьте следующее `Button` объявление сразу после `TextView` элемента:

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Добавьте следующий код в конец метода `MainActivity.OnCreate`:

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Этот код регистрирует текущий маркер в окне вывода при нажатии кнопки **маркера журнала** .

### <a name="handle-notification-intents"></a>Обработку целей уведомлений

Когда пользователь отменяет уведомление, выданное из **фкмклиент**, все данные, сопровождающие сообщение уведомления, становятся доступными в `Intent` дополнение. Измените **MainActivity.CS** и добавьте следующий код в начало `OnCreate` метода (перед вызовом `IsPlayServicesAvailable` ):

```csharp
if (Intent.Extras != null)
{
    foreach (var key in Intent.Extras.KeySet())
    {
        var value = Intent.Extras.GetString(key);
        Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
    }
}
```

Средство запуска приложения `Intent` срабатывает, когда пользователь отменяет свое сообщение уведомления, поэтому этот код будет записывать все сопутствующие данные в `Intent` окно вывода. Если `Intent` должно быть инициировано другое, `click_action` поле сообщения уведомления должно иметь значение `Intent` (средство запуска `Intent` используется, если не `click_action` указано иное).

## <a name="background-notifications"></a>Фоновые уведомления

Создайте и запустите приложение **фкмклиент** . Появится кнопка " **токен журнала** ":

[![Отображается кнопка "токен журнала"](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

Нажмите кнопку **токен журнала** . В окне вывода интегрированной среды разработки должно отобразиться сообщение следующего вида:

[![Маркер идентификатора экземпляра, отображаемый в окне вывода](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

Длинная строка с меткой **Token** — это маркер идентификатора экземпляра, который будет вставлен в консоль Firebase &ndash; выберите и скопируйте эту строку в буфер обмена. Если токен идентификатора экземпляра не отображается, добавьте следующую строку в начало `OnCreate` метода, чтобы убедиться, что **google-services.jsв** был проанализирован правильно:

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

`google_app_id`Значение, записываемое в окно вывода, должно соответствовать `mobilesdk_app_id` значению, записанному в **google-services.js**. Объект `Resource.String.google_app_id` создается MSBuild при обработке **google-services.jsв**. 

### <a name="send-a-message"></a>Отправка сообщения

Войдите в [консоль Firebase](https://console.firebase.google.com), выберите свой проект, щелкните **уведомления**и щелкните **отправить первое сообщение**:

[![Кнопка "отправить первое сообщение"](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

На странице **Создание сообщения** введите текст сообщения и выберите **одно устройство**. Скопируйте токен идентификатора экземпляра из окна вывода IDE и вставьте его в поле **токен регистрации FCM** консоли Firebase:

[![Диалоговое окно создания сообщения](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

На устройстве Android (или в эмуляторе) зазадний план приложения, коснувшись кнопки **обзора** Android и прикосновением к начальному экрану. Когда устройство будет готово, щелкните **Отправить сообщение** в консоли Firebase:

[![Кнопка отправки сообщения](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

Когда появится диалоговое окно **проверки сообщения** , нажмите кнопку **Отправить**.
Значок уведомления должен появиться в области уведомлений устройства (или эмулятора):

[![Отображается значок уведомления](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

Чтобы просмотреть сообщение, откройте значок уведомления. Сообщение уведомления должно быть в том, что было введено в **текстовое поле сообщения** консоли Firebase:

[![На устройстве отображается сообщение уведомления](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

Коснитесь значка уведомления, чтобы запустить приложение **фкмклиент** . `Intent`Дополнительные данные, отправленные в **фкмклиент** , перечислены в окне Вывод IDE:

[![Намеренные дополнительные списки из ключа, идентификатора сообщения и ключа свертывания](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

В этом примере параметру **from** присваивается номер проекта Firebase приложения (в этом примере — `41590732` ), а **collapse_key** задается имя пакета (**com. Xamarin. фкмексампле**).
Если сообщение не получено, попробуйте удалить приложение **фкмклиент** на устройстве (или в эмуляторе) и повторите описанные выше действия.

> [!NOTE]
> При принудительном закрытии приложения FCM прекращает доставку уведомлений. Android предотвращает непреднамеренное или ненужные запуски компонентов остановленных приложений в фоновом режиме. (Дополнительные сведения об этом поведении см. [в разделе Запуск элементов управления для остановленных приложений](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) По этой причине необходимо вручную удалить приложение при каждом запуске и его прекращении из сеанса отладки &ndash; . это заставляет FCM создать новый маркер, чтобы сообщения продолжали получаться.

### <a name="add-a-custom-default-notification-icon"></a>Добавление пользовательского значка уведомления по умолчанию

В предыдущем примере значок уведомления установлен в значок приложения. Следующий XML-код настраивает пользовательский значок по умолчанию для уведомлений. В Android отображается этот пользовательский значок по умолчанию для всех сообщений с уведомлениями, в которых значок уведомления не задан явно.

Чтобы добавить пользовательский значок уведомления по умолчанию, добавьте значок в каталог **Resources/Draw** , измените **AndroidManifest.xml**и вставьте в раздел следующий `<meta-data>` элемент `<application>` :

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

В этом примере в качестве пользовательского значка уведомления по умолчанию будет использоваться значок уведомления, находящийся в **ресурсах/нарисованном/мф \_ \_ \_notification.png** . Если пользовательский значок по умолчанию не настроен в **AndroidManifest.xml** и в полезных данных уведомления не задан значок, Android использует значок приложения в качестве значка уведомления (как показано на снимке экрана с изображением на значке уведомления выше).

## <a name="handle-topic-messages"></a>Обработку сообщений раздела

Код, написанный таким же кодом, обрабатывает маркеры регистрации и добавляет в приложение функции удаленного уведомления. В следующем примере добавляется код, который прослушивает *сообщения раздела* и перенаправляет их пользователю как удаленные уведомления. Сообщения раздела — это FCM сообщения, которые отправляются на одно или несколько устройств, подписанных на определенный раздел. Дополнительные сведения о сообщениях раздела см. в [разделе Обмен сообщениями](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

### <a name="subscribe-to-a-topic"></a>Оформление подписки на тему

Измените **Resources/Layout/Main. axml** и добавьте следующее `Button` объявление сразу после предыдущего `Button` элемента:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

Этот XML-файл добавляет кнопку **подписки на уведомление** в макет.
Измените **MainActivity.CS** и добавьте следующий код в конец `OnCreate` метода:

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Этот код находит кнопку **подписки на уведомление** в макете и назначает обработчик щелчка коду, который вызывает `FirebaseMessaging.Instance.SubscribeToTopic` , передавая подписанный раздел, _Новости_. Когда пользователь нажмет кнопку **подписки** , приложение подписывается на раздел _новостей_ . В следующем разделе с помощью графического интерфейса уведомлений консоли Firebase будет отправлено сообщение с темой _новостей_ .

### <a name="send-a-topic-message"></a>Отправка сообщения раздела

Удалите приложение, перестройте его и запустите снова. Нажмите кнопку **Подписка на уведомления** :

[![Кнопка "подписываться на уведомления"](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

Если подписка на приложение успешно выполнена, в окне вывода интегрированной среды разработки вы увидите **раздел Синхронизация разделов** :

[![Окно вывода показывает сообщение о синхронизации раздела "синхронизировано"](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

Чтобы отправить сообщение раздела, выполните следующие действия.

1. В консоли Firebase щелкните **создать сообщение**.

2. На странице **Создание сообщения** введите текст сообщения и выберите  **раздел**.

3. В раскрывающемся меню **раздел** выберите встроенный раздел  **Новости**:

    [![Выбор раздела новостей](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4. На устройстве Android (или в эмуляторе) зазадний план приложения, коснувшись кнопки **обзора** Android и прикосновением к начальному экрану.

5. Когда устройство будет готово, щелкните **Отправить сообщение** в консоли Firebase.

6. Проверьте окно выходных данных интегрированной среды разработки, чтобы увидеть **/топикс/Невс** в выходных данных журнала:

    [![Отображается сообщение от/топик/Невс](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

Если это сообщение отображается в окне вывода, значок уведомления также должен отображаться в области уведомлений на устройстве Android. Откройте значок уведомления, чтобы просмотреть сообщение раздела:

[![Сообщение раздела отображается как уведомление.](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

Если сообщение не получено, попробуйте удалить приложение **фкмклиент** на устройстве (или в эмуляторе) и повторите описанные выше действия.

## <a name="foreground-notifications"></a>Уведомления переднего плана

Чтобы получать уведомления в приложениях переднего плана, необходимо реализовать `FirebaseMessagingService` . Эта служба также необходима для получения полезных данных и отправки вышестоящего сообщения. В следующих примерах показано, как реализовать службу, расширяющую `FirebaseMessagingService` &ndash; полученное приложение, чтобы обрабатывать удаленные уведомления, выполняющиеся на переднем плане.

### <a name="implement-firebasemessagingservice"></a>Реализация Фиребасемессагингсервице

`FirebaseMessagingService`Служба отвечает за получение и обработку сообщений из Firebase. Каждое приложение должно создать подкласс этого типа и переопределить `OnMessageReceived` для обработки входящего сообщения. Когда приложение находится на переднем плане, `OnMessageReceived` обратный вызов всегда обрабатывает сообщение.

> [!NOTE]
> Приложения имеют только 10 секунд для обработки входящего сообщения Firebase Cloud. Любая работа, которая занимает больше времени, должна быть запланирована для фонового выполнения с помощью библиотеки, такой как [планировщик заданий Android](~/android/platform/android-job-scheduler.md) или [диспетчер заданий Firebase](~/android/platform/firebase-job-dispatcher.md).

Добавьте новый файл с именем **MyFirebaseMessagingService.CS** и замените его код шаблона следующим:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

Обратите внимание, что `MESSAGING_EVENT` Фильтр намерения должен быть объявлен таким образом, чтобы новые сообщения FCM направляться в `MyFirebaseMessagingService` :

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

Когда клиентское приложение получает сообщение от FCM, `OnMessageReceived` извлекает содержимое сообщения из переданного `RemoteMessage` объекта путем вызова его `GetNotification` метода. Затем он записывает содержимое сообщения, чтобы его можно было просмотреть в окне вывода IDE:

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> Если вы устанавливаете точки останова в `FirebaseMessagingService` , сеанс отладки может или не достигать этих точек останова из-за того, как FCM доставляет сообщения.

### <a name="send-another-message"></a>Отправить другое сообщение

Удалите приложение, перестройте его, запустите его еще раз и выполните следующие действия, чтобы отправить другое сообщение:

1. В консоли Firebase щелкните **создать сообщение**.

2. На странице **Создание сообщения** введите текст сообщения и выберите  **одно устройство**.

3. Скопируйте строку токена из окна вывод IDE и вставьте ее в поле **токен регистрации FCM** консоли Firebase, как и раньше.

4. Убедитесь, что приложение выполняется на переднем плане, а затем щелкните **Отправить сообщение** в консоли Firebase:

    [![Отправка другого сообщения из консоли](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5. Когда появится диалоговое окно **проверки сообщения** , нажмите кнопку **Отправить**.

6. Входящее сообщение заносится в окно вывода IDE:

    [![Текст сообщения, выводимого в окне вывода](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)

### <a name="add-a-local-notification-sender"></a>Добавление локального отправителя уведомлений

В этом остальном примере входящее сообщение FCM будет преобразовано в локальное уведомление, которое запускается, когда приложение выполняется на переднем плане. Измените **MyFirebaseMessageService.CS** и добавьте следующие `using` инструкции:

```csharp
using FCMClient;
using System.Collections.Generic;
```

Добавьте следующий метод к `MyFirebaseMessagingService`:

<a name="sendnotification-method"></a>

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (var key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }

    var pendingIntent = PendingIntent.GetActivity(this,
                                                  MainActivity.NOTIFICATION_ID,
                                                  intent,
                                                  PendingIntentFlags.OneShot);

    var notificationBuilder = new  NotificationCompat.Builder(this, MainActivity.CHANNEL_ID)
                              .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                              .SetContentTitle("FCM Message")
                              .SetContentText(messageBody)
                              .SetAutoCancel(true)
                              .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(MainActivity.NOTIFICATION_ID, notificationBuilder.Build());
}
```

Чтобы отличить это уведомление от фоновых уведомлений, этот код помечает уведомления значками, которые отличаются от значка приложения. Добавьтеnotification.pngфайл [МФ \_ stat \_ IC \_ ](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) в ресурсы, которые можно **нарисовать** и включить в проект **фкмклиент** .

`SendNotification`Метод использует `NotificationCompat.Builder` для создания уведомления и `NotificationManagerCompat` используется для запуска уведомления. Уведомление содержит `PendingIntent` , которое позволит пользователю открыть приложение и просмотреть содержимое строки, переданной в `messageBody` . Дополнительные сведения о см `NotificationCompat.Builder` . в разделе [локальные уведомления](~/android/app-fundamentals/notifications/local-notifications.md).

Вызовите `SendNotification` метод в конце `OnMessageReceived` метода:

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

В результате этих изменений `SendNotification` будет выполняться каждый раз при получении уведомления, когда приложение находится на переднем плане, а уведомление появится в области уведомлений.

Если приложение находится в фоновом режиме, [полезные данные сообщения](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) будут определять, как обрабатываются сообщения:

- **Уведомление об** &ndash; сообщения будут отправлены в **область уведомлений**. Там будет отображаться локальное уведомление. Когда пользователь отменяет уведомление, приложение запустится.
- **Данные** &ndash; сообщения будут обрабатываться `OnMessageReceived` .
- **Оба варианта** &ndash; сообщения, которые содержат как уведомления, так и полезные данные, будут доставлены в область уведомлений. При запуске приложения полезные данные будут отображаться в той `Extras` части `Intent` , которая использовалась для запуска приложения.

В этом примере, если приложение находится в фоновом режиме, `SendNotification` будет выполняться, если сообщение содержит полезные данные. В противном случае будет запущено фоновое уведомление (показанное ранее в этом пошаговом руководстве).

### <a name="send-the-last-message"></a>Отправить Последнее сообщение

Удалите приложение, перестройте его, запустите повторно, а затем выполните следующие действия, чтобы отправить Последнее сообщение:

1. В консоли Firebase щелкните **создать сообщение**.

2. На странице **Создание сообщения** введите текст сообщения и выберите **одно устройство**.

3. Скопируйте строку токена из окна вывод IDE и вставьте ее в поле **токен регистрации FCM** консоли Firebase, как и раньше.

4. Убедитесь, что приложение выполняется на переднем плане, а затем щелкните **Отправить сообщение** в консоли Firebase:

    [![Отправка сообщения переднего плана](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

На этот раз сообщение, которое было зарегистрировано в окне вывода, также упаковывается в новое уведомление &ndash; . значок уведомления появляется в области уведомлений во время выполнения приложения на переднем плане.

[![Значок уведомления для сообщения переднего плана](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

При открытии уведомления вы увидите Последнее сообщение, отправленное из графического интерфейса уведомлений консоли Firebase:

[![Уведомление переднего плана, отображаемое со значком переднего плана](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)

## <a name="disconnecting-from-fcm"></a>Отсоединение от FCM

Чтобы отказаться от подписки на раздел, вызовите метод [унсубскрибефромтопик](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) класса [фиребасемессагинг](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) . Например, чтобы отказаться от подписки на раздел _новостей_ , подписанный ранее, в макет можно добавить кнопку **отменить подписывание** со следующим кодом обработчика:

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

Чтобы полностью отменить регистрацию устройства в FCM, удалите идентификатор экземпляра, вызвав метод [делетеинстанцеид](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) в классе [идентификатора экземпляра Firebase](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) . Пример:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

Этот вызов метода удаляет идентификатор экземпляра и связанные с ним данные. В результате Периодическая отправка данных FCM на устройство останавливается.

## <a name="troubleshooting"></a>Устранение неполадок

Ниже описаны проблемы и способы их решения, которые могут возникнуть при использовании Firebase Cloud Messaging с Xamarin. Android.

### <a name="firebaseapp-is-not-initialized"></a>Фиребасеапп не инициализирован

В некоторых случаях может появиться следующее сообщение об ошибке:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Это известная проблема, которую можно решить, очистив решение и перестроить проект (**сборка > Очистить решение**, **создать > перестроить решение**).

## <a name="summary"></a>Сводка

В этом пошаговом руководстве подробно описаны шаги по реализации удаленных уведомлений для Firebase облачных сообщений в приложении Xamarin. Android. В нем описано, как установить необходимые пакеты, необходимые для связи FCM, и как настроить манифест Android для доступа к серверам FCM. В нем приведен пример кода, демонстрирующий, как проверить наличие Сервисы Google Play. Было продемонстрировано, как реализовать службу прослушивателя ИДЕНТИФИКАТОРов экземпляров, которая согласовывается с FCM для маркера регистрации и поясняет, как этот код создает фоновые уведомления, когда приложение находится в фоновом режиме. В нем было объяснено, как подписываться на сообщения раздела и предоставлен пример реализации службы прослушивателя сообщений, которая используется для получения и отображения удаленных уведомлений во время выполнения приложения на переднем плане.

## <a name="related-links"></a>Связанные ссылки

- [Фкмнотификатионс (пример)](/samples/xamarin/monodroid-samples/firebase-fcmnotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [О сообщениях FCM](https://firebase.google.com/docs/cloud-messaging/concept-options)
