---
title: Пошаговое руководство. фоновое расположение в Xamarin. iOS
description: В этом документе представлено пошаговое руководство по использованию сведений о расположении в фоновом приложении Xamarin. iOS. Он описывает необходимые настройки, Пользовательский интерфейс и состояния приложений.
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 2350db2e8d4f43a33b0ce394e06ffd2c16b6b7ad
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436551"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>Пошаговое руководство. фоновое расположение в Xamarin. iOS

В этом примере мы собираемся создать приложение для расположения iOS, которое выводит на экран сведения о текущем расположении: широту, долготу и другие параметры. Это приложение показывает, как правильно выполнять обновления расположения, пока приложение активно или находится в фоновом режиме.

В этом пошаговом руководстве рассматриваются некоторые основные понятия, включая регистрацию приложения в качестве фонового приложения, приостановку обновлений пользовательского интерфейса при работе с приложением в фоновом режиме и работу с `WillEnterBackground` `WillEnterForeground` `AppDelegate` методами и.

## <a name="application-set-up"></a>Настройка приложения

1. Сначала создайте новое **приложение > iOS > приложении с одним представлением (C#)**. Назовите _его и_ убедитесь, что выбраны оба iPad и iPhone.

1. Приложение расположения является приложением в iOS как необходимое для фонового применения. Зарегистрируйте приложение в качестве приложения расположения, изменив файл **info. plist** для проекта.

    В разделе обозреватель решений дважды щелкните файл **info. plist** , чтобы открыть его, и прокрутите список вниз. Установите флажок **включить фоновые режимы** и **обновления расположения** .

    В Visual Studio для Mac он будет выглядеть примерно так:

    [![Установите флажок "включить фоновые режимы" и "расположение обновлений".](location-walkthrough-images/image7.png)](location-walkthrough-images/image7.png#lightbox)

    В Visual Studio необходимо вручную обновить **info. plist** , добавив следующую пару "ключ-значение":

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. Теперь, когда приложение зарегистрировано, оно может получать данные о расположении с устройства. В iOS `CLLocationManager` класс используется для доступа к сведениям о расположении и может вызывать события, предоставляющие обновления расположения.

1. В коде создайте новый класс `LocationManager` с именем, который предоставляет единое место для различных экранов и кода для подписки на обновления расположения. В `LocationManager` классе создайте экземпляр `CLLocationManager` вызванного `LocMgr` :

    ```csharp
    public class LocationManager
    {
        protected CLLocationManager locMgr;

        public LocationManager () {
            this.locMgr = new CLLocationManager();
            this.locMgr.PausesLocationUpdatesAutomatically = false;

            // iOS 8 has additional permissions requirements
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                locMgr.RequestAlwaysAuthorization (); // works in background
                //locMgr.RequestWhenInUseAuthorization (); // only in foreground
            }

            if (UIDevice.CurrentDevice.CheckSystemVersion (9, 0)) {
                locMgr.AllowsBackgroundLocationUpdates = true;
            }
        }

        public CLLocationManager LocMgr {
            get { return this.locMgr; }
        }
    }
    ```

    Приведенный выше код задает ряд свойств и разрешений для класса [кллокатионманажер](xref:CoreLocation.CLLocationManager) :

    - `PausesLocationUpdatesAutomatically` — Это логическое значение, которое можно задать в зависимости от того, разрешена ли системе приостановить обновления расположения. На некоторых устройствах по умолчанию используется `true` , что может привести к тому, что устройство перестает получать обновления в фоновом расположении через 15 минут.
    - `RequestAlwaysAuthorization` — Необходимо передать этот метод, чтобы предоставить пользователю приложения возможность разрешить доступ к расположению в фоновом режиме. `RequestWhenInUseAuthorization` также можно передать, если вы хотите предоставить пользователю возможность разрешить доступ к расположению только в том случае, если приложение находится на переднем плане.
    - `AllowsBackgroundLocationUpdates` — Это логическое свойство, представленное в iOS 9, которое можно задать, чтобы разрешить приложению получать обновления расположения при приостановке.

    > [!IMPORTANT]
    > в iOS 8 (и более поздних версиях) также требуется запись в файле **info. plist** , чтобы отобразить пользователя как часть запроса на авторизацию.

1. Добавьте ключи **info. plist** для типов разрешений, которые требуются приложению — `NSLocationAlwaysUsageDescription` , `NSLocationWhenInUseUsageDescription` и (или) `NSLocationAlwaysAndWhenInUseUsageDescription` — со строкой, которая будет отображаться пользователю в предупреждении, которое запрашивает доступ к данным о расположении.

1. iOS 9 требует, чтобы при `AllowsBackgroundLocationUpdates` использовании **info. plist** ключ был включен в `UIBackgroundModes` значение `location` . Если вы выполнили шаг 2 этого пошагового руководства, он уже должен находиться в файле info. plist.

1. Внутри `LocationManager` класса создайте метод `StartLocationUpdates` с именем со следующим кодом. В этом коде показано, как начать получение обновлений расположения из   `CLLocationManager` :

    ```csharp
    if (CLLocationManager.LocationServicesEnabled) {
        //set the desired accuracy, in meters
        LocMgr.DesiredAccuracy = 1;
        LocMgr.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) =>
        {
            // fire our custom Location Updated event
            LocationUpdated (this, new LocationUpdatedEventArgs (e.Locations [e.Locations.Length - 1]));
        };
        LocMgr.StartUpdatingLocation();
    }
    ```

    В этом методе происходит несколько важных моментов. Сначала мы пропроверяем, имеет ли приложение доступ к данным о расположении на устройстве. Мы проверяем это, вызвав метод `LocationServicesEnabled` в `CLLocationManager` . Этот метод возвращает **значение false** , если пользователь запретил приложению доступ к сведениям о расположении.

1. Затем сообщите диспетчеру расположения, как часто следует обновлять. `CLLocationManager` предоставляет множество параметров для фильтрации и настройки данных о расположении, включая частоту обновлений. В этом примере задайте значение, которое будет `DesiredAccuracy` обновляться всякий раз, когда расположение изменится счетчиком. Дополнительные сведения о настройке частоты обновления расположения и других параметрах см. в [справочнике по классу кллокатионманажер](https://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) в документации Apple.

1. Наконец, вызовите метод `StartUpdatingLocation` в `CLLocationManager` экземпляре. Это указывает диспетчеру местоположения, что необходимо получить начальное исправление в текущем расположении, и начать отправку обновлений

До сих пор диспетчер расположений создан, настроен с типами данных, которые мы хотим получить, и определил исходное расположение. Теперь код должен визуализировать данные о расположении в пользовательском интерфейсе. Это можно сделать с помощью пользовательского события, которое принимает в `CLLocation` качестве аргумента:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

Следующим шагом является подписка на обновления расположения из и `CLLocationManager` `LocationUpdated` Создание пользовательского события, когда становятся доступными новые данные о расположении, передавая в качестве аргумента расположение. Для этого создайте новый класс **LocationUpdateEventArgs.CS**. Этот код доступен в основном приложении и возвращает расположение устройства при возникновении события:

```csharp
public class LocationUpdatedEventArgs : EventArgs
{
    CLLocation location;

    public LocationUpdatedEventArgs(CLLocation location)
    {
       this.location = location;
    }

    public CLLocation Location
    {
       get { return location; }
    }
}
```

## <a name="user-interface"></a>Пользовательский интерфейс

1. Используйте конструктор iOS для создания экрана, в котором будут отображаться сведения о расположении. Дважды щелкните файл **Main. Storyboard** , чтобы начать.

    На экране раскадровки перетащите несколько меток на экран, чтобы они действовали в качестве заполнителей для сведений о расположении. В этом примере имеются метки для широты, долготы, высоты, курса и скорости.

    Макет должен выглядеть следующим образом:

    ![Пример макета пользовательского интерфейса в конструкторе iOS](location-walkthrough-images/image8.png)

1. В Панель решения дважды щелкните `ViewController.cs` файл и измените его, чтобы создать новый экземпляр локатионманажер и вызвать `StartLocationUpdates` на нем.
  Измените код, чтобы он выглядел следующим образом:

    ```csharp
    #region Computed Properties
    public static bool UserInterfaceIdiomIsPhone {
        get { return UIDevice.CurrentDevice.UserInterfaceIdiom == UIUserInterfaceIdiom.Phone; }
    }

    public static LocationManager Manager { get; set;}
    #endregion

    #region Constructors
    public ViewController (IntPtr handle) : base (handle)
    {
    // As soon as the app is done launching, begin generating location updates in the location manager
        Manager = new LocationManager();
        Manager.StartLocationUpdates();
    }

    #endregion
    ```

    Это приведет к запуску обновлений расположения при запуске приложения, хотя данные не будут отображаться.

1. Теперь, когда обновления расположения получены, обновите экран, указав сведения о расположении. Следующий метод получает расположение из нашего `LocationUpdated` события и показывает его в пользовательском интерфейсе:

    ```csharp
    #region Public Methods
    public void HandleLocationChanged (object sender, LocationUpdatedEventArgs e)
    {
        // Handle foreground updates
        CLLocation location = e.Location;

        LblAltitude.Text = location.Altitude + " meters";
        LblLongitude.Text = location.Coordinate.Longitude.ToString ();
        LblLatitude.Text = location.Coordinate.Latitude.ToString ();
        LblCourse.Text = location.Course.ToString ();
        LblSpeed.Text = location.Speed.ToString ();

        Console.WriteLine ("foreground updated");
    }
    #endregion
    ```

Нам по-прежнему нужно подписываться на `LocationUpdated` событие в нашей AppDelegate и вызывать новый метод для обновления пользовательского интерфейса. Добавьте следующий код `ViewDidLoad,` сразу после `StartLocationUpdates` вызова:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```

Теперь при запуске приложения он должен выглядеть примерно так:

[![Пример выполнения приложения](location-walkthrough-images/image5.png)](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>Обработка активных и фоновых состояний

1. Приложение выводит обновления расположения на переднем плане и активно. Чтобы продемонстрировать, что происходит при входе приложения в фоновый режим, переопределите `AppDelegate` методы, которые отслеживании изменений состояния приложения, чтобы приложение записывало данные в консоль при переходе между передним и задним планом:

    ```csharp
    public override void DidEnterBackground (UIApplication application)
    {
        Console.WriteLine ("App entering background state.");
    }

    public override void WillEnterForeground (UIApplication application)
    {
        Console.WriteLine ("App will enter foreground");
    }
    ```

    Добавьте следующий код в, `LocationManager` чтобы постоянно печатать обновленные данные о расположении в выходных данных приложения, чтобы убедиться, что сведения о расположении по-прежнему доступны в фоновом режиме:

    ```csharp
    public class LocationManager
    {
        public LocationManager ()
        {
        ...
        LocationUpdated += PrintLocation;
        }
        ...

        //This will keep going in the background and the foreground
        public void PrintLocation (object sender, LocationUpdatedEventArgs e) {
        CLLocation location = e.Location;
        Console.WriteLine ("Altitude: " + location.Altitude + " meters");
        Console.WriteLine ("Longitude: " + location.Coordinate.Longitude);
        Console.WriteLine ("Latitude: " + location.Coordinate.Latitude);
        Console.WriteLine ("Course: " + location.Course);
        Console.WriteLine ("Speed: " + location.Speed);
        }
    }
    ```

1. Существует одна из оставшихся проблем с кодом: попытка обновить пользовательский интерфейс, если приложение находится в фоновом режиме, приведет к прерыванию iOS. Когда приложение переходит в фоновый режим, код должен отказаться от обновления расположения и прерывать обновление пользовательского интерфейса.

    iOS предоставляет нам уведомления, когда приложение собирается перейти на другое состояние приложения. В этом случае мы можем подписываться на `ObserveDidEnterBackground` уведомление.

    В следующем фрагменте кода показано, как использовать уведомление, чтобы позволить представлению узнать, когда следует остановить обновления пользовательского интерфейса. Будет отправлено `ViewDidLoad` следующее:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    Когда приложение выполняется, выходные данные будут выглядеть примерно так:

    ![Пример выходных данных расположения в консоли](location-walkthrough-images/image6.png)

1. Приложение распечатывает место на экране при работе на переднем плане и при работе в фоновом режиме продолжит печать данных в окне вывода приложения.

Остается только одна незавершенная ошибка: экран запускает обновление пользовательского интерфейса при первой загрузке приложения, но не имеет возможности узнать, когда приложение повторно вводит переднему план. Если фоновое приложение возвращается на передний план, обновления пользовательского интерфейса не возобновляются.

Чтобы устранить эту проблему, вложите вызов для запуска обновлений пользовательского интерфейса в другом уведомлении, которое будет срабатывать, когда приложение переходит в активное состояние:

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

Теперь пользовательский интерфейс начнет обновляться при первом запуске приложения и возобновлять обновление каждый раз, когда приложение возвращается на передний план.

В этом пошаговом руководстве мы создали правильно настроенное фоновое приложение iOS, которое выводит данные о расположении на экран и в окно вывода приложения.

## <a name="related-links"></a>Связанные ссылки

- [Расположение (часть 4) (пример)](/samples/xamarin/ios-samples/location)
- [Справочник по инфраструктуре основных расположений](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)