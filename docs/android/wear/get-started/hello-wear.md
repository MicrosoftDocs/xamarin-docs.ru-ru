---
title: Основные сведения о Wear
description: Создайте свое первое приложение "износ Android" и запустите его в эмуляторе или на устройстве. В этом пошаговом руководстве приводятся пошаговые инструкции по созданию небольшого проекта "износа Android", который обрабатывает нажатия кнопки и отображает счетчик щелчков на устройстве износа. В нем объясняется, как выполнить отладку приложения с помощью эмулятора износа или устройства, которое подключено через Bluetooth к телефону Android. Он также предоставляет набор советов по отладке для износа Android.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/10/2018
ms.openlocfilehash: 8b640e867e3f7adb6960ecda557ac1e410ec5799
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456973"
---
# <a name="hello-wear"></a>Основные сведения о Wear

_Создайте свое первое приложение "износ Android" и запустите его в эмуляторе или на устройстве. В этом пошаговом руководстве приводятся пошаговые инструкции по созданию небольшого проекта "износа Android", который обрабатывает нажатия кнопки и отображает счетчик щелчков на устройстве износа. В нем объясняется, как выполнить отладку приложения с помощью эмулятора износа или устройства, которое подключено через Bluetooth к телефону Android. Он также предоставляет набор советов по отладке для износа Android._

![Снимок экрана приложения "износ", которое будет выполнено в этом учебнике](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>Ваше первое приложение для износа

Выполните следующие действия, чтобы создать первое приложение "износ Xamarin. Android":

### <a name="1-create-a-new-android-project"></a>1. Создание нового проекта Android

Создайте новое **приложение "износ Android**":

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Создание нового приложения "износ Android" в диалоговом окне "Создание проекта"](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Создание нового приложения "износ Android" в диалоговом окне "Создание решения"](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----

Этот шаблон автоматически включает в себя Microsoft **Xamarin Android носимого пользователем Library** NuGet (и зависимости), поэтому у вас будет доступ к мини-приложениям, которые относятся к износу. Если шаблон "недействительность" не отображается, ознакомьтесь с руководством по [установке и настройке](~/android/wear/get-started/installation.md) , чтобы убедиться, что вы установили поддерживаемую пакет SDK для Android. 

### <a name="2-choose-the-correct-target-framework"></a>2. Выберите правильную **целевую платформу**

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Убедитесь, что для параметра **Минимальная Целевая версия Android** установлено значение **Android 5,0 (без описания операций)** или более поздней версии: 

[![Настройка требуемой версии .NET Framework для Android 5,0 в Visual Studio](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Убедитесь, что для целевой платформы задано значение **Android 5,0 (без описания операций)** или более поздней версии:

[![Настройка целевой платформы на Android 5,0 в Visual Studio для Mac](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

Дополнительные сведения о настройке целевой платформы см. в разделе [Основные сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md).

### <a name="3-edit-the-mainaxml-layout"></a>3. изменение макета **Main. axml**

Настройте макет таким образом, чтобы `TextView` он содержал и `Button` для образца: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
  <ScrollView
    android:id="@+id/scroll"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#000000"
    android:fillViewport="true">
    <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:text="Main Activity"
        android:textSize="36sp"
        android:textColor="#006600" />
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:textColor="#cccccc"
        android:id="@+id/result" />
      <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="showNotification"
        android:text="Click Me!"
        android:id="@+id/click_button" />
    </LinearLayout>
  </ScrollView>
</FrameLayout>
```

### <a name="4-edit-the-mainactivitycs-source"></a>4. изменение источника **MainActivity.CS**

Добавьте код для увеличения счетчика и отобразит его при нажатии кнопки: 

```csharp
[Activity (Label = "WearTest", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
  int count = 1;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    Button button = FindViewById<Button> (Resource.Id.click_button);
    TextView text = FindViewById<TextView> (Resource.Id.result);

    button.Click += delegate {
      text.Text = string.Format ("{0} clicks!", count++);
    };
  }
}
```

### <a name="5-setup-an-emulator-or-device"></a>5. Настройка эмулятора или устройства

Следующий шаг — Настройка эмулятора или устройства для развертывания и запуска приложения. Если вы еще не знакомы с процессом развертывания и запуска приложений Xamarin. Android в целом, ознакомьтесь с [кратким руководством по Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).

Если у вас нет такого устройства Android, как Android Смартватч, можно запустить приложение в эмуляторе. Сведения об отладке приложений износа в эмуляторе см. [в разделе Отладка износа Android в эмуляторе](~/android/wear/deploy-test/debug-on-emulator.md).

Если вы используете устройство Android, например "износ Android", вы можете запустить приложение на устройстве, а не в эмуляторе. Дополнительные сведения об отладке на устройстве с износом см. в разделе [Отладка на устройстве с износом](~/android/wear/deploy-test/debug-on-device.md).

### <a name="6-run-the-android-wear-app"></a>6. Запуск приложения "износ Android"

Устройство "износ Android" должно отобразиться в меню устройства. Прежде чем начать отладку, обязательно выберите правильное устройство "износ Android" или AVD. Выбрав устройство, нажмите кнопку Воспроизвести, чтобы развернуть приложение в эмуляторе или на устройстве.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Выбор "износа AVD" в меню "устройство" Visual Studio](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Выбор "износа AVD" в меню "устройство" Visual Studio для Mac](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

В первую очередь может появиться сообщение **только о минуте** (или на другом экране внутреннего): 

![Просмотр эмулятора просмотра всего за минуту...](hello-wear-images/please-wait.png)

При использовании эмулятора наблюдения может потребоваться некоторое время для запуска приложения. При использовании Bluetooth требуется больше времени на развертывание приложения, чем на USB. (Например, это займет около 5 минут, чтобы развернуть это приложение в LG G Watch, подключенном к адаптеру хранилища на основе Bluetooth).

После успешного развертывания приложения на экране устройства «износ» появится экран следующего вида:

[![Начальный экран приложения "износ"](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

Коснитесь элемента **Click Me!** на лицевой стороне устройства «износ» и увидите шаг счетчика с каждым касанием:

[![Снимок экрана приложения "износ" после трех щелчков](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)

## <a name="next-steps"></a>Next Steps

Ознакомьтесь с [примерами износа](/samples/browse/?products=xamarin&term=Xamarin.Android%2bwear) , включая приложения "износ Android", с сопутствующими телефонными приложениями.

Когда вы будете готовы к распространению приложения, см. статью [Работа с упаковкой](~/android/wear/deploy-test/packaging.md).

## <a name="related-links"></a>Связанные ссылки

- [Щелкните приложение Me (пример)](/samples/xamarin/monodroid-samples/wear-weartest)