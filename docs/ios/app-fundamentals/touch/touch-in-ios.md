---
title: Сенсорные события и жесты в Xamarin. iOS
description: В этом документе описывается работа с событиями касания, несколькими касаниями, жестами, несколькими жестами и пользовательскими жестами в приложениях Xamarin. iOS.
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 0fe6b0b46035ac61d4aaddccb585276a80337202
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928813"
---
# <a name="touch-events-and-gestures-in-xamarinios"></a>Сенсорные события и жесты в Xamarin. iOS

Важно понимать события касания и API сенсорного ввода в приложении iOS, так как они являются центральными для всех физических взаимодействий с устройством. Все сенсорные взаимодействия используют `UITouch` объект. Из этой статьи вы узнаете, как использовать `UITouch` класс и его интерфейсы API для поддержки сенсорного ввода. Позже мы развернетесь к нашим знаниям, чтобы научиться поддерживать жесты.

## <a name="enabling-touch"></a>Включение сенсорного ввода

Элементы управления в `UIKit` — эти подклассы из уиконтрол — поэтому зависят от взаимодействия с пользователем о том, что они имеют встроенные жесты в UIKit и поэтому нет необходимости включать сенсорный ввод. Он уже включен.

Однако во многих представлениях в не `UIKit` включено касание по умолчанию. Существует два способа включения сенсорного ввода в элемент управления. Первый способ — установить флажок "взаимодействие с пользователем" на панели свойств в конструкторе iOS, как показано на следующем снимке экрана:

 [![Установите флажок взаимодействие с пользователем включено на панели свойств конструктора iOS.](touch-in-ios-images/image1.png)](touch-in-ios-images/image1.png#lightbox)

Можно также использовать контроллер, чтобы установить `UserInteractionEnabled` для свойства значение true в `UIView` классе. Это необходимо, если пользовательский интерфейс создается в коде.

Примером может быть следующая строка кода:

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>События касания

Существует три этапа касания, которые возникают, когда пользователь касается экрана, перемещает палец или удаляет палец. Эти методы определяются в `UIResponder` , который является базовым классом для UIView. iOS переопределит связанные методы в `UIView` и `UIViewController` для управления сенсорным вызовом:

- `TouchesBegan`— Вызывается при первом появлении экрана.
- `TouchesMoved`— Вызывается при изменении положения сенсорных элементов по мере того, как пользователь перемещает свои пальцы вокруг экрана.
- `TouchesEnded`или `TouchesCancelled` — `TouchesEnded` вызывается при поднятии пальцев пользователя с экрана.  `TouchesCancelled`вызывается, если iOS отменяет касание, например, если пользователь заслайдует пальцем от кнопки, чтобы отменить нажатие клавиши.

События касания проходят рекурсивно вниз по стеку Уивиевс, чтобы проверить, находится ли событие касания внутри границ объекта представления. Это часто называется _проверкой попадания_. Они сначала будут вызываться на самом верхнем `UIView` или `UIViewController` , а затем будут вызываться для `UIView` и `UIViewControllers` под ними в иерархии представления.

`UITouch`Объект будет создан каждый раз, когда пользователь касается экрана. `UITouch`Объект содержит данные о сенсорном касании, например о том, когда произошло касание, где это произошло, если сенсорный ввод был прокруткой и т. д. События касания передают свойство «касания `NSSet` », содержащее одно или несколько касаний. Это свойство можно использовать для получения ссылки на касание и определения ответа приложения.

Классы, которые переопределяют одно из событий касания, должны сначала вызвать базовую реализацию, а затем получить `UITouch` объект, связанный с событием. Чтобы получить ссылку на первое касание, вызовите `AnyObject` свойство и приведите его как, `UITouch` как показано в следующем примере:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        //code here to handle touch
    }
}
```

iOS автоматически распознает пошаговые быстрые касания на экране и соберет их как одно касание в одном `UITouch` объекте. Это позволяет проверить двойное касание так же просто, как проверить `TapCount` свойство, как показано в следующем коде:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        if (touch.TapCount == 2)
        {
            // do something with the double touch.
        }
    }
}
```

