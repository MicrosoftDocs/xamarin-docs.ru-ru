---
title: Android 9 Pie
description: Начните разрабатывать приложения для Android 9 Pie, используя Xamarin.Android.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 6f192e9171a9da2e2ef7e0478b5bb6838676faee
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453445"
---
# <a name="android-pie-features"></a>Возможности Android Pie

_Начните разрабатывать приложения для Android 9 Pie, используя Xamarin.Android._

[Android 9 Pie](https://developer.android.com/about/versions/pie/) теперь доступен в Google. В этом выпуске предоставляется ряд новых возможностей и API. Многие из них необходимы, чтобы использовать новые преимущества последних устройств Android.

![Изображение главного имиджевого баннера Android Pie](pie-images/01-android-p-logo.png)

Эта статья содержит сведения, которые помогут вам приступить к разработке приложений Xamarin.Android для Android Pie. В ней объясняется, как установить необходимые обновления, настроить пакет SDK и подготовить эмулятор или устройство для тестирования. Кроме того, здесь приводится описание новых возможностей Android Pie, а также пример исходного кода, демонстрирующий использование некоторых ключевых функций Android Pie.

Xamarin.Android 9.0 обеспечивает поддержку Android Pie. Дополнительные сведения о поддержке Xamarin.Android для Android Pie см. в [заметках о выпуске Xamarin.Android 9.0](/xamarin/android/release-notes/9/9.0/#android-p-dp1).

## <a name="requirements"></a>Требования

Компоненты из следующего списка необходимы для использования возможностей Android Pie в приложениях на основе Xamarin:

- **Visual Studio** — рекомендуется использовать Visual Studio 2019.
    Если вы используете Visual Studio 2017 в Windows, выполните обновление до Visual Studio 2017 версии 15.8 или более поздней. В macOS выполните обновление до Visual Studio 2017 для Mac версии 7.6 или более поздней.

- **Xamarin.Android** — Xamarin.Android 9.0.0.17 или более поздней версии нужно устанавливать вместе с Visual Studio (Xamarin.Android автоматически устанавливается как часть рабочей нагрузки **Разработка мобильных приложений на .NET**).

- **Java Developer Kit** — для разработки Xamarin Android 9.0 требуется [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (также можно попробовать использовать предварительную версию дистрибутива Майкрософт [OpenJDK](~/android/get-started/installation/openjdk.md)). JDK 8 устанавливается автоматически как часть рабочей нагрузки **Разработка мобильных приложений на .NET**.

- **Пакет SDK для Android** — нужно установить пакет SDK для Android (API 28) или более поздней версии с помощью Диспетчера SDK Android.

## <a name="getting-started"></a>Начало работы

Чтобы приступить к разработке приложений для Android Pie с использованием Xamarin.Android, необходимо скачать и установить последние версии средств и пакетов SDK до создания первого проекта для Android Pie:

1. Рекомендуется использовать Visual Studio 2019. Если вы используете Visual Studio 2017, выполните обновление до [Visual Studio 2017 версии 15.8](/visualstudio/releasenotes/vs2017-relnotes) или более поздней. Если вы используете Visual Studio для Mac, выполните обновление до [Visual Studio 2017 для Mac версии 7.6](/visualstudio/releasenotes/vs2017-relnotes) или более поздней.

2. Установите пакеты **Android Pie (API 28)** и средства с помощью Диспетчера SDK Android.

3. Создайте новый проект Xamarin.Android, предназначенный для **Android 9.0**.

4. Настройте эмулятор или устройство для тестирования приложений Android Pie.

В разделах ниже отдельно рассматривается каждый шаг.

### <a name="update-visual-studio"></a>Обновление Visual Studio

Для создания приложений Android Pie с использованием Xamarin рекомендуется использовать Visual Studio 2019.

Если вы используете Visual Studio 2017, обновите средства до версии 15.8 или более поздней (инструкции см. [здесь](/visualstudio/install/update-visual-studio)). В macOS обновите Visual Studio 2017 для Mac до версии 7.6 или более поздней (инструкции см. в [этой статье](/visualstudio/mac/installation)).

### <a name="install-the-android-sdk"></a>Установка пакета SDK для Android

Чтобы создать проект с использованием Xamarin.Android 9.0, необходимо сначала использовать Диспетчер SDK Android, чтобы установить платформу SDK для **Android Pie (API уровня 28)** или более поздней версии.

1. Запустите Диспетчер SDK. В Visual Studio выберите элементы**Сервис > Android > Диспетчер пакетов SDK Android**. В Visual Studio для Mac выберите элементы **Сервис > Диспетчер пакетов SDK**.

2. В правом нижнем углу щелкните значок шестеренки и выберите элементы **Репозиторий > Google (не поддерживается)** :

    [![Настройка репозитория Google](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. Установите пакеты SDK **Android Pie**, которые указаны в списке как **Android SDK Platform 28** на вкладке **Платформы** (дополнительные сведения об использовании Диспетчера SDK Android см. [здесь](~/android/get-started/installation/android-sdk.md)):

    [![Установка пакетов Android Pie](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. Если вы используете эмулятор, создайте виртуальное устройство, которое поддерживает **API уровня 28**. Дополнительные сведения о создании и настройке виртуальных устройств см. в статье [Управление виртуальными устройствами с помощью Диспетчера устройств Android](~/android/get-started/installation/android-emulator/device-manager.md).

### <a name="start-a-xamarinandroid-project"></a>Запуск проекта Xamarin.Android

Создайте проект Xamarin.Android. Если вы не знакомы с разработкой приложений для Android с использованием Xamarin, ознакомьтесь с [этой статьей](~/android/get-started/hello-android/index.md), чтобы узнать о создании проектов Xamarin.Android.

При создании проекта Android необходимо настроить параметры версии для целевой версии Android 9.0 или более поздней. Например, чтобы создать проект для Android Pie, необходимо настроить целевой уровень API Android проекта, задав **Android 9.0** (API 28). Рекомендуем также задать API 28 или выше для уровня требуемой версии .NET Framework. Дополнительные сведения о настройке уровней API Android см. в [этой статье](~/android/app-fundamentals/android-api-levels.md).

### <a name="configure-a-device-or-emulator"></a>Настройка устройства или эмулятора

При использовании физического устройства, например Nexus или Pixel, можно обновить устройство до Android Pie, следуя инструкциям на странице [Заводские образы для устройств Nexus и Pixel](https://developers.google.com/android/images).

Если вы используете эмулятор, создайте виртуальное устройство, которое поддерживает API уровня 28, и выберите образ на базе x86. Дополнительные сведения о создании виртуальных устройств и управлении ими с помощью Диспетчера устройств Android см. в [этой статье](~/android/get-started/installation/android-emulator/device-manager.md).
Сведения об использовании Android Emulator для тестирования и отладки см. [здесь](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="new-features"></a>Новые функции

Android Pie предоставляет ряд новых возможностей. Некоторые из них предназначены для работы с аппаратными возможностями новых устройств Android, а другие улучшают взаимодействие пользователей с Android.

- **Поддержка выреза в дисплее** — предоставляет API для поиска расположения и формы _выреза_ в верхней части экрана новых устройств Android.

- **Улучшенные уведомления** — теперь в них можно использовать изображения, а новый класс `Person` упрощает общение.

- **Позиционирование в помещении** — поддержка протокола времени кругового пути для Wi-Fi (W-iFi Round-Trip-Time) на платформе, позволяющая приложениям использовать устройства Wi-Fi для навигации в помещении.

- **Поддержка нескольких камер** — обеспечивает одновременный доступ к потокам из нескольких физических камер (например, двойных передних или задних камер).

В следующих разделах описаны эти возможности и приведены краткие примеры кода, которые помогут приступить к их использованию в приложении.

### <a name="display-cutout-support"></a>Поддержка вырезки дисплея

У многих новых устройств Android с безрамочными дисплеями в верхней части есть *вырез* (или "челка") с камерой и динамиком.
На следующем снимке экрана показан пример эмулятора с вырезом:

[![Эмулятор Android с имитацией выреза](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

Чтобы управлять отображением содержимого в окнах приложения на устройствах с вырезом в дисплее, в Android Pie добавлен новый атрибут макета окна [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode). Для этого атрибута следует указать одно из таких значений:

- [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) — окно никогда не может перекрываться областью выреза.

- [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) — окно может расширяться в область выреза, но только по коротким краям дисплея. 

- [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) — окно может расширяться в область выреза, если вырез расположен в уровень с системной панелью.

Например, чтобы предотвратить перекрытие окна приложения областью выреза, задайте для режима макета значение *never*: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

В следующих примерах приведены варианты этих режимов выреза. На первом снимке экрана слева показано приложение в режиме, отличном от полноэкранного. На снимке экрана в центре приложение работает в полноэкранном режиме с атрибутом `LayoutInDisplayCutoutMode`, для которого установлено значение `LayoutInDisplayCutoutModeShortEdges`. Обратите внимание, что белый фон приложения распространяется на область дисплея с вырезом:

[![Пример режимов для выреза дисплея в эмуляторе](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

На последнем снимке экрана (расположенном выше справа) для атрибута `LayoutInDisplayCutoutMode` задано значение `LayoutInDisplayCutoutModeShortNever` до перехода в полноэкранный режим.
Обратите внимание, что белый фон приложения не распространяется на область дисплея с вырезом.

Если вам нужны более подробные сведения об области выреза на устройстве, можно использовать новый класс [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html). `DisplayCutout` представляет область экрана, которую нельзя использовать для отображения содержимого. Эти сведения можно использовать для получения расположения и формы выреза, чтобы приложение не пыталось показывать содержимое в этой нефункциональной области.

Дополнительные сведения о новых возможностях для выреза в Android P см. в разделе [Поддержка выреза в дисплее](https://developer.android.com/about/versions/pie/android-9.0#cutout).

### <a name="notifications-enhancements"></a>Улучшенные уведомления

В Android Pie представлены следующие улучшения в работе с сообщениями:

- Каналы уведомления (введенные в [Android Oreo](~/android/platform/oreo.md)) теперь поддерживают блокировку групп каналов.

- Система уведомлений предусматривает три новые категории запрета беспокоить (приоритетные оповещения, системные звуки и источники мультимедиа). Кроме того, есть семь новых режимов запрета беспокоить, которые можно использовать для подавления визуальных прерываний (таких как индикаторы событий, индикаторы уведомлений, отображение строки состояния и запуск полноэкранных действий).

- Для представления отправителя сообщения добавлен новый класс [Person](https://developer.android.com/reference/android/app/Person.html). Использование этого класса помогает оптимизировать визуализацию каждого уведомления благодаря определению людей, участвующих в беседе (в том числе их аватаров и универсальных кодов ресурсов (URI)).

- Теперь в уведомлениях можно использовать изображения. 

В следующем примере показано, как использовать новые API для создания уведомления с изображением. На следующих снимках экрана показано текстовое уведомление, за которым следует уведомление с внедренным изображением. При открытии уведомления (как показано справа) отображается текст первого уведомления и увеличивается изображение, внедренное во второе уведомление.

[![Пример уведомления с изображением](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

В следующем примере показано, как включить изображение в уведомление Android Pie, и демонстрируется использование нового класса `Person`:

1. Создайте объект `Person`, представляющий отправителя. Например, имя и значок отправителя включены в `fromPerson`:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. Создайте `Notification.MessagingStyle.Message`, с изображением для отправки, передав изображение в новый метод [ Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri).
   Пример:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. Добавьте сообщение в объект `Notification.MessagingStyle`. Пример:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. Вставьте этот стиль в построитель уведомлений. Пример:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. Опубликуйте уведомление. Пример:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

Дополнительные сведения о создании уведомлений см. в [этой статье](~/android/app-fundamentals/notifications/local-notifications.md).

### <a name="indoor-positioning"></a>Позиционирование в помещении

Android Pie обеспечивает поддержку стандарта IEEE 802.11mc (также известного как _W-iFi Round-Trip-Time_ или _Wi-Fi RTT_), что позволяет приложениям обнаруживать расстояние до одной или нескольких точек доступа Wi-Fi. Используя эту информацию, приложение предоставляет преимущества *позиционирования в помещении* с точностью от одного до двух метров. На устройствах Android, обеспечивающих аппаратную поддержку IEEE 801.11mc, приложение может предлагать функции навигации, такие как управление на основе местоположения для смарт-устройств или пошаговые инструкции в магазине:

[![Пример навигации в помещении с использованием Wi-Fi RTT](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

Новый класс [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) и несколько вспомогательных классов создают возможность измерять расстояние до устройств Wi-Fi. Дополнительные сведения о появившихся в Android P API позиционирования в помещении см. в справочнике [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary).

### <a name="multi-camera-support"></a>Поддержка нескольких камер

Многие новые устройства Android имеют двойные камеры переднего или заднего вида, которые полезны для таких функций, как стереозрение, расширенные визуальные эффекты и улучшенные возможности увеличения. В Android P появился новый API [для многокамерной съемки](https://developer.android.com/about/versions/pie/android-9.0#camera), позволяющий приложению использовать *логическую камеру* (или *логическую многокамерность*), поддерживающую две физические камеры или больше.
Чтобы определить, поддерживает ли устройство логически несколько камер, можно просмотреть возможности каждой камеры на устройстве и узнать, поддерживает ли она [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA).

Android Pie также предусматривает новый класс [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html), который можно использовать для сокращения задержек при изначальной записи и устранения необходимости запуска потока с камеры.

Дополнительные сведения о поддержке нескольких камер в Android P см. [здесь](https://developer.android.com/about/versions/pie/android-9.0#camera).

### <a name="other-features"></a>Другие возможности

Android Pie поддерживает еще несколько новых возможностей:

- Новый класс [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) можно использовать для рисования и отображения анимированных изображений.

- Новый класс [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) заменяет `BitmapFactory`. `ImageDecoder` можно использовать для декодирования `AnimatedImageDrawable`.

- Поддержка видео HDR и изображений HEIF.

- [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) усовершенствован для более интеллектуальной работы со связанными с сетью заданиями. Новый метод [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) класса [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters) возвращает лучшую сеть для выполнения любых сетевых запросов для этого задания.

Дополнительные сведения о последних возможностях Android Pie см. на странице [Возможности и API Android 9](https://developer.android.com/about/versions/pie/android-9.0).

## <a name="behavior-changes"></a>Изменения в поведении

Если для целевой версии Android задан уровень API 28, следует учитывать несколько платформенных изменений, которые могут повлиять на поведение приложения, даже если вы не реализуете новые возможности, описанные выше. Далее представлена краткая сводка этих изменений:

- Приложения должны теперь запрашивать разрешение перед использованием служб переднего плана.

- Если приложение выполняет больше одного процесса, оно не может совместно использовать один каталог данных [WebView](xref:Android.Webkit.WebView) в процессах.

- Прямой доступ к каталогу данных другого приложения по пути теперь запрещен.

Дополнительные сведения об изменениях в поведении приложений для Android P см. [здесь](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps).

## <a name="sample-code"></a>Пример кода

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) — это пример приложения Xamarin.Android для Android Pie, на котором показано, как устанавливать режимы отображения выреза, как использовать новый класс `Person` и как отправлять уведомления с внедренным изображением.

## <a name="summary"></a>Сводка

Из этой статьи вы узнали об Android Pie, установке и настройке самых новых средств и пакетов для разработки приложений Xamarin.Android для Android Pie. В статье предоставлен обзор основных возможностей, доступных в Android Pie, а также пример исходного кода для некоторых из них.
В ней содержатся ссылки на документацию по API и разделы для разработчиков под Android, которые помогут вам приступить к созданию приложений для Android Pie. В статье также описаны самые важные изменения в поведении Android Pie, которые могут повлиять на существующие приложения.

## <a name="related-links"></a>Связанные ссылки

- [Android 9 Pie](https://developer.android.com/about/versions/pie/)