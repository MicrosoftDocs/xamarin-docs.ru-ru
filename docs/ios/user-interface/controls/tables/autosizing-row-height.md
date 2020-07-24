---
title: Автоматическое изменение высоты строки в Xamarin. iOS
description: В этом документе описывается, как добавить в таблицу приложений Xamarin. iOS представления строк, высота которых зависит от содержимого. В нем рассматривается Макет ячеек в конструкторе iOS и включается Автомасштабирование высоты.
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 03e6a513f367be4c19f925427c8a896b1fc87107
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996335"
---
# <a name="auto-sizing-row-height-in-xamarinios"></a>Автоматическое изменение высоты строки в Xamarin. iOS

Начиная с iOS 8, компания Apple добавила возможность создания табличного представления ( `UITableView` ), которое может автоматически увеличивать и уменьшать высоту заданной строки на основе размера содержимого с помощью автоматического макета, классов размеров и ограничений.

в iOS 11 добавлена возможность автоматического расширения строк. Размеры верхних, нижних колонтитулов и ячеек теперь можно изменять автоматически в зависимости от их содержимого. Однако если таблица создана в конструкторе iOS, Interface Builder или если она имеет фиксированную высоту строк, необходимо вручную включить автоматическое изменение размера ячеек, как описано в этом разделе.

## <a name="cell-layout-in-the-ios-designer"></a>Макет ячеек в конструкторе iOS

Откройте раскадровку для табличного представления, для которой нужно Автоподбор размера строки в конструкторе iOS, выберите *прототип* ячейки и разработайте макет ячейки. Например.

[![Конструкция прототипа ячейки](autosizing-row-height-images/table01.png)](autosizing-row-height-images/table01.png#lightbox)

Для каждого элемента в прототипе добавьте ограничения для сохранения элементов в правильном положении при изменении размеров представления таблицы для вращения или различных размеров экрана устройства iOS. Например, закрепите в `Title` верхней, левой и правой части *представления содержимого*ячейки:

[![Закрепление заголовка в верхней, левой и правой части представления содержимого ячеек](autosizing-row-height-images/table02.png)](autosizing-row-height-images/table02.png#lightbox)

В нашем примере таблицы мелким `Label` (под `Title` ) является поле, которое может сжиматься и увеличиваться для увеличения или уменьшения высоты строки. Чтобы добиться этого результата, добавьте следующие ограничения, чтобы закрепить левую, правую, верхнюю и нижнюю части Метки:

[![Эти ограничения позволяют закрепить левую, правую, верхнюю и нижнюю части метки](autosizing-row-height-images/table03.png)](autosizing-row-height-images/table03.png#lightbox)

Теперь, когда мы полностью ограничены элементами в ячейке, необходимо уточнить, какой элемент следует растянуть. Для этого задайте приоритет **Хуггинг содержимого** и **Сжатие содержимого** , как это необходимо в разделе **макета** панель свойств.

[![Раздел макета Панель свойств](autosizing-row-height-images/table03a.png)](autosizing-row-height-images/table03a.png#lightbox)

Задайте для элемента, который требуется расширить, значение приоритета **хуггинг и более** **низкое** значение приоритета сопротивления.

Далее необходимо выбрать прототип ячейки и присвоить ему уникальный **идентификатор**:

[![Присвоение прототипу ячейки уникального идентификатора](autosizing-row-height-images/table04.png)](autosizing-row-height-images/table04.png#lightbox)

В нашем примере — `GrowCell` . Это значение будет использоваться позже при заполнении таблицы.

> [!IMPORTANT]
> Если таблица содержит более одного типа ячеек (**прототип**), необходимо убедиться, что каждый тип имеет уникальное `Identifier` значение, чтобы автоматическое изменение размера строк работало.

Для каждого элемента прототипа ячейки присвойте **имя** , чтобы предоставить его коду C#. Например.

[![Назначение имени для предоставления его коду C#](autosizing-row-height-images/table05.png)](autosizing-row-height-images/table05.png#lightbox)

Затем добавьте пользовательский класс для `UITableViewController` , `UITableView` и `UITableCell` (прототип). Например.

[![Добавление пользовательского класса для Уитаблевиевконтроллер, Уитаблевиев и Уитаблецелл](autosizing-row-height-images/table06.png)](autosizing-row-height-images/table06.png#lightbox)

Наконец, чтобы убедиться, что в нашей метке отображается все ожидаемое содержимое, задайте для свойства **строки** значение `0` :

[![Свойство Lines, установленное в значение 0](autosizing-row-height-images/table06.png)](autosizing-row-height-images/table06a.png#lightbox)

Определив пользовательский интерфейс, добавим код, чтобы включить автоматическое изменение высоты строк.

## <a name="enabling-auto-resizing-height"></a>Включение автоподбора высоты

В представлении DataSource ( `UITableViewDatasource` ) или Source () нашего табличного представления `UITableViewSource` , когда мы выведем из очереди ячейку, нам нужно использовать тот `Identifier` , который мы определили в конструкторе. Например.

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

По умолчанию табличное представление будет настроено для автоподбора высоты строки. Чтобы убедиться в этом, `RowHeight` свойству следует присвоить значение `UITableView.AutomaticDimension` . Также необходимо задать `EstimatedRowHeight` свойство в нашем `UITableViewController` . Например.

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

Эта оценка не должна быть точной, просто приблизительная оценка средней высоты каждой строки в табличном представлении.

При использовании этого кода при запуске приложения каждая строка будет сжиматься и увеличиваться в зависимости от высоты последней метки в прототипе ячейки. Например.

[![Пример выполнения таблицы](autosizing-row-height-images/table07.png)](autosizing-row-height-images/table07.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Гровровтабле (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/growrowtable)
