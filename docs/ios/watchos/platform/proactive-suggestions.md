---
title: watchOS упреждающие предложения в Xamarin
description: В этой статье показано, как использовать упреждающие предложения в приложении watchOS 3 для управления участием, позволяя системе автоматически предоставлять пользователю полезную информацию.
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 246757ddc43c5480b26ab4c1360a036fd111dcfa
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435352"
---
# <a name="watchos-proactive-suggestions-in-xamarin"></a>watchOS упреждающие предложения в Xamarin

_В этой статье показано, как использовать упреждающие предложения в приложении watchOS 3 для управления участием, позволяя системе автоматически предоставлять пользователю полезную информацию._

Новые возможности для watchOS 3, упреждающие предложения предоставляют пользователям возможность привлекаться к приложению Xamarin. iOS с помощью упреждающего предоставления пользователю полезной информации в нужное время.

## <a name="about-proactive-suggestions"></a>О упреждающих предложениях

Новая возможность для watchOS 3 `NSUserActivity` включает `MapItem` свойство, которое позволяет приложению предоставлять сведения о расположении, которые можно использовать в других контекстах. Например, если приложение отображает Гостиницы и предоставляет `MapItem` расположение, если пользователь переключился на приложение Maps, будет доступно расположение отеля, который был только что просматривается.

Приложение предоставляет эту функцию системе с помощью набора технологий, таких как `NSUserActivity` , мапкит, Media Player и UIKit. Кроме того, предоставляя поддержку упреждающего предложения для приложения, она становится более глубокой Siri интеграцией бесплатно.

## <a name="location-based-suggestions"></a>Предложения на основе расположения

В watchOS 3 `NSUserActivity` класс включает `MapItem` свойство, позволяющее разработчику указать сведения о расположении, которые можно использовать в других контекстах. Например, если приложение отображает Отзывы о ресторане, разработчик может задать `MapItem` для свойства расположение ресторана, которое пользователь просматривает в приложении. Если пользователь переключается на приложение Maps, расположение ресторана автоматически становится доступным.

Если приложение поддерживает поиск приложений, оно может использовать новые компоненты адреса `CSSearchableItemAttributesSet` класса, чтобы указать расположения, которые пользователь может захотеть посетить. При установке `MapItem` свойства другие свойства автоматически заполняются.

В дополнение к заданию `Latitude` и `Longitude` свойству компонента адреса рекомендуется, чтобы приложение также предоставляя `NamedLocation` `PhoneNumbers` Свойства и, поэтому Siri может инициировать вызов в расположение.

## <a name="contextual-siri-reminders"></a>Контекстные напоминания Siri

Позволяет пользователю использовать Siri, чтобы быстро создать напоминание для просмотра содержимого, просматриваемого в данный момент в приложении позже. Например, если они просматривают отзыв о ресторане в приложении, они могут вызвать Siri и сказать *«напомнить об этом, когда я получаю домой».* Siri создаст напоминание со ссылкой на проверку в приложении.

## <a name="implementing-proactive-suggestions"></a>Реализация упреждающих предложений

Добавление поддержки упреждающего предложения в приложение Xamarin. iOS, как правило, упрощает реализацию нескольких интерфейсов API или расширение нескольких интерфейсов API, которые приложение может уже реализовать.

Упреждающие предложения работают с приложениями тремя основными способами:

- **`NSUserActivity`** — Помогает системе понять, с какой информацией в данный момент работает пользователь на экране.
- **Варианты расположения** . Если приложение предлагает или использует сведения на основе расположения, эти расширения API предлагают новые способы совместного использования этих сведений в приложениях.

И поддерживаются в приложении путем реализации следующего:

- **Контекстные напоминания, Siri** в iOS 10, `NSUserActivity` были расширены, чтобы позволить Siri быстро создать напоминание для просмотра содержимого, которое они сейчас просматривают в приложении позже.
- **Предложения по расположению** . версия iOS 10 расширяет `NSUserActivity` возможности отслеживания мест, просматриваемых внутри приложения, и повышает их во многих местах всей системы.
- **Контекстные запросы Siri**  -  `NSUserActivity` предоставляет контекст для информации, представленной внутри приложения, в Siri, чтобы пользователь мог получать направления или вызывать вызов Siri из приложения.

Все эти функции имеют один общий характер, они используют `NSUserActivity` одну форму или другую для обеспечения их функциональности. 

## <a name="nsuseractivity"></a>NSUserActivity

