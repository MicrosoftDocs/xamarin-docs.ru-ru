---
title: Функции KitKat
description: Android 4.4 (KitKat) предлагает множество функций для пользователей и разработчиков. В этом руководстве описываются некоторые из этих функций, а также приведены примеры кода и сведения о реализации, которые помогут максимально эффективно использовать KitKat.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 4616d5eb9dde29faaf1e2fefe5e3e9917fdc078e
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456721"
---
# <a name="kitkat-features"></a>Функции KitKat

_Android 4.4 (KitKat) предлагает множество функций для пользователей и разработчиков. В этом руководстве описываются некоторые из этих функций, а также приведены примеры кода и сведения о реализации, которые помогут максимально эффективно использовать KitKat._

## <a name="overview"></a>Обзор

Операционная система Android 4.4 (уровень API 19), также известная как "KitKat", была выпущена в конце 2013 года. KitKat предлагает ряд новых функций и усовершенствований, в том числе:

- [взаимодействие с пользователем](#user_experience) &ndash; простая платформа анимации с переходами, полупрозрачные панели состояния и навигации, а также полноэкранный иммерсивный режим, позволяющий повысить удобство работы пользователя;

- [содержимое пользователя](#user_content) &ndash; управление файлами пользователей упрощено с помощью платформы доступа к хранилищу; печать изображений, веб-сайтов и другого содержимого упрощена благодаря улучшенным интерфейсам API печати;

- [оборудование](#hardware) &ndash; любое приложение можно превратить в NFC-карту с помощью эмуляции банковской карты на телефоне NFC; доступны датчики с низким энергопотреблением на основе `SensorManager`;

- [средства для разработчиков](#developer_tools) &ndash; использование презентационных приложений с помощью клиента Android Debug Bridge, доступного в пакете SDK для Android.

Это руководство содержит рекомендации по миграции существующего приложения Xamarin.Android в KitKat, а также общий обзор KitKat для разработчиков для Xamarin.Android.

## <a name="requirements"></a>Требования

Для разработки приложений Xamarin.Android, использующих KitKat, требуется установить *Xamarin.Android 4.11.0* или более поздней версии и Android 4.4 (уровень API 19) с помощью Диспетчера SDK Android, как показано на приведенном ниже снимке экрана.

[![Выбор Android 4.4 в Диспетчере SDK Android](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat"></a>

## <a name="migrating-your-app-to-kitkat"></a>Миграция приложения в KitKat

В этом разделе рассматриваются первоочередные рекомендации, которые помогут перенести существующие приложения в Android 4.4.

### <a name="check-system-version"></a>Проверка версии системы

Если приложение должно быть совместимо с предыдущими версиями Android, необходимо создать оболочку с проверкой версии системы для всего кода, относящегося к KitKat, как показано в примере ниже.

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Пакетная обработка сигналов

В Android используются службы сигнализации для пробуждения приложения в фоновом режиме в указанное время. В KitKat возможности данной функции расширены и применяется пакетная обработка сигналов для снижения энергопотребления. Это означает, что вместо пробуждения каждого приложения в точное время KitKat предпочитает сгруппировать несколько приложений, зарегистрированных для пробуждения в течение одного интервала времени, и одновременно вывести их из спящего режима.
Чтобы указать Android пробудить приложение в течение заданного интервала времени, вызовите `SetWindow` для [`AlarmManager`](xref:Android.App.AlarmManager), передав минимальное и максимальное время пробуждения приложения в миллисекундах, а также операцию, выполняемую при пробуждении.
В следующем коде приведен пример приложения, которое должно быть пробуждено в течение 0,5–1 часа после установки интервала.

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Чтобы продолжить пробуждение приложения в точное время, используйте `SetExact`, передав в него точное время, когда приложение должно быть пробуждено, и выполняемую при этом операцию.

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat больше не позволяет задать точный повторяющийся сигнал. Теперь в приложениях, использующих [`SetRepeating`](xref:Android.App.AlarmManager.SetRepeating*)
и требующих применения точных сигналов, необходимо активировать каждый сигнал вручную.

### <a name="external-storage"></a>Внешнее хранилище

Теперь внешнее хранилище делится на два типа: уникальное хранилище для приложения и данные, совместно используемые несколькими приложениями. Для чтения и записи в конкретное расположение приложения во внешнем хранилище не требуются специальные разрешения. Для взаимодействия с данными в общем хранилище теперь требуется разрешение `READ_EXTERNAL_STORAGE` или `WRITE_EXTERNAL_STORAGE`. Эти два типа можно классифицировать следующим образом.

- Если вы получаете путь к файлу или каталогу путем вызова метода для `Context`, например [`GetExternalFilesDir`](xref:Android.Content.Context.GetExternalFilesDir*)
  или [`GetExternalCacheDirs`](xref:Android.Content.Context.GetExternalCacheDirs),
  - то приложению не требуются дополнительные разрешения.

- Если вы получаете путь к файлу или каталогу путем обращения к свойству или вызова метода для `Environment`, например [`GetExternalStorageDirectory`](xref:Android.OS.Environment.ExternalStorageDirectory)
  или [`GetExternalStoragePublicDirectory`](xref:Android.OS.Environment.GetExternalStoragePublicDirectory*),
  то приложению требуется разрешение `READ_EXTERNAL_STORAGE` или `WRITE_EXTERNAL_STORAGE`.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` подразумевает разрешение `READ_EXTERNAL_STORAGE`, поэтому нужно установить только одно разрешение.

### <a name="sms-consolidation"></a>Консолидация SMS

KitKat упрощает обмен сообщениями для пользователя, объединяя все содержимое SMS в одном приложении по умолчанию, выбранном пользователем. Разработчик должен обеспечить возможность выбора своего приложения для обмена сообщениями по умолчанию, а также корректную работу своего кода в реальной ситуации, если приложение не выбрано. Дополнительные сведения о переносе приложения для обмена SMS в KitKat см. в руководстве [по подготовке приложений для обмена SMS к работе в KitKat](https://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) от Google.

### <a name="webview-apps"></a>Приложения WebView

Возможности [WebView](xref:Android.Webkit.WebView) в KitKat кардинально изменились. Самым значительным изменением является добавление защиты загружаемого содержимого в `WebView`. Хотя большинство приложений, нацеленных на предыдущие версии API, должно работать ожидаемым образом, настоятельно рекомендуется протестировать приложения, использующие класс `WebView`. Дополнительные сведения о затронутых API WebView см. в документации Android о [переходе на WebView в Android 4.4](https://developer.android.com/guide/webapps/migrating.html).

<a name="user_experience"></a>

## <a name="user-experience"></a>Взаимодействие с пользователем

В KitKat представлено несколько новых API для повышения удобства работы пользователей, включая новую платформу перехода для обработки анимации свойств и параметр прозрачности пользовательского интерфейса для настройки тем. Эти изменения описаны ниже.

### <a name="transition-framework"></a>Платформа перехода

Платформа перехода упрощает реализацию анимации. KitKat поддерживает простую анимацию свойств с помощью только одной строки кода, а также настройку переходов с помощью *сцен*.

#### <a name="simple-property-animation"></a>Простая анимация свойств

Новая библиотека переходов Android упрощает анимацию свойств кода программной части. Платформа позволяет выполнять простую анимацию с минимальным использованием кода. Например, приведенный ниже пример кода использует [`TransitionManager.BeginDelayedTransition`](xref:Android.Transitions.TransitionManager.BeginDelayedTransition*)
для анимации, показывая и скрывая `TextView`.

```csharp
using Android.Transitions;

public class MainActivity : Activity
{
    LinearLayout linear;
    Button button;
    TextView text;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        linear = FindViewById<LinearLayout> (Resource.Id.linearLayout);
        button = FindViewById<Button> (Resource.Id.button);
        text = FindViewById<TextView> (Resource.Id.textView);

        button.Click += (o, e) => {

            TransitionManager.BeginDelayedTransition (linear);

            if(text.Visibility != ViewStates.Visible)
            {
                text.Visibility = ViewStates.Visible;
            }
            else
            {
                text.Visibility = ViewStates.Invisible;
            }
        };
    }
}
```

В приведенном выше примере используется платформа перехода для создания автоматического перехода по умолчанию между изменяющимися значениями свойств. Так как анимация обрабатывается одной строкой кода, вы можете легко обеспечить совместимость с более старыми версиями Android, заключив вызов `BeginDelayedTransition` в оболочку с проверкой версии системы. Дополнительные сведения см. в разделе [Миграция приложения в KitKat](#Migrating_Your_App_to_KitKat).

На следующем снимке экрана показано приложение до запуска анимации.

[![Снимок экрана приложения перед началом анимации](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

На следующем снимке экрана показано приложение после завершения анимации.

[![Снимок экрана приложения после выполнения анимации](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Вы можете точнее управлять переходом с помощью сцен, которые рассматриваются в следующем разделе.

#### <a name="android-scenes"></a>Сцены Android

[Сцены](xref:Android.Transitions.Scene) появились в составе платформы перехода, чтобы предоставить разработчику больший контроль над анимацией. Сцены создают динамическую область в пользовательском интерфейсе: вы указываете контейнер и несколько версий, или "сцен", для содержимого XML в контейнере, а Android выполняет оставшуюся часть работы, обрабатывая анимацию переходов между этими сценами. Сцены Android позволяют создавать сложные анимации с минимальными усилиями на стороне разработки.

Статический элемент пользовательского интерфейса, в котором размещается динамическое содержимое, называется *контейнером* или *базой сцены*. В приведенном ниже примере для создания `RelativeLayout` с именем `container` используется Android Designer.

[![Создание контейнера RelativeLayout с помощью Android Designer](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

В примере макета также определена кнопка `sceneButton` под `container`. Эта кнопка будет активировать переход.

Для динамического содержимого в контейнере требуются два новых макета Android. Эти макеты задают только код *внутри* контейнера.
Приведенный ниже пример кода определяет макет *Scene1*, который содержит два текстовых поля, названные "Kit" и "Kat" соответственно, и второй макет, *Scene2*, который содержит те же текстовые поля, но названные наоборот. Код XML выглядит следующим образом.

 **Scene1.axml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kit"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textA"
        android:text="Kat"
        android:textSize="35sp" />
</merge>
```

 **Scene2.axml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kat"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textB"
        android:text="Kit"
        android:textSize="35sp" />
</merge>
```

В приведенном выше примере используется `merge`, чтобы сократить код представления и упростить иерархию представлений. Получить дополнительные сведения о макетах `merge` можно [здесь](https://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Сцена создается путем вызова [`Scene.GetSceneForLayout`](xref:Android.Transitions.Scene.GetSceneForLayout*) и передачи объекта контейнера, идентификатора ресурса файла макета сцены и текущего `Context`, как показано в следующем примере кода.

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

При нажатии кнопки две сцены меняются местами посредством анимации Android со значениями перехода по умолчанию.

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

На следующем снимке экрана показана сцена перед анимацией.

[![Снимок экрана приложения перед началом анимации](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

На следующем снимке экрана показана сцена после анимации.

[![Снимок экрана приложения после завершения анимации](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)

> [!NOTE]
> В библиотеке переходов Android имеется [известная ошибка](https://code.google.com/p/android/issues/detail?id=62450), из-за которой сцены, созданные с помощью `GetSceneForLayout`, прерываются, когда пользователь переходит к действию во второй раз. Обходной путь для Java описан [здесь](http://www.doubleencore.com/2013/11/new-transitions-framework/).

##### <a name="custom-transitions-in-scenes"></a>Пользовательские переходы в сценах

Пользовательский переход можно определить в файле ресурсов XML в каталоге `transition` в разделе `Resources`, как показано на снимке экрана ниже.

[![Расположение файла transition.xml в каталоге Resources/transition](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

В следующем примере кода определяется переход, который анимируется в течение 5 секунд и использует [интерполятор отклонений](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html).

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

Этот переход создается в действии с помощью [TransitionInflater](xref:Android.Transitions.TransitionInflater), как показано в следующем коде.

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

Затем новый переход добавляется в вызов `Go`, который начинает анимацию.

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Полупрозрачный пользовательский интерфейс

KitKat обеспечивает более точный контроль над темами приложения с помощью дополнительных полупрозрачных панелей состояния и навигации. Вы можете изменить прозрачность элементов пользовательского интерфейса системы в том же XML-файле, который используется для определения темы Android. В KitKat представлены следующие свойства:

- `windowTranslucentStatus` — если задано значение true, верхняя строка состояния становится полупрозрачной;

- `windowTranslucentNavigation` — если задано значение true, нижняя строка состояния становится полупрозрачной;

- `fitsSystemWindows` — при включении полупрозрачности для верхней или нижней строки состояния смещается содержимое под прозрачными элементами пользовательского интерфейса. Это происходит по умолчанию. Присвоение этому свойству значения `true` — это простой способ предотвратить перекрытие содержимого полупрозрачными элементами пользовательского интерфейса системы.

Следующий код определяет тему с полупрозрачными панелями состояния и навигации.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <style name="KitKatTheme" parent="android:Theme.Holo.Light">
        <item name="android:windowBackground">@color/xamgray</item>
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:fitsSystemWindows">true</item>
        <item name="android:actionBarStyle">@style/ActionBar.Solid.KitKat</item>
    </style>

    <style name="ActionBar.Solid.KitKat" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
        <item name="android:background">@color/xampurple</item>
    </style>
</resources>
```

На снимке экрана ниже показана тема с полупрозрачными панелями состояния и навигации.

[![Пример снимка экрана приложения с полупрозрачными панелями состояния и навигации](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content"></a>

## <a name="user-content"></a>Содержимое пользователя

### <a name="storage-access-framework"></a>Платформа доступа к хранилищу

Платформа доступа к хранилищу (SAF) — это новый способ взаимодействия пользователей с сохраненным содержимым, таким как изображения, видео и документы. Вместо того, чтобы отображать для пользователей диалоговое окно, позволяющее выбрать приложение для обработки содержимого, KitKat открывает новый пользовательский интерфейс, дающий пользователям возможность обращаться к своим данным в одном объединенном расположении. После выбора содержимого пользователь вернется в приложение, которое запросило это содержимое, и работа приложения продолжится в нормальном режиме.

Для этого изменения требуются два действия на стороне разработчика: сначала приложения, требующие содержимое от поставщиков, должны быть обновлены с учетом нового способа запроса содержимого. Затем приложения, которые записывают данные в `ContentProvider`, необходимо изменить для использования новой платформы. Оба сценария зависят от нового [`DocumentsProvider`](xref:Android.Provider.DocumentsProvider)
API.

#### <a name="documentsprovider"></a>DocumentsProvider

В KitKat взаимодействия с `ContentProviders` абстрагированы посредством класса `DocumentsProvider`. Это означает, что SAF не важно, где физически расположены данные, пока они доступны через API `DocumentsProvider`. Локальные поставщики, облачные службы и внешние запоминающие устройства используют один и тот же интерфейс и обрабатываются одинаково, предоставляя пользователю и разработчику единое место для взаимодействия с содержимым пользователя.

В этом разделе описывается загрузка и сохранение содержимого с помощью платформы доступа к хранилищу.

#### <a name="request-content-from-a-provider"></a>Запрос содержимого от поставщика

Мы можем указать KitKat, что нужно выбрать содержимое с помощью пользовательского интерфейса SAF и намерения `ActionOpenDocument`. Это означает, что нужно подключиться ко всем поставщикам содержимого, доступным для устройства. В это намерение можно добавить фильтрацию, указав `CategoryOpenable`. Это означает, что будет возвращено только содержимое, которое можно открыть (т. е. доступное для использования). KitKat также позволяет фильтровать содержимое с помощью `MimeType`. Например, код, приведенный ниже, фильтрует результаты по `MimeType` изображения.

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Вызов `StartActivityForResult` запускает пользовательский интерфейс SAF, в котором пользователь может выбрать изображение.

[![Пример снимка экрана приложения с использованием платформы доступа к хранилищу для выбора изображения](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

После того как пользователь выбрал изображение, `OnActivityResult` возвращает `Android.Net.Uri` выбранного файла. В приведенном ниже примере кода отображается выбранное пользователем изображение.

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == save_request_code) {
        imageView = FindViewById<ImageView> (Resource.Id.imageView);
        imageView.SetImageURI (data.Data);
    }
}
```

#### <a name="write-content-to-a-provider"></a>Запись содержимого в поставщик

Помимо загрузки содержимого из пользовательского интерфейса SAF, KitKat также позволяет сохранять содержимое в любой `ContentProvider`, который реализует API `DocumentProvider`. При сохранении содержимого используется `Intent` с `ActionCreateDocument`.

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

Приведенный выше пример кода загружает пользовательский интерфейс SAF, позволяя пользователю изменить имя файла и выбрать каталог для размещения нового файла.

[![Снимок экрана: пользователь изменяет имя файла на NewDoc в каталоге Downloads](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

Когда пользователь нажимает кнопку **Сохранить**, `OnActivityResult` передает `Android.Net.Uri` созданного файла, доступ к которому можно получить с помощью `data.Data`. Универсальный код ресурса (URI) можно использовать для потоковой передачи данных в новый файл.

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == write_request_code) {
        using (Stream stream = ContentResolver.OpenOutputStream(data.Data)) {
            Encoding u8 = Encoding.UTF8;
            string content = "Hello, world!";
            stream.Write (u8.GetBytes(content), 0, content.Length);
        }
    }
}
```

Обратите внимание на то, что [`ContentResolver.OpenOutputStream(Android.Net.Uri)`](xref:Android.Content.ContentResolver.OpenOutputStream*)
возвращает `System.IO.Stream`, поэтому весь процесс потоковой передачи может быть написан на .NET.

Дополнительные сведения о загрузке, создании и редактировании содержимого с помощью платформы доступа к хранилищу см. в [документации Android по платформе доступа к хранилищу](https://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Печать

KitKat упрощает печать содержимого благодаря введению [служб печати](xref:Android.PrintServices) и `PrintManager`. KitKat также является первой версией API, полностью использующей [интерфейсы API Google Cloud Print Service](https://developers.google.com/cloud-print/) с помощью [приложения Google Cloud Print](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint).
Большинство устройств, поставляемых с KitKat, автоматически скачивает приложение Google Cloud Print и [подключаемый модуль HP Print Service](https://play.google.com/store/apps/details?id=com.hp.android.printservice) при первом подключении к сети Wi-Fi. Пользователь может проверить параметры печати своего устройства, выбрав **Параметры > Система > Печать**.

[![Пример снимка экрана параметров печати](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)

> [!NOTE]
> Хотя интерфейсы API печати настроены для работы с Google Cloud Print по умолчанию, Android позволяет разработчикам подготавливать содержимое для печати с помощью новых интерфейсов API и передавать их в другие приложения для управления печатью.

#### <a name="printing-html-content"></a>Печать содержимого HTML

KitKat автоматически создает [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter) для веб-представления с помощью `WebView.CreatePrintDocumentAdapter`. Печать веб-содержимого — это скоординированное взаимодействие [`WebViewClient`](xref:Android.Webkit.WebViewClient), который ожидает загрузки содержимого HTML и сообщает о нем действию, чтобы параметр печати стал доступен в меню "Параметры", и действия, которое ожидает от пользователя выбора параметра "Печать" и вызывает `Print`для `PrintManager`. В этом разделе описывается базовая настройка, необходимая для печати экранного содержимого HTML.

Обратите внимание на то, что для загрузки и печати веб-содержимого требуется разрешение на использование Интернета.

[![Настройка разрешения на использование Интернета в параметрах приложения](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>Элемент меню "Печать"

Параметр печати, как правило, отображается в [меню параметров](https://developer.android.com/guide/topics/ui/menus.html#options-menu) действия.
Меню параметров позволяет пользователям выполнять операции с действием. Оно находится в правом верхнем углу экрана и выглядит следующим образом.

[![Пример снимка экрана пункта меню "Печать" в правом верхнем углу экрана](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)

Дополнительный пункты меню можно определить в каталоге *menu* в папке *Resources*. Приведенный ниже код определяет пример пункта меню [Print](xref:Android.Print.PrintManager).

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Взаимодействие с меню параметров в действии осуществляется с помощью методов `OnCreateOptionsMenu` и `OnOptionsItemSelected`.
`OnCreateOptionsMenu` служит для добавления новых пунктов меню, таких как "Печать", из каталога Resources *menu*.
`OnOptionsItemSelected` ожидает, пока пользователь не выберет пункт "Печать" в меню, после чего начинает печать.

```csharp
bool dataLoaded;

public override bool OnCreateOptionsMenu (IMenu menu)
{
    base.OnCreateOptionsMenu (menu);
    if (dataLoaded) {
        MenuInflater.Inflate (Resource.Menu.print, menu);
    }
    return true;
}

public override bool OnOptionsItemSelected (IMenuItem item)
{
    if (item.ItemId == Resource.Id.menu_print) {
        PrintPage ();
        return true;
    }
    return base.OnOptionsItemSelected (item);
}
```

В приведенном выше коде также определена переменная `dataLoaded`, используемая для отслеживания состояния содержимого HTML. `WebViewClient` задаст для этой переменной значение true после загрузки всего содержимого, чтобы действие могло добавить пункт меню "Печать" в меню параметров.

##### <a name="webviewclient"></a>WebViewClient

Задача `WebViewClient` — убедиться, что данные в `WebView` полностью загружены, прежде чем в меню появится параметр печати. Для этого используется метод `OnPageFinished`. `OnPageFinished` ожидает завершения загрузки веб-содержимого и указывает действию повторно создать меню параметров с использованием `InvalidateOptionsMenu`.

```csharp
class MyWebViewClient : WebViewClient
{
    PrintHtmlActivity caller;

    public MyWebViewClient (PrintHtmlActivity caller)
    {
        this.caller = caller;
    }

    public override void OnPageFinished (WebView view, string url)
    {
        caller.dataLoaded = true;
        caller.InvalidateOptionsMenu ();
    }
}
```

`OnPageFinished` также задает для `dataLoaded` значение `true`, поэтому `OnCreateOptionsMenu` может повторно создать меню с параметром "Печать".

##### <a name="printmanager"></a>PrintManager

Приведенный ниже пример кода печатает содержимое `WebView`.

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` принимает в качестве аргументов имя задания печати (в этом примере — MyWebPage), [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter),
который создает документ печати из содержимого, и [`PrintAttributes`](xref:Android.Print.PrintAttributes)
(`null` в примере выше). Можно указать `PrintAttributes`, чтобы разместить содержимое на печатной странице, хотя в большинстве случаев должны подойти атрибуты по умолчанию.

При вызове `Print` загружается пользовательский интерфейс печати, в котором перечислены параметры задания печати. Пользовательский интерфейс предоставляет пользователям возможность печати или сохранения содержимого HTML в формате PDF, как показано на снимках экрана ниже.

[![Снимок экрана PrintHtmlActivity с меню "Печать"](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![Снимок экрана PrintHtmlActivity с меню "Сохранить как PDF"](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware"></a>

## <a name="hardware"></a>Оборудование

KitKat добавляет несколько API для новых функций устройства. Наиболее важными из них являются эмуляция банковской карты на телефоне и новый `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>Эмуляция банковской карты на телефоне в NFC

Эмуляция банковской карты на телефоне (HCE) позволяет приложениям выполнять функции NFC-карт или устройств чтения NFC-карт, не используя собственный защищенный элемент оператора связи. Перед настройкой HCE убедитесь, что эта функция доступна на устройстве с помощью `PackageManager.HasSystemFeature`.

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

Для работы HCE требуется, чтобы функция HCE и разрешение `Nfc` были зарегистрированы в файле `AndroidManifest.xml` приложения.

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![Настройка разрешения NFC в параметрах приложения](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

Для работы HCE нужна возможность выполнения в фоновом режиме. Эта служба должна запускаться, когда пользователь выполняет NFC-транзакцию, даже если приложение, использующее HCE, не запущено. Это можно сделать, написав код HCE как объект `Service`. Служба HCE реализует интерфейс `HostApduService`, который реализует следующие методы.

- *ProcessCommandApdu* — протокольный блок данных прикладного уровня (APDU), который передается между устройством чтения NFC и службой HCE. Этот метод принимает ADPU из устройства чтения и возвращает блок данных.

- *OnDeactivated* — `HostAdpuService` деактивируется, когда служба HCE прекращает взаимодействие с устройством чтения NFC.

Служба HCE также должна быть зарегистрирована в манифесте приложения и декорирована соответствующими разрешениями, фильтром намерений и метаданными. Следующий код представляет собой пример `HostApduService`, зарегистрированного в манифесте Android с помощью атрибута `Service`. Дополнительные сведения об атрибутах см. в руководстве по Xamarin [Манифест приложения](~/android/platform/android-manifest.md).

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("android.nfc.cardemulation.host.apdu_service",
    Resource="@xml/hceservice")]

class HceService : HostApduService
{
    public override byte[] ProcessCommandApdu(byte[] apdu, Bundle extras)
    {
        ...
    }

    public override void OnDeactivated (DeactivationReason reason)
    {
        ...
    }
}
```

Приведенная выше служба предоставляет устройству чтения NFC способ взаимодействия с приложением, но устройство чтения NFC не может узнать, эмулирует ли эта служба NFC-карту, которую необходимо считать. Чтобы помочь устройству чтения NFC идентифицировать службу, можно назначить службе уникальный *идентификатор приложения (AID)* . Мы указываем AID, а также другие метаданные о службе HCE, в XML-файле ресурсов, зарегистрированном с помощью атрибута `MetaData` (см. пример кода выше). В этом файле ресурсов указывается один или несколько фильтров AID. Это строки уникальных идентификаторов в шестнадцатеричном формате, которые соответствуют идентификаторам AID одного или нескольких устройств чтения NFC.

```xml
<host-apdu-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/hce_service_description"
    android:requireDeviceUnlock="false"
    android:apduServiceBanner="@drawable/service_banner">
    <aid-group android:description="@string/aid_group_description"
                android:category="payment">
        <aid-filter android:name="1111111111111111"/>
        <aid-filter android:name="0123456789012345"/>
    </aid-group>
</host-apdu-service>
```

В дополнение к фильтрам AID XML-файл ресурсов содержит описание службы HCE для пользователя, в котором указывается группа AID (платежное приложение или другое), а в случае платежного приложения — баннер размером 260x96 точек, отображаемый для пользователя.

Конфигурация, описанная выше, содержит основные стандартные блоки для приложения, имитирующего NFC-карту. Для настройки NFC требуется выполнить еще нескольких действий, а затем провести тестирование. Дополнительные сведения об эмуляции банковской карты на телефоне см. на [портале документации по Android](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
Чтобы получить дополнительные сведения об использовании NFC в Xamarin, ознакомьтесь с [этими примерами](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Датчики

KitKat обеспечивает доступ к датчикам устройства посредством [`SensorManager`](xref:Android.Hardware.SensorManager).
`SensorManager` позволяет операционной системе запланировать доставку данных датчиков в приложение в пакетах, увеличивая время работы аккумулятора.

KitKat также содержит два новых типа датчика для отслеживания действий пользователя. Они основаны на акселерометре и приведены ниже.

- *StepDetector* — приложение получает уведомление и пробуждается, когда пользователь делает шаг, а детектор предоставляет значение времени выполнения шага.

- *StepCounter* отслеживает количество шагов, сделанных пользователем с момента регистрации датчика *до следующей перезагрузки устройства*.

На следующем снимке экрана показан шагомер в действии.

[![Снимок экрана приложения SensorsActivity, отображающего шагомер](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

Можно создать `SensorManager`, вызвав `GetSystemService(SensorService)` и приведя результат как `SensorManager`. Чтобы использовать шагомер, вызовите `GetDefaultSensor` для `SensorManager`. Вы можете зарегистрировать датчик и ожидать передачи изменений количества шагов с помощью интерфейса [`ISensorEventListener`](xref:Android.Hardware.ISensorEventListener),
как показано в примере кода ниже.

```csharp
public class MainActivity : Activity, ISensorEventListener
{
    float count = 0;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        SensorManager senMgr = (SensorManager) GetSystemService (SensorService);
        Sensor counter = senMgr.GetDefaultSensor (SensorType.StepCounter);
        if (counter != null) {
            senMgr.RegisterListener(this, counter, SensorDelay.Normal);
        }
    }

    public void OnAccuracyChanged (Sensor sensor, SensorStatus accuracy)
    {
        Log.Info ("SensorManager", "Sensor accuracy changed");
    }

    public void OnSensorChanged (SensorEvent e)
    {
        count = e.Values [0];
    }
}
```

`OnSensorChanged` вызывается, если изменяется количество шагов, пока приложение выполняется на переднем плане. Если приложение переключается в фоновый режим или устройство находится в спящем режиме, `OnSensorChanged` не будет вызываться. Тем не менее подсчет шагов продолжится до вызова `UnregisterListener`.

Помните, что *значение числа шагов является накопительным во всех приложениях, в которых зарегистрирован датчик*. Это означает, что даже если вы удалите и переустановите приложение, а затем инициализируете переменную `count` нулем (0) при его запуске, значением, которое передает датчик, останется общее число шагов, выполненных, пока датчик был зарегистрирован в вашем или другом приложении. Вы можете запретить приложению увеличивать значение шагомера, вызвав `UnregisterListener` для `SensorManager`, как показано в следующем коде.

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

При перезагрузке устройства число шагов сбрасывается до 0. Приложению потребуется дополнительный код. чтобы обеспечить точность количества шагов, сообщаемого приложением, независимо от других приложений, использующих датчик, или состояния устройства.

> [!NOTE]
> Хотя API для обнаружения и подсчета шагов входит в состав KitKat, не все телефоны оснащены датчиком. Вы можете проверить наличие датчика, выполнив `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, или убедиться, что `GetDefaultSensor` не возвращает значение `null`.

<a name="developer_tools"></a>

## <a name="developer-tools"></a>Средства разработчика

### <a name="screen-recording"></a>Запись экрана

KitKat включает в себя новые возможности записи экрана, чтобы разработчики могли записывать приложения в действии. Запись экрана доступна с помощью клиента [Android Debug Bridge (ADB)](https://developer.android.com/tools/help/adb.html), который можно скачать в составе пакета SDK для Android.

Чтобы записать экран, подключите свое устройство, затем найдите установленный пакет SDK для Android, перейдите в каталог **platform-tools** и запустите клиент **adb**.

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

Приведенная выше команда запишет 3-минутное видео со скоростью по умолчанию, равной 4 Мбит/с. Чтоб изменить продолжительность, добавьте флаг *--time-limit*.
Чтобы изменить скорость, добавьте флаг *--bit-rate*. Приведенная ниже команда запишет минутное видео со скоростью 8 Мбит/с.

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Видео можно найти на устройстве. Оно появится в коллекции после завершения записи.

## <a name="other-kitkat-additions"></a>Другие дополнения в KitKat

Помимо описанных выше изменений KitKat обеспечивает следующие возможности.

- *Полноэкранный режим*. В KitKat введен новый [иммерсивный режим](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) для просмотра содержимого, запуска игр и других приложений, которые могут использовать преимущества полноэкранного режима.

- *Настройка уведомлений*. Можно получать дополнительные сведения о системных уведомлениях с помощью [`NotificationListenerService`](xref:Android.Service.Notification.NotificationListenerService)
  . Это позволяет по-разному представлять информацию в приложении.

- *Отражение прорисовываемых ресурсов*. Для прорисовываемых ресурсов добавлен новый атрибут [`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored),
  указывающий системе создать зеркальную версию изображений, требующих переворота для макетов, организованных слева направо.

- *Приостановка анимации*. Приостановка и возобновление анимации, созданной с помощью класса [`Animator`](xref:Android.Animation.Animator)
  .

- *Чтение динамически изменяемого текста*. Можно пометить части пользовательского интерфейса, которые динамически обновляются для отображения нового текста, как "динамические области" с помощью нового атрибута [`accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion),
  чтобы новый текст автоматически озвучивался в режиме специальных возможностей.

- *Улучшение работы с аудио*. Можно увеличить громкость дорожек с помощью [`LoudnessEnhancer`](xref:Android.Media.Audiofx.LoudnessEnhancer),
  определить пиковый уровень и RMS аудиопотока с помощью класса [`Visualizer`](xref:Android.Media.Audiofx.Visualizer.MeasurementModePeakRms)
  и получить сведения из [метки времени звука](xref:Android.Media.AudioTimestamp), чтобы упростить синхронизацию звука и видео.

- *Синхронизация ContentResolver с настраиваемым интервалом*. KitKat дает возможность настроить время выполнения запроса на синхронизацию. Можно синхронизировать `ContentResolver`, используя настраиваемое время или интервал, вызвав `ContentResolver.RequestSync` и передав `SyncRequest`.

- *Различение контроллеров*. KitKat контроллерам назначаются уникальные целочисленные идентификаторы, доступ к которым можно получить с помощью свойства `ControllerNumber` устройства. Это помогает различать игроков в игре.

- *Дистанционное управление*. Если внести несколько изменений на стороне оборудования и программного обеспечения, KitKat позволяет превратить устройство с инфракрасным передатчиком в пульт дистанционного управления с помощью `ConsumerIrService` и взаимодействовать с периферийными устройствами с помощью новых API [`RemoteController`](xref:Android.Media.RemoteController)
  Необходимо загрузить и установить пакет NuGet LINQ to Twitter для работы с этим учебником.

Дополнительные сведения об изменениях, описанных выше, см. в обзоре [интерфейсов API Android 4.4](https://developer.android.com/about/versions/android-4.4.html) от Google.

## <a name="summary"></a>Сводка

В этой статье представлены некоторые новые API, доступные в Android 4.4 (уровень API 19), и рекомендации по переносу приложения в KitKat. В ней рассмотрены изменения в интерфейсах API, влияющие на взаимодействие с пользователем, включая *платформу перехода* и новые параметры *управления темами*. Далее в ней рассматривается *платформа доступа к хранилищу* и класс `DocumentsProvider`, а также новые *API печати*. В ней описывается *эмуляция NFC-карты на телефоне* и работа с *маломощными датчиками*, включая два новых датчика для отслеживания действий пользователя. Наконец, в статье демонстрируется запись роликов приложений в реальном времени с помощью функции *записи экрана*, а также приводится подробный список изменений и дополнений в API KitKat.

## <a name="related-links"></a>Связанные ссылки

- [Пример KitKat](/samples/xamarin/monodroid-samples/kitkat)
- [Интерфейсы API Android 4.4](https://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](https://developer.android.com/about/versions/kitkat.html)