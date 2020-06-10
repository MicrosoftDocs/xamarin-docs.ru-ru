---
title: Xamarin. Mac — macOS Sierra устранение неполадок
description: Этот документ содержит несколько советов по устранению неполадок при работе с macOS Sierra в приложениях Xamarin. Mac. Советы относятся к Mac App Store, Apple Pay, двоичной совместимости, CFNetwork, CloudKit и др.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 09/22/2016
ms.openlocfilehash: e7bc6fa12ab6720842ab264678cbf8124353fc40
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574422"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin. Mac — macOS Sierra устранение неполадок

_В этой статье приведены некоторые советы по устранению неполадок при работе с macOS Sierra в приложениях Xamarin. Mac._

В следующих разделах перечислены некоторые известные проблемы, которые могут возникнуть при использовании macOS Sierra с Xamarin. Mac и решения для этих проблем.

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [Двоичная совместимость](#Binary-Compatibility)
- [Протокол HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Основное изображение](#CoreImage)
- [Уведомления](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store"></a>

## <a name="app-store"></a>Магазин App Store

Известные проблемы:

- При тестировании покупок в приложении в изолированной среде диалоговое окно проверки подлинности может появиться дважды.
- При тестировании покупок в приложении с размещенным содержимым в изолированной среде диалоговое окно пароля появляется при каждом переходе приложения на передний план до завершения загрузки содержимого.

<a name="Apple-Pay"></a>

## <a name="apple-pay"></a>Apple Pay

Если при добавлении новой платежной карты в Apple Pay введена неправильная дата истечения срока действия или код безопасности (во вт.), процесс подготовки карт будет прерван.

<a name="Binary-Compatibility"></a>

## <a name="binary-compatibility"></a>Двоичная совместимость

Известные проблемы:

- Вызов `NSObject.ValueForKey` будет приводить к `null` исключению.
- `NSURLSession`И `NSURLConnection` больше не являются комплектами шифров RC4 во время подтверждения TLS для `http://` URL-адресов.
- Приложения могут зависнуть при изменении геометрического представления в `ViewWillLayoutSubviews` `LayoutSubviews` методах или.
- Для всех подключений SSL/TLS симметричный Шифр RC4 теперь отключен по умолчанию. Кроме того, интерфейс API защищенного транспорта больше не поддерживает SSLv3, поэтому рекомендуется как можно быстрее приостанавливаться к работе приложения с шифрованием SHA-1 и 3DES.

<a name="CFNetwork-HTTP-Protocol"></a>

## <a name="cfnetwork-http-protocol"></a>Протокол HTTP CFNetwork

`HTTPBodyStream`Свойство `NSMutableURLRequest` класса должно быть установлено в неоткрытый поток, так как `NSURLConnection` `NSURLSession` теперь строго применяет это требование.

<a name="CloudKit"></a>

## <a name="cloudkit"></a>CloudKit

Длительные операции будут возвращать _«у вас нет разрешения на сохранение файла»._ отобразится.

<a name="CoreImage"></a>

## <a name="core-image"></a>Основное изображение

`CIImageProcessor`API теперь поддерживает произвольное число образов ввода. `CIImageProcessor`API, который был добавлен в macOS Sierra бета-версии 1, будет удален.

<a name="Notifications"></a>

## <a name="notifications"></a>Уведомления

При работе с расширениями содержимого уведомлений контроллеры представлений не освобождаются должным образом и могут привести к сбою при достижении предела памяти расширения.

<a name="NSUserActivity"></a>

## <a name="nsuseractivity"></a>NSUserActivity

После операции передачи `UserInfo` свойство `NSUserActivity` объекта может быть пустым. Явный вызов `BecomeCurrent` `NSUserActivity` объекта в качестве текущего обходного пути.

<a name="Safari"></a>

## <a name="safari"></a>Safari

Вебжеолокатион требует наличия безопасного ( `https://` ) URL-адреса для работы с iOS 10 и macOS Sierra, чтобы предотвратить вредоносное использование данных о расположении.

## <a name="related-links"></a>Связанные ссылки

- [Примеры Mac](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [Новые возможности в macOS 10,12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
