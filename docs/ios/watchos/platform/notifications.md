---
title: Уведомления watchOS в Xamarin
description: В этом документе описывается работа с уведомлениями watchOS в Xamarin. В нем обсуждается создание контроллеров уведомлений, создание уведомлений и тестирование уведомлений.
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: c8275811cb61aaa1a033ca414e9abd4d20ecc873
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435431"
---
# <a name="watchos-notifications-in-xamarin"></a>Уведомления watchOS в Xamarin

Просмотр приложений может получать уведомления, если содержащее приложение iOS поддерживает их. Есть встроенная обработка уведомлений, поэтому вам не *нужно* добавлять дополнительную поддержку уведомлений, описанную ниже, но если вы хотите настроить поведение и внешний вид уведомлений, то можете прочесть его.

Дополнительные сведения о добавлении поддержки уведомлений в приложение iOS в решении см. в документации по [уведомлениям iOS](~/ios/platform/user-notifications/deprecated/index.md) .

## <a name="creating-notification-controllers"></a>Создание контроллеров уведомлений

На контроллерах уведомлений раскадровки имеет особый тип перехода, который запускает их. При перетаскивании нового **контроллера интерфейса уведомлений** в раскадровку он автоматически будет иметь присоединенный перехода:

![Новый контроллер интерфейса уведомлений с подключенным перехода](notifications-images/notification-storyboard1.png)

При выборе уведомления перехода можно изменить его свойства:

![Выбранное уведомление перехода](notifications-images/notification-storyboard2.png)

После настройки контроллера он может выглядеть, как в следующем примере, из Ватчкиткаталог:

![Свойства уведомления](notifications-images/notifications-segue.png)

Существует два типа уведомлений:

- Статическое представление с **коротким** просмотром, не поддерживающее прокрутку, определенное системой.

- Настраиваемое представление с прокруткой **длинным** и прокручиваемым, определяемое вами! Можно указать более простую, статическую и более сложную динамическую версию.

### <a name="short-look-notification-controller"></a>Краткий обзор контроллера уведомлений

Пользовательский интерфейс короткого поиска состоит только из значка приложения, имени приложения и строки заголовка уведомления.

Если пользователь не пропускает уведомление, система автоматически переключается на уведомление о длительном просмотре, содержащее дополнительные сведения.

### <a name="long-look-notification-controller"></a>Контроллер уведомлений длительного вида

Операционная система принимает решение, следует ли отображать статическое или динамическое представление на основе ряда факторов. Необходимо предоставить статический интерфейс и при необходимости также включить динамический интерфейс для уведомлений.

#### <a name="static"></a>Статические

Статическое представление должно быть простым и быстрым для отображения.

![Статическое представление](notifications-images/notification-static.png)

#### <a name="dynamic"></a>Динамический

Динамическое представление может отображать больше данных и предоставляет больше интерактивных возможностей.

![Динамическое представление](notifications-images/notification-dynamic.png)

## <a name="generating-notifications"></a>Создание уведомлений

Уведомления могут поступать с удаленного сервера или могут быть созданы локально в приложении iOS.

Пример создания локальных уведомлений см. в [пошаговом руководстве по уведомлениям iOS](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) .

Локальные уведомления должны иметь `AlertTitle` набор, отображаемый на Apple Watch `AlertTitle` . строка отображается в интерфейсе короткого поиска. И отображаются `AlertTitle` `AlertBody` в списке уведомлений; и `AlertBody` отображаются в интерфейсе с длинным просмотром.

На этом снимке экрана показано, что `AlertTitle` отображается в списке уведомлений, и `AlertBody` отображается в интерфейсе с длинным просмотром:

![На этом снимке экрана показан AlertTitle, отображаемый в списке уведомлений.](notifications-images/watch-notificationslist-sml.png) ![Алертбоди, отображаемый в интерфейсе длинного вида](notifications-images/watch-notificationcontroller-sml.png)

