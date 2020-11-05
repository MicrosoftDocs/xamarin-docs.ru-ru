---
title: Заставка
description: Запуск приложения Android занимает некоторое время, особенно при первом запуске приложения на устройстве. На экране-заставке может отображаться ход выполнения, или для обозначения фирменной символики.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/02/2019
ms.openlocfilehash: bd24fbf6ce99e87c6a7f4ccd8cceef6cbe826f0f
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370537"
---
# <a name="splash-screen"></a>Заставка

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/monodroid-samples/splashscreen)

_Запуск приложения Android занимает некоторое время, особенно при первом запуске приложения на устройстве. На экране-заставке может отображаться ход выполнения, или для обозначения фирменной символики._

## <a name="overview"></a>Обзор

Запуск приложения Android занимает некоторое время, особенно во время первого запуска приложения на устройстве (иногда это называется _холодный запуском_ ). На экране-заставке может отображаться ход выполнения для пользователя или сведения о фирменной символике для обнаружения и продвижения приложения.

В этом руководством рассматривается один прием для реализации экрана-заставки в приложении Android. В нем рассматриваются следующие шаги.

1. Создание изображаемого ресурса для экрана-заставки.

2. Определение новой темы, которая будет отображать рисуемый ресурс.

3. Добавление нового действия в приложение, которое будет использоваться в качестве экрана-заставки, определенного темой, созданной на предыдущем шаге.

