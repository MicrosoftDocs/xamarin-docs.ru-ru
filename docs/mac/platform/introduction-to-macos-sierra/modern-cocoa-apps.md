---
title: Создание современных приложений macOS
description: В этой статье рассматриваются некоторые советы, функции и методы, которые разработчик может использовать для создания современного приложения macOS на Xamarin. Mac.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: d2a3181360abd7ee7a5124602d0c4186b0a8ec38
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430160"
---
# <a name="building-modern-macos-apps"></a>Создание современных приложений macOS

_В этой статье рассматриваются некоторые советы, функции и методы, которые разработчик может использовать для создания современного приложения macOS на Xamarin. Mac._

<a name="Building-Modern-Looks-with-Modern-Views"></a>

## <a name="building-modern-looks-with-modern-views"></a>Создание современных представлений с современными представлениями

Современный вид будет включать современный внешний вид окна и панели инструментов, как показано в примере приложения:

[![Пример современного пользовательского интерфейса приложения Mac](modern-cocoa-apps-images/content08.png)](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views"></a>

### <a name="enabling-full-sized-content-views"></a>Включение представлений содержимого в полном размере

Чтобы добиться этого, в приложении Xamarin. Mac разработчик хочет использовать _представление содержимого полного размера_, что означает, что содержимое распространяется в области инструментов и заголовка и будет автоматически размыто macOS.

Чтобы включить эту функцию в коде, создайте пользовательский класс для `NSWindowController` и сделайте его следующим:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Set window to use Full Size Content View
            Window.StyleMask = NSWindowStyle.FullSizeContentView;
        }
        #endregion
    }
}
```

Эту функцию можно также включить в Interface Builder Xcode, выбрав окно и установив **полный размер представления содержимого**:

[![Изменение основной раскадровки в Interface Builder Xcode](modern-cocoa-apps-images/content01.png)](modern-cocoa-apps-images/content01.png#lightbox)

При использовании представления содержимого полного размера разработчику может потребоваться смещение содержимого под областями заголовка и панели инструментов, чтобы конкретное содержимое (например, метки) не выводится под слайдом.

Чтобы усложнить эту ошибку, области заголовка и панели инструментов могут иметь динамическую высоту в зависимости от действия, выполняемого пользователем, версии macOS, установленной пользователем, и (или) оборудования Mac, на котором выполняется приложение.

В результате просто жестко закодировать смещение при размещении пользовательского интерфейса не удастся. Разработчику необходимо будет использовать динамический подход.

Компания Apple включила [наблюдаемое свойство "ключ-значение](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` " `NSWindow` класса, чтобы получить текущую область содержимого в коде. Разработчик может использовать это значение, чтобы вручную разместить необходимые элементы при изменении области содержимого.

Лучшим решением является использование классов автоматического макета и размера для размещения элементов пользовательского интерфейса в коде или Interface Builder.