## <a name="multi-touch"></a>Несколько сенсорных

По умолчанию для элементов управления множественное касание отключено. Множественное касание можно включить в конструкторе iOS, как показано на следующем снимке экрана:

 [![Поддержка нескольких касаний в конструкторе iOS](touch-in-ios-images/image2.png)](touch-in-ios-images/image2.png#lightbox)

Кроме того, можно настроить программное управление несколькими касаниями, задав `MultipleTouchEnabled` свойство, как показано в следующей строке кода:

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

Чтобы определить, сколько пальцев затронуло экран, используйте `Count` свойство `UITouch` Свойства:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>Определение сенсорного расположения

Метод `UITouch.LocationInView` возвращает объект кгпоинт, содержащий координаты касания в данном представлении. Кроме того, можно проверить, находится ли это расположение внутри элемента управления, вызвав метод `Frame.Contains` . В следующем фрагменте кода приведен пример.

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

Теперь, когда у нас есть основные сведения о мероприятиях касания в iOS, давайте подробнее рассмотрим распознаватели жестов.

## <a name="gesture-recognizers"></a>Распознаватели жестов

Распознаватели жестов могут значительно упростить и сократить усилия по программированию для поддержки сенсорного ввода в приложении. Распознаватели жестов iOS объединяют последовательности сенсорных событий в одно событие касания.

Xamarin. iOS предоставляет класс в `UIGestureRecognizer` качестве базового класса для следующих встроенных распознавателей жестов:

- *Уитапжестуререкогнизер* — для одного или нескольких касаний.
- *Уипинчжестуререкогнизер* — Сжатие и распространение пальцев.
- *Уипанжестуререкогнизер* — панорамирование или перетаскивание.
- *Уисвипежестуререкогнизер* — прокрутка в любом направлении.
- *Уиротатионжестуререкогнизер* — поворот двух пальцев по часовой стрелке или по часовой стрелке.
- *Уилонгпрессжестуререкогнизер* — нажатие и удержание иногда называют длительным нажатием или длинным щелчком.

Базовый шаблон для использования распознавателя жестов выглядит следующим образом:

1. **Создайте экземпляр распознавателя жеста** — сначала создайте экземпляр `UIGestureRecognizer` подкласса. Объект, экземпляр которого создается, будет связан с представлением и будет собираться сборщиком мусора при удалении представления. Нет необходимости создавать это представление как переменную уровня класса.
1. **Настройте все параметры жестов** . следующим шагом является настройка распознавателя жестов. Обратитесь к документации по Xamarin `UIGestureRecognizer` и его подклассам, чтобы получить список свойств, которые можно задать для управления поведением `UIGestureRecognizer` экземпляра.
1. **Настройте целевой объект** — из-за его цели — C Heritage, Xamarin. iOS не создает события, когда распознаватель жестов соответствует жесту.  `UIGestureRecognizer`содержит метод — `AddTarget` , который может принимать анонимный делегат или целевой элемент выбора с кодом, который будет выполнен, когда средство распознавания жестов выполняет сопоставление.
1. **Включить распознаватель жестов** — как и для событий касания, жесты распознаются только в том случае, если включены сенсорные взаимодействия.
1. **Добавление распознавателя жестов к представлению** — заключительный шаг — добавление жеста в представление путем вызова метода `View.AddGestureRecognizer` и передачи ему объекта распознавателя жестов.

Дополнительные сведения о том, как реализовать их в коде, см. в [примерах распознавателя жестов](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) .

При вызове целевого объекта жеста ему передается ссылка на произошедший жест. Это позволяет объекту жеста получать сведения о произошедшем жесте. Объем доступной информации зависит от типа используемого распознавателя жестов. Сведения о данных, доступных для каждого подкласса, см. в документации по Xamarin `UIGestureRecognizer` .

Важно помнить, что после добавления распознавателя жестов в представление представление (и все представленные ниже его представления) не получит никаких событий касания. Чтобы разрешить события касания одновременно с жестами, `CancelsTouchesInView` свойство должно иметь значение false, как показано в следующем коде:

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

Каждый `UIGestureRecognizer` имеет свойство State, которое предоставляет важную информацию о состоянии распознавателя жестов. При каждом изменении значения этого свойства iOS будет вызывать метод подписки, который предоставляет ему обновление. Если пользовательское средство распознавания жестов никогда не обновляет свойство State, подписчик никогда не вызывается, и визуализация распознавателя жестов бесполезна.

Жесты можно суммировать как один из двух типов:

1. *Дискретный* — эти жесты срабатывают только при первом их распознавании.
1. *Непрерывно* — эти жесты продолжают срабатывать, пока они распознаны.

Распознаватели жестов находятся в одном из следующих состояний:

- *Возможно* — это начальное состояние всех распознавателей жестов. Это значение используется по умолчанию для свойства State.
- *Начало* — при первом распознавании непрерывного жеста состояние имеет значение начато. Это позволяет подписываться на различия между началом распознавания жестов и его изменением.
- *Изменено* — после начала непрерывного жеста, но еще не завершенного, состояние будет изменено при каждом перемещении или изменении касания, если оно все еще находится в пределах ожидаемых параметров жеста.
- *Отменено* — это состояние будет установлено, если распознаватель начал изменение, а затем отменяются таким образом, чтобы больше не соответствовать шаблону жеста.
- *Распознано* — состояние будет задано, когда распознаватель жестов будет соответствовать набору касаний и сообщит подписчику о завершении жеста.
- *Завершено* — это псевдоним для распознанного состояния.
- *Failed* — Если распознаватель жестов больше не будет соответствовать касаниям, которое он ожидает, состояние изменится на Failed (сбой).

Xamarin. iOS представляет эти значения в `UIGestureRecognizerState` перечислении.

## <a name="working-with-multiple-gestures"></a>Работа с несколькими жестами

По умолчанию iOS не разрешает одновременное выполнение жестов по умолчанию. Вместо этого каждый распознаватель жестов будет принимать события касания в недетерминированном порядке. В следующем фрагменте кода показано, как сделать средство распознавания жестов запущенным одновременно:

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

Кроме того, можно отключить жест в iOS. Существует два свойства делегата, которые позволяют распознавателю жестов проверять состояние приложения и текущие события касания, чтобы принимать решения о том, как и когда должен распознаться жест. Это два события:

1. *Шаулдрецеиветауч* — этот делегат вызывается прямо перед тем, как распознаватель жестов передается событием касания, и дает возможность проверить касания и решить, какие касания будет обрабатываться распознавателем жестов.
1. *Шаулдбегин* — вызывается, когда распознаватель пытается изменить состояние с возможно на другое состояние. Если вернуть значение false, состояние распознавателя жеста будет изменено на Failed.

Эти методы можно переопределить строго типизированным `UIGestureRecognizerDelegate` , нестрогим делегатом или выполнить привязку с помощью синтаксиса обработчика событий, как показано в следующем фрагменте кода:

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

Наконец, можно поставить в очередь распознаватель жестов, чтобы он был успешным только в случае сбоя другого распознавателя жестов. Например, один распознаватель жестов TAP должен быть успешным только в случае сбоя распознавателя жеста двойного касания. В следующем фрагменте кода приведен пример.

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>Создание пользовательского жеста

Хотя iOS предоставляет некоторые распознаватели жестов по умолчанию, в некоторых случаях может потребоваться создать собственные распознаватели жестов. Создание пользовательского распознавателя жестов включает следующие шаги.

1. Подкласс `UIGestureRecognizer` .
1. Переопределите соответствующие методы событий касания.
1. Всплывающее состояние распознавания с помощью свойства State базового класса.

Практический пример этого рассматривается в пошаговом руководстве [Использование Touch в iOS](ios-touch-walkthrough.md) .
