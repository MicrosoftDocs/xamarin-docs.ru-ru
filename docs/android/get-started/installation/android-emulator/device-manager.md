---
title: Управление виртуальными устройствами с помощью Android Device Manager
description: Это руководство описывает использование Android Device Manager для создания и настройки виртуальных устройств Android (AVD), эмулирующих физические устройства Android. Эти виртуальные устройства позволяют запустить и протестировать приложение без использования физического устройства.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.custom: video
ms.date: 01/22/2019
ms.openlocfilehash: 13b66f3484f274c04282ef1964107fcb53c62e38
ms.sourcegitcommit: 63029dd7ea4edb707a53ea936ddbee684a926204
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98609798"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Управление виртуальными устройствами с помощью Android Device Manager

_Это руководство описывает использование Android Device Manager для создания и настройки виртуальных устройств Android (AVD), эмулирующих физические устройства Android. Эти виртуальные устройства позволяют запускать и тестировать приложение без использования физического устройства._

Убедившись, что аппаратное ускорение включено (как описано в статье [Аппаратное ускорение для производительной работы эмулятора](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), воспользуйтесь _Android Device Manager_ (который также называют _Xamarin Android Device Manager_), чтобы создать виртуальные устройства, которые можно использовать для тестирования и отладки приложения.

::: zone pivot="windows"

## <a name="android-device-manager-on-windows"></a>Android Device Manager в Windows

Эта статья поясняет, как создавать, дублировать, настраивать и запускать виртуальные устройства Android с помощью Android Device Manager.

[![Снимок экрана с Android Device Manager на вкладке устройств](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Android Device Manager предназначен для создания и настройки _виртуальных устройств Android_ (AVD), запускаемых в [Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).
Каждое AVD является конфигурацией эмулятора, имитирующей физическое устройство Android. Это позволяет запускать и тестировать приложение в различных конфигурациях, имитирующих различные физические устройства Android.

## <a name="requirements"></a>Требования

Чтобы использовать Android Device Manager, необходимо следующее:

- Visual Studio 2019 (Community, Professional или Enterprise).

- ИЛИ Visual Studio 2017 версии 15.8 или более поздней. Поддерживаются выпуски Visual Studio Community, Professional и Enterprise.

- Инструменты Visual Studio для Xamarin версии 4.9 или более поздней.

- Необходимо установить пакет SDK для Android (см. раздел [Настройка пакета SDK для Android для Xamarin.Android](~/android/get-started/installation/android-sdk.md)).
  Обязательно установите пакет SDK для Android в расположение по умолчанию (если это еще не сделано): **C:\\Program Files (x86)\\Android\\android-sdk**.

- Следующие пакеты должны быть установлены (через [диспетчер пакетов SDK для Android](~/android/get-started/installation/android-sdk.md)): 
  - **Android SDK Tools версии 26.1.1** или более поздней
  - **Средства платформы пакета SDK для Android 27.0.1** или более поздней версии
  - **Средства сборки пакета SDK для Android 27.0.3** или более поздней версии 
  - **Android Emulator 27.2.7** или более поздней версии. 

  Эти пакеты должны отображаться с состоянием **Установлено**, как показано на следующем снимке экрана:

  [![Установка Android SDK Tools](device-manager-images/win/02-sdk-tools-sml.png)](device-manager-images/win/02-sdk-tools.png#lightbox)

## <a name="launching-the-device-manager"></a>Запуск диспетчера устройств

Запустите Android Device Manager в меню **Сервис**, выбрав **Сервис > Android > Диспетчер устройств с Android**:

[![Запуск диспетчера устройств из меню "Сервис"](device-manager-images/win/03-tools-menu-sml.png)](device-manager-images/win/03-tools-menu.png#lightbox)

Если вы видите следующее диалоговое окно ошибки при запуске, см. инструкции по обходу в разделе [Устранение неполадок](#troubleshooting):

![Диалоговое окно ошибки экземпляра SDK для Android](device-manager-images/win/04-sdk-error.png)

## <a name="main-screen"></a>Главный экран

При первом запуске диспетчер устройств Android выводит экран, где отображаются все настроенные виртуальные устройства. Для каждого виртуального устройства указаны параметры **Имя**, **ОС** (версия Android), **Процессор**, **Память**, а также **Разрешение** экрана:

[![Список установленных устройств и их параметров](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

Если выбрать устройство в списке, справа отображается кнопка **Запустить**. Можно нажать кнопку **Запустить**, чтобы запустить эмулятор с этим виртуальным устройством:

[![Кнопка "Запустить" для образа устройства](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

После запуска эмулятора с выбранным виртуальным устройством кнопка **Запустить** изменяется на кнопку **Остановить**, позволяющую остановить эмулятор:

[![Кнопка "Остановить" для работающего устройства](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>Создание устройства

Чтобы создать устройство, нажмите кнопку **Создать** в верхней правой части экрана:

[![Кнопка "Создать" для создания устройства](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

При нажатии кнопки **Создать** открывается экран **Новое устройство**:

[![Экран "Новое устройство" диспетчера устройств](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

Чтобы настроить новое устройство на экране **Новое устройство**, выполните следующие действия:

1. Задайте новое имя для устройства. В следующем примере новое устройство называется **Pixel_API_27**:

   [![Именование нового устройства](device-manager-images/win/10-device-name-sml.png)](device-manager-images/win/10-device-name.png#lightbox)

2. Выберите физическое устройство для эмуляции, щелкнув раскрывающееся меню **Базовое устройство**:

   [![Выбор физического устройства для эмуляции](device-manager-images/win/11-device-menu-sml.png)](device-manager-images/win/11-device-menu.png#lightbox)

3. Выберите тип процессора для этого виртуального устройства, щелкнув раскрывающееся меню **Процессор**. **x86** обеспечивает максимальную производительность, так как позволяет эмулятору воспользоваться преимуществами [аппаратного ускорения](~/android/get-started/installation/android-emulator/hardware-acceleration.md).
   Вариант **X86_64** также использует аппаратное ускорение, но работает немного медленнее, чем **x86** (**x86_64** обычно используется для тестирования 64-разрядных приложений):

   [![Выбор типа процессора](device-manager-images/win/12-processor-type-menu-sml.png)](device-manager-images/win/12-processor-type-menu.png#lightbox)

4. Выберите версию Android (уровень API), щелкнув раскрывающееся меню **ОС**. Например, выберите **Oreo 8.1 — API 27** для создания виртуального устройства для API уровня 27:

   [![Выбор версии Android](device-manager-images/win/13-android-version-w158-sml.png)](device-manager-images/win/13-android-version-w158.png#lightbox)

   Если выбран Android (уровень API), который еще не был установлен, диспетчер устройств выдаст сообщение **Новое устройство будет загружено** в нижней части экрана &ndash; он загрузит и установит необходимые файлы, создавая новое виртуальное устройство:

   ![Будет скачан новый образ устройства](device-manager-images/win/14-automatic-download-w158.png)

5. Если вы хотите включить API сервисов Google Play на виртуальном устройстве, включите параметр **API Google**. Чтобы включить приложение Google Play Маркет, включите параметр **Google Play Маркет**:

   [![Выбор сервисов Google Play и Google Play Маркет](device-manager-images/win/15-google-play-services-sml.png)](device-manager-images/win/15-google-play-services.png#lightbox)

   Обратите внимание, что образы Google Play Маркет доступны только для некоторых типов базовых устройств, например Pixel, Pixel 2, Nexus 5 и Nexus 5X.

6. Измените нужные свойства. Сведения о том, как это сделать, см. в разделе [Изменение свойств виртуальных устройств Android](~/android/get-started/installation/android-emulator/device-properties.md).

7. Добавьте любые дополнительные свойства, которые нужно задать явно. На экране **Новое устройство** перечислены только часто изменяемые свойства, но можно щелкнуть раскрывающееся меню **Добавление свойства** в нижней части, чтобы добавить дополнительные свойства:

   [![Раскрывающееся меню "Добавление свойства"](device-manager-images/win/16-add-property-menu-sml.png)](device-manager-images/win/16-add-property-menu.png#lightbox)

    Можно также определить пользовательское свойство, выбрав **Пользовательский...** в верхней части списка свойств.

8. Нажмите кнопку **Создать** в нижнем правом углу, чтобы создать устройство:

   [![Кнопка "Создать"](device-manager-images/win/17-create-button-sml.png)](device-manager-images/win/17-create-button.png#lightbox)

9. Может появиться экран **Принятие условий лицензионного соглашения**. Если вы согласны с условиями лицензии, щелкните **Принять**:

   [![Экран "Принятие условий лицензионного соглашения"](device-manager-images/win/18-license-acceptance-sml.png)](device-manager-images/win/18-license-acceptance.png#lightbox)

10. Пока устройство создается, Android Device Manager добавляет его в список установленных виртуальных устройств, отображая индикатор хода выполнения **Создание**:

    [![Индикатор хода выполнения "Создание"](device-manager-images/win/19-creating-the-device-sml.png)](device-manager-images/win/19-creating-the-device.png#lightbox)

11. После завершения создания новое устройство отображается в списке установленных виртуальных устройств с кнопкой **Запустить**, готовое к запуску:

    [![Только что созданное устройство, готовое к запуску](device-manager-images/win/20-created-device-sml.png)](device-manager-images/win/20-created-device.png#lightbox)

### <a name="edit-device"></a>Изменение устройства

Чтобы изменить существующее виртуальное устройство, выберите его и нажмите кнопку **Изменить** в правом верхнем углу:

[![Кнопка "Изменить" для изменения устройства](device-manager-images/win/21-edit-button-sml.png)](device-manager-images/win/21-edit-button.png#lightbox)

При щелчке элемента **Изменить** запускается редактор устройств для выбранного виртуального устройства:

[![Окно редактора устройств](device-manager-images/win/22-device-editor-sml.png)](device-manager-images/win/22-device-editor.png#lightbox)

На экране **Редактор устройств** указаны свойства виртуального устройства в столбце **Свойство** с соответствующими значениями каждого свойства в столбце **Значение**. При выборе свойства справа отображается его подробное описание.

Чтобы изменить свойство, измените его значение в столбце **Значение**.
Например, на следующем снимке экрана свойство `hw.lcd.density` изменяется с **480** на **240**:

[![Пример изменения устройства](device-manager-images/win/23-device-editing-sml.png)](device-manager-images/win/23-device-editing.png#lightbox)

После внесения необходимых изменений в конфигурацию нажмите кнопку **Сохранить**.
Дополнительные сведения об изменении свойств виртуального устройства см. в разделе [Изменение свойств виртуальных устройств Android](~/android/get-started/installation/android-emulator/device-properties.md).

### <a name="additional-options"></a>Дополнительные параметры

Дополнительные параметры для работы с устройствами доступны в раскрывающемся меню **Дополнительные параметры** (&hellip;) в правом верхнем углу:

[![Расположение меню с дополнительными параметрами](device-manager-images/win/24-overflow-menu-sml.png)](device-manager-images/win/24-overflow-menu.png#lightbox)

Меню с дополнительными параметрами содержит следующие элементы:

- **Duplicate and Edit** (Дублировать и править) — дублирует выбранное устройство и открывает экран **Новое устройство** с другим уникальным именем. Например, если выбрать **Pixel_API_27** и щелкнуть **Скопировать и изменить**, к имени добавляется счетчик:

  [![Экран "Duplicate and Edit" (Дублировать и править)](device-manager-images/win/25-dupe-and-edit-sml.png)](device-manager-images/win/25-dupe-and-edit.png#lightbox)

- **Отобразить в проводнике** — открывает окно проводника в папке с файлами для виртуального устройства. Например, если выбрать **Pixel_API_27** и щелкнуть **Отобразить в проводнике**, открывается окно, похожее на следующее:

  [![Результаты выбора параметра "Отобразить в проводнике"](device-manager-images/win/26-reveal-in-explorer-sml.png)](device-manager-images/win/26-reveal-in-explorer.png#lightbox)

- **Сброс параметров** — восстанавливает стандартные настройки выбранного устройства, удаляя любые изменения, внесенные пользователем во внутреннее состояние устройства во время его работы (также удаляется любой актуальный моментальный снимок [быстрой загрузки](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot) при его наличии). Эта операция не затрагивает изменения, внесенные на виртуальном устройстве во время его создания и редактирования. Отображается диалоговое окно с напоминанием, что этот сброс невозможно отменить. Нажмите кнопку **Сброс до заводских настроек** для подтверждения сброса:

  ![Диалоговое окно "Сброс до заводских настроек"](device-manager-images/win/27-factory-reset.png)

- **Удалить** — удаляет выбранное виртуальное устройство без возможности восстановления. Отображается диалоговое окно с напоминанием, что такое удаление невозможно отменить. Щелкните **Удалить**, если уверены, что устройство нужно удалить.

  ![Диалоговое окно "Удалить устройство"](device-manager-images/win/28-delete-device-w158.png)

::: zone-end
::: zone pivot="macos"

## <a name="android-device-manager-on-macos"></a>Android Device Manager в macOS

Эта статья поясняет, как создавать, дублировать, настраивать и запускать виртуальные устройства Android с помощью Android Device Manager.

[![Снимок экрана с Android Device Manager на вкладке устройств](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Это руководство распространяется только на Visual Studio для Mac.
Xamarin Studio не совместим с Android Device Manager.

Android Device Manager предназначен для создания и настройки *виртуальных устройств Android* (AVD), запускаемых в [Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).
Каждое AVD является конфигурацией эмулятора, имитирующей физическое устройство Android. Это позволяет запускать и тестировать приложение в различных конфигурациях, имитирующих различные физические устройства Android.

## <a name="requirements"></a>Требования

Чтобы использовать Android Device Manager, необходимо следующее:

- Visual Studio для Mac 7.6 или более поздней версии.

- Необходимо установить пакет SDK для Android (см. раздел [Настройка пакета SDK для Android для Xamarin.Android](~/android/get-started/installation/android-sdk.md)).

- Следующие пакеты должны быть установлены (через [диспетчер пакетов SDK для Android](~/android/get-started/installation/android-sdk.md)): 
  - **Средства пакета SDK версии 26.1.1** или более поздней
  - **Средства платформы пакета SDK для Android 28.0.1** или более поздней версии 
  - **Средства сборки пакета SDK для Android 26.0.3** или более поздней версии

  Эти пакеты должны отображаться с состоянием **Установлено**, как показано на следующем снимке экрана:

  [![Установка Android SDK Tools](device-manager-images/mac/02-sdk-tools-sml.png)](device-manager-images/mac/02-sdk-tools.png#lightbox)

## <a name="launching-the-device-manager"></a>Запуск диспетчера устройств

Запустите Android Device Manager, выбрав **Сервис > Диспетчер устройств**:

[![Запуск диспетчера устройств из меню "Сервис"](device-manager-images/mac/03-tools-menu-sml.png)](device-manager-images/mac/03-tools-menu.png#lightbox)

Если вы видите следующее диалоговое окно ошибки при запуске, см. инструкции по обходу в разделе [Устранение неполадок](#troubleshooting):

![На снимке экрана показано диалоговое окно ошибки экземпляра пакета SDK для Android для устранения неполадок.](device-manager-images/mac/04-sdk-instance-error.png)

## <a name="main-screen"></a>Главный экран

При первом запуске диспетчер устройств Android выводит экран, где отображаются все настроенные виртуальные устройства. Для каждого виртуального устройства указаны параметры **Имя**, **ОС** (версия Android), **Процессор**, **Память**, а также **Разрешение** экрана:

[![Список установленных устройств и их параметров](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

Если выбрать устройство в списке, справа отображается кнопка **Воспроизведение**. Можно нажать кнопку **Воспроизведение**, чтобы запустить эмулятор с этим виртуальным устройством:

[![Кнопка "Воспроизведение" для образа устройства](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

После запуска эмулятора с выбранным виртуальным устройством кнопка **Воспроизвести** изменяется на кнопку **Остановить**, позволяющую остановить эмулятор:

[![Кнопка "Остановить" для работающего устройства](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

При остановке эмулятора может появиться запрос с вопросом, следует ли сохранить текущее состояние для следующей быстрой загрузки:

![Диалоговое окно "Сохранить текущее состояние для быстрой загрузки"](device-manager-images/mac/08-save-for-quick-boot-m76.png)

Сохранение текущего состояния позволит эмулятору загрузиться быстрее при повторном запуске этого виртуального устройства. Дополнительные сведения о быстрой загрузке см. в разделе [Быстрая загрузка](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot).

### <a name="new-device"></a>Создание устройства

Чтобы создать устройство, нажмите кнопку **Создать устройство** в верхней левой части экрана:

[![Кнопка "Создать" для создания устройства](device-manager-images/mac/09-new-button-sml.png)](device-manager-images/mac/09-new-button.png#lightbox)

При нажатии кнопки **Создать устройство** открывается экран **Новое устройство**:

[![Экран "Новое устройство" диспетчера устройств](device-manager-images/mac/10-new-device-editor-sml.png)](device-manager-images/mac/10-new-device-editor.png#lightbox)

Чтобы настроить новое устройство на экране **Новое устройство**, выполните следующие действия:

1. Задайте новое имя для устройства. В следующем примере новое устройство называется **Pixel_API_27**:

   [![Именование нового устройства](device-manager-images/mac/11-device-name-m76-sml.png)](device-manager-images/mac/11-device-name-m76.png#lightbox)

2. Выберите физическое устройство для эмуляции, щелкнув раскрывающееся меню **Базовое устройство**:

   [![Выбор физического устройства для эмуляции](device-manager-images/mac/12-device-menu-m76-sml.png)](device-manager-images/mac/12-device-menu-m76.png#lightbox)

3. Выберите тип процессора для этого виртуального устройства, щелкнув раскрывающееся меню **Процессор**. **x86** обеспечивает максимальную производительность, так как позволяет эмулятору воспользоваться преимуществами [аппаратного ускорения](~/android/get-started/installation/android-emulator/hardware-acceleration.md).
   Вариант **X86_64** также использует аппаратное ускорение, но работает немного медленнее, чем **x86** (**x86_64** обычно используется для тестирования 64-разрядных приложений):

   [![Выбор типа процессора](device-manager-images/mac/13-processor-type-menu-m76-sml.png)](device-manager-images/mac/13-processor-type-menu-m76.png#lightbox)

4. Выберите версию Android (уровень API), щелкнув раскрывающееся меню **ОС**. Например, выберите **Oreo 8.1 — API 27** для создания виртуального устройства для API уровня 27:

   [![Выбор версии Android](device-manager-images/mac/14-android-screenshot-m76-sml.png)](device-manager-images/mac/14-android-screenshot-m76.png#lightbox)

   Если выбран Android (уровень API), который еще не был установлен, диспетчер устройств выдаст сообщение **Новое устройство будет загружено** в нижней части экрана &ndash; он загрузит и установит необходимые файлы, создавая новое виртуальное устройство:

   ![Будет скачан новый образ устройства](device-manager-images/mac/15-automatic-download-m76.png)

5. Если вы хотите включить API сервисов Google Play на виртуальном устройстве, включите параметр **API Google**. Чтобы включить приложение Google Play Маркет, включите параметр **Google Play Маркет**:

   [![Выбор сервисов Google Play и Google Play Маркет](device-manager-images/mac/16-google-play-services-m76-sml.png)](device-manager-images/mac/16-google-play-services-m76.png#lightbox)

   Обратите внимание, что образы Google Play Маркет доступны только для некоторых типов базовых устройств, например Pixel, Pixel 2, Nexus 5 и Nexus 5X.

6. Измените нужные свойства. Сведения о том, как это сделать, см. в разделе [Изменение свойств виртуальных устройств Android](~/android/get-started/installation/android-emulator/device-properties.md).

7. Добавьте любые дополнительные свойства, которые нужно задать явно. На экране **Новое устройство** перечислены только часто изменяемые свойства, но можно щелкнуть раскрывающееся меню **Добавление свойства** в нижней части, чтобы добавить дополнительные свойства:

   [![Раскрывающееся меню "Добавление свойства"](device-manager-images/mac/17-add-property-menu-m76-sml.png)](device-manager-images/mac/17-add-property-menu-m76.png#lightbox)

   Можно также определить пользовательское свойство, выбрав **Пользовательский...** в верхней части списка свойств.

8. Нажмите кнопку **Создать** в нижнем правом углу, чтобы создать устройство:

   ![Кнопка "Создать"](device-manager-images/mac/18-create-button-m76.png)

9. Пока устройство создается, Android Device Manager добавляет его в список установленных виртуальных устройств, отображая индикатор хода выполнения **Создание**:

   [![Индикатор хода выполнения "Создание"](device-manager-images/mac/19-creating-the-device-m76-sml.png)](device-manager-images/mac/19-creating-the-device-m76.png#lightbox)

10. После завершения создания новое устройство отображается в списке установленных виртуальных устройств с кнопкой **Запустить**, готовое к запуску:

    [![Только что созданное устройство, готовое к запуску](device-manager-images/mac/20-created-device-m76-sml.png)](device-manager-images/mac/20-created-device-m76.png#lightbox)

### <a name="edit-device"></a>Изменение устройства

Для изменения существующего виртуального устройства щелкните раскрывающееся меню **Дополнительные параметры** (значок шестеренки) и выберите **Изменить**:

[![Пункт меню "Изменить" для изменения нового устройства](device-manager-images/mac/21-edit-button-m76-sml.png)](device-manager-images/mac/21-edit-button-m76.png#lightbox)

При щелчке элемента **Изменить** запускается редактор устройств для выбранного виртуального устройства:

[![Окно редактора устройств](device-manager-images/mac/22-device-editor-sml.png)](device-manager-images/mac/22-device-editor.png#lightbox)

На экране **Редактор устройств** указаны свойства виртуального устройства в столбце **Свойство** с соответствующими значениями каждого свойства в столбце **Значение**. При выборе свойства справа отображается его подробное описание.

Чтобы изменить свойство, измените его значение в столбце **Значение**.
Например, на следующем снимке экрана свойство `hw.lcd.density` изменяется с **480** на **240**:

[![Пример изменения устройства](device-manager-images/mac/23-device-editing-sml.png)](device-manager-images/mac/23-device-editing.png#lightbox)

После внесения необходимых изменений в конфигурацию нажмите кнопку **Сохранить**.
Дополнительные сведения об изменении свойств виртуального устройства см. в разделе [Изменение свойств виртуальных устройств Android](~/android/get-started/installation/android-emulator/device-properties.md).

### <a name="additional-options"></a>Дополнительные параметры

Дополнительные параметры для работы с устройством доступны в раскрывающемся меню, расположенном слева от кнопки **Воспроизвести**:

[![Расположение меню с дополнительными параметрами](device-manager-images/mac/24-overflow-menu-sml.png)](device-manager-images/mac/24-overflow-menu.png#lightbox)

Меню с дополнительными параметрами содержит следующие элементы:

- **Изменить** — открывает выбранное устройство в редакторе устройств, как описано выше.

- **Duplicate and Edit** (Дублировать и править) — дублирует выбранное устройство и открывает экран **Новое устройство** с другим уникальным именем. Например, если выбрать **Pixel 2 API 28** и щелкнуть **Скопировать и изменить**, к имени добавляется счетчик:

  [![Экран "Duplicate and Edit" (Дублировать и править)](device-manager-images/mac/25-dupe-and-edit-sml.png)](device-manager-images/mac/25-dupe-and-edit.png#lightbox)

- **Отобразить в Finder** — открывает окно macOS Finder, в котором показана папка с файлами для виртуального устройства. Например, если выбрать **Pixel 2 API 28** и щелкнуть **Отобразить в средстве поиска**, открывается окно, похожее на следующее:

  [![Результаты выбора параметра "Отобразить в средстве поиска"](device-manager-images/mac/26-reveal-in-finder-sml.png)](device-manager-images/mac/26-reveal-in-finder.png#lightbox)

- **Сброс параметров** — восстанавливает стандартные настройки выбранного устройства, удаляя любые изменения, внесенные пользователем во внутреннее состояние устройства во время его работы (также удаляется любой актуальный моментальный снимок [быстрой загрузки](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot) при его наличии). Эта операция не затрагивает изменения, внесенные на виртуальном устройстве во время его создания и редактирования. Отображается диалоговое окно с напоминанием, что этот сброс невозможно отменить. Нажмите кнопку **Сброс до заводских настроек** для подтверждения сброса.

  ![Диалоговое окно "Сброс до заводских настроек"](device-manager-images/mac/27-factory-reset-m76.png)

- **Удалить** — удаляет выбранное виртуальное устройство без возможности восстановления. Отображается диалоговое окно с напоминанием, что такое удаление невозможно отменить. Щелкните **Удалить**, если уверены, что устройство нужно удалить.

  ![Диалоговое окно "Удалить устройство"](device-manager-images/mac/28-delete-device-m76.png)

-----

<a name="troubleshooting"></a>

## <a name="troubleshooting"></a>Устранение неполадок

В следующих разделах описано, как диагностировать и устранять неполадки, которые могут возникать при использовании Android Device Manager для настройки виртуальных устройств.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

### <a name="android-sdk-in-non-standard-location"></a>Пакет SDK для Android в нестандартном расположении

Как правило, пакет SDK для Android устанавливается в следующем расположении:

**C:\\Program Files (x86)\\Android\\android-sdk**

Если пакет SDK не установлен в этом расположении, при запуске Android Device Manager может возникнуть следующая ошибка:

![Ошибка экземпляра SDK для Android](device-manager-images/win/29-sdk-error.png)

Чтобы решить эту проблему, выполните следующие действия:

1. На рабочем столе Windows перейдите в **C:\\Users\\*имя_пользователя*\\AppData\\Roaming\\XamarinDeviceManager**:

   ![Расположение файла журнала для Android Device Manager](device-manager-images/win/30-log-files.png)

2. Дважды щелкните, чтобы открыть один из файлов журнала, и найдите **Путь к файлу конфигурации**. Пример:

   [![Путь к файлу конфигурации в файле журнала](device-manager-images/win/31-config-file-path-sml.png)](device-manager-images/win/31-config-file-path.png#lightbox)

3. Перейдите в это расположение и дважды щелкните **user.config**, чтобы открыть его.

4. В **user.config** найдите элемент `<UserSettings>` и добавьте к нему атрибут **AndroidSdkPath**. Задайте для этого атрибута путь, по которому установлен пакет SDK для Android на вашем компьютере, и сохраните файл. Например, `<UserSettings>` будет выглядеть следующим образом, если пакет SDK для Android был установлен в **C:\\Programs\\Android\\SDK**:

   ```xml
   <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:ProgramsAndroidSDK" />
   ```

Внесение этого изменения в **user.config** позволит запустить Android Device Manager.

### <a name="wrong-version-of-android-sdk-tools"></a>Неправильная версия Android SDK Tools

При отсутствии Android SDK Tools версии 26.1.1 или более поздней при запуске может появиться следующее диалоговое окно ошибки:

![На снимке экрана показано диалоговое окно ошибки экземпляра пакета SDK для Android.](device-manager-images/win/32-sdk-instance-error.png)

Если вы видите это диалоговое окно ошибки, щелкните **ОК**, чтобы открыть диспетчер пакетов SDK для Android. В диспетчере пакетов SDK для Android откройте вкладку **Сервис** и установите следующие пакеты:

- **Android SDK Tools 26.1.1** или более поздней версии
- **Средства платформы пакета SDK для Android 27.0.1** или более поздней версии
- **Средства сборки пакета SDK для Android 27.0.3** или более поздней версии

### <a name="snapshot-disables-wifi-on-android-oreo"></a>При создании моментального снимка в Android Oreo отключается Wi-Fi

Если для Android Oreo настроено виртуальное устройство AVD с имитацией доступа по Wi-Fi, перезапуск AVD после создания моментального снимка может привести к отключению доступа по Wi-Fi.

Действия по решению этой проблемы:

1. Выберите AVD в Android Device Manager.

2. В меню дополнительных параметров выберите пункт **Отобразить в проводнике**.

3. Последовательно выберите **snapshots (моментальные снимки) > default_boot**.

4. Удалите файл **snapshot.pb**:

   ![Расположение файла snapshot.pb](device-manager-images/win/33-delete-snapshot.png)

5. Перезапустите AVD.

После внесения этих изменений AVD перезапустится в состоянии, которое позволяет возобновить работу Wi-Fi.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

### <a name="wrong-version-of-android-sdk-tools"></a>Неправильная версия Android SDK Tools

При отсутствии Android SDK Tools версии 26.1.1 или более поздней при запуске может появиться следующее диалоговое окно ошибки:

![Диалоговое окно ошибки экземпляра SDK для Android](device-manager-images/mac/29-sdk-instance-error.png)

Если вы видите это диалоговое окно ошибки, нажмите кнопку **ОК**, чтобы открыть диспетчер пакетов SDK для Android. В диспетчере пакетов SDK для Android откройте вкладку **Сервис** и установите следующие пакеты:

- **Android SDK Tools 26.1.1** или более поздней версии
- **Средства платформы пакета SDK для Android 28.0.1** или более поздней версии
- **Средства сборки пакета SDK для Android 26.0.3** или более поздней версии

### <a name="snapshot-disables-wifi-on-android-oreo"></a>При создании моментального снимка в Android Oreo отключается Wi-Fi

Если для Android Oreo настроено виртуальное устройство AVD с имитацией доступа по Wi-Fi, перезапуск AVD после создания моментального снимка может привести к отключению доступа по Wi-Fi.

Действия по решению этой проблемы:

1. Выберите AVD в Android Device Manager.

2. В меню дополнительных параметров выберите пункт **Отобразить в средстве поиска**.

3. Последовательно выберите **snapshots (моментальные снимки) > default_boot**.

4. Удалите файл **snapshot.pb**:

   [![Расположение файла snapshot.pb](device-manager-images/mac/30-delete-snapshot-sml.png)](device-manager-images/mac/30-delete-snapshot.png#lightbox)

5. Перезапустите AVD.

После внесения этих изменений AVD перезапустится в состоянии, которое позволяет возобновить работу Wi-Fi.

-----

### <a name="generating-a-bug-report"></a>Создание отчета об ошибках

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Если вы обнаружили проблему с Android Device Manager, которую не удается разрешить с помощью приведенных выше советов, отправьте отчет об ошибках. Для этого щелкните правой кнопкой мыши строку заголовка и выберите пункт **Generate Bug Report** (Создать отчет об ошибках):

[![Расположение пункта меню для заполнения отчета об ошибках](device-manager-images/win/34-bug-report-sml.png)](device-manager-images/win/34-bug-report.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Если вы обнаружили проблему с Android Device Manager, которую не удается решить с помощью приведенных выше советов, отправьте отчет об ошибках. Для этого последовательно выберите **Справка > Сообщить о проблеме**:

[![Расположение пункта меню для заполнения отчета об ошибках](device-manager-images/mac/31-bug-report-sml.png)](device-manager-images/mac/31-bug-report.png#lightbox)

::: zone-end

## <a name="summary"></a>Сводка

Это руководство содержит общие сведения об Android Device Manager, доступном в Инструментах Visual Studio для Xamarin и Visual Studio для Mac. Оно описывает важные функции, такие как запуск и остановка эмулятора Android, выбор виртуального устройства Android (AVD) для запуска, создание и изменение виртуальных устройств. В нем рассказывается, как редактировать свойства профиля оборудования для дальнейшей настройки, и приводятся советы по устранению распространенных неполадок.

## <a name="related-links"></a>Связанные ссылки

- [Изменения в инструментарии пакета SDK для Android](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Отладка приложений в Android Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)
- [Заметки о выпуске пакета SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/How-to-Create-and-Manage-Your-Own-Android-Emulators/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