[![Пример экрана-заставки Xamarin Logo, за которым следует экран приложения](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)

## <a name="requirements"></a>Требования

В этом учебнике предполагается, что приложение ориентировано на API Android уровня 21 или более поздней версии. Приложение должно также содержать пакеты NuGet **Xamarin. Android. support. v4** и **Xamarin. Android. support. версии 7. AppCompat** , добавленные в проект.

Весь код и XML в этом разделе можно найти в примере проекта [SplashScreen](/samples/xamarin/monodroid-samples/splashscreen) для этого руководством.

## <a name="implementing-a-splash-screen"></a>Реализация экрана-заставки

Самый быстрый способ отображения экрана-заставки — создать пользовательскую тему и применить ее к действию, которое покажет экран-заставку. Когда действие отображается, оно загружает тему и применяет рисуемый ресурс (на который ссылается тема) к фону действия. Такой подход позволяет избежать необходимости создавать файл макета.

Экран-заставка реализуется как действие, которое отображает нарисованную марку, выполняет инициализацию и запускает все задачи. После начальной загрузки приложения действие экрана-заставки запускает основное действие и удаляет себя из стека приложения.

### <a name="creating-a-drawable-for-the-splash-screen"></a>Создание рисунка для экрана-заставки

На экране-заставке будет отображаться XML, отображаемый в фоновом режиме для действия заставки. Для вывода изображения необходимо использовать точечный рисунок (например, PNG или JPG).

Пример приложения определяет нарисованный, вызываемый **splash_screen.xml**. Для этого можно использовать [список слоев](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) для центрирования изображения экрана-заставки в приложении, как показано в следующем коде XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash_logo"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

`layer-list`В данном случае изображение-заставка выравнивается по цвету фона, заданному `@color/splash_background` ресурсом. Образец приложения определяет этот цвет в файле **Resources/Values/colors.xml** :

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  ...
  <color name="splash_background">#FFFFFF</color>
</resources>
```

Дополнительные сведения об `Drawable` объектах см. в [документации Google на устройстве Android](https://developer.android.com/reference/android/graphics/drawable/Drawable).

### <a name="implementing-a-theme"></a>Реализация темы

Чтобы создать пользовательскую тему для действия на экране-заставку, измените (или добавьте) **значения файла/styles.xml** и создайте новый `style` элемент для заставки. Пример файла **значений и style.xml** показан ниже с `style` именем **мисеме. Splash** :

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

  <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>  
    <item name="android:windowFullscreen">true</item>  
    <item name="android:windowContentOverlay">@null</item>  
    <item name="android:windowActionBar">true</item>  
  </style>
</resources>
```

**Мисеме. Splash** является весьма Spartan &ndash; он объявляет фон окна, явно удаляет строку заголовка из окна и объявляет, что он находится в полноэкранном режиме. Если вы хотите создать экран-заставку, который эмулирует пользовательский интерфейс приложения до того, как действие будет расровнить первый макет, можно использовать `windowContentOverlay` вместо `windowBackground` в определении стиля. В этом случае необходимо также изменить **splash_screen.xml** нарисованной, чтобы она отображала ЭМУЛЯЦИЮ пользовательского интерфейса.

### <a name="create-a-splash-activity"></a>Создание действия-заставки

Теперь нам требуется новое действие для запуска Android, которое имеет образ заставки и выполняет любые задачи запуска. Следующий код является примером полной реализации экрана-заставки:

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity` явно использует тему, созданную в предыдущем разделе, переопределяя тему приложения по умолчанию.
Нет необходимости загружать макет в `OnCreate` , так как тема объявляет изображение, которое можно изобразить в качестве фона.

Важно установить `NoHistory=true` атрибут таким образом, чтобы действие было удалено из стека назад. Чтобы кнопка "назад" не отменила процесс запуска, можно также переопределить `OnBackPressed` и сделать это не так:

```csharp
public override void OnBackPressed() { }
```

Работа по запуску выполняется асинхронно в `OnResume` . Это необходимо, чтобы при запуске не замедлить работу или задержать внешний вид экрана запуска. После завершения работы `SplashActivity` запустится `MainActivity` и пользователь может начать взаимодействие с приложением.

Это новое `SplashActivity` значение задается в качестве действия запуска для приложения путем присвоения `MainLauncher` атрибуту значения `true` . Так как `SplashActivity` теперь является действием запуска, необходимо изменить `MainActivity.cs` и удалить `MainLauncher` атрибут из `MainActivity` :

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>Альбомный режим

Экран-заставка, реализованный на предыдущих шагах, будет правильно отображаться в книжном и альбомном режиме. Однако в некоторых случаях необходимо иметь отдельные экраны-заставки для книжных и альбомных режимов (например, если изображение-заставка находится в полноэкранном режиме).

Чтобы добавить экран-заставку для альбомного режима, выполните следующие действия.

1. В папке **Resources/Draw (ресурсы и нарисованная** ) добавьте альбомную версию изображения экрана-заставки, которую вы хотите использовать. В этом примере **splash_logo_land.png** является альбомной версией эмблемы, которая использовалась в приведенных выше примерах (она использует белую букву вместо синего).

2. В папке **Resources/Draw (ресурсы и нарисованная** ) создайте альбомную версию `layer-list` рисунка, которая была определена ранее (например, **splash_screen_land.xml** ). В этом файле задайте путь к точечному рисунку для альбомной версии изображения экрана-заставки. В следующем примере **splash_screen_land.xml** использует **splash_logo_land.png** :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <item>
        <color android:color="@color/splash_background"/>
      </item>
      <item>
        <bitmap
            android:src="@drawable/splash_logo_land"
            android:tileMode="disabled"
            android:gravity="center"/>
      </item>
    </layer-list>
    ```

3. Создайте папку " **Resources/Values-Land** ", если она еще не существует.

4. Добавьте файлы **colors.xml** и **style.xml** в **значения-Land** (они могут быть скопированы и изменены из существующих  **значений/colors.xml** и файлов **значений и style.xml** ).

5. Измените **значения-Land/style.xml** так, чтобы в нем использовалась альбомная версия рисунка для `windowBackground` . В этом примере используется **splash_screen_land.xml** :

    ```xml
    <resources>
      <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
      </style>
        <style name="MyTheme" parent="MyTheme.Base">
      </style>
      <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
        <item name="android:windowBackground">@drawable/splash_screen_land</item>
        <item name="android:windowNoTitle">true</item>  
        <item name="android:windowFullscreen">true</item>  
        <item name="android:windowContentOverlay">@null</item>  
        <item name="android:windowActionBar">true</item>  
      </style>
    </resources>
    ```

6. Измените **значения-Land/colors.xml** , чтобы настроить цвета, которые будут использоваться в альбомной версии экрана-заставки. В этом примере цвет фона заставки меняется на синий для альбомного режима:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
      <color name="primary">#2196F3</color>
      <color name="primaryDark">#1976D2</color>
      <color name="accent">#FFC107</color>
      <color name="window_background">#F5F5F5</color>
      <color name="splash_background">#3498DB</color>
    </resources>
    ```

7. Выполните сборку и запустите приложение еще раз. Переворачивайте устройство в альбомный режим, пока экран-заставка все еще отображается. Экран-заставка изменится на альбомную версию:

    [![Поворот экрана-заставки в альбомный режим](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)

Обратите внимание, что использование экранной заставки с альбомным режимом не всегда обеспечивает удобство работы. По умолчанию Android запускает приложение в книжной ориентации и переходит в альбомный режим, даже если устройство уже находится в альбомном режиме. В результате, если приложение запускается, когда устройство находится в альбомной ориентации, устройство ненадолго отображает портретный экран-заставку, а затем анимируется поворот с книжной страницы на альбомный экран-заставку. К сожалению, этот начальный переход на книжную ориентацию происходит даже при `ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape` указании флагов действия Splash. Лучший способ обойти это ограничение — создать одно изображение экрана-заставки, которое правильно визуализируется как в книжной, так и альбомной ориентации.

## <a name="summary"></a>Сводка

В этом руководством обсуждался один из способов реализации экрана-заставки в приложении Xamarin. Android. а именно, применение пользовательской темы к действию запуска.

## <a name="related-links"></a>Связанные ссылки

- [SplashScreen (пример)](/samples/xamarin/monodroid-samples/splashscreen)
- [Список слоев](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [Шаблоны проектирования материалов — экраны запуска](https://material.io/design/communication/launch-screen.html#usage)