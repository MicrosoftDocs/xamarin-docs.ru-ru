---
title: Установка и требования к книгам
description: В этом документе описывается, как скачать и установить Xamarin Workbooks, обсуждаются поддерживаемые платформы и требования к системе.
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: a044169f86b46abff4158011e99320c528180ffc
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573889"
---
# <a name="workbooks-installation-and-requirements"></a>Установка и требования к книгам

<a name="install"></a>

## <a name="download-and-install"></a>Скачивание и установка

<!-- markdownlint-disable MD001 -->

# <a name="windows"></a>[Windows](#tab/windows)

1. Ознакомьтесь с [требованиями](#requirements) ниже.
2. Скачайте и установите [Xamarin Workbooks для Windows](https://dl.xamarin.com/interactive/XamarinInteractive.msi).
3. Начните [играть](~/tools/workbooks/workbook.md) с книгами.

# <a name="macos"></a>[macOS](#tab/macos)

1. Ознакомьтесь с [требованиями](#requirements) ниже.
2. Скачайте и установите [Xamarin Workbooks для Mac](https://dl.xamarin.com/interactive/XamarinInteractive.pkg).
3. Начните [с](~/tools/workbooks/workbook.md)этого.

-----

## <a name="requirements"></a>Требования

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10,11 или более поздней версии
- **Windows** 7 или более поздняя (с Internet Explorer 11 или более поздней версии и .NET 4.6.1 или более поздней версии)

#### <a name="supported-app-platforms"></a>Поддерживаемые платформы приложений

|Платформа приложений|Поддержка ОС|Примечания|
|--- |--- |--- |
|Mac|Поддерживается только на компьютерах Mac|
|iOS|Поддерживается в Mac и Windows|Xamarin. iOS 11,0 и Xcode 9,0 или более поздней версии должны быть установлены на компьютере Mac. Для выполнения книг iOS в Windows требуется узел сборки Mac, на котором выполняются все указанные выше компоненты, а также [Удаленный симулятор iOS](~/tools/ios-simulator/index.md) , установленный в Windows.|
|Android|Поддерживается в Mac и Windows|Необходимо использовать Google, Visual Studio или Xamarin Android Emulator с виртуальным устройством >= 5,0|
|WPF|Поддерживается только в Windows|
|Консоль (.NET Framework)|Поддерживается в Mac и Windows|
|Консоль (.NET Core)|Поддерживается в Mac и Windows|

## <a name="reporting-bugs"></a>Сообщение об ошибках

Сообщите о [проблемах на GitHub][bugs]и включите в него все приведенные ниже сведения.

### <a name="log-files"></a>Файлы журналов

Всегда прикреплять книги файлы журналов клиента:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4. x также обладает возможностью выбора файла журнала в Finder (macOS) или Explorer (Windows) непосредственно из главного меню:

- **Справка > обнаружить файл журнала**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Пути к журналам для книг 1,3 и более ранних версий:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Сведения о версии платформы

Очень полезно узнать сведения о вашей операционной системе и установленных продуктах Xamarin.

В главном меню в книгах:

- **Справка > копирование сведений о версии**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Инструкции для книг 1,3 и более ранних версий:

Visual Studio для Mac

- **Visual Studio > о Visual Studio > показывать подробные сведения > сведения о копировании**
- Вставить в отчет об ошибках

Visual Studio

- **Справка > о > скопировать сведения о программе Visual Studio**
- Сообщите нам о версии операционной системы и о том, используете ли вы 32-разрядную или 64-разрядную версию Windows.

### <a name="samples"></a>Примеры

Если вы можете присоединиться к файловым **книгам** или создать ссылки на них, с которыми возникают проблемы, это может помочь быстрее устранить ошибку.

### <a name="devices"></a>Устройства

Если у вас возникли проблемы с подключением к книге iOS или Android и вы уже выполнили проверку на [странице устранения неполадок](~/tools/workbooks/troubleshooting/index.md), необходимо знать следующее:

- Имя устройства, к которому вы пытаетесь подключиться
- Версия ОС устройства
- Android: Убедитесь, что используется эмулятор x86.
- Android: какую платформу эмулятора вы используете? Эмулятор Google?
  Android Emulator Visual Studio? Xamarin Android Player?
- iOS в Windows: какая версия удаленного имитатора Xamarin для iOS установлена (установите флажок **Установка и удаление программ** на **панели управления**)?
- iOS в Windows: также укажите сведения о версии платформы для узла сборки Mac
- Устройство подключено к сети (Проверьте через веб-браузер)?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Удаление

### <a name="windows"></a>Windows

В зависимости от того, как были получены книги, может потребоваться выполнить две процедуры удаления. Проверьте оба этих параметра, чтобы полностью удалить программное обеспечение.

#### <a name="visual-studio-installer"></a>Visual Studio Installer

Если у вас есть Visual Studio 2017, откройте **Visual Studio Installer**и просмотрите **отдельные компоненты** для **Xamarin Workbooks**. Если флажок установлен, снимите его и нажмите кнопку **изменить** , чтобы удалить.

#### <a name="system-uninstall"></a>Удаление системы

Если книги были установлены самостоятельно с помощью скачанного установщика, необходимо удалить их с помощью страницы параметры системы **& компонентов** в Windows 10 или с помощью компонента **Установка и удаление программ** на панели управления в предыдущих версиях Windows.

> **Запуск параметров > > приложений системы > & компонентов**

![](install-images/windows-remove.png "Xamarin Workbooks as listed in &quot;Apps &amp; features&quot;")

**По-прежнему следует выполнить процедуру Visual Studio Installer убедиться, что книги не переустанавливаются без вашего ведома.**

<a name="uninstall-macos"></a>

### <a name="macos"></a>MacOS

Начиная с [1.2.2](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/interactive/interactive-1.2.md), Xamarin Workbooks можно удалить из терминала, выполнив:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

В процессе удаления будут подробно описаны файлы и каталоги, которые будут удалены, и запрашивать подтверждение перед продолжением.

Передайте `-help` аргумент в `uninstall` скрипт для более сложных сценариев.

В более старых версиях требуется вручную удалить следующие компоненты:

1. Приложение Workbooks в `"/Applications/Xamarin Workbooks.app"`
2. Приложение Inspector в `"Applications/Xamarin Inspector.app"`
3. Надстройки `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` и `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`
4. Inspector и вспомогательные файлы `/Library/Frameworks/Xamarin.Interactive.framework` и `/Library/Frameworks/Xamarin.Inspector.framework`

## <a name="downgrading"></a>Переход

Идентификатор пакета для **книг/Xamarin. приложение** изменено с `com.xamarin.Inspector` на `com.xamarin.Workbooks` в версии 1,4, так как книги и инспектор теперь полностью разделены.

Из-за ошибки в старых установщиках невозможно перейти на более раннюю версию 1,4 или более поздней версии с помощью установщиков 1.3.2 или более старых версий.

Чтобы перейти с 1,4 или более новой версии в 1.3.2 или более раннюю версию:

1. [Удаление книг & инспектора вручную](#uninstall-macos)
2. Запуск установщика 1.3.2 или более старой версии `.pkg`
