---
title: Получение ключа API Google Maps
description: Сведения о том, как получить ключ API Google Карт для добавления функций карт в приложение.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 371876d087c7027d4cfe2d2d9ada8b0dbedb5dd5
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "75488976"
---
# <a name="obtaining-a-google-maps-api-key"></a>Получение ключа API Google Maps

Чтобы использовать функции Google Карты в Android, нужно зарегистрироваться в Google и получить ключ API Карт. Без этого в приложениях вместо карты будет отображаться пустая сетка. Нужно получить ключ API Google Карт для Android версии 2, так как ключи от более старого API Google Карт для Android версии 1 не будут работать.

Чтобы получить ключ API Карт версии 2, необходимо сделать следующее:

1. Получите отпечаток SHA-1 хранилища ключей, которое используется для подписывания приложения.
2. Создайте проект в консоли Google API.
3. Получите ключ API.

## <a name="obtaining-your-signing-key-fingerprint"></a>Получение отпечатка ключа подписывания

Чтобы запросить у Google ключ API для Карт, нужно знать отпечаток SHA-1 хранилища ключей, которое используется для подписывания приложения.
Как правило, вам для этого нужно определить отпечаток SHA-1 для хранилища ключей отладки, а затем отпечаток SHA-1 для хранилища ключей, которое будет использоваться для подписывания приложения для выпуска.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

По умолчанию хранилище ключей, которое используется для подписывания отладочных версий приложений Xamarin.Android, находится по следующему пути:

**C:\\Users\\[ИМЯ_ПОЛЬЗОВАТЕЛЯ]\\AppData\\Local\\Xamarin\\Mono for Android\\debug.keystore**

Информация о хранилище ключей отображается при вызове команды `keytool` из JDK. Обычно это средство находится в каталоге bin для Java:

**C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_[ВЕРСИЯ]\\bin\\keytool.exe**

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

По умолчанию хранилище ключей, которое используется для подписывания отладочных версий приложений Xamarin.Android, находится по следующему пути:

**/Users/[ИМЯ ПОЛЬЗОВАТЕЛЯ]/.local/share/Xamarin/Mono for Android/debug.keystore**

Информация о хранилище ключей отображается при вызове команды `keytool` из JDK. Обычно это средство находится в каталоге bin для Java:

**/System/Library/Java/JavaVirtualMachines/[ВЕРСИЯ].jdk/Contents/Home/bin/keytool**

-----

Откройте средство ключей при помощи следующей команды (учитывая указанные выше пути):

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Пример для Debug.keystore

Для ключа отладки по умолчанию (который автоматически создается для отладки) используйте следующую команду:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----

### <a name="production-keys"></a>Ключи для рабочей среды

При развертывании приложения в Google Play необходимо его [подписать закрытым ключом](~/android/deploy-test/signing/index.md).
Нужно выполнить `keytool` с данными закрытого ключа, а полученный отпечаток SHA-1 применить для создания ключа API Google Карт для рабочей среды. Не забудьте перед развертыванием включить в файл **AndroidManifest.xml** правильный ключ API Google Карт.

### <a name="keytool-output"></a>Выходные данные keytool

В окне консоли должны отобразиться выходные данные, примерно как показано ниже:

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

Отпечаток SHA-1 (указанный после **SHA1**) потребуется вам далее в рамках этого руководства.

## <a name="creating-an-api-project"></a>Создание проекта API

Получив отпечаток SHA-1 для хранилища ключей подписывания, следует создать новый проект в консоли API-интерфейсов Google (или добавить службу API Google Карт для Android версии 2 в существующий проект).

