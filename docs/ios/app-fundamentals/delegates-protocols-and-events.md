---
title: События, протоколы и делегаты в Xamarin. iOS
description: В этом документе описывается работа с событиями, протоколами и делегатами в Xamarin. iOS. Эти фундаментальные понятия являются повсеместными в разработке Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/17/2017
ms.openlocfilehash: ce436f907c70657ff6d08f39bdec9e7d796d519c
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431025"
---
# <a name="events-protocols-and-delegates-in-xamarinios"></a>События, протоколы и делегаты в Xamarin. iOS

Xamarin. iOS использует элементы управления для предоставления событий большинству пользовательских взаимодействий.
Приложения Xamarin. iOS используют эти события во многом так же, как и традиционные приложения .NET. Например, класс Xamarin. iOS Уибуттон имеет событие с именем Таучупинсиде и использует это событие так же, как если бы этот класс и событие находились в приложении .NET.

Помимо этого подхода .NET, Xamarin. iOS предоставляет другую модель, которая может использоваться для более сложного взаимодействия и привязки данных. В этой методологии используются делегаты и протоколы Apple Calls. Делегаты похожи на делегаты в C#, но вместо определения и вызова одного метода делегат в цели-C является целым классом, который соответствует протоколу. Протокол аналогичен интерфейсу в C#, за исключением того, что его методы могут быть необязательными. Например, чтобы заполнить Уитаблевиев данными, необходимо создать класс делегата, реализующий методы, определенные в протоколе Уитаблевиевдатасаурце, который Уитаблевиев вызовет для заполнения самого себя.

В этой статье вы узнаете обо всех этих темах, обеспечивая надежную основу для обработки сценариев обратного вызова в Xamarin. iOS, включая:

- **События** — использование событий .NET с элементами управления UIKit.
- **Протоколы** — изучение протоколов и их использования, а также создание примера, предоставляющего данные для заметки на карте.
- **Делегаты** — изучение делегатов цели-C путем расширения примера Map для управления взаимодействием с пользователем, включающим заметку, а затем изучения различий между строгими и слабыми делегатами и моментами использования каждого из них.

Чтобы продемонстрировать протоколы и делегаты, мы создадим простое приложение Map, которое добавляет к карте заметку, как показано ниже:

[ ![ Пример простого приложения для карт, добавляющего заметку к сопоставлению](delegates-protocols-and-events-images/01-map-sml.png)](delegates-protocols-and-events-images/01-map.png#lightbox). 
 [ ![ Пример заметки, добавленный на карту](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png)](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Прежде чем приступить к работе с этим приложением, давайте начнем с просмотра событий .NET в UIKit.

## <a name="net-events-with-uikit"></a>События .NET с UIKit

Xamarin. iOS предоставляет события .NET для элементов управления UIKit. Например, Уибуттон имеет событие Таучупинсиде, которое обрабатывается как обычно в .NET, как показано в следующем коде, использующем лямбда-выражение C#:

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

Это также можно реализовать с помощью анонимного метода в стиле C# 2,0, подобного приведенному ниже.

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

Приведенный выше код работает в `ViewDidLoad` методе UIViewController. `aButton`Переменная ссылается на кнопку, которую можно добавить либо в конструкторе iOS, либо с помощью кода. На следующем рисунке показана кнопка, добавленная в конструктор iOS:

[![Кнопка, добавленная в конструктор iOS](delegates-protocols-and-events-images/02-interface-builder-outlet-sml.png)](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin. iOS также поддерживает стиль целевого действия по подключению кода к взаимодействию, которое происходит с элементом управления. Чтобы создать целевое действие для кнопки **Hello** , дважды щелкните его в конструкторе iOS. Будет отображен файл кода программной части UIViewController, и разработчику будет предложено выбрать расположение для вставки метода подключения:

[![Файл кода программной части Уивиевконтроллерс](delegates-protocols-and-events-images/03-interface-builder-action-sml.png)](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

После выбора расположения создается новый метод, который поддается подсети элементу управления. В следующем примере сообщение будет записано в консоль при нажатии кнопки:

[![При нажатии кнопки на консоль будет выведено сообщение](delegates-protocols-and-events-images/05-interface-builder-action-sml.png)](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

Дополнительные сведения о шаблоне целевого действия iOS см. в разделе "Целевая операция" [базовых компетенций приложения для iOS](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html) в библиотеке разработчиков iOS Apple.

Дополнительные сведения об использовании конструктора iOS с Xamarin. iOS см. в обзорной документации по [конструктору iOS](~/ios/user-interface/designer/index.md) .

## <a name="events"></a>События

Если вы хотите перехватить события из Уиконтрол, у вас есть ряд параметров: от использования лямбда-выражений и делегатов C# до использования интерфейсов API цели низкого уровня.

В следующем разделе показано, как вы захватываете событие TouchDown на кнопке в зависимости от требуемого объема управления.

## <a name="c-style"></a>Стиль C#

Использование синтаксиса делегата:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {
    Console.WriteLine ("Touched");
};
```

Если вместо этого вы хотите использовать лямбда-выражения:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

Если требуется, чтобы несколько кнопок использовали один и тот же обработчик для совместного использования одного и того же кода:

```csharp
void handler (object sender, EventArgs args)
{
   if (sender == button1)
      Console.WriteLine ("button1");
   else
      Console.WriteLine ("some other button");
}

button1.TouchDown += handler;
button2.TouchDown += handler;
```

## <a name="monitoring-more-than-one-kind-of-event"></a>Наблюдение за несколькими видами событий

События C# для флагов Уиконтролевент имеют сопоставление «один к одному» с отдельными флагами. Если требуется, чтобы один и тот же фрагмент кода обрабатывал два или более событий, используйте `UIControl.AddTarget` метод:

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Использование лямбда-синтаксиса:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Если необходимо использовать низкоуровневые функции цели-C, такие как подключение к конкретному экземпляру объекта и вызов определенного селектора:

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

Обратите внимание, что при реализации метода экземпляра в унаследованном базовом классе он должен быть открытым методом.

## <a name="protocols"></a>Протоколы

Протокол — это функция языка с целевым языком, которая предоставляет список объявлений методов. Он аналогичен интерфейсу в C#, но основное различие состоит в том, что протокол может иметь необязательные методы. Необязательные методы не вызываются, если класс, который внедряет протокол, не реализует их. Кроме того, один класс в цели-C может реализовывать несколько протоколов, точно так же как класс C# может реализовывать несколько интерфейсов.

Apple использует протоколы в iOS для определения контрактов для используемых классов, в то время как абстрактный класс от вызывающего объекта работает так же, как и интерфейс C#. Протоколы используются как в сценариях, не являющихся делегатами (например, в `MKAnnotation` примере ниже), так и с делегатами (как показано далее в этом документе в разделе делегаты).

### <a name="protocols-with-xamarinios"></a>Протоколы с Xamarin. iOS

Рассмотрим пример с использованием протокола цели-C из Xamarin. iOS. В этом примере мы будем использовать `MKAnnotation` протокол, который является частью `MapKit` платформы. `MKAnnotation` — это протокол, позволяющий любому объекту, который принимает его, предоставлять сведения о заметке, которую можно добавить к карте. Например, объект, реализующий, `MKAnnotation` предоставляет расположение заметки и связанный с ней заголовок.

Таким образом, `MKAnnotation` протокол используется для предоставления необходимых данных, сопровождающих заметку. Фактическое представление заметки создается на основе данных в объекте, который использует `MKAnnotation` протокол. Например, текст для выноски, который появляется при касании пользователем заметки (как показано на снимке экрана ниже), берется из `Title` Свойства класса, реализующего протокол:

 [![Пример текста для вызова при касании пользователем заметки](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png)](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Как описано в следующем разделе, « [протоколы глубокое углубление](#protocols-deep-dive)», Xamarin. iOS привязывает протоколы к абстрактным классам. Для `MKAnnotation` протокола связанный класс C# называется, `MKAnnotation` чтобы имитировать имя протокола, а является подклассом `NSObject` корневого базового класса для кокоатауч. Для этого протокола требуется реализация метода получения и задания для координаты; Однако заголовок и подзаголовок необязательны. Поэтому в `MKAnnotation` классе `Coordinate` свойство является *абстрактным*, требует его реализации, а `Title` `Subtitle` Свойства и помечаются как *Virtual*, делая их необязательными, как показано ниже:

```csharp
[Register ("MKAnnotation"), Model ]
public abstract class MKAnnotation : NSObject
{
    public abstract CLLocationCoordinate2D Coordinate
    {
        [Export ("coordinate")]
        get;
        [Export ("setCoordinate:")]
        set;
    }

    public virtual string Title
    {
        [Export ("title")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }

    public virtual string Subtitle
    {
        [Export ("subtitle")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }
...
}
```

Любой класс может предоставлять данные аннотации простым наследованием от `MKAnnotation` , пока `Coordinate` не будет реализовано по крайней мере свойство. Например, ниже приведен пример класса, который принимает координаты в конструкторе и возвращает строку для заголовка:

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return title;
        }
    }
}
```

С помощью протокола, к которому он привязан, любой класс, который подклассы `MKAnnotation` может предоставить соответствующие данные, которые будут использоваться картой при создании представления заметки. Чтобы добавить заметку к сопоставлению, просто вызовите `AddAnnotation` метод `MKMapView` экземпляра, как показано в следующем коде:

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
    new CLLocationCoordinate2D (42.3467512, -71.0969456); // Boston

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

Переменная Map здесь является экземпляром `MKMapView` класса, который представляет собой класс, представляющий саму карту. `MKMapView`Будет использовать данные, `Coordinate` производные от `SampleMapAnnotation` экземпляра, для позиционирования представления заметок на карте.

`MKAnnotation`Протокол предоставляет известный набор возможностей для любых объектов, реализующих его, без потребителя (в данном случае — карта), которым необходимо знать сведения о реализации. Это упрощает добавление в карту множества возможных аннотаций.

### <a name="protocols-deep-dive"></a>Подробный обзор протоколов

Поскольку интерфейсы C# не поддерживают дополнительные методы, Xamarin. iOS сопоставляет протоколы с абстрактными классами. Таким образом, внедрение протокола в цель-C выполняется в Xamarin. iOS путем наследования от абстрактного класса, привязанного к протоколу и реализации необходимых методов. Эти методы будут представлены в классе как абстрактные методы. Необязательные методы из протокола будут привязаны к виртуальным методам класса C#.

Например, ниже приведена часть `UITableViewDataSource` протокола, привязанная в Xamarin. iOS:

```csharp
public abstract class UITableViewDataSource : NSObject
{
    [Export ("tableView:cellForRowAtIndexPath:")]
    public abstract UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath);
    [Export ("numberOfSectionsInTableView:")]
    public virtual int NumberOfSections (UITableView tableView){...}
...
}
```

Обратите внимание, что класс является абстрактным. Xamarin. iOS делает класс абстрактным для поддержки необязательных или обязательных методов в протоколах.
Однако в отличие от протоколов цели-C (или интерфейсов C#) классы C# не поддерживают множественное наследование. Это влияет на структуру кода C#, использующего протоколы, и обычно приводит к вложенным классам. Дополнительные сведения об этой проблемы описаны далее в этом документе в разделе делегаты.

 `GetCell(…)` — Это абстрактный метод, привязанный к *селектору*цели-C, `tableView:cellForRowAtIndexPath:` который является обязательным методом `UITableViewDataSource` протокола. Селектор — это термин цели-C для имени метода. Для принудительного применения этого метода Xamarin. iOS объявляет его как абстрактный. Другой метод, `NumberOfSections(…)` , привязан к `numberOfSectionsInTableview:` . Этот метод является необязательным в протоколе, поэтому Xamarin. iOS объявляет его как виртуальный, делая его необязательным для переопределения в C#.

Xamarin. iOS берет на себя всю привязку iOS. Тем не менее, если вам когда-либо потребуется привязать протокол из цели – C вручную, это можно сделать путем оформления класса с помощью `ExportAttribute` . Это тот же метод, который используется в самой Xamarin. iOS.

Дополнительные сведения о том, как привязать типы цели-C в Xamarin. iOS, см. в статье [типы целевой привязки статьи-c](~/ios/platform/binding-objective-c/index.md).

Однако мы еще не работаем с протоколами. Они также используются в iOS в качестве базиса для делегатов цели-C, что является темой следующего раздела.

## <a name="delegates"></a>Делегаты

в iOS используются делегаты цели-C для реализации шаблона делегирования, в котором один объект передает данные в другую. Объект, выполняющий работу, является делегатом первого объекта. Объект сообщает своему делегату о необходимости выполнять работу, отправляя ИТ сообщения после определенного момента. Отправка такого сообщения, как и в случае задания, функционально эквивалентно вызову метода в C#. Делегат реализует методы в ответ на эти вызовы, поэтому предоставляет приложению функциональные возможности.

Делегаты позволяют расширять поведение классов без необходимости создавать подклассы. Приложения в iOS часто используют делегаты, когда один класс обращается к другому после возникновения важного действия. Например, `MKMapView` класс обращается к своему делегату, когда пользователь касается заметки на карте, предоставляя автору класса делегата возможность отвечать в приложении. Вы можете использовать пример этого типа использования делегата далее в этой статье, в примере с помощью делегата с Xamarin. iOS.

На этом этапе может быть интересно, как класс определяет, какие методы вызывать для своего делегата. Это еще одно место, где используются протоколы. Как правило, методы, доступные для делегата, берутся из протоколов, которые они принимают.

### <a name="how-protocols-are-used-with-delegates"></a>Использование протоколов с делегатами

Ранее мы видели, как протоколы используются для поддержки добавления заметок к карте.
Протоколы также используются для предоставления известного набора методов для вызовов класса после определенных событий, например после того, как пользователь отметит заметку на карте или выберет ячейку в таблице. Классы, реализующие эти методы, называются делегатами классов, которые их вызывают.

Классы, поддерживающие делегирование, делают это, предоставляя свойство делегата, которому назначается класс, реализующий делегат. Методы, реализуемые для делегата, зависят от протокола, к которому применяется конкретный делегат. Для `UITableView` метода вы реализуете `UITableViewDelegate` протокол для `UIAccelerometer` метода, реализуете `UIAccelerometerDelegate` и т. д. для любых других классов в рамках iOS, для которых требуется предоставить делегат.

`MKMapView`Класс, который мы видели в предыдущем примере, также имеет свойство Delegate, которое будет вызываться после возникновения различных событий. Делегат для `MKMapView` имеет тип `MKMapViewDelegate` .
Этот пример будет использоваться чуть ниже в примере для реагирования на заметку после ее выбора, но сначала давайте рассмотрим разницу между строгими и слабыми делегатами.

### <a name="strong-delegates-vs-weak-delegates"></a>Строгие делегаты и слабые делегаты

Делегаты, которые мы рассматривали до сих пор, являются надежными делегатами, то есть они являются строго типизированными. Привязки Xamarin. iOS поставляются со строго типизированным классом для каждого протокола делегата в iOS. Однако iOS также имеет концепцию ненадежного делегата. Вместо создания подкласса, привязанного к протоколу цели-C для конкретного делегата, iOS также позволяет самостоятельно привязывать методы протокола к любому классу, производному от Нсобжект, дополняя методы Експортаттрибуте, а затем предоставляя соответствующие селекторы.
При использовании этого подхода экземпляр класса назначается свойству Веакделегате, а не свойству делегата. Слабый делегат обеспечивает гибкость для использования класса делегата в другой иерархии наследования. Рассмотрим пример Xamarin. iOS, использующий как надежные, так и слабые делегаты.

### <a name="example-using-a-delegate-with-xamarinios"></a>Пример использования делегата с Xamarin. iOS

Чтобы выполнить код в ответ на попытку пользователя коснуться заметки в нашем примере, можно `MKMapViewDelegate` создать подкласс и назначить экземпляр `MKMapView` `Delegate` свойству. `MKMapViewDelegate`Протокол содержит только необязательные методы.
Таким образом, все методы являются виртуальными, привязанными к этому протоколу в классе Xamarin. iOS `MKMapViewDelegate` . Когда пользователь выбирает заметку, `MKMapView` экземпляр отправляет `mapView:didSelectAnnotationView:` сообщение его делегату. Чтобы справиться с этим в Xamarin. iOS, необходимо переопределить `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` метод в подклассе мкмапвиевделегате следующим образом:

```csharp
public class SampleMapDelegate : MKMapViewDelegate
{
    public override void DidSelectAnnotationView (
        MKMapView mapView, MKAnnotationView annotationView)
    {
        var sampleAnnotation =
            annotationView.Annotation as SampleMapAnnotation;

        if (sampleAnnotation != null) {

            //demo accessing the coordinate of the selected annotation to
            //zoom in on it
            mapView.Region = MKCoordinateRegion.FromDistance(
                sampleAnnotation.Coordinate, 500, 500);

            //demo accessing the title of the selected annotation
            Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
        }
    }
}
```

Показанный выше класс Самплемапделегате реализуется как вложенный класс в контроллере, содержащем `MKMapView` экземпляр. В цели-C вы часто видите, что контроллер принимает несколько протоколов непосредственно в пределах класса. Однако поскольку протоколы привязаны к классам в Xamarin. iOS, классы, реализующие строго типизированные делегаты, обычно включаются как вложенные классы.

После реализации класса делегата необходимо только создать экземпляр делегата в контроллере и присвоить его `MKMapView` `Delegate` свойству, как показано ниже:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    SampleMapDelegate _mapDelegate;
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //set the map's delegate
        _mapDelegate = new SampleMapDelegate ();
        map.Delegate = _mapDelegate;
        ...
    }
    class SampleMapDelegate : MKMapViewDelegate
    {
        ...
    }
}
```

Чтобы использовать слабый делегат для выполнения той же задачи, необходимо самостоятельно привязать метод к любому классу, производному от, `NSObject` и присвоить его `WeakDelegate` свойству объекта `MKMapView` . Так как `UIViewController` класс в конечном итоге является производным от `NSObject` (как и каждый класс цели-C в кокоатауч), мы можем просто реализовать метод `mapView:didSelectAnnotationView:` , привязанный непосредственно к контроллеру, и назначить контроллеру `MKMapView` `WeakDelegate` , избегая необходимости дополнительного вложенного класса. Этот подход показан в приведенном ниже коде.

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        //assign the controller directly to the weak delegate
        map.WeakDelegate = this;
    }
    //bind to the Objective-C selector mapView:didSelectAnnotationView:
    [Export("mapView:didSelectAnnotationView:")]
    public void DidSelectAnnotationView (MKMapView mapView,
        MKAnnotationView annotationView)
    {
        ...
    }
}
```

При выполнении этого кода приложение работает точно так же, как и при выполнении строго типизированной версии делегата. Преимуществом этого кода является то, что слабый делегат не требует создания дополнительного класса, созданного при использовании строго типизированного делегата. Однако это касается расходов типа "безопасность". Если бы в селекторе, переданном в, была допущена ошибка `ExportAttribute` , не удастся обнаружить время выполнения.

### <a name="events-and-delegates"></a>События и делегаты

Делегаты используются для обратных вызовов в iOS аналогично тому, как .NET использует события. Чтобы сделать API iOS и способ использования делегатов цели-C более похожими на .NET, Xamarin. iOS предоставляет события .NET во многих местах, где делегаты используются в iOS.

Например, Предыдущая реализация, в которой `MKMapViewDelegate` ответ на выбранную аннотацию, можно также реализовать в Xamarin. iOS с помощью события .NET. В этом случае событие задается в `MKMapView` и вызывается `DidSelectAnnotationView` . Он будет иметь `EventArgs` подкласс типа `MKMapViewAnnotationEventsArgs` . `View`Свойство объекта `MKMapViewAnnotationEventsArgs` дает вам ссылку на представление заметки, из которого можно продолжить ту же реализацию, которую вы ранее использовали, как показано ниже:

```csharp
map.DidSelectAnnotationView += (s,e) => {
    var sampleAnnotation = e.View.Annotation as SampleMapAnnotation;
    if (sampleAnnotation != null) {
        //demo accessing the coordinate of the selected annotation to
        //zoom in on it
        mapView.Region = MKCoordinateRegion.FromDistance (
            sampleAnnotation.Coordinate, 500, 500);

        //demo accessing the title of the selected annotation
        Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
    }
};
```

## <a name="summary"></a>Сводка

В этой статье описано, как использовать события, протоколы и делегаты в Xamarin. iOS. Мы увидели, как Xamarin. iOS предоставляет обычные события стиля .NET для элементов управления.
Далее мы узнали о протоколах цели-C, в том числе о том, как они отличаются от интерфейсов C# и от того, как Xamarin. iOS использует их. Наконец, мы рассматривали делегаты цели-C с точки зрения Xamarin. iOS. Мы увидели, как Xamarin. iOS поддерживает как строго, так и слабо типизированные делегаты, так и как привязывать события .NET к методам делегатов.

## <a name="related-links"></a>Связанные ссылки

- [Протоколы, делегаты и события (пример)](/samples/xamarin/ios-samples/protocols-delegates-events)
- [Привет, iOS](~/ios/get-started/hello-ios/index.md)
- [Типы цели привязки-C](~/ios/platform/binding-objective-c/index.md)
- [Язык программирования "цель-C"](https://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Проектирование пользовательских интерфейсов в Xcode 4](https://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [Компетенции основных приложений для iOS](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)