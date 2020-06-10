---
title: Совместимость с iOS 9
description: Даже если вы не планируете добавлять компоненты iOS 9 в приложение, вы должны перестроить приложения с помощью последней версии Xamarin.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 429b15b8e0f2b66b8a0edcdf386ef7778cf4a9ca
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574123"
---
# <a name="ios-9-compatibility"></a>Совместимость с iOS 9

_Даже если вы не планируете добавлять компоненты iOS 9 в приложение, вы должны перестроить приложения с помощью последней версии Xamarin._

> [!IMPORTANT]
> Информация на этой странице предназначена для клиентов с приложениями, которые уже находятся в магазине приложений, предназначенных для iOS 8 или более ранних версий, которые еще не отправили обновления для совместимости с iOS 9. Если вы уже используете последние версии — Xcode 7 и Xamarin. iOS 9 для разработки приложений, ознакомьтесь с [введением в iOS 9](~/ios/platform/introduction-to-ios9/index.md).

При появления первых бета-версий iOS 9 мы обнаружили две проблемы с более старыми версиями Xamarin, которые были объявлены как старые приложения, которые не удается запустить в iOS 9:

- Приложения сборки для iOS 8 или более ранней версии не могут запускаться на 32-разрядных устройствах (включая приложения, созданные с помощью [Unified API](~/cross-platform/macios/unified/index.md)).
- Сбой P/Invoke с полным путем не указан.

Обновление установки Xamarin до последней стабильной версии канала, а затем перестроение и повторное развертывание приложений устраняет эти две проблемы.

_Даже если вы не планируете обновлять приложение с помощью функций iOS 9, мы рекомендуем повторно выполнить сборку с последней версией Xamarin и повторно отправить ее в App Store_.

Это обеспечит запуск приложения на iOS 9 после обновления клиентов.
Вы можете продолжить поддержку iOS 8 — перестроение с использованием последнего выпуска не влияет на целевую версию приложения.

Если при тестировании существующих приложений в iOS 9 возникли проблемы, ознакомьтесь с разделом [улучшение совместимости](#compat) ниже.

### <a name="updating-with-visual-studio"></a>Обновление с помощью Visual Studio

Рекомендуется явно проверить, что Visual Studio обновлен до последней стабильной версии.

## <a name="what-about-components-nugets-and-other-libraries"></a>Как насчет компонентов, NuGet и других библиотек?

Вам **не** нужно ждать новых версий каких бы то ни было компонентов или NuGet, которые вы используете для решения двух упомянутых выше проблем.
Эти проблемы решены просто путем повторного создания приложения с последним стабильным выпуском Xamarin. iOS.

Точно так же поставщики компонентов и авторы NuGet **не** обязаны отправлять новые сборки только для устранения двух упомянутых выше проблем. Однако если какой-либо компонент или NuGet использует `UICollectionView` или загружает представления из файлов **XIB** , *может* потребоваться обновление, чтобы устранить проблемы совместимости с iOS 9, упомянутые ниже.

<a name="compat"></a>

## <a name="improving-compatibility-in-your-code"></a>Улучшение совместимости в коде

Существует несколько вариантов шаблонов кода, которые *используются* для работы в более ранних версиях iOS, прерывающих работу в iOS 9. Ниже приведены некоторые возможные проблемы (и их решения), которые могут возникнуть при тестировании на iOS 9:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>Уиколлектионвиевцелл. ContentView имеет значение NULL в конструкторах

**Причина:** В iOS 9 `initWithFrame:` конструктор теперь необходим из-за изменений в работе iOS 9 в качестве [состояния документации уиколлектионвиев](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Если вы зарегистрировали класс для указанного идентификатора и хотите создать новую ячейку, эта ячейка инициализируется путем вызова ее `initWithFrame:` метода.

**Исправление:** Добавьте `initWithFrame:` конструктор следующим образом:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Связанные примеры: [мотионграф](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [тексткитдемо](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView не удается инициализировать с помощью программиста при загрузке представления из XIB или NIB

**Причина:** Конструктор вызывается `initWithCoder:` при загрузке представления из файла Interface Builder XIB. Если этот конструктор не экспортирован в неуправляемый код, он не может вызвать нашу управляемую версию. Ранее (например, в iOS 8) `IntPtr` конструктор был вызван для инициализации представления.

**Исправление:** Создайте и экспортируйте `initWithCoder:` конструктор следующим образом:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Связанный пример: [чат](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

### <a name="dyld-message-no-cache-image-with-name"></a>Сообщение дилд: нет образа кэша с именем...

В журнале может произойти сбой со следующими данными:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Причина:** Это ошибка в машинном компоновщике Apple, который происходит при создании общедоступной инфраструктуры (Жаваскрипткоре был открыт в iOS 7, прежде чем он был закрытой платформой), а целевой объект развертывания приложения — для версии iOS, когда платформа была закрыта. В этом случае компоновщик Apple свяжется с закрытой версией платформы вместо общедоступной версии.

**Исправление:** Это будет решено для iOS 9, но существует простое решение, которое можно применить самостоятельно. в этом случае вы можете выбрать более позднюю версию iOS в проекте (в этом случае можно попробовать iOS 7). Другие платформы могут столкнуться с аналогичными проблемами, например, если платформа WebKit стала общедоступной в iOS 8 (и, таким образом, для iOS 7 будет возникать эта ошибка; следует выбрать iOS 8 для использования WebKit в приложении).

## <a name="related-links"></a>Связанные ссылки

- [сведения о выпуске совместимости iOS 9](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Введение в iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [Обновление приложений Xamarin. iOS до iOS9 (видео)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
