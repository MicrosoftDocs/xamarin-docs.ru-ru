---
title: Краткое руководство по tvOS
description: В этом руководстве описывается создание первого приложения Xamarin. tvOS и его цепочки инструментов разработки. В нем также представлен конструктор Xamarin, который предоставляет элементы управления ИП в коде и демонстрирует создание, запуск и тестирование приложения Xamarin. tvOS.
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 02/02/2018
ms.openlocfilehash: beaa26e0d028fb614baf76ff52d05b80584f14d4
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435253"
---
# <a name="hello-tvos-quick-start-guide"></a>Краткое руководство по tvOS

_В этом руководстве описывается создание первого приложения Xamarin. tvOS и его цепочки инструментов разработки. В нем также представлен конструктор Xamarin, который предоставляет элементы управления ИП в коде и демонстрирует создание, запуск и тестирование приложения Xamarin. tvOS._

Компания Apple выпустила 5-й выпуск Apple TV, на которой работает tvOS 11.

Платформа Apple TV открыта для разработчиков, позволяя создавать полнофункциональные впечатляющие приложения и освобождать их с помощью встроенного магазина приложений Apple TV.

Если вы знакомы с разработкой Xamarin. iOS, вы должны найти переход на tvOS довольно просто. Большинство API-интерфейсов и функций одинаковы, однако многие общие API недоступны (например, WebKit). Кроме того, работа с с помощью Siri Remote создает некоторые проблемы проектирования, отсутствующие на устройствах iOS на основе сенсорного экрана.

