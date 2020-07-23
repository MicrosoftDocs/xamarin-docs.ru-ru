---
title: Отслеживание пальцев с несколькими касаниями в Xamarin. iOS
description: В этом документе описывается, как отслеживание отдельных пальцев в жестовх с несколькими касаниями в приложении Xamarin. iOS. Пример приложения для рисования на основе пальца.
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: b1ba548135cedd951d7f0a349f273b29182839d1
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928683"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Отслеживание пальцев с несколькими касаниями в Xamarin. iOS

_В этом документе показано, как отвести события касания от нескольких пальцев_

Иногда многоприкосновенное приложение должно относиться к отдельным пальцам, когда они одновременно перемещаются на экране. Одним из типичных приложений является программа рисования пальца. Вы хотите, чтобы пользователь мог рисовать с одним пальцем, а также рисовать с несколькими пальцами одновременно. По мере того как программа обрабатывает несколько событий касания, она должна различать эти пальцы.

Когда палец впервые касается экрана, iOS создает [`UITouch`](xref:UIKit.UITouch) объект для этого пальца. Этот объект остается таким же, как палец перемещается на экране, а затем убирается с экрана, после чего объект удаляется. Чтобы контролировать пальцы, программа должна избегать `UITouch` непосредственного сохранения этого объекта. Вместо этого он может использовать [`Handle`](xref:Foundation.NSObject.Handle) свойство типа `IntPtr` для уникальной идентификации этих `UITouch` объектов.

