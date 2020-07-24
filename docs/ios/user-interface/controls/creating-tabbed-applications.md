---
title: Панели вкладок и контроллеры панели вкладок в Xamarin. iOS
description: В этом документе описываются контроллеры панели вкладок iOS и способы их использования с Xamarin. iOS. В нем демонстрируется настройка Уитаббарконтроллер, работа с изображениями, установка значений значков, работа с событиями и многое другое.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 2e8dde87456c6e33eda6846967ceea13eb412b93
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934307"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Панели вкладок и контроллеры панели вкладок в Xamarin. iOS

Приложения с вкладками используются в iOS для поддержки пользовательских интерфейсов, в которых доступ к нескольким экранам можно получить без определенного порядка. С помощью `UITabBarController` класса приложения могут легко включать поддержку таких сценариев с несколькими экранами. `UITabBarController`осуществляет управление несколькими экранами, позволяя разработчику приложения сосредоточиться на деталях каждого экрана.

Как правило, приложения с вкладками строятся с учетом `UITabBarController` `RootViewController` основного окна. Однако с помощью некоторого дополнительного кода приложения с вкладками также могут использоваться для последующего перехода на другой начальный экран, например сценарий, в котором приложение сначала отображает экран входа, а затем — интерфейс с вкладками.

На этой странице обсуждаются оба сценария: Если вкладки находятся в корне иерархии представления приложения, а также в нерабочем `RootViewController` сценарии.

## <a name="introducing-uitabbarcontroller"></a>Знакомство с Уитаббарконтроллер

`UITabBarController`Поддерживает разработку приложений с вкладками следующим образом:

- Разрешение добавления к нему нескольких контроллеров.
- Предоставление пользовательского интерфейса с вкладками через `UITabBar` класс, чтобы разрешить пользователю переключаться между контроллерами и их представлениями.

Контроллеры добавляются в с `UITabBarController` помощью его `ViewControllers` свойства, которое является `UIViewController` массивом. `UITabBarController`Сам по себе обрабатывает загрузку соответствующего контроллера и представление его представления на основе выбранной вкладки.

Вкладки являются экземплярами `UITabBarItem` класса, которые содержатся в `UITabBar` экземпляре. Каждый `UITabBar` экземпляр доступен через `TabBarItem` свойство контроллера на каждой вкладке.

Чтобы понять, как работать с `UITabBarController` , давайте рассмотрим создание простого приложения, использующего его.

## <a name="tabbed-application-walkthrough"></a>Пошаговое руководство по приложению с вкладками

В этом пошаговом руководстве мы создадим следующее приложение:

