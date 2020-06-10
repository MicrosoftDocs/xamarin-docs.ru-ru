---
title: Списки источников в Xamarin. Mac
description: В этой статье рассматривается работа с исходными списками в приложении Xamarin. Mac. Здесь описывается создание и обслуживание списков источников в Xcode и Interface Builder и взаимодействие с ними в коде C#.
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: aed11ff8794f79ff7e16fbb8401a95a5697d2d4b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572069"
---
# <a name="source-lists-in-xamarinmac"></a>Списки источников в Xamarin. Mac

_В этой статье рассматривается работа с исходными списками в приложении Xamarin. Mac. Здесь описывается создание и обслуживание списков источников в Xcode и Interface Builder и взаимодействие с ними в коде C#._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же исходным спискам, что и разработчик, работающий на *уровне цели-C* и *Xcode* . Так как Xamarin. Mac интегрируется напрямую с Xcode, можно использовать _Interface Builder_ Xcode для создания и обслуживания списков источников (или при необходимости создавать их непосредственно в коде C#).

Исходный список — это особый тип представления структуры, используемый для отображения источника действия, например боковой панели в Finder или iTunes.

[![](source-list-images/source05.png "An example source list")](source-list-images/source05.png#lightbox)

В этой статье рассматриваются основы работы с исходными списками в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в этой статье.

Возможно, вы захотите ознакомиться с [предоставлением классов и методов C# для цели-c](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) . здесь также объясняются `Register` команды и, `Export` используемые для подключения классов c# к объектам и элементам пользовательского интерфейса на языке c.

<a name="Introduction_to_Outline_Views"></a>

## <a name="introduction-to-source-lists"></a>Общие сведения о списках источников

Как упоминалось выше, исходный список представляет собой особый тип представления структуры, используемый для отображения источника действия, например боковой панели в Finder или iTunes. Исходный список — это тип таблицы, позволяющий пользователю разворачивать или сворачивать строки иерархических данных. В отличие от табличного представления элементы исходного списка не расположены в плоском списке, они организованы в виде иерархии, например файлов и папок на жестком диске. Если элемент в исходном списке содержит другие элементы, он может быть развернут или свернут пользователем.

Исходный список представляет собой специально оформленное представление структуры ( `NSOutlineView` ), которое само по себе является подклассом табличного представления ( `NSTableView` ) и, как таковое, наследует большую часть его поведения от родительского класса. В результате многие операции, поддерживаемые представлением структуры, также поддерживаются исходным списком. Приложение Xamarin. Mac управляет этими функциями и может настраивать параметры исходного списка (в коде или Interface Builder), чтобы разрешить или запретить определенные операции.

Исходный список не хранит собственные данные, вместо этого он использует источник данных ( `NSOutlineViewDataSource` ) для предоставления требуемых строк и столбцов по мере необходимости.

Поведение исходного списка можно настроить, предоставив подкласс делегата представления структуры ( `NSOutlineViewDelegate` ) для поддержки типа структуры для выбора функциональности, выбора и изменения элементов, а также пользовательских представлений для отдельных элементов.

Так как исходный список использует большую часть поведения и функций с табличным представлением и представлением структуры, перед продолжением работы с этой статьей может потребоваться перейти к документации по [представлениям таблиц](~/mac/user-interface/table-view.md) и [представлениям структуры](~/mac/user-interface/outline-view.md) .

<a name="Working_with_Source_Lists"></a>

## <a name="working-with-source-lists"></a>Работа с списками источников

Исходный список — это особый тип представления структуры, используемый для отображения источника действия, например боковой панели в Finder или iTunes. В отличие от представлений структуры, прежде чем определить список источников в Interface Builder, создадим резервные классы в Xamarin. Mac.

Сначала создадим новый `SourceListItem` класс для хранения данных для нашего исходного списка. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить**  >  **новый файл...** Выберите **Общий**  >  **пустой класс**, введите `SourceListItem` для **имени** и нажмите кнопку **создать** :

[![](source-list-images/source01.png "Adding an empty class")](source-list-images/source01.png#lightbox)

Сделайте так, чтобы `SourceListItem.cs` файл выглядел следующим образом: 

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListItem: NSObject, IEnumerator, IEnumerable
    {
        #region Private Properties
        private string _title;
        private NSImage _icon;
        private string _tag;
        private List<SourceListItem> _items = new List<SourceListItem> ();
        #endregion

        #region Computed Properties
        public string Title {
            get { return _title; }
            set { _title = value; }
        }

        public NSImage Icon {
            get { return _icon; }
            set { _icon = value; }
        }

        public string Tag {
            get { return _tag; }
            set { _tag=value; }
        }
        #endregion

        #region Indexer
        public SourceListItem this[int index]
        {
            get
            {
                return _items[index];
            }

            set
            {
                _items[index] = value;
            }
        }

        public int Count {
            get { return _items.Count; }
        }

        public bool HasChildren {
            get { return (Count > 0); }
        }
        #endregion

        #region Enumerable Routines
        private int _position = -1;

        public IEnumerator GetEnumerator()
        {
            _position = -1;
            return (IEnumerator)this;
        }

        public bool MoveNext()
        {
            _position++;
            return (_position < _items.Count);
        }

        public void Reset()
        {_position = -1;}

        public object Current
        {
            get 
            { 
                try 
                {
                    return _items[_position];
                }

                catch (IndexOutOfRangeException)
                {
                    throw new InvalidOperationException();
                }
            }
        }
        #endregion

        #region Constructors
        public SourceListItem ()
        {
        }

        public SourceListItem (string title)
        {
            // Initialize
            this._title = title;
        }

        public SourceListItem (string title, string icon)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
        }

        public SourceListItem (string title, string icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
        }

        public SourceListItem (string title, NSImage icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon, string tag)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
        }

        public SourceListItem (string title, NSImage icon, string tag, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
            this.Clicked = clicked;
        }
        #endregion

        #region Public Methods
        public void AddItem(SourceListItem item) {
            _items.Add (item);
        }

        public void AddItem(string title) {
            _items.Add (new SourceListItem (title));
        }

        public void AddItem(string title, string icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, string icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, NSImage icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon, string tag) {
            _items.Add (new SourceListItem (title, icon, tag));
        }

        public void AddItem(string title, NSImage icon, string tag, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, tag, clicked));
        }

        public void Insert(int n, SourceListItem item) {
            _items.Insert (n, item);
        }

        public void RemoveItem(SourceListItem item) {
            _items.Remove (item);
        }

        public void RemoveItem(int n) {
            _items.RemoveAt (n);
        }

        public void Clear() {
            _items.Clear ();
        }
        #endregion

        #region Events
        public delegate void ClickedDelegate();
        public event ClickedDelegate Clicked;

        internal void RaiseClickedEvent() {
            // Inform caller
            if (this.Clicked != null)
                this.Clicked ();
        }
        #endregion
    }
}
```

В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить**  >  **новый файл...** Выберите **Общий**  >  **пустой класс**, введите `SourceListDataSource` в качестве **имени** и нажмите кнопку **создать** . Сделайте так, чтобы `SourceListDataSource.cs` файл выглядел следующим образом:

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDataSource : NSOutlineViewDataSource
    {
        #region Private Variables
        private SourceListView _controller;
        #endregion

        #region Public Variables
        public List<SourceListItem> Items = new List<SourceListItem>();
        #endregion

        #region Constructors
        public SourceListDataSource (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Properties
        public override nint GetChildrenCount (NSOutlineView outlineView, Foundation.NSObject item)
        {
            if (item == null) {
                return Items.Count;
            } else {
                return ((SourceListItem)item).Count;
            }
        }

        public override bool ItemExpandable (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, Foundation.NSObject item)
        {
            if (item == null) {
                return Items [(int)childIndex];
            } else {
                return ((SourceListItem)item) [(int)childIndex]; 
            }
        }

        public override NSObject GetObjectValue (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            return new NSString (((SourceListItem)item).Title);
        }
        #endregion

        #region Internal Methods
        internal SourceListItem ItemForRow(int row) {
            int index = 0;

            // Look at each group
            foreach (SourceListItem item in Items) {
                // Is the row inside this group?
                if (row >= index && row <= (index + item.Count)) {
                    return item [row - index - 1];
                }

                // Move index
                index += item.Count + 1;
            }

            // Not found 
            return null;
        }
        #endregion
    }
}
```

