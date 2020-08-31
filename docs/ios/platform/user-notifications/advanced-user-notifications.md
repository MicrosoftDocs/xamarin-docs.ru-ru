---
title: Расширенные уведомления пользователей в Xamarin. iOS
description: В этой статье более подробно рассматривается инфраструктура User Notifications, появившаяся в iOS 10. В нем обсуждаются пользовательские уведомления, Пользовательский интерфейс уведомлений, вложения мультимедиа, настраиваемые пользовательские интерфейсы и многое другое.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/03/2018
ms.openlocfilehash: ce4614f7b21a42945a8541f2b18877e75a200f1a
ms.sourcegitcommit: f6a2f07d2e689e0cfd01b30008d50c83c63fa70c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2020
ms.locfileid: "89052777"
---
# <a name="advanced-user-notifications-in-xamarinios"></a>Расширенные уведомления пользователей в Xamarin. iOS

В iOS 10 платформа уведомлений пользователя обеспечивает доставку и обработку локальных и удаленных уведомлений. С помощью этой платформы расширение приложения или приложения может запланировать доставку локальных уведомлений, указав набор условий, например расположение или время суток.

## <a name="about-user-notifications"></a>Об уведомлениях пользователей

Новая платформа уведомлений пользователя обеспечивает доставку и обработку локальных и удаленных уведомлений. С помощью этой платформы расширение приложения или приложения может запланировать доставку локальных уведомлений, указав набор условий, например расположение или время суток.

Кроме того, приложение или расширение могут получать (и потенциально изменять) как локальные, так и удаленные уведомления по мере их доставки на устройство iOS пользователя.

Новая платформа пользовательского интерфейса уведомлений пользователя позволяет приложению или расширению приложения настраивать внешний вид как локальных, так и удаленных уведомлений, когда они представлены пользователю.

Эта платформа предоставляет следующие возможности, которые приложение может доставлять уведомления пользователю:

- **Визуальные оповещения** — где уведомление выводится из верхней части экрана в виде баннера.
- **Звук и вибрации** — могут быть связаны с уведомлением.
- **Значок приложения знакомство** — там, где значком приложения отображается значок, показывающий, что новое содержимое доступно. Например количество непрочитанных сообщений электронной почты.

Кроме того, в зависимости от текущего контекста пользователя существуют различные способы представления уведомления:

- Если устройство разблокировано, уведомление будет выдвигаться с верхней части экрана в виде баннера.
- Если устройство заблокировано, уведомление будет отображаться на экране блокировки пользователя.
- Если пользователь пропустил уведомление, он может открыть центр уведомлений и просмотреть все доступные, ожидающие уведомления.

Приложение Xamarin. iOS имеет два типа уведомлений о пользователях, которые могут быть отправлены:

- **Локальные уведомления** — они отправляются приложениями, установленными локально на устройстве пользователей.
- **Удаленные уведомления** — отправляются с удаленного сервера и либо представляются пользователю, либо инициирует фоновое обновление содержимого приложения.

Дополнительные сведения см. в документации по [расширенным пользовательским уведомлениям](~/ios/platform/user-notifications/enhanced-user-notifications.md) .

## <a name="the-new-user-notification-interface"></a>Новый интерфейс уведомления пользователя

Пользовательские уведомления в iOS 10 представляются новой конструкцией пользовательского интерфейса, которая предоставляет больше содержимого, например заголовок, подзаголовок и необязательные мультимедийные вложения, которые можно представить на экране блокировки в виде баннера в верхней части устройства или в центре уведомлений.

Независимо от того, где отображается уведомление пользователя в iOS 10, оно имеет тот же вид и возможности, что и те же функции и функциональные возможности.

В iOS 8 компания Apple предоставила такие уведомления, в которых разработчик может присоединить настраиваемые действия к уведомлению и разрешить пользователю предпринимать действия с уведомлением без запуска приложения. В iOS 9 улучшенные возможности уведомлений Apple с быстрым ответом, позволяющие пользователю отвечать на уведомления с текстовым вводом.

