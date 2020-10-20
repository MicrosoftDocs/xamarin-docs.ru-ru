---
title: Можно ли подключиться с виртуальной машины Windows к эмуляторам Android под управлением Mac?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 336fb2ae1f1619994b11b630a6cb6726f17d0758
ms.sourcegitcommit: 6d347e1d7641ac1d2b389fb1dc7a6882a08f7c00
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/08/2020
ms.locfileid: "91851526"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Можно ли подключиться с виртуальной машины Windows к эмуляторам Android под управлением Mac?

Чтобы подключиться к Android Emulator, работающему на компьютере Mac, из виртуальной машины Windows, выполните следующие действия.

> [!NOTE]
> Рекомендуется использовать Android Emulator без встроенного Магазина Google Play.

1. Запустите эмулятор на компьютере Mac.

2. Завершите работу `adb` на компьютере Mac:

    ```bash
    adb kill-server
    ```

3. Обратите внимание, что эмулятор прослушивается на 2 портах TCP в сетевом интерфейсе замыкания на себя:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Для подключения к `adb` используется порт с нечетным номером. См. также [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4. _Вариант 1_. Используйте `nc`, чтобы пересылать входящие TCP-пакеты, полученные извне через порт 5555 (или любой другой предпочтительный порт) на порт с нечетным номером в интерфейсе замыкания на себя (в этом примере **127.0.0.1 5555**), а также для пересылки исходящих пакетов другим способом.

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    Если команды `nc` продолжают выполняться в окне терминала, пакеты перенаправляются должным образом. Вы можете ввести в окне терминала Control-C, чтобы выйти из команд `nc` после завершения работы с эмулятором.

    (Вариант 1 обычно проще, чем вариант 2, особенно если включен параметр **Системные настройки > Безопасность и конфиденциальность > Брандмауэр**.)

    _Вариант 2_. Используйте `pfctl` для перенаправления пакетов TCP с порта `5555` (или любого другого предпочтительного порта) в [общем сетевом](https://kb.parallels.com/en/4948) интерфейсе нечетный порт в интерфейсе замыкания на себя (в этом примере `127.0.0.1:5555`):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Эта команда настраивает перенаправление портов с помощью системной службы `pf packet filter`. Разрывы строк важны. Проследите, чтобы они сохранились при копировании. Кроме того, при использовании Parallels необходимо изменить имя интерфейса *vmnet8* на другое. `vmnet8` — это имя специального *устройства NAT* для режима *Shared Networking* (Общий сетевой интерфейс) в VMWare Fusion. Для Parallels, вероятно, подойдет сетевой интерфейс [vnic0](https://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5. Подключитесь к эмулятору с компьютера Windows:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Замените "ip-address-of-the-mac" на IP-адрес компьютера Mac, например тот, который указан `ifconfig vmnet8 | grep 'inet '`. При необходимости замените `5555` другим предпочтительным портом, выбранным на шаге 4\. (Примечание. Доступ к `adb` из командной строки можно получить с помощью команды [**Сервис > Android > Командная строка Adb Android**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) в Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Альтернативный способ с использованием `ssh`

Если вы включили _Удаленный вход_ на компьютере Mac, для подключения к эмулятору можно использовать перенаправление портов с помощью `ssh`.

1. Установите SSH-клиент в Windows. Один из вариантов — установить [Git для Windows](https://git-for-windows.github.io/). Тогда команда `ssh` будет доступна в командной строке **Git Bash**.

2. Выполните описанные выше шаги 1–3, чтобы запустить эмулятор, завершить работу сервера `adb` на компьютере Mac и задать порты эмулятора.

3. Запустите `ssh` в Windows, чтобы настроить двустороннюю переадресацию портов между локальным портом в Windows (в этом примере `localhost:15555`) и портом эмулятора с нечетным номером в интерфейсе замыкания на себя на компьютере Mac (в этом примере `127.0.0.1:5555`):

    ```cmd
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Замените `mac-username` именем пользователя компьютера Mac, как указано в `whoami`. Замените `ip-address-of-the-mac` IP-адресом компьютера Mac.

4. Подключитесь к эмулятору через локальный порт в Windows:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Примечание. Доступ к `adb` из командной строки можно легко получить с помощью команды [**Сервис > Android > Командная строка Adb Android** в Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Небольшое предупреждение. Если в качестве локального порта используется порт `5555`, `adb` считает, что эмулятор выполняется локально в Windows. Это не создает проблем в Visual Studio, но в Visual Studio для Mac приводит к завершению работы приложения сразу после запуска.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>Альтернативный метод, использующий `adb -H`, пока не поддерживается.

Теоретически можно было бы использовать встроенные возможности `adb` для подключения к серверу `adb`, работающему на удаленном компьютере (см. пример [https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325)).
Однако расширения IDE Xamarin.Android в настоящее время не предоставляют возможности для настройки этого параметра.

## <a name="contact-information"></a>Контактные данные

В этом документе рассматривается текущее поведение по состоянию на март 2016 года. Методика, описанная в этом документе, не является частью стабильного набора тестов для Xamarin, поэтому в будущем ее поддержка может прерваться.

Если вы заметили, что методика больше не работает, или обратили внимание на другие ошибки в документе, вы можете добавить обсуждение в следующую беседу форума: [http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](https://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Спасибо!