Код, как и в следующем примере, можно использовать для размещения элементов пользовательского интерфейса с помощью классов автомакета и размера в контроллере представления приложения.

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        #region Computed Properties
        public NSLayoutConstraint topConstraint { get; set; }
        #endregion

        ...

        #region Override Methods
        public override void UpdateViewConstraints ()
        {
            // Has the constraint already been set?
            if (topConstraint == null) {
                // Get the top anchor point
                var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
                var topAnchor = contentLayoutGuide.TopAnchor;

                // Found?
                if (topAnchor != null) {
                    // Assemble constraint and activate it
                    topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
                    topConstraint.Active = true;
                }
            }

            base.UpdateViewConstraints ();
        }
        #endregion
    }
}
```

Этот код создает хранилище для верхнего ограничения, которое будет применено к метке ( `ItemTitle` ), чтобы убедиться, что он не находится в области заголовка и панели инструментов:

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

Переопределяя метод контроллера представления `UpdateViewConstraints` , разработчик может проверить, было ли уже создано необходимое ограничение, и при необходимости создать его.

Если необходимо построить новое ограничение, свойство окна, к которому `ContentLayoutGuide` осуществляется доступ и приведению к элементу управления, который должен быть ограничен `NSLayoutGuide` :

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

Доступ к свойству Топанчор `NSLayoutGuide` доступа осуществляется, и, если он доступен, он используется для создания нового ограничения с требуемым суммой смещения, а новое ограничение становится активным для применения.

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars"></a>

### <a name="enabling-streamlined-toolbars"></a>Включение упрощенных панелей инструментов

Обычное окно macOS включает стандартную строку заголовка при запуске на верхней границе окна. Если в окне также есть панель инструментов, она будет отображаться под этой областью заголовка окна:

[![Стандартная панель инструментов Mac](modern-cocoa-apps-images/content02.png)](modern-cocoa-apps-images/content02.png#lightbox)

При использовании упрощенной панели инструментов область заголовка исчезает, а панель инструментов перемещается вверх в расположение строки заголовка, в строке с кнопками Закрыть, свернуть и развернуть окна.

[![Упрощенная панель инструментов Mac](modern-cocoa-apps-images/content03.png)](modern-cocoa-apps-images/content03.png#lightbox)

Упрощенная панель инструментов включается путем переопределения `ViewWillAppear` метода `NSViewController` и выглядит следующим образом:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

Этот результат обычно используется для _Shoebox приложений_ (одно окно приложений), таких как карты, календарь, примечания и системные настройки.

<a name="Using-Accessory-View-Controllers"></a>

### <a name="using-accessory-view-controllers"></a>Использование контроллеров представлений аксессуаров

В зависимости от структуры приложения разработчику также может потребоваться дополнить область заголовка с помощью контроллера представлений принадлежности, который отображается прямо под областью заголовка или панели инструментов, чтобы предоставить пользователю контекстные элементы управления в зависимости от активности, в которой они находятся в данный момент.

[![Пример контроллера представления аксессуаров](modern-cocoa-apps-images/content04.png)](modern-cocoa-apps-images/content04.png#lightbox)

Контроллер представления аксессуаров будет автоматически размыт и масштабироваться системой без участия разработчика.

Чтобы добавить контроллер представления аксессуаров, выполните следующие действия.

1. В **обозревателе решений** дважды щелкните файл `Main.storyboard`, чтобы открыть его для редактирования.
2. Перетащите **Пользовательский контроллер представления** в иерархию окна:

    [![Добавление нового пользовательского контроллера представления](modern-cocoa-apps-images/content05.png)](modern-cocoa-apps-images/content05.png#lightbox)
3. Разметка пользовательского интерфейса представления принадлежности:

    [![Проектирование нового представления](modern-cocoa-apps-images/content06.png)](modern-cocoa-apps-images/content06.png#lightbox)
4. Предоставьте представление "принадлежность" в качестве **розетки** и других **действий** или **розеток** для своего пользовательского интерфейса:

    [![Добавление требуемой розетки](modern-cocoa-apps-images/content07.png)](modern-cocoa-apps-images/content07.png#lightbox)
5. Сохраните изменения.
6. Вернитесь к Visual Studio для Mac, чтобы синхронизировать изменения.

Измените `NSWindowController` и сделайте это следующим образом:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }
        #endregion
    }
}
```

Ключевые моменты этого кода — это то место, где для представления задано пользовательское представление, которое было определено в Interface Builder и предоставляется в качестве **розетки**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

И `LayoutAttribute` , который определяет, _где_ будет отображаться принадлежность:

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

Так как macOS теперь полностью локализован, `Left` Свойства и являются `Right` `NSLayoutAttribute` устаревшими и должны быть заменены на `Leading` и `Trailing` .

<a name="Using-Tabbed-Windows"></a>

### <a name="using-tabbed-windows"></a>Использование окон с вкладками

Кроме того, система macOS может добавить в окно приложения контроллеры представления аксессуаров. Например, для создания окон с вкладками, в которых несколько окон приложения объединяются в одно виртуальное окно:

