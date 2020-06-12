---
title: Использование API Карт Google в приложении
description: Как реализовать функции API Карт Google версии 2 в приложении Xamarin.Android.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: 2779019fff1354b18d285775daf224ffc1402fd4
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571367"
---
# <a name="using-the-google-maps-api-in-your-application"></a>Использование API Карт Google в приложении

Использовать приложение Карты удобно, но иногда требуется добавить карты непосредственно в свое приложение. Помимо встроенного приложения карт, Google также предлагает [собственный API сопоставления для Android](https://developers.google.com/maps/documentation/android-sdk/intro).
API Карт подходит для случаев, когда требуется обеспечить больший контроль над процессом сопоставления. Возможности API Карт:

- программное изменение перспективы на карте;
- добавление и настройка маркеров;
- наложение на карту заметок.

В отличие от устаревшего API Карт Google для Android версии 1, API Карт Google для Android версии 2 является частью [Сервисов Google Play](https://developers.google.com/android/guides/overview).
Приложение Xamarin.Android должно соответствовать определенным требованиям, чтобы использовать API Карт Google для Android.

## <a name="google-maps-api-prerequisites"></a>Необходимые условия для использования API Карт Google

Прежде чем можно будет использовать API Карт, необходимо выполнить несколько шагов, в том числе:

- [получить ключ API Карт](#obtain-maps-key);
- [установить пакет SDK Сервисов Google Play](#install-gps-sdk);
- [установить пакет Xamarin.GooglePlayServices.Maps из NuGet](#install-gpsmaps-nuget);
- [указать необходимые разрешения](#declare-permissions);
- [(необязательно) создать эмулятор с помощью API Google](#create-emulator-with-google-api).

### <a name="obtain-a-google-maps-api-key"></a><a name="obtain-maps-key"></a>Получение ключа API Карт Google

Первым шагом является получение ключа API Карт Google. Учтите, что невозможно будет использовать ключ API из устаревшего выпуска API Карт Google версии 1. Сведения о том, как получить ключ API и использовать его в Xamarin.Android, см. в разделе [Получение ключа API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

### <a name="install-the-google-play-services-sdk"></a><a name="install-gps-sdk"></a> Установка пакета SDK Сервисов Google Play

Сервисы Google Play — это технология Google, которая позволяет приложениям Android использовать преимущества различных функций Google, таких как Google+, In-App Billing и Карты. Эти функции доступны на устройствах Android в качестве фоновых служб, которые содержатся в [ APK Сервисов Google Play](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en).

Приложения Android взаимодействуют с Сервисами Google Play через клиентскую библиотеку Сервисов Google Play. Эта библиотека содержит интерфейсы и классы отдельных служб, таких как Карты. На следующей схеме показана связь между приложением Android и Сервисами Google Play.

![Схема обновления APK Сервисов Google Play в Магазине Google Play](maps-api-images/play-services-diagram.png)

API Карт для Android поставляется в составе Сервисов Google Play.
Прежде чем приложение Xamarin.Android сможет использовать API Карт, необходимо установить пакет SDK Сервисов Google Play с помощью [Диспетчера SDK Android](~/android/get-started/installation/android-sdk.md). На следующем снимке экрана показано, где в Диспетчере SDK Android можно найти клиент пакета SDK Сервисов Google Play.

![Сервисы Google Play в разделе "Extras" (Дополнительные) в Диспетчере SDK Android](maps-api-images/image01.png)

> [!NOTE]
> APK Сервисов Google Play — это лицензированный продукт, который может быть доступен не на всех устройствах. Если он не установлен на устройстве, Карты Google работать не будут.

### <a name="install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget"></a> Установка пакета Xamarin.GooglePlayServices.Maps из NuGet

Пакет [Xamarin.GooglePlayServices.Maps](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps) содержит привязки Xamarin.Android для API Карт из Сервисов Google Play.
Чтобы добавить пакет Карт из Сервисов Google Play, щелкните правой кнопкой мыши папку **Ссылки** проекта в обозревателе решений и выберите **Управление пакетами NuGet**.

![Обозреватель решений с выбранным пунктом контекстного меню "Управление пакетами NuGet" в папке "Ссылки"](maps-api-images/image02.png)

Откроется **диспетчер пакетов NuGet**. Щелкните **Обзор** и в поле поиска введите **Xamarin Google Play Services Maps**. Выберите **Xamarin.GooglePlayServices.Maps** и щелкните **Установить**. (Если этот пакет уже установлен, щелкните **Обновить**.)

[![Диспетчер пакетов NuGet с выбранным пакетом Xamarin.GooglePlayServices.Maps](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Обратите внимание на то, что также будут установлены следующие пакеты зависимостей:

- **Xamarin.GooglePlayServices.Base**
- **Xamarin.GooglePlayServices.Basement**
- **Xamarin.GooglePlayServices.Tasks**

### <a name="specify-the-required-permissions"></a><a name="declare-permissions"></a> Указание необходимых разрешений

Приложения должны указать требования к оборудованию и разрешениям для использования API Карт Google.  Некоторые разрешения автоматически предоставляются пакетом SDK Сервисов Google Play, и разработчику не нужно явно добавлять их в **AndroidManfest.XML**.

- **Доступ к состоянию сети** &ndash; API Карт должен иметь возможность проверить, можно ли скачать фрагменты карты.

- **Доступ к Интернету** &ndash; необходим для скачивания фрагментов карты и взаимодействия с серверами Google Play для доступа к API.

Следующие разрешения и компоненты должны быть указаны в **AndroidManifest.XML** для API Карт Google для Android.

- **OpenGL ES v2** &ndash; приложение должно требовать наличие OpenGL ES версии 2.

- **Ключ API Карт Google** &ndash; используется для подтверждения того, что приложение зарегистрировано и ему разрешено использовать Сервисы Google Play. Дополнительные сведения об этом ключе см. в разделе [Получение ключа API Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

- **Запрос устаревшего клиента HTTP Apache** &ndash; приложения, нацеленные на Android 9.0 (уровень API 28) или более поздней версии, должны указать, что устаревший клиент HTTP Apache является необязательной для использования библиотекой.

- **Доступ к веб-службам Google** &ndash; приложению требуются разрешения на доступ к веб-службам Google, поддерживающим API Карт для Android.

- **Разрешения для уведомлений Сервисов Google Play** &ndash; приложению должно быть предоставлено разрешение на получение удаленных уведомлений от Сервисов Google Play.

- **Доступ к поставщикам сведений о расположении** &ndash; эти разрешения являются необязательными.
   Они позволяют классу `GoogleMap` отображать расположение устройства на карте.

Кроме того, в Android 9 клиентская библиотека Apache HTTP удалена из bootclasspath, поэтому она недоступна для приложений, нацеленных на уровень API 28 или более поздней версии. Чтобы продолжить использование HTTP-клиента Apache в приложениях, нацеленных на уровень API 28 или более поздней версии, необходимо добавить приведенную ниже строку в узел `application` файла **AndroidManifest.XML**.

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> Самые старые версии пакета SDK для Google Play требовали, чтобы приложение запрашивало разрешение `WRITE_EXTERNAL_STORAGE`. Это требование не распространяется на последние привязки Xamarin для Сервисов Google Play.

В следующем фрагменте кода приведен пример параметров, которые необходимо добавить в **AndroidManifest.XML**.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="23" android:targetSdkVersion="28" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- Necessary for apps that target Android 9.0 or higher -->
    <uses-library android:name="org.apache.http.legacy" android:required="false" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
        <!-- Necessary for apps that target Android 9.0 or higher -->
        <uses-library android:name="org.apache.http.legacy" android:required="false" />
    </application>
</manifest>
```

Помимо запроса **AndroidManifest.XML** с разрешениями, приложение также должно выполнять проверку разрешений `ACCESS_COARSE_LOCATION` и `ACCESS_FINE_LOCATION` во время выполнения. Дополнительные сведения о выполнении проверок разрешений во время выполнения см. в руководстве [по управлению разрешениями Xamarin.Android](~/android/app-fundamentals/permissions.md).

### <a name="create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api"></a>Создание эмулятора с помощью API Google

В случае, если физическое устройство Android с Сервисами Google Play отсутствует, можно создать образ эмулятора для разработки. Дополнительные сведения см. в статье [Диспетчер устройств](~/android/get-started/installation/android-emulator/device-manager.md).

## <a name="the-googlemap-class"></a>Класс GoogleMap

После выполнения необходимых условий можно приступить к разработке приложения и использовать API Карт для Android. Класс [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) — это основной API, который приложение Xamarin.Android будет использовать для отображения Карт Google для Android и взаимодействия с ними. Этот класс отвечает за выполнение следующих задач.

- Взаимодействие с Сервисами Google Play для авторизации приложения с помощью веб-службы Google.

- Скачивание, кэширование и отображение фрагментов карты.

- Отображение для пользователя элементов управления пользовательского интерфейса, таких как сдвиг и масштабирование.

- Отображение маркеров и геометрических фигур на картах.

`GoogleMap` добавляется в действие одним из двух способов.

- **MapFragment**. [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) — это специализированный объект Fragment, который используется для размещения объекта `GoogleMap`. Для `MapFragment` требуется уровень API для Android 12 или более поздняя версия.
   Более старые версии Android могут использовать [SupportMapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment).  В этом руководством основное внимание уделяется использованию класса `MapFragment`.

- **MapView**. [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) — это специализированный подкласс View, который может использоваться для размещения объекта `GoogleMap`. Пользователи этого класса должны пересылать все методы жизненного цикла действия в класс `MapView`.

Каждый из этих контейнеров предоставляет свойство `Map`, которое возвращает экземпляр `GoogleMap`. Следует отдавать предпочтение классу [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment), так как это более простой API, уменьшающий объем стандартного кода, который разработчику необходимо реализовать вручную.

### <a name="adding-a-mapfragment-to-an-activity"></a>Добавление MapFragment в действие

На следующем снимке экрана показан пример простого `MapFragment`.

[![Снимок экрана устройства с фрагментом Карт Google](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Как и для других классов Fragment, существуют два способа добавления `MapFragment` в действие.

- **Декларативно**. `MapFragment` можно добавить с помощью XML-файла макета для действия. В приведенном ниже фрагменте XML демонстрируется использование элемента `fragment`.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

- **Программно**. Экземпляр `MapFragment` можно создать программно с помощью метода [`MapFragment.NewInstance`](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()), а затем добавить в действие. В этом фрагменте кода показан самый простой способ создания объекта `MapFragment` и его добавления в действие.

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    Объект `MapFragment` можно настроить, передав объект [`GoogleMapOptions`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) в `NewInstance`. Это описано в разделе [Свойства GoogleMap](#googlemap_object) далее в этом руководстве.

Метод `MapFragment.GetMapAsync` используется для инициализации [`GoogleMap`](#googlemap_object), размещенного во фрагменте, и для получения ссылки на объект Map, размещенный в `MapFragment`. Этот метод принимает объект, реализующий интерфейс `IOnMapReadyCallback`.

Этот интерфейс содержит один метод, `IMapReadyCallback.OnMapReady(MapFragment map)`, который будет вызываться, когда приложение может взаимодействовать с объектом `GoogleMap`. В следующем фрагменте кода показано, как действие Android может инициализировать `MapFragment` и реализовать интерфейс `IOnMapReadyCallback`.

```csharp
public class MapWithMarkersActivity : AppCompatActivity, IOnMapReadyCallback
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.MapLayout);

        var mapFragment = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.map);
        mapFragment.GetMapAsync(this);

        // remainder of code omitted
    }

    public void OnMapReady(GoogleMap map)
    {
        // Do something with the map, i.e. add markers, move to a specific location, etc.
    }
}
```

### <a name="map-types"></a>Типы карт

В API Карт Google доступны пять различных типов карт.

- **Обычная** — это тип карты по умолчанию. На ней показаны дороги и важные топографические особенности, а также некоторые искусственные точки интереса (например, здания и мосты).

- **Спутниковая** — на этой карте показаны фотографии со спутников.

- **Гибридная** — на этой карте показаны фотографии со спутников и карты дорог.

- **Ландшафтная** — на этой карте главным образом показаны топографические особенности и некоторые дороги.

- **Нет** — для этой карты не загружаются какие-либо фрагменты, она отображается как пустая сетка.

На рисунке ниже показаны три различных типа карты: слева направо (обычная, гибридная, ландшафтная).

[![Снимки экрана с примерами карт: обычная, гибридная и ландшафтная](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

Свойство `GoogleMap.MapType` используется для задания или изменения отображаемого типа карты. В следующем фрагменте кода показано, как отобразить спутниковую карту.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```

### <a name="googlemap-properties"></a><a name="googlemap_object"></a>Свойства GoogleMap

`GoogleMap` определяет несколько свойств, которые позволяют управлять функциональностью и внешним видом карты. Одним из способов настройки начального состояния `GoogleMap` является передача объекта [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) при создании `MapFragment`. В следующем фрагменте кода представлен один из примеров использования объекта `GoogleMapOptions` при создании `MapFragment`.

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, mapFragment, "map");
fragTx.Commit();
```

Другим способом настройки `GoogleMap` является управление свойствами [UiSettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) объекта Map. В следующем примере кода показано, как настроить `GoogleMap` для отображения элементов управления масштабом и компаса.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>Взаимодействие с GoogleMap

API Карт для Android предоставляет интерфейсы API, позволяющие действию изменять перспективу, добавлять маркеры, размещать пользовательские наложения или отображать геометрические фигуры. В этом разделе рассказывается, как выполнить некоторые из этих задач в Xamarin.Android.

### <a name="changing-the-viewpoint"></a>Изменение перспективы

Карты моделируются в виде плоской поверхности и отображаются на экране на основе проекции Меркатора. Представление карты — это *камера*, направленная строго сверху на эту поверхность. Положением камеры можно управлять, изменяя расположение, масштаб, наклон и азимут. Класс [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) используется для изменения расположения камеры. Экземпляры объектов `CameraUpdate` не создаются напрямую. Вместо этого API Карт предоставляет класс [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory).

После создания объекта `CameraUpdate` он передается в качестве параметра в метод [GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) или [GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)). Метод `MoveCamera` мгновенно обновляет карту, а метод `AnimateCamera` обеспечивает плавный анимированный переход.

Этот фрагмент кода демонстрирует простой пример того, как использовать `CameraUpdateFactory` для создания `CameraUpdate`, увеличивающего уровень масштаба на единицу.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

API Карт предоставляет класс [CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html), который будет объединять все возможные значения расположения камеры. Экземпляр этого класса может быть предоставлен методу [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29), который возвращает объект `CameraUpdate`. API Карт также содержит класс [CameraPosition.Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html), предоставляющий текучий API для создания объектов `CameraPosition`.
В следующем фрагменте кода показан пример создания `CameraUpdate` из `CameraPosition` и его использования для изменения расположения камеры на `GoogleMap`.

```csharp
public void OnMapReady(GoogleMap map)
{
    LatLng location = new LatLng(50.897778, 3.013333);

    CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
    builder.Target(location);
    builder.Zoom(18);
    builder.Bearing(155);
    builder.Tilt(65);

    CameraPosition cameraPosition = builder.Build();

    CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

    map.MoveCamera(cameraUpdate);
}
```

В предыдущем фрагменте кода определенное расположение на карте представлено классом [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng). Уровень масштаба равен 18, что является произвольным значением масштаба, используемым в Картах Google. Азимут — это показания компаса, измеряемые по часовой стрелке от направления на север. Свойство Tilt управляет углом обзора и задает угол в 25 градусов от вертикали. На следующем снимке экрана показан `GoogleMap` после выполнения предыдущего кода.

[![Пример Карты Google, показывающей указанное расположение под углом](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)

### <a name="drawing-on-the-map"></a>Отображение на карте

API Карт для Android предоставляет API для отображения следующих элементов на карте:

- **маркеры** — это специальные значки, используемые для указания отдельного расположения на карте;

- **наложения** — это изображения, с помощью которых на карте можно указать коллекцию расположений или область;

- **линии, многоугольники и окружности** — это интерфейсы API, позволяющие действиям добавлять фигуры на карту.

#### <a name="markers"></a>Markers

API Карт предоставляет класс [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker), который инкапсулирует все данные об отдельном расположении на карте. По умолчанию класс Marker использует стандартный значок, предоставляемый Картами Google. Можно настроить внешний вид маркера и его реагирование на щелчки пользователя.

##### <a name="adding-a-marker"></a>Добавление маркера

Чтобы добавить маркер на карту, необходимо создать объект [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions), а затем вызвать метод [AddMarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) для экземпляра `GoogleMap`. Этот метод вернет объект [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker).

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    map.AddMarker(markerOpt1);
}
```

Заголовок маркера будет отображаться в *окне сведений*, когда пользователь коснется маркера. На следующем снимке экрана показано, как выглядит этот маркер.

[![Пример Карт Google с маркером и окном сведений для холма Вими-Ридж](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)

##### <a name="customizing-a-marker"></a>Настройка маркера

Можно настроить значок, используемый маркером, вызвав метод `MarkerOptions.InvokeIcon` при добавлении маркера на карту.
Этот метод принимает объект [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor), содержащий данные, необходимые для отображения значка. Класс [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) предоставляет несколько вспомогательных методов, упрощающих создание `BitmapDescriptor`. Ниже перечислены некоторые из этих методов.

- `DefaultMarker(float colour)` &ndash; использование маркера Карт Google по умолчанию, но другого цвета.

- `FromAsset(string assetName)` &ndash; использование пользовательского значка из указанного файла в папке Assets.

- `FromBitmap(Bitmap image)` &ndash; в качестве значка используется указанное растровое изображение.

- `FromFile(string fileName)` &ndash; создание пользовательского значка из файла по указанному пути.

- `FromResource(int resourceId)` &ndash; создание пользовательского значка из указанного ресурса.

В следующем фрагменте кода показан пример создания маркера по умолчанию голубого цвета.

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    var bmDescriptor = BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan);
    markerOpt1.InvokeIcon(bmDescriptor);

    map.AddMarker(markerOpt1);
}
```

#### <a name="info-windows"></a>Окна сведений

*Окна сведений* — это специальные всплывающие окна, которые отображают сведения для пользователя при касании определенного маркера. По умолчанию в окне сведений отображается содержимое заголовка маркера. Если заголовок не назначен, окно сведений не отображается. Одновременно может отображаться только одно окно сведений.

Окно сведений можно настроить, реализовав интерфейс [GoogleMap.IInfoWindowAdapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter). В этом интерфейсе доступны два важных метода.

- `public View GetInfoWindow(Marker marker)` &ndash; при вызове этого метода возвращается настраиваемое окно сведений для маркера. Если возвращается значение `null`, будет использоваться метод отображения окна по умолчанию. Если возвращается объект View, то этот объект будет помещен внутрь рамки окна сведений.

- `public View GetInfoContents(Marker marker)` &ndash; этот метод будет вызван, только если GetInfoWindow вернет `null`. Этот метод может возвращать значение `null`, если должно использоваться применяемое по умолчанию отображение содержимого окна сведений. В противном случае этот метод должен возвращать объект View с содержимым окна сведений.

Окно сведений не является динамическим представлением. Вместо этого Android преобразовывает объект View в статическое растровое изображение и показывает его на изображении. Это означает, что окно сведений не может реагировать на события касания или жесты, а его содержимое не будет автоматически обновляться. Чтобы обновить окно сведений, необходимо вызвать метод [GoogleMap.ShowInfoWindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()).

На следующем рисунке показано несколько примеров настраиваемых окон сведений. Слева показано изображение с настроенным содержимым, а справа — изображение, на котором для окна с содержимым настроены скругленные углы.

![Примеры окон маркеров для Мельбурна, включая значок и население. У окна сведений справа скруглены углы.](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>Объекты GroundOverlay

В отличие от маркеров, указывающих определенное расположение на карте, [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) — это изображение, которое используется для указания коллекции расположений или области на карте.

##### <a name="adding-a-groundoverlay"></a>Добавление GroundOverlay

Добавление наземного наложения на карту аналогично добавлению маркера. Сначала создается объект [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions). Затем этот объект передается в качестве параметра в метод [`GoogleMap.AddGroundOverlay`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)), который возвращает объект `GroundOverlay`. Этот фрагмент кода является примером добавления наземного наложения на карту.

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

На следующем снимке экрана показано. как это наложение выглядит на карте.

[![Пример карты с наложенным изображением полярного медведя](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)

#### <a name="lines-circles-and-polygons"></a>Линии, окружности и многоугольники

Существуют три простых типа геометрических фигур, которые можно добавить на карту. Они перечислены ниже.

- **Ломаная линия** — это ряд соединенных сегментов линии. Она может отмечать путь на карте или образовывать геометрическую фигуру.

- **Окружность** — на карте будет нарисована окружность.

- **Многоугольник** — это замкнутая фигура, позволяющая обвести области на карте.

##### <a name="polylines"></a>Ломаные линии

[Ломаная линия](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline) — это список последовательных объектов `LatLng`, которые определяют вершины каждого сегмента линии. Чтобы создать ломаную линию, сначала нужно создать объект `PolylineOptions` и добавить в него точки. Затем объект `PolylineOption` передается в объект `GoogleMap` путем вызова метода `AddPolyline`.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.

googleMap.AddPolyline(rectOptions);
```

##### <a name="circles"></a>Окружности

Окружности создаются путем создания объекта [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions), который определяет центр и радиус окружности в метрах. Окружность рисуется на карте путем вызова [GoogleMap.AddCircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions)).
В следующем фрагменте кода показано, как нарисовать окружность.

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```

##### <a name="polygons"></a>Многоугольники

Объекты `Polygon` похожи на объекты `Polyline`, за исключением того, что они замкнутые. Объекты `Polygon` — это замкнутые фигуры с заполненной серединой.
Объекты `Polygon` создаются точно так же, как `Polyline`, за исключением того, что вызывается метод [GoogleMap.AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)).

В отличие от объектов `Polyline`, объекты `Polygon` являются замкнутыми. Многоугольник замыкается методом `AddPolygon`, который соединяет линией первую и последнюю точки. В следующем фрагменте кода создается закрашенный прямоугольник в области, использованной в предыдущем фрагменте кода с примером `Polyline`.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```

## <a name="responding-to-user-events"></a>Реагирование на события пользователя

Существуют три типа взаимодействия пользователя с картой:

- **щелчок маркера** — пользователь щелкает маркер;

- **перетаскивание маркера** — пользователь касается маркера и не отпускает его;

- **щелчок окна сведений** — пользователь щелкает окно сведений.

Каждое из этих событий будет рассмотрено более подробно ниже.

### <a name="marker-click-events"></a>События щелчка маркера

Событие `MarkerClicked` возникает при касании маркера пользователем. Это событие принимает объект `GoogleMap.MarkerClickEventArgs` в качестве параметра. Этот класс содержит два свойства.

- `GoogleMap.MarkerClickEventArgs.Handled` &ndash; этому свойству нужно присвоить значение `true`, чтобы указать, что обработчик событий принял событие. Если для этого свойства задано значение `false`, то в дополнение к пользовательскому поведению обработчика событий будет выполнено поведение по умолчанию.

- `Marker` &ndash; это свойство является ссылкой на маркер, вызвавший событие `MarkerClick`.

В этом фрагменте кода показан пример `MarkerClick`, который изменит положение камеры на карте.

```csharp
void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;

    var marker = markerClickEventArgs.Marker;
    if (marker.Id.Equals(gotMauiMarkerId))
    {
        LatLng InMaui = new LatLng(20.72110, -156.44776);

        // Move the camera to look at Maui.
        PositionPolarBearGroundOverlay(InMaui);
        googleMap.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(InMaui, 13));
        gotMauiMarkerId = null;
        polarBearMarker.Remove();
        polarBearMarker = null;
    }
    else
    {
        Toast.MakeText(this, $"You clicked on Marker ID {marker.Id}", ToastLength.Short).Show();
    }
}
```

### <a name="marker-drag-events"></a>События перетаскивания маркера

Это событие возникает, когда пользователь перетаскивает маркер. По умолчанию маркеры невозможно перетаскивать. Маркер можно сделать перетаскиваемым, присвоив свойству `Marker.Draggable` значение `true` или вызвав метод `MarkerOptions.Draggable`, указав `true` в качестве параметра.

Чтобы перетащить маркер, пользователь должен щелкнуть маркер и не отнимать палец от карты. Когда пользователь проведет этим пальцем по экрану, маркер последует за ним. Когда пользователь отнимет палец от экрана, маркер останется на выбранном месте.

Ниже перечислены различные события, которые будут создаваться для перетаскиваемого маркера.

- `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; это событие возникает, когда пользователь начинает перетаскивать маркер.

- `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; это событие возникает во время перетаскивания маркера.

- `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; это событие возникает, когда пользователь завершает перетаскивать маркер.

Каждый `EventArgs` содержит одно свойство `P0`, которое является ссылкой на перетаскиваемый объект `Marker`.

### <a name="info-window-click-events"></a>События щелчка окна сведений

Одновременно может отображаться только одно окно сведений. Когда пользователь щелкает окно сведений на карте, объект Map создает событие `InfoWindowClick`. В следующем фрагменте кода показано, как подключить обработчик к событию.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.InfoWindowClick += MapOnInfoWindowClick;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.Marker;
    // Do something with marker.
}
```

Помните, что окно сведений является статическим объектом `View`, отображаемым на карте в виде картинки. Любые мини-приложения, такие как кнопки, флажки или текстовые представления, помещенные в окно сведений, будут статичны и не будут реагировать на какие-либо связанные события пользователя.

## <a name="related-links"></a>Связанные ссылки

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Сервисы Google Play](https://developers.google.com/android/guides/overview)
- [API Карт Google для Android версии 2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [APK Сервисов Google Play](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Получение ключа API Карт Google](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element)