1. В браузере откройте страницу [Google Developers Console API & Services Dashboard](https://console.developers.google.com/apis/dashboard/) (Консоль разработчиков API и панель мониторинга служб Google) и щелкните **Select a project** (Выбрать проект). Щелкните имя проекта или создайте новый, щелкнув **NEW PROJECT** (Создать проект):

   [![Кнопка создания проекта на панели мониторинга разработчика Google](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. Если вы выбрали создание проекта, введите имя для него в открывшемся диалоговом окне **New Project** (Новый проект). С помощью этого диалогового окна будет создан уникальный идентификатор проекта, основанный на его имени. Теперь щелкните **Create** (Создать), как показано в следующем примере:

   [![Новый проект с именем XamarinMapsDemo](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. Примерно через минуту проект будет готов и для него отобразится **панель мониторинга**. Щелкните **Enable APIs and services** (Включить API-интерфейсы и службы).

   [![Щелчок API Google Карт для Android в разделе "Библиотека"](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. На странице **API Library** (Библиотека API) щелкните **Maps SDK for Android** (Пакет SDK Карт для Android). На следующей странице щелкните **ENABLE** (Включить), чтобы включить службу для этого проекта:

   [![Нажатие кнопки "Включить" в разделе "Панель мониторинга"](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

На этом этапе будет создан проект API и в него будет добавлен API Google Карт для Android версии 2. Но вы не сможете использовать этот API в проекте, пока не создадите для него учетные данные. В следующем разделе объясняется, как создать ключ API и включить приложение Xamarin.Android в список разрешений, чтобы ему было разрешено использовать этот ключ.

## <a name="obtaining-the-api-key"></a>Получение ключа API

После создания проекта API на **консоли разработчика Google** вам нужно создать ключ API для Android. Приложения Xamarin.Android должны иметь этот ключ API, чтобы получить доступ к API Карт для Android версии 2.

1. На странице **Maps SDK for Android** (Пакет SDK Карт для Android), которая отобразится после нажатия кнопки **ENABLE** (Включить) на предыдущем шаге, откройте вкладку **Credentials** (Учетные данные) и нажмите кнопку **Create credentials** (Создать учетные данные).

   [![Сообщение об учетных данных пакета SDK Карт для Android](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. Щелкните **API key** (Ключ API):

   [![Диалоговое окно добавления учетных данных в проект](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. После нажатия этой кнопки создается ключ API. Далее необходимо ограничить этот ключ, чтобы только нужное приложение обращалось к API с помощью этого ключа. Щелкните **RESTRICT KEY** (Ограничить ключ):

   [![Щелчок параметра ограничения ключа на странице учетных данных](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. В поле **Имя** замените значение **API Key 1** понятным именем, которое поможет идентифицировать назначение этого ключа (в нашем примере это **XamarinMapsDemoKey**). Теперь щелкните переключатель **Android apps** (Приложения Android):

   [![Выбор "Приложения Android" на странице "Учетные данные"](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Чтобы добавить отпечаток SHA-1, щелкните **+ Add package name and fingerprint** (Добавить имя пакета и отпечаток):

   [![Выбор параметра добавления имени пакета и отпечатка](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. Введите имя пакета для приложения и отпечаток сертификата SHA-1 (полученный с помощью `keytool`, как описано выше в этом руководстве). В следующем примере используются имя пакета `XamarinMapsDemo` и отпечаток сертификата SHA-1, полученный из **debug.keystore**:

   [![Введено имя пакета com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Обратите внимание, что для доступа из APK к Google Картам необходимо добавить отпечатки SHA-1 и имена пакетов для каждого хранилища ключей (отладки и выпуска), которое используется для подписывания APK. Например, если вы используете один компьютер для отладки и другой для создания APK выпуска, следует поместить отпечаток сертификата SHA-1 из хранилища ключей отладки на первый компьютер, о отпечаток сертификата SHA-1 из хранилища ключей выпуска на второй компьютер. Щелкните **+ Add package name and fingerprint** (Добавить имя пакета и отпечаток), чтобы добавить еще один отпечаток и имя пакета, как показано в следующем примере:

   [![Добавление второго отпечатка создает еще один сертификат SHA-1](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. Чтобы сохранить внесенные изменения, нажмите кнопку **Save** (Сохранить). Далее вы вернетесь к списку ключей API. Если у вас есть другие ключи API, созданные ранее, они также будут перечислены здесь. В этом примере отображается только один ключ API, который мы создали в предыдущих шагах:

   [![XamarinMapsDemoKey отображается в списке ключей API](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>Подключение проекта к оплачиваемой учетной записи

С 11 июня 2018 г. ключ API не будет работать, если проект не подключен к оплачиваемой учетной записи (несмотря на то, что эта служба предоставляется бесплатно для мобильных приложений).

1. Нажмите кнопку меню "гамбургер" и выберите страницу **Billing** (Выставление счетов):

   [![Выбор пункта выставления счетов в меню "гамбургер"](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. Свяжите проект с оплачиваемой учетной записью, щелкнув **Link a billing account** (Связать учетную запись выставления счетов) а затем **CREATE BILLING ACCOUNT** (Создать учетную запись выставления счетов) во всплывающем окне (если у вас нет учетной записи, вам будет предложено создать новую):

   [![Привязка проекта к учетной записи выставления счетов](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>Добавление ключа в проект

Наконец, добавьте этот ключ API в файл **AndroidManifest.xml** в приложении Xamarin.Android. В следующем примере `YOUR_API_KEY` заменяется ключом API, который мы создали на предыдущих шагах:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...
  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```

## <a name="related-links"></a>Связанные ссылки

- [Консоль API Google](https://code.google.com/apis/console/)
- [Ключ API Google Карт](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
