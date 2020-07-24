---
title: Работа с действиями строк в Xamarin. iOS
description: В этом руководстве показано, как создавать пользовательские действия по считыванию строк таблицы с помощью Уисвипеактионсконфигуратион или Уитаблевиевровактион
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/25/2017
ms.openlocfilehash: 40640f903ad82c5f154dbb74307c0003d7271561
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936608"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Работа с действиями строк в Xamarin. iOS

_В этом руководстве показано, как создавать пользовательские действия по считыванию строк таблицы с помощью Уисвипеактионсконфигуратион или Уитаблевиевровактион_

![Демонстрация действий по считыванию строк](row-action-images/action02.png)

в iOS предусмотрено два способа выполнения действий с таблицей: `UISwipeActionsConfiguration` и `UITableViewRowAction` .

`UISwipeActionsConfiguration`была введена в iOS 11 и используется для определения набора действий, которые должны выполняться, когда пользователь просматривает _любое направление_ на строке в табличном представлении. Такое поведение аналогично поведению собственного mail. app.

`UITableViewRowAction`Класс используется для определения действия, которое будет выполнено, когда пользователь просматривает строку в табличном представлении влево по горизонтали.
Например, при редактировании таблицы прокрутка влево по строке по умолчанию отображает кнопку **Удалить** . Присоединив несколько экземпляров `UITableViewRowAction` класса к `UITableView` , можно определить несколько пользовательских действий, каждое из которых имеет собственный текст, форматирование и поведение.

## <a name="uiswipeactionsconfiguration"></a>уисвипеактионсконфигуратион

Для реализации действий по считыванию необходимо выполнить три шага `UISwipeActionsConfiguration` .

1. Переопределите `GetLeadingSwipeActionsConfiguration` методы и/или `GetTrailingSwipeActionsConfiguration` . Эти методы возвращают `UISwipeActionsConfiguration` .
2. Создайте экземпляр `UISwipeActionsConfiguration` возвращаемого объекта. Этот класс принимает массив `UIContextualAction` .
3. Создайте `UIContextualAction`.

Они более подробно описаны в следующих разделах.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. Реализация методов Свипеактионсконфигуратионс

`UITableViewController`(а также `UITableViewSource` и `UITableViewDelegate` ) содержат два метода: `GetLeadingSwipeActionsConfiguration` и `GetTrailingSwipeActionsConfiguration` , которые используются для реализации набора действий по считыванию в строке представления таблицы. Первое действие по прокрутке означает прокрутку с левой стороны экрана на языке слева направо и с правой стороны экрана на языке с письмом справа налево.

