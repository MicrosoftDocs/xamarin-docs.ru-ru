---
title: tvOS в Xamarin — внутренние
description: Документы, описывающие внутреннюю работу tvOS в Xamarin, основанную на Xamarin. iOS. Содержимое ссылки описывает сборки, целевые платформы и связанные понятия iOS.
ms.prod: xamarin
ms.assetid: 8C076FED-9C03-44DE-9723-0E20272DD16B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
ms.openlocfilehash: 850ddad9c1d83d15f5116bfb4efc58e42164c20d
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434981"
---
# <a name="tvos-in-xamarin-internals"></a>tvOS в Xamarin — внутренние 

## <a name="assemblies"></a>[Сборки](~/ios/tvos/internals/assemblies.md)

Список сборок, поддерживаемых Xamarin для приложений Xamarin. tvOS.

## <a name="target-frameworks"></a>[Целевые платформы](~/ios/tvos/internals/frameworks.md)

В этой статье рассматриваются типы целевых платформ (библиотеки базовых классов), доступные в Xamarin. tvOS, и последствия выбора конкретной цели для приложения Xamarin. tvOS.

## <a name="related-ios-articles"></a>Связанные статьи по iOS

Следующие статьи относятся только к iOS, но относятся к tvOS (поскольку tvOS 9 является подмножеством iOS 9).

### <a name="unified-api"></a>[Unified API](~/cross-platform/macios/unified/index.md)

В появились новые унифицированные интерфейсы API, позволяющие упростить совместное использование кода между Apple TV и базой кода iOS, а также поддержку 64-разрядных API и 64-разрядной компиляции.  

### <a name="api-design"></a>[Разработка API](~/ios/internals/api-design/index.md)

Описывает принципы разработки, лежащие в основе привязки API.

### <a name="limitations"></a>[Ограничения](~/ios/internals/limitations.md)

В этом разделе описываются ловушки и ограничения, которые следует учитывать при работе с Xamarin. iOS, многие из которых применимы к Xamarin. tvOS.

### <a name="linker"></a>[Компоновщик](~/ios/deploy-test/linker.md)

Описание работы компоновщика для обеспечения минимального возможного пакета приложения, а также изменения параметров и использования.

### <a name="localization-and-internationalization"></a>[Локализация и интернационализация](~/ios/app-fundamentals/localization/index.md)

В этом руководстве описывается добавление кодировок в приложение Xamarin. iOS для поддержки интернационализации.

### <a name="mtouch"></a>[mtouch](~/ios/deploy-test/mtouch.md)

Сведения о средстве командной строки mtouch.exe, которое преобразует проект в приложение, которое можно использовать в iOS.

### <a name="linking-native-libraries"></a>[Связывание собственных библиотек](~/ios/platform/native-interop.md)

Xamarin. iOS поддерживает связывание с собственными библиотеками C и библиотеками цели-C. В этом документе описывается, как связать собственные библиотеки C с проектом Xamarin. iOS. Сведения о том, как сделать то же самое для библиотек цели-C, см. в документе « &nbsp; [Binding цели-c» Types](~/ios/platform/binding-objective-c/index.md) &nbsp; .

## <a name="objective-c-selectors"></a>[Селекторы цели-C](~/ios/internals/objective-c-selectors.md)

Примечания и использование для вызова селекторов цели-C (методов) напрямую.

### <a name="systemdata"></a>[System.Data](~/ios/data-cloud/system.data.md)

Сведения и инструкции по использованию System. Data для доступа к встроенной системе базы данных SQLite.

### <a name="threading"></a>[Работа с потоками](~/ios/app-fundamentals/threading.md)

Примечания по использованию потоков в приложениях Xamarin. iOS.

### <a name="xib-code-generation"></a>[Создание кода для XIB-файлов](~/ios/internals/xib-code-generation.md)

Как Visual Studio для Mac интегрируется с Interface Builder Xcode, чтобы можно было использовать Interface Builder для разработки пользовательского интерфейса.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)