В этом учебнике приведу вводные сведения о работе с tvOS в приложении Xamarin. Дополнительные сведения о tvOS см. в документации Apple по [началу работы с Apple TV 4](https://developer.apple.com/tvos/) .

## <a name="overview"></a>Обзор

Xamarin. tvOS позволяет разрабатывать полностью собственные приложения Apple TV на C# и .NET, используя те же библиотеки и элементы управления интерфейса OS X, которые используются при разработке в *SWIFT* (или *цели-C*) и *Xcode*.

Кроме того, поскольку приложения Xamarin. tvOS написаны на C# и .NET, общий серверный код можно использовать в приложениях Xamarin. iOS, Xamarin. Android и Xamarin. Mac. Все это, обеспечивая собственный интерфейс для каждой платформы.

В этой статье представлены основные понятия, необходимые для создания приложения Apple TV с помощью Xamarin. tvOS и Visual Studio. Вы узнаете, как создать базовое приложение **Hello, tvOS** , которое считает количество нажатий кнопки:

[![Пример запуска приложения](hello-tvos-images/run05.png)](hello-tvos-images/run05.png#lightbox)

Будут рассмотрены следующие концепции:

- **Visual Studio для Mac**  — общие сведения о Visual Studio для Mac и о создании приложений Xamarin. tvOS с ним.
- **Анатомия приложения Xamarin. tvOS** — что состоит из приложения Xamarin. tvOS.
- **Создание пользовательского интерфейса** — использование для Xamarin Designer для iOS создания пользовательского интерфейса.
- **Развертывание и тестирование** — запуск и тестирование приложения в симуляторе tvOS и на реальном tvOS оборудовании.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Запуск нового приложения Xamarin. tvOS в Visual Studio для Mac

Как упоминалось выше, мы создадим приложение Apple TV с именем `Hello-tvOS` , которое добавляет одну кнопку и метку на основной экран. При нажатии кнопки метка будет отображать количество раз нажатия.

Чтобы приступить к работе, давайте выполним следующие действия.

1. Запустите Visual Studio для Mac:

    [![Visual Studio для Mac](hello-tvos-images/setup01.png)](hello-tvos-images/setup01.png#lightbox)
2. Щелкните ссылку **создать решение...** в верхнем левом углу экрана, чтобы открыть диалоговое окно **Создание проекта** .
3. Выберите приложение **tvOS**  >  **App**  >  с**единым представлением** и нажмите кнопку **Далее** :

    [![Выбор приложения с одним представлением](hello-tvos-images/setup02.png)](hello-tvos-images/setup02.png#lightbox)
4. Введите `Hello, tvOS` в поле **имя приложения**, введите **идентификатор организации** и нажмите кнопку **Далее** :

    [![Введите Hello, tvOS](hello-tvos-images/setup04.png)](hello-tvos-images/setup04.png#lightbox)
5. Введите `Hello_tvOS` в качестве **имени проекта** и нажмите кнопку **создать** :

    [![Введите Хеллотвос](hello-tvos-images/setup03.png)](hello-tvos-images/setup03.png#lightbox)

Visual Studio для Mac создаст новое приложение Xamarin. tvOS и отобразит файлы по умолчанию, которые будут добавлены в решение приложения:

 [![Представление файлов по умолчанию](hello-tvos-images/project01.png)](hello-tvos-images/project01.png#lightbox)

В Visual Studio для Mac используются **решения** и **проекты**точно так же, как и в Visual Studio. Решение — это контейнер, который может содержать один или несколько проектов. проекты могут включать приложения, вспомогательные библиотеки, тестовые приложения и т. д. В этом случае Visual Studio для Mac создали как решение, так и проект приложения.

При желании можно создать один или несколько проектов библиотеки кода, содержащих общий общий код. Эти проекты библиотеки могут использоваться проектом приложения или совместно с другими проектами приложений Xamarin. tvOS (или Xamarin. iOS, Xamarin. Android и Xamarin. Mac в зависимости от типа кода), так же, как и при создании стандартного приложения .NET.

## <a name="anatomy-of-a-xamarintvos-app"></a>Анатомия приложения Xamarin. tvOS

Если вы знакомы с программированием для iOS, здесь вы заметите много сходствов. На самом деле, tvOS 9 является подмножеством iOS 9, поэтому многие концепции будут пересекаться.

Давайте взглянем на файлы в проекте:

- `Main.cs` — этот файл содержит основную точку входа приложения. При запуске приложения она содержит самый первый класс и выполняемый метод.
- `AppDelegate.cs` — Этот файл содержит главный класс приложения, отвечающий за прослушивание событий операционной системы.
- `Info.plist` — Этот файл содержит свойства приложения, такие как имя приложения, значки и т. д.
- `ViewController.cs` — Это класс, представляющий главное окно и управляющий его жизненным циклом.
- `ViewController.designer.cs` — Этот файл содержит программный код, помогающий интегрироваться с пользовательским интерфейсом главного экрана.
- `Main.storyboard` — Пользовательский интерфейс для главного окна. Этот файл можно создать и поддерживать с помощью Xamarin Designer для iOS.

В следующих разделах мы рассмотрим некоторые из этих файлов. Мы подробно рассмотрим их позже, но лучше понять их основы.

### <a name="maincs"></a>Main.cs

`Main.cs`Файл содержит статический метод, `Main` который создает новый экземпляр приложения Xamarin. tvOS и передает имя класса, который будет выполнять обработку событий ОС, что в нашем случае является `AppDelegate` классом:

```csharp
using UIKit;

namespace Hello_tvOS
{
    public class Application
    {
        // This is the main entry point of the application.
        static void Main (string[] args)
        {
            // if you want to use a different Application Delegate class from "AppDelegate"
            // you can specify it here.
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs`Файл содержит наш `AppDelegate` класс, который отвечает за создание окна и прослушивание событий ОС:

```csharp
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    // The UIApplicationDelegate for the application. This class is responsible for launching the
    // User Interface of the application, as well as listening (and optionally responding) to application events from iOS.
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        // class-level declarations

        public override UIWindow Window {
            get;
            set;
        }

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Override point for customization after application launch.
            // If not required for your application you can safely delete this method

            return true;
        }

        public override void OnResignActivation (UIApplication application)
        {
            // Invoked when the application is about to move from active to inactive state.
            // This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message)
            // or when the user quits the application and it begins the transition to the background state.
            // Games should use this method to pause the game.
        }

        public override void DidEnterBackground (UIApplication application)
        {
            // Use this method to release shared resources, save user data, invalidate timers and store the application state.
            // If your application supports background execution this method is called instead of WillTerminate when the user quits.
        }

        public override void WillEnterForeground (UIApplication application)
        {
            // Called as part of the transition from background to active state.
            // Here you can undo many of the changes made on entering the background.
        }

        public override void OnActivated (UIApplication application)
        {
            // Restart any tasks that were paused (or not yet started) while the application was inactive.
            // If the application was previously in the background, optionally refresh the user interface.
        }

        public override void WillTerminate (UIApplication application)
        {
            // Called when the application is about to terminate. Save data, if needed. See also DidEnterBackground.
        }
    }
}
```

Этот код, возможно, неизвестен, если вы не создали приложение iOS раньше, но это довольно просто. Давайте рассмотрим важные строки.

Сначала давайте рассмотрим объявление переменной уровня класса:

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

`Window`Свойство предоставляет доступ к главному окну. tvOS использует то, что известно как шаблон *представления модели* (MVC). Как правило, для каждого создаваемого окна (и для многих других элементов в Windows) существует контроллер, который отвечает за жизненный цикл окна, например показывает его, добавляя к нему новые представления (элементы управления) и т. д.

Теперь у нас есть `FinishedLaunching` метод. Этот метод выполняется после создания экземпляра приложения и отвечает за фактическое создание окна приложения и начало процесса отображения в нем представления. Так как наше приложение использует раскадровку для определения своего пользовательского интерфейса, дополнительный код не требуется.

В шаблоне предусмотрено множество других методов, таких как `DidEnterBackground` и `WillEnterForeground` . Их можно безопасно удалить, если события приложения не используются в приложении.

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController`Класс является основным контроллером главного окна. Это означает, что он отвечает за жизненный цикл главного окна. Мы подробно рассмотрим эту информацию позже, теперь давайте кратко рассмотрим ее.

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
    }
}
```

#### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Файл конструктора для класса главного окна пуст, но он будет автоматически заполнен Visual Studio для Mac по мере создания пользовательского интерфейса с помощью конструктора iOS:

```csharp
using Foundation;

namespace HellotvOS
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

Мы не будем беспокоиться о файлах конструктора, так как они просто управляются с помощью Visual Studio для Mac и просто предоставляют необходимый Партнерский код, который обеспечивает доступ к элементам управления, добавляемым в любое окно или представление в нашем приложении.

Теперь, когда мы создали приложение Xamarin. tvOS и получили базовое представление о его компонентах, давайте взглянем на создание пользовательского интерфейса.

<a name="Creating-the-User-Interface"></a>

## <a name="creating-the-user-interface"></a>Создание пользовательского интерфейса

Для создания пользовательского интерфейса для приложения Xamarin. tvOS не нужно использовать Xamarin Designer для iOS. Пользовательский интерфейс можно создать непосредственно из кода C#, но это выходит за рамки этой статьи. Для простоты мы будем использовать конструктор iOS для создания пользовательского интерфейса в оставшейся части этого руководства.

Чтобы приступить к созданию пользовательского интерфейса, дважды щелкните `Main.storyboard` файл в **Обозреватель решений** , чтобы открыть его для редактирования в конструкторе iOS:

[![Файл Main.storyboard в обозревателе решений](hello-tvos-images/designer01.png)](hello-tvos-images/designer01.png#lightbox)

Это должен запустить конструктор и выглядеть следующим образом:

[![Конструктор](hello-tvos-images/designer02.png)](hello-tvos-images/designer02.png#lightbox)

Дополнительные сведения о конструкторе iOS и его работе см. в статье Введение в руководство по [Xamarin Designer для iOS](~/ios/user-interface/designer/introduction.md) .

Теперь можно приступить к добавлению элементов управления в область конструктора приложения Xamarin. tvOS.

Выполните следующие действия.

1. Выберите **область элементов**, которая должна находиться справа от области конструктора:

    [![Панель элементов](hello-tvos-images/designer03.png)](hello-tvos-images/designer03.png#lightbox)

    Если вы не можете найти его здесь, перейдите к разделу **просмотр > pad > панели элементов** , чтобы просмотреть его.
2. Перетащите **метку** с **панели элементов** в область конструктора:

    [![Перетащите метку с панели элементов](hello-tvos-images/designer04.png)](hello-tvos-images/designer04.png#lightbox)
3. Щелкните свойство **Title** на **панели свойств** и измените заголовок кнопки на `Hello, tvOS` и установите **Размер шрифта** 128:

    [![Задайте для заголовка значение Привет, tvOS и задайте размер шрифта 128.](hello-tvos-images/designer05.png)](hello-tvos-images/designer05.png#lightbox)
4. Измените размер метки таким образом, чтобы все слова отображались и расположиться по центру в верхней части окна:

    [![Изменение размера и центрирования метки](hello-tvos-images/designer06.png)](hello-tvos-images/designer06.png#lightbox)
5. Теперь метка должна быть ограничена положением, чтобы она была указана как предполагалось. независимо от размера экрана. Для этого щелкните метку, пока не появится маркер « *T-в форме* »:

    [![Маркер в форме T-формы](hello-tvos-images/designer07.png)](hello-tvos-images/designer07.png#lightbox)
6. Чтобы ограничить метку горизонтально, выделите центральный квадрат и перетащите его на вертикальную линию:

    [![Выделить центральный квадрат](hello-tvos-images/designer08.png)](hello-tvos-images/designer08zoom.png#lightbox)

     Метка должна быть включена в оранжевый цвет.
7. Выберите маркер T в верхней части метки и перетащите его в верхнюю границу окна:

    [![Перетащите маркер на верхний край окна](hello-tvos-images/designer09.png)](hello-tvos-images/designer09.png#lightbox)
8. Затем щелкните ширину, а затем — *маркер кости* по высоте, как показано ниже:

    [![Маркеры ширины и высоты](hello-tvos-images/designer10.png)](hello-tvos-images/designer10.png#lightbox)

     При щелчке по каждому *маркеру кости* выберите ширину и высоту соответственно, чтобы задать фиксированные размеры.
9. По завершении ограничения должны выглядеть так же, как на вкладке Макет панели свойств:

    [![Примеры ограничений](hello-tvos-images/designer11.png)](hello-tvos-images/designer11.png#lightbox)
10. Перетащите **кнопку** из **панели элементов** и поместите ее под меткой.
11. Щелкните свойство **Title** на **панели свойств** и измените заголовок кнопки на `Click Me` :

    [![Изменение заголовка кнопок для нажатия кнопки "мне"](hello-tvos-images/designer12.png)](hello-tvos-images/designer12.png#lightbox)
12. Повторите шаги с 5 по 8, чтобы ограничить кнопку в окне tvOS. Однако вместо того, чтобы перетаскивать T-маркер в верхнюю часть окна (как в шаге #7), перетащите его в нижнюю часть Метки:

    [![Ограничение кнопки](hello-tvos-images/designer14.png)](hello-tvos-images/designer14.png#lightbox)
13. Перетащите еще одну метку под кнопкой, чтобы ее размер совпадал с шириной первой метки, и задайте для ее **выравнивания** значение по **центру**:

    [![Перетащите другую метку под кнопкой, наведите ее на ту же ширину, что и у первой метки, и задайте для ее выравнивания значение по центру.](hello-tvos-images/designer15.png)](hello-tvos-images/designer15.png#lightbox)
14. Как и в первой метке и кнопке, установите эту метку в центре и закрепите ее в расположении и размере:

    [![Закрепить метку в расположении и размере](hello-tvos-images/designer16.png)](hello-tvos-images/designer16.png#lightbox)
15. Сохраните изменения в пользовательском интерфейсе.

При изменении размера и перемещении элементов управления следует заметить, что конструктор предоставляет полезные подсказки по использованию, основанные на [рекомендациях по управлению персоналом Apple TV](https://developer.apple.com/tvos/human-interface-guidelines/). Эти рекомендации помогут вам создать высококачественные приложения, которые будут иметь привычный вид для пользователей Apple TV.

Если взглянуть на раздел « **Структура документа** », обратите внимание на отображение макета и иерархии элементов, составляющих пользовательский интерфейс.

[![Раздел "Структура документа"](hello-tvos-images/designer17.png)](hello-tvos-images/designer17.png#lightbox)

Здесь можно выбрать элементы для изменения или перетащить, чтобы изменить порядок элементов пользовательского интерфейса при необходимости. Например, если элемент пользовательского интерфейса был охвачен другим элементом, можно перетащить его в нижнюю часть списка, чтобы сделать его самым верхним элементом в окне.

Теперь, когда мы создали пользовательский интерфейс, необходимо предоставить элементы пользовательского интерфейса, чтобы Xamarin. tvOS мог обращаться к ним и взаимодействовать с ними в коде C#.

### <a name="accessing-the-controls-in-the-code-behind"></a>Доступ к элементам управления в коде программной части

Есть два основных способа доступа к элементам управления, добавленным в конструктор iOS из кода:

- Создание обработчика событий для элемента управления.
- Присвоение имени элементу управления, чтобы впоследствии можно было ссылаться на него.

При добавлении любого из этих элементов разделяемый класс в `ViewController.designer.cs` будет обновлен для отражения изменений. Это позволит вам получить доступ к элементам управления в контроллере представления.

### <a name="creating-an-event-handler"></a>Создание обработчика событий

В этом примере приложения при нажатии кнопки требуется _что-то_ сделать, поэтому обработчик событий необходимо добавить к конкретному событию на кнопке. Чтобы настроить это, выполните следующие действия.

1. В конструкторе Xamarin iOS нажмите кнопку на контроллере представления.
2. На панели свойств перейдите на вкладку **события** .

    [![Вкладка "События"](hello-tvos-images/event1.png)](hello-tvos-images/event1.png#lightbox)
3. Нахождение события Таучупинсиде и предоставление ему обработчика событий с именем `Clicked` :

    [![Событие Таучупинсиде](hello-tvos-images/event2.png)](hello-tvos-images/event2.png#lightbox)
4. При нажатии клавиши **Ввод**будет открыт файл **ViewController**. cs, предлагающий расположения обработчика событий в коде. Используйте клавиши со стрелками на клавиатуре, чтобы задать расположение:

    [![Задание расположения](hello-tvos-images/event3.png)](hello-tvos-images/event3.png#lightbox)
5. Будет создан разделяемый метод, как показано ниже.

    [![Разделяемый метод](hello-tvos-images/event4.png)](hello-tvos-images/event4.png#lightbox)

Теперь можно приступить к добавлению кода, чтобы позволить кнопке работать.

### <a name="naming-a-control"></a>Именование элемента управления

При нажатии кнопки метка должна обновляться в зависимости от числа щелчков. Для этого нам нужно будет получить доступ к метке в коде. Это делается путем присвоения имени. Выполните следующие действия.

1. Откройте раскадровку и выберите метку в нижней части контроллера представления.
2. На панели свойств выберите вкладку **мини** -приложение:

    [![Выберите вкладку мини-приложение](hello-tvos-images/name1.png)](hello-tvos-images/name1.png#lightbox)
3. В разделе **удостоверение > имя**добавьте `ClickedLabel` :

    [![Задать ClickedLabel](hello-tvos-images/name2.png)](hello-tvos-images/name2.png#lightbox)

Теперь все готово для начала обновления метки!

### <a name="how-controls-are-accessed"></a>Доступ к элементам управления

При выборе `ViewController.designer.cs` в **Обозреватель решений** вы сможете увидеть, как `ClickedLabel` метка и `Clicked` обработчик событий были сопоставлены с **розеткой** и **действием** в C#:

[![Переменные экземпляров и действия](hello-tvos-images/accesscontrol.png)](hello-tvos-images/accesscontrol.png#lightbox)

Вы также можете заметить, что `ViewController.designer.cs` является разделяемым классом, поэтому Visual Studio для Mac не нужно изменять, `ViewController.cs` который перезапишет все изменения, внесенные в класс.

Предоставление элементов пользовательского интерфейса таким образом позволяет обращаться к ним в контроллере представления.

Обычно вам никогда не нужно открывать `ViewController.designer.cs` себя, он был представлен здесь только для образовательных целей.

<a name="Writing-the-Code"></a>

## <a name="writing-the-code"></a>Написание кода

После создания пользовательского интерфейса и его элементов пользовательского интерфейса, предоставляемых коду посредством **розеток** и **действий**, мы, наконец, готовы написать код для предоставления функциональных возможностей программы.

В нашем приложении при каждом нажатии первой кнопки мы собираемся обновить нашу метку, чтобы продемонстрировать, сколько раз была нажата кнопка. Чтобы сделать это, необходимо открыть `ViewController.cs` файл для редактирования, дважды щелкнув его в **панель решения**:

[![Панель решения](hello-tvos-images/code01.png)](hello-tvos-images/code01.png#lightbox)

Во-первых, нам нужно создать переменную уровня класса в нашем `ViewController` классе для контроля количества выполненных щелчков. Измените определение класса и придайте ему следующий вид:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Затем в том же классе ( `ViewController` ) необходимо переопределить метод **ViewDidLoad** и добавить код для установки начального сообщения для метки:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

Нам нужно использовать `ViewDidLoad` вместо другого метода, такого как `Initialize` , поскольку `ViewDidLoad` вызывается *после* загрузки ОС и создания экземпляра пользовательского интерфейса из `.storyboard` файла. Если мы попытались получить доступ к элементу управления Label до того `.storyboard` , как файл будет полностью загружен и создан, возникнет `NullReferenceException` ошибка, поскольку элемент управления "метка" еще не создавался.

Далее необходимо добавить код для реагирования на нажатие пользователем кнопки. Добавьте следующий объект к созданному разделяемому классу:

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

Этот код будет вызываться каждый раз, когда пользователь нажимает кнопку.

Теперь, когда все готово, мы готовы к созданию и тестированию приложения Xamarin. tvOS.

## <a name="testing-the-application"></a>Тестирование приложения

Пора создать и запустить наше приложение, чтобы убедиться, что оно выполняется должным образом. Мы можем выполнить сборку и запуск всего за один шаг, или же мы можем создать его, не запуская его.

При создании приложения можно выбрать тип создаваемой сборки:

- **Отладка** — отладочная сборка компилируется в файл "" (приложение) с дополнительными метаданными, что позволяет отлаживать происходящее во время выполнения приложения.
- **Release** — сборка выпуска также создает файл "", но не содержит отладочную информацию, поэтому она меньше и выполняется быстрее.  

Тип сборки можно выбрать в **селекторе конфигурации** в левом верхнем углу экрана Visual Studio для Mac:

[![Выберите тип сборки](hello-tvos-images/run01.png)](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>Построение приложения

В нашем случае нам требуется только отладочная сборка, поэтому убедитесь, что выбрана **Отладка** . Давайте сначала создадим наше приложение, нажав кнопку **⌘ B**, либо выбрав в меню **Сборка** пункт **собрать все**.

Если ошибок нет, в строке состояния Visual Studio для Mac появится сообщение **сборка прошла удачно** . Если возникли ошибки, проверьте проект и убедитесь, что выполнены все шаги правильно. Начните с подтверждения того, что код (как в Xcode, так и в Visual Studio для Mac) соответствует коду в учебнике.

### <a name="running-the-application"></a>Запуск приложения

Чтобы запустить приложение, у нас есть три варианта:

- Нажмите сочетание клавиш **⌘ + ВВОД**.
- В меню **Запуск** выберите **Отладка**.
- На панели инструментов Visual Studio для Mac нажмите кнопку **Воспроизведение** (сразу над **обозревателем решений**).

Приложение будет строиться (если оно еще не создано), запуститься в режиме отладки, будет запущен симулятор tvOS, и приложение запустится и отобразит главное окно интерфейса:

[![Начальный экран примера приложения](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

В меню **оборудование** выберите пункт **Отображать пульт дистанционного управления Apple TV** , чтобы вы могли управлять симулятором.

[![Выберите "Показывать удаленные Apple TV"](hello-tvos-images/run04.png)](hello-tvos-images/run04.png#lightbox)

Используя удаленное симулятор, при нажатии на кнопку несколько раз метка должна быть обновлена с учетом числа:

[![Метка с обновленным числом](hello-tvos-images/run05.png)](hello-tvos-images/run05.png#lightbox)

Поздравляем! Здесь мы рассмотрели много вас, но если вы пропустили это руководство от начала работы, вы получите полное представление о компонентах приложения Xamarin. tvOS, а также о средствах, используемых для их создания.

## <a name="where-to-next"></a>Далее?

Разработка приложений Apple TV представляет собой несколько проблем из-за разрыва связи между пользователем и интерфейсом (он находится в комнате, не наладонем пользователя), и ограничения tvOS места на размерах и хранилище приложений.

В результате мы настоятельно рекомендуем читать следующие документы перед переходом к проектированию приложения Xamarin. tvOS:

- [Введение в tvOS 9](~/ios/tvos/platform/tvos9.md) . в этой статье представлены все новые и измененные API-интерфейсы и функции, доступные в tvOS 9 для разработчиков Xamarin. tvOS.
- [Работа с навигацией и фокусом](~/ios/tvos/app-fundamentals/navigation-focus.md) . пользователи вашего приложения Xamarin. tvOS не будут взаимодействовать с интерфейсом напрямую, как с iOS, где они наследуют образы на экране устройства, но косвенно из комнаты, использующей Siri Remote. В этой статье рассматривается понятие фокуса и его использование для управления навигацией в пользовательском интерфейсе приложения Xamarin. tvOS.
- [Siri Remote and Bluetooth Controllers](~/ios/tvos/platform/remote-bluetooth.md) — основной способ взаимодействия пользователей с Apple TV и приложения Xamarin. tvOS — через включенный Siri Remote. Если приложение является игрой, при необходимости можно создать поддержку сторонних производителей для устройств iOS (MFI) и игровых контроллеров Bluetooth в приложении. В этой статье описывается поддержка новых Siri удаленных устройств и игровых контроллеров Bluetooth в приложениях Xamarin. tvOS.
- [Ресурсы и хранилище данных](~/ios/tvos/app-fundamentals/resources-data-storage.md) — в отличие от устройств iOS, новый Apple TV не предоставляет постоянное локальное хранилище для приложений tvOS. В результате, если ваше приложение Xamarin. tvOS должно сохранять информацию (например, предпочтения пользователя), оно должна хранить и получать эти данные из iCloud. В этой статье рассматривается работа с ресурсами и хранением постоянных данных в приложении Xamarin. tvOS.
- [Работа со значками и изображениями](~/ios/tvos/app-fundamentals/icons-images.md) . Создание значков и изображений привлекая является важной частью разработки впечатляющих пользователей для приложений Apple TV. В этом руководстве рассматриваются шаги, необходимые для создания и включения необходимых графических ресурсов для приложений Xamarin. tvOS.
- [Пользовательский интерфейс](~/ios/tvos/user-interface/index.md) : Общие сведения о пользовательском интерфейсе (UX), включая элементы управления пользовательского интерфейса, используют Interface Builder и принципы разработки UX при работе с Xamarin. tvOS.
- [Развертывание и тестирование](~/ios/tvos/deploy-test/index.md) — в этом разделе рассматриваются темы, используемые для тестирования приложения, а также способы его распространения. В этом разделе рассматриваются такие темы, как средства, используемые для отладки, развертывания в тест-инженерах и публикации приложения в Apple TV App Store.

Если у вас возникли проблемы, связанные с Xamarin. tvOS, ознакомьтесь с документацией по [устранению неполадок](~/ios/tvos/troubleshooting.md) , чтобы получить список известных проблем и решений.

## <a name="summary"></a>Сводка

В этой статье предоставлено краткое руководство по разработке приложений для tvOS с Visual Studio для Mac путем создания простого приложения Hello, tvOS. В нем были рассмотрены основы подготовки устройств tvOS, создания интерфейса, кодирования для tvOS и тестирования в симуляторе tvOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Руководства по tvOSму интерфейсу](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Руководством по программированию приложений для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Создание приложений для tvOS с помощью Xamarin (видео)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)