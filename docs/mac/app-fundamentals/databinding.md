---
title: Привязка данных и кодирование значений ключа в Xamarin. Mac
description: В этой статье рассматривается использование кодирования и значения ключа для обеспечения привязки данных к элементам пользовательского интерфейса в Interface Builder Xcode.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 2f9b4dfac4909a5c68b6c53fc913925ee95313a3
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997518"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Привязка данных и кодирование значений ключа в Xamarin. Mac

_В этой статье рассматривается использование кодирования и значения ключа для обеспечения привязки данных к элементам пользовательского интерфейса в Interface Builder Xcode._

## <a name="overview"></a>Обзор

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тому же коду и методам привязки данных, что и разработчик, работающий на *уровне цели-C* и *Xcode* . Так как Xamarin. Mac интегрируется непосредственно с Xcode, можно использовать _Interface Builder_ Xcode для привязки данных к элементам пользовательского интерфейса вместо написания кода.

Благодаря использованию кодирования и методов привязки данных в приложении Xamarin. Mac можно значительно сократить объем кода, который необходимо написать и поддерживать, чтобы заполнить элементы пользовательского интерфейса и работать с ними. Кроме того, у вас есть преимущество в дальнейшем разделять резервные данные (_модель данных_) от интерфейса пользователя переднего плана (_модель-представление-контроллер_), что упрощает обслуживание, более гибкое проектирование приложений.

[![Пример выполняющегося приложения](databinding-images/intro01.png "Пример выполняющегося приложения")](databinding-images/intro01-large.png#lightbox)

В этой статье рассматриваются основные принципы работы с кодированием и привязкой данных в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в этой статье.

Возможно, стоит взглянуть на [предоставление классов и методов C# для цели-c](~/mac/internals/how-it-works.md) в документе [внутренних компонентов Xamarin. Mac](~/mac/internals/how-it-works.md) , а также объясняются `Register` атрибуты и, `Export` используемые для подключения классов c# к объектам и элементам пользовательского интерфейса на языке c.

<a name="What_is_Key-Value_Coding"></a>

## <a name="what-is-key-value-coding"></a>Что такое кодирование "ключ — значение"

Кодирование "ключ-значение" (КВК) — это механизм непрямого доступа к свойствам объекта с помощью клавиш (специально отформатированных строк) для обнаружения свойств вместо доступа к ним через переменные экземпляра или методы доступа ( `get/set` ). Реализуя методы доступа, совместимые с кодированием значений, в приложении Xamarin. Mac, вы получаете доступ к другим macOS (ранее известным как OS X), таким как "ключ — значение" (кво), привязка данных, основные данные, привязки Cocoa и возможности сценариев.

Благодаря использованию кодирования и методов привязки данных в приложении Xamarin. Mac можно значительно сократить объем кода, который необходимо написать и поддерживать, чтобы заполнить элементы пользовательского интерфейса и работать с ними. Кроме того, у вас есть преимущество в дальнейшем разделять резервные данные (_модель данных_) от интерфейса пользователя переднего плана (_модель-представление-контроллер_), что упрощает обслуживание, более гибкое проектирование приложений.

Например, рассмотрим следующее определение класса для объекта, совместимого с КВК:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        public PersonModel ()
        {
        }
    }
}
```

`[Register("PersonModel")]`Во-первых, атрибут регистрирует класс и предоставляет его цели — C. Затем класс должен наследовать от `NSObject` (или от подкласса, который наследует от `NSObject` ), в результате чего добавляется несколько базовых методов, позволяющих КВК соответствовать классу. Далее `[Export("Name")]` атрибут предоставляет `Name` свойство и определяет значение ключа, которое впоследствии будет использоваться для доступа к свойству с помощью методов КВК и кво.

Наконец, чтобы иметь возможность принимать изменения, внесенные в ключ, в значение свойства, метод доступа должен заключить изменения в его значение `WillChangeValue` в `DidChangeValue` вызовах методов и (указав тот же ключ, что и `Export` атрибут).  Например.

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Этот шаг _очень_ важен для привязки данных в Interface Builder Xcode (как будет показано далее в этой статье).

Дополнительные сведения см. в статье программное обеспечение для [программирования кодирования ключевых значений](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)Apple.

### <a name="keys-and-key-paths"></a>Ключи и пути к ключам

_Ключ_ — это строка, которая определяет конкретное свойство объекта. Как правило, ключ соответствует имени метода доступа в объекте, совместимом с кодированием значений ключа. Ключи должны использовать кодировку ASCII, как правило, начинаются с строчной буквы и не могут содержать пробелы. Поэтому, учитывая приведенный выше пример, это будет `Name` ключевое значение `Name` свойства `PersonModel` класса. Ключ и имя свойства, которые они предоставляют, не должны совпадать, но в большинстве случаев они являются.

_Путь к ключу_ — это строка разделенных точками ключей, используемая для указания иерархии свойств объекта для обхода. Свойство первого ключа в последовательности отсчитывается относительно получателя, и каждый последующий ключ вычисляется относительно значения предыдущего свойства. Аналогично использованию точечной нотации для прохода по объекту и его свойствам в классе C#.

Например, если вы развернули `PersonModel` класс и добавили `Child` свойство:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        private PersonModel _child = new PersonModel();

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Child")]
        public PersonModel Child {
            get { return _child; }
            set {
                WillChangeValue ("Child");
                _child = value;
                DidChangeValue ("Child");
            }
        }

        public PersonModel ()
        {
        }
    }
}
```