[![Пример окна Mac с вкладками](modern-cocoa-apps-images/content08.png)](modern-cocoa-apps-images/content08.png#lightbox)

Как правило, разработчику необходимо выполнить ограниченное действие, используя окна с вкладками в своих приложениях Xamarin. Mac, система будет автоматически выполнять эти операции следующим образом:

- При вызове метода Windows будет автоматически отображаться на вкладке `OrderFront` .
- При вызове метода Windows автоматически отменяет вкладки `OrderOut` .
- В коде все окна с вкладками по-прежнему считаются видимыми, но все несамые нестандартные вкладки скрыты системой с помощью Кореграфикс.
- Используйте `TabbingIdentifier` свойство объекта, `NSWindow` чтобы сгруппировать окна на вкладках.
- Если это `NSDocument` приложение на основе, некоторые из этих функций будут автоматически включены (например, кнопка «плюс» будет добавлена на панель вкладок) без каких-либо действий разработчика.
- Приложения, не `NSDocument` основанные на, могут включить кнопку "плюс" в группе вкладок, чтобы добавить новый документ путем переопределения `GetNewWindowForTab` метода `NSWindowsController` .

Объединяя все части, `AppDelegate` приложение, которое хотели бы использовать системные окна с вкладками, может выглядеть следующим образом:

```csharp
using AppKit;
using Foundation;

namespace MacModern
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewDocumentNumber { get; set; } = 0;
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

        #region Custom Actions
        [Export ("newDocument:")]
        public void NewDocument (NSObject sender)
        {
            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow (this);
        }
        #endregion
    }
}
```

Где `NewDocumentNumber` свойство отслеживает количество новых созданных документов, а `NewDocument` метод создает новый документ и отображает его.

`NSWindowController`Затем может выглядеть следующим образом:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Application Access
        /// <summary>
        /// A helper shortcut to the app delegate.
        /// </summary>
        /// <value>The app.</value>
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void SetDefaultDocumentTitle ()
        {
            // Is this the first document?
            if (App.NewDocumentNumber == 0) {
                // Yes, set title and increment
                Window.Title = "Untitled";
                ++App.NewDocumentNumber;
            } else {
                // No, show title and count
                Window.Title = $"Untitled {App.NewDocumentNumber++}";
            }
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Prefer Tabbed Windows
            Window.TabbingMode = NSWindowTabbingMode.Preferred;
            Window.TabbingIdentifier = "Main";

            // Set default window title
            SetDefaultDocumentTitle ();

            // Set window to use Full Size Content View
            // Window.StyleMask = NSWindowStyle.FullSizeContentView;

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }

        public override void GetNewWindowForTab (NSObject sender)
        {
            // Ask app to open a new document window
            App.NewDocument (this);
        }
        #endregion
    }
}
```

Где статическое `App` свойство предоставляет ярлык для перехода к `AppDelegate` . `SetDefaultDocumentTitle`Метод задает заголовок нового документа на основе числа созданных новых документов.

Следующий код сообщает macOS, что приложение предпочитает использовать вкладки и предоставляет строку, позволяющую группировать окна приложения по вкладкам:

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

А следующий метод переопределения добавляет кнопку «плюс» на панель вкладок, которая создает новый документ по щелчку пользователя:

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation"></a>

### <a name="using-core-animation"></a>Использование основной анимации

Основная анимация — это высокоуправляемый модуль визуализации графики, встроенный в macOS. Основная анимация оптимизирована для использования преимуществ GPU (графического процессора), доступных в современных macOS оборудовании, в отличие от выполнения графических операций на ЦП, что может замедлить работу компьютера.

`CALayer`, Предоставляемый основной анимацией, может использоваться для таких задач, как быстрая и жидкий прокрутки и анимации. Пользовательский интерфейс приложения должен состоять из нескольких подпросмотров и слоев, чтобы полностью воспользоваться преимуществами основной анимации.

`CALayer`Объект предоставляет несколько свойств, позволяющих разработчику контролировать, что отображается на экране пользователю, например:

- `Content` — Может иметь значение `NSImage` или `CGImage` , которое предоставляет содержимое слоя.
- `BackgroundColor` — Задает цвет фона слоя в виде `CGColor`
- `BorderWidth` — Задает ширину границы.
- `BorderColor` — Задает цвет границы.

Чтобы использовать основные графические объекты в пользовательском интерфейсе приложения, необходимо использовать представления _слоев_ , которые компания Apple предлагает разработчику всегда включать в представлении содержимого окна. Таким образом, все дочерние представления также будут автоматически наследовать резервные слои.

Кроме того, Apple предлагает использовать многослойные представления, в отличие от добавления нового в `CALayer` качестве подслоя, так как система автоматически обрабатывает несколько обязательных параметров (например, тех, которые необходимы для отображения Retina).

Резервное копирование слоев можно включить, задав `WantsLayer` для параметра a `NSView` значение `true` или внутри Interface Builder Xcode в **инспекторе эффектов представления** , установив флажок **основной уровень анимации**:

[![Инспектор эффектов представления](modern-cocoa-apps-images/content09.png)](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers"></a>

#### <a name="redrawing-views-with-layers"></a>Перерисовка представлений с помощью слоев

Еще один важный шаг при использовании представлений слоев в приложении Xamarin. Mac — установка значения в `LayerContentsRedrawPolicy` `NSView` `OnSetNeedsDisplay` в `NSViewController` . Пример:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Если разработчик не устанавливает это свойство, представление будет перерисовано при каждом изменении источника фрейма, что не требуется для повышения производительности. Если этому свойству задано значение для `OnSetNeedsDisplay` разработчика `NeedsDisplay` , для `true` принудительного перерисовки содержимого потребуется вручную задать значение.

Если представление помечено как "грязное", система проверяет `WantsUpdateLayer` свойство представления. Если он возвращает `true` `UpdateLayer` , вызывается метод, в противном случае `DrawRect` вызывается метод представления для обновления содержимого представления.

При необходимости компания Apple предлагает следующие рекомендации по обновлению содержимого представлений:

- Apple предпочитает использовать `UpdateLater` `DrawRect` при любой возможности, так как это обеспечивает значительное повышение производительности.
- Используйте те же самые `layer.Contents` элементы пользовательского интерфейса, которые выглядят похожими.
- Компания Apple также предпочитает разработчикам создавать пользовательский интерфейс, используя стандартные представления, такие как `NSTextField` , если это возможно.

Чтобы использовать `UpdateLayer` , создайте пользовательский класс для `NSView` и сделайте код следующим:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView
    {
        #region Computed Properties
        public override bool WantsLayer {
            get { return true; }
        }

        public override bool WantsUpdateLayer {
            get { return true; }
        }
        #endregion

        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void DrawRect (CoreGraphics.CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

        }

        public override void UpdateLayer ()
        {
            base.UpdateLayer ();

            // Draw view
            Layer.BackgroundColor = NSColor.Red.CGColor;
        }
        #endregion
    }
}
```

