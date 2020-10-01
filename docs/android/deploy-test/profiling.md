---
title: Профилирование приложений Android
description: Это руководство поясняет, как использовать средства профилирования для изучения производительности и использования памяти в приложении Android.
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C823FEE-A6F6-4C31-9EB6-E51407A2FD8E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 96c9e750238bce35d5c8c12dd2ceba6be7ab8fcc
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454823"
---
# <a name="profiling-android-apps"></a>Профилирование приложений Android

Перед развертыванием приложения в Магазине важно выявить и устранить все узкие места по производительности, проблемы с использованием чрезмерного количества памяти или неэффективным использованием сетевых ресурсов. Для этого доступны два средства профилирования:

- Xamarin Profiler 
- Android Profiler в Android Studio

Это руководство содержит общие сведения о Xamarin Profiler, а также информацию о начале работы с Android Profiler.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin Profiler — это автономное приложение, интегрированное с Visual Studio и Visual Studio для Mac для профилирования приложений Xamarin из интегрированной среды разработки. Дополнительные сведения об использовании Xamarin Profiler см. в статье [Xamarin Profiler](~/tools/profiler/index.md).

> [!NOTE]
> Чтобы разблокировать компонент Xamarin Profiler в Visual Studio Enterprise для Windows или Visual Studio для Mac, вы должны быть подписчиком [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/).

## <a name="android-studio-profiler"></a>Профилировщик Android Studio

Android Studio 3.0 и более поздних версий включает в себя средство Android Profiler. Его можно использовать для измерения производительности приложения Xamarin Android, созданного с помощью Visual Studio, &ndash; без потребности в лицензии Visual Studio Enterprise. Однако в отличие от Xamarin Profiler средство Android Profiler не интегрировано с Visual Studio и может использоваться только для профилирования пакета приложения Android (APK), который был создан заранее и импортирован в Android Profiler.

### <a name="launching-a-xamarin-android-app-in-android-profiler"></a>Запуск приложения Xamarin Android в Android Profiler

Ниже описано, как запустить приложение Xamarin Android в средстве Android Profiler среды Android Studio. На примерах снимков экрана ниже приложение [XamagonXuzzle](/samples/xamarin/mobile-samples/liveplayer-xamagonxuzzlelp/) Xamarin Forms создается и профилируется с помощью Android Profiler:

1. В параметрах сборки проекта Android отключите параметр **Использовать общую среду выполнения**. Это позволяет выполнить сборку пакета приложения Android (APK) без зависимости от общей среды выполнения Mono времени разработки.

    ![Отключение параметра "Использовать общую среду выполнения"](profiling-images/vswin/01-turn-off-shared-runtime.png)

2. Выполните сборку приложения для **отладки** и разверните его на физическом устройстве или в эмуляторе. В результате создается подписанная **отладочная** версия APK-файла.
    Для примера **XamagonXuzzle** итоговый APK-файл называется **com.companyname.XamagonXuzzle Signed.apk**.

