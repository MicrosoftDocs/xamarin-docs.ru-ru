---
title: Возможности Android 10
description: Начните разрабатывать приложения для Android 10, используя Xamarin.Android.
ms.assetid: B3342772-FB88-4B7F-BC15-8BC78EED749E
author: JonDouglas
ms.author: jodou
ms.date: 09/17/2019
ms.openlocfilehash: 1ce0233a45211ee91b9f1e30247ac18cdd5c5a66
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454303"
---
# <a name="android-10-with-xamarin"></a>Android 10 с Xamarin

_Начните разрабатывать приложения для Android 10, используя Xamarin.Android._

Корпорация Google выпустила ОС Android 10. В этом выпуске предоставляется ряд новых возможностей и API. Многие из них необходимы, чтобы использовать новые преимущества последних устройств Android.

![Логотип Android 10](~/android/platform/android-10-images/android10_black.png)

Эта статья содержит сведения, которые помогут вам приступить к разработке приложений Xamarin.Android для Android 10. В ней объясняется, как установить необходимые обновления, настроить пакет SDK и подготовить эмулятор или устройство для тестирования. Кроме того, здесь приводится описание новых возможностей Android 10, а также пример исходного кода, демонстрирующий использование некоторых ключевых функций Android 10.

Xamarin.Android 10.0 предоставляет поддержку Android 10. Дополнительные сведения о поддержке Android 10 в Xamarin.Android см. в заметках о выпуске [здесь](/xamarin/android/release-notes/10/10.0).

## <a name="requirements"></a>Требования

Компоненты из следующего списка необходимы для использования возможностей Android 10 в приложениях на основе Xamarin.

- **Visual Studio** — рекомендуется использовать Visual Studio 2019. В Windows выполните обновление до Visual Studio версии 16.3 или более поздней. В macOS выполните обновление до Visual Studio 2019 для Mac версии 8.3 или более поздней.
- **Xamarin.Android**. Вместе с Visual Studio нужно установить Xamarin.Android 10.0 или более поздней версии (Xamarin.Android автоматически устанавливается в составе рабочей нагрузки **Разработка мобильных приложений на .NET** в ОС Windows или в составе **установщика Visual Studio для Mac**).
- Пакет **Java Developer Kit**. Для разработки на Xamarin.Android 10.0 требуется JDK 8. Дистрибутив OpenJDK от корпорации Майкрософт автоматически устанавливается в составе Visual Studio.
- **Пакет SDK для Android** — нужно установить пакет SDK для Android с API 29 с помощью Диспетчера SDK Android.

## <a name="get-started"></a>Начало работы

Чтобы приступить к разработке приложений для Android 10 с использованием Xamarin.Android, необходимо скачать и установить последние версии средств и пакетов SDK до создания первого проекта для Android 10:

1. **Рекомендуется использовать Visual Studio 2019**. Обновитесь до Visual Studio 2019 версии 16.3 или более поздней. Если вы используете Visual Studio для Mac 2019, выполните обновление до Visual Studio 2019 для Mac версии 8.3 или более поздней.
2. Установите пакеты и средства **Android 10 (API 29)** с помощью Диспетчера SDK Android.
    - Платформа SDK для Android 10 (API уровня 29)
    - Образ системы Android 10 (API уровня 29)
    - Пакет SDK со средствами сборки для Android версии 29.0.0 и выше
    - Пакет SDK со средствами платформы для Android версии 29.0.0 и выше
    - Эмулятор Android версии 29.0.0 и выше
3. Создайте новый проект Xamarin.Android, предназначенный для Android 10.0.
4. Настройте эмулятор или устройство для тестирования приложений Android 10.

Каждый из этих шагов подробно рассмотрен ниже.

### <a name="update-visual-studio"></a>Обновление Visual Studio

Для создания приложений Android 10 с использованием Xamarin мы рекомендуем использовать Visual Studio 2019.

Если вы используете Visual Studio 2019, обновите его до Visual Studio 2019 версии 16.3 или более поздней (инструкции см. [здесь](/visualstudio/install/update-visual-studio)). На платформе macOS обновите Visual Studio 2019 до версии 8.3 или более поздней (инструкции см. [здесь](/visualstudio/mac/update)).

