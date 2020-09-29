---
title: Отображение предупреждений в Xamarin. iOS
description: В этом документе описывается, как отображать предупреждения в Xamarin. iOS с помощью интерфейсов API Уиалертконтроллер, появившихся в iOS 8.
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: b7dae8afbb5db378687f9ecb9469236dc68831ae
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435487"
---
# <a name="displaying-alerts-in-xamarinios"></a>Отображение предупреждений в Xamarin. iOS

Начиная с iOS 8, Уиалертконтроллер заменил Уиактионшит, а Уиалертвиев оба, которые теперь являются устаревшими.

В отличие от заменяющих классов, являющихся подклассами UIView, Уиалертконтроллер является подклассом UIViewController.

Используется `UIAlertControllerStyle` для указания типа оповещения для отображения. Эти типы оповещений:

- **уиалертконтроллерстилеактионшит**
  - До iOS 8 это было бы Уиактионшит
- **уиалертконтроллерстилеалерт**
  - До iOS 8 это было бы Уиалертвиево 

При создании контроллера предупреждений необходимо выполнить три шага.

- Создайте и настройте оповещение с помощью:
  - title
  - сообщение
  - преферредстиле

- Используемых Добавление текстового поля
- Добавление необходимых действий
- Представление контроллера представления

Простейшее предупреждение содержит одну кнопку, как показано на следующем снимке экрана:

 ![Оповещение с одной кнопкой](alerts-images/alert1.png)

Ниже приведен код для вывода простого оповещения.

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

Отображение предупреждения с несколькими параметрами выполняется аналогичным образом, но добавляет два действия. Например, на следующем снимке экрана показано предупреждение с двумя кнопками:

 ![Оповещение с двумя кнопками](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

Оповещения также могут отображать лист действий, как показано на снимке экрана ниже:

 ![Оповещение на листе действий](alerts-images/alert3.png)

Кнопки добавляются к предупреждению с помощью `AddAction` метода:

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>Связанные ссылки

- [Элементы управления (пример)](/samples/xamarin/ios-samples/controls)
- [Контроллер предупреждений](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)