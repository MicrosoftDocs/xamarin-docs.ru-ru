---
title: Создание пользовательских интерфейсов iOS в коде в Xamarin. iOS
description: В этом документе описывается, как использовать код для создания пользовательского интерфейса для приложения Xamarin. iOS. В нем рассматриваются контроллеры представлений, создание иерархии представлений, обработка вращения и многое другое.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/03/2018
ms.openlocfilehash: 7b6852485fed6cc14c9f9b2e1a303b7c2e576da9
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433576"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Создание пользовательских интерфейсов iOS в коде в Xamarin. iOS

Пользовательский интерфейс приложения iOS похож на витрину — приложение обычно получает одно окно, но оно может заполнять окно большим количеством объектов по мере необходимости, а объекты и упорядочение можно изменить в зависимости от того, что нужно отобразить в приложении. В этом сценарии объекты — то, что видит пользователь — называются представлениями. Чтобы создать один экран в приложении, представления помещаются поверх другого в иерархии представления содержимого, а иерархия управляется одним контроллером представления. Приложения с несколькими экранами используют несколько иерархий представлений содержимого, каждая из которых имеет свой контроллер представления. Приложение размещает представления в окне, чтобы создать другую иерархию представлений содержимого в зависимости от экрана, на котором находится пользователь.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

На следующей схеме показаны связи между окном, представлениями, вложенными представлениями и контроллером представления, которые позволяют вывести пользовательский интерфейс на экран устройства:

[![На этой схеме показаны связи между окном, представлениями, подпредставлениями и контроллером представления.](ios-code-only-images/image9.png)](ios-code-only-images/image9.png#lightbox)

Эти иерархии представлений могут быть созданы с помощью [Xamarin Designer для iOS](~/ios/user-interface/designer/index.md) в Visual Studio, однако полезно иметь фундаментальное представление о том, как полностью работать в коде. В этой статье рассматриваются некоторые основные моменты, которые помогут вам начать работу с разработкой пользовательского интерфейса только для кода.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

На следующей схеме показаны связи между окном, представлениями, вложенными представлениями и контроллером представления, которые позволяют вывести пользовательский интерфейс на экран устройства:

[![На этой схеме показаны связи между окном, представлениями, подпредставлениями и контроллером представления.](ios-code-only-images/image9.png)](ios-code-only-images/image9.png#lightbox)

Эти иерархии представлений можно создавать с помощью [Xamarin Designer для iOS](~/ios/user-interface/designer/index.md) в Visual Studio для Mac, однако полезно иметь фундаментальное представление о том, как полностью работать в коде. В этой статье рассматриваются некоторые основные моменты, которые помогут вам начать работу с разработкой пользовательского интерфейса только для кода.

-----

## <a name="creating-a-code-only-project"></a>Создание проекта только для кода

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="ios-blank-project-template"></a>шаблон пустого проекта iOS

Сначала создайте проект iOS в Visual Studio, используя **файл > новый проект > Visual C# > iPhone & iPad > приложение iOS (Xamarin)** , как показано ниже:

[![Диалоговое окно создания проекта](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

Затем выберите шаблон проекта **пустое приложение** :

[![Диалоговое окно выбора шаблона](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

Шаблон пустого проекта добавляет в проект 4 файла:

[![Файлы проекта](ios-code-only-images/empty-project.w157-sml.png "Файлы проекта")](ios-code-only-images/empty-project.w157.png#lightbox)

1. **AppDelegate.CS** — содержит  `UIApplicationDelegate` подкласс,  `AppDelegate` который используется для управления событиями приложения из iOS. Окно приложения создается в `AppDelegate`  `FinishedLaunching` методе.
1. **Main.CS** — содержит точку входа для приложения, указывающую класс для  `AppDelegate` .
1. **Info. plist** — файл списка свойств, содержащий сведения о конфигурации приложения.
1. Права **. plist** — файл списка свойств, содержащий сведения о возможностях и разрешениях приложения.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

## <a name="ios-templates"></a>шаблоны iOS

Visual Studio для Mac не предоставляет пустой шаблон. Все шаблоны поставляются с поддержкой раскадровки, которую Apple рекомендует использовать в качестве основного способа создания пользовательского интерфейса. Однако можно полностью создать пользовательский интерфейс в коде.

Ниже приведены инструкции по удалению раскадровки из приложения.

1. Используйте шаблон приложения с одним представлением для создания нового проекта iOS:

    [![Использование шаблона приложения с одним представлением](ios-code-only-images/single-view-app.png)](ios-code-only-images/single-view-app.png#lightbox)

1. Удалите `Main.Storyboard` файлы и `ViewController.cs` . **Не** удаляйте `LaunchScreen.Storyboard` . Контроллер представления должен быть удален, так как он является кодом программной части для контроллера представления, созданного в раскадровке:
1. Во всплывающем диалоговом окне обязательно выберите **Удалить** .

    [![Выберите Удалить во всплывающем диалоговом окне.](ios-code-only-images/delete.png)](ios-code-only-images/delete.png#lightbox)

1. В файле info. plist удалите сведения в параметре **Deployment (сведения о развертывании > основной интерфейс** ):

    [![Удалите сведения в параметре Main интерфейса.](ios-code-only-images/main-interface.png)](ios-code-only-images/main-interface.png#lightbox)

1. Наконец, добавьте следующий код в `FinishedLaunching` метод в классе AppDelegate:

    ```csharp
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
    ```

Код, добавленный в `FinishedLaunching` метод на шаге 5 выше, является минимальным объемом кода, необходимого для создания окна для приложения iOS.

-----

приложения iOS создаются с использованием [шаблона MVC](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#model-view-controller-mvc). На первом экране отображается приложение, созданное на основе контроллера корневого представления окна. Дополнительные сведения о шаблоне MVC см. в [многоэкранной программе Hello, iOS](~/ios/get-started/hello-ios-multiscreen/index.md) .

Реализация, `AppDelegate` добавленная шаблоном, создает окно приложения, в котором имеется только одно приложение для каждого приложения iOS, и делает его видимым с помощью следующего кода:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    public override UIWindow Window
            {
                get;
                set;
            }

    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Если вы уже работали с этим приложением, скорее всего, будет создано исключение, указывающее, что `Application windows are expected to have a root view controller at the end of application launch` . Добавим контроллер и сделав его контроллером корневого представления приложения.

## <a name="adding-a-controller"></a>Добавление контроллера

Приложение может содержать несколько контроллеров представления, но для управления всеми контроллерами представления необходимо иметь один корневой контроллер представления.  Добавьте контроллер в окно, создав `UIViewController` экземпляр и задав для него `Window.RootViewController` свойство:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;

        Window.RootViewController = controller;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

С каждым контроллером связано представление, доступное из `View` Свойства. Приведенный выше код изменяет свойство представления `BackgroundColor` на, чтобы `UIColor.LightGray` оно было видимым, как показано ниже:

 [![Фон представления является видимым светло-серым](ios-code-only-images/image1.png)](ios-code-only-images/image1.png#lightbox)

Можно также установить любой `UIViewController` подкласс таким `RootViewController` образом, включая контроллеры из UIKit, а также те, которые мы сами пишем. Например, следующий код добавляет в `UINavigationController` качестве `RootViewController` :

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;
        controller.Title = "My Controller";

        var navController = new UINavigationController(controller);

        Window.RootViewController = navController;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

При этом создается контроллер, вложенный в контроллер навигации, как показано ниже:

 [![Контроллер, вложенный в контроллер навигации](ios-code-only-images/image2.png)](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Создание контроллера представления

Теперь, когда мы увидели, как добавить контроллер в `RootViewController` окно, давайте посмотрим, как создать пользовательский контроллер представления в коде.

Добавьте новый класс с именем `CustomViewController` , как показано ниже.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Добавьте новый класс с именем Кустомвиевконтроллер.](ios-code-only-images/customviewcontroller.w157-sml.png)](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

[![Добавьте новый класс с именем Кустомвиевконтроллер.](ios-code-only-images/new-file.png)](ios-code-only-images/new-file.png#lightbox)

-----

Класс должен наследовать от `UIViewController` , который находится в `UIKit` пространстве имен, как показано ниже.

```csharp
using System;
using UIKit;

namespace CodeOnlyDemo
{
    class CustomViewController : UIViewController
    {
    }
}
```

## <a name="initializing-the-view"></a>Инициализация представления

`UIViewController` содержит метод, вызываемый `ViewDidLoad` при первой загрузке контроллера представления в память. Это место, необходимое для инициализации представления, например для задания свойств.

Например, следующий код добавляет кнопку и обработчик событий для принудительной отправки нового контроллера представления в стек навигации при нажатии кнопки:

```csharp
using System;
using CoreGraphics;
using UIKit;

namespace CodyOnlyDemo
{
    public class CustomViewController : UIViewController
    {
        public CustomViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            View.BackgroundColor = UIColor.White;
            Title = "My Custom View Controller";

            var btn = UIButton.FromType (UIButtonType.System);
            btn.Frame = new CGRect (20, 200, 280, 44);
            btn.SetTitle ("Click Me", UIControlState.Normal);

            var user = new UIViewController ();
            user.View.BackgroundColor = UIColor.Magenta;

            btn.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (user, true);
            };

            View.AddSubview (btn);

        }
    }
}
```

Чтобы загрузить этот контроллер в приложение и продемонстрировать простую навигацию, создайте новый экземпляр `CustomViewController` . Создайте новый контроллер навигации, передайте экземпляр контроллера представления и задайте новый контроллер навигации `RootViewController` в окне в `AppDelegate` формате как раньше:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Теперь при загрузке приложения `CustomViewController` загружается внутри контроллера навигации:

 [![Кустомвиевконтроллер загружается внутри контроллера навигации](ios-code-only-images/customvc.png)](ios-code-only-images/customvc.png#lightbox)

При нажатии кнопки будет _принудительно_ помещен новый контроллер представления в стек навигации:

[![Новый контроллер представления, помещенный в стек навигации](ios-code-only-images/customvca.png)](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Создание иерархии представлений

В приведенном выше примере мы начали создавать пользовательский интерфейс в коде, добавив кнопку к контроллеру представления.

пользовательские интерфейсы iOS состоят из иерархии представлений. Дополнительные представления, такие как метки, кнопки, ползунки и т. д., добавляются в виде подчиненных представлений некоторого родительского представления.

Например, отредактируйте `CustomViewController` экран, чтобы создать вход, где пользователь может ввести имя пользователя и пароль. Экран будет состоять из двух текстовых полей и кнопки.

### <a name="adding-the-text-fields"></a>Добавление текстовых полей

Сначала удалите кнопку и обработчик событий, добавленные в разделе [Инициализация раздела представление](#initializing-the-view) .

Добавьте элемент управления для имени пользователя, создав и инициализируя объект, `UITextField` а затем добавив его в иерархию представлений, как показано ниже:

```csharp
class CustomViewController : UIViewController
{
    UITextField usernameField;

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        View.BackgroundColor = UIColor.Gray;

        nfloat h = 31.0f;
        nfloat w = View.Bounds.Width;

        usernameField = new UITextField
        {
            Placeholder = "Enter your username",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 82, w - 20, h)
        };

        View.AddSubview(usernameField);
    }
}
```

При создании мы `UITextField` устанавливаем `Frame` свойство, чтобы определить его расположение и размер. В iOS координата 0, 0 находится в левом верхнем углу с + x справа и + y. После установки `Frame` вместе с рядом других свойств мы вызываем, `View.AddSubview` чтобы добавить объект `UITextField` в иерархию представления. Это делает `usernameField` Подпредставление `UIView` экземпляра, `View` на который ссылается свойство. Вложенное представление добавляется с z-порядком, который выше родительского представления, поэтому он отображается перед родительским представлением на экране.

Приложение с `UITextField` включенным приложением показано ниже:

 [![Приложение с текстовое поле uitextfield](ios-code-only-images/image4.png)](ios-code-only-images/image4.png#lightbox)

Можно добавить `UITextField` для пароля подобным образом, только на этот раз мы устанавливаем `SecureTextEntry` свойство в значение true, как показано ниже:

```csharp
public class CustomViewController : UIViewController
{
    UITextField usernameField, passwordField;
    public override void ViewDidLoad()
    {
       // keep the code the username UITextField
        passwordField = new UITextField
        {
            Placeholder = "Enter your password",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 114, w - 20, h),
            SecureTextEntry = true
        };

      View.AddSubview(usernameField);
      View.AddSubview(passwordField);
   }
}

```

Параметр `SecureTextEntry = true` скрывает текст, вводимый `UITextField` пользователем, как показано ниже:

 [![Параметр Секуретекстентри True скрывает текст, вводимый пользователем](ios-code-only-images/image4a.png)](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Добавление кнопки

Далее мы добавим кнопку, чтобы пользователь мог отправить имя пользователя и пароль. Кнопка добавляется в иерархию представлений, как и любой другой элемент управления, путем передачи ее в качестве аргумента в метод родительского представления `AddSubview` .

Следующий код добавляет кнопку и регистрирует обработчик событий для `TouchUpInside` события:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

После этого появится экран входа, как показано ниже:

 [![Экран входа](ios-code-only-images/image5.png)](ios-code-only-images/image5.png#lightbox)

В отличие от предыдущих версий iOS, фон кнопки по умолчанию является прозрачным. Изменение `BackgroundColor` Свойства кнопки изменяет это:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Это приведет к появлению квадратной кнопки, а не обычной закругленной кнопки. Чтобы получить скругленную сторону, используйте следующий фрагмент кода:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

После внесения этих изменений представление будет выглядеть следующим образом:

[![Пример выполнения представления](ios-code-only-images/image6.png)](ios-code-only-images/image6.png#lightbox)

## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Добавление нескольких представлений в иерархию представлений

iOS предоставляет средство для добавления нескольких представлений в иерархию представлений с помощью `AddSubviews` .

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton });
```

## <a name="adding-button-functionality"></a>Добавление функции кнопки

При нажатии кнопки пользователи будут предполагать, что что-то произойдет. Например, отображается предупреждение или выполняется переход на другой экран.

Давайте добавим код для отправки второго контроллера представления в стек навигации.

Сначала создайте второй контроллер представления:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Затем добавьте в событие функциональные возможности `TouchUpInside` .

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Навигация показана ниже:

[![Навигация показана на этой диаграмме](ios-code-only-images/navigation.png)](ios-code-only-images/navigation.png#lightbox)

Обратите внимание, что по умолчанию при использовании контроллера навигации iOS предоставляет приложению панель навигации и кнопку «назад», позволяющую перемещаться назад по стеку.

## <a name="iterating-through-the-view-hierarchy"></a>Перебор иерархии представлений

Можно выполнить итерацию по иерархии подпредставлений и выбрать любое конкретное представление. Например, чтобы найти каждую `UIButton` и присвоить ей другую кнопку `BackgroundColor` , можно использовать следующий фрагмент кода.

```csharp
foreach(var subview in View.Subviews)
{
    if (subview is UIButton)
    {
         var btn = subview as UIButton;
         btn.BackgroundColor = UIColor.Green;
    }
}
```

Это, однако, не будет работать, если представление, для которого выполняется итерация, является объектом, `UIView` так как все представления будут возвращаться как `UIView` объекты, добавляемые к родительскому представлению `UIView` .

## <a name="handling-rotation"></a>Обработка вращения

Если пользователь поворачивает устройство в альбомную, размеры элементов управления не изменяются соответствующим образом, как показано на следующем снимке экрана:

[![Если пользователь поворачивает устройство в альбомную, размеры элементов управления не изменяются соответствующим образом.](ios-code-only-images/image7.png)](ios-code-only-images/image7.png#lightbox)

Один из способов исправить это — задать `AutoresizingMask` свойство для каждого представления. В этом случае мы хотим, чтобы элементы управления растягиваются по горизонтали, поэтому мы бы настроили каждое из них `AutoresizingMask` . Следующий пример предназначен для `usernameField` , но он должен применяться к каждому мини-приложению в иерархии представлений.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Теперь при вращении устройства или симулятора все растягивается, чтобы заполнить дополнительное пространство, как показано ниже:

[![Все элементы управления растягиваются для заполнения дополнительного пространства](ios-code-only-images/image8.png)](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Создание пользовательских представлений

Помимо использования элементов управления, которые являются частью UIKit, можно также использовать пользовательские представления. Пользовательское представление может быть создано путем наследования от `UIView` и переопределения `Draw` . Давайте создадим пользовательское представление и добавим его в иерархию представлений для демонстрации.

### <a name="inheriting-from-uiview"></a>Наследование от UIView

Первое, что нам нужно сделать, — это создать класс для пользовательского представления. Мы сделаем это с помощью шаблона **класса** в Visual Studio, чтобы добавить пустой класс с именем `CircleView` . Для базового класса должно быть установлено значение `UIView` , которое мы отзываем в `UIKit` пространстве имен. Кроме того, также потребуется `System.Drawing` пространство имен. Другие `System.*` пространства имен не будут использоваться в этом примере, поэтому их можно удалить.

Класс должен выглядеть следующим образом:

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>Рисование в UIView

В каждом `UIView` из `Draw` них имеется метод, который вызывается системой при необходимости его прорисовки. `Draw` метод никогда не должен вызываться напрямую. Он вызывается системой во время обработки цикла выполнения. В первый раз через цикл выполнения после добавления представления в иерархию представлений `Draw` вызывается его метод. Последующие вызовы `Draw` происходят, когда представление помечено как требующее прорисовку путем вызова метода `SetNeedsDisplay` или `SetNeedsDisplayInRect` в представлении.

Мы можем добавить код рисования в представление, добавив такой код в переопределенный `Draw` метод, как показано ниже:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    //get graphics context
    using (var g = UIGraphics.GetCurrentContext())
    {
        // set up drawing attributes
        g.SetLineWidth(10.0f);
        UIColor.Green.SetFill();
        UIColor.Blue.SetStroke();

        // create geometry
        var path = new CGPath();
        path.AddArc(Bounds.GetMidX(), Bounds.GetMidY(), 50f, 0, 2.0f * (float)Math.PI, true);

        // add geometry to graphics context and draw
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);
    }
}
```

Так как `CircleView` имеет значение `UIView` , также можно задать `UIView` Свойства. Например, можно задать `BackgroundColor` в конструкторе:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Чтобы использовать `CircleView` только что созданное, мы можем добавить его в качестве подпредставления в иерархию представления в существующем контроллере, как это делалось `UILabels` `UIButton` ранее, или загрузить его как представление нового контроллера. Давайте вернемся к последнему.

### <a name="loading-a-view"></a>Загрузка представления

 `UIViewController` имеет метод с именем `LoadView` , который вызывается контроллером для создания представления. Это место, необходимое для создания представления и его назначения `View` свойству контроллера.

Во-первых, нам нужен контроллер, поэтому создайте новый пустой класс с именем `CircleController` .

В `CircleController` добавьте следующий код, чтобы присвоить значение `View` a `CircleView` (не следует вызывать `base` реализацию в переопределении):

```csharp
using UIKit;

namespace CodeOnlyDemo
{
    class CircleController : UIViewController
    {
        CircleView view;

        public override void LoadView()
        {
            view = new CircleView();
            View = view;
        }
    }
}
```

Наконец, необходимо предоставить контроллер во время выполнения. Для этого добавьте обработчик событий для кнопки Submit (отправить), которая была добавлена ранее, следующим образом:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Теперь, когда мы запустим приложение и нажмете кнопку Submit (отправить), отобразится новое представление с кругом:

[![Отобразится новое представление с кругом](ios-code-only-images/circles.png)](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Создание экрана запуска

[Экран запуска](~/ios/app-fundamentals/images-icons/launch-screens.md) отображается, когда приложение запускается в качестве способа отображения пользователям, на которых оно отвечает. Поскольку экран запуска отображается при загрузке приложения, его нельзя создать в коде, так как приложение по-прежнему загружается в память.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

При создании проекта iOS в Visual Studio для вас предоставляется экран запуска в виде файла XIB, который находится в папке **Resources** внутри проекта.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

При создании проекта iOS в Visual Studio для Mac для вас предоставляется экран запуска в виде файла раскадровки.

-----

Это можно изменить, дважды щелкнув его и открыв в конструкторе iOS.

Apple рекомендует использовать файл. XIB или раскадровку для приложений, предназначенных для iOS 8 или более поздней версии. при запуске любого файла в конструкторе iOS вы используете классы размеров и автоматическую разметку для адаптации макета, чтобы он правильно отображался и выглядел надлежащим образом, для всех размеров устройств. Чтобы обеспечить поддержку приложений, предназначенных для более ранних версий, можно использовать статический образ запуска в дополнение к. XIB или раскадровку.

Дополнительные сведения о создании экрана запуска см. в следующих документах:

- [Создание экрана запуска с помощью. XIB](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)
- [Управление экранами запуска с помощью раскадровок](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Начиная с iOS 9, Apple рекомендует использовать раскадровки в качестве основного метода создания экрана запуска.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Создание образа запуска для приложений до iOS 8

Статический образ можно использовать в дополнение к XIB или экрану запуска раскадровки, если приложение предназначено для версий, предшествующих iOS 8.

Это статическое изображение можно задать в файле info. plist или в качестве каталога активов (для iOS 7) в приложении. Необходимо предоставить отдельные образы для каждого размера устройства (320x480, 640x960, 640x1136), на котором может работать приложение. Дополнительные сведения о размерах экрана запуска см. в разделе " [изображения экрана запуска](~/ios/app-fundamentals/images-icons/launch-screens.md) ".

> [!IMPORTANT]
> Если у приложения нет экрана запуска, вы можете заметить, что он не полностью соответствует экрану. В этом случае необходимо включить в 640x1136 (по крайней мере) образ с именем `Default-568@2x.png` в info. plist.

## <a name="summary"></a>Сводка

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

В этой статье описано, как программно разрабатывать приложения iOS в Visual Studio. Мы рассмотрели, как создать проект на основе пустого шаблона проекта, обсуждая создание и Добавление корневого контроллера представления в окно. Затем мы показали, как использовать элементы управления из UIKit для создания иерархии представлений в контроллере для разработки экрана приложения. Далее мы рассмотрели, как разметить представления соответствующим образом на разных ориентациях, и мы увидели, как создать пользовательское представление с помощью подклассов `UIView` , а также как загрузить представление в контроллере. Наконец, мы изучили, как добавить экран запуска в приложение.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

В этой статье описано, как программно разрабатывать приложения iOS в Visual Studio для Mac. Мы рассмотрели, как создать проект на основе одного шаблона представления, обсуждая создание и Добавление корневого контроллера представления в окно. Затем мы показали, как использовать элементы управления из UIKit для создания иерархии представлений в контроллере для разработки экрана приложения. Далее мы рассмотрели, как разметить представления соответствующим образом на разных ориентациях, и мы увидели, как создать пользовательское представление с помощью подклассов `UIView` , а также как загрузить представление в контроллере. Наконец, мы изучили, как добавить экран запуска в приложение.

-----

## <a name="related-links"></a>Связанные ссылки

- [Симплелогин (пример)](/samples/xamarin/ios-samples/simplelogin)