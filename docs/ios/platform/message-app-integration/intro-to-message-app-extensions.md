---
title: Основные сведения о расширении приложения для сообщений в Xamarin. iOS
description: В этой статье показано, как включить расширение приложения сообщений в решение Xamarin. iOS, которое интегрируется с приложением для работы с сообщениями и предоставляет пользователю новые функциональные возможности.
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/02/2017
ms.openlocfilehash: 2cc27b18bdb58ee633cae2d61e8cc6a8064df581
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937137"
---
# <a name="message-app-extension-basics-in-xamarinios"></a>Основные сведения о расширении приложения для сообщений в Xamarin. iOS

_В этой статье показано, как включить расширение приложения сообщений в решение Xamarin. iOS, которое интегрируется с приложением для работы с сообщениями и предоставляет пользователю новые функциональные возможности._

Новое в iOS 10, расширение приложения для обмена сообщениями интегрируется с приложением **сообщений** и предоставляет пользователю новые функциональные возможности. Расширение может передавать текст, наклейки, файлы мультимедиа и интерактивные сообщения.

## <a name="about-message-app-extensions"></a>О расширениях приложений сообщений

Как указано выше, расширение приложения сообщений интегрируется с приложением **сообщений** и предоставляет пользователю новые функциональные возможности. Расширение может передавать текст, наклейки, файлы мультимедиа и интерактивные сообщения. Доступны два типа расширения приложения для сообщений:

- **Пакеты наклеек** — содержит коллекцию наклеек, которые пользователь может добавить в сообщение. Пакеты наклейок можно создавать без написания кода.
- **приложение iMessage** . может представлять пользовательский пользовательский интерфейс в приложении для работы с сообщениями для выбора наклеек, ввода текста, включая файлы мультимедиа (с необязательными преобразованиями типов), а также для создания, изменения и отправки сообщений о взаимодействии.

Расширения приложений для сообщений предоставляют три основных типа содержимого:

- **Интерактивные сообщения** — это тип пользовательского содержимого сообщений, создаваемого приложением, когда пользователь отправит сообщение, приложение будет запущено на переднем плане.
- **Наклейки** — это изображения, создаваемые приложением, которые могут быть включены в сообщения, отправляемые между пользователями.
- **Другое поддерживаемое содержимое** . приложение может предоставлять содержимое, например фотографии, видео, текст или ссылки на любые другие типы содержимого, которые всегда поддерживались приложением messages.

В составе iOS 10 теперь приложение сообщений содержит собственное встроенное хранилище приложений. В этом хранилище будут отображаться и переноситься все приложения, включающие расширения для приложений сообщений. В хранилище приложений новые сообщения будут отображаться все приложения, скачанные из магазина сообщений, чтобы обеспечить быстрый доступ к пользователям.

Кроме того, в iOS 10 добавлена встроенная возможность добавления атрибутов приложений, которая позволяет пользователю легко обнаружить приложение. Например, если один пользователь отправляет другое содержимое из приложения, которое не устанавливало второй пользователь (например, наклейка), имя отправляющего приложения отображается под содержимым в журнале сообщений. Если пользователь нажмет имя приложения, будет открыто приложение для хранения сообщений и приложение, выбранное в магазине.

Расширения приложений для работы с сообщениями похожи на существующие приложения iOS, знакомые разработчикам с созданием и получат доступ ко всем стандартным платформам и функциям стандартного приложения iOS. Пример:

- Они имеют доступ к покупкам в приложении.
- У них есть доступ к Apple Pay.
- Они имеют доступ к оборудованию устройства, например к камере.

Расширения для приложений для обработки сообщений поддерживаются только в iOS 10, однако содержимое, отправляемое этими расширениями, отображается на устройствах watchOS и macOS. На _странице новые недавние_ данные, добавленной в watchOS 3, будут отображаться последние наклейки, отправленные с телефона, включая сообщения из расширений сообщений, и разрешить пользователю отправлять эти наклейки из просмотра.

## <a name="about-the-messages-framework"></a>О платформе сообщений

