---
title: Нерекомендуемые технологии уведомлений в Xamarin. iOS
description: В этом документе описываются технологии уведомлений iOS, которые устарели в пользу платформы уведомлений пользователей, представленной в iOS 10.
ms.prod: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/07/2016
ms.openlocfilehash: a97885518c0646d2d8315b5da5eb12c71d693cf6
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574084"
---
# <a name="deprecated-notification-technologies-in-xamarinios"></a>Нерекомендуемые технологии уведомлений в Xamarin. iOS

В этом разделе показано, как реализовать локальные и Push-уведомления в Xamarin. iOS. В нем объясняются различные элементы пользовательского интерфейса уведомления iOS и обсуждаются функции API, связанные с созданием и отображением уведомлений.

> [!IMPORTANT]
> Сведения в этом разделе относятся к iOS 9 и более ранним версиям, поэтому она оставлена для поддержки более старых версий iOS. Для iOS 10 и более поздних версий ознакомьтесь с [руководством по платформе пользовательских уведомлений](~/ios/platform/user-notifications/index.md) для поддержки локальных и удаленных уведомлений на устройстве iOS.

## <a name="sections"></a>Разделы

<a name="Local Notifications In iOS"></a>

## <a name="local-notifications-in-ios"></a>[Локальные уведомления в iOS](local-notifications-in-ios.md)

В этом разделе обсуждается, как реализовать локальные уведомления в Xamarin. iOS. В нем объясняются различные элементы пользовательского интерфейса уведомления iOS и обсуждаются функции API, связанные с созданием и отображением уведомлений.

<a name="Local Notifications Walkthrough"></a>

## <a name="walkthrough---using-local-notifications-in-xamarinios"></a>[Пошаговое руководство. Использование локальных уведомлений в Xamarin. iOS](local-notifications-in-ios-walkthrough.md)

В этом разделе мы рассмотрим, как использовать локальные уведомления в приложении Xamarin. iOS. В нем демонстрируются основы создания и публикации уведомления, которое будет отображать оповещение при получении приложением.

<a name="Remote Notifications In iOS"></a>

## <a name="remote-notifications-in-ios"></a>[Удаленные уведомления в iOS](remote-notifications-in-ios.md)

В этом разделе рассматриваются push-уведомления в iOS. В ней представлены служба шлюза push-уведомлений Apple (APNS) и роль, которую она играет в уведомлениях о публикации в приложениях iOS. В нем объясняется, как создать сертификаты безопасности, необходимые для включения push-уведомлений и обсуждения. В итоге в этом разделе обсуждаются некоторые задачи обслуживания, которые должны выполняться серверами приложений для наблюдения за мобильными устройствами клиента.

## <a name="related-links"></a>Связанные ссылки

- [Уведомления (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/notifications)
