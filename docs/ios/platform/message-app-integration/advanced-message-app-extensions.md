---
title: Расширенные расширения приложений для сообщений в Xamarin. iOS
description: В этой статье показаны дополнительные методы работы с расширениями приложений сообщений в решении Xamarin. iOS, которое интегрируется с приложением сообщений и предоставляет пользователю новые функциональные возможности.
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: c303125ac02c8e7c47fb16928df1432c66a8e8d2
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937154"
---
# <a name="advanced-message-app-extensions-in-xamarinios"></a>Расширенные расширения приложений для сообщений в Xamarin. iOS

_В этой статье показаны дополнительные методы работы с расширениями приложений сообщений в решении Xamarin. iOS, которое интегрируется с приложением сообщений и предоставляет пользователю новые функциональные возможности._

Новое в iOS 10, расширение приложения для обмена сообщениями интегрируется с приложением **сообщений** и предоставляет пользователю новые функциональные возможности. Расширение может передавать текст, наклейки, файлы мультимедиа и интерактивные сообщения.

## <a name="about-message-app-extensions"></a>О расширениях приложений сообщений

Как указано выше, расширение приложения сообщений интегрируется с приложением **сообщений** и предоставляет пользователю новые функциональные возможности. Расширение может передавать текст, наклейки, файлы мультимедиа и интерактивные сообщения. Доступны два типа расширения приложения для сообщений:

- **Пакеты наклеек** — содержит коллекцию наклеек, которые пользователь может добавить в сообщение. Пакеты наклейок можно создавать без написания кода.
- **приложение iMessage** . может представлять пользовательский пользовательский интерфейс в приложении для работы с сообщениями для выбора наклеек, ввода текста, включая файлы мультимедиа (с необязательными преобразованиями типов), а также для создания, изменения и отправки сообщений о взаимодействии.

Расширения приложений для сообщений предоставляют три основных типа содержимого:

- **Интерактивные сообщения** — это тип пользовательского содержимого сообщений, создаваемого приложением, когда пользователь отправит сообщение, приложение будет запущено на переднем плане.
- **Наклейки** — это изображения, создаваемые приложением, которые могут быть включены в сообщения, отправляемые между пользователями. Пример реализации приложения пакета наклейок см. в нашем примере приложения " [Построитель](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder) ".
- **Другое поддерживаемое содержимое** . приложение может предоставлять содержимое, например фотографии, видео, текст или ссылки типа, которые всегда поддерживались приложением messages.

В составе iOS 10 теперь приложение сообщений содержит собственное встроенное хранилище приложений. В этом хранилище будут отображаться и переноситься все приложения, включающие расширения для приложений сообщений. В хранилище приложений новые сообщения будут отображаться все приложения, скачанные из магазина сообщений, чтобы обеспечить быстрый доступ к пользователям.

Кроме того, в iOS 10 добавлена встроенная возможность добавления атрибутов приложений, которая позволяет пользователю легко обнаружить приложение. Например, если один пользователь отправляет другое содержимое из приложения, которое не устанавливало второй пользователь (например, наклейка), имя отправляющего приложения отображается под содержимым в журнале сообщений. Если пользователь нажмет имя приложения, будет открыто приложение для хранения сообщений и приложение, выбранное в магазине.

Расширения приложений для работы с сообщениями похожи на существующие приложения iOS, которые знакомы разработчику, и получат доступ ко всем стандартным платформам и функциям стандартного приложения iOS. Например.

- Они имеют доступ к покупкам в приложении.
- У них есть доступ к Apple Pay.
- Они имеют доступ к оборудованию устройства, например к камере.

Расширения для приложений для обработки сообщений поддерживаются только в iOS 10, однако содержимое, отправляемое этими расширениями, отображается на устройствах watchOS и macOS. На _странице новые недавние_ данные, добавленной в watchOS 3, будут отображаться последние наклейки, отправленные с телефона, включая сообщения из расширений сообщений, и разрешить пользователю отправлять эти наклейки из просмотра.

## <a name="about-interactive-messages"></a>О интерактивных сообщениях

Интерактивные сообщения представляют пользовательский пузырьк сообщений и предоставляются расширением приложения для обработки сообщений. Они позволяют пользователю создавать содержимое интерактивного сообщения, вставлять его в поле ввода сообщения и отсылать.

