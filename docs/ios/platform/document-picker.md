---
title: Средство выбора документов в Xamarin. iOS
description: В этом документе описывается средство выбора документов iOS и обсуждается его использование в Xamarin. iOS. Он рассматривает iCloud, документы, общий код настройки, расширения поставщиков документов и многое другое.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: c8cffc9593b6de059a877654994e6747204ce34d
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435397"
---
# <a name="document-picker-in-xamarinios"></a>Средство выбора документов в Xamarin. iOS

Средство выбора документов позволяет совместно использовать документы в разных приложениях. Эти документы могут храниться в iCloud или в каталоге другого приложения. Документы совместно используются через набор [расширений поставщика документов](~/ios/platform/extensions.md) , установленных пользователем на устройстве.

Из-за сложности синхронизации документов в приложениях и в облаке они представляют определенный объем необходимой сложности.

## <a name="requirements"></a>Требования

Для выполнения действий, описанных в этой статье, необходимо выполнить следующие действия.

- **Xcode 7 и iOS 8 или более поздней версии** — на компьютере разработчика должны быть установлены и настроены API Apple Xcode 7 и iOS 8 или более поздней версии.
- **Visual Studio или Visual Studio для Mac** — должна быть установлена последняя версия Visual Studio для Mac.
- **устройство iOS** — устройство iOS под управлением iOS 8 или более поздней версии.

## <a name="changes-to-icloud"></a>Изменения в iCloud

Для реализации новых функций средства выбора документов в службу iCloud Apple были внесены следующие изменения:

- Управляющая программа iCloud полностью переписана с помощью CloudKit.
- Существующие функции iCloud переименованы на диск iCloud.
- В iCloud добавлена поддержка ОС Microsoft Windows.
- В Mac OS Finder добавлена папка iCloud.
- устройства iOS могут получать доступ к содержимому папки Mac OS iCloud.

