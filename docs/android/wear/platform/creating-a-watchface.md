---
title: Создание лица для просмотра неизноса Android 1,0
description: В этом руководство объясняется, как реализовать пользовательскую службу отслеживания лиц для Android — износа 1,0. Пошаговые инструкции предназначены для создания отключенной службы Digital Watch, за которой следует дополнительный код для создания часового следа в аналоговом стиле.
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/23/2018
ms.openlocfilehash: 81dc3d23ea606a525fcc1bbffafa7d3bfdf6b332
ms.sourcegitcommit: e27e29c14b783263e063baaa65d4eecb8dd31f57
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98628869"
---
# <a name="creating-a-watch-face"></a>Создание оформлений экрана

_В этом руководство объясняется, как реализовать пользовательскую службу отслеживания лиц для Android — износа 1,0. Пошаговые инструкции предназначены для создания отключенной службы Digital Watch, за которой следует дополнительный код для создания часового следа в аналоговом стиле._

## <a name="overview"></a>Общие сведения

В этом пошаговом руководстве создается базовая служба отслеживания лиц, которая иллюстрирует основные принципы создания пользовательского циферблата по 1,0 износу Android (в часах).
В службе первоначального просмотра отображаются простые цифровые часы, в которых отображается текущее время в часах и минутах:

[![На снимке экрана показан первоначальный цифровой видеоролик.](creating-a-watchface-images/01-initial-face.png "Пример снимка экрана первоначального цифрового часового лица")](creating-a-watchface-images/01-initial-face.png#lightbox)

После разработки и тестирования этого цифрового циферблата будет добавлен дополнительный код для его обновления до более замысловатого аналогового лица с тремя руки:

[![На снимке экрана показан окончательный экран аналогового циферблата.](creating-a-watchface-images/02-example-watchface.png "Пример снимка экрана окончательного аналогового циферблата")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Службы просмотра лиц упаковываются и устанавливаются как часть приложения «износа 1,0». В следующих примерах `MainActivity` содержится не только код из шаблона приложения "износ 1,0", поэтому служба отслеживания лиц может быть упакована и развернута в разумном контрольном лице как часть приложения. По сути, это приложение будет использоваться исключительно как транспортное средство для получения сведений об отучении и тестировании службы "наблюдатель лиц", загруженной в устройство износа (или эмулятора) с недействительной 1,0.

## <a name="requirements"></a>Требования

Чтобы реализовать службу отслеживания лиц, необходимо следующее:

- Android 5,0 (уровень API 21) или более поздней версии на устройстве или эмуляторе износа.

- [Библиотеки поддержки Xamarin Android](https://www.nuget.org/packages/Xamarin.Android.Wear) , которые необходимо добавить в проект Xamarin. Android.

Хотя Android 5,0 является минимальным уровнем API для реализации службы отслеживания лиц, рекомендуется использовать Android 5,1 или более позднюю версию. Устройства Android с Android 5,1 (API 22) и более поздних версий позволяют приложениям-выпускам управлять отображением на экране, пока устройство находится *в режиме низкого* энергопотребления. Когда устройство выходит из *окружающего* режима низкого энергопотребления, оно находится в *интерактивном* режиме. Дополнительные сведения об этих режимах см. [в разделе обеспечение видимости приложения](https://developer.android.com/training/wearables/apps/always-on.html).

## <a name="start-an-app-project"></a>Запуск проекта приложения

Создание проекта Android износа 1,0 с именем **ватчфаце** (Дополнительные сведения о создании проектов Xamarin. Android см. в статье [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)):

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Диалоговое окно создания проекта](creating-a-watchface-images/03-wear-project-vs-sml.png "Выбор приложения "износ" в диалоговом окне "Создание проекта"")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Диалоговое окно создания проекта](creating-a-watchface-images/03-wear-project-xs-sml.png "Выбор приложения "износ" в диалоговом окне "Создание проекта"")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----

Задайте имя пакета `com.xamarin.watchface` :

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Параметр имени пакета](creating-a-watchface-images/04-package-name-vs.png "Задайте для имени пакета значение com. Xamarin. ватчфаце")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Параметр имени пакета](creating-a-watchface-images/04-package-name-xs.png "Задайте для имени пакета значение com. Xamarin. ватчфаце")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Кроме того, прокрутите вниз и включите разрешения " **Интернет** " и " **WAKE_LOCK** ":