При этом будут предоставлены данные для нашего исходного списка.

В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить**  >  **новый файл...** Выберите **Общий**  >  **пустой класс**, введите `SourceListDelegate` в качестве **имени** и нажмите кнопку **создать** . Сделайте так, чтобы `SourceListDelegate.cs` файл выглядел следующим образом:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDelegate : NSOutlineViewDelegate
    {
        #region Private variables
        private SourceListView _controller;
        #endregion

        #region Constructors
        public SourceListDelegate (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Methods
        public override bool ShouldEditTableColumn (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            return false;
        }

        public override NSCell GetCell (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            nint row = outlineView.RowForItem (item);
            return tableColumn.DataCellForRow (row);
        }

        public override bool IsGroupItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            NSTableCellView view = null;

            // Is this a group item?
            if (((SourceListItem)item).HasChildren) {
                view = (NSTableCellView)outlineView.MakeView ("HeaderCell", this);
            } else {
                view = (NSTableCellView)outlineView.MakeView ("DataCell", this);
                view.ImageView.Image = ((SourceListItem)item).Icon;
            }

            // Initialize view
            view.TextField.StringValue = ((SourceListItem)item).Title;

            // Return new view
            return view;
        }

        public override bool ShouldSelectItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return (outlineView.GetParent (item) != null);
        }

        public override void SelectionDidChange (NSNotification notification)
        {
            NSIndexSet selectedIndexes = _controller.SelectedRows;

            // More than one item selected?
            if (selectedIndexes.Count > 1) {
                // Not handling this case
            } else {
                // Grab the item
                var item = _controller.Data.ItemForRow ((int)selectedIndexes.FirstIndex);

                // Was an item found?
                if (item != null) {
                    // Fire the clicked event for the item
                    item.RaiseClickedEvent ();

                    // Inform caller of selection
                    _controller.RaiseItemSelected (item);
                }
            }
        }
        #endregion
    }
}
```

Это обеспечит поведение нашего исходного списка.

Наконец, в **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить**  >  **новый файл...** Выберите **Общий**  >  **пустой класс**, введите `SourceListView` в качестве **имени** и нажмите кнопку **создать** . Сделайте так, чтобы `SourceListView.cs` файл выглядел следующим образом:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }
        
        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

При этом создается пользовательский подкласс повторного использования `NSOutlineView` ( `SourceListView` ), который можно использовать для создания исходного списка в любом приложении Xamarin. Mac, которое мы делаем.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode"></a>

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Создание и обслуживание списков источников в Xcode

Теперь давайте разработаем список источников в Interface Builder. Дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder и перетащите разделенное представление из **инспектора библиотек**, добавьте его в контроллер представления и настройте его для изменения размера с помощью представления в **редакторе ограничений**:

[![](source-list-images/source00.png "Editing constraints")](source-list-images/source00.png#lightbox)

Затем перетащите исходный список из **инспектора библиотеки**, добавьте его в левую часть разделенного представления и задайте для него изменение размера с помощью представления в **редакторе ограничений**:

[![](source-list-images/source02.png "Editing constraints")](source-list-images/source02.png#lightbox)

Затем перейдите в **представление удостоверений**, выберите исходный список и измените **класс** на `SourceListView` :

[![](source-list-images/source03.png "Setting the class name")](source-list-images/source03.png#lightbox)

Наконец, создайте **выход** для нашего исходного списка, вызываемого `SourceList` в `ViewController.h` файле:

[![](source-list-images/source04.png "Configuring an Outlet")](source-list-images/source04.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

<a name="Populating the Source List"></a>

## <a name="populating-the-source-list"></a>Заполнение исходного списка

Давайте изменим `RotationWindow.cs` файл в Visual Studio для Mac и сделайте его `AwakeFromNib` метод следующим:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Populate source list
    SourceList.Initialize ();

    var library = new SourceListItem ("Library");
    library.AddItem ("Venues", "house.png", () => {
        Console.WriteLine("Venue Selected");
    });
    library.AddItem ("Singers", "group.png");
    library.AddItem ("Genre", "cards.png");
    library.AddItem ("Publishers", "box.png");
    library.AddItem ("Artist", "person.png");
    library.AddItem ("Music", "album.png");
    SourceList.AddItem (library);

    // Add Rotation 
    var rotation = new SourceListItem ("Rotation"); 
    rotation.AddItem ("View Rotation", "redo.png");
    SourceList.AddItem (rotation);

    // Add Kiosks
    var kiosks = new SourceListItem ("Kiosks");
    kiosks.AddItem ("Sign-in Station 1", "imac");
    kiosks.AddItem ("Sign-in Station 2", "ipad");
    SourceList.AddItem (kiosks);

    // Display side list
    SourceList.ReloadData ();
    SourceList.ExpandItem (null, true);

}
```

`Initialize ()`Метод должен вызываться для **выхода** из исходного списка _перед_ добавлением к нему элементов. Для каждой группы элементов мы создадим родительский элемент, а затем добавим подэлементы к этому элементу группы. Затем каждая группа добавляется в коллекцию исходного списка `SourceList.AddItem (...)` . Последние две строки загружают данные для исходного списка и разворачивают все группы:

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Наконец, измените `AppDelegate.cs` файл и сделайте `DidFinishLaunching` метод следующим:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

При запуске приложения будут отображены следующие:

[![](source-list-images/source05.png "An example app run")](source-list-images/source05.png#lightbox)

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с исходными списками в приложении Xamarin. Mac. Мы узнали, как создавать и обслуживать списки источников в Interface Builder Xcode и как работать с исходными списками в коде C#.

## <a name="related-links"></a>Связанные ссылки

- [Макаутлинес (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Представления таблиц](~/mac/user-interface/table-view.md)
- [Представления структур](~/mac/user-interface/outline-view.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Общие сведения о представлениях структуры](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [нсаутлиневиев](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [нсаутлиневиевдатасаурце](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [нсаутлиневиевделегате](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
