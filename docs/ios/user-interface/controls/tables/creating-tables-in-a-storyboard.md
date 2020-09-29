---
title: Работа с таблицами в конструкторе iOS
description: В предыдущих разделах мы изучили разработку с помощью таблиц. В этом случае в пятом и последнем разделе мы будем объединять полученные данные и создаем базовое приложение для создания списка дел с помощью раскадровки.
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 963151c8f5f88373fd6d71a2bb74bd2dbe5d6ab5
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91430078"
---
# <a name="working-with-tables-in-the-ios-designer"></a>Работа с таблицами в конструкторе iOS

Раскадровки — это WYSIWYG-способ создания приложений iOS и поддерживается в Visual Studio на Mac и Windows. Дополнительные сведения о раскадровках см. в документе [Введение в раскадровки](~/ios/user-interface/storyboards/index.md) . Раскадровки также позволяют изменять макеты ячеек *в* таблице, что упрощает разработку с помощью таблиц и ячеек.

При настройке свойств представления таблицы в конструкторе iOS существует два типа содержимого ячеек, которые можно выбрать: **динамическое** или **статическое** содержимое прототипа.

<a name="Prototype_Content"></a>

## <a name="dynamic-prototype-content"></a>Динамическое содержимое прототипа

Объект `UITableView` с содержимым прототипа обычно предназначен для вывода списка данных, в которых ячейка прототипа (или ячейки, как можно определить более одного) используется повторно для каждого элемента в списке. Не требуется создавать экземпляры ячеек, они получаются в `GetView` методе путем вызова `DequeueReusableCell` метода его `UITableViewSource` .

 <a name="Static_Content"></a>

## <a name="static-content"></a>Статическое содержимое

`UITableView`статические содержимое позволяет разрабатывать таблицы прямо в области конструктора. Ячейки можно перетаскивать в таблицу и настраивать, изменяя свойства и добавляя элементы управления.

 <a name="Creating_a_Storyboard-driven_app"></a>

## <a name="creating-a-storyboard-driven-app"></a>Создание приложения, управляемого раскадровкой

