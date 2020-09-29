---
title: Использование iCloud с Xamarin. iOS
description: В этом документе описывается iCloud и его использование в приложениях Xamarin. iOS. Здесь обсуждаются хранилище ключей, хранилище документов и резервное копирование iCloud.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/09/2016
ms.openlocfilehash: cace68a1b85a4404774ca88ec697d419920d05cb
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91432899"
---
# <a name="using-icloud-with-xamarinios"></a>Использование iCloud с Xamarin. iOS

API хранилища iCloud в iOS 5 позволяет приложениям сохранять пользовательские документы и данные приложений в центральном расположении и обращаться к этим элементам со всех устройств пользователя.

Доступно четыре типа хранилища:

- **Хранилище "ключ — значение** " — для совместного использования небольших объемов данных с приложением на других устройствах пользователя.

- **Данных Storage** — для хранения документов и других данных в учетной записи iCloud пользователя с помощью подкласса данных.

- **CoreData** — хранилище базы данных SQLite.

- **Отдельные файлы и каталоги** — для управления множеством различных файлов непосредственно в файловой системе.

В этом документе рассматриваются первые два типа пар "ключ-значение" и подклассов данных, а также способы использования этих функций в Xamarin. iOS.

> [!IMPORTANT]
> Компания Apple [предоставляет инструменты](https://developer.apple.com/support/allowing-users-to-manage-data/), которые помогают разработчикам надлежащим образом соблюдать Общий регламент по защите данных Европейского союза (GDPR).

## <a name="requirements"></a>Требования

- Последняя стабильная версия Xamarin. iOS
- Xcode 10
- Visual Studio для Mac или Visual Studio 2019.

## <a name="preparing-for-icloud-development"></a>Подготовка к разработке в iCloud

Приложения должны быть настроены для использования iCloud как на [портале подготовки Apple](https://developer.apple.com/account/ios/overview.action) , так и в самом проекте. Перед началом разработки для iCloud (или пробных примеров) выполните приведенные ниже действия.

Чтобы правильно настроить приложение для доступа к iCloud, выполните следующие действия.

- **Найдите теамид** -login для  [Developer.Apple.com](https://developer.apple.com) и посетите  **центр участников > свою учетную запись > учетной записи разработчика** , чтобы получить идентификатор команды (или отдельный идентификатор для отдельных разработчиков). Это будет строка из 10 символов (например, **A93A5CM278** ). Эта форма является частью "идентификатора контейнера".

- **Создание идентификатора приложения** . чтобы создать идентификатор приложения, выполните действия, описанные в  [разделе Подготовка для технологий хранения статьи Руководство по подготовке устройств](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md), и убедитесь, что в качестве разрешенной службы установлен флажок **iCloud** .

 [![Проверка iCloud как разрешенной службы](introduction-to-icloud-images/icloud-sml.png)](introduction-to-icloud-images/icloud.png#lightbox)

- **Создание нового профиля подготовки** . чтобы создать профиль подготовки, выполните действия, описанные в  [руководстве по подготовке устройств](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) .

- **Добавьте идентификатор контейнера в права. plist** — формат идентификатора контейнера — `TeamID.BundleID` . Дополнительные сведения см. в руководстве по [работе с](~/ios/deploy-test/provisioning/entitlements.md) назначениями.

- **Настройте свойства проекта** — в файле info. plist убедитесь, что **идентификатор пакета** соответствует набору **идентификаторов пакета** при [создании идентификатора приложения](~/ios/deploy-test/provisioning/capabilities/index.md). Подписывание пакета iOS использует **профиль подготовки** , который содержит идентификатор приложения в службе приложений iCloud, и **выбранный файл прав** . Это можно сделать в Visual Studio в области Свойства проекта.

- **Включение iCloud на устройстве** — перейдите в раздел **Параметры > iCloud** и убедитесь, что устройство зарегистрировано.
Выберите и включите параметр **documents & Data** .

- **Для тестирования iCloud необходимо использовать устройство** . оно не будет работать в симуляторе.
На самом деле, для просмотра iCloud в действии необходимо, чтобы два или больше устройств вошли в учетную информацию с одним и тем же идентификатором Apple ID.

## <a name="key-value-storage"></a>Хранилище "ключ — значение"

Хранилище "ключ — значение" предназначено для небольших объемов данных, которые пользователь может сохранить на разных устройствах, например на последней странице, которую они просматривали в книге или журнале. Хранилище "ключ-значение" не следует использовать для резервного копирования данных.

Существуют некоторые ограничения, которые следует учитывать при использовании хранилища ключей и значений.

- **Максимальный размер ключа** — имена ключей не могут быть длиннее 64 байт.

- **Максимальный размер значения** — нельзя хранить более 64 килобайт в одном значении.

- **Максимальный размер хранилища "ключ — значение" для** приложений — приложения могут хранить всего до 64 КБ данных типа "ключ — значение". Попытки задать ключи за пределами этого ограничения завершатся ошибкой, и предыдущее значение будет сохранено.

- **Типы данных** . можно хранить только базовые типы, такие как строки, числа и логические значения.

В примере **иклаудкэйвалуе** показано, как это работает. В примере кода создается ключ с именем для каждого устройства. Этот ключ можно задать на одном устройстве и посмотреть, что значение будет распространено на другие. Он также создает раздел "Shared" (общий), который можно редактировать на любом устройстве. Если вы редактируете на нескольких устройствах одновременно, iCloud выбирает значение "WINS" (с использованием отметки времени изменения) и распространяется.

На этом снимке экрана показан пример использования. При получении уведомлений об изменениях из iCloud они выводятся в виде прокручиваемого текста в нижней части экрана и обновляются в полях ввода.

 [![Поток сообщений между устройствами](introduction-to-icloud-images/icloud-kv-arrows.png)](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Установка и извлечение данных

В этом коде показано, как задать строковое значение.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Вызов функции Synchronize гарантирует, что значение сохраняется только в локальном хранилище диска. Синхронизация в iCloud происходит в фоновом режиме и не может быть "принудительной" кодом приложения. При хорошем подключении к сети Синхронизация часто происходит в течение 5 секунд, однако, если сеть низкая (или отключена), обновление может занять много времени.

Получить значение можно с помощью этого кода:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Значение извлекается из локального хранилища данных. Этот метод не пытается связаться с серверами iCloud, чтобы получить значение "latest". iCloud будет обновлять локальное хранилище данных по собственному расписанию.

### <a name="deleting-data"></a>Удаление данных

Чтобы полностью удалить пару "ключ-значение", используйте метод Remove следующим образом:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Наблюдение за изменениями

Приложение также может получать уведомления при изменении значений в iCloud путем добавления наблюдателя в `NSNotificationCenter.DefaultCenter` .
В следующем коде из метода **KeyValueViewController.CS** `ViewWillAppear` показано, как прослушивать эти уведомления и создать список ключей, которые были изменены:

```csharp
keyValueNotification =
NSNotificationCenter.DefaultCenter.AddObserver (
    NSUbiquitousKeyValueStore.DidChangeExternallyNotification, notification => {
    Console.WriteLine ("Cloud notification received");
    NSDictionary userInfo = notification.UserInfo;

    var reasonNumber = (NSNumber)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangeReasonKey);
    nint reason = reasonNumber.NIntValue;

    var changedKeys = (NSArray)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangedKeysKey);
    var changedKeysList = new List<string> ();
    for (uint i = 0; i < changedKeys.Count; i++) {
        var key = changedKeys.GetItem<NSString> (i); // resolve key to a string
        changedKeysList.Add (key);
    }
    // now do something with the list...
});
```

Код может выполнить определенное действие со списком измененных ключей, например обновить локальную копию или обновить пользовательский интерфейс новыми значениями.

Возможные причины изменения: Серверчанже (0), Инитиалсинкчанже (1) или Куотавиолатиончанже (2). Вы можете получить доступ к причине и при необходимости выполнить другую обработку (например, может потребоваться удалить некоторые ключи в результате *куотавиолатиончанже*).

## <a name="document-storage"></a>Хранилище документов

Хранилище документов iCloud предназначено для управления данными, важными для приложения (и для пользователя). Он может использоваться для управления файлами и другими данными, которые должны выполняться приложением, в то же время предоставляя функции резервного копирования и совместного использования на основе iCloud на всех устройствах пользователей.

На этой схеме показано, как все они объединяются. На каждом устройстве хранятся данные, сохраненные в локальном хранилище (Убикуитиконтаинер), и управляющая программа iCloud, заданная операционной системой, отвечает за отправку и получение данных в облаке. Для предотвращения параллельного доступа все файлы Убикуитиконтаинер должны быть выполнены через Филепресентер/Филекурдинатор. `UIDocument`Класс реализует их. в этом примере показано, как использовать данных.

 [![Общие сведения о хранилище документов](introduction-to-icloud-images/icloud-overview.png)](introduction-to-icloud-images/icloud-overview.png#lightbox)

В примере Иклаудуидок реализуется простой `UIDocument` подкласс, содержащий одно текстовое поле. Текст отображается в, `UITextView` а изменения распространяются в iCloud на другие устройства с сообщением с уведомлением, показанным красным цветом. Пример кода не имеет решения с более сложными функциями iCloud, такими как разрешение конфликтов.

На этом снимке экрана показан пример приложения — после изменения текста и нажатия **упдатечанжекаунт** документ синхронизируется через iCloud с другими устройствами.

 [![На этом снимке экрана показан пример приложения после изменения текста и нажатия клавиши Упдатечанжекаунт](introduction-to-icloud-images/iclouduidoc.png)](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Пример Иклаудуидок состоит из пяти частей:

1. **Доступ к убикуитиконтаинер** — определите, включено ли iCloud, и, если да, путь к области хранилища iCloud приложения.

1. **Создание** подкласса данных. Создайте класс для промежуточного хранения между хранилищем iCloud и объектами модели.

1. **Поиск и открытие документов iCloud** . Используйте `NSFileManager` и `NSPredicate` для поиска документов iCloud и их открытия.

1. **Отображение документов iCloud** . Предоставьте свойства `UIDocument` , чтобы можно было взаимодействовать с элементами управления ИП.

1. **Сохранение документов iCloud** . Следите за тем, чтобы изменения, внесенные в пользовательском интерфейсе, сохранялись на дисках и iCloud.

Все операции iCloud выполняются (или выполняются) асинхронно, чтобы они не блокировались при ожидании чего-либо. В примере вы увидите три разных способа выполнения этой задачи:

 **Потоки** — при `AppDelegate.FinishedLaunching` первом вызове метода `GetUrlForUbiquityContainer` выполняются в другом потоке, чтобы предотвратить блокировку основного потока.

 **Нотификатионцентер** — регистрация для получения уведомлений при выполнении асинхронных операций, таких как `NSMetadataQuery.StartQuery` Complete.

 **Обработчики завершения** — передача методов для выполнения по завершении асинхронных операций, таких как `UIDocument.Open` .

### <a name="accessing-the-ubiquitycontainer"></a>Доступ к Убикуитиконтаинер

Первым шагом при использовании хранилища документов iCloud является определение того, включено ли iCloud, и, если это так, расположение "общедоступного контейнера" (каталог, в котором хранятся файлы с поддержкой iCloud на устройстве).

Этот код находится в `AppDelegate.FinishedLaunching` методе примера.

```csharp
// GetUrlForUbiquityContainer is blocking, Apple recommends background thread or your UI will freeze
ThreadPool.QueueUserWorkItem (_ => {
    CheckingForiCloud = true;
    Console.WriteLine ("Checking for iCloud");
    var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer (null);
    // OR instead of null you can specify "TEAMID.com.your-company.ApplicationName"

    if (uburl == null) {
        HasiCloud = false;
        Console.WriteLine ("Can't find iCloud container, check your provisioning profile and entitlements");

        InvokeOnMainThread (() => {
            var alertController = UIAlertController.Create ("No \uE049 available",
            "Check your Entitlements.plist, BundleId, TeamId and Provisioning Profile!", UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Destructive, null));
            viewController.PresentViewController (alertController, false, null);
        });
    } else { // iCloud enabled, store the NSURL for later use
        HasiCloud = true;
        iCloudUrl = uburl;
        Console.WriteLine ("yyy Yes iCloud! {0}", uburl.AbsoluteUrl);
    }
    CheckingForiCloud = false;
});
```

Хотя этот пример не делает этого, Apple рекомендует вызывать Жетурлфорубикуитиконтаинер всякий раз, когда приложение поступает на передний план.

### <a name="creating-a-uidocument-subclass"></a>Создание подкласса данных

Все файлы и каталоги iCloud (IE. все, что хранится в каталоге Убикуитиконтаинер) должны управляться с помощью методов Нсфилеманажер, реализации протокола Нсфилепресентер и записи через Нсфилекурдинатор.
Самый простой способ сделать это — не писать его самостоятельно, но подкласс данных, который делает все самостоятельно.

Существует только два метода, которые необходимо реализовать в подклассе данных для работы с iCloud:

- **Лоадфромконтентс** — передается в NSData содержимого файла для распаковки классов модели.

- **Контентсфортипе** — запрос на предоставление NSData представления класса модели/ES для сохранения на диск (и в облаке).

В этом примере кода из **иклаудуидок\монкэйдокумент.КС** показано, как реализовать данных.

```csharp
public class MonkeyDocument : UIDocument
{
    // the 'model', just a chunk of text in this case; must easily convert to NSData
    NSString dataModel;
    // model is wrapped in a nice .NET-friendly property
    public string DocumentString {
        get {
            return dataModel.ToString ();
        }
        set {
            dataModel = new NSString (value);
        }
    }
    public MonkeyDocument (NSUrl url) : base (url)
    {
        DocumentString = "(default text)";
    }
    // contents supplied by iCloud to display, update local model and display (via notification)
    public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("LoadFromContents({0})", typeName);

        if (contents != null)
            dataModel = NSString.FromData ((NSData)contents, NSStringEncoding.UTF8);

        // LoadFromContents called when an update occurs
        NSNotificationCenter.DefaultCenter.PostNotificationName ("monkeyDocumentModified", this);
        return true;
    }
    // return contents for iCloud to save (from the local model)
    public override NSObject ContentsForType (string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("ContentsForType({0})", typeName);
        Console.WriteLine ("DocumentText:{0}",dataModel);

        NSData docData = dataModel.Encode (NSStringEncoding.UTF8);
        return docData;
    }
}
```

В этом случае модель данных очень проста — одно текстовое поле. Модель данных может быть сложной по мере необходимости, например XML-документу или двоичным данным. Основной ролью реализации данных является преобразование между классами модели и представлением NSData, которое можно сохранить или загрузить на диск.

### <a name="finding-and-opening-icloud-documents"></a>Поиск и открытие документов iCloud

Пример приложения работает только с одним файлом — test.txt, поэтому код в **AppDelegate.CS** создает `NSPredicate` и, `NSMetadataQuery` в частности, для этого имени файла. `NSMetadataQuery`Выполняется асинхронно и отправляет уведомление по завершении. `DidFinishGathering` вызывается наблюдателем уведомлений, останавливает запрос и вызывает Лоаддокумент, который использует `UIDocument.Open` метод с обработчиком завершения, чтобы попытаться загрузить файл и отобразить его в `MonkeyDocumentViewController` .

```csharp
string monkeyDocFilename = "test.txt";
void FindDocument ()
{
    Console.WriteLine ("FindDocument");
    query = new NSMetadataQuery {
        SearchScopes = new NSObject [] { NSMetadataQuery.UbiquitousDocumentsScope }
    };

    var pred = NSPredicate.FromFormat ("%K == %@", new NSObject[] {
        NSMetadataQuery.ItemFSNameKey, new NSString (MonkeyDocFilename)
    });

    Console.WriteLine ("Predicate:{0}", pred.PredicateFormat);
    query.Predicate = pred;

    NSNotificationCenter.DefaultCenter.AddObserver (
        this,
        new Selector ("queryDidFinishGathering:"),
        NSMetadataQuery.DidFinishGatheringNotification,
        query
    );

    query.StartQuery ();
}

[Export ("queryDidFinishGathering:")]
void DidFinishGathering (NSNotification notification)
{
    Console.WriteLine ("DidFinishGathering");
    var metadataQuery = (NSMetadataQuery)notification.Object;
    metadataQuery.DisableUpdates ();
    metadataQuery.StopQuery ();

    NSNotificationCenter.DefaultCenter.RemoveObserver (this, NSMetadataQuery.DidFinishGatheringNotification, metadataQuery);
    LoadDocument (metadataQuery);
}

void LoadDocument (NSMetadataQuery metadataQuery)
{
    Console.WriteLine ("LoadDocument");

    if (metadataQuery.ResultCount == 1) {
        var item = (NSMetadataItem)metadataQuery.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);
        doc = new MonkeyDocument (url);

        doc.Open (success => {
            if (success) {
                Console.WriteLine ("iCloud document opened");
                Console.WriteLine (" -- {0}", doc.DocumentString);
                viewController.DisplayDocument (doc);
            } else {
                Console.WriteLine ("failed to open iCloud document");
            }
        });
    } // TODO: if no document, we need to create one
}
```

### <a name="displaying-icloud-documents"></a>Отображение документов iCloud

Отображение данных не должно отличаться от других классов модели. свойства отображаются в элементах управления пользовательского интерфейса, которые, возможно, редактируются пользователем и затем записываются обратно в модель.

В примере **иклаудуидок\монкэйдокументвиевконтроллер.КС** отображает текст монкэйдокумент в `UITextView` . `ViewDidLoad` прослушивает уведомление, отправленное в `MonkeyDocument.LoadFromContents` методе. `LoadFromContents` вызывается, когда в iCloud есть новые данные для файла, поэтому уведомление указывает на то, что документ был обновлен.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Обработчик уведомлений с примером кода вызывает метод для обновления пользовательского интерфейса, в данном случае без обнаружения или разрешения конфликтов.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>Сохранение документов iCloud

Чтобы добавить данных в iCloud, можно вызвать `UIDocument.Save` напрямую (только для новых документов) или переместить существующий файл с помощью `NSFileManager.DefaultManager.SetUbiquitious` . В примере кода создается новый документ непосредственно в универсальном контейнере с этим кодом (здесь есть два обработчика завершения, один для операции, `Save` а другой — для открытия):

```csharp
var docsFolder = Path.Combine (iCloudUrl.Path, "Documents"); // NOTE: Documents folder is user-accessible in Settings
var docPath = Path.Combine (docsFolder, MonkeyDocFilename);
var ubiq = new NSUrl (docPath, false);
var monkeyDoc = new MonkeyDocument (ubiq);
monkeyDoc.Save (monkeyDoc.FileUrl, UIDocumentSaveOperation.ForCreating, saveSuccess => {
Console.WriteLine ("Save completion:" + saveSuccess);
if (saveSuccess) {
    monkeyDoc.Open (openSuccess => {
        Console.WriteLine ("Open completion:" + openSuccess);
        if (openSuccess) {
            Console.WriteLine ("new document for iCloud");
            Console.WriteLine (" == " + monkeyDoc.DocumentString);
            viewController.DisplayDocument (monkeyDoc);
        } else {
            Console.WriteLine ("couldn't open");
        }
    });
} else {
    Console.WriteLine ("couldn't save");
}
```

Последующие изменения в документе не сохраняются напрямую, вместо этого мы будем сообщать о том `UIDocument` , что он изменился `UpdateChangeCount` , и автоматически запланировать операцию сохранения на диске:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Управление документами iCloud

Пользователи могут управлять документами iCloud в каталоге " **документы** " в "повсеместном" контейнере вне приложения с помощью параметров. они могут просмотреть список файлов и прокрутить его для удаления. Код приложения должен иметь возможность обрабатывать ситуацию, когда пользователь удаляет документы. Не храните внутренние данные приложений в каталоге **Documents** .

 [![Рабочий процесс управления документами iCloud](introduction-to-icloud-images/icloudstorage.png)](introduction-to-icloud-images/icloudstorage.png#lightbox)

Пользователи также получат различные предупреждения при попытке удалить приложение с поддержкой iCloud с устройства, чтобы сообщить им о состоянии документов iCloud, связанных с этим приложением.

 [![Пример диалогового окна, когда пользователь пытается удалить приложение с поддержкой iCloud с устройства](introduction-to-icloud-images/icloud-delete1.png)](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![Пример диалогового окна, когда пользователь пытается удалить приложение с поддержкой iCloud с устройства](introduction-to-icloud-images/icloud-delete2.png)](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>Резервное копирование iCloud

Хотя резервное копирование в iCloud не является функцией, доступ к которой осуществляется напрямую разработчиками, способ разработки приложения может повлиять на взаимодействие с пользователем.
Apple предоставляет разработчикам [рекомендации по хранению данных iOS](https://developer.apple.com/icloud/documentation/data-storage/) в своих приложениях iOS.

Самое важное обстоятельство в том, что приложение хранит большие файлы, которые не создаются пользователем (например, приложение для чтения журнала, в котором хранятся сотни и мегабайты содержимого на одну ошибку). Apple предпочитает не хранить данные такого рода, где они будут архивироваться в iCloud и без необходимости заполнять квоту iCloud пользователя.

Приложения, хранящие большие объемы данных, такие как, должны либо хранить их в одном из пользовательских каталогов, не имеющих резервных копий (например, Или использовать `NSFileManager.SetSkipBackupAttribute` для применения флага к этим файлам, чтобы в iCloud они игнорировались во время операций резервного копирования.

## <a name="summary"></a>Сводка

В этой статье появилась новая функция iCloud, входящая в iOS 5. Мы рассмотрели шаги, необходимые для настройки проекта на использование iCloud, а затем предоставили примеры реализации функций iCloud.

В примере хранилища "ключ — значение" показано, как iCloud можно использовать для хранения небольшого объема данных, аналогичного сохраненному способу Нсусерпреференцес. В примере данных показано, как можно хранить и синхронизировать более сложные данные на нескольких устройствах через iCloud.

И, наконец, она включала краткое обсуждение того, как добавление резервной копии iCloud должно повлиять на структуру приложения.

## <a name="related-links"></a>Связанные ссылки

- [Введение в iCloud (пример)](/samples/xamarin/ios-samples/introductiontoicloud)
- [Пример кода для семинара iCloud](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [Слайды по семинарам iCloud](https://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [Нсубикуитаускэйвалуесторе iCloud](https://developer.apple.com/library/prerelease/ios/)
- [Хранилище iCloud](https://support.apple.com/kb/HT4847)