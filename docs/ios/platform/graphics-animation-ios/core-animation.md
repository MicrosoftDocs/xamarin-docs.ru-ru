---
title: Основная анимация в Xamarin. iOS
description: В этой статье рассматривается основная платформа анимации, в которой показано, как она обеспечивает высокую производительность, жидкие анимации в UIKit, а также как использовать ее непосредственно для управления анимацией более низкого уровня.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: d25d48421ad9b05925c1fa373ddba600ad3bac2e
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431232"
---
# <a name="core-animation-in-xamarinios"></a>Основная анимация в Xamarin. iOS

_В этой статье рассматривается основная платформа анимации, в которой показано, как она обеспечивает высокую производительность, жидкие анимации в UIKit, а также как использовать ее непосредственно для управления анимацией более низкого уровня._

iOS включает [*основную анимацию*](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) для обеспечения поддержки анимаций для представлений в приложении.
Все анимационные анимации в iOS, такие как прокрутка таблиц и считывание данных между различными представлениями, выполняются так же, как и в зависимости от основной анимации.

Основная анимация и основные графические платформы могут совместно работать, создавая привлекательную и анимированную двухмерную графику. На самом деле основная анимация может даже преобразовывать 2D-графику в трехмерном пространстве, создавая невероятно Цинематикные возможности. Тем не менее, чтобы создать истинную трехмерную графику, необходимо использовать нечто вроде OpenGL ES, или, чтобы игры могли работать с API, например с помощью «черно-игр», хотя трехмерность выходит за рамки этой статьи.

<a name="Using_Core_Animation"></a>

## <a name="core-animation"></a>Core Animation

в iOS используется основная платформа анимации для создания эффектов анимации, таких как переход между представлениями, скользящие меню и эффекты прокрутки, чтобы присвоить несколько имен. Существует два способа работы с анимацией.