## <a name="testing-notifications"></a>Тестирование уведомлений

Уведомления (локальные и удаленные) можно только правильно протестировать на устройстве, однако их можно имитировать с помощью **JSON** -файла в симуляторе iOS.

### <a name="testing-on-apple-watch"></a>Тестирование на Apple Watch

При тестировании уведомлений на Apple Watch Помните, что в [документации Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) указано следующее:

> Когда одно из локальных или удаленных уведомлений вашего приложения поступает на устройстве iPhone пользователя, iOS принимает решение, следует ли отображать это уведомление на устройстве iPhone или на Apple Watch.

Это аллудинг тот факт, что iOS определяет, будет ли уведомление отображаться на iPhone или на часах. Если парное устройство iPhone активно при получении уведомления, это уведомление, скорее всего, будет отображаться на устройстве iPhone и *не* будет направлено на контрольное значение.

Чтобы убедиться, что уведомление отображается в контрольном списке, выключите экран iPhone (нажмите кнопку питания один раз) или разрешите переход в спящий режим. Если парное контрольное значение находится в диапазоне, оно будет направляться в надеты, и оно появится в контрольном списке (незаметно).

### <a name="testing-on-the-ios-simulator"></a>Тестирование в симуляторе iOS

При тестировании режима уведомления в симуляторе iOS *необходимо* предоставить тестовую ПОЛЕЗНУЮ нагрузку JSON. Задайте путь в окне **пользовательские аргументы выполнения** в Visual Studio для Mac.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Visual Studio для Mac отображает дополнительные параметры, если в качестве **запускаемого проекта**задано расширение Watch.
Щелкните правой кнопкой мыши проект расширения Watch и выберите команду **выполнить с > пользовательских параметров...**:

[![Работа с пользовательскими свойствами](notifications-images/runwith-customparams-sml.png)](notifications-images/runwith-customparams.png#lightbox)

Откроется окно **аргументы выполнения** , содержащее вкладку **WatchKit** . Выберите **уведомление** и укажите полезные данные JSON, а затем нажмите кнопку **выполнить** , чтобы запустить приложение Watch в симуляторе:

[![Выбор полезных данных уведомления по умолчанию](notifications-images/runwith-execargs-sml.png)](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Чтобы задать полезные данные тестового уведомления в Visual Studio, щелкните расширение Watch правой кнопкой мыши, чтобы изменить **Свойства проекта**. Перейдите к разделу **Отладка** и выберите JSON-файл уведомлений из списка (он автоматически перечислит все файлы JSON, содержащиеся в проекте).

[![Выбор JSON-файла уведомлений](notifications-images/runwith-execargs-sml-vs.png)](notifications-images/runwith-execargs-vs.png#lightbox)

Если расширение Watch является **запускаемым проектом**, Visual Studio отобразит дополнительные параметры, как показано ниже. Выберите один из параметров **уведомления** , чтобы запустить приложение Watch в режиме **уведомления** (с помощью JSON-файла, выбранного в окне "Свойства"):

![Меню "устройство"](notifications-images/runwith-vs.png)

-----

Контроллер уведомлений по умолчанию выглядит следующим образом при тестировании в симуляторе с помощью файла полезных данных JSON по умолчанию:

![Пример уведомления](notifications-images/notification-debug-sml.png)

Также можно использовать [командную строку](~/ios/watchos/troubleshooting.md#command_line) для запуска симулятора iOS.

### <a name="example-notification-payload"></a>Пример полезных данных уведомления

В примере из [каталога контрольных материалов](/samples/xamarin/ios-samples/watchos-watchkitcatalog) приведен пример файла полезных данных JSON **NotificationPayload.jsв** (см. ниже).

```json
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```

## <a name="related-links"></a>Связанные ссылки

- [Ватчкиткаталог (пример)](/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Документация по уведомлениям в комплекте Apple Watch](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)