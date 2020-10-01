---
title: Устранение неполадок в Android Emulator
description: В этой статье рассказывается, как диагностировать и устранять неполадки, которые могут возникать при использовании Android Emulator.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/27/2018
ms.openlocfilehash: 34416145fcf66f28d80a35a7d4dbc9b2fc2c6351
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454485"
---
# <a name="android-emulator-troubleshooting"></a>Устранение неполадок в Android Emulator

_В этой статье описаны наиболее типичные ошибки и предупреждения, возникающие при настройке и запуске Android Emulator. Кроме того, описываются решения для этих ошибок, а также различные советы по устранению неполадок, которые помогут вам диагностировать проблемы с эмулятором._

::: zone pivot="windows"

## <a name="deployment-issues-on-windows"></a>Проблемы развертывания в Windows

Эмулятор может отображать некоторые сообщения об ошибках при развертывании приложения. Здесь описаны наиболее распространенные ошибки и решения.

### <a name="deployment-errors"></a>Ошибки развертывания

Если отображается ошибка о сбое при установке APK на ваш компьютер или при запуске Android Debug Bridge (**adb**), убедитесь, что пакет SDK для Android может подключиться к эмулятору. Для проверки связи с эмулятором сделайте следующее:

1. Запустите эмулятор из **Android Device Manager** (выберите ваше виртуальное устройство и зажмите **Запустить**).

2. Откройте командную строку и перейдите в папку, в которой установлен **adb**. Если пакет SDK для Android установлен в расположении по умолчанию, **adb** находится в каталоге **C:\\Program Files (x86)\\Android\\android-sdk\\platform-tools\\adb.exe**; в противном случае измените этот путь для расположения пакета SDK для Android на вашем компьютере.

3. Введите следующую команду:

   ```shell
   adb devices
   ```

4. Если эмулятор доступен из Android SDK, то он отобразится в списке подключенных устройств. Пример:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Если эмулятор не появился в этом списке, запустите **Диспетчер пакетов SDK для Android**, примените все обновления и запустите эмулятор еще раз.

### <a name="mmio-access-error"></a>Ошибка доступа к MMIO

Если появляется сообщение **Произошла ошибка доступа к MMIO**, перезапустите эмулятор.

<a name="gps-win"></a>

## <a name="missing-google-play-services"></a>Отсутствующие сервисы Google Play

Если виртуальное устройство, запущенное в эмуляторе, не поддерживает установленные сервисы Google Play или Google Play Маркет, это часто связано с тем, что виртуальное устройство было создано без этих пакетов. При создании виртуального устройства (см. раздел [Управление виртуальными устройствами с помощью Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)) не забудьте выбрать один или оба из следующих вариантов:

- **API Google** &ndash; включает Сервисы Google Play в виртуальное устройство.
- **Google Play Маркет** &ndash; включает Google Play Маркет в виртуальное устройство.

Например, это виртуальное устройство будет включать сервисы Google Play и Google Play Маркет:

[![Пример AVD с включенными сервисами Google Play и Google Play Маркет](troubleshooting-images/win/00-add-gps-w158-sml.png)](troubleshooting-images/win/00-add-gps-w158.png#lightbox)

> [!NOTE]
> Образы Google Play Маркет доступны только для некоторых типов базовых устройств, например Pixel, Pixel 2, Nexus 5 и Nexus 5X.

<a name="perf-win"></a>

## <a name="performance-issues"></a>Проблемы производительности

Проблемы производительности обычно вызваны одной из следующих проблем:

- Эмулятор работает без аппаратного ускорения.

- Виртуальное устройство, работающее в эмуляторе, не использует образ системы на базе x86.

В следующих разделах подробно описаны эти сценарии.

### <a name="hardware-acceleration-is-not-enabled"></a>Аппаратное ускорение отключено

Если не включено аппаратное ускорение, при запуске виртуального устройства из Device Manager будет отображаться диалоговое окно с сообщением об ошибке, указывающее, что платформа гипервизора Windows (WHPX) настроена неправильно:

![Пример предупреждения Device Manager](troubleshooting-images/win/01-dev-mgr-warning-w158.png)

Если это сообщение об ошибке отображается, см. инструкции по проверке и включению аппаратного ускорения в разделе [Проблемы с аппаратным ускорением](#accel-issues-win).

### <a name="acceleration-is-enabled-but-the-emulator-runs-too-slowly"></a>Аппаратное ускорение включено, но эмулятор работает слишком медленно 

Распространенной причиной этой проблемы является отсутствие образа на базе x86 в виртуальном устройстве (AVD). При создании виртуального устройства (см. раздел [Управление виртуальными устройствами с помощью Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)) не забудьте выбрать образ системы на базе x86:

[![Выбор образа системы на базе x86 для виртуального устройства](troubleshooting-images/win/02-x86-virtual-device-w158-sml.png)](troubleshooting-images/win/02-x86-virtual-device-w158.png#lightbox)

<a name="accel-issues-win"></a>

## <a name="hardware-acceleration-issues"></a>Проблемы с аппаратным ускорением

При использовании Hyper-V или HAXM для аппаратного ускорения вы можете столкнуться с проблемами конфигурации или конфликтами с другим программным обеспечением на компьютере. Проверьте, включено ли аппаратное ускорение (и какой метод ускорения используется в эмуляторе), введя следующую команду в командной строке:

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator-check.exe" accel
```

Эта команда предполагает, что пакет SDK для Android установлен в расположении по умолчанию **C:\\Program Files (x86)\\Android\\android-sdk**; в противном случае измените этот путь для расположения пакета SDK для Android на вашем компьютере.

### <a name="hardware-acceleration-not-available"></a>Аппаратное ускорение недоступно

Если Hyper-V доступен, ответом на команду **emulator-check.exe accel** будет следующее сообщение:

```cmd
HAXM is not installed, but Windows Hypervisor Platform is available.
```

Если HAXM доступен, выводится подобное сообщение:

```cmd
HAXM version 6.2.1 (4) is installed and usable.
```

Если аппаратное ускорение недоступно, будет отображаться следующее сообщение (эмулятор ищет HAXM, если не удается найти Hyper-V):

```cmd
HAXM is not installed on this machine
```

Если аппаратное ускорение недоступно, инструкции по его включению на компьютере см. в разделе [Ускорение с помощью Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vswin#hyper-v-win).

### <a name="incorrect-bios-settings"></a>Неправильные настройки BIOS

Если BIOS настроен неправильно и не поддерживает аппаратное ускорение, при выполнении команды **emulator-check.exe accel** будет отображаться подобное сообщение:

```cmd
VT feature disabled in BIOS/UEFI
```

Чтобы устранить эту проблему, перезагрузите BIOS на компьютере и включите следующие параметры:

- Технология виртуализации (может иметь другое название в зависимости от производителя системной платы).
- Предотвращение исполнения данных на основе оборудования.

Если аппаратное ускорение включено и BIOS настроен правильно, эмулятор должен успешно работать с аппаратным ускорением.
Однако с Hyper-V и HAXM все равно могут возникать проблемы, как описано далее.

### <a name="hyper-v-issues"></a>Проблемы с Hyper-V

В некоторых случаях при включении одновременно **Hyper-V** и **платформы гипервизора Windows** в диалоговом окне **Включение и отключение компонентов Windows** Hyper-V включается некорректно. Чтобы убедиться, что Hyper-V включен, сделайте следующее:

1. Введите **powershell** в поле поиска Windows.

2. Щелкните правой кнопкой мыши **Windows PowerShell** в результатах поиска и выберите **Запуск от имени администратора**.

3. В консоли PowerShell выполните следующую команду:

    ```powershell
    Get-WindowsOptionalFeature -FeatureName Microsoft-Hyper-V-All -Online
    ```

    Если Hyper-V не включен, будет отображаться сообщение, как в следующем примере, указывающее, что Hyper-V **отключен**:

    ```
    FeatureName      : Microsoft-Hyper-V-All
    DisplayName      : Hyper-V
    Description      : Provides services and management tools for creating and running virtual machines and their resources.
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

4. В консоли PowerShell выполните следующую команду:

    ```powershell
    Get-WindowsOptionalFeature -FeatureName HypervisorPlatform -Online
    ```

    Если гипервизор не включен, будет отображаться сообщение, как в следующем примере, указывающее, что платформа гипервизора **отключена**:

    ```
    FeatureName      : HypervisorPlatform
    DisplayName      : Windows Hypervisor Platform
    Description      : Enables virtualization software to run on the Windows hypervisor
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

Если Hyper-V и (или) платформа гипервизора не включены, используйте следующие команды PowerShell для их включения:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Enable-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform -All
```

После завершения этих команд выполните перезагрузку. 

Дополнительные сведения о включении Hyper-V (в том числе методы включения Hyper-V с помощью средства обслуживания и администрирования образов развертывания) см. в разделе [Установка Hyper-V](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

### <a name="haxm-issues"></a>Проблемы с HAXM

Проблемы с HAXM часто вызваны конфликтом с другими технологиями виртуализации, неправильной конфигурацией или устаревшим драйвером HAXM.

### <a name="haxm-process-is-not-running"></a>Процесс HAXM не запущен

Если решение HAXM установлено, введите следующую команду в командной строке, чтобы проверить, что процесс HAXM запущен:

```cmd
sc query intelhaxm
```

Если процесс HAXM запущен, вы увидите примерно следующий результат:

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```

Если параметр `STATE` не равен `RUNNING`, то для решения этой проблемы см. раздел [Использование Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

<a name="virt-conflicts"></a>

#### <a name="haxm-virtualization-conflicts"></a>Конфликты HAXM с другими технологиями виртуализации

HAXM может конфликтовать с другими технологиями, использующими виртуализацию, такими как Hyper-V, Windows Device Guard и некоторые антивирусы:

- **Hyper-V** &ndash; если вы используете версию Windows младше **версии Windows 10 с обновлением за апрель 2018 г. (сборка 1803)** , а Hyper-V включен, см. раздел [Отключение Hyper-V](#disable-hyperv), чтобы включить HAXM.

- **Device Guard** &ndash; Device Guard и Credential Guard могут препятствовать отключению Hyper-V на компьютерах с Windows. Порядок отключения Device Guard и Credential Guard см. в разделе [Отключение Device Guard](#disable-devguard).

- **Антивирусное ПО** &ndash; если на вашем компьютере запущенно антивирусное ПО, использующее аппаратную виртуализацию (например, Avast), отключите или удалите его, перезагрузите компьютер и снова запустите эмулятор Android.

#### <a name="incorrect-bios-settings"></a>Неправильные настройки BIOS

Если вы используете HAXM на компьютере с Windows, HAXM не заработает, пока технология виртуализации (Intel VT-x) не будет включена в BIOS. Если VT-x отключен, то при попытке запуска Android Emulator вы получите следующую ошибку:

**Компьютер удовлетворяет требованиям для запуска HAXM, но технология виртуализации Intel (VT-x) отключена.**

Для исправления этой ошибки перезагрузите компьютер в BIOS, включите VT-x и SLAT (трансляция адресов второго уровня) и перезагрузите компьютер обратно в Windows.

<a name="disable-hyperv"></a>

#### <a name="disabling-hyper-v"></a>Отключение Hyper-V

Если вы используете версию Windows младше **версии Windows 10 с обновлением за апрель 2018 г. (сборка 1803)** , а Hyper-V включен, нужно отключить Hyper-V и перезапустить компьютер для установки и использования HAXM. Если вы используете **версию Windows 10 с обновлением за апрель 2018 г. (сборка 1803)** или более позднюю, Android Emulator версии 27.2.7 или более поздней может использовать Hyper-V (вместо HAXM) для аппаратного ускорения, поэтому отключать Hyper-V не требуется.

Технологию Hyper-V можно отключить из панели управления, выполнив следующие действия:

1. Введите **компоненты windows** в поле поиска Windows и выберите **Включение и отключение компонентов Windows** в результатах поиска.

2. Снимите флажок **Hyper-V**:

    ![Отключение технологии Hyper-V в диалоговом окне "Компоненты Windows"](troubleshooting-images/win/03-uncheck-hyper-v.png)

3. Перезагрузите компьютер.

Также для отключения гипервизора Hyper-V можно использовать следующую команду Powershell:

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM и Microsoft Hyper-V не могут быть активны одновременно. К сожалению, переключение между технологиями Hyper-V и HAXM без перезагрузки компьютера невозможно. 

В некоторых случаях выполнение описываемых выше действий не позволяет отключить технологию Hyper-V, если включены функции Device Guard и Credential Guard. Если вам не удается отключить Hyper-V (или по всем признакам эта технология отключена, но установка HAXM все равно завершается сбоем), отключите функции Device Guard и Credential Guard, выполнив действия, описываемые в следующем разделе.

<a name="disable-devguard"></a>

#### <a name="disabling-device-guard"></a>Отключение функции Device Guard

Функции Device Guard и Credential Guard могут препятствовать отключению технологии Hyper-V на компьютерах под управлением ОС Windows. Это часто происходит на компьютерах, присоединенных к доменам, которые настраиваются и контролируются управляющей ими организацией. Чтобы проверить, выполняется ли функция **Device Guard** в ОС Windows 10, выполните следующие действия:

1. Введите **Сведения о системе** в поле поиска Windows и выберите **Сведения о системе** в результатах поиска.

2. В разделе **Сведения о системе** проверьте наличие службы **Безопасность на основе виртуализации Device Guard** и убедитесь, что она имеет состояние **Выполняется**:

   [![Функция Device Guard установлена и выполняется](troubleshooting-images/win/04-device-guard-sml.png)](troubleshooting-images/win/04-device-guard.png#lightbox)

Если функция Device Guard включена, выполните следующие действия для ее отключения:

1. Убедитесь, что технология **Hyper-V** отключена в окне **Включение или отключение компонентов Windows**, как описывается в предыдущих разделах.

2. В поле поиска Windows введите **gpedit.msc** и выберите элемент **Изменение групповой политики**. Будет запущен **редактор локальных групповых политик**.

3. В **редакторе локальных групповых политик** выберите **Конфигурация компьютера > Административные шаблоны > Система > Device Guard**:

   [![Функция Device Guard в редакторе локальных групповых политик](troubleshooting-images/win/05-group-policy-editor-sml.png)](troubleshooting-images/win/05-group-policy-editor.png#lightbox)

4. Измените значение параметра **Включить средство обеспечения безопасности на основе виртуализации** на **Отключено** (как показано выше) и закройте **редактор локальных групповых политик**.

5. В поле поиска Windows введите **cmd**. Когда в результатах поиска появится элемент **Командная строка**, щелкните пункт **Командная строка** правой кнопкой мыши и выберите **Запустить от имени администратора**.

6. Скопируйте и вставьте следующие команды в окно командной строки (если диск **Z:** используется, выберите вместо него букву свободного диска):

    ```cmd
    mountvol Z: /s
    copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
    bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
    bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
    mountvol Z: /d
    ```

7. Перезапустите компьютер. На экране загрузки должен появиться запрос следующего вида:

   **Вы действительно хотите отключить Credential Guard?**

   Нажмите указанную в запросе клавишу, чтобы отключить Credential Guard.

8. После перезагрузки компьютера еще раз убедитесь, что технология Hyper-V отключена (см. ранее описываемые действия).

Если технология Hyper-V по-прежнему не отключена, значит, отключение функций Device Guard или Credential Guard запрещено политиками на вашем присоединенном к домену компьютере. В таком случае вы можете запросить у администратора домена исключение, которое позволит отключить Credential Guard. Кроме того, вы можете использовать для работы с HAXM компьютер, который не присоединен к домену.

## <a name="additional-troubleshooting-tips"></a>Дополнительные советы по устранению неполадок

Следующие рекомендации часто помогают диагностировать проблемы с эмулятором Android.

### <a name="starting-the-emulator-from-the-command-line"></a>Запуск эмулятора из командной строки

Если эмулятор еще не запущен, вы можете запустить его из командной строки (а не из среды Visual Studio) для просмотра его выходных данных. Как правило, образы AVD эмулятора Android хранятся в следующем расположении (замените *имя_пользователя* своим именем пользователя Windows):

**C:\\Users\\*имя_пользователя*\\.android\\avd**

Запустите эмулятор с образом AVD из этого расположения, передав имя папки AVD. Например, эта команда запускает виртуальное устройство Android с именем **Pixel_API_27**:

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator.exe" -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_API_27 -prop monodroid.avdname=Pixel_API_27
```

В этом примере предполагается, что пакет SDK для Android установлен в расположении по умолчанию **C:\\Program Files (x86)\\Android\\android-sdk**; в противном случае измените этот путь для расположения пакета SDK для Android на вашем компьютере.

При выполнении этой команды вы получите много строк выходных данных во время запуска эмулятора. В частности, вы увидите строки, как в следующем примере, если аппаратное ускорение включено и работает должным образом (в этом примере для аппаратного ускорения используется HAXM):

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: HAXM version 6.2.1 (4) is installed and usable.
```

### <a name="viewing-device-manager-logs"></a>Просмотр журналов Device Manager

Часто можно диагностировать проблемы с эмулятором, просмотрев журналы Device Manager. Эти журналы записываются в следующее расположение:

**C:\\Users\\*имя_пользователя*\\AppData\\Roaming\\XamarinDeviceManager**

Просмотрите файл **DeviceManager.log** в текстовом редакторе, например в Блокноте. Следующий пример записи журнала указывает, что HAXM не найден на компьютере:

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```

::: zone-end
::: zone pivot="macos"

## <a name="deployment-issues-on-macos"></a>Проблемы развертывания в macOS

Эмулятор может отображать некоторые сообщения об ошибках при развертывании приложения. Ниже описаны наиболее распространенные ошибки и решения.

### <a name="deployment-errors"></a>Ошибки развертывания

Если отображается ошибка о сбое при установке APK на ваш компьютер или при запуске Android Debug Bridge (**adb**), убедитесь, что пакет SDK для Android может подключиться к эмулятору. Для проверки связи сделайте следующее:

1. Запустите эмулятор из **Android Device Manager** (выберите ваше виртуальное устройство и зажмите **Запустить**).

2. Откройте командную строку и перейдите в папку, в которой установлен **adb**. Если пакет SDK для Android установлен в расположении по умолчанию, **adb** находится в каталоге **~/Library/Developer/Xamarin/android-sdk-macosx/platform-tools/adb**; в противном случае измените этот путь для расположения пакета SDK для Android на вашем компьютере.

3. Введите следующую команду:

   ```shell
   adb devices
   ```

4. Если эмулятор доступен из Android SDK, то он отобразится в списке подключенных устройств. Пример:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Если эмулятор не появился в этом списке, запустите **Диспетчер пакетов SDK для Android**, примените все обновления и запустите эмулятор еще раз.

### <a name="mmio-access-error"></a>Ошибка доступа к MMIO

Если появляется сообщение **Произошла ошибка доступа к MMIO**, перезапустите эмулятор.

<a name="gps-mac"></a>

## <a name="missing-google-play-services"></a>Отсутствующие сервисы Google Play

Если виртуальное устройство, запущенное в эмуляторе, не поддерживает установленные сервисы Google Play или Google Play Маркет, это обычно связано с тем, что виртуальное устройство было создано без этих пакетов. При создании виртуального устройства (см. раздел [Управление виртуальными устройствами с помощью Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)) не забудьте выбрать один или оба из следующих вариантов:

- **API Google** &ndash; включает Сервисы Google Play в виртуальное устройство.
- **Google Play Маркет** &ndash; включает Google Play Маркет в виртуальное устройство.

Например, это виртуальное устройство будет включать сервисы Google Play и Google Play Маркет:

[![Пример AVD с включенными сервисами Google Play и Google Play Маркет](troubleshooting-images/mac/01-google-play-services-m75-sml.png)](troubleshooting-images/mac/01-google-play-services-m75.png#lightbox)

> [!NOTE]
> Образы Google Play Маркет доступны только для некоторых типов базовых устройств, например Pixel, Pixel 2, Nexus 5 и Nexus 5X.

<a name="perf-mac"></a>

## <a name="performance-issues"></a>Проблемы производительности

Проблемы производительности обычно вызваны одной из следующих проблем:

- Эмулятор работает без аппаратного ускорения.

- Виртуальное устройство, работающее в эмуляторе, не использует образ системы на базе x86.

В следующих разделах подробно описаны эти сценарии.

### <a name="hardware-acceleration-is-not-enabled"></a>Аппаратное ускорение отключено

Если аппаратное ускорение не включено, может появиться диалоговое окно с сообщением о том, что **устройство будет запущено без ускорения**, при развертывании приложения в эмуляторе Android. Если вы не знаете, включено ли аппаратное ускорение на компьютере (или хотите знать, какая технология обеспечивает ускорение), см. инструкции по проверке и включению аппаратного ускорения в разделе [Проблемы с аппаратным ускорением](#accel-issues-mac).

### <a name="acceleration-is-enabled-but-the-emulator-runs-too-slowly"></a>Аппаратное ускорение включено, но эмулятор работает слишком медленно 

Распространенной причиной этой проблемы является отсутствие образа на базе x86 в виртуальном устройстве. При создании виртуального устройства (см. раздел [Управление виртуальными устройствами с помощью Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)) не забудьте выбрать образ системы на базе x86:

[![Выбор образа системы на базе x86 для виртуального устройства](troubleshooting-images/mac/02-x86-virtual-device-m75-sml.png)](troubleshooting-images/mac/02-x86-virtual-device-m75.png#lightbox)

<a name="accel-issues-mac"></a>

## <a name="hardware-acceleration-issues"></a>Проблемы с аппаратным ускорением

При использовании платформы гипервизора или HAXM для аппаратного ускорения эмулятора вы можете столкнуться с проблемами, вызванными неправильной установкой или устаревшей версией macOS. Следующие разделы помогут вам устранить эту проблему.

<a name="hypervisor-issues"></a>

### <a name="hypervisor-framework-issues"></a>Проблемы с платформой гипервизора

Если вы используете macOS 10.10 или более поздней версии на более новом компьютере Mac, эмулятор Android будет автоматически использовать платформу гипервизора для аппаратного ускорения. Но на более старых компьютерах Mac или компьютерах Mac под управлением версии macOS ранее 10.10 платформа гипервизора может не поддерживаться.

Чтобы определить, поддерживает ли компьютер Mac платформу гипервизора,откройте окно терминала и введите следующую команду:

```bash
sysctl kern.hv_support
```

Если компьютер Mac поддерживает платформу гипервизора, эта команда вернет следующий результат:

```bash
kern.hv_support: 1
```

Если платформа гипервизора недоступна на компьютере Mac, следуйте инструкциям в разделе [Ускорение с помощью HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#haxm-mac), чтобы вместо этого использовать HAXM для ускорения.

### <a name="haxm-issues"></a>Проблемы с HAXM

Если Android Emulator не запускается, обычно это вызвано проблемами с HAXM. Проблемы с HAXM часто вызваны конфликтом с другими технологиями виртуализации, неправильной конфигурацией или устаревшим драйвером HAXM. Переустановите драйвер HAXM при помощи процедуры, описанной в разделе [Установка HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#install-haxm-mac).

## <a name="additional-troubleshooting-tips"></a>Дополнительные советы по устранению неполадок

Следующие рекомендации часто помогают диагностировать проблемы с эмулятором Android.

### <a name="starting-the-emulator-from-the-command-line"></a>Запуск эмулятора из командной строки

Если эмулятор еще не запущен, вы можете запустить его из командной строки (а не из среды Visual Studio для Mac) для просмотра его выходных данных. Как правило, образы AVD эмулятора Android хранятся в следующем расположении:

**~/.android/avd**

Запустите эмулятор с образом AVD из этого расположения, передав имя папки AVD. Например, эта команда запускает виртуальное устройство Android с именем **Pixel_2_API_28**:

```cmd
~/Library/Developer/Xamarin/android-sdk-macosx/emulator/emulator -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_2_API_28 -prop monodroid.avdname=Pixel_2_API_28
```

Если пакет SDK для Android установлен в расположении по умолчанию, эмулятор находится в каталоге **~/Library/Developer/Xamarin/android-sdk-macosx/emulator**; в противном случае измените этот путь для расположения пакета SDK для Android на вашем компьютере Mac.

При выполнении этой команды вы получите много строк выходных данных во время запуска эмулятора. В частности, вы увидите строки, как в следующем примере, если аппаратное ускорение включено и работает должным образом (в этом примере для аппаратного ускорения используется платформа гипервизора):

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: Hypervisor.Framework OS X Version 10.13
```

### <a name="viewing-device-manager-logs"></a>Просмотр журналов Device Manager

Часто можно диагностировать проблемы с эмулятором, просмотрев журналы Device Manager. Эти журналы записываются в следующее расположение:

**~/Library/Logs/XamarinDeviceManager**

Просмотрите файл **Android Devices.log**, дважды щелкнув его, чтобы открыть его в приложении консоли. Следующий пример записи журнала указывает, что HAXM не найден:

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```

::: zone-end