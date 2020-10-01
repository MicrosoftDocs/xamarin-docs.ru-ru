---
title: Производительность Xamarin.iOS
description: В этом документе описываются методы, с помощью которых можно повысить производительность и использование памяти в приложениях Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/29/2016
ms.openlocfilehash: 06d413b2bf07df38f78e93af027de2c7dc0badcc
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436781"
---
# <a name="xamarinios-performance"></a>Производительность Xamarin.iOS

Низкая производительность приложения проявляется по-разному. Из-за нее приложение может переставать отвечать на запросы, могут возникать задержки при прокрутке и сокращаться время работы батареи. Однако оптимизация производительности предусматривает не только правильную реализацию кода. Необходимо также учитывать эффективность работы пользователей. Например, чтобы повысить удобство работы, необходимо сделать так, чтобы выполнение операций не мешало пользователю выполнять другие действия.

В этом документе описываются методы, с помощью которых можно повысить производительность и использование памяти в приложениях Xamarin.iOS.

> [!NOTE]
> Прежде чем прочитать эту статью, ознакомьтесь со статьей [Кроссплатформенная производительность](~/cross-platform/deploy-test/memory-perf-best-practices.md), в которой описываются универсальные для всех платформ способы оптимизации использования памяти и повышения производительности приложений, построенных с помощью платформы Xamarin.

## <a name="avoid-strong-circular-references"></a>Устранение циклических строгих ссылок

В некоторых ситуациях могут образовываться циклы строгих ссылок, из-за которых память, занимаемая объектами, может не освобождаться сборщиком мусора. Например, рассмотрим ситуацию, когда подкласс, производный от [`NSObject`](xref:Foundation.NSObject), например класс, наследуемый от [`UIView`](xref:UIKit.UIView), добавляется в контейнер, производный от `NSObject`, и на него имеется строгая ссылка из Objective-C, как показано в следующем примере кода:

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    Container parent;
    public MyView (Container parent)
    {
        this.parent = parent;
    }

    void PokeParent ()
    {
        parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Когда в этом коде создается экземпляр `Container`, объект C# будет иметь строгую ссылку на объект Objective-C. Аналогичным образом, экземпляр `MyView` будет также иметь строгую ссылку на объект Objective-C.

Кроме того, вызов `container.AddSubview` будет увеличивать число ссылок в неуправляемом экземпляре `MyView`. В таком случае среда выполнения Xamarin.iOS создает экземпляр `GCHandle`, чтобы поддерживать объект `MyView` в управляемом коде в активном состоянии, так как нет гарантии, что в управляемых объектах будет сохраняться ссылка на него. С точки зрения управляемого кода, память, занимаемая объектом `MyView`, освобождалась бы после вызова [`AddSubview`](xref:UIKit.UIView.AddSubview(UIKit.UIView)), если бы не экземпляр `GCHandle`.

Неуправляемый объект `MyView` будет иметь экземпляр `GCHandle`, указывающий на управляемый объект, что называется *строгой связью*. Управляемый объект будет содержать ссылку на экземпляр `Container`. Экземпляр `Container`, в свою очередь, будет иметь управляемую ссылку на объект `MyView`.

В ситуациях, когда содержащийся объект сохраняет ссылку на свой контейнер, есть несколько способов, как поступить с циклической ссылкой:

- Разорвите цикл вручную, присвоив ссылке на контейнер значение `null`.
- Вручную удалите содержащийся объект из контейнера.
- Вызовите метод `Dispose` для объектов.
- Чтобы избежать циклической ссылки, используйте слабую ссылку на контейнер. Дополнительные сведения о слабых ссылках см. ниже.

### <a name="using-weakreferences"></a>Использование слабых ссылок

Чтобы предотвратить возникновение цикла, можно использовать слабую ссылку из дочернего в родительский элемент. Например, приведенный выше код можно записать следующим образом:

```csharp
class Container : UIView
{
    public void Poke ()
    {
        // Call this method to poke this object
    }
}

class MyView : UIView
{
    WeakReference<Container> weakParent;
    public MyView (Container parent)
    {
        this.weakParent = new WeakReference<Container> (parent);
    }

    void PokeParent ()
    {
        if (weakParent.TryGetTarget (out var parent))
            parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Здесь содержащийся объект не проверяет активности родительского элемента. Однако родительский элемент проверяет активность дочернего путем вызова к `container.AddSubView`.

Это происходит и в интерфейсах API iOS на основе шаблона делегата или источника данных, где равноправный класс содержит реализацию, например при задании свойства [`Delegate`](xref:UIKit.UITableView.Delegate*)
или [`DataSource`](xref:UIKit.UITableView.DataSource*)
в классе [`UITableView`](xref:UIKit.UITableView).

В случае с классами, которые создаются исключительно для реализации протокола, например [`IUITableViewDataSource`](xref:UIKit.IUITableViewDataSource), вместо создания подкласса вы можете просто реализовать интерфейс в классе, переопределить метод и присвоить свойство `DataSource` указателю `this`.

#### <a name="weak-attribute"></a>Слабый атрибут

В [Xamarin.iOS 11.10](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_11/xamarin.ios_11.10.md#WeakAttribute) появился атрибут `[Weak]`. Как и `WeakReference <T>`, `[Weak]` можно использовать для разрушения [строгих циклических ссылок](#avoid-strong-circular-references), но при этом код будет еще короче.

Рассмотрим приведенный ниже код, где используется `WeakReference <T>`:

```csharp
public class MyFooDelegate : FooDelegate {
    WeakReference<MyViewController> controller;
    public MyFooDelegate (MyViewController ctrl) => controller = new WeakReference<MyViewController> (ctrl);
    public void CallDoSomething ()
    {
        MyViewController ctrl;
        if (controller.TryGetTarget (out ctrl)) {
            ctrl.DoSomething ();
        }
    }
}
```

Эквивалентный код с `[Weak]` гораздо короче:

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

Ниже приведен еще один пример использования `[Weak]` в контексте шаблона [делегирования](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html):

```csharp
public class MyViewController : UIViewController
{
    WKWebView webView;

    protected MyViewController (IntPtr handle) : base (handle) { }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        webView = new WKWebView (View.Bounds, new WKWebViewConfiguration ());
        webView.UIDelegate = new UIDelegate (this);
        View.AddSubview (webView);
    }
}