Пример Сторибоардтабле содержит простое приложение "основной/подробности", которое использует оба типа Уитаблевиев в раскадровке. В оставшейся части этого раздела описывается создание небольшого примера списка задач, который будет выглядеть по завершении:

 [![Примеры экранов](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

Пользовательский интерфейс будет построен с раскадровкой, и оба экрана будут использовать Уитаблевиев. На главном экране для макета строки используется *содержимое прототипа* , а на экране подробностей используется *статическое содержимое* для создания формы ввода данных с помощью пользовательских макетов ячеек.

## <a name="walkthrough"></a>Пошаговое руководство

Создайте новое решение в Visual Studio с помощью **(создать) новый проект... > приложение с одним представлением (C#)** и назовите его _сторибоардтаблес_.

 [![Диалоговое окно создания проекта](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

Решение откроется с некоторыми файлами C# и `Main.storyboard` уже созданным файлом. Дважды щелкните `Main.storyboard` файл, чтобы открыть его в конструкторе iOS.

<a name="Modifying_the_Storyboard"></a>

## <a name="modifying-the-storyboard"></a>Изменение раскадровки

Раскадровка будет изменена в три этапа:

- Сначала рассматривайте необходимые контроллеры представлений и задайте их свойства.
- Во вторых, создайте пользовательский интерфейс, перетащив объекты в представление
- Наконец, добавьте необходимый класс UIKit в каждое представление и присвойте различным элементам управления имя, чтобы на них можно было ссылаться в коде.

После завершения раскадровки можно добавить код, чтобы сделать все работу.

<a name="Layout_The_View_Controllers"></a>

### <a name="layout-the-view-controllers"></a>Разметка контроллеров представления

Первое изменение раскадровки — удаление существующего подробного представления и замена его на Уитаблевиевконтроллер. Выполните следующие действия.

1. Выберите полосу в нижней части контроллера представления и удалите ее.
2. Перетащите **контроллер навигации** и **контроллер представления таблицы** на раскадровку из панели элементов. 
3. Создайте перехода из корневого контроллера представления до второго контроллера представления таблицы, который был только что добавлен. Чтобы создать перехода, перетащите элемент управления + перетаскивание *из ячейки Details* в только что добавленный уитаблевиевконтроллер. Выберите параметр  **Показывать** в разделе  **перехода Selection (выбор**). 
4. Выберите новый перехода, который вы создали, и присвойте ему идентификатор для ссылки на этот перехода в коде. Щелкните перехода и введите `TaskSegue`  **идентификатор** в  **панель свойств**следующим образом:    
  [![Именование перехода на панели свойств](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. Затем настройте два представления таблицы, выбрав их и используя Панель свойств. Не забудьте выбрать вид, а не контроллер представления — можно использовать структуру документа для выбора.

6. Измените корневой контроллер представления на  **содержимое: динамические прототипы** (представление область конструктора будет помечено как  **содержимое прототипа** ):

    [![Установка для свойства содержимого динамических прототипов](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7. Измените новый **уитаблевиевконтроллер** на "  **содержимое": статические ячейки**. 

8. Для нового Уитаблевиевконтроллер необходимо указать имя класса и идентификатор. Выберите контроллер представления и введите _таскдетаилвиевконтроллер_ для **класса** в **панель свойств** — в панель решения будет создан новый `TaskDetailViewController.cs` файл. Введите **сторибоардид** в качестве _подробностей_, как показано в примере ниже. Он будет использоваться позже для загрузки этого представления в код C#:  

    [![Задание идентификатора раскадровки](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. Теперь область конструктора раскадровки должна выглядеть следующим образом (заголовок элемента навигации контроллера корневого представления был изменен на "доска дел"):

    [![Область конструктора](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  

<a name="Create_the_UI"></a>

### <a name="create-the-ui"></a>Создание пользовательского интерфейса

После настройки представлений и переходов необходимо добавить элементы пользовательского интерфейса.

#### <a name="root-view-controller"></a>Корневой контроллер представления

Сначала выберите ячейку прототипа в контроллере главного представления и задайте **идентификатор** как _таскцелл_, как показано ниже. Он будет использоваться позже в коде для получения экземпляра этого Уитаблевиевцелл:

 [![Задание идентификатора ячейки](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

Далее необходимо создать кнопку, которая будет добавлять новые задачи, как показано ниже:

[![элемент кнопки на панели навигации](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

Выполните следующие действия. 

- Перетащите **элемент панели** инструментов с панели элементов в правой части _панели навигации_.
- В **панель свойств**в разделе **элемент кнопки на панели** выберите  **идентификатор: добавить** (чтобы сделать его *+* кнопкой «плюс»). 
- Присвойте ему имя, чтобы его можно было идентифицировать в коде на более позднем этапе. Обратите внимание, что для контроллера корневого представления необходимо указать имя класса (например, **итемвиевконтроллер**), чтобы можно было задать имя элемента кнопки панели.

#### <a name="taskdetail-view-controller"></a>Контроллер представления Таскдетаил

Подробное представление требует гораздо больше работы. Ячейки табличного представления должны быть перенесены в представление и затем заполнены метками, текстовыми представлениями и кнопками. На снимке экрана ниже показан готовый пользовательский интерфейс с двумя разделами. Один раздел содержит три ячейки, три метки, два текстовых поля и один переключатель, а второй раздел содержит одну ячейку с двумя кнопками:

 [![макет подробного представления](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

Ниже приведены действия по созданию полного макета.

Выберите табличное представление и откройте **панель свойств**. Обновите следующие свойства:

- **Разделы**: _2_ 
- **Стиль**: _сгруппировано_
- **Разделитель**: _нет_
- **Выделенный фрагмент**: _нет выделенных элементов_

Выберите верхний раздел и в разделе **свойства > раздел представление таблицы** измените **строки** на _3_, как показано ниже:

 [![Установка для верхнего раздела трех строк](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

Для каждой ячейки откройте **панель свойств** и задайте следующие значения:

- **Стиль**:  _Пользовательский_
- **Идентификатор**: выберите уникальный идентификатор для каждой ячейки (например, «_Title_», «_Notes_», «_done_»).
- Перетащите необходимые элементы управления, чтобы создать макет, показанный на снимке экрана (поместите **уилабел**, **текстовое поле uitextfield** и **уисвитч** в нужные ячейки, и установите соответствующие метки, IE. Заголовок, заметки и готово).

Во втором разделе Задайте для **строк** значение _1_ и захватите нижний маркер изменения размера ячейки, чтобы сделать ее более высокой.

- **Задайте идентификатор**: уникальное значение (например, "Save"). 
- **Задайте фон**:  _очистить цвет_ .
- Перетащите две кнопки в ячейку и задайте соответствующие названия (например, _сохранить_ и _Удалить_), как показано ниже:

   [![Задание двух кнопок в нижнем разделе](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

На этом этапе может также потребоваться задать ограничения для ячеек и элементов управления, чтобы обеспечить адаптивный макет.

### <a name="adding-uikit-class-and-naming-controls"></a>Добавление класса UIKit и элементов управления именованием

Существует несколько заключительных этапов создания раскадровки. Сначала мы должны присвоить каждому элементу управления имя в поле **Identity > Name** , чтобы их можно было использовать в коде позже. Назовите их следующим образом:

- **Title текстовое поле uitextfield** : _титлетекст_
- **Примечания текстовое поле uitextfield** : _нотестекст_
- **Уисвитч** : _донесвитч_
- **Удалить уибуттон** : _делетебуттон_
- **Сохранить уибуттон** : _савебуттон_

<a name="Adding_Code"></a>

## <a name="adding-code"></a>Добавление кода

Оставшаяся часть работы будет выполняться в Visual Studio на Mac или Windows с помощью C#. Обратите внимание, что имена свойств, используемых в коде, соответствуют этим наборам в пошаговом руководстве выше.

Сначала мы хотим создать `Chores` класс, который предоставит способ получения и задания значения ID, Name, Notes и Boolean, чтобы мы могли использовать эти значения во всем приложении.

Добавьте в `Chores` класс следующий код:

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

Затем создайте `RootTableSource` класс, наследующий от `UITableViewSource` . 

Различие между этим и раскадровым представлением заключается в том, что `GetView` методу не нужно создавать экземпляры ячеек — `theDequeueReusableCell` метод всегда возвращает экземпляр ячейки прототипа (с совпадающим идентификатором).

Ниже приведен код из `RootTableSource.cs` файла:

```csharp
public class RootTableSource : UITableViewSource
{
// there is NO database or storage of Tasks in this example, just an in-memory List<>
Chores[] tableItems;
string cellIdentifier = "taskcell"; // set in the Storyboard

    public RootTableSource(Chores[] items)
    {
        tableItems = items;
    }

public override nint RowsInSection(UITableView tableview, nint section)
{
  return tableItems.Length;
}

public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
  // in a Storyboard, Dequeue will ALWAYS return a cell, 
  var cell = tableView.DequeueReusableCell(cellIdentifier);
  // now set the properties as normal
  cell.TextLabel.Text = tableItems[indexPath.Row].Name;
  if (tableItems[indexPath.Row].Done)
    cell.Accessory = UITableViewCellAccessory.Checkmark;
  else
    cell.Accessory = UITableViewCellAccessory.None;
  return cell;
}
public Chores GetItem(int id)
{
  return tableItems[id];
}
```

Чтобы использовать `RootTableSource` класс, создайте новую коллекцию в `ItemViewController` конструкторе:

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

В параметре `ViewWillAppear` передать коллекцию в источник и присвоить представление таблицы:

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

Если запустить приложение сейчас, на главном экране будет загружен и отображен список двух задач. Когда задача затронула перехода, определяемую раскадровкой, появится экран сведений, но в данный момент данные не будут отображаться.

Чтобы отправить параметр в перехода, переопределите `PrepareForSegue` метод и задайте свойства в `DestinationViewController` (в `TaskDetailViewController` этом примере —). Экземпляр класса целевого контроллера представления будет создан, но еще не отображается для пользователя. Это означает, что вы можете задать свойства класса, но не изменять элементы управления пользовательского интерфейса:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
    {
      if (segue.Identifier == "TaskSegue") { // set in Storyboard
        var navctlr = segue.DestinationViewController as TaskDetailViewController;
        if (navctlr != null) {
          var source = TableView.Source as RootTableSource;
          var rowPath = TableView.IndexPathForSelectedRow;
          var item = source.GetItem(rowPath.Row);
          navctlr.SetTask (this, item); // to be defined on the TaskDetailViewController
        }
      }
    }
```

В `TaskDetailViewController` `SetTask` методе присваивает свои параметры свойствам, чтобы на них можно было ссылаться в виеввиллаппеар. Свойства элемента управления не могут быть изменены в, `SetTask` поскольку могут отсутствовать при `PrepareForSegue` вызове:

```csharp
Chore currentTask {get;set;}
    public ItemViewController Delegate {get;set;} // will be used to Save, Delete later

public override void ViewWillAppear (bool animated)
    {
      base.ViewWillAppear (animated);
      TitleText.Text = currentTask.Name;
      NotesText.Text = currentTask.Notes;
      DoneSwitch.On = currentTask.Done;
    }

    // this will be called before the view is displayed
    public void SetTask (ItemViewController d, Chore task) {
      Delegate = d;
      currentTask = task;
    }
```

Теперь перехода будет открывать экран сведений и отображать сведения о выбранной задаче. К сожалению, для кнопок « **сохранить** » и « **Удалить** » нет реализации. Перед реализацией кнопок добавьте эти методы в **ItemViewController.CS** , чтобы обновить базовые данные и закрыть экран сведений:

```csharp
public void SaveTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
        NavigationController.PopViewController(true);
}

public void DeleteTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
  chores.Remove(oldTask);
        NavigationController.PopViewController(true);
}
```

Далее необходимо добавить `TouchUpInside` обработчик событий кнопки в `ViewDidLoad` метод **TaskDetailViewController.CS**. `Delegate`Ссылка на свойство `ItemViewController` была создана специально, поэтому мы можем вызвать метод `SaveTask` и `DeleteTask` , который закрывает это представление как часть своей операции:

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

Последний оставшийся набор функций для сборки — создание новых задач. В **ItemViewController.CS** добавьте метод, который создает новые задачи и открывает подробное представление. Чтобы создать экземпляр представления из раскадровки, используйте `InstantiateViewController` метод с `Identifier` для этого представления — в этом примере это будет "Detail":

```csharp
public void CreateTask () 
    {
      // first, add the task to the underlying data
      var newId = chores[chores.Count - 1].Id + 1;
      var newChore = new Chore{Id = newId};
      chores.Add (newChore);

      // then open the detail view to edit it
      var detail = Storyboard.InstantiateViewController("detail") as TaskDetailViewController;
      detail.SetTask (this, newChore);
      NavigationController.PushViewController (detail, true);
    }
```

Наконец, Назовите кнопку на панели навигации в методе **ItemViewController.CS**, `ViewDidLoad` чтобы вызвать ее:

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

Это завершает пример раскадровки — готовое приложение выглядит следующим образом:

[![Готовое приложение](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

В примере показано следующее:

- Создание таблицы с содержимым прототипа, где ячейки определяются для повторного использования для вывода списков данных. 
- Создание таблицы со статическим содержимым для создания формы ввода. Это включало изменение стиля таблицы и добавление разделов, ячеек и элементов управления ИП. 
- Создание перехода и переопределение  `PrepareForSegue` метода для уведомления целевого представления о необходимых параметрах. 
- Загрузка представлений раскадровки напрямую с помощью  `Storyboard.InstantiateViewController` метода.

## <a name="related-links"></a>Связанные ссылки

- [Сторибоардтабле (пример)](/samples/xamarin/ios-samples/storyboardtable)
- [Введение в раскадровку](~/ios/user-interface/storyboards/index.md)