### <a name="install-the-android-sdk"></a>Установка пакета SDK для Android

Чтобы создать проект с использованием Xamarin.Android 10.0, необходимо сначала с помощью Диспетчера SDK Android установить платформу SDK для **Android 10 (API уровня 29)** .

1. Запустите Диспетчер SDK. В Visual Studio выберите элементы **Сервис > Android > Диспетчер пакетов SDK Android**. В Visual Studio для Mac выберите элементы **Сервис > Диспетчер пакетов SDK**.
2. В правом нижнем углу щелкните значок шестеренки и выберите элементы **Репозиторий > Google (не поддерживается)** :

    ![Выбор репозитория для Диспетчера SDK Android](~/android/platform/android-10-images/sdkrepository.png)

3. Установите пакеты **SDK платформы для Android 10**, которые обозначены в списке как **Android SDK Platform 29** на вкладке **Платформы** (дополнительные сведения об использовании Диспетчера SDK Android см. [здесь](../get-started/installation/android-sdk.md)):

    ![Вкладка "Платформы" Диспетчера SDK Android](~/android/platform/android-10-images/sdkplatforms.png)

### <a name="create-a-xamarinandroid-project"></a>Создание проекта Xamarin.Android

Создайте проект Xamarin.Android. Если вы не знакомы с разработкой приложений для Android с использованием Xamarin, ознакомьтесь с [этой статьей](../get-started/hello-android/index.md), чтобы узнать о создании проектов Xamarin.Android.

При создании проекта Android необходимо настроить параметры версии для целевой версии Android 10.0 или более поздней. Например, чтобы выбрать проект для Android 10, необходимо настроить для проекта целевой уровень API Android — **Android 10.0 (API 29)** . API 29 должна быть указана как для **версии целевой платформы**, так и для **версии целевого пакета SDK для Android**. Дополнительную информацию о настройке уровней API Android, см. в [этой статье](../app-fundamentals/android-api-levels.md).

![Целевая платформа Xamarin.Android](~/android/platform/android-10-images/targetframework.png)

### <a name="configure-a-device-or-emulator"></a>Настройка устройства или эмулятора