Как упоминалось выше, `NSUserActivity` помогает системе понять, какие сведения пользователь в данный момент работает на экране. `NSUserActivity` — это облегченный механизм кэширования состояния, позволяющий захватывать действия пользователя по мере их перемещения по приложению. Например, рассмотрим приложение ресторана:

[![Приложение ресторана](proactive-suggestions-images/activity02.png)](proactive-suggestions-images/activity02.png#lightbox)

Со следующими взаимодействиями:

1. Когда пользователь работает с приложением, `NSUserActivity` создается для повторного создания состояния приложения позже.
2. Если пользователь выполняет поиск ресторана, то выполняется та же схема создания действий.
3. И снова, когда пользователь просматривает результат. В последнем случае, если пользователь просматривает расположение и в iOS 10, система более осведомлена о определенных понятиях (например, о расположении или взаимодействии связи).

Внимательно взгляните на последний экран:

[![Полезные данные Нсусерактивити](proactive-suggestions-images/activity03.png)](proactive-suggestions-images/activity03.png#lightbox)

Здесь приложение создает `NSUserActivity` и заполняет его информацией для повторного создания состояния позже. Приложение также включало некоторые метаданные, такие как имя и адрес расположения. После создания этого действия приложение позволяет iOS понять, что оно представляет текущее состояние пользователя.

Затем приложение принимает решение о том, что действие будет объявлено при передаче, которое сохраняется как временное значение для предложений о расположении или добавляется в индекс Spotlight на устройстве для отображения в результатах поиска.

Дополнительные сведения о ведении и поиске Spotlight см. в статьях [Введение в руководства по](~/ios/platform/handoff.md) использованию [API-интерфейсов поиска в iOS 9](~/ios/platform/search/index.md) .

### <a name="creating-an-activity"></a>Создание действия

Перед созданием действия необходимо создать идентификатор типа действия для его идентификации. Идентификатор типа действия — это короткая строка, добавляемая в `NSUserActivityTypes` массив файла приложения, `Info.plist` используемая для уникальной идентификации данного типа действия пользователя. В массиве будет одна запись для каждого действия, которое поддерживает приложение и предоставляет поиск приложения. Дополнительные сведения см. в [справочнике по созданию идентификаторов типов действий](~/ios/platform/search/nsuseractivity.md) .

Рассмотрим пример действия:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

Новое действие создается с помощью идентификатора типа действия. Затем создаются некоторые метаданные, определяющие действие, чтобы это состояние можно было восстановить позже. Затем действие получает значимое название и прикрепляется к сведениям о пользователе. Наконец, некоторые возможности включены и действие отправляется в систему.

Приведенный выше код можно дополнительно улучшить, включив в него метаданные, которые предоставляют контекст для действия, внося следующие изменения:

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

Если у разработчика есть веб-сайт, который может отображать те же сведения, что и приложение, приложение может включать URL-адрес, а содержимое может отображаться на других устройствах, на которых не установлено приложение (через "переносить"):

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Восстановление действия

Чтобы ответить пользователю, коснувшись результата поиска ( `NSUserActivity` ) для приложения, измените файл **AppDelegate.CS** и переопределите `ContinueUserActivity` метод. Пример:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Убедитесь, что это тот же идентификатор типа действия ( `com.xamarin.platform` ), что и созданное ранее действие. Приложение использует сведения, хранящиеся в компоненте, `NSUserActivity` для восстановления состояния, в котором пользователь вышел из системы.

### <a name="benefits-of-creating-an-activity"></a>Преимущества создания действия

С минимальным объемом приведенного выше кода приложение теперь может воспользоваться преимуществами трех новых функций iOS 10:

- **Handoff**
- **Поиск Spotlight**
- **Контекстные напоминания Siri**

В следующем разделе мы рассмотрим включение двух других новых функций iOS 10:

- **Варианты расположения**
- **Контекстные запросы Siri**

### <a name="location-based-suggestions"></a>Предложения на основе расположения 

Возьмем пример приложения для поиска ресторанов выше. Если `NSUserActivity` все метаданные и атрибуты были реализованы и правильно заполнены, пользователь сможет сделать следующее:

1. Найдите Ресторан в приложении, которое хотели бы встретиться с друзьями по адресу.
2. Если пользователь переключается на приложение Maps, адрес ресторана автоматически предлагается в качестве назначения.
3. Это даже работает для сторонних приложений (поддерживающих `NSUserActivity` ), поэтому пользователь может переключиться на приложение с общим доступом, и адрес ресторана будет автоматически предложен в качестве назначения.
4. Он также предоставляет контекст для Siri, поэтому пользователь может вызвать Siri в приложении ресторана и спросить *«Get Directions...»* , и Siri предоставит направления для ресторана, который просматривает пользователь.

Все функции, описанные выше, имеют один общий характер, и все они указывают, откуда поступило предложение. В примере выше это вымышленное приложение для проверки ресторана.

watchOS 3 улучшена, чтобы реализовать эту функциональность для приложения с помощью нескольких небольших изменений и дополнений к существующим платформам:

- `NSUserActivity` содержит дополнительные поля для записи сведений о местоположении, которые просматриваются в приложении.
- В Мапкит и Кореспотлигхт было внесено несколько дополнений для захвата расположения.
- Функции, учитывающие расположение, добавлены в Siri, Maps, многозадачность и другие приложения в системе.

Чтобы реализовать предложения на основе расположения, начните с того же кода действия, приведенного выше:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

Если приложение использует Мапкит, то просто добавьте текущую карту `MKMapItem` в действие:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Если приложение не использует Мапкит, оно может внедрить Поиск приложения и указать следующие новые атрибуты для расположения:

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

Подробнее рассмотрим приведенный выше код. Во-первых, необходимо указать имя расположения в каждом экземпляре:

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Затем текстовое описание, которое требуется для текстовых экземпляров (например, клавиатура Куикктипе):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Широту и долготу необязательны, но убедитесь, что пользователь направляется в точное расположение, куда приложение хочет отправить их:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Устанавливая номера телефонов, приложение может получить доступ к Siri, чтобы пользователь мог вызвать Siri из приложения, указав нечто вроде, * "позвонить в этом месте":

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Наконец, приложение может указать, подходит ли экземпляр для навигации и телефонных звонков:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

## <a name="activities-best-practices"></a>Рекомендации по действиям

При работе с действиями компания Apple предлагает следующие рекомендации:

- Используется `NeedsSave` для обновлений отложенных полезных данных.
- Обязательно обеспечьте строгую ссылку на текущее действие.
- Переносите небольшие полезные данные, которые содержат достаточно информации для восстановления состояния.
- Убедитесь, что идентификаторы типов действий являются уникальными и описательными, используя нотацию в обратную DNS, чтобы указать их. 

## <a name="consuming-location-suggestions"></a>Использование предложений по расположению

В следующем разделе рассматривается использование предложения Location, полученного из других частей системы (например, приложения Maps) или других сторонних приложений.

## <a name="routing-apps-and-locations-suggestions"></a>Предложения по маршрутизации приложений и расположений

В этом разделе рассматривается использование предложений о расположении непосредственно из приложения маршрутизации. Чтобы приложение маршрутизации добавили эту функцию, разработчик будет использовать имеющуюся `MKDirectionsRequest` платформу следующим образом:

- Для продвижения приложения в многозадачной области.
- Для регистрации приложения в качестве приложения маршрутизации.
- Для управления запуском приложения с помощью `MKDirectionsRequest` объекта мапкит.
- Дайте watchOS возможность узнать, как предложить приложение на основе участия пользователя.

Когда приложение запускается с помощью `MKDirectionsRequest` объекта мапкит, оно должно автоматически начать перенаправление пользователя в запрошенное расположение или предоставить пользовательский интерфейс, который позволяет пользователю легко начать получать указания. Пример:

```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
    {
        ...

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
                var request = new MKDirectionsRequest (url);
                var coordinate = request.Destination?.Placemark.Location?.Coordinate;
                var address = request.Destination.Placemark.AddressDictionary;
                if (coordinate.IsValid()) {
                    var geocoder = new CLGeocoder ();
                    geocoder.GeocodeAddress (address, (place, err) => {
                        // Handle the display of the address

                    });
                }
            }

            return true;
        }
    }
}
```

Подробно изучите этот код. Он проверяет, является ли он допустимым запросом на назначение:

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Если это так, он создает `MKDirectionsRequest` из URL-адреса:

```csharp
var request = new MKDirectionsRequest(url);
```

В watchOS 3 вы можете отправить в приложение адрес, у которого нет географических координат. в этом случае разработчику нужно будет закодировать адрес:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address

});

```

## <a name="summary"></a>Сводка

В этой статье были рассмотрены упреждающие предложения и показано, как разработчик может использовать их для передачи трафика в приложение Xamarin. iOS для watchOS. В нем рассмотрены действия по реализации упреждающего предложения и предоставлены рекомендации по использованию.

## <a name="related-links"></a>Связанные ссылки

- [Примеры watchOS](/samples/browse/?products=xamarin&term=Xamarin.iOS%2bwatchOS)
- [SiriKit по программированию](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)