<a name="Using-Modern-Drag-and-Drop"></a>

## <a name="using-modern-drag-and-drop"></a>Использование современных перетаскиваний

Чтобы предоставить пользователю современные возможности перетаскивания, разработчик должен внедрить _перетаскивание флоккинг_ в свои операции перетаскивания приложения. Перетащите Флоккинг, где каждый отдельный файл или элемент изначально отображается как отдельный элемент, который флоккс (группировать вместе с курсором с количеством элементов) по мере того, как пользователь продолжит операцию перетаскивания.

Если пользователь завершает операцию перетаскивания, отдельные элементы будут Унфлокк и возвращаться к их исходным расположениям.

В следующем примере кода включается перетаскивание Флоккинг для пользовательского представления:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView, INSDraggingSource, INSDraggingDestination
    {
        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void MouseDragged (NSEvent theEvent)
        {
            // Create group of string to be dragged
            var string1 = new NSDraggingItem ((NSString)"Item 1");
            var string2 = new NSDraggingItem ((NSString)"Item 2");
            var string3 = new NSDraggingItem ((NSString)"Item 3");

            // Drag a cluster of items
            BeginDraggingSession (new [] { string1, string2, string3 }, theEvent, this);
        }
        #endregion
    }
}
```

Флоккингный результат достигается путем отправки каждого переперетаскиваемого элемента в `BeginDraggingSession` метод в `NSView` качестве отдельного элемента в массиве.

При работе с `NSTableView` или `NSOutlineView` Используйте `PastboardWriterForRow` метод класса, `NSTableViewDataSource` чтобы начать операцию перетаскивания:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDataSource: NSTableViewDataSource
    {
        #region Constructors
        public ContentsTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override INSPasteboardWriting GetPasteboardWriterForRow (NSTableView tableView, nint row)
        {
            // Return required pasteboard writer
            ...

            // Pasteboard writer failed
            return null;
        }
        #endregion
    }
}
```

Это позволяет разработчику предоставить индивидуальный объект `NSDraggingItem` для каждого элемента в таблице, который перетаскивается в отличие от старого метода `WriteRowsWith` , записывающего все строки как одну группу на монтажный стол.

При работе с `NSCollectionViews` снова используйте метод, а не `PasteboardWriterForItemAt` `WriteItemsAt` метод, когда начинается перетаскивание.

