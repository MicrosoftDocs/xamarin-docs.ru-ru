---
title: Дополнительные изменения в watchOS 3 Framework
description: В этом документе описаны различные изменения инфраструктуры, появившиеся в watchOS 3, а также работа с ними в Xamarin. Обсуждаются основные данные, основные движения, Foundation, HealthKit, HomeKit, PassKit и UIKit.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 50f671febdc698454dd5bc7e00d040672c7a79af
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436526"
---
# <a name="additional-watchos-3-frameworks-changes"></a>Дополнительные изменения в watchOS 3 Framework

_В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих платформ для watchOS 3._

В дополнение к основным изменениям в iOS компания Apple внесла изменения в несколько существующих платформ в watchOS 3.

## <a name="core-data"></a>Основные данные

В основную платформу данных для наблюдения за OS 3 были внесены следующие улучшения.

- Корневые объекты [нсманажедобжектконтекст](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) поддерживают одновременную ошибку и выборку без сериализации.
- Класс [нсперсистентсторекурдинатор](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) поддерживает пул хранилищ данных SQLite.
- [Нсманажедобжектконтекст](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) объекты с хранилищами данных SQLite в режиме журнала Wal поддерживают новую функцию создания запросов, где контексты управляемых объектов (MOC) могут быть закреплены в конкретных версиях базы данных для последующей выборки и сбоя транзакций.
- Использование высокого уровня `NSPersistenceContainer` для ссылки на `NSPersistentStoreCoordinator` [Нсманажедобжектмодел](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) и другие основные ресурсы по настройке данных.
- Было добавлено несколько новых удобных методов для `NSManagedObject` упрощения выборки и создания подклассов.

Дополнительные сведения см. в [справочнике по основной платформе данных](https://developer.apple.com/reference/coredata)Apple.

## <a name="core-motion"></a>Основное движение

В основную платформу Motion для наблюдения за OS 3 были внесены следующие улучшения.

- Новое событие Motion устройства использует акселерометр и гироскопом для предоставления обновлений движения и ориентации. Приложение может зарегистрироваться для этого обновления (в тарифах до 100Hz).
- Новое событие Педометер позволяет быстро получать уведомления в режиме реального времени при приостановке и возобновлении работы пользователя. Используйте [кмпедометер](https://developer.apple.com/reference/coremotion/cmpedometer) для регистрации в качестве основного или фонового события педометер.

## <a name="foundation"></a>Foundation

В базовую платформу для наблюдения за OS 3 были внесены следующие улучшения.

- Новый класс [нсдатеинтервал](https://developer.apple.com/reference/foundation/nsdateinterval) используется для вычисления интервалов даты и времени, таких как длительность, для сравнения интервалов и тестирования для пересечения интервалов.
- К классу [нслокал](https://developer.apple.com/reference/foundation/nslocale) были добавлены несколько новых свойств для получения локальных сведений и доступных форматов отображаемых данных.
- Используйте новый класс [нсмеасуремент](https://developer.apple.com/reference/foundation/nsmeasurement) для преобразования между разными единицами измерения (ЕИ) или вычисления значений в разных уомс.
- Используйте новый класс [нсмеасурементформаттер](https://developer.apple.com/reference/foundation/nsmeasurementformatter) , чтобы отформатировать локализованные измерения для отображения конечному пользователю.
- Используйте новые классы [нсунит](https://developer.apple.com/reference/foundation/nsunit) и [нсдименсион](https://developer.apple.com/reference/foundation/nsdimension) для представления конкретных уомс.

## <a name="healthkit"></a>HealthKit

В HealthKit Framework для наблюдения за OS 3 были внесены следующие улучшения.

- Используйте новый класс [хкворкаутконфигуратион](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) , чтобы указать `ActivityType` и `LocationType` для тренировки.
- [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) `WheelchairUse` Для работы с коляска связанными данными о работоспособности были добавлены новые хквхилчаирусеобжект и метод класса [хкхеалссторе](https://developer.apple.com/reference/healthkit/hkhealthstore) .
- Добавлены новые ключи метаданных для типов погоды (например `HKWeatherConditionClear` , и `HKWeatherConditionCloudy` ), а также были добавлены типы тренировок (такие как `HKWorkoutActivityTypeFlexibility` и `HKWorkoutActivityTypeWheelchairRunPace` ).

## <a name="homekit"></a>HomeKit

В HomeKit Framework для наблюдения за OS 3 были внесены следующие улучшения.

- Добавлена возможность просмотра и взаимодействия с HomeKit подключенными IP-камерами.
- Добавлены несколько новых служб и характеристик.
- Добавлен дополнительный контекст и конфигурация основных служб и служб Link Services.

## <a name="passkit"></a>PassKit

В PassKit Framework для наблюдения за OS 3 были внесены следующие улучшения.

- Расширяет платформу для поддержки безопасных платежей в приложении на Apple Watch как физических товаров, так и услуг.
- Теперь доступны следующие классы: [пкпаймент](https://developer.apple.com/reference/passkit/pkpayment), [пкпайментмесод](https://developer.apple.com/reference/passkit/pkpaymentmethod), [пкпайментрекуест](https://developer.apple.com/reference/passkit/pkpaymentrequest) и [пкпайменттокен](https://developer.apple.com/reference/passkit/pkpaymenttoken)

## <a name="uikit"></a>UIKit

В UIKit Framework для наблюдения за OS 3 были внесены следующие улучшения.

- Для поддержки динамического типа в метках текстовые поля и текстовые поля используют новый `PreferredFontForTextStyle` метод `UIFont` класса.
- `ColorWithDisplayP3`Метод добавлен для поддержки расширенного цвета.

## <a name="related-links"></a>Связанные ссылки

- [Примеры watchOS](/samples/browse/?products=xamarin&term=Xamarin.iOS%2bwatchos)
- [Новые возможности в watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)