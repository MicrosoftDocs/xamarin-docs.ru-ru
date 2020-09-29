---
title: Где я могу найти информацию о версии и журналы
description: В этом документе описывается, где искать сведения о версии Xamarin и журналах. Эти сведения полезны при диагностике проблем, отправке ошибок или получении поддержки.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 997c6398c4cd9c4f4be6fbcd60847d82b0cae13d
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457839"
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Где я могу найти информацию о версии и журналы

## <a name="outline"></a>Контур

- [Сведения о версии](#version-information)
  - Сведения о версии Windows
  - Сведения о версии Mac
  - Android SDK Tools, Platform-Tools, Build-Tools
- [Журналы IDE и установщика](#ide-and-installer-logs)
  - [Журналы Windows](#windows-logs)
    - Xamarin Studio
    - Xamarin для Visual Studio
    - Универсальный установщик Xamarin
    - Отдельные `.msi` установщики, подробные журналы
    - Запуск Visual Studio, подробные журналы
  - [Журналы Mac](#mac-logs)
    - Узел сборки
  - Visual Studio для Mac
    - Xamarin Studio
    - Установщик Xamarin
- [Подробные выходные данные сборки](#verbose-build-output-logs)
- [Журналы отладки для приложений Xamarin. Android и Xamarin. iOS](#debug-logs-for-xamarin-apps)
  - `adb`Журналы logcat для Android
  - журналы симуляторов iOS (на Mac)
  - журналы устройств iOS (на Mac)

## <a name="version-information"></a><a id="version-information" name="version-information" />Сведения о версии

Обычно лучше всего отправить обратно все сведения из кнопок **Копировать информацию** . В противном случае часто требуется запросить дополнительную информацию. Например, при устранении неполадок могут быть важны версии операционной системы, версия Xcode, установленные уровни API Android и версия .NET.

### <a name="windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Сведения о версии Windows

#### <a name="xamarin-studio"></a>Xamarin Studio

**Справка > о > показывать сведения > копирование сведений [кнопка]**

#### <a name="visual-studio"></a>Visual Studio

**Справка > о Microsoft Visual Studio > копировать сведения [кнопка]**

### <a name="mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Сведения о версии Mac

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Visual Studio > о Visual Studio > отображение подробных сведений > копирование информации [кнопка]**

### <a name="android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK Tools, Platform-Tools, Build-Tools

Откройте диспетчер пакет SDK для Android и Создайте снимок экрана с разделом "лучшие **средства** ".

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Средства > открыть пакет SDK для Android Manager**

#### <a name="visual-studio"></a>Visual Studio

**Средства > Android > открыть диспетчер пакет SDK для Android...**

## <a name="ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />Журналы IDE и установщика

Для каждого расположения журнала обязательно заархивируйте и прикрепите всю папку журнала.

### <a name="windows-logs"></a><a id="windows-logs" name="windows-logs" />Журналы Windows

#### <a name="visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Инструменты Visual Studio для Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Как получить журналы установки Visual Studio](/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Универсальный установщик Xamarin

`%LOCALAPPDATA%\Xamarin\Universal`

Это журналы из `XamarinInstaller.exe` установщика.

#### <a name="individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Отдельные `.msi` установщики, подробные журналы

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Ссылка: [Параметры командной строки](/windows/win32/msi/command-line-options)

#### <a name="visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Запуск Visual Studio, подробные журналы

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Ссылка: [/log (devenv.exe)](/visualstudio/ide/reference/log-devenv-exe)

### <a name="mac-logs"></a><a id="mac-logs" name="mac-logs" />Журналы Mac

Можно выбрать пункт меню **переход > переход к папке** в Finder, а затем скопировать и вставить любой из этих путей в диалоговое окно.

#### <a name="visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio для Mac

`~/Library/Logs/VisualStudio/7.0` (это значение может изменяться в зависимости от используемой версии).

Эту папку также можно открыть с помощью "Help-> открыть каталог журналов".

#### <a name="xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (это значение может изменяться в зависимости от используемой версии).

Эту папку также можно открыть с помощью "Help-> открыть каталог журналов".

#### <a name="xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Универсальный установщик Xamarin

`~/Library/Logs/XamarinInstaller/Universal`

Это журналы из `XamarinInstaller.dmg` установщика.

#### <a name="xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Узел сборки Xamarin

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Подробные выходные данные сборки

1. Включите [выходные данные диагностики MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2. Для приложений iOS также включите **подробные выходные данные mtouch** , добавив `-v -v -v -v` **Свойства проекта > iOS Build > General (TAB) > дополнительные параметры > дополнительные аргументы mtouch**.

3. Очистите и перестройте проект.

4. Скопируйте и вставьте выходные данные сборки из IDE в текстовый файл.
     - Visual Studio (Windows): **просмотр > выходных данных > Показать выходные данные: сборка**
     - Visual Studio для Mac: **просмотр > pad > ошибках > выходные данные сборки (вкладка)**

## <a name="debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Журналы отладки для приложений Xamarin. Android и Xamarin. iOS

### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Просмотр выходных данных > приложения > Pad**

(Обратите внимание, что этот пункт меню будет отображаться только после запуска приложения.)

### <a name="visual-studio"></a>Visual Studio

**Просмотр выходных данных > > Отображение выходных данных: Отладка**

### <a name="android-adb-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />[`adb`](https://developer.android.com/tools/help/adb.html)Журналы logcat для Android

После выполнения `adb` команды присоедините файл **android_logcat.txt** к рабочему столу. В этих инструкциях предполагается, что подключено только одно устройство.

См. также страницу [журнала отладки Android](~/android/deploy-test/debugging/android-debug-log.md) .

#### <a name="visual-studio"></a>Visual Studio

1. **Средства > Android > запустить командную строку ADB для Android**
2. Очистите журнал: `adb logcat -c`
3. Воспроизведите проблему.
4. Вывод журнала: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1. **Средства > открыть пакет SDK для Android командной строки**
2. Очистите журнал: `adb logcat -c`
3. Воспроизведите проблему.
4. Вывод журнала: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />журналы симуляторов iOS (на Mac)

- Чтобы получить доступ к системному журналу, выберите **отладка > открыть системный журнал...** в приложении для симуляторов iOS.

- Чтобы просмотреть отчеты о сбоях в симуляторе, откройте консоль. app и перейдите по адресу `~/Library/Logs > DiagnosticReports` .

### <a name="ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />журналы устройств iOS (на Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Просмотр > Pad > журнал устройств iOS**

#### <a name="xcode"></a>Xcode

**Окна > устройств > $ {DeviceName}**

Отчеты о сбоях доступны при нажатии кнопки **Просмотр журналов устройств** . Системный журнал для устройства отображается в нижней части окна со стрелкой раскрытия.

#### <a name="xcode-5"></a>Xcode 5

**Окно > Организатор > устройств (вкладка) > $ {DeviceName}**