Почти всегда программа, которая отслеживает отдельных пальцев, поддерживает словарь для отслеживания касаний. Для программы iOS ключ словаря — это `Handle` значение, идентифицирующее конкретный палец. Значение словаря зависит от приложения. В программе [финжерпаинт](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint) каждый палец (от прикосновения к выпуску) связан с объектом, который содержит всю информацию, необходимую для отрисовки линии, нарисованной с помощью этого пальца. `FingerPaintPolyline`Для этой цели программа определяет небольшой класс:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new CGPath();
    }

    public CGColor Color { set; get; }

    public float StrokeWidth { set; get; }

    public CGPath Path { private set; get; }
}
```

Каждая Ломаная линия имеет цвет, толщину штриха и графический [`CGPath`](xref:CoreGraphics.CGPath) объект iOS для накопления и отрисовки нескольких точек линии при ее прорисовке.

Весь остальной код, показанный ниже, содержится в `UIView` производном названии `FingerPaintCanvasView` . Этот класс поддерживает словарь объектов типа `FingerPaintPolyline` в течение времени, когда они активно нарисованы одним или несколькими пальцами:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Этот словарь позволяет представлению быстро получать сведения, `FingerPaintPolyline` связанные с каждым пальцем, на основе `Handle` свойства `UITouch` объекта.

`FingerPaintCanvasView`Класс также поддерживает `List` объект для завершенных ломаных линий:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Объекты в этой области `List` находятся в том же порядке, в котором они были выведены.

`FingerPaintCanvasView`переопределяет пять методов, определенных `View` :

- [`TouchesBegan`](xref:UIKit.UIResponder.TouchesBegan(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesMoved`](xref:UIKit.UIResponder.TouchesMoved(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesEnded`](xref:UIKit.UIResponder.TouchesEnded(Foundation.NSSet,UIKit.UIEvent))
- [`TouchesCancelled`](xref:UIKit.UIResponder.TouchesCancelled(Foundation.NSSet,UIKit.UIEvent))
- [`Draw`](xref:UIKit.UIView.Draw(CoreGraphics.CGRect))

Различные `Touches` переопределения накапливаются точки, составляющие ломаные линии.

Переопределение [ `Draw` ] рисует завершенные ломаные линии, а затем выполняющиеся ломаные линии:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (CGContext context = UIGraphics.GetCurrentContext())
    {
        // Stroke settings
        context.SetLineCap(CGLineCap.Round);
        context.SetLineJoin(CGLineJoin.Round);

        // Draw the completed polylines
        foreach (FingerPaintPolyline polyline in completedPolylines)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }

        // Draw the in-progress polylines
        foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Каждое `Touches` Переопределение потенциально сообщает о действиях нескольких пальцев, обозначенных одним или несколькими `UITouch` объектами, хранящимися в `touches` аргументе метода. `TouchesBegan`Переопределяет цикл через эти объекты. Для каждого `UITouch` объекта метод создает и инициализирует новый `FingerPaintPolyline` объект, включая сохранение исходного расположения пальца, полученного из `LocationInView` метода. Этот `FingerPaintPolyline` объект добавляется в `InProgressPolylines` словарь, используя `Handle` свойство объекта в `UITouch` качестве ключа словаря:

```csharp
public override void TouchesBegan(NSSet touches, UIEvent evt)
{
    base.TouchesBegan(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Create a FingerPaintPolyline, set the initial point, and store it
        FingerPaintPolyline polyline = new FingerPaintPolyline
        {
            Color = StrokeColor,
            StrokeWidth = StrokeWidth,
        };

        polyline.Path.MoveToPoint(touch.LocationInView(this));
        inProgressPolylines.Add(touch.Handle, polyline);
    }
    SetNeedsDisplay();
}
```

Метод завершается вызовом метода `SetNeedsDisplay` для создания вызова `Draw` переопределения и обновления экрана.

Когда палец или пальцы перемещаются на экране, `View` получает несколько вызовов своего `TouchesMoved` переопределения. Это переопределение аналогично перебирает `UITouch` объекты, хранящиеся в `touches` аргументе, и добавляет текущее расположение пальца в графический путь:

```csharp
public override void TouchesMoved(NSSet touches, UIEvent evt)
{
    base.TouchesMoved(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Add point to path
        inProgressPolylines[touch.Handle].Path.AddLineToPoint(touch.LocationInView(this));
    }
    SetNeedsDisplay();
}
```

`touches`Коллекция содержит только те `UITouch` объекты для пальцов, которые были перемещены с момента последнего вызова метода `TouchesBegan` или `TouchesMoved` . Если вам когда-либо нужны `UITouch` объекты, соответствующие *всем* пальцам, которые в данный момент находятся в связи с экраном, эти сведения можно получить с помощью `AllTouches` свойства `UIEvent` аргумента в методе.

`TouchesEnded`Переопределение имеет два задания. Он должен добавить последнюю точку к графическому контуру и переместить `FingerPaintPolyline` объект из `inProgressPolylines` словаря в `completedPolylines` список:

```csharp
public override void TouchesEnded(NSSet touches, UIEvent evt)
{
    base.TouchesEnded(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Get polyline from dictionary and remove it from dictionary
        FingerPaintPolyline polyline = inProgressPolylines[touch.Handle];
        inProgressPolylines.Remove(touch.Handle);

        // Add final point to path and save with completed polylines
        polyline.Path.AddLineToPoint(touch.LocationInView(this));
        completedPolylines.Add(polyline);
    }
    SetNeedsDisplay();
}
```

`TouchesCancelled`Переопределение обрабатывается путем простого прерывания `FingerPaintPolyline` объекта в словаре:

```csharp
public override void TouchesCancelled(NSSet touches, UIEvent evt)
{
    base.TouchesCancelled(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        inProgressPolylines.Remove(touch.Handle);
    }
    SetNeedsDisplay();
}
```

В целом, эта обработка позволяет программе [финжерпаинт](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint) отслеживанию отдельных пальцев и выводить результаты на экране:

[![Отслеживание отдельных пальцев и рисование результатов на экране](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Теперь вы узнали, как можно отключать отдельные пальцы на экране и отличать их друг от друга.

## <a name="related-links"></a>Связанные ссылки

- [Эквивалентное руководством по Xamarin для Android](~/android/app-fundamentals/touch/touch-tracking.md)
- [Финжерпаинт (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)