3. Откройте папку проекта и перейдите в **bin/Debug**. В этой папке найдите версию **Signed.apk** приложения и скопируйте ее в удобное место (например, на рабочий стол). На следующем снимке экрана указан APK-файл **com.companyname.XamagonXuzzle Signed.apk**, копируемый на рабочий стол:

    [![Расположение отладочного подписанного APK-файла](profiling-images/vswin/02-locating-the-debug-apk-sml.png)](profiling-images/vswin/02-locating-the-debug-apk.png#lightbox)

4. Запустите Android Studio и выберите **Profile or debug APK** (Профилирование или отладка APK):

    ![Запуск профилировщика с экрана запуска Android Studio](profiling-images/vswin/03-android-studio.png)

5. В диалоговом окне **Select APK File** (Выбор APK-файла) перейдите к ранее созданному и скопированному APK-файлу. Выберите этот файл и нажмите кнопку **ОК**. 
    
    ![Выбор APK в диалоговом окне "Select APK File" (Выбор APK-файла)](profiling-images/vswin/04-select-apk-dialog.png)

6. Android Studio загружает APK-файл и дизассемблирует **classes.dex**:

    ![Настройка APK-файла](profiling-images/vswin/05-setting-up-the-apk.png)

7. После загрузки APK среда Android Studio отображает для него приведенный ниже экран проекта. Щелкните правой кнопкой мыши имя приложения в представлении в виде дерева слева и выберите пункт **Open Module Settings** (Открыть параметры модуля):

    [![Расположение элемента меню "Open Module Settings" (Открыть параметры модуля)](profiling-images/vswin/06-open-module-settings-sml.png)](profiling-images/vswin/06-open-module-settings.png#lightbox)

8. Перейдите в раздел **Параметры проекта > Модули**, выберите узел **-Подписан** для приложения, а затем щелкните **&lt;Без SDK&gt;** :

    [![Переход к параметру пакета SDK](profiling-images/vswin/07-project-settings-modules-sml.png)](profiling-images/vswin/07-project-settings-modules.png#lightbox)

9. В раскрывающемся меню **Модуль SDK** выберите уровень пакета SDK для Android, который использовался для сборки приложения (в этом примере для сборки **XamagonXuzzle** использовался уровень API 26):

    [![Настройка уровня для пакета SDK проекта](profiling-images/vswin/08-project-sdk-level-sml.png)](profiling-images/vswin/08-project-sdk-level.png#lightbox)

    Нажмите кнопки **Применить** и **ОК**, чтобы сохранить этот параметр.

10. Запуск профилировщика с помощью значка на панели инструментов:

    [![Расположение значка профилировщика на панели инструментов](profiling-images/vswin/09-launch-profiler-sml.png)](profiling-images/vswin/09-launch-profiler.png#lightbox)

11. Выберите целевой объект развертывания для запуска и профилирования приложения и нажмите кнопку **ОК**. Целевым объектом развертывания может быть физическое устройство либо виртуальное устройство, запущенное в эмуляторе. В этом примере используется устройство Nexus 5X:

    ![Выбор целевого объекта развертывания](profiling-images/vswin/10-select-deployment-target.png)

12. После запуска профилировщику потребуется несколько секунд для подключения к устройству развертывания и процессу приложения. При установке APK-файла Android Profiler выведет сообщения **No connected devices** (Подключенные устройства отсутствуют) и **No debuggable processes** (Нет отлаживаемых процессов).

    [![Установка APK-файла профилировщиком](profiling-images/vswin/11-no-connected-devices-sml.png)](profiling-images/vswin/11-no-connected-devices.png#lightbox)

13. Через несколько секунд Android Profiler завершает установку APK-файла и запускает его, сообщая имя устройства и имя профилируемого процесса приложения (в данном примере это **LGE Nexus 5X** и **com.companyname.XamagonXuzzle**, соответственно):

    [![Окно профилировщика после запуска](profiling-images/vswin/12-profiler-starts-sml.png)](profiling-images/vswin/12-profiler-starts.png#lightbox)

14. После идентификации устройства и отлаживаемых процессов Android Profiler начинает профилирование приложения:

    [![Отображение профилировщика для выполняющегося приложения](profiling-images/vswin/13-profiler-running-sml.png)](profiling-images/vswin/13-profiler-running.png#lightbox)

15. Если нажать кнопку **RANDOMIZE** (Случайный выбор) для **XamagonXuzzle** (что приводит к смещению и рандомизации плиток), вы заметите повышение загрузки ЦП во время интервала рандомизации приложения:

    [![Загрузка ЦП при нажатии кнопки CPU "RANDOMIZE" (Случайный выбор)](profiling-images/vswin/14-tap-randomize-sml.png)](profiling-images/vswin/14-tap-randomize.png#lightbox)

### <a name="using-the-android-profiler"></a>Использование Android Profiler

Подробные сведения об использовании Android Profiler приведены в [документации по Android Studio](https://developer.android.com/studio/profile/android-profiler.html).
Для разработчиков Xamarin Android могут представлять интерес следующие разделы:

- [Профилировщик ЦП](https://developer.android.com/studio/profile/cpu-profiler.html) помогает проверить загрузку ЦП и активность потоков для приложения в реальном времени.

- [Профилировщик памяти](https://developer.android.com/studio/profile/memory-profiler.html) отображает график использования памяти приложением в реальном времени, а также позволяет регистрировать выделения памяти для анализа.

- [Профилировщик сети](https://developer.android.com/studio/profile/network-profiler.html) отображает сведения о сетевой активности в реальном времени для данных, отправляемых и получаемых приложением.