Поскольку уведомления пользователей являются более неотъемлемой частью взаимодействия с пользователем в iOS 10, компания Apple предоставляет дополнительные расширенные уведомления для поддержки трехмерного касания, когда пользователь нажимает на уведомление, а пользовательский интерфейс отображается для обеспечения богатого взаимодействия с уведомлением.

При отображении пользовательского интерфейса уведомления настраиваемого пользователя, если пользователь взаимодействует с любыми действиями, присоединенными к уведомлению, Пользовательский интерфейс можно мгновенно обновить, чтобы оставить отзыв о том, что было изменено.

С новой версии до iOS 10 API пользовательского интерфейса уведомлений пользователя позволяет приложению Xamarin. iOS легко использовать преимущества этих новых функций пользовательского интерфейса уведомлений пользователей.

## <a name="adding-media-attachments"></a>Добавление мультимедийных вложений

Одним из наиболее распространенных элементов, которыми обмениваются пользователи, являются фотографии, поэтому iOS 10 добавил возможность присоединить мультимедийный элемент (например, фотографию) непосредственно к уведомлению, где оно будет доступно пользователю вместе с остальной частью содержимого уведомления.

Однако из-за размера, связанного с отправкой даже небольшого изображения, присоединение его к удаленным полезным данным уведомлений станет непрактичным. Чтобы справиться с этой ситуацией, разработчик может использовать новое расширение службы в iOS 10 для загрузки образа из другого источника (например, хранилища данных CloudKit) и подключения его к содержимому уведомления перед его отображением пользователю.

Чтобы удаленное уведомление было изменено с помощью расширения службы, его полезные данные должны быть помечены как изменяемые. Пример:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

Рассмотрим следующий обзор процесса:

[![Добавление процесса вложений мультимедиа](advanced-user-notifications-images/extension02.png)](advanced-user-notifications-images/extension02.png#lightbox)

После доставки удаленного уведомления на устройство (через APNs) расширение службы может загрузить требуемый образ с помощью любого нужного средства (например, `NSURLSession` ) и после того, как он получит образ, он может изменить содержимое уведомления и отобразить его пользователю.

Ниже приведен пример того, как этот процесс может быть обработан в коде:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyNotification
{
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Constructors
        public NotificationService (IntPtr handle) : base(handle)
        {
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            // Get file URL
            var attachementPath = request.Content.UserInfo.ObjectForKey (new NSString ("my-attachment"));
            var url = new NSUrl (attachementPath.ToString ());

            // Download the file
            var localURL = new NSUrl ("PathToLocalCopy");

            // Create attachment
            var attachmentID = "image";
            var options = new UNNotificationAttachmentOptions ();
            NSError err;
            var attachment = UNNotificationAttachment.FromIdentifier (attachmentID, localURL, options , out err);

            // Modify contents
            var content = request.Content.MutableCopy() as UNMutableNotificationContent;
            content.Attachments = new UNNotificationAttachment [] { attachment };

            // Display notification
            contentHandler (content);
        }

        public override void TimeWillExpire ()
        {
            // Handle service timing out
        }
        #endregion
    }
}
```

При получении уведомления от APNs пользовательский адрес образа считывается из содержимого, а файл загружается с сервера. Затем `UNNotificationAttachement` создается с уникальным идентификатором и локальным расположением образа (в виде `NSUrl` ). Создается изменяемая копия содержимого уведомления и добавляются вложения мультимедиа. Наконец, уведомление отображается пользователю путем вызова `contentHandler` .

После добавления вложения к уведомлению система берет на себя перемещение и управление этим файлом.

В дополнение к удаленным уведомлениям, представленным выше, вложения мультимедиа также поддерживаются в локальных уведомлениях, где `UNNotificationAttachement` создается и вкладывается в уведомление вместе с его содержимым.

Уведомление в iOS 10 поддерживает мультимедийные вложения изображений (статические и GIF), аудио или видео, а система автоматически отображает правильный пользовательский интерфейс для каждого из этих типов вложений при отображении уведомления для пользователя.

> [!NOTE]
> Следует соблюдать осторожность, чтобы оптимизировать размер носителя и время, необходимое для загрузки носителя с удаленного сервера (или для сборки носителя для локальных уведомлений), так как система накладывает ограничения на оба варианта при запуске расширения службы приложения. Например, рассмотрите возможность отправки в уведомление уменьшенной версии изображения или маленьких клипов видео.

## <a name="creating-custom-user-interfaces"></a>Создание настраиваемых пользовательских интерфейсов

Чтобы создать настраиваемый пользовательский интерфейс для пользовательских уведомлений, разработчику необходимо добавить расширение содержимого уведомления (новое в iOS 10) в решение приложения.

Расширение содержимого уведомлений позволяет разработчику добавлять собственные представления в пользовательский интерфейс уведомлений и вырисовывать любое требуемое содержимое. Начиная с iOS 12, расширения содержимого уведомлений поддерживают Интерактивные элементы управления пользовательского интерфейса, такие как кнопки и ползунки. Дополнительные сведения см. в документации по [интерактивным уведомлениям в iOS 12](~/ios/platform/introduction-to-ios12/notifications/interactive.md) .

Для поддержки взаимодействия с пользователем с уведомлением пользователя необходимо создать настраиваемые действия, зарегистрировать их в системе и присоединить к уведомлению до того, как оно будет запланировано в системе. Для обработки этих действий будет вызвано расширение содержимого уведомления. Дополнительные сведения о настраиваемых действиях см. в разделе [Работа с действиями с уведомлениями](~/ios/platform/user-notifications/enhanced-user-notifications.md) в документе [расширенных уведомлений пользователя](~/ios/platform/user-notifications/enhanced-user-notifications.md) .

При отображении пользователю пользовательского интерфейса с пользовательским интерфейсом он будет иметь следующие элементы:

[![Уведомление пользователя с настраиваемыми элементами пользовательского интерфейса](advanced-user-notifications-images/customui01.png)](advanced-user-notifications-images/customui01.png#lightbox)

Если пользователь взаимодействует с настраиваемыми действиями (представленными под уведомлением), можно обновить пользовательский интерфейс, чтобы дать пользователю отзыв о том, что произошло при вызове данного действия.

### <a name="adding-a-notification-content-extension"></a>Добавление расширения содержимого уведомлений

Чтобы реализовать пользовательский интерфейс уведомления для пользовательского интерфейса в приложении Xamarin. iOS, выполните следующие действия.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Откройте решение приложения в Visual Studio для Mac.
2. Щелкните правой кнопкой мыши имя решения в **панель решения** и выберите **Добавить**  >  **Добавить новый проект**.
3. Выберите **iOS**  >  **расширения**iOS расширения  >  **содержимого уведомлений** и нажмите кнопку **Далее** : 

    [![Выбор расширений содержимого уведомлений](advanced-user-notifications-images/notify01.png)](advanced-user-notifications-images/notify01.png#lightbox)
4. Введите **имя** для расширения и нажмите кнопку **Далее** : 

    [![Введите имя для расширения](advanced-user-notifications-images/notify02.png)](advanced-user-notifications-images/notify02.png#lightbox)
5. При необходимости измените **имя проекта** или **имя решения** , а затем нажмите кнопку **создать** : 

    [![Изменение имени проекта и/или имени решения](advanced-user-notifications-images/notify03.png)](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Откройте решение приложения в Visual Studio для Mac.
2. Щелкните правой кнопкой мыши имя решения в **Обозреватель решений** и выберите **Добавить > новый проект...**.
3. Выберите **Visual C# > расширения iOS > расширение содержимого уведомлений**:

    [![Выбор расширений содержимого уведомлений](advanced-user-notifications-images/notify01.w157-sml.png)](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. Введите **имя** расширения и нажмите кнопку **ОК** .

-----

При добавлении расширения содержимого уведомлений в решение в проекте расширения будут созданы три файла:

1. `NotificationViewController.cs` — Это основной контроллер представления для расширения содержимого уведомлений.
2. `MainInterface.storyboard` — Где разработчик видит видимый пользовательский интерфейс для расширения содержимого уведомления в конструкторе iOS.
3. `Info.plist` — Управляет конфигурацией расширения содержимого уведомлений.

Файл по умолчанию `NotificationViewController.cs` выглядит следующим образом:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

        }
        #endregion
    }
}
```

