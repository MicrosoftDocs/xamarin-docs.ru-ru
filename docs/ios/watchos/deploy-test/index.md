---
title: Развертывание и тестирование приложений watchOS с помощью Xamarin
description: В этом документе описывается развертывание и тестирование приложений watchOS, созданных с помощью Xamarin. Он содержит контрольный список развертывания, обсуждает явные идентификаторы приложений с подстановочными знаками и рассматривает группы приложений.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 4e2ff46174d9dbb9171a470c389ffe301f6d0d60
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569651"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>Развертывание и тестирование приложений watchOS с помощью Xamarin

## <a name="deployment-checklist"></a>Контрольный список развертывания

При развертывании в контрольном списке или при отправке в App Store необходимо выполнить действия, описанные на этой странице.

- В **центре разработки iOS**:
  - [Идентификаторы приложений](#App_IDs) созданы.
  - Настроенные [группы приложений](#App_Groups) (при необходимости).
  - Созданы профили подготовки распространения

- В решении:

  - Убедитесь, что заданы [идентификаторы пакетов и ссылки проекта](~/ios/watchos/get-started/installation.md) .
  - Проверьте [правильность настройки](~/ios/watchos/app-fundamentals/icons.md)значков.
  - Проверьте номера версий пакетов, совпадающие во всех проектах.
  - Настройте права **. plist** для групп приложений (при необходимости).

- Затем следуйте инструкциям на странице:
  - [Развертывание в Apple Watch для тестирования](~/ios/watchos/deploy-test/device.md)или
  - [Отправка в App Store](~/ios/watchos/deploy-test/appstore.md).

<a name="App_IDs"></a>

## <a name="app-ids"></a>Идентификаторы приложений

Как обсуждалось в [инструкциях по установке](~/ios/watchos/get-started/installation.md), все три проекта в приложении наблюдения имеют связанные идентификаторы пакетов, например:

- Унифицированный проект Xamarin. iOS —`com.xamarin.WatchKitCatalog`
- Проект расширения WatchKit —`com.xamarin.WatchKitCatalog.watchkitextension`
- Просмотр проекта приложения-`com.xamarin.WatchKitCatalog.watchkitapp`

Для всех трех проектов требуется соответствующий профиль подготовки распространения, либо использование явно идентификаторов приложений для каждого, либо идентификатор приложения с подстановочными знаками.

### <a name="explicit-app-ids"></a>Явные идентификаторы приложений

Создайте **идентификатор приложения** для каждого идентификатора пакета проекта (который будет выглядеть следующим образом в центре разработки iOS):

![Идентификаторы пакетов в центре разработки iOS](images/appids-specific-sml.png)

При создании или настройке идентификаторов приложений не забудьте включить конкретные функции, необходимые для приложения. Это могут быть push-уведомления и группы приложений.

Вам потребуется создать профиль подготовки распространения для каждого идентификатора приложения.

### <a name="wildcard-app-id"></a>Идентификатор приложения с подстановочными знаками

Кроме того, можно создать **идентификатор приложения** с подстановочными знаками, который соответствует всем трем проектам, например `com.xamarin.*` .

Обратите внимание, что некоторые функции не могут использоваться с ИДЕНТИФИКАТОРом приложения с подстановочными знаками (например, Push-уведомления). Если приложению требуются эти функции, необходимо создать явные идентификаторы приложений.

Для распространения вам потребуется только создать один профиль подготовки распространения для идентификатора приложения с подстановочными знаками.

<a name="App_Groups"></a>

## <a name="app-groups"></a>Группы приложений

Вы можете использовать группу приложений для обмена данными между приложением iOS и расширением Watch. Необходимо убедиться, что в решении есть:

- Настройка **группы приложений** на портале разработчика Apple. раздел **идентификаторы & профили** .

- Включенные **группы приложений** (и ПРЕДОСТАВЛЕНные **идентификаторы группы приложений** *) в приложении* iOS и **идентификаторе приложения** и правах расширения контрольных значений **. plist**.

### <a name="certificates-identifiers--profiles"></a>Сертификаты, идентификаторы & профили

Чтобы использовать группу приложений, создайте запись на экране **группы приложений** . В приведенном ниже примере имя группы имеет тот же стиль обратных DNS, который обычно используется для идентификаторов приложений, но с `group.` префиксом (который требуется):

![Идентификатор](images/appgroups-new-sml.png)

Группа приложений появится в списке:

![Список идентификаторов](images/appgroups-setup-sml.png)

После создания группы на нее можно будет ссылаться в конфигурации **идентификатора приложения** . Не забудьте включить его в приложение iOS и просмотреть **Идентификаторы приложений**расширения.

![Доступные конфигурации](images/appgroups-sml.png)

**Не** включайте группы приложений в Apple Watch идентификатор приложения. Его не обязательно включать для просмотра.

### <a name="entitlementsplist"></a>Entitlements.plist

Некоторые функции приложения (например, Для групп приложений) необходимо задать свои права.
Дважды щелкните, чтобы изменить файл прав **. plist** в следующих проектах:

- проект приложения iOS
- Просмотр проекта расширения

.![Редактор прав. plist](images/entitlements-plist-sml.png)

**Не** включайте права в проект Watch App. Его не обязательно включать для просмотра.

## <a name="related-links"></a>Связанные ссылки

- [Рекомендации по отправке Apple WatchKit](https://developer.apple.com/app-store/watch/)