Новая версия для iOS 10. платформа сообщений предоставляет интерфейс между расширением Message Apps и приложением сообщений на устройстве iOS пользователя. Когда пользователь запускает приложение из приложения для обмена сообщениями, эта платформа позволяет обнаруживать приложение и предоставляет данные и контекст, необходимые для размещения пользовательского интерфейса.

После запуска приложения пользователь взаимодействует с ним для создания нового содержимого, доступного через сообщение. Затем приложение использует платформу messages для пересылки вновь созданного содержимого в приложение messages для обработки.

Расширения платформы сообщений и приложения сообщений основаны на существующих технологиях расширения приложений iOS. Дополнительные сведения о расширениях приложений см. в статье [по программированию для расширения приложения](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)Apple.

В отличие от других точек расширения, предоставленных компанией Apple во всей системе, разработчику не нужно предоставлять ведущее приложение для расширений сообщений, так как само приложение сообщения выступает в качестве контейнера. Тем не менее, разработчик имеет возможность включить расширение "сообщения приложения" в новое или существующее приложение iOS и отправить его вместе с пакетом.

Если расширения приложений сообщений включены в пакет приложения iOS, значок приложения будет отображаться как на начальном экране устройства, так и в ящике приложения для сообщений в приложении "сообщения". Если он не включен в набор приложений, расширение "приложения для сообщений" будет отображаться только в хранилище приложений сообщений.

Даже если расширения для приложений сообщений не включены в пакет приложений узла, разработчику потребуется предоставить значок приложения в пакете расширения "приложения сообщений", так как это значок, который будет отображаться в других частях системы, таких как ящик приложения сообщений или параметры, для расширения.

## <a name="about-stickers"></a>О наклеек

Компания Apple разработала наклейки как новый способ обмена данными с iMessage, позволяя отправлять наклейки в виде любого другого содержимого сообщений, или они могут быть присоединены к предыдущему всплывающему сообщению внутри беседы.

Что такое наклейки?

- Они представляют собой образы, предоставляемые расширением для приложений для обработки сообщений.
- Они могут быть либо анимированными, либо статическими изображениями.
- Они предоставляют новый способ совместного использования содержимого изображения в приложении.

Создавать наклейки можно двумя способами:

1. Расширения приложений для обмена сообщениями с пакетом наклейок можно создавать в Xcode, не включая код. Все, что требуется, — это активы для наклеек и значков приложений.
2. Создав стандартное расширение приложений для обработки сообщений, которое предоставляет наклейки из кода с помощью платформы messages.

### <a name="creating-sticker-packs"></a>Создание пакетов наклейок

Пакеты наклейок создаются на основе специального шаблона в Xcode и просто предоставляют статический набор ресурсов изображений, которые можно использовать в качестве наклеек. Как упоминалось выше, для них не требуется никакого кода, разработчик просто перетаскивает файлы изображений в папку с пакетом наклеек в каталоге активов наклеек.

Для включения образа в пакет наклеек он должен соответствовать следующим требованиям.

- Изображения должны быть в формате PNG, АПНГ, GIF или JPEG. Компания Apple предлагает использовать только форматы PNG и АПНГ при предоставлении материалов наклейок.
- Анимированные наклейки поддерживают только форматы АПНГ и GIF.
- Изображения наклейок должны обеспечивать прозрачный фон, так как они могут быть помещены в беседу пользователя по пузырьковым сообщениям.
- Отдельные файлы изображений должны быть меньше 500 КБ.
- Изображения не могут быть меньше 100x100 точек или больше, чем 206 x 206 точек.

> [!IMPORTANT]
> Изображения наклейок всегда должны предоставляться при `@3x` разрешении в диапазоне от 300 x 300 до 618 x 618 пикселей. Система будет автоматически формировать `@2x` `@1x` версии и при необходимости в среде выполнения.

Компания Apple предлагает протестировать ресурсы изображений наклейки на различных цветных фоне (например, белый, черный, красный, желтый и многоцветный) и фотографии, чтобы обеспечить наилучшее представление всех возможных ситуаций.

Пакеты наклеек могут предоставлять наклейки в одном из трех доступных размеров:

- **Малый** 100 x 100 точек.
- **Средние** — 136 x 136 точек. Это размер по умолчанию.
- **Крупные** точки — 206 x 206.

