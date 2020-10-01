---
title: Службы расположения в Android
description: В этом руководстве рассматривается отслеживание расположения в приложениях Android и описывается, как узнать расположение пользователя с помощью API службы расположения Android, а также Fused Location Provider, доступного в API служб расположения Google.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/22/2018
ms.openlocfilehash: 37bfed9a2b49be2d04f58dab70d3185418f0f67d
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457475"
---
# <a name="location-services-on-android"></a>Службы расположения в Android

_В этом руководстве рассматривается отслеживание расположения в приложениях Android и описывается, как узнать расположение пользователя с помощью API службы расположения Android, а также Fused Location Provider, доступного в API служб расположения Google._

Android предоставляет доступ к различным технологиям определения расположения, например расположения вышек сотовой связи, Wi-Fi и GPS. Сведения о каждой технологии определения расположения абстрагированы от *поставщиков сведений о расположении*. Это позволяет приложениям получать расположения одинаковым способом независимо от используемого поставщика. В этом руководстве рассматривается Fused Location Provider — компонент Сервисов Google Play, который интеллектуально определяет лучший метод получения расположения устройств в зависимости от доступных поставщиков и способа использования устройства. Мы рассмотрим API службы расположения Android и узнаем, как взаимодействовать с системной службой расположения с помощью `LocationManager`. Во второй части этого руководством рассматривается использование API служб расположения Android с помощью `LocationManager`.

Как правило, в приложениях следует использовать Fused Location Provider вместо устаревшего API службы расположения Android, только если это необходимо.

## <a name="location-fundamentals"></a>Принципы определения расположения

Независимо от того, какой API вы выбрали для работы с данными расположения, некоторые принципы в Android неизменны. В этом разделе описываются поставщики сведений о расположении и разрешения, связанные с расположением.

### <a name="location-providers"></a>Поставщики сведений о расположении

Для определения расположения пользователя используется несколько технологий. Используемое оборудование зависит от типа *поставщика сведений о расположении*, выбранного для сбора данных. Android использует три поставщика сведений о расположении.