Разработчик всегда должен избегать размещения больших файлов на монтажном столе. Новые возможности для macOS Sierra, _обещания файлов_ позволяют разработчику размещать ссылки на файлы на монтажном столе, которые позже будут выполнены, когда пользователь завершит операцию удаления с помощью новых `NSFilePromiseProvider` классов и `NSFilePromiseReceiver` .

<a name="Using-Modern-Event-Tracking"></a>

## <a name="using-modern-event-tracking"></a>Использование современного отслеживания событий

Для элемента пользовательского интерфейса (например, `NSButton` ), добавленного в заголовок или область панели инструментов, пользователь должен иметь возможность щелкнуть элемент и вызвать событие как обычное (например, отображение всплывающего окна). Однако, поскольку элемент также находится в области заголовка или панели инструментов, пользователь должен иметь возможность щелкнуть и перетащить элемент, чтобы также переместить окно.

Чтобы сделать это в коде, создайте пользовательский класс для элемента (например, `NSButton` ) и переопределите `MouseDown` событие следующим образом:

```csharp
public override void MouseDown (NSEvent theEvent)
{
    var shouldCallSuper = false;

    Window.TrackEventsMatching (NSEventMask.LeftMouseUp, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Handle event as normal
        stop = true;
        shouldCallSuper = true;
    });

    Window.TrackEventsMatching(NSEventMask.LeftMouseDragged, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Pass drag event to window
        stop = true;
        Window.PerformWindowDrag (evt);
    });

    // Call super to handle mousedown
    if (shouldCallSuper) {
        base.MouseDown (theEvent);
    }
}
```

Этот код использует `TrackEventsMatching` метод объекта, к `NSWindow` которому присоединен элемент пользовательского интерфейса, чтобы перехватить `LeftMouseUp` `LeftMouseDragged` события и. Для `LeftMouseUp` события элемент пользовательского интерфейса реагирует как стандартный. Для `LeftMouseDragged` события событие передается `NSWindow` `PerformWindowDrag` методу для перемещения окна на экране.

Вызов `PerformWindowDrag` метода `NSWindow` класса предоставляет следующие преимущества.

- Это позволяет перемещать окно, даже если приложение зависла (например, при обработке глубокого цикла).
- Переключение пространства будет работать должным образом.
- Панель пробелов будет отображаться как обычная.
- Привязка и выравнивание окна работают как обычные.

<a name="Using-Modern-Container-View-Controls"></a>

## <a name="using-modern-container-view-controls"></a>Использование современных элементов управления "представление контейнера"

macOS Sierra предоставляет множество современных улучшений существующих элементов управления представления контейнера, доступных в предыдущей версии операционной системы.

<a name="Table View Enhancements"></a>

## <a name="table-view-enhancements"></a>Улучшения табличного представления

Разработчик всегда должен использовать новую `NSView` версию на основе элементов управления представлением контейнеров, например `NSTableView` . Пример:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }
        #endregion
    }
}
```

Это позволяет прикреплять пользовательские действия строк таблицы к определенным строкам в таблице (например, прокрутка вправо для удаления строки). Чтобы включить такое поведение, переопределите `RowActions` метод `NSTableViewDelegate` :

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }

        public override NSTableViewRowAction [] RowActions (NSTableView tableView, nint row, NSTableRowActionEdge edge)
        {
            // Take action based on the edge
            if (edge == NSTableRowActionEdge.Trailing) {
                // Create row actions
                var editAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Regular, "Edit", (action, rowNum) => {
                    // Handle row being edited
                    ...
                });

                var deleteAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Destructive, "Delete", (action, rowNum) => {
                    // Handle row being deleted
                    ...
                });

                // Return actions
                return new [] { editAction, deleteAction };
            } else {
                // No matching actions
                return null;
            }
        }
        #endregion
    }
}
```

Статический объект `NSTableViewRowAction.FromStyle` используется для создания нового действия строки таблицы со следующими стилями:

- `Regular` — Выполняет стандартное обратимое действие, например изменение содержимого строки.
- `Destructive` — Выполняет разрушительное действие, например удаление строки из таблицы. Эти действия будут отображены с красным фоном.

<a name="Scroll-View-Enhancements"></a>

## <a name="scroll-view-enhancements"></a>Улучшения представления прокрутки

