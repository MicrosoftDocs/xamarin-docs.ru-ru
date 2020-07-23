---
title: Файл iTunesMetadata.plist в приложениях Xamarin.iOS
description: Статья описывает файл iTunesMetadata.plist, который используется для предоставления информации iTunes о приложении при распространении напрямую для тестирования или корпоративного развертывания.
ms.prod: xamarin
ms.assetid: 70676eba-6a99-4a3a-bccc-84359fe9c2c3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 76f5a10720505796e30079d6be3506bc66636f7e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938129"
---
# <a name="the-itunesmetadataplist-file-in-xamarinios-apps"></a>Файл iTunesMetadata.plist в приложениях Xamarin.iOS

_В статье описывается файл iTunesMetadata.plist, который предоставляет в iTunes информацию о приложении при прямом распространении для тестирования или корпоративного развертывания._

При создании приложения в iTunes Connect (для продажи или для бесплатного распространения через iTunes App Store) разработчик может указать сведения, такие как жанр приложения, поджанр, уведомление об авторских правах, поддерживаемые устройства iOS и требуемые параметры устройства. В распространяемых напрямую приложениях iOS, предназначенных для тестировщиков или корпоративных пользователей, эта информация отсутствует.

Для предоставления этой информации при распространении напрямую можно создать и включить в IPA необязательный файл `iTunesMetadata.plist`. PLIST-файл представляет собой XML-файл со специальным форматированием (подробные сведения см. в разделе [Руководство по программированию списка свойств](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html) Apple), который содержит пары "ключ-значение", задающие сведения об этом приложении iOS.

<a name="iTunesMetadata_contents"></a>

## <a name="the-itunesmetadataplist-contents"></a>Содержимое файла iTunesMetadata.plist

Ниже представлен типовой пример файла `iTunesMetadata.plist`, используемого для предоставления iTunes сведений при прямом распространении:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>UIRequiredDeviceCapabilities</key>
    <dict>
        <key>armv7</key>
        <true/>
        <key>front-facing-camera</key>
        <true/>
    </dict>
    <key>artistName</key>
    <string>Company, Inc.</string>
    <key>bundleDisplayName</key>
    <string>App Name</string>
    <key>bundleShortVersionString</key>
    <string>1.5.1</string>
    <key>bundleVersion</key>
    <string>1.5.1</string>
    <key>copyright</key>
    <string>© 2015 Company, Inc.</string>
    <key>drmVersionNumber</key>
    <integer>0</integer>
    <key>fileExtension</key>
    <string>.app</string>
    <key>gameCenterEnabled</key>
    <false/>
    <key>gameCenterEverEnabled</key>
    <false/>
    <key>genre</key>
    <string>Games</string>
    <key>genreId</key>
    <integer>6014</integer>
    <key>itemName</key>
    <string>App Name</string>
    <key>kind</key>
    <string>software</string>
    <key>playlistArtistName</key>
    <string>Company, Inc.</string>
    <key>playlistName</key>
    <string>App Name</string>
    <key>releaseDate</key>
    <string>2015-11-18T03:23:10Z</string>
    <key>s</key>
    <integer>143441</integer>
    <key>softwareIconNeedsShine</key>
    <false/>
    <key>softwareSupportedDeviceIds</key>
    <array>
        <integer>9</integer>
    </array>
    <key>softwareVersionBundleId</key>
    <string>com.company.appid</string>
    <key>subgenres</key>
    <array>
        <dict>
            <key>genre</key>
            <string>Puzzle</string>
            <key>genreId</key>
            <integer>7012</integer>
        </dict>
        <dict>
            <key>genre</key>
            <string>Word</string>
            <key>genreId</key>
            <integer>7019</integer>
        </dict>
    </array>
    <key>versionRestrictions</key>
    <integer>16843008</integer>
</dict>
</plist>

```

Значения каждого ключа будут подробно рассмотрены ниже.

### <a name="uirequireddevicecapabilities"></a>UIRequiredDeviceCapabilities

Ключ `UIRequiredDeviceCapabilities` сообщает iTunes, какие функции должно поддерживать конкретное устройство для установки приложения на это устройство iOS. Ключ состоит из словаря (`<dict>...</dict>`) функций (`<key>...</key>`), а также логического значения для каждой функции. Если значение для функции `true`, то наличие этой функции необходимо. Если `false`, то эта функция должна отсутствовать в устройстве. Пример:

```xml
<key>UIRequiredDeviceCapabilities</key>
<dict>
    <key>armv7</key>
    <true/>
    <key>front-facing-camera</key>
    <true/>