- [Через UIKit](#Using_UIKit_Animation), который включает анимацию на основе представлений, а также анимированные переходы между контроллерами.
- [Через основную анимацию](#Using_Core_Animation), слои которой напрямую, позволяя более детально управлять.

<a name="Using_UIKit_Animation"></a>

## <a name="using-uikit-animation"></a>Использование анимации UIKit

UIKit предоставляет несколько функций, которые упрощают добавление анимации в приложение. Несмотря на то, что он использует основную анимацию, он абстрагирует ее, так что вы работаете только с представлениями и контроллерами.

В этом разделе обсуждаются функции анимации UIKit, включая:

- Переходы между контроллерами
- Переходы между представлениями
- Просмотр анимации свойств

### <a name="view-controller-transitions"></a>Переходы контроллера представления

 `UIViewController` предоставляет встроенную поддержку перехода между контроллерами представления с помощью `PresentViewController` метода. При использовании `PresentViewController` переход ко второму контроллеру можно дополнительно анимировать.

Например, рассмотрим приложение с двумя контроллерами, в которых кнопка на первом контроллере вызывается `PresentViewController` для вывода второго контроллера. Чтобы управлять тем, какая анимация перехода используется для отображения второго контроллера, просто установите его [`ModalTransitionStyle`](xref:UIKit.UIModalTransitionStyle) свойство, как показано ниже:

```csharp
SecondViewController vc2 = new SecondViewController {
  ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

В этом случае `PartialCurl` используется анимация, хотя доступны несколько других, в том числе:

- `CoverVertical` — Слайды в нижней части экрана
- `CrossDissolve` — Старое представление постепенно исчезает, & новое представление постепенно исчезает
- `FlipHorizontal` — Вертикальное перелистывание справа налево. При отклонении перехода переворачивается слева направо.

Чтобы анимировать переход, передайте в `true` качестве второго аргумента `PresentViewController` :

```csharp
PresentViewController (vc2, true, null);
```

На следующем снимке экрана показано, как выглядит переход в `PartialCurl` случае:

 ![На этом снимке экрана показан переход Партиалкурл](core-animation-images/06-view-transitions.png)

### <a name="view-transitions"></a>Просмотр переходов

Помимо переходов между контроллерами, UIKit также поддерживает анимацию переходов между представлениями для переключения одного представления на другое.

Например, предположим, что у вас есть контроллер с `UIImageView` , где нажатие на изображение должно отобразить вторую `UIImageView` . Чтобы анимировать вид представления изображения для перехода к второму представлению изображения, достаточно просто вызвать `UIView.Transition` , передав ему `toView` и, `fromView` как показано ниже:

```csharp
UIView.Transition (
  fromView: view1,
  toView: view2,
  duration: 2,
  options: UIViewAnimationOptions.TransitionFlipFromTop |
    UIViewAnimationOptions.CurveEaseInOut,
  completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` также принимает `duration` параметр, который управляет временем выполнения анимации, а также [`options`](xref:UIKit.UIViewAnimationOptions) позволяет указать такие элементы, как используемая анимация и функция плавности. Кроме того, можно указать обработчик завершения, который будет вызываться при завершении анимации.

На следующем снимке экрана показан анимированный переход между представлениями изображений, когда `TransitionFlipFromTop` используется.

 ![На этом снимке экрана показан анимированный переход между представлениями изображений при использовании Транситионфлипфромтоп](core-animation-images/07-animated-transition.png)

### <a name="view-property-animations"></a>Просмотр анимаций свойств

UIKit поддерживает бесплатное анимацию различных свойств `UIView` класса, включая:

- Frame
- Bounds
- Center
- Коэффициент альфа
- Преобразование
- Color

Эти анимации происходят неявно путем указания изменений свойств в `NSAction` делегате, переданном статическому `UIView.Animate` методу. Например, следующий код анимирует центральную точку объекта `UIImageView` :

```csharp
pt = imgView.Center;

UIView.Animate (
  duration: 2, 
  delay: 0, 
  options: UIViewAnimationOptions.CurveEaseInOut | 
    UIViewAnimationOptions.Autoreverse,
  animation: () => {
    imgView.Center = new CGPoint (View.Bounds.GetMaxX () 
      - imgView.Frame.Width / 2, pt.Y);},
  completion: () => {
    imgView.Center = pt; }
);
```

Это приводит к анимации на изображении в верхней части экрана, как показано ниже:

 ![Анимация изображения в верхней части экрана в качестве выходных данных](core-animation-images/08-animate-center.png)

Как и в `Transition` случае с методом, `Animate` можно задать длительность, а также функцию плавности. В этом примере также используется `UIViewAnimationOptions.Autoreverse` параметр, который вызывает анимацию анимации из значения обратно в исходное значение. Однако код также задает `Center` начальное значение обратно в обработчике завершения. Хотя анимация выполняет интерполяцию значений свойств с течением времени, фактическое значение модели свойства всегда является конечным значением, которое было задано. В этом примере значение — это точка, расположенная ближе к правой части. Без установки в качестве `Center` начальной точки, в которой анимация завершается из-за того, что `Autoreverse` задается, изображение будет привязано к правой стороне после завершения анимации, как показано ниже:

 ![Без настройки центра на начальную точку, после завершения анимации изображение будет привязано к правой стороне.](core-animation-images/09-animation-complete.png)

## <a name="using-core-animation"></a>Использование основной анимации

 `UIView` анимация позволяет использовать много возможностей и должна использоваться, если это возможно из-за простоты реализации. Как упоминалось ранее, анимация UIView использует основную платформу анимации. Однако некоторые вещи не могут быть выполнены с помощью `UIView` анимации, например для анимации дополнительных свойств, которые не могут быть анимированы с помощью представления, или интерполяции вдоль нелинейного пути. В таких случаях, когда требуется более тонкий контроль, можно также использовать основную анимацию напрямую.

### <a name="layers"></a>Слои

При работе с основной анимацией анимация происходит через *слои*, имеющие тип `CALayer` . Слой концептуально похож на представление в том, что существует иерархия слоев, во многом подобно иерархии представлений. На самом деле слои переводятся в представления с добавлением поддержки взаимодействия с пользователем. Доступ к слою любого представления можно получить через `Layer` свойство представления. Фактически, контекст, используемый в `Draw` методе, на `UIView` самом деле создается из слоя. На внутреннем уровне уровень резервирования `UIView` имеет свой делегат для самого представления, который вызывает `Draw` . Таким образом, при рисовании в `UIView` вы фактически рисуете в своем слое.

Анимация слоя может быть либо неявной, либо явной. Неявные анимации являются декларативными. Вы просто объявляете, какие свойства слоя должны изменяться, и анимация работает. Явная анимация с другой стороны создается с помощью класса анимации, который добавляется к слою. Явная анимация позволяет дополнительно управлять созданием анимации. В следующих разделах подробно рассматриваются явные и неявные анимации.

### <a name="implicit-animations"></a>Неявные анимации

Одним из способов анимации свойств слоя является неявная анимация. `UIView` анимации создают неявные анимации. Однако можно также создавать неявные анимации непосредственно для слоя.

Например, следующий код устанавливает слой `Contents` из изображения, устанавливает ширину и цвет границы и добавляет слой в качестве подслоя слоя представления:

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();

  layer = new CALayer ();
  layer.Bounds = new CGRect (0, 0, 50, 50);
  layer.Position = new CGPoint (50, 50);
  layer.Contents = UIImage.FromFile ("monkey2.png").CGImage;
  layer.ContentsGravity = CALayer.GravityResize;
  layer.BorderWidth = 1.5f;
  layer.BorderColor = UIColor.Green.CGColor;

  View.Layer.AddSublayer (layer);
}
```

Чтобы добавить неявную анимацию для слоя, просто заключите изменения свойств в `CATransaction` . Это позволяет анимировать свойства, которые не могут быть анимированы с анимацией представления, например, `BorderWidth` и, `BorderColor` как показано ниже:

```csharp
public override void ViewDidAppear (bool animated)
{
  base.ViewDidAppear (animated);

  CATransaction.Begin ();
  CATransaction.AnimationDuration = 10;
  layer.Position = new CGPoint (50, 400);
  layer.BorderWidth = 5.0f;
  layer.BorderColor = UIColor.Red.CGColor;
  CATransaction.Commit ();
}
```

Этот код также анимирует слой `Position` , который является расположением точки привязки слоя, измеряемой в левом верхнем углу координат верхнего слоя. Точка привязки слоя — это нормализованная точка в системе координат слоя.

На следующем рисунке показано расположение и точка привязки:

 ![На рисунке показано расположение и точка привязки](core-animation-images/10-postion-anchorpt.png)

При выполнении примера, `Position` `BorderWidth` и `BorderColor` анимации, как показано на следующих снимках экрана:

 ![При выполнении этого примера значение свойства положением, BorderWidth и BorderColor анимируется, как показано](core-animation-images/11-implicit-animation.png)

### <a name="explicit-animations"></a>Явная анимация

В дополнение к неявным анимациям основная анимация включает различные классы, наследуемые от `CAAnimation` , которые позволяют инкапсулировать анимации, которые затем явно добавляются в слой. Они позволяют более детально контролировать анимацию, например изменять начальное значение анимации, группировать анимации и указывать опорные кадры, чтобы разрешить нелинейные пути.

В следующем коде показан пример явной анимации с использованием `CAKeyframeAnimation` для слоя, показанного ранее (в разделе неявных анимаций):

```csharp
public override void ViewDidAppear (bool animated)
{
  base.ViewDidAppear (animated);
  
  // get the initial value to start the animation from
  CGPoint fromPt = layer.Position;
  
  /* set the position to coincide with the final animation value
  to prevent it from snapping back to the starting position
  after the animation completes*/
  layer.Position = new CGPoint (200, 300);
  
  // create a path for the animation to follow
  CGPath path = new CGPath ();
  path.AddLines (new CGPoint[] { fromPt, new CGPoint (50, 300), new CGPoint (200, 50), new CGPoint (200, 300) });
  
  // create a keyframe animation for the position using the path
  CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
  animPosition.Path = path;
  animPosition.Duration = 2;
  
  // add the animation to the layer.
  /* the "position" key is used to overwrite the implicit animation created
  when the layer positino is set above*/
  layer.AddAnimation (animPosition, "position");
}
```

Этот код изменяет `Position` слой, создавая путь, который затем используется для определения анимации опорного кадра. Обратите внимание, что `Position` для слоя задано окончательное значение `Position` из анимации. Без этого слой внезапно вернется к нему `Position` перед анимацией, поскольку анимация изменяет только значение представления, а не фактическое значение модели. Установив значение модели в окончательное значение из анимации, слой останется в конце анимации.

На следующих снимках экрана показан слой, содержащий анимированное изображение по указанному пути:

 ![На этом снимке экрана показан слой, содержащий анимированное изображение по указанному пути](core-animation-images/12-explicit-animation.png)

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели возможности анимации, предоставляемые с помощью *основных платформ анимации* . Мы рассматривали основную анимацию, показывая, как она задействует анимацию в UIKit и как ее можно использовать непосредственно для элемента управления анимации более низкого уровня.

## <a name="related-links"></a>Связанные ссылки

- [Пример основной анимации](/samples/xamarin/ios-samples/graphicsandanimation)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Пошаговое руководство по графике и анимации](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)