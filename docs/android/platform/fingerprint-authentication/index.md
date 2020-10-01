---
title: Проверка подлинности по отпечаткам
description: В этом руководстве описано добавление проверки подлинности по отпечаткам, представленную в Android 6.0, в приложение Xamarin.Android.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: a3fd03ece3618172e68539e2a92e8924e85e2ce5
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453887"
---
# <a name="fingerprint-authentication"></a>Проверка подлинности по отпечаткам

_В этом руководстве описано добавление проверки подлинности по отпечаткам, представленную в Android 6.0, в приложение Xamarin.Android._

## <a name="fingerprint-authentication-overview"></a>Обзор проверки подлинности по отпечаткам

Появление сканеров отпечатков на устройствах Android предоставляет приложению альтернативу традиционному методу проверки подлинности пользователя по имени пользователя или паролю. Использование отпечатков для проверки подлинности пользователя позволяет включить в структуру приложения безопасность, которая является менее навязчивой, чем имя пользователя и пароль.

Целевые устройства с API FingerprintManager и сканером отпечатков работают с API уровня 23 (Android 6.0) или выше. API находятся в пространстве имен `Android.Hardware.Fingerprints`. В библиотеке поддержки Android версии 4 предусмотрены версии API отпечатков, предназначенные для старых версий Android. API совместимости, доступные в пространстве имен `Android.Support.v4.Hardware.Fingerprint`, распространяются с помощью [пакета NuGet Xamarin.Android.Support.v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).

[FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (а также его аналог библиотеки поддержки [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) является основным классом для использования оборудования сканирования отпечатков. Этот класс является программой-оболочкой пакета SDK для Android для службы уровня системы, которая управляет взаимодействием с самим оборудованием. Он отвечает за запуск сканера отпечатков и реагирование на отзыв от сканера. Этот класс имеет довольно простой интерфейс с тремя членами.

- **`Authenticate`** &ndash; этот метод инициализирует сканер оборудования и запускает службу в фоновом режиме, ожидая сканирования отпечатка пользователя.
- **`EnrolledFingerprints`** &ndash; это свойство возвратит `true`, если для устройства пользователь зарегистрировал один или несколько отпечатков.
- **`HardwareDetected`** &ndash; это свойство используется для определения того, поддерживает ли устройство сканирование отпечатков.

Для запуска сканера отпечатков приложение Android использует метод `FingerprintManager.Authenticate`. В следующем фрагменте кода приведен пример его вызова с помощью API совместимости библиотеки поддержки:

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

В этом руководстве объясняется использование API `FingerprintManager` для улучшения приложения Android с проверкой подлинности по отпечаткам. В нем рассматривается создание экземпляров и `CryptoObject` для защиты результатов сканера отпечатков. Мы рассмотрим, как приложение должно создать подкласс `FingerprintManager.AuthenticationCallback` и реагировать на отзывы от сканера отпечатков. Наконец, мы узнаем, как зарегистрировать отпечаток на устройстве или эмуляторе Android и как использовать **adb** для имитации сканирования отпечатков.

## <a name="requirements"></a>Requirements (Требования)

Для проверки подлинности по отпечаткам требуется Android 6.0 (API уровня 23) или более поздней версии и устройство со сканером отпечатков. 

Отпечаток пальца следует зарегистрировать на устройстве для каждого пользователя, который должен пройти проверку подлинности. Это включает в себя настройку блокировки экрана, которая использует пароль, ПИН-код, схему прокрутки или распознавание лиц. Некоторые функции проверки подлинности по отпечаткам можно имитировать в Android Emulator.  Дополнительные сведения об этих двух разделах см. в статье [Регистрация отпечатка пальца](enrolling-fingerprint.md). 

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Android. Пример отпечатка пальца](/samples/xamarin/monodroid-samples/fingerprintguide)
- [Xamarin.Android. Пример диалогового окна отпечатка](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [Запрос прав доступа](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](xref:Android.Content.Context)
- [API для отпечатков и платежей (видео)](https://youtu.be/VOn7VrTRlA4)