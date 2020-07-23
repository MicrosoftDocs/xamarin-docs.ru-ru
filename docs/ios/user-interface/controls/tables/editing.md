---
title: Редактирование таблиц с помощью Xamarin. iOS
description: В этом документе описывается изменение таблиц в Xamarin. iOS. В нем обсуждается прокрутка для удаления, режима редактирования и вставки строк.
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: c022c1af78a5a3800cd61096c3f142c1ed0235e7
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930940"
---
# <a name="editing-tables-with-xamarinios"></a>Редактирование таблиц с помощью Xamarin. iOS

Функции редактирования таблиц включаются переопределением методов в `UITableViewSource` подклассе. Простейшим поведением при редактировании является жест считывания и удаления, который можно реализовать с помощью одного переопределения метода.
Более сложное редактирование (включая перемещение строк) можно выполнить с помощью таблицы в режиме редактирования.

## <a name="swipe-to-delete"></a>Прокрутить для удаления

Функция прокрутки и удаления — это естественный жест в iOS, который пользователи хотят. 

 [![Пример прокрутки для удаления](editing-images/image10.png)](editing-images/image10.png#lightbox)

Существует три переопределения метода, которые влияют на жест прокрутки для отображения кнопки **удаления** в ячейке:

- **Коммитедитингстиле** — источник таблицы определяет, переопределяется ли этот метод, и автоматически включает жест прокрутки на удаление. Реализация метода должна вызывать метод `DeleteRows` , `UITableView` чтобы привести к исчезновению ячеек, а также удалить базовые данные из модели (например, массив, словарь или базу данных). 
- **Канедитров** — при переопределении коммитедитингстиле все строки считаются изменяемыми. Если этот метод реализован и возвращает значение false (для некоторых определенных строк или для всех строк), то жест прокрутки на удаление не будет доступен в этой ячейке. 
- **Титлефорделетеконфирматион** — при необходимости задает текст для кнопки **Удалить** . Если этот метод не реализован, текст кнопки будет "Удалить". 

Эти методы реализуются в `TableSource` классе следующим образом:

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

В этом примере был `UITableViewSource` обновлен для использования в `List<TableItem>` качестве источника данных (вместо массива строк), так как он поддерживает добавление и удаление элементов из коллекции.

## <a name="edit-mode"></a>Режим правки

Когда таблица находится в режиме редактирования, пользователь видит в каждой строке красный мини-приложение "останавливаться", которое при наличии кнопки открывает кнопку "Удалить". В таблице также отображается значок Handle, указывающий на возможность перетаскивания строки для изменения порядка.
Пример **таблидитмоде** реализует эти функции, как показано ниже.

 [![Пример Таблидитмоде реализует эти функции, как показано ниже.](editing-images/image11.png)](editing-images/image11.png#lightbox)

Существует ряд различных методов `UITableViewSource` , которые влияют на поведение режима изменения таблицы:

- **Канедитров** — возможность изменения каждой строки. Возвращает значение false, чтобы предотвратить одновременное считывание и удаление в режиме редактирования. 
- **Канмоверов** — возвращает значение true, чтобы разрешить перемещение "Handle" или "false", чтобы предотвратить перемещение. 
- **Едитингстилефорров** — когда таблица находится в режиме редактирования, возвращаемое значение этого метода определяет, отображает ли ячейка красный значок удаления или зеленый значок "Добавить". Возвращает `UITableViewCellEditingStyle.None` , если строка не должна быть редактируемой. 
- **Моверов** — вызывается при перемещении строки, чтобы базовая структура данных могла быть изменена в соответствии с данными, как она отображается в таблице. 

Реализация для первых трех методов относительно проста, если вы не хотите использовать `indexPath` метод для изменения поведения конкретных строк, просто жестко возвращаемые значения для всей таблицы.

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

`MoveRow`Реализация немного сложнее, так как ей нужно изменить базовую структуру данных, чтобы она соответствовала новому заказу. Поскольку данные реализуются как приведенный `List` ниже код, удаляет элемент данных в старом расположении и вставляет его в новое место. Если данные хранились в таблице базы данных SQLite со столбцом Order (например,), этому методу потребуется выполнить некоторые операции SQL для изменения порядка чисел в этом столбце.

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

Наконец, чтобы получить таблицу в режиме редактирования, кнопке " **изменить** " необходимо вызвать метод `SetEditing` Like следующим образом.

```csharp
table.SetEditing (true, true);
```

После того как пользователь завершит редактирование, кнопка **done** должна отключить режим редактирования:

```csharp
table.SetEditing (false, true);
```

## <a name="row-insertion-editing-style"></a>Стиль редактирования вставки строк

Вставка строк из таблицы является нетипичным пользовательским интерфейсом. Основным примером в стандартных приложениях iOS является экран **Изменение контакта** . На этом снимке экрана показано, как работает функция вставки строк — в режиме редактирования имеется дополнительная строка, которая (при нажатии) вставляет дополнительные строки в данные. По завершении редактирования временная строка **(Добавление новой)** удаляется.

 [![После завершения редактирования удаляется временная строка добавления новой строки.](editing-images/image12.png)](editing-images/image12.png#lightbox)

Существует ряд различных методов `UITableViewSource` , которые влияют на поведение режима изменения таблицы. Эти методы были реализованы следующим образом в примере кода:

- **Едитингстилефорров** — возвращает `UITableViewCellEditingStyle.Delete` для строк, содержащих данные, и возвращается `UITableViewCellEditingStyle.Insert` для последней строки (которая будет добавлена специально для поведения кнопки вставки). 
- **Кустомиземоветаржет** — в то время как пользователь перемещает ячейку, возвращаемое значение из этого необязательного метода может переопределить свой вариант расположения. Это означает, что можно предотвратить удаление ячейки в определенных позициях, например в этом примере, предотвращающем перемещение любой строки после строки **(Добавление новой)** . 
- **Канмоверов** — возвращает значение true, чтобы разрешить перемещение "Handle" или "false", чтобы предотвратить перемещение. В этом примере последняя строка имеет скрытый элемент "Handle", так как он предназначен только для вставки в качестве кнопки "вставить". 

Мы также добавим два пользовательских метода для добавления строки "Insert", а затем удаляем ее снова, когда она больше не нужна. Они вызываются с помощью кнопок " **изменить** " и " **Готово** ":

- **Виллбегинтаблидитинг** — при постановке кнопки " **изменить** " она вызывает метод `SetEditing` , чтобы перевести таблицу в режим редактирования. Это активирует метод Виллбегинтаблидитинг, где в конце таблицы отображается строка **(добавить новую)** , которая будет использоваться в качестве кнопки "вставить". 
- **Дидфиништаблидитинг** — когда затронута кнопка Done (Готово), `SetEditing` вызывается снова, чтобы отключить режим редактирования. В примере кода удаляется строка **(добавить новую)** из таблицы, когда редактирование больше не требуется. 

Эти переопределения методов реализуются в примере файла **таблидитмодеадд/Code/таблесаурце. CS**:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

Эти два настраиваемых метода используются для добавления и удаления строки **(Добавление новой)** при включении или отключении режима редактирования таблицы.

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

Наконец, этот код создает экземпляры кнопок " **изменить** " и " **Готово** " с лямбда-выражениями, которые включают или отключают режим изменения, когда они затронуты:

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

Этот шаблон пользовательского интерфейса вставки строк не используется очень часто, однако можно также использовать `UITableView.BeginUpdates` `EndUpdates` методы и для анимации вставки или удаления ячеек в любой таблице. Правило использования этих методов заключается в том, что разница в значении, возвращаемой `RowsInSection` между `BeginUpdates` `EndUpdates` вызовами и, должна совпадать с количеством ячеек, добавленных или удаленных с помощью `InsertRows` `DeleteRows` методов и. Если базовый источник данных не изменяется в соответствии с операциями вставки или удаления в табличном представлении, возникнет ошибка.

## <a name="related-links"></a>Связанные ссылки

- [Воркингвистаблес (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