`DidReceiveNotification`Метод вызывается при расширении уведомления пользователем, чтобы расширение содержимого уведомлений могла заполнять пользовательский интерфейс содержимым `UNNotification` . В приведенном выше примере к представлению добавлена метка, доступная для кода с именем `label` и используемая для отображения текста уведомления.

### <a name="setting-the-notification-content-extensions-categories"></a>Настройка категорий расширения содержимого уведомлений

Системе необходимо знать, как найти расширение содержимого уведомлений приложения на основе категорий, на которые он отвечает. Выполните следующие действия.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Дважды щелкните `Info.plist` файл расширения в **панель решения** , чтобы открыть его для редактирования.
2. Перейдите в представление **исходного кода**.
3. Разверните `NSExtension` ключ.
4. Добавьте `UNNotificationExtensionCategory` ключ в качестве **строки** типа со значением категории, к которой принадлежит расширение (в этом примере — INVITE): 

    [![Добавление ключа Уннотификатионекстенсионкатегори](advanced-user-notifications-images/customui02.png)](advanced-user-notifications-images/customui02.png#lightbox)
5. Сохраните изменения.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Дважды щелкните `Info.plist` файл расширения в **Обозреватель решений** , чтобы открыть его для редактирования.
2. Разверните `NSExtension` ключ.
3. Добавьте `UNNotificationExtensionCategory` ключ в качестве **строки** типа со значением категории, к которой принадлежит расширение (в этом примере — INVITE): 

    [![Добавление ключа Уннотификатионекстенсионкатегори](advanced-user-notifications-images/customui02w.png)](advanced-user-notifications-images/customui02w.png#lightbox)
4. Сохраните изменения.

-----

Категории расширения содержимого уведомлений ( `UNNotificationExtensionCategory` ) используют те же значения категории, которые используются для регистрации действий уведомления. В ситуации, когда приложение будет использовать один и тот же пользовательский интерфейс для нескольких категорий, переключиться `UNNotificationExtensionCategory` на **массив** типов и предоставить все необходимые категории. Пример:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Категории расширения содержимого уведомлений](advanced-user-notifications-images/customui03.png)](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Категории расширения содержимого уведомлений](advanced-user-notifications-images/customui03w.png)](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>Скрытие содержимого уведомлений по умолчанию

В ситуации, когда пользовательский интерфейс уведомления будет отображать то же содержимое, что и уведомление по умолчанию (заголовок, подзаголовок и текст, отображаемые в нижней части пользовательского интерфейса уведомления), эти сведения по умолчанию можно скрыть, добавив `UNNotificationExtensionDefaultContentHidden` ключ к `NSExtensionAttributes` ключу как тип **Boolean** со значением `YES` в файле расширения `Info.plist` :

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Поиск сведений по умолчанию](advanced-user-notifications-images/customui04.png)](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Поиск сведений по умолчанию](advanced-user-notifications-images/customui04w.png)](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>Разработка пользовательского интерфейса

Чтобы создать настраиваемый пользовательский интерфейс расширения содержимого уведомлений, дважды щелкните `MainInterface.storyboard` файл, чтобы открыть его для редактирования в конструкторе iOS. Перетащите элементы, необходимые для создания требуемого интерфейса (например `UILabels` , и `UIImageViews` ).

> [!NOTE]
> Начиная с iOS 12, расширение содержимого уведомления может включать интерактивные элементы управления, такие как кнопки и текстовые поля. Дополнительные сведения см. в документации по [интерактивным уведомлениям в iOS 12](~/ios/platform/introduction-to-ios12/notifications/interactive.md) .

После размещения пользовательского интерфейса и необходимых элементов управления для кода C# откройте `NotificationViewController.cs` для редактирования и измените `DidReceiveNotification` метод, чтобы заполнить пользовательский интерфейс, когда пользователь развернет уведомление. Пример:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

        }
        #endregion
    }
}
```

### <a name="setting-the-content-area-size"></a>Задание размера области содержимого

Чтобы настроить размер области содержимого, отображаемой для пользователя, приведенный ниже код задает `PreferredContentSize` для свойства в `ViewDidLoad` методе нужный размер. Этот размер можно также изменить, применив ограничения к представлению в конструкторе iOS, и разработчик должен выбрать метод, который лучше подходит для них.

Так как система уведомлений уже выполняется до вызова расширения содержимого уведомления, область содержимого начинает работать в полном размере и анимируется до требуемого размера, когда пользователь будет представлен пользователю.

Чтобы устранить этот результат, измените `Info.plist` файл для расширения и присвойте `UNNotificationExtensionInitialContentSizeRatio` ключу `NSExtensionAttributes` ключа значение Type **Number** со значением, представляющим требуемое соотношение. Пример:

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Ключ Уннотификатионекстенсионинитиалконтентсизератио](advanced-user-notifications-images/customui05.png)](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Ключ Уннотификатионекстенсионинитиалконтентсизератио](advanced-user-notifications-images/customui05w.png)](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>Использование мультимедийных вложений в пользовательском ИНТЕРФЕЙСе

Поскольку вложенные файлы (как показано в разделе [Добавление вложений мультимедиа](#adding-media-attachments) выше) являются частью полезных данных уведомления, доступ к ним можно получить и отобразить в расширении содержимого уведомления так же, как и в пользовательском интерфейсе уведомления по умолчанию.

Например, если пользовательский пользовательский интерфейс выше включал в себя объект `UIImageView` , предоставленный коду C#, можно использовать следующий код, чтобы заполнить его с помощью вложения мультимедиа:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }
        #endregion
    }
}
```

