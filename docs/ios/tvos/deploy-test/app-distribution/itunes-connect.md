---
title: Настройка приложения tvOS в iTunes Connect
description: В этой статье представлено дополнительное руководство по настройке приложения iOS в iTunes Connect для конфигураций, связанных с tvOS.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: b316e0629bce66e295241be9d67dc4eb37ce1972
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939572"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>Настройка приложения tvOS в iTunes Connect

_В этой статье представлено дополнительное руководство по настройке приложения iOS в iTunes Connect для конфигураций, связанных с tvOS._

Помимо конфигураций и настроек, которые необходимо выполнить, следуя указаниям в руководстве по [настройке приложения iOS в iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) , в этом документе рассматриваются конкретные конфигурации, которые потребуется для выпуска приложения Xamarin. TvOS в магазине приложений Apple TV.

<a name="Adding-a-tvOS-Release-Version"></a>

## <a name="adding-a-tvos-release-version"></a>Добавление версии выпуска tvOS

Если вы создаете новое приложение для выпуска в магазине приложений Apple TV или добавляете поддержку Apple TV в существующее приложение iOS, вам потребуется создать запись iTunes Connect и настроить ее с помощью следующих руководств по работе с iOS:

- [Создание записи iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Управление видео и снимками экрана приложения](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [Управление именем, описанием, сведениями о новых возможностях, ключевыми словами и URL-адресами](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [Обслуживание общих сведений](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

При необходимости также может потребоваться:

- [Сведения Game Center](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [Сведения о покупках из приложения](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

Выполнив все описанные выше действия, откройте запись iTunes Connect и выберите Добавить поддержку tvOS на боковой панели слева:

[![Добавление поддержки tvOS с помощью боковой панели слева](itunes-connect-images/connect01.png)](itunes-connect-images/connect01.png#lightbox)

Затем будут доступны экраны конкретной информации tvOS для данной записи iTunes Connect:

[![Экран сведений о конкретной tvOS](itunes-connect-images/connect02.png)](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information"></a>

## <a name="tvos-version-information"></a>Сведения о версии tvOS

На боковой панели слева выберите **1,0 Подготовка к отправке** в разделе приложение tvOS:

[![Сведения о версии tvOS](itunes-connect-images/connect03.png)](itunes-connect-images/connect03.png#lightbox)

На этом экране укажите следующие сведения:

- Необходимые снимки экрана, описание, ключевые слова и URL-адреса.
- Общие сведения о приложении, такие как номер версии, авторские права и возрастная категория.
- Необязательные покупки в приложении.
- Необязательная поддержка Game Center с списки лидеров и достижениями.
- Необходимые сведения о проверке приложения, такие как контакты, демонстрационные учетные записи и заметки.

После введения необходимых сведений нажмите кнопку **сохранить** в правом верхнем углу экрана, чтобы сохранить изменения:

[![Сведения о версии tvOS, готовые к отправке](itunes-connect-images/connect04.png)](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review"></a>

## <a name="preparing-to-submit-for-review"></a>Подготовка к отправке для проверки

Когда вы готовы отправить ваше приложение Xamarin. tvOS в магазин приложений Apple TV для проверки, вернитесь в запись iTunes Connect приложения и нажмите кнопку **Отправить для проверки** в правом верхнем углу экрана:

[![Отправить для проверки](itunes-connect-images/connect05.png)](itunes-connect-images/connect05.png#lightbox)

<a name="Summary"></a>

## <a name="summary"></a>Итоги

В этой статье представлен обзор настроек tvOS, необходимых в iTunes Connect для выпуска приложения tvOS в Apple TV App Store.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
