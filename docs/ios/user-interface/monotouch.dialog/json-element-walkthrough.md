---
title: Использование JSON для создания пользовательского интерфейса в Xamarin. iOS
description: Однокасание. Dialog (MT. D) включает поддержку динамического создания пользовательского интерфейса через данные JSON. В этом руководстве мы рассмотрим, как использовать Жсонелемент для создания пользовательского интерфейса из JSON, который либо включен в приложение, либо загружен с удаленного URL-адреса.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: c19cf373a89798703224574fe13724dfbade33bb
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436687"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>Использование JSON для создания пользовательского интерфейса в Xamarin. iOS

_Однокасание. Dialog (MT. D) включает поддержку динамического создания пользовательского интерфейса через данные JSON. В этом руководстве мы рассмотрим, как использовать Жсонелемент для создания пользовательского интерфейса из JSON, который либо включен в приложение, либо загружен с удаленного URL-адреса._

Машин. D поддерживает создание пользовательских интерфейсов, объявленных в JSON. Если элементы объявляются с помощью JSON, MT. D создаст связанные элементы автоматически. JSON можно загрузить из локального файла, с проанализированным `JsonObject` экземпляром или даже с удаленного URL-адреса.

Машин. D поддерживает полный набор функций, доступных в API Elements при использовании JSON. Например, приложение на следующем снимке экрана полностью объявляется с помощью JSON:

Например [ ![ , приложение на этом снимке экрана полностью объявляется с помощью JSON](json-element-walkthrough-images/01-load-from-file.png)](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ , например, приложение на этом снимке экрана полностью объявляется с помощью JSON](json-element-walkthrough-images/01-load-from-file.png)](json-element-walkthrough-images/01-load-from-file.png#lightbox) .

Перейдем к примеру из [пошагового руководства по API Elements](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) , в котором показано, как добавить экран сведений о задаче с помощью JSON.

## <a name="setting-up-mtd"></a>Настройка MT. Четырехмерного

Машин. D распространяется с помощью Xamarin. iOS. Чтобы использовать его, щелкните правой кнопкой мыши узел **ссылки** проекта Xamarin. iOS в Visual Studio 2017 или Visual Studio для Mac и добавьте ссылку на сборку " **котушь. Dialog-1** ". Затем добавьте `using MonoTouch.Dialog` в исходный код инструкции, если это необходимо.

## <a name="json-walkthrough"></a>Пошаговое руководство по JSON

Пример для этого пошагового руководства позволяет создавать задачи. При выборе задачи на первом экране отображается экран сведений, как показано ниже.

 [![При выборе задачи на первом экране отображается экран сведений, как показано](json-element-walkthrough-images/03-task-list.png)](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>Создание JSON

В этом примере мы загружаем JSON из файла в проекте с именем `task.json` . Машин. D — ожидание, что JSON должен соответствовать синтаксису, который отражает API элементов. Точно так же, как и при использовании API Elements из кода, в коде JSON объявляются разделы и в этих разделах добавляются элементы. Чтобы объявить разделы и элементы в JSON, мы используем строки "sections" и "Elements" соответственно в качестве ключей. Для каждого элемента связанный тип элемента задается с помощью `type` ключа. Каждое другое свойство Elements устанавливается с именем свойства в качестве ключа.

Например, в следующем JSON описываются разделы и элементы для сведений о задаче.

```json
{
    "title": "Task",
    "sections": [
        {
            "elements" : [
                {
                    "id" : "task-description",
                    "type": "entry",
                    "placeholder": "Enter task description"
                },
                {
                    "id" : "task-duedate",
                    "type": "date",
                    "caption": "Due Date",
                    "value": "00:00"
                }
            ]
        }
    ]
}
```

Обратите внимание, что в JSON выше содержится идентификатор для каждого элемента. Любой элемент может содержать идентификатор, чтобы ссылаться на него во время выполнения. Мы посмотрим, как это используется, когда мы покажем, как загрузить JSON в коде.

## <a name="loading-the-json-in-code"></a>Загрузка JSON в коде

После определения JSON необходимо загрузить его в MT. D с помощью `JsonElement` класса. Предполагается, что файл с созданным ранее JSON был добавлен в проект с именем sample.json и при условии действия сборки содержимого, а загрузка `JsonElement` — так же просто, как вызов следующей строки кода:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Так как мы добавляем это по запросу при каждом создании задачи, мы можем изменить эту кнопку, щелкнув приведенный выше пример API элементов следующим образом:

```csharp
_addButton.Clicked += (sender, e) => {
    ++n;

    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

    var taskElement = JsonElement.FromFile ("task.json");

    _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>Доступ к элементам во время выполнения

Помните, что мы добавили идентификатор в оба элемента при объявлении их в JSON File. Мы можем использовать свойство ID для доступа к каждому элементу во время выполнения, чтобы изменить их свойства в коде. Например, следующий код ссылается на элементы Entry и Date, чтобы задать значения из объекта Task:

```csharp
_addButton.Clicked += (sender, e) => {
    ++n;

    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

    var taskElement = JsonElement.FromFile ("task.json");

    taskElement.Caption = task.Name;

    var description = taskElement ["task-description"] as EntryElement;

    if (description != null) {
        description.Caption = task.Name;
        description.Value = task.Description;       
    }

    var duedate = taskElement ["task-duedate"] as DateElement;

    if (duedate != null) {                
        duedate.DateValue = task.DueDate;
    }
    _rootElement [0].Add (taskElement);
};
```

## <a name="loading-json-from-a-url"></a>Загрузка JSON из URL-адреса

Машин. D также поддерживает динамическую загрузку JSON из внешнего URL-адреса, просто передавая URL-адрес конструктору объекта `JsonElement` . Машин. D развернет иерархию, объявленную в JSON по требованию при переходе между экранами. Например, рассмотрим файл JSON, такой как приведенный ниже, расположенный в корне локального веб-сервера:

```json
{
    "type": "root",
    "title": "home",
    "sections": [
        {
            "header": "Nested view!",
            "elements": [
                {
                    "type": "boolean",
                    "caption": "Just a boolean",
                    "id": "first-boolean",
                    "value": false
                },
                {
                    "type": "string",
                    "caption": "Welcome to the nested controller"
                }
            ]
        }
    ]
}
```

Мы можем загрузить его, используя, `JsonElement` как в следующем коде:

```csharp
_rootElement = new RootElement ("Json Example") {
    new Section ("") {
        new JsonElement ("Load from url", "http://localhost/sample.json")
    }
};
```

Во время выполнения файл будет извлечен и проанализирован с помощью MT. D, когда пользователь переходит во второе представление, как показано на снимке экрана ниже:

 [![Файл будет извлечен и проанализирован с помощью MT. D, когда пользователь переходит к второму представлению](json-element-walkthrough-images/04-json-web-example.png)](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>Сводка

В этой статье показано, как создать интерфейс с помощью MT. D из JSON. Было показано, как загрузить JSON, входящий в файл, с приложением, а также с удаленного URL-адреса. Также показано, как получить доступ к элементам, описанным в JSON во время выполнения.

## <a name="related-links"></a>Связанные ссылки

- [Мтджсондемо (пример)](/samples/xamarin/ios-samples/mtdjsondemo)
- [Введение в бескасание. Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Пошаговое руководство по API элементов](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Пошаговое руководство по API отражения](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Диалоговое окно с несенсорным касанием на GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Приложение Твитстатион](https://github.com/migueldeicaza/TweetStation)
- [Справочник по классам Уитаблевиевконтроллер](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Справочник по классам Уинавигатионконтроллер](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)