Так как вложение мультимедиа управляется системой, оно находится за пределами песочницы приложения. Расширение должно информировать систему о том, что ему нужен доступ к файлу, вызвав `StartAccessingSecurityScopedResource` метод. Когда расширение выполняется с помощью файла, ему необходимо вызвать метод, `StopAccessingSecurityScopedResource` чтобы освободить его подключение.

### <a name="adding-custom-actions-to-a-custom-ui"></a>Добавление настраиваемых действий в пользовательский интерфейс

Настраиваемые кнопки действий можно использовать для добавления интерактивности в пользовательский пользовательский интерфейс уведомления. Дополнительные сведения о настраиваемых действиях см. в разделе [Работа с действиями с уведомлениями](~/ios/platform/user-notifications/enhanced-user-notifications.md) в документе [расширенных уведомлений пользователя](~/ios/platform/user-notifications/enhanced-user-notifications.md) .

Помимо настраиваемых действий, расширение содержимого уведомлений может отвечать на следующие встроенные действия:

- **Действие по умолчанию** . это происходит, когда пользователь отменяет уведомление для открытия приложения и отображает подробные сведения об этом уведомлении.
- **Действие "отклонить** " — это действие отправляется в приложение, когда пользователь закрывает данное уведомление.

Расширения содержимого уведомлений также могут обновлять пользовательский интерфейс при вызове пользователем одного из настраиваемых действий, например, отображение даты, принятой при нажатии пользователем кнопки **принять** пользовательское действие. Кроме того, расширения содержимого уведомлений могут сообщить системе о задержке закрытия пользовательского интерфейса уведомления, чтобы пользователь мог увидеть результат действия до закрытия уведомления.

Это делается путем реализации второй версии `DidReceiveNotification` метода, включающей обработчик завершения. Пример:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;
using CoreGraphics;

namespace myApp {
    public class NotificationViewController : UIViewController, UNNotificationContentExtension {

        public override void ViewDidLoad() {
            base.ViewDidLoad();

            // Adjust the size of the content area
            var size = View.Bounds.Size
            PreferredContentSize = new CGSize(size.Width, size.Width/2);
        }

