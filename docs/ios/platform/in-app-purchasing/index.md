---
title: Покупка в приложении в Xamarin. iOS
description: В этом документе описывается Продажа цифровых продуктов и служб с помощью API-интерфейсов StoreKit. Он содержит ссылки на руководства, которые обсуждают конфигурацию, потребляемые продукты, непотребленные продукты, транзакции, подписки и многое другое.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 7e061f12e45e6d60c1772fc8699ffdb1f1b1d730
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430400"
---
# <a name="in-app-purchasing-in-xamarinios"></a>Покупка в приложении в Xamarin. iOS

приложения iOS могут продавать цифровые продукты или службы с помощью StoreKit — набор API-интерфейсов, предоставляемых iOS, которые взаимодействуют с серверами Apple для проведения финансовых транзакций с пользователем по идентификатору Apple ID. API-интерфейсы StoreKit в основном связаны с получением сведений о продукте и проведении транзакций — нет компонента пользовательского интерфейса. Приложения, реализующие приобретение в приложении, должны создавать собственный пользовательский интерфейс и отслеживанию приобретенных элементов с помощью пользовательского кода, чтобы предоставить пользователю необходимые продукты или услуги.

Для предоставления функций приобретения в приложении необходимо выполнить ряд действий.

- **Настройка приложения** — профиль подготовки приложения должен быть правильно настроен.
- **Создание продуктов** — описания и цены продукта должны быть созданы на портале iTunes Connect.
- **Реализация StoreKit** — API StoreKit должен быть реализован в соответствии с типами продаваемых продуктов.
- **Создание пользовательского интерфейса и самих продуктов** — продукты должны быть реализованы, включая механизмы для отслеживания каждой покупки и резервного копирования и восстановления при необходимости.
- **Мониторинг продаж и получения фондов** — использование информации, предоставляемой iTunes Connect, для мониторинга тенденций продаж и отслеживания дохода.

В этом документе объясняется, как выполнить все эти действия для предоставления покупок из приложения с помощью Xamarin. iOS.

## <a name="requirements"></a>Требования

Для поддержки покупок в приложении необходимо использовать Xamarin. iOS 5,0 или более поздней версии с Xcode 7 и более поздних версий.

## <a name="contents"></a>Содержимое

- [Основные сведения о покупке из приложения и ее настройка](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

- [StoreKit обзор и получение сведений о продуктах](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

- [Приобретение готовых к использованию продуктов](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

- [Приобретение неготовых к использованию продуктов](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

- [Транзакции и проверки](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

- [Подписки и отчетность](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>Сводка

В этой статье представлено понятие приобретения в приложении, в котором описано, как настроить приложение, чтобы воспользоваться его преимуществами и представить примеры с помощью Xamarin. iOS. Он охватывает следующие темы:

- **портал подготовки iOS** — рекомендации по включению функций приобретения в приложении.
- **iTunes Connect** — Настройка продуктов для продажи в приложении.
- **Комплект магазина** — объяснение классов, используемых для создания функций приобретения в приложении.
- **Написание кода приложения для приобретения** — примеры создания покупки из приложения в приложении Xamarin. iOS.
- **Отчеты** — обзор статистики, доступной через iTunes Connect.

## <a name="related-links"></a>Связанные ссылки

- [инапппурчасесампле](/samples/xamarin/ios-samples/storekit/)
- [В статье по программированию покупки приложения](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [Руководство по iTunes Connect для разработчиков](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Справочник по платформе Store Kit](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Идентификаторы продуктов для покупки в приложении Q&A](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Техническое примечание о приобретении в приложении](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Ваши первые отправки магазина приложений](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [Советы по отправке в App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Руководство по оценке приложений App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Управление приложениями](https://developer.apple.com/appstore/resources/managing/index.html)