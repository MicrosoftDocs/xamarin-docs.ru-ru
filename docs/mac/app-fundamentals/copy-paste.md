---
title: Копирование и вставка в Xamarin. Mac
description: В этой статье рассматривается работа с монтажным столом для копирования и вставки в приложение Xamarin. Mac. В нем показано, как работать со стандартными типами данных, которые могут совместно использоваться несколькими приложениями, а также как поддерживать пользовательские данные в определенном приложении.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: e85a273c75fd09672c6c75738adcdc576705af09
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433984"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Копирование и вставка в Xamarin. Mac

_В этой статье рассматривается работа с монтажным столом для копирования и вставки в приложение Xamarin. Mac. В нем показано, как работать со стандартными типами данных, которые могут совместно использоваться несколькими приложениями, а также как поддерживать пользовательские данные в определенном приложении._

## <a name="overview"></a>Обзор

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к одному и тому же монтажному столу (копирование и вставка), который разработчик, работающий на цели-C, имеет.

В этой статье мы рассмотрим два основных способа использования монтажного стола в приложении Xamarin. Mac:

1. **Стандартные типы данных** . Поскольку операции на монтажном столе обычно выполняются между двумя несвязанными приложениями, ни одно из приложений не знает типы данных, которые поддерживает другой. Чтобы максимально увеличить потенциал совместного использования, на монтажном столе может содержаться несколько представлений определенного элемента (используя стандартный набор типов данных), что позволяет приложению, использующему приложение, выбрать версию, которая лучше всего подходит для своих нужд.
2. **Пользовательские данные** . для поддержки копирования и вставки сложных данных в Xamarin. Mac можно определить пользовательский тип данных, который будет обрабатываться на монтажном столе. Например, приложение векторного рисования, которое позволяет пользователю копировать и вставлять сложные фигуры, состоящие из нескольких типов данных и точек.

