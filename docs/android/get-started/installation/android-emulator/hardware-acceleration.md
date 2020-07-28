---
title: Аппаратное ускорение для производительной работы эмулятора (Hyper-V и HAXM)
description: В этой статье рассказывается, как использовать функции аппаратного ускорения компьютера для максимизации производительности эмулятора Android Emulator.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: jondouglas
ms.author: jodou
ms.date: 02/13/2020
ms.openlocfilehash: 93336f5a741cf9643288163f2bf1d8ef44ea52d2
ms.sourcegitcommit: d42dadc6da584953865b1adb616489d91a7b25d0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/20/2020
ms.locfileid: "86473441"
---
# <a name="hardware-acceleration-for-emulator-performance-hyper-v--haxm"></a>Аппаратное ускорение для производительной работы эмулятора (Hyper-V и HAXM)

_В этой статье рассказывается, как использовать функции аппаратного ускорения компьютера для обеспечения максимальной производительности эмулятора Android Emulator._

Visual Studio упрощает тестирование и отладку приложений Xamarin.Android с помощью Android Emulator в ситуациях, когда использовать устройство с Android невозможно или неудобно.
Однако если на компьютере недоступно аппаратное ускорение, Android Emulator работает слишком медленно. Вы можете значительно повысить производительность эмулятора Android Emulator, используя специальные образы виртуальных устройств на платформе x86 в сочетании с функциями виртуализации на компьютере.

| Сценарий    | HAXM        | WHPX       | Hypervisor.Framework |
| ----------- | ----------- | -----------| ----------- |
| У вас процессор Intel | X | X | X |
| У вас процессор AMD   |   | X |   |
| Требуется поддержка Hyper-V |   | X |   |
| Требуется поддержка вложенной виртуализации |   | Ограниченный |   |
| Требуется использовать такие технологии, как Docker  |   | X | X |

::: zone pivot="windows"

## <a name="accelerating-android-emulators-on-windows"></a>Ускорение эмуляторов Android в Windows

Следующие технологии виртуализации доступны для ускорения эмулятора Android:

1. **Microsoft Hyper-V и платформа гипервизора Windows (WHPX)** .
   [Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/) — это функция виртуализации в Windows, которая позволяет запускать виртуализированные компьютерные системы на физическом главном компьютере.

2. **Intel Hardware Accelerated Execution Manager (HAXM)** .
   HAXM — это механизм виртуализации для компьютеров на базе процессоров Intel.

Для достижения наилучшего взаимодействия с пользователем в Windows рекомендуется использовать WHPX для ускорения Android Emulator. Если WHPX недоступен на вашем компьютере, можно использовать HAXM. Эмулятор Android автоматически использует аппаратное ускорение, если соблюдены следующие условия:

- Аппаратное ускорение доступно и включено на компьютере разработчика.

- Эмулятор работает с образом системы, созданным для виртуального устройства на платформе **x86**.

