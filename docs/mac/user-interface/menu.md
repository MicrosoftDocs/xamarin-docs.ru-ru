---
title: Меню в Xamarin. Mac
description: В этой статье рассматривается работа с меню в приложении Xamarin. Mac. Здесь описывается создание и обслуживание меню и пунктов меню в Xcode и Interface Builder и работа с ними программными средствами.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 2a7a09b1dec840090587377199533ef8c6ec63b5
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573173"
---
# <a name="menus-in-xamarinmac"></a>Меню в Xamarin. Mac

_В этой статье рассматривается работа с меню в приложении Xamarin. Mac. Здесь описывается создание и обслуживание меню и пунктов меню в Xcode и Interface Builder и работа с ними программными средствами._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же меню Cocoa, что и разработчик, работающий на уровне цели-C и Xcode. Поскольку Xamarin. Mac интегрируется непосредственно с Xcode, можно использовать Interface Builder Xcode для создания и обслуживания строк меню, меню и пунктов меню (или при необходимости создавать их непосредственно в коде C#).

Меню являются неотъемлемой частью пользовательского интерфейса приложения Mac и обычно отображаются в различных частях интерфейсного пользователя:

- **Строка меню приложения** — это главное меню, которое отображается в верхней части экрана для каждого приложения Mac.
- **Контекстные меню** — отображаются, когда пользователь щелкает правой кнопкой мыши элемент в окне.
- **Строка состояния** — это область в правой части строки меню приложения, отображаемая в верхней части экрана (слева от пункта меню часы), и расширяется влево по мере добавления элементов к ним.
- **Меню закрепления** — меню для каждого приложения в закрепления, которое появляется, когда пользователь щелкает правой кнопкой мыши или щелкает значок приложения, или когда пользователь щелкает значок, и удерживает кнопку мыши.
- **Всплывающая кнопка и раскрывающиеся списки** — всплывающая кнопка отображает выбранный элемент и предоставляет список параметров, которые нужно выбрать по щелчку пользователя. Раскрывающийся список — это тип всплывающей кнопки, обычно используемой для выбора команд, относящихся к контексту текущей задачи. Оба могут находиться в любом месте окна.

[![Пример меню](menu-images/intro01.png "Пример меню")](menu-images/intro01-large.png#lightbox)

В этой статье рассматриваются основы работы с Cocoa меню, меню и элементами меню в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в этой статье.

Возможно, вы захотите ознакомиться с [предоставлением классов и методов C# для цели-c](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) . здесь также объясняются `Register` атрибуты и, `Export` используемые для подключения классов c# к объектам и элементам пользовательского интерфейса на языке c.

## <a name="the-applications-menu-bar"></a>Строка меню приложения 

В отличие от приложений, работающих в ОС Windows, где каждое окно может иметь собственную строку меню, каждое приложение, работающее в macOS, имеет одну строку меню, которая работает в верхней части экрана, используемой для каждого окна в этом приложении:

[![Строка меню](menu-images/appmenu01.png "Строка меню")](menu-images/appmenu01-large.png#lightbox)

Элементы в этой строке меню активируются или деактивируются на основе текущего контекста или состояния приложения и его пользовательского интерфейса в любой момент. Например: Если пользователь выбирает текстовое поле, элементы в меню **Правка** будут включены, например **Копировать** и **Вырезать**.

В соответствии с Apple и по умолчанию все приложения macOS имеют стандартный набор меню и пунктов меню, которые отображаются в строке меню приложения:

- **Меню Apple** . Это меню предоставляет доступ к элементам всей системы, доступным для пользователя, независимо от того, какое приложение выполняется. Разработчик не может изменить эти элементы.
- **Меню "приложение** " — это меню отображает имя приложения полужирным шрифтом и помогает пользователю определить, какое приложение выполняется в данный момент. Он содержит элементы, которые применяются к приложению как к целому, а не к определенному документу или процессу, например выход из приложения.
- **Меню "файл** " — элементы, используемые для создания, открытия и сохранения документов, с которыми работает приложение. Если приложение не основано на документе, это меню можно переименовать или удалить.
- **Меню «Правка** » — содержит такие команды, как **вырезание**, **копирование**и **Вставка** , которые используются для изменения или изменения элементов пользовательского интерфейса приложения.
- **Меню "формат** " — Если приложение работает с текстом, в этом меню содержатся команды для настройки форматирования этого текста.
- **Меню "вид** " — содержит команды, влияющие на отображение (Просмотр) содержимого в пользовательском интерфейсе приложения.
- **Меню для конкретных приложений** — это все меню, характерные для вашего приложения (например, меню закладок для веб-браузера). Они должны отображаться между меню **вид** и **окно** на панели.
- **Меню "окно** " — содержит команды для работы с окнами в приложении, а также список текущих открытых окон.
- **Меню "Справка** ". Если приложение предоставляет справку на экране, меню "Справка" должно быть самым подменю на панели. 

Дополнительные сведения о строке меню приложения и стандартных меню и пунктах меню см. в разделе [рекомендации по использованию человеческого интерфейса](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)Apple.

### <a name="the-default-application-menu-bar"></a>Строка меню приложения по умолчанию

При создании нового проекта Xamarin. Mac вы автоматически получаете стандартную строку меню приложения по умолчанию с типичными элементами, которые обычно macOS в приложении (как описано в разделе выше). Строка меню по умолчанию приложения определена в файле **Main. Storyboard** (вместе с остальной частью пользовательского интерфейса приложения) в проекте на **панель решения**:  

![Выбор основной раскадровки](menu-images/appmenu02.png "Выбор основной раскадровки")

Дважды щелкните файл **Main. Storyboard** , чтобы открыть его для редактирования в Interface Builder Xcode и вы увидите интерфейс редактора меню:

[![Изменение пользовательского интерфейса в Xcode](menu-images/defaultbar01.png "Изменение пользовательского интерфейса в Xcode")](menu-images/defaultbar01-large.png#lightbox)

Здесь можно щелкнуть элемент меню **Открыть** в меню **файл** и изменить или изменить его свойства в **инспекторе атрибутов**:

[![Изменение атрибутов меню](menu-images/defaultbar02.png "Изменение атрибутов меню")](menu-images/defaultbar02-large.png#lightbox)

Далее в этой статье мы будем добавлять, изменять и удалять меню и элементы. Сейчас мы хотим узнать, какие меню и пункты меню доступны по умолчанию и как они были автоматически предоставлены коду с помощью набора предопределенных возможностей и действий (Дополнительные сведения см. в документации по процедурам [и действиям](~/mac/get-started/hello-mac.md#outlets-and-actions) ).

Например, если щелкнуть **инспектор подключений** для пункта меню **Открыть** , он будет автоматически привязан к `openDocument:` действию: 

[![Просмотр присоединенного действия](menu-images/defaultbar03.png "Просмотр присоединенного действия")](menu-images/defaultbar03-large.png#lightbox)

Если выбрать **первый ответчик** в **иерархии интерфейсов** и прокрутить вниз в **инспекторе подключений**, вы увидите определение `openDocument:` действия, к которому присоединен пункт меню **Открыть** (вместе с несколькими другими действиями по умолчанию для приложения, которые не будут автоматически привязаны к элементам управления):

[![Просмотр всех вложенных действий](menu-images/defaultbar04.png "Просмотр всех вложенных действий")](menu-images/defaultbar04-large.png#lightbox) 

Почему это важно? В следующем разделе будет показано, как эти автоматически определенные действия работают с другими элементами пользовательского интерфейса Cocoa, чтобы автоматически включать и отключать пункты меню, а также предоставлять встроенные функции для элементов.

Позже мы будем использовать эти встроенные действия для включения и отключения элементов из кода и предоставления собственных функций при их выборе.

<a name="Built-In_Menu_Functionality"></a>

### <a name="built-in-menu-functionality"></a>Встроенные функции меню

Если вы использовали только что созданное приложение Xamarin. Mac перед добавлением каких-либо элементов пользовательского интерфейса или кода, вы заметите, что некоторые элементы автоматически подсоединены и включены (с полной функциональностью, встроенной автоматически), например " **Quit** " в меню " **приложение** ":

![Включенный пункт меню](menu-images/appmenu03.png "Включенный пункт меню")

Хотя другие пункты меню, такие как **вырезание**, **копирование**и **Вставка** , не выполняются:

![Отключенные пункты меню](menu-images/appmenu04.png "Отключенные пункты меню")

Давайте откроем приложение и дважды щелкните файл **Main. Storyboard** в **панель решения** , чтобы открыть его для редактирования в Interface Builder Xcode. Затем перетащите **текстовое представление** из **библиотеки** на контроллер представления окна в **редакторе интерфейса**:

[![Выбор текстового представления из библиотеки](menu-images/appmenu05.png "Выбор текстового представления из библиотеки")](menu-images/appmenu05-large.png#lightbox)

В **редакторе ограничений** Прикрепите текстовое представление к краям окна и задайте его там, где оно растет и сжимается, щелкнув все четыре красной кнопки I-беамс в верхней части редактора и нажав кнопку **Добавить 4 ограничения** :

[![Изменение очередностью](menu-images/appmenu06.png "Изменение очередностью")](menu-images/appmenu06-large.png#lightbox)

Сохраните изменения в структуре пользовательского интерфейса и переключитесь обратно на Visual Studio для Mac, чтобы синхронизировать изменения с проектом Xamarin. Mac. Теперь запустите приложение, введите некоторый текст в текстовое представление, выберите его и откройте меню **Правка** :

![Элементы меню автоматически включаются и отключаются.](menu-images/appmenu07.png "Элементы меню автоматически включаются и отключаются.")

Обратите внимание, что элементы " **Вырезать**", " **Копировать**" и " **Вставить** " автоматически включены и полностью работают, без написания одной строки кода. 

В чем причина? Помните о встроенных предопределенных действиях, которые связаны с элементами меню по умолчанию (как было показано выше), большинство элементов пользовательского интерфейса Cocoa, которые являются частью macOS, имеют встроенные обработчики для определенных действий (например, `copy:` ). Таким образом, когда они добавляются в окно, активно и выбрано, соответствующий элемент меню или элементы, присоединенные к этому действию, автоматически включаются. Если пользователь выбирает этот пункт меню, функциональные возможности, встроенные в элемент пользовательского интерфейса, вызываются и выполняются, все без участия разработчика.

### <a name="enabling-and-disabling-menus-and-items"></a>Включение и отключение меню и элементов

По умолчанию каждый раз, когда происходит событие пользователя, `NSMenu` автоматически включает и отключает каждое видимое меню и пункт меню на основе контекста приложения. Включить или отключить элемент можно тремя способами:

- **Автоматическое включение меню** — пункт меню включается, если `NSMenu` может найти подходящий объект, отвечающий на действие, к которому привязан элемент. Например, представленное выше текстовое представление содержит встроенное подключение к `copy:` действию.
- **Настраиваемые действия и валидатеменуитем:** — для любого элемента меню, привязанного к [окну или настраиваемому действию контроллера представлений](#Working-with-Custom-Window-Actions), можно добавить `validateMenuItem:` действие и вручную включить или отключить пункты меню.
- **Включение меню вручную** `Enabled` `NSMenuItem` . для включения или отключения каждого элемента в меню вручную задайте для свойства значение.

Чтобы выбрать систему, задайте `AutoEnablesItems` свойство объекта `NSMenu` . `true`является автоматическим (поведение по умолчанию) и `false` выполняется вручную. 

> [!IMPORTANT]
> Если вы решили использовать ручное меню, ни один из пунктов меню, даже не управляемые классами AppKit, как `NSTextView` , обновляются автоматически. Вы несете ответственность за включение и отключение всех элементов вручную в коде.

#### <a name="using-validatemenuitem"></a>Использование Валидатеменуитем

Как упоминалось выше, для любого элемента меню, привязанного к [настраиваемому действию контроллера окна или представления](#Working-with-Custom-Window-Actions), можно добавить `validateMenuItem:` действие и вручную включить или отключить пункты меню.

В следующем примере `Tag` свойство будет использоваться для выбора типа элемента меню, который будет включен или отключен `validateMenuItem:` действием в зависимости от состояния выбранного текста в `NSTextView` . `Tag`Свойство было задано в Interface Builder для каждого пункта меню:

![Установка свойства Tag](menu-images/validate01.png "Установка свойства Tag")

И следующий код добавлен в контроллер представления:

```csharp
[Action("validateMenuItem:")]
public bool ValidateMenuItem (NSMenuItem item) {

    // Take action based on the menu item type
    // (As specified in its Tag)
    switch (item.Tag) {
    case 1:
        // Wrap menu items should only be available if
        // a range of text is selected
        return (TextEditor.SelectedRange.Length > 0);
    case 2:
        // Quote menu items should only be available if
        // a range is NOT selected.
        return (TextEditor.SelectedRange.Length == 0);
    }

    return true;
}
```

При выполнении этого кода и отсутствии выделенного текста в `NSTextView` два элемента меню "оболочка" отключаются (даже если они связаны с действиями на контроллере представления):

![Отображение отключенных элементов](menu-images/validate02.png "Отображение отключенных элементов")

Если раздел текста выбран и меню открывается снова, то будут доступны два элемента меню "заключение":

![Отображение включенных элементов](menu-images/validate03.png "Отображение включенных элементов")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>Включение и реагирование на пункты меню в коде

Как мы видели выше, просто добавляя определенные элементы пользовательского интерфейса Cocoa в нашу структуру пользовательского интерфейса (например, текстовое поле), некоторые элементы меню по умолчанию будут включены и работать автоматически, не требуя написания кода. Теперь рассмотрим добавление собственного кода C# в проект Xamarin. Mac для включения пункта меню и предоставления функциональных возможностей, когда пользователь выбирает его.

Например, пусть мы хотим, чтобы пользователь мог использовать элемент **Открыть** в меню **файл** для выбора папки. Так как мы хотим, чтобы это была функция на уровне приложения и не ограничена окном "предоставление" или элементом пользовательского интерфейса, мы добавим код для его решения в делегате приложения.

В **панель решения**дважды щелкните `AppDelegate.CS` файл, чтобы открыть его для редактирования:

![Выбор делегата приложения](menu-images/appmenu08.png "Выбор делегата приложения")

Добавьте следующий код под `DidFinishLaunching` методом:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

Теперь выполним приложение и откроем меню **файл** : 

![Меню «файл»](menu-images/appmenu09.png "Меню «файл»")

Обратите внимание, что теперь пункт **Открыть** меню включен. Если выбрать его, откроется диалоговое окно Открыть:

![Открытое диалоговое окно](menu-images/appmenu10.png "Открытое диалоговое окно")

Если нажать кнопку " **Открыть** ", отобразится сообщение с предупреждением:

![Пример сообщения диалогового окна](menu-images/appmenu11.png "Пример сообщения диалогового окна")

Вот ключевая строка `[Export ("openDocument:")]` , которая говорит, `NSMenu` что наш **AppDelegate** имеет метод `void OpenDialog (NSObject sender)` , отвечающий на `openDocument:` действие. Если вы запомним выше, пункт меню **Открыть** автоматически поддается этому действию по умолчанию в Interface Builder:

[![Просмотр вложенных действий](menu-images/defaultbar03.png "Просмотр вложенных действий")](menu-images/defaultbar03-large.png#lightbox)

Теперь рассмотрим создание собственного меню, пунктов меню и действий и реагирование на них в коде.

### <a name="working-with-the-open-recent-menu"></a>Работа с меню "открыть недавно"

По умолчанию меню **файл** содержит **открытый** элемент, который отслеживает последние несколько файлов, открытых пользователем в приложении. При создании `NSDocument` на основе приложения Xamarin. Mac это меню будет автоматически обработано. Для любого другого типа приложения Xamarin. Mac вы будете отвечать за управление этим элементом меню и реагирование на него вручную.

Чтобы вручную обрабатывали меню **Открыть недавно** , сначала необходимо сообщить о том, что новый файл открыт или сохранен с помощью следующей команды:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

Несмотря на то, что приложение не используется `NSDocuments` , вы по-прежнему используете `NSDocumentController` для поддержания меню **Открыть последние** , отправляя в `NSUrl` метод компонента объект с расположением файла `NoteNewRecentDocumentURL` `SharedDocumentController` .

Далее необходимо переопределить `OpenFile` метод делегата приложения, чтобы открыть любой файл, выбираемый пользователем из меню **Открыть последние** . Пример.

```csharp
public override bool OpenFile (NSApplication sender, string filename)
{
    // Trap all errors
    try {
        filename = filename.Replace (" ", "%20");
        var url = new NSUrl ("file://"+filename);
        return OpenFile(url);
    } catch {
        return false;
    }
}
```

`true`Если файл можно открыть, возвращается значение, `false` а для пользователя отображается встроенное предупреждение о том, что не удалось открыть файл.

Так как имя файла и путь, возвращаемые из меню **Открыть последние** , могут содержать пробелы, необходимо надлежащим образом отэкранированить этот символ перед созданием, иначе возникнет `NSUrl` ошибка. Мы делаем это с помощью следующего кода:

```csharp
filename = filename.Replace (" ", "%20");
```

Наконец, мы создаем объект `NSUrl` , указывающий на файл, и используем вспомогательный метод в делегате приложения, чтобы открыть новое окно и загрузить в него файл:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Чтобы извлечь все вместе, давайте рассмотрим пример реализации в файле **AppDelegate.CS** :

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace MacHyperlink
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }

        public override bool OpenFile (NSApplication sender, string filename)
        {
            // Trap all errors
            try {
                filename = filename.Replace (" ", "%20");
                var url = new NSUrl ("file://"+filename);
                return OpenFile(url);
            } catch {
                return false;
            }
        }
        #endregion

        #region Private Methods
        private bool OpenFile(NSUrl url) {
            var good = false;

            // Trap all errors
            try {
                var path = url.Path;

                // Is the file already open?
                for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
                    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
                    if (content != null && path == content.FilePath) {
                        // Bring window to front
                        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
                        return true;
                    }
                }

                // Get new window
                var storyboard = NSStoryboard.FromName ("Main", null);
                var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

                // Display
                controller.ShowWindow(this);

                // Load the text into the window
                var viewController = controller.Window.ContentViewController as ViewController;
                viewController.Text = File.ReadAllText(path);
                viewController.SetLanguageFromPath(path);
                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                viewController.View.Window.RepresentedUrl = url;

                // Add document to the Open Recent menu
                NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);

                // Make as successful
                good = true;
            } catch {
                // Mark as bad file on error
                good = false;
            }

            // Return results
            return good;
        }
        #endregion

        #region actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = true;
            dlg.CanChooseDirectories = false;

            if (dlg.RunModal () == 1) {
                // Nab the first file
                var url = dlg.Urls [0];

                if (url != null) {
                    // Open the document in a new window
                    OpenFile (url);
                }
            }
        }
        #endregion
    }
}
```

В зависимости от требований приложения пользователю может быть нежелательно открывать один и тот же файл одновременно в нескольких окнах. В нашем примере приложения, если пользователь выбирает уже открытый файл (в **открытом** или **открытом** виде). пункты меню), окно, содержащее файл, переносится на передний план.

Для этого мы использовали следующий код в нашем вспомогательном методе:

```csharp
var path = url.Path;

// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

Мы разработали наш `ViewController` класс для хранения пути к файлу в его `Path` свойстве. Далее мы выполним циклический перебор всех открытых окон в приложении. Если файл уже открыт в одном из окон, он переносится на передний план всех остальных окон с помощью:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Если совпадений не найдено, открывается новое окно с загруженным файлом, и файл отмечается в меню **Открыть недавно** :

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);

// Load the text into the window
var viewController = controller.Window.ContentViewController as ViewController;
viewController.Text = File.ReadAllText(path);
viewController.SetLanguageFromPath(path);
viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
viewController.View.Window.RepresentedUrl = url;

// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

<a name="Working-with-Custom-Window-Actions"></a>

### <a name="working-with-custom-window-actions"></a>Работа с действиями настраиваемого окна

Точно так же, как встроенные действия **первого респондента** , которые поставляются до стандартных пунктов меню, можно создавать новые настраиваемые действия и связывать их с элементами меню в Interface Builder.

Сначала определите настраиваемое действие на одном из контроллеров окна приложения. Пример.

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Затем дважды щелкните файл раскадровки приложения в **панель решения** , чтобы открыть его для редактирования в Interface Builder Xcode. Выберите **первый респондент** в **сцене приложения**, а затем перейдите в **инспектор атрибутов**:

![Инспектор атрибутов](menu-images/action01.png "Инспектор атрибутов")

Нажмите кнопку **+** в нижней части **инспектора атрибутов** , чтобы добавить новое настраиваемое действие:

![Добавление нового действия](menu-images/action02.png "Добавление нового действия")

Присвойте ему имя, совпадающее с именем настраиваемого действия, созданного на контроллере окна.

![Изменение имени действия](menu-images/action03.png "Изменение имени действия")

Щелчок и перетаскивание пункта меню на **первый респондент** в **сцене приложения**. Из раскрывающегося списка выберите только что созданное действие ( `defineKeyword:` в этом примере):

![Присоединение действия](menu-images/action04.png "Присоединение действия")

Сохраните изменения в раскадровке и вернитесь в Visual Studio для Mac, чтобы синхронизировать изменения. Если запустить приложение, элемент меню, в который вы подключили настраиваемое действие, будет автоматически включен или отключен (на основе окна с открытым действием), и выбор пункта меню приведет к срабатыванию действия:

[![Тестирование нового действия](menu-images/action05.png "Тестирование нового действия")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus"></a>

### <a name="adding-editing-and-deleting-menus"></a>Добавление, изменение и удаление меню

Как мы видели в предыдущих разделах, приложение Xamarin. Mac поставляется с предустановленным количеством меню и пунктов меню по умолчанию, которые будут автоматически активированы и реагировать на эти элементы управления пользовательского интерфейса. Мы также рассмотрели, как добавить код в приложение, которое также будет включать и отвечать на эти элементы по умолчанию.

В этом разделе мы рассмотрим удаление пунктов меню, которые нам не нужны, реорганизация меню и добавление новых меню, пунктов меню и действий.

Дважды щелкните файл **Main. Storyboard** в **панель решения** , чтобы открыть его для редактирования:

[![Изменение пользовательского интерфейса в Xcode](menu-images/maint01.png "Изменение пользовательского интерфейса в Xcode")](menu-images/maint01-large.png#lightbox)

Для нашего конкретного приложения Xamarin. Mac мы не будем использовать меню « **вид** по умолчанию», поэтому мы будем его удалить. В **иерархии интерфейс** выберите пункт меню **вид** , который является частью главной строки меню:

![Выбор пункта меню «Вид»](menu-images/maint02.png "Выбор пункта меню «Вид»")

Нажмите клавишу DELETE или BACKSPACE, чтобы удалить меню. Далее мы не будем использовать все элементы в меню **Формат** , и мы хотим переместить элементы, которые мы будем использовать, из вложенных меню. В **иерархии интерфейс** выберите следующие пункты меню:

![Выделение нескольких элементов](menu-images/maint03.png "Выделение нескольких элементов")

Перетащите элементы из родительского **меню** в подменю, в котором они сейчас находятся:

[![Перетаскивание пунктов меню в родительское меню](menu-images/maint04.png "Перетаскивание пунктов меню в родительское меню")](menu-images/maint04-large.png#lightbox)

Теперь меню должно выглядеть следующим образом:

[![Элементы в новом расположении](menu-images/maint05.png "Элементы в новом расположении")](menu-images/maint05-large.png#lightbox)

Теперь можно перетащить подменю **текст** из меню **Формат** и поместить его в главное меню в меню **Формат** и **окно** :

[![Текстовое меню](menu-images/maint06.png "Текстовое меню")](menu-images/maint06-large.png#lightbox)

Вернемся к меню **Формат** и удалим элемент вложенного меню **Шрифт** . Затем выберите меню **Формат** и переименуйте его "Шрифт":

[![Меню «Шрифт»](menu-images/maint07.png "Меню «Шрифт»")](menu-images/maint07-large.png#lightbox)

Теперь создадим настраиваемое меню предопределенных фраз, которое автоматически добавляется к тексту в представлении текста при их выборе. В поле поиска в нижней части окна **инспектора библиотеки** введите в меню. Это упростит поиск и работу со всеми элементами пользовательского интерфейса меню:

![Инспектор библиотек](menu-images/maint08.png "Инспектор библиотек")

Теперь выполним следующие действия, чтобы создать наше меню:

1. Перетащите **пункт меню** из **инспектора библиотеки** в строку меню между меню " **текст** " и " **окно** ": 

    ![Выбор нового пункта меню в библиотеке](menu-images/maint10.png "Выбор нового пункта меню в библиотеке")
2. Переименуйте элемент "фразы": 

    [![Задание имени меню](menu-images/maint09.png "Задание имени меню")](menu-images/maint09-large.png#lightbox)
3. Затем перетащите **меню** из **инспектора библиотеки**: 

    ![Выбор меню из библиотеки](menu-images/maint11.png "Выбор меню из библиотеки")
4. Перетащите **меню** в созданный **элемент меню** и измените его имя на "фразы": 

    [![Изменение имени меню](menu-images/maint12.png "Изменение имени меню")](menu-images/maint12-large.png#lightbox)
5. Теперь переименуем три **элемента меню** "адрес", "Дата" и "приветствие": 

    [![Меню фраз](menu-images/maint13.png "Меню фраз")](menu-images/maint13-large.png#lightbox)
6. Добавим четвертый пункт **меню** , перетащив **пункт меню** из **инспектора библиотеки** и вызвав его «Signature»: 

    [![Изменение имени пункта меню](menu-images/maint14.png "Изменение имени пункта меню")](menu-images/maint14-large.png#lightbox)
7. Сохраните изменения в строке меню.

Теперь создадим набор настраиваемых действий, чтобы наши новые элементы меню были доступны коду C#. В Xcode давайте перейдем к представлению **помощника** :

[![Создание необходимых действий](menu-images/maint15.png "Создание необходимых действий")](menu-images/maint15-large.png#lightbox)

Давайте выполним следующие действия.

1. Перетащите элемент управления из пункта меню **адрес** в файл **AppDelegate. h** .
2. Переключите тип **подключения** на **действие**: 

    [![Выбор типа действия](menu-images/maint17.png "Выбор типа действия")](menu-images/maint17-large.png#lightbox)
3. Введите **имя** "фрасеаддресс" и нажмите кнопку " **подключить** ", чтобы создать новое действие: 

    [![Настройка действия](menu-images/maint18.png "Настройка действия")](menu-images/maint18-large.png#lightbox)
4. Повторите описанные выше шаги для пунктов меню **Дата**, **Приветствие**и **подпись** . 

    [![Завершенные действия](menu-images/maint19.png "Завершенные действия")](menu-images/maint19-large.png#lightbox)
5. Сохраните изменения в строке меню.

Далее нам нужно создать выход для нашего текстового представления, чтобы мы могли настроить его содержимое из кода. Выберите файл **ViewController. h** в **редакторе помощника** и создайте новую розетку с именем `documentText` :

[![Создание розетки](menu-images/maint20.png "Создание розетки")](menu-images/maint20-large.png#lightbox)

Вернитесь к Visual Studio для Mac, чтобы синхронизировать изменения из Xcode. Затем измените файл **ViewController.CS** и сделайте его следующим:

```csharp
using System;

using AppKit;
using Foundation;

namespace MacMenus
{
    public partial class ViewController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }

        public string Text {
            get { return documentText.Value; }
            set { documentText.Value = value; }
        } 
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            App.textEditor = this;
        }

        public override void ViewWillDisappear ()
        {
            base.ViewDidDisappear ();

            App.textEditor = null;
        }
        #endregion
    }
}
```

Это предоставляет текст текстового представления за пределами `ViewController` класса и информирует делегат приложения о том, что окно получает или теряет фокус. Теперь измените файл **AppDelegate.CS** и сделайте его следующим:

```csharp
using AppKit;
using Foundation;
using System;

namespace MacMenus
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public ViewController textEditor { get; set;} = null;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = false;
            dlg.CanChooseDirectories = true;

            if (dlg.RunModal () == 1) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
                    MessageText = "Folder Selected"
                };
                alert.RunModal ();
            }
        }

        partial void phrasesAddress (Foundation.NSObject sender) {

            textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
        }

        partial void phrasesDate (Foundation.NSObject sender) {

            textEditor.Text += DateTime.Now.ToString("D");
        }

        partial void phrasesGreeting (Foundation.NSObject sender) {

            textEditor.Text += "Dear Sirs,\n\n";
        }

        partial void phrasesSignature (Foundation.NSObject sender) {

            textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
        }
        #endregion
    }
}
```

Здесь мы сделали `AppDelegate` разделяемый класс, чтобы мы могли использовать действия и возможности, определенные в Interface Builder. Мы также предоставляем, `textEditor` чтобы отвести трассировку о том, какое окно сейчас находится в фокусе.

Для работы с пользовательскими меню и элементами меню используются следующие методы:

```csharp
partial void phrasesAddress (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
}

partial void phrasesDate (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += DateTime.Now.ToString("D");
}

partial void phrasesGreeting (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Dear Sirs,\n\n";
}

partial void phrasesSignature (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
}
```

Теперь, если мы выполним приложение, все элементы в меню « **фраза** » будут активными и будут добавлять фразу «присвоить» к представлению текста, если они выбраны:

![Пример выполняемого приложения](menu-images/maint21.png "Пример выполняемого приложения")

Теперь, когда у нас есть основы работы с строкой меню приложения, давайте взглянем на создание настраиваемого контекстного меню.

### <a name="creating-menus-from-code"></a>Создание меню из кода

Помимо создания меню и пунктов меню с Interface Builder Xcode, могут возникнуть ситуации, когда приложению Xamarin. Mac нужно создать, изменить или удалить меню, подменю или пункт меню из кода.

В следующем примере создается класс для хранения сведений о пунктах меню и подменю, которые будут динамически создаваться в режиме реального времени:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace AppKit.TextKit.Formatter
{
    public class LanguageFormatCommand : NSObject
    {
        #region Computed Properties
        public string Title { get; set; } = "";
        public string Prefix { get; set; } = "";
        public string Postfix { get; set; } = "";
        public List<LanguageFormatCommand> SubCommands { get; set; } = new List<LanguageFormatCommand>();
        #endregion

        #region Constructors
        public LanguageFormatCommand () {

        }

        public LanguageFormatCommand (string title)
        {
            // Initialize
            this.Title = title;
        }

        public LanguageFormatCommand (string title, string prefix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
        }

        public LanguageFormatCommand (string title, string prefix, string postfix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
            this.Postfix = postfix;
        }
        #endregion
    }
}
```

#### <a name="adding-menus-and-items"></a>Добавление меню и элементов

Если этот класс определен, следующая подпрограммы будет анализировать коллекцию `LanguageFormatCommand` объектов и рекурсивно создавать новые меню и пункты меню, добавляя их в конец существующего меню (созданного в Interface Builder), которое было передано:

```csharp
private void AssembleMenu(NSMenu menu, List<LanguageFormatCommand> commands) {
    NSMenuItem menuItem;

    // Add any formatting commands to the Formatting menu
    foreach (LanguageFormatCommand command in commands) {
        // Add separator or item?
        if (command.Title == "") {
            menuItem = NSMenuItem.SeparatorItem;
        } else {
            menuItem = new NSMenuItem (command.Title);

            // Submenu?
            if (command.SubCommands.Count > 0) {
                // Yes, populate submenu
                menuItem.Submenu = new NSMenu (command.Title);
                AssembleMenu (menuItem.Submenu, command.SubCommands);
            } else {
                // No, add normal menu item
                menuItem.Activated += (sender, e) => {
                    // Apply the command on the selected text
                    TextEditor.PerformFormattingCommand (command);
                };
            }
        }
        menu.AddItem (menuItem);
    }
}
``` 

Для любого `LanguageFormatCommand` объекта, имеющего пустое `Title` свойство, эта подпрограммы создает **пункт меню разделителя** (тонкая серая линия) между разделами меню:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Если указано название, то создается новый элемент меню с таким названием:

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Если `LanguageFormatCommand` объект содержит дочерние `LanguageFormatCommand` объекты, то создается подменю, и `AssembleMenu` метод рекурсивно вызывается для создания этого меню:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Для любого нового пункта меню, не имеющего подменю, добавляется код для управления пунктом меню, выбранным пользователем:

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Тестирование создания меню

При наличии всего приведенного выше кода, если `LanguageFormatCommand` была создана следующая коллекция объектов:

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Strong","**","**"));
FormattingCommands.Add(new LanguageFormatCommand("Emphasize","_","_"));
FormattingCommands.Add(new LanguageFormatCommand("Inline Code","`","`"));
FormattingCommands.Add(new LanguageFormatCommand("Code Block","```\n","\n```"));
FormattingCommands.Add(new LanguageFormatCommand("Comment","<!--","-->"));
FormattingCommands.Add (new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Unordered List","* "));
FormattingCommands.Add(new LanguageFormatCommand("Ordered List","1. "));
FormattingCommands.Add(new LanguageFormatCommand("Block Quote","> "));
FormattingCommands.Add (new LanguageFormatCommand ());

var Headings = new LanguageFormatCommand ("Headings");
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 1","# "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 2","## "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 3","### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 4","#### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 5","##### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 6","###### "));
FormattingCommands.Add (Headings);

FormattingCommands.Add(new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Link","[","]()"));
FormattingCommands.Add(new LanguageFormatCommand("Image","![](",")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[![](",")](LinkImageHere)"));
```

И эта коллекция передается `AssembleMenu` функции (в меню **Формат** , установленном в качестве базового), будут созданы следующие динамические меню и пункты меню:

![Новые пункты меню в работающем приложении](menu-images/dynamic01.png "Новые пункты меню в работающем приложении")

#### <a name="removing-menus-and-items"></a>Удаление меню и элементов

Если нужно удалить любое меню или пункт меню из пользовательского интерфейса приложения, можно использовать `RemoveItemAt` метод класса, указав `NSMenu` для него Отсчитываемый от нуля индекс удаляемого элемента.

Например, чтобы удалить меню и пункты меню, созданные в приведенной выше подпрограмме, можно использовать следующий код:

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

В приведенном выше коде первые четыре пункта меню создаются в Interface Builder и вне области, доступных в приложении, поэтому они не удаляются динамически.

<a name="Contextual_Menus"></a>

## <a name="contextual-menus"></a>Контекстные меню

Контекстные меню появляются, когда пользователь щелкает правой кнопкой мыши элемент в окне. По умолчанию некоторые элементы пользовательского интерфейса, встроенные в macOS, уже имеют вложенные контекстные меню (например, представление текста). Однако иногда требуется создать собственные настраиваемые контекстные меню для элемента пользовательского интерфейса, добавленного в окно.

Давайте изменим файл **Main. Storyboard** в Xcode и добавим окно **окна** в нашу структуру, присвойте его **классу** значение "нспанел" в **инспекторе удостоверений**, добавьте новый элемент **помощника** в меню **окно** и вложите его в новое окно с помощью команды **показ перехода**:

[![Установка типа перехода](menu-images/context01.png "Установка типа перехода")](menu-images/context01-large.png#lightbox)

Давайте выполним следующие действия.

1. Перетащите **метку** из **инспектора библиотек** в окно **панели** и задайте для его текста значение "свойство": 

    [![Изменение значения метки](menu-images/context03.png "Изменение значения метки")](menu-images/context03-large.png#lightbox)
2. Затем перетащите **меню** из **инспектора библиотек** на контроллер представления в иерархии представлений и переименуйте три элемента меню: **документ**, **текст** и **Шрифт**.

    [![Обязательные пункты меню](menu-images/context02.png "Обязательные пункты меню")](menu-images/context02-large.png#lightbox)
3. Теперь перетащите элемент управления из **метки свойства** в **меню**:

    [![Перетаскивание для создания перехода](menu-images/context04.png "Перетаскивание для создания перехода")](menu-images/context04-large.png#lightbox)
4. Во всплывающем диалоговом окне выберите **меню**: 

    ![Установка типа перехода](menu-images/context05.png "Установка типа перехода")
5. В **инспекторе удостоверений**задайте для класса контроллера представления значение "панелвиевконтроллер": 

    [![Установка класса перехода](menu-images/context10.png "Установка класса перехода")](menu-images/context10-large.png#lightbox)
6. Переключитесь обратно на Visual Studio для Mac синхронизации, а затем вернитесь в Interface Builder.
7. Перейдите в **Редактор помощника** и выберите файл **панелвиевконтроллер. h** .
8. Создайте действие для пункта меню **документа** с именем `propertyDocument` : 

    [![Настройка действия](menu-images/context06.png "Настройка действия")](menu-images/context06-large.png#lightbox)
9. Повторите создание действий для остальных пунктов меню: 

    [![Необходимые действия](menu-images/context07.png "Необходимые действия")](menu-images/context07-large.png#lightbox)
10. Наконец, создайте выход для **метки свойства** с именем `propertyLabel` : 

    [![Настройка розетки](menu-images/context08.png "Настройка розетки")](menu-images/context08-large.png#lightbox)
11. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Измените файл **PanelViewController.CS** и добавьте следующий код:

```csharp
partial void propertyDocument (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Document";
}

partial void propertyFont (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Font";
}

partial void propertyText (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Text";
}
```

Теперь, если мы запустим приложение и щелкнули правой кнопкой мыши метку свойства на панели, мы увидим настраиваемое контекстное меню. Если выбрать и элемент в меню, то значение метки изменится:

![Выполнение контекстного меню](menu-images/context09.png "Выполнение контекстного меню")

Теперь рассмотрим создание меню строки состояния.

## <a name="status-bar-menus"></a>Меню строки состояния

В меню строки состояния отображается коллекция пунктов меню состояния, предоставляющих взаимодействие с пользователем или обратные отзывы, например меню или изображение, отражающее состояние приложения. Меню строки состояния приложения включено и активно, даже если приложение выполняется в фоновом режиме. Строка состояния на уровне системы находится в правой части строки меню приложения и является единственной строкой состояния, которая в настоящее время доступна в macOS.

Давайте изменим наш файл **AppDelegate.CS** и создадим `DidFinishLaunching` метод следующим образом:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Create a status bar menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Text";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Text");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        PhraseAddress(address);
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        PhraseDate(date);
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        PhraseGreeting(greeting);
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        PhraseSignature(signature);
    };
    item.Menu.AddItem (signature);
}
```

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;`предоставляет нам доступ к строке состояния на уровне системы. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);`создает новый элемент строки состояния. Здесь мы создадим меню и несколько пунктов меню и присоединяем меню к только что созданному элементу строки состояния. 

При запуске приложения отобразится новый элемент строки состояния. Выбор элемента в меню приведет к изменению текста в текстовом представлении: 

![Меню строки состояния работает](menu-images/statusbar01.png "Меню строки состояния работает")

Теперь давайте взглянем на создание пользовательских элементов меню Dock.

## <a name="custom-dock-menus"></a>Настраиваемые меню закрепления

Меню Dock (Закрепляемое) отображается для приложения Mac, когда пользователь щелкает правой кнопкой мыши значок приложения на панели Dock.

![Пользовательское меню закрепления](menu-images/dock01.png "Пользовательское меню закрепления")

Давайте создадим пользовательское меню Dock для нашего приложения, выполнив следующие действия.

1. В Visual Studio для Mac щелкните правой кнопкой мыши проект приложения и выберите **Добавить**  >  **новый файл...** В диалоговом окне новый файл выберите **Xamarin. Mac**  >  **пустое определение интерфейса**, используйте "доккмену" в качестве **имени** и нажмите кнопку " **создать** ", чтобы создать новый файл **доккмену. XIB** :

    ![Добавление пустого определения интерфейса](menu-images/dock02.png "Добавление пустого определения интерфейса")
2. В **панель решения**дважды щелкните файл **доккмену. XIB** , чтобы открыть его для редактирования в Xcode. Создайте новое **меню** со следующими элементами: **адрес**, **Дата**, **Приветствие**и **подпись** . 

    [![Компоновка пользовательского интерфейса](menu-images/dock03.png "Компоновка пользовательского интерфейса")](menu-images/dock03-large.png#lightbox)
3. Теперь давайте подключим новые пункты меню к нашим существующим действиям, созданным для нашего пользовательского меню в разделе [Добавление, изменение и удаление меню](#Adding,_Editing_and_Deleting_Menus) выше. Переключитесь в **инспектор подключений** и выберите **первый ответчик** в **иерархии интерфейсов**. Прокрутите вниз и найдите `phraseAddress:` действие. Перетащите линию из круга для этого действия в пункт меню " **адрес** ":

    [![Перетаскивание для связывания действия](menu-images/dock04.png "Перетаскивание для связывания действия")](menu-images/dock04-large.png#lightbox)
4. Повторите все остальные пункты меню, присоединив их к соответствующим действиям: 

    [![Необходимые действия](menu-images/dock05.png "Необходимые действия")](menu-images/dock05-large.png#lightbox)
5. Затем выберите **приложение** в **иерархии интерфейсов**. В **инспекторе подключений**перетащите линию из круга в `dockMenu` розетке в меню, которое мы только что создали:

    [![Перетаскивание провода](menu-images/dock06.png "Перетаскивание провода")](menu-images/dock06-large.png#lightbox)
6. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.
7. Дважды щелкните файл **info. plist** , чтобы открыть его для редактирования: 

    [![Редактирование файла INFO. plist](menu-images/dock07.png "Редактирование файла Info.plist")](menu-images/dock07-large.png#lightbox)
8. Перейдите на вкладку **источник** в нижней части экрана: 

    [![Выбор представления источника](menu-images/dock08.png "Выбор представления источника")](menu-images/dock08-large.png#lightbox)
9. Нажмите кнопку **Добавить новую запись**, нажмите зеленую кнопку со знаком «плюс», задайте для свойства имя «аппледоккмену» и значение «доккмену» (имя нашего нового XIB-файла без расширения): 

    [![Добавление элемента Доккмену](menu-images/dock09.png "Добавление элемента Доккмену")](menu-images/dock09-large.png#lightbox)

Теперь, если мы запустим приложение и щелкнули его правой кнопкой мыши на значке закрепления, отобразятся новые пункты меню:

![Пример работающего меню Dock](menu-images/dock10.png "Пример работающего меню Dock")

Если выбрать один из пользовательских элементов в меню, текст в нашем текстовом представлении будет изменен.

<a name="Pop-up_Menus_and_Pull-Down_Lists"></a>

## <a name="pop-up-button-and-pull-down-lists"></a>Всплывающая кнопка и раскрывающиеся списки

Всплывающая кнопка отображает выбранный элемент и предоставляет список параметров, которые нужно выбрать по щелчку пользователя. Раскрывающийся список — это тип всплывающей кнопки, обычно используемой для выбора команд, относящихся к контексту текущей задачи. Оба могут находиться в любом месте окна.

Давайте создадим настраиваемую всплывающую кнопку для нашего приложения, выполнив следующие действия.

1. Измените файл **Main. Storyboard** в Xcode и перетащите **всплывающую кнопку** из **инспектора библиотек** в окно **панели** , созданное в разделе [контекстных меню](#Contextual_Menus) : 

    [![Добавление всплывающей кнопки](menu-images/popup01.png "Добавление всплывающей кнопки")](menu-images/popup01-large.png#lightbox)
2. Добавьте новый пункт меню и задайте заголовки элементов во всплывающем окне: **адрес**, **Дата**, **Приветствие**и **подпись** . 

    [![Настройка пунктов меню](menu-images/popup02.png "Настройка пунктов меню")](menu-images/popup02-large.png#lightbox)
3. Теперь подключим новые пункты меню к существующим действиям, созданным для нашего пользовательского меню в разделе [Добавление, изменение и удаление меню](#Adding,_Editing_and_Deleting_Menus) выше. Переключитесь в **инспектор подключений** и выберите **первый ответчик** в **иерархии интерфейсов**. Прокрутите вниз и найдите `phraseAddress:` действие. Перетащите линию из круга для этого действия в пункт меню " **адрес** ": 

    [![Перетаскивание для связывания действия](menu-images/popup03.png "Перетаскивание для связывания действия")](menu-images/popup03-large.png#lightbox)
4. Повторите все остальные пункты меню, присоединив их к соответствующим действиям: 

    [![Все необходимые действия](menu-images/popup04.png "Все необходимые действия")](menu-images/popup04-large.png#lightbox)
5. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь, если мы запустим приложение и выбрали элемент из всплывающего окна, текст в нашем текстовом представлении изменится:

![Пример выполняемого всплывающего окна](menu-images/popup05.png "Пример выполняемого всплывающего окна")

Вы можете создавать и работать с раскрывающимися списками точно так же, как всплывающие кнопки. Вместо присоединения к существующему действию можно создать собственные пользовательские действия, как и для нашего контекстного меню в разделе [контекстных меню](#Contextual_Menus) .

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с меню и элементами меню в приложении Xamarin. Mac. Сначала мы рассмотрели строку меню приложения, а затем рассматривали создание контекстных меню, далее мы рассмотрели меню строки состояния и настраиваемые меню закрепления. Наконец, мы охвачены всплывающие меню и раскрывающиеся списки.

## <a name="related-links"></a>Связанные ссылки

- [Макменус (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macmenus)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Рекомендации по работе с персоналом — меню](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Общие сведения о меню приложений и всплывающих списках](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
