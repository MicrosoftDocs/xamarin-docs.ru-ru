---
title: Предоставление обратной связи Хаптик в Xamarin. iOS
description: В этом документе описывается, как обеспечить обратную связь хаптик в приложении Xamarin. iOS. В нем обсуждаются Уиимпактфидбаккженератор, Уинотификатионфидбаккженератор и Уиселектионфидбаккженератор.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: e6879db1631bc6b58a36142344ff1fba997ee4fb
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91432044"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Предоставление обратной связи Хаптик в Xamarin. iOS

<a name="Overview"></a>

## <a name="overview"></a>Обзор

На iPhone 7 и iPhone 7 Plus компания Apple включила новые ответы хаптик, которые предоставляют дополнительные способы физического привлечения пользователя. Обратная связь хаптик (часто именуемая просто AS Хаптикс) использует сенсорное касание (с помощью Force, вибрации или движения) в разработке пользовательского интерфейса. Используйте эти новые параметры обратной связи тактиле для получения внимания пользователя и подготовьте свои действия.

Будут подробно рассмотрены следующие темы:

- [О Хаптик отзыва](#About-Haptic-Feedback)
- [уиимпактфидбаккженератор](#UIImpactFeedbackGenerator)
- [уинотификатионфидбаккженератор](#UINotificationFeedbackGenerator)
- [уиселектионфидбаккженератор](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback"></a>

## <a name="about-haptic-feedback"></a>О Хаптик отзыва

Несколько встроенных элементов пользовательского интерфейса уже предоставляют хаптик отзывы, такие как подборки, переключатели и ползунки. iOS 10 теперь добавляет возможность программного запуска хаптикс с помощью конкретного подкласса `UIFeedbackGenerator` класса.

Разработчик может использовать один из следующих `UIFeedbackGenerator` подклассов для программного запуска обратной связи хаптик:

- `UIImpactFeedbackGenerator` — Используйте этот генератор обратной связи, чтобы дополнить действие или задачу, например выпустить "суд" при наличии слайдов представления или при конфликте двух объектов на экране.
- `UINotificationFeedbackGenerator` — Используйте этот генератор обратной связи для таких уведомлений, как завершение действия, сбой или предупреждение любого другого типа.
- `UISelectionFeedbackGenerator` — Используйте этот генератор обратной связи для изменяемого выбора, например комплектации элемента из списка.

<a name="UIImpactFeedbackGenerator"></a>

### <a name="uiimpactfeedbackgenerator"></a>уиимпактфидбаккженератор

Используйте этот генератор обратной связи, чтобы дополнить действие или задачу, например выпустить «суд» при наличии слайдов представления или при конфликте двух объектов на экране.

Используйте следующий код, чтобы активировать отзыв о влиянии:

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Когда разработчик создает новый экземпляр `UIImpactFeedbackGenerator` класса, он предоставляет `UIImpactFeedbackStyle` стойкость отзыва следующим образом:

- `Heavy`
- `Medium`
- `Light`

`Prepare`Метод `UIImpactFeedbackGenerator` вызывается для информирования системы о том, что хаптик обратная связь, чтобы она могла максимально сокращать задержку.

`ImpactOccurred`Затем метод активирует хаптик отзыв.

<a name="UINotificationFeedbackGenerator"></a>

### <a name="uinotificationfeedbackgenerator"></a>уинотификатионфидбаккженератор

Используйте этот генератор обратной связи для таких уведомлений, как завершение действия, сбой или предупреждение любого другого типа.

Чтобы активировать отзыв уведомлений, используйте следующий код:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Создается новый экземпляр `UINotificationFeedbackGenerator` класса и `Prepare` вызывается его метод для информирования системы о том, что хаптик обратная связь, чтобы она могла максимально сокращать задержку.

`NotificationOccurred`Вызывается для инициации обратной связи хаптик с заданным `UINotificationFeedbackType` :

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator"></a>

### <a name="uiselectionfeedbackgenerator"></a>уиселектионфидбаккженератор

Используйте этот генератор обратной связи для активно изменяемого выбора, например комплектации элемента из списка.

Используйте следующий код, чтобы активировать отзыв о выборе:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Создается новый экземпляр `UISelectionFeedbackGenerator` класса и `Prepare` вызывается его метод для информирования системы о том, что хаптик обратная связь, чтобы она могла максимально сокращать задержку.

`SelectionChanged`Затем метод активирует хаптик отзыв.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены новые типы отзывов о хаптик, доступные в iOS 10, и способы их реализации в Xamarin. iOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры iOS 10](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS10)