</dict>
```

Указывает на то, что для установки данного приложения на устройство это устройство iOS должно поддерживать набор инструкций ARM7 и иметь фронтальную камеру. Полный список допустимых значений см. в разделе [UIRequiredDeviceCapabilities](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW3) документации корпорации Apple.

### <a name="artistname-and-playlistartistname"></a>artistName и playlistArtistName

В ключах `artistName` и `playlistArtistName` указывается название компании, создавшей приложение. Название будет отображаться в iTunes. Пример.

```xml
<key>artistName</key>
<string>Company, Inc.</string>
...
<key>playlistArtistName</key>
<string>Company, Inc.</string>
```

### <a name="bundledisplayname-itemname-and-playlistname"></a>bundleDisplayName, itemName и playlistName

В ключах `bundleDisplayName`, `itemName` и `playlistName` указывается имя приложения iOS, которое будет отображаться в iTunes. Пример.

```xml
<key>bundleDisplayName</key>
<string>App Name</string>
...
<key>itemName</key>
<string>App Name</string>
...
<key>playlistName</key>
<string>App Name</string>
```

### <a name="bundleshortversionstring-and-bundleversion"></a>bundleShortVersionString и bundleVersion

В ключах `bundleShortVersionString` и `bundleVersion` указывается номер версии приложения iOS, который отображается в iTunes. Пример.

```xml
<key>bundleShortVersionString</key>
<string>1.5.1</string>
<key>bundleVersion</key>
<string>1.5.1</string>
```

### <a name="softwareversionbundleid"></a>softwareVersionBundleId

В ключе `softwareVersionBundleId` указывается идентификатор пакета приложения. Пример.

```xml
<key>softwareVersionBundleId</key>
<string>com.company.appid</string>
```

### <a name="copyright"></a>авторские права

В ключе `copyright`указывается уведомление об авторских правах, которое отображается в iTunes. Пример.

```xml
<key>copyright</key>
<string>© 2015 Company, Inc.</string>
```

### <a name="releasedate"></a>releaseDate

В ключе `releaseDate` указывается дата выпуска приложения iOS. Дата выпуска отображается в iTunes. Пример.

```xml
<key>releaseDate</key>
<string>2015-11-18T03:23:10Z</string>
```

### <a name="softwareiconneedsshine"></a>softwareIconNeedsShine

В ключе `softwareIconNeedsShine` указывается, требуется ли _подсветка_ значка приложения в iOS 6 (и ранее). Пример.

```xml
<key>softwareIconNeedsShine</key>
<false/>
```

### <a name="gamecenterenabled-and-gamecentereverenabled"></a>gameCenterEnabled и gameCenterEverEnabled

Ключи `gameCenterEnabled` и `gameCenterEverEnabled` сообщают iTunes, поддерживает ли это приложение Apple Game Center. Пример.

```xml
<key>gameCenterEnabled</key>
<false/>
<key>gameCenterEverEnabled</key>
<false/>
```

### <a name="genre-genreid-and-subgenres"></a>genre, genreId и subgenres

Ключи `genre` и `genreId` сообщают iTunes, к какому жанру относится приложение. Пример.

```xml
<key>genre</key>
<string>Games</string>
<key>genreId</key>
<integer>6014</integer>
```

Дополнительно можно использовать ключ `subgenres` для определения до двух поджанров приложения iOS. Пример.

```xml
<key>subgenres</key>
<array>
    <dict>
        <key>genre</key>
        <string>Puzzle</string>
        <key>genreId</key>
        <integer>7012</integer>
    </dict>
    <dict>
        <key>genre</key>
        <string>Word</string>
        <key>genreId</key>
        <integer>7019</integer>
    </dict>
</array>
```

Apple определяет следующие жанры и идентификаторы жанров для приложений iOS:

[!include[](~/ios/includes/table-appstore.md)]

### <a name="softwaresupporteddeviceids"></a>softwareSupportedDeviceIds

Ключ `softwareSupportedDeviceIds` сообщает iTunes, какие устройства с iOS поддерживает это приложение. Пример.

```xml
<key>softwareSupportedDeviceIds</key>
<array>
    <integer>9</integer>
