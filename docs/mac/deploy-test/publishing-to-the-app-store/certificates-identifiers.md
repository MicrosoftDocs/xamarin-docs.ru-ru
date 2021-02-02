---
title: Сертификаты и идентификаторы в Xamarin.Mac
description: В этом руководстве описывается создание необходимых сертификатов и идентификаторов, которые потребуются для публикации приложения Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 12/17/2019
ms.openlocfilehash: ae62a695670f50c5385b9279c6d6de79b6d59fb7
ms.sourcegitcommit: 513feb0e07558766e3de4a898e53d56b27c20559
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697570"
---
# <a name="certificates-and-identifiers-in-xamarinmac"></a>Сертификаты и идентификаторы в Xamarin.Mac

_В этом руководстве описывается создание необходимых сертификатов и идентификаторов, которые потребуются для публикации приложения Xamarin.Mac._

## <a name="setup"></a>Установка

Чтобы настроить компьютер Mac для разработки, посетите [Apple Developer Member Center](https://developer.apple.com). Щелкните ссылку на **учетную запись** и выполните вход. Главное меню показано ниже:

> [!div class="mx-imgBorder"]
> [![Центр разработчиков Apple](certificates-identifiers-images/devcenter01.png)](certificates-identifiers-images/devcenter01-large.png#lightbox)

Нажмите кнопку **Certificates, Identifiers & Profiles** (Сертификаты, идентификаторы и профили) или кнопку "плюс" возле заголовка **Certificates** (Сертификаты):

> [!div class="mx-imgBorder"]
> [![Выбор параметра Certificates, Identifiers & Profiles (Сертификаты, идентификаторы и профили)](certificates-identifiers-images/devcenter02.png)](certificates-identifiers-images/devcenter02-large.png#lightbox)

Выберите тип сертификата и щелкните **Continue** (Продолжить):

> [!div class="mx-imgBorder"]
> [![Переход по ссылке Certificates (Сертификаты)](certificates-identifiers-images/devcenter03.png)](certificates-identifiers-images/devcenter03-large.png#lightbox)

В открывшемся окне можно при необходимости скачать **промежуточные сертификаты** (центры сертификации Worldwide Developer Relations и Developer ID) (последний элемент внизу страницы). Однако эти сертификаты должны быть автоматически настроены средой Xcode.

Далее в этом разделе подробно описаны действия, необходимые для разработчиков приложений Mac:

- **Register Mac App ID** (Регистрация идентификатора приложения Mac) — разработчику необходимо выполнить эти действия для каждого создаваемого приложения.
- **Register macOS Systems** (Регистрация систем macOS) — требуется только при добавлении компьютеров для тестирования.
- **Создание сертификатов** требуется выполнить при настройке сертификатов и выполнять в дальнейшем при их продлении.
- **Create Provisioning Profile** (Создание профиля подготовки) — разработчику необходимо выполнять эти действия для каждого создаваемого приложения, а также при добавлении новых систем.

## <a name="register-mac-app-id"></a>Регистрация идентификатора приложения Mac

Для каждого приложения необходимо регистрировать идентификатор. Чтобы создать запись, выполните указанные ниже действия:

1. Выберите знак "плюс" (+) или **Register an App ID** (Регистрация идентификатора приложения):

    > [!div class="mx-imgBorder"]
    > [![Снимок экрана: область с информацией о начале работы с идентификаторами приложений на странице Certificates, Identifiers and Profiles (Сертификаты, идентификаторы и профили).](certificates-identifiers-images/appid01.png)](certificates-identifiers-images/appid01-large.png#lightbox)

1. Выберите **App IDs** (Идентификаторы приложения).

    > [!div class="mx-imgBorder"]
    > [![Снимок экрана: параметр регистрации нового идентификатора.](certificates-identifiers-images/appid02.png)](certificates-identifiers-images/appid02-large.png#lightbox)

1. Введите **описание** и выберите любые **службы приложений**, которые потребуются приложению. Необходимо использовать платформу **macOS**. Выберите **описание** (используемое только на этом портале). Введите **идентификатор пакета**, который должен соответствовать файлу **Info.plist**. Выберите необходимые для приложения возможности.

    > [!div class="mx-imgBorder"]
    > [![Ввод описания и выбор служб приложений](certificates-identifiers-images/appid03.png)](certificates-identifiers-images/appid03-large.png#lightbox)

    Нажмите кнопку **Continue** (Продолжить), чтобы просмотреть выбранные параметры.

1. Если все правильно, нажмите кнопку **Register** (Зарегистрировать), чтобы завершить установку:

    > [!div class="mx-imgBorder"]
    > [![Проверка введенных данных](certificates-identifiers-images/appid04.png)](certificates-identifiers-images/appid04-large.png#lightbox)

1. Проверьте сведения и нажмите кнопку **Submit** (Отправить):

    > [!div class="mx-imgBorder"]
    > ![Проверка сведений](certificates-identifiers-images/appid05.png)

Некоторые **службы приложений** могут требовать дальнейшей настройки (например, iCloud). В этом случае выберите только что созданный идентификатор приложения и нажмите кнопку **Edit** (Изменить):

> [!div class="mx-imgBorder"]
> [![Изменение нового идентификатора приложения](certificates-identifiers-images/appid06.png)](certificates-identifiers-images/appid06-large.png#lightbox)

Чтобы настроить службы iCloud, например, нажмите кнопку **Edit** (Изменить):

> [!div class="mx-imgBorder"]
> [![Настройка служб iCloud](certificates-identifiers-images/appid07.png)](certificates-identifiers-images/appid07-large.png#lightbox)

## <a name="register-macos-devices"></a>Регистрация устройств macOS

Чтобы создать профиль подготовки для тестирования, разработчику необходимо зарегистрировать свои компьютеры Mac. Для проверки можно зарегистрировать не более 100 компьютеров.

1. В центре разработчика Mac щелкните ссылку **All** (Все) в разделе **Devices** (Устройства) и нажмите кнопку **+** :

    > [!div class="mx-imgBorder"]
    > [![Добавление нового компьютера](certificates-identifiers-images/device01.png)](certificates-identifiers-images/device01-large.png#lightbox)

1. Введите **имя** и идентификатор **UUID** добавляемого компьютера, а затем нажмите кнопку **Continue** (Продолжить). Проверьте сведения и нажмите кнопку **Register** (Зарегистрировать):

    > [!div class="mx-imgBorder"]
    > [![Снимок экрана: страница регистрации нового устройства, на которой можно ввести имя и UUID.](certificates-identifiers-images/device02.png)](certificates-identifiers-images/device02-large.png#lightbox)

1. Проверьте и подтвердите введенные данные:

    > [!div class="mx-imgBorder"]
    > [![Снимок экрана: страница регистрации нового устройства, на которой можно подтвердить имя и UUID.](certificates-identifiers-images/device03.png)](certificates-identifiers-images/device03-large.png#lightbox)

## <a name="create-certificates"></a>Создание сертификатов

В разделе Certificates (Сертификаты) можно создать несколько разных типов сертификатов, которые будут использоваться для подписывания приложений Mac:

> [!div class="mx-imgBorder"]
> [![Создание сертификата](certificates-identifiers-images/devcenter04.png)](certificates-identifiers-images/devcenter04-large.png#lightbox)

Существует 5 основных типов сертификатов, соответствующих потребностям разработчиков macOS:

- **Сертификат разработки Mac** — необязателен в общем случае, но требуется, если разработчик планирует использовать такие функции, как iCloud или push-уведомления. Сертификат разработки необходим для создания профилей подготовки, которые предоставляют доступ к этим функциям.
- **Сертификат распространения приложений Mac** — разработчику потребуется один такой сертификат для приложения и еще один — для установщика.
- **Сертификат распространения установщика Mac** — разработчику потребуется один такой сертификат для приложения и еще один — для установщика.
- **Сертификат идентификатора разработчика для установщика** — сертификаты установщика для распространения за пределами Mac App Store.
- **Сертификат идентификатора разработчика приложения** — сертификаты приложения для распространения за пределами Mac App Store.

В следующих разделах приводятся примеры создания этих типов сертификатов.

### <a name="mac-development-certificate"></a>Сертификат разработки Mac

Как было сказано ранее, сертификат разработки Mac требуется, только если используются такие функции macOS, как iCloud или push-уведомления.

Чтобы создать сертификат разработки, выполните указанные ниже действия:

1. Установите переключатель в положение **Mac Development** (Разработка Mac) и нажмите кнопку **Continue** (Продолжить):

    > [!div class="mx-imgBorder"]
    > [![Добавление сертификата разработки](certificates-identifiers-images/certif02.png)](certificates-identifiers-images/certif02-large.png#lightbox)

1. Отправьте _запрос подписания сертификата_. Файл запроса сертификата (с расширением `.certSigningRequest`) будет сохранен на локальном компьютере Mac. Щелкните **Choose file** (Выбрать файл), чтобы выбрать запрос сертификата, а затем — **Continue** (Продолжить).

    > [!div class="mx-imgBorder"]
    > [![Отправка файла запроса сертификата](certificates-identifiers-images/certif03.png)](certificates-identifiers-images/certif03-large.png#lightbox)

    Сведения об использовании программы [Keychain Access](https://help.apple.com/developer-account/#/devbfa00fef7) для создания файла запроса сертификата см. по **этой ссылке**.

1. Выберите **Download** (Скачать), чтобы получить файл сертификата, и дважды щелкните его, чтобы установить:

    > [!div class="mx-imgBorder"]
    > [![Скачивание файла сертификата](certificates-identifiers-images/certif04.png)](certificates-identifiers-images/certif04-large.png#lightbox)

Как упоминалось ранее, сертификат разработчика требуется не всегда, а только если разработчик внедряет такие функции macOS, как iCloud и push-уведомления. Он также необходим для создания **профиля подготовки к разработке**, который требуется для тестирования приложений Mac App Store.

### <a name="mac-app-store-certificates"></a>Сертификаты Mac App Store

Для выпуска приложения в App Store необходимо два сертификата:

- сертификат **распространения приложений Mac**, используемый для подписывания приложения;
- сертификат **распространения установщика Mac**, чтобы подписать установщик.

> [!TIP]
> При присвоении имен запросам сертификатов используйте описательные имена, содержащие текст `Application` и `Installer`, чтобы их можно было определить позднее.

Сначала создайте сертификат установщика:

1. Выберите тип сертификата **распространения установщика Mac** и нажмите кнопку **Continue** (Продолжить):

    > [!div class="mx-imgBorder"]
    > [![Создание сертификата App Store](certificates-identifiers-images/certif05.png)](certificates-identifiers-images/certif05-large.png#lightbox)

1. На следующей странице объясняется, как использовать **доступ к связке ключей** для создания файла запроса сертификата. Следуйте инструкциям:

    > [!div class="mx-imgBorder"]
    > [![Отправка запроса сертификата](certificates-identifiers-images/certif06.png)](certificates-identifiers-images/certif06-large.png#lightbox)

    Сведения об использовании программы [Keychain Access](https://help.apple.com/developer-account/#/devbfa00fef7) для создания файла запроса сертификата см. по **этой ссылке**. Не забудьте выбрать имя сертификата, соответствующее его _типу_ (приложение или установщик).

1. Нажмите кнопку **Download** (Скачать), чтобы получить сертификат, и дважды щелкните его, чтобы установить его в **связке ключей**:

    > [!div class="mx-imgBorder"]
    > [![Скачивание сертификата App Store](certificates-identifiers-images/certif07.png)](certificates-identifiers-images/certif07-large.png#lightbox)

**Выполните те же действия для сертификата распространения приложений Mac.**

![Сертификат распространения приложений Mac](certificates-identifiers-images/certif08.png)

### <a name="developer-id-certificates"></a>Сертификаты идентификации разработчиков

Для самостоятельного выпуска приложения Xamarin.Mac (не через Apple App Store) вам потребуется два сертификата:

- сертификат **идентификатора разработчика для установщика**, используемый для подписывания приложения;
- сертификат **идентификатора разработчика приложения**, чтобы подписать установщик.

> [!TIP]
> При присвоении имен запросам сертификатов используйте описательные имена, содержащие текст `Application` и `Installer`, чтобы их можно было определить позднее.

После создания, скачивания и установки сертификатов они будут отображаться в программе **Keychain Access**:

[Список сертификатов в программе Keychain Access](certificates-identifiers-images/certif09.png)

## <a name="related-links"></a>Связанные ссылки

- [Установка](/visualstudio/mac/installation/)
- [Пример кода приложения "Привет, Mac"](~/mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Идентификатор разработчика и привратник](https://developer.apple.com/developer-id/)
