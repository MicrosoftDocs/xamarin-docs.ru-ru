---
title: В какой версии Xamarin.Android добавлена поддержка Lollipop?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 27631d19ea157c1af08666b55a067e729b806434
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453640"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>В какой версии Xamarin.Android добавлена поддержка Lollipop?

> [!NOTE]
> Это руководство было изначально написано для предварительной версии Android L.

- [В Xamarin.Android 4.17](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.17/index.md) добавлена поддержка предварительной версии Android L.
- [Xamarin.Android 4.20](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.20/index.md) — добавлена поддержка Android Lollipop.

Xamarin активно поддерживает текущий стабильный выпуск инструментов Xamarin. Приведенные ниже сведения предоставляются "как есть" для более старых версий средств. Последние сведения о выпусках Xamarin см. в [заметках о выпуске](/xamarin/android/release-notes/).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>"Отсутствие android.jar для API уровня 21" в предварительной версии Android L

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Может отобразиться следующее сообщение (или похожее) об ошибке:

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Это сообщение означает, что платформа пакета SDK для Android для API уровня 21 не установлена. Установите ее в Диспетчер SDK Android (**Средства > Открыть Диспетчер SDK Android...** ), или измените проект Xamarin.Android, указав целевую версию API.

Есть несколько возможных вариантов решения этой проблемы.

1. Измените проект так, чтобы его целевым объектом был API уровня 19 или ниже.

2. Переименуйте папку Android-21 с Android-21 на Android-L. (Лучше всего использовать его только как временное исправление, оно может не работать должным образом.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Временной переход на использование более ранней версии — предварительной версии API Android 21 "L" [1]:

    1. Удалите **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21** 
    2. Извлеките [1] в папку **C:\\Users\\&lt;username&gt;\\AppData\\Local\\Android\\android-sdk\\platforms**, чтобы создать папку **Android-L**.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Может отобразиться следующее сообщение (или похожее) об ошибке:

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Это означает, что платформа SDK для Android для API уровня 21 не установлена. Установите ее с помощью Диспетчера SDK Android (Средства > Диспетчер SDK...) или измените проект Xamarin.Android так, чтобы он указывал на установленную версию API.

Есть несколько возможных вариантов решения этой проблемы.

1. Измените проект так, чтобы его целевым объектом был API уровня 19 или ниже.

2. Переименуйте папку Android-21 с Android-21 на Android-L. (Лучше всего использовать его только как временное исправление, оно может не работать должным образом.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Временной переход на использование более ранней версии — предварительной версии API Android 21 "L" [1]:

    1. Удалите **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2. Извлеките [1] в **/Users/username/Library/Developer/Xamarin/android-sdk-macosx**, чтобы создать папку **android-L**.

-----

[1] — [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)