- **Поставщик данных GPS** &ndash; технология GPS обеспечивает наиболее точные сведения о расположении, потребляет больше всего электроэнергии и лучше всего подходит для туризма. Этот поставщик использует сочетание технологии GPS и технологии [aGPS](https://en.wikipedia.org/wiki/Assisted_GPS), которая возвращает данные GPS, собранные вышками сотовой связи.

- **Поставщик данных сети** &ndash; предоставляет сочетание данных Wi-Fi и сотовой связи, включая данные aGPS, собранные вышками сотовой связи. Он использует меньше электроэнергии, чем поставщик данных GPS, но возвращает данные о расположении переменной точности.

- **Пассивный поставщик** &ndash; дополнительный вариант, при котором другие приложения или службы запрашивают у поставщика создание данных расположения в приложении. Это менее надежный, но более энергосберегающий вариант, который идеально подходит для приложений, для работы которых не требуется постоянное обновление данных расположения.

Поставщики сведений о расположении доступны не всегда. Например, может потребоваться использовать GPS для приложения, но функция GPS может быть отключена в параметрах или устройство может вообще не поддерживать технологию GPS. Если конкретный поставщик недоступен, при выборе этого поставщика может быть возвращено значение `null`.

### <a name="location-permissions"></a>Разрешения расположения

Приложению с поддержкой расположения требуется доступ к аппаратным датчикам устройства для получения данных GPS, Wi-Fi и сотовой сети. Управление доступом осуществляется с помощью соответствующих разрешений в манифесте приложения Android.
Могут быть доступны два разрешения. В зависимости от требований вашего приложения и выбранного API, необходимо разрешить одно из следующих действий.

- `ACCESS_FINE_LOCATION` &ndash; разрешает приложению доступ к GPS.
    Требуется для использования *поставщика данных GPS* и *пассивного поставщика* (*пассивному поставщику требуется разрешение на доступ к данным GPS, собираемым другим приложением или службой*). Необязательное разрешение для *поставщика данных сети*.

- `ACCESS_COARSE_LOCATION` &ndash; разрешает приложению доступ к расположению вышек сотовой связи и Wi-Fi. Требуется для *поставщика данных сети*, если не задано `ACCESS_FINE_LOCATION`.

Для приложений, нацеленных на API версии 21 (Android 5.0 Lollipop) или более поздней версии, можно включить `ACCESS_FINE_LOCATION` и запускать их на устройствах, на которых отсутствует оборудование GPS. Если для приложения требуется оборудование GPS, необходимо явным образом добавить элемент `android.hardware.location.gps` `uses-feature` в манифест Android. Дополнительные сведения доступны в справочных материалах по элементу [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) Android.

Чтобы задать разрешения, на **Панели решения** разверните папку **Свойства** и дважды щелкните файл **AndroidManifest.xml**. Разрешения будут перечислены в разделе **Необходимые разрешения**.

[![Снимок экрана: параметры обязательных разрешений в манифесте Android](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Установка любого из этих разрешений укажет Android, что приложению требуется разрешение пользователя для доступа к поставщикам сведений о расположении. Устройства, использующие уровень API 22 (Android 5.1) или ниже, будут предлагать пользователю предоставить эти разрешения при каждой установке приложения. На устройствах с уровнем API 23 (Android 6.0) или выше приложение должно будет выполнить проверку разрешений во время выполнения, прежде чем отправить запрос к поставщику сведений о расположении. 

> [!NOTE]
>Примечание. Установка `ACCESS_FINE_LOCATION` подразумевает доступ к примерным и точным данным расположения. Не нужно задавать оба разрешения. Задайте только *минимальное* разрешение, достаточное для работы приложения.

В этом фрагменте кода приведен пример того, как проверить, имеет ли приложение разрешение для `ACCESS_FINE_LOCATION`.

```csharp
 if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.AccessFineLocation) == Permission.Granted)
{
    StartRequestingLocationUpdates();
    isRequestingLocationUpdates = true;
}
else
{
    // The app does not have permission ACCESS_FINE_LOCATION 
}
```

Приложения должны быть устойчивы к ситуациям, когда пользователь не предоставил разрешение (или отменил его), и продолжать работу. Дополнительные сведения о реализации проверок разрешений во время выполнения в Xamarin.Android приведены в [руководстве по управлению разрешениями](~/android/app-fundamentals/permissions.md).

## <a name="using-the-fused-location-provider"></a>Использование Fused Location Provider

Поставщик Fused Location Provider предпочтителен для приложений Android, которые получают данные об изменении расположения от устройства, так как он эффективно выбирает поставщик сведений о расположении во время выполнения, чтобы обеспечить оптимальную информацию о расположении и экономить заряд аккумулятора. Например, пользователь, гуляющий на открытом воздухе, получает лучшие координаты расположения с помощью GPS. Если он затем зайдет в здание, где сигнал GPS слабый (или отсутствует), Fused Location Provider может автоматически переключиться на Wi-Fi, который лучше работает в помещениях.

API Fused Location Provider предоставляет множество других инструментов для расширения возможностей определения расположения, включая геозоны и мониторинг активности. В этом разделе описываются основы настройки `LocationClient`, установка поставщиков и получение данных о расположении пользователя.

Fused Location Provider является компонентом [Сервисов Google Play](https://developer.android.com/google/play-services/index.html).
Для работы API Fused Location Provider в приложении должен быть правильно установлен и настроен пакет Сервисов Google Play, а на устройстве должен быть установлен APK Сервисов Google Play.

Прежде чем приложение Xamarin.Android сможет использовать Fused Location Provider, в проект нужно добавить пакет **Xamarin.GooglePlayServices.Location**. Кроме того, во все файлы исходного кода, которые ссылаются на классы, описанные ниже, должны быть добавлены следующие инструкции `using`.

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Проверка установки Сервисов Google Play

Работа Xamarin.Android завершится аварийно, если при попытке использовать Fused Location Provider не будут установлены Сервисы Google Play (или этот они окажутся устаревшими). В этом случае возникнет исключение времени выполнения.  Если Сервисы Google Play не установлены, приложение должно вернуться к использованию службы расположения Android, о которой говорилось выше. Если Сервисы Google Play устарели, приложение может отобразить для пользователя сообщение с просьбой обновить установленную версию Сервисов Google Play.

В этом фрагменте кода приведен пример того, как действие Android может программно проверить, установлены ли Сервисы Google Play.

```csharp
bool IsGooglePlayServicesInstalled()
{
    var queryResult = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
    if (queryResult == ConnectionResult.Success)
    {
        Log.Info("MainActivity", "Google Play Services is installed on this device.");
        return true;
    }

    if (GoogleApiAvailability.Instance.IsUserResolvableError(queryResult))
    {
        // Check if there is a way the user can resolve the issue
        var errorString = GoogleApiAvailability.Instance.GetErrorString(queryResult);
        Log.Error("MainActivity", "There is a problem with Google Play Services on this device: {0} - {1}",
                  queryResult, errorString);

        // Alternately, display the error to the user.
    }

    return false;
}
```

### <a name="fusedlocationproviderclient"></a>FusedLocationProviderClient

Для взаимодействия с Fused Location Provider приложению Xamarin.Android нужен экземпляр `FusedLocationProviderClient`. Этот класс предоставляет необходимые методы для подписки на данные об изменении расположения и получения последнего известного расположения устройства.

Метод `OnCreate` действия подходит для получения ссылки на `FusedLocationProviderClient`, как показано в следующем фрагменте кода.

```csharp
public class MainActivity: AppCompatActivity
{
    FusedLocationProviderClient fusedLocationProviderClient;

    protected override void OnCreate(Bundle bundle) 
    {
        fusedLocationProviderClient = LocationServices.GetFusedLocationProviderClient(this);
    }
}
```

### <a name="getting-the-last-known-location"></a>Получение последнего известного расположения

Метод `FusedLocationProviderClient.GetLastLocationAsync()` предоставляет простой и неблокирующий способ, позволяющий приложению Xamarin.Android быстро получить последнее известное расположение устройства с минимальными затратами на написание кода.

В этом фрагменте кода показано, как использовать метод `GetLastLocationAsync` для получения данных о расположении устройства.

```csharp
async Task GetLastLocationFromDevice()
{
    // This method assumes that the necessary run-time permission checks have succeeded.
    getLastLocationButton.SetText(Resource.String.getting_last_location);
    Android.Locations.Location location = await fusedLocationProviderClient.GetLastLocationAsync();

    if (location == null)
    {
        // Seldom happens, but should code that handles this scenario
    }
    else
    {
        // Do something with the location 
        Log.Debug("Sample", "The latitude is " + location.Latitude);
    }
}
```

### <a name="subscribing-to-location-updates"></a>Подписка на данные об изменении расположения

Приложение Xamarin.Android может также подписаться на данные об изменении расположения из Fused Location Provider с помощью метода `FusedLocationProviderClient.RequestLocationUpdatesAsync`, как показано в следующем фрагменте кода.

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```

Этот метод принимает два параметра.

- **`Android.Gms.Location.LocationRequest`** . Объект `LocationRequest` определяет, как приложение Xamarin.Android передает параметры работы Fused Location Provider. `LocationRequest` содержит такие сведения, как часто следует отправлять запросы или какова важность точности данных об изменении расположения. Например, важный запрос данных о расположении приведет к тому, что при определении расположения устройство будет использовать GPS и, следовательно, его энергопотребление возрастет. В этом фрагменте кода показано, как создать `LocationRequest` для расположения с высокой точностью и примерно каждые пять минут проверять наличие изменений расположения, отправляя запросы не чаще чем раз в две минуты. Fused Location Provider будет использовать `LocationRequest`, чтобы определять, какой поставщик сведений о расположении использовать при попытке определить расположение устройства.

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```

- **`Android.Gms.Location.LocationCallback`** . Чтобы получать данные об изменении расположения, приложение Xamarin.Android должно вывести подкласс из абстрактного класса `LocationProvider`. Этот класс предоставлял два метода, которые могут быть вызваны Fused Location Provider для передачи в приложение обновленных сведений о расположении. Это будет подробнее рассмотрено ниже.

Чтобы уведомить приложение Xamarin.Android об изменении расположения, Fused Location Provider вызовет `LocationCallBack.OnLocationResult(LocationResult result)`. Параметр `Android.Gms.Location.LocationResult` будет содержать сведения об изменении расположения.

Когда Fused Location Provider обнаруживает изменение доступности данных о расположении, он вызывает метод `LocationProvider.OnLocationAvailability(LocationAvailability
locationAvailability)`. Если свойство `LocationAvailability.IsLocationAvailable` возвращает `true`, то можно предположить, что результаты обнаружения расположения устройства, переданные `OnLocationResult`, являются точными и актуальными, что соответствует требованиям `LocationRequest`. Если `IsLocationAvailable` имеет значение false, то `OnLocationResult` не будет возвращать результаты поиска.

Этот фрагмент кода является примером реализации объекта `LocationCallback`.

```csharp
public class FusedLocationProviderCallback : LocationCallback
{
    readonly MainActivity activity;

    public FusedLocationProviderCallback(MainActivity activity)
    {
        this.activity = activity;
    }

    public override void OnLocationAvailability(LocationAvailability locationAvailability)
    {
        Log.Debug("FusedLocationProviderSample", "IsLocationAvailable: {0}",locationAvailability.IsLocationAvailable);
    }

    public override void OnLocationResult(LocationResult result)
    {
        if (result.Locations.Any())
        {
            var location = result.Locations.First();
            Log.Debug("Sample", "The latitude is :" + location.Latitude);
        }
        else
        {
            // No locations to work with.
        }
    }
}
```

## <a name="using-the-android-location-service-api"></a>Использование API службы расположения Android

Служба расположения Android — это более старый API для работы со сведениями о расположении в Android. Данные о расположении собираются аппаратными датчиками и передаются в системную службу, к которой можно обращаться в приложении с помощью класса `LocationManager` и интерфейса `ILocationListener`.

Служба расположения лучше всего подходит для приложений, работающих на устройствах, на которых не установлены Сервисы Google Play.

Служба расположения — это особый тип [службы](https://developer.android.com/guide/components/services.html), управляемой системой. Системная служба взаимодействует с оборудованием устройства и работает непрерывно. Чтобы использовать данные об изменении расположения в нашем приложении, мы подпишемся на эти данные из системной службы расположения, используя `LocationManager` и вызов `RequestLocationUpdates`.

Чтобы получить расположение пользователя с помощью службы расположения Android, требуется выполнить несколько действий.

1. Получите ссылку на службу `LocationManager`.
2. Реализуйте интерфейс `ILocationListener` и обрабатывайте события при изменении расположения.
3. Используйте `LocationManager`, чтобы запрашивать данные об изменении расположения для указанного поставщика. `ILocationListener` из предыдущего шага будет использоваться для получения обратных вызовов от `LocationManager`.
4. Прекратите получение данных об изменении расположения, когда они перестанут быть нужны приложению.

### <a name="location-manager"></a>Диспетчер расположения

Можно получить доступ к системной службе расположения с помощью экземпляра класса `LocationManager`. `LocationManager` — это специальный класс, который позволяет взаимодействовать с системной службой расположения и вызывать методы для нее. Приложение может получить ссылку на `LocationManager`, вызвав `GetSystemService` и передав тип службы, как показано ниже.

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` удобно использовать для получения ссылки на `LocationManager`.
Рекомендуется задать `LocationManager` в качестве переменной класса, чтобы мы могли вызвать ее в различных точках жизненного цикла действия.

### <a name="request-location-updates-from-the-locationmanager"></a>Запрос данных об изменении расположения из LocationManager

Когда приложение получит ссылку на `LocationManager`, ему нужно указать `LocationManager` требуемый тип сведений о расположении и частоту обновления данных. Для этого вызовите `RequestLocationUpdates` для объекта `LocationManager` и передайте в него какие-либо критерии для данных об изменении и обратный вызов для получения данных об изменении расположения. Этот обратный вызов является типом, который должен реализовывать интерфейс `ILocationListener` (более подробно это описывается далее в этом разделе).

Метод `RequestLocationUpdates` сообщает системной службе расположения, что приложение должно начать получать данные об изменении расположения. Этот метод позволяет указать поставщик, а также пороги времени и расстояния для управления частотой обновления. Например, приведенный ниже метод запрашивает данные об изменении расположения от поставщика сведений о расположении GPS каждые 2000 миллисекунд и только при изменении расположения более чем на 1 метр.

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Приложение должно запрашивать данные об изменении расположения только так часто, как это необходимо для его эффективной работы. Это увеличивает время работы аккумулятора и повышает удобство работы пользователя.

### <a name="responding-to-updates-from-the-locationmanager"></a>Реагирование на данные об изменении расположения из LocationManager

После того как приложение запросило данные об изменении из `LocationManager`, оно может получить сведения от службы, реализовав интерфейс [`ILocationListener`](xref:Android.Locations.ILocationListener). Этот интерфейс предоставляет четыре метода для ожидания передачи данных службы расположения и поставщика сведений о расположении, `OnLocationChanged`. Система будет вызывать `OnLocationChanged`, когда расположение пользователя изменится настолько, чтобы соответствовать критериям изменения, заданным при запросе данных об изменении расположения. 

В следующем коде показаны методы в интерфейсе `ILocationListener`.

```csharp
public class MainActivity : AppCompatActivity, ILocationListener
{
    TextView latitude;
    TextView longitude;
    
    public void OnLocationChanged (Location location)
    {
        // called when the location has been updated.
    }
    
    public OnProviderDisabled(string locationProvider)
    {
        // called when the user disables the provider
    }
    
    public OnProviderEnabled(string locationProvider)
    {
        // called when the user enables the provider
    }
    
    public OnStatusChanged(string locationProvider, Availability status, Bundle extras)
    {
        // called when the status of the provider changes (there are a variety of reasons for this)
    }
}
```

### <a name="unsubscribing-to-locationmanager-updates"></a>Отмена подписки на данные об изменении из LocationManager

Чтобы обеспечить экономию системных ресурсов, приложение должно как можно скорее отменить подписку на данные об изменении расположения. Метод `RemoveUpdates` указывает `LocationManager` остановить отправку данных об изменении в наше приложение.  Например, действие может вызвать `RemoveUpdates` в методе `OnPause`, чтобы снизить энергопотребление, если приложение не нуждается в данных об изменении расположения, пока это действие не отображается на экране.

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Если приложению необходимо получать данные об изменении расположения в фоновом режиме, необходимо создать пользовательскую службу, которая подписывается на системную службу расположения. Дополнительные сведения см. в руководстве по [использованию фонового режима для служб Android](~/android/app-fundamentals/services/index.md).

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>Определение лучшего поставщика сведений о расположении для LocationManager

Приведенное выше приложение задает поставщик сведений о расположении GPS. Однако использовать GPS не всегда возможно. Например, устройство может находиться в помещении или не обладать приемником GPS. В этом случае при запросе поставщика возвращается значение `null`.

Чтобы приложение работало, когда использование GPS невозможно, добавьте метод `GetBestProvider`, чтобы при запуске приложения запросить наилучший доступный поставщик сведений о расположении (поддерживаемый устройством и подходящий для пользователя). Вместо передачи конкретного поставщика с помощью объекта [`Criteria` ](xref:Android.Locations.Criteria)можно указать `GetBestProvider` требования к поставщику, такие как точность и энергопотребление. `GetBestProvider` возвращает наиболее подходящий поставщик для заданных критериев.

В следующем коде показано, как получить наилучший доступный поставщик и использовать его при запросе данных об изменении расположения.

```csharp
Criteria locationCriteria = new Criteria();   
locationCriteria.Accuracy = Accuracy.Coarse;
locationCriteria.PowerRequirement = Power.Medium;

locationProvider = locationManager.GetBestProvider(locationCriteria, true);

if(locationProvider != null)
{
    locationManager.RequestLocationUpdates (locationProvider, 2000, 1, this);
}
else
{
    Log.Info(tag, "No location providers available");
}
```

> [!NOTE]
> Если пользователь отключил все поставщики сведений о расположении, `GetBestProvider` вернет значение `null`. Чтобы увидеть, как этот код работает на реальном устройстве, обязательно включите GPS, Wi-Fi и сети сотовой связи, выбрав **Параметры Google > Расположение > Режим**, как показано на следующем снимке экрана.
>
> [![Экран параметров режима определения расположения на телефоне Android](location-images/location-02.png)](location-images/location-02.png#lightbox)
>
> На следующем снимке экрана показано приложение определения расположения, использующее `GetBestProvider`.
>
> [![Приложение GetBestProvider, отображающее широту, долготу и поставщика](location-images/location-03.png)](location-images/location-03.png#lightbox)
>
> Помните, что `GetBestProvider` не изменяет поставщик динамически. Вместо этого он один раз за жизненный цикл действия определяет наилучший доступный поставщик. Если установки поставщика его состояние изменится, приложению потребуется дополнительный код в методах `ILocationListener` &ndash; `OnProviderEnabled`, `OnProviderDisabled` и `OnStatusChanged` &ndash;, чтобы обработать все возможности, связанные с переключением поставщика.

## <a name="summary"></a>Сводка

В этом разделе описано получение расположения пользователя с помощью службы расположения Android и Fused Location Provider из API служб расположения Google.

## <a name="related-links"></a>Связанные ссылки

- [Location (пример)](/samples/xamarin/monodroid-samples/location)
- [FusedLocationProvider (пример)](/samples/xamarin/monodroid-samples/fusedlocationprovider)
- [Сервисы Google Play](https://developer.android.com/google/play-services/index.html)
- [Класс Criteria](xref:Android.Locations.Criteria)
- [Класс LocationManager](xref:Android.Locations.LocationManager)
- [Класс LocationListener](xref:Android.Locations.ILocationListener)
- [API LocationClient](https://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [API LocationListener](https://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [API LocationRequest](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)