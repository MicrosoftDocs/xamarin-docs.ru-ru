---
title: Apple Pay в Xamarin. iOS
description: В этом руководством рассматривается настройка среды Xamarin. iOS для использования с Apple Pay для оплаты физических товаров, таких как еда, развлечения и членство в приложении. Он содержит сведения о необходимых идентификаторах, сертификатах и правах.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: 51a18042bea820221f450d8d7ceedf3f69359a6d
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431633"
---
# <a name="apple-pay-in-xamarinios"></a>Apple Pay в Xamarin. iOS

_В этом руководством рассматривается настройка среды Xamarin. iOS для использования с Apple Pay для оплаты физических товаров, таких как еда, развлечения и членство в приложении. Он содержит сведения о необходимых идентификаторах, сертификатах и правах._

Apple Pay была введена вместе с iOS 8, позволяя пользователям платить за физические товары, такие как еда, развлечения и членство через свои устройства iOS. Он доступен на iPhone 6 и iPhone 6 Plus и может также составлять пару с Apple Watch для покупок в магазине. При использовании на iPhone он использует сенсорный идентификатор как способ подтверждения и авторизации транзакций на кредитную или дебетовую карту пользователя.

## <a name="requirements"></a>Требования

Apple Pay доступен только в iOS 8 и более поздних версиях и поэтому требует минимум Xcode 6.

Следующие элементы также необходимы для интеграции Apple Pay в приложение:

- Платформа платежных процессоров
- Идентификатор продавца
- Сертификат Apple Pay
- Назначение Apple Pay

В этом документе более подробно рассматриваются эти элементы.

## <a name="differences-between-apple-pay-and-iap"></a>Различия между Apple Pay и IAP

Основное различие между Apple Pay и *покупкой в приложении* (IAP) относится к продуктам, которые они продают. *Физические* товары продаются через Apple Pay; Все эти примеры описаны в еда, проживание и физические развлечения (например, билеты в кинотеатрах). В отличие от этого, IAP продает *Виртуальные* товары, такие как Premium или дополнительное содержимое, и подписки — Подумайте о дополнительных месяцах службы потоковой передачи или в игре.

Используемые платформы также являются ключевым отличием; [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) используется для Apple Pay, а [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) предоставляет API инфраструктуры для IAP.

С Apple Pay Apple [утверждает](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) , что "[не взимается] пользователи, продавцы или разработчики для использования Apple Pay для платежей". В отличие от этого, IAP имеет 30% оплаты за каждую транзакцию. Кроме того, при использовании Apple Pay транзакция не проходит через Apple, вместо этого она проходит через платежную платформу.

## <a name="using-a-payment-processor-platform"></a>Использование платформы платежных процессоров

Одной из основных частей Apple Pay является обработка платежей. Хотя это можно сделать самостоятельно, оно требует значительных знаний о криптографии, как описано [в этом разделе.](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html)
С другой стороны, платформы обработки платежей обрабатывают эти операции, позволяя сосредоточиться на создании приложения.

Есть два варианта:

- Создание **чередующихся** подписок на [stripe.com](https://stripe.com/) для доступа к их API.

- **Как judopay** — извлеките свой [пример кода Xamarin на GitHub](https://github.com/Judopay/Xamarin-Sample-App)и зарегистрируйтесь по адресу [JudoPay.com](https://www.judopay.com/).

## <a name="provisioning-for-apple-pay"></a>Подготовка для Apple Pay

Настройка приложения для использования Apple Pay требует установки на портале разработчика Apple и в приложении. Для успешной подготовки приложения для Apple платите необходимо выполнить ряд действий.

1. Создайте идентификатор продавца:
    - Выполните действия, описанные [здесь](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid) .
2. Создайте идентификатор приложения с возможностью применить оплату и добавьте в него оптовый магазин:
    - Выполните действия, описанные [здесь](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid) .
3. Создайте сертификат для идентификатора продавца:
    - Выполните действия, описанные [здесь](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate) .
4. Создайте профиль подготовки с созданным ИДЕНТИФИКАТОРом приложения:
    - Выполните действия, описанные [здесь](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning) .
5. Добавьте Apple Payные права:
    - Выберите отправку Apple Purchase, как описано [здесь](~/ios/deploy-test/provisioning/entitlements.md), или вручную добавьте пару "ключ-значение" в файл [отсюда](~/ios/deploy-test/provisioning/entitlements.md)

## <a name="working-with-apple-pay"></a>Работа с Apple Pay

Компания Apple предоставила несколько усовершенствований Apple Pay в iOS 10, которые позволяют пользователю выполнять безопасных платежей с веб-сайтов и через взаимодействие с Siri и картами.

В iOS 10 Добавлено несколько новых API, которые работают с iOS и watchOS для поддержки динамических платежных сетей и новой тестовой среды "песочницы".

### <a name="apple-pay-website-integration"></a>Интеграция Apple Pay веб-сайтов

В iOS 10 разработчик может включать Apple Pay непосредственно в свои веб-сайты с помощью **APPLEPAY JS**. Пользователи, просматривающие веб-сайт с помощью Safari в iOS или macOS, могут выполнять платежи с Apple Pay, проверяя транзакцию на iPhone или Apple Watch. Дополнительные сведения см. в [справочнике по платформе Apple APPLEPAY JP](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>Усовершенствования платформы PassKit

В iOS 10 платформа PassKit была расширена для поддержки Apple Pay вне `UIKit` и позволяет поставщикам карт предоставлять собственные карты в своих приложениях.

#### <a name="supporting-apple-pay-outside-of-uikit"></a>Поддержка Apple Pay за пределами UIKit

С помощью [пкпайментаусоризатионконтроллер](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) и [пкпайментаусориксатионконтроллерделегате](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)приложение может поддерживать те же функциональные возможности, что и [пкпайментаусоризатионвиевконтроллер](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) , без использования UIKit. Хотя этот новый API необходим для поддержки Apple Pay Apple Watch (а также в конкретных целях), он является необязательным в других ситуациях (например, в существующих приложениях). Однако компания Apple предлагает как можно быстрее перейти на новый API, чтобы обеспечить широкую Apple Payную поддержку во всех приложениях разработчика с единой базой кода. Дополнительные сведения об целей и интеграции Siri см. в документации по [SiriKit](~/ios/platform/sirikit/index.md) .

#### <a name="presenting-issuer-cards-from-within-apps"></a>Представление карточек поставщика в приложениях

В iOS 10 в платформу PassKit были добавлены новые функции, позволяющие поставщикам карт предоставлять свои карты в своих приложениях. Разработчик может добавить `PKPaymentButtonTypeInStore` уибуттон в пользовательский интерфейс приложения, который будет отображать кнопку Apple Pay для карточки.

`PresentPaymentPass`Метод класса [пкпасслибрари](https://developer.apple.com/reference/passkit/pkpasslibrary) также можно использовать для программного вывода карты.

### <a name="new-payment-network-support"></a>Поддержка новой оплаты по сети

До iOS 10 приложение может автоматически поддерживать новую платежную сеть, когда она станет доступной, без необходимости изменения, повторной компиляции приложения и повторной отправки в App Store.

Новый метод [аваилабленетворкс](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) `PKPaymentNetwork` класса позволяет приложению обнаруживать сети, доступные на устройстве пользователя во время выполнения. Кроме того, свойство [суппортеднетворкс](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) было расширено, чтобы взять имя поставщика платежа в качестве аргумента. Используя эти методы, приложение может автоматически поддерживать любую сеть, поддерживаемую поставщиком платежей.

Дополнительные сведения см. в нашей [Apple Pay конфигурации](~/ios/platform/apple-pay.md) и в разделе [руководств по Apple Pay](https://developer.apple.com/apple-pay/)Apple.

### <a name="new-testing-environment"></a>Новая тестовая среда

В iOS 10 компания Apple представила новую тестовую среду, позволяющую разработчику подготавливать проверочные платежные карты непосредственно на устройстве iOS. Затем эта новая тестовая среда возвращает в приложение зашифрованные проверочные данные.

Чтобы включить новую среду тестирования, выполните следующие действия.

1. Создайте новую тестовую учетную запись iCloud в iTunes Connect.
2. Войдите на устройство iOS с помощью новой учетной записи тестирования.
3. Задайте нужный регион для тестирования приложения в.
4. Используйте одну из платежных [Apple Pay](https://developer.apple.com/apple-pay/) карт для создания платежей.

> [!IMPORTANT]
> При переключении учетных записей iCloud устройство автоматически переключается на новую среду тестирования. Тем не менее Apple по-прежнему **требует** , чтобы приложение проверялось на реальные карты в рабочей среде перед отправкой в магазин приложений iTunes.

## <a name="summary"></a>Сводка

В этой статье мы изучили различные элементы, необходимые для использования Apple Pay в приложении. Мы рассмотрели, как создать идентификатор продавца и как он используется в рамках прав **. plist**, который необходимо изменить вручную.

## <a name="related-links"></a>Связанные ссылки

- [Покупки из приложений](~/ios/platform/in-app-purchasing/index.md)
- [Введение в PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Емпориум (пример)](/samples/xamarin/ios-samples/ios9-emporium)