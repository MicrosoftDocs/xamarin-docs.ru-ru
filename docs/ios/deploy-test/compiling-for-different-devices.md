---
title: Компиляция для разных устройств в Xamarin.iOS
description: Этот документ описывает различные параметры конфигурации сборки, которые можно использовать для настройки сборки Xamarin.iOS для различных устройств.
ms.prod: xamarin
ms.assetid: 3B259248-887E-3E4F-E09C-7AD28C2A8CEE
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 2f70dd3b18c36d478548672bb78d329cb2a4c9ab
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938779"
---
# <a name="compiling-for-different-devices-in-xamarinios"></a>Компиляция для разных устройств в Xamarin.iOS

Вы можете настроить свойства сборки для исполняемого файла на странице свойств проекта **Сборка iOS**. Чтобы открыть эту страницу, щелкните имя проекта правой кнопкой мыши и выберите **Параметры > Сборка iOS** (в Visual Studio для Mac) или **Свойства** (в Visual Studio).

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Страница свойств "Сборка iOS" в Project](compiling-for-different-devices-images/image1.png)](compiling-for-different-devices-images/image1.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Страница свойств "Сборка iOS" в Project](compiling-for-different-devices-images/image1a.png)](compiling-for-different-devices-images/image1a.png#lightbox)

-----

Помимо параметров конфигурации, доступных в пользовательском интерфейсе, вы можете создать собственный набор параметров командной строки и передать его в [средство сборки Xamarin.iOS (mtouch)](~/ios/deploy-test/mtouch.md).

## <a name="sdk-options"></a>Параметры пакета SDK

Visual Studio для Mac позволяет настроить для пакета SDK два важных свойства: версию пакета SDK для iOS, которая используется при сборке программы, и цель развертывания (которая обозначает минимально необходимую версию iOS).

**Версия пакета SDK** для IOS позволяет указать разные версии опубликованных Apple пакетов SDK. Эти сведения сообщат Xamarin.iOS, какие компиляторы, компоновщики и библиотеки нужно использовать при сборке. Щелкните проект правой кнопкой мыши и выберите **Параметры**, а затем **Сборка iOS** в окне "Параметры":

[![Выбор версии пакета SDK в окне "Параметры"](compiling-for-different-devices-images/sdk-version-sml.png)](compiling-for-different-devices-images/sdk-version.png#lightbox)

Параметр **Цель развертывания** определяет минимальную необходимую версию операционной системы, на которой может выполняться приложение. Этот параметр задается в файле **Info.plist** проекта. В качестве минимально необходимой выбирайте такую версию, которая содержит все нужные API-интерфейсы для запуска приложения.

[![Установка целевого объекта развертывания в файле Info.plist](compiling-for-different-devices-images/deployment-target-sml.png)](compiling-for-different-devices-images/deployment-target.png#lightbox)

Как правило, API-интерфейс Xamarin.iOS предоставляет все методы, доступные в последней версии пакета SDK, и мы при необходимости предоставляем удобные методы, позволяющие определить доступность определенных функциональных возможностей во время выполнения (например, `UIDevice.UserInterfaceIdiom` и `UIDevice.IsMultitaskingSupported` работают на Xamarin.iOS всегда и не требуют от вас никаких действий).

## <a name="linking"></a>Компоновка

Страница с информацией о [компоновщике](~/ios/deploy-test/linker.md) позволит узнать, как компоновщик помогает уменьшить размер исполняемых файлов, и как его наиболее эффективно использовать.

## <a name="code-generation-engine"></a>Модуль создания кода

Начиная с версии 4.0, Xamarin.iOS поддерживает две серверные системы создания кода. Это обычный Mono и модуль на основе оптимизирующего компилятора LLVM. У каждого из них есть преимущества и недостатки.

Обычно в процессе разработки чаще применяется модуль создания кода Mono, который позволяет быстро повторять регулярные процессы. Для сборки выпуска и для развертывания в AppStore лучше переключиться на модуль создания кода LLVM.

Серверный модуль оптимизации LLVM создает код, который работает быстрее и занимает меньше места, чем код Mono, но при этом компиляция требует много больше времени.

Вы можете выбрать используемый вариант в параметрах сборки iOS в Visual Studio или Visual Studio для Mac.

[![Включение LLVM](compiling-for-different-devices-images/image2.png)](compiling-for-different-devices-images/image2.png#lightbox)

[![Включение LLVM](compiling-for-different-devices-images/image2a.png)](compiling-for-different-devices-images/image2a.png#lightbox)

## <a name="architecture-support"></a>Поддержка архитектур

### <a name="armv6-xamarinios-discontinued-support-for-armv6-with-v810"></a>ARMv6 (Xamarin.iOS прекратил поддержку ARMv6 с версии 8.10)

- iPhone (оригинальный), 3G
- iPod 1-го и 2-го поколения

### <a name="armv7"></a>ARMv7

- iPhone 3GS, 4, 4S
- iPad 1, 2, 3, Mini
- iPod 3-го, 4-го и 5-го поколения

### <a name="armv7s"></a>ARMv7s

- iPhone 5
- iPhone 5c
- iPad 4

Если вы создаете код только для процессора ARMv7s, он будет выполняться немного быстрее, но совсем не будет работать на системах ARMv7 или ARMv6, если не включить в сборку файл в толстом двоичном формате, который содержит несколько исполняемых объектов.

### <a name="arm64-xamarinios-started-supporting-arm64-in-v86"></a>ARM64 (Xamarin.iOS начал поддерживать ARM64 с версии 8.6)

- iPhone 5s
- iPhone SE
- iPhone 6, 6 Plus
- iPhone 6s, 6s Plus
- iPhone 7, 7 Plus
- iPhone 8, 8 Plus
- iPhone X
- iPad Air
- iPad Air 2
- iPad Mini 2, 3, 4
- iPad Pro (все версии)

Обратите внимание, что переданные в App Store сборки должны поддерживать 64-разрядные системы. Это обязательное требование [Apple](https://developer.apple.com/news/?id=12172014b). Кроме того, iOS 11 поддерживает только 64-разрядные приложения.

### <a name="arm-thumb-2-support"></a>Поддержка ARM Thumb-2

Thumb — это более компактный набор инструкций, используемый процессорами ARM. Включив поддержку Thumb, вы сможете уменьшить размер исполняемого файла, но в ущерб времени его выполнения. Thumb поддерживается на ARMv7 и ARMv7s.

## <a name="conditional-framework-usage"></a>Использование условной структуры

Если в проекте нужны какие-то функции новых выпусков iOS, вам может потребоваться условная структура для выбора платформ. Для примера предположим, что вы хотите использовать iAd при работе на iOS 4.0, но не терять при этом и поддержку устройств 3.x. Чтобы решить эту задачу, нужно задать для Xamarin.iOS "слабую" привязку к платформе iAd. Слабая привязка означает, что платформа будет загружаться только по запросу при первом вызове класса из этой платформы.

Для этого следует выполнить следующие действия:

- Откройте **Параметры проекта** и перейдите к панели **Сборка iOS**.
- Добавьте `'-gcc_flags "-weak_framework iAd"'` в раздел **Дополнительные параметры** для каждой конфигурации, к которой должна действовать слабая привязка:

[![Дополнительные параметры](compiling-for-different-devices-images/image3.png)](compiling-for-different-devices-images/image3.png#lightbox)

Помимо этого, нужно запретить использование в коде тех типов, которые не существуют в более старых версиях iOS при работе на устройствах с этими версиями. Эту задачу можно решить несколькими способами, например с помощью синтаксического анализа `UIDevice.CurrentDevice.SystemVersion`.

## <a name="related-links"></a>Связанные ссылки

- [Компоновщик](~/ios/deploy-test/linker.md)