Используйте инспектор атрибутов Xcode, чтобы задать размер для всего пакета наклеек и предоставить только ресурсы изображений, соответствующие запрошенному размеру, для получения наилучших результатов в браузере наклейок в приложении "сообщения".

Дополнительные сведения см. в справочнике [по нашим приложениям](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder) и [сообщениям](https://developer.apple.com/reference/messages)Apple.

## <a name="creating-a-custom-sticker-experience"></a>Создание пользовательских интерфейсов наклейок

Если приложению требуется больший контроль или гибкость, чем предоставляется пакетом наклеек, он может включать расширение приложения сообщения и предоставлять наклейки с помощью платформы messages для пользовательского интерфейса наклеек.

Каковы преимущества создания пользовательской наклейки?

1. Позволяет приложению настраивать способ отображения наклеек для пользователей приложения. Например, для представления наклеек в формате, отличном от стандартного макета сетки или на другом цветном фоне.
2. Позволяет динамически создавать наклейки из кода, а не включать в качестве статических ресурсов изображения.
3. Позволяет динамически скачивать ресурсы изображений наклеек с веб-сервера разработчика без выпуска новой версии в App Store.
4. Разрешает доступ к камере устройства для создания наклеек на лету.
5. Позволяет выполнять покупки в приложении, чтобы пользователь мог приобрести дополнительные наклейки внутри приложения.

Чтобы создать пользовательский интерфейс наклейки, выполните следующие действия.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Запустите Visual Studio для Mac.
2. Откройте решение, чтобы добавить расширение приложения сообщений в.
3. Выберите расширения **iOS**  >  **Extensions**  >  **iMessage расширение** и нажмите кнопку **Далее** :

    [![Выбор расширения iMessage](intro-to-message-app-extensions-images/message01.png)](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Введите **имя расширения** и нажмите кнопку **Далее** :

    [![Введите имя расширения](intro-to-message-app-extensions-images/message02.png)](intro-to-message-app-extensions-images/message02.png#lightbox)
5. Нажмите кнопку **создать** , чтобы создать расширение:

    [![Нажмите кнопку "создать"](intro-to-message-app-extensions-images/message03.png)](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Запустите среду Visual Studio.
2. Откройте решение, чтобы добавить расширение приложения для сообщений.
3. Выберите **расширения iOS > расширение iMessage (IOS)** и нажмите кнопку **Далее** :

    [![Выбор расширения iMessage (iOS)](intro-to-message-app-extensions-images/message01.w157-sml.png)](intro-to-message-app-extensions-images/message01.w157.png#lightbox)

4. Введите **имя** и нажмите кнопку " **ОК** ".

-----

По умолчанию `MessagesViewController.cs` файл будет добавлен в решение. Это Главная точка входа в расширение, которая наследуется от `MSMessageAppViewController` класса.

Платформа сообщений предоставляет классы для предоставления пользователям доступных наклеек:

- `MSStickerBrowserViewController`— Управляет представлением, в котором будут представлены наклейки. Он также соответствует `IMSStickerBrowserViewDataSource` интерфейсу, который возвращает число наклеек и наклейку для заданного индекса браузера.
- `MSStickerBrowserView`— Это представление, в котором будут отображаться доступные наклейки.
- `MSStickerSize`— Определяет отдельные размеры ячеек для сетки наклеек, представленных в представлении браузера.

### <a name="creating-a-custom-sticker-browser"></a>Создание пользовательского браузера наклейок

Разработчик может дополнительно настроить взаимодействие наклейок для пользователя, предоставив пользовательский браузер наклейок ( `MSMessageAppBrowserViewController` ) в расширении приложения сообщений. Пользовательский браузер наклейок изменяет способ представления наклеек пользователям при выборе наклейок для включения в поток сообщений.

Выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. В **панель решения**щелкните правой кнопкой мыши имя проекта расширения и выберите **Добавить**  >  **новый файл...**  >  **iOS | **  >  **Контроллер интерфейса**Apple Watch.
2. Введите `StickerBrowserViewController` в поле **имя** и нажмите кнопку **New (создать** ):

    [![Введите Стиккербровсервиевконтроллер для имени](intro-to-message-app-extensions-images/browser01.png)](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Откройте файл `StickerBrowserViewController.cs` для редактирования.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. В **Обозреватель решений**щелкните правой кнопкой мыши имя проекта расширения и выберите **Добавить**  >  **новый файл...**  >  **iOS | **  >  **Контроллер интерфейса**Apple Watch.
2. Введите `StickerBrowserViewController` в поле **имя** и нажмите кнопку **New (создать** ):

    [![Введите Стиккербровсервиевконтроллер для имени](intro-to-message-app-extensions-images/browser01.w157-sml.png)](intro-to-message-app-extensions-images/browser01.w157.png#lightbox)
3. Откройте файл `StickerBrowserViewController.cs` для редактирования.

-----

Сделайте так, чтобы они `StickerBrowserViewController.cs` выглядели следующим образом:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MonkeyStickers
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MSStickerSize stickerSize) : base (stickerSize)
        {
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            ...
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();
        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            return Stickers[(int)index];
        }
        #endregion
    }
}
```

Подробнее рассмотрим приведенный выше код. Он создает хранилище для наклеек, предоставляемых расширением:

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

И переопределяет два метода `MSStickerBrowserViewController` класса, чтобы предоставить данные для браузера из этого хранилища данных:

```csharp
public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
{
    return Stickers.Count;
}

public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
{
    return Stickers[(int)index];
}
```

`CreateSticker`Метод получает путь к ресурсу изображения из пакета расширения и использует его для создания нового экземпляра объекта `MSSticker` из этого ресурса, который добавляется в коллекцию:

```csharp
private void CreateSticker (string assetName, string localizedDescription)
{

    // Get path to asset
    var path = NSBundle.MainBundle.PathForResource (assetName, "png");
    if (path == null) {
        Console.WriteLine ("Couldn't create sticker {0}.", assetName);
        return;
    }

    // Build new sticker
    var stickerURL = new NSUrl (path);
    NSError error = null;
    var sticker = new MSSticker (stickerURL, localizedDescription, out error);
    if (error == null) {
        // Add to collection
        Stickers.Add (sticker);
    } else {
        // Report error
        Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
    }
}
```

`LoadSticker`Метод вызывается из `ViewDidLoad` , чтобы создать наклейку из именованного ресурса изображения (включенного в пакет приложения) и добавить его в коллекцию наклеек.

Чтобы реализовать пользовательский браузер наклейок, измените `MessagesViewController.cs` файл и сделайте его следующим:

```csharp
using System;
using UIKit;
using Messages;

namespace MonkeyStickers
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public StickerBrowserViewController BrowserViewController { get; set;}
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);
        }
        #endregion
    }
}
```

Подробно Взгляните на этот код, он создает хранилище для пользовательского браузера:

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

В `ViewDidLoad` методе создается экземпляр и настраивается новый браузер:

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

Затем он добавляет браузер в представление для его отображения:

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>Дальнейшая настройка наклейок

Дальнейшая настройка наклейок возможна благодаря включению в расширение приложения сообщений только двух классов:

- `MSStickerView`
- `MSSticker`

С помощью описанных выше методов расширение может поддерживать выбор наклейок, не зависящих от стандартного метода браузера наклеек. Кроме того, отображение наклейок может переключаться между двумя разными режимами представления:

- **Compact** — это режим по умолчанию, в котором вид наклейок занимает последние 25% представления сообщений.
- **Развернуто** — представление наклейок заполняет все представление сообщений.

Это представление наклейок может переключаться между этими режимами программным путем или вручную пользователем.

Взгляните на следующий пример обработки переключения между двумя разными режимами представления. Для каждого состояния потребуется два разных контроллера представления. `StickerBrowserViewController`Обработчик обрабатывает **компактное** представление и выглядит следующим образом:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MessageExtension
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set; }
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MessagesViewController messagesAppViewController, MSStickerSize stickerSize) : base (stickerSize)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("add", "Add New Sticker");
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();

        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            // Wanting to add a new sticker?
            if (index == 0) {
                // Yes, ask controller to present add sticker interface
                MessagesAppViewController.AddNewSticker ();
                return null;
            } else {
                // No, return existing sticker
                return Stickers [(int)index];
            }
        }
        #endregion
    }
}
```

