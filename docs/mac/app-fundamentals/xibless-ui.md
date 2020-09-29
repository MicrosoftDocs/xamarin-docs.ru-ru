---
title: Структура пользовательского интерфейса. Storyboard/. XIB-меньше в Xamarin. Mac
description: В этой статье рассматривается создание пользовательского интерфейса приложения Xamarin. Mac непосредственно из кода C# без файлов с раскадровкой, файлов XIB или Interface Builder.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 47025f48b1f3455f70a077f2ca55da2cdeeba761
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430887"
---
# <a name="storyboardxib-less-user-interface-design-in-xamarinmac"></a>Структура пользовательского интерфейса. Storyboard/. XIB-меньше в Xamarin. Mac

_В этой статье рассматривается создание пользовательского интерфейса приложения Xamarin. Mac непосредственно из кода C# без файлов с раскадровкой, файлов XIB или Interface Builder._

## <a name="overview"></a>Обзор

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же элементам пользовательского интерфейса и средствам, что и разработчик, работающий на *уровне цели-C* и *Xcode* . Как правило, при создании приложения Xamarin. Mac вы будете использовать Interface Builder Xcode с файлами. Storyboard или. XIB для создания и обслуживания пользовательского интерфейса приложения.

Вы также можете создать часть пользовательского интерфейса приложения Xamarin. Mac непосредственно в коде C#. В этой статье рассматриваются основы создания пользовательских интерфейсов и элементов пользовательского интерфейса в коде C#.

