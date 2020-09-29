---
title: Перетаскивание в Xamarin. iOS
description: В этом документе описывается, как реализовать перетаскивание в приложениях Xamarin. iOS с помощью API, появившихся в iOS 11. В частности, в нем обсуждается включение перетаскивания в Уитаблевиев.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/05/2017
ms.openlocfilehash: bbdfe9682474352fab7c0ae3b1cd7d949cbca0b4
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437166"
---
# <a name="drag-and-drop-in-xamarinios"></a>Перетаскивание в Xamarin. iOS

_Реализация перетаскивания для iOS 11_

iOS 11 включает поддержку перетаскивания для копирования данных между приложениями на iPad. Пользователи могут выбирать и перетаскивать все типы содержимого из приложений, расположенных параллельно, или путем перетаскивания значка приложения, который запускает приложение для открытия и разрешает удаление данных:

![Перетаскивание примера из пользовательского приложения в приложение "Примечания"](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Перетаскивание доступно только в том же приложении на iPhone.

Рассмотрите возможность создания или редактирования содержимого, поддерживающего операции перетаскивания.

- Текстовые элементы управления поддерживают перетаскивание для всех приложений, созданных в iOS 11, без дополнительной работы.
- Табличные представления и представления коллекций включают улучшения iOS 11, упрощающие добавление поведения перетаскивания.
- Любое другое представление можно сделать для поддержки перетаскивания с дополнительной настройкой.

При добавлении в приложения поддержки перетаскивания можно обеспечить различные уровни качества содержимого. Например, можно предоставить как форматированный текст, так и текст в виде обычного текста, чтобы принимающее приложение могло выбрать, которое лучше подходит для цели перетаскивания. Кроме того, можно настроить визуализацию перетаскивания, а также включить одновременное перетаскивание нескольких элементов.

## <a name="drag-and-drop-with-text-controls"></a>Перетаскивание с помощью текстовых элементов управления

`UITextView` и `UITextField` автоматически поддерживают перетаскивание выделенного текста и удаление текстового содержимого в.

<a name="uitableview"></a>

## <a name="drag-and-drop-with-uitableview"></a>Перетаскивание с помощью Уитаблевиев

`UITableView` имеет встроенную обработку для перетаскивания и перетаскивания строк таблицы, что требует лишь нескольких методов включения поведения по умолчанию.

Существует два интерфейса:

- `IUITableViewDragDelegate` — Сведения о пакетах при инициировании перетаскивания в табличном представлении.
- `IUITableViewDropDelegate` — Обрабатывает сведения при попыток и завершении удаления.

В [примере драганддроптаблевиев](/samples/xamarin/ios-samples/ios11-draganddroptableview) эти два интерфейса реализуются в `UITableViewController` классе вместе с делегатом и источником данных. Они назначаются в `ViewDidLoad` методе:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Ниже описан минимальный необходимый код для этих двух интерфейсов.

### <a name="table-view-drag-delegate"></a>Делегат перетаскивания табличного представления

Единственный метод, _необходимый_ для поддержки перетаскивания строки из табличного представления, — это `GetItemsForBeginningDragSession` . Если пользователь начинает перетаскивать строку, будет вызван этот метод.

Реализация показана ниже. Он извлекает данные, связанные с перетаскиваемых строкой, кодирует их и настраивает, `NSItemProvider` который определяет, каким образом приложения будут выполнять "Drop" части операции (например, могут ли они управлять типом данных, `PlainText` в примере):

```csharp
public UIDragItem[] GetItemsForBeginningDragSession (UITableView tableView,
  IUIDragSession session, NSIndexPath indexPath)
{
  // gets the 'information' to be dragged
  var placeName = model.PlaceNames[indexPath.Row];
  // convert to NSData representation
  var data = NSData.FromString(placeName, NSStringEncoding.UTF8);
  // create an NSItemProvider to describe the data
  var itemProvider = new NSItemProvider();
  itemProvider.RegisterDataRepresentation(UTType.PlainText,
                                NSItemProviderRepresentationVisibility.All,
                                (completion) =>
  {
    completion(data, null);
    return null;
  });
  // wrap in a UIDragItem
  return new UIDragItem[] { new UIDragItem(itemProvider) };
}
```

В делегате перетаскивания можно реализовать множество необязательных методов, которые могут быть реализованы для настройки поведения перетаскивания, такие как предоставление нескольких представлений данных, которые можно использовать в целевых приложениях (например, форматированный текст, а также обычный текст или векторные и растровые версии рисунка). Можно также предоставить пользовательские представления данных, которые будут использоваться при перетаскивании в одном приложении.

### <a name="table-view-drop-delegate"></a>Drop делегат табличного представления

Методы Drop-делегата вызываются, когда операция перетаскивания выполняется над представлением таблицы или завершается над ней. Необходимые методы определяют, разрешено ли удаление данных и какие действия выполняются, если Удаление завершено:

- `CanHandleDropSession` — Пока выполняется перетаскивание и, возможно, оно удаляется в приложении, этот метод определяет, разрешено ли удаление перетаскиваемых данных.
- `DropSessionDidUpdate` — Пока выполняется перетаскивание, этот метод вызывается для определения действия, которое предназначается. Для определения поведения и визуальной обратной связи, предоставленной пользователю, можно использовать сведения из табличного представления, для которого выполняется перетаскивание, сеанс перетаскивания и возможный путь индекса.
- `PerformDrop` — Когда пользователь завершает перетаскивание (путем поднятия пальца), этот метод извлекает перетаскиваемые данные и изменяет табличное представление, добавляя данные в новую строку (или строки).

#### <a name="canhandledropsession"></a>канхандледропсессион

`CanHandleDropSession` Указывает, может ли табличное представление принимать перетаскиваемые данные. В этом фрагменте кода `CanLoadObjects` используется для подтверждения того, что это табличное представление может принимать строковые данные.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>дропсессиондидупдате

`DropSessionDidUpdate`Метод вызывается повторно во время выполнения операции перетаскивания, чтобы предоставить пользователю визуальные подсказки.

В приведенном ниже коде `HasActiveDrag` используется для определения того, была ли операция создана в текущем представлении таблицы. В этом случае допускается перемещение только отдельных строк.
Если перетаскивание происходит из другого источника, будет показана операция копирования:

```csharp
public UITableViewDropProposal DropSessionDidUpdate(UITableView tableView, IUIDropSession session, NSIndexPath destinationIndexPath)
{
  // The UIDropOperation.Move operation is available only for dragging within a single app.
  if (tableView.HasActiveDrag)
  {
    if (session.Items.Length > 1)
    {
        return new UITableViewDropProposal(UIDropOperation.Cancel);
    } else {
        return new UITableViewDropProposal(UIDropOperation.Move, UITableViewDropIntent.InsertAtDestinationIndexPath);
    }
  } else {
    return new UITableViewDropProposal(UIDropOperation.Copy, UITableViewDropIntent.InsertAtDestinationIndexPath);
  }
}
```

Операция DROP может быть одной из `Cancel` , `Move` или `Copy` .

Целью перетаскивания может быть Вставка новой строки или добавление или дополнение данных в существующую строку.

#### <a name="performdrop"></a>перформдроп

`PerformDrop`Метод вызывается, когда пользователь завершает операцию, и изменяет табличное представление и источник данных, чтобы отразить удаленные данные.

```csharp
public void PerformDrop(UITableView tableView, IUITableViewDropCoordinator coordinator)
{
  NSIndexPath indexPath, destinationIndexPath;
  if (coordinator.DestinationIndexPath != null)
  {
    indexPath = coordinator.DestinationIndexPath;
    destinationIndexPath = indexPath;
  }
  else
  {
    // Get last index path of table view
    var section = tableView.NumberOfSections() - 1;
    var row = tableView.NumberOfRowsInSection(section);
    destinationIndexPath = NSIndexPath.FromRowSection(row, section);
  }
  coordinator.Session.LoadObjects(typeof(NSString), (items) =>
  {
    // Consume drag items
    List<string> stringItems = new List<string>();
    foreach (var i in items)
    {
      var q = NSString.FromHandle(i.Handle);
      stringItems.Add(q.ToString());
    }
    var indexPaths = new List<NSIndexPath>();
    for (var j = 0; j < stringItems.Count; j++)
    {
      var indexPath1 = NSIndexPath.FromRowSection(destinationIndexPath.Row + j, destinationIndexPath.Section);
      model.AddItem(stringItems[j], indexPath1.Row);
      indexPaths.Add(indexPath1);
    }
    tableView.InsertRows(indexPaths.ToArray(), UITableViewRowAnimation.Automatic);
  });
}
```

Можно добавить дополнительный код для асинхронной загрузки больших объектов данных.

### <a name="testing-drag-and-drop"></a>Тестирование перетаскивания

Для тестирования [образца](/samples/xamarin/ios-samples/ios11-draganddroptableview)необходимо использовать iPad.
Откройте пример вместе с другим приложением (например, примечаниями) и перетащите строки и текст между ними:

![снимок экрана: операция перетаскивания выполняется](drag-and-drop-images/01-sml.png)

## <a name="related-links"></a>Связанные ссылки

- [Инструкции по перетаскиванию интерфейса пользователя (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Пример перетаскивания табличного представления](/samples/xamarin/ios-samples/ios11-draganddroptableview)
- [Пример перетаскивания представления коллекции](/samples/xamarin/ios-samples/ios11-draganddropcollectionview)
- [Введение в перетаскивание (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Перетаскивание с помощью коллекции и представления таблицы (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/223/)