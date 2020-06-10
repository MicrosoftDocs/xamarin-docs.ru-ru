---
title: Введение в watchOS 3
description: В этой статье представлены все новые и измененные API-интерфейсы и функции, доступные в watchOS 3 для разработчиков Xamarin.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/07/2017
ms.openlocfilehash: abe9c95c889aed5258ea3a5367e05368ddb7c77f
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574175"
---
# <a name="introduction-to-watchos-3"></a>Введение в watchOS 3

_В этой статье представлены все новые и измененные API-интерфейсы и функции, доступные в watchOS 3 для разработчиков Xamarin._

В этом документе рассматриваются следующие темы:

- [Новые возможности в watchOS 3](#Whats-New-in-watchOS-3)
  - [Улучшения Apple Pay](#Apple-Pay-Enhancements) добавляют поддержку платежей в приложении на Apple Watch.
  - [Фоновые задачи](#Background-Tasks) предоставляют приложению возможность обновлять свои данные в фоновом режиме, чтобы оно было готово, когда это необходимо пользователю.
  - В watchOS 3 были внесены [улучшения сложности](#Complications-Enhancements) , которые предоставляют новые функции для приложений.
  - [Новые доступные платформы](#Newly-Available-Frameworks) доступны для приложений watchOS.
  - [Упреждающие предложения](#Proactive-Suggestions) позволяют приложению заранее показывать сведения пользователю.
  - В watchOS 3 внесено несколько [улучшений безопасности и конфиденциальности](#Security-and-Privacy-Enhancements) .
  - [Моментальные снимки и закрепление](#Snapshots-and-Dock) предоставляют пользователю быстрый доступ к приложениям watchOS приложений.
  - Пользовательские [уведомления](#User-Notifications) предоставляют пользователю как локальные, так и удаленные уведомления.
  - В watchOS 3 были сделаны несколько [улучшений инфраструктуры подключения](#Watch-Connectivity-Framework-Enhancements) .
  - В watchOS 3 было внесено несколько [улучшений платформы WatchKit](#WatchKit-Framework-Enhancements) .
  - [Усовершенствованные приложения для тренировки](#Workout-App-Enhancements) предоставляют новые возможности для Apple Watch приложений, связанных с тренировками.
- В watchOS 3 были сделаны [Дополнительные изменения платформы](#Additional-Framework-Changes) .
- [Устаревшие API-интерфейсы](#Deprecated-APIs) в watchOS 3.

<a name="Whats-New-in-watchOS-3"></a>

## <a name="whats-new-in-watchos-3"></a>Новые возможности в watchOS 3

Компания Apple добавила несколько новых интерфейсов API и служб в watchOS 3 вместе с множеством усовершенствований существующих функций, включая:

<a name="Apple-Pay-Enhancements"></a>

## <a name="apple-pay-enhancements"></a>Улучшения Apple Pay

В watchOS 3 платформа PassKit была расширена, чтобы обеспечить поддержку безопасных и внутренних платежей в приложении (как физических товаров, так и услуг) для приложений, работающих на Apple Watch.

Используйте новые классы [пкпайментаусоризатионконтроллер](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) и [пкпайментаусоризатионконтроллерделегате](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) для предоставления и реагирования на интерфейс, в котором пользователь может авторизовать запросы на оплату.

Чтобы узнать больше, ознакомьтесь с нашим руководством по [улучшению Apple Pay](~/ios/watchos/platform/apple-pay.md) .

<a name="Background-Tasks"></a>

## <a name="background-tasks"></a>Фоновые задачи

в watchOS 3 представлено несколько фоновых задач, которые приложение может использовать для обновления сведений, гарантирующих, что оно должно иметь содержимое, необходимое пользователю, прежде чем открыть его.

Доступны следующие новые фоновые задачи.

- **Фоновое обновление приложения** . Задача [вкаппликатионрефрешбаккграундтаск](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) позволяет приложению обновить свое состояние в фоновом режиме. Обычно это будет включать в себя другую задачу, например загрузку нового содержимого из Интернета с помощью [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Фоновое обновление моментального снимка** . Задача [вкснапшотрефрешбаккграундтаск](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) позволяет приложению обновлять содержимое и пользовательский интерфейс до того, как система создаст моментальный снимок, который будет использоваться для заполнения закрепления.
- **Подключение к фоновому просмотру** . Задача [вкватчконнективитирефрешбаккграундтаск](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) запускается для приложения при получении фоновых данных из парного iPhone.
- **Фоновый сеанс URL-адреса** — задача [вкурлсессионрефрешбаккграундтаск](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) запускается для приложения, когда фоновому переносу требуется авторизация или завершение (успешно или по ошибке).

Чтобы узнать больше, ознакомьтесь с нашим руководством по [фоновым задачам](~/ios/watchos/platform/background-tasks.md) .

<a name="Complications-Enhancements"></a>

## <a name="complications-enhancements"></a>Улучшения сложности

Сложности — это небольшие визуальные элементы, которые позволяют быстро получить полезную информацию. В зависимости от выбранного лица с контрольными данными пользователь может настроить просмотр с помощью одной или нескольких осложнений.

watchOS 3 дает приложению возможность создать одну или несколько осложнений для приложения Watch, чтобы пользователь мог быстро получить доступ к своей информации из циферблата.

Кроме того, сложности предоставляют следующие преимущества:

- Пользователь может быстро запустить приложение, коснувшись его сложности непосредственно из циферблата.
- Наличие одной из осложнений приложения на наблюдателе заставляет систему поддерживать приложение в состоянии готовности к запуску, когда оно пытается запустить приложение в фоновом режиме, поддерживать его в памяти и требует дополнительного времени для обновления.
- Сложности гарантированно загружаются по крайней мере 50 обновлений push-уведомлений в день.
- Когда приложение включает сложности, оно будет представлено в коллекции Apple Watch лиц.

В watchOS 3 платформа Клокккит теперь включает несколько новых шаблонов для дополнительных крупных осложнений, таких как [клккомпликатионтемплатикстраларжеколумнстекст](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) и [клккомпликатионтемплатикстраларжерингимаже](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). Кроме того, чтобы создать локализуемый текст, используйте новые методы класса [клктекстпровидер](https://developer.apple.com/reference/clockkit/clktextprovider) .

Чтобы узнать больше, ознакомьтесь с нашим руководством по [быстрому взаимодействию для watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) .

<a name="Newly-Available-Frameworks"></a>

## <a name="newly-available-frameworks"></a>Новые доступные платформы

watchOS 3 включает несколько существующих платформ Apple, которые ранее были недоступны, например:

- **SceneKit** — используйте SceneKit для включения трехмерных моделей в пользовательский интерфейс контрольного приложения, включая большинство функций, доступных на других платформах, таких как освещение, заливка, анимация, физика и частицы. Трехмерные пространственное аудио, настраиваемые или OpenGL-шейдеры, основные фильтры образов и физически используемые материалы не поддерживаются.
- **SpriteKit** — используйте SpriteKit для подготовки к просмотру и анимации спрайтов в пользовательском интерфейсе приложения Watch, включая большинство функций, доступных на других платформах, таких как действия, физика, освещение и системы частиц. Трехмерный Пространственный звук, воспроизведение видео и фильтры основных образов не поддерживаются.
- **Авфаундатион** — для управления и воспроизведения звука.
- **CloudKit** — перемещение данных между контейнерами приложения Watch и iCloud.
- **Основной звук** — для управления типами данных для представления звуковых потоков, сложных буферов и значений времени.
- **Gamekit** — создание социальных игр.

<a name="Proactive-Suggestions"></a>

## <a name="proactive-suggestions"></a>Упреждающие предложения

watchOS 3 позволяет приложению заранее предоставлять сведения пользователю в рамках заданных контекстов. Для поддержки этой функции [нсусерактивити](https://developer.apple.com/reference/foundation/nsuseractivity) теперь включает `MapItem` свойство, которое позволяет приложению предоставлять сведения о расположении для последующего использования другими приложениями.

Чтобы узнать больше, ознакомьтесь с нашим руководством по [упреждающим рекомендациям](~/ios/watchos/platform/proactive-suggestions.md) .

<a name="Security-and-Privacy-Enhancements"></a>

## <a name="security-and-privacy-enhancements"></a>Улучшения безопасности и конфиденциальности

Компания Apple внесла несколько усовершенствований в безопасность и конфиденциальность в watchOS 3, что поможет разработчику повысить безопасность своих приложений и обеспечить конфиденциальность конечного пользователя.

В результате приложения, работающие в watchOS 3 (или более поздней версии), должны статически объявлять свои цели для доступа к конкретным функциям или сведениям о пользователях, вводя в свои файлы один или несколько специфических для конфиденциальности ключей `Info.plist` , объясняющих пользователю, почему приложение хочет получить доступ.

Так как watchOS 3 совместно используют эти изменения с iOS 10, дополнительные сведения см. в статье о [усовершенствованиях по безопасности и конфиденциальности](~/ios/app-fundamentals/security-privacy.md) в iOS 10.

<a name="Snapshots-and-Dock"></a>

## <a name="snapshots-and-dock"></a>Моментальные снимки и закрепление

В watchOS 3 Компания Apple добавила закрепление, где пользователи могут закреплять свои любимые приложения и быстро получать к ним доступ. Когда пользователь нажимает кнопку сбоку на Apple Watch, отображается коллекция снимков закрепленных приложений. Пользователь может прокрутить влево или вправо, чтобы найти нужное приложение, а затем коснуться приложения, чтобы запустить его, заменив моментальный снимок на интерфейс выполняющегося приложения.

Система периодически создает моментальные снимки пользовательского интерфейса приложения и использует эти моментальные снимки для заполнения документов. watchOS предоставляет приложению возможность обновить содержимое и пользовательский интерфейс перед созданием моментального снимка.

Дополнительные сведения см. в руководстве по [фоновым задачам](~/ios/watchos/platform/background-tasks.md) и [справочнике](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) по Apple вкснапшотрефрешбаккграундтаск.

<a name="User-Notifications"></a>

## <a name="user-notifications"></a>Уведомления пользователей

Платформа уведомлений пользователей, представленная в watchOS 3, поддерживает доставку как локальных, так и удаленных уведомлений в Apple Watch. Используйте эту платформу для планирования уведомлений на основе конкретных условий, например времени суток или расположения, а также для получения и обслуживания уведомлений.

Чтобы узнать больше, ознакомьтесь с нашим руководством по [быстрому взаимодействию для watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) .

<a name="Watch-Connectivity-Framework-Enhancements"></a>

## <a name="watch-connectivity-framework-enhancements"></a>Ознакомьтесь с усовершенствованиями инфраструктуры подключений

Новое `HasContentPending` свойство класса [вксессион](https://developer.apple.com/reference/watchconnectivity/wcsession) указывает, что сеанс получил данные в фоновом режиме, который необходимо обработать. И `RemainingComplicationUserInfoTransfers` свойство возвращает оставшееся время, в течение которого приложение iOS может обновить свою WatchOS сложность.

Чтобы узнать больше, ознакомьтесь с нашим руководством по [фоновым задачам](~/ios/watchos/platform/background-tasks.md) .

<a name="WatchKit-Framework-Enhancements"></a>

## <a name="watchkit-framework-enhancements"></a>Усовершенствования платформы WatchKit

watchOS 3 включает в себя несколько усовершенствований платформы WatchKit, включая следующие:

- Приложение может получить состояние Digital Crown с помощью нового класса [вккровнсекуенцер](https://developer.apple.com/reference/watchkit/wkcrownsequencer) и получать обновления, когда пользователь поворачивает самые с помощью класса [вккровнделегате](https://developer.apple.com/reference/watchkit/wkcrowndelegate) .
- Класс [вкекстенсион](https://developer.apple.com/reference/watchkit/wkextension) теперь включает в себя `ApplicationState` метод и константу [вкаппликатионстате](https://developer.apple.com/reference/watchkit/wkapplicationstate) , которые приложение может использовать для отслеживания состояния среды выполнения приложения. `WKExtension`также предоставляет два новых метода, которые можно использовать для планирования фоновых задач.
- [Вкекстенсионделегате](https://developer.apple.com/reference/watchkit/wkextensiondelegate) теперь включает новые `ApplicationWillEnterForeground` `ApplicationDidEnterBackground` `HandleBackgroundTasks` методы и для отслеживания изменений состояния приложения и управления обновлениями фоновых задач.
- Добавлен новый класс [вкжестуререкогнизер](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) для предоставления следующих типов распознавания жестов для приложений Watch: [вклонгпрессжестуререкогнизер](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [вкпанжестуререкогнизер](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [вксвипежестуререкогнизер](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) и [WKTapGestureRecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- Новый класс [вкинтерфацехмкамера](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) предоставляет интерфейс для любой IP-камеры, подключенной к HomeKit.
- Новый класс [вкинтерфацеинлинемовие](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) позволяет приложению отображать фильм «Афиша», который заменяется выполняемым фильмом при его касании пользователем.
- Новый класс [вкинтерфацепайментбуттон](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) позволяет приложению представлять кнопку Apple Pay в пользовательском интерфейсе, которая будет инициировать запрос на оплату при касании.
- Новый класс [вкинтерфацескнсцене](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) представляет интерфейс для отображения сцены SceneKit на Apple Watch.
- Новый класс [вкинтерфацесксцене](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) представляет интерфейс для отображения сцены SpriteKit на Apple Watch.

Чтобы узнать больше, ознакомьтесь с нашим руководством по [быстрому взаимодействию для watchOS 3](~/ios/watchos/platform/quick-interaction-techniques.md) .

<a name="Workout-App-Enhancements"></a>

## <a name="workout-app-enhancements"></a>Усовершенствования приложения для тренировок

Новые возможности для watchOS 3. приложения, связанные с тренировками, могут работать в фоновом режиме на Apple Watch. Чтобы включить эту функцию (и получить доступ к данным HealthKit), приложение должно включить `WKBackgroundModes` ключ в `Info.plist` файл со значением `workout-processing` .

Кроме того, разработчик теперь может запускать приложение watchOS тренировки из версии приложения iOS на связанном iPhone.

Чтобы узнать больше, ознакомьтесь с нашим руководством по [усовершенствованиям нашего приложения для тренировки](~/ios/watchos/platform/workout-apps.md) .

<a name="Additional-Framework-Changes"></a>

## <a name="additional-framework-changes"></a>Дополнительные изменения платформы

В дополнение к основным изменениям и дополнениям, перечисленным выше, Apple внесла много дополнительных изменений в незначительную платформу в watchOS 3.

Чтобы узнать больше, ознакомьтесь с нашим [дополнительным руководством по изменениям в платформе](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) .

<a name="Deprecated-APIs"></a>

## <a name="deprecated-apis"></a>Устаревшие интерфейсы API

Следующие API-интерфейсы являются устаревшими в watchOS 3:

- `UILocalNotification`Класс UIKit является устаревшим и должен быть заменен платформой уведомлений пользователя.

Полный список устаревших и нерекомендуемых изменений см. в документации по [различиям в watchOS 2,2 в watchOS 3,0](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) .

## <a name="related-links"></a>Связанные ссылки

- [Примеры watchOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [Новые возможности в watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