Путь к ключу к имени дочернего элемента будет `self.Child.Name` или просто `Child.Name` (в зависимости от того, как использовалось значение ключа).

### <a name="getting-values-using-key-value-coding"></a>Получение значений с помощью кодирования "ключ — значение"

`ValueForKey`Метод возвращает значение для указанного ключа (в виде `NSString` ) относительно экземпляра класса КВК, получающего запрос. Например, если `Person` является экземпляром `PersonModel` класса, определенного выше:

```csharp
// Read value
var name = Person.ValueForKey (new NSString("Name"));
```

Будет возвращено значение `Name` свойства для этого экземпляра `PersonModel` .

### <a name="setting-values-using-key-value-coding"></a>Установка значений с помощью кодирования «ключ-значение»

Аналогичным образом `SetValueForKey` задается значение для указанного ключа (в виде `NSString` ) относительно экземпляра класса КВК, получающего запрос. Опять же, используя экземпляр `PersonModel` класса, как показано ниже:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Измените значение `Name` Свойства на `Jane Doe` .

<a name="Observing_Value_Changes"></a>

### <a name="observing-value-changes"></a>Наблюдение за изменениями значений

С помощью отслеживания "ключ-значение" (кво) можно присоединить наблюдатель к конкретному ключу класса, совместимого с КВК, и получать уведомления каждый раз, когда изменяется значение этого ключа (с помощью методов КВК или прямого доступа к заданному свойству в коде C#). Например.

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Теперь при каждом `Name` `Person` изменении свойства экземпляра `PersonModel` класса новое значение записывается на консоль.

Дополнительные сведения см. [в статье Введение в знакомство с руководством по программированию](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)для Apple.

## <a name="data-binding"></a>Привязка данных

В следующих разделах показано, как можно использовать кодирование значений и ключ-значение для привязки данных к элементам пользовательского интерфейса в Interface Builder Xcode вместо чтения и записи значений с помощью кода C#. Таким образом, _модель данных_ отделяется от представлений, используемых для их отображения, что делает приложение Xamarin. Mac более гибким и удобным в обслуживании. Кроме того, значительно сокращается объем кода, который должен быть написан.

<a name="Defining_your_Data_Model"></a>

### <a name="defining-your-data-model"></a>Определение модели данных

Прежде чем можно будет привязать данные к элементу пользовательского интерфейса в Interface Builder, необходимо, чтобы в приложении Xamarin. Mac был определен совместимый класс КВК/кво, который будет использоваться в качестве _модели данных_ для привязки. Модель данных предоставляет все данные, которые будут отображаться в пользовательском интерфейсе, и получает любые изменения данных, которые пользователь делает в пользовательском интерфейсе во время выполнения приложения.

Например, если вы напишете приложение, которое управляет группой сотрудников, для определения модели данных можно использовать следующий класс:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

Большинство функций этого класса были рассмотрены в разделе [что такое кодирование "ключ — значение](#What_is_Key-Value_Coding) " выше. Тем не менее давайте взглянем на несколько определенных элементов и дополнений, которые позволяли этому классу работать в качестве модели данных для **контроллеров массива** и **контроллеров дерева** (которые мы будем использовать позже для **представлений дерева**привязки данных, **представлений структуры** и **представлений коллекций**).

Во-первых, так как сотрудник может быть руководителем, мы использовали `NSArray` (точнее, `NSMutableArray` чтобы можно было изменить значения), чтобы разрешить к ним доступ сотрудникам, которыми они управляют.

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Здесь следует обратить внимание на две вещи:

1. Мы использовали `NSMutableArray` вместо стандартного массива или коллекции C#, так как это требование к связыванию данных с элементами управления AppKit, такими как **табличные**представления, представления и **коллекции** **структуры** .
2. Мы предоставили массив сотрудников путем приведения их к типу `NSArray` для привязки данных и изменили его форматированное имя в формате C#, `People` на одно из которых предполагается привязка данных, `personModelArray` в виде **массива {class_name}** (Обратите внимание, что первый символ был преобразован в нижний регистр).

Далее необходимо добавить некоторые специальные открытые методы имени для поддержки **контроллеров массива** и **контроллеров дерева**:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Они позволяют контроллерам запрашивать и изменять отображаемые данные. Как и представленное `NSArray` выше, они имеют очень специфичное соглашение об именовании (отличающееся от стандартных соглашений об именовании C#):

- `addObject:`— Добавляет объект в массив.
- `insertObject:in{class_name}ArrayAtIndex:`— Где `{class_name}` — это имя класса. Этот метод вставляет объект в массив по заданному индексу.
- `removeObjectFrom{class_name}ArrayAtIndex:`— Где `{class_name}` — это имя класса. Этот метод удаляет объект в массиве по указанному индексу.
- `set{class_name}Array:`— Где `{class_name}` — это имя класса. Этот метод позволяет заменить существующий объект на новый.

Внутри этих методов мы упакованы изменения в массив в `WillChangeValue` и `DidChangeValue` сообщениях для обеспечения соответствия кво.

Наконец, поскольку `Icon` свойство зависит от значения `isManager` свойства, изменения `isManager` свойства могут не отражаться в `Icon` для элементов пользовательского интерфейса, привязанных к данным (во время кво):

```csharp
[Export("Icon")]
public NSImage Icon {
    get {
        if (isManager) {
            return NSImage.ImageNamed ("group.png");
        } else {
            return NSImage.ImageNamed ("user.png");
        }
    }
}
```

Чтобы исправить это, мы используем следующий код:

```csharp
[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");
    }
}
```

Обратите внимание, что в дополнение к собственному ключу `isManager` метод доступа также отправляет `WillChangeValue` `DidChangeValue` сообщения и для ключа, `Icon` так что он также будет видеть изменения.

Мы будем использовать `PersonModel` модель данных на протяжении всего остальной части этой статьи.

<a name="Simple_Data_Binding"></a>

### <a name="simple-data-binding"></a>Простая привязка данных

Определив модель данных, давайте рассмотрим простой пример привязки данных в Interface Builder Xcode. Например, добавим форму в приложение Xamarin. Mac, которое можно использовать для изменения того `PersonModel` , который мы определили ранее. Мы добавим несколько текстовых полей и флажок, чтобы отобразить и изменить свойства нашей модели.

Сначала добавим новый **контроллер представления** в наш файл **Main. Storyboard** в Interface Builder и назовите его класс `SimpleViewController` :

[![Добавление нового контроллера представления](databinding-images/simple01.png "Добавление нового контроллера представления")](databinding-images/simple01-large.png#lightbox)

Затем вернитесь к Visual Studio для Mac, измените файл **SimpleViewController.CS** (который был автоматически добавлен в наш проект) и предоставьте экземпляр, `PersonModel` который мы будем привязать к данным в нашей форме. Добавьте следующий код:

```csharp
private PersonModel _person = new PersonModel();
...

[Export("Person")]
public PersonModel Person {
    get {return _person; }
    set {
        WillChangeValue ("Person");
        _person = value;
        DidChangeValue ("Person");
    }
}
```

После загрузки представления создадим экземпляр нашего `PersonModel` и заполните его следующим кодом:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set a default person
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    Person = Craig;

}
```

Теперь нужно создать нашу форму, дважды щелкнув файл **Main. Storyboard** , чтобы открыть его для редактирования в Interface Builder. Макет формы должен выглядеть следующим образом:

[![Изменение раскадровки в Xcode](databinding-images/simple02.png "Изменение раскадровки в Xcode")](databinding-images/simple02-large.png#lightbox)

Для привязки данных формы к `PersonModel` , которую мы предоставляем через `Person` ключ, выполните следующие действия.

1. Выберите текстовое поле **имя сотрудника** и перейдите в **инспектор привязок**.
2. Установите флажок **привязать к** и выберите в раскрывающемся списке пункт **Простой контроллер представления** . Далее введите `self.Person.Name` **путь к ключу**:

    [![Ввод пути к ключу](databinding-images/simple03.png "Ввод пути к ключу")](databinding-images/simple03-large.png#lightbox)
3. Выберите поле " **род занятий** " и установите флажок " **привязать к** " и выберите в раскрывающемся списке пункт " **Простой контроллер представления** ". Далее введите `self.Person.Occupation` **путь к ключу**:

    [![Ввод пути к ключу](databinding-images/simple04.png "Ввод пути к ключу")](databinding-images/simple04-large.png#lightbox)
4. Установите флажок **сотрудник является руководителем** и установите флажок **привязать к** и выберите в раскрывающемся списке пункт **Простой контроллер представления** . Далее введите `self.Person.isManager` **путь к ключу**:

    [![Ввод пути к ключу](databinding-images/simple05.png "Ввод пути к ключу")](databinding-images/simple05-large.png#lightbox)
5. Выберите текстовое поле **число управляемых сотрудников** и установите флажок **привязать к** и выберите в раскрывающемся списке пункт **Простой контроллер представления** . Далее введите `self.Person.NumberOfEmployees` **путь к ключу**:

    [![Ввод пути к ключу](databinding-images/simple06.png "Ввод пути к ключу")](databinding-images/simple06-large.png#lightbox)
6. Если сотрудник не является руководителем, нам нужно скрыть текстовое поле число управляемых сотрудников и надпись.
7. Выберите элемент **"управляемая метка" число сотрудников** , разверните **скрытые** турндовн и установите флажок " **привязать к** " и выберите в раскрывающемся списке пункт " **Простой контроллер представления** ". Далее введите `self.Person.isManager` **путь к ключу**:

    [![Ввод пути к ключу](databinding-images/simple07.png "Ввод пути к ключу")](databinding-images/simple07-large.png#lightbox)
8. Выберите `NSNegateBoolean` в раскрывающемся списке **преобразователь значений** :

    ![Выбор преобразования «Нснегатебулеан Key»](databinding-images/simple08.png "Выбор преобразования «Нснегатебулеан Key»")
9. Это говорит о том, что метка будет скрыта, если `isManager` свойство имеет значение `false` .
10. Повторите шаги 7 и 8 для текстового поля **число управляемых сотрудников** .
11. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

При запуске приложения значения из `Person` свойства будут автоматически заполнять нашу форму:

[![Отображение автоматически заполняемой формы](databinding-images/simple09.png "Отображение автоматически заполняемой формы")](databinding-images/simple09-large.png#lightbox)

Любые изменения, вносимые пользователями в форму, будут записаны обратно в `Person` свойство в контроллере представления. Например, отмена выбора **сотрудника является руководителем** , который обновляет `Person` экземпляр `PersonModel` и текстовое поле **число управляемых сотрудников** , а также автоматически скрывает текстовые поля (через привязку данных):

[![Скрытие количества сотрудников для руководителей, не относящихся к руководителям](databinding-images/simple10.png "Скрытие количества сотрудников для руководителей, не относящихся к руководителям")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding"></a>

### <a name="table-view-data-binding"></a>Привязка данных табличного представления

Теперь, когда у нас есть основы привязки данных, рассмотрим более сложную задачу привязки данных, используя _контроллер массива_ и привязку данных к табличному представлению. Дополнительные сведения о работе с табличными представлениями см. в документации по [табличным представлениям](~/mac/user-interface/table-view.md) .

Сначала добавим новый **контроллер представления** в наш файл **Main. Storyboard** в Interface Builder и назовите его класс `TableViewController` :

[![Добавление нового контроллера представления](databinding-images/table01.png "Добавление нового контроллера представления")](databinding-images/table01-large.png#lightbox)

Теперь изменим файл **TableViewController.CS** (который был автоматически добавлен в наш проект) и предоставляем массив ( `NSArray` ) `PersonModel` классов, которым мы будем привязать к данным нашу форму. Добавьте следующий код:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Как и в приведенном `PersonModel` выше классе в разделе [Определение модели данных](#Defining_your_Data_Model) , мы предоставили четыре специально именованных открытых метода, чтобы контроллер массива и читал и записывал данные из нашей коллекции `PersonModels` .

После загрузки представления необходимо заполнить наш массив следующим кодом:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Теперь нужно создать представление таблицы, дважды щелкнув файл **Main. Storyboard** , чтобы открыть его для редактирования в Interface Builder. Макет таблицы должен выглядеть следующим образом:

[![Создание макета нового табличного представления](databinding-images/table02.png "Создание макета нового табличного представления")](databinding-images/table02-large.png#lightbox)

Необходимо добавить **контроллер массива** для предоставления привязанных данных в таблицу. выполните следующие действия.

1. Перетащите **контроллер массива** из **инспектора библиотек** в **Редактор интерфейса**:

    ![Выбор контроллера массива из библиотеки](databinding-images/table03.png "Выбор контроллера массива из библиотеки")
2. В **иерархии интерфейс** выберите **контроллер массива** и перейдите в **инспектор атрибутов**.

    [![Выбор инспектора атрибутов](databinding-images/table04.png "Выбор инспектора атрибутов")](databinding-images/table04-large.png#lightbox)
3. Введите `PersonModel` для **имени класса**, нажмите кнопку **со знаком «плюс** » и добавьте три ключа. Назовите `Name` их `Occupation` и `isManager` :

    ![Добавление требуемых путей к ключам](databinding-images/table05.png "Добавление требуемых путей к ключам")
4. Это указывает контроллеру массива, что именно управляет массивом и какие свойства он должен предоставлять (через ключи).
5. Переключитесь в **инспектор привязок** и в разделе **массив содержимого** выберите **Привязка к** и **контроллер представления таблицы**. Введите **путь к ключу модели** `self.personModelArray` :

    ![Ввод пути к ключу](databinding-images/table06.png "Ввод пути к ключу")
6. Это связывает контроллер массива с массивом `PersonModels` , который мы предоставляем в нашем контроллере представления.

Теперь необходимо привязать представление таблицы к контроллеру массива, выполнив следующие действия.

1. Выберите представление таблицы и **инспектора привязки**:

    [![Выбор инспектора привязки](databinding-images/table07.png "Выбор инспектора привязки")](databinding-images/table07-large.png#lightbox)
2. В разделе **содержимое таблицы** Турндовн выберите **Привязка к** и **контроллер массива**. Введите `arrangedObjects` для поля **ключа контроллера** :

    ![Определение ключа контроллера](databinding-images/table08.png "Определение ключа контроллера")
3. Выберите **ячейку табличное представление** в столбце **сотрудник** . В **инспекторе привязок** в разделе **значение** турндовн выберите **Привязка к** и **представление ячейки таблицы**. Введите `objectValue.Name` для **пути к ключу модели**:

    [![Задание пути к ключу модели](databinding-images/table09.png "Задание пути к ключу модели")](databinding-images/table09-large.png#lightbox)
4. `objectValue`является текущим `PersonModel` в массиве, управляемом контроллером массива.
5. Выберите **ячейку табличное представление** в столбце « **профессия** ». В **инспекторе привязок** в разделе **значение** турндовн выберите **Привязка к** и **представление ячейки таблицы**. Введите `objectValue.Occupation` для **пути к ключу модели**:

    [![Задание пути к ключу модели](databinding-images/table10.png "Задание пути к ключу модели")](databinding-images/table10-large.png#lightbox)
6. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

При запуске приложения таблица будет заполнена нашим массивом `PersonModels` :

[![Запуск приложения](databinding-images/table11.png "Запуск приложения")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding"></a>

### <a name="outline-view-data-binding"></a>Привязка данных представление "Структура"

Привязка данных к представлению структуры очень похожа на привязку к табличному представлению. Основное отличие заключается в том, что для предоставления привязанных данных в режиме структуры мы будем использовать **контроллер дерева** вместо **контроллера массива** . Дополнительные сведения о работе с представлениями структуры см. в документации по [представлениям структуры](~/mac/user-interface/outline-view.md) .

Сначала добавим новый **контроллер представления** в наш файл **Main. Storyboard** в Interface Builder и назовите его класс `OutlineViewController` :

[![Добавление нового контроллера представления](databinding-images/outline01.png "Добавление нового контроллера представления")](databinding-images/outline01-large.png#lightbox)

Теперь изменим файл **OutlineViewController.CS** (который был автоматически добавлен в наш проект) и предоставляем массив ( `NSArray` ) `PersonModel` классов, которым мы будем привязать к данным нашу форму. Добавьте следующий код:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Как и в приведенном `PersonModel` выше классе в разделе [Определение модели данных](#Defining_your_Data_Model) , мы предоставили четыре специально именованных открытых метода, чтобы контроллер дерева и читал и записывал данные из нашей коллекции `PersonModels` .

После загрузки представления необходимо заполнить наш массив следующим кодом:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (Craig);

    var Larry = new PersonModel ("Larry O'Brien", "API Documentation Manager");
    Larry.AddPerson (new PersonModel ("Mike Norman", "API Documenter"));
    AddPerson (Larry);

}
```

Теперь нужно создать представление структуры, дважды щелкнув файл **Main. Storyboard** , чтобы открыть его для редактирования в Interface Builder. Макет таблицы должен выглядеть следующим образом:

[![Создание представления структуры](databinding-images/outline02.png "Создание представления структуры")](databinding-images/outline02-large.png#lightbox)

Необходимо добавить **контроллер дерева** для предоставления привязанных данных в структуру. выполните следующие действия.

1. Перетащите **контроллер дерева** из **инспектора библиотек** в **Редактор интерфейса**:

    ![Выбор контроллера дерева из библиотеки](databinding-images/outline03.png "Выбор контроллера дерева из библиотеки")
2. Выберите в **иерархии интерфейсов** элемент **контроллер дерева** и перейдите в **инспектор атрибутов**:

    [![Выбор инспектора атрибутов](databinding-images/outline04.png "Выбор инспектора атрибутов")](databinding-images/outline04-large.png#lightbox)
3. Введите `PersonModel` для **имени класса**, нажмите кнопку **со знаком «плюс** » и добавьте три ключа. Назовите `Name` их `Occupation` и `isManager` :

    ![Добавление требуемых путей к ключам](databinding-images/outline05.png "Добавление требуемых путей к ключам")
4. Это указывает контроллеру дерева, что именно управляет массивом, и какие свойства он должен предоставлять (через ключи).
5. В разделе **контроллер дерева** введите `personModelArray` для **дочерних элементов**, введите `NumberOfEmployees` в поле **число** и введите `isEmployee` в поле **конечный**:

    ![Установка путей к ключам контроллера дерева](databinding-images/outline05.png "Установка путей к ключам контроллера дерева")
6. Это указывает контроллеру дерева, где найти дочерние узлы, сколько дочерних узлов есть и если текущий узел имеет дочерние узлы.
7. Переключитесь в **инспектор привязок** и в разделе **массив содержимого** выберите **Привязка к** и **владелец файла**. Введите **путь к ключу модели** `self.personModelArray` :

    ![Изменение пути к ключу](databinding-images/outline06.png "Изменение пути к ключу")
8. Это связывает контроллер дерева с массивом `PersonModels` , который мы предоставляем в нашем контроллере представления.

Теперь необходимо привязать представление структуры к контроллеру дерева, выполнив следующие действия.

1. Выберите режим структуры и в **инспекторе привязок** выберите:

    [![Выбор инспектора привязки](databinding-images/outline07.png "Выбор инспектора привязки")](databinding-images/outline07-large.png#lightbox)
2. Под **содержимым в представлении структуры** Турндовн выберите **Привязка к** и **контроллер дерева**. Введите `arrangedObjects` для поля **ключа контроллера** :

    ![Настройка ключа контроллера](databinding-images/outline08.png "Настройка ключа контроллера")
3. Выберите **ячейку табличное представление** в столбце **сотрудник** . В **инспекторе привязок** в разделе **значение** турндовн выберите **Привязка к** и **представление ячейки таблицы**. Введите `objectValue.Name` для **пути к ключу модели**:

    [![Ввод пути к ключу модели](databinding-images/outline09.png "Ввод пути к ключу модели")](databinding-images/outline09-large.png#lightbox)
4. `objectValue`является текущим `PersonModel` в массиве, управляемом контроллером дерева.
5. Выберите **ячейку табличное представление** в столбце « **профессия** ». В **инспекторе привязок** в разделе **значение** турндовн выберите **Привязка к** и **представление ячейки таблицы**. Введите `objectValue.Occupation` для **пути к ключу модели**:

    [![Ввод пути к ключу модели](databinding-images/outline10.png "Ввод пути к ключу модели")](databinding-images/outline10-large.png#lightbox)
6. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

При запуске приложения структура будет заполнена нашим массивом `PersonModels` :

[![Запуск приложения](databinding-images/outline11.png "Запуск приложения")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Привязка данных в представлении коллекции

Привязка данных с представлением коллекции во многом аналогична привязке с табличным представлением, так как контроллер массива используется для предоставления данных коллекции. Так как представление коллекции не имеет предопределенного формата отображения, требуется больше работы, чтобы обеспечить взаимодействие с пользователем и отслеживание выбора пользователей.

> [!IMPORTANT]
> Из-за проблемы в Xcode 7 и macOS 10,11 (и более поздних версиях) представления коллекций не могут использоваться в файлах раскадровки (раскадровки). В результате, чтобы определить представления коллекций для приложений Xamarin. Mac, необходимо по-прежнему использовать файлы. XIB. Дополнительные сведения см. в документации по [представлениям коллекции](~/mac/user-interface/collection-view.md) .

<!--KKM 012/16/2015 - Once Apple fixes the issue with Xcode and Collection Views in Storyboards, we can uncomment this section.

First, let's add a new **View Controller** to our **Main.storyboard** file in Interface Builder and name its class `CollectionViewController`:

![Add a View Controller](databinding-images/collection01.png)

Next, let's edit the **CollectionViewController.cs** file (that was automatically added to our project) and expose an array (`NSArray`) of `PersonModel` classes that we will be data binding our form to. Add the following code:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Just like we did on the `PersonModel` class above in the [Defining your Data Model](#Defining_your_Data_Model) section, we've exposed four specially named public methods so that the Array Controller and read and write data from our collection of `PersonModels`.

Next when the View is loaded, we need to populate our array with this code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Now we need to create our Collection View, double-click the **Main.storyboard** file to open it for editing in Interface Builder. Layout the Collection View to look something like the following:

![Create the CollectionView](databinding-images/collection02.png)

When you add a Collection View to a User Interface design, two extra elements are also added:

1. **Collection View Item** -  That manages a single instance of an item in the collection.
2. **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

Select the view and make it look like the following using an Image View and two Text Fields:

![Add controls](databinding-images/collection03.png)

One thing to note here, a `NSBox` was added behind everything in the view with the following attributes:

![NSBox](databinding-images/collection04.png)

We'll be using this box to provide feedback to the user when an item is selected in the Collection View.

We need to add an **Array Controller** to provide bound data to our collection, do the following:

1. Drag an **Array Controller** from the **Library Inspector** onto the **Interface Editor**: <br/>![Array Controller](databinding-images/table03.png)
2. Select **Array Controller** in the **Interface Hierarchy** and switch to the **Attribute Inspector**: <br/>![Attribute inspector](databinding-images/collection05.png)
3. Enter `PersonModel` for the **Class Name**, click the **Plus** button and add four Keys. Name them `Icon`, `Name`, `Occupation` and `isManager`: <br/>![Set the class and key names](databinding-images/table05.png)
4. This tells the Array Controller what it is managing an array of, and which properties it should expose (via Keys).
5. Switch to the **Bindings Inspector** and under **Content Array** select **Bind to** and **File's Owner**. Enter a **Model Key Path** of `self.personModelArray`: <br/>![Bindings inspector](databinding-images/table06.png)
6. This ties the Array Controller to the array of `PersonModels` that we exposed on our View Controller.

Now we need to bind our Collection View to the Array Controller, do the following:

1. Select the Collection View and the **Binding Inspector**: <br/>![Binding inspector](databinding-images/collection06.png)
2. Under the **Contents** turndown, select **Bind to** and **Array Controller**. Enter `arrangedObjects` for the **Controller Key** field: <br/>![Contents](databinding-images/collection07.png)
3. Under the **Selection Indexes** turndown, select **Bind to** and **Array Controller**. Enter `selectionIndexes` for the **Controller Key** field: <br/>![Selection indexes](databinding-images/collection08.png)
4. Select the **Image View**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Person** (the name of our Collection View Item). Enter `representedObject.Icon` for the **Model Key Path**: <br/>![Binding inspector values](databinding-images/collection09.png)
5. `representedObject` is the current `PersonModel` in the array being managed by the Array Controller.
6. Select the first **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Name` for the **Model Key Path**: <br/>![Label value](databinding-images/collection10.png)
7. Select the second **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Occupation` for the **Model Key Path**: <br/>![Label value](databinding-images/collection11.png)
8. Select the `NSBox`. In the **Bindings Inspector** under the **Hidden** turndown, select **Bind to** and **Collection View Item**. Enter `selected` for the **Model Key Path** and `NSNegateBoolean` for the **Value Transformer**: <br/>![NSBox value](databinding-images/collection12.png)
9. Save your changes and return to Visual Studio for Mac to sync with Xcode.

If we run the application, the table will be populated with our array of `PersonModels`:

![Populated table](databinding-images/collection13.png)

For more information on working with Collection Views, please see our [Collection Views](~/mac/user-interface/collection-view.md) documentation.-->

## <a name="debugging-native-crashes"></a>Отладка собственных сбоев

Ошибка в привязках данных может привести к _сбою собственного сбоя_ в неуправляемом коде и привести к сбою приложения Xamarin. Mac с `SIGABRT` ошибкой:

[![Пример собственного диалогового окна сбоя](databinding-images/debug01.png "Пример собственного диалогового окна сбоя")](databinding-images/debug01-large.png#lightbox)

При привязке данных обычно возникают четыре основные причины сбоев машинного кода:

1. Модель данных не наследует от `NSObject` или от подкласса `NSObject` .
2. Вы не предоставили свое свойство цели-C с помощью `[Export("key-name")]` атрибута.
3. Вы не переносите изменения в значения метода доступа в `WillChangeValue` и `DidChangeValue` вызовы методов (указав тот же ключ, что и `Export` атрибут).
4. Неверный или неверно типизированный ключ в **инспекторе привязки** в Interface Builder.

### <a name="decoding-a-crash"></a>Декодирование сбоя

Давайте попробуем выполнить собственный сбой в нашей привязке данных, чтобы мы могли продемонстрировать, как ее можно будет разместить и исправить. В Interface Builder Давайте изменим привязку первой метки в представлении коллекции с `Name` на `Title` :

[![Изменение ключа привязки](databinding-images/debug02.png "Изменение ключа привязки")](databinding-images/debug02-large.png#lightbox)

Сохраните изменения, вернитесь в Visual Studio для Mac для синхронизации с Xcode и запустите наше приложение. При отображении представления коллекции происходит сбой приложения с `SIGABRT` ошибкой (как показано в **выходных данных приложения** в Visual Studio для Mac), поскольку не `PersonModel` предоставляет свойство с ключом `Title` :

[![Пример ошибки привязки](databinding-images/debug03.png "Пример ошибки привязки")](databinding-images/debug03-large.png#lightbox)

Если перейти к верхней части ошибки в **выходных данных приложения** , можно увидеть ключ для решения проблемы:

[![Поиск проблемы в журнале ошибок](databinding-images/debug04.png "Поиск проблемы в журнале ошибок")](databinding-images/debug04-large.png#lightbox)

Эта строка говорит нам, что ключ `Title` не существует в объекте, к которому выполняется привязка. Если изменить привязку обратно на `Name` Interface Builder, сохранить, синхронизировать, перестроить и запустить приложение, оно будет выполняться без проблем.

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с привязкой данных и кодированием значений ключа в приложении Xamarin. Mac. Во-первых, мы рассматривали предоставление класса C# для цели-C с помощью кодирования "ключ — значение" (КВК) и "ключ — значение" (кво). Далее показано, как использовать класс, совместимый с кво, и данные, привязывать их к элементам пользовательского интерфейса в Interface Builder Xcode. Наконец, он показал сложную привязку данных с помощью **контроллеров массива** и **контроллеров дерева**.

## <a name="related-links"></a>Связанные ссылки

- [Раскадровка Макдатабиндинг (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabinding-storyboard)
- [Макдатабиндинг XIB (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabinding-xibs)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Стандартные элементы управления](~/mac/user-interface/standard-controls.md)
- [Табличные представления](~/mac/user-interface/table-view.md)
- [Представления структуры](~/mac/user-interface/outline-view.md)
- [Представления коллекций](~/mac/user-interface/collection-view.md)
- [Краткое справочное по программированию для кодирования значений](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Знакомство с руководством по программированию "ключ — значение"](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Введение в разделы по программированию привязок Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Введение в Справочник по привязкам Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [нсколлектионвиев](https://developer.apple.com/documentation/appkit/nscollectionview)
- [Рекомендации по созданию пользовательских интерфейсов в macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