> [!IMPORTANT]
> Вы не можете запускать эмулятор на базе ускоренной виртуальной машины внутри другой виртуальной машины, например под управлением VirtualBox, VMWare или Docker. Эмулятор Android следует запускать [непосредственно на системном оборудовании](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Сведения о запуске и отладке с помощью эмулятора Android см. в статье [Отладка в Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

<a name="hyper-v-win"></a>

## <a name="accelerating-with-hyper-v"></a>Ускорение с помощью Hyper-V

Перед включением Hyper-V прочтите следующий раздел, чтобы убедиться, что компьютер поддерживает Hyper-V.

### <a name="verifying-support-for-hyper-v"></a>Проверка поддержки Hyper-V

Hyper-V работает на платформе гипервизора Windows. Чтобы использовать эмулятор Android с Hyper-V, компьютер должен отвечать следующим условиям для поддержки платформы гипервизора Windows:

- Оборудование компьютера должно соответствовать следующим требованиям:

  - 64-разрядный ЦП Intel или AMD Ryzen с преобразованием адресов второго уровня (SLAT).
  - ЦП должен поддерживать расширения режима мониторинга виртуальной машины (VT-c на процессорах Intel).
  - Не менее 4 ГБ памяти.

- В BIOS компьютера необходимо включить следующие элементы:

  - Технология виртуализации (может иметь другое название в зависимости от производителя системной платы).
  - Предотвращение исполнения данных на основе оборудования.

- Компьютер необходимо обновить до Windows 10, обновление за апрель 2018 г. (сборка 1803), или более поздней версии. Выполните следующие действия, чтобы проверить актуальность вашей версии Windows:

  1. В поле поиска Windows введите **Сведения**.
  2. В результатах поиска выберите **Сведения о компьютере**.
  3. Прокрутите диалоговое окно **Сведения** до раздела **Характеристики Windows**.
  4. Должна быть указана **версия** не ранее 1803:

      [![Спецификации Windows](hardware-acceleration-images/win/01-about-windows-w10-sml.png)](hardware-acceleration-images/win/01-about-windows-w10.png#lightbox)

Чтобы убедиться, что компьютерное оборудование и программное обеспечение совместимо с Hyper-V, откройте командную строку и введите следующую команду:

```cmd
systeminfo
```

Если все указанные требования Hyper-V имеют значение **Да**, компьютер поддерживает Hyper-V. Пример:

[![Пример выходных данных сведений о системе](hardware-acceleration-images/win/02-systeminfo-w158-sml.png)](hardware-acceleration-images/win/02-systeminfo-w158.png#lightbox)

### <a name="enabling-hyper-v-acceleration"></a>Включение ускорения Hyper-V

Если ваш компьютер соответствует приведенным выше критериям, выполните следующие действия для ускорения эмулятора Android с помощью Hyper-V.

1. Введите **компоненты windows** в поле поиска Windows и выберите **Включение и отключение компонентов Windows** в результатах поиска. В диалоговом окне **Компоненты Windows** включите **Hyper-V** и **платформу гипервизора Windows**:

    [![Включение Hyper-V и платформы гипервизора Windows](hardware-acceleration-images/win/03-hyper-v-settings-w158-sml.png)](hardware-acceleration-images/win/03-hyper-v-settings-w158.png#lightbox)

   После внесения этих изменений перезагрузите компьютер.
   
> [!IMPORTANT]
>
> В Windows 10 с обновлением за октябрь 2018 г. (RS5) и более поздних версий необходимо только включить Hyper-V, так как он будет автоматически использовать платформу гипервизора Windows (WHPX).

2. **Установите [Visual Studio 15.8 или более поздней версии](https://visualstudio.microsoft.com/vs/)** (в этой версии Visual Studio интегрированная среда разработки поддерживает запуск эмулятора Android с Hyper-V).

3. **Установите пакет Android Emulator версии 27.2.7 или более поздней**. Для установки пакета последовательно выберите **Сервис > Android > Диспетчер пакетов SDK для Android** в Visual Studio. Откройте вкладку **Инструменты** и убедитесь, что установлена версия эмулятора Android не ниже 27.2.7. Кроме того, убедитесь, что установлен компонент Android SDK Tools версии 26.1.1 или более поздней:

    [![Диалоговое окно "Пакеты SDK и инструменты для Android"](hardware-acceleration-images/win/04-sdk-manager-w158-sml.png)](hardware-acceleration-images/win/04-sdk-manager-w158.png#lightbox)

При создании виртуального устройства (см. раздел [Управление виртуальными устройствами с помощью Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)) не забудьте выбрать образ системы на базе **x86**. Если вы используете образ системы на основе ARM, виртуальное устройство не ускорится и будет работать медленно.

## <a name="accelerating-with-haxm"></a>Ускорение с помощью HAXM

Используйте HAXM для ускорения эмулятора Android, если компьютер не поддерживает Hyper-V. Отключите [отключить Device Guard](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vswin#disable-devguard), чтобы использовать HAXM.

### <a name="verifying-haxm-support"></a>Проверка поддержки HAXM

Чтобы определить, поддерживает ли оборудование HAXM, следуйте инструкциям из раздела [Мой процессор поддерживает технологию виртуализации Intel?](https://www.intel.com/content/www/us/en/support/processors/000005486.html).
Если оборудование поддерживает HAXM, проверьте, установлен ли HAXM:

1. Откройте окно командной строки и введите следующую информацию:

    ```cmd
    sc query intelhaxm
    ```

2. Посмотрите в выходных данных, запущен ли процесс HAXM. Если да, для состояния `intelhaxm` будет отображаться `RUNNING`. Пример:

    ![Выходные данные команды запроса sc, если HAXM доступен](hardware-acceleration-images/win/05-sc_query-w158.png)

   Если `STATE` не имеет значение `RUNNING`, HAXM не установлен.

Если компьютер поддерживает HAXM, но он не установлен, установите его, выполнив действия, приведенные в следующем разделе.

<a name="install-haxm-win"></a>

### <a name="installing-haxm"></a>Установка HAXM

Пакеты установки HAXM для Windows можно найти на странице выпусков GitHub, посвященной [Intel Hardware Accelerated Execution Manager](https://github.com/intel/haxm/releases). Чтобы скачать и установить решение HAXM, выполните следующие действия:

1. Скачайте с веб-сайта Intel последнюю версию установщика [подсистемы виртуализации HAXM](https://github.com/intel/haxm/releases) для ОС Windows. Скачивая установщик HAXM с веб-сайта Intel, вы гарантированно получаете последнюю версию этой программы.

2. Чтобы начать работу с установщиком HAXM, запустите файл **intelhaxm-android.exe**. Примите значения по умолчанию, предлагаемые в диалоговых окнах установщика:

   ![Окно программы установки Intel Hardware Accelerated Execution Manager](hardware-acceleration-images/win/06-haxm-installer.png)

При создании виртуального устройства (см. раздел [Управление виртуальными устройствами с помощью Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)) не забудьте выбрать образ системы на базе **x86**. Если вы используете образ системы на основе ARM, виртуальное устройство не ускорится и будет работать медленно.

## <a name="troubleshooting"></a>Устранение неполадок

Сведения о решении проблем с аппаратным ускорением см. в руководстве по [устранению неполадок](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vswin#accel-issues-win) для эмулятора Android.

::: zone-end
::: zone pivot="macos"

## <a name="accelerating-android-emulators-on-macos"></a>Ускорение эмуляторов Android в macOS

Следующие технологии виртуализации доступны для ускорения эмулятора Android:

1. **Платформа гипервизора Apple**.
   [Гипервизор](https://developer.apple.com/documentation/hypervisor) входит в состав macOS 10.10 и более поздних версий и позволяет запускать виртуальные машины на компьютере Mac.

2. **Intel Hardware Accelerated Execution Manager (HAXM)** .
   [HAXM](https://software.intel.com/articles/intel-hardware-accelerated-execution-manager-intel-haxm) — это механизм виртуализации для компьютеров на базе процессоров Intel.

Рекомендуется использовать платформу гипервизора для ускорения эмулятора Android. Если платформа гипервизора недоступна на компьютере Mac, можно использовать HAXM. Эмулятор Android автоматически использует аппаратное ускорение, если соблюдены следующие условия:

- Аппаратное ускорение доступно и включено на компьютере разработчика.

- Эмулятор работает с образом системы, созданным для виртуального устройства на платформе **x86**.

> [!IMPORTANT]
>
> Вы не можете запускать эмулятор на базе ускоренной виртуальной машины внутри другой виртуальной машины, например под управлением VirtualBox, VMWare или Docker. Эмулятор Android следует запускать [непосредственно на системном оборудовании](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Сведения о запуске и отладке с помощью эмулятора Android см. в статье [Отладка в Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

<a name="hypervisor"></a>

## <a name="accelerating-with-the-hypervisor-framework"></a>Ускорение с помощью платформы гипервизора

Для использования эмулятора Android с платформой гипервизора компьютер Mac должен соответствовать следующим критериям:

- Mac должен работать под управлением macOS 10.10 или более поздней версии.

- ЦП компьютера Mac должен поддерживать платформу гипервизора.

Если компьютер Mac соответствует этим критериям, Android Emulator будет автоматически использовать платформу гипервизора для ускорения. Если вы не уверены, поддерживает ли Mac платформу гипервизора, см. руководство [Устранение неполадок](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vsmac#hypervisor-issues), чтобы проверить это.

Если платформа гипервизора не поддерживается на компьютере Mac, используйте решение HAXM для ускорения эмулятора Android (описывается далее).

<a name="haxm-mac"></a>

## <a name="accelerating-with-haxm"></a>Ускорение с помощью HAXM

Если компьютер Mac не поддерживает платформу гипервизора (или ваша версия macOS ниже 10.10), используйте **Intel Hardware Accelerated Execution Manager** ([HAXM](https://software.intel.com/articles/intel-hardware-accelerated-execution-manager-intel-haxm)) для ускорения эмулятора Android.

Прежде чем использовать эмулятор Android с HAXM в первый раз, рекомендуется проверить наличие установленного решения HAXM и его доступность для эмулятора Android.

### <a name="verifying-haxm-support"></a>Проверка поддержки HAXM

Проверьте, установлен ли HAXM:

1. Откройте терминал и введите следующую команду:

    ```bash
    ~/Library/Developer/Xamarin/android-sdk-macosx/tools/emulator -accel-check
    ```

   Эта команда предполагает, что пакет SDK для Android установлен в расположении по умолчанию **~/Library/Developer/Xamarin/android-sdk-macosx**; в противном случае измените этот путь для расположения пакета SDK для Android на Mac.

2. Если решение HAXM установлено, приведенная выше команда вернет подобный результат:

    ```bash
    HAXM version 7.2.0 (3) is installed and usable.
    ```

   Если решение HAXM *не* установлено, возвращается следующее сообщение:

    ```bash
    HAXM is not installed on this machine (/dev/HAX is missing).
    ```

Если решение HAXM не установлено, установите его, выполнив действия, приведенные в следующем разделе.

<a name="install-haxm-mac"></a>

### <a name="installing-haxm"></a>Установка HAXM

Пакеты установки HAXM для macOS можно найти на странице [Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager). Чтобы скачать и установить решение HAXM, выполните следующие действия:

1. Скачайте с веб-сайта Intel последнюю версию установщика [подсистемы виртуализации HAXM](https://software.intel.com/android/articles/intel-hardware-accelerated-execution-manager/) для ОС macOS.

2. Запустите установщик HAXM. Примите значения по умолчанию, предлагаемые в диалоговых окнах установщика:

   [![Окно программы установки Intel Hardware Accelerated Execution Manager](hardware-acceleration-images/mac/01-haxm-installer-sml.png)](hardware-acceleration-images/mac/01-haxm-installer.png#lightbox)

## <a name="troubleshooting"></a>Устранение неполадок

Сведения о решении проблем с аппаратным ускорением см. в руководстве по [устранению неполадок](~/android/get-started/installation/android-emulator/troubleshooting.md?tabs=vsmac#accel-issues-mac) для эмулятора Android.

::: zone-end

## <a name="related-links"></a>Связанные ссылки

- [Запуск приложений в эмуляторе Android](https://developer.android.com/studio/run/emulator)
