---
title: Устранение неполадок приложений tvOS 10, созданных с помощью Xamarin
description: Эта статья содержит несколько советов по устранению неполадок при работе с tvOS 10 в приложениях Xamarin. Здесь описываются проблемы, связанные с магазином приложений, двоичной совместимостью, CFNetwork Хттппротокол, CloudKit, основным образом, Нсусерактивити и UIKit.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 65a76c0196b79a17f935f59902c8e6d2f9f25933
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434924"
---
# <a name="troubleshooting-tvos-10-apps-built-with-xamarin"></a>Устранение неполадок приложений tvOS 10, созданных с помощью Xamarin

В следующих разделах перечислены некоторые известные проблемы, которые могут возникнуть при использовании tvOS 10 с Xamarin, и решение этих проблем.

- [App Store](#App-Store)
- [Двоичная совместимость](#Binary-Compatibility)
- [Протокол HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Основное изображение](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store"></a>

## <a name="app-store"></a>App Store

Известные проблемы:

- При тестировании покупок в приложении в изолированной среде диалоговое окно проверки подлинности может появиться дважды.
- При тестировании покупок в приложении с размещенным содержимым в изолированной среде диалоговое окно пароля появляется при каждом переходе приложения на передний план до завершения загрузки содержимого.

<a name="Binary-Compatibility"></a>

## <a name="binary-compatibility"></a>Двоичная совместимость

Известные проблемы:

- Вызов `NSObject.ValueForKey` будет приводить к `null` исключению.
- Ссылка на шрифт по имени при вызове `UIFont.WithName` приведет к сбою.
- `NSURLSession`И `NSURLConnection` больше не являются комплектами шифров RC4 во время подтверждения TLS для `http://` URL-адресов.
- Приложения могут зависнуть при изменении геометрического представления в `ViewWillLayoutSubviews` `LayoutSubviews` методах или.
- Для всех подключений SSL/TLS симметричный Шифр RC4 теперь отключен по умолчанию. Кроме того, интерфейс API защищенного транспорта больше не поддерживает SSLv3, поэтому рекомендуется как можно быстрее приостанавливаться к работе приложения с шифрованием SHA-1 и 3DES.

<a name="CFNetwork-HTTP-Protocol"></a>

## <a name="cfnetwork-http-protocol"></a>Протокол HTTP CFNetwork

`HTTPBodyStream`Свойство `NSMutableURLRequest` класса должно быть установлено в неоткрытый поток, так как `NSURLConnection` `NSURLSession` теперь строго применяет это требование.

<a name="CloudKit"></a>

## <a name="cloudkit"></a>CloudKit

Длительные операции будут возвращать _«у вас нет разрешения на сохранение файла»._ ошибка".

<a name="CoreImage"></a>

## <a name="core-image"></a>Основное изображение

`CIImageProcessor`API теперь поддерживает произвольное число образов ввода. `CIImageProcessor` API, который был добавлен в tvOS 10 Beta 1, будет удален.

<a name="NSUserActivity"></a>

## <a name="nsuseractivity"></a>NSUserActivity

После операции передачи `UserInfo` свойство `NSUserActivity` объекта может быть пустым. Явным образом вызовите `BecomeCurrent` объект нсусерактивити ' в качестве текущего обходного пути.

<a name="UIKit"></a>

## <a name="uikit"></a>UIKit

Известные проблемы:

- Изменения в фоновом представлении `UINavigationBar` `UITabBar` или `UIToolBar` могут привести к передаче макета для разрешения нового внешнего вида. Попытка изменить эти представления внутри `LayoutSubviews` `UpdateConstraints` `WillLayoutSubviews` события, или `DidUpdateSubviews` может привести к бесконечному циклу макета.
- В tvOS 10 вызов `RemoveGestureRecognizer` метода `UIView` объекта явным образом отменяет любой распознаватель выполняемых жестов.
- Представленные контроллеры представления теперь могут повлиять на внешний вид строки состояния.
- tvOS 10 требует, чтобы разработчик вызывал `base.AwakeFromNib` при подклассах `UIViewController` и переопределении `AwakeFromNib` метода.
- Приложения с пользовательскими `UIView` подклассами, переопределяющими `LayoutSubviews` и грязными макетом перед вызовом, `base.LayoutSubviews` могут активировать бесконечный цикл макета в tvOS 10.
- Ресурсы, связанные с направлением или перевернутым изображениями, не передаются при назначении `UIButton` объектам.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [Новые возможности в tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)