При непосредственном использовании прокрутки ( `NSScrollView` ) или в рамках другого элемента управления (например `NSTableView` ,) содержимое представления прокрутки можно сдвинуть под областью заголовка и панели инструментов в приложении Xamarin. Mac, используя современный вид и представления.

В результате первый элемент в области содержимого с прокруткой может частично закрываться областью заголовка и панели инструментов.

Чтобы устранить эту ошибку, компания Apple добавила в класс два новых свойства `NSScrollView` :

- `ContentInsets` — Позволяет разработчику предоставить `NSEdgeInsets` объект, определяющий смещение, которое будет применено к верхней части представления прокрутки.
- `AutomaticallyAdjustsContentInsets` — Если `true` режим прокрутки будет автоматически обслуживать `ContentInsets` для разработчика.

С помощью `ContentInsets` разработчика можно настроить начало представления прокрутки, чтобы разрешить включение таких аксессуаров, как:

- Индикатор сортировки, похожий на тот, который показан в почтовом приложении.
- Поле поиска.
- Кнопка Обновить или обновить.

<a name="Auto-Layout-and-Localization-in-Modern-Apps"></a>

## <a name="auto-layout-and-localization-in-modern-apps"></a>Автоматический макет и локализация в современных приложениях

Компания Apple включила в Xcode несколько технологий, позволяющих разработчикам легко создавать международные приложения macOS. Xcode теперь позволяет разработчику отделить пользовательский текст от конструкции пользовательского интерфейса приложения в его файлах раскадровки и предоставляет средства для обслуживания этого разделения при изменении пользовательского интерфейса.

Дополнительные сведения см. в статье о [международной связи и локализации](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)Apple.

<a name="Implementing-Base-Internationalization"></a>

### <a name="implementing-base-internationalization"></a>Реализация базового международного использования

Реализуя базовую многоязычность, разработчик может предоставить один файл раскадровки для представления пользовательского интерфейса приложения и отделения всех строк, доступных пользователю.

Когда разработчик создает первоначальный файл раскадровки (или файлы), определяющий пользовательский интерфейс приложения, они будут построены в базовой международной локализации (языке, который говорит разработчик).

Затем разработчик может экспортировать локализацию и базовые строки интернационализации (в макете пользовательского интерфейса раскадровки), которые можно преобразовать на несколько языков.

Позже эти локализации можно импортировать, и Xcode создаст файлы строк, зависящие от языка, для раскадровки.

<a name="Implementing-Auto-Layout-to-Support-Localization"></a>

### <a name="implementing-auto-layout-to-support-localization"></a>Реализация автоматического макета для поддержки локализации

Поскольку локализованные версии строковых значений могут иметь значительно отличающиеся размеры и (или) направление чтения, разработчик должен использовать автоматическую разметку для размещения и изменения размера пользовательского интерфейса приложения в файле раскадровки.

Для Apple рекомендуется выполнить следующие действия.

- **Удалить ограничения фиксированной ширины** — для всех текстовых представлений должно быть разрешено изменять размер в зависимости от их содержимого. Представление с фиксированной шириной может обрезать содержимое на определенных языках.
- **Использовать встроенные размеры содержимого** — по умолчанию текстовые представления будут иметь Автоподбор размера в соответствии с содержимым. Для текстового представления, для которого неправильно задано значение размера, выберите их в Xcode Interface Builder а затем выберите **изменить**  >  **Размер в соответствии с содержимым**.
- **Применить начальные и конечные атрибуты** , так как направление текста может меняться в зависимости от языка пользователя, используйте новые `Leading` атрибуты и ограничения, а не `Trailing` существующие `Right` `Left` атрибуты и. `Leading` и `Trailing` будут автоматически корректироваться в зависимости от направления языков.
- **Закреплять представления в смежные представления** — это позволяет представлениям изменять расположение и размер по мере изменения представлений в ответ на выбранный язык.
- **Не устанавливайте минимальный или максимальный размер Windows** . Разрешите Windows изменить размер, так как выбранный язык изменит размеры областей содержимого.
- **Непрерывное тестирование изменений макета** во время разработки в приложении должно выполняться на разных языках. Дополнительные сведения см. в документации Apple по [тестированию международного приложения](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) .
- **Использование нсстакквиевс для закрепления представлений вместе**  -  `NSStackViews` позволяет менять содержимое и увеличивать его в соответствии с прогнозируемыми способами, а размер содержимого изменяется в зависимости от выбранного языка.

