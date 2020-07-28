---
title: Внутреннее распространение для приложений Xamarin.iOS
description: Этот документ содержит краткий обзор внутреннего распространения приложений внутри организации в рамках участия в программе Apple Enterprise Developer Program.
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 1c70aca4812214b424820ecb5a769a871e7e703c
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937908"
---
# <a name="in-house-distribution-for-xamarinios-apps"></a>Внутреннее распространение для приложений Xamarin.iOS

_Этот документ содержит краткий обзор распространения приложений внутри организации в рамках участия в программе Apple Enterprise Developer Program._

После разработки приложения Xamarin.iOS наступает следующий этап жизненного цикла разработки ПО — распространение приложения среди пользователей. Собственные приложения можно распространять *внутри организации* (прежнее название — корпоративные) по **программе Apple Developer Enterprise Program**, которая предоставляет следующие преимущества:

- Приложение не нужно отправлять на рассмотрение в Apple.
- Нет никаких ограничений на количество устройств, где можно развернуть приложение
  - Следует отметить, что Apple ясно дает понять, что собственные приложения предназначены только для внутреннего использования.

Также важно отметить, что программа Enterprise Program:

- не предоставляет доступ к iTunes Connect для распространения или тестирование (включая TestFlight);
- требует оплаты ежегодного членства в размере 299 USD.

Все приложения по-прежнему должны быть подписаны Apple.

<a name="testing"></a>

## <a name="testing-your-application"></a>Тестирование приложения

Тестирование приложения осуществляется с помощью прямого распространения. Для получения дополнительных сведений о тестировании см. в руководстве по [прямому распространению](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). Имейте в виду, что в тестировании может принимать участие не больше 100 устройств.

<a name="setup"></a>

## <a name="getting-set-up-for-distribution"></a>Настройка для распространения

Как и в других программах для разработчиков Apple, создавать сертификаты распространения и профили подготовки в рамках программы Apple Developer Enterprise Program могут только агенты и администраторы команды.

Срок действия сертификатов Apple Developer Enterprise Program составляет три года, а профилей подготовки — один год.