[![Создание интерактивного содержимого сообщения](advanced-message-app-extensions-images/interactive01.png)](advanced-message-app-extensions-images/interactive01.png#lightbox)

Принимающий пользователь может ответить на интерактивное сообщение, коснувшись его всплывающего сообщения в журнале сообщений, чтобы загрузить расширение приложения сообщения, которое его создало. Расширение будет запущено в полноэкранном режиме и позволит пользователю создать ответ и отправить его обратному пользователю.

[![Расширение запущено на весь экран](advanced-message-app-extensions-images/interactive02.png)](advanced-message-app-extensions-images/interactive02.png#lightbox)

Следующие разделы подробно рассматриваются ниже.

- Общие сведения об API сообщений
- Жизненный цикл расширения
- Составление сообщения
- Отправка сообщения

## <a name="messages-api-overview"></a>Общие сведения об API сообщений

При вызове пользователем в нижней части журнала сообщений в режиме Compact View появится расширение приложения сообщения:

[![Общие сведения об API сообщений](advanced-message-app-extensions-images/interactive03.png)](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. `MSMessageAppViewController`Объект в расширении приложения сообщения является основным классом, который вызывается, когда пользователю отображается представление расширения.
2. Диалог представлен пользователю как `MSConversation` экземпляр объекта.
3. `MSMessage`Класс представляет заданный всплывающий объект сообщения в диалоге.
4. `MSSession`управляет отправкой сообщения.
5. `MSMessageTemplateLayout`управляет отображением сообщения

## <a name="the-extension-lifecycle"></a>Жизненный цикл расширения

Взгляните на процесс расширения приложения сообщений, которое становится активным:

[![Процесс расширения приложения сообщений становится активным](advanced-message-app-extensions-images/interactive04.png)](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. При запуске расширения (например, из ящика приложения) приложение Message запустит процесс.
2. `DidBecomeActive`Метод вызывается и передается `MSConversation` , который представляет диалог, в котором выполняется расширение приложения сообщения.
3. Так как расширение основано на `UIViewController` обеих `ViewWillAppear` `ViewDidAppear` вызовах и.

Далее рассмотрим процесс отключения расширения приложения для сообщений:

[![Процесс расширения приложения сообщений становится неактивным](advanced-message-app-extensions-images/interactive05.png)](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. При деактивации расширения приложения для сообщений `ViewWillDisappear` метод будет вызван первым.
2. Затем `ViewDidDisappear` будет вызван метод.
3. `WillResignActive`Метод вызывается и передается `MSConversation` , который представляет диалог, в котором выполняется расширение приложения сообщения. На этом этапе будет выпущено подключение между приложением "сообщения" и расширением.
4. В более поздней точке процесс завершается приложением messages.

Так как расширение является коротким процессом, оно завершается активно системой для экономии энергии процессора и аккумулятора. Разработчик должен учитывать это при проектировании и реализации расширения приложения для обработки сообщений.

## <a name="composing-a-message"></a>Составление сообщения

Когда расширение приложения сообщения выполняется в процессе и отображает пользовательский интерфейс, для создания нового сообщения можно использовать следующий код:

```csharp
MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
{
    var components = new NSUrlComponents {
        QueryItems = iceCream.QueryItems
    };

    var layout = new MSMessageTemplateLayout {
        Image = iceCream.RenderSticker (true),
        Caption = caption
    };

    var message = new MSMessage (session ?? new MSSession()) {
        Url = components.Url,
        Layout = layout
    };

    return message;
}
```

Этот код создает новый объект `MSMessage` и задает несколько свойств (например, `Url` ). Хотя сообщение может быть создано только в iOS, оно может быть отправлено как в iOS, так и в macOS.

Если пользователь щелкнет всплывающее сообщение в диалоговом окне macOS, Mac попытается открыть адрес, указанный в URL-адресе в браузере. В результате веб-сайт разработчика должен иметь возможность показывать некоторое представление сообщения в веб-браузере на компьютерах на базе macOS.

`AccessibilityLabel`Свойство используется программами чтения с экрана для чтения записи диалога с пользователем. `Layout`Свойство указывает, как будет отображаться сообщение, в настоящее время поддерживается только `MSMessageTemplateLayout` и выглядит следующим образом:

[![Шаблон Мсмессажетемплателайаут](advanced-message-app-extensions-images/interactive06.png)](advanced-message-app-extensions-images/interactive06.png#lightbox)

`Image`Свойство объекта `MSMessageTemplateLayout` предоставляет содержимое для основного текста мессажебуббле на экране. `MediaFileUrl`Свойство также предоставляет содержимое для текста всплывающего окна сообщения, но позволяет получить содержимое, которое не поддерживается `UIImage` (например, видеофайл, который должен пройти в фоновом режиме). Если указаны оба `Image` `MediaFileUrl` Свойства и, `Image` свойство имеет приоритет. `MediaFileUrl`Поддерживает файлы PNG, JPEG, GIF и Video (в любом формате, который может воспроизводиться платформой проигрывателя мультимедиа) в форматах мультимедиа.

Рекомендуемый размер носителя — 300 x 300 пикселей в разрешении 3 раза. Также принимаются немного больше и меньше ресурсов, и Apple предлагает тестирование с несколькими различными размерами, чтобы получить лучшие результаты. Приложение для работы с сообщениями выполнит выборку и масштабировать этот носитель по мере необходимости.

Когда ресурсы отправляются получателю, все подключенные файлы мультимедиа будут автоматически перекодироваться приложением messages, чтобы оптимизировать их из передачи по сети. По этой причине Apple не рекомендует включать текст на носитель, прикрепленный к сообщению, так как носитель будет масштабироваться и сжиматься для передачи, что может привести к визуализации текста неразборчива.

`ImageTitle`Свойства и `ImageSubtitle` предоставляют описание носителя, представленного в пузырьке сообщений. Эти свойства будут отправлены в виде текста на принимающее устройство, где они будут четко отображены в левом нижнем углу изображения.

`Caption`Свойства, `SubCaption` `TrailingCaption` и более подробно `TrailingSubcaption` описывают изображение и будут подготавливаться к просмотру в разделе под изображением. Если задать для всех этих свойств, `null` будет создаваться пузырьковая подсеть сообщений без области субтитров:

[![Пузырьковое сообщение без области субтитров](advanced-message-app-extensions-images/interactive07.png)](advanced-message-app-extensions-images/interactive07.png#lightbox)

Последнее, что нужно отметить, — это приложение messages, которое будет отображать значок расширения приложения сообщения в верхнем левом углу всплывающего сообщения.

## <a name="sending-a-message"></a>Отправка сообщения

После `MSMessage` составления a можно использовать следующий код для его отправки:

```csharp
public void SendMessage (MSMessage message)
{
    // Insert the message into the conversation
    ActiveConversation.InsertMessage (message, (error) => {
        // Did the message send successfully?
        if (error == null) {
            // Handle successful send
        } else {
            // Report Error
            Console.WriteLine ("Error: {0}", error);
        }
    };

}
```

`ActiveConversation`Свойство объекта `MSMessagesAppViewController` будет содержать текущий диалог, в котором было запущено расширение приложения сообщений.

Вызовите метод, `InsertMessage` `MSConversation` чтобы включить сообщение в диалог и обработайте любые ошибки, которые могут возникнуть. Если сообщение успешно включено, всплывающее сообщение будет отображаться в поле ввода.

Кроме того, расширение может передавать в диалоге различные типы данных, например:

- **Полнотекстовым** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Вло** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Stickers**  -  Наклейки `ActiveConversation.InsertSticker (sticker, (obj) => {...});` где `sticker` — `MSSticker` .

Как только новое содержимое появится в поле ввода, пользователь сможет отправить сообщение, нажав синюю кнопку **Отправить** (как и обычное сообщение). Не существует способа, которым расширение приложения для сообщений автоматически отправляет содержимое, этот процесс полностью находится под контролем пользователя.

## <a name="handling-the-compact-and-expanded-modes"></a>Обработка компактного и расширенного режимов

Расширение приложения сообщений может отображаться в одном из двух режимов просмотра:

[![Расширение приложения сообщений, отображаемое в двух разных режимах представления: Compact & развернуто](advanced-message-app-extensions-images/interactive08.png)](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** — это режим по умолчанию, в котором расширение приложения для сообщений занимает последние 25% представления сообщений. В компактном режиме приложение не имеет доступа к распознавателям жестов с горизонтальной прокруткой или прокрутке. Приложение имеет доступ к полю ввода, и вызовы `InsertMessage` будут немедленно отображаться пользователю.
- **Развернуто** — расширение приложения для сообщений заполняет все представление сообщений. У него нет доступа к полю ввода, но у него есть доступ к распознавателям пера для клавиатуры, горизонтальной прокрутки и считывания.

Расширение приложения для обмена сообщениями может переключаться между этими режимами программно или вручную пользователем в любое время и должно быть быстро реагировать на любые изменения в режиме просмотра.

Взгляните на следующий пример обработки переключения между двумя разными режимами представления. Для каждого состояния потребуется два разных контроллера представления. `StickerBrowserViewController`Обработчик обрабатывает **свернутое** представление и `AddStickerViewController` будет обрабатывать **развернутое** представление:

```csharp
using System;

using Messages;
using Foundation;
using UIKit;

namespace MessagesExtension {
    public partial class MessagesViewController : MSMessagesAppViewController, IIceCreamsViewControllerDelegate, IBuildIceCreamViewControllerDelegate {
        public MessagesViewController (IntPtr handle) : base (handle)
        {
        }

        public override void WillBecomeActive (MSConversation conversation)
        {
            base.WillBecomeActive (conversation);

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, PresentationStyle);
        }

        public override void WillTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected an active converstation");

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, presentationStyle);
        }

        void PresentViewController (MSConversation conversation, MSMessagesAppPresentationStyle presentationStyle)
        {
            // Determine the controller to present.
            UIViewController controller;

            if (presentationStyle == MSMessagesAppPresentationStyle.Compact) {
                // Show a list of previously created ice creams.
                controller = InstantiateIceCreamsController ();
            } else {
                var iceCream = new IceCream (conversation.SelectedMessage);
                controller = iceCream.IsComplete ? InstantiateCompletedIceCreamController (iceCream) : InstantiateBuildIceCreamController (iceCream);
            }

            foreach (var child in ChildViewControllers) {
                child.WillMoveToParentViewController (null);
                child.View.RemoveFromSuperview ();
                child.RemoveFromParentViewController ();
            }

            AddChildViewController (controller);
            controller.View.Frame = View.Bounds;
            controller.View.TranslatesAutoresizingMaskIntoConstraints = false;
            View.AddSubview (controller.View);

            controller.View.LeftAnchor.ConstraintEqualTo (View.LeftAnchor).Active = true;
            controller.View.RightAnchor.ConstraintEqualTo (View.RightAnchor).Active = true;
            controller.View.TopAnchor.ConstraintEqualTo (View.TopAnchor).Active = true;
            controller.View.BottomAnchor.ConstraintEqualTo (View.BottomAnchor).Active = true;

            controller.DidMoveToParentViewController (this);
        }

        UIViewController InstantiateIceCreamsController ()
        {
            // Instantiate a `IceCreamsViewController` and present it.
            var controller = Storyboard.InstantiateViewController (IceCreamsViewController.StoryboardIdentifier) as IceCreamsViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an IceCreamsViewController from the storyboard");

            controller.Builder = this;
            return controller;
        }

        UIViewController InstantiateBuildIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (BuildIceCreamViewController.StoryboardIdentifier) as BuildIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an BuildIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            controller.Builder = this;

            return controller;
        }

        public UIViewController InstantiateCompletedIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (CompletedIceCreamViewController.StoryboardIdentifier) as CompletedIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an CompletedIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            return controller;
        }

        public void DidSelectAdd (IceCreamsViewController controller)
        {
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void Build (BuildIceCreamViewController controller, IceCreamPart iceCreamPart)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected a conversation");

            var iceCream = controller.IceCream;
            if (iceCream == null)
                throw new Exception ("Expected the controller to be displaying an ice cream");

            string messageCaption = string.Empty;
            var b = iceCreamPart as Base;
            var s = iceCreamPart as Scoops;
            var t = iceCreamPart as Topping;

            if (b != null) {
                iceCream.Base = b;
                messageCaption = "Let's build an ice cream";
            } else if (s != null) {
                iceCream.Scoops = s;
                messageCaption = "I added some scoops";
            } else if (t != null) {
                iceCream.Topping = t;
                messageCaption = "Our finished ice cream";
            } else {
                throw new Exception ("Unexpected type of ice cream part selected.");
            }

            // Create a new message with the same session as any currently selected message.
            var message = ComposeMessage (iceCream, messageCaption, conversation.SelectedMessage?.Session);

            // Add the message to the conversation.
            conversation.InsertMessage (message, null);

            // If the ice cream is complete, save it in the history.
            if (iceCream.IsComplete) {
                var history = IceCreamHistory.Load ();
                history.Append (iceCream);
                history.Save ();
            }

            Dismiss ();
        }

        MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
        {
            var components = new NSUrlComponents {
                QueryItems = iceCream.QueryItems
            };

            var layout = new MSMessageTemplateLayout {
                Image = iceCream.RenderSticker (true),
                Caption = caption
            };

            var message = new MSMessage (session ?? new MSSession()) {
                Url = components.Url,
                Layout = layout
            };

            return message;
        }
    }
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

При необходимости приложение могло бы использовать `WillTransition` метод для изменения режима просмотра до того, как он будет представлен пользователю (как это делается в примере Ицекреам Builder выше). Дополнительные сведения см. в дополнительной документации по [настройке наклейок](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) .

## <a name="replying-to-a-message"></a>Ответ на сообщение

Существует два случая, когда необходимо обрабатывать расширение приложения сообщения при ответе на сообщение:

[![Расширение приложения сообщения в режимах "Неактивный" и "активный"](advanced-message-app-extensions-images/interactive09.png)](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **Расширение неактивно** . в записи сообщения появляется одно из сообщений о расширении приложения сообщений, которое пользователь может коснуться, чтобы активировать расширения и продолжить интерактивный диалог.
- **Расширение активно** . пользователь может коснуться всплывающего сообщения о расширении приложения сообщения в записи сообщения, чтобы перейти в режим расширенного просмотра и продолжить интерактивный процесс с того места, на котором они остановились.

### <a name="the-extension-is-inactive"></a>Расширение неактивно

При нажатии всплывающего сообщения пользователем в записи о сообщении, если расширение приложения сообщения неактивно, произойдет следующий процесс:

[![Обработка неактивного всплывающего сообщения](advanced-message-app-extensions-images/interactive10.png)](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. Пользователь касания всплывающего сообщения расширения.
2. При запуске расширения приложение Message запустит процесс.
3. `DidBecomeActive`Метод вызывается и передается `MSConversation` , который представляет диалог, в котором выполняется расширение приложения сообщения.
4. Так как расширение основано на `UIViewController` обеих `ViewWillAppear` `ViewDidAppear` вызовах и.

По завершении процесса расширение приложения сообщения будет представлено в расширенном режиме просмотра.

### <a name="the-extension-is-active"></a>Расширение активно

При нажатии всплывающего сообщения пользователем в записи о сообщении и при активном расширении приложения сообщения произойдет следующий процесс:

[![Обработка активного всплывающего сообщения](advanced-message-app-extensions-images/interactive11.png)](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. Пользователь касания всплывающего сообщения расширения.
2. Так как расширение приложения сообщений уже активно, `WillTransition` метод метода `MSMessagesAppViewController` вызывается для обработки переключения из сжатия в расширенный режим просмотра.
3. `DidSelectMessage`Метод `MSMessagesAppViewController` вызывается и передает объект `MSMessage` и `MSConversation` , которому принадлежит пузырьковое сообщение.
4. `DidTransition`Метод `MSMessagesAppViewController` вызывается для управления переключением с Compact на расширенный режим просмотра.

Опять же, после завершения процесса расширение приложения сообщения будет представлено в расширенном режиме просмотра.

### <a name="accessing-the-selected-message"></a>Доступ к выбранному сообщению

В любом случае, когда пользователь касается пузырька сообщений, принадлежащего расширению приложения сообщений, ему необходимо получить доступ к элементу `MSMessage` , который был нажат с помощью `SelectedMessage` свойства `MSConversation` .

Например.

```csharp
using System;
using Foundation;
using Messages;
using UIKit;

namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        ...

        #region Override Methods
        public override void DidSelectMessage (MSMessage message, MSConversation conversation)
        {
            base.DidSelectMessage (message, conversation);

            // Get selected message
            var selected = conversation.SelectedMessage;

            // Present the user interface to continue editing
            // the conversation
            ...
        }
        #endregion
    }
}
```

Выбранное сообщение должно отображаться в пользовательском интерфейсе расширения приложения для сообщений, и пользователю должно быть разрешено составление ответа.

## <a name="removing-partially-completed-messages"></a>Удаление частично завершенных сообщений

В процессе отправки различных шагов интерактивного диалога между двумя пользователями в диалоге, частично завершенное пузырьковое сообщение может начать рассылку сообщения:

[![Частично завершенные пузырьки сообщений могут перегружать сообщения](advanced-message-app-extensions-images/interactive12.png)](advanced-message-app-extensions-images/interactive12.png#lightbox)

Вместо этого расширение приложения сообщения должно сворачивать предыдущее сообщение в кратком комментарии в записи сообщения:

[![Сворачивание поверх предыдущего сообщения в записи сообщения](advanced-message-app-extensions-images/interactive13.png)](advanced-message-app-extensions-images/interactive13.png#lightbox)

Это обрабатывается с помощью, `MSSession` чтобы свернуть все существующие шаги. Поэтому `DidSelectMessage` метод класса можно изменить так, `MSMessagesAppViewController` чтобы он выглядел следующим образом:

```csharp
public override void DidSelectMessage (MSMessage message, MSConversation conversation)
{
    base.DidSelectMessage (message, conversation);

    MSSession session;
    var summary = "";

    // Get selected message
    var selected = conversation.SelectedMessage;

    // Does the selected message already have a session?
    if (selected.Session == null) {
        // No, create a new session
        session = new MSSession ();
        summary = "Let's build an ice cream";
    } else {
        // Yes, use the existing session
        session = selected.Session;
        summary = "I added some scoops";
    }

    // Create an instance of the message being composed
    var existingMessage = new MSMessage (session);
    message.SummaryText = summary;
    ...

    // Present the user interface to continue editing
    // the conversation
    ...
}
```

Если выбранное сообщение уже имеет выход `MSSession` , используется другой объект `MSSession` . `SummaryText`Свойство объекта `MSMessage` используется для добавления заголовка к свернутым предыдущим шагам. Если `SummaryText` свойство имеет значение `null` , предыдущие шаги диалога будут полностью удалены из записи диалога.

## <a name="advanced-message-api-features"></a>Функции API расширенных сообщений

Благодаря основным функциям нового API сообщений, рассмотренным выше, ознакомьтесь с более сложными функциями, встроенными в платформу Apple.

Во первых, существует несколько других методов переопределения в `MSMessagesAppViewController` классе, обеспечивающих более глубокий доступ к диалогу.

- `DidStartSendingMessage`— Вызывается, когда пользователь направляет кнопку Send. Это не означает, что сообщение фактически было доставлено получателю, только что был запущен процесс отправки.
- `DidCancelSendingMessage`Это происходит, когда пользователь нажмет кнопку *X* в правом верхнем углу всплывающего сообщения в записи диалога.
- `DidReceiveMessage`— Этот метод вызывается, когда расширение приложения сообщения активно, получено новое сообщение от одного из участников диалога.

### <a name="group-conversations"></a>Групповые беседы

Расширение приложения для сообщений можно использовать, когда пользователи вовлечены в групповые беседы (с тремя или более людьми), и это необходимо учитывать при проектировании и реализации расширения приложения для обработки сообщений.

Взгляните на следующее взаимодействие в групповой беседе с тремя пользователями:

[![Взаимодействие группового общения с тремя пользователями](advanced-message-app-extensions-images/interactive14.png)](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. Пользователь 1 отправляет интерактивное сообщение группы с запросом пользователя 2 и пользователя 3 для выбора заказать гамбургер.
2. Пользователь 2 выбирает Tomatoes.
3. Пользователь 3 выбирает поля выбора.
4. Выбор пользователя 2 и пользователя 3 приступает к пользователю 1 почти в то же время. В результате выбранный пользователь 2 сворачивается в сводную строку и становится недоступным. Это может быть также отражено, когда отображается выбранный пользователем пользователь и свернутый пользователь 3.

В любом случае это поведение нежелательно, так как пользователь 1 должен иметь доступ к выбору пользователя 2 и пользователя 3. Чтобы справиться с этой ситуацией, компания Apple предложит, что расширение приложения для сообщений сохраняет состояние сообщения в облаке, и использует свойство URL-адреса объекта `MSMessage` (который отправляется между пользователями) для доступа к этому состоянию.

Когда пользователь отправляет сообщение, создается маркер сеанса, который передается в облако с текущим состоянием сообщения. Когда пользователь отменяет всплывающее сообщение в записи диалога, маркер сеанса используется для получения текущего состояния сеанса из облака.

### <a name="sender-identifiers"></a>Идентификаторы отправителя

Чтобы обсудить доступ к идентификатору отправителя сообщения, возьмем пример диалога группы, приведенный выше:

[![Групповые диалоги, отправляющие идентификаторы](advanced-message-app-extensions-images/interactive15.png)](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. Опять же, пользователь 1 отправляет интерактивное сообщение группы с запросом пользователя 2 и пользователя 3 для выбора заказать гамбургер.
2. Пользователь 3 выбирает поля выбора.
3. Выбранный пользователь 3 приступает к пользователю 1, а пользователь 2 еще не ответил на него.
4. Поскольку Apple имеет дело с конфиденциальностью пользователей, расширение приложения сообщения знает только о уникальном идентификаторе (как `NSUUID` ), которому назначен каждый участник диалога. На локальном устройстве известен только идентификатор текущего пользователя.
5. `MSMessage`Имеет `SenderIdentifier` свойство, совпадающее с одним из пользователей в списке участников, известном для расширения.
6. Каждый пользовательское устройство имеет собственную копию списка участников, где также известен только идентификатор локального пользователя.
7. При отправке сообщения его `SenderIdentifier` свойство также называется локальным пользователем.

Идентификаторы отправителя можно использовать следующими способами.

- Просмотрев список участников, расширение может получить количество пользователей в диалоге.
- Когда расширение получает сообщение от пользователя, оно может вести отслеживание идентификатора отправителя. Если он получает другое сообщение с тем же идентификатором отправителя, расширение знает, что он относится к тому же пользователю.
- Они могут использоваться для выявления конкретного пользователя в диалоге.

Идентификатор отправителя можно использовать в любом текстовом поле объекта, `MSMessageTemplateLayout` добавив префикс доллара ( `$` ). Например.

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

Когда приложение messages отображает всплывающее сообщение с таким типом форматирования, оно заменит в `$uuid...` диалоговом окне диалога, отправившего сообщение, имя контакта участника.

Идентификатор отправителя уникален на каждом устройстве, поэтому снова взгляните на схему выше. Обратите внимание, что устройство пользователя 1 и устройство пользователя 3 имеют разные уникальные идентификаторы отправителя для каждого участника диалога.

Идентификаторы отправителя ограничиваются установкой расширения приложения для сообщений. Поэтому, если пользователь удаляет и переустанавливает расширение приложения сообщения, для новой установки будут созданы новые идентификаторы отправителя.

Для доступа к идентификаторам отправителя расширение может использовать следующий код:

```csharp
public override void DidStartSendingMessage (MSMessage message, MSConversation conversation)
{
    base.DidStartSendingMessage (message, conversation);

    // Get the sender's participant ID
    var senderID = message.SenderParticipantIdentifier;

    // Get the local participant ID
    var localID = conversation.LocalParticipantIdentifier;

    // Get the remote participant IDs
    var remoteIDs = conversation.RemoteParticipantIdentifiers;
}
```

## <a name="supported-platforms"></a>Поддерживаемые платформы

Интерактивные сообщения, создаваемые расширением приложения для сообщений, будут доставляться на следующих платформах Apple:

- watchOS 3
- macOS Sierra.
- iOS 10

Из трех платформ только iOS 10 позволит пользователю создавать интерактивное сообщение. На macOS Sierra, если пользователь щелкнет интерактивное всплывающее сообщение, URL-адрес, прикрепленный к, `MSMessage` будет открыт в Safari, и в нем должно отобразиться представление сообщения.

В watchOS приложение messages может переложить интерактивное сообщение на подключенное устройство iOS, где пользователь может составить ответ.

API новых сообщений поддерживает откат, если на старых платформах Apple получено интерактивное сообщение:

- watchOS 2 +
- OS X 10,11 +
- iOS 9 +

Они будут доставляться в резервном формате в виде двух отдельных сообщений:

- Один из них будет изображением, предоставленным макетом шаблона.
- Другой будет URL-адрес, указанный в `MSMessage` .

## <a name="summary"></a>Сводка

В этой статье представлены расширенные методы работы с расширениями приложений сообщений в решении Xamarin. iOS, которое интегрируется с приложением **сообщений** и предоставляет пользователю новые функции.

## <a name="related-links"></a>Связанные ссылки

- [Построитель немороженных (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder)
- [Справочник по сообщениям](https://developer.apple.com/reference/messages)
- [Инструкции по программированию расширения приложения](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