<a name="Localizing-in-Xcodes-Interface-Builder"></a>

### <a name="localizing-in-xcodes-interface-builder"></a>Локализация в Interface Builder Xcode

Компания Apple предоставила несколько функций в Interface Builder Xcode, которые разработчик может использовать при проектировании или редактировании пользовательского интерфейса приложения для поддержки локализации. Раздел **направление текста** в **инспекторе атрибутов** позволяет разработчику предоставлять подсказки о том, как направление должно использоваться и обновляться при выборе представления на основе текста (например, `NSTextField` ):

[![Параметры направления текста](modern-cocoa-apps-images/content10.png)](modern-cocoa-apps-images/content10.png#lightbox)

Существует три возможных значения **направления текста**:

- **Естественное** — макет основан на строке, назначенной элементу управления.
- **Слева направо** — макет всегда принудительно устанавливается слева направо.
- **Справа налево** — макет всегда принудительно устанавливается справа налево.

Существует два возможных значения для **макета**:

- **Слева направо** — макет всегда находится слева направо.
- **Справа налево** — макет всегда находится справа налево.

Обычно их не следует изменять, если не требуется определенное выравнивание.

Свойство **Mirror** сообщает системе, что необходимо отразить определенные свойства элемента управления (например, расположение ячейки). Он имеет три возможных значения:

- **Автоматически** — местоположение будет автоматически меняться в зависимости от направления выбранного языка.
- **В интерфейсе справа** налево — расположение изменится только на языках с письмом справа налево.
- **Никогда** — местоположение никогда не изменится.

Если разработчик указал **центр**, **Выровнять** или **полностью** выравнивание содержимого текстового представления, они никогда не будут отражены на основе выбранного языка.

До macOS Sierra элементы управления, созданные в коде, не будут автоматически зеркально отображаться. Для управления зеркальным отображением разработчику пришлось использовать код, подобный приведенному ниже.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Setting a button's mirroring based on the layout direction
    var button = new NSButton ();
    if (button.UserInterfaceLayoutDirection == NSUserInterfaceLayoutDirection.LeftToRight) {
        button.Alignment = NSTextAlignment.Right;
        button.ImagePosition = NSCellImagePosition.ImageLeft;
    } else {
        button.Alignment = NSTextAlignment.Left;
        button.ImagePosition = NSCellImagePosition.ImageRight;
    }
}
```

Где и устанавливаются на `Alignment` `ImagePosition` основе элемента `UserInterfaceLayoutDirection` управления.

macOS Sierra добавляет несколько новых удобных конструкторов (через статический `CreateButton` метод), которые принимают несколько параметров (например, Title, Image и Action) и автоматически отображаются правильно. Пример:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances"></a>

## <a name="using-system-appearances"></a>Использование системных представлений

Современные приложения macOS могут использовать новый Темный внешний вид интерфейса, который хорошо подходит для создания, редактирования и представления изображений.

[![Пример темного пользовательского интерфейса окна Mac](modern-cocoa-apps-images/content11.png)](modern-cocoa-apps-images/content11.png#lightbox)

Это можно сделать, добавив одну строку кода до представления окна. Пример:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        ...

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Apply the Dark Interface Appearance
            View.Window.Appearance = NSAppearance.GetAppearance (NSAppearance.NameVibrantDark);

            ...
        }
        #endregion
    }
}
```

Статический `GetAppearance` метод `NSAppearance` класса используется для получения именованного внешнего вида из системы (в данном случае `NSAppearance.NameVibrantDark` ).

У Apple есть следующие рекомендации по использованию отображения систем.

- Предпочитать именованные цвета с жестко зафиксированными значениями (например `LabelColor` , и `SelectedControlColor` ).
- По возможности используйте стандартный системный стиль управления.

Приложение macOS, использующее внешний вид системы, будет автоматически правильно работать для пользователей, которые включили специальные возможности из приложения "Системные настройки". В результате компания Apple предлагает разработчику всегда использовать внешний вид системы в своих macOS приложениях.

<a name="Designing-UIs-with-Storyboards"></a>

## <a name="designing-uis-with-storyboards"></a>Разработка интерфейсов пользователя с помощью раскадровок

Раскадровки позволяют разработчику не только проектировать отдельные элементы, составляющие пользовательский интерфейс приложения, но и визуализировать и спроектировать поток пользовательского интерфейса и иерархию заданных элементов.