public class UIDelegate : WKUIDelegate
{
    [Weak] MyViewController controller;

    public UIDelegate (MyViewController ctrl) => controller = ctrl;

    public override void RunJavaScriptAlertPanel (WKWebView webView, string message, WKFrameInfo frame, Action completionHandler)
    {
        var msg = $"Hello from: {controller.Title}";
        var alertController = UIAlertController.Create (null, msg, UIAlertControllerStyle.Alert);
        alertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
        controller.PresentViewController (alertController, true, null);
        completionHandler ();
    }
}
```

### <a name="disposing-of-objects-with-strong-references"></a>Освобождение объектов со строгими ссылками

Если существует строгая ссылка и трудно удалить зависимость, очистите родительский указатель с помощью метода `Dispose`.

В случае с контейнерами переопределите метод `Dispose`, чтобы удалить содержащиеся объекты, как показано в следующем примере кода:

```csharp
class MyContainer : UIView
{
    public override void Dispose ()
    {
        // Brute force, remove everything
        foreach (var view in Subviews)
        {
              view.RemoveFromSuperview ();
        }
        base.Dispose ();
    }
}
```

В случае с дочерним объектом, сохраняющим строгую ссылку на свой родительский объект, очистите ссылку на родительский объект в реализации `Dispose`:

```csharp
class MyChild : UIView
{
    MyContainer container;
    public MyChild (MyContainer container)
    {
        this.container = container;
    }
    public override void Dispose ()
    {
        container = null;
    }
}
```

Дополнительные сведения об освобождении строгих ссылок см. в разделе [Освобождение ресурсов IDisposable](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
Полезную информацию можно также найти в следующей записи блога: [Xamarin.iOS, the garbage collector and me](https://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me) (Xamarin.iOS, сборщик мусора и мои наблюдения).

### <a name="more-information"></a>Дополнительные сведения

Дополнительные сведения см. в записи [Rules to Avoid Retain Cycles](https://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) (Правила по избежанию сохраняемых циклов) в блоге Cocoa With Love, на странице [Is this a bug in MonoTouch GC](https://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) (Является ли это ошибкой в сборщике мусора MonoTouch) на сайте StackOverflow и на странице [Why can't MonoTouch GC kill managed objects with refcount > 1?](https://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) (Почему сборщик мусора MonoTouch не может уничтожать управляемые объекты с refcount > 1?) на сайте StackOverflow.

## <a name="optimize-table-views"></a>Оптимизация табличных представлений

Пользователи рассчитывают на быструю прокрутку и загрузку экземпляров [`UITableView`](xref:UIKit.UITableView). Если ячейки будут содержать иерархии представлений с глубоким уровнем вложения или сложные макеты, производительность прокрутки может заметно снижаться. Тем не менее существует несколько способов повысить производительность `UITableView`:

- Повторное использование ячеек. Дополнительные сведения см. в разделе [Повторное использование ячеек](#reuse-cells).
- Сокращение числа вложенных представлений.
- Кэширование содержимого ячейки, извлеченного из веб-службы.
- Кэширование высоты строк, если они не идентичны.
- Придание непрозрачности ячейке и любым другим представлениям.
- Избежание масштабирования изображения и градиентов.

Совместное применение этих способов позволяет обеспечить плавную прокрутку экземпляров [`UITableView`](xref:UIKit.UITableView).

### <a name="reuse-cells"></a>Повторное использование ячеек

Если в [`UITableView`](xref:UIKit.UITableView) отображаются сотни строк, на создание сотен объектов [`UITableViewCell`](xref:UIKit.UITableViewCell) затрачивается большой объем памяти, хотя при этом на экране одновременно отображается лишь несколько из них. Вместо этого в память можно загружать только многократно используемые ячейки, которые отображаются на экране, загружая в них необходимое **содержимое**. Это позволяет избежать создания сотен дополнительных объектов и значительно сэкономить время и ресурсы памяти.

Таким образом, если ячейка пропадает с экрана, ее представление может быть помещено в очередь для повторного использования, как показано в следующем примере кода:

```csharp
class MyTableSource : UITableViewSource
{
    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        // iOS will create a cell automatically if one isn't available in the reuse pool
        var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

