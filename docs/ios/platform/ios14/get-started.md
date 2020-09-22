---
title: Начало работы с iOS 14
description: В этом документе описывается, как настроить сборку приложений iOS 14 с помощью Xamarin. В нем описано, как загрузить Xcode 12 и обновить Visual Studio для Mac.
ms.prod: xamarin
ms.assetid: 0d721b4b-86bd-495a-8c0f-1f2f9fd6855e
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/17/2020
ms.openlocfilehash: 1bf77ca71d5fda945c81ac3ac6b338eec0164a76
ms.sourcegitcommit: 0c45e3f810947e3d43223aa01bf3e43a0defca65
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2020
ms.locfileid: "90843620"
---
# <a name="get-started-with-ios-14"></a>Начало работы с iOS 14

В этом документе описывается, как приступить к созданию приложений Xamarin, которые вызывают интерфейсы API, выпущенные с помощью Xcode 12 для iOS 14. Для использования предварительной версии требуется macOS Catalina 10.15.4 или более поздней версии.

## <a name="download-and-install"></a>Загрузите и установите

1. **Установка Xcode 12** . Зарегистрированные разработчики Apple могут загрузить и установить последнюю версию Xcode 12 с [портала для разработчиков Apple](https://developer.apple.com/download/) или из **магазина приложений**.

2. **Запустите Xcode 12** — запустите Xcode 12 перед обновлением и запуском Visual Studio для Mac или Visual Studio 2019, так как в нем устанавливаются средства, необходимые для работы Xamarin.

3. **Обновление Visual Studio для Mac и Visual Studio 2019** — убедитесь, что у вас установлена последняя стабильная версия Xamarin.

4. Используемых **Установка iOS 14 на устройствах iOS** . для тестирования приложений, использующих API-интерфейсы, появившиеся в Xcode 12, зарегистрированные разработчики Apple могут [загрузить](https://developer.apple.com/download) и установить операционную систему на своих устройствах. 

   > [!TIP]
   > Даже если ваше приложение не использует новые API, обязательно создайте его с новейшими пакетами SDK Xcode 12 и протестируйте его, чтобы убедиться, что все работает правильно. Если приложение не вызывает новые API, его можно перекомпилировать с помощью этих новых пакетов SDK и протестировать на устройствах, которые еще не были обновлены до новой операционной системы.
   >
   > Перед обновлением устройств до последних версий операционной системы с Apple для тестирования приложений Xamarin необходимо выполнить следующие действия:
   >
   > - Сведения об обновлениях операционной системы см. в [заметках о выпуске Apple](https://developer.apple.com/download/) .

## <a name="related-links"></a>Связанные ссылки

- [Скачать Xcode](https://developer.apple.com/download/)
- [Заметки о выпуске Xamarin.iOS](/xamarin/ios/release-notes/14/14.0)
- [Заметки о выпуске Xcode 12](https://developer.apple.com/documentation/xcode-release-notes/xcode-12-release-notes)