`AddStickerViewController`Будет работать **развернутое** представление наклейок и выглядеть следующим образом:

```csharp
using System;
using Foundation;
using UIKit;
using Messages;

namespace MessageExtension
{
    public class AddStickerViewController : UIViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set;}
        public MSSticker NewSticker { get; set;}
        #endregion

        #region Constructors
        public AddStickerViewController (MessagesViewController messagesAppViewController)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Override Method
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Build interface to create new sticker
            var cancelButton = new UIButton (UIButtonType.RoundedRect);
            cancelButton.TouchDown += (sender, e) => {
                // Cancel add new sticker
                MessagesAppViewController.CancelAddNewSticker ();
            };
            View.AddSubview (cancelButton);

            var doneButton = new UIButton (UIButtonType.RoundedRect);
            doneButton.TouchDown += (sender, e) => {
                // Add new sticker to collection
                MessagesAppViewController.AddStickerToCollection (NewSticker);
            };
            View.AddSubview (doneButton);

            ...
        }
        #endregion
    }
}
```

`MessageViewController`Реализует эти контроллеры представлений для сохранения запрошенного состояния:

```csharp
using System;
using UIKit;
using Messages;

namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public bool IsAddingSticker { get; set;}
        public StickerBrowserViewController BrowserViewController { get; set; }
        public AddStickerViewController AddStickerController { get; set;}
        #endregion

        #region Constructors
        protected MessagesViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Public Methods
        public void PresentStickerBrowser ()
        {
            // Is the Add sticker view being displayed?
            if (IsAddingSticker) {
                // Yes, remove it from view
                AddStickerController.RemoveFromParentViewController ();
                AddStickerController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);

            // Save mode
            IsAddingSticker = false;
        }

        public void PresentAddSticker ()
        {
            // Is the sticker browser being displayed?
            if (!IsAddingSticker) {
                // Yes, remove it from view
                BrowserViewController.RemoveFromParentViewController ();
                BrowserViewController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (AddStickerController);
            AddStickerController.DidMoveToParentViewController (this);
            View.AddSubview (AddStickerController.View);

            // Save mode
            IsAddingSticker = true;
        }

        public void AddNewSticker ()
        {
            // Switch to expanded view mode
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void CancelAddNewSticker ()
        {
            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }

        public void AddStickerToCollection (MSSticker sticker)
        {
            // Add sticker to collection
            BrowserViewController.Stickers.Add (sticker);

            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (this, MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Create new Add controller and configure it as well
            AddStickerController = new AddStickerViewController (this);
            AddStickerController.View.Frame = View.Frame;

            // Initially present the sticker browser
            PresentStickerBrowser ();
        }

        public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            base.DidTransition (presentationStyle);

            // Take action based on style
            switch (presentationStyle) {
            case MSMessagesAppPresentationStyle.Compact:
                PresentStickerBrowser ();
                break;
            case MSMessagesAppPresentationStyle.Expanded:
                PresentAddSticker ();
                break;
            }
        }
        #endregion
    }
}
```

Когда пользователь запрашивает добавить новую наклейку в доступную коллекцию, новый объект становится `AddStickerViewController` видимым контроллером, а представление наклейок переходит в **развернутое** представление:

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

Когда пользователь выбирает добавляемую наклейку, она добавляется в доступную для нее коллекцию и запрашивается **компактное** представление:

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
}
```

`DidTransition`Метод переопределен для управления переключением между двумя режимами:

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
```

## <a name="summary"></a>Итоги

В этой статье рассматривается расширение приложения сообщений в решении Xamarin. iOS, которое интегрируется с приложением **сообщений** и предоставляет пользователю новые функции. Он покрывает использование расширения для отправки текста, наклеек, файлов мультимедиа и интерактивных сообщений.

## <a name="related-links"></a>Связанные ссылки

- [Построитель немороженных (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder)
- [Справочник по сообщениям](https://developer.apple.com/reference/messages)
- [Инструкции по программированию расширения приложения](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