Если вы используете физическое устройство, например Pixel, обновление Android 10 вы можете скачать, просто перейдя к разделу Система > Обновление системы > Проверить обновления в настройках телефона. Если вы предпочитаете изменить прошивку устройства, воспользуйтесь инструкциями по [прошивке заводского образа](https://developers.google.com/android/images) или [обновлению образа по беспроводной сети](https://developers.google.com/android/ota) для вашего устройства.

Если вы используете эмулятор, создайте виртуальное устройство, которое поддерживает API уровня 29, и выберите образ на базе x86. Дополнительные сведения о создании виртуальных устройств и управлении ими с помощью Диспетчера устройств Android см. в [этой статье](../get-started/installation/android-emulator/device-manager.md). Сведения об использовании Android Emulator для тестирования и отладки см. [здесь](../deploy-test/debugging/debug-on-emulator.md).

## <a name="new-features"></a>Новые функции

Android 10 предоставляет ряд новых возможностей. Некоторые из них предназначены для работы с аппаратными возможностями новых устройств Android, а другие улучшают взаимодействие пользователей с Android.

## <a name="enhance-your-app-with-android-10-features-and-apis"></a>Усовершенствуйте свое приложение с новыми возможностями и API Android 10

Когда вы будете готовы, приступайте к освоению Android 10 и изучите  [новые функции и API](https://developer.android.com/preview/api-overview.html), которые стали вам доступны. Вот лишь некоторые из самых интересных возможностей, с которых вы можете начать.

Мы рекомендуем использовать эти возможности во всех приложениях.

- **Темная тема** обеспечивает единообразный интерфейс для пользователей, которые используют темную тему на уровне системы, и могут теперь  [добавить темную тему](https://developer.android.com/preview/features/darktheme) или включить  [принудительное применение темной темы](https://developer.android.com/preview/features/darktheme#force_dark).

![Темная тема](~/android/platform/android-10-images/darktheme.png)

- **Полная поддержка  [навигации при помощи жестов](https://developer.android.com/preview/features/gesturalnav)**   в приложении за счет разумного дополнения системных жестов пользовательскими.

![Навигация при помощи жестов](~/android/platform/android-10-images/gesturenavigation.png)

- **Оптимизация для складных устройств:**   предоставляет полноценный оптимизированный интерфейс для современных инновационных  [складных устройств](https://developer.android.com/preview/features/foldables).

![Складные устройства](~/android/platform/android-10-images/foldable.png)

Следующие функции будут полезны, если имеют отношение к функциональности приложения.

- **Больше интерактивности в уведомлениях.**   Если вы отправляете уведомления с сообщениями, добавьте к ним  [рекомендуемые ответы и действия](https://developer.android.com/preview/features#smart-suggestions), которые позволят заинтересовать пользователей и дать им возможность быстро реагировать на сообщения.
- **Улучшенная биометрия.**  Если вы используете проверку подлинности по биометрическим данным, переходите на поддержку  [BiometricPrompt](https://developer.android.com/reference/androidx/biometric/BiometricPrompt), которая гораздо лучше поддерживает вход по отпечатку пальца на современных устройствах.
- **Улучшенная запись.**  Чтобы было удобнее добавлять субтитры или записывать прохождение игр, включите  [запись воспроизводимого звука](https://developer.android.com/preview/features/playback-capture). Это очень привлекательно для многих пользователей, и ваше приложение станет более доступным.
- **Улучшенные кодеки.**  Для мультимедийных приложений примените  [AV1](https://en.wikipedia.org/wiki/AV1) для потоковой передачи видео и  [HDR10+](https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10+) для видео с поддержкой HDR. Для потоковой передачи речи и музыки вы можете применить шифрование  [Opus](http://opus-codec.org/), а для музыкантов есть  [собственный API для MIDI](https://developer.android.com/preview/features/midi) .
- **Улучшенные API для сетевого взаимодействия.**  Если ваше приложение работает с устройствами Интернета вещей через канал Wi-Fi, обязательно попробуйте новые  [API сетевых подключений](https://developer.android.com/preview/features#peer2peer) для функций настройки, скачивания и печати.

Это лишь малая доля из большого числа новых возможностей и API в Android 10. Чтобы увидеть полный список, посетите  [сайт по Android 10 для разработчиков](https://developer.android.com/about/versions/10/highlights).

## <a name="behavior-changes"></a>Изменения в поведении

Если для целевой версии Android задан уровень API 29, то существуют несколько изменений платформы, которые могут повлиять на поведение приложения, даже если вы не реализуете новые возможности, описанные выше. Далее представлена краткая сводка этих изменений:

- [Чтобы гарантировать стабильность и совместимость приложений, платформа Android теперь ограничивает интерфейсы, кроме пакетов SDK, которые может использовать приложение в Android 10](https://developer.android.com/about/versions/10/behavior-changes-10#non-sdk-restrictions).
- [Изменилась работа с общей памятью](https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory).
- [Среда выполнения Android и корректность работы AOT](https://developer.android.com/about/versions/10/behavior-changes-10#system-only-oat).
- [Разрешения на полноэкранные намерения должны запрашивать `USE_FULL_SCREEN_INTENT`](https://developer.android.com/about/versions/10/behavior-changes-10#full-screen-intents).
- [Поддержка складных устройств](https://developer.android.com/about/versions/10/behavior-changes-10#foldables).

## <a name="summary"></a>Сводка

Из этой статьи вы узнали об Android 10, установке и настройке новейших средств и пакетов для разработки приложений для Android 10 на Xamarin.Android. Она предоставила вам обзор новых возможностей, доступных в Android 10. В ней содержатся ссылки на документацию по API и разделы для разработчиков под Android, которые помогут вам приступить к созданию приложений для Android 10. В статье также выделяются самые важные изменения в поведении Android 10, которые могут повлиять на существующие приложения.

## <a name="related-links"></a>Связанные ссылки

- [Android 10](https://developer.android.com/about/versions/10)