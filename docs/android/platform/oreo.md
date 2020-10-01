---
title: Функции Oreo
description: Как приступить к использованию Xamarin.Android для разработки приложений для последней версии Android.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: davidortinau
ms.author: daortin
ms.date: 07/06/2018
ms.openlocfilehash: 4c017f7b27d646155bb5c8e556955f495bde6be7
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457566"
---
# <a name="oreo-features"></a>Функции Oreo

_Как приступить к использованию Xamarin.Android для разработки приложений для последней версии Android._

[Android 8.0 Oreo](https://developer.android.com/index.html) — это последняя версия Android, доступная в Google. Android Oreo предоставляет множество новых функций, представляющих интерес для разработчиков Xamarin.Android. Это каналы уведомлений, индикаторы уведомлений, пользовательские шрифты в XML, скачиваемые шрифты, автозаполнение и режим "картинка в картинке" (PIP). Android Oreo включает в себя новые API для этих новых возможностей, и эти API доступны для приложений Xamarin.Android при использовании Xamarin.Android 8.0 и более поздних версий.

[![Изображение героя Android Oreo](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

Эта статья содержит сведения, которые помогут вам приступить к разработке приложений Xamarin.Android для Android 8.0 Oreo. В нее объясняется, как установить необходимые обновления, настроить пакет SDK и создать эмулятор (или устройство) для тестирования. Здесь также приводится описание новых функций в Android 8.0 Oreo со ссылками на примеры приложений, демонстрирующих использование функций Android Oreo в приложениях Xamarin.Android.

## <a name="requirements"></a>Требования

Ниже приведены компоненты, необходимые для использования возможностей Android Oreo в приложениях на основе Xamarin.

- **Visual Studio** &ndash;Если вы используете Windows, требуется версия 15.5 или более поздняя версия Visual Studio.  Если вы используете Mac, требуется Visual Studio для Mac версии 7.2.0.

- **Xamarin.Android** &ndash;Необходимо установить и настроить Xamarin.Android 8.0 или более поздней версии в Visual Studio.

- **Пакет SDK для Android** &ndash;Необходимо установить пакет SDK для Android 8.0 (API 26) или более поздней версии через Диспетчер SDK Android.

## <a name="getting-started"></a>Начало работы

Чтобы приступить к использованию приложений для Android Oreo с помощью Xamarin.Android, необходимо скачать и установить последние версии средств и пакетов SDK до создания проекта Android Oreo:

1. Выполните обновление до последней версии Visual Studio.

2. Установите пакеты и инструменты **Android 8.0.0 (API 26)** или более поздней версии с помощью Диспетчера SDK.

3. Создайте новый проект Xamarin.Android, предназначенный для Android Oreo (API 26).

4. Настройте эмулятор или устройство для тестирования приложений Android Oreo.

В следующих разделах отдельно рассматривается каждый шаг.

### <a name="update-visual-studio-and-xamarinandroid"></a>Обновление Visual Studio и Xamarin.Android

Чтобы добавить поддержку Android Oreo в Visual Studio, выполните следующие действия.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

- Для Visual Studio 2019 используйте [Диспетчер SDK](~/android/get-started/installation/android-sdk.md), чтобы установить уровень API 26.0 или более поздней версии.

- Если используется Visual Studio 2017, сделайте следующее.

    1. Выполните обновление до Visual Studio 2017 версии 15.7 или более поздней версии (ознакомьтесь с [обновлением Visual Studio 2017](/visualstudio/install/update-visual-studio)).

    2. Используйте [Диспетчер SDK](~/android/get-started/installation/android-sdk.md), чтобы установить уровень API 26.0 или более поздней версии.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

- Выполните обновление до последней стабильной версии Visual Studio для Mac, как описано в разделе [Обновление Visual Studio для Mac](/visualstudio/mac/update).

-----

Дополнительные сведения о поддержке Xamarin в Android Oreo доступны в [заметках о выпуске Xamarin.Android 8.0](/xamarin/android/release-notes/8/8.0/).

### <a name="install-the-android-sdk"></a>Установка пакета SDK для Android

Чтобы создать проект с использованием Xamarin.Android 8.0, необходимо сначала использовать Диспетчер SDK Android, чтобы установить платформу SDK для **Android 8.0 Oreo** или более поздней версии. Необходимо также установить Android SDK Tools 26.0 или более поздней версии.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Чтобы запустить Диспетчер SDK в Visual Studio, щелкните **Сервис > Android > Диспетчер пакетов SDK Android**.

2. Установите пакеты **Android 8.0 Oreo**. Если используется эмулятор пакета SDK для Android, не забудьте добавить образы систем **x86**, которые вам понадобятся.

    [![Выбор пакетов Android 8.0 в Диспетчере SDK Android](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Установите **Android SDK Tools 26.0.2** или более поздней версии, **Android SDK Platform-Tools 26.0.0** или более поздней версии и **Android SDK Build-Tools 26.0.0** или более поздней версии.

    [![Выбор Android SDK Tools 26 в Диспетчере SDK Android](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Чтобы запустить Диспетчер SDK в Visual Studio для Mac, щелкните **Сервис > Диспетчер пакетов SDK**.

2. Установите пакеты SDK **Android 8.0 Oreo**. Если используется эмулятор пакета SDK для Android, не забудьте добавить образы систем **x86**, которые вам понадобятся.

    [![Выбор пакетов Android 8.0 в Диспетчере SDK](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Установите **Android SDK Tools 26.0.2** или более поздней версии, **Android SDK Platform-Tools 26.0.0** или более поздней версии и **Android SDK Build-Tools 26.0.0** или более поздней версии.

    [![Выбор Android SDK Tools 26 в Диспетчере SDK](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----

### <a name="start-a-xamarinandroid-project"></a>Запуск проекта Xamarin.Android

Создайте проект Xamarin.Android. Если ранее вы не разрабатывали приложения для Android с использованием Xamarin, ознакомьтесь с [этой статьей](~/android/get-started/hello-android/index.md), чтобы узнать, как создавать проекты Xamarin.Android.

При создании проекта Android необходимо настроить параметры версии для целевой версии Android 8.0 или более поздней версии. Например, чтобы выбрать проект для Android 8.0, необходимо настроить для проекта целевой уровень API Android — **Android 8.0 (API 26)** . Рекомендуем также задать API 26 или выше для уровня требуемой версии .NET Framework. Дополнительные сведения о настройке уровней API Android см. в разделе [Общие сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md).

### <a name="configure-an-emulator-or-device"></a>Настройка эмулятора или устройства

При попытке запустить AVD Manager Google по умолчанию на основе графического пользовательского интерфейса после установки Android SDK Tools 26.0 (или более поздней версии) может появиться приведенное ниже диалоговое окно с сообщением об ошибке, которое предписывает использовать программу командной строки AVD Manager **avdmanager**.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Диалоговое окно предупреждения диспетчера Android Emulator](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Диалоговое окно предупреждения диспетчера Android Emulator](oreo-images/mac/03-avd-warning.png)

-----

Это сообщение отображается, так как Google больше не предоставляет изолированный графический пользовательский интерфейс AVD Manager, поддерживающий API 26.0 и более поздние версии. Для Android 8.0 Oreo необходимо использовать диспетчер Xamarin Android Emulator Manager или программу командной строки `avdmanager`, чтобы создать виртуальные устройства для Android Oreo.

Использование Диспетчера устройств Android для создания виртуальных устройств и управления ими описывается в разделе [Управление виртуальными устройствами с помощью Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).
Чтобы создать виртуальные устройства без Диспетчера устройств Android, выполните действия, описанные в следующем разделе.

#### <a name="creating-virtual-devices-using-avdmanager"></a>Создание виртуальных устройств с помощью avdmanager

Чтобы использовать **avdmanager** для создания виртуального устройства, выполните следующие действия.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Откройте командное окно и задайте для `JAVA_HOME` расположение пакета SDK для Java на компьютере. Для типичной установки Xamarin можно использовать следующую команду.

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2. Добавьте расположение папки `bin` с пакетом SDK для Android в `PATH`.
    Для типичной установки Xamarin можно использовать следующую команду.

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3. Закройте текущее командное окно и откройте новое. Создайте виртуальное устройство с помощью команды [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html). Например, чтобы создать AVD с именем **AVD-Oreo-8.0** с помощью системного образа x86 для уровня API 26, используйте приведенную ниже команду.

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4. При появлении запроса **Do you wish to create a custom hardware profile? [no]** (Создать настраиваемый профиль оборудования? [Нет]) можно ввести **no** (Нет) и принять профиль оборудования по умолчанию. Если вы указали **yes** (Да), **avdmanager** предложит список вопросов по настройке профиля оборудования.

После использования **avdmanager** для создания виртуального устройства оно будет добавлено в раскрывающееся меню устройств.

[![Новое устройство AVD, добавленное в раскрывающееся меню устройств](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Откройте окно **Терминала** и перейдите в каталог инструментов пакета SDK для Android на компьютере Mac. Для типичной установки Xamarin можно использовать следующую команду.

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2. Создайте виртуальное устройство с помощью команды [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html). Например, чтобы создать AVD с именем **AVD-Oreo-8.0** с помощью системного образа x86 для уровня API 26, используйте приведенную ниже команду.

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3. При появлении запроса **Do you wish to create a custom hardware profile? [no]** (Создать настраиваемый профиль оборудования? [Нет]) можно ввести **no** (Нет) и принять профиль оборудования по умолчанию. Если вы указали **yes** (Да), **avdmanager** предложит список вопросов по настройке профиля оборудования.

После использования **avdmanager** для создания виртуального устройства оно будет добавлено в раскрывающееся меню устройств.

[![Новое устройство AVD, добавленное в раскрывающееся меню устройств](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Сведения о настройке Android Emulator для тестирования и отладки см. в разделе [Отладка приложений в Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

При использовании физического устройства, такого как Nexus или Pixel, можно либо обновить устройство с помощью автоматического обновления по беспроводной связи (OTA), либо скачать образ системы и напрямую установить его на устройство. Дополнительные сведения о ручном обновлении устройства до Android Oreo см. в статье [Factory Images for Nexus and Pixel Devices](https://developers.google.com/android/images) (Заводские образы для устройств Nexus и Pixel).

## <a name="new-features"></a>Новые функции

В Android Oreo введено множество новых функций и возможностей, таких как каналы уведомлений, индикаторы уведомлений, пользовательские шрифты в XML, скачиваемые шрифты, автозаполнение и режим "картинка в картинке". В следующих разделах описаны эти функции и приведены ссылки, которые помогут приступить к их использованию в приложении.

### <a name="notification-channels"></a>Каналы уведомлений

*Каналы уведомлений* — это категории для уведомлений, определяемые приложением.
Вы можете создать канал уведомлений для каждого типа уведомления, которые необходимо отправлять, а также создать каналы уведомлений для отображения выбора, сделанного пользователями вашего приложения. Новая функция каналов уведомлений позволяет пользователям точно управлять различными видами уведомлений. Например, если вы реализуете приложение для обмена сообщениями, то можно создать отдельные каналы уведомлений для каждой группы общения, созданной пользователем.

В примере [каналов уведомлений](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) объясняется, как создать канал уведомлений и использовать его для отправки локальных уведомлений. Код реального приложения приведен в примере [каналов уведомлений](/samples/xamarin/monodroid-samples/android-o-notificationchannels). Этот пример приложения управляет двумя каналами и задает дополнительные параметры уведомлений.

### <a name="notification-badges"></a>Индикаторы уведомлений

Индикаторы уведомлений — это небольшие точки, отображаемые над значками приложений, как показано на следующем снимке экрана.

[![Пример индикаторов уведомлений на значках приложения](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Эти точки указывают на наличие новых уведомлений для одного или нескольких каналов уведомлений в приложении, связанном с этим значком приложения. Они означают, что пользователь еще не закрыл уведомление или не отреагировал на него. Пользователи могут нажать значок и удержать его, чтобы просмотреть уведомления, связанные с индикатором уведомлений, а затем закрыть уведомления или отреагировать на них с помощью появившегося меню длительного нажатия.

Дополнительные сведения об индикаторах уведомлений см. в разделе для разработчиков для Android [Notification Badges](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) (Индикаторы уведомлений).

### <a name="custom-fonts-in-xml"></a>Пользовательские шрифты в XML

Android Oreo представляет *шрифты в XML*, что позволяет внедрять пользовательские шрифты в качестве ресурсов. Поддерживаются форматы шрифтов OpenType (**OTF**-файлы) и TrueType (**TTF**-файлы). Чтобы добавить шрифты в качестве ресурсов, выполните следующие действия.

1. Создайте папку **Resources/font**.

2. Скопируйте файлы шрифтов (например, **TTF**- и **OTF**-файлы) в папку **Resources/font**. 

3. При необходимости переименуйте файлы шрифтов, чтобы они соответствовал соглашениям об именовании файлов Android (т. е. используйте в именах файлов только строчные буквы *a–z*, цифры *0–9* и знаки подчеркивания). Например, файл шрифта `Pacifico-Regular.ttf` можно переименовать в `pacifico.ttf`.

4. Примените пользовательский шрифт с помощью нового атрибута `android:fontFamily` в XML макета. Например, в объявлении `TextView` ниже используется добавленный ресурс шрифта **pacifico.ttf**.

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Можно также создать XML-файл семейства шрифтов, описывающий несколько шрифтов, а также сведения о стиле и толщине. Дополнительные сведения см. в разделе для разработчиков для Android [Fonts in XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) (Шрифты в XML).

### <a name="downloadable-fonts"></a>Скачиваемые шрифты

Начиная с Android Oreo, приложения могут запрашивать шрифты от поставщика, а не объединять их в APK. Шрифты скачиваются из сети только по мере необходимости. Эта функция сокращает размер APK, уменьшая использование памяти телефона и мобильного канала передачи данных. Эту функцию можно также использовать в версиях API Android 14 и более поздних версиях, установив пакет библиотеки поддержки версии 26 для Android.

Если приложению требуется шрифт, создайте объект `FontsRequest` (указав шрифт для скачивания) и передайте его в метод `FontsContract` для скачивания шрифта. Ниже более подробно описывается процесс скачивания шрифта.

1. Создайте экземпляр объекта [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html). 

2. Выведите подкласс и создайте экземпляр [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3. Реализуйте метод [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29), который используется для обработки запроса шрифта.

4. Реализуйте метод [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29), который используется для сообщения приложению об ошибках, происходящих во время обработки запроса шрифта.

5. Вызовите метод [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)), чтобы получить шрифт от поставщика шрифтов. 

При вызове метода `RequestFonts` сначала проверяется, не сохранен ли шрифт в локальном кэше (из предыдущего вызова метода `RequestFont`). Если его нет в кэше, метод вызывает поставщик шрифтов, асинхронно получает шрифт, а затем передает результаты в приложение, вызывая метод `OnTypeFaceRetrieved`.

В примере [Downloadable Fonts](/samples/xamarin/monodroid-samples/android-o-downloadablefonts) демонстрируется использование функции скачивания шрифтов в Android Oreo. 

Дополнительные сведения о скачивании шрифтов см. в разделе для разработчиков Android [Downloadable Fonts](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) (Скачиваемые шрифты).

### <a name="autofill"></a>Автозаполнение

Новая платформа _автозаполнения_ в Android Oreo упрощает пользователям обработку повторяющихся задач, таких как вход в систему, создание учетной записи и транзакции по кредитной карте. Пользователи тратят меньше времени на повторный ввод информации (который может приводить к ошибкам ввода). Прежде чем приложение сможет работать с платформой автозаполнения, в параметрах системы необходимо включить службу автозаполнения (пользователи могут включить или отключить автозаполнение).

В примере [AutofillFramework](/samples/xamarin/monodroid-samples/android-o-autofillframework) демонстрируется использование платформы автозаполнения. Он включает в себя реализацию клиентских действий с представлениями, которые должны быть заполнены автоматически, и службу, которая может предоставить данные автозаполнения для действий клиента.

Дополнительные сведения о новой функции автозаполнения и о том, как оптимизировать приложение для автозаполнения, см. в разделе для разработчиков для Android [Autofill Framework](https://developer.android.com/guide/topics/text/autofill.html) (Платформа автозаполнения).

### <a name="picture-in-picture-pip"></a>Режим "картинка в картинке" (PIP)

Android Oreo позволяет запустить действие в режиме "картинка в картинке" (PIP), наложив его на экран другого действия. Эта функция предназначена для воспроизведения видео.

Чтобы указать, что действие приложения может использовать режим PIP, установите значение true для приведенного ниже флага в манифесте Android.

```xml
android:supportsPictureInPicture
```

Чтобы указать, как действие должно вести себя в режиме PIP, используйте новый объект [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html). `PictureInPictureParams` представляет набор параметров, которые используются для инициализации и обновления действия в режиме PIP (например, для задания предпочтительных пропорций для действия). Ниже приведены новые методы PIP, добавленные в `Activity` в Android Oreo.

- [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; переключает действие в режим PIP. Действие размещается в углу экрана, а остальная часть экрана заполняется предыдущим действием, которое было на экране.

- [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; обновляет параметры конфигурации PIP действия (например, для изменения пропорций).

Пример [PictureInPicture](/samples/xamarin/monodroid-samples/android-o-pictureinpicture) демонстрирует основы использования режима "картинка в картинке" (PIP) для наладонных устройств, представленного в Oreo. Пример воспроизводит видео, которое не прерывается при переключении между режимами отображения и другими действиями.

### <a name="other-features"></a>Другие возможности

Android Oreo содержит множество других новых функций, таких как библиотека поддержки эмодзи, API расположения, ограничение работы в фоновом режиме, широкая цветовая палитра для приложений, новые аудиокодеки, усовершенствования WebView, улучшенная поддержка навигации с помощью клавиатуры и новый API AAudio (Pro Audio) для высококачественного звука с низкой задержкой. Дополнительные сведения об этих возможностях см. в статье для разработчиков [Android 8.0 Features and APIs](https://developer.android.com/about/versions/oreo/android-8.0.html) (Возможности и интерфейсы API Android 8.0).

## <a name="behavior-changes"></a>Изменения в работе

Android Oreo содержит различные изменения в работе системы и API, которые могут оказать влияние на функциональность существующих приложений. Эти изменения описаны ниже.

### <a name="background-execution-limits"></a>Ограничения фонового выполнения

Чтобы улучшить взаимодействие с пользователем, Android Oreo накладывает ограничения на возможности приложений, работающих в фоновом режиме. Например, если пользователь смотрит видео или играет в игру, приложение, работающее в фоновом режиме, может снизить производительность приложения, отображающего видео на переднем плане. В результате Android Oreo накладывает следующие ограничения на приложения, которые не взаимодействуют с пользователем напрямую.

1. **Ограничения фоновых служб** &ndash; Если приложение выполняется в фоновом режиме, ему предоставляется несколько минут, в течение которых оно может создавать и использовать службы. По истечении этого периода Android останавливает фоновую службу приложения и считает ее _неактивной_.

2. **Ограничения трансляции** &ndash; Android 7.0 (API 25) накладывают ограничения на трансляции, для получения которых регистрируется приложение. Android Oreo усиливает эти ограничения. Например, приложения Android Oreo больше не могут регистрировать широковещательные приемники для неявных трансляций в своих манифестах.

Дополнительные сведения о новых ограничениях выполнения в фоновом режиме см. в разделе для разработчиков для Android [Background Execution Limits](https://developer.android.com/about/versions/oreo/background.html) (Ограничения фонового выполнения).

### <a name="breaking-changes"></a>Критические изменения

Приложения, нацеленные на Android Oreo или более поздние версии, должны быть изменены для поддержки приведенных ниже изменений, где это применимо.

- Android Oreo прекращает поддержку задания приоритета отдельных уведомлений. Вместо этого при создании канала уведомлений задается рекомендуемый уровень важности. Уровень важности, назначаемый каналу уведомлений, применяется ко всем сообщениям, отправляемым в него.

- Для приложений, нацеленных на Android Oreo, `PendingIntent.GetService()` не работает из-за новых ограничений, накладываемых на службы, запущенные в фоновом режиме. Если вы разрабатываете приложения для Android Oreo, вместо этого следует использовать [Pendingintents.Broadcast](xref:Android.App.PendingIntent.GetBroadcast*).  

## <a name="sample-code"></a>Пример кода

Существует несколько примеров Xamarin.Android, демонстрирующих использование преимуществ функций Android Oreo.

- [NotificationsChannels](/samples/xamarin/monodroid-samples/android-o-notificationchannels) демонстрирует использование новой системы каналов уведомлений, представленной в Android Oreo. Этот пример управляет двумя каналами уведомлений: одним с важностью по умолчанию и другим с высокой важностью.

- [PictureInPicture](/samples/xamarin/monodroid-samples/android-o-pictureinpicture) демонстрирует основы использования режима "картинка в картинке" (PIP) для наладонных устройств, представленного в Oreo. Пример воспроизводит видео, которое не прерывается при переключении между режимами отображения и другими действиями.

- В примере [AutofillFramework](/samples/xamarin/monodroid-samples/android-o-autofillframework) демонстрируется использование платформы автозаполнения. Он включает в себя реализацию клиентских действий с представлениями, которые должны быть заполнены автоматически, и службу, которая может предоставить данные автозаполнения для действий клиента.

- В примере [Downloadable Fonts](/samples/xamarin/monodroid-samples/android-o-downloadablefonts) показано, как использовать функцию скачивания шрифтов, описанную выше.

- В примере [EmojiCompat](/samples/xamarin/monodroid-samples/android-o-emojicompat) демонстрируется использование библиотеки поддержки EmojiCompat. Вы можете использовать эту библиотеку, чтобы предотвратить отображение отсутствующих знаков эмодзи в приложении в виде пустых квадратов.

- В примере [Location Updates Pending Intent](/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdpendintent) демонстрируется использование API расположения для получения данных об изменении расположения устройства с помощью `PendingIntent`.

- В примере [Location Updates Foreground Service](/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdfgservice) демонстрируется использование API расположения для получения данных об изменении расположения устройства с помощью привязанной и запущенной службы переднего плана.

## <a name="video"></a>Видео

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Разработка для Android 8.0 Oreo на C#**

## <a name="summary"></a>Сводка

Из этой статьи вы узнали об Android Oreo, установке и настройке новейших инструментов и пакетов для разработки приложений Xamarin.Android на Android Oreo. В статье предоставлен обзор основных возможностей, доступных в Android Oreo, а также ссылки на примеры исходного кода для нескольких новых функций. В ней содержатся ссылки на документацию по API и разделы для разработчиков для Android, которые помогут вам приступить к созданию приложений для Android Oreo. В статье также описаны самые важные изменения в поведении Android Oreo, которые могут повлиять на существующие приложения.

## <a name="related-links"></a>Связанные ссылки

- [Android 8.0 Oreo](https://developer.android.com/index.html)