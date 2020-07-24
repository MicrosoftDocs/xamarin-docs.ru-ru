---
title: Создание приложения Xamarin. iOS с помощью API отражения
description: В этом документе описан API-интерфейс отражения на основе атрибутов, который создает пользовательский интерфейс на основе классов, оформленных атрибутами.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: bdbff7760e7680173c57e5fc83cecb80967c0a51
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996101"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Создание приложения Xamarin. iOS с помощью API отражения

MT. API отражения D позволяет декорированию классов с помощью атрибутов, которые MT. D использует для автоматического создания экранов. API отражения обеспечивает привязку между этими классами и то, что отображается на экране. Хотя этот API не предоставляет детализированный элемент управления, который делает API элементов, он сокращает сложность за счет автоматического создания иерархии элементов на основе декорирования классов.

## <a name="setting-up-mtd"></a>Настройка MT. Четырехмерного

Машин. D распространяется с помощью Xamarin. iOS. Чтобы использовать его, щелкните правой кнопкой мыши узел **ссылки** проекта Xamarin. iOS в Visual Studio 2017 или Visual Studio для Mac и добавьте ссылку на сборку " **котушь. Dialog-1** ". Затем добавьте `using MonoTouch.Dialog` в исходный код инструкции, если это необходимо.

## <a name="getting-started-with-the-reflection-api"></a>Приступая к работе с API отражения

Использовать API отражения очень просто:

1. Создание класса, снабженного MT. Атрибуты D.
1. Создание `BindingContext` экземпляра, передавая ему экземпляр указанного выше класса.
1. Создание `DialogViewController` , передавая ему `BindingContext’s` `RootElement` .

Рассмотрим пример, иллюстрирующий использование API отражения. В этом примере мы создадим простой экран ввода данных, как показано ниже:

 [![В этом примере мы создадим простой экран ввода данных, как показано ниже.](reflection-api-walkthrough-images/01-expense-entry.png)](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>Создание класса с помощью MT. Атрибуты D

В первую очередь нам нужно использовать API отражения — это класс, дополненный атрибутами. Эти атрибуты будут использоваться MT. D — внутренне создание объектов из API элементов. Например, рассмотрим следующее определение класса:

```csharp
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;

    [Section("Expense Details")]

    [Caption("Description")]
    [Entry]
    public string Details;

    [Checkbox]
    public bool IsApproved = true;
}
```

В `SectionAttribute` результате будут `UITableView` созданы разделы, содержащие строковый аргумент, используемый для заполнения заголовка раздела. После объявления раздела все поля, следующие за ним, будут включаться в этот раздел, пока не будет объявлен другой раздел.
Тип элемента пользовательского интерфейса, созданного для поля, будет зависеть от типа поля и MT. Атрибут D.

Например, `Name` поле имеет значение, `string` а оно дополнено `EntryAttribute` . Это приводит к добавлению строки в таблицу с полем ввода текста и указанным заголовком. Аналогичным образом `IsApproved` поле — это `bool` , в `CheckboxAttribute` результате чего строка таблицы с флажком справа от ячейки таблицы. Машин. D использует имя поля, автоматически добавляя пробел, чтобы создать заголовок в этом случае, так как он не указан в атрибуте.

## <a name="adding-the-bindingcontext"></a>Добавление BindingContext

Чтобы использовать `Expense` класс, необходимо создать `BindingContext` . `BindingContext`— Это класс, который будет привязывать данные из класса с атрибутами для создания иерархии элементов. Чтобы создать его, просто создайте экземпляр класса и передайте его в конструктор.

Например, чтобы добавить пользовательский интерфейс, объявленный с помощью атрибута в `Expense` классе, включите следующий код в метод класса `FinishedLaunching` `AppDelegate` :

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Затем все, что нужно сделать для создания пользовательского интерфейса, — добавить в `BindingContext` `DialogViewController` и задать его в качестве значения в `RootViewController` окне, как показано ниже:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{   
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    var expense = new Expense ();
    var bctx = new BindingContext (null, expense, "Create a task");
    var dvc = new DialogViewController (bctx.Root);

    window.RootViewController = dvc;
    window.MakeKeyAndVisible ();

    return true;
}
```

Запуск приложения теперь приводит к отображению экрана, показанного выше.

### <a name="adding-a-uinavigationcontroller"></a>Добавление Уинавигатионконтроллер

Обратите внимание, что заголовок «создание задачи», переданный в, `BindingContext` не отображается. Это вызвано тем, что не `DialogViewController` является частью `UINavigatonController` . Измените код, чтобы добавить в `UINavigationController` качестве окна, `RootViewController,` и добавьте его в `DialogViewController` качестве корня, `UINavigationController` как показано ниже:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Теперь при запуске приложения заголовок отображается на `UINavigationController’s` панели навигации, как показано ниже.

 [![Теперь при запуске приложения заголовок отображается на панели навигации Уинавигатионконтроллерс.](reflection-api-walkthrough-images/02-create-task.png)](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Благодаря включению `UINavigationController` можно использовать преимущества других функций MT. D, для которого требуется Навигация. Например, можно добавить перечисление в класс, `Expense` чтобы определить категорию для расходов и MT. D создаст экран выбора автоматически. Чтобы продемонстрировать, измените `Expense` класс так, чтобы он включал `ExpenseCategory` поле, как показано ниже.

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}

public class Expense
{
    …

    [Caption("Category")]
    public Category ExpenseCategory;
}
```

Выполнение приложения теперь приводит к отображению новой строки в таблице для категории, как показано ниже.

 [![Выполнение приложения теперь приводит к отображению новой строки в таблице для категории, как показано ниже.](reflection-api-walkthrough-images/03-set-details.png)](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Выбор строки приводит к переходу приложения на новый экран со строками, соответствующими перечислениям, как показано ниже:

 [![Выбор строки приводит к переходу приложения на новый экран со строками, соответствующими перечислению](reflection-api-walkthrough-images/04-set-category.png)](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье представлено пошаговое руководство по API отражения. Мы показали, как добавить атрибуты в класс для управления отображаемыми сведениями. Мы также рассмотрели, как использовать `BindingContext` для привязки данных класса к создаваемой иерархии элементов, а также как использовать MT. D с `UINavigationController` .

## <a name="related-links"></a>Связанные ссылки

- [Мтдрефлектионвалксраугх (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdreflectionwalkthrough)
- [Общие сведения о диалоговом окне для бескасания](~/ios/user-interface/monotouch.dialog/index.md)
- [Пошаговое руководство по API элементов](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Пошаговое руководство по элементу JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Диалоговое окно с несенсорным касанием на GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Приложение Твитстатион](https://github.com/migueldeicaza/TweetStation)
- [Справочник по классам Уитаблевиевконтроллер](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Справочник по классам Уинавигатионконтроллер](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