[![Необходимые разрешения](creating-a-watchface-images/05-required-permissions-vs.png "Включить разрешения для Интернета и WAKE_LOCK")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Установите для минимальной версии Android значение **android 5,1 (API уровня 22)**.
Кроме того, включите разрешения **Интернета** и **вакелокк** :

[![Необходимые разрешения](creating-a-watchface-images/05-required-permissions-xs.png "Включить разрешения для Интернета и Вакелокк")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

Затем скачайте [preview.png](creating-a-watchface-images/preview.png) &ndash; он будет добавлен в папку **драваблес** далее в этом пошаговом руководстве.

## <a name="add-the-xamarinandroid-wear-package"></a>Добавление пакета износа Xamarin. Android

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Запустите диспетчер пакетов NuGet (в Visual Studio щелкните правой кнопкой мыши **ссылки** в **Обозреватель решений** и выберите **Управление пакетами NuGet...**). Обновите проект до последней стабильной версии **Xamarin. Android. износа**:

[![Добавление диспетчера пакетов NuGet](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Добавление пакета Xamarin. Android. износа")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

Затем, если **Xamarin. Android. support. V13** установлен, удалите его:

[![Удаление диспетчера пакетов NuGet](creating-a-watchface-images/07-uninstall-v13-sml.png "Удалите Xamarin. support. V13")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Запустите диспетчер пакетов NuGet (в Visual Studio для Mac щелкните правой кнопкой мыши **пакеты** в **области решение** и выберите команду **Добавить пакеты...**). Обновите проект до последней стабильной версии **Xamarin. Android. износа**:

[![Добавление диспетчера пакетов NuGet](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Добавление пакета Xamarin. Android. износа")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----

Создание и запуск приложения на устройстве или в эмуляторе (Дополнительные сведения о том, как это сделать, см. в разделе Руководство по [Начало работы](~/android/wear/get-started/index.md) ). На устройстве "износ" должно отобразиться следующее окно приложения:

[![Снимок экрана приложения](creating-a-watchface-images/08-app-screen.png "Экран приложения на устройстве износа")](creating-a-watchface-images/08-app-screen.png#lightbox)

На этом этапе базовое приложение "простой" не имеет функциональных возможностей наблюдения, так как оно еще не предоставляет реализацию службы просмотра лиц. Эта служба будет добавлена далее.

## <a name="canvaswatchfaceservice"></a>канвасватчфацесервице

Износ Android реализует Просмотр наблюдателей через `CanvasWatchFaceService` класс. `CanvasWatchFaceService` является производным от `WatchFaceService` , который является производным от, `WallpaperService` как показано на следующей схеме:

[![Схема наследования](creating-a-watchface-images/09-inheritance-diagram-sml.png "Схема наследования Канвасватчфацесервице")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` включает вложенный `CanvasWatchFaceService.Engine` объект; он создает экземпляр `CanvasWatchFaceService.Engine` объекта, который выполняет фактическую работу по рисованию циферблата. `CanvasWatchFaceService.Engine` является производным от `WallpaperService.Engine` , как показано на схеме выше.

На этой диаграмме не показана схема `Canvas` , `CanvasWatchFaceService` используемая для рисования грани &ndash; просмотра `Canvas` . это передается через `OnDraw` метод, как описано ниже.

В следующих разделах будет создана пользовательская служба отслеживания лиц, выполнив следующие действия.

1. Определите класс с именем `MyWatchFaceService` , производным от  `CanvasWatchFaceService` .

2. В среде `MyWatchFaceService` Создайте вложенный класс с именем  `MyWatchFaceEngine` , производным от  `CanvasWatchFaceService.Engine` .

3. В `MyWatchFaceService` реализуйте `CreateEngine` метод, который создает экземпляр `MyWatchFaceEngine` и возвращает его.

4. В `MyWatchFaceEngine` реализуйте `OnCreate` метод, чтобы создать стиль для просмотра граней и выполнить другие задачи инициализации.

5. Реализуйте `OnDraw` метод `MyWatchFaceEngine` . Этот метод вызывается каждый раз, когда необходимо перерисовать грань (т. е. *аннулирование*). `OnDraw` метод, рисующий (и перерисующий) элементы лицевой стороны, например час, минуту и секунды.

6. Реализуйте `OnTimeTick` метод `MyWatchFaceEngine` .
    `OnTimeTick` метод вызывается по крайней мере один раз в минуту (в режиме окружающей среды и интерактивных режимов) или при изменении даты и времени.

Дополнительные сведения о `CanvasWatchFaceService` см. в документации по API [Канвасватчфацесервице](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) для Android.
Аналогичным образом [канвасватчфацесервице. Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) описывает фактическую реализацию циферблата.

### <a name="add-the-canvaswatchfaceservice"></a>Добавление Канвасватчфацесервице

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Добавьте новый файл с именем **MyWatchFaceService.CS** (в Visual Studio щелкните правой кнопкой мыши **ватчфаце** в **Обозреватель решений**, а затем выберите пункт **Добавить > новый элемент...** и выберите **класс**).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Добавьте новый файл с именем **MyWatchFaceService.CS** (в Visual Studio для Mac щелкните правой кнопкой мыши проект **ватчфаце** , выберите команду **Добавить > создать файл...** и выберите **пустой класс**).

-----

Замените содержимое этого файла следующим кодом:

```csharp
using System;
using Android.Views;
using Android.Support.Wearable.Watchface;
using Android.Service.Wallpaper;
using Android.Graphics;

namespace WatchFace
{
    class MyWatchFaceService : CanvasWatchFaceService
    {
        public override WallpaperService.Engine OnCreateEngine()
        {
            return new MyWatchFaceEngine(this);
        }

        public class MyWatchFaceEngine : CanvasWatchFaceService.Engine
        {
            CanvasWatchFaceService owner;
            public MyWatchFaceEngine (CanvasWatchFaceService owner) : base(owner)
            {
                this.owner = owner;
            }
        }
    }
}
```

`MyWatchFaceService` (производный от `CanvasWatchFaceService` ) — Основная программа для просмотра циферблата. `MyWatchFaceService` реализует только один метод, `OnCreateEngine` который создает экземпляр и возвращает `MyWatchFaceEngine` объект ( `MyWatchFaceEngine` производный от `CanvasWatchFaceService.Engine` ). Экземпляр `MyWatchFaceEngine` объекта должен возвращаться как `WallpaperService.Engine` . Инкапсулированный `MyWatchFaceService` объект передается в конструктор.

`MyWatchFaceEngine` — Это фактическая реализация контрольного лица &ndash; , содержащая код, который рисует грань просмотра. Он также обрабатывает системные события, такие как изменения экрана (режимы окружения и интерактивности, выключение экрана и т. д.).

### <a name="implement-the-engine-oncreate-method"></a>Реализация метода OnCreate обработчика

`OnCreate`Метод инициализирует поверхность просмотра. Добавьте следующее поле в `MyWatchFaceEngine` :

```csharp
Paint hoursPaint;
```

Этот `Paint` объект будет использоваться для отображения текущего времени на наблюдателе. Затем добавьте следующий метод в `MyWatchFaceEngine` :

```csharp
public override void OnCreate(ISurfaceHolder holder)
{
    base.OnCreate (holder);

    SetWatchFaceStyle (new WatchFaceStyle.Builder(owner)
        .SetCardPeekMode (WatchFaceStyle.PeekModeShort)
        .SetBackgroundVisibility (WatchFaceStyle.BackgroundVisibilityInterruptive)
        .SetShowSystemUiTime (false)
        .Build ());

    hoursPaint = new Paint();
    hoursPaint.Color = Color.White;
    hoursPaint.TextSize = 48f;
}
```

`OnCreate` вызывается вскоре после `MyWatchFaceEngine` запуска. Он настраивает элемент `WatchFaceStyle` управления (который управляет взаимодействием устройства по износу) и создает экземпляр `Paint` объекта, который будет использоваться для отображения времени.

Вызов метода `SetWatchFaceStyle` делает следующее:

1. Задает *режим просмотра* , в результате чего `PeekModeShort` уведомления отображаются в виде мелких карточек просмотра.

2. Задает видимость фона `Interruptive` , что приводит к тому, что фон отображаемой карточки будет отображаться только в том случае, если он представляет собой прерывание уведомления.

3. Отключает прорисовку времени пользовательского интерфейса системы по умолчанию на экране просмотра, чтобы пользовательское лицо наблюдения могла отображать время.

Дополнительные сведения об этих и других параметрах в стиле просмотра см. в документации по API Android [ватчфацестиле. Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) .

После `SetWatchFaceStyle` завершения `OnCreate` создает экземпляр `Paint` объекта ( `hoursPaint` ) и устанавливает его цвет в белый и его размер текста равным 48 пикселям (значение[TEXTSIZE](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) должно быть указано в пикселях).

### <a name="implement-the-engine-ondraw-method"></a>Реализация метода OnDraw Engine

Этот `OnDraw` метод, возможно, является самым важным `CanvasWatchFaceService.Engine` методом &ndash; , который фактически рисует элементы с контрольными знаками, такие как цифры и циферблаты.
В следующем примере на экране просмотра выводится строка времени.
Добавьте следующий метод к `MyWatchFaceEngine`:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str,
        (float)(frame.Left + 70),
        (float)(frame.Top  + 80), hoursPaint);
}
```

При вызове Android `OnDraw` он передается в `Canvas` экземпляр и границы, в которых может быть нарисован лицо. В приведенном выше примере кода `DateTime` используется для вычисления текущего времени в часах и минутах (в 12-часовом формате). Результирующая строка времени рисуется на холсте с помощью `Canvas.DrawText` метода. Строка будет отображаться на 70 пикселей сверху от левого края до 80 пикселей вниз от верхнего края.

Дополнительные сведения о `OnDraw` методе см. в документации по API [OnDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine#ondraw) для Android.

### <a name="implement-the-engine-ontimetick-method"></a>Реализация метода Онтиметикк Engine

Android периодически вызывает `OnTimeTick` метод для обновления времени, отображаемого лицом просмотра. Он вызывается по крайней мере один раз в минуту (как в режиме окружающего внешнего, так и интерактивного режима), так и при изменении даты/времени или часового пояса. Добавьте следующий метод к `MyWatchFaceEngine`:

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

Эта реализация `OnTimeTick` просто вызывает `Invalidate` . `Invalidate`Метод планирует `OnDraw` перерисовку циферблата.

Дополнительные сведения о `OnTimeTick` методе см. в документации по API [Онтиметикк](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) для Android.

## <a name="register-the-canvaswatchfaceservice"></a>Регистрация Канвасватчфацесервице

`MyWatchFaceService` должен быть зарегистрирован в **AndroidManifest.xml** связанного с ним приложения "износ". Для этого добавьте в раздел следующий код XML `<application>` :

```xml
<service
    android:name="watchface.MyWatchFaceService"
    android:label="Xamarin Sample"
    android:allowEmbedded="true"
    android:taskAffinity=""
    android:permission="android.permission.BIND_WALLPAPER">
    <meta-data
        android:name="android.service.wallpaper"
        android:resource="@xml/watch_face" />
    <meta-data
        android:name="com.google.android.wearable.watchface.preview"
        android:resource="@drawable/preview" />
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
        <category android:name="com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
</service>
```

Этот XML-код выполняет следующие действия:

1. Задает `android.permission.BIND_WALLPAPER` разрешение. Это разрешение дает службе "наблюдатель лиц" разрешение на изменение рабочего стола системы на устройстве. Обратите внимание, что это разрешение должно быть задано в `<service>` разделе, а не во внешнем `<application>` разделе.

2. Определяет `watch_face` ресурс. Этот ресурс является коротким XML-файлом, который объявляет `wallpaper` ресурс (этот файл будет создан в следующем разделе).

3. Объявляет изображение с `preview` возможностью рисования, которое будет отображаться на экране выбора контрольного значения выборки.

4. Включает, `intent-filter` чтобы позволить Android понять, что  `MyWatchFaceService` будет отображать экран с изображением циферблата.

Это завершает код для базового `WatchFace` примера. Следующим шагом является добавление необходимых ресурсов.

## <a name="add-resource-files"></a>Добавление файлов ресурсов

Перед запуском службы Watch необходимо добавить ресурс **watch_face** и изображение для предварительного просмотра. Сначала создайте новый XML-файл в **Resources/XML/watch_face.xml** и замените его содержимое следующим XML-кодом:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

Задать для этого файла действие сборки **AndroidResource**:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Действие построения](creating-a-watchface-images/10-android-resource-vs.png "Задать для действия сборки значение AndroidResource")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Действие построения](creating-a-watchface-images/10-android-resource-xs.png "Задать для действия сборки значение AndroidResource")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

Этот файл ресурсов определяет простой `wallpaper` элемент, который будет использоваться для просмотра циферблата.

Если вы еще не сделали этого, скачайте [preview.png](creating-a-watchface-images/preview.png).
Установите его на ресурсы, а также **Нарисуйте или preview.png**. Не забудьте добавить этот файл в `WatchFace` проект. Это изображение для предварительного просмотра отображается для пользователя в средстве выбора контрольных лиц на устройстве износа. Чтобы создать изображение предварительного просмотра для вашего собственного контрольного лица, можно сделать снимок экрана просмотра во время его работы. (Дополнительные сведения о получении снимков экрана от износа устройств см. в разделе [Создание снимков экрана](~/android/wear/deploy-test/debug-on-device.md#screenshots)).

## <a name="try-it"></a>Тестирование

Выполните сборку и развертывание приложения на устройстве износа. Появится экран «износ приложения». Выполните следующие действия, чтобы включить новое лицо наблюдения:

1. Проведите вправо до тех пор, пока не появится фон экрана контрольных значений.

2. Нажимайте и держите в любом месте на фоне экрана в течение двух секунд.

3. Проведите пальцем слева направо, чтобы просмотреть различные грани просмотра.

4. Выберите ролик по **образцу Xamarin** (отображается справа):

    [![Средство выбора ватчфаце](creating-a-watchface-images/11-watchface-picker.png "Проведите пальцем, чтобы найти пример для просмотра примера Xamarin")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5. Коснитесь пункта Просмотр **образца Xamarin** , чтобы выбрать его.

Это изменит поверхность циферблата на устройстве износа для использования службы настраиваемых наблюдателей, реализованной на данный момент:

[![На снимке экрана показан пользовательский цифровой контрольный список, работающий на устройстве износа.](creating-a-watchface-images/12-digital-watchface.png "Пользовательское цифровое наблюдение, выполняемое на устройстве износа")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

Это относительно грубыйое лицо, так как реализация приложения настолько проста (например, не включает фон контрольного лица) и не вызывает `Paint` методы сглаживания для улучшения внешнего вида.
Однако он реализует функцию «Несамостоятельная кость», необходимую для создания пользовательского циферблата контрольных значений.

В следующем разделе эта контрольная область будет обновлена до более сложной реализации.

## <a name="upgrading-the-watch-face"></a>Обновление грани просмотра

В оставшейся части этого пошагового руководства `MyWatchFaceService` будет выполнено обновление для показа аналогового видеоролика и его расширение для поддержки большего числа функций. Для создания обновленного лица контрольных значений будут добавлены следующие возможности.

1. Указывает время с аналоговыми часами, минутами и секундами.

2. Реагирует на изменения в видимости.

3. Реагирует на изменения между окружающим режимом и интерактивным режимом.

4. Считывает свойства базового износа устройства.

5. Автоматически обновляет время, когда происходит изменение часового пояса.

Перед реализацией приведенных ниже изменений кода Скачайте [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true), распакуйте его и переместите распакованные PNG-файлы в **ресурсы или нарисованные** (перезапишите предыдущую **preview.png**). Добавьте новые PNG-файлы в `WatchFace` проект.

### <a name="update-engine-features"></a>Функции обработчика обновлений

Следующим шагом является обновление **MyWatchFaceService.CS** до реализации, которая рисует аналоговый видеоролик для просмотра и поддерживает новые функции. Замените содержимое **MyWatchFaceService.CS** на аналоговую версию кода для просмотра в [MyWatchFaceService.CS](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (вы можете вырезать и вставить этот источник в существующий **MyWatchFaceService.CS**).

Эта версия **MyWatchFaceService.CS** добавляет дополнительный код к существующим методам и включает дополнительные переопределенные методы для добавления дополнительных функциональных возможностей. В следующих разделах представлен интерактивный обзор исходного кода.

#### <a name="oncreate"></a>OnCreate

Обновленный метод **OnCreate** настраивает стиль для просмотра, как и раньше, но включает некоторые дополнительные шаги.

1. Задает фоновое изображение для **xamarin_background** ресурса, который находится в **resides/HDPI/xamarin_background.png**.

2. Инициализирует `Paint` объекты для рисования часовой руки, минуты и секунды.

3. Инициализирует `Paint` объект для рисования квантов времени вокруг края циферблата.

4. Создает таймер, который вызывает `Invalidate` метод (перерисовку), так что вторая рука будет перерисовываться каждую секунду. Обратите внимание, что этот таймер необходим, так как `OnTimeTick` вызывает `Invalidate` только один раз каждую минуту.

Этот пример включает только один образ **xamarin_background.png** ; Однако может потребоваться создать другое фоновое изображение для каждой плотности экрана, которую будет поддерживать пользовательское лицо-наблюдатель.

#### <a name="ondraw"></a>OnDraw

Обновленный метод **OnDraw** рисует видеоролик с аналоговым стилем, выполнив следующие действия.

1. Возвращает текущее время, которое теперь поддерживается в `time` объекте.

2. Определяет границы поверхности рисования и ее центра.

3. Рисует фон, масштабируется по размеру устройства при прорисовке фона.

4. Рисует двенадцать *тактов* вокруг циферблата часов (соответствующих часам на циферблате часов).

5. Вычисляет угол, поворот и длину для каждой руки контрольных значений.

6. Рисует каждую руку на поверхности просмотра. Обратите внимание, что вторая рука не рисуется, если контрольные значения находятся в окружающем режиме.

#### <a name="onpropertieschanged"></a>онпропертиесчанжед

Этот метод вызывается для информирования `MyWatchFaceEngine` о свойствах «износа» устройства (например, о низком уровне окружающей среды и защите в режиме записи). В `MyWatchFaceEngine` этот метод выполняет проверку только для режима небольшого двоичного окружения (в режиме с низкими разрядами, экран поддерживает меньшее число бит для каждого цвета).

Дополнительные сведения об этом методе см. в документации по API [онпропертиесчанжед](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) для Android.

#### <a name="onambientmodechanged"></a>онамбиентмодечанжед

Этот метод вызывается, когда износ устройства переходит в окружающий режим или выходит из него. В `MyWatchFaceEngine` реализации контрольный ролик отключает сглаживание при использовании в окружающем режиме.

Дополнительные сведения об этом методе см. в документации по API [онамбиентмодечанжед](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) для Android.

#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

Этот метод вызывается каждый раз, когда контрольные значения становятся видимыми или скрытыми. В `MyWatchFaceEngine` этот метод регистрирует или отменяет регистрацию приемника часового пояса (описан ниже) в соответствии с состоянием видимости.

Дополнительные сведения об этом методе см. в документации по API [онвисибилитичанжед](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) для Android.

### <a name="time-zone-feature"></a>Функция часового пояса

Новый **MyWatchFaceService.CS** также включает функции для обновления текущего времени при каждом изменении часового пояса (например, при командировке по часовым поясам). Ближе к концу **MyWatchFaceService.CS** определяется изменение часового пояса `BroadcastReceiver` , обрабатывающее объекты намерения изменения часового пояса:

```csharp
public class TimeZoneReceiver: BroadcastReceiver
{
    public Action<Intent> Receive { get; set; }
    public override void OnReceive (Context context, Intent intent)
    {
        if (Receive != null)
            Receive (intent);
    }
}
```

`RegisterTimezoneReceiver`Методы и `UnregisterTimezoneReceiver` вызываются `OnVisibilityChanged` методом.
`UnregisterTimezoneReceiver` вызывается, когда состояние видимости наблюдателей изменяется на скрыто. Когда поверхность просмотра снова становится видимой, `RegisterTimezoneReceiver` вызывается (см `OnVisibilityChanged` . метод).

Метод Engine `RegisterTimezoneReceiver` объявляет обработчик для этого события приемника часового пояса `Receive` ; этот обработчик обновляет `time` объект с новым временем при пересечении часового пояса:

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

Для получателя часового пояса создается и регистрируется фильтр намерения:

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

`UnregisterTimezoneReceiver`Метод отменяет регистрацию получателя часового пояса:

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>Запуск улучшенного циферблата

Снова создайте и разверните приложение на устройстве "износ". Выберите пункт Просмотреть грань в средстве выбора наблюдателей. Предварительный просмотр в средстве выбора контрольных значений отображается слева, а новый значок просмотра отображается справа:

[![На снимке экрана показана Улучшенная аналоговая грань в средстве выбора и на устройстве.](creating-a-watchface-images/13-analog-watchface.png "Усовершенствование аналогового лица в средстве выбора и на устройстве")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

На этом снимке экрана вторая рука перемещается один раз в секунду. При запуске этого кода на устройстве, выполняющем износ, вторая рука исчезает, когда контроль переходит в окружающий режим.

## <a name="summary"></a>Сводка

В этом пошаговом руководстве была реализована и протестирована пользовательская износа ватчфаце для Android 1,0. `CanvasWatchFaceService` `CanvasWatchFaceService.Engine` Были введены классы и, а также были реализованы ключевые методы класса Engine для создания простого цифрового циферблата. Эта реализация была обновлена с учетом дополнительных функциональных возможностей для создания аналогового циферблата, а дополнительные методы были реализованы для обработки изменений видимости, внешнего режима и различий в свойствах устройства. Наконец, был реализован широковещательный приемник часового пояса, который следит за тем, чтобы часы автоматически изменяли время, когда пересекается часовой пояс.

## <a name="related-links"></a>Связанные ссылки

- [Создание граней для просмотра](https://developer.android.com/training/wearables/watch-faces/index.html)
- [Пример Ватчфаце](/samples/xamarin/monodroid-samples/wear-watchface)
- [Ватчфацесервице. Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)