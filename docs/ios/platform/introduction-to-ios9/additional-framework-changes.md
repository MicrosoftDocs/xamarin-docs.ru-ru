---
title: Дополнительные изменения платформ iOS 9
description: В этом документе описаны дополнительные изменения платформы, появившиеся в iOS 9. В нем обсуждаются Авфаундатион, Авкит и CloudKit.
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 8a4b1b5ed3089cb7044850cf19629fa063e43703
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436343"
---
# <a name="additional-ios-9-frameworks-changes"></a>Дополнительные изменения платформ iOS 9

_В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих платформ для iOS 9._

[![Логотип iOS 9](additional-framework-changes-images/ios9-sml.png)](additional-framework-changes-images/ios9.png#lightbox)

В дополнение к основным изменениям в iOS компания Apple внесла изменения в несколько существующих платформ в iOS 9.

## <a name="avfoundation-framework-additions"></a>Дополнения Авфаундатион Framework

В Авфаундатион Framework класс [авспичсинсесисвоице](xref:AVFoundation.AVSpeechSynthesisVoice) теперь позволяет указать в дополнение к языку по идентификатору голоса.

Например, следующий код возвращает список всех доступных голосов:

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

Затем можно использовать один из голосов из списка, задав его в качестве `Voice` Свойства экземпляра класса [авспеачуттеранце](xref:AVFoundation.AVSpeechUtterance) .

Класс [авкуеуеплайер](xref:AVFoundation.AVQueuePlayer) теперь поддерживает смесь потоковой передачи в Интернете и файлового носителя в очереди. В предыдущих версиях можно было только ставить в очередь носители одного типа.

Дополнительные сведения см. в [справочнике по Авспичсинсесисвоице](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice)Apple.

## <a name="avkit-framework-additions"></a>Дополнения Авкит Framework

Для работы с новой функцией "изображение в картинке" (PIP) платформа Авкит включает в себя новые `AVPictureInPictureController` классы и [авплайервиевконтроллер](xref:AVKit.AVPlayerViewController) :

- **Авпиктуреинпиктуреконтроллер** — этот класс позволяет приложению iOS 9 отвечать на запросы пользователя, запускающего воспроизведение видео в перемещаемом окне PIP с изменяемым размером на iPad.
- **Авплайервиевконтроллер** — управляет `AVPlayer` контроллером, используемым для представления видео в ПЕРЕМЕЩАЕМом окне PIP с изменяемым размером на iPad.

Дополнительные сведения см. в статье о [многозадачности для iPad](~/ios/platform/introduction-to-ios9/index.md#multitasking) , [справочнике](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) по Apple авпиктуреинпиктуреконтроллер и [справочнике по авплайервиевконтроллер](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>Знакомство с веб-службами CloudKit

Платформа CloudKit упрощает разработку приложений, обращающихся к iCloud. Сюда входит получение данных приложения и права доступа к ресурсам, а также возможность безопасного хранения сведений о приложении. Этот пакет обеспечивает пользователям уровень анонимности, предоставляя доступ к приложениям с помощью идентификаторов iCloud без предоставления персональных данных.

Новая платформа _веб-служб CloudKit_ предоставляет библиотеку JavaScript (CloudKit JS), которая может быть включена на веб-сайте для предоставления доступа к тем же данным и содержимому CloudKit, что и ваше приложение Xamarin. iOS.

> [!IMPORTANT]
> Прежде чем получить доступ к, представлять или обновлять содержимое базы данных CloudKit с помощью CloudKit JS, необходимо предварительно определить схему этой базы данных.

Дополнительные сведения см. в следующих документах:

- [Введение в CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) . наши общие сведения об использовании CloudKit в приложении Xamarin. iOS.
- [CloudKit быстрое начало](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) — введение Apple в CloudKit.
- [Справочник по CLOUDKIT JS](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) — документация по Apple CloudKit JS.
- [Каталог CloudKit. Введение в CloudKit (Cocoa и JavaScript)](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) — пример приложения Apple с использованием CloudKit и CloudKit JS.

> [!IMPORTANT]
> Компания Apple [предоставляет инструменты](https://developer.apple.com/support/allowing-users-to-manage-data/), которые помогают разработчикам надлежащим образом соблюдать Общий регламент по защите данных Европейского союза (GDPR).

## <a name="foundation-framework-additions"></a>Дополнения к Foundation Framework

Компания Apple включила следующие изменения в платформу Foundation в iOS 9:

### <a name="changes-to-nsbundle"></a>Изменения в Нсбундле

В класс [нсбундле](xref:Foundation.NSBundle) для iOS 9 были внесены следующие изменения:

- `GetPreservationPriorityForTag (NSString tag)` — Получает текущий приоритет сохранения для ресурсов с заданным тегом. Допустимые значения находятся в диапазоне от `0.0` до `1.0` , первыми очищаются ресурсы с наименьшим приоритетом.
- `SetPreservationPriorityForTag (double priority, NSSet tags)` — Задает текущий приоритет хранения для ресурсов с заданными тегами. Допустимые значения находятся в диапазоне от `0.0` до `1.0` , первыми очищаются ресурсы с наименьшим приоритетом.

Дополнительные сведения см. в [справочнике по Нсбундле](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle)Apple.

### <a name="changes-to-nsprocessinfo"></a>Изменения в Нспроцессинфо

Каждый процесс, выполняемый на устройстве iOS, имеет единый _Агент обработки данных процесса_ (PIA). Используйте класс [нспроцессинфо](xref:Foundation.NSProcessInfo) для предоставления сведений о текущем PIA и управлении питанием и температурным управлением для данного процесса.

Например, чтобы управлять автоматическим завершением процесса, можно использовать следующий код:

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

Дополнительные сведения см. в [справочнике по Нспроцессинфо](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo)Apple.

### <a name="reacting-to-low-power-mode"></a>Реагирование на режим низкого энергопотребления

Используйте `LowPowerModeEnabled` свойство класса [нспроцессинфо](xref:Foundation.NSProcessInfo) , чтобы определить, включен ли режим низкого энергопотребления на устройстве iOS, на котором запущено приложение. Пример:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>Изменения платформы HealthKit

Компания Apple включила следующие изменения в платформу [HealthKit](xref:HealthKit) в iOS 9:

- Поддержка полного удаления и отслеживания удаления записей в базе данных HealthKit. Дополнительные сведения см. в [справочнике по классам](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) [хкделетедобжект](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [хканчоредобжекткуери](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) и хкхеалссторе Apple.
- Новые категории и характеристики отслеживания были добавлены в `HKQuantityTypeIdentifier` класс (например, `UVExposure` ) и в `HKCategoryTypeIdentifier` класс (например, `OvulationTestResult` ). 

Дополнительные сведения о работе с HealthKit в Xamarin. iOS см. в статье [Введение в](~/ios/platform/healthkit.md) документацию по HealthKit.

## <a name="local-authentication-framework-changes"></a>Изменения в инфраструктуре локальной проверки подлинности

Компания Apple включила следующие изменения в [локальную инфраструктуру проверки подлинности](xref:LocalAuthentication) в iOS 9:

- С помощью `EvaluateAccessControl` `EvaluatePolicy` методов и класса [лаконтекст](xref:LocalAuthentication.LAContext) теперь можно повторно использовать идентификаторы сенсорного экрана, соответствующие предыдущим успешным попыткам разблокировки.
- Возможность получить список зарегистрированных пальцев.
- Поддержка отслеживания при добавлении или удалении пальца из проверки подлинности.
- Возможность использования _контекста проверки подлинности_ в вызовах цепочки ключей и поддержки для оценки списков управления доступом к цепочке ключей.
- Возможность отмены запроса пользователя из кода.

Дополнительные сведения см. [в статье идентификатор Touch и идентификатор лица с Xamarin. iOS](~/ios/platform/touch-id-face-id.md).

### <a name="lacontext-changes"></a>Лаконтекст изменения

В класс [лаконтекст](xref:LocalAuthentication.LAContext) для iOS 9 были внесены следующие изменения:

- **Таучидаусентикатионмаксимумалловаблереуседуратион** — возвращает максимальное время, в течение которого можно повторно использовать проверку подлинности Touch ID.
- **Евалуатедполицидомаинстате** — Возвращает или задает состояние оцененной политики.
- **Максбиометрифаилурес** — амортизируется в iOS 9.
- **Таучидаусентикатионалловаблереуседуратион** Возвращает или задает время, в течение которого можно повторно использовать проверку подлинности Touch ID.
- **Евалуатеакцессконтрол** — асинхронно оценивает политику проверки подлинности.
- **Аннулирование** — делает недействительным данный идентификатор проверки подлинности с помощью сенсорного ввода.
- **Credential** -возвращает значение, `true` Если учетные данные в настоящее время заданы.
- **Сеткредентиалтипе** Задает указанный тип учетных данных.

Дополнительные сведения см. в [справочнике по Лаконтекст](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) Apple.

## <a name="mapkit-framework-changes"></a>Изменения платформы Мапкит

Компания Apple включила следующие изменения в платформу [мапкит](xref:MapKit) в iOS 9:

- Мапкит теперь предоставляет поддержку для запуска приложения Map непосредственно в направлениях передачи и для запроса транзитного предполагаемого времени прибытия (с) с помощью классов [мклаунчоптионс](xref:MapKit.MKLaunchOptions) и [мкдиректионс](xref:MapKit.MKLaunchOptions) .
- Результаты поиска, возвращаемые функцией Мапкит и классом [клжеокодер](xref:CoreLocation.CLGeocoder) , также могут предоставлять часовой пояс результата.
- Теперь можно полностью настроить заметки к карте, представленные приложением iOS, с помощью `DetailCalloutAccessoryView` Свойства класса [мканнотатионвиев](xref:MapKit.MKAnnotationView) .

Дополнительные сведения о работе с картами и заметками в [документации по Xamarin](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) . iOS и Apple см. в статье о [картах iOS](~/ios/user-interface/controls/ios-maps/index.md) и [пошаговом руководстве по мапкит](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) .

## <a name="passkit-framework-additions"></a>Дополнения PassKit Framework

Компания Apple включила следующие изменения в платформу [PassKit](xref:PassKit) в iOS 9:

- Apple Pay теперь поддерживает как дебетовые, так и кредитные карты, а также карты обнаружения. Дополнительные сведения см. в разделе " **платежные сети** " [справочника по классу пкпайментрекуест](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) Apple.
- Непосредственно в приложении Xamarin. iOS теперь можно добавлять платежные сети и издатели карточек в Apple Pay. Дополнительные сведения см. в [справочнике по классу Пкаддпайментпассвиевконтроллер](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) Apple.

Дополнительные сведения о работе с PassKit в Xamarin. iOS см. в статье [Введение в](~/ios/platform/passkit.md) документацию по PassKit.

## <a name="safari-services-framework-additions"></a>Дополнения к платформе служб Safari

Компания Apple включила следующие изменения в платформу [служб Safari](xref:SafariServices) в iOS 9:

- Теперь можно использовать новый класс [сфсафаривиевконтроллер](xref:SafariServices.SFSafariViewController) для показа веб-содержимого в приложении Xamarin. iOS. Он обеспечивает возможность совместного использования данных веб-сайта и файлов cookie с помощью приложения Safari и включает несколько функций Safari (таких как читатель и автозаполнение). [Сфсафаривиевконтроллер](xref:SafariServices.SFSafariViewController) применяет кнопку **Готово** , которая вернет пользователей в приложение по завершении просмотра веб-содержимого.

Поскольку класс [сфсафаривиевконтроллер](xref:SafariServices.SFSafariViewController) предназначен для отображения одной страницы веб-содержимого, рекомендуется использовать его для замены любых элементов управления [вквебкит](xref:WebKit.WKWebView) или [уивебвиев](xref:UIKit.UIWebView) в существующих приложениях Xamarin. iOS.

### <a name="displaying-a-website"></a>Отображение веб-сайта

Приведенный ниже код является примером вызова [сфсафаривиевконтроллер](xref:SafariServices.SFSafariViewController) из другого контроллера представления:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>Изменения платформы UIKit

Компания Apple включила множество улучшений в несколько элементов платформы [UIKit](xref:UIKit) для iOS 9. Эти изменения будут подробно описаны в следующих разделах.

### <a name="3d-touch-events"></a>События трехмерного касания

В iOS 9 и iPhone 6S and iPhone 6S Plus трехмерное касание добавляет жесты с учетом нажима в приложения iOS. В результате, если приложение выполняется в iOS 9 (или более поздней версии), а устройство iOS может поддерживать трехмерное касание, изменения в нажиме приведут к `TouchesMoved` возникновению события.

В связи с этим изменением поведения приложения iOS должны быть готовы к `TouchesMoved` вызову события чаще, даже если координаты X/Y не изменялись.

Дополнительные сведения см. [в статье Введение в трехмерное сенсорное](~/ios/platform/3d-touch.md) руководством.

### <a name="document-open-in-place-functionality"></a>Возможность открытия документов на месте

С помощью `FinishedLaunching (application, launchOptions)` `WillFinishLaunching (Application, launchOptions)` методов или класса [уиаппликатионделегате](xref:UIKit.UIApplicationDelegate) теперь можно открыть документ и изменить его на месте (в отличие от работы с копией).

Чтобы обеспечить поддержку новой функциональности с открытым размещением, добавьте `LSSupportsOpeningDocumentsInPlace` ключ в файл **info. plist** приложения Xamarin. iOS со значением `YES` .

Дополнительные сведения см. в [справочнике по Уиаппликатионделегате](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) Apple.

### <a name="enhanced-touch-events"></a>Улучшенные события касания

Компания Apple предоставила несколько улучшений для событий касания в iOS 9. В их число входит возможность использования сенсорного прогнозирования и получения доступа к промежуточным касаниям между обновлениями дисплея.

Дополнительные сведения см. в статье [по обработке событий Apple для iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) .

### <a name="fetching-tailored-content"></a>Получение специализированного содержимого

Новый `NSDataAsset` класс позволяет приложению Xamarin. iOS получать содержимое, адаптированное к памяти и графическим возможностям устройства iOS, на котором она выполняется в данный момент.

### <a name="new-layout-anchors"></a>Новые привязки макета

`NSLayoutAnchor` `NSLayoutDimension` Классы привязки New и Layout работают с новыми свойствами привязки класса [UIView](xref:UIKit.UIView) (например `LeadingAnchor` , и `WidthAnchor` ), чтобы упростить создание макета в iOS 9.

Дополнительные сведения о работе с классами автомакета и размера в приложении Xamarin. iOS и Справочнике по [нслайаутанчор](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [Нслайаутдименсион](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) Reference и [UIView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) см. в статье [Введение в унифицированные раскадровки](~/ios/user-interface/storyboards/unified-storyboards.md) .

### <a name="new-readable-content-margins"></a>Новые доступные для чтения поля содержимого

Новый `UILayoutGuide` класс можно использовать для предоставления доступных для чтения полей содержимого и определения областей рисования содержимого внутри представления. Дополнительные сведения см. в [справочнике по Уилайаутгуиде](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) Apple.

### <a name="text-input-in-notifications-modifications"></a>Ввод текста в уведомлениях об изменениях

Класс [уиусернотификатионактион](xref:UIKit.UIUserNotificationAction) содержит новое `Behavior` свойство, которое можно использовать для поддержки ввода текста из уведомлений.

### <a name="uiapplicationdelegate-changes"></a>Уиаппликатионделегате изменения

Хотя компания Apple не является устаревшей, она предлагает заменить все вызовы `FinishedLaunching (UIApplication application)` метода класса [уиаппликатионделегате](xref:UIKit.UIApplicationDelegate) методами `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` или `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` .

Дополнительные сведения см. в [справочнике по Уиаппликатионделегате](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) Apple.

### <a name="uikit-dynamics-changes"></a>Изменения UIKit Dynamics

Компания Apple включила следующие изменения в Dynamics UIKit в iOS 9:

- Теперь Dynamics обеспечивает поддержку непрямоугольных границ конфликтов.
- Новый настраиваемый `UIFieldBehavior` класс используется для поддержки различных типов полей.
- К классу добавлены дополнительные типы вложений `UIAttachmentBehavior` .

Дополнительные сведения см. в [справочнике по Уиаттачмент](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) Apple.

### <a name="uipickerview-and-uidatepicker-changes"></a>Изменения Уипиккервиев и Уидатепиккер

До iOS 9 элементы управления [уипиккервиев](xref:UIKit.UIPickerView) и [уидатепиккер](xref:UIKit.UIDatePicker) были неизменяемыми и автоматически изменяют размер, чтобы заполнить ширину контейнера (обычно это ширина устройства iOS, в котором выполнялось приложение).

В iOS 9 автоматическое изменение размера больше не происходит, и элементы управления будут подготавливаться к просмотру по ширине в 320 точек на всех устройствах iOS независимо от размера и ориентации экрана.

Чтобы исправить эту ситуацию, используйте классы с автоматической разметкой и размером, чтобы закрепить ширину элемента управления по краям родительского контейнера (представления) и указать требуемую высоту. Дополнительные сведения о работе с классами автоматического макета и размера в приложении Xamarin. iOS см. в документации по [унифицированным раскадровкам](~/ios/user-interface/storyboards/unified-storyboards.md) .

### <a name="new-uitextinputassistantitem-class"></a>Новый класс Уитекстинпутассистантитем

Используйте новый `UITextInputAssistantItem` класс для групп кнопок панели макета на _панели ярлыков_. Панель ярлыков — это новая область, доступная на экранной клавиатуре для ввода сочетаний клавиш.

## <a name="related-links"></a>Связанные ссылки

- [Примеры для iOS 9](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS9)
- [Введение в iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [Новые возможности iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)