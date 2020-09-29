---
title: SiriKit в Xamarin. iOS
description: В этой статье показано, как использовать SiriKit в приложении Xamarin. iOS для предоставления служб, доступных пользователю с помощью Siri на устройстве iOS.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: c50d02b4fc806a9ce466da450cfeb6afaa68df93
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433691"
---
# <a name="sirikit-in-xamarinios"></a>SiriKit в Xamarin. iOS

_В этой статье показано, как использовать SiriKit в приложении Xamarin. iOS для предоставления служб, доступных пользователю с помощью Siri на устройстве iOS._

В iOS 10 SiriKit позволяет приложению iOS предоставлять службы, доступные пользователю, с помощью Siri и приложения Maps на устройстве iOS с помощью расширений приложений и новых платформ **пользовательского интерфейса** для **целей** и целей.

Siri работает с понятием **доменов**, группами известных действий для связанных задач. Каждое взаимодействие приложения с Siri должно находиться в одном из известных доменов служб следующим образом:

- Вызов аудио или видео.
- Заменяйте его.
- Управление тренировки.
- Отправка.
- Поиск фотографий.
- Отправка или получение платежей.

Когда пользователь выполняет запрос Siri, включающий одну из служб расширения приложения, SiriKit отправляет расширение объект **намерения** , описывающий запрос пользователя вместе с любыми вспомогательными данными. Затем расширение приложения создает соответствующий объект **ответа** для заданной **цели**, чтобы получить сведения о том, как расширение может справиться с запросом.

## <a name="understanding-sirikit-concepts"></a>[Основные сведения о понятиях SiriKit](~/ios/platform/sirikit/understanding-sirikit.md)

В этой статье рассматриваются основные понятия, которые необходимо выполнить для работы с SiriKit в приложении Xamarin. iOS. Он охватывает новые точки расширения пользовательского интерфейса и способы их работы с приложением и словарем пользователя, чтобы открыть приложение для Siri.

## <a name="implementing-sirikit"></a>[Реализация SiriKit](~/ios/platform/sirikit/implementing-sirikit.md)

В этой статье рассматриваются шаги, необходимые для реализации поддержки SiriKit в приложениях Xamarin. iOS. Прежде чем пытаться добавить поддержку SiriKit в приложение, разработчик должен ознакомиться с руководством по основным понятиям SiriKit. в этом разделе рассматриваются основные понятия, которые необходимы для успешной реализации.

## <a name="related-links"></a>Связанные ссылки

- [Пример Елизачат](/samples/xamarin/ios-samples/ios10-elizachat)
- [SiriKit по программированию](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Ссылка на платформу целей](https://developer.apple.com/reference/intents)
- [Справочник по структуре пользовательского интерфейса для целей](https://developer.apple.com/reference/intentsui)