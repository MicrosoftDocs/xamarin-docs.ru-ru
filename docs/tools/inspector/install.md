---
title: Установка и требования инспектора
description: В этом документе описывается, как установить Xamarin Inspector и обсуждаются поддерживаемые платформы операционной системы, IDE и приложений.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: 39e7a6c86a2d4ab4231a29c0e1014de702af01ac
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573421"
---
# <a name="inspector-installation-and-requirements"></a>Установка и требования инспектора

## <a name="download-and-installation"></a>Загрузка и установка

# <a name="windows"></a>[Windows](#tab/windows)

1. Скачайте и установите [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) и выберите рабочую нагрузку **Разработка мобильных приложений на .NET** .
1. [Войдите](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) , чтобы включить корпоративную подписку.
1. [Проверьте](~/tools/inspector/inspect.md) свое приложение!

# <a name="macos"></a>[macOS](#tab/macos)

1. Скачайте и установите [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/).
1. [Войдите](https://docs.microsoft.com/visualstudio/mac/activation) , чтобы включить корпоративную подписку.
1. [Проверьте](~/tools/inspector/inspect.md) свое приложение!

-----

## <a name="requirements"></a>Требования

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10,11 или более поздней версии
- **Windows** 7 или более поздняя (с Internet Explorer 11 или более поздней версии и .NET 4.6.1 или более поздней версии)

### <a name="supported-ides"></a>Поддерживаемые IDE

- Visual Studio для Mac
- Visual Studio 2017 с рабочей нагрузкой **для мобильных приложений на платформе .NET**

Интерактивная проверка приложений доступна для корпоративных клиентов.

<a name="supported-platforms"></a>

### <a name="supported-app-platforms"></a>Поддерживаемые платформы приложений

|Платформа приложений|Поддержка интегрированной среды разработки|Примечания|
|--- |--- |--- |
|Mac|Поддерживается только в Visual Studio для Mac|
|iOS|Поддерживается в Visual Studio 2017 и Visual Studio для Mac| Для поведения компоновщика необходимо задать значение " **не связывать** " (в разделе Параметры проекта **сборки iOS** ) |
|Android|Поддерживается в Visual Studio 2017 и Visual Studio для Mac|Необходимо выбрать Android >= 4.0.3 с включенным **Фастдев** .<br />Необходимо использовать Эмуляторы Google, Visual Studio или Xamarin Android. В настоящее время Эмуляторы Android 7 могут не допускать проверку.|
|WPF|Поддерживается только в Visual Studio 2017|

<a name="reporting-bugs"></a>

## <a name="reporting-bugs"></a>Сообщение об ошибках

Ошибки следует сообщать непосредственно через Visual Studio:

- **Справка > отправить отзыв > сообщить о проблеме**

Включите следующие сведения:

### <a name="platform-version-information"></a>Сведения о версии платформы

Эти сведения являются жизненно важными.

Visual Studio для Mac

- **Visual Studio > о Visual Studio > показывать подробные сведения > сведения о копировании**
- Вставить в отчет об ошибках

Visual Studio

- **Справка > о > скопировать сведения о программе Visual Studio**
- Сообщите нам о версии операционной системы и о том, используете ли вы 32-разрядную или 64-разрядную версию Windows.

### <a name="log-files"></a>Файлы журналов

Всегда Прикрепите файлы журнала клиента IDE и инспектора.

Клиент инспектора

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4. x также обладает возможностью выбора файла журнала в Finder (macOS) или Explorer (Windows) непосредственно из главного меню:

- **Справка > обнаружить файл журнала**

Visual Studio для Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Содержимое панели **вывода** Visual Studio также может быть информативным.

### <a name="project-settings"></a>Параметры проекта

Если вы можете присоединить **CSPROJ** к проекту, который вы пытаетесь проверить, это было бы очень полезным. Это проще, чем спрашивать о отдельных параметрах.

Также убедитесь, что вы используете конфигурацию отладки.

### <a name="selected-devices"></a>Выбранные устройства

Для Android и iOS очень важно, что устройство, на котором выполняется отладка, при необходимости проверки. Необходимо знать следующее:

- Имя устройства, как показано в интегрированной среде разработки
- Версия ОС устройства
- Android: Убедитесь, что используется эмулятор x86.
- Android: какую платформу эмулятора вы используете? Эмулятор Google? Android Emulator Visual Studio? Xamarin Android Player?
- Должно ли приложение, которое вы отлаживается, правильно отображаться и работать на устройстве?
- Устройство подключено к сети (Проверьте через веб-браузер)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