[![Пример выполняющегося приложения](copy-paste-images/intro01.png "Пример выполняющегося приложения")](copy-paste-images/intro01-large.png#lightbox)

В этой статье рассматриваются основные принципы работы с монтажным столом в приложении Xamarin. Mac для поддержки операций копирования и вставки. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в этой статье.

Возможно, стоит взглянуть на [предоставление классов и методов C# для цели-c](~/mac/internals/how-it-works.md) в документе [внутренних компонентов Xamarin. Mac](~/mac/internals/how-it-works.md) , а также объясняются `Register` атрибуты и, `Export` используемые для подключения классов c# к объектам и элементам пользовательского интерфейса на языке c.

## <a name="getting-started-with-the-pasteboard"></a>Начало работы с монтажным знаком

На монтажном столе представлен стандартизированный механизм обмена данными в рамках определенного приложения или между приложениями. Типичное использование монтажного стола в приложении Xamarin. Mac заключается в обработке операций копирования и вставки, однако также поддерживаются и другие операции (например, перетаскивание & Drop и Службы приложений).

Чтобы быстро приступить к работе, мы начнем с простого практического знакомства с использованием монтажных переходов в приложении Xamarin. Mac. Позже мы предоставим подробное объяснение того, как работает монтажный стол и какие методы используются.

В этом примере мы создадим простое приложение на основе документов, которое управляет окном, содержащим представление изображений. Пользователь сможет копировать и вставлять изображения между документами в приложении и в другие приложения или несколько окон в одном приложении.

### <a name="creating-the-xamarin-project"></a>Создание проекта Xamarin

Во-первых, мы создадим новый документ на основе приложения Xamarin. Mac, для которого будет добавлена поддержка копирования и вставки.

Выполните следующие действия.

1. Запустите Visual Studio для Mac и щелкните ссылку **создать проект...** .
2. Выберите приложение **Mac**  >  **App**  >  **Cocoa App**, а затем нажмите кнопку **Далее** : 

    [![Создание нового проекта приложения Cocoa](copy-paste-images/sample01.png "Создание нового проекта приложения Cocoa")](copy-paste-images/sample01-large.png#lightbox)
3. Введите `MacCopyPaste` в качестве **имени проекта** и все остальное по умолчанию. Щелкните "Далее. 

    [![Задание имени проекта](copy-paste-images/sample01a.png "Задание имени проекта")](copy-paste-images/sample01a-large.png#lightbox)

4. Нажмите кнопку **создать** : 

    [![Подтверждение новых параметров проекта](copy-paste-images/sample02.png "Подтверждение новых параметров проекта")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Добавление NSDocument

Далее мы добавим пользовательский `NSDocument` класс, который будет использоваться в качестве фонового хранилища для пользовательского интерфейса приложения. Он будет содержать одно представление изображения и уметь Копировать изображение из представления на монтажный стол по умолчанию, а также как создать изображение из монтажного стола по умолчанию и отобразить его в представлении изображений.

Щелкните правой кнопкой мыши проект Xamarin. Mac в **панель решения** и выберите **Добавить**  >  **новый файл...**

![Добавление NSDocument в проект](copy-paste-images/sample03.png "Добавление NSDocument в проект")

Введите в поле **Имя** значение `ImageDocument` и нажмите кнопку **Новый**. Измените класс **ImageDocument.CS** и сделайте его следующим:

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

Подробнее рассмотрим часть кода ниже.

Следующий код предоставляет свойство для проверки существования данных изображения на монтажном столе по умолчанию, если изображение доступно, `true` возвращается else `false` .

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

Следующий код копирует изображение из представления присоединенного изображения в монтажный стол по умолчанию:

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

Следующий код демонстрирует изображение из монтажного стола по умолчанию и отображает его в присоединенном представлении изображения (если монтажный стол содержит допустимое изображение):

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

С помощью этого документа мы создадим пользовательский интерфейс для приложения Xamarin. Mac.

### <a name="building-the-user-interface"></a>Создание пользовательского интерфейса

Дважды щелкните файл **Main. Storyboard** , чтобы открыть его в Xcode. Затем добавьте панель инструментов и изображение и настройте их следующим образом:

[![Изменение панели инструментов](copy-paste-images/sample04.png "Изменение панели инструментов")](copy-paste-images/sample04-large.png#lightbox)

Добавьте **элемент панели инструментов** копировать и вставить изображение в левую часть панели инструментов. Мы будем использовать эти сочетания клавиш для копирования и вставки в меню Правка. Затем добавьте четыре **элемента панели инструментов с изображением** в правую часть панели инструментов. Мы будем использовать их для заполнения образа некоторыми изображениями по умолчанию.

Дополнительные сведения о работе с панелями инструментов см. в документации по нашим [панелям инструментов](~/mac/user-interface/toolbar.md) .

Теперь давайте рассмотрим следующие возможности и действия для элементов панели инструментов и изображения.

[![Создание розеток и действий](copy-paste-images/sample05.png "Создание розеток и действий")](copy-paste-images/sample05-large.png#lightbox)

Дополнительные сведения о работе с розетками и действиями см. в разделе " [розетки и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) " документации по [Hello, Mac](~/mac/get-started/hello-mac.md) .

### <a name="enabling-the-user-interface"></a>Включение пользовательского интерфейса

С помощью нашего пользовательского интерфейса, созданного в Xcode, и нашего элемента пользовательского интерфейса, доступного через функции и действия, необходимо добавить код для включения пользовательского интерфейса. Дважды щелкните файл **ImageWindow.CS** в **панель решения** и сделайте его следующим:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

Подробнее рассмотрим этот код ниже.

Сначала мы предоставляем экземпляр `ImageDocument` класса, который мы создали ранее:

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

С помощью `Export` , `WillChangeValue` и `DidChangeValue` мы настроили `Document` свойство, чтобы разрешить кодирование значений ключа и привязку данных в Xcode.

Мы также предоставляем образ из образа, который мы добавили в пользовательский интерфейс в Xcode со следующим свойством:

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

После загрузки и отображения главного окна мы создадим экземпляр нашего класса и подключим `ImageDocument` к нему образ пользовательского интерфейса с помощью следующего кода:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

Наконец, в ответ на нажатие кнопки Копировать и вставить панели инструментов мы вызываем экземпляр `ImageDocument` класса для выполнения фактической работы:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Включение меню "файл" и "Правка"

Последнее, что нам нужно сделать, — это включить **Новый** пункт меню « **файл** » (для создания новых экземпляров главного окна) и включить пункты « **Вырезать**», « **Копировать** » и « **Вставить** » из меню « **Правка** ».

Чтобы включить **Новый** пункт меню, измените файл **AppDelegate.CS** и добавьте следующий код:

```csharp
public int UntitledWindowCount { get; set;} =1;
...

[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Дополнительные сведения см. в разделе [Работа с несколькими окнами](~/mac/user-interface/window.md) документации по [Windows](~/mac/user-interface/window.md) .

Чтобы включить пункты меню **Вырезать**, **Копировать** и **Вставить** , измените файл **AppDelegate.CS** и добавьте следующий код:

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

Для каждого пункта меню мы получаем текущее, самое верхнее окно ключа и приведено его к нашему `ImageWindow` классу:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

Теперь мы вызываем `ImageDocument` экземпляр класса этого окна для выполнения действий копирования и вставки. Пример: 

```csharp
window.Document.CopyImage (sender);
```

Мы хотим, чтобы **вырезание**, **копирование** и **Вставка** элементов меню были доступны, если на монтажном столе по умолчанию или на изображении текущего активного окна есть графические данные.

Добавим файл **EditMenuDelegate.CS** в проект Xamarin. Mac и сделайте его следующим:

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

Опять же, мы получаем текущее, самое верхнее окно и используем его `ImageDocument` экземпляр класса для просмотра наличия требуемых данных изображения. Затем мы используем `MenuWillHighlightItem` метод, чтобы включить или отключить каждый элемент на основе этого состояния.

Измените файл **AppDelegate.CS** и сделайте так, чтобы `DidFinishLaunching` метод выглядел следующим образом:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Сначала отключите автоматическое включение и отключение пунктов меню в меню Правка. Далее мы присоединяем экземпляр `EditMenuDelegate` класса, который мы создали ранее.

Дополнительные сведения см. в документации по [меню](~/mac/user-interface/menu.md) .

### <a name="testing-the-app"></a>Проверка приложения

Все готово для тестирования приложения. Выполните сборку и запуск приложения и отобразится основной интерфейс:

![Запуск приложения](copy-paste-images/run01.png "Запуск приложения")

При открытии меню Правка Обратите внимание, что команда **Вырезать**, **Копировать** и **Вставить** отключена, так как на изображении нет изображения или на монтажном столе по умолчанию:

![Открытие меню "Правка"](copy-paste-images/run02.png "Открытие меню "Правка"")

Если вы добавите изображение в хорошо созданное изображение и снова откроете меню Правка, теперь элементы будут включены:

![Отображение элементов меню "Правка" включено](copy-paste-images/run03.png "Отображение элементов меню "Правка" включено")

При копировании изображения и выборе пункта **создать** в меню файл можно вставить этот образ в новое окно:

![Вставление изображения в новое окно](copy-paste-images/run04.png "Вставление изображения в новое окно")

В следующих разделах мы рассмотрим работу с монтажным знаком в приложении Xamarin. Mac.

## <a name="about-the-pasteboard"></a>Сведения о монтажном столе

В macOS (прежнее название — OS X) монтажный стол ( `NSPasteboard` ) обеспечивает поддержку нескольких серверных процессов, таких как копирование & вставку, перетаскивание & перетаскивание и службы приложений. В следующих разделах мы подробнее рассмотрим несколько основных концепций монтажного стола.

### <a name="what-is-a-pasteboard"></a>Что такое монтажный стол?

`NSPasteboard`Класс предоставляет стандартизированный механизм обмена данными между приложениями или в рамках конкретного приложения. Основная функция монтажного стола предназначена для обработки операций копирования и вставки:

1. Когда пользователь выбирает элемент в приложении и использует пункт меню **Вырезать** или **Копировать** , на монтажном столе помещаются одно или несколько представлений выбранного элемента.
2. Когда пользователь использует пункт меню « **Вставить** » (в том же приложении или другой), версия данных, которую он может обменяться, копируется из монтажного стола и добавляется в приложение.

Менее очевидные варианты использования монтажного стола включают операции поиска, перетаскивания, перетаскивания и служб приложений.

- Когда пользователь инициирует операцию перетаскивания, данные перетаскивания копируются на монтажный стол. Если операция перетаскивания заканчивается на другое приложение, это приложение копирует данные с монтажного стола.
- Для служб перевода данные, подводимые для преобразования, копируются на монтажный стол с помощью запрашивающего приложения. Служба приложений извлекает данные с монтажного стола, выполняет перевод, а затем возвращает данные обратно на монтажном столе.

В простейшей форме монтажные области используются для перемещения данных внутри конкретного приложения или между приложениями и таким образом находятся в особой глобальной области памяти вне процесса приложения. Хотя понятия, связанные с монтажными столами, легко распознаются, необходимо учитывать несколько более сложных сведений. Эти сведения будут рассмотрены ниже.

### <a name="named-pasteboards"></a>Именованные монтажные столе

Монтажный стол может быть общедоступным или частным и может использоваться для различных целей в приложении или между несколькими приложениями. macOS предоставляет несколько стандартных монтажных столе, каждый из которых имеет определенную четкое использование:

- `NSGeneralPboard` — Монтажный стол по умолчанию для операций **вырезания**, **копирования** и **вставки** .
- `NSRulerPboard` — Поддерживает операции **вырезания**, **копирования** и **вставки** на **линейках**.
- `NSFontPboard` — Поддерживает операции **вырезания**, **копирования** и **вставки** `NSFont` объектов.
- `NSFindPboard` — Поддерживает определенные приложением панели поиска, которые могут совместно использовать искомый текст.
- `NSDragPboard` — Поддерживает операции **перетаскивания &** .

В большинстве случаев используется один из системных монтажных систем. Но могут возникнуть ситуации, требующие создания собственных монтажных столе. В таких ситуациях можно использовать `FromName (string name)` метод `NSPasteboard` класса для создания пользовательского монтажного стола с заданным именем.

При необходимости можно вызвать `CreateWithUniqueName` метод `NSPasteboard` класса, чтобы создать «монтажный стол с уникальным именем».

### <a name="pasteboard-items"></a>Элементы монтажного стола

Каждый фрагмент данных, записываемый приложением на монтажный стол, считается _элементом монтажного стола_ , а монтажный стол может содержать несколько элементов одновременно. Таким образом, приложение может записывать несколько версий данных, копируемых на монтажный стол (например, обычный текст и форматированный текст), а извлечение приложения может считывать только те данные, которые могут быть обработаны (например, только обычный текст).

### <a name="data-representations-and-uniform-type-identifiers"></a>Представления данных и универсальные идентификаторы типов

Операции на монтажном столе обычно выполняются между двумя (или более) приложениями, не имеющими сведений о них или типами данных, которые могут быть обработаны. Как указано в приведенном выше разделе, чтобы максимально увеличить вероятность совместного использования информации, монтажный стол может содержать несколько представлений копируемых и вставляемых данных.

Каждое представление определяется с помощью универсального идентификатора типа (UTI), который представляет собой только простую строку, однозначно идентифицирующую тип отображаемой даты (Дополнительные сведения см. в [обзорной документации по универсальным идентификаторам типов](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) .) 

При создании пользовательского типа данных (например, графического объекта в векторном приложении для рисования) можно создать собственный UTI, чтобы однозначно идентифицировать его в операциях копирования и вставки.

Когда приложение готовится к вставке данных, скопированных с помощью монтажного стола, оно должно найти представление, которое лучше подходит для его возможностей (если таковые имеются). Как правило, это будет самый широкий доступный тип (например, форматированный текст для приложения для обработки текста), что приведет к простейшим формам, доступным в качестве обязательного (обычный текст для простого текстового редактора).

<a name="Promised_Data"></a>

### <a name="promised-data"></a>Обещанные данные

В общем случае, чтобы обеспечить максимально эффективное совместное использование приложений, необходимо предоставить как можно больше представлений копируемых данных. Однако из-за ограничений по времени или памяти может оказаться нецелесообразным написание каждого типа данных на монтажном столе.

В этом случае можно поместить первое представление данных на монтажный стол, и принимающее приложение может запросить другое представление, которое можно создать в режиме реального времени перед операцией вставки.

При помещении начального элемента на монтажном столе будет указано, что одно или несколько других доступных представлений предоставляются объектом, который соответствует `NSPasteboardItemDataProvider` интерфейсу. Эти объекты будут предоставлять дополнительные представления по запросу в соответствии с запросом принимающего приложения.

### <a name="change-count"></a>Число изменений

Каждый монтажный стол поддерживает _число изменений_ , увеличивающееся каждый раз при объявлении нового владельца. Приложение может определить, изменилось ли содержимое монтажного стола с момента последней проверки, проверив значение счетчика изменений.

Используйте `ChangeCount` методы и `ClearContents` `NSPasteboard` класса для изменения счетчика изменений данного монтажного стола.

## <a name="copying-data-to-a-pasteboard"></a>Копирование данных на монтажный стол

Чтобы выполнить копирование, сначала нужно получить доступ к монтажному столу, удалив все существующие содержимое и записав столько данных, сколько необходимо для монтажного стола.

Пример:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

Как правило, вы просто пишете на основной монтажный стол, как мы сделали в приведенном выше примере. Любой объект, отправляемый в `WriteObjects` метод, *должен* соответствовать `INSPasteboardWriting` интерфейсу. Некоторые встроенные классы (такие как,,,, `NSString` `NSImage` `NSURL` `NSColor` `NSAttributedString` и `NSPasteboardItem` ) автоматически соответствуют этому интерфейсу.

При написании пользовательского класса данных на монтажном столе он должен соответствовать `INSPasteboardWriting` интерфейсу или быть заключен в экземпляр `NSPasteboardItem` класса (см. раздел [пользовательские типы данных](#Custom_Data_Types) ниже).

## <a name="reading-data-from-a-pasteboard"></a>Чтение данных с монтажного стола

Как упоминалось выше, чтобы максимально увеличить вероятность совместного использования данных приложениями, на монтажном столе может быть записано несколько представлений копируемых данных. Это приложение может выбрать наиболее широкую версию для своих возможностей (если таковые существуют).

### <a name="simple-paste-operation"></a>Простая операция вставки

Данные из монтажного стола считываются с помощью `ReadObjectsForClasses` метода. Для этого потребуется два параметра:

1. Массив `NSObject` типов, основанных на классе, которые необходимо считать из монтажного стола. Сначала следует упорядочить его с использованием наиболее желаемого типа данных, а остальные типы — в порядке убывания.
2. Словарь, содержащий дополнительные ограничения (например, ограничение на конкретные типы содержимого URL-адресов) или пустой словарь, если дальнейшие ограничения не требуются.

Метод возвращает массив элементов, которые соответствуют заданным условиям, и, следовательно, содержат не более одного и того же числа запрашиваемых типов данных. также возможно, что ни один из запрашиваемых типов не существует и будет возвращен пустой массив.

Например, следующий код проверяет, существует ли объект `NSImage` на монтажном столе, и отображает его в образе, если это происходит:

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>Запрос нескольких типов данных

В зависимости от типа создаваемого приложения Xamarin. Mac оно может иметь возможность обрабатывать несколько представлений вставленных данных. В этом случае для извлечения данных из монтажного стола существует два сценария.

1. Выполните один вызов `ReadObjectsForClasses` метода и предоставьте массив всех требуемых представлений (в предпочтительном порядке).
2. Выполните несколько вызовов метода, `ReadObjectsForClasses` запрашивая другой массив типов каждый раз.

Дополнительные сведения о получении данных из монтажного стола см. в разделе **простая операция вставки** выше.

### <a name="checking-for-existing-data-types"></a>Проверка существующих типов данных

Иногда может потребоваться проверить, содержит ли монтажный стол заданное представление данных, без фактического считывания данных из этого монтажного стола (например, включение пункта меню « **Вставить** » только при наличии допустимых данных).

Вызовите `CanReadObjectForClasses` метод монтажного стола, чтобы узнать, содержит ли он данный тип.

Например, следующий код определяет, содержит ли общий монтажный стол `NSImage` экземпляр:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>Чтение URL-адресов из монтажного стола

В зависимости от функции данного приложения Xamarin. Mac может потребоваться чтение URL-адресов с монтажного стола, но только в том случае, если они соответствуют заданному набору критериев (например, указание файлов или URL-адресов определенного типа данных). В этом случае можно указать дополнительные условия поиска, используя второй параметр `CanReadObjectForClasses` `ReadObjectsForClasses` методов или.

<a name="Custom_Data_Types"></a>

## <a name="custom-data-types"></a>Пользовательские типы данных

Иногда необходимо сохранить собственные пользовательские типы на монтажном столе из приложения Xamarin. Mac. Например, приложение векторного рисования, позволяющее пользователю копировать и вставлять графические объекты.

В этом случае необходимо спроектировать пользовательский класс данных, чтобы он наследовался от, `NSObject` и он соответствует некоторым интерфейсам ( `INSCoding` `INSPasteboardWriting` и `INSPasteboardReading` ). При необходимости можно использовать `NSPasteboardItem` для инкапсуляции копируемых или вставленных данных.

Оба эти варианта будут подробно рассмотрены ниже.

### <a name="using-a-custom-class"></a>Использование пользовательского класса

В этом разделе мы развернув простой пример приложения, созданного в начале этого документа, и добавим пользовательский класс для отслеживания сведений об изображении, которое мы копируем и вставляя в Windows.

Добавьте в проект новый класс и вызовите его **ImageInfo.CS**. Измените файл и сделайте его следующим:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

В следующих разделах мы рассмотрим этот класс подробно.

#### <a name="inheritance-and-interfaces"></a>Наследование и интерфейсы

Прежде чем пользовательский класс данных можно будет записывать в монтажный стол или считывать из него, он должен соответствовать `INSPastebaordWriting` `INSPasteboardReading` интерфейсам и. Кроме того, он должен наследовать от `NSObject` интерфейса, а также соответствовать `INSCoding` интерфейсу:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Класс также должен быть предоставлен для цели-C с помощью `Register` директивы и должен предоставлять все необходимые свойства или методы с помощью `Export` . Пример:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Мы предоставляем два поля данных, которые будет содержать этот класс — имя изображения и его тип (JPG, PNG и т. д.). 

Дополнительные сведения см. в разделе [предоставление классов и методов C# для цели-c](~/mac/internals/how-it-works.md) в документации по [внутренним компонентам Xamarin. Mac](~/mac/internals/how-it-works.md) . здесь объясняются `Register` атрибуты и, `Export` используемые для подключения классов c# к объектам и элементам пользовательского интерфейса на языке c.

#### <a name="constructors"></a>Конструкторы

Для нашего пользовательского класса данных требуется два конструктора (должным образом предоставлены цели-C), чтобы его можно было читать из монтажного стола:

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

Во-первых, мы предоставляем _пустой_ конструктор в методе по умолчанию — C `init` .

Далее мы предоставляем `NSCoding` соответствующий конструктор, который будет использоваться для создания нового экземпляра объекта с помощью монтажного стола при вставлении под экспортированным именем `initWithCoder` .

Этот конструктор принимает объект `NSCoder` (созданный `NSKeyedArchiver` при записи на монтажном столе), извлекает пары "ключ-значение" и сохраняет их в поля свойств класса данных.

#### <a name="writing-to-the-pasteboard"></a>Запись на монтажный стол

При `INSPasteboardWriting` условии соответствия интерфейсу необходимо предоставить два метода и, при необходимости, третий метод, чтобы класс можно было записать на монтажный стол.

Во первых, нам нужно сообщить монтажному столу, какие представления типов данных можно записать в пользовательский класс:

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Каждое представление определяется с помощью универсального идентификатора типа (UTI), который представляет собой только простую строку, однозначно идентифицирующую тип представляемых данных (Дополнительные сведения см. в [обзорной документации по универсальным идентификаторам типов](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) .)

Для нашего пользовательского формата мы создаем собственный UTI: "com. Xamarin. Image-info" (Обратите внимание, что в обратную нотацию аналогично идентификатору приложения). Наш класс также может писать стандартную строку на монтажном столе ( `public.text` ). 

Далее необходимо создать объект в запрошенном формате, который фактически записывается на монтажный стол.

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

Для `public.text` типа возвращается простой отформатированный `NSString` объект. Для пользовательского `com.xamarin.image-info` типа мы используем `NSKeyedArchiver` интерфейс и, `NSCoder` чтобы кодировать пользовательский класс данных в парный Архив пар "ключ — значение". Для фактической работы с кодировкой нам потребуется реализовать следующий метод:

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Отдельные пары "ключ-значение" записываются в кодировщик и будут декодированы с помощью второго конструктора, добавленного выше.

При необходимости можно включить следующий метод для определения любых параметров при записи данных на монтажный стол:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

В настоящее время `WritingPromised` доступен только параметр, который должен использоваться, если заданный тип является только обещанным и не записывается на монтажный стол. Дополнительные сведения см. в разделе « [обещанные данные](#Promised_Data) » выше.

Используя эти методы, можно использовать следующий код для записи нашего пользовательского класса на монтажный стол:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Чтение с монтажного стола

В соответствие с `INSPasteboardReading` интерфейсом необходимо предоставить три метода, чтобы пользовательский класс данных можно было читать из монтажного стола.

Во первых, нам нужно сообщить монтажному столу, какие представления типов данных может считывать настраиваемый класс из буфера обмена:

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Опять же, они определяются как простые UTI и те же типы, которые мы определили в разделе « **запись в монтажный стол** » выше.

Далее необходимо указать монтажному столу, _как_ будут считываться каждый из типов UTI с помощью следующего метода:

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

Для `com.xamarin.image-info` типа мы сообщаем монтажному столу декодировать пару «ключ-значение», созданную `NSKeyedArchiver` при записи класса на монтажный стол, вызвав `initWithCoder:` конструктор, добавленный в класс.

Наконец, необходимо добавить следующий метод для чтения других представлений данных UTI из монтажного стола:

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

При наличии всех этих методов пользовательский класс данных можно считать с помощью монтажного стола, используя следующий код:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>Использование Нспастебоардитем

Иногда приходится писать на монтажный стол пользовательские элементы, которые не гарантируют создание пользовательского класса или вы хотите предоставить данные в общем формате, только по мере необходимости. В таких ситуациях можно использовать `NSPasteboardItem` .

`NSPasteboardItem`Предоставляет точный контроль над данными, записываемыми на монтажный стол, и предназначен для временного доступа. он должен быть удален после того, как он будет записан на монтажный стол.

#### <a name="writing-data"></a>Запись данных

Чтобы записать пользовательские данные в, необходимо `NSPasteboardItem` предоставить пользовательский `NSPasteboardItemDataProvider` . Добавьте в проект новый класс и вызовите его **ImageInfoDataProvider.CS**. Измените файл и сделайте его следующим:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

Как и в случае с пользовательским классом данных, нам нужно использовать `Register` `Export` директивы и для предоставления его цели — C. Класс должен наследовать от `NSPasteboardItemDataProvider` и должен реализовывать `FinishedWithDataProvider` методы и `ProvideDataForType` .

Используйте `ProvideDataForType` метод для предоставления данных, которые будут заключены в оболочку, `NSPasteboardItem` следующим образом:

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

В этом случае мы сохраняем два фрагмента информации о нашем образе (Name и ImageType) и запишем их в простую строку ( `public.text` ).

Введите запись данных на монтажном столе, используя следующий код:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>Чтение данных

Для считывания данных с монтажного стола используйте следующий код:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с монтажным знаком в приложении Xamarin. Mac для поддержки операций копирования и вставки. Во-первых, в нем появился простой пример, получающий представление о стандартных монтажных столах. Далее на монтажном столе потребовался подробный обзор, а также чтение и запись данных из него. Наконец, он рассматривал использование пользовательского типа данных для поддержки копирования и вставления сложных типов данных в приложении.

## <a name="related-links"></a>Связанные ссылки

- [Маккопипасте (пример)](/samples/xamarin/mac-samples/maccopypaste)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Путеводитель по программированию для монтажного стола](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [Рекомендации по созданию пользовательских интерфейсов в macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)