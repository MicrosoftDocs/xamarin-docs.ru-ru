---
title: Установка в Windows
description: В этом руководстве описаны действия по установке Xamarin.Android для Visual Studio в Windows, а также содержатся сведения о настройке Xamarin.Android для создания первого приложения Xamarin.Android.
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 20e75914699a60496860d906c74046f9057dd78f
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84568286"
---
# <a name="windows-installation"></a>Установка в Windows

_В этом руководстве описаны действия по установке Xamarin.Android для Visual Studio в Windows, а также содержатся сведения о настройке Xamarin.Android для создания первого приложения Xamarin.Android._

## <a name="overview"></a>Обзор

Поскольку теперь Xamarin является бесплатным компонентом всех выпусков Visual Studio и не требует отдельной лицензии, для скачивания и установки средств Xamarin.Android можно использовать установщик Visual Studio.
(Выполнять действия по установке вручную и лицензированию, которые требовались для более ранних версий Xamarin.Android, больше не нужно.) В этом руководстве рассматриваются следующие темы:

- Настройка пользовательских расположений для пакета Java Development Kit, пакета SDK для Android и пакета NDK для Android.

- Запуск диспетчера пакетов SDK для Android для скачивания и установки дополнительных компонентов пакета SDK для Android.

- Подготовка устройства или эмулятора Android для отладки и тестирования.

- Создание первого проекта приложения Xamarin.Android.

После выполнения действий в этом руководстве ваша рабочая установка Xamarin.Android будет интегрирована в Visual Studio, и вы будете готовы приступить к созданию первого приложения Xamarin.Android.

## <a name="installation"></a>Установка

Подробные сведения об установке Xamarin для использования с Visual Studio в Windows см. в руководстве [Установка в Windows](~/get-started/installation/windows.md).

## <a name="configuration"></a>Параметр Configuration

Для создания приложений в Xamarin.Android используется пакет Java Development Kit (JDK) и пакет SDK для Android. Во время установки установщик Visual Studio помещает эти средства в расположения по умолчанию и настраивает среду разработки с соответствующей конфигурацией путей. Чтобы просмотреть или изменить эти расположения, последовательно выберите **Сервис > Параметры > Xamarin > Параметры Android**:

![Снимок экрана диалогового окна параметров Xamarin Android](windows-images/07-settings.png)

В большинстве случаев эти расположения по умолчанию будут работать без дальнейших изменений. Однако в Visual Studio можно настроить пользовательские расположения для этих средств (например, если пакеты Java JDK, SDK для Android или NDK установлены в другом месте). Нажмите кнопку **Изменить** рядом с путем, который требуется изменить, а затем перейдите в новое расположение.

Xamarin.Android использует пакет [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), который необходим при разработке для API уровня 24 или выше (пакет JDK 8 также поддерживает уровни API ниже 24). При разработке специально для уровня API 23 или ниже можно продолжать использовать пакет [JDK 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html).

> [!IMPORTANT]
> Xamarin.Android не поддерживает пакет JDK 9.

### <a name="android-sdk-manager"></a>Диспетчер Android SDK

Android использует несколько параметров уровня API Android для определения совместимости приложения в разных версиях Android (дополнительные сведения об уровнях API Android см. в статье [Общие сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md)).
В зависимости от того, с какими уровнями API Android вы будете работать, может потребоваться скачать и установить дополнительные компоненты пакета SDK для Android. Кроме того, может потребоваться установить дополнительные средства и образы эмулятора из пакета SDK для Android. Для этого следует использовать **диспетчер пакетов SDK для Android**. Запустите **диспетчер пакетов SDK для Android**, выбрав **Сервис > Android > Диспетчер пакетов SDK для Android**:

[![Запуск диспетчера пакетов SDK для Android](windows-images/08-sdk-manager-sml.png)](windows-images/08-sdk-manager.png#lightbox)

Диспетчер пакетов SDK для Android установлен в Visual Studio по умолчанию:

![Снимок экрана с примером диспетчера пакетов SDK для Android Google](windows-images/09-google-sdk-manager.png)

С помощью диспетчера пакетов SDK для Android Google можно устанавливать пакеты инструментов SDK для Android до версии 25.2.3. Однако если необходимо использовать более позднюю версию пакета инструментов SDK для Android, нужно установить подключаемый модуль диспетчера пакетов SDK для Android Xamarin для Visual Studio (доступен в Visual Studio Marketplace). Это необходимо, так как автономный диспетчер пакетов SDK Google был объявлен нерекомендуемым в версии 25.2.3 пакета инструментов SDK для Android. 

Дополнительные сведения об использовании диспетчера пакетов SDK для Android Xamarin см. в статье [Установка пакета SDK для Android](~/android/get-started/installation/android-sdk.md).

### <a name="android-emulator"></a>Эмулятор Android

[Android Emulator](https://developer.android.com/studio/run/emulator) — это удобный инструмент для разработки и тестирования приложений Xamarin.Android. Физическое устройство, например планшет, не всегда доступно во время разработки, или разработчик хочет выполнять тесты на интеграцию на компьютере до фиксации кода.

Эмуляция устройства Android на компьютере включает в себя следующие компоненты:

- **Эмулятор Google Android**. Это эмулятор, основанный на [QEMU](https://www.qemu.org/) и запускающий эмуляцию устройства на рабочей станции разработчика.
- **Образ эмулятора**. _Образ эмулятора_ — это шаблон или спецификация оборудования и операционной системы, которая предназначена для виртуализации. Например, один образ эмулятора определяет требования к оборудованию для Android 7.0 с установленными службами Google Play на Nexus 5X. Другой образ эмулятора может определять планшет с диагональю 10" под управлением Android 6.0.
- **Виртуальное устройство Android (AVD)** . _Виртуальное устройство Android_ — это эмуляция устройства Android, созданная из образа эмулятора. При запуске и тестировании приложений Android Xamarin.Android запустит эмулятор Android, запускающий определенное виртуальное устройство Android, установит пакет приложений для Android и запустит приложение.

Можно значительно повысить производительность при разработке на компьютерах с процессором x86, если использовать особые образы эмуляторов, оптимизированные для архитектуры x86, и одну из двух технологий виртуализации:

1. Microsoft Hyper-V &ndash; доступно на компьютерах под управлением Windows 10 с обновлением за апрель 2018 г. или более поздней версии.
2. Hardware Accelerated Execution Manager (HAXM) от Intel &ndash; доступно на компьютерах с процессором x86 на базе OS X, macOS или более старой версии Windows.

Дополнительные сведения об Android Emulator, Hyper-V и HAXM см. в разделе [Аппаратное ускорение для производительной работы эмулятора](~/android/get-started/installation/android-emulator/hardware-acceleration.md).

> [!NOTE]
> В версиях Windows до Windows 10 с обновлением за апрель 2018 г. HAXM не совместим с Hyper-V. В этом случае необходимо [отключить Hyper-V](~/android/get-started/installation/android-emulator/troubleshooting.md#disable-hyperv) или использовать более медленные образы эмулятора без оптимизации для x86.

<a name="device"></a>

### <a name="android-device"></a>Устройство Android

Если у вас есть физическое устройство Android для тестирования, настройте его для использования в разработке. Сведения о настройке устройства Android для разработки, его подключении к компьютеру для запуска и отладки приложений Xamarin.Android см. в статье [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="create-an-application"></a>Создание приложения

Теперь, когда вы установили Xamarin.Android, можно запустить Visual Studio для создания проекта. Выберите **Файл > Создать > проект**, чтобы приступить к созданию приложения:

![Создание нового проекта](windows-images/10-new-project.png)

В диалоговом окне **Новый проект** в разделе **Шаблоны** выберите **Android** и на правой панели щелкните **Приложение Android**. Введите имя приложения (на снимке экрана ниже приложение называется **MyApp**), а затем нажмите кнопку **ОК**:

[![Снимок экрана диалогового окна "Новый проект" для создания пустого приложения Android](windows-images/11-first-app-sml.w157.png)](windows-images/11-first-app.w157.png#lightbox)

Вот и все! Теперь вы готовы использовать Xamarin.Android для создания приложений Android!

## <a name="summary"></a>Сводка

В этой статье вы узнали, как настроить и установить платформу Xamarin.Android в Windows, как (необязательно) настроить Visual Studio с пользовательскими расположениями для пакета Java JDK и пакета SDK для Android, как запустить диспетчер пакетов SDK для установки дополнительных компонентов пакета SDK для Android, как настроить устройство или эмулятор Android и как приступить к созданию первого приложения.

Следующим шагом является изучение учебников [Привет, Android](~/android/get-started/hello-android/index.md), содержащих сведения о создании работающего приложения Xamarin.Android.

## <a name="related-links"></a>Связанные ссылки

- [Скачать Visual Studio 2012](https://visualstudio.microsoft.com/vs/)
- [Установка инструментов Visual Studio для Xamarin](~/get-started/installation/windows.md)
- [Требования к системе](~/cross-platform/get-started/requirements.md)
- [Установка пакета SDK для Android](~/android/get-started/installation/android-sdk.md)
- [Настройка эмулятора Android](~/android/get-started/installation/android-emulator/index.md)
- [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md)
- [Запуск приложений в эмуляторе Android](https://developer.android.com/studio/run/emulator#Requirements)
