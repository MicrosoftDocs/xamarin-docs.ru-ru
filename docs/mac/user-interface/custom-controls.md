---
title: Создание пользовательских элементов управления в Xamarin. Mac
description: В этом документе описывается создание пользовательских элементов управления в Xamarin. Mac. Здесь показано, как создать пользовательский элемент управления, отвести его состояние, нарисовать его интерфейс, реагировать на ввод пользователя и использовать элемент управления в приложении.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 8ed83ee8f0bded6258b695f7a6383cda1929f542
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997089"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Создание пользовательских элементов управления в Xamarin. Mac

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же пользовательским элементам управления, что и разработчик, работающий в *Цель-C*, *SWIFT* и *Xcode* . Так как Xamarin. Mac интегрируется напрямую с Xcode, можно использовать _Interface Builder_ Xcode для создания и обслуживания пользовательских элементов управления (или при необходимости создавать их непосредственно в коде C#).

Хотя macOS предоставляет множество встроенных пользовательских элементов управления, могут возникнуть ситуации, когда необходимо создать пользовательский элемент управления для предоставления функциональных возможностей, которые не предоставляются готовым к работе, или для соответствия настраиваемой теме пользовательского интерфейса (например, игрового интерфейса).

[![Пример пользовательского элемента управления пользовательского интерфейса](custom-controls-images/intro01.png)](custom-controls-images/intro01.png#lightbox)

В этой статье рассматриваются основные сведения о создании настраиваемого пользовательского интерфейса для повторного использования в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в этой статье.

Возможно, вы захотите ознакомиться с [предоставлением классов и методов C# для цели-c](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) . здесь также объясняются `Register` команды и, `Export` используемые для подключения классов c# к объектам и элементам пользовательского интерфейса на языке c.

<a name="Introduction-to-Outline-Views"></a>

## <a name="introduction-to-custom-controls"></a>Общие сведения о пользовательских элементах управления

Как упоминалось выше, иногда требуется создать многократно используемый настраиваемый элемент управления пользовательского интерфейса, чтобы предоставить уникальные функциональные возможности для пользовательского интерфейса приложения Xamarin. Mac или создать настраиваемую тему пользовательского интерфейса (например, игровой интерфейс).

В таких ситуациях можно легко наследовать от `NSControl` и создать настраиваемое средство, которое можно добавить в пользовательский интерфейс приложения с помощью кода C# или с помощью Interface Builder Xcode. При наследовании от `NSControl` пользовательского элемента управления автоматически будут использоваться все стандартные функции, которые имеет встроенный элемент управления пользовательского интерфейса (например, `NSButton` ).

Если пользовательский элемент управления пользовательского интерфейса просто отображает информацию (например, пользовательскую диаграмму и графическое средство), может потребоваться наследование от `NSView` вместо `NSControl` .

Независимо от того, какой базовый класс используется, основные шаги по созданию пользовательского элемента управления одинаковы.

В этой статье будет создан компонент пользовательского переключателя отражения, который предоставляет уникальную тему пользовательского интерфейса и пример создания полнофункционального пользовательского элемента управления пользовательского интерфейса.

<a name="Building-the-Custom-Control"></a>

## <a name="building-the-custom-control"></a>Создание пользовательского элемента управления

Так как создаваемый пользовательский элемент управления будет отвечать на ввод данных пользователем (нажатие левой кнопки мыши), мы будем наследовать от `NSControl` . Таким образом, наш пользовательский элемент управления автоматически будет иметь все стандартные функции, которые встроенный элемент управления пользовательского интерфейса имеет и отвечают как стандартный элемент управления macOS.

В Visual Studio для Mac откройте проект Xamarin. Mac, для которого необходимо создать настраиваемый элемент управления пользовательского интерфейса (или создайте новый). Добавьте новый класс и вызовите его `NSFlipSwitch` :

[![Добавление нового класса](custom-controls-images/custom01.png)](custom-controls-images/custom01.png#lightbox)

Затем измените `NSFlipSwitch.cs` класс и сделайте его следующим:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using AppKit;
using CoreGraphics;

namespace MacCustomControl
{
    [Register("NSFlipSwitch")]
    public class NSFlipSwitch : NSControl
    {
        #region Private Variables
        private bool _value = false;
        #endregion

        #region Computed Properties
        public bool Value {
            get { return _value; }
            set {
                // Save value and force a redraw
                _value = value;
                NeedsDisplay = true;
            }
        }
        #endregion

        #region Constructors
        public NSFlipSwitch ()
        {
            // Init
            Initialize();
        }

        public NSFlipSwitch (IntPtr handle) : base (handle)
        {
            // Init
            Initialize();
        }

        [Export ("initWithFrame:")]
        public NSFlipSwitch (CGRect frameRect) : base(frameRect) {
            // Init
            Initialize();
        }

        private void Initialize() {
            this.WantsLayer = true;
            this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
        }
        #endregion

        #region Draw Methods
        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Use Core Graphic routines to draw our UI
            ...

        }
        #endregion

        #region Private Methods
        private void FlipSwitchState() {
            // Update state
            Value = !Value;
        }
        #endregion

    }
}
```

Первое, что следует обратить внимание на наш пользовательский класс, наследуется от `NSControl` и используя команду **Register** , чтобы предоставить этот класс для цели-C и Xcode Interface Builder:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

В следующих разделах подробно рассматривается остальная часть приведенного выше кода.

<a name="Tracking-the-Controls-State"></a>

### <a name="tracking-the-controls-state"></a>Отслеживание состояния элемента управления

Так как наш пользовательский элемент управления является переключателем, нам нужен способ отслеживания состояния переключателя. Мы обработаем это с помощью следующего кода в `NSFlipSwitch` :

```csharp
private bool _value = false;
...

public bool Value {
    get { return _value; }
    set {
        // Save value and force a redraw
        _value = value;
        NeedsDisplay = true;
    }
}
```

При изменении состояния коммутатора нам нужен способ обновления пользовательского интерфейса. Это делается путем принудительного перерисовки элементом управления пользовательского интерфейса `NeedsDisplay = true` .

Если нашему элементу управления требуется больше места в одном состоянии (например, коммутатор с несколькими состояниями с тремя позициями), можно было бы использовать **перечисление** для отслеживания состояния. В нашем примере выполняется простое **логическое** значение.

Мы также добавили вспомогательный метод для переключения состояния переключения между включением и отключением.

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Позже мы развернем этот вспомогательный класс, чтобы сообщить вызывающему объекту об изменении состояния переключателей.

<a name="Drawing-the-Controls-Interface"></a>

### <a name="drawing-the-controls-interface"></a>Рисование интерфейса элемента управления

Мы будем использовать основные подпрограммы графического рисования для рисования пользовательского интерфейса пользовательского элемента управления во время выполнения. Прежде чем это сделать, необходимо включить слои для нашего элемента управления. Мы делаем это с помощью следующего закрытого метода:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Этот метод вызывается из каждого из конструкторов элемента управления, чтобы обеспечить правильную настройку элемента управления. Например.

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Далее необходимо переопределить `DrawRect` метод и добавить основные графические подпрограммы для отрисовки элемента управления:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Мы будем настраивать визуальное представление элемента управления при изменении его состояния (например, переход от **On** к **Off**). При каждом изменении состояния можно использовать `NeedsDisplay = true` команду, чтобы принудительно перерисовывать элемент управления для нового состояния.

<a name="Responding-to-User-Input"></a>

### <a name="responding-to-user-input"></a>Реагирование на ввод данных пользователем

Существует два основных способа добавления пользовательского ввода в пользовательский элемент управления: **переопределить подпрограммы обработки мыши** или **распознаватели жестов**. Используемый метод будет основываться на функциональных возможностях, необходимых для нашего элемента управления.

> [!IMPORTANT]
> Для любого создаваемого пользовательского элемента управления следует использовать либо **методы переопределения** , _либо_ **распознаватели жестов**, но не оба одновременно, так как они могут конфликтовать друг с другом.

<a name="Summary"></a>

#### <a name="handling-user-input-with-override-methods"></a>Обработка пользовательского ввода с помощью методов переопределения

Объекты, наследующие `NSControl` (или `NSView` ), имеют несколько методов переопределения для обработки ввода с клавиатуры или мыши. Для нашего примера элемента управления необходимо перевернуть состояние **переключения между и Выкл., когда** пользователь **Off** щелкает элемент управления с помощью левой кнопки мыши. Мы можем добавить следующие методы переопределения в `NSFlipSwitch` класс для их решения:

```csharp
#region Mouse Handling Methods
// --------------------------------------------------------------------------------
// Handle mouse with Override Methods.
// NOTE: Use either this method or Gesture Recognizers, NOT both!
// --------------------------------------------------------------------------------
public override void MouseDown (NSEvent theEvent)
{
    base.MouseDown (theEvent);

    FlipSwitchState ();
}

public override void MouseDragged (NSEvent theEvent)
{
    base.MouseDragged (theEvent);
}

public override void MouseUp (NSEvent theEvent)
{
    base.MouseUp (theEvent);
}

public override void MouseMoved (NSEvent theEvent)
{
    base.MouseMoved (theEvent);
}
## endregion
```

В приведенном выше коде мы вызываем `FlipSwitchState` метод (определенный выше) для отражения состояния включения или выключения переключателя в `MouseDown` методе. Это также приведет к перерисовке элемента управления для отражения текущего состояния.

<a name="Handling-User-Input-with-Gesture-Recognizers"></a>

#### <a name="handling-user-input-with-gesture-recognizers"></a>Обработка вводимых пользователем данных с помощью распознавателей жестов

При необходимости можно использовать распознаватели жестов для работы пользователя, взаимодействующего с элементом управления. Удалите переопределения, добавленные выше, измените `Initialize` метод и сделайте его следующим:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;

    // --------------------------------------------------------------------------------
    // Handle mouse with Gesture Recognizers.
    // NOTE: Use either this method or the Override Methods, NOT both!
    // --------------------------------------------------------------------------------
    var click = new NSClickGestureRecognizer (() => {
        FlipSwitchState();
    });
    AddGestureRecognizer (click);
}
```

Здесь мы создаем новый `NSClickGestureRecognizer` и вызывая наш `FlipSwitchState` метод для изменения состояния коммутатора, когда пользователь щелкает его с помощью левой кнопки мыши. `AddGestureRecognizer (click)`Метод добавляет распознаватель жестов к элементу управления.

Опять же, используемый метод зависит от того, что мы пытаемся выполнить с помощью нашего пользовательского элемента управления. Если для взаимодействия с пользователем требуется низкий уровень доступа, используйте методы переопределения. Если нам нужны стандартные функции, такие как щелчки мыши, используйте распознаватели жестов.

<a name="Responding-to-State-Change-Events"></a>

### <a name="responding-to-state-change-events"></a>Реагирование на события изменения состояния

Когда пользователь изменяет состояние пользовательского элемента управления, нам нужен способ реагирования на изменение состояния в коде (например, при щелчке на пользовательской кнопке).

Чтобы предоставить эту функцию, измените `NSFlipSwitch` класс и добавьте следующий код:

```csharp
#region Events
public event EventHandler ValueChanged;

internal void RaiseValueChanged() {
    if (this.ValueChanged != null)
        this.ValueChanged (this, EventArgs.Empty);

    // Perform any action bound to the control from Interface Builder
    // via an Action.
    if (this.Action !=null)
        NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
}
## endregion
```

Затем измените `FlipSwitchState` метод и сделайте его следующим:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

Во-первых, мы предоставляем `ValueChanged` событие, в котором мы можем добавить обработчик в код C#, чтобы мы могли выполнить действие, когда пользователь изменяет состояние переключателя.

Во-вторых, поскольку наш пользовательский элемент управления наследует от `NSControl` , он автоматически имеет **действие** , которое можно назначить в Interface Builder Xcode. Чтобы вызвать это **действие** при изменении состояния, мы используем следующий код:

```csharp
if (this.Action !=null)
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

Сначала мы проверяем, было ли назначено **действие** элементу управления. Затем мы вызываем **действие** , если оно было определено.

<a name="Using-the-Custom-Control"></a>

## <a name="using-the-custom-control"></a>Использование пользовательского элемента управления

Если пользовательский элемент управления определен полностью, мы можем добавить его в пользовательский интерфейс приложения Xamarin. Mac с помощью кода C# или в Interface Builder Xcode.

Чтобы добавить элемент управления с помощью Interface Builder, сначала выполните чистую сборку проекта Xamarin. Mac, а затем дважды щелкните `Main.storyboard` файл, чтобы открыть его в Interface Builder для изменения:

[![Изменение раскадровки в Xcode](custom-controls-images/custom02.png)](custom-controls-images/custom02.png#lightbox)

Затем перетащите элемент в `Custom View` структуру пользовательского интерфейса:

[![Выбор пользовательского представления из библиотеки](custom-controls-images/custom03.png)](custom-controls-images/custom03.png#lightbox)

Если пользовательское представление все еще выбрано, переключитесь в **инспектор удостоверений** и измените **класс** представления на `NSFlipSwitch` :

[![Задание класса представления](custom-controls-images/custom04.png)](custom-controls-images/custom04.png#lightbox)

Переключитесь в **Редактор помощника** и создайте **выход** для пользовательского элемента управления (обязательно привяжите его к `ViewController.h` файлу, а не к `.m` файлу):

[![Настройка новой розетки](custom-controls-images/custom05.png)](custom-controls-images/custom05.png#lightbox)

Сохраните изменения, вернитесь в Visual Studio для Mac и разрешите синхронизацию изменений. Измените `ViewController.cs` файл и сделайте `ViewDidLoad` метод следующим:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Do any additional setup after loading the view.
    OptionTwo.ValueChanged += (sender, e) => {
        // Display the state of the option switch
        Console.WriteLine("Option Two: {0}", OptionTwo.Value);
    };
}
```

Здесь мы реагируем на событие, которое `ValueChanged` мы определили выше в `NSFlipSwitch` классе, и записываюте текущее **значение** , когда пользователь щелкает элемент управления.

При необходимости можно вернуться к Interface Builder и определить **действие** для элемента управления:

[![Настройка нового действия](custom-controls-images/custom06.png)](custom-controls-images/custom06.png#lightbox)

Снова измените `ViewController.cs` файл и добавьте следующий метод:

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Следует использовать либо **событие** , либо определить **действие** в Interface Builder, но не следует использовать оба метода одновременно или они могут конфликтовать друг с другом.

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье подробно рассмотрено создание повторно используемого настраиваемого элемента управления пользовательского интерфейса в приложении Xamarin. Mac. Мы увидели, как нарисовать пользовательский интерфейс пользовательских элементов управления, два основных способа реагирования на ввод данных от мыши и пользователей и как предоставлять новый элемент управления действиям в Interface Builder Xcode.

## <a name="related-links"></a>Связанные ссылки

- [Маккустомконтрол (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccustomcontrol)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Обработка событий мыши](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