[![Пример приложения с вкладками](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

Хотя в Visual Studio для Mac уже есть шаблон приложения с вкладками, в этом примере эти инструкции работают из пустого проекта, чтобы лучше понять, как создается приложение.

### <a name="creating-the-application"></a>Создание приложения

Начните с создания нового приложения.

Выберите пункт меню **файл > создать > решение** в Visual Studio для Mac и выберите **приложение > iOS > пустой шаблон проекта** , назовите проект `TabbedApplication` , как показано ниже:

[![Выберите шаблон пустого проекта](creating-tabbed-applications-images/newsolution1.png)](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![Назовите проект Таббедаппликатион](creating-tabbed-applications-images/newsolution2.png)](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>Добавление Уитаббарконтроллер

Затем добавьте пустой класс, выбрав **файл > новый файл** и выбрав шаблон **Общий: пустой класс** . Присвойте файлу имя, `TabController` как показано ниже:

[![Добавление класса Табконтроллер](creating-tabbed-applications-images/02-newclass.png)](creating-tabbed-applications-images/02-newclass.png#lightbox)

`TabController`Класс будет содержать реализацию объекта `UITabBarController` , который будет управлять массивом `UIViewControllers` . Когда пользователь выбирает вкладку, `UITabBarController` позаботится о представлении для соответствующего контроллера представления.

Для реализации `UITabBarController` необходимо выполнить следующие действия.

1. Установите для базового класса значение `TabController` `UITabBarController` .
1. Создайте `UIViewController` экземпляры для добавления в `TabController` .
1. Добавьте `UIViewController` экземпляры в массив, назначенный `ViewControllers` свойству объекта `TabController` .

Чтобы выполнить следующие действия, добавьте в класс следующий код `TabController` :

```csharp
using System;
using UIKit;

namespace TabbedApplication {
    public class TabController : UITabBarController {

        UIViewController tab1, tab2, tab3;

        public TabController ()
        {
            tab1 = new UIViewController();
            tab1.Title = "Green";
            tab1.View.BackgroundColor = UIColor.Green;

            tab2 = new UIViewController();
            tab2.Title = "Orange";
            tab2.View.BackgroundColor = UIColor.Orange;

            tab3 = new UIViewController();
            tab3.Title = "Red";
            tab3.View.BackgroundColor = UIColor.Red;

            var tabs = new UIViewController[] {
                tab1, tab2, tab3
            };

            ViewControllers = tabs;
        }
    }
}
```

Обратите внимание, что для каждого `UIViewController` экземпляра мы устанавливаем `Title` свойство объекта `UIViewController` . Когда контроллеры добавляются в `UITabBarController` , `UITabBarController` компонент будет считывать `Title` для каждого контроллера и отображать его на соответствующей метке вкладки, как показано ниже:

[![Пример запуска приложения](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Задание Табконтроллер в качестве Рутвиевконтроллер

Порядок размещения контроллеров на вкладках соответствует порядку их добавления в `ViewControllers` массив.

Чтобы `UITabController` Загрузить для загрузки в качестве первого экрана, необходимо сделать его окном `RootViewController` , как показано в следующем коде для `AppDelegate` :

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TabController tabController;

    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        window = new UIWindow (UIScreen.MainScreen.Bounds);

        tabController = new TabController ();
        window.RootViewController = tabController;

        window.MakeKeyAndVisible ();

        return true;
    }
}
```

Если запустить приложение сейчас, `UITabBarController` будет загружена первая вкладка, выбранная по умолчанию. Выбор любой из других вкладок приводит к отображению представления связанного контроллера, представленного, `UITabBarController,` как показано ниже, где конечный пользователь выбрал вторую вкладку:

![Вторая показанная вкладка](creating-tabbed-applications-images/03-secondtab-sml.png)

### <a name="modifying-tabbaritems"></a>Изменение Таббаритемс

Теперь, когда у нас есть приложение для работы с вкладками, измените, `TabBarItem` чтобы изменить отображаемое изображение и текст, а также добавить значок на одну из вкладок.

#### <a name="setting-a-system-item"></a>Задание системного элемента

Во-первых, давайте настроим первую вкладку на использование системного элемента. В конструкторе `TabController` удалите строку, которая задает `Title` экземпляр контроллера для `tab1` экземпляра, и замените ее следующим кодом, чтобы задать `TabBarItem` свойство контроллера:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

При создании `UITabBarItem` с помощью в `UITabBarSystemItem` iOS автоматически предоставляются заголовок и изображение, как показано на снимке экрана ниже, где показаны значок и заголовок элемента **"Избранное"** на первой вкладке:

 ![Первая вкладка со значком звездочки](creating-tabbed-applications-images/04a-tabimage-sml.png)

#### <a name="setting-the-image"></a>Настройка образа

В дополнение к использованию системного элемента в качестве заголовка и изображения `UITabBarItem` можно задать пользовательские значения. Например, измените код, который задает `TabBarItem` свойство контроллера с именем `tab2` , следующим образом:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

В приведенном выше коде предполагается, что в `second.png` корневую папку проекта (или в каталоге **ресурсов** ) добавлен образ с именем. Для поддержки всех плотий экрана требуются три изображения, как показано ниже:

![Изображения, добавленные в проект](creating-tabbed-applications-images/tabbedimages7new.png)

Инструкции по правильным измерениям см. в разделе **Размер значка панели вкладок** на [странице пользовательских значков Apple](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons/) . Рекомендуемый размер зависит от стиля изображения (круглая, квадратная, широкая или высота).

`Image`Свойству необходимо задать только имя файла **second.png** , IOS автоматически загрузит файлы с более высоким разрешением при необходимости. Дополнительные сведения см. в руководствах по [работе с изображениями](~/ios/app-fundamentals/images-icons/index.md) . По умолчанию элементы панели вкладок окрашены серым цветом, при выборе которых выделяется синий цвет.

#### <a name="overriding-the-title"></a>Переопределение заголовка

Если `Title` свойство задано непосредственно в `TabBarItem` , оно переопределяет любое значение, заданное для `Title` в самом контроллере.

На второй вкладке (средняя) на этом снимке экрана показан пользовательский заголовок и изображение:

![Вторая вкладка с квадратным значком](creating-tabbed-applications-images/05-customtab-sml.png)

#### <a name="setting-the-badge-value"></a>Задание значения эмблемы

На вкладке также может отображаться значок. Например, добавьте следующую строку кода, чтобы задать значок на третьей вкладке:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

При выполнении этой команды в левом верхнем углу вкладки, как показано ниже, отображается красная метка со строкой "Привет".

![Вторая вкладка с эмблемой Hi](creating-tabbed-applications-images/06-badge-sml.png)

Эмблема часто используется для отображения числа непрочтенных, новых элементов. Чтобы удалить эмблему, присвойте свойству значение `BadgeValue` null, как показано ниже.

```csharp
tab3.TabBarItem.BadgeValue = null;
```

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Вкладки в сценариях, не Рутвиевконтроллер

В приведенном выше примере мы показали, как работать с, `UITabBarController` когда он является `RootViewController` окном. В этом примере мы рассмотрим, как использовать, когда это не так, `UITabBarController` `RootViewController` и покажу, как это было создано, используя раскадровки.

### <a name="initial-screen-example"></a>Пример начального экрана

В этом сценарии начальный экран загружается из контроллера, который не является `UITabBarController` . Когда пользователь взаимодействует с экраном, коснувшись кнопки, тот же контроллер представления загружается в `UITabBarController` , который затем предоставляется пользователю. На следующем снимке экрана показана блок-схема приложения.

[![На этом снимке экрана показан поток приложения](creating-tabbed-applications-images/inital-screen-application.png)](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Давайте начнем новое приложение для этого примера. Опять же, мы будем использовать шаблон **приложения > iPhone > пустой проект (C#)** , на этот раз наименее проект `InitialScreenDemo` .

В этом примере раскадровка используется для размещения контроллеров представления. Чтобы добавить раскадровку, сделайте следующее:

- Щелкните правой кнопкой мыши имя проекта и выберите **добавить > новый файл**.

- Когда появится диалоговое окно Создание файла, перейдите к **iOS > пустая раскадровка iPhone**.

Давайте назовем этот новый **файл mainstoryboard** раскадровки, как показано ниже:

[![Добавление файла файл mainstoryboard в проект](creating-tabbed-applications-images/new-file-dialog.png)](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Есть несколько важных шагов, которые необходимо учитывать при добавлении раскадровки в ранее не раскадровый файл, который рассматривается в руководстве [Общие сведения о раскадровках](~/ios/user-interface/storyboards/index.md) . Эти особые значения приведены ниже.

1. Добавьте имя раскадровки в раздел " **основной интерфейс** " раздела `Info.plist` :

    [![Установка файл mainstoryboard для основного интерфейса](creating-tabbed-applications-images/project-options.png)](creating-tabbed-applications-images/project-options.png#lightbox)
1. В `App Delegate` Переопределите метод окна с помощью следующего кода:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

В этом примере нам потребуется три контроллера представления. Один из них `ViewController1` будет использоваться как первый контроллер представления и на первой вкладке. Два других, именуемые `ViewController2` и `ViewController3` , которые будут использоваться во второй и третьей вкладках соответственно.

Откройте конструктор, дважды щелкнув файл файл mainstoryboard. Storyboard, и перетащите три контроллера представления в область конструктора. Мы хотим, чтобы каждый из этих контроллеров представлений имел свой собственный класс, соответствующий приведенному выше имени, поэтому в поле **удостоверение > класс**введите имя, как показано на снимке экрана ниже:

[![Присвойте классу значение ViewController1](creating-tabbed-applications-images/class-name.png)](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio для Mac автоматически создаст необходимые классы и файлы конструктора, это можно увидеть в Панель решения, как показано ниже:

[![Автоматически создаваемые файлы в проекте](creating-tabbed-applications-images/solution-pad2.png)](creating-tabbed-applications-images/solution-pad2.png#lightbox)

#### <a name="creating-the-ui"></a>Создание пользовательского интерфейса

Далее мы создадим простой пользовательский интерфейс для каждого из представлений ViewController с помощью Xamarin iOS Designer.

Нам нужно перетащить объект `Label` и на `Button` ViewController1 из **области элементов** в правой части. Далее мы будем использовать Панель свойств, чтобы изменить имя и текст элементов управления следующим образом:

- **Метка** : `Text`  =  **одна**
- **Кнопка** : `Title`  =  **пользователь выполняет некоторое начальное действие**

Мы будем контролировать видимость нашей кнопки в `TouchUpInside` событии, и нам нужно ссылаться на него в коде программной части. Давайте назовем его **именем** `aButton` в панель свойств, как показано на следующем снимке экрана:

[![Задайте имя Абуттон в Панель свойств](creating-tabbed-applications-images/abutton-properties.png)](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Теперь область конструктора должны выглядеть примерно так, как показано на снимке экрана ниже:

[![Теперь ваш область конструктора должен выглядеть примерно так, как на этом снимке экрана](creating-tabbed-applications-images/design-surface1.png)](creating-tabbed-applications-images/design-surface1.png#lightbox)

Давайте добавим несколько дополнительных сведений в `ViewController2` и `ViewController3` , добавляя метку к каждому из них, и изменяете текст на "два" и "три" соответственно. Это подчеркивает пользователя, на котором мы рассматриваете эту вкладку или представление.

#### <a name="wiring-up-the-button"></a>Накладка кнопки

Мы собираемся загрузить `ViewController1` при первом запуске приложения. Когда пользователь нажмет кнопку, кнопка будет скрыта и загружена `UITabBarController` с `ViewController1` экземпляра на первой вкладке.

Когда пользователь освобождает `aButton` , мы хотим активировать событие таучупинсиде. Настроим кнопку, а на **вкладке события** панели свойств Объявите обработчик событий – `InitialActionCompleted` – так что на него можно будет ссылаться в коде. Это показано на следующем снимке экрана:

[![Когда пользователь отпускает Абуттон, активируйте событие Таучупинсиде.](creating-tabbed-applications-images/event-handler.png)](creating-tabbed-applications-images/event-handler.png#lightbox)

Теперь необходимо сообщить контроллеру представления о необходимости скрыть кнопку при срабатывании события `InitialActionCompleted` . В `ViewController1` добавьте следующий разделяемый метод:

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

Сохраните файл и запустите приложение. Должен отобразиться экран, а кнопка исчезнет.

#### <a name="adding-the-tab-bar-controller"></a>Добавление контроллера панели вкладок

Теперь изначальное представление работает правильно. Затем мы хотим добавить его в `UITabBarController` , а также представления 2 и 3. Откроем раскадровку в конструкторе.

На **панели элементов**найдите **контроллер панели вкладок** в разделе Controllers & Objects и перетащите его на область конструктора. Как видно на снимке экрана ниже, контроллер панели вкладок не является интерфейсом пользователя и поэтому по умолчанию открывает два контроллера представления:

[![Добавление контроллера панели вкладок к макету](creating-tabbed-applications-images/tabbarcontroller.png)](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Удалите эти новые контроллеры представлений, щелкнув черную полосу внизу и нажав клавишу DELETE.

В нашей раскадровке мы можем использовать переходов для управления переходами между Таббарконтроллер и нашими контроллерами представления. После взаимодействия с первоначальным представлением мы хотим загрузить его в Таббарконтроллер, представляемый пользователю. Давайте настроим это в конструкторе.

**Удерживая нажатой клавишу CTRL** , **перетащите указатель** мыши к таббарконтроллер. При наведении указателя мыши появится контекстное меню. Мы хотим использовать модальный перехода.

Чтобы настроить каждую из вкладок, **нажмите клавишу CTRL** для каждого из таббарконтроллер на каждом из наших контроллеров представления в порядке от одного до трех и выберите **вкладку** связь в контекстном меню, как показано ниже:

[![Выбор связи вкладки](creating-tabbed-applications-images/context-menu.png)](creating-tabbed-applications-images/context-menu.png#lightbox)

Раскадровка должна быть похожа на снимок экрана ниже:

[![Раскадровка должна напоминать этот снимок экрана](creating-tabbed-applications-images/segue-layout.png)](creating-tabbed-applications-images/segue-layout.png#lightbox)

Если щелкнуть один из элементов панели вкладок и просмотреть панель свойств, можно увидеть несколько различных параметров, как показано ниже:

[![Задание параметров вкладки в обозревателе свойств](creating-tabbed-applications-images/properties-panel.png)](creating-tabbed-applications-images/properties-panel.png#lightbox)

Это можно использовать для изменения определенных атрибутов, таких как эмблема, заголовок и идентификатор iOS, а также других.

Если мы сохранили и запустили приложение, мы найдем, что кнопка появится снова при загрузке экземпляра ViewController1 в Таббарконтроллер. Чтобы исправить это, проверьте, имеет ли текущее представление родительский контроллер представления. Если это так, мы понимаем, что мы в Таббарконтроллер, и поэтому кнопка должна быть скрыта. Добавим приведенный ниже код в класс ViewController1:

```csharp
public override void ViewDidLoad ()
{
    if (ParentViewController != null){
        aButton.Hidden = true;
    }
}
```

При запуске приложения и нажатии пользователем кнопки на первом экране загружается Уитаббарконтроллер с представлением с первого экрана, размещенного на первой вкладке, как показано ниже:

[![Выходные данные примера приложения](creating-tabbed-applications-images/first-view-sml.png)](creating-tabbed-applications-images/first-view.png#lightbox)

## <a name="summary"></a>Сводка

В этой статье описано, как использовать `UITabBarController` в приложении. Мы рассмотрели, как загружать контроллеры на каждую вкладку, а также как задавать свойства на вкладках, таких как заголовок, изображение и эмблема. Затем мы проверили, используя раскадровки, как загрузить `UITabBarController` во время выполнения, если это `RootViewController` не окно.

## <a name="related-links"></a>Связанные ссылки

- [Создание приложений с вкладками (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Справочник по классам Уитаббарконтроллер](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