Контроллеры позволяют разработчикам собираются элементы в единицу композиции и переходов абстрактным и удаляют типичный "связывающий код", необходимый для перемещения по иерархии представления:

[![Изменение пользовательского интерфейса в Interface Builder Xcode](modern-cocoa-apps-images/content12.png)](modern-cocoa-apps-images/content12.png#lightbox)

Дополнительные сведения см. [в статье Введение в](~/mac/platform/storyboards/index.md) документацию по раскадровкам.

Существует множество экземпляров, в которых для заданной сцены, определенной в раскадровке, требуются данные из предыдущей сцены в иерархии представлений. Компания Apple предлагает следующие рекомендации по передаче данных между сценами:

- ДепенданЦиес данных всегда должен быть каскадным вниз по иерархии.
- Старайтесь не использовать структурную депенданЦиес пользовательского интерфейса прописано, так как это ограничивает гибкость пользовательского интерфейса.
- Используйте интерфейсы C# для предоставления универсальных депенданЦиес данных.

Контроллер представления, который выступает в качестве источника перехода, может переопределить `PrepareForSegue` метод и выполнить необходимую инициализацию (например, передачу данных) перед выполнением перехода для отображения целевого контроллера представления. Пример:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

Дополнительные сведения см. в нашей документации по [переходов](~/mac/platform/storyboards/indepth.md#Segues) .

<a name="Propagating-Actions"></a>

## <a name="propagating-actions"></a>Распространение действий

В зависимости от структуры приложения macOS может возникнуть ситуация, когда лучший обработчик для действия элемента управления пользовательского интерфейса может находиться в любом расположении в иерархии пользовательского интерфейса. Обычно это справедливо для меню и пунктов меню, которые находятся в собственной сцене, отдельно от остальной части пользовательского интерфейса приложения.

Для решения этой ситуации разработчик может создать настраиваемое действие и передать действие в цепочку респондента. Дополнительные сведения см. в документации по [действиям с пользовательскими окнами](~/mac/user-interface/menu.md) .

<a name="Modern-Mac-Features"></a>

## <a name="modern-mac-features"></a>Современные функции Mac

Компания Apple включила несколько функций, ориентированных на пользователей, в macOS Sierra, которые позволяют разработчику максимально использовать платформу Mac, например:

- **Нсусерактивити** — позволяет приложению описать действие, в котором в данный момент участвует пользователь. `NSUserActivity` изначально был создан для поддержки передачи, когда действие, запущенное на одном из устройств пользователя, может быть выбрано и продолжено на другом устройстве. `NSUserActivity` работает в macOS так же, как и в iOS, поэтому ознакомьтесь [с нашим введением в](~/ios/platform/handoff.md) документацию по возврату документации по iOS для получения дополнительных сведений.
- **Siri On Mac** -Siri использует текущее действие ( `NSUserActivity` ) для предоставления контекста командам, которые может выдать пользователь.
- **Восстановление состояния** . когда пользователь завершает приложение в macOS, а затем перезапустит его, приложение автоматически вернется к предыдущему состоянию. Разработчик может использовать API восстановления состояния для кодирования и восстановления временных состояний пользовательского интерфейса до отображения пользовательского интерфейса пользователю. Если приложение `NSDocument` основано на, восстановление состояния обрабатывается автоматически. Чтобы включить восстановление состояния для `NSDocument` приложений, не основанных на, задайте `Restorable` для свойства класса значение `NSWindow` `true` .
- **Документы в облаке** — прежде чем macOS Sierra, приложению пришлось явно принять участие в работе с документами на диске iCloud пользователя. В macOS Sierra папки " **Рабочий стол** пользователя" и " **документы** " могут синхронизироваться с устройством iCloud автоматически с помощью системы. В результате могут быть удалены локальные копии документов, чтобы освободить пространство на компьютере пользователя. `NSDocument` Эти изменения будут автоматически обработаны приложениями на основе приложений. Все остальные типы приложений должны использовать `NSFileCoordinator` для синхронизации чтения и записи документов.

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье рассмотрено несколько советов, функций и методик, которые разработчик может использовать для создания современного приложения macOS на Xamarin. Mac.

## <a name="related-links"></a>Связанные ссылки

- [Примеры macOS](/samples/browse/?products=xamarin&term=Xamarin.Mac)