        // Perform required cell actions
        return cell;
    }
}
```

Во время прокрутки [`UITableView`](xref:UIKit.UITableView) вызывает переопределение `GetCell` для запроса отображения новых представлений. Это переопределение затем вызывает метод [`DequeueReusableCell`](xref:UIKit.UITableView.DequeueReusableCell(Foundation.NSString)), и если ячейка доступна для повторного использования, она будет возвращена.

Дополнительные сведения см. в разделе [Повторное использование ячеек](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) статьи [Заполнение таблицы данными](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

## <a name="use-opaque-views"></a>Использование непрозрачных представлений

Убедитесь в том, что у всех представлений, для которых не определена прозрачность, задано свойство [`Opaque`](xref:UIKit.UIView.Opaque). Это позволит обеспечить оптимальное преобразование представлений для просмотра системой отрисовки. Это особенно важно, если представление внедрено в [`UIScrollView`](xref:UIKit.UIScrollView) или является частью сложной анимации. В противном случае система отрисовки будет сочетать представления с другим содержимым, что может заметно сказаться на производительности.

## <a name="avoid-fat-xibs"></a>Избежание толстых файлов XIB

Хотя файлы XIB были в основном заменены раскадровками, они по-прежнему могут применяться в некоторых ситуациях. Когда файл XIB загружается в память, все его содержимое, включая изображения, также загружается в память. Если файл XIB содержит представление, которое не используется немедленно, оно напрасно занимает память. Поэтому при использовании файлов XIB необходимо делать так, чтобы на каждый контроллер представления приходился только один файл XIB, и по возможности разделять иерархию контроллеров представлений на отдельные файлы XIB.

## <a name="optimize-image-resources"></a>Оптимизация графических ресурсов

Изображения — это один из самых ресурсоемких ресурсов в приложениях. Они часто хранятся в высоком разрешении. Поэтому при отображении изображения из пакета приложения в [`UIImageView`](xref:UIKit.UIImageView) размеры изображения и `UIImageView` должны быть одинаковыми. Масштабирование изображений во время выполнения может быть ресурсоемкой операцией, особенно если представление `UIImageView` внедрено в [`UIScrollView`](xref:UIKit.UIScrollView).

Дополнительные сведения см. в разделе [Оптимизация графических ресурсов](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) в руководстве [Кроссплатформенная производительность](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="test-on-devices"></a>Тестирование на устройствах

К развертыванию и тестированию приложения на физических устройствах следует приступать как можно раньше. Симуляторы не отражают особенности работы и ограничения устройств с абсолютной точностью, поэтому важно проводить тесты на реальных устройствах начиная с самых ранних этапов разработки.

В частности, симулятор никоим образом не воссоздает ограничения памяти и ЦП физического устройства.

## <a name="synchronize-animations-with-the-display-refresh"></a>Синхронизация анимации с обновлением экрана

В играх выполнение игровой логики и обновление экрана происходят с высокой частотой. Как правило, экран обновляется с частотой от тридцати до шестидесяти кадров в секунду. Некоторые разработчики стремятся достичь максимальной частоты обновления экрана и пытаются превзойти показатель в шестьдесят кадров в секунду.

Однако сервер дисплея производит его обновление с максимальной частотой шестьдесят раз в секунду. Поэтому попытка обновлять экран чаще может привести к разрывам изображения и небольшим задержкам. Код лучше структурировать так, чтобы обновление экрана было синхронизировано с обновлением дисплея. Этого можно достичь с помощью класса [`CoreAnimation.CADisplayLink`](xref:CoreAnimation.CADisplayLink), который представляет собой таймер, подходящий для визуализации и игр с частотой обновления шестьдесят кадров в секунду.

## <a name="avoid-core-animation-transparency"></a>Избежание прозрачности в базовой анимации

Избегайте прозрачности в базовой анимации, чтобы повысить производительность компоновки растровых изображений. Как правило, следует по возможности избегать прозрачных слоев и размытых границ.

## <a name="avoid-code-generation"></a>Избежание создания кода

Динамического создания кода с помощью `System.Reflection.Emit` или *среды DLR* следует избегать, так как ядро iOS запрещает выполнение динамического кода.

## <a name="summary"></a>Сводка

В этой статье были рассмотрены методы повышения производительности приложений, созданных на платформе Xamarin.iOS. Вместе они могут значительно снизить загрузку ЦП и сократить объем памяти, используемой приложением.

## <a name="related-links"></a>Связанные ссылки

- [Кроссплатформенная производительность](~/cross-platform/deploy-test/memory-perf-best-practices.md)