[![Редактор кода Visual Studio для Mac](xibless-ui-images/intro01.png "Редактор кода Visual Studio для Mac")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code"></a>

## <a name="switching-a-window-to-use-code"></a>Переключение окна на использование кода

При создании нового приложения Xamarin. Mac Cocoa по умолчанию получается стандартное пустое окно. Это окно определяется в файле **Main. раскадровка** (или традиционного файла **MainWindow. XIB**), который автоматически включается в проект. Сюда также входит файл **ViewController.CS** , который управляет основным представлением приложения (или, как правило, это **MainWindow.cs** и файл **MainWindowController.CS** ).

Чтобы переключиться в окно Ксиблесс для приложения, выполните следующие действия.

1. Откройте приложение, для которого требуется отменить использование `.storyboard` файлов или XIB, чтобы определить пользовательский интерфейс в Visual Studio для Mac.
2. В **панель решения**щелкните правой кнопкой мыши файл **Main. Storyboard** или **MainWindow. XIB** и выберите **Удалить**:

    ![Удаление основной раскадровки или окна](xibless-ui-images/switch01.png "Удаление основной раскадровки или окна")
3. В **диалоговом окне Удалить**нажмите кнопку **Удалить** , чтобы полностью удалить. Storyboard или. XIB из проекта:

    ![Подтверждение удаления](xibless-ui-images/switch02.png "Подтверждение удаления")

Теперь необходимо изменить файл **MainWindow.CS** , чтобы определить макет окна и изменить файл **ViewController.CS** или **MainWindowController.CS** , чтобы создать экземпляр нашего `MainWindow` класса, так как мы больше не используем файл. Storyboard или. XIB.

Современные приложения Xamarin. Mac, использующие раскадровки для своего пользовательского интерфейса, не могут автоматически включать файлы **MainWindow.CS**, **ViewController.CS** или **MainWindowController.CS** . При необходимости просто добавьте в проект новый пустой класс C# (**Добавить**  >  **новый файл...**  >  **Общие**  >  сведения **Пустой класс**) и присвойте ему имя, совпадающее с отсутствующим файлом.

### <a name="defining-the-window-in-code"></a>Определение окна в коде

Затем измените файл **MainWindow.CS** и сделайте его следующим:

```csharp
using System;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindow : NSWindow
    {
        #region Private Variables
        private int NumberOfTimesClicked = 0;
        #endregion

        #region Computed Properties
        public NSButton ClickMeButton { get; set;}
        public NSTextField ClickMeLabel { get ; set;}
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }

        public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
            // Define the user interface of the window here
            Title = "Window From Code";

            // Create the content view for the window and make it fill the window
            ContentView = new NSView (Frame);

            // Add UI elements to window
            ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
                AutoresizingMask = NSViewResizingMask.MinYMargin
            };
            ContentView.AddSubview (ClickMeButton);

            ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
                BackgroundColor = NSColor.Clear,
                TextColor = NSColor.Black,
                Editable = false,
                Bezeled = false,
                AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
                StringValue = "Button has not been clicked yet."
            };
            ContentView.AddSubview (ClickMeLabel);
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Wireup events
            ClickMeButton.Activated += (sender, e) => {
                // Update count
                ClickMeLabel.StringValue = (++NumberOfTimesClicked == 1) ? "Button clicked one time." : string.Format("Button clicked {0} times.",NumberOfTimesClicked);
            };
        }
        #endregion

    }
}
```

Давайте рассмотрим несколько ключевых элементов.

Во-первых, мы добавили несколько _вычисленных свойств_ , которые будут действовать в качестве розеток (как если бы окно было создано в файле. Storyboard или. XIB):

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

Это предоставит нам доступ к элементам пользовательского интерфейса, которые будут отображаться в окне. Так как окно не было сведено из файла. Storyboard или. XIB, нам нужен способ создания экземпляра (как мы увидим далее в `MainWindowController` классе). Вот что делает этот новый метод конструктора:

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

Именно здесь мы разработаем макет окна и разместим все элементы пользовательского интерфейса, необходимые для создания требуемого пользовательского интерфейса. Перед добавлением элементов пользовательского интерфейса в окно требуется _представление содержимого_ , в котором будут содержаться элементы:

```csharp
ContentView = new NSView (Frame);
```

Это приведет к созданию представления содержимого, которое будет заполнять окно. Теперь мы добавим в окно наш первый элемент пользовательского интерфейса — `NSButton` .

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

Первое, что следует отметить, это то, что, в отличие от iOS, macOS использует математическую нотацию для определения своей системы координат окна. Таким образом, точка отсчета находится в левом нижнем углу окна, и значения увеличиваются вправо и находятся в правом верхнем углу окна. При создании новой `NSButton` учетной записи мы будем учитывать, как определить ее расположение и размер на экране.

`AutoresizingMask = NSViewResizingMask.MinYMargin`Свойство сообщает кнопке, что она должна находиться в том же расположении, что и верх окна, когда размер окна изменяется по вертикали. Опять же, это необходимо, поскольку (0, 0) находится в левом нижнем углу окна.

Наконец, `ContentView.AddSubview (ClickMeButton)` метод добавляет в `NSButton` представление содержимого, чтобы оно отображалось на экране при запуске приложения и отображенном окне.

Затем в окно добавляется метка, отображающая количество раз, когда была `NSButton` нажата кнопка:

```csharp
ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
    BackgroundColor = NSColor.Clear,
    TextColor = NSColor.Black,
    Editable = false,
    Bezeled = false,
    AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
    StringValue = "Button has not been clicked yet."
};
ContentView.AddSubview (ClickMeLabel);
```

Поскольку у macOS нет определенного элемента пользовательского интерфейса _Label_ , мы добавили специально оформленный, нередактируемый, `NSTextField` чтобы действовать как метка. Точно так же, как и на кнопке, размер и расположение принимают учетные записи, которые (0, 0) находятся в левом нижнем углу окна. `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin`Свойство использует оператор **or** для объединения двух `NSViewResizingMask` компонентов. Это приведет к тому, что метка останется в том же расположении, что и верхняя часть окна, когда размер окна изменяется вертикально и сжимается и увеличивается по ширине по мере изменения размера окна по горизонтали.

Опять же, `ContentView.AddSubview (ClickMeLabel)` метод добавляет в `NSTextField` представление содержимого, чтобы оно отображалось на экране при запуске приложения и открытом окне.

### <a name="adjusting-the-window-controller"></a>Настройка контроллера окна

Поскольку структура `MainWindow` больше не загружается из файла. Storyboard или. XIB, необходимо внести некоторые изменения в контроллер окна. Измените файл **MainWindowController.CS** и сделайте его следующим:

```csharp
using System;

using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindowController : NSWindowController
    {
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindowController (NSCoder coder) : base (coder)
        {
        }

        public MainWindowController () : base ("MainWindow")
        {
            // Construct the window from code here
            CGRect contentRect = new CGRect (0, 0, 1000, 500);
            base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);

            // Simulate Awaking from Nib
            Window.AwakeFromNib ();
        }

        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }

        public new MainWindow Window {
            get { return (MainWindow)base.Window; }
        }
    }
}

```

Давайте обсудим ключевые элементы этого изменения.

Сначала мы определим новый экземпляр `MainWindow` класса и присваиваем его свойству базового контроллера окна `Window` :

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Расположение окна экрана определяется с помощью `CGRect` . Как и в системе координат окна, экран определяет (0, 0) как нижний левый угол. Далее мы определим стиль окна с помощью оператора **или** , чтобы объединить две или более `NSWindowStyle` функции:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
```

Доступны следующие `NSWindowStyle` возможности.

- Без **рамки** . окно не будет иметь границы.
- **Под названием** — окно будет иметь заголовок.
- **Клосабле** — окно содержит кнопку Закрыть и может быть закрыто.
- **Миниатуризабле** — окно содержит кнопку миниатуризе и может быть сведено к минимальному.
- **Изменяемый** размер — окно будет иметь кнопку изменения размера и может быть изменяемым.
- **Программа** — окно — это окно в стиле программы (панель).
- **Докмодал** — если окно является панелью, оно будет задокументировать как модальный, а не модальный системный.
- **Нонактиватингпанел** — если окно является панелью, главное окно не будет создано.
- **Текстуредбаккграунд** — это окно будет иметь текстурированный фон.
- Без **масштабирования** — окно не будет масштабироваться.
- **Унифиедтитлеандтулбар** — области заголовка окна и панели инструментов будут присоединены.
- **HUD** — окно будет отображаться в виде панели отображения заголовков.
- **Фуллскринвиндов** — окно может входить в полноэкранный режим.
- **Фуллсизеконтентвиев** — представление содержимого окна находится позади области заголовка и панели инструментов.

Последние два свойства определяют _тип буферизации_ для окна и, если отрисовка окна будет отложена. Дополнительные сведения о `NSWindows` см. в документации Apple [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) .

Наконец, поскольку окно не было сведено из файла. Storyboard или. XIB, нам нужно смоделировать его в нашей **MainWindowController.CS** , вызвав `AwakeFromNib` метод Windows:

```csharp
Window.AwakeFromNib ();
```

Это позволит вам программировать код в окне так же, как стандартное окно, загруженное из файла. Storyboard или. XIB.

### <a name="displaying-the-window"></a>Отображение окна

После удаления файла. Storyboard или. XIB и изменения файлов **MainWindow.CS** и **MainWindowController.CS** вы будете использовать окно так же, как и в обычном окне, которое было создано в Xcode Interface Builder с XIB-файлом.

Следующий пример создаст новый экземпляр окна и его контроллер и отобразит окно на экране:

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

На этом этапе, если приложение запущено и нажата несколько раз, появится следующее:

![Пример выполнения приложения](xibless-ui-images/run01.png "Пример выполнения приложения")

## <a name="adding-a-code-only-window"></a>Добавление окна только кода

Если нужно добавить только код, ксиблесс окно в существующее приложение Xamarin. Mac, щелкните правой кнопкой мыши проект в **панель решения** и выберите **Добавить**  >  **новый файл..**. В диалоговом окне **новый файл** выберите **Xamarin. Mac**  >  **Cocoa Window с контроллером**, как показано ниже:

![Добавление нового контроллера окна](xibless-ui-images/add01.png "Добавление нового контроллера окна")

Как и раньше, мы удалим файл по умолчанию. Storyboard или. XIB из проекта (в данном случае **секондвиндов. XIB**) и выполните действия, описанные в разделе [Переключение окна, чтобы использовать код](#Switching_a_Window_to_use_Code) , приведенный выше, чтобы охватить определение окна в коде.

## <a name="adding-a-ui-element-to-a-window-in-code"></a>Добавление элемента пользовательского интерфейса в окно в коде

Было ли окно создано в коде или загружено из файла. Storyboard или. XIB, иногда требуется добавить элемент пользовательского интерфейса в окно из кода. Пример:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Приведенный выше код создает новый объект `NSButton` и добавляет его в `MyWindow` экземпляр окна для вывода. По сути, любой элемент пользовательского интерфейса, который можно определить в Interface Builder Xcode в файле. Storyboard или. XIB, можно создать в коде и отобразить в окне.

## <a name="defining-the-menu-bar-in-code"></a>Определение строки меню в коде

Из-за текущих ограничений в Xamarin. Mac не рекомендуется создавать строку меню приложения Xamarin. Mac — `NSMenuBar` — в коде, но продолжать использовать файл **Main. Storyboard** или **MainMenu. XIB** для определения. С другой стороны, можно добавлять и удалять меню и пункты меню в коде C#.

Например, измените файл **AppDelegate.CS** и сделайте так, чтобы `DidFinishLaunching` метод выглядел следующим образом:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    // Create a Status Bar Menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Phrases";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Phrases");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        Console.WriteLine("Address Selected");
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        Console.WriteLine("Date Selected");
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        Console.WriteLine("Greetings Selected");
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        Console.WriteLine("Signature Selected");
    };
    item.Menu.AddItem (signature);
}
```

Приведенный выше код создает меню строки состояния из кода и отображает его при запуске приложения. Дополнительные сведения о работе с меню см. в документации по [меню](~/mac/user-interface/menu.md) .

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается создание пользовательского интерфейса приложения Xamarin. Mac в коде C# в отличие от использования Interface Builder Xcode с файлами. Storyboard или. XIB.

## <a name="related-links"></a>Связанные ссылки

- [Макксиблесс (пример)](/samples/xamarin/mac-samples/macxibless)
- [Windows](~/mac/user-interface/window.md)
- [Меню](~/mac/user-interface/menu.md)
- [Рекомендации по созданию пользовательских интерфейсов в macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Введение в Windows](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)