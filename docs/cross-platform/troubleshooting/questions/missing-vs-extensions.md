---
title: Отсутствуют расширения Visual Studio после установки
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 976d0882c5875c1d3e1c8f0ea1732de08df8e07f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996179"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Отсутствуют расширения Visual Studio после установки

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Сообщение об ошибке: этот проект несовместим с текущим выпуском Visual Studio

Убедитесь, что установлен Visual Studio 2017 (Community, Professional или Enterprise) или более новой версии.

См. также [требования Windows](~/cross-platform/get-started/requirements.md#windows-requirements).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Возможное исправление 1. Измените установку, чтобы убедиться, что расширения Visual Studio установлены

В некоторых случаях установщик Xamarin может автоматически отменить проверку параметров установки расширений Visual Studio. Если причина проблемы, установите недостающие расширения Visual Studio с помощью команды **изменения** установщика. Например, чтобы установить расширения для Visual Studio 2013:

1. Откройте панель управления **программы и компоненты** Windows.

2. Щелкните правой кнопкой мыши запись **Xamarin** и выберите **изменить**.

3. Нажмите кнопку **Далее**, а затем — **изменить**.

4. Убедитесь, что для параметра **Xamarin для Visual Studio 2013** установлено значение установить:

    ![Параметр установки "включить Xamarin для Visual Studio 2013"](missing-vs-extensions-images/installer.png)

5. Выполните остальные действия в мастере установки.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Возможное исправление 2. Попросите Visual Studio снова настроить расширения

1. Проверьте, скопированы ли расширения Xamarin в папку расширений Visual Studio:

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Если расширения установлены правильно (для версии 3.1.228), в папке будет 60 элементов:

    ![Список содержимого папки Xamarin\3.1.228.0 в обозревателе](missing-vs-extensions-images/folder.png)

2. Убедившись, что эта папка выглядит правильно, сообщите Visual Studio о необходимости повторной настройки расширений:

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Возможное исправление 3. Попробуйте заново установить Xamarin

1. На панели управления Windows удалите следующие элементы:

    * Xamarin

    * Xamarin для Windows

    * Xamarin.Android

    * Xamarin.iOS

    * Xamarin для Visual Studio

2. В обозревателе удалите все оставшиеся файлы из папок расширения Xamarin Visual Studio (все версии, включая **программные файлы** и **программные файлы (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3. Также проверьте каталог "Виртуалсторе", чтобы узнать, есть ли какие либо наложения копий любых каталогов расширений:

    `%LOCALAPPDATA%\VirtualStore`

4. Откройте редактор реестра (regedit).

5. Найдите следующий раздел.

    _HKEY \_ Local \_ MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6. Найдите и удалите все записи, соответствующие этому шаблону.

    _C:\Program Files \* \Microsoft Visual Studio 1 \* . 0 \ Common7\IDE\Extensions\Xamarin_

7. Найдите следующий раздел.

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8. Удалите все записи, которые могут иметь отношение к Xamarin. Вот пример, который используется для того, чтобы вызвать проблемы в старых версиях Xamarin:

    _Моно. VisualStudio. Shell, 1.0_

9. Перезагрузите систему.

10. Переустановите текущую стабильную версию Xamarin из [VisualStudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Возможное исправление 4. Восстановление установки Visual Studio

1. Откройте панель управления **программы и компоненты** Windows.

2. Щелкните правой кнопкой мыши соответствующую запись Microsoft Visual Studio и выберите **изменить** .

3. В открывшемся диалоговом окне Visual Studio нажмите кнопку **восстановить** .