</array>
```

Допустимы следующие значения:

- 1 — классические iPhone
- 2 — iPod Touch
- 4 — iPad
- 9 — современные iPhone

### <a name="standard-keys"></a>Стандартные ключи

Следующие ключи содержатся во всех файлах `iTunesMetadata.plist` всех приложений iOS и всегда имеют одни и те же значения:

```xml
<key>drmVersionNumber</key>
<integer>0</integer>
<key>fileExtension</key>
<string>.app</string>
...
<key>kind</key>
<string>software</string>
...
<key>s</key>
<integer>143441</integer>
...
<key>versionRestrictions</key>
<integer>16843008</integer>
```

<a name="iTunesMetadata_creating"></a>

## <a name="creating-an-itunesmetadataplist-file"></a>Создание файла iTunesMetadata.plist

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

 При работе с файлом `iTunesMetadata.plist` в Visual Studio для Mac возможны два варианта:

- Создать и настроить файл а визуальном редакторе файлов plist Visual Studio для Mac.
- Создать и настроить файл в текстовом редакторе.

 Ниже описываются оба способа.

### <a name="using-the-visual-plist-editor"></a>Использование визуального редактора файлов plist

Выполните следующие действия:

1. В **Обозревателе решений** щелкните правой кнопкой на файле проекта Xamarin.iOS и выберите **Добавить** > **Новый файл...**
2. В диалоговом окне создания файла выберите **iOS** > **Список свойств**:

    ![Выберите список свойств iOS](itunesmetadata-images/image01.png)
3. Введите в поле **Имя** значение `iTunesMetadata` и нажмите кнопку **Новый**.
4. Для редактирования файла дважды щелкните на файле `iTunesMetadata.plist` в **Обозревателе решений**:

    ![Редактор файла iTunesMetadata.plist](itunesmetadata-images/image02.png)
5. Нажмите зеленый **+** для создания новой записи и введите `UIRequiredDeviceCapabilities` в качестве имени ключа:

    ![Создайте запись и введите UIRequiredDeviceCapabilities в качестве имени ключа](itunesmetadata-images/image03.png)
6. Нажмите на тип значение **Строка** и выберите **Словарь** в раскрывающемся списке:

    ![Выберите "Словарь" в раскрывающемся списке](itunesmetadata-images/image04.png)
7. Нажмите стрелку вниз слева от имени свойства, чтобы показать элементы словаря:

    ![Раскройте элементы словаря](itunesmetadata-images/image05.png)
8. Нажмите на надпись **Добавить запись**, затем нажмите зеленый **+** для добавления записи в словарь:

    ![Добавьте запись в словарь](itunesmetadata-images/image06.png)
9. Введите `armv7` в качестве имени ключа, выберите тип **Логическое** и введите **Да** в качестве значения:

    ![Введите armv7 в качестве имени ключа, выберите тип "Логическое" и введите "Да" в качестве значения](itunesmetadata-images/image07.png)
10. Повторяйте эти шаги, пока не заполните все необходимые пары ключ/значение в файле `iTunesMetadata.plist` (подробные сведения см. в разделе [Содержимое файла iTunesMetadata.plist](#iTunesMetadata_contents)).

11. Сохраните изменения в файле plist.

### <a name="using-a-plain-text-editor"></a>Использование текстового редактора

Выполните следующие действия:

1. Создайте в текстовом редакторе новый файл и назовите его `iTunesMetadata.plist`.
2. Скопируйте пример содержимого файла из раздела [Содержимое файла iTunesMetadata.plist](#iTunesMetadata_contents) выше.
3. Вставьте содержимое в файл и отредактируйте его под свои нужды.
4. Сохраните файл и вернитесь в Visual Studio для Mac.
5. В **Обозревателе решений** щелкните правой кнопкой на файле проекта Xamarin.iOS и выберите **Добавить** > **Существующие файлы...** .
6. В диалоговом окне открытия файла выберите файл `iTunesMetadata.plist`, который мы создали выше, и нажмите кнопку **OK**.
7. Оставьте значение поля **Действие при построении** в значении **Нет**.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Плагин Xamarin для Visual Studio поддерживает только визуальный редактор для файлов `Info.plist` и `Entitlement.plist`, поэтому вам нужно создать файл `iTunesMetadata.plist` в обычном текстовом редакторе и вручную включить его в проект Xamarin.iOS.

Выполните следующие действия:

1. Создайте в текстовом редакторе новый файл и назовите его `iTunesMetadata.plist`.
2. Скопируйте пример содержимого файла из раздела [Содержимое файла iTunesMetadata.plist](#iTunesMetadata_contents) выше.
3. Вставьте содержимое в файл и отредактируйте его под свои нужды.
4. Сохраните файл и вернитесь в Visual Studio.
5. В **Обозревателе решений** щелкните правой кнопкой на файле проекта Xamarin.iOS и выберите **Добавить** > **Существующие файлы...** .
6. В диалоговом окне открытия файла выберите файл `iTunesMetadata.plist`, который мы создали выше, и нажмите кнопку **Открыть**.
7. Оставьте значение поля **Действие при построении** в значении **Нет**.

-----

Позднее выберите файл `iTunesMetadata.plist` при подготовке к сборке IPA-файла в среде разработки.

## <a name="summary"></a>Сводка

Статья описывает файл `iTunesMetadata.plist`, который используется для предоставления информации iTunes о распространяемом напрямую приложении iOS. Приведено описание стандартных ключей в файле plist, а также процесс создания и настройки файла в Visual Studio и Visual Studio для Mac.

## <a name="related-links"></a>Связанные ссылки

- [Распространение через Магазин приложений](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Настройка приложения в iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Публикация в App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Внутреннее распространение](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Прямое распространение](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Поддержка IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Устранение неполадок](~/ios/deploy-test/troubleshooting.md)
