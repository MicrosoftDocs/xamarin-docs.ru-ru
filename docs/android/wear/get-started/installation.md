---
title: 'Установка и Настройка "износа ОС" в Xamarin. Android '
description: В этой статье рассматриваются этапы установки и сведения о конфигурации, необходимые для подготовки компьютера и устройств для разработки в Android. К концу этой статьи вы получите рабочую установку Xamarin. Android износа, интегрированную в Visual Studio для Mac и (или) Microsoft Visual Studio, и вы будете готовы приступить к созданию первого приложения "износ Xamarin. Android".
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 7c63f4c245d6fc5cb6e9da6320e159229b07de6e
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453679"
---
# <a name="install-and-setup-wear-os-on-xamarinandroid"></a>Установка и настройка износа ОС в Xamarin. Android

_В этой статье рассматриваются этапы установки и сведения о конфигурации, необходимые для подготовки компьютера и устройств для разработки в Android. К концу этой статьи вы получите рабочую установку Xamarin. Android износа, интегрированную в Visual Studio для Mac и (или) Microsoft Visual Studio, и вы будете готовы приступить к созданию первого приложения "износ Xamarin. Android"._

## <a name="requirements"></a>Требования

Для создания приложений "износ Android" на основе Xamarin необходимо следующее:

- **Visual Studio или Visual Studio для Mac** &ndash; Требуется Visual Studio 2017 Community или более поздней версии.

- **Xamarin. Android** &ndash; xamarin. Android 4,17 или более поздней версии должен быть установлен и настроен с помощью Visual Studio или Visual Studio для Mac.

- **Пакет SDK для Android** -пакет SDK для Android 5.0.1 (API 21) или более поздней версии должен быть установлен с помощью диспетчера пакет SDK для Android.

- **Пакет Java Developer Kit** &ndash; Для разработки Xamarin Android требуется   [JDK 1,8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) , если вы разрабатываете для API уровня 24 или выше (JDK 1,8 также поддерживает уровни API, предшествующие 24).

Вы можете продолжать использовать [JDK 1,7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) при разработке специально для API уровня 23 или более ранней версии.

> [!IMPORTANT]
> Xamarin.Android не поддерживает пакет JDK 9.

## <a name="installation"></a>Установка

После установки Xamarin. Android выполните следующие действия, чтобы приступить к созданию и тестированию приложений для работы с Android.

1. Установите необходимые пакет SDK для Android и средства.
2. Настройка тестового устройства.
3. Создайте свое первое приложение "износ Android".

Эти действия описаны в следующих разделах.

### <a name="install-android-sdk-and-tools"></a>Установка пакет SDK для Android и средств

Запустите **диспетчер пакет SDK для Android**:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Запуск диспетчера пакет SDK для Android в Visual Studio](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Запуск диспетчера пакет SDK для Android в Visual Studio для Mac](installation-images/xs/sdk-menu.png)

-----

Убедитесь, что установлены следующие пакет SDK для Android и средства:

- Android SDK Tools v 24.0.0 или более поздней версии и
- Android 4.4 W (API20) или
- Android 5.0.1 (API21) или более поздней версии.

Если вы не установили последнюю версию пакета SDK и средств, скачайте необходимые средства SDK *и* биты API (для поиска выбранных интерфейсов API может потребоваться прокрутить немного &ndash; ):

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Пример снимка экрана диспетчера SDK по включению компонентов 5.0.1 для Android](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Пример снимка экрана диспетчера SDK для включения компонентов Android 4,4 и 5.0.1](installation-images/xs/sdk-select.png)

-----

## <a name="configuration"></a>Конфигурация

Прежде чем можно будет использовать тест приложения, необходимо настроить эмулятор износа Android или фактическое устройство "износ Android".

### <a name="android-wear-emulator"></a>Эмулятор износа Android

Перед использованием эмулятора износа Android необходимо настроить виртуальное устройство Android "износа Android" (AVD) с помощью **диспетчера эмуляторов Google**:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Запуск диспетчера Android Emulator из Visual Studio](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

![Запуск Android Emulator Manager из Visual Studio для Mac](installation-images/xs/emulator-menu.png)

-----

Дополнительные сведения о настройке эмулятора износа для Android см. [в разделе Отладка износа Android в эмуляторе](~/android/wear/deploy-test/debug-on-emulator.md).

### <a name="android-wear-device"></a>Устройство "износ Android"

Если вы используете устройство Android, например "износ Android", вы можете выполнить отладку приложения на этом устройстве вместо использования эмулятора. Сведения о разработке с помощью устройства "износ" см. [в разделе Отладка на устройстве с износом](~/android/wear/deploy-test/debug-on-device.md).

## <a name="create-your-first-android-wear-app"></a>Создание первого приложения "износ Android"

Следуйте инструкциям по [привету,](~/android/wear/get-started/hello-wear.md) чтобы создать свое первое контрольное приложение.

## <a name="packaging-your-app"></a>Упаковка приложения

Приложения "износ Android" всегда распространяются с помощью вспомогательного приложения для телефона Android.

При добавлении приложения "износ Android" в качестве ссылки на основное приложение Android оно автоматически считается проектом "износ Android" и будет создавать все необходимые XML-и метаданные. Кроме того, он проверяет соответствие номеров пакетов и версий, чтобы вы могли легко поставлять приложения в Google Play.

Дополнительные сведения о пакетировании приложений для износа см. в разделе [Работа с упаковкой](~/android/wear/deploy-test/packaging.md).

## <a name="related-links"></a>Связанные ссылки

- [Скелетонвеар (пример)](/samples/xamarin/monodroid-samples/wear-skeletonwear)