        public void DidReceiveNotification(UNNotification notification) {

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = Content.UserInfo["location"] as string;
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments[0];
                if (attachment.Url.StartAccessingSecurityScopedResource()) {
                    EventImage.Image = UIImage.FromFile(attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch(response.ActionIdentifier){
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
    }
}
```

При добавлении `Server.PostEventResponse` обработчика в `DidReceiveNotification` метод расширения содержимого уведомления расширение *должно* поддерживать все пользовательские действия. Расширение также может перенаправить пользовательские действия в включающее приложение, изменив `UNNotificationContentExtensionResponseOption` . Пример:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>Работа с действием ввода текста в пользовательском ИНТЕРФЕЙСе

В зависимости от структуры приложения и уведомления могут возникнуть моменты, требующие от пользователя ввести текст в уведомление (например, ответ на сообщение). Расширение содержимого уведомлений имеет доступ ко встроенному действию ввода текста, как и стандартное уведомление.

Пример:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Computed Properties
        // Allow to take input
        public override bool CanBecomeFirstResponder {
            get { return true; }
        }

        // Return the custom created text input view with the
        // required buttons and return here
        public override UIView InputAccessoryView {
            get { return InputView; }
        }
        #endregion

        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Private Methods
        private UNNotificationCategory MakeExtensionCategory ()
        {

            // Create Accept Action
            ...

            // Create decline Action
            ...

            // Create Text Input Action
            var commentID = "comment";
            var commentTitle = "Comment";
            var textInputButtonTitle = "Send";
            var textInputPlaceholder = "Enter comment here...";
            var commentAction = UNTextInputNotificationAction.FromIdentifier (commentID, commentTitle, UNNotificationActionOptions.None, textInputButtonTitle, textInputPlaceholder);

            // Create category
            var categoryID = "event-invite";
            var actions = new UNNotificationAction [] { acceptAction, declineAction, commentAction };
            var intentIDs = new string [] { };
            var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);

            // Return new category
            return category;

        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Is text input?
            if (response is UNTextInputNotificationResponse) {
                var textResponse = response as UNTextInputNotificationResponse;
                Server.Send (textResponse.UserText, () => {
                    // Close Notification
                    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
                });
            }

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch (response.ActionIdentifier) {
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
        #endregion
    }
}
```

Этот код создает новое действие ввода текста и добавляет его в категорию расширения (в `MakeExtensionCategory` методе). В `DidReceive` методе переопределения он обрабатывает ввод текста пользователем с помощью следующего кода:

```csharp
// Is text input?
if (response is UNTextInputNotificationResponse) {
    var textResponse = response as UNTextInputNotificationResponse;
    Server.Send (textResponse.UserText, () => {
        // Close Notification
        completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
    });
}
```

Если при разработке вызывается Добавление пользовательских кнопок в поле ввода текста, добавьте следующий код, чтобы включить их:

```csharp
// Allow to take input
public override bool CanBecomeFirstResponder {
    get {return true;}
}

// Return the custom created text input view with the
// required buttons and return here
public override UIView InputAccessoryView {
    get {return InputView;}
}
```

Когда пользователь запускает действие комментария, необходимо активировать как контроллер представления, так и поле ввода пользовательского текста:

```csharp
// Update UI when the user interacts with the
// Notification
Server.PostEventResponse += (response) {
    // Take action based on the response
    switch(response.ActionIdentifier){
    ...
    case "comment":
        BecomeFirstResponder();
        TextField.BecomeFirstResponder();
        break;
    }

    // Close Notification
    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);

};
```

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается использование новой платформы уведомлений пользователей в приложении Xamarin. iOS. Он охватывает Добавление мультимедийных вложений в локальное и удаленное уведомление и охватывает его с помощью пользовательского интерфейса уведомления нового пользователя для создания настраиваемых пользовательских интерфейсов уведомлений.

## <a name="related-links"></a>Связанные ссылки

- [Примеры iOS 10](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [Справочник по Усернотификатионс Framework](https://developer.apple.com/reference/usernotifications)
- [усернотификатионсуи](https://developer.apple.com/reference/usernotificationsui)
- [Локальное и удаленное уведомление по программированию](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