В следующем примере (из примера [таблесвипеактионс](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions) ) показано, как реализовать начальную конфигурацию считывания. Из контекстных действий создаются два действия, которые описаны [ниже](#create-uicontextualaction). Затем эти действия передаются в инициализированное [`UISwipeActionsConfiguration`](#create-uiswipeactionsconfigurations) , которое используется в качестве возвращаемого значения.

```csharp
public override UISwipeActionsConfiguration GetLeadingSwipeActionsConfiguration(UITableView tableView, NSIndexPath indexPath)
{
    //UIContextualActions
    var definitionAction = ContextualDefinitionAction(indexPath.Row);
    var flagAction = ContextualFlagAction(indexPath.Row);

    //UISwipeActionsConfiguration
    var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction });

    leadingSwipe.PerformsFirstActionWithFullSwipe = false;

    return leadingSwipe;
}
```

<a name="create-uiswipeactionsconfigurations"></a>

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. Создайте экземпляр`UISwipeActionsConfiguration`

Создайте экземпляр с `UISwipeActionsConfiguration` помощью метода, `FromActions` чтобы добавить новый массив объектов `UIContextualAction` , как показано в следующем фрагменте кода:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

Важно отметить, что порядок отображения действий зависит от того, как они передаются в массив. Например, приведенный выше код для ведущих прокрутки отображает следующие действия:

![ведущие действия по считыванию, отображаемые в строке таблицы](row-action-images/action03.png)

Для прокрутки в конце будут показаны действия, показанные на следующем рисунке:

![завершающие действия по считыванию, отображаемые в строке таблицы](row-action-images/action04.png)

В этом фрагменте кода также используется новое `PerformsFirstActionWithFullSwipe` свойство. По умолчанию для этого свойства задано значение true, означающее, что первое действие в массиве произойдет, когда пользователь полностью пройдет по строке. Если у вас нет необратимого действия (например, "Удалить"), это может не быть идеальным поведением, поэтому его следует установить в значение `false` .

<a name="create-uicontextualaction"></a>

### <a name="create-a-uicontextualaction"></a>Создание класса `UIContextualAction`

В контекстном действии создается действие, которое будет отображаться при считывании пользователем строки таблицы.

Для инициализации действия необходимо указать `UIContextualActionStyle` , заголовок и `UIContextualActionHandler` . `UIContextualActionHandler`Принимает три параметра: действие, представление, в котором было показано действие, и обработчик завершения:

```csharp
public UIContextualAction ContextualFlagAction(int row)
{
    var action = UIContextualAction.FromContextualActionStyle
                    (UIContextualActionStyle.Normal,
                        "Flag",
                        (FlagAction, view, success) => {
                            var alertController = UIAlertController.Create($"Report {words[row]}?", "", UIAlertControllerStyle.Alert);
                            alertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, null));
                            alertController.AddAction(UIAlertAction.Create("Yes", UIAlertActionStyle.Destructive, null));
                            PresentViewController(alertController, true, null);

                            success(true);
                        });

    action.Image = UIImage.FromFile("feedback.png");
    action.BackgroundColor = UIColor.Blue;

    return action;
}
```

Можно изменять различные визуальные свойства, например цвет фона или изображение действия. Приведенный выше фрагмент кода демонстрирует добавление изображения к действию и задание синего цвета фона.

После создания контекстных действий они могут использовать для инициализации `UISwipeActionsConfiguration` в `GetLeadingSwipeActionsConfiguration` методе.

## <a name="uitableviewrowaction"></a>уитаблевиевровактион

Чтобы определить одно или несколько действий пользовательской строки для `UITableView` , необходимо создать экземпляр `UITableViewDelegate` класса и переопределить `EditActionsForRow` метод. Например.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Foundation;
using UIKit;

namespace BasicTable
{
    public class TableDelegate : UITableViewDelegate
    {
        #region Constructors
        public TableDelegate ()
        {
        }

        public TableDelegate (IntPtr handle) : base (handle)
        {
        }

        public TableDelegate (NSObjectFlag t) : base (t)
        {
        }

        #endregion

        #region Override Methods
        public override UITableViewRowAction[] EditActionsForRow (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewRowAction hiButton = UITableViewRowAction.Create (
                UITableViewRowActionStyle.Default,
                "Hi",
                delegate {
                    Console.WriteLine ("Hello World!");
                });
            return new UITableViewRowAction[] { hiButton };
        }
        #endregion
    }
}
```

Статический `UITableViewRowAction.Create` метод используется для создания нового `UITableViewRowAction` , который будет отображать кнопку **Hi** , когда пользователь проводит левую горизонтальную строку в строке таблицы. Позже создается новый экземпляр класса, который `TableDelegate` прикрепляется к `UITableView` . Например.

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

Если при выполнении приведенного выше кода пользователь проводит влево по строке таблицы, вместо кнопки **Удалить** отображается кнопка **Hi** , которая отображается по умолчанию:

[![Кнопка "Привет", отображаемая вместо кнопки "Удалить"](row-action-images/action01.png)](row-action-images/action01.png#lightbox)

Если пользователь нажмет кнопку **Hi** , `Hello World!` он будет записан в консоль в Visual Studio для Mac или Visual Studio при запуске приложения в режиме отладки.

## <a name="related-links"></a>Связанные ссылки

- [Таблесвипеактионс (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions)
- [Воркингвистаблес (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