Важно отметить, что сертификаты с истекшим сроком действия невозможно обновить, вместо этого потребуется заменить такой сертификат новым, как описано [ниже](#certificate).

<a name="certificate"></a>

## <a name="creating-a-distribution-certificate"></a>Создание сертификата распространения

1. Перейдите к разделу *Certificates, Identifiers & Profiles* (Сертификаты, профили и идентификаторы) в Центре разработчиков Apple.
2. В разделе *Certificates* (Сертификаты) выберите **Production** (Производство).
3. Нажмите кнопку **+** , чтобы создать новый сертификат.
4. В разделе *Production* (Производство) установите флажок **In-House and Ad Hoc** (Собственный и прямой):

   [![Выбор элемента In-House and Ad Hoc (Собственный и прямой)](in-house-distribution-images/createcertmanually01.png)](in-house-distribution-images/createcertmanually01.png#lightbox)

5. Нажмите кнопку "Continue" (Продолжить) и следуйте инструкциям, чтобы создать запрос подписи сертификата через доступ к цепочке ключей:

   [![Создание запроса подписи сертификата через доступ к цепочке ключей](in-house-distribution-images/createcertmanually02.png)](in-house-distribution-images/createcertmanually02.png#lightbox)

6. После создания запроса согласно инструкции нажмите кнопку "Continue" (Продолжить) и отправьте его в центр участников:

   [![Отправка запроса подписи сертификата в центр участников](in-house-distribution-images/createcertmanually03.png)](in-house-distribution-images/createcertmanually03.png#lightbox)

7. Нажмите "Generate" (Создать), чтобы создать сертификат.
8. Скачайте готовый сертификат и дважды щелкните файл, чтобы установить его.
9. На этом этапе сертификат должен быть установлен на компьютере, однако может потребоваться обновить профили, чтобы убедиться, что они видны в Xcode.

Можно также запросить сертификат в Xcode через диалоговое окно "Preferences" (Параметры). Выполните указанные ниже действия:

1. Выберите свою команду и щелкните *View Details* (Показать подробности):

   [![Выбор своей команды](in-house-distribution-images/selectteam.png)](in-house-distribution-images/selectteam.png#lightbox)

2. Затем нажмите кнопку **Create** (Создать) рядом с полем **iOS Distribution Certificate** (Сертификат распространения iOS):

   [![Создание сертификата распространения iOS](in-house-distribution-images/selectcert.png)](in-house-distribution-images/selectcert.png#lightbox)

3. Далее щелкните **знак "плюс" (+)** и выберите пункт **iOS App Store**:

   [![Выбор пункта "iOS App Store"](in-house-distribution-images/selectcert.png)](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile"></a>

## <a name="creating-a-distribution-provisioning-profile"></a>Создание профиля подготовки распространения

<a name="appid"></a>

### <a name="creating-an-app-id"></a>Создание идентификатора приложения

Как и в случае с любым другим создаваемым профилем подготовки, идентификатор приложения требуется для идентификации приложения, которое будет распространяться на устройство пользователя. Если вы еще не создали идентификатор, выполните следующие действия, чтобы создать его:

1. В [центре разработчиков Apple](https://developer.apple.com/account/overview.action) перейдите в раздел *Certificates, Identifiers and Profiles* (Сертификаты, идентификаторы и профили). В разделе **Identifiers** (Идентификаторы) выберите элемент **App IDs** (ИД приложений).
2. Нажмите кнопку **+** и укажите **Name** (Имя) для идентификации приложения на портале.
3. Префикс приложения уже должен быть задан в качестве идентификатора вашей команды. Изменить его невозможно. Выберите "Explicit" (Явный) или "Wildcard App ID" (Шаблон ИД приложения), а затем введите идентификатор пакета (имя DNS в обратном порядке): **Explicit**: com.[доменное_имя].[имя_приложения] **Wildcard**:com.[доменное_имя].*
4. Выберите любые [службы приложений](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services), которые требуются вашему приложению.
5. Нажмите кнопку **Continue** (Продолжить) и следуйте инструкциям на экране, чтобы создать идентификатор приложения.

Получив компоненты, необходимые для создания профиля распределения, выполните следующие действия, чтобы создать его:

1. Вернитесь на портал подготовки Apple и выберите **Provisioning** > **Distribution** (Подготовка > Распространение):

   [![Выбор элемента "Provisioning" (Подготовка) > "Distribution" (Распространение)](in-house-distribution-images/distribute01.png)](in-house-distribution-images/distribute01.png#lightbox)

2. Нажмите кнопку **+** и выберите тип профиля распространения, который нужно создать для **внутреннего распространения**:

   [![Создание профиля внутреннего распространения](in-house-distribution-images/distribute02.png)](in-house-distribution-images/distribute02.png#lightbox)

3. Нажмите кнопку **Continue** (Продолжить) и выберите из раскрывающегося списка ИД приложения, для которого необходимо создать профиль распространения:

   [![Выбор идентификатора приложения в раскрывающемся списке](in-house-distribution-images/distribute03.png)](in-house-distribution-images/distribute03.png#lightbox)

4. Нажмите кнопку **Continue** (Продолжить) и выберите сертификат распространения, необходимый для подписи приложения:

   [![Выбор сертификата распространения, необходимого для подписи приложения](in-house-distribution-images/distribute04.png)](in-house-distribution-images/distribute04.png#lightbox)

5. Нажмите кнопку **Continue** (Продолжить) и введите **имя** нового профиля распространения:

   [![Ввод имени нового профиля распространения](in-house-distribution-images/distribute06.png)](in-house-distribution-images/distribute06.png#lightbox)

6. Нажмите кнопку **Generate** (Создать), чтобы создать профиль и завершить процесс.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

 Может потребоваться выйти из программы Visual Studio для Mac, а также обновить список доступных удостоверений подписи и профилей подготовки в Xcode (в соответствии с инструкциями из раздела [Запрос удостоверений подписывания](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), прежде чем новый профиль распространения станет доступен в Visual Studio для Mac.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Может потребоваться выйти из программы Visual Studio, а также обновить список доступных удостоверений подписи и профилей подготовки в Xcode на Mac узла сборки (в соответствии с инструкциями из раздела [Запрос удостоверений подписывания](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), прежде чем новый профиль распространения станет доступен в Visual Studio.

-----

<a name="inhouse"></a>

## <a name="distributing-your-app-in-house"></a>Распространение приложения внутри организации

В рамках программы Apple Developer Enterprise Program лицензиатом является лицо, ответственное за распространение приложения, а также за соблюдение [рекомендаций](https://developer.apple.com/programs/enterprise/) компании Apple.

Ваше приложение можно безопасно распространять самыми разными способами, например:

- Локально через iTunes
- Сервер MDM
- Внутренний безопасный веб-сервер
- Адрес эл. почты

Чтобы распространить приложение любым из этих способов, нужно сначала создать файл IPA, как описано в следующем разделе.

### <a name="creating-an-ipa-for-in-house-deployment"></a>Создание IPA для внутреннего развертывания

После подготовки приложения можно упаковать в файл, называемый *IPA*. Это ZIP-файл, содержащий приложение, а также дополнительные метаданные и значки. IPA используется для добавления приложения локально в iTunes, чтобы его можно было синхронизировать непосредственно на устройство, включенное в профиль подготовки.

Дополнительные сведения о создании IPA см. в руководстве [Поддержка IPA](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="summary"></a>Сводка

В этой статье представлен краткий обзор распространения приложений Xamarin.iOS внутри организации.

## <a name="related-links"></a>Связанные ссылки

- [Распространение через Магазин приложений](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Прямое распространение](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Файл iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Поддержка IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