> [!IMPORTANT]
> Компания Apple [предоставляет инструменты](https://developer.apple.com/support/allowing-users-to-manage-data/), которые помогают разработчикам надлежащим образом соблюдать Общий регламент по защите данных Европейского союза (GDPR).

## <a name="what-is-a-document"></a>Что такое документ?

При ссылке на документ в iCloud это единственная изолированная сущность, которая должна рассматриваться для пользователя. Пользователю может потребоваться изменить документ или поделиться им с другими пользователями (например, с помощью электронной почты).

Существует несколько типов файлов, которые пользователь сразу будет распознавать как документы, например страницы, файлы основного или нумерованного документа. Однако iCloud не ограничивается этой концепцией. Например, состояние игры (например, совпадение шахмата) может рассматриваться как документ и храниться в iCloud. Этот файл может быть передан между устройствами пользователя и позволяет ему выбрать игру, в которой они остановились на другом устройстве.

## <a name="dealing-with-documents"></a>Работа с документами

Прежде чем углубляться в код, необходимый для использования средства выбора документов с помощью Xamarin, в этой статье будут рассмотрены рекомендации по работе с документами iCloud и внесены некоторые изменения в существующие API, необходимые для поддержки средства выбора документов.

### <a name="using-file-coordination"></a>Использование координации файлов

Поскольку файл может быть изменен из нескольких разных расположений, для предотвращения потери данных необходимо использовать координацию.

 [![Использование координации файлов](document-picker-images/image1.png)](document-picker-images/image1.png#lightbox)

Давайте посмотрим на приведенную выше иллюстрацию:

1. Устройство iOS, использующее координацию файлов, создает новый документ и сохраняет его в папку iCloud.
2. iCloud сохраняет измененный файл в облаке для распространения на каждое устройство.
3. Подключенный Mac видит измененный файл в папке iCloud и использует координацию файлов для копирования изменений в файл.
4. Устройство, не использующее координацию файлов, вносит изменения в файл и сохраняет его в папку iCloud. Эти изменения немедленно реплицируются на другие устройства.

Предположим, что исходное устройство iOS или Mac изменило файл, теперь изменения теряются и перезаписываются версией файла с несогласованного устройства. Чтобы предотвратить потери данных, координацию файлов необходимо при работе с документами на основе облака.

### <a name="using-uidocument"></a>Использование данных

 `UIDocument` делает что-то простое (или `NSDocument` на macOSе), выполняя всю тяжелую работу разработчика. Она предоставляет встроенную координацию файлов с фоновыми очередями для предотвращения блокировки пользовательского интерфейса приложения.

 `UIDocument` предоставляет несколько интерфейсов API высокого уровня, которые упрощают разработку приложения Xamarin для любой цели, которую требует разработчик.

Следующий код создает подкласс `UIDocument` для реализации универсального текстового документа, который можно использовать для хранения и извлечения текста из iCloud:

```csharp
using System;
using Foundation;
using UIKit;

namespace DocPicker
{
    public class GenericTextDocument : UIDocument
    {
        #region Private Variable Storage
        private NSString _dataModel;
        #endregion

        #region Computed Properties
        public string Contents {
            get { return _dataModel.ToString (); }
            set { _dataModel = new NSString(value); }
        }
        #endregion

        #region Constructors
        public GenericTextDocument (NSUrl url) : base (url)
        {
            // Set the default document text
            this.Contents = "";
        }

        public GenericTextDocument (NSUrl url, string contents) : base (url)
        {
            // Set the default document text
            this.Contents = contents;
        }
        #endregion

        #region Override Methods
        public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Were any contents passed to the document?
            if (contents != null) {
                _dataModel = NSString.FromData( (NSData)contents, NSStringEncoding.UTF8 );
            }

            // Inform caller that the document has been modified
            RaiseDocumentModified (this);

            // Return success
            return true;
        }

        public override NSObject ContentsForType (string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Convert the contents to a NSData object and return it
            NSData docData = _dataModel.Encode(NSStringEncoding.UTF8);
            return docData;
        }
        #endregion

        #region Events
        public delegate void DocumentModifiedDelegate(GenericTextDocument document);
        public event DocumentModifiedDelegate DocumentModified;

        internal void RaiseDocumentModified(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentModified != null) {
                this.DocumentModified (document);
            }
        }
        #endregion
    }
}
```

Приведенный `GenericTextDocument` выше класс будет использоваться в этой статье при работе с компонентом выбора документов и внешними документами в приложении Xamarin. iOS 8.

## <a name="asynchronous-file-coordination"></a>Асинхронная координация файлов

iOS 8 предоставляет несколько новых функций асинхронной координации файлов через новые API-интерфейсы координации файлов. До iOS 8 все существующие API координации файлов были полностью синхронны. Это означало, что разработчик отвечал за реализацию собственной фоновой очереди, чтобы предотвратить блокировку пользовательского интерфейса приложения в координации файлов.

Новый `NSFileAccessIntent` класс содержит URL-адрес, указывающий на файл, и несколько параметров для управления требуемым типом координации. Следующий код демонстрирует перемещение файла из одного расположения в другое с помощью целей.

```csharp
// Get source options
var srcURL = NSUrl.FromFilename ("FromFile.txt");
var srcIntent = NSFileAccessIntent.CreateReadingIntent (srcURL, NSFileCoordinatorReadingOptions.ForUploading);

// Get destination options
var dstURL = NSUrl.FromFilename ("ToFile.txt");
var dstIntent = NSFileAccessIntent.CreateReadingIntent (dstURL, NSFileCoordinatorReadingOptions.ForUploading);

// Create an array
var intents = new NSFileAccessIntent[] {
    srcIntent,
    dstIntent
};

// Initialize a file coordination with intents
var queue = new NSOperationQueue ();
var fileCoordinator = new NSFileCoordinator ();
fileCoordinator.CoordinateAccess (intents, queue, (err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}",err.LocalizedDescription);
    }
});
```

## <a name="discovering-and-listing-documents"></a>Обнаружение и перечисление документов

Для обнаружения и перечисления документов используются существующие `NSMetadataQuery` API-интерфейсы. В этом разделе рассматриваются новые функции, добавленные в `NSMetadataQuery` , которые делают работу с документами еще проще, чем раньше.

### <a name="existing-behavior"></a>Существующее поведение

До iOS 8 `NSMetadataQuery` было настроено время от времени для отправки локальных изменений файлов, таких как удаления, создание и переименование.

 [![Обзор изменений в локальном файле Нсметадатакуери](document-picker-images/image2.png)](document-picker-images/image2.png#lightbox)

Расшифровка схемы выше.

1. Для файлов, которые уже существуют в контейнере приложения, существуют  `NSMetadataQuery`  `NSMetadata` предварительно созданные и помещенные в очередь записи, чтобы они были мгновенно доступны для приложения.
1. Приложение создает новый файл в контейнере приложения.
1. Перед тем как  `NSMetadataQuery` увидеть изменения в контейнере приложения и создать необходимую запись, возникает задержка  `NSMetadata` .

Из-за задержки в создании `NSMetadata` записи приложению пришлось открыть два источника данных: один для локальных изменений файла и один для облачных изменений.

### <a name="stitching"></a>Совмещение

В iOS 8 `NSMetadataQuery` проще использовать непосредственно с новой функцией, называемой сшивания:

 [![Нсметадатакуери с новой функцией, именуемой сшивка](document-picker-images/image3.png)](document-picker-images/image3.png#lightbox)

Использование сшивания на приведенной выше схеме:

1. Как и ранее, для файлов, которые уже существуют в контейнере приложения,  `NSMetadataQuery` существуют  `NSMetadata` предварительно созданные и помещенные в очередь записи.
1. Приложение создает новый файл в контейнере приложения, используя координацию файлов.
1. Обработчик в контейнере приложения видит изменения и вызовы  `NSMetadataQuery` для создания требуемой  `NSMetadata` записи.
1. `NSMetadata`Запись создается непосредственно после файла и становится доступной для приложения.

С помощью сшивания приложения больше не нужно открывать источник данных для отслеживания изменений локальных и облачных файлов. Теперь приложение может использовать `NSMetadataQuery` напрямую.

> [!IMPORTANT]
> Сшивка работает, только если приложение использует координацию файлов, как показано в разделе выше. Если координация файлов не используется, API-интерфейсы по умолчанию используют существующее поведение до iOS 8.

### <a name="new-ios-8-metadata-features"></a>Новые функции метаданных iOS 8

В iOS 8 добавлены следующие новые функции `NSMetadataQuery` :

- `NSMetatadataQuery` теперь может перечислить нелокальные документы, хранящиеся в облаке.
- Добавлены новые API для доступа к сведениям о метаданных в облачных документах.
- Существует новый  `NSUrl_PromisedItems` интерфейс API, который будет получать доступ к атрибутам файлов, которые могут иметь или не иметь доступ к содержимому локально.
- Используйте  `GetPromisedItemResourceValue` метод для получения сведений о конкретном файле или используйте  `GetPromisedItemResourceValues` метод для получения сведений о нескольких файлах за раз.

Для работы с метаданными были добавлены два новых флага координации файлов:

- `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly`
- `NSFileCoordinatorWriteContentIndependentMetadataOnly`

При использовании указанных выше флагов содержимое файла документа не обязательно должно быть доступно локально для использования.

В следующем фрагменте кода показано, как использовать `NSMetadataQuery` для запроса существования определенного файла и сборки файла, если он не существует:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

#region Static Properties
public const string TestFilename = "test.txt";
#endregion

#region Computed Properties
public bool HasiCloud { get; set; }
public bool CheckingForiCloud { get; set; }
public NSUrl iCloudUrl { get; set; }

public GenericTextDocument Document { get; set; }
public NSMetadataQuery Query { get; set; }
#endregion

#region Private Methods
private void FindDocument () {
    Console.WriteLine ("Finding Document...");

    // Create a new query and set it's scope
    Query = new NSMetadataQuery();
    Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

    // Build a predicate to locate the file by name and attach it to the query
    var pred = NSPredicate.FromFormat ("%K == %@"
        , new NSObject[] {
            NSMetadataQuery.ItemFSNameKey
            , new NSString(TestFilename)});
    Query.Predicate = pred;

    // Register a notification for when the query returns
    NSNotificationCenter.DefaultCenter.AddObserver (this,
            new Selector("queryDidFinishGathering:"),             NSMetadataQuery.DidFinishGatheringNotification,
            Query);

    // Start looking for the file
    Query.StartQuery ();
    Console.WriteLine ("Querying: {0}", Query.IsGathering);
}

[Export("queryDidFinishGathering:")]
public void DidFinishGathering (NSNotification notification) {
    Console.WriteLine ("Finish Gathering Documents.");

    // Access the query and stop it from running
    var query = (NSMetadataQuery)notification.Object;
    query.DisableUpdates();
    query.StopQuery();

    // Release the notification
    NSNotificationCenter.DefaultCenter.RemoveObserver (this
        , NSMetadataQuery.DidFinishGatheringNotification
        , query);

    // Load the document that the query returned
    LoadDocument(query);
}

private void LoadDocument (NSMetadataQuery query) {
    Console.WriteLine ("Loading Document...");    

    // Take action based on the returned record count
    switch (query.ResultCount) {
    case 0:
        // Create a new document
        CreateNewDocument ();
        break;
    case 1:
        // Gain access to the url and create a new document from
        // that instance
        NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

        // Load the document
        OpenDocument (url);
        break;
    default:
        // There has been an issue
        Console.WriteLine ("Issue: More than one document found...");
        break;
    }
}
#endregion

#region Public Methods
public void OpenDocument(NSUrl url) {

    Console.WriteLine ("Attempting to open: {0}", url);
    Document = new GenericTextDocument (url);

    // Open the document
    Document.Open ( (success) => {
        if (success) {
            Console.WriteLine ("Document Opened");
        } else
            Console.WriteLine ("Failed to Open Document");
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public void CreateNewDocument() {
    // Create path to new file
    // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
    var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
    var docPath = Path.Combine (docsFolder, TestFilename);
    var ubiq = new NSUrl (docPath, false);

    // Create new document at path
    Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
    Document = new GenericTextDocument (ubiq);

    // Set the default value
    Document.Contents = "(default value)";

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
        Console.WriteLine ("Save completion:" + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
        } else {
            Console.WriteLine ("Unable to Save Document");
        }
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public bool SaveDocument() {
    bool successful = false;

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
        Console.WriteLine ("Save completion: " + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
            successful = true;
        } else {
            Console.WriteLine ("Unable to Save Document");
            successful=false;
        }
    });

    // Return results
    return successful;
}
#endregion

#region Events
public delegate void DocumentLoadedDelegate(GenericTextDocument document);
public event DocumentLoadedDelegate DocumentLoaded;

internal void RaiseDocumentLoaded(GenericTextDocument document) {
    // Inform caller
    if (this.DocumentLoaded != null) {
        this.DocumentLoaded (document);
    }
}
#endregion
```

### <a name="document-thumbnails"></a>Эскизы документов

Компания Apple полагает, что при составлении списка документов для приложения лучше использовать предварительный просмотр. Это дает контекст конечных пользователей, поэтому он может быстро найти документ, с которым они хотят работать.

До iOS 8 с отображением предварительных версий документов требуется пользовательская реализация. Новые возможности iOS 8 — это атрибуты файловой системы, позволяющие разработчикам быстро работать с эскизами документов.

#### <a name="retrieving-document-thumbnails"></a>Получение эскизов документа

При вызове `GetPromisedItemResourceValue` `GetPromisedItemResourceValues` методов или `NSUrl_PromisedItems` возвращается API, `NSUrlThumbnailDictionary` . Единственным ключом в этом словаре является `NSThumbnial1024X1024SizeKey` и его сопоставление `UIImage` .

#### <a name="saving-document-thumbnails"></a>Сохранение эскизов документов

Самый простой способ сохранить эскиз — использовать `UIDocument` . Вызвав `GetFileAttributesToWrite` метод `UIDocument` и установив эскиз, он будет автоматически сохранен, если файл документа имеет значение. Управляющая программа iCloud увидит это изменение и распространит ее на iCloud. На Mac OS X эскизы автоматически формируются для разработчика с помощью подключаемого модуля быстрого поиска.

Используя основы работы с документами на основе iCloud, а также изменения существующего API, мы готовы реализовать контроллер представления средства выбора документов в мобильном приложении Xamarin iOS 8.

## <a name="enabling-icloud-in-xamarin"></a>Включение iCloud в Xamarin

Прежде чем средство выбора документов можно будет использовать в приложении Xamarin. iOS, необходимо включить поддержку iCloud в приложении и через Apple.

Ниже приведены пошаговые инструкции по подготовке к работе для iCloud.

1. Создайте контейнер iCloud.
2. Создайте идентификатор приложения, который содержит службу приложений iCloud.
3. Создайте профиль подготовки, включающий этот идентификатор приложения.

В руководстве по [работе с возможностями](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) рассматриваются первые два шага. Чтобы создать профиль подготовки, выполните действия, описанные в руководстве по [профилю подготовки](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) .

В следующих шагах пошаговых руководств описывается процесс настройки приложения для iCloud.

Выполните следующие действия.

1. Откройте проект в Visual Studio для Mac или Visual Studio.
2. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите пункт Параметры.
3. В диалоговом окне Параметры выберите **приложение iOS**, убедитесь, что **идентификатор пакета** соответствует тому, который был определен в **идентификаторе** приложения, созданном ранее для приложения.
4. Выберите **Подписывание пакета iOS**, выберите **удостоверение разработчика** и **профиль подготовки** , созданный ранее.
5. Нажмите кнопку **ОК** , чтобы сохранить изменения и закрыть диалоговое окно.
6. Щелкните правой кнопкой мыши `Entitlements.plist` **Обозреватель решений** , чтобы открыть его в редакторе.

    > [!IMPORTANT]
    > В Visual Studio может потребоваться открыть редактор прав, щелкнув его правой кнопкой мыши и выбрав пункт **Открыть с помощью...** и выбор редактора списка свойств

7. Установите флажок  **включить iCloud** ,  **документы iCloud** ,  **хранилище ключей** и  **CloudKit** .
8. Убедитесь, что **контейнер** существует для приложения (как было создано выше). Например, `iCloud.com.your-company.AppName`.
9. Сохраните изменения в файле.

Дополнительные сведения о правах см. в руководстве по [работе с](~/ios/deploy-test/provisioning/entitlements.md) назначениями.

Выполнив приведенную выше настройку, приложение теперь может использовать облачные документы и новый контроллер представления средства выбора документов.

## <a name="common-setup-code"></a>Распространенный код настройки

Прежде чем приступить к работе с контроллером представления средства выбора документов, необходимо выполнить стандартный код установки. Начните с изменения `AppDelegate.cs` файла приложения и сделайте его следующим:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

namespace DocPicker
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Static Properties
        public const string TestFilename = "test.txt";
        #endregion

        #region Computed Properties
        public override UIWindow Window { get; set; }
        public bool HasiCloud { get; set; }
        public bool CheckingForiCloud { get; set; }
        public NSUrl iCloudUrl { get; set; }

        public GenericTextDocument Document { get; set; }
        public NSMetadataQuery Query { get; set; }
        public NSData Bookmark { get; set; }
        #endregion

        #region Private Methods
        private void FindDocument () {
            Console.WriteLine ("Finding Document...");

            // Create a new query and set it's scope
            Query = new NSMetadataQuery();
            Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

            // Build a predicate to locate the file by name and attach it to the query
            var pred = NSPredicate.FromFormat ("%K == %@",
                 new NSObject[] {NSMetadataQuery.ItemFSNameKey
                , new NSString(TestFilename)});
            Query.Predicate = pred;

            // Register a notification for when the query returns
            NSNotificationCenter.DefaultCenter.AddObserver (this
                , new Selector("queryDidFinishGathering:")
                , NSMetadataQuery.DidFinishGatheringNotification
                , Query);

            // Start looking for the file
            Query.StartQuery ();
            Console.WriteLine ("Querying: {0}", Query.IsGathering);
        }

        [Export("queryDidFinishGathering:")]
        public void DidFinishGathering (NSNotification notification) {
            Console.WriteLine ("Finish Gathering Documents.");

            // Access the query and stop it from running
            var query = (NSMetadataQuery)notification.Object;
            query.DisableUpdates();
            query.StopQuery();

            // Release the notification
            NSNotificationCenter.DefaultCenter.RemoveObserver (this
                , NSMetadataQuery.DidFinishGatheringNotification
                , query);

            // Load the document that the query returned
            LoadDocument(query);
        }

        private void LoadDocument (NSMetadataQuery query) {
            Console.WriteLine ("Loading Document...");    

            // Take action based on the returned record count
            switch (query.ResultCount) {
            case 0:
                // Create a new document
                CreateNewDocument ();
                break;
            case 1:
                // Gain access to the url and create a new document from
                // that instance
                NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
                var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

                // Load the document
                OpenDocument (url);
                break;
            default:
                // There has been an issue
                Console.WriteLine ("Issue: More than one document found...");
                break;
            }
        }
        #endregion

        #region Public Methods

        public void OpenDocument(NSUrl url) {

            Console.WriteLine ("Attempting to open: {0}", url);
            Document = new GenericTextDocument (url);

            // Open the document
            Document.Open ( (success) => {
                if (success) {
                    Console.WriteLine ("Document Opened");
                } else
                    Console.WriteLine ("Failed to Open Document");
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        public void CreateNewDocument() {
            // Create path to new file
            // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
            var docPath = Path.Combine (docsFolder, TestFilename);
            var ubiq = new NSUrl (docPath, false);

            // Create new document at path
            Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
            Document = new GenericTextDocument (ubiq);

            // Set the default value
            Document.Contents = "(default value)";

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
                Console.WriteLine ("Save completion:" + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                } else {
                    Console.WriteLine ("Unable to Save Document");
                }
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        /// <summary>
        /// Saves the document.
        /// </summary>
        /// <returns><c>true</c>, if document was saved, <c>false</c> otherwise.</returns>
        public bool SaveDocument() {
            bool successful = false;

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
                Console.WriteLine ("Save completion: " + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                    successful = true;
                } else {
                    Console.WriteLine ("Unable to Save Document");
                    successful=false;
                }
            });

            // Return results
            return successful;
        }
        #endregion

        #region Override Methods
        public override void FinishedLaunching (UIApplication application)
        {

            // Start a new thread to check and see if the user has iCloud
            // enabled.
            new Thread(new ThreadStart(() => {
                // Inform caller that we are checking for iCloud
                CheckingForiCloud = true;

                // Checks to see if the user of this device has iCloud
                // enabled
                var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer(null);

                // Connected to iCloud?
                if (uburl == null)
                {
                    // No, inform caller
                    HasiCloud = false;
                    iCloudUrl =null;
                    Console.WriteLine("Unable to connect to iCloud");
                    InvokeOnMainThread(()=>{
                        var okAlertController = UIAlertController.Create ("iCloud Not Available", "Developer, please check your Entitlements.plist, Bundle ID and Provisioning Profiles.", UIAlertControllerStyle.Alert);
                        okAlertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        Window.RootViewController.PresentViewController (okAlertController, true, null);
                    });
                }
                else
                {    
                    // Yes, inform caller and save location the Application Container
                    HasiCloud = true;
                    iCloudUrl = uburl;
                    Console.WriteLine("Connected to iCloud");

                    // If we have made the connection with iCloud, start looking for documents
                    InvokeOnMainThread(()=>{
                        // Search for the default document
                        FindDocument ();
                    });
                }

                // Inform caller that we are no longer looking for iCloud
                CheckingForiCloud = false;

            })).Start();

        }

        // This method is invoked when the application is about to move from active to inactive state.
        // OpenGL applications should use this method to pause.
        public override void OnResignActivation (UIApplication application)
        {
        }

        // This method should be used to release shared resources and it should store the application state.
        // If your application supports background execution this method is called instead of WillTerminate
        // when the user quits.
        public override void DidEnterBackground (UIApplication application)
        {
            // Trap all errors
            try {
                // Values to include in the bookmark packet
                var resources = new string[] {
                    NSUrl.FileSecurityKey,
                    NSUrl.ContentModificationDateKey,
                    NSUrl.FileResourceIdentifierKey,
                    NSUrl.FileResourceTypeKey,
                    NSUrl.LocalizedNameKey
                };

                // Create the bookmark
                NSError err;
                Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

                // Was there an error?
                if (err != null) {
                    // Yes, report it
                    Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
                }
            }
            catch (Exception e) {
                // Report error
                Console.WriteLine ("Error: {0}", e.Message);
            }
        }

        // This method is called as part of the transition from background to active state.
        public override void WillEnterForeground (UIApplication application)
        {
            // Is there any bookmark data?
            if (Bookmark != null) {
                // Trap all errors
                try {
                    // Yes, attempt to restore it
                    bool isBookmarkStale;
                    NSError err;
                    var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

                    // Was there an error?
                    if (err != null) {
                        // Yes, report it
                        Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
                    } else {
                        // Load document from bookmark
                        OpenDocument (srcUrl);
                    }
                }
                catch (Exception e) {
                    // Report error
                    Console.WriteLine ("Error: {0}", e.Message);
                }
            }

        }

        // This method is called when the application is about to terminate. Save data, if needed.
        public override void WillTerminate (UIApplication application)
        {
        }
        #endregion

        #region Events
        public delegate void DocumentLoadedDelegate(GenericTextDocument document);
        public event DocumentLoadedDelegate DocumentLoaded;

        internal void RaiseDocumentLoaded(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentLoaded != null) {
                this.DocumentLoaded (document);
            }
        }
        #endregion
    }
}

```

> [!IMPORTANT]
> Приведенный выше код включает код из раздела обнаружение и перечисление документов выше. Он представлен здесь целиком, как показано в фактическом приложении. Для простоты этот пример работает только с одним жестко запрограммированным файлом ( `test.txt` ).

Приведенный выше код предоставляет несколько ярлыков дисков iCloud, чтобы упростить работу с ними в оставшейся части приложения.

Затем добавьте следующий код в любое представление или контейнер представления, в котором будет использоваться средство выбора документов или работа с облачными документами:

```csharp
using CloudKit;
...

#region Computed Properties
/// <summary>
/// Returns the delegate of the current running application
/// </summary>
/// <value>The this app.</value>
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

При этом добавляется ярлык для получения `AppDelegate` доступа к ярлыкам iCloud, созданным ранее.

Используя этот код, давайте рассмотрим реализацию контроллера представления средства выбора документов в приложении Xamarin iOS 8.

## <a name="using-the-document-picker-view-controller"></a>Использование контроллера представления средства выбора документов

До iOS 8 было очень сложно получить доступ к документам из другого приложения, поскольку не существовало способа обнаружить документы вне приложения в приложении.

### <a name="existing-behavior"></a>Существующее поведение

 [![Общие сведения о существующем поведении](document-picker-images/image31.png)](document-picker-images/image31.png#lightbox)

Давайте рассмотрим доступ к внешнему документу до iOS 8:

1. Сначала пользователю нужно будет открыть приложение, изначально создавшее документ.
1. Документ выбран и  `UIDocumentInteractionController` используется для отправки документа в новое приложение.
1. Наконец, копия исходного документа помещается в контейнер нового приложения.

Из этого документа можно открыть и изменить второе приложение.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Обнаружение документов за пределами контейнера приложения

В iOS 8 приложение может получить доступ к документам за пределами собственного контейнера приложений.

 [![Обнаружение документов за пределами контейнера приложения](document-picker-images/image32.png)](document-picker-images/image32.png#lightbox)

С помощью нового средства выбора документов iCloud ( `UIDocumentPickerViewController` ) приложение iOS может напрямую обнаруживать и получать доступ за пределами контейнера приложения. `UIDocumentPickerViewController`Предоставляет пользователю механизм для предоставления доступа к обнаруженным документам и их редактирования с помощью разрешений.

Приложение должно быть включено в средство выбора документов iCloud и быть доступно другим приложениям для обнаружения и работы с ними. Чтобы приложение Xamarin iOS 8 имело общий доступ к контейнеру приложения, измените `Info.plist` файл в стандартном текстовом редакторе и добавьте следующие две строки в нижнюю часть словаря (между `<dict>...</dict>` тегами):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController`Предоставляет отличный новый пользовательский интерфейс, позволяющий пользователю выбирать документы. Чтобы отобразить контроллер представления средства выбора документов в приложении Xamarin iOS 8, выполните следующие действия.

```csharp
using MobileCoreServices;
...

// Allow the Document picker to select a range of document types
        var allowedUTIs = new string[] {
            UTType.UTF8PlainText,
            UTType.PlainText,
            UTType.RTF,
            UTType.PNG,
            UTType.Text,
            UTType.PDF,
            UTType.Image
        };

        // Display the picker
        //var picker = new UIDocumentPickerViewController (allowedUTIs, UIDocumentPickerMode.Open);
        var pickerMenu = new UIDocumentMenuViewController(allowedUTIs, UIDocumentPickerMode.Open);
        pickerMenu.DidPickDocumentPicker += (sender, args) => {

            // Wireup Document Picker
            args.DocumentPicker.DidPickDocument += (sndr, pArgs) => {

                // IMPORTANT! You must lock the security scope before you can
                // access this file
                var securityEnabled = pArgs.Url.StartAccessingSecurityScopedResource();

                // Open the document
                ThisApp.OpenDocument(pArgs.Url);

                // IMPORTANT! You must release the security lock established
                // above.
                pArgs.Url.StopAccessingSecurityScopedResource();
            };

            // Display the document picker
            PresentViewController(args.DocumentPicker,true,null);
        };

pickerMenu.ModalPresentationStyle = UIModalPresentationStyle.Popover;
PresentViewController(pickerMenu,true,null);
UIPopoverPresentationController presentationPopover = pickerMenu.PopoverPresentationController;
if (presentationPopover!=null) {
    presentationPopover.SourceView = this.View;
    presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Down;
    presentationPopover.SourceRect = ((UIButton)s).Frame;
}
```

> [!IMPORTANT]
> Разработчик должен вызвать `StartAccessingSecurityScopedResource` метод класса, `NSUrl` прежде чем можно будет получить доступ к внешнему документу. `StopAccessingSecurityScopedResource`Метод должен быть вызван для освобождения блокировки безопасности сразу после загрузки документа.

### <a name="sample-output"></a>Пример выходных данных

Ниже приведен пример того, как приведенный выше код будет отображать средство выбора документов при запуске на устройстве iPhone.

1. Пользователь запускает приложение, и отображается основной интерфейс:   

    [![Отобразится основной интерфейс.](document-picker-images/image33.png)](document-picker-images/image33.png#lightbox)
1. Пользователь нажмет кнопку **действия** в верхней части экрана и получит запрос на выбор **поставщика документов** из списка доступных поставщиков:   

    [![Выберите поставщика документов из списка доступных поставщиков](document-picker-images/image34.png)](document-picker-images/image34.png#lightbox)
1. Отобразится **контроллер представления средства выбора документов** для выбранного **поставщика документов**:   

    [![Отобразится контроллер представления средства выбора документов.](document-picker-images/image35.png)](document-picker-images/image35.png#lightbox)
1. Пользователь нажмет **папку документа** , чтобы отобразить ее содержимое:   

    [![Содержимое папки документов](document-picker-images/image36.png)](document-picker-images/image36.png#lightbox)
1. Пользователь выбирает **документ** , и **средство выбора документов** закрывается.
1. Основной интерфейс отображается повторно, **документ** загружается из внешнего контейнера и отображается его содержимое.

Реальное отображение контроллера представления средства выбора документов зависит от того, какие поставщики документов установил пользователь на устройстве и какой режим выбора документов был реализован. В приведенном выше примере используется режим открытия, другие типы режимов будут подробно рассмотрены ниже.

## <a name="managing-external-documents"></a>Управление внешними документами

Как упоминалось выше, приложение могло бы получить доступ только к тем документам, которые были частью своего контейнера приложения. В iOS 8 приложение может обращаться к документам из внешних источников:

 [![Общие сведения об управлении внешними документами](document-picker-images/image37.png)](document-picker-images/image37.png#lightbox)

Когда пользователь выбирает документ из внешнего источника, ссылочный документ записывается в контейнер приложения, указывающий на исходный документ.

Чтобы упростить добавление этой новой возможности в существующие приложения, в API были добавлены несколько новых функций `NSMetadataQuery` . Как правило, приложение использует область повсеместного документа для вывода списка документов, которые находятся в контейнере приложения. При использовании этой области будут отображаться только документы в контейнере приложения.

При использовании новой области повсеместного внешнего документа будут возвращены документы, которые находятся за пределами контейнера приложения и возвращают метаданные для них. Объект `NSMetadataItemUrlKey` будет указывать на URL-адрес, где фактически расположен документ.

Иногда приложению не требуется работать с документами, на которые указывает ссылка. Вместо этого приложение хочет работать со ссылочным документом напрямую. Например, приложение может захотеть отобразить документ в папке приложения в пользовательском интерфейсе или разрешить пользователю перемещать ссылки внутри папки.

В iOS 8 `NSMetadataItemUrlInLocalContainerKey` для прямого доступа к справочному документу предоставлен новый объект. Этот ключ указывает на фактическую ссылку на внешний документ в контейнере приложения.

`NSMetadataUbiquitousItemIsExternalDocumentKey`Используется для проверки того, является ли документ внешним по отношению к контейнеру приложения. Объект `NSMetadataUbiquitousItemContainerDisplayNameKey` используется для доступа к имени контейнера, который является частью исходной копии внешнего документа.

### <a name="why-document-references-are-required"></a>Зачем нужны ссылки на документы

Основная причина, по которой в iOS 8 используются ссылки для доступа к внешним документам, — безопасность. Ни одному приложению не предоставляется доступ к контейнеру другого приложения. Это можно сделать только с помощью средства выбора документов, так как оно выполняется вне процесса и имеет доступ к всей системе.

Единственный способ получить доступ к документу вне контейнера приложения — с помощью средства выбора документов, и если URL-адрес, возвращаемый средством выбора, является областью безопасности. URL-адрес с областью действия безопасности содержит достаточно сведений, чтобы выбрать документ, а также права с областью действия, необходимые для предоставления приложению доступа к документу.

Важно отметить, что если URL-адрес с областью действия безопасности был сериализован в строку и затем десериализован, сведения о безопасности будут потеряны, и файл будет недоступен по URL-адресу. Функция ссылки на документ предоставляет механизм для возврата к файлам, на которые указывают эти URL-адреса.

Поэтому, если приложение получает `NSUrl` из одного из справочных документов, к нему уже присоединена область безопасности, и ее можно использовать для доступа к файлу. По этой причине мы настоятельно рекомендуем использовать разработчика, `UIDocument` поскольку он обрабатывает всю эту информацию и процессы для них.

### <a name="using-bookmarks"></a>Использование закладок

Не всегда можно перечислять документы приложения, чтобы вернуться к конкретному документу, например при восстановлении состояния. iOS 8 предоставляет механизм создания закладок, предназначенных непосредственно для конкретного документа.

Следующий код создаст закладку из `UIDocument` `FileUrl` Свойства:

```csharp
// Trap all errors
try {
    // Values to include in the bookmark packet
    var resources = new string[] {
        NSUrl.FileSecurityKey,
        NSUrl.ContentModificationDateKey,
        NSUrl.FileResourceIdentifierKey,
        NSUrl.FileResourceTypeKey,
        NSUrl.LocalizedNameKey
    };

    // Create the bookmark
    NSError err;
    Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

    // Was there an error?
    if (err != null) {
        // Yes, report it
        Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
    }
}
catch (Exception e) {
    // Report error
    Console.WriteLine ("Error: {0}", e.Message);
}
```

Существующий API закладки используется для создания закладки для существующего объекта `NSUrl` , который можно сохранить и загрузить для предоставления прямого доступа к внешнему файлу. Следующий код восстановит закладку, созданную ранее:

```csharp
if (Bookmark != null) {
    // Trap all errors
    try {
        // Yes, attempt to restore it
        bool isBookmarkStale;
        NSError err;
        var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

        // Was there an error?
        if (err != null) {
            // Yes, report it
            Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
        } else {
            // Load document from bookmark
            OpenDocument (srcUrl);
        }
    }
    catch (Exception e) {
        // Report error
        Console.WriteLine ("Error: {0}", e.Message);
    }
}
```

## <a name="open-vs-import-mode-and-the-document-picker"></a>Открытие и выбор режима импорта и выбора документов

Контроллер представления средства выбора документов включает два разных режима работы:

1. **Режим открытия** — в этом режиме при выборе пользователем и внешним документом средство выбора документов создаст закладку с областью безопасности в контейнере приложения.   

    [![Закладка с областью действия безопасности в контейнере приложения](document-picker-images/image37.png)](document-picker-images/image37.png#lightbox)
1. **Режим импорта** — в этом режиме, когда пользователь выбирает и внешний документ, средство выбора документов не создает закладку, а копирует файл во временное расположение и предоставляет приложению доступ к документу в этом расположении:   

    [![Средство выбора документов скопирует файл во временную папку и предоставит приложению доступ к документу в этом расположении.](document-picker-images/image38.png)](document-picker-images/image38.png#lightbox)   
 После завершения работы приложения по какой-либо причине временное расположение очищается и файл удаляется. Если приложению необходимо сохранить доступ к файлу, необходимо создать копию и поместить ее в контейнер приложения.

Режим открытия удобен, когда приложению требуется совместная работа с другим приложением и совместное использование изменений, внесенных в документ с помощью этого приложения. Режим импорта используется, когда приложению не требуется совместно использовать свои изменения в документе с другими приложениями.

## <a name="making-a-document-external"></a>Создание внешнего документа

Как отмечалось выше, приложение iOS 8 не имеет доступа к контейнерам за пределами своего контейнера приложений. Приложение может выполнять запись в собственный контейнер локально или во временное расположение, а затем использовать специальный режим документа для перемещения результирующего документа за пределы контейнера приложения в выбранное пользователем расположение.

Чтобы переместить документ во внешнее расположение, выполните следующие действия.

1. Сначала создайте новый документ в локальном или временном расположении.
1. Создайте объект  `NSUrl` , указывающий на новый документ.
1. Откройте новый контроллер представления средства выбора документов и передайте его в  `NSUrl` режим `MoveToService` .
1. После того, как пользователь выберет новое расположение, документ будет перемещен из текущего расположения в новое расположение.
1. Справочный документ будет записан в контейнер приложения приложения, чтобы файл по-прежнему мог быть доступен для создания приложения.

Следующий код можно использовать для перемещения документа во внешнее расположение: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

Справочный документ, возвращаемый описанным выше процессом, точно такой же, как и один, созданный в режиме открытия документа. Однако бывают ситуации, когда приложению может потребоваться переместить документ, не сохраняя ссылки на него.

Чтобы переместить документ без создания ссылки, используйте `ExportToService` режим. Например, `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`.

При использовании `ExportToService` режима документ копируется во внешний контейнер, а существующая копия остается в исходном расположении.

## <a name="document-provider-extensions"></a>Расширения поставщика документов

В iOS 8 Apple хочет, чтобы конечный пользователь мог получить доступ к любым своим облачным документам независимо от того, где они существуют. Для достижения этой цели в iOS 8 предусмотрен новый механизм расширения поставщика документов.

### <a name="what-is-a-document-provider-extension"></a>Что такое расширение поставщика документов?

Проще говоря, расширение поставщика документов — это способ, которым разработчик или сторонние разработчики могут предоставить приложению альтернативное хранилище документов, доступ к которому можно получить точно так же, как и имеющееся хранилище iCloud.

Пользователь может выбрать одно из этих альтернативных мест хранения в средстве выбора документов и использовать одни и те же режимы доступа (открытие, импорт, перемещение или экспорт) для работы с файлами в этом расположении.

Это реализуется с помощью двух разных расширений:

- **Расширение средства выбора документов** — предоставляет  `UIViewController` подкласс, предоставляющий графический интерфейс для выбора документа из альтернативного места хранения. Этот подкласс будет отображаться как часть контроллера представления средства выбора документов.
- **Файл — расширение** — это расширение, не являющееся расширением пользовательского интерфейса, которое работает с фактическим указанием содержимого файлов. Эти расширения предоставляются через координацию файлов ( `NSFileCoordinator` ). Это еще один важный случай, когда требуется координация файлов.

На следующей диаграмме показан типичный поток данных при работе с расширениями поставщика документов:

 [![На этой схеме показан типичный поток данных при работе с расширениями поставщика документов](document-picker-images/image39.png)](document-picker-images/image39.png#lightbox)

Выполняется следующая процедура.

1. Приложение представляет собой контроллер средства выбора документов, позволяющий пользователю выбрать файл для работы.
1. Пользователь выбирает альтернативное расположение файла, а пользовательское  `UIViewController` расширение вызывается для вывода пользовательского интерфейса.
1. Пользователь выбирает файл из этого расположения, и URL-адрес передается обратно средству выбора документов.
1. Средство выбора документов выбирает URL-адрес файла и возвращает его в приложение для работы пользователя.
1. URL-адрес передается координатору файлов для возврата содержимого файлов приложению.
1. Координатор файлов вызывает расширение поставщика пользовательских файлов для получения файла.
1. Содержимое файла возвращается координатору файлов.
1. Содержимое файла возвращается приложению.

### <a name="security-and-bookmarks"></a>Безопасность и закладки

В этом разделе будет кратко рассмотрено, как безопасность и постоянный доступ к файлам через закладки работают с расширениями поставщика документов. В отличие от поставщика документов iCloud, который автоматически сохраняет безопасность и закладки в контейнере приложения, расширения поставщика документов не являются частью системы ссылок на документы.

Например, в корпоративном параметре, который предоставляет собственное безопасное хранилище данных Организации, администраторы не хотят получать конфиденциальную корпоративную информацию, доступную или обработанную общедоступными серверами iCloud. Поэтому встроенная система ссылок на документы не может использоваться.

Систему закладок по-прежнему можно использовать, и она отвечает расширению поставщика файлов, чтобы правильно обработать URL-адрес с закладкой и вернуть содержимое документа, на который указывает.

В целях безопасности в iOS 8 имеется уровень изоляции, сохраняющий сведения о том, какой из приложений имеет доступ к какому идентификатору в поставщике файлов. Следует отметить, что Управление доступом к файлам осуществляется этим уровнем изоляции.

На следующей диаграмме показан поток данных при работе с закладками и расширением поставщика документов.

 [![На этой диаграмме показан поток данных при работе с закладками и расширением поставщика документов](document-picker-images/image40.png)](document-picker-images/image40.png#lightbox)

Выполняется следующая процедура.

1. Приложение собирается перейти на задний план и сохранить его состояние. Он вызывает метод `NSUrl` , чтобы создать закладку для файла в альтернативном хранилище.
1. `NSUrl` вызывает расширение поставщика файлов для получения постоянного URL-адреса документа.
1. Расширение поставщика файлов возвращает URL-адрес в виде строки в `NSUrl` .
1. Объект `NSUrl` объединяет URL-адрес в закладку и возвращает его в приложение.
1. Когда приложение авакесся в фоновом режиме и нуждается в восстановлении состояния, оно передает закладку в `NSUrl` .
1. `NSUrl` вызывает расширение поставщика файлов с URL-адресом файла.
1. Поставщик расширений файлов обращается к файлу и возвращает расположение файла в `NSUrl` .
1. Расположение файла объединяется со сведениями о безопасности и возвращается в приложение.

Отсюда приложение может получить доступ к файлу и работать с ним в нормальном режиме.

### <a name="writing-files"></a>Запись файлов

В этом разделе мы кратко рассмотрим, как выполняется запись файлов в альтернативное расположение с расширением поставщика документов. Приложение iOS будет использовать координацию файлов для сохранения информации на диск в контейнере приложения. Вскоре после успешного написания файла расширение поставщика файлов будет уведомлено об изменении.

На этом этапе расширение поставщика файлов может начать отправку файла в альтернативное расположение (или пометить файл как "грязный" и отправить запрос).

### <a name="creating-new-document-provider-extensions"></a>Создание расширений для нового поставщика документов

Создание расширений поставщика документов выходит за рамки этой вводной статьи. Эта информация представлена здесь, чтобы продемонстрировать, что в зависимости от расширений, загруженных пользователем на устройстве iOS, приложение может иметь доступ к расположениям хранилища документов за пределами указанного в заводах Apple местоположения iCloud.

Разработчик должен учитывать этот факт при использовании средства выбора документов и работе с внешними документами. Они не должны рассчитывать, что документ размещен в iCloud.

Дополнительные сведения о создании поставщика хранилища или расширения средства выбора документов см. в документе [Введение в расширения приложения](~/ios/platform/extensions.md) .

## <a name="migrating-to-icloud-drive"></a>Миграция на диск iCloud

В iOS 8 пользователи могут продолжить использовать существующую систему документов iCloud, используемую в iOS 7 (и в предыдущих системах), или перенести существующие документы в новый механизм накопителей iCloud.

В Mac OS X Yosemite Apple не обеспечивает обратную совместимость, поэтому все документы должны быть перенесены на диск iCloud, иначе они больше не будут обновляться на разных устройствах.

После того как учетная запись пользователя будет перенесена на диск iCloud, только устройства, использующие iCloud, смогут распространить изменения на документы на этих устройствах.

> [!IMPORTANT]
> Разработчики должны знать, что новые функции, описанные в этой статье, доступны только в том случае, если учетная запись пользователя была перенесена на диск iCloud.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены изменения существующих API-интерфейсов iCloud, необходимых для поддержки диска iCloud, и нового контроллера представления средства выбора документов. Она посмотрела координацию файлов и почему она важна при работе с документами на основе облака. В нем рассматривается настройка, необходимая для включения облачных документов в приложении Xamarin. iOS, и предоставлен вводный взгляд на работу с документами вне контейнера приложений приложения с помощью контроллера представления средства выбора документов.

Кроме того, в этой статье кратко описаны расширения поставщиков документов и причины, по которым разработчик должен учитывать их при написании приложений, которые могут обрабатывать облачные документы.

## <a name="related-links"></a>Связанные ссылки

- [Докпиккер (пример)](/samples/xamarin/ios-samples/ios8-docpicker)
- [Введение в iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Общие сведения о расширениях приложений](~/ios/platform/extensions.md)