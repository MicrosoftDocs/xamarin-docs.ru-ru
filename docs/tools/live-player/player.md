---
title: Приложение Xamarin Live Player
description: В этом документе описывается приложение Xamarin Live Player, которое можно использовать для предварительного просмотра изменений кода на устройстве. В нем обсуждаются программы установки, примеры, журналы, параметры, Управление устройствами и многое другое.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: davidortinau
ms.author: daortin
ms.date: 06/13/2019
ms.openlocfilehash: 7c99fb593f0515dc45b64c01621cb453340f7bc1
ms.sourcegitcommit: 4bbf54d2bc1df96af69814e2e5dae47be12e0474
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102602870"
---
# <a name="xamarin-live-player-app"></a>Приложение Xamarin Live Player

![Предварительная версия](~/media/shared/preview.png)

> [!WARNING]
> Предварительный просмотр Xamarin Live Player завершен. Приложение больше не доступно. Приведенные ниже инструкции предназначены для пользователей, которые продолжают использовать предварительную версию с Visual Studio 2017.

> [!TIP]
> Вы можете использовать [горячую перезагрузку XAML (~/ксамарин-Формс/ксамл/хот-релоад/индекс.МД) в Visual Studio 2019 или Visual Studio для Mac для просмотра своих макетов на экране по мере их редактирования.

При запуске Xamarin Live Player приложение выглядит следующим образом:

![Снимок экрана приложения для Android Live Player](player-images/app-android-sml.png)

При нажатии клавиши **связать с Visual Studio** используйте камеру для сканирования штрихкода, показанного на компьютере:

![Снимок экрана: сканер штрихкодов Android](player-images/scan-android-sml.png)

Если соединение установлено успешно, код должен выполняться на устройстве почти мгновенно (например, в [примере калькулятора](https://github.com/xamarin/mobile-samples/tree/master/LivePlayer/BasicCalculator):

![Пример приложения калькулятора, выполняемого на устройстве](player-images/basic-calculator-sml.png)

## <a name="options"></a>Варианты

Нажмите кнопку со сведениями **(i)** в нижней части приложения, чтобы открыть меню **параметров** :

[![Снимок экрана: меню параметров](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Журналы

Просмотрите журналы, чтобы диагностировать проблемы.

### <a name="settings"></a>Параметры

- Переключить отображение ошибок компиляции и времени выполнения.
- Сведения о версии.
- Отправить отзыв.

[![Снимок экрана параметров](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>управление устройствами;

Чтобы подключить устройство в первый раз, следуйте инструкциям в разделе [требования & установки](~/tools/live-player/install.md). Вы можете связать несколько устройств и управлять ими с помощью интегрированной среды разработки.

# <a name="visual-studio-2017"></a>[Visual Studio 2017](#tab/windows)

В Visual Studio выберите **сервис > Xamarin Live Player > Управление устройствами...**

![Окно управления устройствами](player-images/manage-tools-menu-vs.png)

Это окно позволяет выполнять следующие действия.

- Связывание нового устройства путем сканирования кода
- Кроме того, можно связать устройство, введя код, отображаемый на экране
- Удалить существующие устройства из списка

Это окно также можно открыть из списка устройств.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

В Visual Studio для Mac выберите **сервис > (Xamarin Live Player) Управление устройствами...**

![На снимке экрана показано управление устройствами, выбранными в окне "Инструменты".](player-images/manage-tools-menu.png)

Это окно позволяет выполнять следующие действия.

- Связывание нового устройства путем сканирования кода
- Кроме того, можно связать устройство, введя код, отображаемый на экране
- Удалить существующие устройства из списка

![На снимке экрана показано окно Xamarin Live Player с возможностью предварительного просмотра приложения.](player-images/manage.png)

Это окно также можно открыть из списка устройств:

![Выбор устройств Xamarin Live Player в списке устройств](player-images/manage-device-menu.png)

-----

Если возникнут проблемы, см. раздел [ограничения и устранение неполадок](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>Связанные ссылки

- [Устранение неполадок](~/tools/live-player/troubleshooting.md)
